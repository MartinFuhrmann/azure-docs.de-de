---
title: include file
description: Includedatei für vertrauliche Clientszenario-Landing Pages (Daemon, Web-App, Web-API)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 8f98808aa0f8a2c32e2117447824114747091a82
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68912371"
---
## <a name="registration-of-secrets-or-certificates"></a>Registrierung von geheimen Schlüsseln oder Zertifikaten

Wie für jede vertrauliche Clientanwendung müssen Sie einen geheimen Schlüssel oder ein geheimes Zertifikat registrieren. Sie können Ihre geheimen Anwendungsschlüssel entweder über die interaktive Oberfläche im [Azure-Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) oder mithilfe von Befehlszeilentools (wie PowerShell) registrieren.

### <a name="registering-client-secrets-using-the-application-registration-portal"></a>Registrieren von geheimen Clientschlüsseln mit dem Anwendungsregistrierungsportal

Die Verwaltung von Clientanmeldeinformationen erfolgt auf der Seite **Certificates & secrets (Zertifikate und geheime Schlüssel)** einer Anwendung:

![image](../articles/active-directory/develop/media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets.png)

- Der (auch als geheimer Clientschlüssel bezeichnete) geheime Anwendungsschlüssel wird von Azure AD während der Registrierung der vertraulichen Clientanwendung generiert. Diese Generierung erfolgt, wenn Sie **Neuer geheimer Clientschlüssel** auswählen. An diesem Punkt müssen Sie vor dem Auswählen von **Speichern** die Zeichenfolge des geheimen Schlüssels in die Zwischenablage kopieren, um sie in Ihrer App verwenden zu können. Diese Zeichenfolge wird anschließend nicht mehr angezeigt.
- Das Zertifikat wird mithilfe der Schaltfläche **Zertifikat hochladen** in die Anwendungsregistrierung hochgeladen. Azure AD unterstützt nur Zertifikate, die direkt in der Anwendung registriert sind und nicht Vertrauensketten befolgen.

Weitere Informationen finden Sie unter [Schnellstart: Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs | Hinzufügen von Anmeldeinformationen zu Ihrer Webanwendung](../articles/active-directory/develop/quickstart-configure-app-access-web-apis.md#add-credentials-to-your-web-application).



### <a name="registering-client-secrets-using-powershell"></a>Registrieren von geheimen Clientschlüsseln mit PowerShell

Alternativ können Sie Ihre Anwendung in Azure AD mithilfe von Befehlszeilentools registrieren. Im Beispiel [active-directory-dotnetcore-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) wird gezeigt, wie Sie einen geheimen Anwendungsschlüssel oder ein Zertifikat in einer Azure AD-Anwendung registrieren:

- Weitere Informationen darüber, wie Sie einen geheimen Anwendungsschlüssel registrieren, finden Sie unter [AppCreationScripts/Configure.ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts/Configure.ps1#L190).
- Weitere Informationen darüber, wie Sie ein Zertifikat in der Anwendung registrieren, finden Sie unter [AppCreationScripts-withCert/Configure.ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts-withCert/Configure.ps1#L162-L178).
