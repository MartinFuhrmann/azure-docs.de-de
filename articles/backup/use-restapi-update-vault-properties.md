---
title: Aktualisieren der Konfigurationseigenschaften eines Recovery Services-Tresors mithilfe der REST-API
description: In diesem Artikel erfahren Sie, wie Sie die Konfiguration eines Tresors mithilfe der REST-API aktualisieren.
ms.topic: conceptual
ms.date: 12/06/2019
ms.assetid: 9aafa5a0-1e57-4644-bf79-97124db27aa2
ms.openlocfilehash: cbd958dd71d2d62f7b4c7e8d66ab7e56dc679a51
ms.sourcegitcommit: d614a9fc1cc044ff8ba898297aad638858504efa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74998724"
---
# <a name="update-azure-recovery-services-vault-configurations-using-rest-api"></a>Aktualisieren von Konfigurationen eines Azure Recovery Services-Tresors mithilfe der REST-API

In diesem Artikel erfahren Sie, wie Sie sicherungsbezogene Konfigurationen in einem Azure Recovery Services-Tresor mithilfe der REST-API aktualisieren.

## <a name="soft-delete-state"></a>Zustand des vorläufigen Löschens

Das Löschen von Sicherungen eines geschützten Elements ist ein kritischer Vorgang, der überwacht werden muss. Zum Schutz vor versehentlicher Löschung verfügt der Azure Recovery Services-Tresor über eine Funktion für vorläufiges Löschen. Diese Funktion sorgt dafür, dass Kunden gelöschte Sicherungen innerhalb eines bestimmten Zeitfensters nach der Löschung wiederherstellen können.

Es gibt jedoch Szenarien, in denen diese Funktion nicht erforderlich ist. Ein Azure Recovery Services-Tresor, der Sicherungselemente enthält, kann nicht gelöscht werden (auch dann nicht, wenn sich die Elemente im Zustand „Vorläufig gelöscht“ befinden). Dies kann problematisch sein, wenn der Tresor umgehend gelöscht werden muss. Ein Beispiel: Bei Bereitstellungsvorgängen werden die erstellten Ressourcen häufig im gleichen Workflow bereinigt. Eine Bereitstellung kann einen Tresor erstellen, Sicherungen für ein Element konfigurieren, eine Testwiederherstellung durchführen und anschließend die Sicherungselemente und den Tresor wieder löschen. Wenn der Tresor nicht gelöscht werden kann, ist unter Umständen die gesamte Bereitstellung nicht erfolgreich. Die umgehende Löschung kann nur durch Deaktivieren des vorläufigen Löschens sichergestellt werden.

Aus diesem Grund muss sich der Kunde je nach Szenario sehr genau überlegen, ob er das vorläufige Löschen für einen bestimmten Tresor deaktiviert. Weitere Informationen finden Sie im [Artikel zu vorläufigem Löschen](backup-azure-security-feature-cloud.md#soft-delete).

### <a name="fetch-soft-delete-state-using-rest-api"></a>Abrufen des Zustands des vorläufigen Löschens mithilfe der REST-API

Das vorläufige Löschen ist für neu erstellte Recovery Services-Tresore standardmäßig aktiviert. Informationen zum Abrufen/Aktualisieren des Zustands des vorläufigen Löschens für einen Tresor finden Sie in der konfigurationsbezogenen [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/backup/backupresourcevaultconfigs) des Sicherungstresors.

Verwenden Sie den folgenden *GET*-Vorgang, um den aktuellen Zustand des vorläufigen Löschens für einen Tresor abzurufen:

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupconfig/vaultconfig?api-version=2019-05-13
```

Der GET-URI verfügt über die Parameter `{subscriptionId}`, `{vaultName}` und `{vaultresourceGroupName}`. In diesem Beispiel ist `{vaultName}` auf „testVault“ und `{vaultresourceGroupName}` auf „testVaultRG“ festgelegt. Da alle erforderlichen Parameter im URI angegeben sind, besteht keine Notwendigkeit eines separaten Anforderungstexts.

```http
GET https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupconfig/vaultconfig?api-version=2019-05-13
```

#### <a name="responses"></a>Antworten

Die erfolgreiche Antwort für den GET-Vorgang sieht wie folgt aus:

|NAME  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|200 – OK     |   [BackupResourceVaultConfig](https://docs.microsoft.com/rest/api/backup/backupresourcevaultconfigs/get#backupresourcevaultconfigresource)      | OK        |

##### <a name="example-response"></a>Beispielantwort

Nach Übermittlung der GET-Anforderung wird eine Antwort vom Typ „200“ (erfolgreich) zurückgegeben.

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testvaultRG/providers/Microsoft.RecoveryServices/vaults/testvault/backupconfig/vaultconfig",
  "name": "vaultconfig",
  "type": "Microsoft.RecoveryServices/vaults/backupconfig",
  "properties": {
    "enhancedSecurityState": "Enabled",
    "softDeleteFeatureState": "Enabled"
  }
}
```

### <a name="update-soft-delete-state-using-rest-api"></a>Aktualisieren des Zustands des vorläufigen Löschens mithilfe der REST-API

Verwenden Sie den folgenden *PATCH*-Vorgang, um den Zustand des vorläufigen Löschens des Recovery Services-Tresors mithilfe der REST-API zu aktualisieren:

```http
PATCH https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupconfig/vaultconfig?api-version=2019-05-13
```

Der PATCH-URI verfügt über die Parameter `{subscriptionId}`, `{vaultName}` und `{vaultresourceGroupName}`. In diesem Beispiel ist `{vaultName}` auf „testVault“ und `{vaultresourceGroupName}` auf „testVaultRG“ festgelegt. Wenn wir den URI mit den obigen Werten versehen, sieht er wie folgt aus:

```http
PATCH https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupconfig/vaultconfig?api-version=2019-05-13
```

#### <a name="create-the-request-body"></a>Erstellen des Anforderungstexts

Die folgenden allgemeinen Definitionen werden verwendet, um einen Anforderungstext zu erstellen:

Ausführlichere Informationen finden Sie in der [REST-API-Dokumentation](https://docs.microsoft.com/rest/api/backup/backupresourcevaultconfigs/update#request-body).

|NAME  |Erforderlich  |Typ  |BESCHREIBUNG  |
|---------|---------|---------|---------|
|eTag     |         |   Zeichenfolge      |  Optionales ETag       |
|location     |  true       |Zeichenfolge         |   Speicherort von Ressourcen      |
|properties     |         | [VaultProperties](https://docs.microsoft.com/rest/api/recoveryservices/vaults/createorupdate#vaultproperties)        |  Eigenschaften des Tresors       |
|tags     |         | Object        |     Ressourcentags    |

#### <a name="example-request-body"></a>Beispiel für Anforderungstext

Im folgenden Beispiel wird der Zustand des vorläufigen Löschens in „Disabled“ (Deaktiviert) geändert:

```json
{
  "properties": {
    "enhancedSecurityState": "Enabled",
    "softDeleteFeatureState": "Disabled"
  }
}
```

#### <a name="responses"></a>Antworten

Die erfolgreiche Antwort für den PATCH-Vorgang sieht wie folgt aus:

|NAME  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|200 – OK     |   [BackupResourceVaultConfig](https://docs.microsoft.com/rest/api/backup/backupresourcevaultconfigs/get#backupresourcevaultconfigresource)      | OK        |

##### <a name="example-response"></a>Beispielantwort

Nach Übermittlung der PATCH-Anforderung wird eine Antwort vom Typ „200“ (erfolgreich) zurückgegeben.

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testvaultRG/providers/Microsoft.RecoveryServices/vaults/testvault/backupconfig/vaultconfig",
  "name": "vaultconfig",
  "type": "Microsoft.RecoveryServices/vaults/backupconfig",
  "properties": {
    "enhancedSecurityState": "Enabled",
    "softDeleteFeatureState": "Disabled"
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

[Erstellen Sie eine Sicherungsrichtlinie zum Sichern einer Azure-VM in diesem Tresor](backup-azure-arm-userestapi-createorupdatepolicy.md).

Weitere Informationen zu den Azure-REST-APIs finden Sie in den folgenden Dokumenten:

- [Azure Recovery Services-Anbieter – REST-API](/rest/api/recoveryservices/)
- [Erste Schritte mit der Azure-REST-API](/rest/api/azure/)