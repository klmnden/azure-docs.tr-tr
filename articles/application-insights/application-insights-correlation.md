---
title: Azure Application Insights Telemetri bağıntısı | Microsoft Docs
description: Application Insights telemetri bağıntısı
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/09/2018
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: eb14a3bc76fef37cdff4ed49cdbb6a99eac40928
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51280172"
---
# <a name="telemetry-correlation-in-application-insights"></a>Application ınsights telemetri bağıntısı

Mikro hizmetler dünyasında, hizmetin çeşitli bileşenlerinin çalışmanın her mantıksal işlemi gerektirir. Bu bileşenlerin her birini ayrı olarak tarafından izlenebilir [Application Insights](app-insights-overview.md). Web uygulaması bileşeni, kullanıcı kimlik bilgilerini doğrulamak için kimlik doğrulama sağlayıcısı bileşeniyle ve görselleştirme için verileri almak için API bileşeni ile iletişim kurar. API bileşen kendi sırasını, diğer hizmetlerden veri sorgulamak ve önbelleği sağlayıcısı bileşenlerini kullanın ve fatura bileşeni bu çağrı hakkında bilgilendir. Application ınsights'ı destekleyen dağıtılmış telemetri bağıntısı. Hangi hatalarının veya performans düşüşü sorumlu bir bileşendir algılamak olanak tanır.

Bu makalede, birden çok bileşen tarafından gönderilen telemetrinin bağıntısını kurmanızı Application Insights tarafından kullanılan veri modeli açıklanmaktadır. Bağlam yayma teknikleri ve protokolleri ele alınmaktadır. Ayrıca, farklı diller ve platformlarda bağıntı kavramları uygulanması da kapsar.

## <a name="telemetry-correlation-data-model"></a>Telemetri bağıntısı veri modeli

Application Insights'ı tanımlayan bir [veri modeli](application-insights-data-model.md) dağıtılmış telemetri bağıntısı için. Telemetri mantıksal işlemi ile ilişkilendirilecek adlı bir bağlam alan her telemetri öğesine sahip `operation_Id`. Bu tanımlayıcı dağıtılmış izlemede her telemetri öğesinin tarafından paylaşılır. Böylece tek bir katman alınan telemetri kaybı ile hala diğer bileşenleri tarafından raporlanan telemetri ilişkilendirebilirsiniz.

Dağıtılmış bir mantıksal işlemi genellikle bir dizi daha küçük işlemlerini - bileşenlerinden biri tarafından işlenen isteklerin oluşur. Bu işlemler tarafından tanımlanan [istek telemetri](application-insights-data-model-request-telemetry.md). Her istek telemetrisi kendi bölümüne sahiptir `id` , küresel olarak benzersiz şekilde tanımlar. Ve tüm telemetri - izlemelerini, özel durumlar, bu istekle ilişkili vb. ayarlamalısınız `operation_parentId` istek değerine `id`.

Başka bir bileşen tarafından temsil edilen http çağrısı gibi her giden işlem [bağımlılık telemetrisi](application-insights-data-model-dependency-telemetry.md). Bağımlılık telemetrisi ayrıca tanımlar kendi `id` genel olarak benzersiz. Bu bağımlılık çağrısının tarafından başlatılan istek telemetrisi, olarak kullandığı `operation_parentId`.

Dağıtılmış bir mantıksal işlemi kullanarak görünümü oluşturabilirsiniz `operation_Id`, `operation_parentId`, ve `request.id` ile `dependency.id`. Bu alanlar da nedensellik ilişkilerini çağrı sırasını, telemetri tanımlayın.

Mikro hizmetler ortamında izlemelerinden bileşenleri için farklı Pano'yu gidebilir. Her bileşen kendi izleme anahtarı, Application Insights'ta sahip olabilir. Mantıksal işlem için telemetri almak için her depolama alanından verileri sorgulamak gerekir. Pano'yu sayısı çok büyük olduğunda, sonraki ara nerede ipucu olması gerekir.

Application Insights veri modeli, bu sorunu çözmek için iki alan tanımlar: `request.source` ve `dependency.target`. İlk alanı, bağımlılık istek başlatılan bileşeni belirtir ve saniye bileşeni bağımlılık çağrısının bir yanıt döndürdü tanımlar.


## <a name="example"></a>Örnek

Geçerli pazar fiyat STOCKS API olarak adlandırılan harici API kullanarak bir hisse senedi gösteren bir uygulama hisse SENEDİ FİYATLAR örneği ele alalım. Hisse SENEDİ FİYATLARINI uygulamaya sahip bir sayfa `Stock page` istemci web tarayıcısı kullanılarak açılan `GET /Home/Stock`. HTTP çağrısı kullanarak stok API uygulama sorgular `GET /api/stock/value`.

Çalışan bir sorgu elde edilen telemetri çözümleyebilirsiniz:

```
(requests | union dependencies | union pageViews) 
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

Tüm telemetri öğelerinin kök paylaşmak sonucu görünümü notta `operation_Id`. Ajax çağırdığınızda sayfası - yeni benzersiz kimliği yapılan `qJSXU` bağımlılık telemetrisi için atanan ve sayfa görüntülemesi'nın kimliği olarak kullanılan `operation_ParentId`. Sunucu isteği ajax'ın kimliği olarak sırayla kullanan `operation_ParentId`, vb.

| Itemtype   | ad                      | id           | operation_ParentId | operation_ıd |
|------------|---------------------------|--------------|--------------------|--------------|
| Sayfa görünümü   | Stok sayfası                |              | STYz               | STYz         |
| bağımlılık | GET /Home/stok           | qJSXU        | STYz               | STYz         |
| istek    | GET Home/stok            | KqKwlrSt9PA = | qJSXU              | STYz         |
| bağımlılık | /Api/Stock/Value Al      | bBrf2L7mm2g= | KqKwlrSt9PA =       | STYz         |

Şimdi, çağrı `GET /api/stock/value` yapılan sunucu kimliğini bilmek isteyeceğiniz bir dış hizmet. Böylece, ayarlayabilirsiniz `dependency.target` uygun şekilde alanı. Dış hizmet izleme - desteklemediğinde `target` gibi hizmet konak adını ayarlayın `stock-prices-api.com`. Ancak, hizmeti önceden tanımlanmış bir döndürerek kendisini tanımlar, HTTP üst bilgisi - `target` Application Insights'ın dağıtılmış izleme telemetrisi, hizmetten sorgulayarak oluşturmanızı sağlayan bir hizmet kimliği içeriyor. 

## <a name="correlation-headers"></a>Bağıntı üstbilgileri

RFC teklifi için üzerinde çalıştığımız [bağıntı HTTP Protokolü](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Bu teklif, iki üstbilgi tanımlar:

- `Request-Id` çağrı genel olarak benzersiz kimliğini Yürüt
- `Correlation-Context` -Dağıtılmış izleme özellikleri ad değer çiftleri koleksiyonu Yürüt

Standart Ayrıca iki şemaları tanımlar `Request-Id` oluşturma - düz ve hiyerarşik. Düz şemasıyla yoktur iyi bilinen `Id` için tanımlanan anahtar `Correlation-Context` koleksiyonu.

Application Insights tanımlar [uzantısı](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) bağıntı HTTP protokolü için. Kullandığı `Request-Context` ad değer çiftleri koleksiyonu şu anki çağırıcı veya Aranan tarafından kullanılan özellikleri yaymak için. Application Insights SDK'sını ayarlamak için bu üstbilgiyi kullanır `dependency.target` ve `request.source` alanları.

### <a name="w3c-distributed-tracing"></a>W3C dağıtılmış izleme

Biz için geçiş [W3C dağıtılmış izleme biçimi](https://w3c.github.io/trace-context/). Tanımlar:
- `traceparent` -Genel olarak benzersiz işlem kimliği ve aramanın benzersiz tanımlayıcısı
- `tracestate` -İzleme sistem belirli bağlamı yürütür.

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-classic-apps"></a>W3C dağıtılmış izleme ASP.NET Klasik uygulamaları desteğini etkinleştirme

Bu özellik, sürüm 2.8.0-beta1 ile başlayan Microsoft.applicationınsights.Web ve Microsoft.ApplicationInsights.DependencyCollector paketlerdeki kullanılabilir.
Bu **kapalı** etkinleştirmek için varsayılan olarak, değişiklik `ApplicationInsights.config`:

* altında `RequestTrackingTelemetryModule` ekleme `EnableW3CHeadersExtraction` ayarlanan değere sahip öğe `true`
* altında `DependencyTrackingTelemetryModule` ekleme `EnableW3CHeadersInjection` ayarlanan değere sahip öğe `true`

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>ASP.NET Core uygulamaları için Dağıtılmış W3C İzleme desteğini etkinleştirme

Bu özellik, sürüm 2.5.0-beta1 Microsoft.ApplicationInsights.DependencyCollector sürüm 2.8.0-beta1 ile Microsoft.ApplicationInsights.AspNetCore içinde kullanılabilir.
Bu **kapalı** etkinleştirmek için varsayılan olarak ayarlanmış `ApplicationInsightsServiceOptions.RequestCollectionOptions.EnableW3CDistributedTracing` için `true`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry(o => 
        o.RequestCollectionOptions.EnableW3CDistributedTracing = true );
    // ....
}
```

## <a name="open-tracing-and-application-insights"></a>Açık izleme ve Application Insights

[Açık izleme veri modeli belirtimi](http://opentracing.io/) ve Application Insights veri modelleri, aşağıdaki şekilde eşlenir:

| Application Insights                  | İzlemeyi Aç                                      |
|------------------------------------   |-------------------------------------------------  |
| `Request`, `PageView`                 | `Span` ile `span.kind = server`                  |
| `Dependency`                          | `Span` ile `span.kind = client`                  |
| `Id` ' ın `Request` ve `Dependency`    | `SpanId`                                          |
| `Operation_Id`                        | `TraceId`                                         |
| `Operation_ParentId`                  | `Reference` tür `ChildOf` (üst aralık)   |

Application Insights veri modeli hakkında daha fazla bilgi için bkz. [veri modeli](application-insights-data-model.md). 

Bkz: açık izleme [belirtimi](https://github.com/opentracing/specification/blob/master/specification.md) ve [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) açık izleme kavramları tanımları için.


## <a name="telemetry-correlation-in-net"></a>. NET'te telemetri bağıntısı

Zaman içinde telemetri ve tanılama günlüklerini ilişkilendirmek için çeşitli yöntemler .NET tanımlanır. Var. `System.Diagnostics.CorrelationManager` izlemek için izin verme [LogicalOperationStack ve ActivityID](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). `System.Diagnostics.Tracing.EventSource` ve Windows ETW yöntemi tanımlayabilirsiniz [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx). `ILogger` kullanan [günlük kapsamları](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). WCF ve Http kablo "geçerli" bağlam yayma ayarlama.

Ancak bu yöntemleri otomatik dağıtılmış izleme desteği etkinleştirmeniz kaydetmedi. `DiagnosticsSource` Makine bağıntı desteklemek için bir yol otomatiktir. .NET kitaplıkları tanılama kaynak desteği ve taşıma gibi http üzerinden bağıntı bağlam makine yayılmasını çapraz otomatik izin.

[Etkinlikleri kılavuza](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) tanılama kaynakta izleme etkinliklerin temellerini açıklar. 

ASP.NET Core 2.0 Http üst bilgileri ve yeni etkinlik başlangıç ayıklama destekler. 

`System.Net.HttpClient` Başlangıç sürümü `4.1.0` bağıntı Http üst bilgilerini otomatik ekleme destekler ve http çağrısı bir etkinlik olarak izleme.

Yeni bir Http Modülü [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) ASP.NET Klasik için. Bu modül, telemetri bağıntısı DiagnosticsSource kullanarak uygular. Gelen istek üst bilgilere göre etkinlik başlar. İsteğin işlenmesinin farklı aşamalar alınan telemetri hatalarla ilintilidir. Her IIS aşaması farklı Yönet iş parçacıklarında çalıştırdığında bile durumlarda.

Application Insights SDK'sı başlangıç sürümü `2.4.0-beta1` DiagnosticsSource ve etkinlik telemetri toplamak ve geçerli etkinliği ile ilişkilendirmek için kullanır. 

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-the-java-sdk"></a>Java SDK telemetri bağıntısı
[Application Insights Java SDK'sı](app-insights-java-get-started.md) otomatik sürümü itibarıyla telemetri bağıntısı destekler `2.0.0`. Otomatik olarak doldurulur `operation_id` istek kapsamında verilen tüm telemetri (izlemelerini, özel durumlar, özel olaylar, vb.). Bu da (HTTP aracılığıyla hizmet çağrıları için yukarıda) bağıntı üst bilgileri yayma varsa üstlenir [Java SDK'sı aracı](app-insights-java-agent.md) yapılandırılır. Not: Yalnızca Apache HTTP istemcisi yapılan çağrılar bağıntı özelliği için desteklenir. Spring Rest şablonu veya Feign kullanıyorsanız, her ikisi de bileşenler Apache HTTP istemci ile kullanılabilir.

Mesajlaşma teknolojilerinin (örneğin, Kafka, RabbitMQ, Azure Service Bus) arasında bağlamı otomatik yayma şu anda desteklenmiyor. Ancak, mümkündür kullanarak bu senaryolara el ile kod `trackDependency` ve `trackRequest` verebileceğiniz bir bağımlılık telemetrisi olan bir üretici tarafından sıraya bir ileti ve bir ileti tüketicisi tarafından işlenmekte olan isteği temsil eder API. Bu durumda, her ikisi de `operation_id` ve `operation_parentId` ileti özelliklerinde yayılır.

<a name="java-role-name"></a>
### <a name="role-name"></a>Rol Adı
Bazen, bileşen adları görüntülenir şekilde özelleştirmek isteyebilirsiniz [Uygulama Haritası](app-insights-app-map.md). Bunu yapmak için el ile ayarlayabileceğiniz `cloud_roleName` aşağıdakilerden birini yaparak:

(Tüm telemetri öğelerinin etiketlenir) bir telemetri Başlatıcısı
```Java
public class CloudRoleNameInitializer extends WebTelemetryInitializerBase {

    @Override
    protected void onInitializeTelemetry(Telemetry telemetry) {
        telemetry.getContext().getTags().put(ContextTagKeys.getKeys().getDeviceRoleName(), "My Component Name");
    }
  }
```
Aracılığıyla [cihaz bağlamı sınıfının](https://docs.microsoft.com/java/api/com.microsoft.applicationinsights.extensibility.context._device_context) (yalnızca bu telemetri öğesinin etiketli)
```Java
telemetry.getContext().getDevice().setRoleName("My Component Name");
```

## <a name="next-steps"></a>Sonraki adımlar

- [Özel telemetri yazma](app-insights-api-custom-events-metrics.md)
- Yerleşik mikro hizmet Application Insights'ın tüm bileşenleri. Kullanıma [desteklenen platformlar](app-insights-platforms.md).
- Bkz: [veri modeli](application-insights-data-model.md) için Application Insights türleri ve veri modeli.
- Bilgi edinmek için nasıl [genişletmek ve telemetri filtreleme](app-insights-api-filtering-sampling.md).
- [Application Insights yapılandırma başvurusu](app-insights-configuration-with-applicationinsights-config.md)

