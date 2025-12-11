# Primi passi
Monta i dischi SATA SSD nelle slitte e inseriscile negli alloggiamenti anteriori del server DELL.
Collega un **cavo Ethernet RJ45** alla porta **dedicata iDRAC** (etichettata "iDRAC" sul retro del server Dell PowerEdge R730xd).
Collega l'**alimentazione** (verifica che entrambi gli alimentatori siano accoppiati se ridondanti).
Collega un **cavo VGA** alla porta video del server (non HDMI/displayPort). <br>
![installazione dischi ](./imgs/truenas2.png)

## Configurazione rete iDRAC su Dell PowerEdge R730xd

1. Accendi il server e premi **F2** per entrare in **System Setup**.
2. Vai su **iDRAC Settings**.
3. Entra in **Network**.
4. Seleziona **IPv4 Settings**.
5. Imposta **Enable IPv4** su `Enabled`.
6. Imposta **IP Address Source** su `DHCP`.
7. Imposta **Use DHCP to obtain DNS server addresses** su `Enabled` (se vuoi che usi il DNS fornito dal DHCP).
8. Salva le impostazioni con **Apply** e poi esci con **Finish** / **Exit**.
9. Riavvia il server e recupera l'indirizzo IP assegnato a iDRAC (dal display LCD frontale o dal tuo server DHCP).

### Configurazione rete su porta ETH3

1. Collega un **cavo Ethernet Cat6 UTP** alla porta **Ethernet 3** del server.
2. Avvia il server e premi **F10** per entrare in **Lifecycle Controller**.
3. Vai su **Network**.
4. Seleziona l'interfaccia **eth3**.
5. Imposta l'assegnazione IP su **DHCP**.
6. Salva/applica le impostazioni.

### Aggiornamento via HTTPS

1. Dal Lifecycle Controller seleziona **Firmware Update** / **Update via HTTPS**.
2. Conferma le impostazioni di default del server di aggiornamento.
3. Avvia la procedura di update e attendi il completamento.

### Abilitazione Virtual Console (HTML5)

1. Accedi all'interfaccia web di iDRAC dal browser.
2. Vai in **Settings** → **Virtual Console**.
3. Imposta **Plugin Type** su **HTML5**.
4. Clicca su **Launch Virtual Console**.
5. Se il browser blocca il popup, consenti sempre popup e reindirizzamenti per l'indirizzo di iDRAC dalle impostazioni del browser.

### Montare l'ISO di TrueNAS come Virtual Media

1. Nella finestra della **Virtual Console**, apri il menu **Virtual Media**.
2. Clicca su **Connect Virtual Media** (se non è già connesso).
3. Seleziona **Map CD/DVD**.
4. Scegli il file ISO di **TrueNAS** precedentemente scaricato.
5. Clicca su **Map Device** per montare l'immagine come unità ottica virtuale.

6. ### Verifica presenza RAID Controller

1. Spegni il server in sicurezza.
2. Apri il coperchio superiore del Dell PowerEdge R730xd.
3. Verifica la presenza di un **Dell PERC H730** (o controller RAID equivalente) collegato alla backplane dei dischi, necessario per la gestione dei dischi posteriori.

### Boot da Virtual Optical Drive

1. Accendi il server.
2. Premi **F11** per accedere al **Boot Manager** (UEFI).
3. Seleziona **One-Time Boot**.
4. Scegli **UEFI: Virtual Optical Drive** come dispositivo di boot.
5. Esegui un **cold boot** se necessario (riavvio a freddo senza togliere alimentazione, dal menu iDRAC o dal front panel).

### Installazione TrueNAS

1. Dal menu di boot seleziona l'opzione **Install/Upgrade** TrueNAS.
2. Premi **Enter** e per ridondanza scegli entrambi gli ssd posteriori
3. Scegli password per l'administer user
4. Fai login con username **truenas_admin** e la password scelta prima.

### Primo avvio di TrueNAS

1. Accendi il server NAS.
2. Dal **Virtual Console** (iDRAC) attendi la schermata nera di boot di TrueNAS.
3. In alto, nella console, viene mostrato l'**indirizzo IP** assegnato a TrueNAS (es. `http://192.168.X.Y/`).  
4. Dal tuo PC, apri il browser e collegati a quell'indirizzo IP.
5. Effettua il login con le credenziali di default (oppure con quelle impostate in fase di installazione).

![Schermata di boot TrueNAS con IP assegnato](./img/truenas1.png)

### Creazione del pool di storage

1. Vai in **Storage** → **Pools**.
2. Clicca su **Add** per creare un nuovo pool.
3. Inserisci un **nome** per il pool (es. `pool_dati`).
4. Seleziona tutti i dischi **anteriori** (es. dischi SATA 3.5" da 1 TB).
5. Scegli come layout **RAIDZ1**:
   - RAIDZ1 fornisce **singola parità** (tolleranza alla perdita di 1 disco).
   - È concettualmente simile a un RAID5, ma implementato da ZFS.
6. Imposta:
   - **Width** = numero totale dei dischi anteriori che vuoi usare nel vdev (es. 6, 8, 10…).
   - **VDEVs** = 1 (un solo vdev RAIDZ1 che contiene tutti questi dischi).
7. Salta le impostazioni avanzate se non strettamente necessarie.
8. Clicca su **Create** e conferma l’avviso di formattazione dei dischi.

### Backup configurazione TrueNAS

1. Vai in **System** → **Update**.
2. Clicca su **Save Config** / **Download Configuration**.
3. Scarica e conserva il file di configurazione (.db) in un luogo sicuro.

### Verifica stato pool da CLI

1. Apri una shell sul server TrueNAS (via **Shell** nell'interfaccia web oppure SSH).
2. Esegui il comando: zpool status
3. Verifica che il pool appena creato risulti in stato `ONLINE` e che tutti i dischi siano elencati correttamente.






