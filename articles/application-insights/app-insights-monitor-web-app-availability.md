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
    ms.date="05/20/2016"
    ms.author="awills"/>

# Web sitelerinin kullanılabilirlik ve yanıt hızını izleme


[AZURE.INCLUDE [app-insights-selector-get-started-dotnet](../../includes/app-insights-selector-get-started-dotnet.md)]

Web uygulamanızı dağıttıktan sonra kullanılabilirlik ve yanıt hızını izlemek için web testleri ayarlayabilirsiniz. Application Insights dünyanın her yerindeki noktalarından düzenli aralıklarla web istekleri gönderir; uygulamanız da yavaş yanıtlarsa ya da hiç yanıtlamazsa sizi uyarabilir.

![Web testi örneği](./media/app-insights-monitor-web-app-availability/appinsights-10webtestresult.png)

Genel İnternet'ten erişilebilen herhangi bir HTTP veya HTTPS uç noktası için web testi ayarlayabilirsiniz.

İki tür web testi bulunur:

* [URL ping testi](#set-up-a-url-ping-test): Azure portalında oluşturabileceğiniz basit bir test.
* [Çok adımlı web testi](#multi-step-web-tests): Visual Studio Ultimate veya Visual Studio Enterprise’da oluşturup portala yüklediğiniz test.

Her uygulama kaynağı için 10 web testine kadar test oluşturabilirsiniz.


## URL ping testi ayarlama

### <a name="create"></a>1. Yeni kaynak oluşturulsun mu?

Bu uygulama için zaten [Application Insights kaynağı ayarladıysanız][start] ve kullanılabilirlik verilerini aynı yerde görmek istiyorsanız bu adımı atlayın.

[Microsoft Azure](http://azure.com) oturumu açın, [Azure portal](https://portal.azure.com)’a gidin ve yeni bir Application Insights kaynağı oluşturun.

![Yeni > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Yeni kaynakla ilgili Genel Bakış dikey penceresi açılır. Her istediğinizde bunu [Azure portalda](https://portal.azure.com) bulmak için **Gözat**’a tıklayın.

### <a name="setup"></a>2. Web testi oluşturma

Application Insights kaynağınızda Kullanılabilirlik kutucuğunu arayın. Uygulamanızla ilgili Web testleri dikey penceresini açmak için tıklayın ve bir web testi ekleyin.

![En azından web sitenizin URL'sini doldurma](./media/app-insights-monitor-web-app-availability/13-availability.png)

- **URL** ortak internet'ten görünür olmalıdır. Bir sorgu dizesi içerebilir; bu nedenle, örneğin, veritabanınızla biraz alıştırma yapabilirsiniz. URL yeniden yönlendirme adresine çözümlenirse, en fazla 10 yeniden yönlendirmeyi izleyeceğiz.
- **Bağımlı istekleri ayrıştır**: Görüntüler, betikler, stil dosyaları ve sayfanın diğer kaynakları testin bir parçası olarak istenir. Testin tamamının zaman aşımı süresi içinde tüm bu kaynaklar sorunsuz yüklenemezse test başarısız olur.
- **Yeniden denemeyi etkinleştir**: Test başarısız olduğunda, kısa bir süre sonra yeniden denenir. Art arda üç deneme başarısız olursa bir hata bildirilir. Sonraki testler bundan sonra her zamanki test sıklığında gerçekleştirilir. Bir sonraki başarılı olana kadar yeniden deneme geçici olarak askıya alınır. Bu kural her test konuma bağımsız olarak uygulanır. (Bu ayarı öneriyoruz. Ortalama olarak hataların yaklaşık %80’i yeniden deneme sırasında kaybolur.)
- **Test sıklığı**: Her test konumdan testin ne sıklıkta çalıştırılacağını ayarlar. 5 dakikalık bir sıklık ve beş test konumuyla siteniz ortalama olarak dakikada bir test edilir.
- **Test konumları**, sunucularımızın URL’nize web istekleri gönderdiği yerlerdir. Bir konumdan fazla seçin; böylece, web sitenizdeki ağ sorunlarını ayırt edebilirsiniz. En fazla 16 konum seçebilirsiniz.

- **Başarı ölçütleri**:

    **Test zaman aşımı**: Yavaş yanıtlar hakkında uyarı almak için bunu azaltın. Yanıtlar sitenizden bu süre içinde alınmadıysa test başarısız sayılır. **Bağımlı istekleri ayrıştır**’ı seçerseniz, tüm görüntüler, stil dosyaları, betikler ve diğer bağımlı kaynaklar bu süre içinde alınmalıdır.

    **HTTP yanıtı**: Başarılı sayılan döndürüldü durum kodu. 200, normal web sayfası döndürüldüğünü belirten koddur.

    **İçerik eşleşmesi**: "Hoş geldiniz!" gibi bir dize. Oluştuğu her yanıtta test edeceğiz. Joker karakter bulunmayan düz bir dize olmalıdır. Sayfanızın içeriği değişirse bunu güncelleştirmeniz gerektiğini unutmayın.


- **Uyarılar**, varsayılan olarak, beş dakikayı geçen bir sürede üç konumda hata varsa size gönderilir. Tek konumdaki hata daha çok bir ağ sorunudur, sitenizle ilgili değildir. Ancak, eşiği daha fazla veya daha az hassas olarak değiştirebilirsiniz; size kimlerin e-posta göndermesi gerektiğini de değiştirebilirsiniz.

    Bir uyarı ortaya çıktığında çağrılan bir [web kancası](../azure-portal/insights-webhooks-alerts.md) ayarlayabilirsiniz.

#### Daha fazla URL test etme

Daha fazla test ekleyin. Örneğin, giriş sayfanızın test edilmesinin yanı sıra, arama URL’sini de test ederek veritabanınızın çalıştığından emin olursunuz.


### <a name="monitor"></a>3. Kullanılabilirlik raporlarını görüntüleme

1-2 dakika sonra, kullanılabilirlik/web testi dikey penceresinde **Yenile**’ye tıklayın. (Otomatik olarak yenilenmez.)

![Giriş dikey penceresinde özet sonuçları](./media/app-insights-monitor-web-app-availability/14-availSummary.png)

Bu döneme ait daha ayrıntılı bir görünüm için özet grafiğin üstteki herhangi bir çubuğuna tıklayın.

Bu grafikler, bu uygulamanın tüm web testleri için sonuçları birleştirir.

#### Web sayfanızın bileşenleri

Görüntüler, stil sayfaları, betikler ve test ettiğiniz web sayfasının diğer statik bileşenleri testin bir parçası olarak istenir.  

Kaydedilen yanıt süresi yüklemenin tamamlanması için tüm bileşenlerle ilgili geçen süredir.

Herhangi bir bileşen yüklenemezse test başarısız olarak işaretlenir.

## <a name="failures"></a>Hata görürseniz...

Kırmızı noktaya tıklayın.

![Kırmızı noktaya tıklama](./media/app-insights-monitor-web-app-availability/14-availRedDot.png)

Bunun yerine, ekranı kaydırıp %100 başarı değerinden küçük olduğunu gördüğünüz bir teste de tıklayabilirsiniz.

![Belirli bir web testine tıklama](./media/app-insights-monitor-web-app-availability/15-webTestList.png)

Böylece, bu testle ilgili sonuçları görürsünüz.

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

Çok adımlı bir test oluşturmak için Visual Studio’yu kullanarak senaryoyu kaydedin ve kaydı Application Insights'a yükleyin. Application Insights senaryoyu aralıklarla yanıtlayıp yanıtları doğrulayacaktır.

Testlerinizde kodlanmış işlevleri kullanamadığınızı unutmayın: senaryo adımları .webtest dosyasında betik olarak yer almalıdır.

#### 1. Senaryo kaydetme

Web oturumu kaydetmek için Visual Studio Enterprise veya Ultimate kullanın. 

1. Web performans testi projesi oluşturun.

    ![Visual Studio'da, Web Performansı ve Yük Testi şablonundan yeni bir proje oluşturun.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

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

Betikler, stil sayfaları, görüntüler ve diğerleri de aralarında olmak üzere testin başarılı olması için sayfanın tüm kaynaklarının doğru yüklenmiş olması gerektiğini unutmayın.

Web testinin tamamen .webtest dosyasında olması gerektiğini unutmayın. Testte kodlanmış işlevleri kullanamazsınız.


### Çok adımlı testinizde bağlı kalma süresi ve rasgele rakamlar

Dış bir kaynağa ait stoklar gibi zamana bağımlı veriler alan bir aracı test ettiğinizi varsayalım. Web testinizi kaydettiğinizde, belirli zamanları kullanmanız gerekse de, bunları testin parametreleri (StartTime ve EndTime) olarak ayarlarsınız.

![Parametrelere sahip web testi.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

Testi çalıştırdığınızda, EndTime her zaman geçerli zaman, StartTime da 15 dakika öncesi olmalıdır.

Web Testi Eklentileri bunu yapmanın bir yolunu sağlar.

1. İstediğiniz her değişken parametre değeri için bir web testi eklentisi ekleyin. Web testi araç çubuğunda, **Web Testi Eklentisi Ekle**’yi seçin.

    ![Web Testi Eklentisi Ekle’yi, sonra da bir türü seçin.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    Bu örnekte, Tarih Saat Eklentisinin iki örneğini kullanacağız. "15 dakika önce" için biri, "şimdi" için de bir başkası.

2. Her eklentinin özelliklerini açın. Buna bir ad verip geçerli saat olarak kullanılmak üzere ayarlayın. Bunlardan birini Dakika Ekle = 15 olarak ayarlayın.

    ![Adı, Geçerli Saati Kullan’ı ve Dakika Ekle’yi ayarlayın.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)

3. Web testi parametrelerinde, eklenti adına başvurmak için {{plug-in name}} kullanın.

    ![Test parametresinde {{plug-in name}} kullanın.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

Artık testi portala yükleyin. Testin her çalıştırılışında dinamik değerler kullanacaktır.

## Oturum açmayla ilgilenme

Kullanıcılarınız uygulamanızda oturum açarsa, oturum açma benzetimi için bir dizi seçeneğiniz vardır; bu nedenle, oturum açmanın ötesinde sayfaları test edebilirsiniz. Kullandığınız yaklaşım, uygulamanın sağladığı güvenlik türüne bağlıdır.

Her durumda, yalnızca test amacıyla bir hesap oluşturmanız gerekir. Olabiliyorsa, izinlerini salt okunur olacak şekilde kısıtlayın.

* Basit kullanıcı adı ve parola: Normal yollardan web testini kaydetmek yeterlidir. Önce tanımlama bilgilerini silin.
* SAML kimlik doğrulaması Bunun için, web testlerinde kullanıma uygun SAML eklentisini kullanabilirsiniz.
* İstemci parolası: uygulamanızda istemci parolasını içeren bir oturum açma yolu varsa bunu kullanın. Azure Active Directory bunu sağlar. 
* Açık Kimlik Doğrulaması - örneğin, Microsoft veya Google hesabınızla oturum açma. OAuth kullanan çok sayıda uygulama alternatif istemci parolası da sağlar; bu nedenle ilk taktik bunun incelenmesidir. Testinizde OAuth kullanılarak oturum açılmışsa genel yaklaşım şöyledir:
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

## Automation

* Otomatik olarak [web testini ayarlamak için PowerShell betiklerini kullanın](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/). 
* Bir uyarı ortaya çıktığında çağrılan bir [web kancası](../azure-portal/insights-webhooks-alerts.md) ayarlayın.

## Sorularınız mı var? Sorunlarınız mı var?

* *Web testimden kod çağırabilir miyim?*

    Hayır. Test adımları .webtest dosyasında olmalıdır. Bu nedenle, başka web testlerini çağıramaz, döngüleri kullanamazsınız. Ancak yararlı bulabileceğiniz bir dizi eklenti vardır.

* *HTTPS destekleniyor mu?*

    Şu anda SSL 3.0 ve TLS 1.0 sürümlerini destekliyoruz.

* *"Web testleri" ve "kullanılabilirlik testleri" arasında bir fark var mı?*

    Bu iki terimi birbirlerinin yerine kullanırız.

* *Kullanılabilirlik testlerini, güvenlik duvarının arkasında çalışan kendi dahili sunucumuzda kullanmayı tercih ediyorum.*

    Bu makalenin sonundaki listede yer alan IP adreslerinden isteklere izin vermesi için güvenlik duvarınızı yapılandırın.

* *Çok adımlı web testi yüklenemiyor*

    300K boyut sınırı var.

    Döngüler desteklenmez.

    Başka web testlerine başvurular desteklenmez.

    Veri kaynakları desteklenmez.

    
* *Çok adımlı testim tamamlanmadı*

    Test başına 100 istek sınırı var.

    İki dakikadan uzun çalışırsa test durdurulacak.

* *İstemci sertifikasıyla testi nasıl çalıştırırım?*

    Üzgünüz, bunu desteklemiyoruz.


## <a name="video"></a>Video

> [AZURE.VIDEO monitoring-availability-with-application-insights]

## <a name="next"></a>Sonraki adımlar

[Tanılama günlüklerini arama][diagnostic]

[Sorun giderme][qna]


## web testlerinin IP adresleri

Web testlerine izin vermek için güvenlik duvarını açmanız gerekirse, geçerli IP adreslerinin listesi burada verilmiştir. Zaman zaman değiştirilebilir.

80 (http) ve 443 (https) bağlantı noktalarını açın.

```

213.199.178.54
213.199.178.55
213.199.178.56
213.199.178.61
213.199.178.57
213.199.178.58
213.199.178.59
213.199.178.60
213.199.178.63
213.199.178.64
207.46.98.158
207.46.98.159
207.46.98.160
207.46.98.157
207.46.98.152
207.46.98.153
207.46.98.156
207.46.98.162
207.46.98.171
207.46.98.172
65.55.244.40
65.55.244.17
65.55.244.42
65.55.244.37
65.55.244.15
65.55.244.16
65.55.244.44
65.55.244.18
65.55.244.46
65.55.244.47
207.46.14.60
207.46.14.61
207.46.14.62
207.46.14.55
207.46.14.63
207.46.14.64
207.46.14.51
207.46.14.52
207.46.14.56
207.46.14.65
157.55.14.60
157.55.14.61
157.55.14.62
157.55.14.47
157.55.14.64
157.55.14.65
157.55.14.43
157.55.14.44
157.55.14.49
157.55.14.50
65.54.66.56
65.54.66.57
65.54.66.58
65.54.66.61
207.46.71.54
207.46.71.52
207.46.71.55
207.46.71.38
207.46.71.51
207.46.71.57
207.46.71.58
207.46.71.37
202.89.228.67
202.89.228.68
202.89.228.69
202.89.228.57
65.54.78.49
65.54.78.50
65.54.78.51
65.54.78.54
94.245.82.32
94.245.82.33
94.245.82.37
94.245.82.38
94.245.72.44
94.245.72.45
94.245.72.46
94.245.72.49
207.46.56.57
207.46.56.58
207.46.56.59
207.46.56.67
207.46.56.61
207.46.56.62
207.46.56.63
207.46.56.64
65.55.82.84
65.55.82.85
65.55.82.86
65.55.82.81
65.55.82.87
65.55.82.88
65.55.82.89
65.55.82.90
65.55.82.91
65.55.82.92
94.245.78.40
94.245.78.41
94.245.78.42
94.245.78.45
70.37.147.43
70.37.147.44
70.37.147.45
70.37.147.48
94.245.66.43
94.245.66.44
94.245.66.45
94.245.66.48

```


<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md



<!--HONumber=Jun16_HO2-->


