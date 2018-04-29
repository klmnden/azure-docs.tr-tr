---
title: Bir Azure ağı erişim denetimi listesi nedir?
description: Azure'daki erişim denetim listeleri hakkında bilgi edinin
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
ms.openlocfilehash: 0f22bae1e1e40d3ea735861b9a633ae46eae5c33
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="what-is-an-endpoint-access-control-list"></a>Bir uç noktası erişim denetimi listesi nedir?

> [!IMPORTANT]
> Azure sahip iki farklı [dağıtım modellerini](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oluşturmak ve kaynaklarla çalışmak için: Resource Manager ve klasik. Bu makale klasik dağıtım modelini incelemektedir. Microsoft, en yeni dağıtımların Resource Manager dağıtım modelini kullanmasını önerir. 

Bir uç noktası erişim denetimi listesi (ACL) Azure dağıtımınız için kullanılabilir bir güvenlik yeniliktir. Bir ACL seçmeli olarak izin veren veya trafiği bir sanal makine uç noktası için reddetme olanağı sağlar. Bu paket filtreleme özelliği, ek bir güvenlik katmanı sağlar. Yalnızca uç noktaları için ağ ACL'leri belirtebilirsiniz. Bir sanal ağ veya bir sanal ağda bulunan belirli bir alt ağ için bir ACL belirtemezsiniz. ACL'ler yerine, mümkün olduğunda ağ güvenlik grupları (Nsg'ler) kullanmak için önerilir. Nsg'ler hakkında daha fazla bilgi için bkz: [ağ güvenlik grubu genel bakış](virtual-networks-nsg.md)

ACL, PowerShell veya Azure portalı kullanılarak yapılandırılabilir. PowerShell kullanarak bir ağ ACL yapılandırmak için bkz: [yönetme erişim denetim listeleri için PowerShell kullanarak uç noktaları](virtual-networks-acl-powershell.md). Azure portalı kullanarak bir ağ ACL yapılandırmak için bkz: [uç noktaları bir sanal makine için ayarlamak üzere nasıl](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Ağ ACL'leri kullanarak aşağıdakileri yapabilirsiniz:

* Seçmeli olarak izin vermek veya uzak alt ağ bir sanal makine giriş uç noktası için IPv4 adres aralığı göre gelen trafiği engelle.
* Kara liste IP adresleri
* Sanal makine uç noktası başına birden çok kural oluşturma
* Kullanım Kuralları doğru kümesini emin olmak için kural sıralama verilen sanal makine uç noktası (en düşük, büyüğe) üzerinde uygulanır
* Belirli bir uzak alt IPv4 adresi için bir ACL belirtin.

Bkz: [Azure sınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) ACL sınırları için makale.

## <a name="how-acls-work"></a>ACL'ler nasıl çalışır
Bir ACL kuralları listesini içeren bir nesnedir. Bir ACL oluşturmak ve sanal makine uç noktası için geçerli olduğunda, paket filtreleme VM ana bilgisayar düğümü üzerinde gerçekleşir. Bu, uzak IP adreslerinden gelen trafik, VM'de eşleşen ACL kuralları yerine ana bilgisayar düğümü tarafından filtre anlamına gelir. Bu, VM paket filtreleme değerli CPU döngülerini harcama önler.

Bir sanal makine oluşturulduğunda, varsayılan bir ACL tüm gelen trafiği engellemek için yürürlükte yerleştirilir. Ancak, bir uç nokta (3389 bağlantı noktası için), ardından ACL değiştiren Bu uç nokta için tüm gelen trafiğe izin vermek için varsayılan oluşturulur. Uzak bir alt ağdan gelen trafiği sonra Bu uç noktasına izin verilir ve hiçbir güvenlik duvarı sağlama gereklidir. Uç noktalar için bu bağlantı noktalarını oluşturulan sürece diğer tüm bağlantı noktalarına gelen trafik için engellenir. Giden trafik, varsayılan olarak izin verilir.

**Örnek varsayılan ACL tablo**

| **Kuralı #** | **Uzak alt ağ** | **uç noktası** | **İzin ver ve Reddet** |
| --- | --- | --- | --- |
| 100 |0.0.0.0/0 |3389 |İzin ver |

## <a name="permit-and-deny"></a>İzin ve reddetme
Seçmeli olarak izin vermek veya "izin ver" belirten kuralları veya "Reddet" oluşturarak sanal makine giriş uç noktası için ağ trafiğini engellemek. Bir uç noktası oluşturulduğunda, varsayılan olarak, uç nokta için tüm trafiğe izin verildiğini dikkate almak önemlidir. Bu nedenle, izin verme/reddetme kurallarını oluşturma ve sanal makine uç noktası erişmesine izin vermek için seçtiğiniz ağ trafiği üzerinde ayrıntılı denetim isterseniz, bunları uygun öncelik sırasına göre yerleştirmek anlamak önemlidir.

Dikkate alınacak noktalar:

1. **Hiçbir ACL –** bir uç noktası oluşturulduğunda, varsayılan olarak şu uç nokta için tüm izin verir.
2. **İzin ver -** bir veya daha fazla "izin ver" aralıkları eklediğinizde, varsayılan olarak tüm aralıklarını engelleme. Yalnızca izin verilen IP aralığı paketler sanal makine uç noktası ile iletişim kuramaz.
3. **Reddetme -** bir veya daha fazla "Reddet" aralıkları eklediğinizde, varsayılan olarak tüm trafiği aralıklarına vermiş olursunuz.
4. **İzin verme ve reddetme - birleşimi** "izin ver" ve "izin verilen veya reddedilen belirli bir IP aralığı ayırması istediğinizde reddetme" bileşimini kullanabilirsiniz.

## <a name="rules-and-rule-precedence"></a>Kurallar ve kural önceliği
Ağ ACL'leri belirli bir sanal makine uç noktalarda ayarlanabilir. Örneğin, bir sanal makineye hangi kilitleri erişim tuşunu belirli IP adresleri oluşturulan bir RDP uç noktası için ACL ağ belirtebilirsiniz. Aşağıdaki tablo için RDP erişime izin vermek için ortak sanal IP (VIP), belirli bir aralıkla erişim vermek için bir yol gösterir. Diğer uzak IP'leri engellenir. Biz izleyin bir *en düşük öncelik kazanır* kural sırası.

### <a name="multiple-rules"></a>Birden çok kural
Yalnızca iki ortak IPv4 adres aralıklarını (65.0.0.0/8 ve 159.0.0.0/8), RDP uç noktasına erişimine izin vermek istiyorsanız, aşağıdaki örnekte, bu iki belirterek elde edebilirsiniz *izin* kuralları. Bu durumda, varsayılan olarak bir sanal makine için RDP oluşturulduğundan, uzak bir alt ağa datalı RDP bağlantı noktasına erişim kilitleme isteyebilirsiniz. Aşağıdaki örnek, RDP için erişime izin vermek için ortak sanal IP (VIP), belirli bir aralıkla erişim vermek için bir yol gösterir. Diğer uzak IP'leri engellenir. Bu, belirli bir sanal makine uç noktası için ağ ACL'leri ayarlanabilir ve varsayılan olarak erişimi reddedilir çünkü çalışır.

**Örnek – birden çok kural**

| **Kuralı #** | **Uzak alt ağ** | **uç noktası** | **İzin ver ve Reddet** |
| --- | --- | --- | --- |
| 100 |65.0.0.0/8 |3389 |İzin ver |
| 200 |159.0.0.0/8 |3389 |İzin ver |

### <a name="rule-order"></a>Kural sırası
Birden çok kural için bir uç nokta belirtilebilir çünkü hangi kuralın önceliklidir belirlemek için kuralları düzenlemek için bir yol olmalıdır. Kural sırası önceliği belirtir. ACL'ler izleyin ağ bir *en düşük öncelik kazanır* kural sırası. Aşağıdaki örnekte, uç nokta bağlantı noktası 80 üzerinde seçmeli olarak yalnızca belirli IP adresi aralıklarını erişimi verilir. Bir reddetme kuralı sahibiz bunu yapılandırmak için (kural \# 100) 175.1.0.1/24 alan adres. İkinci bir kural, ardından diğer tüm adresler 175.0.0.0/8 altında erişimine 200 önceliğe sahip belirtilir.

**Örnek – kural önceliği**

| **Kuralı #** | **Uzak alt ağ** | **uç noktası** | **İzin ver ve Reddet** |
| --- | --- | --- | --- |
| 100 |175.1.0.1/24 |80 |Reddet |
| 200 |175.0.0.0/8 |80 |İzin ver |

## <a name="network-acls-and-load-balanced-sets"></a>Ağ ACL'leri ve yük dengeli ayarlar
Ağ ACL'leri bir yük dengeli kümesi uç noktasında belirtilebilir. Yük dengelenmiş bir küme için bir ACL belirtilirse, bu yük dengelenmiş küme tüm sanal makinelerin ağ ACL uygulanır. Örneğin, bir yük dengeli kümesi "Bağlantı noktası 80" oluşturulur ve yük dengeli küme 3 VM varsa, "VM otomatik olarak diğer VM'ler için uygulanacak bir bağlantı noktası 80" noktadaki ACL ağ oluşturuldu.

![Ağ ACL'leri ve yük dengeli ayarlar](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Sonraki Adımlar
[PowerShell kullanarak uç noktalar için erişim denetim listeleri yönetme](virtual-networks-acl-powershell.md)

