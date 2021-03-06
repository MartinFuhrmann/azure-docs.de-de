---
title: include file
description: include file
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 11/04/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 21b982389b186e949b21352f4b11bd6b4aa06dcb
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74279207"
---
<!-- F-series, Fs-series* -->

Für Compute optimierte VM-Größen verfügen über ein hohes Verhältnis zwischen CPU und Arbeitsspeicher. Diese Größen sind ideal für Webserver, Netzwerkappliances, Batchvorgänge und Anwendungsserver mit mittlerer Auslastung. Dieser Artikel enthält Informationen über die Anzahl von vCPUs, Datenträgern und NICs. Er umfasst zudem Informationen zum Speicherdurchsatz und zur Netzwerkbandbreite für die jeweiligen Größen in dieser Gruppe.

Die Fsv2-Serie basiert auf dem Prozessor Intel® Xeon® Platinum 8168. Dieser verfügt über eine kontinuierliche Turbo-Taktfrequenz von 3,4 GHz für alle Kerne und eine maximale Turbofrequenz von 3,7 GHz für Einzelkerne. Intel® AVX-512-Anweisungen sind neu auf von Intel skalierbaren Prozessoren. Diese Anweisungen können bei Gleitkommaoperationen mit einfacher und doppelter Genauigkeit mit Vektorverarbeitungsworkloads die Leistung verdoppeln. Sie bewältigen demnach alle Rechenworkloads wirklich schnell.

Die Fsv2-Serie hat einen niedrigeren Listenpreis pro Stunde und bietet auf Basis der Azure-Compute-Einheit (Azure Compute Unit, ACU) das beste Preis-Leistungs-Verhältnis pro vCPU im Azure-Portfolio.

## <a name="fsv2-series-sup1sup"></a>Fsv2-Serie <sup>1</sup>

ACU: 195–210

Storage Premium  Unterstützt

Storage Premium-Zwischenspeicherung:  Unterstützt

| Size             | vCPUs | Memory: GiB | Temporärer Speicher (SSD): GiB | Max. Anzahl Datenträger | Maximaler Durchsatz (Cache und temporärer Speicher): IOPS/MBps (Cachegröße in GiB) | Maximaler Durchsatz des Datenträgers ohne Cache: IOPS/MBps | Maximale Anzahl NICs/Erwartete Netzwerkbandbreite (Mbit/s) |
|------------------|--------|-------------|----------------|----------------|--------------------------|--------------------------|-------------------------|
| Standard_F2s_v2  | 2      | 4           | 16             | 4              | 4000/31 (32)           | 3200/47                | 2/875                 |
| Standard_F4s_v2  | 4      | 8           | 32             | 8              | 8000/63 (64)           | 6400/95                | 2/1750               |
| Standard_F8s_v2  | 8      | 16          | 64             | 16             | 16000/127 (128)        | 12800/190              | 4/3500               |
| Standard_F16s_v2 | 16     | 32          | 128            | 32             | 32000/255 (256)        | 25600/380              | 4/7000               |
| Standard_F32s_v2 | 32     | 64          | 256            | 32             | 64000/512 (512)        | 51200/750              | 8/14000              |
| Standard_F48s_v2 | 48     | 96          | 384            | 32             | 96000 / 768 (768)        | 76800 / 1100             | 8 / 21000              |
| Standard_F64s_v2 | 64     | 128         | 512            | 32             | 128000/1024 (1024)     | 80000/1100             | 8/28000              |
| Standard_F72s_v2<sup>2,&nbsp;3</sup> | 72 | 144 | 576         | 32             | 144000/1152 (1520)     | 80000/1100             | 8 / 30000              |

<sup>1</sup> Virtuelle Computer der Fsv2-Serie verfügen über Hyper-Threading-Technologie von Intel®.

<sup>2</sup> Bei der Verwendung von mehr als 64 vCPUs ist eines dieser unterstützten Gastbetriebssysteme erforderlich: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 und Red Hat Enterprise Linux, CentOS 7.3 oder Oracle Linux 7.3 mit LIS 4.2.1.

<sup>3</sup> Instanz wird isoliert auf dedizierter Hardware ausgeführt, die für einen einzigen Kunden bereitgestellt wird.
