---
title: Azure CLI Betik Örneği - Özel bir etki alanını bir işlev uygulaması ile eşleme | Microsoft Docs
description: Azure CLI Betik Örneği - Azure’da özel bir etki alanını bir işlev uygulaması ile eşleme.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: azure-functions
ms.devlang: azurecli
ms.topic: sample
ms.date: 07/04/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 19fd73dff045d6d784dd70d1f3ad539a22faf413
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075476"
---
# <a name="map-a-custom-domain-to-a-function-app"></a>Özel bir etki alanını işlev uygulaması ile eşleme

Bu örnek betik App Service planında bir işlev uygulaması oluşturur ve ardından bunu sizin sağladığınız özel etki alanına eşler. İşlev uygulamanızı barındırılan bir [Premium planı](../functions-scale.md#premium-plan) veya [App Service planı](../functions-scale.md#app-service-plan), bir CNAME ya da bir A kaydı kullanarak özel bir etki alanını eşleyebilirsiniz. [Tüketim planındaki](../functions-scale.md#consumption-plan) işlev uygulamalarında yalnızca CNAME seçeneği desteklenir. Bu örnekte, App Service planı oluşturulur ve etki alanını eşlemek için A kaydı gerekir. 

Bu örnek betiği çalıştırmak için, özel etki alanınızda web uygulamanızın varsayılan etki alanı adına işaret eden bir A kaydını zaten yapılandırmış olmalısınız. Daha fazla bilgi için bkz. [Azure App Service için özel etki alanını eşleme yönergeleri](https://aka.ms/appservicecustomdns). 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI’yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a function app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | İşlev uygulaması için gereken bir depolama hesabı oluşturur. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az-appservice-plan-create) | Özel etki alanını eşlemek için gereken bir App Service planı oluşturur. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | App Service planında bir işlev uygulaması oluşturur. |
| [az functionapp config hostname add](https://docs.microsoft.com/cli/azure/functionapp/config/hostname#az-functionapp-config-hostname-add) | Özel bir etki alanını işlev uygulaması ile eşler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek İşlevler CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.
