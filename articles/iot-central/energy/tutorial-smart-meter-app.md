---
title: 'Tutorial: Erstellen einer Analyse-App für intelligente Zähler mit IoT Central'
description: 'Tutorial: Erfahren Sie, wie Sie eine Überwachungsanwendung für intelligente Zähler mithilfe von Azure IoT Central-Anwendungsvorlagen erstellen.'
author: op-ravi
ms.author: omravi
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: a73f4d75811a384eb822d1f8594a22506509d560
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74112535"
---
# <a name="tutorial-create-and-walk-through-the-smart-meter-monitoring-app-template"></a>Tutorial: Erstellen und Durchlaufen der App-Vorlage für die intelligente Zählerüberwachung 

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In diesem Tutorial werden die Schritte zum Erstellen der Überwachungsanwendung für intelligente Zähler erläutert, die ein Beispielgerätemodell mit simulierten Daten enthält. In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> * Kostenloses Erstellen einer App für intelligente Zähler
> * Anwendungsablauf
> * Bereinigen von Ressourcen


Wenn Sie noch nicht über ein Abonnement verfügen, [erstellen Sie ein kostenloses Testkonto](https://azure.microsoft.com/free).

## <a name="prerequisites"></a>Voraussetzungen
- Keine
- Ein Azure-Abonnement wird empfohlen, ist aber für das Testen nicht erforderlich.

## <a name="create-a-smart-meter-monitoring-app"></a>Erstellen einer App für die Überwachung von intelligenten Stromzählern 

Sie können diese Anwendung in drei einfachen Schritten erstellen:

1. Öffnen Sie die [Azure IoT Central-Startseite](https://apps.azureiotcentral.com), und klicken Sie auf **Erstellen**, um eine neue Anwendung zu erstellen. 

2. Wählen Sie die Registerkarte **Energie** aus, und klicken Sie auf der Anwendungskachel **Überwachung eines intelligenten Zählers** auf **App erstellen**.

    > [!div class="mx-imgBorder"]
    > ![Erstellen einer App](media/tutorial-iot-central-smart-meter/smart-meter-build.png)
    

3. Über **App erstellen** wird das Formular **Neue Anwendung** geöffnet. Geben Sie wie in der nachfolgenden Abbildung dargestellt die erforderlichen Details ein:
    * **Anwendungsname**: Geben Sie einen Namen für die IoT Central-Anwendung an. 
    * **URL**: Geben Sie eine IoT Central-URL an. Die Eindeutigkeit wird auf der Plattform überprüft.
    * **Kostenlose 7-Tage-Testversion**: Wenn Sie bereits über ein Azure-Abonnement verfügen, wird die Standardeinstellung empfohlen. Wenn Sie noch nicht über ein Azure-Abonnement verfügen, beginnen Sie mit einer kostenlosen Testversion.
    * **Abrechnungsinfo**: Die Anwendung selbst ist kostenlos. Die Detailangaben zu Verzeichnis, Azure-Abonnement und Region sind für die Bereitstellung der Ressourcen für die App erforderlich.
    * Klicken Sie unten auf der Seite auf die Schaltfläche **Erstellen**. Die App wird innerhalb etwa einer Minute erstellt.     
        > [!div class="mx-imgBorder"]
        > ![Formular für neue Anwendung](media/tutorial-iot-central-smart-meter/smart-meter-create-new-app.png)


### <a name="verify-the-application-and-simulated-data"></a>Überprüfen der Anwendung und der simulierten Daten

Sie können Ihre neu erstellte App für intelligente Zähler jederzeit ändern. Stellen Sie sicher, dass die App wie erwartet bereitgestellt wurde und ausgeführt wird, bevor Sie sie ändern.

Wechseln Sie zum Überprüfen der App-Erstellung und der Datensimulation zum **Dashboard**. Wenn die Kacheln mit einigen Daten angezeigt werden, war die Bereitstellung Ihrer App erfolgreich. Bei der Datensimulation kann es einige Minuten dauern, bis die Daten generiert wurden. Warten Sie daher 1–2 Minuten. 

## <a name="application-walk-through"></a>Anwendungsablauf
Nachdem Sie die App-Vorlage erfolgreich bereitgestellt haben, enthält sie ein4n intelligentes BeispielZähler, ein Gerätemodell und ein Dashboard. 

Adatum ist ein fiktives Energieversorgungsunternehmen, das intelligente Zähler überwacht und verwaltet. Im Dashboard für die Überwachung von intelligenten Zählern werden die entsprechenden Eigenschaften, Daten und Beispielbefehle angezeigt. In diesem Dashboard können Bediener und Supportteams proaktiv die folgenden Aktivitäten durchführen, bevor daraus Supportfälle werden: 
* Überprüfen der aktuellen Informationen zum intelligenten Zähler und dessen Installationsposition auf der Karte
* Proaktives Überprüfen des Status des intelligenten Zählers und des Verbindungsstatus 
* Überwachen der minimalen und maximalen Spannungswerte für die Netzwerkintegrität 
* Überprüfen der Energie-, Leistung- und Spannungstrends zur Erfassung anomaler Muster 
* Nachverfolgen des gesamten Energieverbrauchs für Planungs- und Abrechnungszwecke
* Befehls- und Steuerungsvorgänge, z. B. erneutes Verbinden des Zählers und Aktualisieren der Firmwareversion In der Vorlage werden über die Befehlsschaltflächen die möglichen Funktionen angezeigt, jedoch keine realen Befehle gesendet. 

> [!div class="mx-imgBorder"]
> ![Dashboard für die intelligente Zählerüberwachung](media/tutorial-iot-central-smart-meter/smart-meter-dashboard.png)

### <a name="devices"></a>Geräte
Die App enthält ein Beispielgerät für einen intelligenten Zähler. Sie können die Gerätedetails anzeigen, indem Sie auf die Registerkarte **Geräte** klicken.

> [!div class="mx-imgBorder"]
> ![Intelligente Zähler](media/tutorial-iot-central-smart-meter/smart-meter-devices.png)

Klicken Sie auf den Link für das Beispielgerät **SM0123456789**, um die Gerätedetails anzuzeigen. Auf der Seite **Eigenschaften aktualisieren** können Sie die schreibbaren Eigenschaften des Gerätes aktualisieren und die aktualisierten Werte im Dashboard anzeigen.

> [!div class="mx-imgBorder"]
> ![Eigenschaften des intelligenten Zählers](media/tutorial-iot-central-smart-meter/smart-meter-device-properties.png)

### <a name="device-template"></a>Gerätevorlage
Klicken Sie auf die Registerkarte **Gerätevorlagen**, um das Gerätemodell für intelligente Zähler anzuzeigen. Das Modell umfasst eine vordefinierte Schnittstelle für Daten, Eigenschaften, Befehle und Ansichten.

> [!div class="mx-imgBorder"]
> ![Gerätevorlagen für intelligente Zähler](media/tutorial-iot-central-smart-meter/smart-meter-device-template.png)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie diese Anwendung nicht mehr verwenden möchten, können Sie sie mit den folgenden Schritten löschen:

1. Öffnen Sie im linken Bereich die Registerkarte „Verwaltung“.
2. Wählen Sie „Anwendungseinstellungen“ aus, und klicken Sie unten auf der Seite auf die Schaltfläche „Löschen“. 

    > [!div class="mx-imgBorder"]
    > ![Löschen der Anwendung](media/tutorial-iot-central-smart-meter/smart-meter-delete-app.png)


## <a name="next-steps"></a>Nächste Schritte
* Informationen zur App-Architektur für intelligente Zähler finden Sie im [Konzeptartikel](https://docs.microsoft.com/azure/iot-central/energy/concept-iot-central-smart-meter-app).
* Erstellen Sie Anwendungsvorlagen für intelligente Zähler kostenlos: [App für intelligente Zähler](https://apps.azureiotcentral.com/build/new/smart-meter-monitoring)
* Weitere Informationen zu IoT Central finden Sie in der [Übersicht über IoT Central](https://docs.microsoft.com/azure/iot-central/).
