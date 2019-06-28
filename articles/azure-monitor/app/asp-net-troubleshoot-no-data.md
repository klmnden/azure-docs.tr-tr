---
title: Veri bulunmama sorunlarını giderme - .NET için Application Insights
description: Azure Application Insights verileri göremiyor musunuz? Burada deneyin.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: e231569f-1b38-48f8-a744-6329f41d91d3
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/23/2018
ms.author: mbullwin
ms.openlocfilehash: 3820a5d7becef275ed3408f01cc53ad8590ba60e
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272408"
---
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Veri bulunmama sorunlarını giderme - .NET için Application Insights
## <a name="some-of-my-telemetry-is-missing"></a>Telemetrimi bazıları eksik
*Uygulama anlayışları'nda, yalnızca bir kesir uygulamamı tarafından oluşturulan olayların görüyorum.*

* Aynı kesir sürekli görüyorsanız, büyük olasılıkla nedeniyle Uyarlamalı olan [örnekleme](../../azure-monitor/app/sampling.md). Bunu doğrulamak için (genel bakış dikey penceresinden) araması'nı açın ve bir istek veya diğer olay örneğini bakın. Özellikler bölümü altındaki tam özellik ayrıntılarını almak için "..." tıklayın. Varsa sayısı > 1 istek ve örnekleme işlemi olduğundan.
* Aksi takdirde, aldığınızı olası bir [veri hızı sınırı](../../azure-monitor/app/pricing.md#limits-summary) fiyatlandırma planınız için. Bu sınırlar, dakika başına uygulanır.

*Veri kaybı rastgele yaşıyor.*

* Veri kaybıyla yaşıyorsanız denetleyin [Telemetri kanal](telemetry-channels.md#does-applicationinsights-channel-offer-guaranteed-telemetry-delivery-or-what-are-the-scenarios-where-telemetry-can-be-lost)

* Telemetri kanaldaki tüm bilinen sorunlar denetle [Github deposu](https://github.com/Microsoft/ApplicationInsights-dotnet/issues)

*Veri kaybı konsol uygulaması veya Web uygulaması, uygulama durmak için olduğunda yaşıyor.*

* SDK'sı kanal telemetri arabellekte tutar ve bunları toplu olarak gönderir. Uygulama kapatılıyor, açıkça çağırmak gerekebilir [Flush()](api-custom-events-metrics.md#flushing-data). Davranışını `Flush()` gerçek üzerinde bağlıdır [kanal](telemetry-channels.md#built-in-telemetrychannels) kullanılır.

## <a name="no-data-from-my-server"></a>My sunucusundan veri yok
*Uygulamamı my web sunucusunda yüklü ve artık tüm telemetrisini göremiyorum. Geliştirme makineme Tamam çalıştınız.*

* Güvenlik Duvarı sorun büyük olasılıkla. [Application Insights'ı veri göndermek için güvenlik duvarı özel durumlarını ayarlama](../../azure-monitor/app/ip-addresses.md).
* IIS Server bazı Önkoşullar eksik: .NET genişletilebilirliği 4.5 ve ASP.NET 4.5.

*Ben [Durum İzleyicisi'nin yüklü](../../azure-monitor/app/monitor-performance-live-website-now.md) web sunucuma mevcut uygulamaları izleyin. Herhangi bir sonuç göremiyorum.*

* Bkz: [Durum İzleyicisi sorun giderme](../../azure-monitor/app/monitor-performance-live-website-now.md#troubleshoot).

## <a name="q01"></a>Visual Studio 'Application Insights Ekle' seçeneği
*Mevcut bir projeyi Çözüm Gezgini'nde sağ tıkladığımda, herhangi bir Application Insights seçeneği görmüyorum.*

* Tüm türleri .NET projesinin araçları tarafından desteklenir. Web ve WCF proje desteklenir. Masaüstü veya hizmet uygulamalar gibi diğer proje türleri için yine de [bir Application Insights SDK'sını projenize el ile ekleme](../../azure-monitor/app/windows-desktop.md).
* Olduğundan emin olun [Visual Studio 2013 Update 3 veya üzeri](https://docs.microsoft.com/visualstudio/releasenotes/vs2013-update3-rtm-vs). Application Insights SDK'sı sağlayan Geliştirici analiz araçları ile önceden yüklenmiş olarak sunulur.
* Seçin **Araçları**, **Uzantılar ve güncelleştirmeler** denetimi **Developer Analytics Tools** yüklü ve etkin. Öyleyse **güncelleştirmeleri** kullanılabilir bir güncelleştirme olup olmadığını görmek için.
* Yeni Proje iletişim kutusunu açın ve ASP.NET Web uygulamasını seçin. Application Insights seçeneği yok görürseniz, daha sonra Araçlar yüklenir. Aksi durumda, Developer Analytics Tools ' ı yeniden yüklemeyi deneyin.

## <a name="q02"></a>Application Insights ekleme işlemi başarısız oldu
*Varolan bir projeye Application Insights ekleme çalıştığınızda bir hata iletisi konusuna bakın.*

Olası nedenler:

* Application Insights portalıyla iletişim başarısız oldu; veya
* Azure hesabınızda bir sorun yoktur;
* Yalnızca [abonelik veya burada denemeye yeni kaynak oluşturmak için grubu okuma erişimini](../../azure-monitor/app/resources-roles-access-control.md).

Düzeltme:

* Oturum açma kimlik bilgileri için doğru Azure hesabını sağlanan denetleyin.
* Tarayıcınızda erişiminiz olduğunu denetleyin [Azure portalında](https://portal.azure.com). Ayarları'nı açın ve herhangi bir kısıtlama olup olmadığına bakın.
* [Mevcut projenize Application Insights ekleme](../../azure-monitor/app/asp-net.md): Çözüm Gezgini'nde projenize sağ tıklayın ve "Ekle Application Insights."

## <a name="emptykey"></a>"İzleme anahtarını boş bırakılamaz" hatasını alıyorum
Application Insights veya belki de bir günlük bağdaştırıcısı yüklemekte ederken bir sorun var gibi görünüyor.

Çözüm Gezgini'nde projenize sağ tıklayıp seçin **Application Insights > Application ınsights'ı Yapılandır**. Azure'da oturum açmanız davet bir iletişim kutusu alırsınız ve bir Application Insights kaynağı oluşturun veya mevcut bir yeniden kullanın.

## <a name="NuGetBuild"></a> "NuGet paketleri my yapı sunucusunda eksik"
*Her şey Tamam geliştirme makineme hata ayıklama, ancak yapı sunucusunda bir NuGet hatası alıyorum oluşturur.*

Lütfen [NuGet paketi geri yüklemeyi](https://docs.nuget.org/Consume/Package-Restore) ve [paket otomatik geri](https://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-to-open-application-insights-from-visual-studio"></a>Visual Studio'da Application Insights'ı açmak için menü komutu eksik
*Çözüm Gezgini proje sağ tıkladığımda, herhangi bir Application Insights komut göremiyorum veya açık Application ınsights'ı komut göremiyorum.*

Olası nedenler:

* Application Insights kaynağını el ile oluşturduysanız veya proje için Application Insights araçları tarafından desteklenmeyen bir tür ise.
* Geliştirici analiz araçları, Visual Studio'da devre dışı bırakıldı.
* Visual Studio 2013 güncelleştirme 3 eskidir.

Düzeltme:

* Visual Studio sürümünüz 2013 güncelleştirme 3 veya sonrası olduğundan emin olun.
* Seçin **Araçları**, **Uzantılar ve güncelleştirmeler** denetimi **Geliştirici analiz araçları** yüklü ve etkin. Öyleyse **güncelleştirmeleri** kullanılabilir bir güncelleştirme olup olmadığını görmek için.
* Çözüm Gezgini'nde projenize sağ tıklayın. Komut görürseniz **Application Insights > Application ınsights'ı Yapılandır**, projenizin kaynak Application Insights hizmetine bağlanmak için kullanın.

Aksi takdirde, proje türünüzü doğrudan Geliştirici analiz araçları tarafından desteklenmiyor. Telemetrinizi görmek için oturum açın [Azure portalında](https://portal.azure.com), sol gezinti çubuğunda Application Insights'ı seçin ve uygulamanızı seçin.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>'Erişim engellendi' Application ınsights'ı Visual Studio'dan açarken
*'Application ınsights'ı Aç' menü komutu bana Azure portalına götürür, ancak bir 'erişim engellendi' hatası alıyorum.*

Varsayılan tarayıcınızı en son kullanılan Microsoft oturum açma erişimi yok [bu uygulama için Application Insights eklendiğinde, oluşturulan kaynak](../../azure-monitor/app/asp-net.md). İki olası nedeni vardır:

* \- Belki de bir iş ve kişisel Microsoft hesabı birden fazla Microsoft hesabınız var mı? Varsayılan tarayıcınızı en son kullanılan oturum açma erişimi olan olandan farklı bir hesap aitti [projeye Application Insights ekleyin](../../azure-monitor/app/asp-net.md).
  * Düzeltme: Adınızı, tarayıcı penceresinin sağ üst ve oturum kapatma. Daha sonra erişimi olan hesapla oturum açın. Sonra sol gezinti çubuğunda Application Insights'ı tıklatın ve uygulamanızı seçin.
* Başka birinin Application Insights projeye eklenir ve size unuttunuz [kaynak grubuna erişim](../../azure-monitor/app/resources-roles-access-control.md) , oluşturulduğu.
  * Düzeltme: Bunlar bir kurumsal hesap kullandıysanız, takıma ekleyebilirsiniz; veya, kaynak grubuna bireysel erişim verebilirsiniz.

## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>'Varlık bulunamadı' Application ınsights'ı Visual Studio'dan açarken
*'Application ınsights'ı Aç' menü komutu bana Azure portalına götürür, ancak "varlık bulunamadı" hatasını alıyorum.*

Olası nedenler:

* Uygulamanız için Application Insights kaynağı silinmiş; veya
* İzleme anahtarını ayarlayın, veya Applicationınsights.config dosyasında doğrudan proje dosyasına güncelleştirmeden düzenleyerek değiştirildi.

İzleme anahtarı telemetrinin gönderildiği yeri Applicationınsights.config denetimlerinde. Proje dosyasındaki bir çizgi Visual Studio'da komutunu kullandığınızda, hangi kaynak açıldığında denetler.

Düzeltme:

* Çözüm Gezgini'nde projeye sağ tıklayın ve Application Insights, Application ınsights'ı Yapılandır'ı seçin. İletişim kutusunda, var olan bir kaynağa telemetri göndermek ya da yeni bir tane oluşturun ya da seçebilirsiniz. veya:
* Kaynağı doğrudan açın. Oturum [Azure portalında](https://portal.azure.com), Application Insights sol gezinti çubuğunda Koruma'ya tıklayın ve ardından uygulamanızı seçin.

## <a name="where-do-i-find-my-telemetry"></a>Telemetrimi nerede bulabilirim?
*Ben oturum açtığı [Microsoft Azure Portal'da](https://portal.azure.com), ve Azure Giriş Panosu'nda arayarak. Bu nedenle burada my Application Insights verilerini bulabilirim?*

* Application ınsights'ı ve ardından, uygulama adı sol gezinti çubuğunda Koruma'ya tıklayın. İçin gereken tüm projeleri burada yoksa, [eklemek veya web projenize Application Insights'ı yapılandırın](../../azure-monitor/app/asp-net.md).  
  Burada bazı Özet grafikleri görürsünüz. Daha fazla ayrıntı için bloblarda tıklayabilirsiniz.
* Uygulamanızı hata ayıklarken, Visual Studio'da Application Insights düğmesine tıklayın.

## <a name="q03"></a> Hiçbir sunucu verisi (veya hiç veri yok)
*Uygulamamı çalıştırıldı ve daha sonra Microsoft Azure'da Application Insights hizmetine açılır, ancak '... nasıl toplayacağınızı öğrenin' veya 'Yapılandırılmadı.' tüm grafikleri Göster* Veya, *yalnızca sayfa görünümü ve kullanıcı verilerini ancak sunucu verileri.*

* Uygulamanızı (F5) Visual Studio'da hata ayıklama modunda çalıştırın. Uygulamayı birkaç telemetri oluşturmak için kullanın. Visual Studio çıktı penceresinde kaydedilen olayları gördüğünüzü kontrol edin.  
  ![](./media/asp-net-troubleshoot-no-data/output-window.png)
* Application Insights Portalı'nda açmak [tanılama araması](../../azure-monitor/app/diagnostic-search.md). Veriler genellikle burada ilk görünür.
* Yenile düğmesine tıklayın. Dikey kendisini düzenli aralıklarla yeniler, ancak ayrıca el ile bunu yapabilirsiniz. Daha büyük zaman aralıkları için yenileme aralığını uzun.
* İzleme anahtarı eşleşen denetleyin. Uygulamanızı Application Insights portalında, ana dikey penceresinde, **Essentials** açılan listesinde, bakmak **izleme anahtarını**. Ardından, Visual Studio projenizde, Applicationınsights.config açın ve Bul `<instrumentationkey>`. İki anahtar eşit olup olmadığını denetleyin. Aksi takdirde:  
  * Portalda, Application Insights'ı tıklatın ve uygulama kaynağı için sağ ok tuşu ile bakın. veya
  * Visual Studio Çözüm Gezgini'nde projeye sağ tıklayın ve Application Insights, yapılandırma öğesini seçin. Uygulamanın doğru kaynağa telemetri göndermesine izin sıfırlayın.
  * Eşleşen anahtarları bulamazsanız, aynı oturum açma kimlik bilgileri olarak Visual Studio portalına kullandığınızdan emin olun.
* İçinde [Microsoft Azure giriş Panosu](https://portal.azure.com), hizmet durumu eşlemeyi bakın. Bazı uyarı göstergelerden varsa, bunlar için Tamam döndürmüş ve ardından kapatın ve, Application Insights uygulama dikey pencereyi yeniden açın kadar bekleyin.
* Ayrıca [durumu blogumuzu](https://blogs.msdn.microsoft.com/servicemap-status/).
* Herhangi bir kod yazdınız [sunucu tarafı SDK](../../azure-monitor/app/api-custom-events-metrics.md) izleme anahtarını değişebilir `TelemetryClient` örnekleri veya `TelemetryContext`? Veya, yazdınız bir [filtre veya örnekleme yapılandırma](../../azure-monitor/app/api-filtering-sampling.md) filtrelemesi çok fazla kullanıma?
* Applicationınsights.config dosyasını düzenlediyseniz, dikkatli bir şekilde yapılandırmasını denetleyin [TelemetryInitializers ve TelemetryProcessors](../../azure-monitor/app/api-filtering-sampling.md). Veri göndermek SDK'sı bir yanlış adlandırılan türü veya parametresi neden olabilir.

## <a name="q04"></a>Sayfa görüntülemeleri, tarayıcılar kullanımını veri yok
*Veriler sunucu yanıt süresi ve sunucu istekleri grafikleri, ancak hiçbir veri sayfa görünümü yükleme süresi veya tarayıcı ya da kullanım dikey pencerelerinde görüyorum.*

Web sayfalarındaki veriler gelir. 

* Mevcut bir web projesi için Application Insights eklediyseniz [betikleri el ile eklemek zorunda](../../azure-monitor/app/javascript.md).
* Internet Explorer, site uyumluluk modunda görüntülenmiyor emin olun.
* (Bazı tarayıcılarda F12 ardından ağ) tarayıcının hata ayıklama için gönderilen veri olduğunu doğrulamak için özelliğiyle `dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Hiçbir bağımlılık veya özel durum verileri
Bkz: [bağımlılık telemetrisi](../../azure-monitor/app/asp-net-dependencies.md) ve [özel durum telemetrisi](asp-net-exceptions.md).

## <a name="no-performance-data"></a>Performans verisi
Performans verileri (CPU, GÇ oranı vb.) için kullanılabilir [Java web Hizmetleri](../../azure-monitor/app/java-collectd.md), [Windows Masaüstü uygulamaları](../../azure-monitor/app/windows-desktop.md), [Durum İzleyicisi'ni yükleyin, web uygulamaları ve Hizmetleri'IIS](../../azure-monitor/app/monitor-performance-live-website-now.md), ve [Azure bulut Hizmetleri](../../azure-monitor/app/app-insights-overview.md). Sunucu Ayarları altında bulabilirsiniz.

## <a name="no-server-data-since-i-published-the-app-to-my-server"></a>(Sunucu) veri için my server uygulaması yayımladım beri
* Aslında tüm Microsoft kopyaladığınız denetleyin. Sunucuya Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll birlikte Applicationınsights DLL'leri
* Güvenlik Duvarı'nda gerekebilir [bazı TCP bağlantı noktalarını açma](../../azure-monitor/app/ip-addresses.md).
* Bir ara sunucu kullanmak, şirket ağı dışına göndermek, ayarlayın varsa [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) Web.config dosyasında
* Windows Server 2008 için: Aşağıdaki güncelleştirmeleri yüklediğinizden emin olun: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).

## <a name="i-used-to-see-data-but-it-has-stopped"></a>Verileri görmek için kullandım, ancak bu durdurdu
* Denetleme [durumu blog](https://blogs.msdn.com/b/applicationinsights-status/).
* Veri noktalarının aylık kota ulaşmış olabilirsiniz? Ayarlar/kota ve fiyatlandırma öğrenmek için açın. Bu durumda, planınızı yükseltin veya ek kapasite için ödeme yaparsınız. Bkz: [düzeni fiyatlandırma](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="i-dont-see-all-the-data-im-expecting"></a>Bekleniyor verilerini göremiyorum
Uygulamanız çok miktarda veri gönderir ve ASP.NET sürüm 2.0.0-beta3 veya daha sonra Application Insights SDK kullanıyorsanız [Uyarlamalı örnekleme](../../azure-monitor/app/sampling.md) özellik çalışır ve yalnızca bir yüzdesini telemetrinizi gönderme.

Devre dışı bırakabilirsiniz, ancak bu önerilmemektedir. Örnekleme, ilgili telemetri doğru tanılama amacıyla iletilen şekilde tasarlanmıştır.

## <a name="client-ip-address-is-0000"></a>İstemci IP adresi 0.0.0.0 ise

5 Şubat 2018'de, istemci IP adresini günlüğe kaldırdık duyurduk. Bu, coğrafi konum etkilemez.

> [!NOTE]
> IP adresinin ilk 3 sekizlik tabanda gerekiyorsa, kullanabileceğiniz bir [telemetri başlatıcısını](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer) özel bir öznitelik eklemek için.
> Bu, 5 Şubat 2018’den önce toplanan verileri etkilemez.

## <a name="wrong-geographical-data-in-user-telemetry"></a>Kullanıcı telemetri yanlış coğrafi verileri
Şehir, bölge ve ülke boyutları IP adreslerinden türetilir ve her zaman doğru değil. Bu IP adresleri için konum ilk işlenen ve ardından depolanacak 0.0.0.0 olarak değiştirildi.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Azure Cloud Services’da çalıştırma üzerine “yöntem bulunamadı” özel durumu
.NET 4.6 için mi oluşturdunuz? 4.6 sürümü Azure Cloud Services rollerinde otomatik olarak desteklenmez. Uygulamanızı çalıştırmadan önce [her role 4.6 sürümünü yükleyin](../../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="troubleshooting-logs"></a>Sorun giderme günlükleri

Altyapınız için sorun giderme günlükleri tutmak için bu yönergeleri izleyin.

### <a name="net-framework"></a>.NET Framework

1. Yükleme [Microsoft.AspNet.ApplicationInsights.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNet.ApplicationInsights.HostingStartup) NuGet paketi. Yüklemeniz gereken sürüm yüklü geçerli sürümü ile eşleşmelidir `Microsoft.ApplicationInsighs`

2. Şunlar için applicationınsights.config dosyasını değiştirin:

    ```xml
    <TelemetryModules>
      <Add Type="Microsoft.ApplicationInsights.Extensibility.HostingStartup.FileDiagnosticsTelemetryModule, Microsoft.AspNet.ApplicationInsights.HostingStartup">
        <Severity>Verbose</Severity>
        <LogFileName>mylog.txt</LogFileName>
        <LogFilePath>C:\\SDKLOGS</LogFilePath>
      </Add>
    </TelemetryModules>
    ```
    Uygulamanız için yapılandırılan konuma yazma izinlerine sahip olmalıdır

3. Bu yeni ayarları SDK'sı tarafından alınır, böylece işlemini yeniden başlatın.

4. İşiniz bittiğinde bu değişiklikleri geri al.

### <a name="net-core"></a>.NET Core

1. Yükleme [Microsoft.AspNetCore.ApplicationInsights.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.ApplicationInsights.HostingStartup) NuGet paketi. Yüklemeniz gereken sürüm yüklü geçerli sürümü ile eşleşmelidir `Microsoft.ApplicationInsights`

2. Değiştirme `ConfigureServices` yönteminde, `Startup.cs` sınıfı.:

    ```csharp
    services.AddSingleton<ITelemetryModule, FileDiagnosticsTelemetryModule>();
    services.ConfigureTelemetryModule<FileDiagnosticsTelemetryModule>( (module, options) => {
        module.LogFilePath = "C:\\SDKLOGS";
        module.LogFileName = "mylog.txt";
        module.Severity = "Verbose";
    } );
    ```
    Uygulamanız için yapılandırılan konuma yazma izinlerine sahip olmalıdır

3. Bu yeni ayarları SDK'sı tarafından alınır, böylece işlemini yeniden başlatın.

4. İşiniz bittiğinde bu değişiklikleri geri al.


## <a name="PerfView"></a> PerfView ile günlükleri toplama
[PerfView](https://github.com/Microsoft/perfview) toplamak ve birçok kaynaktan tanılama bilgileri görselleştirmenin CPU, bellek ve diğer sorunları gidermeye yardımcı olmak ücretsiz bir tanılama ve performans analizi aracı.

Application Insights SDK'sı tarafından PerfView yakalanabilir EventSource kendiliğinden sorun giderme günlükleri oturum açın.

Günlükleri toplamak için PerfView indirin ve şu komutu çalıştırın:
```cmd
PerfView.exe collect -MaxCollectSec:300 -NoGui /onlyProviders=*Microsoft-ApplicationInsights-Core,*Microsoft-ApplicationInsights-Data,*Microsoft-ApplicationInsights-WindowsServer-TelemetryChannel,*Microsoft-ApplicationInsights-Extensibility-AppMapCorrelation-Dependency,*Microsoft-ApplicationInsights-Extensibility-AppMapCorrelation-Web,*Microsoft-ApplicationInsights-Extensibility-DependencyCollector,*Microsoft-ApplicationInsights-Extensibility-HostingStartup,*Microsoft-ApplicationInsights-Extensibility-PerformanceCollector,*Microsoft-ApplicationInsights-Extensibility-PerformanceCollector-QuickPulse,*Microsoft-ApplicationInsights-Extensibility-Web,*Microsoft-ApplicationInsights-Extensibility-WindowsServer,*Microsoft-ApplicationInsights-WindowsServer-Core,*Microsoft-ApplicationInsights-Extensibility-EventSourceListener,*Microsoft-ApplicationInsights-AspNetCore
```

Bu parametre gerektiği gibi değiştirebilirsiniz:
- **MaxCollectSec**. PerfView süresiz olarak çalışmasını ve sunucunuzun performansından etkilenmesini önlemek için bu parametreyi ayarlayın.
- **OnlyProviders**. Yalnızca SDK'sından günlükleri toplamak için bu parametreyi ayarlayın. Belirli araştırmalarınıza temel alan bu liste özelleştirebilirsiniz. 
- **NoGui**. GUI olmadan günlükleri toplamak için bu parametreyi ayarlayın.


Daha fazla bilgi için
- [Performans izleme PerfView ile kayıt](https://github.com/dotnet/roslyn/wiki/Recording-performance-traces-with-PerfView).
- [Uygulama öngörüleri olay kaynakları](https://github.com/microsoft/ApplicationInsights-Home/tree/master/Samples/ETW)

## <a name="still-not-working"></a>Yine de çalışmıyor...
* [Uygulama anlayışları Forumu](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)
