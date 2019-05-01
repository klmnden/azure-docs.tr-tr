---
title: Kotalar, SKU ve bölge kullanılabilirliği Azure Kubernetes Service (AKS)
description: Varsayılan kotaları, kısıtlı düğümü VM SKU boyutları ve bölge kullanılabilirliği Azure Kubernetes Service (AKS) hakkında bilgi edinin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: iainfou
ms.openlocfilehash: abeb9ef6e467b62cf7332e01e1b77c710b9ba4f4
ms.sourcegitcommit: 524625dd12e0537173616a991802075e2dc9da12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "64413015"
---
# <a name="quotas-virtual-machine-size-restrictions-and-region-availability-in-azure-kubernetes-service-aks"></a>Bölge kullanılabilirliği Azure Kubernetes Service (AKS) kotaları ve sanal makine boyutu sınırlamaları

Tüm Azure hizmetleri, kaynak ve özelliklere yönelik bazı varsayılan limitler ve kotalar içerir. Düğüm boyutu için belirli bir sanal makine (VM) sonra SKU'lar kullanım için kısıtlanmış.

Bu makalede, Azure bölgelerindeki AKS hizmeti kullanılabilirliği yanı sıra, Azure Kubernetes Service (AKS) kaynağına ilişkin varsayılan kaynak limitlerinin ayrıntıları.

## <a name="service-quotas-and-limits"></a>Hizmet kotaları ve limitleri

[!INCLUDE [container-service-limits](../../includes/container-service-limits.md)]

## <a name="provisioned-infrastructure"></a>Sağlanan altyapı

Tüm diğer ağ, işlem ve depolama sınırları, sağlanan altyapıya uygulanır. İlgili sınırlar için bkz. [Azure aboneliği ve hizmet sınırları](../azure-subscription-service-limits.md).

## <a name="restricted-vm-sizes"></a>Kısıtlı VM boyutları

Bir AKS kümesindeki her düğüm, sabit bir vCPU ve bellek gibi kaynakların miktarını içerir. Bir AKS düğümü yeterli işlem kaynakları içeriyorsa, pod'ların düzgün çalışması başarısız olabilir. Gerekli emin olmak için *kube-system* pod'ların ve uygulamalarınızın güvenilir bir şekilde zamanlanabilir, AKS içinde aşağıdaki VM SKU'ları kullanılamaz:

- Standard_A0
- Standard_A1
- Standard_A1_v2
- Standard_B1s
- Standard_B1ms
- Standard_F1
- Standard_F1s

VM türleri ve işlem kaynaklarını hakkında daha fazla bilgi için bkz. [azure'da sanal makine boyutları][vm-skus].

## <a name="region-availability"></a>Bölge kullanılabilirliği

Where en son listesi için dağıtma ve kümeleri çalıştırın, bkz: [AKS bölge kullanılabilirliği][region-availability].

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Artırmayı destekleyen bir veya daha fazla kaynak için artış istemek üzere bir [Azure destek isteği][azure-support] gönderin (**Sorun türü** olarak "Kota" öğesini seçin).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[region-availability]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service

<!-- LINKS - Internal -->
[vm-skus]: ../virtual-machines/linux/sizes.md
