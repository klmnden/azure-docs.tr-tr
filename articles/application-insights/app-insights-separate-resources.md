---
title: "Telemetri geliştirme, test ve yayın Azure Application Insights'ta | Microsoft Docs"
description: "Doğrudan telemetri için geliştirme, test ve üretim Damgalar farklı kaynakları."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 578e30f0-31ed-4f39-baa8-01b4c2f310c9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: mbullwin
ms.openlocfilehash: 8d95958bce0597bfb16ef1c6b99b72ce9134e66f
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="separating-telemetry-from-development-test-and-production"></a>Geliştirme, Test ve üretim telemetri ayırma

Bir web uygulaması bir sonraki sürümünün geliştirirken, karışık istemediğiniz [Application Insights](app-insights-overview.md) telemetri yeni sürümü ve önceden yayımlanmış bir sürüm. Karışıklığı önlemek için Application Insights kaynaklarını ayrı izleme anahtarı (ikeys) ile ayırmak için farklı geliştirme aşamaları telemetri gönderin. Sürümü bir aşamadan diğerine taşınırken izleme anahtarı değiştirmek kolaylaştırmak için yapılandırma dosyasında yerine kodda ikey ayarlamak yararlı olabilir. 

(Sisteminiz bir Azure bulut hizmeti ise var. [ayrı ikeys ayarlama başka bir yöntem](app-insights-cloudservices.md).)

## <a name="about-resources-and-instrumentation-keys"></a>Kaynaklar ve izleme anahtarı hakkında

Web uygulamanız için Application Insights izleme işlevini ayarlama Application Insights oluşturduğunuzda *kaynak* Microsoft azure'da. Azure portalında görebilir ve uygulamanızdan toplanan telemetri analiz etmek için bu kaynak açın. Kaynak tarafından tanımlanan bir *izleme anahtarını* (ikey). Uygulamanızı izlemek için Application Insights paketini yüklediğinizde, telemetri göndermeyi nerede olduğunu bilmesini şekilde, izleme anahtarı ile yapılandırabilirsiniz.

Genellikle ayrı kaynaklar ya da tek bir paylaşılan kaynağa farklı senaryolarda kullanmak için seçin:

* Farklı, bağımsız uygulamalar - her uygulama için ayrı kaynak ve ikey kullanın.
* Birden çok bileşenleri ya da bir iş uygulaması - rolleri kullanmak bir [tek bir paylaşılan kaynak](app-insights-monitor-multi-role-apps.md) tüm bileşen uygulamaları için. Telemetri filtre veya cloud_RoleName özelliği tarafından ayrılmış.
* Geliştirme, Test ve yayın - ayrı kaynak ve ikey 'damga' ın sistem sürümleri veya üretim aşaması için kullanın.
* A | B testi - tek bir kaynak kullanın. Çeşitler tanımlayan telemetri özellik eklemek için bir TelemetryInitializer oluşturun.


## <a name="dynamic-ikey"></a>Dinamik izleme anahtarı

Kod üretim aşamaları arasında hareket ederken ikey değiştirmek kolaylaştırmak için yerine kodda yapılandırma dosyasında ayarlayın.

Anahtar başlatma yöntemini, bir ASP.NET hizmetinde global.aspx.cs gibi ayarlayın:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.AppSettings["ikey"];
      ...

Bu örnekte, web yapılandırma dosyası farklı sürümlerini farklı kaynaklar için ikeys yerleştirilir. -Release komut dosyasının bir parçası yapabileceğiniz - web yapılandırma dosyasını değiştirme hedef kaynak değiştireceksiniz.

### <a name="web-pages"></a>Web sayfaları
İKey Ayrıca, uygulamanızın web sayfalarında kullanılır [hızlı başlangıç dikey penceresinden aldığınız betik](app-insights-javascript.md). Harfi harfine betiğe kodlama yerine, sunucu durumundan oluşturur. Örneğin, bir ASP.NET uygulamasında:

*Razor JavaScript'te*

    <script type="text/javascript">
    // Standard Application Insights web page script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      "@Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey"
    }) // ...


## <a name="create-additional-application-insights-resources"></a>Ek Application Insights kaynakları oluşturun
Telemetri aynı bileşenin farklı Damgalar (dev/test/üretim) veya farklı bir uygulama bileşenleri için ayırmak için daha sonra yeni bir Application Insights kaynağı oluşturma gerekir.

İçinde [portal.azure.com](https://portal.azure.com), Application Insights kaynağı ekleyin:

![Yeni, Application Insights öğesine tıklayın](./media/app-insights-separate-resources/01-new.png)

* **Uygulama türü** genel bakış dikey penceresinde ve kullanılabilir özellikler Görünenleri etkiler [ölçüm Gezgini](app-insights-metrics-explorer.md). Uygulama, türünü görmüyorsanız, web sayfaları için web türlerinden birini seçin.
* **Kaynak grubu** gibi özellikleri yönetmek için kullanışlı olan [erişim denetimi](app-insights-resources-roles-access-control.md). Ayrı kaynak grupları, geliştirme, test ve üretim için kullanabilirsiniz.
* **Abonelik** ödeme hesabınız azure'da.
* **Konum** burada biz verilerinizi tutmak olduğu. Şu anda değiştirilemez. 
* **Pano ekleyin** kaynağınız için hızlı erişim kutucuğu, Azure giriş sayfasında koyar. 

Kaynak oluşturma birkaç saniye sürer. Bu yapıldığında, bir uyarı görürsünüz.

(Yazabileceğiniz bir [PowerShell Betiği](app-insights-powershell-script-create-resource.md) otomatik olarak bir kaynak oluşturmak için.)

### <a name="getting-the-instrumentation-key"></a>İzleme anahtarı alma
İzleme anahtarını oluşturduğunuz kaynak tanımlar. 

![Essentials'ı tıklatın, CTRL + C izleme anahtarı](./media/app-insights-separate-resources/02-props.png)

Tüm kaynaklar da uygulamanızı veri gönderir izleme anahtarı gerekir.

## <a name="filter-on-build-number"></a>Yapı numarası filtre
Uygulamanızı yeni bir sürümü yayımladığınızda, telemetri farklı yapılar ayırmak için istersiniz.

Uygulama sürümü özelliği filtre uygulayabilirsiniz ayarlayabilirsiniz [arama](app-insights-diagnostic-search.md) ve [ölçüm Gezgini](app-insights-metrics-explorer.md) sonuçları.

![Bir özellik filtreleme](./media/app-insights-separate-resources/050-filter.png)

Uygulama sürümü özelliğinin ayarlanması birkaç farklı yöntem vardır.

* Doğrudan ayarlayın:

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`
* Bu satırda sarmalamak bir [telemetri Başlatıcı](app-insights-api-custom-events-metrics.md#defaults) TelemetryClient hepsinin tutarlı bir şekilde ayarlandığından emin olmak için.
* [ASP.NET] Sürüm kümesinde `BuildInfo.config`. Web modülü BuildLabel düğümü sürümünden yukarı seçer. Bu dosyayı projenize eklemek ve her zaman Kopyala özelliği Çözüm Gezgini'nde ayarlamayı unutmayın.

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] BuildInfo.config Msbuild'de otomatik olarak oluşturur. Bunu yapmak için birkaç satır ekleyin, `.csproj` dosyası:

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
    ```

    Bu adlı bir dosya oluşturur *yourProjectName*. BuildInfo.config. Yayımlama işlemi için BuildInfo.config yeniden adlandırır.

    Visual Studio ile derlediğinizde derleme etiketi yer tutucu (AutoGen_...) içerir. Ancak MSBuild ile yapılandırıldığında, doğru sürüm numarasıyla doldurulur.

    Sürüm numaraları oluşturmak MSBuild sağlamak için sürüm gibi ayarlayın `1.0.*` AssemblyReference.cs içinde

## <a name="version-and-release-tracking"></a>Sürüm ve sürüm izleme
Uygulama sürümünü izlemek için `buildinfo.config` dosyasının Microsoft Build Engine işleminiz tarafından oluşturulduğundan emin olun. .csproj dosyanızda şunları ekleyin:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Yapı bilgisi mevcut olduğunda Application Insights web modülü **Uygulama sürümünü** telemetrinin her bir öğesine bir özellik olarak ekler. Bu sayede, [tanılama aramaları](app-insights-diagnostic-search.md) gerçekleştirirken veya [ölçümleri keşfederken](app-insights-metrics-explorer.md) sürüme göre filtreleyebilirsiniz.

Bununla birlikte, derleme sürüm numarasının Visual Studio’daki geliştirici derlemesi tarafından değil de yalnızca Microsoft Build Engine tarafından oluşturulduğunu fark edersiniz.

### <a name="release-annotations"></a>Sürüm ek açıklamaları
Visual Studio Team Services’ı kullanıyorsanız yeni bir sürüm yayımladığınızda grafiklerinize [ek açıklama işaretçisi](app-insights-annotations.md) eklenmesini sağlayabilirsiniz. Aşağıdaki görüntüde bu işaretin nasıl göründüğü gösterilmiştir.

![Bir grafikteki örnek sürüm ek açıklamasının ekran görüntüsü](./media/app-insights-asp-net/release-annotation.png)
## <a name="next-steps"></a>Sonraki adımlar

* [Birden çok rol için paylaşılan kaynakları](app-insights-monitor-multi-role-apps.md)
* [A ayırt etmek için bir Telemetri Başlatıcı oluşturun | B çeşitleri](app-insights-api-filtering-sampling.md#add-properties)
