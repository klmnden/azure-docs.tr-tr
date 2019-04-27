---
title: Azure CLI betik örneği - yüksek kullanılabilirlik uygulamaları için trafiği yönlendirme | Microsoft Docs
description: Azure CLI betik örneği - yüksek kullanılabilirlik uygulamaları için trafiği yönlendirme
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: tysonn
tags: azure-infrastructure
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 06/26/2018
ms.author: kumud
ms.openlocfilehash: 3922eb76fa0954b9c02cc86f98acb142cc1d1fee
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60565318"
---
# <a name="route-traffic-for-high-availability-of-applications"></a>Uygulamaları yüksek kullanılabilirlik için trafiği yönlendirme

Bu betik bir kaynak grubu, iki app service planı, iki web uygulaması, traffic manager profili ve iki traffic manager uç noktası oluşturur. Uygulama birincil bölge kullanılamaz duruma geldiğinde, traffic Manager trafiği birincil bölgeye olarak tek bir bölge içinde uygulamaya ve ikincil bölgeye yönlendirir. Betiği çalıştırmadan önce mywebapp şeklindedir ve MyWebAppL1 MyWebAppL2 değerleri Azure genelinde benzersiz değerlerle değiştirmelisiniz. Betiği çalıştırdıktan sonra uygulama URL'si mywebapp.trafficmanager.net ile birincil bölgedeki erişebilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]


## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Betik örneği çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu, App Service uygulamasını kaldırmak için kullanılabilir ve tüm ilgili kaynakları.

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, web uygulaması, traffic manager profili ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan) | App Service planı oluşturur. Azure web uygulamanız için bir sunucu grubu gibi budur. |
| [az webapp create](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | App Service planı içinde bir Azure web uygulaması oluşturur. |
| [az ağ traffic manager profili oluşturma](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile) | Bir Azure Traffic Manager profili oluşturur. |
| [az ağ traffic manager uç noktası oluşturma](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint) | Azure Traffic Manager profiline bir uç nokta ekler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek App Service CLI betik örnekleri bulunabilir [belgeleri Azure ağ](../cli-samples.md).
