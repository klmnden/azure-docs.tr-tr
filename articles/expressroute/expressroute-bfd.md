---
title: ExpressRoute üzerinden BFD yapılandırma | Microsoft Docs
description: Bu belge, özel eşleme üzerinden ExpressRoute bağlantı hattının BFD yapılandırma hakkında yönergeler sağlar.
documentationcenter: na
services: expressroute
author: rambk
manager: tracsman
editor: ''
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 8/17/2018
ms.author: rambala
ms.openlocfilehash: 6d941bf810a45e8808f83c4df701a856f664c7ef
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51631668"
---
# <a name="configure-bfd-over-expressroute"></a>ExpressRoute üzerinden BFD yapılandırın

ExpressRoute özel eşlemesi üzerinde çift yönlü iletme algılama (BFD) destekler. ExpressRoute üzerinden BFD etkinleştirerek, Microsoft Enterprise edge (MSEE) cihazlar ve ExpressRoute bağlantı hattı (PE) sonlandırmak yönlendiriciler arasında bağlantı hatası algılama hızlandırabilirsiniz. (Yönetilen Katman 3 bağlantısı hizmetiyle gittiyse) müşteri Edge'i yönlendirme cihazları veya iş ortağı uç yönlendirme cihazları üzerinden ExpressRoute sonlandırabilirsiniz. Bu belge ihtiyacını BFD ve ExpressRoute üzerinden BFD etkinleştirme konusunda size kılavuzluk eder.

## <a name="need-for-bfd"></a>BFD gereksinimini

ExpressRoute bağlantı hattı üzerinden BFD etkinleştirme avantajı Aşağıdaki diyagramda gösterilmiştir: [ ![1]][1]

Yönetilen Katman 3 bağlantılarını veya ExpressRoute bağlantı hattı Katman 2 bağlantıları ya da etkinleştirebilirsiniz. ExpressRoute bağlantı yolunda bir veya daha fazla katman 2 cihazlar varsa her iki durumda da, üstteki BGP ile bağlantı hataları algılama yolu sorumluluğunu arasındadır.

MSEE cihazlarda BGP keepalive ve durma süresini genellikle 60-180 saniye sırasıyla yapılandırılır. Bu nedenle, en fazla götürecek bir bağlantı hatası algılamak için üç dakika bağlantı hatası ve alternatif bağlantı trafiğini geçin.

Müşteri sınır eşleme cihazı daha düşük BGP keepalive ve durma süresini yapılandırarak BGP zamanlayıcılar kontrol edebilirsiniz. İki eşleme cihazlar arasında BGP zamanlayıcılar eşleşirse, eşler arasında BGP oturumu alt zamanlayıcı değeri kullanırsınız. BGP keepalive üç saniye ve onlarca saniye sırasına göre Durma süresini olabildiğince düşük olarak ayarlanabilir. Ancak, protokol işlem kullanımı yoğun olduğundan BGP zamanlayıcılar agresif daha az tercih ayarlanıyor.

Bu senaryoda BFD yardımcı olabilir. BFD subsecond zaman aralığındaki düşük ek yük bağlantı hatası algılama sağlar. 


## <a name="enabling-bfd"></a>BFD etkinleştirme

Varsayılan olarak tüm yeni oluşturulan ExpressRoute özel eşlemesi arabirimlerde Msee'ler altında BFD yapılandırılır. Bu nedenle, BFD etkinleştirmek için PEs üzerinde BFD yapılandırılması gerekir. BFD olan iki adımlı işlem: arabiriminde BFD yapılandırın ve ardından BGP oturumu için bağlantı.

Bir örnek (Cisco IOS XE kullanarak) PE yapılandırması aşağıda gösterilmiştir. 

    interface TenGigabitEthernet2/0/0.150
      description private peering to Azure
      encapsulation dot1Q 15 second-dot1q 150
      ip vrf forwarding 15
      ip address 192.168.15.17 255.255.255.252
      bfd interval 300 min_rx 300 multiplier 3


    router bgp 65020
      address-family ipv4 vrf 15
        network 10.1.15.0 mask 255.255.255.128
        neighbor 192.168.15.18 remote-as 12076
        neighbor 192.168.15.18 fall-over bfd
        neighbor 192.168.15.18 activate
        neighbor 192.168.15.18 soft-reconfiguration inbound
      exit-address-family

>[!NOTE]
>Bir mevcut özel eşdüzey hizmet sağlama altında BFD etkinleştirmek için; eşleme sıfırlamak gerekir. Bkz: [sıfırlama ExpressRoute eşlemeleri][ResetPeering]
>

## <a name="bfd-timer-negotiation"></a>BFD Zamanlayıcı anlaşması

BFD eşleri arasında iki eş daha yavaş aktarım hızını belirler. Msee BFD aktarım/alma aralıkları 300 milisaniye olarak ayarlanır. Belirli senaryolarda aralığı 750 milisaniye olarak daha yüksek bir değer ayarlanabilir. Yüksek değerler yapılandırarak, daha uzun olacak şekilde Bu aralıklar zorunlu kılabilirsiniz; Ancak, daha kısa değil.

>[!NOTE]
>Coğrafi olarak yedekli ExpressRoute özel eşlemesi devreler yapılandırmış olmanız veya siteden siteye IPSec VPN bağlantısını ExpressRoute özel eşlemesi için; yedekleme Özel eşleme üzerinden BFD etkinleştirme, bir ExpressRoute bağlantı hatası aşağıdaki daha hızlı yük devretme yardımcı olacaktır. 
>

## <a name="next-steps"></a>Sonraki Adımlar

Daha fazla bilgi veya Yardım için aşağıdaki bağlantıları kontrol edin:

- [ExpressRoute devre oluşturma ve değiştirme][CreateCircuit]
- [ExpressRoute devresi için yönlendirme oluşturma ve değiştirme][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-bfd/BFD_Need.png "BFD hızlandıran bağlantı hatası kesinti süresi"

<!--Link References-->
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[ResetPeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-reset-peering






