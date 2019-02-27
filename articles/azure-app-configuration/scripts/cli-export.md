---
title: Azure CLI betik örneği - bir Azure uygulama yapılandırma Store dışarı aktarma | Microsoft Docs
description: Bir uygulama yapılandırma Azure Mağazası'ndan dışarı aktarmak için bilgi ve örnek betikler sağlar
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
ms.openlocfilehash: 3310dc5d72284e8d94b95b855fee90d560205fa4
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884785"
---
# <a name="export-from-an-azure-app-configuration-store"></a>Bir uygulama yapılandırma Azure Mağazası'ndan dışarı aktarma

Bu örnek betik bir Azure uygulama yapılandırması deposundan anahtar-değer verir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

Aşağıdaki komutu yürüterek Azure uygulama yapılandırma CLI uzantısını yüklemeniz gerekir:

        az extension add -n appconfig

## <a name="sample-script"></a>Örnek betik

```azurecli-interactive
#!/bin/bash

# Export all key-values
az appconfig kv export --name myTestAppConfigStore --file ~/Export.json
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir uygulama yapılandırma deposu dışarı aktarmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az appconfig dışarı aktarma](/cli/azure/ext/appconfig/appconfig#ext-appconfig-az-appconfig-export) | Uygulama yapılandırması dışarı kaynağını depolayın. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek uygulama yapılandırma CLI betiği örnekleri, içinde bulunabilir [Azure uygulama yapılandırması belgeleri](../cli-samples.md).
