---
title: 'Birlikte çalışabilirlik Azure arka uç bağlantısı özellikleri: Veri düzlemi analizi | Microsoft Docs'
description: Bu makalede, ExpressRoute, siteden siteye VPN ve sanal ağ eşlemesi ile Azure arasında birlikte çalışabilirlik analiz etmek için kullanabileceğiniz test kurulum veri düzlemi analizini sağlar.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: f4d94536a8c1b509e0ce435a764e69984b5d415e
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499310"
---
# <a name="interoperability-in-azure-back-end-connectivity-features-data-plane-analysis"></a>Birlikte çalışabilirlik Azure arka uç bağlantısı özellikleri: Veri düzlemi analizi

Bu makalede veri düzlemi analizini [kurulumunu test][Setup]. Ayrıca inceleyebilirsiniz [test Kurulum Yapılandırması] [ Configuration] ve [denetim düzlemi analiz] [ Control-Analysis] testi kurulumun.

Veri düzlemi analizi başka bir topoloji için bir yerel ağ üzerinden (LAN ya da sanal ağ) geçiş yapan paket tarafından gerçekleştirilecek yolunu inceler. İki yerel ağ arasındaki veri yolu simetrik olmak zorunda değildir. Bu nedenle, bu makalede, yerel ağ iletme yolundan ters yolundan ayrı olarak başka bir ağa analiz ediyoruz.

## <a name="data-path-from-the-hub-vnet"></a>Sanal hub'ından veri yolu

### <a name="path-to-the-spoke-vnet"></a>Uç sanal ağ yolu

Sanal ağ (VNet) eşlemesi, eşlenen iki sanal ağ arasında ağ köprüsü işlevselliğine öykünür. Traceroute, VNet burada gösterilen uçtaki bir VM için bir merkez sanal ağa çıkış:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.11.30.4

    Trace complete.

Merkez sanal ağa bağlantı grafik görünümünü ve Azure Ağ İzleyicisi perspektifinden uç sanal ağı aşağıdaki şekilde gösterilmiştir:


[![1]][1]

### <a name="path-to-the-branch-vnet"></a>VNet dal yolu

Traceroute, VNet burada gösterilen daldaki bir VM için bir merkez sanal ağa çıkış:

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms     1 ms     1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     2 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

Bu traceroute ilk atlamanın Azure VPN ağ geçidi merkez sanal ağa VPN ağ geçidi ' dir. İkinci atlama dalın VNet VPN ağ geçidi ' dir. Dalın VNet VPN ağ geçidinin IP adresi, merkez sanal ağa tanıtılan değil. Üçüncü atlama dal sanal ağ üzerindeki bir vm'dir.

Merkez sanal ağa bağlantı grafik görünümünü ve ' % s'dalı VNet Ağ İzleyicisi perspektifinden aşağıdaki şekilde gösterilmiştir:

[![2]][2]

Aynı bağlantı için Ağ İzleyicisi ızgara görünümünde aşağıdaki şekilde gösterilmiştir:

[![3]][3]

### <a name="path-to-on-premises-location-1"></a>Şirket içi konuma 1 yolu

Traceroute çıktısını bir merkez sanal ağa bir VM ile şirket içi konum 1 aşağıda gösterilmiştir:

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     2 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

Bu traceroute ilk atlamanın Azure ExpressRoute ağ geçidi tünel uç noktası bir Microsoft Kurumsal kenar yönlendirici (MSEE) için ' dir. İkinci ve üçüncü atlama müşteri (CE) sınır yönlendiricisi ve şirket içi konum 1 LAN IP'ler ' dir. Bu IP adresleri, merkez sanal ağa tanıtılan değildir. Dördüncü atlama, şirket içi konum 1 vm'dir.


### <a name="path-to-on-premises-location-2"></a>Şirket içi konuma 2 yolu

Şirket içi konum 2'de bir VM için bir merkez sanal ağa traceroute çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.1.31.10

    Tracing route to 10.1.31.10 over a maximum of 30 hops

      1    76 ms    75 ms    75 ms  10.10.30.134
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    75 ms    75 ms    75 ms  10.1.31.10

    Trace complete.

Bu traceroute ilk atlamanın bir MSEE için ExpressRoute ağ geçidi tünel uç noktadır. İkinci ve üçüncü atlama CE yönlendirici ve şirket içi konum 2 LAN IP'ler ' dir. Bu IP adresleri, merkez sanal ağa tanıtılan değildir. Dördüncü atlama şirket içi konum 2'üzerindeki bir vm'dir.

### <a name="path-to-the-remote-vnet"></a>Uzak sanal ağ yolu

Uzak sanal ağ içindeki bir VM için bir merkez sanal ağa traceroute çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    69 ms  10.17.30.4

    Trace complete.

Bu traceroute ilk atlamanın bir MSEE için ExpressRoute ağ geçidi tünel uç noktadır. İkinci atlama uzaktan sanal ağın ağ geçidi IP ' dir. Merkez sanal ağda ikinci atlama IP aralığı tanıtılan değil. Üçüncü atlama uzak sanal ağ üzerindeki bir vm'dir.

## <a name="data-path-from-the-spoke-vnet"></a>Veri yolundan bir uç sanal ağı

Uç sanal ağı, merkez sanal ağa ağ görünümünü paylaşır. VNet eşlemesi aracılığıyla doğrudan bir uç sanal ağı bağlı bir uç sanal ağı, merkez sanal ağa uzak ağ geçidi bağlantısı kullanır.

### <a name="path-to-the-hub-vnet"></a>Merkez sanal ağa yolu

Traceroute, bir VM sanal ağ burada gösterilen hub'ında uç sanal ağı çıktı:

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.10.30.4

    Trace complete.

### <a name="path-to-the-branch-vnet"></a>VNet dal yolu

Traceroute, VNet burada gösterilen daldaki bir VM için uç sanal ağı çıktı:

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

Bu traceroute ilk atlamanın merkez sanal ağa VPN geçididir. İkinci atlama dalın VNet VPN ağ geçidi ' dir. Dalın VNet VPN ağ geçidinin IP adresi merkez/uç içinde VNet tanıtılan değil. Üçüncü atlama dal sanal ağ üzerindeki bir vm'dir.

### <a name="path-to-on-premises-location-1"></a>Şirket içi konuma 1 yolu

Uç sanal ağı şirket içi konum 1'deki VM'ye traceroute çıktısı aşağıda gösterilmiştir:

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

Bu traceroute ilk atlamanın hub sanal ağın ExpressRoute ağ geçidi tünel uç bir MSEE için ' dir. İkinci ve üçüncü atlama CE yönlendirici ve şirket içi konum 1 LAN IP'ler ' dir. Bu IP adresleri hub/uç VNet tanıtılan değildir. Dördüncü atlama, şirket içi konum 1 vm'dir.

### <a name="path-to-on-premises-location-2"></a>Şirket içi konuma 2 yolu

Uç sanal ağı şirket içi konum 2'de bir VM traceroute çıktısı aşağıda gösterilmiştir:


    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

Bu traceroute ilk atlamanın hub sanal ağın ExpressRoute ağ geçidi tünel uç bir MSEE için ' dir. İkinci ve üçüncü atlama CE yönlendirici ve şirket içi konum 2 LAN IP'ler ' dir. Bu IP adresleri hub/uç sanal ağlarında tanıtılan değildir. Dördüncü atlama, şirket içi konum 2'deki vm'dir.

### <a name="path-to-the-remote-vnet"></a>Uzak sanal ağ yolu

Uzak sanal ağ içindeki bir sanal makineye uç sanal ağı traceroute çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.10.30.133
      2     *        *        *     Request timed out.
      3    71 ms    70 ms    70 ms  10.17.30.4

    Trace complete.

Bu traceroute ilk atlamanın hub sanal ağın ExpressRoute ağ geçidi tünel uç bir MSEE için ' dir. İkinci atlama uzaktan sanal ağın ağ geçidi IP ' dir. İkinci atlama IP aralığı hub/uç VNet tanıtılan değil. Üçüncü atlama uzak sanal ağ üzerindeki bir vm'dir.

## <a name="data-path-from-the-branch-vnet"></a>VNet dalından veri yolu

### <a name="path-to-the-hub-vnet"></a>Merkez sanal ağa yolu

Traceroute, VNet burada gösterilen hub'ında bir VM sanal ağ daldan çıktı:

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     3 ms  10.10.30.4

    Trace complete.

Bu traceroute ilk atlamanın, dalın VNet VPN geçididir. İkinci atlama merkez sanal ağa VPN ağ geçidi ' dir. Merkez sanal ağa VPN ağ geçidi IP adresini uzak sanal ağda tanıtılan değil. Üçüncü atlama hub sanal ağ üzerindeki bir vm'dir.

### <a name="path-to-the-spoke-vnet"></a>Uç sanal ağ yolu

Traceroute, VNet burada gösterilen uçtaki bir VM sanal ağ daldan çıktı:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     1 ms    <1 ms     1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     2 ms  10.11.30.4

    Trace complete.

Bu traceroute ilk atlamanın, dalın VNet VPN geçididir. İkinci atlama merkez sanal ağa VPN ağ geçidi ' dir. Merkez sanal ağa VPN ağ geçidi IP adresini uzak sanal ağda tanıtılan değil. Üçüncü atlama uç sanal ağ üzerindeki bir vm'dir.

### <a name="path-to-on-premises-location-1"></a>Şirket içi konuma 1 yolu

Bir VM, şirket içi konum 1 VNet dalından traceroute çıktı aşağıda gösterilmiştir:

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.2.30.125
      4     *        *        *     Request timed out.
      5     3 ms     3 ms     3 ms  10.2.30.10

    Trace complete.

Bu traceroute ilk atlamanın, dalın VNet VPN geçididir. İkinci atlama merkez sanal ağa VPN ağ geçidi ' dir. Merkez sanal ağa VPN ağ geçidi IP adresini uzak sanal ağda tanıtılan değil. Üçüncü atlama birincil CE yönlendiricisinde VPN tüneli sonlandırma noktasıdır. Dördüncü atlama bir iç şirket içi konum 1 IP adresidir. Bu LAN IP adresi dışında CE yönlendirici tanıtılan değil. Beşinci atlama hedef VM ile şirket içi konum 1 ' dir.

### <a name="path-to-on-premises-location-2-and-the-remote-vnet"></a>Şirket içi konum 2 ve uzak sanal ağ yolu

Denetim düzlemi analiz ele aldığımız gibi VNet dal yok görünürlük şirket içi konum 2 veya uzak sanal ağ başına ağ yapılandırması vardır. Aşağıdaki ping sonuçları onaylayın: 

    C:\Users\rb>ping 10.1.31.10

    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

    C:\Users\rb>ping 10.17.30.4

    Pinging 10.17.30.4 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.17.30.4:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

## <a name="data-path-from-on-premises-location-1"></a>Veri yolundan şirket içi konum 1

### <a name="path-to-the-hub-vnet"></a>Merkez sanal ağa yolu

Traceroute, şirket içi konum 1'den bir VM sanal ağ burada gösterilen hub'ında çıktı:

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     2 ms     2 ms     2 ms  10.10.30.4

    Trace complete.

Bu traceroute ilk iki atlama şirket içi ağın bir parçasıdır. Üçüncü atlama CE yönlendirici yüzler birincil MSEE'nin arabirimidir. Dördüncü atlama merkez sanal ağı ExpressRoute ağ geçidi ' dir. IP aralığı merkez sanal ağı ExpressRoute ağ geçidi, şirket içi ağa tanıtılan değil. Beşinci atlama, hedef VM olur.

Ağ İzleyicisi, yalnızca Azure merkezli bir görünüm sağlar. Bir şirket içi perspektifi için Azure Ağ Performansı İzleyicisi kullanırız. Ağ Performansı İzleyicisi veri yolu analizi için Azure dışındaki ağlarda sunuculara yüklemek için kullanabileceğiniz aracıları sağlar.

Aşağıdaki şekilde, sanal makineye ' % s'merkez sanal ağı ExpressRoute aracılığıyla şirket içi konum 1. VM bağlantı topolojisi görünümünü gösterir:

[![4]][4]

Daha önce bahsedildiği gibi test kurulumu bir siteden siteye VPN ExpressRoute merkez sanal ağı ile şirket içi konum 1 ila yedekleme bağlantı kullanır. Yedek veri yolu test etmek için şimdi şirket içi konum 1 birincil CE yönlendirici karşılık gelen MSEE arasında bir ExpressRoute bağlantı hatası anlamına. Bir ExpressRoute bağlantı hatası anlamına için MSEE yüzler CE arabirimi kapatın:

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     3 ms  10.10.30.4

    Trace complete.

ExpressRoute bağlantı kapalı olduğunda aşağıdaki şekilde sanal makineye ' % s'merkez sanal ağa siteden siteye VPN bağlantısı aracılığıyla şirket içi konum 1. VM bağlantısı topoloji görünümü gösterir:

[![5]][5]

### <a name="path-to-the-spoke-vnet"></a>Uç sanal ağ yolu

Traceroute, şirket içi konum 1'den bir VM sanal ağ burada gösterilen uçtaki çıktı:

Şimdi veri yolu analizlerini doğru uç sanal ağı ExpressRoute birincil bağlantı geri getirin:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     3 ms     2 ms     2 ms  10.11.30.4

    Trace complete.

Birincil veri yolu analizi geri kalanında 1 ExpressRoute bağlantısının getirin.

### <a name="path-to-the-branch-vnet"></a>VNet dal yolu

Traceroute, şirket içi konum 1'den çıkış VNet burada gösterilen daldaki bir VM için:

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

### <a name="path-to-on-premises-location-2"></a>Şirket içi konuma 2 yolu

İçinde ettiğimizden [denetim düzlemi analiz][Control-Analysis], şirket içi konum 1 başına ağ yapılandırmasının şirket içi konum 2 için hiçbir görünürlük sahiptir. Aşağıdaki ping sonuçları onaylayın: 

    C:\Users\rb>ping 10.1.31.10
    
    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

### <a name="path-to-the-remote-vnet"></a>Uzak sanal ağ yolu

Uzak sanal ağ içindeki bir VM için şirket içi konum 1 traceroute çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2     2 ms     5 ms     7 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5    69 ms    70 ms    69 ms  10.17.30.4

    Trace complete.

## <a name="data-path-from-on-premises-location-2"></a>Veri yolundan şirket içi konum 2

### <a name="path-to-the-hub-vnet"></a>Merkez sanal ağa yolu

Traceroute, şirket içi konum 2'den bir VM sanal ağ burada gösterilen hub'ında çıktı:

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.4
      3    <1 ms    <1 ms    <1 ms  192.168.31.22
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.10.30.4

    Trace complete.

### <a name="path-to-the-spoke-vnet"></a>Uç sanal ağ yolu

Traceroute, şirket içi konum 2'den bir VM sanal ağ burada gösterilen uçtaki çıktı:

    C:\Windows\system32>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops
      1    <1 ms    <1 ms     1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.0
      3    <1 ms    <1 ms    <1 ms  192.168.31.18
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.11.30.4

    Trace complete.

### <a name="path-to-the-branch-vnet-on-premises-location-1-and-the-remote-vnet"></a>VNet, dal yolu şirket içi konum 1 ve uzak sanal ağ

İçinde ettiğimizden [denetim düzlemi analiz][Control-Analysis], şirket içi konum 1 hiçbir görünürlük dal VNet, şirket içi konum 1 veya uzak sanal ağ başına ağ yapılandırması vardır. 

## <a name="data-path-from-the-remote-vnet"></a>Uzak sanal ağ veri yolu

### <a name="path-to-the-hub-vnet"></a>Merkez sanal ağa yolu

Traceroute, uzaktan sanal ağdan bir VM sanal ağ burada gösterilen hub'ında çıktı:

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    65 ms    65 ms    65 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    68 ms  10.10.30.4

    Trace complete.

### <a name="path-to-the-spoke-vnet"></a>Uç sanal ağ yolu

Traceroute, uzaktan sanal ağdan bir VM sanal ağ burada gösterilen uçtaki çıktı:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    71 ms    69 ms    69 ms  10.11.30.4

    Trace complete.

### <a name="path-to-the-branch-vnet-and-on-premises-location-2"></a>VNet dal yolu ve şirket içi konum 2

İçinde ettiğimizden [denetim düzlemi analiz][Control-Analysis], uzak sanal ağ yok görünürlük VNet dal ya da şirket içi konum 2 ağ yapılandırmasını başına sahiptir. 

### <a name="path-to-on-premises-location-1"></a>Şirket içi konuma 1 yolu

VM ile şirket içi konum 1 için Uzak sanal ağdan traceroute çıktı aşağıda gösterilmiştir:

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    69 ms    69 ms    69 ms  10.2.30.10

    Trace complete.


## <a name="expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Tutarlılığın ExpressRoute ve siteden siteye VPN bağlantısı

###  <a name="site-to-site-vpn-over-expressroute"></a>ExpressRoute üzerinden siteden siteye VPN

ExpressRoute özel olarak şirket içi ağınız ve Azure Vnet'ler arasında veri alışverişi eşleme Microsoft kullanarak siteden siteye VPN yapılandırabilirsiniz. Bu yapılandırma ile gizliliği, kimlik doğrulaması ve bütünlük ile veri alışverişinde bulunabilir. Veri değişimi yürütmeyi de olur. ExpressRoute eşdüzey hizmet sağlama Microsoft kullanarak siteden siteye IPSec VPN tüneli modunda yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute Microsoft eşlemesi üzerinde siteden siteye VPN][S2S-Over-ExR]. 

Microsoft eşlemesi kullanan bir siteden siteye VPN yapılandırma birincil sınırlama aktarım hızıdır. IPSec tüneli üzerinden aktarım hızı ile VPN ağ geçidi kapasitesi sınırlı. VPN gateway performansı ExpressRoute üretilen işten daha küçük. Bu senaryoda, ExpressRoute bant genişliği kullanımını iyileştirmek için yüksek oranda güvenli trafiği IPSec tünel kullanılarak ve diğer tüm trafiği için özel eşdüzey hizmet sağlama kullanarak yardımcı olur.

### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>ExpressRoute için bir güvenli bir yük devretme yolu olarak siteden siteye VPN

ExpressRoute, yüksek kullanılabilirlik sağlamak için yedekli devre çiftinin görev yapar. Farklı Azure bölgelerinde coğrafi olarak yedekli ExpressRoute bağlantı yapılandırabilirsiniz. Ayrıca, bir Azure bölgesi içinde bizim test Kurulum gösterildiği şekilde bir yük devretme yolu için ExpressRoute bağlantınızı oluşturmak için bir siteden siteye VPN kullanabilirsiniz. ExpressRoute ve siteden siteye VPN üzerinden aynı ön eklerin tanıtılıp, Azure ExpressRoute önceliklendirir. ExpressRoute ve siteden siteye VPN arasında asimetrik yönlendirme önlemek için ağ yapılandırması de siteden siteye VPN bağlantısı kullanmadan önce ExpressRoute bağlantısı kullanarak reciprocate şirket içi.

ExpressRoute ve siteden siteye VPN için bir arada var olabilen bağlantılar yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute ve siteden siteye birlikte kullanımı][ExR-S2S-CoEx].

## <a name="extend-back-end-connectivity-to-spoke-vnets-and-branch-locations"></a>Arka uç bağlantı uç sanal ağları ve şube konumları için genişletin

### <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>VNet eşlemesi kullanarak uç VNet bağlantısı

Merkez ve uç VNet mimarisi yaygın olarak kullanılır. Hub merkezi bir şirket içi ağınıza, uç sanal ağları arasında bağlantı noktası gören azure'daki bir sanal ağ ' dir. Uçlar hub'la eş sanal ağ olan ve hangi iş yüklerini yalıtmak için kullanabilirsiniz. Hub'ı bir ExpressRoute veya VPN bağlantısı aracılığıyla şirket içi veri merkeziniz arasındaki trafik akışı. Mimarisi hakkında daha fazla bilgi için bkz. [Azure'da merkez-uç ağ topolojisi uygulama][Hub-n-Spoke].

Bir bölge içinde eşlemesi sanal ağda uç sanal ağları, uzak ağlarla iletişim kuracak hub VNet ağ geçitleriniz (hem VPN ve ExpressRoute ağ geçitleri) kullanabilirsiniz.

### <a name="branch-vnet-connectivity-by-using-site-to-site-vpn"></a>Siteden siteye VPN kullanarak sanal ağa bağlantı dal

Sanal ağlar farklı bölgelerde ve şirket içi ağlarda hub sanal ağ birbirleriyle iletişim kurmak için dal isteyebilirsiniz. Yerel Azure bu yapılandırma için siteden siteye VPN bağlantısı bir VPN kullanarak çözümüdür. Bir alternatif, hub'ı yönlendirme için bir ağ sanal Gereci (NVA) kullanmaktır.

Daha fazla bilgi için [VPN ağ geçidi nedir?] [ VPN] ve [yüksek oranda kullanılabilir bir NVA dağıtın][Deploy-NVA].


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [ExpressRoute SSS] [ ExR-FAQ] için:
-   Bir ExpressRoute ağ geçidine bağlanabilir kaç ExpressRoute bağlantı hatları öğrenin.
-   Bir ExpressRoute bağlantı hattına bağlayabilirsiniz kaç ExpressRoute ağ geçitleri hakkında bilgi edinin.
-   Diğer ExpressRoute ölçek sınırları hakkında bilgi edinin.


<!--Image References-->
[1]: ./media/backend-interoperability/HubVM-SpkVM.jpg "merkez sanal ağa bağlantısı uç sanal ağı için Ağ İzleyicisi görünümünü"
[2]: ./media/backend-interoperability/HubVM-BranchVM.jpg "merkez sanal ağa bağlantısı bir dalı VNet Ağ İzleyicisi görünümünü"
[3]: ./media/backend-interoperability/HubVM-BranchVM-Grid.jpg "Ağ İzleyicisi ızgara görünümünde bir dal VNet için bir merkez sanal ağa bağlantısı"
[4]: ./media/backend-interoperability/Loc1-HubVM.jpg "konumu 1. VM bağlantısı merkez sanal ağa ExpressRoute 1 aracılığıyla Ağ Performansı İzleyicisi görünümünü"
[5]: ./media/backend-interoperability/Loc1-HubVM-S2S.jpg "konumu 1. VM bağlantısı merkez sanal ağa bir siteden siteye VPN aracılığıyla Ağ Performansı İzleyicisi görünümünü"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/networking/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[VNet-Config]: https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering


