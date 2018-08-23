---
title: Performansla ilgili önemli noktalar için Azure Traffic Manager | Microsoft Docs
description: Traffic Manager ve Traffic Manager kullanırken sitenizin performansını test etme performansını anlama
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
ms.openlocfilehash: 43bb407730594498cfe9c78810c4e9dfb17e4af4
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42056086"
---
# <a name="performance-considerations-for-traffic-manager"></a>Traffic Manager için performans konuları

Bu sayfa, Traffic Manager'ı kullanarak performans konuları açıklanmaktadır. Aşağıdaki senaryoyu göz önünde bulundurun:

WestUS ve ping'in ekran bölgelerinde sitenizin örnekleri var. Örneklerden birini traffic manager araştırması için sistem durumu denetimi başarısız oluyor. Uygulama trafiği sağlıklı bölgeye yönlendirilir. Bu yük devretme bekleniyordu, ancak performans şimdi uzak bir bölgeye seyahat trafiğinin gecikme süresini dayalı bir sorun olabilir.

## <a name="performance-considerations-for-traffic-manager"></a>Traffic Manager için performans konuları

Traffic Manager sitenizde sahip olabilecek tek performans ilk DNS araması etkisidir. Traffic Manager profilinizin adı için bir DNS istek trafficmanager.net bölgeyi barındıran Microsoft DNS kök sunucu tarafından işlenir. Traffic Manager, doldurur ve düzenli olarak güncelleştirmeleri, Microsoft'un DNS kök sunucuları Traffic Manager ilkesini ve araştırma sonuçları göre. Çağrılarda bile ilk DNS araması sırasında Traffic Manager DNS sorgu gönderilir.

Traffic Manager oluşur çeşitli bileşenlerinin: DNS ad sunucuları, bir API hizmeti, depolama katmanı ve bir uç nokta izleme hizmeti. Traffic Manager hizmeti bileşeni başarısız olursa, Traffic Manager profili ile ilişkili DNS adı üzerinde hiçbir etkisi yoktur. Microsoft DNS sunucuları kayıtlara değişmeden kalır. Ancak, uç nokta izleme ve DNS güncelleştirme durum değil. Bu nedenle, Traffic Manager DNS birincil arızalandığında yük devretme sitenize işaret edecek şekilde güncelleştirmek mümkün değil.

DNS ad çözümlemesi hızlı çalışır ve sonuçları önbelleğe alınır. İstemcinin ad çözümlemesi için kullandığı DNS sunucularını ilk DNS araması hızına bağlıdır. Genellikle, bir istemci bir DNS araması yaklaşık 50 ms içinde tamamlayabilirsiniz. Arama sonuçlarının DNS için-yaşam süresi (TTL) boyunca önbelleğe alınır. Varsayılan TTL Traffic Manager için 300 saniyedir.

Trafik Traffic Manager üzerinden geçmez. DNS arama işlemi tamamlandıktan sonra istemci, web sitenizin bir örneği için bir IP adresi vardır. İstemci o adrese doğrudan bağlanır ve Traffic Manager üzerinden geçmez. Seçtiğiniz Traffic Manager ilkesini DNS performans üzerindeki etkisi yoktur. Ancak, bir performans yönlendirme yöntemi uygulama deneyimi olumsuz yönde etkileyebilir. Örneğin, ilkeniz Asya'da barındırılan örneği Kuzey Amerika giden trafiği yönlendirir, bu oturumlar için ağ gecikme süresi bir performans sorunu olabilir.

## <a name="measuring-traffic-manager-performance"></a>Traffic Manager performansını ölçme

Performans ve Traffic Manager profili davranışını anlamak için kullanabileceğiniz birkaç Web sitesini vardır. Çoğu bu sitelerden ücretsizdir ancak kısıtlamaları olabilir. Bazı siteler Gelişmiş izleme ve raporlama bir ücret karşılığında sunar.

Bu siteler ölçü DNS gecikme süreleri ve görüntüleme araçları, istemci konumlarının dünyanın dört bir yanındaki çözümlenen IP adresleri. Bu araçların çoğu, DNS sonuçları önbelleğe alma işlemi. Bu nedenle, araçları, tam DNS araması bir test her çalıştığında gösterir. Kendi istemciden test ettiğinizde, yalnızca bir kez TTL süresi sırasında tam DNS Arama performansını karşılaşırsınız.

## <a name="sample-tools-to-measure-dns-performance"></a>DNS performansını ölçmek için örnek araçları

* [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS birçok performans araçları sunar. DNS karşılaştırma aracı, DNS adını çözümlemek için ne kadar sürer ve nasıl, diğer DNS hizmeti sağlayıcıları için karşılaştırır gösterebilirsiniz.

* [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    En basit Araçlar WebSitePulse biridir. DNS çözümleme süresi, ilk baytı, son bayt ve diğer performans istatistikleri görmek için URL'yi girin. Üç farklı test konumundan seçebilirsiniz. Bu örnekte, ilk yürütme DNS araması 0.204 sn aldığını gösterir bakın.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Sonuçları önbelleğe alındığı için ikinci test aynı Traffic Manager uç noktası için DNS araması 0.002 saniye sürer.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

* [CA uygulama Sentetik izleme](https://asm.ca.com/en/checkit.php)

    Daha önce Watchmouse onay Web sitesi aracı olarak bilinen, bu site Göster, DNS çözümleme süresi birden fazla coğrafi bölgeyi aynı anda. DNS çözümleme süresi, bağlantı süresi ve farklı coğrafi konumlardan hızından görmek için URL'yi girin. Bu test, dünyanın dört bir yanındaki farklı konumlar için hangi barındırılan hizmet döndürülen görmek için kullanın.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

* [Pingdom](http://tools.pingdom.com/)

    Bu araç, bir web sayfasının her öğe için performans istatistiklerini sağlar. Analiz sayfası sekmesi DNS Arama harcadığı zaman yüzdesini gösterir.

* [My DNS nedir?](http://www.whatsmydns.net/)

    Bu site, 20 farklı konumlardan DNS arama yapar ve sonuçları bir haritada görüntüler.

* [Web arabirimi inin](http://www.digwebinterface.com)

    Bu site, daha ayrıntılı CNAME ve A kayıtları dahil olmak üzere DNS bilgi gösterir. 'Renklendir'i output' ve 'İstatistikler' seçenekleri altında denetleyin ve 'All' altında bir ad seçin emin olun.

## <a name="next-steps"></a>Sonraki Adımlar

[Traffic Manager trafik yönlendirme yöntemleri hakkında](traffic-manager-routing-methods.md)

[Traffic Manager ayarlarınızı test etme](traffic-manager-testing-settings.md)

[Traffic Manager üzerindeki işlemler (REST API Başvurusu)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager)

