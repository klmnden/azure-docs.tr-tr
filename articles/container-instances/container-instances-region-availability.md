---
title: "Azure Container Instances bölge ve kaynak kullanılabilirliği"
description: "Hangi Azure bölgelerinin kapsayıcı örneklerini dağıtmayı desteklediğini ve bu örnekler için CPU ve bellek sınırlarını öğrenin."
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 08/31/2017
ms.author: marsma
ms.openlocfilehash: ace4eb6b284f2c1b2caeb54c1d686e68cacb1725
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="region-availability-for-azure-container-instances"></a>Azure Container Instances için bölge kullanılabilirliği

Önizleme sırasında, Azure Container Instances aşağıdaki bölgelerde belirtilen CPU ve bellek sınırları dahilinde kullanılabilir.

| Konum | İşletim Sistemi | CPU | Bellek (GB) |
| -------- | -- | :---: | :-----------: |
| Batı Avrupa, Batı ABD, Doğu ABD | Linux | 2 | 7 |
| Batı Avrupa, Batı ABD, Doğu ABD | Windows | 2 | 3,5 |

## <a name="resource-availability"></a>Kaynak kullanılabilirliği

Bu kaynak sınırları dahilinde oluşturulan kapsayıcı örnekleri, dağıtım bölgesinde kullanılabilirliğe tabidir. Bir bölge ağı yük altında olduğunda, örnek dağıtırken hatayla karşılaşabilirsiniz.

Bu tür dağıtım hatalarını azaltmak için, daha düşük CPU ve bellek ayarları ile örnekleri dağıtmayı deneyin veya dağıtımınızı daha sonra gerçekleştirmeyi deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Kapsayıcı örneği dağıtımıyla ilgili sorunları giderme hakkında daha fazla bilgi için, bkz. [Azure Container Instances ile ilgili dağıtım sorunlarını giderme](container-instances-troubleshooting.md).