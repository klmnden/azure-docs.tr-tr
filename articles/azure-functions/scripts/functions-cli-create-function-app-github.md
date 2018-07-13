---
title: Bir İşlev Uygulaması oluşturma ve işlev kodunu GitHub'dan dağıtma | Microsoft Docs
description: Azure CLI Betiği Örneği - Bir İşlev Uygulaması oluşturma ve işlev kodunu GitHub'dan dağıtma
services: functions
keywords: ''
author: syntaxc4
ms.author: cfowler
ms.date: 04/27/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: f013303cb99cc549cb0a8d72e2e9c60209008c67
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38317922"
---
# <a name="create-a-function-app-and-deploy-function-code-from-github"></a>Bir işlev uygulaması oluşturma ve işlev kodunu GitHub'dan dağıtma

Bu örnek betik, [tüketim planını](../functions-scale.md#consumption-plan) ilgili kaynaklarıyla kullanarak bir işlev uygulaması oluşturur ve işlev kodunuzu genel bir GitHub deposundan dağıtır (sürekli dağıtım olmadan). İşlev kodunun GitHub'dan sürekli olarak teslim edilmesi için, [İşlev uygulaması oluşturma ve GitHub'dan sürekli olarak dağıtma](functions-cli-create-function-app-github-continuous.md) konusunu okuyun.

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

Bu örnek, bir Azure İşlev uygulaması oluşturur ve işlev kodunu GitHub'dan dağıtır.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github/deploy-function-app-with-function-github.sh?highlight=3 "Create a function app with deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Tablodaki her komut, komuta özgü belgelere yönlendirir. Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | App Service planı oluşturur. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) | Azure İşlevi uygulaması oluşturur. |
| [az appservice web source-control config](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Bir işlev uygulamasını Git veya Mercurial deposu ile ilişkilendirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.
