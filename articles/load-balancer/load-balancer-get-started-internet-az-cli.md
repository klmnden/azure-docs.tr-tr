---
title: Azure CLI ile bölgesel olarak yedekli genel IP adresi ön uç ile bir genel Load Balancer Standard oluşturma | Microsoft Docs
description: Azure CLI ile bölgesel olarak yedekli genel IP adresi ön uç ile bir genel Load Balancer Standard oluşturma konusunda bilgi edinin
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
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: 23c9d1b56c9a7c49b2cabe434693797fc2d23dfa
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967685"
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-frontend-using-azure-cli"></a>Azure CLI ile bölgesel olarak yedekli ön uç ile bir genel Load Balancer Standard oluşturma

Bu makalede adımları genel oluşturma işleminde [Load Balancer Standard](https://aka.ms/azureloadbalancerstandard) genel IP standart bir adres kullanarak bölgesel olarak yedekli bir ön uç ile.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz, en son yüklediğinizden emin olun [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve bir Azure hesabı ile oturum açtığınız [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az_login).

> [!NOTE]
 Kullanılabilirlik bölgeleri, seçili Azure kaynakları ve bölgeler ve sanal makine boyutu aileleri için kullanılabilir. Kullanmaya başlamak nasıl daha fazla bilgi ve hangi Azure kaynakları, bölgeleri ve kullanılabilirlik alanları ile deneyebilirsiniz sanal makine boyutu aileleri için bkz. [kullanılabilirlik alanlarına genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Aşağıdaki komutu kullanarak bir kaynak grubu oluşturun:

```azurecli-interactive
az group create --name myResourceGroupSLB --location westeurope
```

## <a name="create-a-public-ip-standard"></a>Genel bir IP Standard oluşturma

Bir genel IP aşağıdaki komutu kullanarak standart oluşturun:

```azurecli-interactive
az network public-ip create --resource-group myResourceGroupSLB --name myPublicIP --sku Standard
```

## <a name="create-a-load-balancer"></a>Yük dengeleyici oluşturma

Standart genel aşağıdaki komutu kullanarak önceki adımda oluşturduğunuz IP bir genel Load Balancer Standard oluşturma:

```azurecli-interactive
az network lb create --resource-group myResourceGroupSLB --name myLoadBalancer --public-ip-address myPublicIP --frontend-ip-name myFrontEnd --backend-pool-name myBackEndPool --sku Standard
```

## <a name="create-an-lb-probe-on-port-80"></a>Bağlantı noktası 80 üzerinde bir LB araştırma oluşturma

Aşağıdaki komutu kullanarak bir yük dengeleyici durum araştırması oluşturun:

```azurecli-interactive
az network lb probe create --resource-group myResourceGroupSLB --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80
```

## <a name="create-an-lb-rule-for-port-80"></a>Bağlantı noktası 80 için bir LB kuralı oluşturma

Aşağıdaki komutu kullanarak bir yük dengeleyici kuralı oluşturun:

```azurecli-interactive
az network lb rule create --resource-group myResourceGroup --lb-name myLoadBalancer --name myLoadBalancerRuleWeb \
  --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEnd \
  --backend-pool-name myBackEndPool --probe-name myHealthProbe
```

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Standard Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md).



