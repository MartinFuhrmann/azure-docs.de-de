---
title: 'Azure CLI-Skriptbeispiel: Exportieren aus einem Azure App Configuration-Speicher | Microsoft-Dokumentation'
description: Informationen und Beispielskripts zum Exportieren aus einem Azure App Configuration-Speicher
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: azure-app-configuration
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 7969300e9865c943c0f3759277bd26529c0c5464
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2019
ms.locfileid: "74184885"
---
# <a name="export-from-an-azure-app-configuration-store"></a>Exportieren aus einem Azure App Configuration-Speicher

Dieses Beispielskript exportiert Schlüssel-Wert-Paare aus einem Azure App Configuration-Speicher.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

Führen Sie zunächst den folgenden Befehl aus, um die CLI-Erweiterung für Azure App Configuration zu installieren:

        az extension add -n appconfig

## <a name="sample-script"></a>Beispielskript

```azurecli-interactive
#!/bin/bash

# Export all key-values
az appconfig kv export --name myTestAppConfigStore --file ~/Export.json
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle für das Exportieren aus einem App Configuration-Speicher. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az appconfig kv export](/cli/azure/ext/appconfig/appconfig/kv#ext-appconfig-az-appconfig-kv-export) | Dient zum Exportieren aus einer App Configuration-Speicherressource. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für App Configuration finden Sie in den [CLI-Beispielen für Azure App Configuration](../cli-samples.md).
