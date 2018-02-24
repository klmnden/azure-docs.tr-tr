---
title: "Ağ Azure günlük analizi Performans İzleyicisi çözümde | Microsoft Docs"
description: "ExpressRoute Manager özelliği ağ Performans İzleyicisi'nde, uçtan uca bağlantı ve performans arasında şube ofislerinde ve Azure, Azure ExpressRoute izlemenize olanak tanır."
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 405bcd7d69e93f838d35b61be489464fcf55ee88
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="expressroute-manager"></a>ExpressRoute Manager

ExpressRoute Yöneticisi özelliği [Ağ Performansı İzleyicisi](log-analytics-network-performance-monitor.md) uçtan uca bağlantı ve performans arasında şube ofislerinde ve Azure, Azure ExpressRoute izlemenize olanak tanır. Anahtar avantajları şunlardır: 

- ExpressRoute bağlantı hatları aboneliğinizle ilişkili otomatik algılama 
- Bant genişliği kullanımı, kaybı ve gecikme hattı, eşleme ve sanal ağ düzeyinde ExpressRoute için izleme 
- ExpressRoute bağlantı hatları, ağ topolojisinin bulma 

![ExpressRoute Monitor](media/log-analytics-network-performance-monitor/expressroute-intro.png)

## <a name="configuration"></a>Yapılandırma 
Ağ Performansı İzleyicisi Yapılandırması'nı açmak için açın [Ağ Performansı İzleyicisi çözüm](log-analytics-network-performance-monitor.md) tıklatıp **yapılandırma** düğmesi.

### <a name="configure-nsg-rules"></a>NSG kuralları yapılandırın 
NPM izleme için kullanılmakta olan sunucular için azure'da, ağ güvenlik grubu (NSG) kuralları tarafından NPM yapay işlemleri için kullanılan bağlantı noktası TCP trafiğine izin verecek şekilde yapılandırmanız gerekir. Varsayılan bağlantı noktası 8084 ' dir. Böylece, bir şirket içi ile iletişim kurmak için Azure VM yüklü OMS Aracısı İzleme Aracısı. 

NSG hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

>[!NOTE]
> Aracıları (hem şirket içi sunucu Aracısı hem de Azure sunucusu aracı) yüklediyseniz ve bu adımla devam etmeden önce EnableRules.ps1 PowerShell Betiği çalıştırdıktan emin olun. 

 
### <a name="discover-expressroute-peering-connections"></a>ExpressRoute eşdüzey hizmet sağlaması bağlantıları Bul 
 
1. Tıklayın **ExpressRoute eşlemeler** görünümü.  
2. Tıklayın **Şimdi Bul** Azure Abonelikteki sanal ağlar için bağlı özel eşlemeler, bu günlük analizi çalışma alanı ile bağlantılı tüm ExpressRoute Bul düğmesi.  

>[!NOTE]  
> Çözüm şu anda yalnızca ExpressRoute özel eşlemeler bulur. 

>[!NOTE]  
> Yalnızca bu özel eşlemeler, bu günlük analizi çalışma alanı ile bağlantılı abonelikle ilişkili sanal ağlar için bağlandığı bulunur. Bu çalışma alanına bağlı abonelik dışında ExpressRoute sanal ağlara bağlı ise, bu Aboneliklerde günlük analizi çalışma alanı oluşturmak ve bu eşlemeler izlemek için NPM kullanmak gerekir. 

 ![ExpressRoute Yapılandır](media/log-analytics-network-performance-monitor/expressroute-configure.png)
 
 Bulma tamamlandıktan sonra bulunan özel eşliği bağlantılar bir tabloda listelenmiştir. Bu eşlemeler için izleme başlangıçta devre dışı durumda olacaktır. 

### <a name="enable-monitoring-of-the-expressroute-peering-connections"></a>ExpressRoute eşleme bağlantıları izlemeyi etkinleştir 

1. Bağlanmakta özel eşleme tıklatın ilgileniyor izleme.  
2. RHS bölmesinde, onay kutusunu tıklayın **bu eşlemesi izlemek**. 
3. Bu bağlantı için sistem durumu olayları oluşturmak istiyorsanız, ardından denetleyin **etkinleştir Bu eşleme için sistem durumu izleme**. 
4. İzleme koşulları seçin. Eşik değerleri yazarak, sistem olay oluşturma için özel eşikler ayarlayabilirsiniz. Eşleme bağlantı için seçilen eşiğin üstünde koşul değerini gider olduğunda, bir sistem durumu olayı oluşturulur. 
5. Tıklayın **eklemek aracıları** Bu eşleme bağlantı izleme için kullanmayı düşündüğünüz izleme aracıları seçin. Bağlantı, bu eşleme için bağlı Azure VNET içinde en az bir aracı ve aynı zamanda bu eşleme için bağlı en az bir şirket içi aracı, her iki ucunda aracıları ekleme emin olmanız gerekir. 
6. tıklatın **kaydetmek** yapılandırmayı kaydetmek için. 

![ExpressRoute Yapılandır](media/log-analytics-network-performance-monitor/expressroute-configure-discovery.png)


Kuralları etkinleştirme ve değerleri seçip izlemek istediğiniz aracıları sonra yaklaşık olarak 30-60 dakika doldurma başlamak değerleri için bekleme yoktur ve **ExpressRoute izleme** döşeme kullanılabilir hale gelir. İzleme döşeme gördüğünüzde, ExpressRoute bağlantı hatları ve bağlantı kaynaklarını NPM tarafından izlenen. 

>[!NOTE]
> Yeni sorgu dili yükselttikten çalışma alanları güvenilir Bu yetenek çalışır.  

## <a name="walkthrough"></a>Kılavuz 

Ağ performansı izleme Panosu ExpressRoute bağlantı hatları ve eşleme bağlantıları sistem durumu özetini gösterir. 

![NPM Pano ExpressRoute](media/log-analytics-network-performance-monitor/npm-dashboard-expressroute.png) 

### <a name="circuits-list"></a>Bağlantı hatları listesi 

Tüm izlenen ExpressRoute bağlantı hatları listesini görmek için ExpressRoute bağlantı hatları kutucuğa tıklayın. Bir bağlantı hattı seçin ve sistem durumu, paket kaybı, bant genişliği kullanımı ve gecikme eğilim grafikleri görüntüleyin. Grafikler etkileşimlidir. Grafikler çizdirmek için bir özel zaman penceresi seçebilirsiniz. Yakınlaştırma ve hassas veri noktaları görmek için grafikte bir alanın üzerinde fare sürükleyebilirsiniz. 

![ExpressRoute bağlantı hatları listesi](media/log-analytics-network-performance-monitor/expressroute-circuits.png) 

### <a name="trend-of-loss-latency-and-throughput"></a>Kaybı, gecikme ve verimlilik eğilimi 

Bant genişliği, gecikme ve kaybı grafikleri etkileşimlidir. Fare denetimlerini kullanarak bu grafikler herhangi bir bölüme yakınlaştırabilirsiniz. Bant genişliği, gecikme ve kaybı veri tarih, tıklatarak diğer aralıkları için sol üst Eylemler düğmeyi aşağıda bulunan de görebilirsiniz. 

![ExpressRoute gecikme süresi](media/log-analytics-network-performance-monitor/expressroute-latency.png) 

### <a name="peerings-list"></a>Eşlemeleri listesi 

Tıklayarak **özel eşlemeler** panosundaki bölümünden getirir sanal ağlar için tüm bağlantıların listesi özel eşleme üzerinden. Burada, bir sanal seçebilirsiniz ağ bağlantısı ve sistem durumu, paket kaybı, bant genişliği kullanımı ve gecikme eğilim grafikleri görüntüleyin. 

![ExpressRoute eşlemeleri](media/log-analytics-network-performance-monitor/expressroute-peerings.png) 

### <a name="circuit-topology"></a>Bağlantı hattı topolojisi 

Bağlantı hattı topolojisini görüntülemek için tıklatın **topoloji** döşeme. Bu seçili topoloji görünümü sürer hattı veya eşliği. Topoloji diyagramı ağdaki her segment için gecikme süresini sağlar ve her katman 3 atlama diyagramın bir düğümle temsil edilir. Atlama tıklatarak atlama hakkında daha fazla ayrıntı ortaya çıkarır. Şirket içi atlama çubuğu aşağıdaki taşıyarak dahil etmek için görünürlük düzeyini artırabilirsiniz **filtreleri**. Kaydırıcı çubuğun sola veya sağa hareket, artar/topoloji grafikte durak sayısını azaltır. Her segment arasında gecikme yüksek gecikme kesimleri ağınızdaki daha hızlı yalıtım sağlayan, görülebilir. 

![ExpressRoute topolojisi](media/log-analytics-network-performance-monitor/expressroute-topology.png)  

### <a name="detailed-topology-view-of-a-circuit"></a>Bir bağlantı hattının ayrıntılı topoloji görünümü 

Bu görünüm, VNet bağlantıları gösterir. 

![ExpressRoute VNet](media/log-analytics-network-performance-monitor/expressroute-vnet.png)
 

### <a name="diagnostics"></a>Tanılama 

Ağ Performansı İzleyicisi birkaç hattı bağlantı sorunları tanılamanıza yardımcı olur. Bazı sorunlar aşağıda listelenmiştir 

**Bağlantı hattı çalışmıyor.** Şirket içi kaynakları ve Azure sanal ağlar arasında bağlantı kaybolur hemen NPM size bildirir. Bu, kullanıcı çözümler alma önce öngörülü eylemi gerçekleştirin ve aşağı süresini kısaltmak yardımcı olur 

![Expressroute bağlantı hattı aşağı](media/log-analytics-network-performance-monitor/expressroute-circuit-down.png)
 

**Trafik hedeflenen bağlantı hattı akan değil.** Hedeflenen expressroute bağlantı hattı trafik beklenmedik bir şekilde akışı her NPM size bildirebilir. Bu bağlantı hattı çalışmıyor ve yedekleme yönlendir trafiği akan veya yönlendirme bir sorun olduğunda meydana gelebilir. Bu bilgiler proaktif olarak Yönlendirme ilkelerinizi herhangi bir yapılandırma sorunu yönetmenize ve en iyi ve güvenli rota kullanıldığından emin olun yardımcı olur. 

 

**Trafik birincil bağlantı hattı akan değil.** İkincil expressroute bağlantı hattı trafiği akan özelliği size bildirir. Bağlantı sorunları bu durumda karşılaşmazsınız, ancak proaktif olarak sorun giderme birincil bağlantı hattı sorunları, daha iyi hazırlıklı olun rağmen. 

 
![ExpressRoute trafik akışı](media/log-analytics-network-performance-monitor/expressroute-traffic-flow.png)


**En yüksek kullanımı düşüşünü.** Bant genişliği kullanım eğilimi Azure iş yükü düşüşünü en yüksek bant genişliği kullanımı nedeniyle olsun veya olmasın tanımlamak için Gecikme Eğilimi ile ilişkilendirmek ve uygun eylemi gerçekleştirin.  

![ExpressRoute Monitor](media/log-analytics-network-performance-monitor/expressroute-peak-utilization.png)

 

## <a name="next-steps"></a>Sonraki adımlar
* [Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı ağ performansı veri kayıtları görüntülemek için.
