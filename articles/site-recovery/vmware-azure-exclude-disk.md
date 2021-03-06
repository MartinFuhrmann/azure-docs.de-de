---
title: Ausschließen von VMware-VM-Datenträgern von der Notfallwiederherstellung in Azure mit Azure Site Recovery
description: Beschreibt, warum und wie Sie VM-Datenträger bei der Replikation für die VMWare-Notfallwiederherstellung zu Azure ausschließen.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.date: 3/3/2019
ms.author: mayg
ms.topic: conceptual
ms.openlocfilehash: c003620420611f3416e6481c575f987fbd1bd05f
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73622377"
---
# <a name="exclude-disks-from-vmware-vm-replication-to-azure"></a>Ausschließen von Datenträgern von der Replikation virtueller VMware-Computer zu Azure

In diesem Artikel wird beschrieben, wie Datenträger von der Replikation von VMware-VMs nach Azure ausgeschlossen werden. Durch diesen Ausschluss können die beanspruchte Replikationsbandbreite oder die zielseitigen Ressourcen optimiert werden, die solche Datenträger verwenden. Informationen zum Ausschließen von Datenträgern für Hyper-V finden Sie [in diesem Artikel](hyper-v-exclude-disk.md).


## <a name="prerequisites"></a>Voraussetzungen

Standardmäßig werden alle Datenträger auf einem Computer repliziert. Sie müssen den Mobilitätsdienst bei einer Replikation von VMware in Azure vor der Aktivierung der Replikation manuell auf dem Computer installieren, um einen Datenträger von der Replikation auszuschließen.


## <a name="why-exclude-disks-from-replication"></a>Gründe für das Ausschließen von Datenträgern von der Replikation
Das Ausschließen von Datenträgern von der Replikation ist häufig aus folgenden Gründen erforderlich:

- Die auf dem ausgeschlossenen Datenträger verarbeiteten Daten sind nicht wichtig oder müssen nicht repliziert werden.

- Sie können Speicher- und Netzwerkressourcen sparen, indem Sie diese Daten nicht replizieren.

## <a name="what-are-the-typical-scenarios"></a>Was sind die typischen Szenarien?
Sie können spezifische Beispiele für Datenänderungen ermitteln, die hervorragend für den Ausschluss geeignet sind. Zu den Beispielen können Schreibvorgänge in einer Auslagerungsdatei (pagefile.sys) und Schreibvorgänge in der tempdb-Datei von Microsoft SQL Server gehören. Je nach Workload und Speichersubsystem kann in der Auslagerungsdatei eine beträchtliche Menge an Datenänderungen registriert werden. Wenn diese Daten vom primären Standort in Azure repliziert werden, ist der Ressourcenaufwand sehr hoch. Daher können Sie die folgenden Schritte zum Optimieren der Replikation eines virtuellen Computers mit einem einzelnen virtuellen Datenträger ausführen, der das Betriebssystem und die Auslagerungsdatei enthält:

1. Unterteilen Sie den einzelnen virtuellen Datenträger in zwei virtuelle Datenträger. Ein virtueller Datenträger enthält das Betriebssystem und der andere die Auslagerungsdatei.
2. Schließen Sie den Datenträger mit der Auslagerungsdatei von der Replikation aus.

Sie können dementsprechend anhand der folgenden Schritte einen Datenträger optimieren, der sowohl die tempdb-Datei als auch die Systemdatenbankdatei von Microsoft SQL Server enthält:

1. Bewahren Sie die Systemdatenbank und die tempdb-Datei auf zwei unterschiedlichen Datenträgern auf.
2. Schließen Sie den tempdb-Datenträger von der Replikation aus.

## <a name="how-to-exclude-disks-from-replication"></a>Ausschließen von Datenträgern von der Replikation

Folgen Sie dem Workflow für [Replikation aktivieren](vmware-azure-enable-replication.md), um einen virtuellen Computer über das Azure Site Recovery-Portal zu schützen. Verwenden Sie im vierten Schritt des Workflows die Spalte **DISK TO REPLICATE** (ZU REPLIZIERENDER DATENTRÄGER), um Datenträger von der Replikation auszuschließen. Standardmäßig sind alle Datenträger für die Replikation ausgewählt. Deaktivieren Sie das Kontrollkästchen der Datenträger, die Sie von der Replikation ausschließen möchten, und führen Sie dann die Schritte zum Aktivieren der Replikation aus.

![Ausschließen von Datenträgern von der Replikation und Aktivieren der Replikation für Failbacks von VMware auf Azure](./media/vmware-azure-exclude-disk/enable-replication-exclude-disk1.png)


>[!NOTE]
>
> * Sie können nur Datenträger auf VMs ausschließen, auf denen der Mobilitätsdienst bereits installiert ist. Sie müssen den Mobilitätsdienst manuell installieren, da dieser nur nach Aktivierung der Replikation mithilfe des Pushmechanismus installiert wird.
> * Nur Basisdatenträger können von der Replikation ausgeschlossen werden. Sie können kein Betriebssystem und keine dynamischen Datenträger ausschließen.
> * Nach Aktivierung der Replikation können Sie keine Datenträger für die Replikation hinzufügen oder entfernen. Wenn Sie einen Datenträger hinzufügen oder entfernen möchten, müssen Sie den Schutz für den Computer deaktivieren und anschließend wieder aktivieren.
> * Wenn Sie einen Datenträger ausschließen, der für den Betrieb einer Anwendung erforderlich ist, müssen Sie den Datenträger nach dem Failover auf Azure manuell in Azure erstellen, damit die replizierte Anwendung ausgeführt werden kann. Alternativ können Sie Azure Automation in einen Wiederherstellungsplan integrieren, um den Datenträger während des Failovers des Computers zu erstellen.
> * Windows-VM: Für Datenträger, die Sie manuell in Azure erstellen, wird kein Failback durchgeführt. Wenn Sie also beispielsweise ein Failover für drei Datenträger durchführen und zwei Datenträger direkt auf virtuellen Azure-Computern erstellen, erfolgt nur für die drei Datenträger, für die das Failover durchgeführt wurde, ein Failback. Manuell erstellte Datenträger können nicht in das Failback oder in den erneuten Schutz vom lokalen Standort auf Azure einbezogen werden.
> * Linux-VM: Für Datenträger, die Sie manuell in Azure erstellen, wird ein Failback durchgeführt. Wenn Sie beispielsweise ein Failover für drei Datenträger ausführen und zwei Datenträger direkt auf virtuellen Azure-Computern erstellen, wird für alle fünf ein Failback ausgeführt. Sie können keine Datenträger von einem Failback ausschließen, die manuell erstellt wurden.
>


## <a name="end-to-end-scenarios-of-exclude-disks"></a>End-to-End-Szenarien zum Ausschließen von Datenträgern
Betrachten wir zwei Szenarien, damit Sie sich besser mit der Funktion zum Ausschließen von Datenträgern vertraut machen können:

- tempdb-Datenträger von SQL Server
- Datenträger für Auslagerungsdatei (pagefile.sys)

## <a name="example-1-exclude-the-sql-server-tempdb-disk"></a>Beispiel 1: Ausschließen des tempdb-Datenträgers von SQL Server
Angenommen, Sie verwenden einen virtuellen SQL Server-Computer mit einer tempdb, der ausgeschlossen werden kann.

Der Name des virtuellen Datenträgers lautet „SalesDB“.

Folgende Datenträger befinden sich auf dem virtuellen Quellcomputer:


**Name des Datenträgers** | **Gastbetriebssystemdatenträger** | **Laufwerkbuchstabe** | **Datentyp auf dem Datenträger**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Betriebssystem-Datenträger
DB-Disk1| Disk1 | D:\ | SQL-Systemdatenbank und Benutzerdatenbank 1
DB-Disk2 (Datenträger von Schutz ausgeschlossen) | Disk2 | E:\ | Temporäre Dateien
DB-Disk3 (Datenträger von Schutz ausgeschlossen) | Disk3 | F:\ | SQL-tempdb-Datenbank (Ordnerpfad F:\MSSQL\Data\) <br /> <br />Notieren Sie sich den Ordnerpfad vor dem Failover.
DB-Disk4 | Disk4 |G:\ |Benutzerdatenbank 2

Zwei der Datenträger des virtuellen Computers verfügen über temporäre Datenänderungen. Schützen Sie daher zwar den virtuellen Computer „SalesDB“, aber schließen Sie Disk2 und Disk3 von der Replikation aus. Azure Site Recovery repliziert diese Datenträger nicht. Bei einem Failover sind diese Datenträger nicht auf dem virtuellen Failovercomputer in Azure vorhanden.

Folgende Datenträger befinden sich nach einem Failover auf dem virtuellen Azure-Computer:

**Gastbetriebssystemdatenträger** | **Laufwerkbuchstabe** | **Datentyp auf dem Datenträger**
--- | --- | ---
DISK0 | C:\ | Betriebssystem-Datenträger
Disk1 | E:\ | Temporärer Speicher<br /> <br />Azure fügt diesen Datenträger hinzu und weist den ersten verfügbaren Laufwerkbuchstaben zu.
Disk2 | D:\ | SQL-Systemdatenbank und Benutzerdatenbank 1
Disk3 | G:\ | Benutzerdatenbank 2

Da „Disk2“ und „Disk3“ vom virtuellen SalesDB-Computer ausgeschlossen wurden, ist „E:“ der erste verfügbare Laufwerkbuchstabe. Azure weist „E:“ dem temporären Speichervolume zu. Für alle replizierten Datenträger bleiben die Laufwerkbuchstaben unverändert.

Disk3, der Datenträger für SQL-tempdb-Datenträger (tempdb-Ordnerpfad: F:\MSSQL\Data\), wurde von der Replikation ausgeschlossen. Der Datenträger ist nicht auf dem virtuellen Failovercomputer verfügbar. Daher befindet sich der SQL-Dienst im Status „Beendet“ und benötigt den Pfad „F:\MSSQL\Data“.

Es gibt zwei Möglichkeiten, um diesen Pfad zu erstellen:

- Fügen Sie einen neuen Datenträger hinzu, und weisen Sie den tempdb-Ordnerpfad zu.
- Verwenden Sie einen vorhandenen temporären Speicherdatenträger für den tempdb-Ordnerpfad.

### <a name="add-a-new-disk"></a>Hinzufügen eines neuen Datenträgers:

1. Notieren Sie sich vor dem Failover den SQL-Pfad für „tempdb.mdf“ und „tempdb.ldf“.
2. Fügen Sie dem virtuellen Failovercomputer über das Azure-Portal einen neuen Datenträger hinzu, der mindestens die gleiche Größe wie der SQL-tempdb-Quelldatenträger (Disk3) hat.
3. Melden Sie sich beim virtuellen Azure-Computer an. Initialisieren Sie über die Konsole für die Datenträgerverwaltung (diskmgmt.msc) den neu hinzugefügten Datenträger, und formatieren Sie sie.
4. Weisen Sie den gleichen Laufwerkbuchstaben zu, der für den SQL-tempdb-Datenträger verwendet wurde (F:).
5. Erstellen Sie auf dem Volume „F:“ einen tempdb-Ordner (F:\MSSQL\Data).
6. Starten Sie den SQL-Dienst über die Dienstkonsole.

### <a name="use-an-existing-temporary-storage-disk-for-the-sql-tempdb-folder-path"></a>Verwenden Sie einen vorhandenen temporären Speicherdatenträger für den SQL-tempdb-Ordnerpfad:

1. Öffnen Sie eine Eingabeaufforderung.
2. Führen Sie SQL Server im Wiederherstellungsmodus über die Eingabeaufforderung aus.

        Net start MSSQLSERVER /f / T3608

3. Führen Sie den folgenden sqlcmd-Befehl aus, um den tempdb-Pfad in den neuen Pfad zu ändern.

        sqlcmd -A -S SalesDB        **Use your SQL DBname**
        USE master;     
        GO      
        ALTER DATABASE tempdb       
        MODIFY FILE (NAME = tempdev, FILENAME = 'E:\MSSQL\tempdata\tempdb.mdf');
        GO      
        ALTER DATABASE tempdb       
        MODIFY FILE (NAME = templog, FILENAME = 'E:\MSSQL\tempdata\templog.ldf');       
        GO


4. Beenden Sie den Microsoft SQL Server-Dienst.

        Net stop MSSQLSERVER
5. Starten Sie den Microsoft SQL Server-Dienst.

        Net start MSSQLSERVER

Lesen Sie sich die folgenden Informationen zu Azure-Richtlinien für temporäre Speicherdatenträger durch:

* [Using SSDs in Azure VMs to store SQL Server TempDB and Buffer Pool Extensions](https://blogs.technet.microsoft.com/dataplatforminsider/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/) (Verwenden von SSDs in Azure-VMs zum Speichern von SQL Server-TempDB- und Pufferpoolerweiterungen)
* [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance)

## <a name="failback-from-azure-to-an-on-premises-host"></a>Failback (von Azure auf einen lokalen Host)
Als Nächstes werden die Datenträger beschrieben, die repliziert werden, wenn Sie ein Failover von Azure auf Ihren lokalen VMware-Host durchführen. Datenträger, die Sie in Azure manuell erstellen, werden nicht repliziert. Wenn Sie also beispielsweise ein Failover für drei Datenträger durchführen und zwei Datenträger direkt auf virtuellen Azure-Computern erstellen, erfolgt nur für die drei Datenträger, für die das Failover durchgeführt wurde, ein Failback. Manuell erstellte Datenträger können nicht in das Failback oder in den erneuten Schutz vom lokalen Standort auf Azure einbezogen werden. Außerdem werden keine temporären Speicherdatenträger auf lokalen Hosts repliziert.

### <a name="failback-to-original-location-recovery"></a>Failback zur Wiederherstellung am ursprünglichen Speicherort

Im vorherigen Beispiel lautet die Datenträgerkonfiguration des virtuellen Azure-Computers folgendermaßen:

**Gastbetriebssystemdatenträger** | **Laufwerkbuchstabe** | **Datentyp auf dem Datenträger**
--- | --- | ---
DISK0 | C:\ | Betriebssystem-Datenträger
Disk1 | E:\ | Temporärer Speicher<br /> <br />Azure fügt diesen Datenträger hinzu und weist den ersten verfügbaren Laufwerkbuchstaben zu.
Disk2 | D:\ | SQL-Systemdatenbank und Benutzerdatenbank 1
Disk3 | G:\ | Benutzerdatenbank 2

Wenn das Failback auf den ursprünglichen Standort durchgeführt wird, enthält die Datenträgerkonfiguration des virtuellen Failbackcomputers keine ausgeschlossenen Datenträger. Datenträger, die für „VMware auf Azure“ ausgeschlossen wurden, sind daher auf dem virtuellen Failbackcomputer nicht verfügbar.

Datenträger auf dem virtuellen VMWare-Computer (ursprünglicher Speicherort) nach dem geplanten Failover von Azure auf die lokale VMware-Instanz:

**Gastbetriebssystemdatenträger** | **Laufwerkbuchstabe** | **Datentyp auf dem Datenträger**
--- | --- | ---
DISK0 | C:\ | Betriebssystem-Datenträger
Disk1 | D:\ | SQL-Systemdatenbank und Benutzerdatenbank 1
Disk2 | G:\ | Benutzerdatenbank 2

## <a name="example-2-exclude-the-paging-file-pagefilesys-disk"></a>Beispiel 2: Ausschließen des Datenträgers der Auslagerungsdatei (pagefile.sys)

Angenommen, Sie verwenden einen virtuellen Computer mit einem Datenträger für die Auslagerungsdatei, der ausgeschlossen werden kann.
Es gibt zwei Fälle.

### <a name="case-1-the-paging-file-is-configured-on-the-d-drive"></a>Fall 1: Die Auslagerungsdatei ist auf Laufwerk „D:“ konfiguriert.
Die Datenträgerkonfiguration:

**Name des Datenträgers** | **Gastbetriebssystemdatenträger** | **Laufwerkbuchstabe** | **Datentyp auf dem Datenträger**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Betriebssystem-Datenträger
DB-Disk1 (Datenträger aus Schutz ausgeschlossen) | Disk1 | D:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Benutzerdaten 1
DB-Disk3 | Disk3 | F:\ | Benutzerdaten 2

Die Einstellungen der Auslagerungsdatei auf dem virtuellen Quellcomputer:

![Einstellungen der Auslagerungsdatei auf dem virtuellen Quellcomputer](./media/vmware-azure-exclude-disk/pagefile-on-d-drive-sourcevm.png)


Datenträger auf dem virtuellen Azure-Computer nach einem Failover des virtuellen Computers von VMware auf Azure:

**Name des Datenträgers** | **Gastbetriebssystemdatenträger** | **Laufwerkbuchstabe** | **Datentyp auf dem Datenträger**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Betriebssystem-Datenträger
DB-Disk1 | Disk1 | D:\ | Temporärer Speicher<br /> <br />pagefile.sys
DB-Disk2 | Disk2 | E:\ | Benutzerdaten 1
DB-Disk3 | Disk3 | F:\ | Benutzerdaten 2

Da „Disk1 (D:)“ ausgeschlossen wurde, ist „D:“ der erste verfügbare Laufwerkbuchstabe. Azure weist „D:“ dem temporären Speichervolume zu. Da „D:“ auf dem virtuellen Azure-Computer verfügbar ist, bleibt die Einstellung der Auslagerungsdatei des virtuellen Computers identisch.

Die Einstellungen der Auslagerungsdatei auf dem virtuellen Azure-Computer:

![Einstellungen der Auslagerungsdatei auf dem virtuellen Azure-Computer](./media/vmware-azure-exclude-disk/pagefile-on-azure-vm-after-failover.png)

### <a name="case-2-the-paging-file-is-configured-on-another-drive-other-than-d-drive"></a>Fall 2: Die Auslagerungsdatei ist auf einem anderen Laufwerk konfiguriert (nicht auf Laufwerk „D:“).

Die Datenträgerkonfiguration des virtuellen Quellcomputers:

**Name des Datenträgers** | **Gastbetriebssystemdatenträger** | **Laufwerkbuchstabe** | **Datentyp auf dem Datenträger**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Betriebssystem-Datenträger
DB-Disk1 (Datenträger von Schutz ausgeschlossen) | Disk1 | G:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Benutzerdaten 1
DB-Disk3 | Disk3 | F:\ | Benutzerdaten 2

Die Einstellungen der Auslagerungsdatei auf dem lokalen virtuellen Computer:

![Einstellungen der Auslagerungsdatei auf dem lokalen virtuellen Computer](./media/vmware-azure-exclude-disk/pagefile-on-g-drive-sourcevm.png)

Datenträger auf dem virtuellen Azure-Computer nach einem Failover des virtuellen Computers von VMware auf Azure:

**Name des Datenträgers** | **Gastbetriebssystemdatenträger** | **Laufwerkbuchstabe** | **Datentyp auf dem Datenträger**
--- | --- | --- | ---
DB-Disk0-OS | DISK0  |C:\ |Betriebssystem-Datenträger
DB-Disk1 | Disk1 | D:\ | Temporärer Speicher<br /> <br />pagefile.sys
DB-Disk2 | Disk2 | E:\ | Benutzerdaten 1
DB-Disk3 | Disk3 | F:\ | Benutzerdaten 2

Da „D:“ der erste verfügbare Laufwerkbuchstabe ist, weist Azure dem Volume für die temporäre Speicherung „D:“ zu. Für alle replizierten Datenträger bleibt der Laufwerkbuchstabe gleich. Da der Datenträger „G:“ nicht verfügbar ist, verwendet das System das Laufwerk „C:“ für die Auslagerungsdatei.

Die Einstellungen der Auslagerungsdatei auf dem virtuellen Azure-Computer:

![Einstellungen der Auslagerungsdatei auf dem virtuellen Azure-Computer](./media/vmware-azure-exclude-disk/pagefile-on-azure-vm-after-failover-2.png)

## <a name="next-steps"></a>Nächste Schritte
Nachdem die Bereitstellung eingerichtet wurde und ausgeführt wird, können Sie sich über die unterschiedlichen Failoverarten [informieren](site-recovery-failover.md) .
