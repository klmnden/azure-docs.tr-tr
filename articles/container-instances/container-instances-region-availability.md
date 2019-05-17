---
title: Azure Container Instances kaynak kullanılabilirliği
description: İşlem ve bellek kaynakları farklı Azure bölgelerindeki Azure Container Instances hizmetinin kullanılabilirliği.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 05/14/2019
ms.author: danlep
ms.openlocfilehash: 64b60178413e470cc7fe9b3991c6fc29b5a0f860
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65794288"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Azure bölgelerinde Azure Container Instances için kaynak kullanılabilirliği

Bu makalede, Azure bölgelerinde Azure Container Instances işlem ve bellek kaynaklarının ayrıntıları. 

Sunulan en çok kaynak dağıtımını kullanılabilir değerler bir [kapsayıcı grubu](container-instances-container-groups.md). Geçerli yayın zamanında değerlerdir. 

> [!NOTE]
> Bu kaynak sınırları dahilinde oluşturulan kapsayıcı dağıtım bölgesinde kullanılabilirliğe gruplarıdır. Bir bölge ağı yük altında olduğunda, örnek dağıtırken hatayla karşılaşabilirsiniz. Bu tür dağıtım hatalarını azaltmak için daha düşük kaynak ayarları ile örnekleri dağıtmayı deneyin veya dağıtımınızı daha sonra deneyin.

Kotalar ve diğer dağıtımlarınızı sınırları hakkında daha fazla bilgi için bkz. [Azure Container Instances için kotalar ve sınırlar](container-instances-quotas.md).

## <a name="availability---general"></a>Kullanılabilirlik - genel

Aşağıdaki bölgeler ve kaynakları, Linux kapsayıcı gruplarıyla kullanılabilir ve [desteklenen](container-instances-faq.md#what-windows-base-os-images-are-supported) kapsayıcıları, Windows Server 2016'ya göre.

| Location | İşletim Sistemi | CPU | Bellek (GB) |
| -------- | -- | :---: | :-----------: |
| Kanada Orta, Orta Hindistan, Orta ABD, Doğu Asya, Doğu ABD, Doğu ABD 2, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, UK Güney, Batı ABD | Linux | 4 | 16 |
| Batı Avrupa, Batı ABD 2 | Linux | 4 | 14 |
| Avustralya Doğu, Japonya Doğu | Linux | 2 | 8 |
| Kuzey Orta ABD, Güney Hindistan | Linux | 2 | 3,5 |
| Batı Avrupa | Windows | 4 | 16 |
| Doğu ABD, Batı ABD | Windows | 4 | 14 |
| Avustralya Doğu, Kanada Orta, Orta Hindistan, Orta ABD, Doğu Asya, Doğu ABD 2, Japonya Doğu, Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, Güney Hindistan, UK Güney, Batı ABD 2 | Windows | 2 | 3,5 |

## <a name="availability---windows-server-2019-ltsc-1809-deployments-preview"></a>Kullanılabilirlik - Windows Server 2019 LTSC, 1809 dağıtımlar (Önizleme)

Aşağıdaki bölgeler ve kaynakları kapsayıcı grupları Windows Server 2019 tabanlı kapsayıcılar (Önizleme) ile kullanılabilir.

| Location | İşletim Sistemi | CPU | Bellek (GB) |
| -------- | -- | :---: | :-----------: |
| Güneydoğu Asya, Kuzey Avrupa, Batı Avrupa, Orta ABD, Doğu ABD, Batı ABD, Batı ABD 2 | Windows | 4 | 16 |
| Doğu ABD 2 | Windows | 2 | 3,5 |


## <a name="availability---virtual-network-deployment-preview"></a>Kullanılabilirlik - sanal ağ dağıtımı (Önizleme)

Aşağıdaki bölgeler ve kaynakları dağıtılmış bir kapsayıcı grubu için kullanılabilir olan bir [Azure sanal ağı](container-instances-vnet.md) (Önizleme).

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Kullanılabilirlik - GPU kaynakları (Önizleme)

Aşağıdaki bölgeler ve kaynakları ile dağıtılan bir kapsayıcı grubu için kullanılabilir olan [GPU kaynakları](container-instances-gpu.md) (Önizleme).

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ek bölgeler ve en yüksek kullanılabilirliğini görmek istiyorsanız takıma haber [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Kapsayıcı örneği dağıtımıyla ilgili sorunları giderme hakkında daha fazla bilgi için bkz: [Azure Container Instances ile dağıtım sorunlarını giderme](container-instances-troubleshooting.md).
