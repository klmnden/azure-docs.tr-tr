---
title: Azure Container Instances kotaları ve bölge kullanılabilirliği
description: Azure Container Instances hizmetinin varsayılan kotaları ve bölge kullanılabilirliği.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 02/27/2018
ms.author: danlep
ms.openlocfilehash: 2694e8cdc4f1918aab36794804ff48f5a70b44be
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739694"
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Azure Container Instances için kotalar ve bölge kullanılabilirliği

Tüm Azure hizmetleri, kaynak ve özelliklere yönelik bazı varsayılan limitler ve kotalar içerir. Aşağıdaki bölümler birkaç Azure Container Instances (ACI) kaynağına ilişkin varsayılan kaynak limitlerinin yanı sıra Azure bölgelerindeki ACI hizmeti kullanılabilirliğini açıklamaktadır.

## <a name="service-quotas-and-limits"></a>Hizmet kotaları ve limitleri

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Container Instances aşağıdaki bölgelerde belirtilen CPU ve bellek sınırları dahilinde kullanılabilir.

| Konum | İşletim Sistemi | CPU | Bellek (GB) |
| -------- | -- | :---: | :-----------: |
| Doğu ABD, Kuzey Avrupa, Batı Avrupa, Batı ABD, Batı ABD 2 | Linux | 4 | 14 |
| Avustralya Doğu, Doğu ABD 2, Güneydoğu Asya | Linux | 2 | 7 |
| Kanada Orta, Hindistan Orta, ABD Orta Güney | Linux | 2 | 3,5 |
| Doğu ABD, Batı Avrupa, Batı ABD | Windows | 4 | 14 |
| Avustralya Doğu, Kanada Orta, Hindistan Orta, ABD Doğu 2, Kuzey Avrupa, ABD Orta Güney, Güneydoğu Asya, ABD Batı 2 | Windows | 2 | 3,5 |

Bu kaynak sınırları dahilinde oluşturulan kapsayıcı örnekleri, dağıtım bölgesinde kullanılabilirliğe tabidir. Bir bölge ağı yük altında olduğunda, örnek dağıtırken hatayla karşılaşabilirsiniz. Bu tür dağıtım hatalarını azaltmak için, daha düşük CPU ve bellek ayarları ile örnekleri dağıtmayı deneyin veya dağıtımınızı daha sonra gerçekleştirmeyi deneyin.

[aka.ms/aci/feedback](https://aka.ms/aci/feedback) adresinde, ek bölgelerin CPU/Bellek sınırlarına ihtiyacı olduğunu veya bu sınırları artırdığını takıma haber verin.

Kapsayıcı örneği dağıtımıyla ilgili sorunları giderme hakkında daha fazla bilgi için, bkz. [Azure Container Instances ile ilgili dağıtım sorunlarını giderme](container-instances-troubleshooting.md).

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Artırmayı destekleyen bir veya daha fazla kaynak için artış istemek üzere lütfen bir [Azure destek isteği][azure-support] gönderin (**Sorun türü** olarak "Kota" öğesini seçin).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
