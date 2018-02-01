---
title: "Web sitelerinin kullanılabilirlik ve yanıt hızını izleme | Microsoft Docs"
description: "Application Insights’ta web testleri ayarlayın. Web sitesi kullanılamaz duruma gelirse veya yavaş yanıt verirse uyarı alın."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/14/2017
ms.author: sdash
ms.openlocfilehash: c9dd60170e93722cab8e8d5eb5b4202b71bbb8e4
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a>Web sitelerinin kullanılabilirlik ve yanıt hızını izleme
Web uygulamanızı veya web sitenizi herhangi bir sunucuya dağıttıktan sonra kullanılabilirlik ve yanıt hızını izlemeye yönelik testler ayarlayabilirsiniz. [Azure Application Insights](app-insights-overview.md), dünyanın her yerindeki noktalarından uygulamanıza düzenli aralıklarla web istekleri gönderir. Uygulamanız yanıt vermezse veya yavaş yanıt verirse sizi uyarır.

Genel İnternet'ten erişilebilen herhangi bir HTTP veya HTTPS uç noktası için kullanılabilirlik testleri ayarlayabilirsiniz. Test ettiğiniz web sitesine eklemeniz gereken bir şey yoktur. Kendi siteniz olması bile gerekmez. Kullandığınız bir REST API hizmetini test edebilirsiniz.

İki tür kullanılabilirlik testi vardır:

* [URL ping testi](#create): Azure portalında oluşturabileceğiniz basit bir test.
* [Çok adımlı web testi](#multi-step-web-tests): Visual Studio Enterprise’da oluşturup portala yüklediğiniz test.

Her uygulama kaynağı için 100’e kadar kullanılabilirlik testi oluşturabilirsiniz.


> [!NOTE] 
> * Kullanılabilirlik testi konumları kısa süre önce Azure veri merkezlerine taşındı. Bu taşıma işlemi, Azure veri merkezlerinin gelişen ağıyla konum eklememizi sağlıyor.  
> * Testleri güncelleştirmeniz gerekmez. Tüm testler geçirilmiştir ve yeni konumlarında çalışmaktadır. 
>* Daha fazla bilgi için lütfen [hizmet güncelleştirmesi](https://blogs.msdn.microsoft.com/applicationinsights-status/2018/01/24/application-insights-availability-monitoring-test-locations-updated/) makalesini inceleyin.

## <a name="create"></a>Kullanılabilirlik testi raporlarınız için kaynak açma

Web uygulamanız için **Application Insights’ı zaten yapılandırdıysanız**, Application Insights kaynağını [Azure portalında](https://portal.azure.com) açın.

**Veya raporlarınızı yeni bir kaynakta görmek istiyorsanız,** [Microsoft Azure](http://azure.com)’a kaydolun, [Azure portalı](https://portal.azure.com)’na gidin ve bir Application Insights kaynağı oluşturun.

![Yeni > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Yeni kaynağa ait Genel Bakış dikey penceresini açmak için **Tüm kaynaklar**’a tıklayın.

## <a name="setup"></a>URL ping testi oluşturma
Kullanılabilirlik dikey penceresini açın ve bir kullanılabilirlik testi ekleyin.

![En azından web sitenizin URL'sini doldurma](./media/app-insights-monitor-web-app-availability/13-availability.png)

* **URL**, test etmek istediğiniz herhangi bir web sayfası olabilir, ancak ortak İnternet’te görünür olmalıdır. URL bir sorgu dizesi içerebilir. Bu nedenle, örneğin, veritabanınızla biraz alıştırma yapabilirsiniz. URL yeniden yönlendirme adresine çözümlenirse, en fazla 10 yeniden yönlendirmeyi izleriz.
* **Bağımlı istekleri ayrıştır**: Bu seçenek işaretlenirse test, test kapsamındaki web sitesine ait görüntü, betik, stil dosyaları ve diğer dosyaları ister. Kayıtlı yanıt süresi, bu dosyaları almak için geçen süreyi içerir. Testin tamamının zaman aşımı süresi içinde tüm bu kaynaklar sorunsuz yüklenemezse, test başarısız olur. 

    Seçenek işaretlenmezse, test yalnızca belirttiğiniz URL’deki dosyayı ister.
* **Yeniden denemeyi etkinleştir**: Bu seçenek işaretlenirse, test başarısız olduğunda kısa bir süre sonra yeniden denenir. Art arda üç deneme başarısız olursa bir hata bildirilir. Sonraki testler bundan sonra her zamanki test sıklığında gerçekleştirilir. Bir sonraki başarılı olana kadar yeniden deneme geçici olarak askıya alınır. Bu kural her test konuma bağımsız olarak uygulanır. Bu ayar önerilir. Ortalama olarak hataların yaklaşık %80’i yeniden deneme sırasında kaybolur.
* **Test sıklığı**: Her test konumdan testin ne sıklıkta çalıştırılacağını ayarlar. Beş dakikalık sıklığında ve beş test konumuyla, siteniz ortalama olarak dakikada bir test edilir.
* **Test konumları**, sunucularımızın URL’nize web istekleri gönderdiği yerlerdir. Bir konumdan fazla seçin; böylece, web sitenizdeki ağ sorunlarını ayırt edebilirsiniz. En fazla 16 konum seçebilirsiniz.
* **Başarı ölçütleri**:

    **Test zaman aşımı**: Yavaş yanıtlar hakkında uyarı almak için bu değeri azaltın. Yanıtlar sitenizden bu süre içinde alınmadıysa test başarısız sayılır. **Bağımlı istekleri ayrıştır**’ı seçtiyseniz; tüm görüntüler, stil dosyaları, betikler ve diğer bağımlı kaynaklar bu süre içinde alınmış olmalıdır.

    **HTTP yanıtı**: Başarılı sayılan döndürüldü durum kodu. 200, normal web sayfası döndürüldüğünü belirten koddur.

    **İçerik eşleşmesi**: "Hoş geldiniz!" gibi bir dize. Her yanıtta büyük küçük harfe duyarlı bir tam eşleşme oluştuğunu test edebiliriz. Joker karakter bulunmayan düz bir dize olmalıdır. Sayfanızın içeriği değişirse bunu güncelleştirmeniz gerektiğini unutmayın.
* **Uyarılar**, varsayılan olarak, beş dakikayı geçen bir sürede üç konumda hata varsa size gönderilir. Tek konumdaki hata daha çok bir ağ sorunudur, sitenizle ilgili değildir. Ancak, eşiği daha fazla veya daha az hassas olarak değiştirebilirsiniz; size kimlerin e-posta göndermesi gerektiğini de değiştirebilirsiniz.

    Bir uyarı ortaya çıktığında çağrılan bir [web kancası](../monitoring-and-diagnostics/insights-webhooks-alerts.md) ayarlayabilirsiniz. (Ancak şu anda sorgu parametreleri Özellikler aracılığıyla geçirilmez.)

### <a name="test-more-urls"></a>Daha fazla URL test etme
Daha fazla test ekleyin. Örneğin, giriş sayfanızın test edilmesine ek olarak arama URL’sini de test ederek veritabanınızın çalıştığından emin olursunuz.


## <a name="monitor"></a>Kullanılabilirlik testi sonuçlarınızı görme

Birkaç dakika sonra, test sonuçlarını görmek için **Yenile**’ye tıklayın. 

![Giriş dikey penceresinde özet sonuçları](./media/app-insights-monitor-web-app-availability/14-availSummary-3.png)

Dağılım grafiği, tanılama testi adım ayrıntılarını içeren örnek test sonuçlarını gösterir. Test altyapısı, hata içeren testler için tanılama ayrıntılarını depolar. Başarılı testlerde, yürütmelerin bir alt kümesi için tanılama ayrıntıları depolanır. Test zaman damgası, test süresi, konum ve test adını görmek için yeşil/kırmızı noktalardan herhangi birinin üzerine gelin. Test sonucunun ayrıntılarını görmek için dağılım grafiğinde noktalara tıklayarak gezinin.  

Belirli bir testi veya konumu seçin ya da ilgilendiğiniz dönemle ilgili daha fazla sonuç görmek için zaman dilimini küçültün. Arama Gezgini’ni kullanarak tüm yürütmelerden alınan sonuçları görün veya Analytics sorgularını kullanarak bu veriler üzerinde özel raporlar çalıştırın.

Ölçüm Gezgini’nde ham sonuçlara ek olarak iki Kullanılabilirlik ölçümü vardır: 

1. Kullanılabilirlik: Tüm test yürütmelerinde başarılı olan testlerin yüzdelik oranı. 
2. Test Süresi: Tüm test yürütmelerinde ortalama test süresi.

Belirli bir testin ve/veya konumun eğilimlerini analiz etmek için test adına ve konumuna göre filtre uygulayabilirsiniz.

## <a name="edit"></a> Testleri inceleme ve düzenleme

Özet sayfasından belirli bir test seçin. Burada özel sonuçları görebilir ve düzenleyebilir ya da geçici olarak devre dışı bırakabilirsiniz.

![Web testini düzenleme veya devre dışı bırakma](./media/app-insights-monitor-web-app-availability/19-availEdit-3.png)

Hizmetinizde bakım gerçekleştirdiğiniz sırada kullanılabilirlik testlerini veya bunlarla ilişkili uyarıları devre dışı bırakmak isteyebilirsiniz. 

## <a name="failures"></a>Hata görürseniz
Kırmızı noktaya tıklayın.

![Kırmızı noktaya tıklama](./media/app-insights-monitor-web-app-availability/open-instance-3.png)


Bir kullanılabilirlik testi sonucundan şunları yapabilirsiniz:

* Sunucunuzdan alınan yanıtı denetleme.
* Başarısız istek örneği işlenirken toplanan sunucu tarafı telemetrisi ile hatayı tanılayın.
* Sorunu izlemek için bir sorunu veya iş öğesini Git’te ya da VSTS’de günlüğe kaydetme. Hata, bu olayın bir bağlantısını içerir.
* Web testi sonucunu Visual Studio’da açın.

*Sorunsuz görünüyor ancak hata olarak mı bildiriliyor?* Gürültüyü azaltma yolları için bkz. [SSS](#qna).

## <a name="multi-step-web-tests"></a>Çok adımlı web testleri
Bir dizi URL'nin bulunduğu bir senaryoyu izleyebilirsiniz. Örneğin, bir satış web sitesi izliyorsanız, öğelerin alışveriş sepetine doğru eklendiğini test edebilirsiniz.

> [!NOTE] 
> Çok adımlı web testleri ücrete tabidir. [Fiyatlandırma düzeni](http://azure.microsoft.com/pricing/details/application-insights/).
> 

Çok adımlı bir test oluşturmak için Visual Studio Enterprise kullanarak senaryoyu kaydedin ve kaydı Application Insights'a yükleyin. Application Insights, senaryoyu aralıklarla yeniden yürütür ve yanıtları doğrular.

> [!NOTE]
> * Testlerinizde kodlanmış işlevler veya döngüler kullanamazsınız. Test tamamen .webtest betiğinde yer almalıdır. Ancak, standart eklentiler kullanabilirsiniz.
> * Çok adımlı web testlerinde yalnızca İngilizce karakterler desteklenir. Visual Studio’yu diğer dillerde kullanıyorsanız lütfen İngilizce olmayan karakterleri çevirmek/hariç tutmak için web testi tanımı dosyasını güncelleştirin.
>

#### <a name="1-record-a-scenario"></a>1. Senaryo kaydetme
Web oturumu kaydetmek için Visual Studio Enterprise kullanın.

1. Web performans testi projesi oluşturun.

    ![Visual Studio Enterprise sürümünde, Web Performansı ve Yük Testi şablonundan bir proje oluşturun.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

 * *Web Performansı ve Yük Testi şablonunu görmüyor musunuz?* - Visual Studio Enterprise’ı kapatın. **Visual Studio Yükleyicisi**’ni açarak Visual Studio Enterprise yüklemesini değiştirin. **Tek Bileşenler** altında **Web Performansı ve yük testi araçları**’nı seçin.

2. .webtest dosyasını açın ve kaydı başlatın.

    ![.webtest dosyasını açın ve Kaydet’e tıklayın.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)
3. Testinizde benzetimini yapmak istediğiniz kullanıcı işlemlerini yapın: web sitenizi açın, sepete ürün ekleyin ve bunlara devam edin. Sonra testinizi durdurun.

    ![Internet Explorer'da web testi kaydedicisi çalışır.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    Uzun bir senaryo oluşturmayın. 100 adımlık ve 2 dakikalık bir sınır vardır.
4. Testi düzenleme nedenleri:

   * Alınan metin ve yanıt kodlarını denetlemek için doğrulama ekleme.
   * Gereksiz tüm etkileşimleri kaldırma. Ayrıca, resim veya reklam için ya da sitelerin izlenmesi için bağımlı istekleri kaldırabilirsiniz.

     Yalnızca test betiğini düzenleyebildiğinizi, özel kod veya başka web testlerinden çağrı ekleyemediğinizi unutmayın. Testlere döngü eklemeyin. Standart web testi eklentileri kullanabilirsiniz.
5. Çalıştığından emin olmak için testi Visual Studio’da çalıştırın.

    Web test çalıştırıcısı bir web tarayıcısı açar ve kaydettiğiniz eylemleri yineler. Beklediğiniz gibi çalıştığından emin olun.

    ![Visual Studio’da .webtest dosyasını açın ve Çalıştır’a tıklayın.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)

#### <a name="2-upload-the-web-test-to-application-insights"></a>2. Web testi Application Insights'a yükleme
1. Application Insights portalında bir web testi oluşturun.

    ![Web testleri dikey penceresinde Ekle'yi seçin.](./media/app-insights-monitor-web-app-availability/16-another-test.png)
2. Çok adımlı testi seçip .webtest dosyasını yükleyin.

    ![Çok adımlı web testini seçin.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    Test konumları, sıklığı ve uyarı parametrelerini aynı ping testlerinde olduğu gibi aynı şekilde ayarlayın.

#### <a name="3-see-the-results"></a>3. Sonuçları görme

Tek url testlerinde olduğu gibi test sonuçlarını ve hatalarını görüntüleyin.

Ayrıca, test sonuçlarını indirerek Visual Studio’da görüntüleyebilirsiniz.

#### <a name="too-many-failures"></a>Çok fazla hata mı var?

* Yaygın bir başarısızlık nedeni testin çok uzun çalışmasıdır. İki dakikadan uzun çalıştırılmamalıdır.

* Betikler, stil sayfaları, görüntüler ve diğerleri de dahil olmak üzere, testin başarılı olması için sayfanın tüm kaynaklarının doğru yüklenmiş olması gerektiğini unutmayın.

* Web testi tamamen .webtest dosyasında olmalıdır: Testte kodlanmış işlevleri kullanamazsınız.

### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a>Çok adımlı testinizde bağlı kalma süresi ve rasgele rakamlar
Dış bir kaynağa ait stoklar gibi zamana bağımlı veriler alan bir aracı test ettiğinizi varsayalım. Web testinizi kaydettiğinizde, belirli zamanları kullanmanız gerekse de, bunları testin parametreleri (StartTime ve EndTime) olarak ayarlarsınız.

![Parametrelere sahip web testi.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

Testi çalıştırdığınızda, EndTime her zaman geçerli zaman, StartTime da 15 dakika öncesi olmalıdır.

Web Testi Eklentileri, zamanları parametreleme yolunu sağlar.

1. İstediğiniz her değişken parametre değeri için bir web testi eklentisi ekleyin. Web testi araç çubuğunda, **Web Testi Eklentisi Ekle**’yi seçin.

    ![Web Testi Eklentisi Ekle’yi, sonra da bir türü seçin.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    Bu örnekte, Tarih Saat Eklentisinin iki örneğini kullanacağız. Bir örnek "15 dakika önce" için, bir örnek de "şimdi" için.
2. Her eklentinin özelliklerini açın. Buna bir ad verip geçerli saat olarak kullanılmak üzere ayarlayın. Bunlardan birini Dakika Ekle = 15 olarak ayarlayın.

    ![Adı, Geçerli Saati Kullan’ı ve Dakika Ekle’yi ayarlayın.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)
3. Web testi parametrelerinde, eklenti adına başvurmak için {{plug-in name}} kullanın.

    ![Test parametresinde {{plug-in name}} kullanın.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

Artık testi portala yükleyin. Testin her çalıştırılışında dinamik değerler kullanılır.

## <a name="dealing-with-sign-in"></a>Oturum açmayla ilgilenme
Kullanıcılarınız uygulamanızda oturum açarsa, oturum açma benzetimi için bir dizi seçeneğiniz vardır; böylece, oturum açmanın ötesinde sayfaları test edebilirsiniz. Kullandığınız yaklaşım, uygulamanın sağladığı güvenlik türüne bağlıdır.

Her durumda, uygulamanızda yalnızca test amacıyla bir hesap oluşturmalısınız. Mümkünse, web testlerinin gerçek kullanıcıları etkileme olasılığını önlemek için test hesabının izinlerini kısıtlayın.

### <a name="simple-username-and-password"></a>Basit kullanıcı adı ve parola
Web testini normal şekilde kaydedin. Önce tanımlama bilgilerini silin.

### <a name="saml-authentication"></a>SAML kimlik doğrulaması
Web testlerinde kullanıma uygun SAML eklentisini kullanın.

### <a name="client-secret"></a>Gizli anahtar
Uygulamanızda gizli anahtar içeren bir oturum açma yolu varsa bu yolu kullanın. Azure Active Directory (AAD), gizli anahtarla oturum açmayı sağlayan bir hizmet örneğidir. AAD’de gizli anahtar, Uygulama Anahtarı’dır.

Aşağıda uygulama anahtarı kullanan bir Azure web uygulaması için web testi örneği verilmiştir:

![Gizli anahtar örneği](./media/app-insights-monitor-web-app-availability/110.png)

1. Gizli anahtar (AppKey) kullanarak AAD’den belirteç alın.
2. Yanıttan taşıyıcı belirteci ayıklayın.
3. Yetkilendirme üst bilgisinde taşıyıcı belirteç kullanarak API çağırın.

Web testinin gerçek bir istemci olduğundan, yani AAD’de kendi uygulamasına sahip olduğundan emin olun ve bu istemcinin clientId’si ile appkey’ini kullanın. Test edilen hizmetiniz, AAD içinde kendi uygulamasına sahiptir: bu uygulamanın appID URI’si, web testinin “kaynak” alanında yansıtılır.

### <a name="open-authentication"></a>Açık Kimlik Doğrulaması
Microsoft veya Google hesabınızla oturum açma, bir açık kimlik doğrulaması örneğidir. OAuth kullanan çok sayıda uygulama, alternatif gizli anahtar da sağlar; bu nedenle ilk taktiğiniz bu olasılığın incelenmesi olmalıdır.

Testinizde OAuth kullanılarak oturum açılması gerekiyorsa, genel yaklaşım şöyledir:

* Web tarayıcınız, kimlik doğrulama sitesi ve uygulamanız arasındaki trafiği incelemek için Fiddler gibi bir araç kullanın.
* Farklı makineler veya tarayıcılar kullanarak veya uzun aralıklarla (süresi dolacak şekilde belirteçleri izin vermek için) iki veya daha fazla oturum açın.
* Farklı oturumları karşılaştırarak, kimlik doğrulama sitesinden geri geçirilen belirteci tanımlayın; başka bir deyişle oturum açıldıktan sonra uygulama sunucunuza geçirilen belirteç.
* Visual Studio’yu kullanarak web testini kaydetme
* Belirteçleri parametreleyin; belirteç kimlik doğrulayıcıdan döndürüldüğünde ve sitede sorgu sırasında kullanıldığında parametre ayarı.
  (Visual Studio testi parametrelemeyi dener, ancak belirteçleri doğru parametrelemez.)


## <a name="performance-tests"></a>Performans testleri
Web sitenizde bir yük testi çalıştırabilirsiniz. Kullanılabilirlik testinde olduğu gibi dünyanın dört bir yanındaki noktalarımızdan basit istekler ya da çok adımlı istekler gönderebilirsiniz. Kullanılabilirlik testinden farklı olarak eşzamanlı birden fazla kullanıcıyı benzeten çok sayıda istek gönderilir.

Genel Bakış dikey penceresinde **Ayarlar**, **Performans Testleri**’ni açın. Bir test oluşturduğunuzda Visual Studio Team Services hesabı oluşturmaya davet edilirsiniz.

Test tamamlandığında yanıt süreleri ve başarı oranları gösterilir.


![Performans testi](./media/app-insights-monitor-web-app-availability/perf-test.png)

> [!TIP]
> Performans testi etkilerini gözlemlemek için [Canlı Akış](app-insights-live-stream.md)'ı ve [Profil Oluşturucu](app-insights-profiler.md)'yu kullanmanız gerekir.
>

## <a name="automation"></a>Otomasyon
* Otomatik olarak [kullanılabilirlik testi ayarlamak için PowerShell betiklerini kullanın](app-insights-powershell.md#add-an-availability-test).
* Bir uyarı ortaya çıktığında çağrılan bir [web kancası](../monitoring-and-diagnostics/insights-webhooks-alerts.md) ayarlayın.

## <a name="qna"></a>Sorularınız mı var? Sorunlarınız mı var?
* *Protokol ihlali hatası ile aralıklı test hatası?*

    Hata ("protokol ihlali..CR’den sonra LF gelmelidir") sunucu (veya bağımlılıklar) ile ilgili bir sorun olduğunu gösterir. Bu durum, yanıtta hatalı biçimlendirilmiş üst bilgiler ayarlandığında meydana gelir. Yük dengeleyiciler veya CDN'lerden kaynaklanabilir. Özellikle bazı üst bilgiler satır sonunu belirtmek için CRLF kullanmıyor olabilir; bu durum HTTP belirtimini ihlal eder ve bu nedenle .NET WebRequest düzeyinde doğrulama başarısız olur. İhlal edici olabilecek nokta üst bilgilerine yanıtı inceleyin.
    
    Not: URL, HTTP üst bilgilerinin gevşek doğrulaması yapılmış tarayıcılarda başarısız olmayabilir. Bu sorunun ayrıntılı bir açıklamsı için şu blog gönderisine bakın: http://mehdi.me/a-tale-of-debugging-the-linkedin-api-net-and-http-protocol-violations/  
* *Site sorunsuz görünüyor, ancak test hataları görüyorum?*

    * Tüm görüntüleri, betikleri, stil sayfalarını ve sayfa tarafından yüklenen diğer dosyaları denetleyin. Herhangi biri başarısızsa, ana html sayfası Tamam olarak yüklense bile test başarısız olarak raporlanır. Bu tür kaynak hatalarına karşı testin hassasiyetini ortadan kaldırmak için test yapılandırmasında "Bağımlı İstekleri Ayrıştır" seçeneğinin işaretini kaldırın. 

    * Geçici ağ sinyalleri vb. kaynaklı gürültü olasılığını azaltmak için "Test hataları için yeniden denemeyi etkinleştir" yapılandırmasının işaretlendiğinden emin olun. Ayrıca, uygunsuz uyarılara neden olan konuma özgü sorunları önlemek için daha fazla konumdan test yapabilir ve uyarı kuralı eşiğini uygun şekilde yönetebilirsiniz.
    
* *Test hatalarını tanılamak için herhangi bir ilgili sunucu tarafı telemetrisi görmüyorum?*
    
    Sunucu tarafı uygulamanız için Application Insights ayarlanmışsa, bunun nedeni [örnekleme](app-insights-sampling.md) işleminin devam ediyor olması olabilir.
* *Web testimden kod çağırabilir miyim?*

    Hayır. Test adımları .webtest dosyasında olmalıdır. Bu nedenle, başka web testlerini çağıramaz, döngüleri kullanamazsınız. Ancak yararlı bulabileceğiniz bir dizi eklenti vardır.
* *HTTPS destekleniyor mu?*

    TLS 1.1 ve TLS 1.2 desteklenir.
* *"Web testleri" ve "kullanılabilirlik testleri" arasında bir fark var mı?*

    Bu iki terim birbirlerinin yerine kullanılabilir. Kullanılabilirlik testleri, çok adımlı web testlerine ek olarak tek URL ping testlerini de içeren daha genel bir terimdir.
* *Kullanılabilirlik testlerini, güvenlik duvarının arkasında çalışan kendi iç sunucumuzda kullanmak istiyorum.*

    İki olası çözümü vardır:
    
    * Güvenlik duvarınızı, [Web testi aracılarımızın IP adreslerinden](app-insights-ip-addresses.md) gelen isteklere izin verecek şekilde yapılandırın.
    * İç sunucunuzu düzenli olarak test etmek için kendi kodunuzu yazın. Kodu, güvenlik duvarınızın arkasındaki bir test sunucusunda arka plan işlemi olarak çalıştırın. Test işleminiz, temel SDK paketindeki [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) API’sini kullanarak sonuçları Application Insights’a gönderebilir. Bunun için test sunucunuzun Application Insights alım uç noktası ile giden bağlantısının olması gerekir, ancak bu, gelen isteklere izin vermeye göre çok daha küçük bir güvenlik riski oluşturur. Sonuçlar kullanılabilirlik web testi dikey pencerelerinde görünür, ancak Analytics, Search ve Ölçüm Gezgini’nde kullanılabilirlik sonuçları olarak görüntülenir.
* *Çok adımlı web testi karşıya yüklenemiyor*

    300 K boyut sınırı vardır.

    Döngüler desteklenmez.

    Başka web testlerine başvurular desteklenmez.

    Veri kaynakları desteklenmez.
* *Çok adımlı testim tamamlanmıyor*

    Test başına 100 istek sınırı var.

    Test, iki dakikadan uzun çalışırsa durdurulur.
* *İstemci sertifikalarıyla testi nasıl çalıştırırım?*

    Üzgünüz, bunu desteklemiyoruz.


## <a name="next"></a>Sonraki adımlar
[Tanılama günlüklerinde arama yapma][diagnostic]

[Sorun giderme][qna]

[Web testi aracılarının IP adresleri](app-insights-ip-addresses.md)

<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
