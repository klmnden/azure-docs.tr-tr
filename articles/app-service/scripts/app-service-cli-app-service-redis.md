---
title: "Azure CLI betik örnek - bir web uygulamasını redis önbelleği için bağlama | Microsoft Docs"
description: "Azure CLI betik örnek - bir web uygulamasını redis önbelleği için bağlama"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
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
<<<<<<< HEAD
ms.openlocfilehash: c1277a04e3a63c09b2ed7f9304bfc482ab9189a8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
=======
ms.openlocfilehash: 248f26c0562b636caf74409fd76334601931d035
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="connect-a-web-app-to-a-redis-cache"></a>Bir web uygulamasını redis önbelleği için bağlama

Bu örnek betik, bir Azure redis önbelleği ve Azure web uygulaması oluşturur. Uygulama ayarları kullanarak web uygulaması için redis önbelleği sonra bağlar.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, 2.0 veya üstü Azure CLI sürüm gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, web uygulaması, redis önbelleği ve tüm ilişkili kaynakları oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | App Service planı oluşturur. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) | Azure web uygulaması oluşturur. |
| [`az redis create`](/cli/azure/redis?view=azure-cli-latest#az_redis_create) | Yeni Redis önbelleği örneği oluşturun. |
| [`az redis list-keys`](/cli/azure/redis?view=azure-cli-latest#az_redis_list_keys) | Redis önbelleği örneğine erişim anahtarlarını listeler. |
| [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) | Oluşturur veya bir Azure web uygulaması için bir uygulama ayarı güncelleştirir. Uygulama ayarları uygulamanız için ortam değişkenleri olarak sunulur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek uygulama hizmeti CLI kod örnekleri bulunabilir [Azure App Service belgeleri](../app-service-cli-samples.md).
