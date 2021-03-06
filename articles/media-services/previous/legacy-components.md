---
title: Legacy-Komponenten von Azure Media Services | Microsoft-Dokumentation
description: In diesem Thema werden die Legacy-Komponenten von Azure Media Services behandelt.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2019
ms.author: juliako
ms.openlocfilehash: 6a61c868da7b979b1ac5654f4be93b5245179105
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2019
ms.locfileid: "74423916"
---
# <a name="azure-media-services-legacy-components"></a>Legacy-Komponenten von Azure Media Services

Im Laufe der Zeit wurden die Media Service-Komponenten kontinuierlich verfeinert und verbessert. Infolgedessen wurden einige Legacy-Komponenten eingestellt. Die Anweisungen zum Migrieren Ihrer Anwendung von der Legacy-Komponente zu einer aktuellen Komponente finden Sie in den folgenden Artikeln.
 
## <a name="retirement-plans-of-legacy-components-and-migration-guidance"></a>Deaktivierungspläne für Legacy-Komponenten und Hinweise zur Migration

Wir kündigen die Einstellung der Medienprozessoren *Windows Azure Media Encoder* (WAME) und *Azure Media Encoder* (AME) an. Diese Prozessoren werden am 31. März 2020 eingestellt.

* [Migrieren von Windows Azure Media Encoder zu Media Encoder Standard](migrate-windows-azure-media-encoder.md)
* [Migrieren von Azure Media Encoder zu Media Encoder Standard](migrate-azure-media-encoder.md)

Außerdem wird die Deaktivierung der folgenden Media Analytics Medienprozessoren angekündigt: 
 
|Medienprozessorname|Deaktivierungstermin|Zusätzliche Hinweise|
|---|---|
|[Azure Media Indexer 2](media-services-process-content-with-indexer2.md)| 1\. Januar 2020|Dieser Medienprozessor wird vom [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) ersetzt. Weitere Informationen finden Sie unter [Migration von Azure Media Indexer 2 zu Azure Media Services Video Indexer](migrate-indexer-v1-v2.md).|
|[Azure Media Indexer](media-services-index-content.md)|1\. Oktober 2020|Dieser Medienprozessor wird vom [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) ersetzt. Weitere Informationen finden Sie unter [Migration von Azure Media Indexer zu Azure Media Services Video Indexer](migrate-indexer-v1-v2.md).

## <a name="next-steps"></a>Nächste Schritte

[Hinweise zur Migration von Media Services v2 zu v3](../latest/migrate-from-v2-to-v3.md)
