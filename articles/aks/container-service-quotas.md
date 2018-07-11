---
title: Azure Kubernetes Hizmeti (AKS) kotaları ve bölge kullanılabilirliği
description: Azure Kubernetes Hizmeti’nin (AKS) varsayılan kotaları ve bölge kullanılabilirliği.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: overview
ms.date: 06/13/2018
ms.author: iainfou
ms.openlocfilehash: 6d4fa9a06f3baeb51505c3e6060e9689428f8386
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100762"
---
# <a name="quotas-and-region-availability-for-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti’nin (AKS) kotaları ve bölge kullanılabilirliği

Tüm Azure hizmetleri, kaynak ve özelliklere yönelik bazı varsayılan limitler ve kotalar içerir. Aşağıdaki bölümlerde, birkaç Azure Kubernetes Hizmeti (AKS) kaynağına ilişkin varsayılan kaynak limitlerinin yanı sıra Azure bölgelerindeki AKS hizmeti kullanılabilirliği açıklanmaktadır.

## <a name="service-quotas-and-limits"></a>Hizmet kotaları ve limitleri

[!INCLUDE [container-service-limits](../../includes/container-service-limits.md)]

## <a name="provisioned-infrastructure"></a>Sağlanan altyapı

Tüm diğer ağ, işlem ve depolama sınırları, sağlanan altyapıya uygulanır. İlgili sınırlar için bkz. [Azure aboneliği ve hizmet sınırları](../azure-subscription-service-limits.md).

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Kubernetes Hizmeti (AKS), aşağıdaki bölgelerde kullanıma sunulmuştur:

- Avustralya Doğu
- Orta Kanada
- Doğu Kanada
- Orta ABD
- Doğu ABD
- Kuzey Avrupa
- Birleşik Krallık Güney
- Batı Avrupa
- Batı ABD
- Batı ABD 2

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Artırmayı destekleyen bir veya daha fazla kaynak için artış istemek üzere bir [Azure destek isteği][azure-support] gönderin (**Sorun türü** olarak "Kota" öğesini seçin).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
