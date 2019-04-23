---
title: Azure Kubernetes Hizmeti (AKS) kotaları ve bölge kullanılabilirliği
description: Azure Kubernetes Hizmeti’nin (AKS) varsayılan kotaları ve bölge kullanılabilirliği.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: iainfou
ms.openlocfilehash: 8feeaf2e8ee99405ed0de8291fc97dc50db6a386
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59798853"
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
- Kanada Doğu
- Orta Hindistan
- Orta ABD
- Doğu Asya
- Doğu ABD
- Doğu ABD 2
- Fransa Orta
- Japonya Doğu
- Kore Orta
- Kore Güney
- Kuzey Avrupa
- Güneydoğu Asya
- Orta Güney ABD
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
