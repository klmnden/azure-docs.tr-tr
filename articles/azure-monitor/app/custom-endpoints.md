---
title: Azure İzleyici - Azure Application ınsights'ı geçersiz varsayılan SDK uç noktaları | Microsoft Docs
description: Varsayılan Azure Application Insights SDK'sı uç noktaları gibi Azure kamu bölgeleri için değiştirin.
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: mbullwin
ms.openlocfilehash: d086815373b84c0f2a70144a505108875fc04981
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443314"
---
 # <a name="application-insights-overriding-default-endpoints"></a>Application Insights varsayılan uç noktalar geçersiz kılma

Belirli bölgelerde için uygulama anlayışları'ndan veri göndermek için varsayılan uç nokta adresleri geçersiz kılmak gerekir. Her SDK, tüm bu makalede açıklanan biraz daha farklı değişiklikler gerektirir. Örnek kodu ayarlama ve yer tutucu değerlerini değiştirerek bu değişiklikler gerektirir `QuickPulse_Endpoint_Address`, `TelemetryChannel_Endpoint_Address`, ve `Profile_Query_Endpoint_address` belirli bölgeniz için gerçek bir uç nokta adresleri. Bu makalenin sonundaki yapılandırmanın gerekli olduğu bölge için uç nokta adresleri bağlantılar içerir.

## <a name="sdk-code-changes"></a>SDK kod değişiklikleri

### <a name="net-with-applicationinsightsconfig"></a>Applicationınsights.config ile .NET

> [!NOTE]
> SDK'sını yükseltme gerçekleştirilmeden zaman applicationınsights.config dosyası otomatik olarak üzerine yazılır. SDK'sı yükseltmesini gerçekleştirdikten sonra bölgeye özgü uç nokta değerleri yeniden girin emin olun.

```xml
<ApplicationInsights>
  ...
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <QuickPulseServiceEndpoint>Custom_QuickPulse_Endpoint_Address</QuickPulseServiceEndpoint>
    </Add>
  </TelemetryModules>
    ...
  <TelemetryChannel>
     <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </TelemetryChannel>
  ...
  <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
    <ProfileQueryEndpoint>Profile_Query_Endpoint_address</ProfileQueryEndpoint>
  </ApplicationIdProvider>
  ...
</ApplicationInsights>
```

### <a name="net-core"></a>.NET Core

Appsettings.json dosyasını aşağıdaki gibi ana uç ayarlamak için projenizdeki değiştirin:

```json
"ApplicationInsights": {
    "InstrumentationKey": "instrumentationkey",
    "TelemetryChannel": {
      "EndpointAddress": "TelemetryChannel_Endpoint_Address"
    }
  }
```

Canlı Ölçümler ve profili sorgu uç noktası için değerleri, yalnızca kod ayarlanabilir. Kod aracılığıyla tüm uç nokta değerleri için varsayılan değerleri geçersiz kılmak için aşağıdaki değişiklikleri yapın `ConfigureServices` yöntemi `Startup.cs` dosyası:

```csharp
using Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse; //place at top of Startup.cs file

   services.ConfigureTelemetryModule<QuickPulseTelemetryModule>((module, o) => module.QuickPulseServiceEndpoint="QuickPulse_Endpoint_Address");

   services.AddSingleton(new ApplicationInsightsApplicationIdProvider() { ProfileQueryEndpoint = "Profile_Query_Endpoint_address" });

   services.AddSingleton<ITelemetryChannel>(new ServerTelemetryChannel() { EndpointAddress = "TelemetryChannel_Endpoint_Address" });

    //place in ConfigureServices method. If present, place this prior to   services.AddApplicationInsightsTelemetry("instrumentation key");
```

### <a name="java"></a>Java

Varsayılan uç nokta adresini değiştirmek için applicationınsights.xml dosyasını değiştirin.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>ffffeeee-dddd-cccc-bbbb-aaaa99998888</InstrumentationKey>
  <TelemetryModules>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
  </TelemetryModules>
  <TelemetryInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>
  </TelemetryInitializers>
  <!--Add the following Channel value to modify the Endpoint address-->
  <Channel type="com.microsoft.applicationinsights.channel.concrete.inprocess.InProcessTelemetryChannel">
  <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </Channel>
</ApplicationInsights>
```

### <a name="spring-boot"></a>Spring Boot

Değiştirme `application.properties` dosya ve ekleyin:

```yaml
azure.application-insights.channel.in-process.endpoint-address= TelemetryChannel_Endpoint_Address
```

### <a name="nodejs"></a>Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY');
appInsights.defaultClient.config.endpointUrl = "TelemetryChannel_Endpoint_Address"; // ingestion
appInsights.defaultClient.config.profileQueryEndpoint = "Profile_Query_Endpoint_address"; // appid/profile lookup
appInsights.defaultClient.config.quickPulseHost = "QuickPulse_Endpoint_Address"; //live metrics
appInsights.Configuration.start();
```

Uç noktaları ortam değişkenlerini de yapılandırılabilir:

```
Instrumentation Key: "APPINSIGHTS_INSTRUMENTATIONKEY"
Profile Endpoint: "Profile_Query_Endpoint_address"
Live Metrics Endpoint: "QuickPulse_Endpoint_Address"
```

### <a name="javascript"></a>JavaScript

```javascript
<script type="text/javascript">
   var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){
      function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/next/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t
   }({
      instrumentationKey:"INSTRUMENTATION_KEY"
      endpointUrl: "TelemetryChannel_Endpoint_Address"
   });

   window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

## <a name="regions-that-require-endpoint-modification"></a>Uç nokta değişikliği gerektiren bölgeler

Şu anda uç noktası değişiklikleri gerektiren tek bölge olduğunu [Azure kamu](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights) ve [Azure Çin](https://docs.microsoft.com/azure/china/resources-developer-guide).

|Bölge |  Uç nokta adı | Değer |
|-----------------|:------------|:-------------|
| Azure Çin | Telemetri kanal | `https://dc.applicationinsights.azure.cn/v2/track` |
| Azure Çin | QuickPulse (Canlı ölçümleri) |`https://quickpulse.applicationinsights.azure.cn/QuickPulseService.svc` |
| Azure Çin | Profil sorgu |`https://dc.applicationinsights.azure.cn/api/profiles/{0}/appId`  |
| Azure Kamu | Telemetri kanal |`https://dc.applicationinsights.us/v2/track` |
| Azure Kamu | QuickPulse (Canlı ölçümleri) |`https://quickpulse.applicationinsights.us/QuickPulseService.svc` |
| Azure Kamu | Profil sorgu |`https://dc.applicationinsights.us/api/profiles/{0}/appId` |

> [!NOTE]
> Kodsuz Aracı Uzantısı tabanlı Azure uygulama hizmetleri için izleme **şu anda desteklenmeyen** bu bölgelerde. Bu işlevselliği kullanılabilir duruma geldiğinde bu makale güncelleştirilecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure devlet kurumları için özel değişiklikler hakkında daha fazla bilgi için ayrıntılı yönergeler için başvurun [Azure izleme ve Yönetim](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights).
- Azure Çin hakkında daha fazla bilgi için bakın [Azure Çin Playbook](https://docs.microsoft.com/azure/china/).