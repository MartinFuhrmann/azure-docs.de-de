---
title: Erstellen eines Dashboards für die Selektierung von Gesundheitsdaten mit Azure IoT Central | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe von Azure IoT Central-Anwendungsvorlagen ein Dashboard für die Selektierung von Gesundheitsdaten erstellen.
author: philmea
ms.author: philmea
ms.date: 10/23/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: eliotgra
ms.openlocfilehash: 40e850bcbd177b15c91e57ec369c6b04963ffb84
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74132284"
---
# <a name="tutorial-build-a-power-bi-provider-dashboard"></a>Tutorial: Erstellen eines Power BI-Anbieterdashboards

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Im Rahmen einer Lösung für die kontinuierliche Patientenüberwachung möchten Sie auch ein Dashboard erstellen, mit dem das Pflegepersonal im Krankenhaus Patientendaten visualisieren kann. In diesem Tutorial lernen Sie Schritt für Schritt, wie Sie aus Ihrer IoT Central-Vorlage für eine Anwendung zur kontinuierlichen Patientenüberwachung ein Power BI-Dashboard mit Echtzeitstreaming erstellen.

>[!div class="mx-imgBorder"]
>![GIF-Abbildung des Dashboards](media/dashboard-gif-3.gif)

Die Basisarchitektur weist die folgende Struktur auf:

>[!div class="mx-imgBorder"] 
>![Dashboard für die Anbieterselektierung](media/dashboard-architecture.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Exportieren von Daten aus Azure IoT Central nach Azure Event Hubs
> * Einrichten eines Power BI-Streamingdatasets
> * Herstellen einer Verbindung zwischen Ihrer Logik-App und Azure Event Hubs
> * Streamen von Daten aus Ihrer Logik-App nach Power BI
> * Erstellen eines Echtzeitdashboards für die Vitalfunktionen von Patienten

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Eine Azure IoT Central-Vorlage für eine Anwendung zur kontinuierlichen Patientenüberwachung. Wenn Sie noch nicht über eine solche Vorlage verfügen, führen Sie die Schritte zum [Bereitstellen einer Anwendungsvorlage](overview-iot-central-healthcare.md) aus.

* Ein Azure [Event Hubs-Namespace und ein Event Hub](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

* Die Logik-App, in der Sie auf Ihren Event Hub zugreifen möchten. Um Ihre Logik-App mit einem Azure Event Hubs-Trigger starten zu können, benötigen Sie eine [leere Logik-App](https://docs.microsoft.com/azure/logic-apps/quickstart-create-first-logic-app-workflow).

* Ein Power BI-Dienstkonto. Wenn Sie noch nicht über ein solches Konto verfügen, können Sie [ein kostenloses Testkonto für den Power BI-Dienst erstellen](https://app.powerbi.com/). Wenn Sie Power BI noch nicht verwendet haben, sollten Sie zunächst das Tutorial [Erste Schritte mit Power BI](https://docs.microsoft.com/power-bi/service-get-started) lesen.

## <a name="set-up-a-continuous-data-export-to-azure-event-hubs"></a>Einrichten eines kontinuierlichen Datenexports nach Azure Event Hubs
Sie müssen zuerst einen kontinuierlichen Datenexport aus Ihrer Azure IoT Central-App-Vorlage an die Azure Event Hub-Instanz in Ihrem Abonnement einrichten. Befolgen Sie dazu die Schritte zum [Exportieren nach Event Hubs](https://docs.microsoft.com/azure/iot-central/preview/howto-export-data) im vorliegenden Azure IoT Central-Tutorial. Für dieses Tutorial müssen Sie nur die Telemetriedaten exportieren.

## <a name="create-a-power-bi-streaming-dataset"></a>Erstellen eines Power BI-Streamingdatasets

1. Melden Sie sich bei Ihrem Power BI-Konto an.

2. Erstellen Sie in Ihrem bevorzugten Arbeitsbereich ein neues Streamingdataset, indem Sie in der oberen rechten Ecke der Symbolleiste auf die Schaltfläche **+ Erstellen** klicken. Sie müssen ein separates Dataset für jeden Patienten erstellen, dessen Daten auf dem Dashboard angezeigt werden sollen.

    >[!div class="mx-imgBorder"] 
    >![Erstellen des Streamingdatasets](media/create-streaming-dataset.png)

3. Wählen Sie als **API** als Quelle für Ihr Dataset aus.

4. Geben Sie einen **Namen** (z. B. den Namen eines Patienten) für Ihr Dataset sein, und füllen Sie dann die Werte aus Ihrem Stream aus. Unten sehen Sie ein Beispiel, das auf Werten aus den simulierten Geräten in der Vorlage für eine Anwendung zur kontinuierlichen Patientenüberwachung basiert. Das Beispiel weist zwei Patienten auf:

    * Teddy Silvers, für den Daten aus „Smart Knee Brace“ vorliegen
    * Yesenia Sanford, für die Daten aus „Smart Vitals Patch“ vorliegen

    >[!div class="mx-imgBorder"] 
    >![Eingeben von Datasetwerten](media/enter-dataset-values.png)

Weitere Informationen zu Streamingdatasets in Power BI finden Sie im Dokument [Echtzeitstreaming in Power BI](https://docs.microsoft.com/power-bi/service-real-time-streaming).

## <a name="connect-your-logic-app-to-azure-event-hubs"></a>Herstellen einer Verbindung zwischen Ihrer Logik-App und Azure Event Hubs
Um eine Verbindung zwischen Ihrer Logik-App und Azure Event Hubs herzustellen, befolgen Sie die Anweisungen im Dokument [Senden von Ereignissen mit Azure Event Hubs und Azure Logic Apps](https://docs.microsoft.com/azure/connectors/connectors-create-api-azure-event-hubs#add-event-hubs-action). Im Folgenden finden Sie einige empfohlene Parameter:

|Parameter|Wert|
|---|---|
|Content-Typ|Anwendung/json|
|Intervall|3|
|Frequency|Sekunde|

Am Ende dieses Schritts sollte Ihr Logik-App-Designer wie folgt aussehen:

>[!div class="mx-imgBorder"] 
>![Verbindung von Logic Apps mit Event Hubs](media/eh-logic-app.png)

## <a name="stream-data-to-power-bi-from-your-logic-app"></a>Streamen von Daten aus Ihrer Logik-App nach Power BI
Im nächsten Schritt analysieren Sie die Daten aus Ihrem Event Hub, um sie in die zuvor erstellten Power BI-Datasets zu streamen.

1. Bevor Sie diesen Schritt ausführen, müssen Sie die JSON-Nutzlast kennen, die von Ihrem Gerät an Ihren Event Hub gesendet wird. Sehen Sie sich dafür dieses [Beispielschema](https://docs.microsoft.com/azure/iot-central/preview/howto-export-data#telemetry) an, und ändern Sie es so, dass es Ihrem Schema entspricht, oder verwenden Sie den [Service Bus-Explorer](https://github.com/paolosalvatori/ServiceBusExplorer), um die Meldungen zu untersuchen. Wenn Sie die Anwendungen zur kontinuierlichen Patientenüberwachung verwenden, sehen die Meldungen etwa wie folgt aus:

**Smart Vitals Patch-Telemetrie**

```json
{
  "HeartRate": 80,
  "RespiratoryRate": 12,
  "HeartRateVariability": 64,
  "BodyTemperature": 99.08839032397609,
  "BloodPressure": {
    "Systolic": 23,
    "Diastolic": 34
  },
  "Activity": "walking"
}
```

**Smart Knee Brace-Telemetrie**

```json
{
  "Acceleration": {
    "x": 72.73510947763711,
    "y": 72.73510947763711,
    "z": 72.73510947763711
  },
  "RangeOfMotion": 123,
  "KneeBend": 3
}
```

**Properties**

```json
{
  "iothub-connection-device-id": "1qsi9p8t5l2",
  "iothub-connection-auth-method": "{\"scope\":\"device\",\"type\":\"sas\",  \"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
  "iothub-connection-auth-generation-id": "637063718586331040",
  "iothub-enqueuedtime": 1571681440990,
  "iothub-message-source": "Telemetry",
  "iothub-interface-name": "Patient_health_data_3bk",
  "x-opt-sequence-number": 7,
  "x-opt-offset": "3672",
  "x-opt-enqueued-time": 1571681441317
}
```

2. Nachdem Sie die JSON-Nutzlasten überprüft haben, wechseln Sie wieder zum Logik-App-Designer und wählen **+ Neuer Schritt** aus. Suchen Sie nach **Variable initialisieren**, fügen Sie diesen Punkt als nächsten Schritt hinzu, und geben Sie die folgenden Parameter ein:

    |Parameter|Wert|
    |---|---|
    |NAME|Schnittstellenname|
    |type|Zeichenfolge|

    Klicken Sie auf **Speichern**. 

3. Fügen Sie eine weitere Variable namens **Körper** mit dem Typ **string** hinzu. Ihrer Logik-App werden die folgenden Aktionen hinzugefügt:

    >[!div class="mx-imgBorder"]
    >![Variablen initialisieren](media/initialize-string-variables.png)
    
4. Wählen Sie **+ Neuer Schritt** aus, und fügen Sie eine Aktion **JSON analysieren** hinzu. Benennen Sie diese in **Eigenschaften analysieren** um. Wählen Sie als Inhalt **Eigenschaften** aus dem Event Hub aus. Klicken Sie unten auf **Beispielnutzlast zum Generieren eines Schemas verwenden**, und fügen Sie die Beispielnutzlast aus dem obigen Abschnitt „Eigenschaften“ ein.

5. Wählen Sie als Nächstes die Aktion **Variable festlegen** aus, und aktualisieren Sie die Variable **Schnittstellenname** mit dem Wert für **iothub-interface-name** aus den analysierten JSON-Eigenschaften.

6. Fügen Sie als nächste Aktion ein **Teiler**-Steuerelement hinzu, und wählen Sie die Variable **Schnittstellenname** als Parameter für „Ein“ aus. Dieses verwenden Sie als Trichter, um die Daten in das richtige Dataset zu leiten.

7. Suchen Sie in Ihrer Azure IoT Central-Anwendung nach dem Schnittstellennamen für die Daten für „Smart Vitals Patch“ und „Smart Knee Brace“ aus der Ansicht **Gerätevorlagen**. Erstellen Sie zwei verschiedene Fälle für das **Schalter**-Steuerelement für jeden Schnittstellennamen, und benennen Sie das Steuerelement entsprechend. Sie können den Standardfall für die Verwendung des Steuerelements **Beenden** verwenden und auswählen, welcher Status angezeigt werden soll.

    >[!div class="mx-imgBorder"] 
    >![Teiler-Steuerelement](media/split-by-interface.png)

8. Fügen Sie für den Fall **Smart Vitals Patch** eine Aktion **JSON analysieren** hinzu. Wählen Sie als Inhalt die Option **Inhalt** aus dem Event Hub aus. Kopieren Sie die oben genannten Beispielnutzlasten für „Smart Vitals Patch“, und fügen Sie sie ein, um das Schema zu generieren.

9. Fügen Sie eine Aktion **Variable festlegen** hinzu, und aktualisieren Sie die Variable **Körper** mit den Werten für **Körper** aus dem analysierten JSON-Code in Schritt 7.

10. Fügen Sie als nächste Aktion ein Steuerelement **Bedingung** hinzu, und legen Sie die Bedingung auf **Körper**, **enthält**, **HeartRate** fest. Damit wird sichergestellt, dass der richtige Datensatz aus „Smart Vitals Patch“ verwendet wird, bevor Sie das Power BI-Dataset auffüllen. Die Schritte 7–9 sehen folgendermaßen aus:

    >[!div class="mx-imgBorder"] 
    >![Zusätzliche Smart Vitals-Bedingung](media/smart-vitals-pbi.png)

11. Fügen Sie für den Fall **TRUE** der Bedingung eine Aktion hinzu, die die Power BI-Funktion **Zeilen zu einem Dataset hinzufügen** aufruft. Zu diesem Zweck müssen Sie sich bei Power BI anmelden. Der Fall **FALSE** kann wieder das Steuerelement **Beenden** verwenden.

12. Wählen Sie geeignete Werte für **Arbeitsbereich**, **Dataset** und **Tabelle** aus. Ordnen Sie die Parameter, die Sie beim Erstellen des Streamingdatasets in Power BI angegeben haben, den analysierten JSON-Werte zu, die aus Ihrem Event Hub gesendet werden. Die ausgefüllten Aktionen sollten wie folgt aussehen:

    >[!div class="mx-imgBorder"] 
    >![Hinzufügen von Zeilen zu Power BI](media/add-rows-yesenia.png)

13. Fügen Sie für den Schalter-Fall **Smart Knee Brace** eine Aktion **JSON analysieren** ein, um den Inhalt zu analysieren, ähnlich wie in Schritt 7. Verwenden Sie **Zeilen zu einem Dataset hinzufügen**, um das Dataset für Teddy Silvers in Power BI zu aktualisieren.

    >[!div class="mx-imgBorder"] 
    >![Zusätzliche Smart Vitals-Bedingung](media/knee-brace-pbi.png)

14. Klicken Sie auf **Speichern**, und führen Sie dann Ihre Logik-App aus.

## <a name="build-a-real-time-dashboard-for-patient-vitals"></a>Erstellen eines Echtzeitdashboards für die Vitalfunktionen von Patienten
Wechseln Sie jetzt zurück zu Power BI, und klicken Sie auf **+ Erstellen**, um ein neues **Dashboard** zu erstellen. Benennen Sie das Dashboard, und klicken Sie auf **Erstellen**.

Klicken Sie auf die drei Auslassungspunkte in der Navigationsleiste oben, wählen Sie dann **+ Kachel hinzufügen** aus.

>[!div class="mx-imgBorder"] 
>![Hinzufügen einer Kachel zum Dashboard](media/add-tile.png)

Wählen Sie einen Kacheltyp aus, und passen Sie Ihre App nach Ihren Wünschen an.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht weiterverwenden möchten, löschen Sie Ihre Ressourcen wie folgt:

1. Im Azure-Portal können Sie die erstellten Event Hub- und Logic Apps-Ressourcen löschen.

2. Wechseln Sie in Ihrer IoT Central-Anwendung zur Registerkarte „Verwaltung“, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie den Leitfaden für die [Architektur zur kontinuierlichen Überwachung von Patienten](concept-continuous-patient-monitoring-architecture.md).
