---
title: Erstellen einer Desktop-App, die Web-APIs aufruft – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie eine Desktop-App erstellen, die Web-APIs aufruft (Übersicht)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 545012629686e1fe3ece8a48ed852542e09e54fe
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965516"
---
# <a name="scenario-desktop-app-that-calls-web-apis"></a>Szenario: Desktop-App, die Web-APIs aufruft

Erfahren Sie, wie Sie eine Desktop-App erstellen, die Web-APIs aufruft

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Erste Schritte

Sofern nicht bereits geschehen, erstellen Sie nun Ihre erste Anwendung, indem Sie den Schritten in der .NET Desktop-Schnellstartanleitung, der UWP-Schnellstartanleitung oder der Schnellstartanleitung für native macOS-Apps folgen:

> [!div class="nextstepaction"]
> [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API von einer Windows Desktop-App aus](./quickstart-v2-windows-desktop.md)


> [!div class="nextstepaction"]
> [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API von einer UWP-App aus](./quickstart-v2-uwp.md)

> [!div class="nextstepaction"]
> [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API aus einer nativen macOS-App](./quickstart-v2-ios.md)

## <a name="overview"></a>Übersicht

Sie schreiben eine Desktopanwendung und möchten Benutzer bei Ihrer Anwendung anmelden, und Web-APIs wie Microsoft Graph, andere Microsoft-APIs oder Ihre eigene Web-API aufrufen. Sie haben hierfür mehrere Möglichkeiten:

- Sie können den interaktiven Tokenabruf verwenden:

  - Wenn Ihre Desktopanwendung grafische Steuerelemente unterstützt, z.B. im Fall einer Windows.Form-Anwendung, einer WPF-Anwendung oder einer nativen macOS-Anwendung.
  - Wenn es sich um eine .NET Core-Anwendung handelt, und Sie zustimmen, dass die Authentifizierungsinteraktion mit Azure AD im Systembrowser erfolgt.

- Bei Anwendungen, die von Windows gehostet werden, kann ein Token mithilfe der integrierten Windows-Authentifizierung automatisch abgerufen werden, wenn die Anwendungen auf Computern ausgeführt werden, die zu einer Windows-Domäne gehören oder in AAD eingebunden sind.
- Darüber hinaus können Sie, obwohl dies nicht empfohlen wird, Benutzername und Kennwort für öffentliche Clientanwendungen verwenden. In einigen Szenarios (z. B. DevOps) ist ihre Verwendung zwar weiterhin erforderlich, bedenken Sie aber, dass dies Einschränkungen für Ihre Anwendung bedeutet. Ein Benutzer, der beispielsweise die mehrstufige Authentifizierung (bedingter Zugriff) durchführen muss, könnte sich dann nicht anmelden. Auch einmaliges Anmelden wäre für Ihre Anwendung dann nicht möglich.

  Diese Möglichkeit widerspricht außerdem den Prinzipien moderner Authentifizierung und steht nur aus Legacygründen zur Auswahl.

  ![Desktopanwendung](media/scenarios/desktop-app.svg)

- Wenn Sie ein portables Befehlszeilentool schreiben (möglicherweise eine.NET Core-Anwendung, die unter Linux oder Mac ausgeführt wird) und akzeptieren, dass die Authentifizierung an den Systembrowser delegiert wird, können Sie interaktive Authentifizierung verwenden. (.NET Core stellt noch keinen [Webbrowser](https://aka.ms/msal-net-uses-web-browser) zur Verfügung, daher erfolgt die Authentifizierung im Systembrowser.) Ansonsten ist die beste Option in diesem Fall die Verwendung des Gerätecodeflows. Dieser Flow wird auch für Anwendungen ohne Browser verwendet, z. B. für IoT-Anwendungen.

  ![Anwendungen ohne Browser](media/scenarios/device-code-flow-app.svg)

## <a name="specifics"></a>Besonderheiten

Desktopanwendungen weisen mehrere Besonderheiten auf, die hauptsächlich davon abhängen, ob für Ihre Anwendung die interaktive Authentifizierung verwendet wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Desktop-App: App-Registrierung](scenario-desktop-app-registration.md)
