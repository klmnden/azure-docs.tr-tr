---
title: Bir Azure ağına erişim denetim listesi nedir?
description: Azure'da erişim denetim listeleri hakkında bilgi edinin
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: 83d66c84-8f6b-4388-8767-cd2de3e72d76
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: genli
ms.openlocfilehash: 3a7155380a51273d376226c6be7a004f386181ce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61035266"
---
# <a name="what-is-an-endpoint-access-control-list"></a>Uç nokta erişim denetim listesi nedir?

> [!IMPORTANT]
> Azure iki farklı olan [dağıtım modelleri](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oluşturmak ve bu kaynaklarla çalışmak için: Resource Manager ve klasik. Bu makale klasik dağıtım modelini incelemektedir. Microsoft, en yeni dağıtımların Resource Manager dağıtım modelini kullanmasını önerir. 

Bir uç nokta erişim denetim listesi (ACL) Azure dağıtımınız için bir güvenlik geliştirmesi ' dir. Bir ACL seçmeli olarak erişime izin verebilir veya trafiği bir sanal makine uç noktası için olanağı sağlar. Bu paket filtreleme özelliği, ek bir güvenlik katmanı sağlar. Yalnızca uç noktaları için ağ ACL'leri belirtebilirsiniz. ACL'ye yönelik bir sanal ağ veya sanal ağ içinde yer alan belirli bir alt ağ belirtemezsiniz. ACL'ler yerine, mümkün olduğunda ağ güvenlik grupları (Nsg'ler) kullanmak için önerilir. Nsg'leri kullanarak, uç nokta erişim denetim listesi değiştirildi ve artık zorunlu. Nsg'ler hakkında daha fazla bilgi edinmek için [ağ güvenlik grubu genel bakış](security-overview.md)

ACL, PowerShell veya Azure portalını kullanarak yapılandırılabilir. PowerShell kullanarak bir ağ ACL yapılandırmak için bkz [yönetme erişim denetim listeleri için PowerShell kullanarak uç](virtual-networks-acl-powershell.md). Azure portalını kullanarak bir ağ ACL yapılandırmak için bkz [bir sanal makineye uç noktaları ayarlama işlemini](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Ağ ACL'leri kullanarak bunu yapabilirsiniz:

* Seçmeli olarak erişime izin verebilir veya uzak alt ağ IPv4 adres aralığı için bir sanal makine giriş uç noktasına göre gelen trafiği.
* Kara liste IP adresleri
* Sanal makine uç noktası başına birden çok kural oluşturma
* Kullanım Kuralları doğru kümesini emin olmak için kural sıralama belirli sanal makine uç noktası (en düşük, en yüksek için) üzerinde uygulanır
* ACL'ye yönelik belirli bir uzak alt IPv4 adresi belirtin.

Bkz: [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) makale ACL sınırları.

## <a name="how-acls-work"></a>ACL'leri nasıl çalışır?
Bir ACL kurallarının bir listesini içeren bir nesnedir. Bir ACL oluşturmak ve bir sanal makine uç noktası için geçerli olduğunda, paket filtreleme, sanal Makinenizin ana bilgisayar düğümü üzerinde gerçekleşir. Bu, uzak IP adreslerinden gelen trafik için vm'nizdeki eşleşen ACL kuralları yerine ana bilgisayar düğümü tarafından filtrelenir anlamına gelir. Bu, sanal makinenizin üzerinde paket filtreleme değerli CPU döngülerini harcamalarını engeller.

Bir sanal makine oluşturulduğunda varsayılan ACL'nin gelen tüm trafiği engellemek için yerinde konur. Ancak, bir uç noktası (3389 bağlantı noktası için), ardından ACL değiştirilmişse bu uç nokta için tüm gelen trafiğe izin veren varsayılan oluşturulur. Tüm uzak alt ağından gelen trafiğe daha sonra Bu uç noktasına izin verilir ve hiçbir güvenlik duvarı sağlama gereklidir. Uç noktaları için bu bağlantı noktalarını oluşturulan sürece diğer tüm bağlantı noktaları için gelen trafik engellenir. Giden trafiğe varsayılan olarak izin verilir.

**Örnek varsayılan ACL'si tablo**

| **Kuralı #** | **Uzak alt ağ** | **Uç noktası** | **İzin verme ve reddetme** |
| --- | --- | --- | --- |
| 100 |0.0.0.0/0 |3389 |İzin ver |

## <a name="permit-and-deny"></a>İzin ve reddetme
Seçmeli olarak izin vermek veya trafiği bir sanal makine giriş uç noktası için "izin ver" belirten kuralları veya "Reddet" oluşturarak reddet. Bir uç noktası oluşturulduğunda, varsayılan olarak, uç nokta için tüm trafiğe izin verildiğini unutmayın. Bu nedenle, izin verme/reddetme kurallarını oluşturma ve sanal makine uç noktası erişmesine izin vermeyi seçtiğiniz ağ trafik üzerinde ayrıntılı denetim istiyorsanız, bunları uygun öncelik sırasına göre Yerleştir anlamak önemlidir.

Dikkate alınacak noktalar:

1. **Hiçbir ACL –** bir uç noktası oluşturulduğunda, varsayılan olarak biz tüm uç nokta için izin verir.
2. **İzin ver -** bir veya daha fazla "izin ver" aralıkları eklediğinizde, varsayılan olarak tüm aralıkları reddediyorsunuz. Yalnızca izin verilen IP aralığından paketlerin sanal makine uç noktası ile iletişim kurmak mümkün olacaktır.
3. **Reddetme -** bir veya daha fazla "Reddet" aralıkları eklediğinizde, varsayılan olarak tüm trafiği aralıklarına vermiş olursunuz.
4. **İzin verme ve reddetme - birleşimi** "izin ver" ve "izin verilen veya reddedilen için belirli bir IP aralığı ayırma işlemini koordine istediğinizde reddetme" bir birleşimini kullanabilirsiniz.

## <a name="rules-and-rule-precedence"></a>Kurallar ve kural önceliği
Ağ ACL'leri belirli sanal makine uç noktalarına üzerinde ayarlanabilir. Örneğin, bir ağ erişim tuşunu hangi kilitleri belirli IP adresleri bir sanal makine üzerinde oluşturulan bir RDP uç noktası için ACL belirtebilirsiniz. Aşağıdaki tabloda, RDP erişimine izin verecek şekilde genel sanal IP (VIP), belirli bir aralıkla erişim vermek için bir yol gösterir. Diğer uzak tüm IP'lere engellenir. Sizinle bir *en düşük öncelik kazanır* kural sırası.

### <a name="multiple-rules"></a>Birden çok kural
Yalnızca iki ortak IPv4 adres aralıklarını (65.0.0.0/8 ve 159.0.0.0/8), RDP uç noktaya erişime izin vermek istiyorsanız, aşağıdaki örnekte, iki belirterek bunu gerçekleştirebilirsiniz *izin* kuralları. Bu durumda varsayılan sanal makine için RDP oluşturulduğundan, uzak bir alt ağa dayalı RDP bağlantı noktası erişimi kilitleme isteyebilirsiniz. Aşağıdaki örnek, RDP erişimine izin verecek şekilde genel sanal IP (VIP), belirli bir aralıkla erişim vermek için bir yol gösterir. Diğer uzak tüm IP'lere engellenir. Bu belirli bir sanal makine uç noktası için ağ ACL'lerini ayarlanabilir ve varsayılan olarak erişim reddedildi çünkü çalışır.

**Örneğin, birden çok kural**

| **Kuralı #** | **Uzak alt ağ** | **Uç noktası** | **İzin verme ve reddetme** |
| --- | --- | --- | --- |
| 100 |65.0.0.0/8 |3389 |İzin ver |
| 200 |159.0.0.0/8 |3389 |İzin ver |

### <a name="rule-order"></a>Kural sırası
Birden çok kural için bir uç nokta belirtilebilir, çünkü hangi kural öncelikli olur belirlemek için kuralları düzenlemek için bir yol olmalıdır. Kural sırası önceliği belirtir. Ağ ACL'leri izleme bir *en düşük öncelik kazanır* kural sırası. Aşağıdaki örnekte, uç nokta bağlantı noktası 80 üzerinde seçmeli olarak yalnızca belirli IP adresi aralıklarını erişimi verilir. Bir reddetme kuralı sahibiz bunu yapılandırmak için (kural \# 100) 175.1.0.1/24 alan adresleri. İkinci bir kural, ardından 175.0.0.0/8 altında diğer tüm adreslere erişimine 200 önceliğe sahip belirtilir.

**Örneğin, kural önceliği**

| **Kuralı #** | **Uzak alt ağ** | **Uç noktası** | **İzin verme ve reddetme** |
| --- | --- | --- | --- |
| 100 |175.1.0.1/24 |80 |Reddet |
| 200 |175.0.0.0/8 |80 |İzin ver |

## <a name="network-acls-and-load-balanced-sets"></a>Ağ ACL'leri ve yük dengeli kümeler
Bir yük dengeli küme uç noktası üzerinde ağ ACL'lerine belirtilebilir. Bir ACL için yük dengeli bir küme belirtilirse, ACL ağ, yük dengeli kümedeki tüm sanal makinelere uygulanır. Örneğin, bir yük dengeli kümesi "80 numaralı bağlantı noktasıyla" oluşturulur ve yük dengeli küme 3 VM varsa, "VM otomatik olarak diğer Vm'lere uygulanır birinin 80 numaralı bağlantı noktası" uç noktasında ACL ağ oluşturulur.

![Ağ ACL'leri ve yük dengeli kümeler](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Sonraki Adımlar
[PowerShell kullanarak uç noktalar için erişim denetim listelerini yönetebilir](virtual-networks-acl-powershell.md)

