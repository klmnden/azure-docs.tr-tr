<properties
   pageTitle="ExpressRoute için QoS gereksinimleri | Microsoft Azure"
   description="Bu sayfada, ExpressRoute bağlantı hatları için hizmet kalitesini (QoS) yapılandırma ve yönetmeye yönelik ayrıntılı gereksinimler sağlanmıştır."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/18/2016"
   ms.author="cherylmc"/>

# ExpressRoute QoS gereksinimleri

Skype Kurumsal’da farklı QoS davranışları gerektiren çeşitli iş yükleri vardır. ExpressRoute aracılığıyla ses hizmetleri kullanmayı planlıyorsanız, aşağıda açıklanan gereksinimlere uymanız gerekir.

![](./media/expressroute-qos/expressroute-qos.png)

>[AZURE.NOTE] QoS gereksinimleri yalnızca Microsoft eşlemeleri için geçerlidir.

Aşağıdaki tabloda Skype Kurumsal tarafından kullanılan DSCP işaretlerinin bir listesi verilmiştir. Daha fazla bilgi için [Skype Kurumsal için QoS’i yönetme](https://technet.microsoft.com/library/gg405409.aspx) bölümüne başvurun.

| **Trafik Sınıfı** | **İşleme (DSCP İşaretlemesi)** | **Skype Kurumsal İş Yükleri** |
|---|---|---|
| **Ses** | EF (46) | Skype / Lync ses |
| **Etkileşimli** | AF41 (34) | Video |
|   | AF21 (18) | Uygulama paylaşımı | 
|   | CS3 (24) | SIP sinyali |
| **Varsayılan** | AF11 (10) | Dosya aktarımı|
|   | CS0 (0) | Diğer| 


- İş yükleri sınıflandırmanız ve doğru DSCP değerlerini işaretlemeniz gerekir. Ağınızda DSCP işaretlerini ayarlamak için [burada](https://technet.microsoft.com/library/gg405409.aspx) sağlanan yönergeleri izleyin.

- Ağınızda çoklu QoS kuyruklarını yapılandırmanız ve desteklemeniz gerekir. Ses, tek başına bir sınıf olmalı ve ses için RFC 3246 içinde belirtilen EF işlemi uygulanmalıdır. 

- Kuyruğa alma mekanizması, sıkışma algılama ilkesi ve trafik sınıfı başına bant genişliği ayırmayı siz belirleyebilirsiniz. Ancak, Skype Kurumsal iş yükleri için DSCP işaretlemesi korunmalıdır. AF31 (26) gibi yukarıda listelenmeyen DSCP işaretlerini kullanıyorsanız, paketi Microsoft'a göndermeden önce bu DSCP değerinin 0 olarak yeniden yazılması gerekir. Microsoft yalnızca yukarıdaki tabloda gösterilen DSCP değeriyle işaretlenen paketleri gönderir. 

## Sonraki adımlar

- [Yönlendirme](expressroute-routing.md) ve [NAT](expressroute-nat.md) için gereksinimlere bakın.
- ExpressRoute bağlantınızı yapılandırmak için aşağıdaki bağlantılara bakın.

    - [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)
    - [Yönlendirmeyi yapılandırma](expressroute-howto-routing-classic.md)
    - [ExpressRoute bağlantı hattına bir VNet bağlama](expressroute-howto-linkvnet-classic.md)



<!---HONumber=Jun16_HO2-->


