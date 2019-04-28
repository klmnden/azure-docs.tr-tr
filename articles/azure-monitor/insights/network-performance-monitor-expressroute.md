---
title: Ağ Performansı İzleyicisi çözümü Azure Log analytics'te | Microsoft Docs
description: ExpressRoute İzleyicisi özelliği, uçtan uca bağlantıyı ve performansı arasında şubeleriniz ve Azure, Azure ExpressRoute üzerinden izlemek için Ağ Performansı İzleyicisi'nde kullanın.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: abshamsft
ms.openlocfilehash: d0819b57307fc037b3be6ab04ed9ec6c8720a618
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62110149"
---
# <a name="expressroute-monitor"></a>ExpressRoute İzleyicisi

Azure ExpressRoute izleme özelliği kullanabileceğiniz [Ağ Performansı İzleyicisi](network-performance-monitor.md) uçtan uca bağlantıyı ve performansı arasında şubeleriniz ve Azure, Azure ExpressRoute üzerinden izlemek için. Önemli avantajlar şunlardır: 

- Aboneliğinizle ilişkili İntellisense, ExpressRoute devreleri.
- Bant genişliği kullanımı, kayıp ve gecikme süresi devre, eşleme ve Azure sanal ağ düzeyinde ExpressRoute için izleme.
- ExpressRoute devrelerinizin ağ topolojisini bulma.

![ExpressRoute İzleyicisi](media/network-performance-monitor-expressroute/expressroute-intro.png)

## <a name="configuration"></a>Yapılandırma 
Yapılandırma için Ağ Performansı İzleyicisi'ni açmak için açık [Ağ Performansı İzleyicisi çözüm](network-performance-monitor.md) seçip **yapılandırma**.

### <a name="configure-network-security-group-rules"></a>Ağ güvenlik grubu kurallarını yapılandırma 
Ağ Performansı İzleyicisi izlemek için kullanılan sunucular için azure'da yapay işlemler için Ağ Performansı İzleyicisi tarafından kullanılan bağlantı noktası TCP trafiğine izin vermek için ağ güvenlik grubu (NSG) kurallarını yapılandırın. Varsayılan bağlantı noktası 8084 ' dir. Bu yapılandırma sağlayan bir şirket içi ile iletişim kurmak için Azure sanal makinelerinde yüklü Log Analytics aracısını İzleme Aracısı. 

Nsg'ler hakkında daha fazla bilgi için bkz. [ağ güvenlik grupları](../../virtual-network/manage-network-security-group.md). 

>[!NOTE]
> Bu adım ile devam etmeden önce şirket içi sunucu Aracısı'nı ve Azure sunucusu aracısını yükleyin ve EnableRules.ps1 PowerShell betiğini çalıştırın. 

 
### <a name="discover-expressroute-peering-connections"></a>ExpressRoute eşleme bağlantılarını keşfedin 
 
1. Seçin **ExpressRoute eşlemeleri** görünümü.
2. Seçin **Şimdi Bul** Azure aboneliğindeki sanal ağlara bağlı özel eşlemeler bu Azure Log Analytics çalışma alanıyla bağlantılı tüm ExpressRoute bulunacak.

    >[!NOTE]
    > Çözüm, şu anda yalnızca ExpressRoute özel eşlemesi bulur. 

    >[!NOTE]
    > Yalnızca bu Log Analytics çalışma alanıyla bağlantılı aboneliği ile ilişkili sanal ağlara bağlı özel eşlemeler bulunur. ExpressRoute, bu çalışma alanına bağlı abonelik dışında sanal ağlara bağlıysa, bu Aboneliklerde bir Log Analytics çalışma alanı oluşturun. Ardından bu eşlemeleri izlemek için Ağ Performansı İzleyicisi'ni kullanın. 

    ![ExpressRoute İzleyicisi yapılandırma](media/network-performance-monitor-expressroute/expressroute-configure.png)
 
   Bulma tamamlandıktan sonra bulunan özel eşleme bağlantılarını bir tabloda listelenmiştir. Bu eşlemeler için izleme, başlangıçta devre dışı durumda olan. 

### <a name="enable-monitoring-of-the-expressroute-peering-connections"></a>ExpressRoute eşleme bağlantıları izlemeyi etkinleştir 

1. İzlemek istediğiniz özel eşleme bağlantısını seçin.
2. Sağ taraftaki bölmede seçin **Bu eşleme izleme** onay kutusu. 
3. Bu bağlantı için sistem durumu olayları oluşturmak istiyorsanız seçin **etkinleştirme Bu eşleme için sistem durumu izleme**. 
4. İzleme koşulları seçin. İçin sistem durumu olayı oluşturma eşiği değerler girerek, özel eşikler ayarlayabilirsiniz. Eşleme bağlantısını için seçilen eşiğin üzerinde koşul değeri gider her sistem durumu olayı oluşturulur. 
5. Seçin **aracı Ekle** Bu eşleme bağlantısı izleme için kullanmayı düşündüğünüz izleme aracılarını seçmek için. Bağlantının her iki ucunda aracıları eklediğinizden emin olun. Bu eşleme için bağlı sanal ağ içinde en az bir aracı ihtiyacınız vardır. Ayrıca bu eşleme için bağlı en az bir şirket içi Aracısı gerekir. 
6. Seçin **Kaydet** yapılandırmayı kaydetmek için. 

   ![ExpressRoute izleme yapılandırması](media/network-performance-monitor-expressroute/expressroute-configure-discovery.png)


Kuralları ve değerlerini belirleyin ve aracıları etkinleştirdikten sonra değerleri doldurmak 30 ila 60 dakika bekleyin ve **ExpressRoute izleme** döşeme görünür. İzleme kutucukları gördüğünüzde, ExpressRoute bağlantı hatları ve bağlantı kaynakları artık Ağ Performansı İzleyicisi tarafından izlenir. 

>[!NOTE]
> Bu özellik, güvenilir bir şekilde yeni sorgu diline yükseltme yaptı çalışma alanları üzerinde çalışır.

## <a name="walkthrough"></a>Kılavuz 

Ağ Performansı İzleyicisi panoyu ExpressRoute bağlantı hatları ve eşleme bağlantılarını sistem durumu özetini gösterir. 

![Ağ performansı izleme Panosu](media/network-performance-monitor-expressroute/npm-dashboard-expressroute.png) 

### <a name="circuits-list"></a>Bağlantı hatları listesi 

Tüm izlenen ExpressRoute devreleri listesini görmek için ExpressRoute devreleri kutucuğu seçin. Bağlantı hattı seçin ve görüntüleme, sistem durumu, paket kaybı, bant genişliği kullanımı ve gecikme süresi eğilim grafikleri. Grafik etkileşimlidir. Bir özel zaman penceresi, grafik çizim için seçebilirsiniz. Fare, yakınlaştırma ve ayrıntılı veri noktaları görmek için grafikteki bir alana sürükleyin. 

![ExpressRoute bağlantı hatları listesi](media/network-performance-monitor-expressroute/expressroute-circuits.png) 

### <a name="trends-of-loss-latency-and-throughput"></a>Eğilimleri kaybı, gecikme süresi ve aktarım hızı 

Bant genişliği kullanımı, gecikme süresi ve zarar grafik etkileşimlidir. Fare denetimlerini kullanarak bu grafikler herhangi bir bölümünü yakınlaştırabilirsiniz. Bant genişliği, gecikme süresi ve veri kaybı için diğer aralıkları görebilirsiniz. Sol üst köşedeki altında **eylemleri** düğmesini seçme **tarih/saat**. 

![ExpressRoute gecikme süresi](media/network-performance-monitor-expressroute/expressroute-latency.png) 

### <a name="peerings-list"></a>Eşlemeleri listesi 

Özel eşdüzey hizmet sağlama üzerinden sanal ağlara tüm bağlantılar listesini açmak için seçin **özel eşlemeler** Panoda kutucuk. Burada, bir sanal seçebilirsiniz, sistem durumu, paket kaybı, bant genişliği kullanımı ve gecikme süresi eğilim grafikleri görüntülemek ve ağ bağlantısı. 

![ExpressRoute eşlemeleri](media/network-performance-monitor-expressroute/expressroute-peerings.png) 

### <a name="circuit-topology"></a>Bağlantı hattı topolojisi 

Bağlantı hattı topolojiyi görüntülemek için seçin **topolojisi** Döşe. Bu eylem seçili topoloji görünümü için gereken bağlantı hattı veya eşleme. Ağdaki gecikme süresi her segment için topoloji diyagramı sağlar ve her katman 3 atlama diyagramın bir düğümle temsil edilir. Bir atlama seçerek atlama hakkında daha fazla ayrıntı ortaya çıkarır. Şirket içi atlama içerecek şekilde görünürlük düzeyini artırmak için altında kaydırıcı çubuğunu taşımak **filtreleri**. Kaydırıcı çubuğunu taşımak için sol veya sağ artırır veya topoloji graftaki durak sayısını azaltır. Her bir kesim arasında gecikme, Yüksek gecikmeli parçaların ağınızdaki daha hızlı yalıtım sağlayan görünür durumda.

![ExpressRoute topolojisi](media/network-performance-monitor-expressroute/expressroute-topology.png)

### <a name="detailed-topology-view-of-a-circuit"></a>Bir bağlantı hattının ayrıntılı topoloji görünümü 

Bu görünüm, sanal ağ bağlantılarını gösterir. 

![ExpressRoute sanal ağ bağlantıları](media/network-performance-monitor-expressroute/expressroute-vnet.png)
 
## <a name="diagnostics"></a>Tanılama 

Ağ Performansı İzleyicisi birkaç bağlantı hattı bağlantı sorunları tanılamanıza yardımcı olur. Gördüğünüz sorunlardan bazıları aşağıda listelenmiştir.

Bildirim kodları görmek ve bunlar üzerinde uyarılar ayarlayın **LogAnalytics**. Üzerinde **NPM tanılama** sayfası açıklamaları için tetiklenen her bir tanılama iletisi görebilirsiniz.

| Bildirim kodu (günlük) | Açıklama |
| --- | --- |
| 5501 | ExpressRoute devresinin ikincil bağlantısı üzerinden geçiş yapılamıyor |
| 5502 | ExpressRoute devresinin birincil bağlantısı üzerinden geçiş yapılamıyor |
| 5503 | Çalışma alanına bağlı abonelik için devre bulunmadı | 
| 5508 | Yolu için herhangi bir devreden üzerinden trafiği geçirerek olup olmadığını belirlemek kullanabilirsiniz |
| 5510 | Trafik hedeflenen devreden geçmiyor | 
| 5511 | Trafik, hedeflenen sanal ağ üzerinden geçmiyor | 

**Bağlantı hattı çalışmıyor.** Şirket içi kaynaklar ve Azure sanal ağları arasında bağlantı kaybedilirse hemen sonra Ağ Performansı İzleyicisi size bildirir. Bu bildirim yardımcı olan kullanıcı Yardım istekleri almak ve kapalı kalma süresinin azaltılmasına önce proaktif harekete.

![ExpressRoute bağlantı hattı çalışmıyor](media/network-performance-monitor-expressroute/expressroute-circuit-down.png)
 

**Trafiğin hedeflenen devre üzerinden akan değil.** Hedeflenen ExpressRoute bağlantı hattı trafik akışına değil her ağ performansı İzleyicisi size bildirir. Bağlantı hattı çalışmıyor ve trafik, yedekleme rotayı akışına Bu sorun oluşabilir. Yönlendirme bir sorun olduğunda da gerçekleşebilir. Bu bilgiler, yönlendirme ilkelerinizi yapılandırma sorunları proaktif bir şekilde yönetip en iyi ve en güvenli yolu kullanıldığından emin olun yardımcı olur. 

 

**Birincil bağlantı hattı akan değil trafiği.** Ağ Performansı İzleyicisi trafik, ikincil ExpressRoute bağlantı hattı akan olduğunda size bildirir. Herhangi bir bağlantı sorunları yaşadıklarında olmaz, bu durumda, proaktif olarak rağmen birincil bağlantı hattını sorunlarını giderme, daha iyi hazır hale getirir. 

 
![ExpressRoute trafik akışı](media/network-performance-monitor-expressroute/expressroute-traffic-flow.png)


**En yüksek kullanımı nedeniyle performans düşüşü.** Azure iş yükü performans düşüşü bir en yüksek bant genişliği kullanımı nedeniyle olup olmadığını belirlemek için gecikmesi eğilimi ile bant genişliği kullanım eğilimi ilişkilendirebilirsiniz. Ardından uygun eylemi gerçekleştirebilir.

![ExpressRoute bant genişliği kullanımı](media/network-performance-monitor-expressroute/expressroute-peak-utilization.png)

 

## <a name="next-steps"></a>Sonraki adımlar
[Arama günlüklerini](../../azure-monitor/log-query/log-query-overview.md) ayrıntılı ağ performansı verileri kayıtları görüntülemek için.
