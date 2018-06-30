---
title: Veri saklama ve depolama Azure Application ınsights'ta | Microsoft Docs
description: Bekletme ve gizlilik ilkesi bildirimi
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: a6268811-c8df-42b5-8b1b-1d5a7e94cbca
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: mbullwin
ms.openlocfilehash: 897671ef592ac691402a4e452f7a0baa04aa228a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129066"
---
# <a name="data-collection-retention-and-storage-in-application-insights"></a>Application Insights ile veri toplama, tutma ve depolama


Yüklediğinizde [Azure Application Insights] [ start] buluta uygulamanız hakkında telemetri gönderir SDK'sı, uygulamanızda. Doğal olarak, sorumlu geliştiriciler tam olarak hangi verilerin gönderilir, verilere ne olur ve nasıl denetimini tutabilirsiniz bilmek ister. Özellikle de hassas verileri gönderilen, depolanan ve ne kadar güvenli olduğu nedir? 

İlk olarak, kısa yanıtı:

* Hizmete hassas verileri göndermek "kutu dışı" çalışan standart telemetri modülleri düşüktür. Telemetri yük, performans ve kullanım ölçümleri, özel raporlar ve diğer Tanılama verileri ile ilgilidir. Tanılama raporlarında görünür durumda ana kullanıcı verilerini URL'leri; yine de uygun istiyor musunuz? Ancak, uygulamanızın bir URL düz metinde hassas verileri her durumda put döndürmemelidir.
* Tanılama ve izleme kullanım yardımcı olmak için ek özel telemetri gönderen kod yazabilirsiniz. (Bu genişletilebilirlik harika Application Insights özelliğidir.) Bu yanlışlıkla, kişisel ve diğer hassas verileri içeren bu kod yazmayı mümkün olacaktır. Uygulamanız bu tür veriler ile çalışıyorsa, tüm yazdığınız kodları için kapsamlı gözden geçirme işlemleri uygulamalıdır.
* Geliştirme ve uygulamanızı test ederken ne SDK tarafından gönderilen incelemek kolaydır. Veri hata ayıklama çıktı pencerelerinde IDE ve tarayıcı görüntülenir. 
* Veri tutulur [Microsoft Azure](http://azure.com) ABD veya Avrupa sunucuları. (Ancak uygulamanızı her yerden çalıştırabilirsiniz.) Azure sahip [güçlü güvenlik işler ve çok çeşitli uyumluluk standartlarını karşılayan](https://azure.microsoft.com/support/trust-center/). Yalnızca sizin ve ekibinizin belirlenen verilerinize erişimi. Microsoft personeli erişimi yalnızca belirli koşullarda sınırlı bilginiz dahilinde engellemiş. Ancak sunucuları değil, aktarım sırasında şifrelenir.

Bu makalenin geri kalanında daha tam olarak bu yanıtları elaborates. Böylece hemen ekibinizin parçası olmayan iş arkadaşlarınızı Göster kendi içinde olacak şekilde tasarlanmıştır.

## <a name="what-is-application-insights"></a>Application Insights nedir?
[Azure Application Insights] [ start] yardımcı olan Microsoft tarafından sağlanan bir hizmeti geliştirmek, Canlı uygulamanızın kullanılabilirlik ve performans değil. Uygulamanız, test sırasında hem yayımlanan veya dağıtılmış sonra çalıştığı her zaman izler. Application Insights grafikler ve, örneğin Göster tabloları, kullanıcıların çoğunun Al ne zaman günün, nasıl yanıt vereceğini uygulama olur ve ne kadar iyi bağımlı herhangi bir dış hizmetler tarafından sunulur oluşturur. Kilitlenmeler, hatalar veya performans sorunları varsa, nedenini tanılamak için ayrıntılı telemetri verilerde arama yapabilirsiniz. Ve hizmetin kullanılabilirliğini ve performansını, uygulamanızın değişiklikler varsa e-posta gönderir.

Bu işlev alabilmek için kendi kod parçası haline gelir, uygulamanızda bir Application Insights SDK'sı yükleyin. Uygulamanızı çalıştırırken, SDK, işlemi izler ve telemetri Application Insights hizmetine gönderir. Bu tarafından barındırılan bir bulut hizmetidir [Microsoft Azure](http://azure.com). (Ancak tüm uygulamaları, Azure üzerinde barındırılan olanlar yalnızca Application Insights çalışır.)

![Uygulamanıza SDK'sı telemetri Application Insights hizmetine gönderir.](./media/app-insights-data-retention-privacy/01-scheme.png)

Application Insights hizmeti depolar ve telemetri analiz eder. Analiz veya depolanan telemetri arama görmek için Azure hesabınızda oturum açın ve uygulamanız için Application Insights kaynağı açın. Verilere erişim de, diğer ekibinizin üyeleri veya belirtilen Azure aboneleri da paylaşabilirsiniz.

Örneğin bir veritabanına veya harici araçlar Application Insights hizmetinden dışarı veri olabilir. Her aracı hizmetinden elde özel bir anahtar sağlar. Anahtar gerekirse iptal edilebilir. 

Uygulama Insights SDK'ları bir dizi uygulama türleri için kullanılabilir: web kendi J2EE veya ASP.NET sunucusu veya Azure; barındırılan hizmetleri istemciler - diğer bir deyişle, bir web sayfasında çalışan kodu web; Masaüstü uygulamaları ve Hizmetleri; Windows Phone, iOS ve Android gibi cihaz uygulamaları. Bunların tümü aynı hizmet olarak telemetri gönderin.

## <a name="what-data-does-it-collect"></a>Hangi veri toplamak?
### <a name="how-is-the-data-is-collected"></a>Verileri nasıl yapıldığını toplanır?
Üç veri kaynağına vardır:

* Ya da uygulamanızla tümleştirin SDK [geliştirme](app-insights-asp-net.md) veya [çalışma zamanında](app-insights-monitor-performance-live-website-now.md). Farklı uygulama türleri için farklı Sdk'ler vardır. Ayrıca bir [SDK web sayfaları için](app-insights-javascript.md), son kullanıcının tarayıcıda sayfa birlikte yükler.
  
  * Her SDK çeşitli sahip [modülleri](app-insights-configuration-with-applicationinsights-config.md), farklı tür telemetri toplamak için farklı tekniklerini kullanın.
  * SDK geliştirme yüklerseniz, standart modüller yanı sıra kendi telemetrinizi göndermek için kendi API kullanabilirsiniz. Bu özel telemetri göndermek istediğiniz herhangi bir veri içerebilir.
* Bazı web sunucuları da uygulamayı çalıştırın ve CPU, bellek ve ağ doluluk hakkında telemetri göndermesine aracılar vardır. Docker ana bilgisayarları, örneğin, Azure Vm'leri ve [J2EE sunucuları](app-insights-java-agent.md) bu tür aracılar olabilir.
* [Kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md) işlemleri düzenli aralıklarla web uygulamanıza istekleri gönderme Microsoft tarafından çalıştırılır. Sonuçları Application Insights hizmetine gönderilir.

### <a name="what-kinds-of-data-are-collected"></a>Ne tür veriler toplanır?
Ana kategoriler şunlardır:

* [Web sunucusu telemetri](app-insights-asp-net.md) -HTTP istekleri.  URI, istek, yanıt kodu, istemci IP adresi işlenmesi için geçen süre. Oturum kimliği.
* [Web sayfaları](app-insights-javascript.md) -sayfası, kullanıcı ve oturum sayısı. Sayfa yükleme sürelerinin. Özel durumlar. AJAX çağrıları.
* Performans sayaçları - bellek, CPU, IO, ağ doluluk.
* İstemci ve sunucu bağlamı - OS, yerel ayar, cihaz türü, tarayıcı, ekran çözünürlüğü.
* [Özel durumlar](app-insights-asp-net-exceptions.md) ve çökme (Crash) - **yığın dökümleri**, yapı kimliği, CPU türü. 
* [Bağımlılıklar](app-insights-asp-net-dependencies.md) -REST, SQL, AJAX gibi dış hizmetler çağrıları. URI veya bağlantı dizesi, süresi, başarı, komutu.
* [Kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md) -test ve adımları, yanıt süresi.
* [İzleme günlükleri](app-insights-asp-net-trace-logs.md) ve [özel telemetri](app-insights-api-custom-events-metrics.md) - **günlükleri veya telemetri kod herhangi bir şey**.

[Daha fazla ayrıntı](#data-sent-by-application-insights).

## <a name="how-can-i-verify-whats-being-collected"></a>Nasıl ne toplanan doğrulayabilirsiniz?
Visual Studio kullanarak uygulama geliştiriyorsanız, uygulamayı (F5) hata ayıklama modunda çalıştırın. Telemetri çıktı penceresinde görüntülenir. Buradan, kopyalamak ve kolay İnceleme için JSON olarak biçimlendirin. 

![](./media/app-insights-data-retention-privacy/06-vs.png)

Tanılama penceresinde daha okunabilir bir görünüm bulunmaktadır.

Web sayfaları için tarayıcınızın hata ayıklama penceresini açın.

![F12 tuşuna basın ve ağ sekmesini açın.](./media/app-insights-data-retention-privacy/08-browser.png)

### <a name="can-i-write-code-to-filter-the-telemetry-before-it-is-sent"></a>Gönderilmeden önce telemetri filtrelemek için kod yazma?
Bu yazarak sağlayabileceğinizden bir [telemetri işlemci eklentisi](app-insights-api-filtering-sampling.md).

## <a name="how-long-is-the-data-kept"></a>Ne kadar veri Tutuluyor?
Ham veri noktaları (diğer bir deyişle, analizleri sorgulamak ve aramada incelemek öğeleri) 90 gün boyunca tutulur. Daha uzun verileri tutmak gerekiyorsa, kullanabileceğiniz [sürekli verme](app-insights-export-telemetry.md) bir depolama hesabına kopyalamak için.

Toplanan veriler (diğer bir deyişle, sayıları, ortalamalar ve ölçüm Gezgininde gördüğünüz diğer istatistik bilgileri), 1 dakika 90 gün boyunca bir çizgisi adresindeki korunur.

## <a name="who-can-access-the-data"></a>Verilere kimler erişebilir?
Verileri için görünür ve ekip üyelerinin bir kuruluş hesabı varsa. 

Bunu siz ve ekip üyelerinizin tarafından verilmesi ve başka konumlara kopyalanabilir ve diğer kişilere geçirildi.

#### <a name="what-does-microsoft-do-with-the-information-my-app-sends-to-application-insights"></a>Microsoft Application Insights'a Uygulamam gönderir bilgilerle ne yapar?
Microsoft, yalnızca hizmet olanak sağlamak için verileri kullanır.

## <a name="where-is-the-data-held"></a>Verilerin nerede tutulur?
* ABD, Avrupa veya Güneydoğu Asya. Yeni bir Application Insights kaynağı oluşturduğunuzda konumu seçebilirsiniz. 


#### <a name="does-that-mean-my-app-has-to-be-hosted-in-the-usa-europe-or-southeast-asia"></a>Bu, ABD, Avrupa veya Güneydoğu Asya barındırılmasını Uygulamam sahip anlama geliyor?
* Hayır. Uygulamanızı her yerden, kendi şirket içi konak veya Bulut çalıştırabilirsiniz.

## <a name="how-secure-is-my-data"></a>Verilerim nasıl güvenli mi?
Application Insights, bir Azure hizmetidir. Güvenlik ilkeleri bölümünde açıklanmıştır [Azure güvenlik, gizlilik ve uyumluluk incelemeyi](http://go.microsoft.com/fwlink/?linkid=392408).

Verileri Microsoft Azure sunucularda depolanır. Azure Portalı'nda hesapları için hesap kısıtlamaları açıklanmaktadır [belge Azure güvenlik, gizlilik ve Uyumluluk](http://go.microsoft.com/fwlink/?linkid=392408).

Erişim için Microsoft personeli tarafından sınırlandırılır. Biz yalnızca izinle verilerinizi erişmek ve Application Insights kullanımınız desteklemek için gerekli olup olmadığını. 

Bizim müşterilerin tüm uygulamalar (örneğin, veri hızlarını ve ortalama boyutu izlemeleri) toplamında verileri Application Insights geliştirmek için kullanılır.

#### <a name="could-someone-elses-telemetry-interfere-with-my-application-insights-data"></a>Başka birinin telemetri Application Insights verilerimi engelleyebilir mi?
Bunlar, hesabınıza ek telemetri web sayfalarınıza kodda bulunabilir izleme anahtarını kullanarak gönderebilir. Yeterli ek verilerle ölçümlerinizi doğru uygulamanızın performansını ve kullanımını temsil.

Diğer projelerle kod paylaşıyorsanız, izleme anahtarı kaldırmayı unutmayın.

## <a name="is-the-data-encrypted"></a>Veriler şifrelenir mi?
Mevcut sunucularda içinde değil.

Veri merkezleri arasında hareket ederken tüm veriler şifrelenir.

#### <a name="is-the-data-encrypted-in-transit-from-my-application-to-application-insights-servers"></a>Application Insights sunucularına my uygulamasından Aktarımdaki veriler şifrelenir?
Evet, biz web sunucuları, aygıtları ve HTTPS web sayfaları dahil olmak üzere, neredeyse tüm Sdk'lardan portalına veri göndermek için https kullanın. Tek özel durum düz HTTP web sayfalarından gönderilen verilerdir.

## <a name="how-do-i-send-data-to-application-insights-using-tls-12"></a>Nasıl veri Application Insights'a TLS 1.2 kullanarak gönderebilirim?

Application Insights Uç noktalara Aktarımdaki verileri güvenliğinin güvence altına almaya kesinlikle müşterilerin kendi uygulamanızı en az kullanacak biçimde yapılandırmak için öneririz Aktarım Katmanı Güvenliği (TLS) 1.2. TLS/Güvenli Yuva Katmanı (SSL)'ın eski sürümlerini savunmasız bulundu ve bunlar hala şu anda geriye dönük uyumluluk izin vermek için çalışırken, bunlar **önerilmez**, ve endüstri hızla destek bırakmasını taşıma Bu eski protokoller için. 

[PCI güvenlik standartları Council](https://www.pcisecuritystandards.org/) ayarlanmış bir [30 Haziran 2018 son](https://www.pcisecuritystandards.org/pdfs/PCI_SSC_Migrating_from_SSL_and_Early_TLS_Resource_Guide.pdf) eski sürümleri TLS/SSL ve yükseltme daha protokolleri güvenli hale getirmek için devre dışı bırakmak için. Uygulama/istemciler üzerinde en az iletişim kuramıyorsa eski desteği, Azure bırakır sonra TLS 1.2, değil olacaktır Application Insights'a veri gönderebilir. Hangi yaklaşımın sınamak ve uygulamanızın TLS destek doğrulamak için uygulamanızın kullandığı dil/framework yanı sıra işletim sistemi/platform bağlı olarak değişir.

Açıkça sürece yalnızca TLS 1.2 kullanmak için uygulamanızın bu otomatik olarak algılamak ve olduklarında yeni daha güvenli iletişim kuralları yararlanmak izin platform düzeyi güvenlik özellikleri bölünebilir kesinlikle gerekli ayarlanması önerilmez TLS 1.3 gibi kullanılabilir. Kapsamlı denetim belirli TLS/SSL sürümleri cmdlet'e kod için denetlemek için uygulamanızın kodunun gerçekleştirme öneririz.

### <a name="platformlanguage-specific-guidance"></a>Platform/dil özel yönergeler

|Platform/dili | Destek | Daha Fazla Bilgi |
| --- | --- | --- |
| Azure Uygulama Hizmetleri  | Desteklenen yapılandırması gerekli olabilir. | Destek Nisan 2018 tanıtıldı. Duyuru için okuma [yapılandırma ayrıntılarını](https://blogs.msdn.microsoft.com/appserviceteam/2018/04/17/app-service-and-functions-hosted-apps-can-now-update-tls-versions/).  |
| Azure işlevi uygulamaları | Desteklenen yapılandırması gerekli olabilir. | Destek Nisan 2018 tanıtıldı. Duyuru için okuma [yapılandırma ayrıntılarını](https://blogs.msdn.microsoft.com/appserviceteam/2018/04/17/app-service-and-functions-hosted-apps-can-now-update-tls-versions/). |
|.NET | Desteklenen yapılandırma sürümü tarafından değişir. | .NET 4.7 ve önceki sürümler için ayrıntılı yapılandırma bilgileri için bkz [bu yönergeleri](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls#support-for-tls-12).  |
|Durum İzleyicisi | Desteklenen, yapılandırma gerekiyor | Durum İzleyicisi'ni kullanır [işletim sistemi yapılandırması](https://docs.microsoft.com/en-us/windows-server/security/tls/tls-registry-settings) + [.NET Yapılandırması](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls#support-for-tls-12) destek TLS 1.2.
|Node.js |  , V10.5.0 içinde desteklenen yapılandırması gerekli olabilir. | Kullanım [resmi Node.js TLS/SSL belge](https://nodejs.org/api/tls.html) herhangi bir uygulama belirli yapılandırma için. |
|Java | Desteklenen, TLS 1.2 JDK desteği eklenmiştir [JDK 6 güncelleştirme 121](http://www.oracle.com/technetwork/java/javase/overview-156328.html#R160_121) ve [JDK 7](http://www.oracle.com/technetwork/java/javase/7u131-relnotes-3338543.html). | JDK 8'i kullanır [TLS 1.2 varsayılan](https://blogs.oracle.com/java-platform-group/jdk-8-will-use-tls-12-as-default).  |
|Linux | Linux dağıtımları eğilimindedir güvenemeyeceklerini [OpenSSL](https://www.openssl.org) TLS 1.2 desteği.  | Denetleme [OpenSSL değişim günlüğü](https://www.openssl.org/news/changelog.html) OpenSSL sürümünüz desteklenir onaylamak için.|
| Windows 8.0 10 | Desteklenen ve varsayılan olarak etkindir. | Hala kullandığınızı doğrulamak için [varsayılan ayarları](https://docs.microsoft.com/en-us/windows-server/security/tls/tls-registry-settings).  |
| Windows Server 2012 2016 | Desteklenen ve varsayılan olarak etkindir. | Hala kullandığınızı doğrulamak için [varsayılan ayarları](https://docs.microsoft.com/en-us/windows-server/security/tls/tls-registry-settings) |
| Windows 7 SP1 ve Windows Server 2008 R2 SP1 | , Varsayılan olarak etkin değildir ancak desteklenir. | Bkz: [Aktarım Katmanı Güvenliği (TLS) kayıt defteri ayarları](https://docs.microsoft.com/en-us/windows-server/security/tls/tls-registry-settings) nasıl etkinleştirileceği hakkında ayrıntılar için sayfa.  |
| Windows Server 2008 SP2 | TLS 1.2 desteği güncelleştirilmesi gerekiyor. | Bkz: [TLS 1.2 desteği eklemek için güncelleştirme](https://support.microsoft.com/help/4019276/update-to-add-support-for-tls-1-1-and-tls-1-2-in-windows-server-2008-s) Windows Server 2008 SP2. |
|Windows Vista | Desteklenmiyor. | Yok

### <a name="check-what-version-of-openssl-your-linux-distribution-is-running"></a>Hangi OpenSSL sürümü Linux dağıtımınız çalışıp çalışmadığını denetle

Olan OpenSSL hangi sürümünün yüklü olduğunu denetlemek için Terminali açın ve çalıştırın:

```terminal
openssl version -a
```

### <a name="run-a-test-tls-12-transaction-on-linux"></a>Linux'ta TLS 1.2 işlem test çalıştırma

Linux sisteminizi TLS 1.2 iletişim kurup kuramadığını görmek için temel bir ön test çalıştırmak için. Terminali açın ve çalıştırın:

```terminal
openssl s_client -connect bing.com:443 -tls1_2
```

## <a name="personal-data-stored-in-application-insights"></a>Application Insights'ta depolanan kişisel veriler

Bizim [Application Insights kişisel veriler makale](app-insights-customer-data.md) ayrıntılı bu sorunu ele alınmaktadır.

#### <a name="can-my-users-turn-off-application-insights"></a>Kullanıcılarım Application Insights kapatabilir miyim?
Doğrudan yönetilemez. Kullanıcılarınızın Application Insights devre dışı bırakma çalışabilir bir anahtar sunuyoruz yok.

Ancak, böyle bir özellik uygulamanızda uygulayabilirsiniz. Tüm SDK telemetri toplamayı devre dışı bırakır bir API ayarı içerir. 

## <a name="data-sent-by-application-insights"></a>Application Insights tarafından gönderilen verileri
SDK'ları platformları arasında farklılık gösterir ve yüklemek için kullanabileceğiniz birçok bileşen vardır. (Başvurmak [Application Insights - genel bakış][start].) Her bileşen farklı veri gönderir.

#### <a name="classes-of-data-sent-in-different-scenarios"></a>Farklı senaryolarda gönderilen veri sınıfları
| Eylem | (Sonraki tabloya bakın) toplanan veri sınıfları |
| --- | --- |
| [.NET web projeye Application Insights SDK ekleme][greenbrown] |Sunucu bağlamı<br/>Çıkarımı yapılan<br/>Performans sayaçları<br/>İstekler<br/>**Özel durumlar**<br/>Oturum<br/>kullanıcılar |
| [IIS üzerinde Durum İzleyicisi yükleme][redfield] |Bağımlılıklar<br/>Sunucu bağlamı<br/>Çıkarımı yapılan<br/>Performans sayaçları |
| [Java web uygulaması için Application Insights SDK ekleme][java] |Sunucu bağlamı<br/>Çıkarımı yapılan<br/>İstek<br/>Oturum<br/>kullanıcılar |
| [Web sayfasına JavaScript SDK'sı ekleme][client] |ClientContext <br/>Çıkarımı yapılan<br/>Sayfa<br/>ClientPerf<br/>AJAX |
| [Varsayılan özellikleri tanımlama][apiproperties] |**Özellikler** tüm standart ve özel olayları hakkında |
| [Çağrı TrackMetric][api] |Sayısal değerler<br/>**Özellikleri** |
| [Çağrı izleme *][api] |Olay adı<br/>**Özellikleri** |
| [Çağrı TrackException][api] |**Özel durumlar**<br/>Yığın Dökümü<br/>**Özellikleri** |
| SDK, veri toplayamazsınız. Örneğin: <br/> -Performans sayacı erişemiyor<br/> -telemetri Başlatıcı özel durumu |SDK tanılama |

İçin [diğer platformlar için SDK'lar][platforms], kendi belgelere bakın.

#### <a name="the-classes-of-collected-data"></a>Toplanan veri sınıfları
| Toplanan veriler sınıfı | (Kapsamlı bir liste değil) içerir |
| --- | --- |
| **Özellikleri** |**Kodunuz tarafından belirlenen herhangi bir veriyi-** |
| DeviceContext |Kimliği, IP, yerel ayar, cihaz modeli, ağ, ağ türü, OEM adı, ekran çözünürlüğünü, rol örneği, rol adı, cihaz türü |
| ClientContext |İşletim sistemi, yerel ayar, dil, ağ, pencere çözümleme |
| Oturum |oturum kimliği |
| Sunucu bağlamı |Makine adı, yerel ayar, işletim sistemi, cihaz, kullanıcı oturumu, kullanıcı bağlamı, işlemi |
| Çıkarımı yapılan |IP adresi, zaman damgası, işletim sistemi, tarayıcı coğrafi konumdan |
| Ölçümler |Ölçüm adı ve değeri |
| Olaylar |Olay ad ve değer |
| PageViews |URL ve sayfa adı veya ekran adı |
| İstemci performans |URL/sayfa adı, tarayıcı yükleme süresi |
| AJAX |Sunucu için web sayfasından HTTP çağrıları |
| İstekler |URL, süresi, yanıt kodu |
| Bağımlılıklar |Tür (SQL, HTTP,...), bağlantı dizesi veya URI, eşitleme/zaman uyumsuz, süresi, başarı, SQL deyimi (ile durumu İzleyicisi) |
| **Özel durumlar** |Türü, **ileti**, çağrı yığınları, kaynak dosya ve satır numarası, iş parçacığı kimliği |
| Kilitlenmeler |İşlem kimliği, ana işlem kimliği, kilitlenme iş parçacığı kimliği; uygulama düzeltme eki, kimliği, yapı;  özel durum türü, adres, neden; Karıştırılmış simgeleri ve kayıtları, ikili başlangıç ve bitiş adreslerini, ikili dosya adı ve yolu, cpu türü |
| İzleme |**İleti** ve önem düzeyi |
| Performans sayaçları |İşlemci zamanı, kullanılabilir bellek, isteği hızı, özel durum hızı, işlem özel bayt, g/ç hızı, istek süresi, istek sırası uzunluğu |
| Kullanılabilirlik |Web testi yanıt kodu, her test adımı, test adını, zaman damgası, başarı, yanıt süresi, test konumunu süresi |
| SDK tanılama |İzleme iletisi veya özel durumu |

Yapabilecekleriniz [verilerin bazıları Applicationınsights.config düzenleyerek geçiş][config]

## <a name="credits"></a>Jenerik
Bu ürün MaxMind, kullanılabilir tarafından oluşturulan GeoLite2 verileri içeren [ http://www.maxmind.com ](http://www.maxmind.com).



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[platforms]: app-insights-platforms.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

