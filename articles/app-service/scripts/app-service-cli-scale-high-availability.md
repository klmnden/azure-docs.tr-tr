---
title: "Azure CLI komut dosyası örneği - ölçek yüksek availabilty mimarisiyle dünya çapında bir web uygulaması | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - ölçek yüksek availabilty mimarisiyle dünya çapında bir web uygulaması"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: e4033a50-0e05-4505-8ce8-c876204b2acc
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 52679af9cd8f6b455f68b9307ed52202ff74349d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="scale-a-web-app-worldwide-with-a-high-availability-architecture"></a>Bir web uygulaması dünya çapında yüksek kullanılabilirlik mimarisi ile ölçeklendirin

Bu senaryoda, bir kaynak grubu, iki uygulama hizmeti planları, iki web uygulamaları, trafik Yöneticisi profili ve iki trafik Yöneticisi uç noktaları oluşturur. Alıştırma tamamlandıktan sonra bir yüksek kullanılabilir olacaktır sağlayan mimari web uygulamanızın üzerinde en düşük ağ gecikmesini genel kullanılabilirlik sağlar.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-geographic/scale-geographic.sh "Geographic Scale")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, web uygulaması, trafik Yöneticisi profili ve tüm ilgili kaynaklar oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az uygulama hizmeti planı oluşturma](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | App Service planı oluşturur. Bu, Azure web uygulamanız için bir sunucu grubu gibidir. |
| [az webapp oluşturma](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Azure web uygulaması oluşturur. |
| [az ağ trafik Yöneticisi profili oluştur](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#az_network_traffic_manager_profile_create) | Bir Azure Traffic Manager profilini oluşturur. |
| [az ağ trafik Yöneticisi uç noktası oluşturma](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#az_network_traffic_manager_endpoint_create) | Bir uç noktası için bir Azure Traffic Manager profili ekler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek uygulama hizmeti CLI kod örnekleri bulunabilir [Azure App Service belgeleri](../app-service-cli-samples.md).
