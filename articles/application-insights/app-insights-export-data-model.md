---
title: Azure uygulama Öngörüler veri modeli | Microsoft Docs
description: JSON içinde sürekli dışarı aktarılmış ve filtre olarak kullanılan özellikleri tanımlar.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: cabad41c-0518-4669-887f-3087aef865ea
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 03/21/2016
ms.author: mbullwin
ms.openlocfilehash: ee6597b78ac8de8fc3a7f3796010f22919243b23
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294903"
---
# <a name="application-insights-export-data-model"></a>Uygulama Öngörüler dışarı aktarma veri modeli
Bu tabloda telemetri gönderildiği özelliklerini [Application Insights](app-insights-overview.md) SDK'ları portalı.
Bu özellikler, veri çıkışı görürsünüz [sürekli verme](app-insights-export-telemetry.md).
Ayrıca özellik filtrelerini görüntülendikleri [ölçüm Gezgini](app-insights-metrics-explorer.md) ve [tanılama arama](app-insights-diagnostic-search.md).

Dikkat edilecek noktalar:

* `[0]` Bu tablolarda dizin eklemek için sahip olduğu yolu noktasında gösterir; ancak her zaman değil 0.
* Süreler milisaniyeye, bu nedenle 10000000 onda içinde olan 1 saniye ==.
* Tarihler ve saatler UTC olan ve ISO biçiminde verilir. `yyyy-MM-DDThh:mm:ss.sssZ`


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
| context.data.eventTime |dize |UTC |
| context.data.isSynthetic |boole |İstek bir bot veya web testi görünür. |
| context.data.samplingRate |number |Portala gönderilen SDK'sı tarafından oluşturulan telemetri yüzdesi. 0,0 100.0 aralığı. |
| Context.Device |object |İstemci aygıtı |
| Context.Device.browser |dize |IE, Chrome... |
| context.device.browserVersion |dize |Chrome 48,0... |
| context.device.deviceModel |dize | |
| context.device.deviceName |dize | |
| Context.Device.id |dize | |
| Context.Device.Locale |dize |tr GB, de-DE... |
| Context.Device.Network |dize | |
| context.device.oemName |dize | |
| context.device.osVersion |dize |Ana bilgisayar işletim sistemi |
| context.device.roleInstance |dize |Sunucu ana bilgisayar kimliği |
| context.device.roleName |dize | |
| Context.Device.Type |dize |Bilgisayar Tarayıcı... |
| Context.location |object |Clientip türetilmiş. |
| Context.location.City |dize |Biliniyorsa clientip türetilmiş |
| Context.location.clientip |dize |Son Sekizgene 0 olarak gizlidir. |
| Context.location.continent |dize | |
| Context.location.country |dize | |
| Context.location.Province |dize |Eyalet veya il |
| Context.Operation.id |dize |Aynı işlem kimliğine sahip öğeler portalda ilgili öğeler gösterilir. Genellikle istek kimliği. |
| Context.Operation.Name |dize |URL veya isteği adı |
| context.operation.parentId |dize |İç içe geçmiş ilgili öğeler sağlar. |
| Context.Session.id |dize |Aynı kaynak işlemlerinin grubunun kimliği. Bir işlem olmadan 30 dakikalık bir süre bir oturum sonuna işaret eder. |
| context.session.isFirst |boole | |
| context.user.accountAcquisitionDate |dize | |
| context.user.anonAcquisitionDate |dize | |
| context.user.anonId |dize | |
| context.user.authAcquisitionDate |dize |[Kimliği doğrulanmış kullanıcı](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated |boole | |
| internal.data.documentVersion |dize | |
| internal.Data.id |dize | Bir öğe alınan zaman Application Insights'a atanan benzersiz kimliği |

## <a name="events"></a>Olaylar
Tarafından oluşturulan özel olaylar [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent).

| Yol | Tür | Notlar |
| --- | --- | --- |
| [0] olay sayısı |integer |100 / ([örnekleme](app-insights-sampling.md) hızı). Örneğin 4 =&gt; % 25. |
| Olay [0] adı |dize |Olay adı.  En fazla uzunluk 250. |
| Olay [0] URL'si |dize | |
| Olay [0] urlData.base |dize | |
| Olay [0] urlData.host |dize | |

## <a name="exceptions"></a>Özel durumlar
Raporları [özel durumları](app-insights-asp-net-exceptions.md) sunucu ve tarayıcı.

| Yol | Tür | Notlar |
| --- | --- | --- |
| basicException [0] derlemesi |dize | |
| basicException [0] sayısı |integer |100 / ([örnekleme](app-insights-sampling.md) hızı). Örneğin 4 =&gt; % 25. |
| basicException [0] exceptionGroup |dize | |
| basicException [0] exceptionType |dize | |
| basicException [0] failedUserCodeMethod |dize | |
| basicException [0] failedUserCodeAssembly |dize | |
| basicException [0] handledAt |dize | |
| basicException [0] hasFullStack |boole | |
| basicException [0] kimliği |dize | |
| basicException [0] yöntemi |dize | |
| basicException [0] iletisi |dize |Özel durum iletisi. En fazla uzunluk 10k. |
| basicException [0] outerExceptionMessage |dize | |
| basicException [0] outerExceptionThrownAtAssembly |dize | |
| basicException [0] outerExceptionThrownAtMethod |dize | |
| basicException [0] outerExceptionType |dize | |
| basicException [0] outerId |dize | |
| basicException [0] parsedStack [0] derlemesi |dize | |
| basicException [0] [0] parsedStack fileName |dize | |
| basicException [0] [0] parsedStack düzeyi |integer | |
| basicException [0] [0] parsedStack satır |integer | |
| basicException [0] [0] parsedStack yöntemi |dize | |
| basicException [0] yığını |dize |En fazla uzunluk 10k |
| basicException [0] typeName |dize | |

## <a name="trace-messages"></a>İzleme İletileri
Tarafından gönderilen [TrackTrace](app-insights-api-custom-events-metrics.md#tracktrace)ve bunun [günlüğü bağdaştırıcıları](app-insights-asp-net-trace-logs.md).

| Yol | Tür | Notlar |
| --- | --- | --- |
| ileti [0] GünlükçüAdı |dize | |
| ileti [0] parametreleri |dize | |
| ileti [0] ham |dize |Günlük iletisi, uzunluk üst sınırı 10k. |
| [0] iletisi önem düzeyi |dize | |

## <a name="remote-dependency"></a>Uzak bağımlılık
TrackDependency tarafından gönderilir. Rapor performansı ve kullanımı için kullanılan [bağımlılıkları çağrıları](app-insights-asp-net-dependencies.md) server ve AJAX çağrıları tarayıcıda.

| Yol | Tür | Notlar |
| --- | --- | --- |
| remoteDependency [0] zaman uyumsuz |boole | |
| remoteDependency [0] baseName |dize | |
| remoteDependency [0] commandName |dize |Örneğin "home/Index" |
| remoteDependency [0] sayısı |integer |100 / ([örnekleme](app-insights-sampling.md) hızı). Örneğin 4 =&gt; % 25. |
| remoteDependency [0] dependencyTypeName |dize |HTTP SQL... |
| remoteDependency [0] durationMetric.value |number |Bağımlılık yanıtıyla tamamlanmasından çağrı süresi |
| remoteDependency [0] kimliği |dize | |
| remoteDependency [0] adı |dize |URL. En fazla uzunluk 250. |
| remoteDependency [0] resultCode |dize |HTTP bağımlılık |
| remoteDependency [0] başarılı |boole | |
| remoteDependency [0] türü |dize |HTTP Sql... |
| remoteDependency [0] URL'si |dize |En fazla uzunluk 2000 |
| remoteDependency [0] urlData.base |dize |En fazla uzunluk 2000 |
| remoteDependency [0] urlData.hashTag |dize | |
| remoteDependency [0] urlData.host |dize |En fazla uzunluk 200 |

## <a name="requests"></a>İstekler
Tarafından gönderilen [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest). Standart modüller, sunucuda ölçülen bu raporları sunucu yanıt süresi için kullanın.

| Yol | Tür | Notlar |
| --- | --- | --- |
| [0] isteği sayısı |integer |100 / ([örnekleme](app-insights-sampling.md) hızı). Örneğin: 4 =&gt; % 25. |
| İstek [0] durationMetric.value |number |Karşılık gelen istek süresi. 1e7 1'ler == |
| [0] istek kimliği |dize |İşlem kimliği |
| İstek [0] adı |dize |GET/POST + temel url.  En fazla uzunluk 250 |
| İstek [0] yanıt kodu |integer |İstemciye gönderilen HTTP yanıtı |
| İstek [0] başarılı |boole |Varsayılan == (yanıt kodu &lt; 400) |
| İstek [0] URL'si |dize |Konak dahil değil |
| İstek [0] urlData.base |dize | |
| İstek [0] urlData.hashTag |dize | |
| İstek [0] urlData.host |dize | |

## <a name="page-view-performance"></a>Sayfa görünümü performansı
Tarayıcı tarafından gönderilen. (Zaman uyumsuz AJAX çağrıları) tam görüntülenecek isteğini başlatarak kullanıcıdan bir sayfanın işlenmesi için gereken süre ölçer.

Bağlam değerleri, istemci işletim sistemi ve tarayıcı sürümü gösterir.

| Yol | Tür | Notlar |
| --- | --- | --- |
| clientPerformance [0] clientProcess.value |integer |Sayfanın görüntüleme için HTML Alma bitiş saati. |
| clientPerformance [0] adı |dize | |
| clientPerformance [0] networkConnection.value |integer |Bir ağ bağlantısı kurmak için geçen süre. |
| clientPerformance [0] receiveRequest.value |integer |HTML yanıta almak için isteği gönderme bitiş saati. |
| clientPerformance [0] sendRequest.value |integer |Saati HTTP isteği göndermek için gerçekleştirilecek. |
| clientPerformance [0] total.value |integer |Sayfayı görüntülemeye isteği göndermek başlangıç süresi. |
| clientPerformance [0] URL'si |dize |Bu istek URL'si |
| clientPerformance [0] urlData.base |dize | |
| clientPerformance [0] urlData.hashTag |dize | |
| clientPerformance [0] urlData.host |dize | |
| clientPerformance [0] urlData.protocol |dize | |

## <a name="page-views"></a>Sayfa Görüntülemeleri
TrackPageView() tarafından gönderilen veya [stopTrackPage](app-insights-api-custom-events-metrics.md#page-views)

| Yol | Tür | Notlar |
| --- | --- | --- |
| Görünüm [0] sayısı |integer |100 / ([örnekleme](app-insights-sampling.md) hızı). Örneğin 4 =&gt; % 25. |
| Görünüm [0] durationMetric.value |integer |Değeri trackPageView() veya startTrackPage() - isteğe bağlı olarak ayarlanmış stopTrackPage(). Aynı clientPerformance değerleri. |
| Görünüm [0] adı |dize |Sayfa başlığı.  En fazla uzunluk 250 |
| Görünüm [0] URL'si |dize | |
| Görünüm [0] urlData.base |dize | |
| Görünüm [0] urlData.hashTag |dize | |
| Görünüm [0] urlData.host |dize | |

## <a name="availability"></a>Kullanılabilirlik
Raporları [kullanılabilirlik web testleri](app-insights-monitor-web-app-availability.md).

| Yol | Tür | Notlar |
| --- | --- | --- |
| Kullanılabilirlik [0] availabilityMetric.name |dize |availability |
| Kullanılabilirlik [0] availabilityMetric.value |number |1.0 veya 0,0 |
| Kullanılabilirlik [0] sayısı |integer |100 / ([örnekleme](app-insights-sampling.md) hızı). Örneğin 4 =&gt; % 25. |
| Kullanılabilirlik [0] dataSizeMetric.name |dize | |
| Kullanılabilirlik [0] dataSizeMetric.value |integer | |
| Kullanılabilirlik [0] durationMetric.name |dize | |
| Kullanılabilirlik [0] durationMetric.value |number |Test süresi. 1e7 1'ler == |
| Kullanılabilirlik [0] iletisi |dize |Hata tanılama |
| Kullanılabilirlik [0] sonucu |dize |Geçişi/başarısız |
| Kullanılabilirlik [0] runLocation |dize |Http isteği coğrafi kaynağı |
| Kullanılabilirlik [0] testName |dize | |
| Kullanılabilirlik [0] testRunId |dize | |
| Kullanılabilirlik [0] testTimestamp |dize | |

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
