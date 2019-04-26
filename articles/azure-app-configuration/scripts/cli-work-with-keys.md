---
title: Azure CLI betik örneği - bir Azure uygulama yapılandırma Store anahtar değerleri ile çalışma | Microsoft Docs
description: Bir Azure uygulama yapılandırma deposundaki anahtar değerleri ile çalışma hakkında bilgi sağlar.
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
ms.openlocfilehash: 9288ea08da6335dd29e7a15a9bc871b76c1ce7e9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60235145"
---
# <a name="work-with-key-values-in-an-azure-app-configuration-store"></a>Bir Azure uygulama yapılandırma deposundaki anahtar değerleri ile çalışma

Bu örnek betik bir Azure uygulama yapılandırma deposunda yeni bir anahtar-değer oluşturur, tüm mevcut anahtar değerleri listeler, yeni oluşturulan anahtar değerini güncelleştirir ve son olarak siler.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

Aşağıdaki komutu yürüterek Azure uygulama yapılandırma CLI uzantısını yüklemeniz gerekir:

        az extension add -n appconfig

## <a name="sample-script"></a>Örnek betik

```azurecli-interactive
#!/bin/bash

appConfigName=myTestAppConfigStore
newKey="TestKey"

# Create a new key-value 
az appconfig kv set --name $appConfigName --key $newKey --value "Value 1"

# List current key-values
az appconfig kv list --name $appConfigName

# Update new key's value
az appconfig kv set --name $appConfigName --value "Value 2"

# List current key-values
az appconfig kv list --name $appConfigName

# Delete new key
az appconfig kv delete  --name $appConfigName --key $newKey

# List current key-values
az appconfig kv list --name $appConfigName
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir uygulama yapılandırma deposundaki anahtar değerleri üzerinde çalışılacak aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az appconfig kv ayarlayın](/cli/azure/ext/appconfig/appconfig) | Oluşturur veya bir anahtar-değer güncelleştirir. |
| [az appconfig kv listesi](/cli/azure/ext/appconfig/appconfig) | Bir uygulama yapılandırma deposundaki anahtar değerleri listeler. |
| [az appconfig kv Sil](/cli/azure/ext/appconfig/appconfig) | Bir anahtar-değer siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek uygulama yapılandırma CLI betiği örnekleri, içinde bulunabilir [Azure uygulama yapılandırması belgeleri](../cli-samples.md).
