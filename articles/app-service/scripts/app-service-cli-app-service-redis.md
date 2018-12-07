---
title: Azure CLI betik örneği - bir web uygulaması için bir Azure önbelleği için Redis bağlama | Microsoft Docs
description: Azure CLI betik örneği - bir web uygulaması için bir Azure önbelleği için Redis bağlama
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 12/11/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: e909a86912c16cac8f9c92d1632ba2eb29ee1d30
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53012105"
---
# <a name="connect-a-web-app-to-an-azure-cache-for-redis"></a>Bir web uygulaması için bir Azure önbelleği için Redis bağlanın.

Bu örnek betik bir Azure önbelleği için Redis ve Azure web uygulaması oluşturur. Ardından uygulama ayarlarını kullanarak web uygulamasını Azure önbelleği için Redis bağlanır.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, Azure önbelleği için Redis web uygulaması oluşturmak için aşağıdaki komutları kullanır ve tüm ilgili kaynakları. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | App Service planı oluşturur. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Bir Azure web uygulaması oluşturur. |
| [`az redis create`](/cli/azure/redis?view=azure-cli-latest#az-redis-create) | Yeni Azure önbelleği için Redis örneği oluşturun. |
| [`az redis list-keys`](/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys) | Azure önbelleği için Redis örneği için erişim anahtarlarını listeler. |
| [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) | Bir Azure web uygulaması için uygulama ayarı oluşturur veya güncelleştirir. Uygulama ayarları, uygulamanız için ortam değişkenleri olarak kullanıma sunulur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek App Service CLI betik örnekleri, [Azure App Service belgelerinde](../app-service-cli-samples.md) bulunabilir.
