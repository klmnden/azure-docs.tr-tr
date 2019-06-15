---
title: Telemetri geliştirme, test ve Azure Application Insights release | Microsoft Docs
description: Doğrudan telemetri için farklı kaynakları geliştirme, test ve üretim Damgalar.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 578e30f0-31ed-4f39-baa8-01b4c2f310c9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: mbullwin
ms.openlocfilehash: 2e9c599c12ed10327d352baee02500d2284d98d8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60713479"
---
# <a name="separating-telemetry-from-development-test-and-production"></a>Geliştirme, Test ve üretim telemetri ayırma

Bir web uygulaması'nın sonraki sürümü geliştirirken karışımı istemediğiniz [Application Insights](../../azure-monitor/app/app-insights-overview.md) alınan telemetri yeni sürüm ve zaten yayımlanmış sürümü. Karışıklığı önlemek için farklı geliştirme aşamalarına (ikey'leri) ayrı izleme anahtarı ile Application Insights kaynakları ayırmak için telemetri gönderin. Bir sürüm bir aşamadan diğerine taşınırken izleme anahtarını değiştirmek daha kolay hale getirmek için yapılandırma dosyasında değil, kod içinde ikey değerini ayarlamak yararlı olabilir. 

(Sisteminizin Azure bulut hizmeti ise yoktur [ayrı ikey'leri ayarlamanın başka bir yöntem](../../azure-monitor/app/cloudservices.md).)

## <a name="about-resources-and-instrumentation-keys"></a>Kaynaklar ve izleme anahtarları hakkında

Web uygulamanız için Application Insights izleme ayarladığınızda, Application ınsights'ı oluşturma *kaynak* Microsoft azure'da. Azure portalında görebilir ve uygulamanızdan toplanan telemetri analiz etmek için bu kaynak açın. Kaynak tarafından tanımlanan bir *izleme anahtarını* (ikey). Uygulamanızı izlemek için Application Insights paketini yüklediğinizde, telemetri gönderileceği bilir, izleme anahtarı ile yapılandırarak.

Genellikle ayrı kaynaklar ya da tek bir paylaşılan kaynak farklı senaryolarda kullanmayı seçin:

* Farklı ve bağımsız uygulamalar için - her uygulama için ayrı kaynak ve ikey değerini kullanın.
* Birden çok bileşenleri veya bir iş kolu uygulaması - rolleri bir [tek bir paylaşılan kaynak](../../azure-monitor/app/app-map.md) tüm bileşen uygulamaları için. Telemetri, filtre veya cloud_RoleName özelliğiyle bölümlenmiş.
* Geliştirme, Test ve yayın - ayrı kaynak ve ikey 'damga' içinde sistem sürümleri veya üretim aşaması için kullanın.
* A | B test - tek bir kaynak kullanın. Bir özellik çeşitler tanımlayan telemetri eklemek için bir Telemetryınitializer oluşturun.


## <a name="dynamic-ikey"></a> Dinamik izleme anahtarı

Kod üretim aşamalarını arasında hareket ettikçe ikey değiştirmek daha kolay hale getirmek için değil, kod içinde yapılandırma dosyasında ayarlayın.

Bir ASP.NET hizmetinde global.aspx.cs gibi bir başlatma yöntemi anahtarını ayarlayın:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

Bu örnekte, web yapılandırma dosyası farklı sürümlerini farklı kaynaklar için ikey'leri yerleştirilir. -Release betiğinin bir parçası bunu yapabilirsiniz - web yapılandırma dosyasını değiştirme, hedef kaynağın değiştireceksiniz.

### <a name="web-pages"></a>Web sayfaları
İKey Ayrıca, uygulamanızın web sayfaları'nda kullanılan [aldığınız hızlı başlangıç dikey penceresinden betik](../../azure-monitor/app/javascript.md). Tam anlamıyla betiğe kodlama yerine, sunucu durumu oluşturun. Örneğin, bir ASP.NET uygulamasında:

*Razor, JavaScript*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a>Ek Application Insights kaynakları oluşturma
Telemetri aynı bileşenin farklı Damgalar (geliştirme/test/üretim) veya farklı bir uygulama bileşenleri için ayırmak için daha sonra yeni bir Application Insights kaynağı oluşturmanız gerekir.

İçinde [portal.azure.com](https://portal.azure.com), Application Insights kaynağı ekleyin:

![Yeni, Application Insights öğesine tıklayın](./media/separate-resources/01-new.png)

* **Uygulama türü** genel bakış dikey ve bulunan özelliklerin gördüğünüz etkiler [ölçüm Gezgini](../../azure-monitor/app/metrics-explorer.md). Uygulama türünü görmüyorsanız, web sayfaları için web türlerinden birini seçin.
* **Kaynak grubu** gibi özelliklerini yönetmek için bir kolaylık [erişim denetimi](../../azure-monitor/app/resources-roles-access-control.md). Geliştirme, test ve üretim için ayrı kaynak gruplarını kullanabilirsiniz.
* **Abonelik** ödeme hesabınız azure'da.
* **Konum** olan verilerinizi burada saklarız. Şu anda değiştirilemez. 
* **Panoya ekleme** kaynağınızın hızlı erişim kutucuk Azure giriş sayfanızda koyar. 

Kaynak oluşturma, birkaç saniye sürer. İşlem tamamlandığında, bir uyarı görürsünüz.

(Yazabileceğiniz bir [PowerShell Betiği](../../azure-monitor/app/powershell-script-create-resource.md) otomatik olarak bir kaynak oluşturun.)

### <a name="getting-the-instrumentation-key"></a>İzleme anahtarını alma
Oluşturduğunuz kaynağın izleme anahtarını tanımlar. 

![Essentials'ı tıklatın, izleme anahtarı, CTRL + C](./media/separate-resources/02-props.png)

İhtiyacınız olan tüm kaynakların uygulamanızı veri gönderip izleme anahtarı.

## <a name="filter-on-build-number"></a>Derleme numarası üzerinde filtreleme
Uygulamanızın yeni bir sürüm yayımladığınızda, telemetri farklı derlemelerden ayırmak mümkün olmasını istersiniz.

Uygulama sürümü özelliği ayarlayabilirsiniz, böylece, filtreleyebilirsiniz [arama](../../azure-monitor/app/diagnostic-search.md) ve [ölçüm Gezgini](../../azure-monitor/app/metrics-explorer.md) sonuçları.

![Bir özellik üzerinde filtreleme](./media/separate-resources/050-filter.png)

Uygulama sürümü özelliğinin ayarlanması birkaç farklı yöntem vardır.

* Doğrudan ayarlayın:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* Bu satırda kaydırma bir [telemetri başlatıcısını](../../azure-monitor/app/api-custom-events-metrics.md#defaults) için tüm TelemetryClient örneklerini tutarlı bir şekilde ayarlandığından emin olun.
* [ASP.NET] Sürüm kümesinde `BuildInfo.config`. Web modülü sürüm BuildLabel düğümünden'kurmak seçer. Bu dosyayı projenize eklemek ve Çözüm Gezgini'nde her zaman Kopyala özelliğini ayarlamayı unutmayın.

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] Buildınfo.config Msbuild'de otomatik olarak oluşturur. Bunu yapmak için birkaç satır ekleyin, `.csproj` dosyası:

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Bu adlı bir dosya oluşturur. *yourProjectName*. Buildınfo.config. Yayımlama işlemi için Buildınfo.config yeniden adlandırır.

    Visual Studio ile derleme yaparken derleme etiketi (AutoGen_...) yer tutucu içerir. Ancak, MSBuild ile yapılandırıldığında, doğru sürüm numarasıyla doldurulur.

    Sürüm numaraları oluşturmak MSBuild izin vermek için sürümünü gibi ayarlama `1.0.*` AssemblyReference.cs içinde

## <a name="version-and-release-tracking"></a>Sürüm ve sürüm izleme
Uygulama sürümünü izlemek için `buildinfo.config` dosyasının Microsoft Build Engine işleminiz tarafından oluşturulduğundan emin olun. .csproj dosyanızda şunları ekleyin:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Yapı bilgisi mevcut olduğunda Application Insights web modülü **Uygulama sürümünü** telemetrinin her bir öğesine bir özellik olarak ekler. Bu sayede, [tanılama aramaları](../../azure-monitor/app/diagnostic-search.md) gerçekleştirirken veya [ölçümleri keşfederken](../../azure-monitor/app/metrics-explorer.md) sürüme göre filtreleyebilirsiniz.

Bununla birlikte, derleme sürüm numarasının Visual Studio’daki geliştirici derlemesi tarafından değil de yalnızca Microsoft Build Engine tarafından oluşturulduğunu fark edersiniz.

### <a name="release-annotations"></a>Sürüm ek açıklamaları
Azure DevOps kullanırsanız, yapabilecekleriniz [ek açıklama işaretçisi](../../azure-monitor/app/annotations.md) grafiklerinize yeni bir sürümün her eklendi. Aşağıdaki görüntüde bu işaretin nasıl göründüğü gösterilmiştir.

![Bir grafikteki örnek sürüm ek açıklamasının ekran görüntüsü](media/separate-resources/release-annotation.png)
## <a name="next-steps"></a>Sonraki adımlar

* [Birden çok rol için paylaşılan kaynaklar](../../azure-monitor/app/app-map.md)
* [Bir ayırt etmek için bir Telemetri Başlatıcısı oluştur | B çeşitleri](../../azure-monitor/app/api-filtering-sampling.md#add-properties)
