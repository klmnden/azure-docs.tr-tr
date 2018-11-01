---
title: Azure CLI kullanarak bölgesel ön uç ile bir genel Load Balancer Standard oluşturma | Microsoft Docs
description: Azure CLI kullanarak bölgesel ön uç ile bir genel Load Balancer Standard oluşturma konusunda bilgi edinin
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: kumud
ms.openlocfilehash: 3a0fc37b8e2865163ae6c55813d145a568d796e0
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414504"
---
#  <a name="create-a-public-load-balancer-standard-with-zonal-frontend-using-azure-cli"></a>Azure CLI kullanarak bölgesel ön uç ile bir genel Load Balancer Standard oluşturma

Bu makalede adımları genel oluşturma işleminde [Load Balancer Standard](https://aka.ms/azureloadbalancerstandard) bölgesel bir ön uç ile. Sahip. bir bölgedeki tek bir bölge tarafından sunulan herhangi bir gelen veya giden akış bölgesel ön uç anlamına gelir. Ön uç yapılandırmasına bölgesel bir standart genel IP adresi kullanarak bir yük dengeleyici ile bölgesel bir ön uç oluşturabilirsiniz. Kullanılabilirlik alanları standart Load Balancer ile nasıl çalıştığını anlamak için bkz: [Standard Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md). 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz, en son yüklediğinizden emin olun [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve bir Azure hesabı ile oturum açtığınız [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az_login).

> [!NOTE]
> Kullanılabilirlik bölgeleri, seçili Azure kaynakları ve bölgeler ve sanal makine boyutu aileleri için kullanılabilir. Kullanmaya başlamak nasıl daha fazla bilgi ve hangi Azure kaynakları, bölgeleri ve kullanılabilirlik alanları ile deneyebilirsiniz sanal makine boyutu aileleri için bkz. [kullanılabilirlik alanlarına genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Aşağıdaki komutu kullanarak bir kaynak grubu oluşturun:

```azurecli-interactive
az group create --name myResourceGroupZLB --location westeurope
```

## <a name="create-a-public-standard-ip-address"></a>Genel Standart IP adresi oluşturma

Aşağıdaki komutu kullanarak bölgesel bir standart genel IP adresi oluşturun:

```azurecli-interactive
az network public-ip create --resource-group myResourceGroupZLB --name myPublicIPZonal --sku Standard --zone 1
```

## <a name="create-a-load-balancer"></a>Yük dengeleyici oluşturma

Standart genel aşağıdaki komutu kullanarak önceki adımda oluşturduğunuz IP bir genel Load Balancer Standard oluşturma:

```azurecli-interactive
az network lb create --resource-group myResourceGroupZLB --name myLoadBalancer --public-ip-address myPublicIPZonal --frontend-ip-name myFrontEnd --backend-pool-name myBackEndPool --sku Standard
```

## <a name="create-an-lb-probe-on-port-80"></a>Bağlantı noktası 80 üzerinde bir LB araştırma oluşturma

Aşağıdaki komutu kullanarak bir yük dengeleyici durum araştırması oluşturun:

```azurecli-interactive
az network lb probe create --resource-group myResourceGroupZLB --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80
```

## <a name="create-an-lb-rule-for-port-80"></a>Bağlantı noktası 80 için bir LB kuralı oluşturma

Aşağıdaki komutu kullanarak bir yük dengeleyici kuralı oluşturun:

```azurecli-interactive
az network lb rule create --resource-group myResourceGroupZLB --lb-name myLoadBalancer --name myLoadBalancerRuleWeb \
  --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEnd \
  --backend-pool-name myBackEndPool --probe-name myHealthProbe
```

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Standard Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md).



