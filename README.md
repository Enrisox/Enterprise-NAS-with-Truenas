#Durante la mia esperienza in Cloudfire come cloud engineer ho preparato e configurato dei server DELL come backup storage destinato ad aziende clienti, e in questo mini tutorial voglio ripercorrere tutti gli step dall'inizio alla fine, spiegando alla fine anche il processo di installazione TrueNAS.

# Cos'è TrueNAS?
TrueNAS è un sistema operativo per NAS (Network Attached Storage) basato su ZFS, pensato per creare server di storage affidabili, con snapshot, repliche e condivisioni di rete (SMB/NFS/iSCSI) sia in ambienti domestici che aziendali.​
Permette di usare un server (come il tuo Dell R730xd) come appliance di storage centralizzato per file, backup, VM, sorveglianza, ecc.​
Usa il filesystem ZFS, che offre integrità dei dati, snapshot, compressione, RAID software (RAIDZ), replica e funzionalità avanzate per proteggere i dati.​
Storicamente c’erano TrueNAS CORE (base FreeBSD) e TrueNAS SCALE (base Linux/Debian).​
Oggi l’evoluzione è TrueNAS Community Edition con release tipo 25.xx, che unifica il ramo “di comunità” mantenendo ZFS e le funzionalità NAS/virtualizzazione/container.​

## Perché viene usato?
È gratuito, open source e molto diffuso per lab, homelab e piccole/medie aziende che vogliono un NAS potente senza comprare appliance proprietarie.​
Offre una web GUI completa per creare pool di dischi, condivisioni, utenti, snapshot e repliche, riducendo al minimo la necessità di lavorare solo da CLI.

## Enterprise-NAS-with-Truenas
- SERVER DELL R730XD
- ISO TrueNAS 25.10 “Goldeye” community edition
## Layout dischi

- **Bay frontali**: 12 × dischi SATA 3.5" da 1 TB (storage dati principale).
- **Bay posteriori**: 2 × dischi 2.5" da 250 GB circa (utilizzati per il sistema TrueNAS).
- **Controller**: Dell PERC H730 collegato alla backplane dei dischi.
