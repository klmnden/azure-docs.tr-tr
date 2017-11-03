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
ms.date: 08/30/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: c1277a04e3a63c09b2ed7f9304bfc482ab9189a8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connect-a-web-app-to-a-redis-cache"></a>Bir web uygulamasını redis önbelleği için bağlama

Bu senaryoda, bir Azure redis önbelleği ve bir Azure web uygulamasına nasıl oluşturulacağını öğreneceksiniz. Daha sonra uygulama ayarlarını kullanarak web uygulamasını redis önbelleği bağlayacaksınız.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutlar bir kaynak grubu, web uygulaması oluşturun, önbellek ve tüm ilgili kaynaklar redis için kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az uygulama hizmeti planı oluşturma](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | App Service planı oluşturur. Bu, Azure web uygulamanız için bir sunucu grubu gibidir. |
| [az webapp oluşturma](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Azure web uygulaması oluşturur. |
| [az redis oluşturma](https://docs.microsoft.com/en-us/cli/azure/redis#az_redis_create) | Yeni Redis önbelleği örneği oluşturun. Veri depolanacağı budur. |
| [az redis listesi anahtarlar](https://docs.microsoft.com/en-us/cli/azure/redis#az_redis_list_keys) | Redis önbelleği örneğine erişim anahtarlarını listeler. |
| [az webapp config appsettings ayarlama](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) | Oluşturur veya bir Azure web uygulaması için bir uygulama ayarı güncelleştirir. Uygulama ayarları uygulamanız için ortam değişkenleri olarak sunulur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek uygulama hizmeti CLI kod örnekleri bulunabilir [Azure App Service belgeleri](../app-service-cli-samples.md).
