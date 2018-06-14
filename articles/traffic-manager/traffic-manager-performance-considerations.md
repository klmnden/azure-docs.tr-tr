---
title: Başarım Değerlendirmeleri için Azure Traffic Manager | Microsoft Docs
description: Trafik Yöneticisi ve trafik Yöneticisi'ni kullanırken sitenizin performansını test etme performansına anlama
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: timlt
editor: ''
ms.assetid: 3ba5dfa1-2922-43f1-9a23-d06969c4a516
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: f686685138625a53971f1fc5fc754fd22c9d67b2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23876393"
---
# <a name="performance-considerations-for-traffic-manager"></a>Traffic Manager için performans konuları

Bu sayfayı trafik Yöneticisi'ni kullanarak performans konuları açıklanmaktadır. Aşağıdaki senaryoyu göz önünde bulundurun:

Web sitenizi örnekleri WestUS ve EastAsia bölgelerde sahip. Örneklerden birini trafik Yöneticisi araştırma için sistem durumu denetimi başarısız oluyor. Uygulama trafiğini sağlıklı bölgesine yönlendirilir. Bu yük devretme bekleniyordu ancak performans şimdi uzaktaki bir bölgeye seyahat trafiği gecikme dayalı bir sorun olabilir.

## <a name="performance-considerations-for-traffic-manager"></a>Traffic Manager için performans konuları

Trafik Yöneticisi, Web sitenizde sahip olabileceği yalnızca performans ilk DNS araması etkisidir. Trafik Yöneticisi profilinin adı için bir DNS istek trafficmanager.net bölgeyi barındıran Microsoft DNS kök sunucusu tarafından işlenir. Trafik Yöneticisi doldurur ve düzenli olarak güncelleştirmeleri, Microsoft'un DNS kök sunucuları trafik Yöneticisi ilkesini ve yoklama sonuçları göre. Bu nedenle bile ilk DNS araması sırasında hiçbir DNS sorgularını trafik Yöneticisi için gönderilir.

Trafik Yöneticisi oluşur çeşitli bileşenleri: DNS sunucuları, bir API hizmeti, depolama katmanı ve izleme hizmeti bir uç nokta adı. Trafik Yöneticisi hizmet bileşenini başarısız olursa, trafik Yöneticisi profili ile ilişkili DNS adını üzerinde hiçbir etkisi yoktur. Microsoft DNS sunucuları kayıtları değişmeden kalır. Ancak, uç nokta izleme ve DNS güncelleştirme durum değil. Bu nedenle, trafik Yöneticisi DNS birincil sitenizi azaldığında yük devretme sitenize işaret edecek şekilde güncelleştirme mümkün değil.

DNS ad çözümlemesi hızlıdır ve sonuçları önbelleğe alınır. Ad çözümlemesi için istemcinin kullandığı DNS sunucularını ilk DNS araması hızına bağlıdır. Genellikle, istemci DNS araması ~ 50 ms içinde tamamlayabilirsiniz. Arama sonuçlarını DNS için-yaşam süresi (TTL) boyunca önbelleğe alınır. Varsayılan TTL trafik Yöneticisi için 300 saniyedir.

Trafiğin, trafik Yöneticisi ile geçmez. DNS arama işlemi tamamlandıktan sonra istemcinin, web sitenizin bir örneği için bir IP adresi olur. İstemci bu adrese doğrudan bağlanır ve trafik Yöneticisi ile iletmez. Seçtiğiniz trafik Yöneticisi ilkesini DNS performans üzerindeki etkisi yoktur. Ancak, bir performans yönlendirme yöntemi uygulama deneyimi olumsuz yönde etkileyebilir. Örneğin, Kuzey Amerika trafiğinden Asya'da barındırılan örneğine ilkeniz yönlendirir, bu oturum için ağ gecikmesi bir performans sorunu olabilir.

## <a name="measuring-traffic-manager-performance"></a>Trafik Yöneticisi performansı ölçme

Performans ve trafik Yöneticisi profili davranışını anlamak için kullanabileceğiniz çeşitli Web siteleri vardır. Bu sitelere çoğunu ücretsiz ancak sınırlamaları olabilir. Bazı siteler Gelişmiş izleme ve raporlama için ücret sunar.

Bu siteler ölçü DNS gecikmeleri ve görüntüleme araçları dünyanın istemci konumları için çözümlenen IP adresleri. Bu araçların çoğu DNS sonuçları önbelleğe almaz. Bu nedenle, araçları tam DNS araması test her çalıştırıldığında gösterir. Kendi istemciden test ettiğinizde, yalnızca tam DNS arama performansı kez TTL süresi sırasında karşılaşırsınız.

## <a name="sample-tools-to-measure-dns-performance"></a>DNS performansını ölçmek için örnek araçları

* [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS birçok performans araçları sunar. DNS karşılaştırma aracı, DNS adını çözümlemek için gereken süreyi ve, diğer DNS hizmeti sağlayıcıları için nasıl karşılaştırır gösterebilir.

* [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    En basit araçları WebSitePulse biridir. DNS çözümleme süresi, ilk bayta kalan, son bayta kalan ve diğer performans istatistiklerini görmek için URL'yi girin. Üç farklı bir test konumlardan seçebilirsiniz. Bu örnekte, ilk yürütme DNS araması 0.204 sn aldığını gösterir bakın.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Aynı trafik Yöneticisi uç noktası için ikinci test sonuçları önbelleğe alındığı için DNS araması 0.002 sn alır.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

* [CA'ın uygulama Sentetik İzleyicisi](https://asm.ca.com/en/checkit.php)

    Önceden Watchmouse onay Web sitesi aracı olarak bilinen, bu site Göster, DNS çözümleme süresi birden çok coğrafi bölgelerden aynı anda. DNS çözümleme süresi, bağlantı süresi ve birkaç coğrafi konumlar hızından görmek için URL'yi girin. Bu test, dünyanın dört farklı konumlar için hangi barındırılan hizmet döndürülen görmek için kullanın.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

* [Pingdom](http://tools.pingdom.com/)

    Bu araç, bir web sayfasının her öğe için performans istatistikleri sağlar. Sayfa analiz sekmesi DNS araması yapılmasını harcanan süre yüzdesini gösterir.

* [My DNS nedir?](http://www.whatsmydns.net/)

    Bu site 20 farklı konumlardan DNS araması yapar ve sonuçları bir haritada görüntüler.

* [Web arabirimi sorgulaması](http://www.digwebinterface.com)

    Bu site, daha ayrıntılı CNAME ve A kayıtları dahil olmak üzere DNS bilgi gösterir. 'Renklendir'i output' ve 'İstatistiği' seçenekleri altında denetleyin ve 'All' altında Nameservers seçin emin olun.

## <a name="next-steps"></a>Sonraki Adımlar

[Traffic Manager trafik yönlendirme yöntemleri hakkında](traffic-manager-routing-methods.md)

[Trafik Yöneticisi ayarlarınızı sınama](traffic-manager-testing-settings.md)

[Traffic Manager üzerindeki işlemler (REST API Başvurusu)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager cmdlet'leri](http://go.microsoft.com/fwlink/p/?LinkId=400769)

