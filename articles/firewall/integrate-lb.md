---
title: Azure güvenlik duvarı Azure standart Load Balancer ile tümleştirin
description: Azure güvenlik duvarı Azure Standard Load Balancer ile tümleştirmeyi öğrenin
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 4/1/2019
ms.author: victorh
ms.openlocfilehash: 7ee92a7508918635849caafab4632bbba81ee628
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60193790"
---
# <a name="integrate-azure-firewall-with-azure-standard-load-balancer"></a>Azure güvenlik duvarı Azure standart Load Balancer ile tümleştirin

Azure standart Load Balancer (public veya internal) ile bir sanal ağa, bir Azure güvenlik duvarı tümleştirebilirsiniz. 

Bu çok daha kolay bir tasarım olduğu için tercih edilen tasarım, Azure güvenlik duvarı ile iç yük dengeleyici tümleştirme sağlamaktır. Dağıtılan bir hesabınız ve yerinde tutmak istiyorsanız, bir genel yük dengeleyici kullanabilirsiniz. Ancak, herkese açık yük dengeleyici senaryosu işlevsellikle bozabilir bir asimetrik yönlendirme sorununu farkında olmanız gerekir.

Azure Load Balancer hakkında daha fazla bilgi için bkz: [Azure Load Balancer nedir?](../load-balancer/load-balancer-overview.md)

## <a name="public-load-balancer"></a>Herkese açık yük dengeleyici

Bir genel yük dengeleyici ile bir genel ön uç IP adresini yük dengeleyici dağıtılır.

### <a name="asymmetric-routing"></a>Asimetrik yönlendirme

Asimetrik yönlendirme, burada, bir paket için hedef bir yolunu alır ve kaynak döndürülürken başka bir yol alır girebiliriz. Güvenlik duvarının özel IP adresine giderek varsayılan yol bir alt ağ olan ve herkese açık yük dengeleyici kullanıyorsanız bu sorun oluşur. Bu durumda, gelen yük dengeleyici trafik kendi genel IP adresi alınır ancak bir dönüş yolu güvenlik duvarının özel IP adresi üzerinden gider. Güvenlik Duvarı, durum bilgisi olduğundan, Güvenlik Duvarı bu tür bir yerleşik oturumu farkında olmadığından System.Numerics.vectors.dll gelen paket bırakılır.

### <a name="fix-the-routing-issue"></a>Yönlendirme sorunu düzeltin

Bir alt ağa bir Azure güvenlik duvarı dağıttığınızda, tek bir adımda güvenlik duvarının AzureFirewallSubnet üzerinde bulunan özel IP adresi üzerinden alt yönlendirmeye paketleri için bir varsayılan rota oluşturmaktır. Daha fazla bilgi için [Öğreticisi: Dağıtma ve Azure Azure portalını kullanarak güvenlik duvarı yapılandırma](tutorial-firewall-deploy-portal.md#create-a-default-route).

Güvenlik Duvarı, yük dengeleyici senaryosu yapılırsa, Internet trafiğinizi, güvenlik duvarınızın genel IP adresi gelen istersiniz. Burada, güvenlik duvarı, yük dengeleyicinin genel IP adresi NAT paketleri ve güvenlik duvarı kuralları geçerlidir. Sorunun gerçekleştiği budur. Paket gelen Güvenlik Duvarı'nın genel IP adresi, ancak Güvenlik Duvarı (varsayılan yolu kullanarak) özel IP adresi döndürür.
Bu sorunu önlemek için bir Güvenlik Duvarı'nın genel IP adresi için ek konak yolu oluşturun. Güvenlik Duvarı'nın genel IP adresine giden paketlerin Internet üzerinden yönlendirilir. Bu, güvenlik duvarının özel IP adresi için varsayılan yolu alma önler.

![Asimetrik yönlendirme](media/integrate-lb/Firewall-LB-asymmetric.png)

Örneğin, aşağıdaki bir Güvenlik Duvarı'nın genel IP adresi 13.86.122.41 ve özel IP adresi 10.3.1.4 yollardır.

![Rota tablosu](media/integrate-lb/route-table.png)

## <a name="internal-load-balancer"></a>İç yük dengeleyici

Bir iç yük dengeleyiciyle bir özel ön uç IP adresini yük dengeleyici dağıtılır.

Bu senaryo ile asimetrik yönlendirme sorun yoktur. Gelen paketleri, güvenlik duvarının genel IP adresinde geldiğinde, load balancer'ın özel IP adresine çevrilir ve aynı dönüş yolu kullanarak Güvenlik Duvarı'nın özel IP adresine döndürür.

Bu nedenle, bu senaryo için herkese açık yük dengeleyici senaryosu benzer dağıtabilirsiniz, ancak güvenlik duvarı gerek kalmadan genel IP adresinin ana bilgisayar yolu.

## <a name="additional-security"></a>Ek güvenlik

Daha fazla yük dengeli senaryonuz güvenliğini artırmak için ağ güvenlik grupları (Nsg'ler) kullanabilirsiniz.

Örneğin, yük dengeli sanal makineler yer aldığı arka uç alt ağa bir NSG oluşturabilirsiniz. Gelen güvenlik duvarı IP adresini/bağlantı noktasından gelen trafiğe izin veren.

Nsg'ler hakkında daha fazla bilgi için bkz. [güvenlik grupları](../virtual-network/security-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [dağıtma ve bir Azure güvenlik duvarı yapılandırma](tutorial-firewall-deploy-portal.md).