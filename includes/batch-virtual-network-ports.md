---
title: include dosyası
description: include dosyası
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 04/10/2019
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 711b662c35b5f8fec96f1edee765696bc1028bf8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66127505"
---
### <a name="general-requirements"></a>Genel gereksinimler

* Sanal ağın havuzunuzu oluşturmak için kullandığınız Batch hesabıyla aynı abonelikte ve bölgede olması gerekir.

* Sanal ağı kullanan havuzda en fazla 4096 düğüm bulunabilir.

* Havuz için belirtilen alt ağda havuz için hedeflenen VM sayısına yetecek kadar atanmamış IP adresi bulunması gerekir. Başka bir deyişle bu değerin havuzun `targetDedicatedNodes` ve `targetLowPriorityNodes` özelliklerinin toplamı olması gerekir. Alt ağda yeterli sayıda atanmamış IP adresi yoksa havuz işlem düğümlerini kısmen ayırır ve bir yeniden boyutlandırma hatası oluşur. 

* Azure Depolama uç noktanızın sanal ağınızda kullanılan özel DNS sunucuları tarafından çözümlenebilmesi gerekir. Özellikle `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` ve `<account>.blob.core.windows.net` biçimindeki URL'ler çözümlenebilir. 

Batch havuzunun Sanal Makine yapılandırmasında veya Cloud Services yapılandırmasında olma durumunda göre ek sanal ağ gereksinimleri farklı olabilir. Sanal ağa yapılacak yeni havuz dağıtımları için Sanal Makine yapılandırmasının kullanılması önerilir.

### <a name="pools-in-the-virtual-machine-configuration"></a>Sanal Makine yapılandırmasındaki havuzlar

**Desteklenen ağlar** - Yalnızca Azure Resource Manager tabanlı sanal ağlar

**Alt ağ kimliği** - Alt ağı Batch API'leri ile belirtirken alt ağın *kaynak tanımlayıcısını* kullanın. Alt ağ tanımlayıcısı şu biçimdedir:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.Network/virtualNetworks/{network}/subnets/{subnet}
  ```

**İzinler** - Sanal ağ aboneliği veya kaynak grubu güvenlik ilkelerinin veya kilitlerinin belirli bir kullanıcının sanal ağ yönetim izinlerini kısıtlayıp kısıtlamadığını kontrol edin.

**Ek ağ kaynakları** - Batch, sanal ağı içeren kaynak grubuna otomatik olarak ek ağ kaynakları atar. Her 50 adanmış düğümler (veya her 20 düşük öncelikli düğümler) için Batch ayırır: Yük Dengeleyici 1 ağ güvenlik grubu (NSG), 1 genel IP adresini ve 1. Bu kaynaklar, aboneliğin [kaynak kotalarıyla](../articles/azure-subscription-service-limits.md) sınırlıdır. Büyük havuzlar için bu kaynaklardan birinde veya daha fazlasında kota artışı istemeniz gerekebilir.

#### <a name="network-security-groups"></a>Ağ güvenlik grupları

İşlem düğümlerinde görev zamanlayabilmek için alt ağın Batch hizmetinden gelen iletişim isteklerine, Azure Depolama veya diğer kaynaklarla iletişim kurabilmek için de giden iletişim isteklerine izin vermesi gerekir. Batch, Sanal Makine yapılandırmasındaki havuzlar için VM'lere ekli ağ arabirimleri (NIC) düzeyinde NSG'ler ekler. Bu NSG'ler şu trafiğe izin vermek için gelen ve giden bağlantı kurallarını otomatik olarak yapılandırır:

* Batch hizmet rolü IP adreslerinden 29876 ve 29877 numaralı bağlantı noktalarına gelen TCP trafiği. 
* Uzaktan erişime izin vermek için 22 (Linux düğümleri) veya 3389 (Windows düğümler) numaralı bağlantı noktasından gelen TCP trafiği. Belirli çok örnekli görevler (MPI gibi) Linux'ta türleri, aynı zamanda SSH bağlantı noktası 22 IP'ler için Batch işlem düğümleri içeren alt ağında izin verecek şekilde gerekir.
* Sanal ağa giden herhangi bir bağlantı noktasında giden trafik.
* İnternete giden herhangi bir bağlantı noktasında giden trafik.

> [!IMPORTANT]
> Batch tarafından yapılandırılmış olan NSG'lerdeki gelen veya giden kurallarını değiştirirken veya yenilerini eklerken dikkatli olun. Belirtilen alt ağdaki işlem düğümleriyle iletişim kurulması bir NSG tarafından reddedilirse Batch hizmeti, işlem düğümlerinin durumunu **kullanılamıyor** olarak ayarlar.

Batch kendi NSG'lerini yapılandırdığından alt ağ düzeyinde NSG belirtmenize gerek yoktur. Ancak belirtilen alt ağ ile ilişkilendirilmiş Ağ Güvenlik Grupları (NSG) ve/veya güvenlik duvarı varsa gelen ve giden güvenlik kurallarını aşağıdaki tablolarda gösterilen şekilde yapılandırın. Yalnızca dış kaynaklardan havuzu Vm'lere uzaktan erişime izin vermek gerekirse, gelen trafiği 3389 numaralı bağlantı noktasında (Windows) veya 22 (Linux) yapılandırın. Bu ayar havuz VM'lerinin kullanılabilir durumda olması için şart değildir. Çok örnekli görevler MPI gibi belirli türdeki kullanırken, Linux için sanal ağ alt ağ trafiği 22 numaralı bağlantı noktasında etkinleştirmek gerektiğine dikkat edin.

**Gelen güvenlik kuralları**

| Kaynak IP adresleri | Kaynak hizmeti etiketi | Kaynak bağlantı noktaları | Hedef | Hedef bağlantı noktaları | Protocol | Eylem |
| --- | --- | --- | --- | --- | --- | --- |
| Yok | `BatchNodeManagement` [Hizmet etiketi](../articles/virtual-network/security-overview.md#service-tags) | * | Tüm | 29876-29877 | TCP | İzin Ver |
| Kullanıcı kaynak IP'leri gerekirse işlem düğümleri ve/veya işlem düğüm alt Linux, çok örnekli görevler için uzaktan erişim için. | Yok | * | Tüm | 3389 (Windows), 22 (Linux) | TCP | İzin Ver |

**Giden güvenlik kuralları**

| source | Kaynak bağlantı noktaları | Hedef | Hedef hizmet etiketi | Protocol | Eylem |
| --- | --- | --- | --- | --- | --- |
| Tüm | 443 | [Hizmet etiketi](../articles/virtual-network/security-overview.md#service-tags) | `Storage` (aynı bölgede Batch hesabı ve sanal ağ olarak)  | Tüm | İzin Ver |

### <a name="pools-in-the-cloud-services-configuration"></a>Bulut Hizmetleri yapılandırmasındaki havuzlar

**Desteklenen sanal ağlar** - Yalnızca klasik sanal ağlar

**Alt ağ kimliği** - Alt ağı Batch API'leri ile belirtirken alt ağın *kaynak tanımlayıcısını* kullanın. Alt ağ tanımlayıcısı şu biçimdedir:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.ClassicVirtualNetwork /virtualNetworks/{network}/subnets/{subnet}
  ```

**İzinler** - `MicrosoftAzureBatch` hizmet sorumlusu, belirtilen sanal ağ için `Classic Virtual Machine Contributor` Rol Tabanlı Erişim Denetimi (RBAC) rolüne sahip olmalıdır.

#### <a name="network-security-groups"></a>Ağ güvenlik grupları

İşlem düğümlerinde görev zamanlayabilmek için alt ağın Batch hizmetinden gelen iletişim isteklerine, Azure Depolama veya diğer kaynaklarla iletişim kurabilmek için de giden iletişim isteklerine izin vermesi gerekir.

Batch iletişimi yalnızca Batch IP adreslerinden havuz düğümlerine gelen iletişime izin verecek şekilde yapılandırdığından NSG belirtmenize gerek yoktur. Ancak belirtilen alt ağ ile ilişkilendirilmiş NSG'ler ve/veya güvenlik duvarı varsa gelen ve giden güvenlik kurallarını aşağıdaki tablolarda gösterilen şekilde yapılandırın. Belirtilen alt ağdaki işlem düğümleriyle iletişim kurulması bir NSG tarafından reddedilirse Batch hizmeti, işlem düğümlerinin durumunu **kullanılamıyor** olarak ayarlar.

Havuz düğümleri için RDP erişimine izin verecek şekilde gerekiyorsa gelen trafiği 3389 numaralı bağlantı noktasında Windows için yapılandırın. Bu ayar havuz düğümlerinin kullanılabilir durumda olması için şart değildir.

**Gelen güvenlik kuralları**

| Kaynak IP adresleri | Kaynak bağlantı noktaları | Hedef | Hedef bağlantı noktaları | Protocol | Eylem |
| --- | --- | --- | --- | --- | --- |
Tüm <br /><br />Bunun için "tümüne izin ver" izni gerekli olsa da Batch hizmeti her düğümün düzeyinde Batch harici IP adreslerini filtreleyen bir ACL kuralı uygular. | * | Tüm | 10100, 20100, 30100 | TCP | İzin Ver |
| İsteğe bağlı, işlem düğümleri RDP erişimine izin vermek. | * | Tüm | 3389 | TCP | İzin Ver |

**Giden güvenlik kuralları**

| source | Kaynak bağlantı noktaları | Hedef | Hedef bağlantı noktaları | Protocol | Eylem |
| --- | --- | --- | --- | --- | --- |
| Tüm | * | Tüm | 443  | Tüm | İzin Ver |
