---
title: Ağ Azure günlük analizi Performans İzleyicisi çözümde | Microsoft Docs
description: ExpressRoute Manager özelliği ağ Performans İzleyicisi'nde Azure ExpressRoute uçtan uca bağlantısı ve şube ofislerinde ve Azure arasında performansı izlemek için kullanın.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 9610a8b37ead976cfdfa2fed81d4d3932055ddcc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="expressroute-manager"></a>ExpressRoute Manager

Azure ExpressRoute Yöneticisi özelliği kullanabileceğiniz [Ağ Performansı İzleyicisi](log-analytics-network-performance-monitor.md) Azure ExpressRoute uçtan uca bağlantısı ve şube ofislerinde ve Azure arasında performansı izlemek için. Anahtar avantajları şunlardır: 

- Aboneliğinizle ilişkili algılama, expressroute bağlantı.
- Bant genişliği kullanımı, kaybı ve gecikme hattı, eşleme ve Azure sanal ağ düzeyinde ExpressRoute için izleme.
- ExpressRoute bağlantı hatları, ağ topolojisi bulma.

![ExpressRoute Monitor](media/log-analytics-network-performance-monitor/expressroute-intro.png)

## <a name="configuration"></a>Yapılandırma 
Ağ Performansı İzleyicisi Yapılandırması'nı açmak için açın [Ağ Performansı İzleyicisi çözüm](log-analytics-network-performance-monitor.md) seçip **yapılandırma**.

### <a name="configure-network-security-group-rules"></a>Ağ güvenlik grubu kurallarını yapılandırma 
Ağ Performansı İzleyicisi İzleme için kullanılan sunucular için azure'da, ağ güvenlik grubu (NSG) kuralları yapay işlemler için Ağ Performansı İzleyicisi tarafından kullanılan bağlantı noktası TCP trafiğine izin verecek şekilde yapılandırın. Varsayılan bağlantı noktası 8084 ' dir. Bu yapılandırma bir şirket içi ile iletişim kurmak için Azure vm'lerinde yüklü Operations Management Suite aracısının sağlar İzleme Aracısı. 

NSG hakkında daha fazla bilgi için bkz: [ağ güvenlik grupları](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

>[!NOTE]
> Bu adımla devam etmeden önce şirket içi sunucu aracısı ve Azure server aracısı yükleyin ve EnableRules.ps1 PowerShell betiğini çalıştırın. 

 
### <a name="discover-expressroute-peering-connections"></a>ExpressRoute eşleme bağlantıları Bul 
 
1. Seçin **ExpressRoute eşlemeler** görünümü.
2. Seçin **Şimdi Bul** Azure Abonelikteki sanal ağlara bağlanan özel eşlemeler bu Azure günlük analizi çalışma alanı ile bağlantılı tüm ExpressRoute bulmak için.

    >[!NOTE]
    > Çözüm şu anda yalnızca ExpressRoute özel eşlemeler bulur. 

    >[!NOTE]
    > Yalnızca bu günlük analizi çalışma alanı ile bağlantılı abonelikle ilişkili sanal ağlara bağlı özel eşlemeler bulunur. ExpressRoute, bu çalışma alanına bağlı abonelik dışında sanal ağlara bağlıysa, bu Aboneliklerde günlük analizi çalışma alanı oluşturun. Ardından bu eşlemeler izlemek için ağ Performans İzleyicisi'ni kullanın. 

    ![ExpressRoute İzleyicisi'ni yapılandırma](media/log-analytics-network-performance-monitor/expressroute-configure.png)
 
 Bulma tamamlandıktan sonra bulunan özel eşliği bağlantılar bir tabloda listelenmiştir. Bu eşlemeler için izleme başlangıçta bir devre dışı bırakılmış durumda. 

### <a name="enable-monitoring-of-the-expressroute-peering-connections"></a>ExpressRoute eşleme bağlantıları izlemeyi etkinleştir 

1. İzlemek istediğiniz özel eşleme bağlantı seçin.
2. Sağ taraftaki bölmede seçin **bu eşlemesi izlemek** onay kutusu. 
3. Bu bağlantı için sistem durumu olayları oluşturmak istiyorsanız, seçin **etkinleştir Bu eşleme için sistem durumu izleme**. 
4. İzleme koşulları seçin. Eşik değerleri girerek sistem olay oluşturma için özel eşiklerini ayarlayabilirsiniz. Eşleme bağlantı için seçilen eşiğin üstünde koşul değerini gider olduğunda, bir sistem durumu olayı oluşturulur. 
5. Seçin **eklemek aracıları** Bu eşleme bağlantı izleme için kullanmayı düşündüğünüz izleme aracıları seçin. Bağlantının her iki ucunun aracıları eklediğinizden emin olun. Bu eşleme için bağlı sanal ağ içinde en az bir aracı gerekir. Bu eşleme için bağlı en az bir şirket içi aracı da gerekir. 
6. Seçin **kaydetmek** yapılandırmayı kaydetmek için. 

   ![ExpressRoute izleme yapılandırması](media/log-analytics-network-performance-monitor/expressroute-configure-discovery.png)


Kuralları ve değerlerini belirleyin ve aracıları etkinleştirdikten sonra doldurmak için değerlerini 30-60 dakika bekleyin ve **ExpressRoute izleme** döşeme görünür. İzleme döşeme gördüğünüzde, ExpressRoute bağlantı hatları ve bağlantı kaynakları artık ağ Performans İzleyicisi tarafından izlenir. 

>[!NOTE]
> Bu özelliği yeni sorgu dili yükselttikten çalışma alanları güvenilir bir şekilde çalışır.

## <a name="walkthrough"></a>Kılavuz 

Ağ Performansı İzleyicisi Panosu ExpressRoute bağlantı hatları ve eşleme bağlantıları sistem durumu özetini gösterir. 

![Ağ Performans İzleyicisi Panosu](media/log-analytics-network-performance-monitor/npm-dashboard-expressroute.png) 

### <a name="circuits-list"></a>Bağlantı hatları listesi 

Tüm izlenen ExpressRoute bağlantı hatları listesini görmek için ExpressRoute bağlantı hatları döşeme seçin. Bir bağlantı hattı seçin ve sistem durumu, paket kaybı, bant genişliği kullanımı ve gecikme eğilim grafikleri görüntüleyin. Grafikler etkileşimlidir. Grafikler çizdirmek için bir özel zaman penceresi seçebilirsiniz. Fare yakınlaştırmak ve hassas veri noktaları görmek için grafikte bir alana sürükleyin. 

![ExpressRoute bağlantı hatları listesi](media/log-analytics-network-performance-monitor/expressroute-circuits.png) 

### <a name="trends-of-loss-latency-and-throughput"></a>Eğilimleri kaybı, gecikme ve verimlilik 

Bant genişliği kullanımı, gecikme ve kaybı grafikleri etkileşimlidir. Fare denetimlerini kullanarak bu grafikler herhangi bir bölümünü yakınlaştırabilirsiniz. Diğer aralıkları için bant genişliği, gecikme ve veri kaybı de görebilirsiniz. Sol üst köşedeki altında **Eylemler** düğmesi, select **tarih/saat**. 

![ExpressRoute gecikme süresi](media/log-analytics-network-performance-monitor/expressroute-latency.png) 

### <a name="peerings-list"></a>Eşlemeleri listesi 

Özel eşleme üzerinden sanal ağlara tüm bağlantılar listesini getirmek için seçin **özel eşlemeler** döşeme Panoda. Burada, bir sanal seçebilirsiniz ağ bağlantısı ve sistem durumu, paket kaybı, bant genişliği kullanımı ve gecikme eğilim grafikleri görüntüleyin. 

![ExpressRoute eşlemeleri](media/log-analytics-network-performance-monitor/expressroute-peerings.png) 

### <a name="circuit-topology"></a>Bağlantı hattı topolojisi 

Bağlantı hattı topolojisini görüntülemek için seçin **topoloji** döşeme. Seçili topoloji görünümü bu eylemde hattı veya eşliği. Topoloji diyagramı ağdaki her segment için gecikme süresini sağlar ve her katman 3 atlama diyagramın bir düğümle temsil edilir. Bir atlama seçerek atlama hakkında daha fazla ayrıntı ortaya çıkarır. Şirket içi atlama dahil etmek için görünürlük düzeyini artırmak için altında çubuğu kaydırın **filtreleri**. Kaydırıcı çubuğun sola veya sağa artışları taşıma veya topoloji grafikte durak sayısını azaltır. Her segment arasında gecikme Yüksek gecikmeli kesimleri ağınızdaki daha hızlı yalıtım sağlayan, görülebilir. 

![ExpressRoute topolojisi](media/log-analytics-network-performance-monitor/expressroute-topology.png)

### <a name="detailed-topology-view-of-a-circuit"></a>Bir bağlantı hattının ayrıntılı topoloji görünümü 

Bu görünüm, sanal ağ bağlantıları gösterir. 

![ExpressRoute sanal ağ bağlantıları](media/log-analytics-network-performance-monitor/expressroute-vnet.png)
 

### <a name="diagnostics"></a>Tanılama 

Ağ Performansı İzleyicisi birkaç hattı bağlantı sorunları tanılamanıza yardımcı olur. Bazı sorunlar burada listelenir. 

**Bağlantı hattı çalışmıyor.** Şirket içi kaynakları ve Azure sanal ağlar arasında bağlantı kaybolur hemen Ağ Performansı İzleyicisi size bildirir. Bu bildirim yardımcı olan kullanıcı çözümler almak ve kapalı kalma süresini kısaltabilir önce öngörülü adımları uygulayın.

![Expressroute bağlantı hattı çalışmıyor](media/log-analytics-network-performance-monitor/expressroute-circuit-down.png)
 

**Trafik hedeflenen bağlantı hattı akan değil.** Trafik hedeflenen expressroute bağlantı hattı akan değil her ağ performansı İzleyicisi size bildirir. Bağlantı hattı çalışmıyor ve yedekleme yönlendir trafiği akan Bu sorun oluşabilir. Yönlendirme bir sorun olduğunda da gerçekleşebilir. Bu bilgiler proaktif olarak Yönlendirme ilkelerinizi herhangi bir yapılandırma sorunu yönetmenize ve en iyi ve güvenli rota kullanıldığından emin olun yardımcı olur. 

 

**Trafik birincil bağlantı hattı akan değil.** İkincil expressroute bağlantı hattı trafiği akan, Ağ Performansı İzleyicisi bunu size bildirir. Hiçbir bağlantı sorunları yaşıyorsa olmaz olsa bile bu durumda, ileriye dönük olarak birincil bağlantı hattı sorunlarını giderme, daha iyi hazır hale getirir. 

 
![ExpressRoute trafik akışı](media/log-analytics-network-performance-monitor/expressroute-traffic-flow.png)


**En yüksek kullanımı düşüşünü.** Bant genişliği kullanım eğilimi Azure iş yükü düşüşünü en yüksek bant genişliği kullanımı nedeniyle olsun veya olmasın tanımlamak için Gecikme Eğilimi ile ilişkilendirebilirsiniz. Ardından, uygun şekilde önlem alabilirsiniz.

![ExpressRoute bant genişliği kullanımı](media/log-analytics-network-performance-monitor/expressroute-peak-utilization.png)

 

## <a name="next-steps"></a>Sonraki adımlar
[Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı ağ performansı veri kayıtları görüntülemek için.
