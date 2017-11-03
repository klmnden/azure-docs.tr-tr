---
title: "Bir genel yük dengeleyiciye standart Azure CLI kullanarak bölge olarak yedekli genel IP adresi ön uç ile oluşturma | Microsoft Docs"
description: "Bir genel yük dengeleyiciye standart Azure CLI kullanarak bölge olarak yedekli genel IP adresi ön uç ile oluşturmayı öğrenin"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2017
ms.author: kumud
ms.openlocfilehash: 769eb86af3e0506ddf03d1ec616d5a17b7e5f714
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-frontend-using-azure-cli"></a>Bir genel yük dengeleyiciye standart Azure CLI kullanarak bölge olarak yedekli ön uç ile oluşturma

Bu makalede adımları genel oluşturmada size [yük dengeleyici standart](https://aka.ms/azureloadbalancerstandard) ortak IP standart bir adresi kullanarak bir bölge olarak yedekli ön ile.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="register-for-availability-zones-load-balancer-standard-and-public-ip-standard-preview"></a>Kullanılabilirlik bölgeleri, yük dengeleyici standart ve genel IP Standard Önizleme için kaydolun

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.17 çalıştırmasını gerektirir ya da daha yüksek.  Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

>[!NOTE]
[Yük Dengeleyici standart SKU](https://aka.ms/azureloadbalancerstandard) şu anda önizlemede değil. Önizleme sırasında bu özellik genel kullanılabilirlik sunumundaki özelliklerle aynı seviyede kullanılabilirliğe ve güvenilirliğe sahip olmayabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Microsoft Azure Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Genel olarak kullanılabilir kullanmak [yük dengeleyici temel SKU](load-balancer-overview.md) üretim hizmetleriniz için. 

> [!NOTE]
> Kullanılabilirlik bölgeleri önizlemede ve geliştirme için hazır olduğunu ve test senaryoları. Destek select Azure kaynaklarını ve bölgeler ve VM boyutu aileleri için kullanılabilir. Başlamak hakkında daha fazla bilgi ve hangi Azure kaynaklarını, bölgeler ve kullanılabilirlik bölgeleri deneyebilirsiniz VM boyutu aileleri için bkz: [kullanılabilirlik bölgeleri genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

Bir bölge veya yük dengeleyici için ön uç genel IP adresi için bölge olarak yedekli seçeneği belirlemeden önce ilk adımları tamamlamanız gereken [kullanılabilirlik bölgeleri Önizleme için kaydetmek](https://docs.microsoft.com/azure/availability-zones/az-overview).

En son yüklediğinizden emin olun [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)ve bir Azure hesabı ile oturum açmış [az oturum açma](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest#login).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Aşağıdaki komutu kullanarak bir kaynak grubu oluşturun:

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

## <a name="create-a-public-ip-standard"></a>Ortak IP standart oluşturma

Bir ortak IP aşağıdaki komutu kullanarak standart oluşturun:

```azurecli-interactive
az network public-ip create --resource-group myResourceGroup --name myPublicIP --sku Standard
```

## <a name="create-a-load-balancer"></a>Yük dengeleyici oluşturma

Bir genel yük dengeleyiciye standart standart genel aşağıdaki komutu kullanarak önceki adımda oluşturduğunuz IP oluşturun:

```azurecli-interactive
az network lb create --resource-group myResourceGroup --name myLoadBalancer --public-ip-address myPublicIP --frontend-ip-name myFrontEndPool --backend-pool-name myBackEndPool --sku Standard
```

## <a name="create-an-lb-probe-on-port-80"></a>Bağlantı noktası 80 üzerinde bir LB araştırması oluştur

Aşağıdaki komutu kullanarak bir yük dengeleyici durum araştırması oluşturun:

```azurecli-interactive
az network lb probe create --resource-group myResourceGroup --lb-name myLoadBalancer \
  --name myHealthProbe --protocol tcp --port 80
```

## <a name="create-an-lb-rule-for-port-80"></a>80 numaralı bağlantı noktası için bir LB kuralı oluşturma

Aşağıdaki komutu kullanarak bir yük dengeleyici kuralı oluşturun:

```azurecli-interactive
az network lb rule create --resource-group myResourceGroup --lb-name myLoadBalancer --name myLoadBalancerRuleWeb \
  --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-pool-name myBackEndPool --probe-name myHealthProbe
```

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi nasıl [bir kullanılabilirlik bölgesinde bir genel IP oluşturun](../virtual-network/create-public-ip-availability-zone-cli.md)



