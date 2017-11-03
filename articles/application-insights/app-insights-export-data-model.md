---
title: "Azure uygulama Öngörüler veri modeli | Microsoft Docs"
description: "JSON içinde sürekli dışarı aktarılmış ve filtre olarak kullanılan özellikleri tanımlar."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: cabad41c-0518-4669-887f-3087aef865ea
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/21/2016
ms.author: mbullwin
ms.openlocfilehash: 86aef6ba93224bbbb41bc7e651aaeec394fd8718
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="application-insights-export-data-model"></a>Uygulama Öngörüler dışarı aktarma veri modeli
Bu tabloda telemetri gönderildiği özelliklerini [Application Insights](app-insights-overview.md) SDK'ları portalı.
Bu özellikler, veri çıkışı görürsünüz [sürekli verme](app-insights-export-telemetry.md).
Ayrıca özellik filtrelerini görüntülendikleri [ölçüm Gezgini](app-insights-metrics-explorer.md) ve [tanılama arama](app-insights-diagnostic-search.md).

Dikkat edilecek noktalar:

* `[0]`Bu tablolarda dizin eklemek için sahip olduğu yolu noktasında gösterir; ancak her zaman değil 0.
* Süreler milisaniyeye, bu nedenle 10000000 onda içinde olan 1 saniye ==.
* Tarihler ve saatler UTC olan ve ISO biçiminde verilir.`yyyy-MM-DDThh:mm:ss.sssZ`


## <a name="example"></a>Örnek
    // A server report about an HTTP request
    {
    "request": [
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": ""
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0",
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0,
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }

## <a name="context"></a>Bağlam
Tüm telemetri türlerini içerik bölümü tarafından yayımlanır. Bu alanların tümünü her veri noktası ile aktarılır.

| Yol | Tür | Notlar |
| --- | --- | --- |
| Context.Custom.Dimensions [0] |Nesne] |Anahtar-değer çiftleri özel özellikler parametresiyle ayarlayın. Anahtar uzunluğu en fazla 100, en fazla uzunluğu 1024 değerleri. 100'den fazla benzersiz değer özelliği aranabilecek ancak kesimleme kullanılamaz. İkey başına en fazla 200 anahtarları. |
| Context.Custom.Metrics [0] |Nesne] |Anahtar-değer çiftleri TrackMetrics ve özel ölçümleri parametresi tarafından ayarlayın. Anahtar en büyük uzunluğunu 100 değerleri sayısal olabilir. |
| context.data.eventTime |Dize |UTC |
| context.data.isSynthetic |Boole değeri |İstek bir bot veya web testi görünür. |
| context.data.samplingRate |Sayı |Portala gönderilen SDK'sı tarafından oluşturulan telemetri yüzdesi. 0,0 100.0 aralığı. |
| Context.Device |Nesne |İstemci aygıtı |
| Context.Device.browser |Dize |IE, Chrome... |
| context.device.browserVersion |Dize |Chrome 48,0... |
| context.device.deviceModel |Dize | |
| context.device.deviceName |Dize | |
| Context.Device.id |Dize | |
| Context.Device.Locale |Dize |tr GB, de-DE... |
| Context.Device.Network |Dize | |
| context.device.oemName |Dize | |
| context.device.osVersion |Dize |Ana bilgisayar işletim sistemi |
| context.device.roleInstance |Dize |Sunucu ana bilgisayar kimliği |
| context.device.roleName |Dize | |
| Context.Device.Type |Dize |Bilgisayar Tarayıcı... |
| Context.location |Nesne |Clientip türetilmiş. |
| Context.location.City |Dize |Biliniyorsa clientip türetilmiş |
| Context.location.clientip |Dize |Son Sekizgene 0 olarak gizlidir. |
| Context.location.continent |Dize | |
| Context.location.country |Dize | |
| Context.location.Province |Dize |Eyalet veya il |
| Context.Operation.id |Dize |Aynı işlem kimliğine sahip öğeler portalda ilgili öğeler gösterilir. Genellikle istek kimliği. |
| Context.Operation.Name |Dize |URL veya isteği adı |
| context.operation.parentId |Dize |İç içe geçmiş ilgili öğeler sağlar. |
| Context.Session.id |Dize |Aynı kaynak işlemlerinin grubunun kimliği. Bir işlem olmadan 30 dakikalık bir süre bir oturum sonuna işaret eder. |
| context.session.isFirst |Boole değeri | |
| context.user.accountAcquisitionDate |Dize | |
| context.user.anonAcquisitionDate |Dize | |
| context.user.anonId |Dize | |
| context.user.authAcquisitionDate |Dize |[Kimliği doğrulanmış kullanıcı](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated |Boole değeri | |
| internal.data.documentVersion |Dize | |
| internal.Data.id |Dize | |

## <a name="events"></a>Olaylar
Tarafından oluşturulan özel olaylar [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent).

| Yol | Tür | Notlar |
| --- | --- | --- |
| [0] olay sayısı |tamsayı |100 / ([örnekleme](app-insights-sampling.md) hızı). Örneğin 4 =&gt; % 25. |
| Olay [0] adı |Dize |Olay adı.  En fazla uzunluk 250. |
| Olay [0] URL'si |Dize | |
| Olay [0] urlData.base |Dize | |
| Olay [0] urlData.host |Dize | |

## <a name="exceptions"></a>Özel durumlar
Raporları [özel durumları](app-insights-asp-net-exceptions.md) sunucu ve tarayıcı.

| Yol | Tür | Notlar |
| --- | --- | --- |
| basicException [0] derlemesi |Dize | |
| basicException [0] sayısı |tamsayı |100 / ([örnekleme](app-insights-sampling.md) hızı). Örneğin 4 =&gt; % 25. |
| basicException [0] exceptionGroup |Dize | |
| basicException [0] exceptionType |Dize | |
| basicException [0] failedUserCodeMethod |Dize | |
| basicException [0] failedUserCodeAssembly |Dize | |
| basicException [0] handledAt |Dize | |
| basicException [0] hasFullStack |Boole değeri | |
| basicException [0] kimliği |Dize | |
| basicException [0] yöntemi |Dize | |
| basicException [0] iletisi |Dize |Özel durum iletisi. En fazla uzunluk 10k. |
| basicException [0] outerExceptionMessage |Dize | |
| basicException [0] outerExceptionThrownAtAssembly |Dize | |
| basicException [0] outerExceptionThrownAtMethod |Dize | |
| basicException [0] outerExceptionType |Dize | |
| basicException [0] outerId |Dize | |
| basicException [0] parsedStack [0] derlemesi |Dize | |
| basicException [0] [0] parsedStack fileName |Dize | |
| basicException [0] [0] parsedStack düzeyi |tamsayı | |
| basicException [0] [0] parsedStack satır |tamsayı | |
| basicException [0] [0] parsedStack yöntemi |Dize | |
| basicException [0] yığını |Dize |En fazla uzunluk 10k |
| basicException [0] typeName |Dize | |

## <a name="trace-messages"></a>İzleme iletileri
Tarafından gönderilen [TrackTrace](app-insights-api-custom-events-metrics.md#tracktrace)ve bunun [günlüğü bağdaştırıcıları](app-insights-asp-net-trace-logs.md).

| Yol | Tür | Notlar |
| --- | --- | --- |
| ileti [0] GünlükçüAdı |Dize | |
| ileti [0] parametreleri |Dize | |
| ileti [0] ham |Dize |Günlük iletisi, uzunluk üst sınırı 10k. |
| [0] iletisi önem düzeyi |Dize | |

## <a name="remote-dependency"></a>Uzak bağımlılık
TrackDependency tarafından gönderilir. Rapor performansı ve kullanımı için kullanılan [bağımlılıkları çağrıları](app-insights-asp-net-dependencies.md) server ve AJAX çağrıları tarayıcıda.

| Yol | Tür | Notlar |
| --- | --- | --- |
| remoteDependency [0] zaman uyumsuz |Boole değeri | |
| remoteDependency [0] baseName |Dize | |
| remoteDependency [0] commandName |Dize |Örneğin "home/Index" |
| remoteDependency [0] sayısı |tamsayı |100 / ([örnekleme](app-insights-sampling.md) hızı). Örneğin 4 =&gt; % 25. |
| remoteDependency [0] dependencyTypeName |Dize |HTTP SQL... |
| remoteDependency [0] durationMetric.value |Sayı |Bağımlılık yanıtıyla tamamlanmasından çağrı süresi |
| remoteDependency [0] kimliği |Dize | |
| remoteDependency [0] adı |Dize |URL. En fazla uzunluk 250. |
| remoteDependency [0] resultCode |Dize |HTTP bağımlılık |
| remoteDependency [0] başarılı |Boole değeri | |
| remoteDependency [0] türü |Dize |HTTP Sql... |
| remoteDependency [0] URL'si |Dize |En fazla uzunluk 2000 |
| remoteDependency [0] urlData.base |Dize |En fazla uzunluk 2000 |
| remoteDependency [0] urlData.hashTag |Dize | |
| remoteDependency [0] urlData.host |Dize |En fazla uzunluk 200 |

## <a name="requests"></a>İstekler
Tarafından gönderilen [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest). Standart modüller, sunucuda ölçülen bu raporları sunucu yanıt süresi için kullanın.

| Yol | Tür | Notlar |
| --- | --- | --- |
| [0] isteği sayısı |tamsayı |100 / ([örnekleme](app-insights-sampling.md) hızı). Örneğin: 4 =&gt; % 25. |
| İstek [0] durationMetric.value |Sayı |Karşılık gelen istek süresi. 1e7 1'ler == |
| [0] istek kimliği |Dize |İşlem kimliği |
| İstek [0] adı |Dize |GET/POST + temel url.  En fazla uzunluk 250 |
| İstek [0] yanıt kodu |tamsayı |İstemciye gönderilen HTTP yanıtı |
| İstek [0] başarılı |Boole değeri |Varsayılan == (yanıt kodu &lt; 400) |
| İstek [0] URL'si |Dize |Konak dahil değil |
| İstek [0] urlData.base |Dize | |
| İstek [0] urlData.hashTag |Dize | |
| İstek [0] urlData.host |Dize | |

## <a name="page-view-performance"></a>Sayfa görünümü performansı
Tarayıcı tarafından gönderilen. (Zaman uyumsuz AJAX çağrıları) tam görüntülenecek isteğini başlatarak kullanıcıdan bir sayfanın işlenmesi için gereken süre ölçer.

Bağlam değerleri, istemci işletim sistemi ve tarayıcı sürümü gösterir.

| Yol | Tür | Notlar |
| --- | --- | --- |
| clientPerformance [0] clientProcess.value |tamsayı |Sayfanın görüntüleme için HTML Alma bitiş saati. |
| clientPerformance [0] adı |Dize | |
| clientPerformance [0] networkConnection.value |tamsayı |Bir ağ bağlantısı kurmak için geçen süre. |
| clientPerformance [0] receiveRequest.value |tamsayı |HTML yanıta almak için isteği gönderme bitiş saati. |
| clientPerformance [0] sendRequest.value |tamsayı |Saati HTTP isteği göndermek için gerçekleştirilecek. |
| clientPerformance [0] total.value |tamsayı |Sayfayı görüntülemeye isteği göndermek başlangıç süresi. |
| clientPerformance [0] URL'si |Dize |Bu istek URL'si |
| clientPerformance [0] urlData.base |Dize | |
| clientPerformance [0] urlData.hashTag |Dize | |
| clientPerformance [0] urlData.host |Dize | |
| clientPerformance [0] urlData.protocol |Dize | |

## <a name="page-views"></a>Sayfa görünümleri
TrackPageView() tarafından gönderilen veya [stopTrackPage](app-insights-api-custom-events-metrics.md#page-views)

| Yol | Tür | Notlar |
| --- | --- | --- |
| Görünüm [0] sayısı |tamsayı |100 / ([örnekleme](app-insights-sampling.md) hızı). Örneğin 4 =&gt; % 25. |
| Görünüm [0] durationMetric.value |tamsayı |Değeri trackPageView() veya startTrackPage() - isteğe bağlı olarak ayarlanmış stopTrackPage(). Aynı clientPerformance değerleri. |
| Görünüm [0] adı |Dize |Sayfa başlığı.  En fazla uzunluk 250 |
| Görünüm [0] URL'si |Dize | |
| Görünüm [0] urlData.base |Dize | |
| Görünüm [0] urlData.hashTag |Dize | |
| Görünüm [0] urlData.host |Dize | |

## <a name="availability"></a>Kullanılabilirlik
Raporları [kullanılabilirlik web testleri](app-insights-monitor-web-app-availability.md).

| Yol | Tür | Notlar |
| --- | --- | --- |
| Kullanılabilirlik [0] availabilityMetric.name |Dize |availability |
| Kullanılabilirlik [0] availabilityMetric.value |Sayı |1.0 veya 0,0 |
| Kullanılabilirlik [0] sayısı |tamsayı |100 / ([örnekleme](app-insights-sampling.md) hızı). Örneğin 4 =&gt; % 25. |
| Kullanılabilirlik [0] dataSizeMetric.name |Dize | |
| Kullanılabilirlik [0] dataSizeMetric.value |tamsayı | |
| Kullanılabilirlik [0] durationMetric.name |Dize | |
| Kullanılabilirlik [0] durationMetric.value |Sayı |Test süresi. 1e7 1'ler == |
| Kullanılabilirlik [0] iletisi |Dize |Hata tanılama |
| Kullanılabilirlik [0] sonucu |Dize |Geçişi/başarısız |
| Kullanılabilirlik [0] runLocation |Dize |Http isteği coğrafi kaynağı |
| Kullanılabilirlik [0] testName |Dize | |
| Kullanılabilirlik [0] testRunId |Dize | |
| Kullanılabilirlik [0] testTimestamp |Dize | |

## <a name="metrics"></a>Ölçümler
TrackMetric() tarafından oluşturulur.

Ölçü değerini context.custom.metrics[0 içinde bulundu]

Örneğin:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Ölçüm değerleri hakkında
Ölçüm değerleri, hem ölçüm raporlar ve diğer yerlerde, standart nesne yapısıyla raporlanır. Örneğin:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Şu anda - yine de bu değişebilir gelecekte - tüm değerlerin standart SDK modüllerden bildirilen `count==1` ve yalnızca `name` ve `value` alanları kullanışlıdır. Kendi TrackMetric çağrıları yazma varsa, bunlar olacak farklı yalnızca durumda olur diğer parametreler kümesindeki.

Diğer alanları amacı portalına trafiğini azaltmak için SDK toplanacak ölçümleri izin vermektir. Örneğin, her bir ölçüm raporu göndermeden önce birkaç art arda okumalar ortalama. Ardından min, max, standart sapma ve toplam değeri (sum veya ortalama) hesaplamak ve rapor tarafından temsil edilen okumalar sayısına sayısını ayarlamak.

Yukarıdaki tablolarda, biz nadiren kullanılan alanları sayısı, min, max, stdDev ve sampledValue atlamış.

Önceden bir araya getirildiği ölçümleri yerine kullanabileceğiniz [örnekleme](app-insights-sampling.md) telemetri miktarının azaltılmasını gerekiyorsa.

### <a name="durations"></a>Süreleri
Aksi takdirde belirtilenler dışında süreleri 10000000.0 1 saniye anlamına bir milisaniyeye onda içinde gösterilir.

## <a name="see-also"></a>Ayrıca bkz.
* [Application Insights](app-insights-overview.md)
* [Sürekli dışarı aktarma](app-insights-export-telemetry.md)
* [Kod örnekleri](app-insights-export-telemetry.md#code-samples)
