<properties
    pageTitle="Web sitelerinin kullanılabilirlik ve yanıt hızını izleme | Microsoft Azure"
    description="Application Insights’ta web testleri ayarlayın. Web sitesi kullanılamaz duruma gelirse veya yavaş yanıt verirse uyarı alın."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/07/2016"
    ms.author="awills"/>

# Web sitelerinin kullanılabilirlik ve yanıt hızını izleme

Web uygulamanızı veya web sitenizi herhangi bir ana bilgisayara dağıttıktan sonra kullanılabilirlik ve yanıt hızını izlemek için web testleri ayarlayabilirsiniz. [Visual Studio Application Insights](app-insights-overview.md), dünyanın her yerindeki noktalarından uygulamanıza düzenli aralıklarla web istekleri gönderir. Uygulamanız yanıt vermezse veya yavaş yanıt verirse sizi uyarır.

![Web testi örneği](./media/app-insights-monitor-web-app-availability/appinsights-10webtestresult.png)

Genel İnternet'ten erişilebilen herhangi bir HTTP veya HTTPS uç noktası için web testi ayarlayabilirsiniz.

İki tür web testi bulunur:

* [URL ping testi](#create): Azure portalında oluşturabileceğiniz basit bir test.
* [Çok adımlı web testi](#multi-step-web-tests): Visual Studio Ultimate veya Visual Studio Enterprise’da oluşturup portala yüklediğiniz test.

Her uygulama kaynağı için 10 web testine kadar test oluşturabilirsiniz.

## <a name="create"></a>1. Test raporlarınız için kaynak oluşturma

Bu uygulama için zaten [Application Insights kaynağı ayarladıysanız][start] ve kullanılabilirlik raporlarını aynı yerde görmek istiyorsanız bu adımı atlayın.

[Microsoft Azure](http://azure.com) oturumu açın, [Azure portalına](https://portal.azure.com) gidin ve bir Application Insights kaynağı oluşturun.

![Yeni > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Yeni kaynağa ait Genel Bakış dikey penceresini açmak için **Tüm kaynaklar**’a tıklayın.

## <a name="setup"></a>2. URL ping testi oluşturma

Application Insights kaynağınızda Kullanılabilirlik kutucuğunu arayın. Uygulamanızla ilgili Web testleri dikey penceresini açmak için tıklayın ve bir web testi ekleyin.

![En azından web sitenizin URL'sini doldurma](./media/app-insights-monitor-web-app-availability/13-availability.png)

- **URL** ortak internet'ten görünür olmalıdır. Bir sorgu dizesi içerebilir; bu nedenle, örneğin, veritabanınızla biraz alıştırma yapabilirsiniz. URL yeniden yönlendirme adresine çözümlenirse, en fazla 10 yeniden yönlendirmeyi izleriz.
- **Bağımlı istekleri ayrıştır**: Sayfanın görüntüleri, betikleri, stil dosyaları ve diğer kaynakları testin bir parçası olarak istenir ve kaydedilen yanıt süreleri bu süreleri içerir. Testin tamamının zaman aşımı süresi içinde tüm bu kaynaklar sorunsuz yüklenemezse, test başarısız olur.
- **Yeniden denemeyi etkinleştir**: Test başarısız olduğunda, kısa bir süre sonra yeniden denenir. Art arda üç deneme başarısız olursa bir hata bildirilir. Sonraki testler bundan sonra her zamanki test sıklığında gerçekleştirilir. Bir sonraki başarılı olana kadar yeniden deneme geçici olarak askıya alınır. Bu kural her test konuma bağımsız olarak uygulanır. (Bu ayarı öneriyoruz. Ortalama olarak hataların yaklaşık %80’i yeniden deneme sırasında kaybolur.)
- **Test sıklığı**: Her test konumdan testin ne sıklıkta çalıştırılacağını ayarlar. Beş dakikalık sıklığında ve beş test konumuyla, siteniz ortalama olarak dakikada bir test edilir.
- **Test konumları**, sunucularımızın URL’nize web istekleri gönderdiği yerlerdir. Bir konumdan fazla seçin; böylece, web sitenizdeki ağ sorunlarını ayırt edebilirsiniz. En fazla 16 konum seçebilirsiniz.

- **Başarı ölçütleri**:

    **Test zaman aşımı**: Yavaş yanıtlar hakkında uyarı almak için bu değeri azaltın. Yanıtlar sitenizden bu süre içinde alınmadıysa test başarısız sayılır. **Bağımlı istekleri ayrıştır**’ı seçtiyseniz; tüm görüntüler, stil dosyaları, betikler ve diğer bağımlı kaynaklar bu süre içinde alınmış olmalıdır.

    **HTTP yanıtı**: Başarılı sayılan döndürüldü durum kodu. 200, normal web sayfası döndürüldüğünü belirten koddur.

    **İçerik eşleşmesi**: "Hoş geldiniz!" gibi bir dize. Her yanıtta oluşup oluşmadığını test ederiz. Joker karakter bulunmayan düz bir dize olmalıdır. Sayfanızın içeriği değişirse bunu güncelleştirmeniz gerektiğini unutmayın.


- **Uyarılar**, varsayılan olarak, beş dakikayı geçen bir sürede üç konumda hata varsa size gönderilir. Tek konumdaki hata daha çok bir ağ sorunudur, sitenizle ilgili değildir. Ancak, eşiği daha fazla veya daha az hassas olarak değiştirebilirsiniz; size kimlerin e-posta göndermesi gerektiğini de değiştirebilirsiniz.

    Bir uyarı ortaya çıktığında çağrılan bir [web kancası](../azure-portal/insights-webhooks-alerts.md) ayarlayabilirsiniz. (Ancak şu anda sorgu parametreleri Özellikler aracılığıyla geçirilmez.)

### Daha fazla URL test etme

Daha fazla test ekleyin. Örneğin, giriş sayfanızın test edilmesinin yanı sıra, arama URL’sini de test ederek veritabanınızın çalıştığından emin olursunuz.


## <a name="monitor"></a>3. Web testi sonuçlarınıza bakın

1-2 dakika sonra sonuçları şurada görüntülenir: 

![Giriş dikey penceresinde özet sonuçları](./media/app-insights-monitor-web-app-availability/14-availSummary.png)

Bu döneme ait daha ayrıntılı bir görünüm için özet grafiğin herhangi bir çubuğuna tıklayın.

Bu grafikler, bu uygulamanın tüm web testleri için sonuçları birleştirir.


## <a name="failures"></a>Hata görürseniz

Kırmızı noktaya tıklayın.

![Kırmızı noktaya tıklama](./media/app-insights-monitor-web-app-availability/14-availRedDot.png)

Bunun yerine, ekranı kaydırıp %100 başarı değerinden küçük olduğunu gördüğünüz bir teste de tıklayabilirsiniz.

![Belirli bir web testine tıklama](./media/app-insights-monitor-web-app-availability/15-webTestList.png)

Test sonuçları açılır.

![Belirli bir web testine tıklama](./media/app-insights-monitor-web-app-availability/16-1test.png)

Test çeşitli konumlardan çalıştırılır; sonucu %100'den küçük olan birini seçin.

![Belirli bir web testine tıklama](./media/app-insights-monitor-web-app-availability/17-availViewDetails.png)


Aşağı kaydırarak **Başarısız testler**’e gidip bir sonuç seçin.

Portalda değerlendirmek ve neden başarısız olduğun görmek için sonuca tıklayın.

![Web testi çalıştırma sonucu](./media/app-insights-monitor-web-app-availability/18-availDetails.png)


Alternatif olarak, sonuç dosyasını indirip Visual Studio’da inceleyebilirsiniz.


*Sorunsuz görünüyor, ancak hata olarak mı bildiriliyor?* Tüm görüntüleri, betikleri, stil sayfalarını ve sayfa tarafından yüklenen diğer dosyaları denetleyin. Herhangi biri başarısızsa, ana html sayfası Tamam olarak yüklense bile test başarısız olarak raporlanır.



## Çok adımlı web testleri

Bir dizi URL'nin bulunduğu bir senaryoyu izleyebilirsiniz. Örneğin, bir satış web sitesi izliyorsanız, öğelerin alışveriş sepetine doğru eklendiğini test edebilirsiniz.

Çok adımlı bir test oluşturmak için Visual Studio’yu kullanarak senaryoyu kaydedin ve kaydı Application Insights'a yükleyin. Application Insights, senaryoyu aralıklarla yeniden yürütür ve yanıtları doğrular.

Testlerinizde kodlanmış işlevleri kullanamadığınızı unutmayın: senaryo adımları .webtest dosyasında betik olarak yer almalıdır.

#### 1. Senaryo kaydetme

Web oturumu kaydetmek için Visual Studio Enterprise veya Ultimate kullanın. 

1. Web performans testi projesi oluşturun.

    ![Visual Studio’da, Web Performansı ve Yük Testi şablonundan bir proje oluşturun.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

2. .webtest dosyasını açın ve kaydı başlatın.

    ![.webtest dosyasını açın ve Kaydet’e tıklayın.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)

3. Testinizde benzetimini yapmak istediğiniz kullanıcı işlemlerini yapın: web sitenizi açın, sepete ürün ekleyin ve bunlara devam edin. Sonra testinizi durdurun.

    ![Internet Explorer'da web testi kaydedicisi çalışır.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    Uzun bir senaryo oluşturmayın. 100 adımlık ve 2 dakikalık bir sınır vardır.

4. Testi düzenleme nedenleri:
 - Alınan metin ve yanıt kodlarını denetlemek için doğrulama ekleme.
 - Gereksiz tüm etkileşimleri kaldırma. Ayrıca, resim veya reklam için ya da sitelerin izlenmesi için bağımlı istekleri kaldırabilirsiniz.

    Yalnızca test betiğini düzenleyebildiğinizi, özel kod veya başka web testlerinden çağrı ekleyemediğinizi unutmayın. Testlere döngü eklemeyin. Standart web testi eklentileri kullanabilirsiniz.

5. Çalıştığından emin olmak için testi Visual Studio’da çalıştırın.

    Web test çalıştırıcısı bir web tarayıcısı açar ve kaydettiğiniz eylemleri yineler. Beklediğiniz gibi çalıştığından emin olun.

    ![Visual Studio’da .webtest dosyasını açın ve Çalıştır’a tıklayın.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)


#### 2. Web testi Application Insights'a yükleme

1. Application Insights portalında yeni bir web testi oluşturun.

    ![Web testleri dikey penceresinde Ekle'yi seçin.](./media/app-insights-monitor-web-app-availability/16-another-test.png)

2. Çok adımlı testi seçip .webtest dosyasını yükleyin.

    ![Çok adımlı web testini seçin.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    Test konumları, sıklığı ve uyarı parametrelerini aynı ping testlerinde olduğu gibi aynı şekilde ayarlayın.

Tek url testlerinde olduğu gibi test sonuçlarını ve hatalarını görüntüleyin.

Yaygın bir başarısızlık nedeni testin çok uzun çalışmasıdır. İki dakikadan uzun çalıştırılmamalıdır.

Betikler, stil sayfaları, görüntüler ve diğerleri de dahil olmak üzere, testin başarılı olması için sayfanın tüm kaynaklarının doğru yüklenmiş olması gerektiğini unutmayın.

Web testinin tamamen .webtest dosyasında olması gerektiğini unutmayın. Testte kodlanmış işlevleri kullanamazsınız.


### Çok adımlı testinizde bağlı kalma süresi ve rasgele rakamlar

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

## Oturum açmayla ilgilenme

Kullanıcılarınız uygulamanızda oturum açarsa, oturum açma benzetimi için bir dizi seçeneğiniz vardır; böylece, oturum açmanın ötesinde sayfaları test edebilirsiniz. Kullandığınız yaklaşım, uygulamanın sağladığı güvenlik türüne bağlıdır.

Her durumda, uygulamanızda yalnızca test amacıyla bir hesap oluşturmalısınız. Mümkünse, web testlerinin gerçek kullanıcıları etkileme olasılığını önlemek için test hesabının izinlerini kısıtlayın.

### Basit kullanıcı adı ve parola

Web testini normal şekilde kaydedin. Önce tanımlama bilgilerini silin.

### SAML kimlik doğrulaması

Web testlerinde kullanıma uygun SAML eklentisini kullanın.

### Gizli anahtar

Uygulamanızda gizli anahtar içeren bir oturum açma yolu varsa bu yolu kullanın. Azure Active Directory (AAD), gizli anahtarla oturum açmayı sağlayan bir hizmet örneğidir. AAD’de gizli anahtar, Uygulama Anahtarı’dır. 

Aşağıda uygulama anahtarı kullanan bir Azure web uygulaması için web testi örneği verilmiştir:

![Gizli anahtar örneği](./media/app-insights-monitor-web-app-availability/110.png)

1. Gizli anahtar (AppKey) kullanarak AAD’den belirteç alın.
2. Yanıttan taşıyıcı belirteci ayıklayın.
3. Yetkilendirme üst bilgisinde taşıyıcı belirteç kullanarak API çağırın.

Web testinin gerçek bir istemci olduğundan, yani AAD’de kendi uygulamasına sahip olduğundan emin olun ve bu istemcinin clientId’si ile appkey’ini kullanın. Test edilen hizmetiniz, AAD içinde kendi uygulamasına sahiptir: bu uygulamanın appID URI’si, web testinin “kaynak” alanında yansıtılır. 

### Açık Kimlik Doğrulaması

Microsoft veya Google hesabınızla oturum açma, bir açık kimlik doğrulaması örneğidir. OAuth kullanan çok sayıda uygulama, alternatif gizli anahtar da sağlar; bu nedenle ilk taktiğiniz bu olasılığın incelenmesi olmalıdır. 

Testinizde OAuth kullanılarak oturum açılması gerekiyorsa, genel yaklaşım şöyledir:

 * Web tarayıcınız, kimlik doğrulama sitesi ve uygulamanız arasındaki trafiği incelemek için Fiddler gibi bir araç kullanın. 
 * Farklı makineler veya tarayıcılar kullanarak veya uzun aralıklarla (süresi dolacak şekilde belirteçleri izin vermek için) iki veya daha fazla oturum açın.
 * Farklı oturumları karşılaştırarak, kimlik doğrulama sitesinden geri geçirilen belirteci tanımlayın; başka bir deyişle oturum açıldıktan sonra uygulama sunucunuza geçirilen belirteç. 
 * Visual Studio’yu kullanarak web testini kaydetme 
 * Belirteçleri parametreleyin; belirteç kimlik doğrulayıcıdan döndürüldüğünde ve sitede sorgu sırasında kullanıldığında parametre ayarı.
 (Visual Studio testi parametrelemeyi dener, ancak belirteçleri doğru parametrelemez.)


## <a name="edit"></a> Testi düzenleme veya devre dışı bırakma

Düzenlemek veya devre dışı bırakmak için bir test açın.

![Web testini düzenleme veya devre dışı bırakma](./media/app-insights-monitor-web-app-availability/19-availEdit.png)

Hizmetinizde bakımı gerçekleştirirken web testlerini devre dışı bırakmak isteyebilirsiniz.

## Performans testleri

Web sitenizde bir yük testi çalıştırabilirsiniz. Kullanılabilirlik testinde olduğu gibi dünyanın dört bir yanındaki noktalarımızdan basit istekler ya da çok adımlı istekler gönderebilirsiniz. Kullanılabilirlik testinden farklı olarak eşzamanlı birden fazla kullanıcıyı benzeten çok sayıda istek gönderilir.

Genel Bakış dikey penceresinde **Ayarlar**, **Performans Testleri**’ni açın. Bir test oluşturduğunuzda Visual Studio Team Services hesabı oluşturmaya davet edilirsiniz. 

Test tamamlandığında yanıt süreleri ve başarı oranları gösterilir.


## Automation

* Otomatik olarak [web testini ayarlamak için PowerShell betiklerini kullanın](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/). 
* Bir uyarı ortaya çıktığında çağrılan bir [web kancası](../azure-portal/insights-webhooks-alerts.md) ayarlayın.

## Sorularınız mı var? Sorunlarınız mı var?

* *Web testimden kod çağırabilir miyim?*

    Hayır. Test adımları .webtest dosyasında olmalıdır. Bu nedenle, başka web testlerini çağıramaz, döngüleri kullanamazsınız. Ancak yararlı bulabileceğiniz bir dizi eklenti vardır.

* *HTTPS destekleniyor mu?*

    TLS 1.1 ve TLS 1.2 desteklenir.

* *"Web testleri" ve "kullanılabilirlik testleri" arasında bir fark var mı?*

    Bu iki terimi birbirlerinin yerine kullanırız.

* *Kullanılabilirlik testlerini, güvenlik duvarının arkasında çalışan kendi dahili sunucumuzda kullanmayı tercih ediyorum.*

    Güvenlik duvarınızı, [Web testi aracılarının IP adreslerinden](app-insights-ip-addresses.md#availability) gelen isteklere izin verecek şekilde yapılandırın.

* *Çok adımlı web testi yüklenemiyor*

    300 K boyut sınırı vardır.

    Döngüler desteklenmez.

    Başka web testlerine başvurular desteklenmez.

    Veri kaynakları desteklenmez.

    
* *Çok adımlı testim tamamlanmadı*

    Test başına 100 istek sınırı var.

    Test, iki dakikadan uzun çalışırsa durdurulur.

* *İstemci sertifikasıyla testi nasıl çalıştırırım?*

    Üzgünüz, bunu desteklemiyoruz.


## <a name="video"></a>Video

> [AZURE.VIDEO monitoring-availability-with-application-insights]

## <a name="next"></a>Sonraki adımlar

[Tanılama günlüklerini arama][diagnostic]

[Sorun giderme][qna]

[Web testi aracılarının IP adresleri](app-insights-ip-addresses.md)


<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md



<!--HONumber=sep16_HO1-->


