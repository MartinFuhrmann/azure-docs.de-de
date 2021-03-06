---
title: Migrieren von der Sprachübersetzungs-API zum Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Informieren Sie sich, wie Sie Ihre Anwendungen von der Sprachübersetzungs-API zum Speech-Dienst migrieren.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: ba9484bd5b04e5a79da53a0bb78877153be42a43
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74805908"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Migrieren von der Sprachübersetzungs-API zum Speech-Dienst

Verwenden Sie diesen Artikel, um Ihre Anwendungen von der Sprachübersetzungs-API von Microsoft zum [Speech-Dienst](index.md) zu migrieren. Diese Anleitung beschreibt die Unterschiede zwischen der Sprachübersetzungs-API von Microsoft und dem Speech-Dienst und schlägt Strategien für die Migration Ihrer Anwendungen vor.

> [!NOTE]
> Ihr Abonnementschlüssel für die Sprachübersetzungs-API wird nicht vom Speech-Dienst akzeptiert. Sie müssen ein neues Abonnement für den Speech-Dienst erstellen.

## <a name="comparison-of-features"></a>Vergleich der Features

| Feature                                           | Sprachübersetzungs-API                                  | Speech-Dienst | Details                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Übersetzung in Text                               | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Übersetzung in Sprache                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Globaler Endpunkt                                   | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Der Speech-Dienst bietet keinen globalen Endpunkt. Ein globaler Endpunkt kann den Datenverkehr automatisch zum nächstgelegenen regionalen Endpunkt leiten, wodurch die Latenz in Ihrer Anwendung verringert wird.                                                    |
| Regionale Endpunkte                                | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Verbindungszeitlimit                             | 90 Minuten                                               | Mit SDK unbegrenzt 10 Minuten mit einer WebSockets-Verbindung.                                                                                                                                                                                                                                                                                   |
| Authentifizierungsschlüssel im Header                                | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Mehrere Sprachen in einer einzelnen Anforderung übersetzt | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| SDKs verfügbar                                    | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Informationen zu verfügbaren SDKs finden Sie in der [Dokumentation zum Speech-Dienst](index.md).                                                                                                                                                    |
| WebSockets-Verbindungen                            | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Sprachen-API                                     | :heavy_check_mark:                                              | :heavy_minus_sign:                 | Der Speech-Dienst unterstützt denselben Sprachumfang, der im Artikel [Sprachenreferenz für Sprachübersetzungs-API](../translator-speech/languages-reference.md) beschrieben wird. |
| Filter und Markierung für Obszönitäten                       | :heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| .WAV/PCM als Eingabe                                 | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Andere Dateitypen als Eingabe                         | :heavy_minus_sign:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Teilergebnisse                                   | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Informationen zur zeitlichen Steuerung                                       | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                 |
| Korrelations-ID                                    | :heavy_check_mark:                                              | :heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Benutzerdefinierte Spracherkennungsmodelle                              | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Der Speech-Dienst bietet benutzerdefinierte Sprachmodelle, mit denen Sie die Spracherkennung an das spezifische Vokabular Ihres Unternehmens anpassen können.                                                                                                                                           |
| Benutzerdefinierte Übersetzungsmodelle                         | :heavy_minus_sign:                                              | :heavy_check_mark:                 | Wenn Sie die Textübersetzungs-API von Microsoft abonnieren, können Sie den [benutzerdefinierten Translator](https://www.microsoft.com/translator/business/customization/) verwenden, um Ihre eigenen Daten für präzisere Übersetzungen zu verwenden.                                                 |

## <a name="migration-strategies"></a>Migrationsstrategien

Wenn Sie oder Ihr Unternehmen Anwendungen in der Entwicklungs- oder Produktionsumgebung nutzen, die die Sprachübersetzungs-API verwenden, sollten Sie diese aktualisieren, um den Speech-Dienst nutzen zu können. Informationen zu verfügbaren SDKs, Codebeispielen und Tutorials finden Sie in der Dokumentation zum [Speech-Dienst](index.md). Berücksichtigen Sie bei der Migration Folgendes:

* Der Speech-Dienst bietet keinen globalen Endpunkt. Ermitteln Sie, ob Ihre Anwendung effizient funktioniert, wenn sie einen einzigen regionalen Endpunkt für den gesamten Datenverkehr verwendet. Wenn nicht, verwenden Sie die Geolokalisierung, um den effizientesten Endpunkt zu bestimmen.

* Wenn Ihre Anwendung langlebige Verbindungen verwendet und die verfügbaren SDKs nicht nutzen kann, können Sie eine WebSockets-Verbindung verwenden. Steuern Sie das Zeitlimit von 10 Minuten, indem Sie die Verbindung zum jeweils richtigen Zeitpunkt wiederherstellen.

* Wenn Ihre Anwendung die Textübersetzungs-API und die Sprachübersetzungs-API verwendet, um benutzerdefinierte Übersetzungsmodelle zu aktivieren, können Sie mithilfe des Speech-Diensts direkt Kategorie-IDs hinzufügen.

* Im Gegensatz zur Sprachübersetzungs-API kann der Speech-Dienst Übersetzungen in mehrere Sprachen in einer einzigen Anforderung durchführen.

## <a name="next-steps"></a>Nächste Schritte

* [Speech-Dienst kostenlos testen](get-started.md)
* [Schnellstart: Erkennen von Sprache in einer UWP-App mit dem Speech SDK](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp)

## <a name="see-also"></a>Weitere Informationen

* [Worum handelt es sich beim Spracherkennungsdienst?](overview.md)
* [Dokumentation zum Speech-Dienst und Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
