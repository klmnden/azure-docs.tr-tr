---
title: Azure Container Instances kaynak kullanılabilirliği
description: İşlem ve bellek kaynakları farklı Azure bölgelerindeki Azure Container Instances hizmetinin kullanılabilirliği.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 03/01/2019
ms.author: danlep
ms.openlocfilehash: 1ca23a95c746139963aa70ed20bb888152fd5cd8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60537773"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Azure bölgelerinde Azure Container Instances için kaynak kullanılabilirliği

Bu makalede, Azure bölgelerinde Azure Container Instances işlem ve bellek kaynaklarının ayrıntıları. 

Sunulan en çok kaynak dağıtımını kullanılabilir değerler bir [kapsayıcı grubu](container-instances-container-groups.md). Geçerli yayın zamanında değerlerdir. Güncel bilgiler için kullanın [listesi özellikleri](/rest/api/container-instances/listcapabilities/listcapabilities) API. 

> [!NOTE]
> Bu kaynak sınırları dahilinde oluşturulan kapsayıcı dağıtım bölgesinde kullanılabilirliğe gruplarıdır. Bir bölge ağı yük altında olduğunda, örnek dağıtırken hatayla karşılaşabilirsiniz. Bu tür dağıtım hatalarını azaltmak için daha düşük kaynak ayarları ile örnekleri dağıtmayı deneyin veya dağıtımınızı daha sonra deneyin.

Kotalar ve diğer dağıtımlarınızı sınırları hakkında daha fazla bilgi için bkz. [Azure Container Instances için kotalar ve sınırlar](container-instances-quotas.md).

## <a name="availability---general"></a>Kullanılabilirlik - genel

| Location | İşletim Sistemi | CPU | Bellek (GB) |
| -------- | -- | :---: | :-----------: |
| Kanada Orta, Orta ABD, Doğu ABD 2, Orta Güney ABD | Linux | 4 | 16 |
| Doğu ABD, Kuzey Avrupa, Batı Avrupa, Batı ABD, Batı ABD 2 | Linux | 4 | 14 |
| Japonya Doğu | Linux | 2 | 8 |
| Doğu Avustralya, Güneydoğu Asya | Linux | 2 | 7 |
| Orta Hindistan, Doğu Asya, Kuzey Orta ABD, Güney Hindistan | Linux | 2 | 3,5 |
| Doğu ABD, Batı Avrupa, Batı ABD | Windows | 4 | 14 |
| Avustralya Doğu, Kanada Orta, Orta Hindistan, Orta ABD, Doğu Asya, Doğu ABD 2, Japonya Doğu, Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güney Hindistan, Güneydoğu Asya, Batı ABD 2 | Windows | 2 | 3,5 |

## <a name="availability---virtual-network-deployment-preview"></a>Kullanılabilirlik - sanal ağ dağıtımı (Önizleme)

Aşağıdaki bölgeler ve kaynakları dağıtılmış bir kapsayıcı grubu için kullanılabilir olan bir [Azure sanal ağı](container-instances-vnet.md) (Önizleme)

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Kullanılabilirlik - GPU kaynakları (Önizleme)

Aşağıdaki bölgeler ve kaynakları ile dağıtılan bir kapsayıcı grubu için kullanılabilir olan [GPU kaynakları](container-instances-gpu.md) (Önizleme)

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ek bölgeler ve en yüksek kullanılabilirliğini görmek istiyorsanız takıma haber [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Kapsayıcı örneği dağıtımıyla ilgili sorunları giderme hakkında daha fazla bilgi için bkz: [Azure Container Instances ile dağıtım sorunlarını giderme](container-instances-troubleshooting.md).
