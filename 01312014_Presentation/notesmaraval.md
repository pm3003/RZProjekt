# Storage and Backup

## Einführung

### Begriffe - Virtualisierung :

Wo findet die Virtualisierung statt ?

* *Auf Subsystem-ebene* : Teurere RAID-Arrays, mit allem Möglichen, hot plug etc.
* *Auf Netzwerkebene * : LVM, Hypervisor
* *Auf Applicationsebene* : JBOD oder einfache RAID arrays, die ganze  Virtualisierung erfolgt im Hypervisor oder in der LUN Management (Speicherverwaltungssoftware).


### Serverzentrierung - Speicherzentrierung :

Speicherzentrierung : Alle Servers haben können auf den Speicher zugreifen.

### SAN NAS :

NAS : out-of-the-box, file level.
SAN : Block-level

## Festplattentechnologie

Auf Festplatten-ebene, Subsystem-ebene (Rack), Systemebene



### RAID

JBOD, controller, Software Raids, Methoden zur Steigerung der Verfügbarkeit und Sicherheit.

Verschiedene RAIDs für verschiedene Prioritäten.

Supermicro Storage server : RAID 0,1,5,10.

### SCSI

### Beschleunigung und Sicherung von Festplatten-systemen
The following list describes the individual measures that can be taken to increase the
availability of data:

* The data is distributed over several hard disks using RAID processes and supplemented by further data for error correction.Balance between security and performance.
* Hamming code on Individual hard disks.
* Each internal physical hard disk can be connected to the controller via two internal I/O channels. If one of the two channels fails, the other can still be used.
* The controller in the disk subsystem can be realised by several controller instances. If one of the controller instances fails, one of the remaining instances takes over the tasks of the defective instance.
* Other auxiliary components such as power supplies, batteries and fans can often be duplicated so that the failure of one of the components is unimportant. When connecting the power supply it should be ensured that the various power cables are at least connected through various fuses. Even different Power networks.
* Server and disk subsystem are connected together via several I/O channels. If one of the channels fails, the remaining ones can still be used.
* Instant copies can be used to protect against logical errors. For example, it would be possible to create an instant copy of a database every hour. If a table is ‘accidentally’ deleted, then the database could revert to the last instant copy in which the database is still complete.
* Remote mirroring protects against physical damage. If, for whatever reason, the original data can no longer be accessed, operation can continue using the data copy that was generated using remote mirroring.
* Consistency groups
* LUN Masking.

### Fibre Channel :

* Implementiert auf HBA Ebene oder niedriger für FC-0 bis FC-3.
* Verschiedene Topologien : Fabric, Point-to-Point, Loop, Arbitrated Loop, Switched Fabric.

### Alternative : Infiniband
Spezialisiert für clusters.

### Fileservers :
Früher FTP Servers.
NAS : Was man zuhause hat. Meistens Out of the Box. 

Aktualisierung meistens kompliziert.
Alles geht durch TCP/IP. Große Last auf dem Server-CPU.
**Beschleunigen** : TCP/IP offload engines : Netzwerk karten, die TCP/IP implementieren.
RDMA, Virtual Interface.
**Prefetch**
Wenn man eine Anwendung startet, holt der Netzwerk die von der Anwendung gespeicherte Dateien ins Cache.
### Fileservers :

Shared Disk Systems : Ziel : das nicht alles auf TCP/IP beruht. (Benutzung von Ethernet im Serverbereich)
~~ Distributed File Systems (Block-Access über Netzwerke wie Ethernet).


## Betrieb von Speichersystemen

### Grundprinzipen 

Nur Bedarf ergänzen.

### Werkzeuge

### Merkmale
(In-Band /) **Out-of-band** : Separate Übertragung von zB kontrolldaten  und arbeitsdaten, manchmal sogar über verschiedene Netzwerke.

Beispiel : Simple Network Management Protocol.

### Thin Provisioning
An automated storage consolidation process.
Needs an adapted (modern) filesystem (not ntfs).
The SAN automatically declares larger LUNs  than the capacity of available physical drives (or smaller LUNs that are expanded over time). When the requested capacity is approaching the physical capacity, additional drives are plugged in.

When properly managed and monitored, thin provisioning can result in lower storage costs due to better disk utilization and can reduce some of the administrative overhead associated with monitoring individual volumes.

## Supermicro

## VMWare ESXi

Kein RAID Software.