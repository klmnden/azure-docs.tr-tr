---
title: Azure Container Instances kotaları ve bölge kullanılabilirliği
description: Kotalar, sınırlar ve Azure Container Instances hizmetinin bölge kullanılabilirliği.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 02/15/2019
ms.author: danlep
ms.openlocfilehash: c676989b4b882f2b1887a1b6a5091b60027f61d0
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56328421"
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Azure Container Instances için kotalar ve bölge kullanılabilirliği

Tüm Azure hizmetleri, kaynak ve özelliklere yönelik bazı varsayılan limitler ve kotalar içerir. Aşağıdaki bölümler birkaç Azure Container Instances kaynakları yanı sıra Azure bölgelerindeki hizmeti kullanılabilirliğini ilişkin varsayılan kaynak limitlerinin ayrıntılı olarak açıklanmaktadır.

## <a name="service-quotas-and-limits"></a>Hizmet kotaları ve limitleri

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="feature-availability"></a>Özellik kullanılabilirliği

Azure Container Instances, aynı API ile hem Windows hem de Linux kapsayıcıları zamanlayabilir. Ancak, aşağıdaki özellikler yalnızca Linux kapsayıcı grupları şu anda kullanılabilir. Windows desteği planlanmaktadır.

* Kapsayıcı grubu başına birden çok kapsayıcı
* Birim bağlama (Azure dosyaları, emptyDir, GitRepo, gizli)
* Sanal ağ (Önizleme)
* GPU kaynakları (Önizleme)

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Container Instances aşağıdaki bölgelerde belirtilen CPU ve bellek sınırları her kapsayıcı grubu için kullanılabilir. Geçerli yayın zamanında değerlerdir. Güncel bilgiler için kullanın [listesi özellikleri](/rest/api/container-instances/listcapabilities/listcapabilities) API. 

Azure Container Instances ile kullanırken, kullanılabilirlik ve kaynak sınırları değişebilir bir [sanal ağ](container-instances-vnet.md) (Önizleme) ile veya [GPU kaynakları](container-instances-gpu.md) (Önizleme).

| Konum | İşletim Sistemi | CPU | Bellek (GB) |
| -------- | -- | :---: | :-----------: |
| Kanada Orta, Orta ABD, Doğu ABD 2, Orta Güney ABD | Linux | 4 | 16 |
| Doğu ABD, Kuzey Avrupa, Batı Avrupa, Batı ABD, Batı ABD 2 | Linux | 4 | 14 |
| Japonya Doğu | Linux | 2 | 8 |
| Doğu Avustralya, Güneydoğu Asya | Linux | 2 | 7 |
| Orta Hindistan, Doğu Asya, Kuzey Orta ABD, Güney Hindistan | Linux | 2 | 3,5 |
| Doğu ABD, Batı Avrupa, Batı ABD | Windows | 4 | 14 |
| Avustralya Doğu, Kanada Orta, Orta Hindistan, Orta ABD, Doğu Asya, Doğu ABD 2, Japonya Doğu, Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güney Hindistan, Güneydoğu Asya, Batı ABD 2 | Windows | 2 | 3,5 |

Bu kaynak sınırları dahilinde oluşturulan kapsayıcı örnekleri, dağıtım bölgesinde kullanılabilirliğe tabidir. Bir bölge ağı yük altında olduğunda, örnek dağıtırken hatayla karşılaşabilirsiniz. Bu tür dağıtım hatalarını azaltmak için, daha düşük CPU ve bellek ayarları ile örnekleri dağıtmayı deneyin veya dağıtımınızı daha sonra gerçekleştirmeyi deneyin.

[aka.ms/aci/feedback](https://aka.ms/aci/feedback) adresinde, ek bölgelerin CPU/Bellek sınırlarına ihtiyacı olduğunu veya bu sınırları artırdığını takıma haber verin.

Kapsayıcı örneği dağıtımıyla ilgili sorunları giderme hakkında daha fazla bilgi için, bkz. [Azure Container Instances ile ilgili dağıtım sorunlarını giderme](container-instances-troubleshooting.md).

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Artırmayı destekleyen bir veya daha fazla kaynak için artış istemek üzere lütfen bir [Azure destek isteği][azure-support] gönderin (**Sorun türü** olarak "Kota" öğesini seçin).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
