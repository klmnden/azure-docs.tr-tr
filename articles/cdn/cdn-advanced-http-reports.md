---
title: "Azure CDN Gelişmiş HTTP raporları kullanım istatistiklerini çözümleme | Microsoft Docs"
description: "Microsoft Azure CDN'de Gelişmiş HTTP raporları oluşturmayı öğrenin. Bu raporlar CDN etkinliği hakkında ayrıntılı bilgi sağlar."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ef90adc1-580e-4955-8ff1-bde3f3cafc5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 2dfbc046674b2da692f30c945aee3ea25ae524eb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-usage-statistics-with-azure-cdn-advanced-http-reports"></a>Azure CDN Gelişmiş HTTP raporları kullanım istatistiklerini Çözümle
## <a name="overview"></a>Genel Bakış
Bu belgede, Microsoft Azure CDN'de Gelişmiş HTTP raporlama açıklanmaktadır. Bu raporlar CDN etkinliği hakkında ayrıntılı bilgi sağlar.

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Gelişmiş HTTP raporları erişme
1. CDN profili dikey penceresinden tıklayın **Yönet** düğmesi.
   
    ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-advanced-http-reports/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
2. Üzerine gelerek **Analytics** sekmesini ve ardından üzerine gelerek **Gelişmiş HTTP raporları** çıkma.  Tıklayın **HTTP büyük Platform**.
   
    ![CDN Yönetim Portalı - Gelişmiş Raporlar menüsü](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)
   
    Rapor seçeneklerini görüntülenir.

## <a name="geography-reports-map-based"></a>Coğrafya raporları (harita tabanlı)
İçeriğinizi istenmektedir bölgeler belirtmek için bir harita yararlanmak beş rapor vardır. Bu raporlar, dünya harita, Amerika Birleşik Devletleri harita, Kanada harita, Avrupa Harita ve Asya Pasifik harita içindir.

Her eşleme tabanlı rapor o bölgesinden coğrafi varlıkları (yani, Ülkeler, durumları ve İl) kaynaklanan isabet yüzdesi göre derecelendirir. Ayrıca, bir harita içeriğinizi istenmektedir konumları görselleştirmenize yardımcı olmak için sağlanır. İsteğe bağlı bu bölgede deneyimli miktarına göre her bir bölge renk kodlama bunu yapabiliyor. İsteğe bağlı içeriğiniz için yüksek düzeyde koyu bölgeler belirtmek sırada daha hafif gölgeli bölgeler, içerik için daha düşük talep gösterir.

Her bölge için ayrıntılı trafiği ve bant genişliği bilgileri doğrudan eşleme verilmiştir. Bu, isabet, isabet yüzdesi, toplam veri miktarını toplam sayısını görüntülemek için (gigabayt cinsinden) aktarılan ve her bölge için veri yüzdesi aktarılan sağlar. Bir açıklama her bu ölçümleri görüntüleyin. Bir bölge (yani, ülke, eyalet veya bölge) geldiğinizde, son olarak, ad ve bölgede oluştu isabet yüzdesi araç ipucu olarak görüntülenir.

Kısa bir açıklama harita tabanlı Coğrafya rapor her tür için aşağıda verilmiştir.

| Rapor adı | Açıklama |
| --- | --- |
| Dünya Haritası |Bu rapor, CDN içeriğine yönelik tüm dünyadaki talep görüntülemenizi sağlar. Her ülkenin bu bölgesinden kaynaklanan isabet yüzdesi belirtmek için world harita üzerinde kodludur. |
| Amerika Birleşik Devletleri eşleme |Bu rapor, Amerika Birleşik Devletleri'nde CDN içeriğiniz için isteğe bağlı görüntülemenizi sağlar. Her bir durumu, bölgesinden kaynaklanan isabet yüzdesi belirtmek için bu haritaya renkli kodlu. |
| Kanada eşleme |Bu rapor, CDN içeriğiniz için isteğe bağlı Kanada'da görüntülemenizi sağlar. Her bölge, bölgesinden kaynaklanan isabet yüzdesi belirtmek için bu haritaya renkli kodlu. |
| Avrupa eşleme |Bu rapor, CDN içeriğiniz için isteğe bağlı Avrupa'da görüntülemenizi sağlar. Her ülkenin bu bölgesinden kaynaklanan isabet yüzdesi belirtmek için bu haritaya renkli kodlu. |
| Asya Pasifik eşleme |Bu rapor, CDN içeriğiniz için isteğe bağlı Asya'da görüntülemenizi sağlar. Her ülkenin bu bölgesinden kaynaklanan isabet yüzdesi belirtmek için bu haritaya renkli kodlu. |

## <a name="geography-reports-bar-charts"></a>Coğrafya raporları (çubuk grafikler)
Olan üst şehirler ve üst ülkelerde Coğrafya göre istatistiksel bilgiler sağlayan iki ek rapor vardır. Bu raporlar şehirler ve ülkelerde, sırasıyla, bu bölgelerden kaynaklanan sayısı göre derecelendirmek. Bu tür bir rapor oluşturma sırasında bir çubuk grafik üst 10 şehir veya belirli bir platform üzerinde içerik istenen ülkelerde gösterir. Bu çubuk grafiği, içerik istekleri için en yüksek sayıda oluşturmak bölgeleri hızlı bir şekilde değerlendirmek sağlar.

Sol taraftaki grafik (y) belirtilen bölgede kaç tane isabet oluştu gösterir. Doğrudan grafiğin (x ekseni), her üst 10 bölgeler için bir etiket bulacaksınız.

### <a name="using-the-bar-charts"></a>Çubuk grafikler kullanma
* Çubuğunun üzerine getirirseniz, araç ipucu olarak adı ve bölge içinde oluşan toplam sayısı görüntülenir.
* Araç İpucu üst Şehir rapor için bir şehir adı, bölge ve ülke kısaltması tanımlar.
* Şehir veya bir isteğin kaynaklandığı bölge (yani, eyalet/il) belirlenemedi, bilinmeyen olduğunu gösterir. Ülke bilinmeyen sonra iki soru işareti (yani??) ise görüntülenir.
* Bir rapor "Avrupa" veya "Asya/Pasifik bölge." ölçümleri içerebilir Bu öğeler bu bölgelerdeki tüm IP adresleri hakkında istatistiksel bilgiler sağlamak için düşünülmemiştir. Bunun yerine, bunlar yalnızca Avrupa veya Asya/Pasifik yerine üzerinden bir belirli şehir veya ülke genişlediğini IP adreslerini kaynaklanan istekleri için geçerlidir.

Çubuk grafik oluşturmak için kullanılan veri altına görüntülenebilir. Vardır (gigabayt cinsinden) isabet, isabet yüzdesi, veri miktarını toplam sayısını aktarılır ve üst 250 bölgeler için veri yüzdesi aktarılan bulacaksınız. Bir açıklama her bu ölçümleri görüntüleyin.

Aşağıdaki rapor her iki tür için kısa bir açıklaması sağlanır.

| Rapor adı | Açıklama |
| --- | --- |
| Üst Şehir |Bu rapor bu bölgesinden Şehir kaynaklanan sayısı göre derecelendirir. |
| Üst ülkelerde |Bu rapor bu bölgesinden ülkelerde kaynaklanan sayısı göre derecelendirir. |

## <a name="daily-summary"></a>Günlük özeti
Günlük Özeti raporu, toplam sayısı isabetler ve günlük olarak belirli bir platform aktarılan verilerin görüntülemenizi sağlar. Bu bilgiler, hızlı bir şekilde CDN etkinlik desenlerini keşfedilir için kullanılabilir. Örneğin, bu raporu hangi günlerinde daha yüksek deneyimli veya beklenen trafik değerinden daha düşük belirlemenize yardımcı olabilir.

Bu tür bir rapor oluşturma sırasında bir çubuk grafik platforma özgü talep deneyimli miktarı konusunda görsel bir gösterge günlük olarak raporun kapsadığı süre boyunca sağlar. Bu rapordaki her gün için bir çubuk görüntüleyerek görüntülemez. Örneğin, süre seçme "Geçen hafta" adlı bir çubuk grafiği yedi çubukları ile oluşturur. Her çubuk toplam sayısı o gün deneyimli belirtir.

Belirtilen tarihte kaç tane isabet oluştu (y) grafik sol tarafında gösterir. Doğrudan grafiğin (x ekseni) tarihi belirten bir etiket bulabilirsiniz (biçim: YYYY-AA-GG) rapora dahil her gün için.

> [!TIP]
> Çubuğunun üzerine getirirseniz, belirli bir tarihte oluştu toplam sayısı araç ipucu olarak görüntülenir.
> 
> 

Çubuk grafik oluşturmak için kullanılan veri altına görüntülenebilir. Vardır raporun kapsadığı her gün için toplam isabet sayısı ve (gigabayt cinsinden) aktarılan veri miktarını bulacaksınız.

## <a name="by-hour"></a>Saatlik olarak
Tarafından saat raporun isabetler ve saatlik olarak başka bir belirli bir platform aktarılan verilerin toplam sayısı görüntülemenize izin verir. Bu bilgiler, hızlı bir şekilde CDN etkinlik desenlerini keşfedilir için kullanılabilir. Örneğin, bu raporu daha yüksek veya beklenen trafiğe daha düşük deneyimi gün zaman aralıklarında belirlemenize yardımcı olabilir.

Bu tür bir rapor oluşturma sırasında bir çubuk grafik saatlik olarak başka bir raporun kapsadığı süre boyunca yaşadı platforma özgü talep miktarı konusunda görsel bir gösterge sağlar. Bu raporun kapsadığı her saatte bir çubuk görüntüleyerek görüntülemez. Örneğin, 24 saatlik seçerek süre bir çubuk grafiği yirmi dört çubukları ile oluşturur. Her çubuk toplam sayısı bu saatte deneyimli belirtir.

Belirtilen saat üzerinde kaç tane isabet oluştu (y) grafik sol tarafında gösterir. Doğrudan grafiğin (x ekseni), tarih/saat belirten bir etiket bulabilirsiniz (biçim: YYYY-AA-GG ss: dd) rapora dahil her saat. 24 saat biçimini kullanarak bildirilen süresi ve UTC/GMT saat dilimi kullanılarak belirtilir.

> [!TIP]
> Çubuğunun üzerine getirirseniz, bu saatte oluştu toplam sayısı araç ipucu olarak görüntülenir.
> 
> 

Çubuk grafik oluşturmak için kullanılan veri altına görüntülenebilir. Vardır raporun kapsadığı her saat için toplam isabet sayısı ve (gigabayt cinsinden) aktarılan veri miktarını bulacaksınız.

## <a name="by-file"></a>Dosya tarafından
Tarafından dosya raporu, isteğe bağlı ve en çok istenen varlıklar için belirli bir platform üzerinde sonucunda trafik miktarını görüntülemenizi sağlar. Bu tür bir rapor oluşturma sırasında bir çubuk grafik üst 10 en çok istenen varlıklar belirtilen süre boyunca oluşturulur.

> [!NOTE]
> Bu rapor amaçları doğrultusunda, eşdeğer CDN URL'lerinin kenar CNAME URL'leri dönüştürülür. Bu, CDN veya edge CNAME bu istek için kullanılan URL bağımsız olarak bir varlığı ile ilişkili toplam sayısı için doğru bir tally sağlar.
> 
> 

Sol taraftaki grafik (y) belirtilen süre boyunca her varlık için istek sayısını gösterir. Doğrudan grafiği (x ekseni) altında her üst 10 istenen varlıklar için dosya adını belirten bir etiket bulacaksınız.

Çubuk grafik oluşturmak için kullanılan veri altına görüntülenebilir. Her üst 250 istenen varlıklar için aşağıdaki bilgileri var. bulacaksınız: göreli yol, toplam sayısı isabet, isabet yüzdesi, veri miktarını transfer (gigabayt cinsinden) ve veri yüzdesi aktarılır.

## <a name="by-file-detail"></a>Dosya ayrıntısı
Tarafından dosya ayrıntı raporu, isteğe bağlı ve belirli bir varlık için belirli bir platform üzerinde sonucunda trafik miktarını görüntülemenizi sağlar. Bu rapor çok üstünde, Ayrıntılar için dosya seçenektir. Bu seçenek seçilen platformunda en çok istenen varlıklarınızı listesini sağlar. Tarafından dosyası ayrıntısı raporu oluşturmak için dosya ayrıntıları için seçeneğinden istediğiniz varlığı seçin gerekir. Sonrasında, çubuk grafik belirtilen süre boyunca oluşturulan günlük talep miktarını gösterir.

Sol taraftaki (y ekseni) grafiğin isteği sayısı, bir varlığın belirli bir günde yaşadı gösterir. Doğrudan grafiğin (x ekseni) tarihi belirten bir etiket bulabilirsiniz (biçim: YYYY-AA-GG) için hangi CDN varlık için isteğe bağlı bildirildi.

Çubuk grafik oluşturmak için kullanılan veri altına görüntülenebilir. Vardır raporun kapsadığı her gün için toplam isabet sayısı ve (gigabayt cinsinden) aktarılan veri miktarını bulacaksınız.

## <a name="by-file-type"></a>Dosya türü
Dosya türüne göre raporu, isteğe bağlı ve dosya türüne göre sonucunda trafik miktarını görüntülemenizi sağlar. Bu tür bir rapor oluşturma sırasında bir halka grafik üst 10 dosya türleri tarafından oluşturulan isabet yüzdesi belirtir.

> [!TIP]
> Halka grafikte bir dilim üzerine getirirseniz, Internet medya dosya türü araç ipucu olarak görüntülenir türü.
> 
> 

Halka grafiği oluşturmak için kullanılan veri altına görüntülenebilir. Vardır, bulacaksınız dosya adı uzantısı/Internet medya türü, toplam isabet sayısı, isabet, yüzdesi aktarılan veri miktarını (gigabayt cinsinden) ve veri yüzdesi üst 250 dosya türlerinin her biri için aktarılır.

## <a name="by-directory"></a>Dizin ile
Tarafından dizin raporu, isteğe bağlı ve belirli bir dizinden içerik için belirli bir platform üzerinde sonucunda trafik miktarını görüntülemenizi sağlar. Bu tür bir rapor oluşturma sırasında bir çubuk grafik üst 10 dizinlerde içerik tarafından oluşturulan toplam sayısı belirtir.

### <a name="using-the-bar-chart"></a>Çubuk grafik kullanma
* Karşılık gelen dizinin göreli yoluna görüntülemek için bir çubuk üzerine gelerek.
* Bir dizinin bir alt klasöründe depolanan içerik isteğe bağlı dizin hesaplanırken sayılmaz. Bu hesaplama yalnızca gerçek dizinde saklanan içerik için oluşturulan istek sayısı kullanır.
* Bu rapor amaçları doğrultusunda, eşdeğer CDN URL'lerinin kenar CNAME URL'leri dönüştürülür. Bu, CDN veya edge CNAME bu istek için kullanılan URL bağımsız olarak bir varlığı ile ilişkili tüm istatistikler için doğru bir tally sağlar.

Sol taraftaki (y ekseni) grafiğin ilk 10 dizinlerde depolanan içerik istekleri toplam sayısını gösterir. Her çubuk grafik bir dizin temsil eder. Üst 250 tam dizinleri bölümünde listelenen bir dizin çubuğuna eşleştirmek için renk kodlama düzenini kullanın.

Çubuk grafik oluşturmak için kullanılan veri altına görüntülenebilir. Her üst 250 dizinler için aşağıdaki bilgileri var. bulacaksınız: göreli yol, toplam sayısı isabet, isabet yüzdesi, veri miktarını transfer (gigabayt cinsinden) ve veri yüzdesi aktarılır.

## <a name="by-browser"></a>Tarayıcı tarafından
Tarayıcı tarafından raporu, hangi tarayıcılar içerik istemek için kullanılan görüntülemenizi sağlar. Bu tür bir rapor oluşturma sırasında pasta grafiği üst 10 tarayıcılar tarafından işlenen isteklerin yüzdesini belirtir.

### <a name="using-the-pie-chart"></a>Pasta grafik kullanma
* Tarayıcının adını ve sürümünü görüntülemek için pasta grafiğin bir dilimi üzerine gelerek.
* Bu rapor amaçları doğrultusunda, her benzersiz tarayıcı/sürüm bileşimi farklı bir tarayıcı olarak kabul edilir.
* "Diğer" olarak adlandırılan dilim diğer tüm tarayıcılar ve sürümleri tarafından işlenen isteklerin yüzdesini gösterir.

Pasta grafik oluşturmak için kullanılan veri altına görüntülenebilir. Vardır, tarayıcı türü/sürüm numarası, toplam isabet sayısı ve isabet yüzdesi her üst 250 tarayıcılar için bulacaksınız.

## <a name="by-referrer"></a>Göre başvuran
Tarafından başvuran rapor, seçilen platformda içeriğe üst başvuran görüntülemenizi sağlar. Bir başvuran bir istek oluşturulduğu ana bilgisayar adını belirtir. Bu tür bir rapor oluşturma sırasında bir çubuk grafik üst 10 başvuran tarafından oluşturulan isteğe bağlı (yani, isabet) miktarını belirtir.

Grafik (y) sol tarafında bir varlık için her başvuran yaşadı istekleri toplam sayısını gösterir. Her çubuk grafik bir başvuran temsil eder. Üst 250 başvuran bölümünde listelenen bir başvuran çubuğuna eşleştirmek için renk kodlama düzenini kullanın.

Çubuk grafik oluşturmak için kullanılan veri altına görüntülenebilir. Vardır, URL, toplam isabet sayısı ve her üst 250 başvuran oluşturulan isabet yüzdesi bulacaksınız.

## <a name="by-download"></a>Tarafından indirme
Tarafından karşıdan rapor, en çok istenen içeriğiniz için indirme desenleri çözümlemek sağlar. Raporun üst karşılaştırır üst 10 istenilen varlıkların tamamlanmış yüklemeleri yüklemeleriyle çalıştı bir çubuk grafik içerir. Her çubuğu, denenen karşıdan yükleme (mavi) veya tamamlanan yükleme (yeşil) olup göre kodludur.

> [!NOTE]
> Bu rapor amaçları doğrultusunda, eşdeğer CDN URL'lerinin kenar CNAME URL'leri dönüştürülür. Bu, CDN veya edge CNAME bu istek için kullanılan URL bağımsız olarak bir varlığı ile ilişkili tüm istatistikler için doğru bir tally sağlar.
> 
> 

Sol taraftaki (y ekseni) grafiğin her üst 10 istenen varlıklar için dosya adını gösterir. Doğrudan grafiğin (x ekseni) yüklemeleri çalıştı ve tamamlanan toplam sayısını belirtmek etiketleri bulacaksınız.

Doğrudan çubuk grafik aşağıdaki bilgiler listelenir üst 250 istenen varlıkları için: göreli yolu (dosya adı dahil), tamamlanıncaya kadar indirildiği sayısı, istendi sayısı ve tam bir indirme sonuçlandı isteklerin.

> [!TIP]
> Bizim CDN HTTP istemci (yani tarayıcısı) bilgisi verilmediği ne zaman bir varlık tamamen indirilir. Sonuç olarak, bir varlık tamamen durum kodları ve bayt aralığı göre indirildi olup olmadığını hesaplamak sahibiz istekleri. Biz bu hesaplama yaparken Ara ilk 200 Tamam durum koduna isteği olup olmadığını sonuçları şeydir. Bu durumda, biz tüm varlık kapak emin olmak için bayt aralığı isteklerinin arayın. Son olarak, istenen varlık boyutunu aktarılan veri miktarını karşılaştırın. Aktarılan veriler eşit veya bundan büyük dosya boyutu ve bayt aralığı isteklerini bu varlık için uygun olan, isabet tam bir yükleme olarak sayılır.
> 
> Bu rapor interpretive yapısı nedeniyle, bu raporun doğruluğunu ve tutarlılık alter aşağıdaki noktaları göz önünde bulundurmanız.
> 
> * Kullanıcı aracılarına farklı davranır olduğunda trafiği desenlerini doğru şekilde tahmin edilemez. Bu, % 100'den büyük olan tamamlanan yükleme sonuçlara neden olabilir.
> * HTTP aşamalı indirme yararlanmak varlıklar doğru şekilde bu rapor tarafından temsil edilebilir değil. Bir video farklı konumlara arayan kullanıcılar nedeniyle budur.
> 
> 

## <a name="by-404-errors"></a>404 hataları
404 hataları tarafından raporun en fazla 404 bulunamadı durum kodları oluşturur içerik türünü belirlemenize olanak tanır. Raporun üst 404 bulunamadı durum kodu en iyi duruma döndürüldü ilk 10 varlıklar için bir çubuk grafik içerir. Bu çubuk grafik isteği sayısı, o varlıklar için 404 bulunamadı durum kodu ile sonuçlandı istekleri ile karşılaştırır. Her çubuk renk kodludur. Sarı çubuğu istek 404 bulunamadı durum koduna kaynaklandığını göstermek için kullanılır. Kırmızı çubuğu isteği sayısı, varlık için göstermek için kullanılır.

> [!NOTE]
> Bu rapor amaçları doğrultusunda, aşağıdakilere dikkat edin:
> 
> * İsabet herhangi bir istek durum kodu bakılmaksızın bir varlık için temsil eder.
> * Edge CNAME URL'leri, eşdeğer CDN URL'lerinin dönüştürülür. Bu, CDN veya edge CNAME bu istek için kullanılan URL bağımsız olarak bir varlığı ile ilişkili tüm istatistikler için doğru bir tally sağlar.
> 
> 

Sol taraftaki (y ekseni) grafiğin her 404 bulunamadı durum koduna sonuçlandı üst 10 istenen varlıklar için dosya adını gösterir. Doğrudan grafiğin (x ekseni) toplam istek sayısı ve 404 bulunamadı durum koduna sonuçlanan istek sayısını belirten etiketler bulabilirsiniz.

Doğrudan çubuk grafik aşağıdaki bilgiler listelenir üst 250 istenen varlıkları için: göreli yolu (dosya adı dahil), 404 bulunamadı durum koduna sonuçlanan istek sayısı, varlık istendi toplam sayısı ve 404 bulunamadı durum koduna sonuçlandı isteklerin yüzdesi.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure CDN'ye genel bakış](cdn-overview.md)
* [Microsoft Azure cdn'de gerçek zamanlı İstatistikler](cdn-real-time-stats.md)
* [Kurallar altyapısı kullanarak varsayılan HTTP davranışı geçersiz kılma](cdn-rules-engine.md)
* [Edge performansını çözümleme](cdn-edge-performance.md)

