---
title: Veri saklama ve Azure Application Insights depolamada | Microsoft Docs
description: Koruma ve gizlilik ilke bildirimi
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: a6268811-c8df-42b5-8b1b-1d5a7e94cbca
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: mbullwin
ms.openlocfilehash: 0f8f1c5585eb13506baea1e5ddbe611cc931758e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60899257"
---
# <a name="data-collection-retention-and-storage-in-application-insights"></a>Application Insights ile veri toplama, tutma ve depolama

Yüklediğinizde [Azure Application Insights] [ start] buluta uygulamanızla ilgili telemetri gönderdiği SDK'sını uygulamanıza. Doğal olarak, sorumlu geliştiriciler tam olarak hangi veriler gönderilir, verilere ne olur ve denetimini nasıl tutabilirsiniz bilmek istersiniz. Özellikle, hassas verileri gönderilebilir, depolanan ve ne kadar güvenli olduğu nedir? 

İlk olarak, kısa yanıt:

* Hassas verileri hizmete göndermek "kutu dışı" çalıştıran standart telemetri modülleri düşüktür. Telemetri, yük, performans ve kullanım ölçümleri, özel durum raporları ve diğer Tanılama verileri ile ilgilidir. Ana kullanıcı verilerini tanılama raporlarında görünür durumda URL'leri işlevleridir. Ancak, uygulamanızın düz metin biçiminde bir URL içinde hassas verileri her durumda put olmamalıdır.
* Tanılama ve kullanım izleme yardımcı olması için ek özel telemetri gönderen kod yazabilirsiniz. (Bu genişletilebilirlik yer Application Insights harika bir özelliğidir.) Bunun yanlışlıkla, kişisel ve hassas veriler içerir, böylece bu kod yazmak için mümkün olur. Uygulamanız bu tür veriler ile çalışıyorsa, yazdığınız tüm kod kapsamlı bir gözden geçirme işlemlerini uygulamanız gerekir.
* Geliştirme ve uygulamanızı test ederken SDK'sı tarafından gönderildiği incelemek kolaydır. Veri IDE ve tarayıcı hata ayıklama çıktı pencerelerinde görünür. 
* Veriler tutulur [Microsoft Azure](https://azure.com) ABD veya Avrupa sunucuları. (Ancak uygulamanızı herhangi bir yere çalıştırabilirsiniz.) Azure'da [güçlü güvenlik işleyen ve çok çeşitli uyumluluk standartlarını karşıladığını](https://azure.microsoft.com/support/trust-center/). Yalnızca size ve takımınıza atanan verilerinize erişebilirsiniz. Microsoft çalışanları için yalnızca belirli sınırlı koşullarda bilginiz dahilinde sınırlı erişimi. Aktarımda ve bekleme sırasında şifrelenir.

Bu makalenin geri kalanında, daha tam olarak bu yanıtları elaborates. Böylece, hemen bir ekibin parçası olmayan iş arkadaşlarınıza göstermek kendi içinde olacak şekilde tasarlanmıştır.

## <a name="what-is-application-insights"></a>Application Insights nedir?
[Azure Application Insights] [ start] yardımcı olan Microsoft tarafından sağlanan bir hizmeti, Canlı uygulamanızın kullanılabilirliğini ve performansı geliştirmek olduğu. Uygulamanız, test sırasında hem yayımlanan veya dağıttıktan sonra çalıştığı her zaman izler. Application Insights grafikleri ve, örneğin gösteren tablolar, günün hangi saatlerinde size çoğu kullanıcının, uygulamanın nasıl yanıt veriyor ve ne kadar iyi bağımlı olan dış hizmetler tarafından sunulur oluşturur. Kilitlenmeler, hata veya performans sorunları varsa, nedenini tanılamak için ayrıntılı telemetri verilerini aracılığıyla arayabilirsiniz. Ve hizmet kullanılabilirliği ve uygulamanızın performansını herhangi bir değişiklik varsa e-posta gönderir.

Bu işlev alabilmek için kendi kod parçası haline gelir, uygulamanızda bir Application Insights SDK'sını yükleyin. Uygulamanız çalışırken, SDK'sı, işlemi izler ve Application Insights hizmetine telemetri gönderir. Bu, bir bulut hizmeti tarafından barındırılan [Microsoft Azure](https://azure.com). (Ancak tüm uygulamalar, yalnızca o Azure'da barındırılan Application Insights için geçerlidir.)

Application Insights hizmetine depolar ve telemetriyi analiz eder. Analiz veya depolanmış telemetri aracılığıyla aramayı görmek için Azure hesabınızda oturum açın ve uygulamanız için Application Insights kaynağını açın. Ayrıca, diğer ekip üyelerinin veya belirtilen Azure aboneleri ile verilere erişimi paylaşabilirsiniz.

Örneğin bir veritabanı veya harici araçlar Application Insights hizmetinden dışarı aktarılan verileri olabilir. Her aracı bir özel anahtarla hizmetten alacaktır sağlar. Gerekirse, anahtar iptal edilebilir. 

Application Insights SDK'ları çeşitli uygulama türlerini için kullanılabilir: web hizmetleri kendi Java EE veya ASP.NET sunucusu veya Azure; barındırılan diğer bir deyişle, bir web sayfasında çalışan kodu web istemcisi; Masaüstü uygulamaları ve Hizmetleri; cihaz uygulamaları Windows Phone, iOS ve Android gibi. Bunların tümü aynı hizmet telemetri gönderin.

## <a name="what-data-does-it-collect"></a>Hangi veri toplamayı?
### <a name="how-is-the-data-is-collected"></a>Verilerin nasıl olduğunu toplanır?
Üç veri kaynağına vardır:

* Ya da uygulamanızla tümleştirin SDK [geliştirme](../../azure-monitor/app/asp-net.md) veya [çalışma zamanında](../../azure-monitor/app/monitor-performance-live-website-now.md). Farklı uygulama türleri için farklı Sdk'ler vardır. Ayrıca bir [web sayfaları için SDK'sı](../../azure-monitor/app/javascript.md), sayfanın yanı sıra son kullanıcının tarayıcıya yükler.
  
  * Her bir SDK çok sayıda sahiptir [modülleri](../../azure-monitor/app/configuration-with-applicationinsights-config.md), farklı tür telemetri toplamak için farklı teknikleri kullanın.
  * Geliştirme SDK'yı yüklerseniz, standart modüller yanı sıra kendi telemetrinizi göndermek için kendi API kullanabilirsiniz. Bu özel telemetri göndermek istediğiniz herhangi bir veri içerebilir.
* Bazı web sunucuları, ayrıca yanı sıra uygulamayı çalıştırın ve CPU, bellek ve ağ doluluk hakkında telemetri gönderen aracıları vardır. Docker ana bilgisayarları, örneğin, Azure Vm'leri ve [Java EE sunucuları](../../azure-monitor/app/java-agent.md) böyle aracıları olabilir.
* [Kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md) işlemleri, istekleri web uygulamanıza düzenli aralıklarla göndermek Microsoft tarafından çalıştırılır. Sonuçlar Application Insights hizmetine gönderilir.

### <a name="what-kinds-of-data-are-collected"></a>Hangi tür verileri toplanır?
Ana kategoriler şunlardır:

* [Web sunucusu telemetri](../../azure-monitor/app/asp-net.md) -HTTP isteği.  URI, istek, yanıt kodu, istemci IP adresi işlemek için geçen süre. Oturum kimliği.
* [Web sayfaları](../../azure-monitor/app/javascript.md) -sayfası, kullanıcı ve oturum sayıları. Sayfa yükleme süreleri. Özel durumlar. AJAX çağrıları.
* Performans sayaçları - bellek, CPU, GÇ, ağ doluluk.
* İstemci ve sunucu bağlamı - işletim sistemi, yerel ayar, cihaz türü, tarayıcı, ekran çözünürlüğü.
* [Özel durumlar](../../azure-monitor/app/asp-net-exceptions.md) ve kilitlenmeleri - **yığın dökümlerini**, derleme kimliği, CPU türü. 
* [Bağımlılıkları](../../azure-monitor/app/asp-net-dependencies.md) -REST, SQL, AJAX gibi dış hizmetlerle çağırır. URI veya bağlantı dizesi, süre, başarı, komutu.
* [Kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md) -test ve adımları, yanıt süresi.
* [İzleme günlükleri](../../azure-monitor/app/asp-net-trace-logs.md) ve [özel telemetri](../../azure-monitor/app/api-custom-events-metrics.md) - **günlükleri veya telemetri kodu herhangi bir şey**.

[Daha fazla ayrıntı](#data-sent-by-application-insights).

## <a name="how-can-i-verify-whats-being-collected"></a>Nasıl ne toplanan doğrulayabilirsiniz?
Visual Studio kullanarak uygulama geliştiriyorsanız, uygulamayı hata ayıklama modunda (F5) çalıştırın. Telemetri çıktı penceresinde görünür. Buradan kopyalayın ve kolay bir inceleme için JSON olarak biçimlendirin. 

![](./media/data-retention-privacy/06-vs.png)

Tanılama penceresinde de daha okunabilir bir görünüm var.

Web sayfaları için hata ayıklama, tarayıcı penceresi açın.

![F12 tuşuna basın ve ağ sekmesini açın.](./media/data-retention-privacy/08-browser.png)

### <a name="can-i-write-code-to-filter-the-telemetry-before-it-is-sent"></a>Ben gönderilmeden önce telemetri filtreleme için kod yazabilirsiniz?
Bu yazarak yazılabilir bir [telemetri işlemci eklentisi](../../azure-monitor/app/api-filtering-sampling.md).

## <a name="how-long-is-the-data-kept"></a>Verileri ne kadar süreyle tutulur?
Ham veri noktalarını (diğer bir deyişle, Analytics'te sorgu ve arama İnceleme öğeleri) 90 gün boyunca tutulur. Daha uzun verileri tutmak gerekirse kullanabileceğiniz [sürekli dışarı aktarma](../../azure-monitor/app/export-telemetry.md) bir depolama hesabına kopyalamak için.

Toplanan verileri (diğer bir deyişle, sayıları, ortalamalar ve ölçüm Gezgini'nde gördüğünüz diğer istatistiksel veriler), 90 gün boyunca 1 dakikalık bir dilimi korunur.

[Anlık görüntü hata ayıklama](../../azure-monitor/app/snapshot-debugger.md) yedi gün boyunca saklanır. Bu bekletme ilkesi, bir uygulama başına temelinde ayarlanır. Bu değeri arttırmak gerekiyorsa, Azure portalında bir destek talebi açarak artışı isteyebilirsiniz.

## <a name="who-can-access-the-data"></a>Verilere kimler erişebilir?
Verileri sizin için görünür olur ve bir kuruluş hesabı veya takım üyeleriniz varsa. 

Bunu siz ve takım üyeleri tarafından verilmesi ve diğer konumlara kopyalanabilir ve diğer kişilere geçirildi.

#### <a name="what-does-microsoft-do-with-the-information-my-app-sends-to-application-insights"></a>Microsoft Application Insights'a uygulamamın gönderdiği bilgilere ile ne yapar?
Microsoft, yalnızca hizmet olanak sağlamak için verileri kullanır.

## <a name="where-is-the-data-held"></a>Verilerin tutulduğu?
* ABD, Avrupa veya Güneydoğu Asya. Yeni bir Application Insights kaynağı oluşturduğunuzda, konumu seçebilirsiniz. 

#### <a name="does-that-mean-my-app-has-to-be-hosted-in-the-usa-europe-or-southeast-asia"></a>Uygulamamı ABD, Avrupa veya Güneydoğu Asya barındırılması gerekir anlama geliyor?
* Hayır. Uygulamanızı her yerden, kendi şirket içi ana bilgisayarlarına içinde veya bulutta çalıştırabilirsiniz.

## <a name="how-secure-is-my-data"></a>Verilerim ne kadar güvenli mi?
Application Insights, bir Azure hizmetidir. Güvenlik ilkelerinin [Azure güvenlik, gizlilik ve uyumluluk teknik incelemesi](https://go.microsoft.com/fwlink/?linkid=392408).

Veriler, Microsoft Azure sunucularda depolanır. Hesap kısıtlamaları açıklanan Azure Portalı'nda hesaplar için [belge Azure güvenlik, gizlilik ve Uyumluluk](https://go.microsoft.com/fwlink/?linkid=392408).

Microsoft personeli tarafından verilerinize erişimi sınırlıdır. Biz verilerinizi yalnızca sizin izninizle erişmek ve Application Insights kullanımını desteklemek için gerekli olup olmadığını. 

Yanıtlarınız, müşterilerin tüm uygulamalar (örneğin, veri ve izlemeler ortalama boyutu) arasında veri, Application Insights geliştirmek için kullanılır.

#### <a name="could-someone-elses-telemetry-interfere-with-my-application-insights-data"></a>Başkasının telemetri, Application Insights verilerimi engelleyebilir mi?
Bunlar, hesabınıza ek telemetri web sayfalarınıza kodda bulunabilir izleme anahtarını kullanarak gönderebilir. Ek yeterli verilerle ölçümlerinizi doğru uygulamanızın performansını ve kullanımını temsil.

Diğer projeleri ile kod paylaşın, izleme anahtarınızı kaldırmayı unutmayın.

## <a name="is-the-data-encrypted"></a>Veriler şifrelenir?
Tüm veriler bekleme durumundayken şifrelenir ve merkezi olarak arasında veri taşır.

#### <a name="is-the-data-encrypted-in-transit-from-my-application-to-application-insights-servers"></a>Application Insights sunucularına gelen Aktarımdaki veriler şifrelenir?
Evet, biz web sunucuları, cihazlar ve HTTPS web sayfaları dahil olmak üzere neredeyse tüm Sdk'lardan portala veri göndermek için https kullanır. Bunun tek istisnası, düz HTTP web sayfalarından gönderilen verilerdir.

## <a name="does-the-sdk-create-temporary-local-storage"></a>SDK, yerel geçici depolama oluşturuyor mu?

Evet, bazı Telemetri kanalları yerel olarak bir uç nokta ulaşılamıyorsa veri açık kalır. Lütfen hangi çerçeveleri ve telemetri kanalları etkilendiğini görmek için aşağıda inceleyin.

Yerel depolama alanını telemetri kanalları geçici dosyaları, uygulamanızı çalıştıran belirli bir hesabın kısıtlanır TEMP veya APPDATA dizini oluşturun. Bu, bir uç nokta geçici olarak kullanılamıyor veya azaltma sınırına olduğunda ortaya çıkabilir. Bu sorun çözüldükten sonra telemetri kanal tüm yeni ve kalıcı veri göndermeye devam eder.

Bu kalıcı verileri yerel olarak şifrelenmez. Bu önemliyse, verileri gözden geçirin ve özel veri koleksiyonunu sınırlayabilirsiniz. (Bkz [dışarı aktarın ve özel veri silme](https://docs.microsoft.com/azure/application-insights/app-insights-customer-data#how-to-export-and-delete-private-data) daha fazla bilgi için.)

Bu dizin ile belirli güvenlik gereksinimlerini yapılandırmak bir müşteri gerekiyorsa framework yapılandırılabilir. Uygulama çalışan işlemi bu dizine yazma erişimi olduğundan emin olun, ancak aynı zamanda bu dizin, istenmeyen kullanıcılar tarafından okunan telemetri önlemek için korunduğundan emin olun.

### <a name="java"></a>Java

`C:\Users\username\AppData\Local\Temp` kalıcı veri için kullanılır. Bu konum yapılandırma dizininden yapılandırılabilir değildir ve bu klasöre erişim izni belirli bir kullanıcıya gerekli kimlik bilgileriyle sınırlıdır. (Bkz [uygulama](https://github.com/Microsoft/ApplicationInsights-Java/blob/40809cb6857231e572309a5901e1227305c27c1a/core/src/main/java/com/microsoft/applicationinsights/internal/util/LocalFileSystemUtils.java#L48-L72) buraya.)

###  <a name="net"></a>.NET

Varsayılan olarak `ServerTelemetryChannel` geçerli kullanıcının yerel uygulama veri klasörü kullanır `%localAppData%\Microsoft\ApplicationInsights` veya geçici klasörü `%TMP%`. (Bkz [uygulama](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/91e9c91fcea979b1eec4e31ba8e0fc683bf86802/src/ServerTelemetryChannel/Implementation/ApplicationFolderProvider.cs#L54-L84) buraya.)


Yapılandırma dosyası:
```xml
<TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel,   Microsoft.AI.ServerTelemetryChannel">
    <StorageFolder>D:\NewTestFolder</StorageFolder>
</TelemetryChannel>
```

Kod:

- Yapılandırma dosyasından ServerTelemetryChannel Kaldır
- Bu kod parçacığı yapılandırmanıza ekleyin:
  ```csharp
  ServerTelemetryChannel channel = new ServerTelemetryChannel();
  channel.StorageFolder = @"D:\NewTestFolder";
  channel.Initialize(TelemetryConfiguration.Active);
  TelemetryConfiguration.Active.TelemetryChannel = channel;
  ```

### <a name="netcore"></a>NetCore

Varsayılan olarak `ServerTelemetryChannel` geçerli kullanıcının yerel uygulama veri klasörü kullanır `%localAppData%\Microsoft\ApplicationInsights` veya geçici klasörü `%TMP%`. (Bkz [uygulama](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/91e9c91fcea979b1eec4e31ba8e0fc683bf86802/src/ServerTelemetryChannel/Implementation/ApplicationFolderProvider.cs#L54-L84) buraya.) Depolama klasörü belirtilmediği sürece bir Linux ortamında yerel depolama devre dışı bırakılır.

Aşağıdaki kod parçacığını nasıl ayarlanacağını gösterir `ServerTelemetryChannel.StorageFolder` içinde `ConfigureServices()`  yöntemi, `Startup.cs` sınıfı:

```csharp
services.AddSingleton(typeof(ITelemetryChannel), new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
```

(Bkz [AspNetCore özel yapılandırma](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) daha fazla bilgi için. )

### <a name="nodejs"></a>Node.js

Varsayılan olarak `%TEMP%/appInsights-node{INSTRUMENTATION KEY}` kalıcı verileri için kullanılır. Bu klasör erişim izinleri, geçerli kullanıcı ve Yöneticiler için kısıtlanır. (Bkz [uygulama](https://github.com/Microsoft/ApplicationInsights-node.js/blob/develop/Library/Sender.ts) buraya.)

Klasör ön eki `appInsights-node` statik değişkenin çalışma zamanı değerini değiştirerek geçersiz kılınabilir `Sender.TEMPDIR_PREFIX` bulunan [Sender.ts](https://github.com/Microsoft/ApplicationInsights-node.js/blob/7a1ecb91da5ea0febf5ceab13d6a4bf01a63933d/Library/Sender.ts#L384).



## <a name="how-do-i-send-data-to-application-insights-using-tls-12"></a>Nasıl veri Application Insights'a TLS 1.2 kullanarak gönderebilirim?

Application Insights Uç noktalara Aktarımdaki verilerin güvenliğini sağlamak üzere en az kullanmak üzere yapılandırmak için müşterilerin önemle öneririz Aktarım Katmanı Güvenliği (TLS) 1.2. TLS/Güvenli Yuva Katmanı (SSL) daha eski sürümleri, savunmasız bulundu ve bunlar yine de şu anda geriye dönük uyumluluk izin vermek için çalışırken, bunlar **önerilmez**, ve sektör hızla destek bırakmasını taşıma Bu eski protokolleri için. 

[PCI güvenlik standartları Council](https://www.pcisecuritystandards.org/) olarak ayarlanmış bir [son 30 Haziran 2018'ın](https://www.pcisecuritystandards.org/pdfs/PCI_SSC_Migrating_from_SSL_and_Early_TLS_Resource_Guide.pdf) TLS/SSL ve yükseltme daha da protokolleri güvenli hale getirmek için eski sürümlerini devre dışı bırakmak için. Uygulama/istemciler üzerinde en az iletişim kuramıyorsa, eski destek, Azure bıraktığı sonra TLS 1.2 değil okunup verilerini Application Insights'a gönderebilir. Sınama ve doğrulama uygulamanızın TLS desteği için hangi yaklaşımın, uygulamanızın kullandığı dil/framework yanı sıra işletim sistemi/platform bağlı olarak değişir.

Açıkça sürece yalnızca TLS 1.2 kullanmak için uygulamanızı otomatik olarak algılamak ve olduklarında daha yeni daha güvenli protokolleri yararlanmasına olanak tanıyan platform düzeyi güvenlik özellikleri bu bozabilir kesinlikle gerekli ayarlanması önerilmez TLS 1.3 gibi kullanılabilir. Kapsamlı bir denetim için belirli bir TLS/SSL sürümlerinin runbook'a kod denetlemek için uygulamanızın kod gerçekleştirme öneririz.

### <a name="platformlanguage-specific-guidance"></a>Platform/dil özel Kılavuzu

|Platform/dili | Destek | Daha Fazla Bilgi |
| --- | --- | --- |
| Azure Uygulama Hizmetleri  | Desteklenen yapılandırması gerekli olabilir. | Destek Nisan 2018'de Duyuruldu. İçin duyuruyu okuyun [yapılandırma ayrıntılarını](https://blogs.msdn.microsoft.com/appserviceteam/2018/04/17/app-service-and-functions-hosted-apps-can-now-update-tls-versions/).  |
| Azure işlev uygulamaları | Desteklenen yapılandırması gerekli olabilir. | Destek Nisan 2018'de Duyuruldu. İçin duyuruyu okuyun [yapılandırma ayrıntılarını](https://blogs.msdn.microsoft.com/appserviceteam/2018/04/17/app-service-and-functions-hosted-apps-can-now-update-tls-versions/). |
|.NET | Desteklenen yapılandırma sürüme göre değişir. | .NET 4.7 ve önceki sürümleri için ayrıntılı yapılandırma bilgileri için bkz [bu yönergeleri](https://docs.microsoft.com/dotnet/framework/network-programming/tls#support-for-tls-12).  |
|Durum İzleyicisi | Desteklenen, yapılandırma gerekiyor | Durum İzleyicisi'ni dayanır [işletim sistemi yapılandırması](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) + [.NET Yapılandırması](https://docs.microsoft.com/dotnet/framework/network-programming/tls#support-for-tls-12) TLS 1.2 desteği.
|Node.js |  , Desteklenen v10.5.0 içinde yapılandırması gerekli olabilir. | Kullanım [resmi bir Node.js TLS/SSL belge](https://nodejs.org/api/tls.html) için herhangi bir uygulama belirli yapılandırma. |
|Java | Desteklenir, TLS 1.2 JDK desteği eklenmiştir [JDK 6 güncelleştirmesi 121](https://www.oracle.com/technetwork/java/javase/overview-156328.html#R160_121) ve [JDK 7](https://www.oracle.com/technetwork/java/javase/7u131-relnotes-3338543.html). | JDK 8 kullanan [varsayılan olarak TLS 1.2](https://blogs.oracle.com/java-platform-group/jdk-8-will-use-tls-12-as-default).  |
|Linux | Linux dağıtımları eğilimli etmenin [OpenSSL](https://www.openssl.org) TLS 1.2 desteği.  | Denetleme [OpenSSL Changelog](https://www.openssl.org/news/changelog.html) OpenSSL sürümünüz desteklenir onaylamak için.|
| Windows 8.0 10 | Desteklenen ve varsayılan olarak etkindir. | Yine de kullandığınızı doğrulamak için [varsayılan ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings).  |
| Windows Server 2012-2016 | Desteklenen ve varsayılan olarak etkindir. | Yine de kullandığınızı doğrulamak için [varsayılan ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) |
| Windows 7 SP1 ve Windows Server 2008 R2 SP1 | , Varsayılan olarak etkin değildir ancak desteklenir. | Bkz: [Aktarım Katmanı Güvenliği (TLS) kayıt defteri ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) nasıl etkinleştirileceği hakkında daha fazla ayrıntı için.  |
| Windows Server 2008 SP2 | TLS 1.2 desteği güncelleştirilmesi gerekiyor. | Bkz: [TLS 1.2 desteği eklemek için güncelleştirme](https://support.microsoft.com/help/4019276/update-to-add-support-for-tls-1-1-and-tls-1-2-in-windows-server-2008-s) Windows Server 2008 SP2. |
|Windows Vista | Desteklenmiyor. | Yok

### <a name="check-what-version-of-openssl-your-linux-distribution-is-running"></a>Linux dağıtımınıza OpenSSL hangi sürümünü çalışıp çalışmadığını denetle

Openssl hangi sürümünün yüklü olduğunu denetlemek için bir terminal açın ve çalıştırın:

```terminal
openssl version -a
```

### <a name="run-a-test-tls-12-transaction-on-linux"></a>Linux üzerinde TLS 1.2 işlem test çalıştırma

Linux sisteminizi TLS 1.2 iletişim kurabilir görmek için temel bir başlangıç testi çalıştırmak için. Terminali açın ve çalıştırın:

```terminal
openssl s_client -connect bing.com:443 -tls1_2
```

## <a name="personal-data-stored-in-application-insights"></a>Uygulama anlayışları'nda depolanan kişisel verileri

Bizim [Application Insights kişisel verileri makale](../../azure-monitor/platform/personal-data-mgmt.md) ayrıntılı bu sorunu ele alınmaktadır.

#### <a name="can-my-users-turn-off-application-insights"></a>Kullanıcılarım Application ınsights'ı kapatabilir miyim?
Doğrudan yönetilemez. Application ınsights'ı etkinleştirmek için kullanıcılarınızın çalışabilecek bir anahtar sağlıyoruz yok.

Bununla birlikte, uygulamanızda bu tür bir özelliği uygulayabilirsiniz. Tüm SDK'ları telemetri toplamayı devre dışı etkinleştiren bir API ayarı içerir. 

## <a name="data-sent-by-application-insights"></a>Application Insights tarafından gönderilen veriler
SDK'ları platformları arasında farklılık gösterir ve yüklemek için kullanabileceğiniz çeşitli bileşenler vardır. (Bakın [Application Insights - genel bakış][start].) Her bir bileşen farklı veri gönderir.

#### <a name="classes-of-data-sent-in-different-scenarios"></a>Farklı senaryolarda gönderilen veri sınıfları

| Eylem | (Sonraki tabloya bakın) toplanan veri sınıfları |
| --- | --- |
| [.NET web projeye Application Insights SDK'sı ekleme][greenbrown] |ServerContext<br/>Olayla<br/>Performans sayaçları<br/>İstekler<br/>**Özel durumlar**<br/>Oturum<br/>kullanıcılar |
| [IIS Durum İzleyicisi'ni yükleyin][redfield] |Bağımlılıklar<br/>ServerContext<br/>Olayla<br/>Performans sayaçları |
| [Java web uygulaması için Application Insights SDK'sını ekleyin][java] |ServerContext<br/>Olayla<br/>İstek<br/>Oturum<br/>kullanıcılar |
| [Web sayfası için JavaScript SDK'sını ekleyin][client] |ClientContext <br/>Olayla<br/>Sayfa<br/>ClientPerf<br/>Ajax |
| [Varsayılan özellikleri tanımlama][apiproperties] |**Özellikleri** tüm standart ve özel olaylar |
| [Çağrı TrackMetric][api] |Sayısal değerleri<br/>**Özellikleri** |
| [Çağrı izleme *][api] |Olay adı<br/>**Özellikleri** |
| [Çağrı TrackException][api] |**Özel durumlar**<br/>Yığın Dökümü<br/>**Özellikleri** |
| SDK, veri toplanamıyor. Örneğin: <br/> -Performans sayaçlarına erişilemiyor<br/> -telemetri başlatıcısını özel durumu |SDK'sı tanılama |

İçin [diğer platformlar için SDK'lar][platforms], kendi belgelere bakın.

#### <a name="the-classes-of-collected-data"></a>Toplanan verileri sınıfları

| Toplanan verileri sınıfı | İçerir (kapsamlı bir liste değil) |
| --- | --- |
| **Özellikleri** |**Kodunuz tarafından belirlenen tüm veriler-** |
| DeviceContext |Kimliği, IP, yerel ayar, cihaz modeli, ağ, ağ türü, OEM adı, ekran çözünürlüğü, rol örneği, rol adı, cihaz türü |
| ClientContext |İşletim sistemi, yerel ayar, dil, ağ, pencere çözümleme |
| Oturum |Oturum kimliği |
| ServerContext |Makine adı, yerel ayar, işletim sistemi, cihaz, kullanıcı oturum, kullanıcı bağlamı, işlemi |
| Olayla |IP adresi, zaman damgası, işletim sistemi, tarayıcı coğrafi konumdan |
| Ölçümler |Ölçüm adı ve değeri |
| Olaylar |Olay ad ve değer |
| PageViews |URL ve sayfa adı veya ekran adı |
| İstemci performans |URL/sayfa adına, tarayıcı yükleme süresi |
| Ajax |Sunucusuna HTTP çağrıları web sayfasından |
| İstekler |URL'ye, süresini, yanıt kodu |
| Bağımlılıklar |Türü (SQL, HTTP,...), bağlantı dizesi veya URI, eşitleme/zaman uyumsuz, süre, başarı, SQL deyimiyle (Durum İzleyicisi) |
| **Özel durumlar** |Tür, **ileti**, çağrı yığınlarını, kaynak dosya ve satır numarası, iş parçacığı kimliği |
| Kilitlenmeleri |İşlem kimliği, üst işlem kimliği, kilitlenme iş parçacığı kimliği; uygulama düzeltme eki, kimliği, derleme;  özel durum türü, adres, nedeni; Karıştırılmış simgeleri ve kayıtları, ikili başlangıç ve bitiş adreslerini, ikili dosya adı ve yolu, cpu türü |
| İzleme |**İleti** ve önem derecesi |
| Performans sayaçları |İşlemci zamanı, kullanılabilir bellek, istek hızı, özel durum oranı, işleme özel bayt sayısı, g/ç hızı, istek süresi, istek sırası uzunluğu |
| Kullanılabilirlik |Web test yanıt kodu, süre, her test adımı, test adı, zaman damgası, başarı, yanıt süresi, test konumu |
| SDK'sı tanılama |İzleme iletisi veya özel durumu |

Yapabilecekleriniz [verilerin bazıları Applicationınsights.config'i düzenleyerek geçiş][config]

> [!NOTE]
> İstemci IP, coğrafi konum çıkarsamak için kullanılır ancak IP veriler artık varsayılan olarak depolanır ve sıfır ilişkili alanın yazılan. Kişisel veri işleme hakkında daha fazla anlamak için bunu önermemizin [makale](../../azure-monitor/platform/personal-data-mgmt.md#application-data). IP adresi depolamanız gerekiyorsa ile bunu yapabilirsiniz bir [telemetri başlatıcısını](./../../azure-monitor/app/api-filtering-sampling.md#add-properties-itelemetryinitializer).

## <a name="credits"></a>Jenerik
Bu ürünü MaxMind kullanılabilir tarafından oluşturulan GeoLite2 veri içeren [ https://www.maxmind.com ](https://www.maxmind.com).



<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiproperties]: ../../azure-monitor/app/api-custom-events-metrics.md#properties
[client]: ../../azure-monitor/app/javascript.md
[config]: ../../azure-monitor/app/configuration-with-applicationinsights-config.md
[greenbrown]: ../../azure-monitor/app/asp-net.md
[java]: ../../azure-monitor/app/java-get-started.md
[platforms]: ../../azure-monitor/app/platforms.md
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/
[redfield]: ../../azure-monitor/app/monitor-performance-live-website-now.md
[start]: ../../azure-monitor/app/app-insights-overview.md

