---
title: Verwenden des Azure-Portals zum Konfigurieren von kundenseitig verwalteten Schlüsseln
titleSuffix: Azure Storage
description: Erfahren Sie, wie Sie über das Azure-Portal von Kunden verwaltete Schlüssel mit Azure Key Vault für die Azure Storage-Verschlüsselung konfigurieren. Mit von Kunden verwalteten Schlüsseln können Sie Zugriffssteuerungen erstellen, rotieren, deaktivieren und widerrufen.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: b1006fead92763c5c2e670527b5e232618b633e5
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2019
ms.locfileid: "74895306"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-the-azure-portal"></a>Konfigurieren von kundenseitig verwalteten Schlüsseln mit Azure Key Vault über das Azure-Portal

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

In diesem Artikel wird beschrieben, wie Sie eine Azure Key Vault-Instanz mit von Kunden verwalteten Schlüsseln über das [Azure-Portal](https://portal.azure.com/) konfigurieren. Informationen zum Erstellen eines Schlüsseltresors über das Azure-Portal finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe des Azure-Portals](../../key-vault/quick-create-portal.md).

> [!IMPORTANT]
> Bei Verwendung kundenseitig verwalteter Schlüssel mit der Azure Storage-Verschlüsselung müssen für den Schlüsseltresor die beiden Eigenschaften **Vorläufiges Löschen** und **Nicht Bereinigen** festgelegt werden. Diese Eigenschaften sind standardmäßig nicht aktiviert. Um die Eigenschaften zu aktivieren, verwenden Sie entweder PowerShell oder Azure CLI.
> Es werden ausschließlich RSA-Schlüssel und die Schlüsselgröße 2048 unterstützt.

## <a name="enable-customer-managed-keys"></a>Aktivieren von vom Kunden verwalteten Schlüsseln

Um vom Kunden verwaltete Schlüssel im Azure-Portal zu aktivieren, gehen Sie folgendermaßen vor:

1. Navigieren Sie zum Speicherkonto.
1. Klicken Sie auf dem Blatt **Einstellungen** Blatt für das Speicherkonto auf **Verschlüsselung**. Wählen Sie wie in der folgenden Abbildung die Option **Eigenen Schlüssel verwenden**.

    ![Portal-Screenshot mit der Verschlüsselungsoption](./media/storage-encryption-keys-portal/ssecmk1.png)

## <a name="specify-a-key"></a>Angeben eines Schlüssels

Nach der Aktivierung von vom Kunden verwalteten Schlüsseln können Sie einen Schlüssel angeben, der dem Storage-Konto zugeordnet werden soll.

### <a name="specify-a-key-as-a-uri"></a>Festlegen eines Schlüssels als URI

Gehen Sie wie folgt vor, um einen Schlüssel als URI anzugeben:

1. Um den Schlüssel-URI im Azure-Portal anzuzeigen, navigieren Sie zu Ihrem Schlüsseltresor, und wählen die Einstellung **Schlüssel** aus. Wählen Sie den gewünschten Schlüssel aus, und klicken Sie darauf, um die Einstellungen anzuzeigen. Kopieren Sie den Wert im Feld **Schlüsselbezeichner**, das den URI enthält.

    ![Screenshot mit Schlüssel-URI des Schlüsseltresors](media/storage-encryption-keys-portal/key-uri-portal.png)

1. Wählen Sie in den Einstellungen unter **Verschlüsselung** für Ihr Speicherkonto die Option **Schlüssel-URI eingeben** aus.
1. Geben Sie den URI im Feld **Schlüssel-URI** ein.

   ![Screenshot zur Eingabe des Schlüssel-URI](./media/storage-encryption-keys-portal/ssecmk2.png)

### <a name="specify-a-key-from-a-key-vault"></a>Festlegen eines Schlüssels aus einem Schlüsseltresor

Um einen Schlüssel aus einem Schlüsseltresor anzugeben, müssen Sie zunächst sicherstellen, dass Sie über einen Schlüsseltresor mit einem Schlüssel verfügen. Gehen Sie wie folgt vor, um einen Schlüssel aus einem Schlüsseltresor anzugeben:

1. Wählen Sie die Option **Select from Key Vault** (Aus Schlüsseltresor wählen).
2. Wählen Sie den Schlüsseltresor mit dem Schlüssel, den Sie verwenden möchten.
3. Wählen Sie den Schlüssel aus dem Schlüsseltresor.

   ![Screenshot mit Option für vom Kunden verwaltete Schlüssel](./media/storage-encryption-keys-portal/ssecmk3.png)

## <a name="update-the-key-version"></a>Aktualisieren der Schlüsselversion

Wenn Sie eine neue Version eines Schlüssels erstellen, müssen Sie das Speicherkonto aktualisieren, damit dieses die neue Version nutzt. Folgen Sie diesen Schritten:

1. Navigieren Sie zu Ihrem Speicherkonto, und zeigen Sie die Einstellungen zur **Verschlüsselung** an.
1. Geben Sie den URI für die neue Schlüsselversion an. Alternativ können Sie den Schlüsseltresor und den Schlüssel erneut auswählen, um die Version zu aktualisieren.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Storage encryption for data at rest (Azure Storage-Verschlüsselung für ruhende Daten)](storage-service-encryption.md)
- [What is Azure Key Vault? (Was ist Azure Key Vault?)](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
