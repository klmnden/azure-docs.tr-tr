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
ms.openlocfilehash: 1610ea93eed03fe6efe28e63a7151409e1946f5b
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988723"
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
- Doğu ABD 2
- Japonya Doğu
- Kuzey Avrupa
- Birleşik Krallık Güney
- Batı Avrupa
- Batı ABD
- Batı ABD 2

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Artırmayı destekleyen bir veya daha fazla kaynak için artış istemek üzere bir [Azure destek isteği][azure-support] gönderin (**Sorun türü** olarak "Kota" öğesini seçin).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
