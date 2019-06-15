---
title: İçinde Azure Traffic Manager trafik görünümü
description: Traffic Manager trafik görünümü giriş
services: traffic-manager
documentationcenter: traffic-manager
author: asudbring
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: allensu
ms.custom: ''
ms.openlocfilehash: 5a34cf3e41e04367b1cf38015861518fb74dd3f7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67070942"
---
# <a name="traffic-manager-traffic-view"></a>Traffic Manager trafik görünümü

Traffic Manager, böylece son kullanıcılarınızın yönlendirme yöntemine dayalı sağlıklı Uç noktalara yönlendirilir, DNS düzeyinde yönlendirme ile profil oluştururken belirttiğiniz sunar. Trafik görünümü, Traffic Manager, kullanıcı tabanları (DNS Çözümleyicisi ayrıntı düzeyinde) ve bunların trafik desenini bir görünümle sağlar. Trafik görünümü etkinleştirdiğinizde, bu bilgiler ile eyleme dönüştürülebilir Öngörüler sağlamak için işlenir. 

Trafik görünümü kullanarak, şunları yapabilirsiniz:
- (bir yerel DNS Çözümleyicisi düzeyine kadar ayrıntılı), kullanıcı temellerine bulunduğu anlayın.
- trafik (DNS sorgularının Azure Traffic Manager tarafından işlenen olarak gözlemlenen) görüntüleme bu bölgelerden gelen.
- Bu kullanıcılar tarafından karşılaşılan temsili gecikme sürelerini ne olduğunu Öngörüler elde edin.
- belirli bir trafik düzenlerini uç noktalarına sahip olduğunuz Azure bölgeleri için derinlemesine inceleyin her kullanıcı tabanının. 

Örneğin, hangi bölgeleri çok sayıda trafiğine sahip ancak daha yüksek gecikme süreleriyle olumsuz anlamak için trafik görünümü kullanabilirsiniz. Ardından, böylece bu kullanıcıların daha düşük bir gecikme süresi deneyimi yeni Azure bölgelerine, Ayak izi genişletme planlamak için bu bilgileri kullanın.

## <a name="how-traffic-view-works"></a>Trafik görünümü nasıl çalışır?

Trafik görünümü, Traffic Manager'ın bu özelliği etkinleştirilmiş bir profili karşı son yedi gün içinde alınan gelen sorguların bakmak sağlayarak çalışır. Gelen sorgu bilgileri, kullanıcıların konumunu temsili olarak kullanılan DNS Çözümleyicisi kaynak IP'si trafik görünümü ayıklar. Bunlar ardından birlikte, Traffic Manager tarafından tutulan IP adreslerinin coğrafi bilgileri kullanarak kullanıcı temel bölgeler oluşturmak için bir DNS Çözümleyicisi düzeyine ayrıntılı gruplandırılır. Traffic Manager, Azure bölgeleri sorgu yönlendirildi ve kullanıcılar bu bölgelerden trafik akışını haritasıdır oluşturur sonra bakar.  
Sonraki adımda, Traffic Manager, bu bölgelerden kullanıcılar tarafından karşılaşılan ortalama gecikme sürelerini anlamak farklı son kullanıcı ağlar için tutar ağ zeka gecikme tablolar ile Azure bölgesi eşleme kullanıcı temel bölgeye karşılık gelen zaman Azure bölgeleri için bağlanılıyor. Tüm bu hesaplamalar, birleştirilen bir size görüntülenmeden önce yerel DNS Çözümleyicisi IP düzeyi başına. Çeşitli yollarla bilgileri kullanabilir.

Trafik görünümü verileri güncelleştirme sıklığını birden çok iç hizmet değişkene bağlıdır. Ancak, veriler genellikle 24 saatte bir kez güncelleştirilir.

>[!NOTE]
>Trafik Görünümü'nde açıklanan gecikme süresi, son kullanıcı ve bunların için bağlı Azure bölgeleri arasında temsili bir gecikme süresi ve DNS Arama gecikme değil. Trafik görünümü yaptığı yerel DNS Çözümleyicisi kullanılabilir yeterli veri yoksa, sorgu için yönlendirildi Azure bölgesi arasındaki gecikme süresini daha sonra gecikme süresini en iyi çaba tahmin döndürülen null olacaktır. 

## <a name="visual-overview"></a>Görsel bir genel bakış

Ne zaman ulaşmanıza **trafik görünümü** bölümü, Traffic Manager sayfasında, bir coğrafi harita trafik görünümü öngörü bir katman ile birlikte sunulur. Harita kullanıcı tabanı ve Traffic Manager profilinizin uç noktaları hakkında bilgi sağlar.

### <a name="user-base-information"></a>Kullanıcı temel bilgileri

İçin hangi konum bilgileri kullanılabilir bu yerel DNS Çözümleyicileri için bunlar haritada gösterilir. DNS Çözümleyicisi rengini ortalama gecikme süresi, Traffic Manager sorgularını için o DNS Çözümleyicisi kullanan son kullanıcılar tarafından deneyimli gösterir.

DNS Çözümleyicisi konumu haritada üzerine gelin, bunu gösterir:
- DNS Çözümleyicisi IP adresi
- DNS sorgu trafik Traffic Manager tarafından buradan görülen
- hangi trafiğe DNS'den Çözümleyici, uç nokta ve DNS Çözümleyicisi arasında bir çizgi olarak yönlendirildi uç noktaları 
- bunları bağlayan bir çizgi rengi temsil edilen uç noktasına, o konumdan ortalama gecikme süresi

### <a name="endpoint-information"></a>Uç nokta bilgileri

Uç noktaları bulunduğu Azure bölgesine mavi noktalar haritada olarak gösterilir. Uç noktanız dış ve eşlenmiş bir Azure bölgesi yoktur, haritanın en üstünde gösterilir. (Kullanılan DNS Çözümleyicisi üzerinde bağlı olarak) farklı konumları görmek için herhangi bir uç noktaya gelen trafik için bu endpoint burada yönlendirilmiş olan tıklayın. Bağlantıları uç nokta ve DNS Çözümleyicisi konumu arasında bir çizgi olarak gösterilir ve bu çifti arasındaki temsili gecikme sürelerini göre renklendirilmiş. Ayrıca, uç nokta, Azure bölgesi içinde çalıştığı ve toplam ona bu Traffic Manager profili tarafından yönlendirildiniz istek hacmi adını görebilirsiniz.


## <a name="tabular-listing-and-raw-data-download"></a>Tablosal ve ham veri yükleme

Azure portalında bir tablosal biçimde trafik görünümü verileri görüntüleyebilirsiniz. Her DNS Çözümleyicisi IP için bir giriş / uç nokta pair IP adresini DNS Çözümleyicisi, ad ve bir Azure bölgesinde coğrafi konumunu gösterir, uç noktayı (varsa) bulunur, bu DNS Çözümleyicisi için ilişkili istek hacmi uç noktanın ve temsili gecikme sürelerini (kullanılabiliyorsa), DNS kullanarak son kullanıcıların ile ilişkili. Trafik görünümü verileri, tercih ettiğiniz bir analiz iş akışının bir parçası olarak kullanılabilir bir CSV dosyası olarak da indirebilirsiniz.

## <a name="billing"></a>Faturalandırma

Trafik görünümü kullandığınızda, sunulan Öngörüler oluşturmak için kullanılan veri noktalarının sayısına göre faturalandırılır. Şu anda kullanılan tek veri noktası türü, Traffic Manager profilinizin karşı alınan sorgulardır. Fiyatlandırma hakkında ayrıntılı bilgi için ziyaret [Traffic Manager fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi [Traffic Manager nasıl çalışır?](traffic-manager-overview.md)
- Daha fazla bilgi edinin [trafik yönlendirme yöntemlerini](traffic-manager-routing-methods.md) Traffic Manager tarafından desteklenen
- Bilgi edinmek için nasıl [Traffic Manager profili oluşturma](traffic-manager-create-profile.md)

