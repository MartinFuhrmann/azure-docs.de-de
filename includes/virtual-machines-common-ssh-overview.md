---
title: include file
description: include file
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 12/21/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 3f9b86dd3716a25ab95265cf46b616144f57163b
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168636"
---
## <a name="overview-of-ssh-and-keys"></a>Übersicht über SSH und Schlüssel

[SSH](https://www.ssh.com/ssh/) ist ein Protokoll für verschlüsselte Verbindungen, das die sichere Anmeldung über ungesicherte Verbindungen ermöglicht. SSH ist das Standardverbindungsprotokoll für in Azure gehostete virtuelle Linux-Computer. SSH stellt zwar bereits eine verschlüsselte Verbindung bereit, bei Verwendung von Kennwörtern für SSH-Verbindungen ist der virtuelle Computer jedoch weiterhin anfällig für Brute-Force-Angriffe bzw. der Gefahr ausgesetzt, dass das Kennwort erraten wird. Die sicherere und bevorzugte Methode für die Verbindungsherstellung mit einem virtuellen Computer über SSH ist die Verwendung eines Paars aus einem öffentlichen und einem privaten Schlüssel, auch als *SSH-Schlüssel* bezeichnet. 

* Der *öffentliche Schlüssel* wird auf Ihrem virtuellen Linux-Computer oder in einem anderen Dienst platziert, den Sie für die Verschlüsselung mit öffentlichem Schlüssel verwenden möchten.

* Der *private Schlüssel* bleibt auf dem lokalen System. Bewahren Sie diesen privaten Schlüssel sicher auf. Geben Sie ihn nicht weiter.

Wenn Sie einen SSH-Client verwenden, um die Verbindung mit Ihrer Linux-VM (die den öffentlichen Schlüssel besitzt) herzustellen, testet der virtuelle Remotecomputer den Client, um sicherzustellen, dass er den privaten Schlüssel besitzt. Wenn der Client über den privaten Schlüssel verfügt, erhält er Zugriff auf den virtuellen Computer. 

Abhängig von den Sicherheitsrichtlinien in Ihrer Organisation können Sie ein einzelnes Paar aus einem öffentlichen und einem privaten Schlüssel für den Zugriff auf mehrere Azure-VMs und -Dienste verwenden. Sie benötigen nicht für jeden virtuellen Computer oder Dienst, auf den Sie zugreifen möchten, ein separates Schlüsselpaar. 

Sie können Ihren öffentlichen Schlüssel an beliebige Personen weitergeben, während Ihr privater Schlüssel ausschließlich für Ihren Besitz bestimmt ist (bzw. Ihre lokale Sicherheitsinfrastruktur).