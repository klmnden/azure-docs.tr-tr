---
title: Bir genel yük dengeleyiciye standart Azure CLI kullanarak zonal ön uç ile oluşturma | Microsoft Docs
description: Bir genel yük dengeleyiciye standart Azure CLI kullanarak zonal ön uç ile oluşturmayı öğrenin
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: kumud
ms.openlocfilehash: da18693f090a256bf69ea27e46e0ac010eb293b0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zonal-frontend-using-azure-cli"></a>Bir genel yük dengeleyiciye standart Azure CLI kullanarak zonal ön uç ile oluşturma

Bu makalede adımları genel oluşturmada size [yük dengeleyici standart](https://aka.ms/azureloadbalancerstandard) zonal bir ön uç ile. Gelen veya giden akış bir bölgedeki tek bir bölge tarafından sunulan bir zonal ön uç yol sahip. Ön uç yapılandırmasıyla zonal standart genel IP adresi kullanarak bir yük dengeleyici zonal bir ön uç ile oluşturabilirsiniz. Kullanılabilirlik bölgeleri standart yük dengeleyici ile nasıl çalıştığını anlamak için bkz: [standart yük dengeleyici ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md). 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, en son yüklediğinizden emin olun [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve bir Azure hesabı ile oturum açmış [az oturum açma](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az_login).

> [!NOTE]
> Kullanılabilirlik bölgeler için destek, select Azure kaynaklarını ve bölgeler ve VM boyutu aileleri için kullanılabilir. Başlamak hakkında daha fazla bilgi ve hangi Azure kaynaklarını, bölgeler ve kullanılabilirlik bölgeleri deneyebilirsiniz VM boyutu aileleri için bkz: [kullanılabilirlik bölgeleri genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Aşağıdaki komutu kullanarak bir kaynak grubu oluşturun:

```azurecli-interactive
az group create --name myResourceGroupZLB --location westeurope
```

## <a name="create-a-public-standard-ip-address"></a>Ortak bir standart IP adresi oluştur

Aşağıdaki komutu kullanarak zonal bir standart genel IP adresi oluşturun:

```azurecli-interactive
az network public-ip create --resource-group myResourceGroupZLB --name myPublicIPZonal --sku Standard --zone 1
```

## <a name="create-a-load-balancer"></a>Yük dengeleyici oluşturma

Bir genel yük dengeleyiciye standart standart genel aşağıdaki komutu kullanarak önceki adımda oluşturduğunuz IP oluşturun:

```azurecli-interactive
az network lb create --resource-group myResourceGroupZLB --name myLoadBalancer --public-ip-address myPublicIPZonal --frontend-ip-name myFrontEnd --backend-pool-name myBackEndPool --sku Standard
```

## <a name="create-an-lb-probe-on-port-80"></a>Bağlantı noktası 80 üzerinde bir LB araştırması oluştur

Aşağıdaki komutu kullanarak bir yük dengeleyici durum araştırması oluşturun:

```azurecli-interactive
az network lb probe create --resource-group myResourceGroupZLB --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80
```

## <a name="create-an-lb-rule-for-port-80"></a>80 numaralı bağlantı noktası için bir LB kuralı oluşturma

Aşağıdaki komutu kullanarak bir yük dengeleyici kuralı oluşturun:

```azurecli-interactive
az network lb rule create --resource-group myResourceGroupZLB --lb-name myLoadBalancer --name myLoadBalancerRuleWeb \
  --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEnd \
  --backend-pool-name myBackEndPool --probe-name myHealthProbe
```

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [standart yük dengeleyici ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md).



