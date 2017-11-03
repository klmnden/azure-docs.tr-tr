---
title: "Azure CLI ile zoned bir ortak IP adresi oluşturun | Microsoft Docs"
description: "Azure CLI ile bir kullanılabilirlik bölgesindeki bir genel IP oluşturun."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: ef93d43bbd0c58950027810c8c335d9076574326
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-the-azure-cli"></a>Azure CLI ile bir kullanılabilirlik bölgesindeki bir ortak IP adresi oluştur

Bir ortak IP adresi Azure kullanılabilirlik bölgesinde (Önizleme) dağıtabilirsiniz. Bir Azure bölgesi fiziksel olarak ayrı bir bölgede bir kullanılabilirlik bölgedir. Şunları nasıl yapacağınızı öğrenin:

> * Bir kullanılabilirlik bölgesinde bir ortak IP adresi oluşturma
> * Kullanılabilirlik bölgede oluşturulan ilgili kaynakları belirleyin

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI sürümü 2.0.17 sürümünden büyük çalıştırmasını gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

> [!NOTE]
> Kullanılabilirlik bölgeleri önizlemede ve geliştirme için hazır olduğunu ve test senaryoları. Destek select Azure kaynaklarını ve bölgeler ve VM boyutu aileleri için kullanılabilir. Başlamak hakkında daha fazla bilgi ve hangi Azure kaynaklarını, bölgeler ve kullanılabilirlik bölgeleri deneyebilirsiniz VM boyutu aileleri için bkz: [kullanılabilirlik bölgeleri genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview). Destek için [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) üzerinden bize ulaşabilir veya [bir Azure destek bileti açabilirsiniz](../azure-supportability/how-to-create-azure-support-request.md).

## <a name="create-a-zonal-public-ip-address"></a>Zonal bir ortak IP adresi oluşturun

Bir ortak IP adresi aşağıdaki komutu kullanarak, kullanılabilirlik bölge oluşturun:


```azurecli-interactive
az network public-ip create --resource-group myResourceGroup --name myPublicIp --zone 2 --location westeurope
```
> [!NOTE]
> Standart bir SKU genel IP adresini bir sanal makinenin ağ arabirimine atadığınızda amaçlanan trafiğe bir [ağ güvenlik grubuyla](security-overview.md#network-security-groups) açıkça izin vermeniz gerekir. Bir ağ güvenlik grubu oluşturup ilişkilendirene ve istenen trafiğe açıkça izin verene kadar kaynakla erişim kurma girişimleri başarısız olur.

## <a name="get-zone-information-about-a-public-ip-address"></a>Bölge Genel bir IP adresi hakkında bilgi edinin

Bölge bilgileri aşağıdaki komutu kullanarak genel bir IP adresi alın:

```azurecli-interactive
az network public-ip show --resource-group myResourceGroup --name myPublicIp
```

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [kullanılabilirlik bölgeleri](https://docs.microsoft.com/azure/availability-zones/az-overview)
- Daha fazla bilgi edinmek [genel IP adresleri](../virtual-network/virtual-network-public-ip-address.md) 
