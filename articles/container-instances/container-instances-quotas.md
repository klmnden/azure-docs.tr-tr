---
title: Azure Container Instances kotaları ve bölge kullanılabilirliği
description: Azure Container Instances hizmetinin varsayılan kotaları ve bölge kullanılabilirliği.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 12/17/2018
ms.author: danlep
ms.openlocfilehash: 86c169c84e905362e8fdb069e8ceadcb5e19fe67
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53602401"
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Azure Container Instances için kotalar ve bölge kullanılabilirliği

Tüm Azure hizmetleri, kaynak ve özelliklere yönelik bazı varsayılan limitler ve kotalar içerir. Aşağıdaki bölümler birkaç Azure Container Instances (ACI) kaynağına ilişkin varsayılan kaynak limitlerinin yanı sıra Azure bölgelerindeki ACI hizmeti kullanılabilirliğini açıklamaktadır.

## <a name="service-quotas-and-limits"></a>Hizmet kotaları ve limitleri

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Container Instances aşağıdaki bölgelerde belirtilen CPU ve bellek sınırları dahilinde kullanılabilir. Geçerli yayın zamanında değerlerdir. Güncel bilgiler için kullanın [listesi özellikleri](/rest/api/container-instances/listcapabilities/listcapabilities) API.

| Konum | İşletim Sistemi | CPU | Bellek (GB) |
| -------- | -- | :---: | :-----------: |
| Orta Kanada | Linux | 4 | 16 |
| Doğu ABD, Kuzey Avrupa, Batı Avrupa, Batı ABD, Batı ABD 2 | Linux | 4 | 14 |
| Japonya Doğu | Linux | 2 | 8 |
| Avustralya Doğu, Doğu ABD 2, Güneydoğu Asya | Linux | 2 | 7 |
| Orta Hindistan, Doğu Asya, Kuzey Orta ABD, Güney Orta ABD, Güney Hindistan | Linux | 2 | 3,5 |
| Doğu ABD, Batı Avrupa, Batı ABD | Windows | 4 | 14 |
| Avustralya Doğu, Kanada Orta, Orta Hindistan, Doğu Asya, Doğu ABD 2, Doğu Japonya Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güney Hindistan, Güneydoğu Asya, Batı ABD 2 | Windows | 2 | 3,5 |

Bu kaynak sınırları dahilinde oluşturulan kapsayıcı örnekleri, dağıtım bölgesinde kullanılabilirliğe tabidir. Bir bölge ağı yük altında olduğunda, örnek dağıtırken hatayla karşılaşabilirsiniz. Bu tür dağıtım hatalarını azaltmak için, daha düşük CPU ve bellek ayarları ile örnekleri dağıtmayı deneyin veya dağıtımınızı daha sonra gerçekleştirmeyi deneyin.

[aka.ms/aci/feedback](https://aka.ms/aci/feedback) adresinde, ek bölgelerin CPU/Bellek sınırlarına ihtiyacı olduğunu veya bu sınırları artırdığını takıma haber verin.

Kapsayıcı örneği dağıtımıyla ilgili sorunları giderme hakkında daha fazla bilgi için, bkz. [Azure Container Instances ile ilgili dağıtım sorunlarını giderme](container-instances-troubleshooting.md).

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Artırmayı destekleyen bir veya daha fazla kaynak için artış istemek üzere lütfen bir [Azure destek isteği][azure-support] gönderin (**Sorun türü** olarak "Kota" öğesini seçin).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
