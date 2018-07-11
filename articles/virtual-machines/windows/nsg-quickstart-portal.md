---
title: Azure portalını kullanarak bir VM'ye bağlantı noktalarını açmak | Microsoft Docs
description: Bağlantı noktası açma / Azure Portal'da resource manager dağıtım modelini kullanarak Windows sanal makinenize bir uç noktası oluşturma hakkında bilgi edinin
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: cynthn
ms.openlocfilehash: 2820dcabf042d7463f9776b42f277a0457caf3b6
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929024"
---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a>Azure portalı ile bir sanal makineye bağlantı noktaları açma
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Hızlı komutlar
Ayrıca [Azure PowerShell kullanarak aşağıdaki adımları gerçekleştirmek](nsg-quickstart-powershell.md).

İlk olarak, ağ güvenlik grubu oluşturun. Portalda bir kaynak grubu seçin, **Ekle**öğesini arayın ve seçin **ağ güvenlik grubu**:

![Ağ güvenlik grubu Ekle](./media/nsg-quickstart-portal/add-nsg.png)

Ağ güvenlik grubu için bir ad girin, seçin veya bir kaynak grubu oluşturun ve bir konum seçin. Seçin **Oluştur** tamamlandığında:

![Ağ güvenlik grubu oluşturma](./media/nsg-quickstart-portal/create-nsg.png)

Yeni ağ güvenlik grubunuzu seçin. 'Gelen güvenlik kuralları' seçip **Ekle** bir kural oluşturmak için:

![Bir gelen kuralı ekleyin](./media/nsg-quickstart-portal/add-inbound-rule.png)

Trafiğe izin veren bir kural oluşturmak için:

- Seçin **temel** düğmesi. Varsayılan olarak, **Gelişmiş** penceresi altında bazı ek yapılandırma seçeneklerini gibi belirli kaynak IP Blok veya bağlantı noktası aralığını tanımlamak üzere olarak sağlar.
- Ortak bir seçin **hizmet** aşağı açılan menüden gibi *HTTP*. Belirleyebilirsiniz *özel* kullanmak için belirli bir bağlantı sağlamak için. 
- İsterseniz öncelik veya adını değiştirin. Öncelik, kuralları uygulanır - sırası alt sayısal değer etkiler, kural önce uygulanır.
- Hazır olduğunuzda seçin **Tamam** kuralı oluşturmak için:

![Bir gelen kuralı oluşturma](./media/nsg-quickstart-portal/create-inbound-rule.png)

Son adımınız, ağ güvenlik grubunuzu bir alt ağ veya belirli bir ağ arabirimine ilişkilendirin sağlamaktır. Şimdi ağ güvenlik grubu bir alt ağı ile ilişkilendirin. Seçin **alt ağlar**, ardından **ilişkilendirmek**:

![Bir ağ güvenlik grubu bir alt ağı ile ilişkilendirin](./media/nsg-quickstart-portal/associate-subnet.png)

Sanal ağ seçin ve ardından uygun alt ağ seçin:

![Bir ağ güvenlik grubu, sanal ağ ile ilişkilendirme](./media/nsg-quickstart-portal/select-vnet-subnet.png)

80 numaralı bağlantı noktasında trafiğe izin verir ve bir alt ağla ilişkili bir gelen kuralı oluşturulan bir ağ güvenlik grubu oluşturdunuz. Bu alt ağa bağlanma herhangi bir VM bağlantı noktası 80 üzerinde erişilebilir.

## <a name="more-information-on-network-security-groups"></a>Ağ güvenlik grupları hakkında daha fazla bilgi
Hızlı komutlar Burada, sanal makinenizde akan trafikle ayarlayıp çalıştırmaya başlamasına olanak tanır. Ağ güvenlik grupları, çok sayıda harika özellik ve kaynaklarınıza erişimi denetlemek için ayrıntı düzeyi sağlar. Daha fazla bilgi edinebilirsiniz [oluşturma bir ağ güvenlik grubu ve ACL kurallarını burada](../../virtual-network/tutorial-filter-network-traffic.md).

Yüksek oranda kullanılabilir web uygulamaları için Vm'lerinizi bir Azure yük dengeleyici arkasında yerleştirmeniz gerekir. Yük Dengeleyici trafik filtreleme sağlar bir ağ güvenlik grubu Vm'lerle trafiği dağıtır. Daha fazla bilgi için [nasıl yükleneceğini yüksek oranda kullanılabilir bir uygulama oluşturmak için azure'da Linux sanal makineleri Bakiye](tutorial-load-balancer.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekte, HTTP trafiğine izin veren basit bir kuralı oluşturuldu. Aşağıdaki makalelerde daha ayrıntılı ortamları oluşturma hakkında bilgi bulabilirsiniz:

* [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md)
* [Bir ağ güvenlik grubu nedir?](../../virtual-network/security-overview.md)