---
title: Azure Kubernetes Hizmeti (AKS) kotaları ve bölge kullanılabilirliği
description: Azure Kubernetes Hizmeti’nin (AKS) varsayılan kotaları ve bölge kullanılabilirliği.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: iainfou
ms.openlocfilehash: c8a2c0cac963fcc0622cff547e85593a13aa076a
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2019
ms.locfileid: "57243848"
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
- Avustralya Güneydoğu
- Orta Kanada
- Doğu Kanada
- Orta ABD
- Doğu Asya
- Doğu ABD
- Doğu ABD 2
- Fransa Orta
- Japonya Doğu
- Kuzey Avrupa
- Güneydoğu Asya
- Güney Hindistan
- Birleşik Krallık Güney
- Birleşik Krallık Batı
- Batı Avrupa
- Batı ABD
- Batı ABD 2

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Artırmayı destekleyen bir veya daha fazla kaynak için artış istemek üzere bir [Azure destek isteği][azure-support] gönderin (**Sorun türü** olarak "Kota" öğesini seçin).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
