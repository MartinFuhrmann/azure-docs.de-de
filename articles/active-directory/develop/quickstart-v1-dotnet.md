---
title: Anmelden von Benutzern und Aufrufen von Microsoft Graph-APIs mit .NET-Desktop (WPF) aus aufzurufen | Azure
description: Erfahren Sie, wie eine .NET Windows Desktop-Anwendung erstellt wird, die sich für die Anmeldung in Azure AD integriert und über OAuth 2.0 durch Azure AD geschützte APIs aufruft.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0f1aca48c547fc8c298128b35d9c571512fcbdf8
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965941"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-a-net-desktop-wpf-app"></a>Schnellstart: Anmelden von Benutzern und Aufrufen der Microsoft Graph-API aus einer .NET Desktop-App (WPF-App)

[Microsoft Identity Platform](v2-overview.md) ist eine Weiterentwicklung der Azure AD-Entwicklerplattform (Azure Active Directory). Sie ermöglicht Entwicklern das Erstellen von Anwendungen, mit denen alle Microsoft-Identitäten angemeldet werden, sowie das Abrufen von Token zum Aufrufen von Microsoft-APIs, etwa Microsoft Graph oder von Entwicklern erstellte APIs.

Mithilfe der [Microsoft-Authentifizierungsbibliothek (MSAL)](msal-overview.md) können Entwickler Token vom Microsoft Identity Platform-Endpunkt abrufen, um auf geschützte Web-APIs zuzugreifen. Die Active Directory-Authentifizierungsbibliothek (ADAL) arbeitet mit dem Azure AD für Entwickler (v1.0)-Endpunkt zusammen, während MSAL und der Microsoft Identity Platform (v2.0)-Endpunkt integriert sind.

Für neue Desktopanwendungen wird empfohlen, Microsoft Identity Platform (v2.0) und MSAL zu verwenden, um Token zu erhalten und auf sichere Web-APIs zuzugreifen: [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API von einer Windows Desktop-App aus](quickstart-v2-windows-desktop.md)
