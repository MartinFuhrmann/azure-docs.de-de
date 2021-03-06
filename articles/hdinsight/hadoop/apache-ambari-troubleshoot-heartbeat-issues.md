---
title: Apache Ambari-Heartbeatprobleme in Azure HDInsight
description: Bewertung verschiedene Gründe für Apache Ambari-Heartbeatprobleme in Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 09/11/2019
ms.openlocfilehash: 9f7d3fb5363a5eb2d79287bc9a9398bfdcbc4aec
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71087818"
---
# <a name="apache-ambari-heartbeat-issues-in-azure-hdinsight"></a>Apache Ambari-Heartbeatprobleme in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="scenario-high-cpu-utilization"></a>Szenario: Hohe CPU-Auslastung

### <a name="issue"></a>Problem

Beim Ambari-Agent gibt es eine hohe CPU-Auslastung. Dies führt zu Warnungen von der Ambari-Benutzeroberfläche, dass einige Knoten keinen Heartbeat des Ambari-Agents mehr empfangen. Die Warnung zum Verlust des Heartbeats ist normalerweise vorübergehend. 

### <a name="cause"></a>Ursache

In seltenen Fällen kann es aufgrund verschiedener Ambari-Agent-Fehler bei Ihrem Agent eine hohe CPU-Auslastung (nahezu 100 %) geben.

### <a name="resolution"></a>Lösung

1. Identifizieren Sie die Prozess-ID (PID) des Ambari-Agents:

    ```bash
    ps -ef | grep ambari_agent
    ```

1. Führen Sie anschließend den folgenden Befehl aus, um die CPU-Auslastung anzuzeigen:

    ```bash
    top -p <ambari-agent-pid>
    ```

1. Starten Sie Ambari-Agent neu, um das Problem zu beheben:

    ```bash
    service ambari-agent restart
    ```

1. Wenn der Neustart nicht funktioniert, beenden Sie den Ambari-Agent-Prozess, und starten Sie ihn dann:

    ```bash
    kill -9 <ambari-agent-pid>
    service ambari-agent start
    ```

---

## <a name="scenario-ambari-agent-not-started"></a>Szenario: Ambari-Agent wurde nicht gestartet

### <a name="issue"></a>Problem

Ambari-Agent wurde nicht gestartet. Dies führt zu Warnungen von der Ambari-Benutzeroberfläche, dass einige Knoten keinen Heartbeat des Ambari-Agents mehr empfangen.

### <a name="cause"></a>Ursache

Die Warnungen werden dadurch verursacht, dass der Ambari-Agent nicht ausgeführt wird.

### <a name="resolution"></a>Lösung

1. Bestätigen Sie den Status von Ambari-Agent:

    ```bash
    service ambari-agent status
    ```

1. Vergewissern Sie sich, dass die Failovercontrollerdienste ausgeführt werden:

    ```bash
    ps -ef | grep failover
    ```

    Wenn die Failovercontrollerdienste nicht ausgeführt werden, liegt wahrscheinlich ein Problem vor, das den Start des Failovercontrollers durch den HDInsight-Agent verhindert. Überprüfen Sie das Protokoll des HDInsight-Agents aus der Datei `/var/log/hdinsight-agent/hdinsight-agent.out`.

---

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
