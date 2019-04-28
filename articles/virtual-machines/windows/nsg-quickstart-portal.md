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
ms.date: 09/27/2018
ms.author: cynthn
ms.openlocfilehash: cac17403425f53593d4f48692b4216a92c8624e3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61481777"
---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a>Azure portalı ile bir sanal makineye bağlantı noktaları açma
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]


## <a name="sign-in-to-azure"></a>Azure'da oturum açma
https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

1. İçin arama yapın ve sanal makine için kaynak grubunu seçin, **Ekle**öğesini arayın ve seçin **ağ güvenlik grubu**.

2. **Oluştur**’u seçin.

    **Ağ güvenlik grubu oluştur** penceresi açılır.

    ![Ağ güvenlik grubu oluşturma](./media/nsg-quickstart-portal/create-nsg.png)

2. Ağ güvenlik grubu için bir ad girin. 

3. Seçin veya bir kaynak grubu oluşturun ve sonra bir konum seçin.

4. Seçin **Oluştur** ağ güvenlik grubu oluşturun.

## <a name="create-an-inbound-security-rule"></a>Bir gelen güvenlik kuralı oluşturma

1. Yeni ağ güvenlik grubunuzu seçin. 

2. Seçin **gelen güvenlik kuralları**, ardından **Ekle**.

    ![Gelen Kuralı Ekle](./media/nsg-quickstart-portal/add-inbound-rule.png)

3. **Gelişmiş**'i seçin. 

4. Ortak bir seçin **hizmet** aşağı açılan menüden gibi **HTTP**. Belirleyebilirsiniz **özel** kullanmak için belirli bir bağlantı noktası sağlamak istiyorsanız. 

5. İsterseniz değiştirme **öncelik** veya **adı**. Öncelik kuralları uygulanma sırası etkiler: sayısal kural uygulandığında değer azaldıkça, daha önce.

6. Seçin **Ekle** kuralı oluşturmak için.

## <a name="associate-your-network-security-group-with-a-subnet"></a>Ağ güvenlik grubunuzu bir alt ağı ile ilişkilendirin

Gerçekleştirmeniz gereken son adım, ağ güvenlik grubunuzu bir alt ağ ile veya belirli bir ağ arabirimiyle ilişkilendirmektir. Bu örnekte, biz ağ güvenlik grubu bir alt ağ ile ilişkilendireceksiniz. 

1. Seçin **alt ağlar**, ardından **ilişkilendirmek**.

    ![Bir ağ güvenlik grubu bir alt ağı ile ilişkilendirin](./media/nsg-quickstart-portal/associate-subnet.png)

2. Sanal ağınızı ve ardından uygun alt ağı seçin.

    ![Bir ağ güvenlik grubu, sanal ağ ile ilişkilendirme](./media/nsg-quickstart-portal/select-vnet-subnet.png)

    Bu alt ağa bağlanma herhangi bir VM, artık bağlantı noktası 80 üzerinde erişilebilir.

## <a name="additional-information"></a>Ek bilgiler

Ayrıca [Azure PowerShell kullanarak bu makaledeki adımları gerçekleştirmek](nsg-quickstart-powershell.md).

Bu makalede açıklanan komutları VM'nize akan trafiği hızlı şekilde ulaşmasını sağlar. Ağ güvenlik grupları, çok sayıda harika özellik ve kaynaklarınıza erişimi denetlemek için ayrıntı düzeyi sağlar. Daha fazla bilgi için [bir ağ güvenlik grubu ile ağ trafiğini filtreleme](../../virtual-network/tutorial-filter-network-traffic.md).

Yüksek oranda kullanılabilir web uygulamaları için Vm'lerinizi bir Azure yük dengeleyici arkasında yerleştirmeyi göz önünde bulundurun. Yük Dengeleyici, trafiği filtreleme sağlayan bir ağ güvenlik grubu ile VM'ler, trafiği dağıtır. Daha fazla bilgi için [Yük Dengelemesi yüksek oranda kullanılabilir bir uygulama oluşturmak için azure'da Windows sanal makineleri](tutorial-load-balancer.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bağlantı noktası 80 üzerinde HTTP trafiğine izin verir ve ardından bu kural için bir alt ağla ilişkili bir gelen kuralı oluşturulan bir ağ güvenlik grubu oluşturulur. 

Aşağıdaki makalelerde daha ayrıntılı ortamları oluşturma hakkında bilgi bulabilirsiniz:
- [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md)
- [Güvenlik grupları](../../virtual-network/security-overview.md)