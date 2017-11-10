---
title: "Azure uygulama Insights Telemetri bağıntı | Microsoft Docs"
description: "Uygulama Insights telemetri bağıntı"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: mbullwin
ms.openlocfilehash: e821a640d3d75e712c022bd681eb07b83da91911
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="telemetry-correlation-in-application-insights"></a>Application ınsights'ta telemetri bağıntı

Mikro hizmetler dünyanın her mantıksal işlem hizmet çeşitli bileşenlerinin işlerini gerektirir. Bu bileşenlerin her birini tarafından ayrı olarak izlenebilir [Application Insights](app-insights-overview.md). Web uygulaması bileşeni, kullanıcı kimlik bilgilerini doğrulamak için kimlik doğrulama sağlayıcısı bileşeniyle ve görselleştirme için verileri almak için API bileşeni ile iletişim kurar. API bileşeni, dönüş diğer hizmetlerden verileri sorgulamak ve önbellek sağlayıcısı bileşenlerini kullanabilir ve fatura bileşeni bu çağrıyı hakkında bildirim. Uygulama Öngörüler destekler dağıtılmış telemetri bağıntı. Hangi bileşenin hatalar veya performans düşüşü sorumlu algılamasını sağlar.

Bu makalede, birden çok bileşen tarafından gönderilen telemetriyi ilişkilendirmek için Application Insights tarafından kullanılan veri modelini açıklanmaktadır. Bağlam yayma teknikleri ve protokolleri kapsar. Uygulama farklı diller ve platformlarda bağıntı kavramlarını ele alınmaktadır.

## <a name="telemetry-correlation-data-model"></a>Telemetri bağıntı veri modeli

Application Insights tanımlayan bir [veri modeli](application-insights-data-model.md) dağıtılmış telemetri bağıntı için. Telemetri mantıksal işlemiyle ilişkilendirmek için her telemetri öğenin adlı bir bağlam alan sahip `operation_Id`. Bu tanımlayıcı, dağıtılmış izlemede her telemetri öğesi tarafından paylaşılır. Bu nedenle bile telemetri tek bir katmandan kaybı ile hala diğer bileşenler tarafından bildirilen telemetri ilişkilendirebilirsiniz.

Dağıtılmış mantıksal işlemi genellikle daha küçük işlemleri - bileşenlerden biri tarafından işlenen isteklerin kümesi oluşur. Bu işlemler tarafından tanımlanan [isteği telemetri](application-insights-data-model-request-telemetry.md). Her istek telemetri kendi sahip `id` , genel olarak benzersiz şekilde tanımlar. Ve tüm telemetri - izlemeleri, özel durumlar, bu istekle ilişkili vb. ayarlamalısınız `operation_parentId` isteği değerine `id`.

Her giden işlem başka bir bileşen tarafından temsil edilen http çağrısı gibi [bağımlılık telemetrisi](application-insights-data-model-dependency-telemetry.md). Bağımlılık telemetrisi ayrıca tanımlar kendi `id` genel olarak benzersiz. Bu bağımlılık çağrısı tarafından başlatılan isteği telemetri olarak kullanan `operation_parentId`.

Dağıtılmış mantıksal işlemi kullanarak görünümünü oluşturabilirsiniz `operation_Id`, `operation_parentId`, ve `request.id` ile `dependency.id`. Bu alanlar aynı zamanda telemetri çağrıları causality sırasını tanımlar.

Mikro Hizmetleri ortamında bileşenleri izlemeler farklı depolarını gidebilir. Her bileşen, kendi izleme anahtarını Application Insights'ta olabilir. Mantıksal işlem için telemetri almak için her depolama alanından verileri sorgulamak gerekir. Depoları sayısı çok büyük olduğunda, sonraki ara nerede ipucu olması gerekir.

Veri modeli tanımlayan bu sorunu çözmek için iki alan application Insights: `request.source` ve `dependency.target`. İlk alan bağımlılık istek başlatılan bileşen tanımlar ve hangi bileşenin bağımlılık çağrının bir yanıt döndürdü ikinci tanımlar.


## <a name="example"></a>Örnek

Geçerli Piyasa fiyatı hisse API adlı harici API kullanarak hisse senedi gösteren bir uygulama hisse SENEDİ FİYATLARI örneği atalım. Bir sayfa hisse SENEDİ FİYATLARI uygulamasına sahip `Stock page` istemci web tarayıcısı kullanarak açılan `GET /Home/Stock`. Bir HTTP çağrısı kullanarak stok API uygulaması sorgular `GET /api/stock/value`.

Bir sorgu çalıştırılarak elde edilen telemetri çözümleyebilirsiniz:

```
(requests | union dependencies | union pageViews) 
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

Tüm telemetri öğelerini kök paylaşma sonuç görünümü notta `operation_Id`. Ajax çağırdığınızda sayfasından - yeni benzersiz kimlik yapılan `qJSXU` bağımlılık telemetrisi atanır ve sayfa görünümü'nın kimliği olarak kullanılan `operation_ParentId`. Sunucu isteği ajax'ın kimliği olarak sırayla kullanan `operation_ParentId`, vb.

| itemType   | ad                      | id           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| Sayfa görünümü   | Stok sayfası                |              | STYz               | STYz         |
| bağımlılık | GET /Home/stok           | qJSXU        | STYz               | STYz         |
| İstek    | GET Home/stok            | KqKwlrSt9PA = | qJSXU              | STYz         |
| bağımlılık | /Api/Stock/Value Al      | bBrf2L7mm2g = | KqKwlrSt9PA =       | STYz         |

Şimdi, çağrı `GET /api/stock/value` sunucu kimliğinin bilinmesi istediğiniz bir dış hizmet yapılan. Böylece, ayarlayabilirsiniz `dependency.target` uygun şekilde alan. Dış hizmet izleme - desteklemediğinde `target` gibi hizmet ana bilgisayar adı olarak ayarlanmış `stock-prices-api.com`. Ancak bu hizmetin kendisi önceden tanımlanmış bir döndürerek tanımlarsa HTTP üstbilgisi - `target` telemetri bu hizmetinden sorgulayarak dağıtılmış izleme oluşturmak Application Insights sağlayan hizmet kimliği içerir. 

## <a name="correlation-headers"></a>Bağıntı üstbilgileri

RFC teklifi için üzerinde çalıştığımız [bağıntı HTTP Protokolü](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v1.md). Bu teklif iki üstbilgi tanımlar:

- `Request-Id`çağrı genel benzersiz kimliğini Yürüt
- `Correlation-Context`-Dağıtılmış izleme özellikleri ad değer çifti koleksiyonu Yürüt

Standart iki şemaları, ayrıca tanımlar `Request-Id` oluşturma - düz ve hiyerarşik. Düz şemasıyla yoktur iyi bilinen `Id` için tanımlanmış anahtar `Correlation-Context` koleksiyonu.

Application Insights tanımlar [uzantısı](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) bağıntı HTTP protokolü için. Kullandığı `Request-Context` anında arayanlar veya Aranan tarafından kullanılan özelliklerinin koleksiyonunu yaymak için değer çiftleri olarak adlandırın. Application Insights SDK'sı ayarlamak için bu üstbilgiyi kullanır `dependency.target` ve `request.source` alanları.

## <a name="open-tracing-and-application-insights"></a>Açık izleme ve Application Insights

[İzleme açık](http://opentracing.io/) ve Application Insights veri modelleri görünüyor 

- `request`, `pageView` eşlendiği **aralık** ile`span.kind = server`
- `dependency`eşlendiği **aralık** ile`span.kind = client`
- `id`bir `request` ve `dependency` eşlendiği **Span.Id**
- `operation_Id`eşlendiği **TraceId**
- `operation_ParentId`eşlendiği **başvuru** türü`ChildOf`

Bkz: [veri modeli](application-insights-data-model.md) Application Insights türleri ve veri modeli için.

Bkz: [belirtimi](https://github.com/opentracing/specification/blob/master/specification.md) ve [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) açık izleme kavramları tanımları için.


## <a name="telemetry-correlation-in-net"></a>.NET içinde telemetri bağıntı

Zaman içinde .NET telemetri ve tanılama günlüklerini ilişkilendirmek için çeşitli yöntemler tanımlanır. Yoktur `System.Diagnostics.CorrelationManager` izlemek için izin verme [LogicalOperationStack ve ActivityID](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). `System.Diagnostics.Tracing.EventSource`ve Windows ETW yöntemi tanımlayın [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx). `ILogger`kullanan [günlük kapsamları](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). WCF ve Http kablo "geçerli" bağlam yayma ayarlama.

Ancak bu yöntemleri otomatik dağıtılmış İzleme desteğini etkinleştirin alamadık. `DiagnosticsSource`desteklemek için bir yol makine bağıntı otomatik olarak yapılır. .NET kitaplıklarına tanılama kaynak desteklemek ve http gibi aktarım aracılığıyla bağıntı bağlam makine yayılmasını çapraz otomatik sağlar.

[Etkinlikleri kılavuza](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) tanılama kaynağında etkinlikleri izleme temellerini açıklar. 

ASP.NET Core 2.0 Http üstbilgileri ve yeni etkinlik başlangıç ayıklama destekler. 

`System.Net.HttpClient`Başlangıç sürümünden `<fill in>` http çağrısı bir etkinlik olarak izleme ve Http üstbilgileri bağıntı otomatik eklenmesine destekler.

Yeni bir Http modülü yok [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) ASP.NET Classic için. Bu modül telemetri bağıntı DiagnosticsSource kullanarak uygular. Gelen istek üstbilgileri göre etkinlik başlatır. İstek işleme farklı aşamalarını telemetrisinden hatalarla ilintilidir. Her IIS aşaması farklı Yönet iş parçacığı üzerinde çalıştığında bile durumlar için.

Uygulama Insights SDK'sı başlangıç sürümü `2.4.0-beta1` DiagnosticsSource ve etkinlik telemetri toplamak ve geçerli etkinliğin ile ilişkilendirmek için kullanır. 

## <a name="next-steps"></a>Sonraki adımlar

- [Özel telemetri yazın](app-insights-api-custom-events-metrics.md)
- Yerleşik mikro hizmetinizde Application Insights'ın tüm bileşenleri. Kullanıma [desteklenen platformlar](app-insights-platforms.md).
- Bkz: [veri modeli](application-insights-data-model.md) Application Insights türleri ve veri modeli için.
- Bilgi edinmek için nasıl [genişletmek ve filtre telemetri](app-insights-api-filtering-sampling.md).
