---
title: "Bir işlev uygulaması oluşturma ve işlev kodu github'dan dağıtma | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - bir işlev uygulaması oluşturma ve işlev kodu github'dan dağıtma"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 39e26ba6c3ae0927fbf6c62af0bd3b48d16657ad
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-app-and-deploy-function-code-from-github"></a>Bir işlev uygulaması oluşturma ve işlev kodu github'dan dağıtma

Bu örnek komut dosyası kullanarak bir işlev uygulaması oluşturur [tüketim planı](../functions-scale.md#consumption-plan) ile ilgili kaynaklarını ve ortak bir GitHub deposuna (sürekli dağıtımı) olmadan işlev kodunuzdan dağıtır. İşlev kodu github'dan kesintisiz teslim okuma [bir işlev uygulaması oluşturma ve sürekli olarak Github'dan dağıtma](functions-cli-create-function-app-github-continuous.md)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası

Bu örnek bir Azure işlevi uygulamasını oluşturur ve işlev kodu github'dan dağıtır.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "Create a function app with deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Komut belirli belgeleri tablo bağlanan her komut. Bu komut dosyasını aşağıdaki komutları kullanır:

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az depolama hesabı oluşturma](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | App Service planı oluşturur. |
| [az functionapp oluşturma](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) | Bir Azure işlev uygulaması oluşturur. |
| [az appservice web kaynak denetimi yapılandırma](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Bir işlev uygulaması Git veya Mercurial deposu ile ilişkilendirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek Azure işlevleri CLI kod örnekleri bulunabilir [Azure işlevleri belgelerine](../functions-cli-samples.md).
