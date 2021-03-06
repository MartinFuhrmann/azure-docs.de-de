---
title: Joins in Azure Monitor-Protokollabfragen | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Lektion zur Verwendung von Joins in Azure Monitor-Protokollabfragen.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: f2880044e48e59d0d5f005f9772cdd0f807f7f29
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75397838"
---
# <a name="joins-in-azure-monitor-log-queries"></a>Joins in Azure Monitor-Protokollabfragen

> [!NOTE]
> Sie sollten zunächst [Erste Schritte mit Azure Monitor Log Analytics](get-started-portal.md) und [Erste Schritte mit Azure Monitor-Protokollabfragen](get-started-queries.md) lesen, bevor Sie mit dieser Lektion beginnen.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Mithilfe von Joins können Sie Daten aus mehreren Tabellen in derselben Abfrage analysieren. Diese führen die Zeilen zweier Datasets anhand von übereinstimmenden Werten der angegebenen Spalte zusammen.


```Kusto
SecurityEvent 
| where EventID == 4624     // sign-in events
| project Computer, Account, TargetLogonId, LogonTime=TimeGenerated
| join kind= inner (
    SecurityEvent 
    | where EventID == 4634     // sign-out events
    | project TargetLogonId, LogoffTime=TimeGenerated
) on TargetLogonId
| extend Duration = LogoffTime-LogonTime
| project-away TargetLogonId1 
| top 10 by Duration desc
```

In diesem Beispiel filtert das erste Dataset alle Anmeldeereignisse. Dieses Dataset ist mit einem zweiten Dataset verknüpft, das alle Abmeldeereignisse filtert. Die projizierten Spalten sind _Computer_, _Account_, _TargetLogonId_ und _TimeGenerated_. Die Datasets werden von einer freigegebenen Spalte (_TargetLogonId_) korreliert. Die Ausgabe ist ein einzelnes Dataset pro Korrelation, das die An- und Abmeldezeit enthält.

Wenn beide Datasets Spalten mit dem gleichen Namen enthalten, wird den Spalten des rechten Datasets eine Indexnummer hinzugefügt. In diesem Beispiel würden die Ergebnisse _TargetLogonId_ mit Werten der linken Tabelle und _TargetLogonId1_ mit Werten der rechten Tabelle anzeigen. In diesem Fall wurde die zweite _TargetLogonId1_-Spalte mithilfe des `project-away`-Operators entfernt.

> [!NOTE]
> Behalten Sie mithilfe des `project`-Operators nur die relevanten Spalten der verknüpften Datasets bei, um die Leistung zu verbessern.


Verwenden Sie die folgende Syntax, um zwei Datasets zu verknüpfen. Der verknüpfte Schlüssel hat in den beiden Tabellen einen unterschiedlichen Namen:
```
Table1
| join ( Table2 ) 
on $left.key1 == $right.key2
```

## <a name="lookup-tables"></a>Nachschlagetabellen
Joins werden häufig verwendet, um die statische Zuordnung von Werten mithilfe von `datatable` zu verwenden. Diese kann bei der Transformation der Ergebnisse hilfreich sein, um diese besser darstellen zu können. So können Sie beispielsweise den Sicherheitsereignisdaten den Ereignisnamen für jede Ereignis-ID hinzufügen.

```Kusto
let DimTable = datatable(EventID:int, eventName:string)
  [
    4625, "Account activity",
    4688, "Process action",
    4634, "Account activity",
    4658, "The handle to an object was closed",
    4656, "A handle to an object was requested",
    4690, "An attempt was made to duplicate a handle to an object",
    4663, "An attempt was made to access an object",
    5061, "Cryptographic operation",
    5058, "Key file operation"
  ];
SecurityEvent
| join kind = inner
 DimTable on EventID
| summarize count() by eventName
```

![Join mit einer DataTable](media/joins/dim-table.png)

## <a name="join-kinds"></a>Join-Typen
Geben Sie den Join-Typ mit dem _kind_-Argument an. Jeder Typ führt einen anderen Abgleich zwischen den Datensätzen der angegebenen Tabellen durch. Dies wird in der folgenden Tabelle beschrieben.

| Join-Typ | BESCHREIBUNG |
|:---|:---|
| innerunique | Dies ist der Standardjoinmodus. Zuerst werden die Werte der übereinstimmenden Spalten in der linken Tabelle ermittelt, und doppelte Werte werden entfernt.  Dann werden die eindeutigen Werte mit der rechten Tabelle verglichen. |
| Innerer Join | Nur übereinstimmende Datensätze in beiden Tabellen sind in den Ergebnissen enthalten. |
| leftouter | Alle Datensätze in der linken Tabelle sowie übereinstimmende Datensätze in der rechten Tabelle sind in den Ergebnissen enthalten. Nicht übereinstimmende Ausgabeeigenschaften enthalten NULL-Werte.  |
| leftanti | Datensätze von der linken Seite, für die keine Übereinstimmungen auf der rechten Seite vorhanden sind, sind in den Ergebnissen enthalten. Die Ergebnistabelle enthält nur Spalten aus der linken Tabelle. |
| leftsemi | Datensätze von der linken Seite, für die Übereinstimmungen auf der rechten Seite vorhanden sind, sind in den Ergebnissen enthalten. Die Ergebnistabelle enthält nur Spalten aus der linken Tabelle. |


## <a name="best-practices"></a>Bewährte Methoden

Berücksichtigen Sie folgende Punkte, um eine optimale Leistung zu erzielen:

- Verwenden Sie einen Zeitfilter für jede Tabelle, um die Datensätze zu reduzieren, die für den Join ausgewertet werden müssen.
- Verwenden Sie `where` und `project`, um die Anzahl von Zeilen und Spalten in den Eingabetabellen vor dem Join zu reduzieren.
- Wenn eine Tabelle immer kleiner als die andere ist, verwenden Sie diese als die linke Seite der Verknüpfung.


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung von Azure Monitor-Protokollabfragen finden Sie in folgenden weiteren Lektionen:

- [Zeichenfolgenvorgänge](string-operations.md)
- [Aggregationsfunktionen](aggregations.md)
- [Erweiterte Aggregationen](advanced-aggregations.md)
- [JSON und Datenstrukturen](json-data-structures.md)
- [Schreiben von erweiterten Abfragen](advanced-query-writing.md)
- [Diagramme](charts.md)
