---
title: Azure Kubernetes Hizmeti (AKS) kotaları ve bölge kullanılabilirliği
description: Azure Kubernetes Hizmeti’nin (AKS) varsayılan kotaları ve bölge kullanılabilirliği.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: overview
ms.date: 04/26/2018
ms.author: nepeters
ms.openlocfilehash: 4fb03df470972a2920854ec18554d1cb4b310e88
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="quotas-and-region-availability-for-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti’nin (AKS) kotaları ve bölge kullanılabilirliği

Tüm Azure hizmetleri, kaynak ve özelliklere yönelik bazı varsayılan limitler ve kotalar içerir. Aşağıdaki bölümlerde, birkaç Azure Kubernetes Hizmeti (AKS) kaynağına ilişkin varsayılan kaynak limitlerinin yanı sıra Azure bölgelerindeki AKS hizmeti kullanılabilirliği açıklanmaktadır.

## <a name="service-quotas-and-limits"></a>Hizmet kotaları ve limitleri

[!INCLUDE [container-service-limits](../../includes/container-service-limits.md)]

## <a name="provisioned-infrastructure"></a>Sağlanan altyapı

Tüm diğer ağ, işlem ve depolama sınırları, sağlanan altyapıya uygulanır. İlgili sınırlar için bkz. [Azure aboneliği ve hizmet sınırları](../azure-subscription-service-limits.md).

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Kubernetes Hizmeti (AKS), aşağıdaki bölgelerde önizleme için kullanıma sunulmuştur:
- Doğu ABD
- Batı Avrupa
- Orta ABD
- Orta Kanada
- Doğu Kanada

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Artırmayı destekleyen bir veya daha fazla kaynak için artış istemek üzere lütfen bir [Azure destek isteği][azure-support] gönderin (**Sorun türü** olarak "Kota" öğesini seçin).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
