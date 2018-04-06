---
title: Azure CDN kenar düğümü performansını çözümleme | Microsoft Docs
description: Microsoft Azure cdn'de kenar düğümü performansını analiz edin. Edge performans analizi için CDN ayrıntılı bilgi trafiği ve bant genişliği kullanımı sağlar.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: 8cc596a7-3e01-4f76-af7b-a05a1421517e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: ad285b4e2226c85859acb22ba214cc44c77c08e2
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Microsoft Azure CDN’de kenar düğümü performansını çözümleme
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış
Edge performans analizi için CDN ayrıntılı bilgi trafiği ve bant genişliği kullanımı sağlar. Bu bilgiler daha sonra varlıklarınızı nasıl yükleniyor üzerinde daha iyi kavramak olanak tanıyan oluşturan eğilim istatistikler oluşturmak için kullanılabilir önbelleğe ve istemcilerinize teslim edilir. Buna karşılık, bu, bir strateji içeriğinizi teslim en iyi duruma getirme oluşturmasını sağlar ve sorunları olması gerektiğini belirlemek için daha iyi dengeleme CDN tackled. Sonuç olarak, yalnızca veri teslim performansı mümkün olacaktır, ancak, ayrıca, CDN maliyetlerini azaltmak kullanamazsınız.

> [!NOTE]
> Tüm raporlar UTC/GMT gösterimini bir tarih belirtmek için kullanın.
> 
> 

## <a name="reports-and-log-collection"></a>Raporlar ve günlük toplama
Raporlar oluşturmadan önce CDN etkinlik verileri kenar Performans Analizi modülü tarafından toplanan gerekir. Bu koleksiyonu işlemi, bir gün ve onu kapsayan sonra önceki gün sırasında gerçekleşen etkinlik gerçekleşir. Bir raporun istatistikleri işlenmiş ve mutlaka yapın zaman günün istatistikleri örneği temsil eden anlamına gelir, geçerli gün için eksiksiz veri Seti içerir. Performans değerlendirmek için bu raporların birincil işlevi olduğu. Bunlar faturalandırma amaçları veya tam sayısal istatistikler için kullanılmamalıdır.

> [!NOTE]
> Edge performans analitik raporlar oluşturulan ham verileri en az 90 gün için kullanılabilir.
> 
> 

## <a name="dashboard"></a>Pano
Edge Performans Analizi Pano grafiği ve istatistikleri aracılığıyla geçerli ve geçmiş CDN trafiği izler. Bu pano, hesabınız için CDN trafiği performansına yakın zamanda ve uzun vadeli eğilimleri algılamak için kullanın.

Bu panoyu oluşur:

* Anahtar ölçümleri ve eğilimleri görselleştirme sağlayan etkileşimli bir grafik.
* Anahtar ölçümleri ve eğilimleri için uzun vadeli desenleri duygusu sağlayan bir zaman çizelgesi.
* Anahtar ölçümleri ve hakkında istatistiksel bilgi CDN ağımız genel performansı, kullanım ve verimliliği tarafından ölçülen site trafiğini artırır.

### <a name="accessing-the-edge-performance-dashboard"></a>Edge performans Pano erişme
1. CDN profili dikey penceresinden tıklayın **Yönet** düğmesi.
   
    ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
2. Üzerine gelerek **Analytics** sekmesini ve ardından üzerine gelerek **kenar Perfomance Analytics** çıkma.  Tıklayın **Pano**.
   
    Edge düğüm analytics Panosu görüntülenir.

### <a name="chart"></a>Grafik
Pano doğrudan göründüğü zaman çizelgesi seçili süre boyunca bir ölçüm izleyen bir grafik içerir.  Grafikleri son iki yıl CDN etkinlik yukarı bir zaman çizelgesi doğrudan grafiğin altında görüntülenir.

#### <a name="using-the-chart"></a>Grafik kullanma
* Varsayılan olarak, son 30 gün için önbellek verimlilik oranı grafiğinin.
* Bu grafik, günlük olarak Harmanlanmış verilerden oluşturulur.
* Çizgi grafiği bir günde üzerine getirildiğinde bir tarih ve ölçüm değerini belirli bir tarihte belirtir.
* Bir katmana grafik üzerine hafta sonları temsil açık gri dikey çubuk geçiş yapmak için hafta sonları vurgulayın'ı tıklatın. Bu tür bir katmana hafta sonları trafik düzenlerini tanımlamak için yararlıdır.
* Görünüm bir yıl grafik üzerine aynı süre boyunca bir katmana önceki yılın etkinlik geçiş yapmak için önce'ı tıklatın. Bu tür karşılaştırma uzun vadeli CDN kullanım desenlerini bir anlayış sağlar. Grafiğin sağ üst köşede her çizgi grafiği renk kodunu gösteren bir gösterge içerir.

#### <a name="updating-the-chart"></a>Grafik güncelleme
* Zaman aralığı: aşağıdakilerden birini gerçekleştirin:
  * Zaman çizelgesinde istediğiniz bölgeyi seçin. Grafik seçili zaman aralığına karşılık gelen verilerle güncelleştirilir.
  * Grafik en fazla iki yıllık kullanılabilir tüm geçmiş verilerini görüntülemek için çift tıklayın.
* Ölçüm: İstenen ölçüm yanındaki görünür grafiği simgesine tıklayın. Grafik ve zaman çizelgesi karşılık gelen ölçüm verileri ile yenilenecek.

### <a name="key-metrics-and-statistics"></a>Anahtar ölçümleri ve istatistikleri
#### <a name="efficiency-metrics"></a>Verimlilik ölçümleri
Önbellek verimliliği geliştirilmiş olup olmadığını görmek için bu ölçümleri amacı budur. Önbellek verimliliği türetilen başlıca yararları şunlardır:

* Azaltılmış yük kaynak sunucuda olan neden:
  * Daha iyi web sunucusunun performans.
  * İşlem maliyetlerini düşürür.
* Daha fazla isteği doğrudan CDN sunulacak bu yana veri teslim hızlandırma geliştirildi.

| Alan | Açıklama |
| --- | --- |
| Önbellek verimliliği |Önbellekten sunulduğu aktarılan verilerin yüzdesini gösterir. Bu ölçüm ölçümleri istenen içeriğin önbelleğe alınan bir sürüm olduğunda doğrudan CDN (Kenar sunucuları) sunulan sahiplerini (örneğin, web tarayıcısı) |
| İsabet oranı |Önbelleğinden sunulduğunu isteklerinin yüzdesini gösterir. Bu ölçüm ölçümleri istenen içeriğin önbelleğe alınan bir sürüm olduğunda hizmet doğrudan CDN (Kenar sunucuları) sahiplerini (örneğin, web tarayıcısı). |
| % Uzak bayt - önbellek yapılandırma |Kaynak sunucudan atlama önbellek özelliği (HTTP kurallar altyapısı) sonucunda önbelleğe alınmamış CDN (Kenar sunucuları) sunulduğu trafiği yüzdesini gösterir. |
| Uzak bayt - önbellek süresi dolan yüzdesi |Eski içerik COLLECTION sonucunda (Kenar sunucuları) CDN ile kaynak sunuculardan sunulduğu trafiği yüzdesini gösterir. |

#### <a name="usage-metrics"></a>Ölçümleri kullanma
Bu ölçümler amacı aşağıdaki maliyet kesme ölçüleri bir anlayış sağlamaktır:

* CDN aracılığıyla işletim maliyetlerini en aza indirir.
* Önbellek verimliliği ve sıkıştırma aracılığıyla CDN harcamalarını azaltır.

> [!NOTE]
> Trafik birim numaralarını oranları yüzdeleri ve hesaplamalarda kullanıldı ve yüksek hacimli müşteriler için toplam trafiğin bir kısmını yalnızca gösterebilir trafiğin temsil eder.
> 
> 

| Alan | Açıklama |
| --- | --- |
| Giden Ave bayt |CDN (Kenar sunucuları) (örneğin, web tarayıcısı) istemciye sunulan her istek için aktarılan bayt sayısını gösterir. |
| Hiçbir önbellek yapılandırma bayt oranı |CDN (Kenar sunucuları) nedeniyle atlama önbellek özelliğinin önbelleğe alınmamış isteyenin (örneğin, web tarayıcısı) hizmet verilen trafiği yüzdesini gösterir. |
| Sıkıştırılmış bayt oranı |Sıkıştırılmış biçimde CDN (Kenar sunucuları) sahiplerini (örneğin, web tarayıcısı) gönderilen trafiğin yüzdesini gösterir. |
| Giden bayt |CDN (Kenar sunucuları) (örneğin, web tarayıcısı) istemciye teslim edilen bayt cinsinden veri miktarını gösterir. |
| Bayt cinsinden |Bayt cinsinden veri miktarını (örneğin, web tarayıcısı) sahiplerini gönderilen CDN (Kenar sunucuları) gösterir. |
| Bayt uzaktan |Bayt cinsinden (Kenar sunucuları) CDN ile CDN ve müşteri kaynak sunucularından gönderilen veri miktarını gösterir. |

#### <a name="performance-metrics"></a>Performans ölçümleri
Bu ölçümler amacı, trafik için genel CDN performansı izlemektir.

| Alan | Açıklama |
| --- | --- |
| Aktarım hızı |Hangi içerik için bir istek CDN aktarıldı ortalaması gösterir. |
| Süre |Ortalama süreyi belirtir (örneğin, web tarayıcısı) istek sahibine bir varlık teslim etmek için geçen milisaniye cinsinden. |
| Sıkıştırılmış isteği hızı |CDN (Kenar sunucuları) (örneğin, web tarayıcısı) istemciye teslim isabet yüzdesi sıkıştırılmış biçimde gösterir. |
| 4xx hata oranı |4xx durum kodu oluşturulan isabet yüzdesini gösterir. |
| 5XX hata oranı |Bir 5xx durum kodu oluşturulan isabet yüzdesini gösterir. |
| İsabet sayısı |CDN içeriği için istek sayısını gösterir. |

#### <a name="secure-traffic-metrics"></a>Güvenli trafiği ölçümleri
HTTPS trafiği için CDN performansını izlemek için bu ölçümleri amacı budur.

| Alan | Açıklama |
| --- | --- |
| Güvenli önbellek verimliliği |Önbellekten sunulduğunu HTTPS isteklerinde aktarılan verilerin yüzdesini gösterir. Bu ölçüm ölçümleri istenen içeriğin önbelleğe alınan bir sürüm olduğunda hizmet doğrudan CDN (Kenar sunucuları) sahiplerini (örneğin, web tarayıcısı) HTTPS üzerinden. |
| Güvenli aktarım hızı |Hangi içerik sahiplerini (örneğin, web sunucuları) CDN (Kenar sunucuları) aktarıldı ortalaması HTTPS üzerinden gösterir. |
| Ortalama güvenli süresi |Ortalama süreyi belirten bir varlık HTTPS üzerinden bir istemciye (örneğin, web tarayıcısı) sunmak için geçen milisaniye cinsinden. |
| Güvenli İsabetli Okuma |CDN içeriğini HTTPS isteklerinin sayısını gösterir. |
| Giden güvenli bayt |CDN (Kenar sunucuları) (örneğin, web tarayıcısı) istemciye teslim bayt cinsinden HTTPS trafiği miktarını gösterir. |

## <a name="reports"></a>Reports
Bu modüldeki her bir raporu ölçümleri farklı türleri için bant genişliği ve trafik kullanım istatistikleri ve grafik içerir (örneğin, HTTP durum kodları, önbellek durum kodları, istek URL'si, vb..). Bu bilgiler, içerik istemcilerinize nasıl sunulmasını içine daha derin inceleyin ve veri teslim performansını artırmak için CDN davranışına ince ayar yapmak için kullanılabilir.

### <a name="accessing-the-edge-performance-reports"></a>Edge performans raporları erişme
1. CDN profili dikey penceresinden tıklayın **Yönet** düğmesi.
   
    ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-edge-performance/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
2. Üzerine gelerek **Analytics** sekmesini ve ardından üzerine gelerek **kenar Perfomance Analytics** çıkma.  Tıklayın **HTTP büyük nesne**.
   
    Edge düğüm analizi raporları ekran görüntülenir.

| Rapor | Açıklama |
| --- | --- |
| Günlük özeti |Belirtilen bir süre boyunca günlük trafiği eğilimleri görüntülemenize izin verir. Bu grafik her çubuğunda belirli bir tarih temsil eder. Çubuğu boyutu o tarihte oluştu isabetli okuma sayısının toplam sayıyı belirtir. |
| Saatlik özeti |Belirtilen bir süre boyunca trafiği eğilimleri saatlik görüntülemenizi sağlar. Bu grafik her çubuğunda belirli bir tarihte tek bir saat temsil eder. Çubuğu boyutu bu saatte oluştu isabetli okuma sayısının toplam sayıyı belirtir. |
| Protokoller |HTTP ve HTTPS protokolleri arasındaki trafiği dökümünü gösterir. Bir halka grafik Protokolü her tür için oluştu isabet yüzdesini gösterir. |
| HTTP yöntemleri |Hangi HTTP yöntemlerini verilerinizi istemek için kullanılan hızlı bir fikir edinmenizi sağlar. Genellikle, en yaygın HTTP isteği GET, HEAD ve POST yöntemleridir. Bir halka grafik HTTP istek yöntemi her tür için oluştu isabet yüzdesini gösterir. |
| URL'leri |Üst 10 istenen URL görüntüleyen bir grafik içerir. Her URL için bir çubuk görüntülenir. Raporun belirli URL zaman aralığı içinde üretti kaç tane isabet kapsadığı çubuğu yüksekliğini belirtir. İlk 100 istatistiklerini URL'leri doğrudan bu grafiğin altında gösterilen istedi. |
| CNAMEs |10 CNAME'ler varlıklar zamanla istemek için kullanılan bir rapor span üst görüntüleyen bir grafik içerir. İlk 100 istatistiklerini CNAME'ler doğrudan bu grafiğin altında gösterilen istedi. |
| Kaynaklar |Üst 10 CDN görüntüleyen bir grafik veya varlıklar belirtilen bir süre boyunca istendi müşteri kaynak sunucuları içerir. İlk 100 istatistiklerini CDN veya müşteri kaynak sunucuları doğrudan bu grafiğin altında gösterilen istedi. Müşteri kaynak sunucuları, dizin adı seçeneğinde tanımlanan bir ad tarafından tanımlanır. |
| Coğrafi POP |Trafiğinizi ne kadarının bir belirli noktası bulunma (POP) yönlendirilen gösterir. Üç harfli bir POP bizim CDN ağındaki temsil eder. |
| İstemciler |Belirtilen bir süre boyunca varlıklar istenen üst 10 istemcileri görüntüleyen bir grafik içerir. Bu rapor amaçları doğrultusunda, aynı IP adresinden kaynaklanan tüm istekleri aynı istemciden olduğu kabul edilir. Üst 100 istemci için istatistikleri doğrudan bu grafiğin altında görüntülenir. Bu rapor, üst istemciler için yükleme etkinlik desenlerini belirlemek için kullanışlıdır. |
| Önbellek durumları |Genel son kullanıcı deneyimini geliştirmek için yaklaşımlar gösterebilir önbellek davranışını ayrıntılı bir dökümünü sağlar. Hızlı performans Önbelleği İsabetli Okuma gelen olduğundan, veri teslim hızlarını en aza İsabetsiz Önbellek okuma sayısı ve süresi dolan önbelleği isabetli okuma en iyi duruma getirebilirsiniz. |
| Hiçbiri ayrıntıları |Kendisi için önbellek içerik yenilik belirtilen bir süre boyunca denetlendi olmayan varlıklar için üst 10 URL'leri görüntüleyen bir grafik içerir. Bu tür varlıklar için üst 100 URL'leri istatistiklerini doğrudan bu grafiğin altında görüntülenir. |
| CONFIG_NOCACHE ayrıntıları |Müşteri'nin CDN yapılandırması nedeniyle önbelleğe alınmamış varlıklar için en iyi 10 URL'leri görüntüleyen bir grafik içerir. Bu tür varlıklar doğrudan kaynak sunucudan sunulduğunu. Bu tür varlıklar için üst 100 URL'leri istatistiklerini doğrudan bu grafiğin altında görüntülenir. |
| UNCACHEABLE ayrıntıları |İstek üstbilgisi verileri nedeniyle önbelleğe alınmamış varlıklar için üst 10 URL görüntüleyen bir grafik içerir. Bu tür varlıklar için üst 100 URL'leri istatistiklerini doğrudan bu grafiğin altında görüntülenir. |
| TCP_HIT ayrıntıları |Önbellekten hemen sunulan varlıklar için üst 10 URL'leri görüntüleyen bir grafik içerir. Bu tür varlıklar için üst 100 URL'leri istatistiklerini doğrudan bu grafiğin altında görüntülenir. |
| TCP_MISS ayrıntıları |TCP_MISS önbellek durumuna sahip varlıklar için üst 10 URL'leri görüntüleyen bir grafik içerir. Bu tür varlıklar için üst 100 URL'leri istatistiklerini doğrudan bu grafiğin altında görüntülenir. |
| TCP_EXPIRED_HIT ayrıntıları |POP doğrudan sunulduğunu eski varlıklar için üst 10 URL'leri görüntüleyen bir grafik içerir. Bu tür varlıklar için üst 100 URL'leri istatistiklerini doğrudan bu grafiğin altında görüntülenir. |
| TCP_EXPIRED_MISS ayrıntıları |Yeni bir sürümü kaynak sunucudan alınan gerekiyordu eski varlıklar için üst 10 URL'leri görüntüleyen bir grafik içerir. Bu tür varlıklar için üst 100 URL'leri istatistiklerini doğrudan bu grafiğin altında görüntülenir. |
| TCP_CLIENT_REFRESH_MISS Details |İlk 10 URL'leri varlıklar istemciden no-cache isteği nedeniyle bir kaynak sunucudan alındı için görüntüleyen bir çubuk grafik içerir. Bu tür istekleri için üst 100 URL'leri istatistiklerini doğrudan bu grafiğin altına görüntülenir. |
| İstemci istek türleri |HTTP istemcilerini (örn., tarayıcıları) tarafından yapılan istekleri türünü belirtir. Bu rapor, istekleri nasıl ele aldığını bir fikir sağlar bir halka grafik içerir. Bant genişliği ve trafik bilgileri her istek türü için grafiğin altında görüntülenir. |
| Kullanıcı Aracısı |İçeriğinizi bizim CDN aracılığıyla istemek için üst 10 kullanıcı aracıları görüntüleyen bir çubuk grafik içerir. Genellikle, bir kullanıcı web tarayıcısı, media player veya bir cep telefonu tarayıcı aracısıdır. Üst 100 kullanıcı aracıları için istatistikleri doğrudan bu grafiğin altında görüntülenir. |
| Başvuran |Bizim CDN erişilen içerik için üst 10 başvuran görüntüleyen bir çubuk grafik içerir. Genellikle, bir başvuran web sayfasının veya içeriğinize bağlantılar kaynak URL'dir. Ayrıntılı bilgi için en iyi 100 başvuran grafiğin altında sağlanır. |
| Sıkıştırma türleri |İstenilen varlıkların olup bunlar, kenar sunucularımız sıkıştırılan tarafından keser bir halka grafik içeriyor. Sıkıştırılmış varlıklar yüzdesi tarafından kullanılan sıkıştırma türünü ayrılmıştır. Ayrıntılı bilgi grafiğin her sıkıştırma türünü ve durum için sağlanır. |
| Dosya türleri |Hesabınız için bizim CDN aracılığıyla istenen üst 10 dosya türlerini görüntüleyen bir çubuk grafik içerir. Bu rapor amaçları doğrultusunda, bir dosya türü varlığın dosya adı uzantısı ile tanımlanır ve Internet medya türü (örn., .html \[metin/html\], .htm \[metin/html\], .aspx \[metin/html\], vb..). Ayrıntılı bilgi üst 100 dosya türleri için grafiğin altında sağlanır. |
| Benzersiz dosyaları |Belirli bir günde belirli bir süre istenen benzersiz varlıklar toplam sayısı çizer bir grafik içerir. |
| Belirteç kimlik doğrulama özeti |İstenilen varlıkların belirteç tabanlı kimlik doğrulaması ile olup korunan hızlı bir genel bakış sağlayan bir pasta grafik içerir. Korunan varlıklar denenen kendi kimlik doğrulama sonuçlarını göre grafik görüntülenir. |
| Belirteç kimlik doğrulama ayrıntıları Reddet |Belirteç tabanlı kimlik doğrulaması nedeniyle reddedildi üst 10 istekleri görüntülemek izin veren bir çubuk grafik içerir. |
| HTTP yanıt kodları |HTTP durum kodları dökümünü sağlar (örn., 200 Tamam, 403 Yasak, 404 bulunamadı, vb.), teslim HTTP istemcileriniz, kenar sunucularımız. Pasta grafiği, varlıklarınızı nasıl sunulduğunu hızlı bir şekilde değerlendirmek sağlar. Ayrıntılı istatistiksel veriler grafiğin altındaki her yanıt kodu sağlanır. |
| 404 hataları |Bir 404 bulunamadı yanıt kodunda sonuçlanan en fazla 10 istekler görüntülemenizi sağlayan bir çubuk grafik içerir. |
| 403 hataları |Bir 403 Yasak yanıt kodu sonuçlanan en fazla 10 istekler görüntülemenizi sağlayan bir çubuk grafik içerir. Bir müşteri kaynak sunucu veya bizim POP uç sunucusunda tarafından bir istek reddedildiğinde bir 403 Yasak yanıt kodu oluşur. |
| 4xx hataları |Yanıt kodu 400 aralığında sonuçlanan en fazla 10 istekler görüntülemenizi sağlayan bir çubuk grafik içerir. Bu raporun dışında 403 olan bulunamadı ve 404 Yasak yanıt kodları. 4xx yanıt kodu genellikle, bir isteğin sonucu olarak bir istemci hatası reddedilmesi durumunda oluşur. |
| 504 hataları |504 ağ geçidi zaman aşımı yanıt kodunda sonuçlanan en fazla 10 istekler görüntülemenizi sağlayan bir çubuk grafik içerir. Bir 504 ağ geçidi zaman aşımı yanıt kodu, başka bir sunucuyla iletişim kurmak bir HTTP proxy çalışırken zaman aşımı ortaya çıktığında oluşur. Bir uç sunucusu müşteri kaynak sunucu ile iletişim kuramadı bizim CDN söz konusu olduğunda, bir 504 ağ geçidi zaman aşımı yanıt kodu genellikle oluşur. |
| 502 hataları |502 hatalı ağ geçidi yanıt kodunda sonuçlanan en fazla 10 istekler görüntülemenizi sağlayan bir çubuk grafik içerir. Bir 502 hatalı ağ geçidi yanıt kodu, bir sunucu ve bir HTTP proxy arasında bir HTTP protokolü hatası oluştuğunda oluşur. Bir müşteri kaynak sunucu bir uç sunucusu için geçersiz bir yanıt döndürür bizim CDN söz konusu olduğunda, bir 502 hatalı ağ geçidi yanıt kodu genellikle oluşur. Bunu ayrıştırılamıyor veya tamamlanmamış ise, yanıt geçersiz. |
| 5XX hataları |Yanıt kodu 500 aralığında sonuçlanan en fazla 10 istekler görüntülemenizi sağlayan bir çubuk grafik içerir.  Bu raporun dışında 502 hatalı ağ geçidi ve 504 ağ geçidi zaman aşımı yanıt kodları'dır. |

## <a name="see-also"></a>Ayrıca bkz.
* [Azure CDN'ye genel bakış](cdn-overview.md)
* [Microsoft Azure cdn'de gerçek zamanlı İstatistikler](cdn-real-time-stats.md)
* [Kurallar altyapısı kullanarak varsayılan HTTP davranışı geçersiz kılma](cdn-rules-engine.md)
* [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md)

