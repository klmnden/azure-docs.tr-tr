---
title: Azure Application Insights telemetri bağıntısı | Microsoft Docs
description: Application Insights telemetri bağıntısı
services: application-insights
documentationcenter: .net
author: lgayhardt
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/14/2019
ms.reviewer: sergkanz
ms.author: lagayhar
ms.openlocfilehash: 565f08f0c69aef393a9296f3cce90570a3f0bc2c
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59683036"
---
# <a name="telemetry-correlation-in-application-insights"></a>Application ınsights telemetri bağıntısı

Mikro hizmetler dünyasında, hizmetin çeşitli bileşenlerinin yapılacak işleri her bir mantıksal işlemi gerektirir. Bu bileşenlerin her birini ayrı olarak tarafından izlenebilir [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md). Web uygulaması bileşeni, kullanıcı kimlik bilgilerini doğrulamak için kimlik doğrulama sağlayıcısı bileşeni ile ve görselleştirme için verileri almak için API bileşeni ile iletişim kurar. API bileşeni, diğer hizmetlerden veri sorgulamak ve fatura bileşeni bu çağrı hakkında bilgilendirmek için önbelleği sağlayıcısı bileşenlerini kullanın. Bileşeni algılamak için kullanacağınız application Insights destekler dağıtılmış telemetri bağıntısı, hatalar veya performans düşüşü sorumludur.

Bu makalede, birden çok bileşen tarafından gönderilen telemetrinin bağıntısını kurmanızı Application Insights tarafından kullanılan veri modeli açıklanmaktadır. Bağlam yayma teknikleri ve protokoller ele alınmaktadır. Ayrıca, farklı diller ve platformlarda bağıntı kavramları uygulanması da kapsar.

## <a name="data-model-for-telemetry-correlation"></a>Telemetri bağıntısı için veri modeli

Application Insights'ı tanımlayan bir [veri modeli](../../azure-monitor/app/data-model.md) dağıtılmış telemetri bağıntısı için. Telemetri mantıksal işlemi ile ilişkilendirilecek adlı bir bağlam alan her telemetri öğesine sahip `operation_Id`. Bu tanımlayıcı dağıtılmış izlemede her telemetri öğesinin tarafından paylaşılır. Bu nedenle, hatta telemetri kaybı tek bir katmandan ile diğer bileşenleri tarafından raporlanan telemetri yine de ilişkilendirebilirsiniz.

Dağıtılmış bir mantıksal işlemi genellikle bir dizi bileşenlerinden biri tarafından işlenen istekleri daha küçük işlemlerini oluşur. Bu işlemler tarafından tanımlanan [istek telemetri](../../azure-monitor/app/data-model-request-telemetry.md). Her istek telemetrisi kendi bölümüne sahiptir `id` onu benzersiz şekilde ve genel olarak tanımlar. Ve bu istekle ilişkili olan tüm telemetri öğeleri (örneğin, izlemeleri ve özel durumları) ayarlamalısınız `operation_parentId` istek değerine `id`.

Başka bir bileşen için HTTP çağrısı gibi her giden işlem [bağımlılık telemetrisi](../../azure-monitor/app/data-model-dependency-telemetry.md). Bağımlılık telemetrisi ayrıca tanımlar kendi `id` genel olarak benzersiz. Bu bağımlılık çağrısının tarafından başlatılan telemetriyi kullanır bu isteği `id` olarak kendi `operation_parentId`.

Kullanarak dağıtılmış mantıksal işlemi bir görünümünü oluşturabilirsiniz `operation_Id`, `operation_parentId`, ve `request.id` ile `dependency.id`. Bu alanlar da telemetri çağrıları nedensellik ilişkilerini sırasını tanımlar.

Bir mikro hizmetler ortamında farklı depolama öğelerine bileşenleri izlemelerinden gidebilirsiniz. Her bileşen kendi izleme anahtarı, Application Insights'ta sahip olabilir. Mantıksal işlem için telemetri almak için her depolama öğesinden veri sorgulaması gerekir. Depolama öğe sayısı çok büyük olduğunda, sonraki aranacağı hakkında ipucu gerekir. Bu sorunu çözmek için iki alan Application Insights veri modelini tanımlar: `request.source` ve `dependency.target`. İlk alanı, bağımlılık istek başlatılan bileşeni belirtir ve saniye bileşeni bağımlılık çağrısının bir yanıt döndürdü tanımlar.

## <a name="example"></a>Örnek

Adlı bir dış API kullanarak bir hisse senedi geçerli Pazar fiyatını gösterir hisse senedi fiyatlarını adlı bir uygulamanın bir örneği ele alalım `Stock`. Adlı bir sayfa hisse senedi fiyatlarını uygulamaya sahip `Stock page` istemci web tarayıcısı kullanarak açılan `GET /Home/Stock`. Uygulama sorguları `Stock` HTTP çağrısı kullanarak API `GET /api/stock/value`.

Bir sorgu çalıştırılarak elde edilen telemetri çözümleyebilirsiniz:

```kusto
(requests | union dependencies | union pageViews)
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

Sonuçlarda tüm telemetri öğelerinin kök paylaşmak Not `operation_Id`. Ne zaman bir Ajax çağrısı yapıldığında sayfasından yeni bir benzersiz kimliği (`qJSXU`) için bağımlılık telemetrisi atanır ve sayfa görüntülemesi kimliği olarak kullanılan `operation_ParentId`. Sunucu isteği daha sonra Ajax kimlik olarak kullanır `operation_ParentId`.

| Itemtype   | ad                      | Kimlik           | operation_ParentId | operation_ıd |
|------------|---------------------------|--------------|--------------------|--------------|
| Sayfa görünümü   | Stok sayfası                |              | STYz               | STYz         |
| bağımlılık | GET /Home/stok           | qJSXU        | STYz               | STYz         |
| istek    | GET Home/stok            | KqKwlrSt9PA= | qJSXU              | STYz         |
| bağımlılık | /Api/Stock/Value Al      | bBrf2L7mm2g= | KqKwlrSt9PA=       | STYz         |

Zaman çağrı `GET /api/stock/value` yapılan bir dış hizmet için ayarlayabilirsiniz bu nedenle sunucu kimliğini bilmek istiyorsunuz `dependency.target` uygun şekilde alanı. İzleme, dış hizmete desteklemediğinde `target` hizmetin konak adını ayarlayın (örneğin, `stock-prices-api.com`). Ancak, hizmeti önceden tanımlanmış bir HTTP üstbilgisi döndürerek kendisini tanımlar `target` sağlayan bir dağıtılmış izleme telemetrisi, hizmetten sorgulayarak oluşturmak Application Insights hizmet kimliğini içerir.

## <a name="correlation-headers"></a>Bağıntı üstbilgileri

İçin bir RFC teklifi üzerinde çalışıyoruz [bağıntı HTTP Protokolü](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Bu teklif, iki üstbilgi tanımlar:

- `Request-Id`: Çağrı genel olarak benzersiz Kimliğini taşır.
- `Correlation-Context`: Dağıtılmış İzleme özelliklerini ad / değer çiftleri koleksiyonu taşır.

Standart iki şemaları için de tanımlar `Request-Id` oluşturma: sabit ve hiyerarşik. İyi bilinen bir düz şemasıyla `Id` anahtarı için tanımlanmış `Correlation-Context` koleksiyonu.

Application Insights tanımlar [uzantısı](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) bağıntı HTTP protokolü için. Kullandığı `Request-Context` şu anki çağırıcı ya da çağrılan tarafından kullanılan özelliklerin koleksiyonunu yayılması ad-değer çiftleri. Application Insights SDK'sını ayarlamak için bu üstbilgiyi kullanır `dependency.target` ve `request.source` alanları.

### <a name="w3c-distributed-tracing"></a>W3C dağıtılmış izleme

Biz geçiş [W3C dağıtılmış izleme biçimi](https://w3c.github.io/trace-context/). Tanımlar:

- `traceparent`: Genel olarak benzersiz işlem kimliği ve aramanın benzersiz tanımlayıcısı taşır.
- `tracestate`: İzleme sistemine özgü bağlamını yürütür.

#### <a name="enable-w3c-distributed-tracing-support-for-classic-aspnet-apps"></a>Klasik ASP.NET uygulamaları için Dağıtılmış W3C İzleme desteğini etkinleştir

Bu özellik kullanılabilir `Microsoft.ApplicationInsights.Web` ve `Microsoft.ApplicationInsights.DependencyCollector` paketleri ile sürüm 2.8.0-beta1 başlatılıyor.
Bu, varsayılan olarak devre dışıdır. Bunu etkinleştirmek için değiştirme `ApplicationInsights.config`:

- Altında `RequestTrackingTelemetryModule`, ekleme `EnableW3CHeadersExtraction` ayarlanan değere sahip öğe `true`.
- Altında `DependencyTrackingTelemetryModule`, ekleme `EnableW3CHeadersInjection` ayarlanan değere sahip öğe `true`.

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>ASP.NET Core uygulamaları için Dağıtılmış W3C İzleme desteğini etkinleştirme

Bu özellik bulunduğu `Microsoft.ApplicationInsights.AspNetCore` sürüm 2.5.0-beta1 ve `Microsoft.ApplicationInsights.DependencyCollector` sürüm 2.8.0-beta1.
Bu, varsayılan olarak devre dışıdır. Bunu etkinleştirmek için ayarlanmış `ApplicationInsightsServiceOptions.RequestCollectionOptions.EnableW3CDistributedTracing` için `true`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry(o => 
        o.RequestCollectionOptions.EnableW3CDistributedTracing = true );
    // ....
}
```

#### <a name="enable-w3c-distributed-tracing-support-for-java-apps"></a>Java uygulamalarını W3C dağıtılmış İzleme desteğini etkinleştirme

- **Gelen yapılandırma**

  - Java EE uygulama, aşağıdaki ekleme `<TelemetryModules>` Applicationınsights.XML içinde etiketi:

    ```xml
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule>
       <Param name = "W3CEnabled" value ="true"/>
       <Param name ="enableW3CBackCompat" value = "true" />
    </Add>
    ```
  - Spring Boot uygulamaları için aşağıdaki özellikleri ekleyin:

    - `azure.application-insights.web.enable-W3C=true`
    - `azure.application-insights.web.enable-W3C-backcompat-mode=true`

- **Giden yapılandırma**

  AI için aşağıdakileri ekleyin-Agent.xml:

  ```xml
  <Instrumentation>
    <BuiltIn enabled="true">
      <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
    </BuiltIn>
  </Instrumentation>
  ```

  > [!NOTE]
  > Geriye dönük uyumluluk modu, varsayılan olarak etkinleştirilir ve `enableW3CBackCompat` parametresi isteğe bağlıdır. Yalnızca geriye dönük uyumluluk kapatmak istediğinizde, bunu kullanın.
  >
  > İdeal olarak, bu tüm hizmetlerinizi W3C protokolünü destekleyen SDK'sının daha yeni sürümleri için ne zaman güncelleştirildi devre dışı etkinleştirmeniz. Bu yeni SDK'ları için olabildiğince çabuk taşıma önemle öneririz.

> [!IMPORTANT]
> Gelen ve giden yapılandırmaları tam olarak aynı olduğundan emin olun.

## <a name="opentracing-and-application-insights"></a>OpenTracing ve Application Insights

[OpenTracing veri modeli belirtimi](https://opentracing.io/) ve Application Insights veri modelleri, aşağıdaki şekilde eşlenir:

| Application Insights                  | OpenTracing                                       |
|------------------------------------   |-------------------------------------------------  |
| `Request`, `PageView`                 | `Span` ile `span.kind = server`                  |
| `Dependency`                          | `Span` ile `span.kind = client`                  |
| `Id` ' ın `Request` ve `Dependency`    | `SpanId`                                          |
| `Operation_Id`                        | `TraceId`                                         |
| `Operation_ParentId`                  | `Reference` tür `ChildOf` (üst aralık)   |

Daha fazla bilgi için [Application Insights telemetri veri modeli](../../azure-monitor/app/data-model.md). 

OpenTracing OpenTracing kavramları tanımları için bkz. [belirtimi](https://github.com/opentracing/specification/blob/master/specification.md) ve [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md).

## <a name="telemetry-correlation-in-net"></a>. NET'te telemetri bağıntısı

Zamanla, telemetri ve tanılama günlüklerini ilişkilendirmek için çeşitli yollar .NET tanımlanan:

- `System.Diagnostics.CorrelationManager` izlenmesini sağlar [LogicalOperationStack ve ActivityID](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). 
- `System.Diagnostics.Tracing.EventSource` ve olay izleme için Windows (ETW) tanımlayın [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx) yöntemi.
- `ILogger` kullanan [günlük kapsamları](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). 
- Windows Communication Foundation (WCF) ve HTTP kablo "geçerli" bağlam yayma ayarlama.

Ancak, bu yöntemleri otomatik dağıtılmış İzleme desteğini etkinleştir kaydetmedi. `DiagnosticSource` Çapraz makine bağıntı otomatik desteklemek için bir yoldur. .NET kitaplıkları 'DiagnosticSource' desteklemek ve HTTP gibi aktarım aracılığıyla bağıntı bağlam otomatik çapraz makine yayılmasını izin.

[Etkinlikleri kılavuza](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) içinde `DiagnosticSource` izleme etkinliklerin temellerini açıklar.

ASP.NET Core 2.0 HTTP üst bilgileri ve yeni bir etkinlik başlangıç ayıklama destekler.

`System.Net.HttpClient`, sürüm 4.1.0, HTTP çağrısı bir etkinlik bağıntısı HTTP üst bilgileri ve izleme, otomatik yerleştirme destekler.

Yeni bir HTTP modülü [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/), klasik ASP.NET için. Bu modül kullanarak telemetri bağıntısı uygular `DiagnosticSource`. Gelen istek üst bilgilere göre bir etkinlik başlar. İstek işleme bile Internet Information Services (IIS) işleminin her adımı farklı bir yönetilen iş parçacığı üzerinde çalıştığında çalışmaları için farklı aşamalarında alınan telemetri hatalarla ilintilidir.

Başlangıç sürümü 2.4.0-beta1 ile Application Insights SDK kullanan `DiagnosticSource` ve `Activity` telemetri toplamak ve geçerli etkinliği ile ilişkilendirmek için.

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-the-java-sdk"></a>Java SDK telemetri bağıntısı

[Java için Application Insights SDK'sı](../../azure-monitor/app/java-get-started.md) sürüm 2.0.0 ile telemetri başlayan otomatik bağıntı destekler. Otomatik olarak doldurulur `operation_id` istek kapsamında verilen tüm telemetri (örneğin, izlemeler, özel durumlar ve özel olaylar). Bu da (daha önce açıklanan) HTTP aracılığıyla hizmetten hizmete çağrılar için bağıntı üstbilgileri yayma varsa üstlenir [Java SDK'sı aracı](../../azure-monitor/app/java-agent.md) yapılandırılır.

> [!NOTE]
> Yalnızca Apache HTTPClient yapılan çağrılar bağıntı özelliği için desteklenir. Spring RestTemplate veya Feign kullanıyorsanız, her ikisi de bileşenler ile Apache HTTPClient kullanılabilir.

Mesajlaşma teknolojilerinin (Bu tür Kafka, RabbitMQ veya Azure Service Bus) arasında bağlamı otomatik yayma şu anda desteklenmiyor. Ancak, bu senaryolara el ile kullanarak kod mümkün `trackDependency` ve `trackRequest` API'leri. Bu API'ler, bir bağımlılık telemetrisi olan bir üretici tarafından sıraya bir ileti temsil eder ve bir tüketici tarafından işlenmekte olan bir ileti isteğini temsil eder. Bu durumda, her ikisi de `operation_id` ve `operation_parentId` ileti özelliklerinde yayılır.

### <a name="telemetry-correlation-in-asynchronous-java-application"></a>Zaman uyumsuz bir Java uygulamasında telemetri bağıntısı

Zaman uyumsuz bir Spring Boot uygulaması içinde telemetri bağıntısı kurmak için lütfen izleyin [bu](https://github.com/Microsoft/ApplicationInsights-Java/wiki/Distributed-Tracing-in-Asynchronous-Java-Applications) ayrıntılı makaleyi. Spring'ın izleme bir Rehber sağlanır [ThreadPoolTaskExecutor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/concurrent/ThreadPoolTaskExecutor.html) yanı [ThreadPoolTaskScheduler](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/scheduling/concurrent/ThreadPoolTaskScheduler.html). 


<a name="java-role-name"></a>
## <a name="role-name"></a>Rol adı

Bazen, bileşen adları görüntülenir şekilde özelleştirmek isteyebilirsiniz [Uygulama Haritası](../../azure-monitor/app/app-map.md). Bunu yapmak için el ile ayarlayabileceğiniz `cloud_RoleName` aşağıdakilerden birini yaparak:

- Spring Boot ile Application Insights Spring Boot Başlatıcı kullanırsanız, yalnızca gerekli değişiklik application.properties dosyasında uygulama için kendi özel adınızı ayarlamaktır.

  `spring.application.name=<name-of-app>`

  Spring Boot Başlatıcı otomatik olarak atar `cloudRoleName` için girdiğiniz değer `spring.application.name` özelliği.

- Kullanıyorsanız `WebRequestTrackingFilter`, `WebAppNameContextInitializer` uygulama adı otomatik olarak ayarlar. Aşağıdaki yapılandırma dosyanız (Applicationınsights.xml) ekleyin:

  ```XML
  <ContextInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebAppNameContextInitializer" />
  </ContextInitializers>
  ```

- Bulut bağlam sınıfını kullanıyorsanız:

  ```Java
  telemetryClient.getContext().getCloud().setRole("My Component Name");
  ```

## <a name="next-steps"></a>Sonraki adımlar

- Yazma [özel telemetri](../../azure-monitor/app/api-custom-events-metrics.md).
- Daha fazla bilgi edinin [cloud_RoleName ayarlama](../../azure-monitor/app/app-map.md#set-cloud-role-name) diğer SDK'ları için.
- Ekleme, mikro hizmet Application ınsights'ın tüm bileşenleri. Kullanıma [desteklenen platformlar](../../azure-monitor/app/platforms.md).
- Bkz: [veri modeli](../../azure-monitor/app/data-model.md) Application Insights türleri için.
- Bilgi edinmek için nasıl [genişletmek ve telemetri filtreleme](../../azure-monitor/app/api-filtering-sampling.md).
- Gözden geçirme [Application Insights yapılandırma başvurusu](configuration-with-applicationinsights-config.md).
