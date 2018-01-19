---
title: "Azure Container Instances kotaları ve bölge kullanılabilirliği"
description: "Azure Container Instances hizmetinin varsayılan kotaları ve bölge kullanılabilirliği."
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 01/11/2018
ms.author: marsma
ms.openlocfilehash: baf93d4a2a4ba1e05bbf558d0c056fa3aa833fef
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Azure Container Instances için kotalar ve bölge kullanılabilirliği

Tüm Azure hizmetleri, kaynak ve özelliklere yönelik bazı varsayılan limitler ve kotalar içerir. Aşağıdaki bölümler birkaç Azure Container Instances (ACI) kaynağına ilişkin varsayılan kaynak limitlerinin yanı sıra Azure bölgelerindeki ACI hizmeti kullanılabilirliğini açıklamaktadır.

## <a name="service-quotas-and-limits"></a>Hizmet kotaları ve limitleri

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Container Instances aşağıdaki bölgelerde belirtilen CPU ve bellek sınırları dahilinde kullanılabilir.

| Konum | İşletim Sistemi | CPU | Bellek (GB) |
| -------- | -- | :---: | :-----------: |
| Batı Avrupa, Batı ABD, Doğu ABD, Güneydoğu Asya | Linux | 4 | 14 |
| Batı Avrupa, Batı ABD, Doğu ABD, Güneydoğu Asya  | Windows | 4 | 14 |

Bu kaynak sınırları dahilinde oluşturulan kapsayıcı örnekleri, dağıtım bölgesinde kullanılabilirliğe tabidir. Bir bölge ağı yük altında olduğunda, örnek dağıtırken hatayla karşılaşabilirsiniz. Bu tür dağıtım hatalarını azaltmak için, daha düşük CPU ve bellek ayarları ile örnekleri dağıtmayı deneyin veya dağıtımınızı daha sonra gerçekleştirmeyi deneyin.

Kapsayıcı örneği dağıtımıyla ilgili sorunları giderme hakkında daha fazla bilgi için, bkz. [Azure Container Instances ile ilgili dağıtım sorunlarını giderme](container-instances-troubleshooting.md).

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Artırmayı destekleyen bir veya daha fazla kaynak için artış istemek üzere lütfen bir [Azure destek isteği][azure-support] gönderin (**Sorun türü** olarak "Kota" öğesini seçin).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
