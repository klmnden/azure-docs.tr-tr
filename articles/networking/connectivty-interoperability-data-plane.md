---
title: 'ExpressRoute, siteden siteye VPN ve VNet eşlemesi - birlikte çalışabilirliği veri düzlemi çözümleme: Azure arka uç bağlantısı özellikleri birlikte çalışabilirlik | Microsoft Docs'
description: Bu sayfada, ExpressRoute, siteden siteye VPN ve VNet eşlemesi özellikleri birlikte çalışabilirliği analiz etmek için oluşturulan test kurulum veri düzlemi analizini sağlar.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: c9f3824b1e0f44338696ba3c2e434d60eee3af8b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947345"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---data-plane-analysis"></a>ExpressRoute, siteden siteye VPN ve VNet-eşlemesi - veri düzlemi analizi birlikte çalışabilirliği

Bu makalede, testi kurulumun veri düzlemi analizi Bahsedelim. Test Kurulumu gözden geçirmek için bkz: [Test Kurulum][Setup]. Test Kurulumu yapılandırma ayrıntıları gözden geçirmek için bkz: [Test Kurulum Yapılandırması][Configuration]. Denetim düzlemi analizi testi kurulumun gözden geçirmek için bkz: [denetim düzlemi analizi][Control-Analysis].

Veri düzlemi analiz paketlerinin başka bir topoloji için bir yerel ağ (LAN/VNet) üzerinden geçiş yapılan yol inceler. İki yerel ağ arasındaki veri yolu mutlaka simetrik olmayabilir. Bu nedenle, bu makaledeki şimdi bir yerel ağ iletme yolundan ters yolundan ayrı olarak başka bir analiz edin.

##<a name="data-path-from-hub-vnet"></a>Veri yolu Hub vnet'ten

###<a name="path-to-spoke-vnet"></a>Sanal ağ uç yolu

VNet eşlemesi, eşlenen iki sanal ağ arasında ağ köprüsü işlevselliğine öykünür. Hub VNet traceroute çıktısını bir VM sanal ağ uçtaki aşağıda verilmiştir:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.11.30.4

    Trace complete.

Aşağıdaki ekranı küçük resmi Hub sanal ağ ve VNet Azure Ağ İzleyicisi tarafından sunulan uç grafik bağlantı görünümü şu şekildedir:


[![1]][1]

###<a name="path-to-branch-vnet"></a>Dal VNet yolu

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms     1 ms     1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     2 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

Yukarıdaki traceroute ilk atlamanın Hub sanal ağ VPN GW ' dir. İkinci atlama dal, IP adresi Hub sanal ağ içinde yayınlanmaz sanal ağ VPN GW ' dir. Üçüncü atlama dal VNet üzerindeki bir vm'dir.

Aşağıdaki ekranı küçük resmi Hub sanal ağ ve Azure Ağ İzleyicisi tarafından sunulan dal VNet grafik bağlantı görünümü şu şekildedir:

[![2]][2]

Aynı bağlantı için aşağıdaki ekran küçük Azure Ağ İzleyicisi tarafından sunulan kılavuz görünümü şu şekildedir:

[![3]][3]

###<a name="path-to-on-premises-location-1"></a>Şirket içi konum-1 yolu

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     2 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

Yukarıdaki traceroute içinde ilk atlamanın MSEE için ExpressRoute GW tünel uç noktadır. İkinci ve üçüncü atlama CE yönlendirici ve şirket içi konum 1 LAN IP'ler sırasıyla, bu IP adresleri Hub sanal ağ içinde tanıtılmaz. Dördüncü atlama VM üzerinde şirket içi konum-1 ' dir.


###<a name="path-to-on-premises-location-2"></a>Şirket içi konum-2 yolu

    C:\Users\rb>tracert 10.1.31.10

    Tracing route to 10.1.31.10 over a maximum of 30 hops

      1    76 ms    75 ms    75 ms  10.10.30.134
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    75 ms    75 ms    75 ms  10.1.31.10

    Trace complete.

Yukarıdaki traceroute içinde ilk atlamanın MSEE için ExpressRoute GW tünel uç noktadır. İkinci ve üçüncü atlama CE yönlendirici ve şirket içi konum 2 LAN IP'ler sırasıyla, bu IP adresleri Hub sanal ağ içinde tanıtılmaz. Dördüncü atlama şirket içi konum 2'üzerindeki bir vm'dir.

###<a name="path-to-remote-vnet"></a>Uzak sanal ağ yolu

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    69 ms  10.17.30.4

    Trace complete.

Yukarıdaki traceroute içinde ilk atlamanın MSEE için ExpressRoute GW tünel uç noktadır. İkinci atlama uzaktan sanal ağın ağ geçidi IP ' dir. İkinci atlama IP aralığı Hub sanal ağ içinde bildirilmez. Üçüncü atlama uzak sanal ağ üzerindeki bir vm'dir.

##<a name="data-path-from-spoke-vnet"></a>Uç sanal ağ veri yolu

Geri çağırma uç VNet Hub sanal ağın ağ görünümü paylaşın. VNet eşlemesi aracılığıyla doğrudan bir uç sanal ağı bağlı bir uç sanal ağı, merkez sanal ağa uzak ağ geçidi bağlantısı kullanır.

###<a name="path-to-hub-vnet"></a>Merkez sanal ağ yolu

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.10.30.4

    Trace complete.

###<a name="path-to-branch-vnet"></a>Dal VNet yolu

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

Yukarıdaki traceroute ilk atlamanın Hub sanal ağ VPN GW ' dir. İkinci atlama dal, IP adresi merkez/uç sanal ağ içinde yayınlanmaz sanal ağ VPN GW ' dir. Üçüncü atlama dal VNet üzerindeki bir vm'dir.

###<a name="path-to-on-premises-location-1"></a>Şirket içi konum-1 yolu

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

Yukarıdaki traceroute ilk atlamanın MSEE Hub sanal ağın ExpressRoute GW tünel uç noktasına ' dir. İkinci ve üçüncü atlama CE yönlendirici ve şirket içi konum 1 LAN IP'ler sırasıyla, bu IP adresleri merkez/uç sanal ağ içinde tanıtılmaz. Dördüncü atlama VM üzerinde şirket içi konum-1 ' dir.

###<a name="path-to-on-premises-location-2"></a>Şirket içi konum-2 yolu

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

Yukarıdaki traceroute ilk atlamanın MSEE Hub sanal ağın ExpressRoute GW tünel uç noktasına ' dir. İkinci ve üçüncü atlama CE yönlendirici ve şirket içi konum 2 LAN IP'ler sırasıyla, bu IP adresleri Hub/bağlı sanal ağlar içinde tanıtılmaz. Dördüncü atlama şirket içi konum 2'üzerindeki bir vm'dir.

###<a name="path-to-remote-vnet"></a>Uzak sanal ağ yolu

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.10.30.133
      2     *        *        *     Request timed out.
      3    71 ms    70 ms    70 ms  10.17.30.4

    Trace complete.

Yukarıdaki traceroute ilk atlamanın MSEE Hub sanal ağın ExpressRoute GW tünel uç noktasına ' dir. İkinci atlama uzaktan sanal ağın ağ geçidi IP ' dir. İkinci atlama IP aralığı merkez/uç sanal ağ içinde bildirilmez. Üçüncü atlama uzak sanal ağ üzerindeki bir vm'dir.

##<a name="data-path-from-branch-vnet"></a>Dal vnet'ten veri yolu

###<a name="path-to-hub-vnet"></a>Merkez sanal ağ yolu

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     3 ms  10.10.30.4

    Trace complete.

Yukarıdaki traceroute ilk atlamanın dal vnet'in VPN GW ' dir. İkinci atlama IP adresleri uzak sanal ağ içinde yayınlanmaz Hub sanal ağ VPN GW ' dir. Üçüncü atlama Hub sanal ağ üzerindeki bir vm'dir.

###<a name="path-to-spoke-vnet"></a>Sanal ağ uç yolu

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     1 ms    <1 ms     1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     2 ms  10.11.30.4

    Trace complete.

Yukarıdaki traceroute ilk atlamanın dal vnet'in VPN GW ' dir. İkinci atlama IP adresleri, uzak sanal ağ içinde bildirilmez, Hub sanal ağ VPN GW ve üçüncü atlama uç sanal ağ üzerindeki VM.

###<a name="path-to-on-premises-location-1"></a>Şirket içi konum-1 yolu

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.2.30.125
      4     *        *        *     Request timed out.
      5     3 ms     3 ms     3 ms  10.2.30.10

    Trace complete.

Yukarıdaki traceroute ilk atlamanın dal vnet'in VPN GW ' dir. İkinci atlama IP adresleri uzak sanal ağ içinde yayınlanmaz Hub sanal ağ VPN GW ' dir. Üçüncü atlama birincil CE yönlendiricisinde VPN tüneli sonlandırma noktasıdır. Dördüncü atlama CE yönlendirici dışında yayınlanmaz şirket içi konum 1 LAN IP adresi, iç bir IP adresidir. Beşinci atlama hedef VM üzerinde şirket içi konum-1 ' dir.

###<a name="path-to-on-premises-location-2-and-remote-vnet"></a>Şirket içi konum 2 ve uzak sanal ağ yolu

Denetim düzlemi analizi önceki ele aldığımız gibi VNet dal yok görünürlük veya şirket içi konum-2, hem de uzak sanal ağ başına ağ yapılandırması vardır. Aşağıdaki ping sonuçları olgu onaylayın. 

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

##<a name="data-path-from-on-premises-location-1"></a>Şirket içi konum-1'den veri yolu

###<a name="path-to-hub-vnet"></a>Merkez sanal ağ yolu

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     2 ms     2 ms     2 ms  10.10.30.4

    Trace complete.

Yukarıdaki traceroute ilk iki atlama şirket içi ağın bir parçasıdır. Üçüncü atlama CE yönlendirici karşılıklı birincil MSEE'nin arabirimidir. Dördüncü atlama ExpressRoute G/W sanal ağ, IP aralığı şirket içi ağda yayınlanmaz hub'ının ' dır. Beşinci atlama, hedef VM olur.

Azure Ağ İzleyicisi, yalnızca Azure odaklı görünümünü sağlar. Bu nedenle, şirket içi odaklı görünümü için şu Azure Ağ Performansı İzleyicisi'ni (NPM) kullanılmıştır. Azure dışındaki ağ yüklü sunucular ve veri yolu analizlerini aracıları NPM sağlar.

Aşağıdaki ekranı küçük resmi topoloji sanal makine şirket içi konum 1 VM bağlantısını merkez sanal ağı ExpressRoute aracılığıyla şirket görünümüdür.

[![4]][4]

Geri çağırırsanız, test Kurulum Merkezi sanal ağ ile şirket içi konum-1 ila ExpressRoute için yedekleme bağlantısı siteden siteye VPN kullanır. Geri datapath test etmek için şimdi MSEE karşılıklı CE arabirimi kapatarak şirket içi konum 1 birincil CE yönlendirici ve karşılık gelen MSEE arasında bir ExpressRoute bağlantı hatası anlamına.

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     3 ms  10.10.30.4

    Trace complete.

ExpressRoute bağlantı kapalı olduğunda aşağıdaki ekranı küçük resmi topoloji sanal makine şirket içi konum 1 VM bağlantısını siteden siteye VPN bağlantısı üzerinden sanal hub'ında görünümüdür.

[![5]][5]

###<a name="path-to-spoke-vnet"></a>Sanal ağ uç yolu

Bize uç VNet doğrultusunda datapath analizlerini ExpressRoute birincil bağlantı geri getirin.

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     3 ms     2 ms     2 ms  10.11.30.4

    Trace complete.

Birincil ExpressRoute-1 bağlantı datapath analiz geri kalanı için bize getirecek.

###<a name="path-to-branch-vnet"></a>Dal VNet yolu

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

###<a name="path-to-on-premises-location-2"></a>Şirket içi konum-2 yolu

Denetim düzlemi analizi önceki ele aldığımız gibi şirket içi konum 1 hiçbir şirket içi konum-2 ağ yapılandırmasını başına görünürlük sahiptir. Aşağıdaki ping sonuçları olgu onaylayın. 

    C:\Users\rb>ping 10.1.31.10
    
    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

###<a name="path-to-remote-vnet"></a>Uzak sanal ağ yolu

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2     2 ms     5 ms     7 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5    69 ms    70 ms    69 ms  10.17.30.4

    Trace complete.

##<a name="data-path-from-on-premises-location-2"></a>Şirket içi konum-2'den veri yolu

###<a name="path-to-hub-vnet"></a>Merkez sanal ağ yolu

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.4
      3    <1 ms    <1 ms    <1 ms  192.168.31.22
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.10.30.4

    Trace complete.

###<a name="path-to-spoke-vnet"></a>Sanal ağ uç yolu

    C:\Windows\system32>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops
      1    <1 ms    <1 ms     1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.0
      3    <1 ms    <1 ms    <1 ms  192.168.31.18
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.11.30.4

    Trace complete.

###<a name="path-to-branch-vnet-on-premises-location-1-and-remote-vnet"></a>Sanal ağ, şirket içi konum-1 ve uzak VNet dal yolu

Denetim düzlemi analizi önceki ele aldığımız gibi şirket içi konum 1 dal VNet yok görünürlüğe sahip olduğu, konum-1 ve uzak sanal ağ ağ yapılandırmasının şirket. 

##<a name="data-path-from-remote-vnet"></a>Uzak sanal ağ veri yolu

###<a name="path-to-hub-vnet"></a>Merkez sanal ağ yolu

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    65 ms    65 ms    65 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    68 ms  10.10.30.4

    Trace complete.

###<a name="path-to-spoke-vnet"></a>Sanal ağ uç yolu

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    71 ms    69 ms    69 ms  10.11.30.4

    Trace complete.

### <a name="path-to-branch-vnet-and-on-premises-location-2"></a>Dal VNet ve şirket içi konum-2 yolu

Denetim düzlemi analizi önceki ele aldığımız gibi uzak sanal ağ yok görünürlük dal VNet ve şirket içi konum-2 ağ yapılandırmasını başına sahiptir. 


### <a name="path-to-on-premises-location-1"></a>Şirket içi konum-1 yolu

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    69 ms    69 ms    69 ms  10.2.30.10

    Trace complete.


## <a name="further-reading"></a>Daha fazla bilgi

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>ExpressRoute ve siteden siteye VPN bağlantısı dağıtımınızla kullanma

####<a name="site-to-site-vpn-over-expressroute"></a>ExpressRoute üzerinden siteden siteye VPN

Özel olarak gizliliği, yürütmeyi, kimlik doğrulaması ve bütünlük ile şirket içi ağınız ve Azure Vnet'ler arasında veri alışverişi ExpressRoute Microsoft eşlemesi üzerinde siteden siteye VPN yapılandırılabilir. ExpressRoute Microsoft eşlemesi üzerinde siteden siteye IPSec VPN tüneli modunda yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute Microsoft eşlemesi üzerinde siteden siteye VPN][S2S-Over-ExR]. 

Aktarım hızı, Microsoft eşlemesi üzerinden S2S VPN yapılandırma ana sınırlamasıdır. IPSec tüneli üzerinden aktarım hızı ile VPN ağ geçidi kapasitesi sınırlı. ExpressRoute üretilen kıyasla daha az VPN GW aktarım hızıdır. Böyle senaryolarda güvenli yüksek trafik ve diğer tüm trafiği için özel eşdüzey hizmet sağlama için IPSec tüneli kullanarak ExpressRoute bant genişliği kullanımını en iyi duruma getirme yardımcı olacaktır.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>ExpressRoute için bir güvenli bir yük devretme yolu olarak siteden siteye VPN
ExpressRoute, yüksek kullanılabilirlik sağlamak için yedekli devre çiftinin sunulur. Farklı Azure bölgelerinde coğrafi olarak yedekli ExpressRoute bağlantı yapılandırabilirsiniz. ExpressRoute bağlantınızı için bir yük devretme yolu isterseniz de bizim test ayarında gibi belirli bir Azure bölgesi içinde siteden siteye VPN kullanarak bunu yapabilirsiniz. ExpressRoute ve S2S VPN üzerinden aynı ön eklerin tanıtılıp, Azure ExpressRoute üzerinden S2S VPN tercih eder. ExpressRoute ve S2S VPN arasında asimetrik yönlendirme önlemek için ağ yapılandırması de S2S VPN bağlantısı ExpressRoute belgelemeyi reciprocate şirket içi.

ExpressRoute ve siteden siteye VPN bir arada var olabilen bağlantılar yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute ve siteden siteye birlikte kullanımı][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Sanal ağlar ve şube konumları uç için arka uç bağlantısı genişletme

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Uç sanal ağ bağlantısı kullanarak VNet eşlemesi

Merkez ve uç Vnet mimarisi yaygın olarak kullanılır. Hub'ı, merkezi bir şirket içi ağınıza, uç sanal ağları arasında bağlantı noktası gören azure'daki bir sanal ağın (VNet) olan. Uçlar hub'la eş ve iş yüklerini yalıtmak için kullanılabilir sanal ağlar ' dir. Hub'ı bir ExpressRoute veya VPN bağlantısı aracılığıyla şirket içi veri merkeziniz arasındaki trafik akışı. Mimarisi hakkında daha fazla ayrıntı için bkz. [merkez ve uç mimarisi][Hub-n-Spoke]

VNet eşlemesi bir bölge içinde uç sanal ağları hub VNet ağ geçitleriniz (hem VPN ve ExpressRoute ağ geçitleri), uzak ağlarla iletişim kuracak kullanmasını sağlar.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Siteden siteye VPN kullanarak sanal ağa bağlantı dal

İstediğiniz dalı (farklı bölgelerde) sanal ağlar ve şirket içi ağlarda hub sanal ağ birbirleriyle iletişim, yerel Azure VPN kullanarak siteden siteye VPN bağlantısı çözümüdür. Alternatif bir seçenek, hub'ı yönlendirme için bir NVA kullanmaktır.

VPN ağ geçitlerini yapılandırmak için bkz: [VPN ağ geçidi yapılandırma][VPN]. Yüksek oranda kullanılabilir bir NVA dağıtmak için bkz: [yüksek oranda kullanılabilir bir NVA dağıtın][Deploy-NVA].

## <a name="next-steps"></a>Sonraki adımlar

Kaç ExpressRoute bağlantı hatları için bir ExpressRoute ağ geçidiyle bağlanabilir veya kaç ExpressRoute ağ geçidi bir ExpressRoute bağlantı hattına bağlayabilirsiniz öğrenmek ya da diğer ExpressRoute ölçek sınırları öğrenmek için bkz. [ExpressRoute SSS][ExR-FAQ]


<!--Image References-->
[1]: ./media/backend-interoperability/HubVM-SpkVM.jpg "Hub VNet bağlantısı uç sanal ağ için Ağ İzleyicisi görünümünü"
[2]: ./media/backend-interoperability/HubVM-BranchVM.jpg "Hub VNet bağlantısı dal vnet'e Ağ İzleyicisi görünümünü"
[3]: ./media/backend-interoperability/HubVM-BranchVM-Grid.jpg "ızgara görünümünde Hub VNet bağlantısı dal vnet'e Ağ İzleyicisi"
[4]: ./media/backend-interoperability/Loc1-HubVM.jpg "konum 1 VM'den bağlantı hub'ı vnet'e ExpressRoute 1 aracılığıyla Ağ Performansı İzleyicisi görünümünü"
[5]: ./media/backend-interoperability/Loc1-HubVM-S2S.jpg "konum 1 VM bağlantısı S2S VPN aracılığıyla Merkezi sanal ağ için Ağ Performansı İzleyicisi görünümünü"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-config
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




