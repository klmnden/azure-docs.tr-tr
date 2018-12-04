---
title: Zaman serisi sorgu | Microsoft Docs
description: Axure Time Series Insights (Önizleme) zaman serisi sorgu
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: f5370d354833e9507d0794d7bc407a7ea2294e1a
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856943"
---
# <a name="time-series-query"></a>Zaman serisi sorgu

**Zaman serisi sorgu** (TSQ), işlem ve uygun ölçekte zaman serisi öngörüleri (TSI) depolanan zaman serisi verileri almak kolaylaştırır. Dönüştürme ve depolama alanından verileri almak için hesaplama tanımları TSQ yararlanır. Bunu gelen yapar **zaman serisi modelleri** (TSM) veya satır içi değişken tanımlar.

TSQ iki farklı şekilde verileri alır. TSQ kaynak Sağlayıcısı'ndan kayıtlı veri alabilir veya veri azaltabilir veya sağlamak için belirtilen yöntemi kullanarak sinyalleri dönüştürmek için işlemleri yeniden yapılandırma birleştirebilir ve zaman serisi verileri üzerinde hesaplamalar gerçekleştirin.

Zaman serisi öngörüleri verilerini yerel güncelleştirme TSI Gezgini veya genel API yüzeyi aracılığıyla erişilebilir. TSQ ayrıca bir ifade dili sunar, böylece daha gelişmiş kendi TSI, yazabilirsiniz. Hedeflerimiz TSQ ile aşağıda verilmiştir:

![Hedef][1]

## <a name="core-apis"></a>Core API'leri

Çekirdek destekliyoruz API'leri aşağıdadır.

![tsq][2]

### <a name="getmodelsettings-api"></a>getModelSettings API

GetModelSettings API, ortamları bölüm anahtarı için otomatik olarak oluşturulan model döndürmek için kullanılır.

API getModelSettings aşağıdaki parametreleri alır:

* *ad*: almak istediğiniz modelin adı.
* *timeSeriesIdProperties*

  * *Adı*
  * *type*

* *defaultTypeID*

#### <a name="getmodelsettings-json-request-and-response-example"></a>İstek ve yanıt getModelSettings JSON örneği

Bir GET HTTP isteği verilen:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/modelSettings?api-version=API_VERSION
```

| Ad | Açıklama | Örnek |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  Ortamınızın adını  | `environment123` |
| API_VERSION  |  API Belirtimi | `2018-11-01-preview` |

Yanıt:

```JSON
{
    "modelSettings": {
        "name": "DefaultModel",
        "timeSeriesIdProperties": [
            {
                "name": "someType1",
                "type": "String"
            }
        ],
        "defaultTypeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393"
    }
}
```

### <a name="gettypes-api"></a>getTypes API

Tüm zaman serisi türleri ve bunların ilişkili değişkenler modeli için API getTypes döndürür.

API getTypes aşağıdaki parametreleri alır:

* *typeId*: türünün benzersiz tanımlayıcısı değişmez.
* *timeSeriesId*: **zaman serisi kimliği** olay akışını ve model içindeki veriler için benzersiz bir anahtardır. Bu anahtar, TSI verileri bölümlemek için kullandığı ' dir.
* *Açıklama*: türünün açıklaması.
* *hierarchyIds*: ilişkili hierarchyIds türü için bir dizi.
* *instanceFields*:
  * *durumu*
  * *Şehir*

#### <a name="gettypes-json-request-and-response-example"></a>İstek ve yanıt getTypes JSON örneği

Bir HTTP POST isteği verilen:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/types/$batch?api-version=API_VERSION
```

| Ad | Açıklama | Örnek |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  Ortamınızın adını  | `environment123` |
| API_VERSION  |  API Belirtimi | `2018-11-01-preview` |

Aşağıdaki JSON gövdesi ve değişken özniteliklere sahip:

| Öznitelik | Gerekli veya isteğe bağlı |
| --- | --- |
| **tür**  |  Gerekli  |
| **Filtre**  |  İsteğe bağlı |
| **value**  | Boş veya belirtilmedi  |
| **ilişkilendirme**  |  İsteğe bağlı |
| **Toplama**  |  Gerekli |

```JSON
{
    "get": null,
    "put": [
        {
            "name": "SampleType",
            "description": "This is type 2",
            "variables": {
                "Avg Temperature": {
                    "kind": "numeric",
                    "filter": null,
                    "value": { "tsx": "$event.temperature.Double" },
                    "interpolation": "None",
                    "aggregation": {"tsx": "avg($value)"}
                },
                "Count Temperature": {
                    "kind": "aggregate",
                    "filter": null,
                    "value": null,
                    "interpolation": "None",
                    "aggregation": {"tsx": "count()"}
                },
                "Min Temperature": {
                    "kind": "aggregate",
                    "filter": null,
                    "value": null,
                    "interpolation": "None",
                    "aggregation": {"tsx": "min($event.temperature)"}
                },
            }
        }
    ]
}
```

Yanıt:

```JSON
{
    "get": null,
    "put": [
        {
            "timeSeriesType": {
                "id": "fc4f526c-da6e-4b85-87f7-16f6cf9b69be",
                "name": "type2",
                "description": "This is type 2",
                "variables": {
                    "Avg Temperature": {
                        "kind": "numeric",
                        "filter": null,
                        "value": { "tsx": "$event.temperature.Double" },
                        "interpolation": "None",
                        "aggregation": {"tsx": "avg($value)"}
                    },
                    "Count Temperature": {
                        "kind": "aggregate",
                        "filter": null,
                        "value": null,
                        "interpolation": "None",
                        "aggregation": {"tsx": "count()"}
                    },
                    "Min Temperature": {
                        "kind": "aggregate",
                        "filter": null,
                        "value": null,
                        "interpolation": "None",
                        "aggregation": {"tsx": "min($event.temperature)"}
                    }
                }
            },
            "error": null
        }
    ]
}
```

### <a name="gethierarchies-api"></a>getHierarchies API

Tüm zaman serisi hiyerarşiler ve bunların ilişkili alan yollarının API getHierarchies döndürür.

API getHierarchies aşağıdaki parametreleri alır:

* *Kimliği*: hiyerarşi için benzersiz şekilde tanımlayan anahtar.
* *ad*: hiyerarşi adı
* *Kaynak*
* *instanceFields*
  * *Yıl*
  * *Ay*

#### <a name="gethierarchies-json-request-and-response-example"></a>İstek ve yanıt getHierarchies JSON örneği

Bir HTTP POST isteği verilen:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/hierarchies/$batch?api-version=API_VERSION
```

| Ad | Açıklama | Örnek |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  Ortamınızın adını  | `environment123` |
| API_VERSION  |  API Belirtimi | `2018-11-01-preview` |

JSON gövdesi ile:

```JSON
{
    "get": null,
    "put": [
        {
            "id": "4c6f1231-f632-4d6f-9b63-e366d04175e3",
            "name": "Location",
            "source": {
                "instanceFieldNames": [
                    "state",
                    "city"
                ]
            }
        }
    ]
}
```

Yanıt:

```JSON
{
    "get": null,
    "put": [
        {
            "hierarchy": {
                "id": "4c6f1231-f632-4d6f-9b63-e366d04175e3",
                "name": "Location",
                "source": {
                    "instanceFieldNames": [
                        "state",
                        "city"
                    ]
                }
            },
            "error": null
        }
    ]
}
```

### <a name="getinstances-api"></a>getInstances API

API getInstances, zaman serisi örnekleri ve tüm ilişkili örneği alanlarının döndürmek için kullanılır.

API getInstances aşağıdaki parametreleri alır:

* *ad*: sorgulama yanı sıra UX'i içinde değiştirerek için kullanılan örneği ile ilişkili adı
* *typeId*: türü için benzersiz şekilde tanımlayan anahtar.
* *timeSeriesId*: **zaman serisi kimliği** olay akışını ve model içindeki veriler için benzersiz bir anahtardır. Bu anahtar, TSI verileri bölümlemek için kullandığı ' dir.
* *Açıklama*: örneği açıklaması.
* *hierarchyIds*: bir veya daha fazla hierarchyIds her hiyerarşi benzersiz olarak tanımlayan bir dizi.
* *instanceFields*:
  * *durumu*
  * *Şehir*

#### <a name="getinstances-json-request-and-response-example"></a>İstek ve yanıt getInstances JSON örneği

Bir HTTP POST isteği verilen:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/instances/$batch?api-version=API_VERSION
```

| Ad | Açıklama | Örnek |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  Ortamınızın adını  | `environment123` |
| API_VERSION  |  API Belirtimi | `2018-11-01-preview` |

JSON gövdesi ile:

```JSON
{
    "get": null,
    "put": [
        {
            "name": "SampleName",
            "typeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393",
            "timeSeriesId": [
                "samplePartitionKeyValueOne"
            ],
            "description": "floor 100",
            "hierarchyIds": [
                "e37a4666-9650-42e6-a6d2-788f12d11158"
            ],
            "instanceFields": {
                "state": "California",
                "city": "Los Angeles"
            }
        }
    ]
}
```

Yanıt:

```JSON
{
    "get": null,
    "put": [
        {
            "instance": null,
            "error": null
        },
        {
            "instance": null,
            "error": null
        }
    ]
}
```

### <a name="aggregateseries-api"></a>aggregateSeries API

Örnekleme ve toplama API'si sorgu ve yakalanan olayları TSI verileri alma sağlar aggregateSeries aggregate veya örnek işlevleri kullanarak verileri kaydedilir.

API aggregateSeries aşağıdaki parametreleri alır:

* *timeSeriesId*: **zaman serisi kimliği** olay akışını ve model içindeki veriler için benzersiz bir anahtardır. Bu anahtar, TSI verileri bölümlemek için kullandığı ' dir.
* *searchSpan*: Bu toplama ifadesi için timespan ve demet boyutu.
* *Filtre*: isteğe bağlı koşul yan tümcesi.
* *Aralığı*: aralığı, veri hesaplanan.
* *ProjectedVariables*: kapsamındaki hesaplanmasını değişkenleri.
* *InlineVariables(optional)*: ya istiyoruz değişkenleri tanımları geçersiz kılmak o gelen TSM veya hesaplama için sağlanan satır içi üzerinden gelen.

Sağlanan temel her zaman aralığı için her bir değişken için bir TSV API getSeries döndürür **zaman serisi kimliği** ve sağlanan değişkenleri kümesi. API getSeries TSM içinde depolanan veya satır içi aggregate veya örnek verilere sağlanan değişkenleri yararlanarak azaltma ulaşır.

> [!NOTE]
> Değişken ilişkilendirme şu anda desteklenmiyor.

Toplama türleri desteklenir:

* `Min`
* `Max`
* `Sum`
* `Count`
* `Average`

#### <a name="aggregateseries-json-request-and-response-example"></a>İstek ve yanıt aggregateSeries JSON örneği

Bir HTTP POST isteği verilen:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/query?api-version=API_VERSION
```

| Ad | Açıklama | Örnek |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  Ortamınızın adını  | `environment123` |
| API_VERSION  |  API Belirtimi | `2018-11-01-preview` |

JSON gövdesi ile:

```JSON
{
    "aggregateSeries": {
        "timeSeriesId": ["da2754af-cc51-46b3-b18f-6231f8781a12","PU.98"],
        "searchSpan": {
            "from": {"dateTime": "2016-08-01T00:00:00Z"},
            "to": {"dateTime": "2016-08-01T00:10:00Z"}
        },
        //Optional
        //If provided, Reduction should be specified in Variable Definition
        "interval": "PT1M",
        "filter": null, //Optional
        "projectedVariables": [
            "Count",
            "Average Temperature",
            "Min Temperature"
        ],
        "inlineVariables": {
            "Average Temperature": {
                "kind": "numeric",
                "filter": null,
                "value": { "tsx": "$event.temperature.Double" },
                "interpolation": "None",
                "aggregation": {"tsx": "avg($value)"}
            },
            //Default Type Count Experience
            "Count": {
                "kind": "aggregate",
                "filter": null,
                "value": null,
                "interpolation": "None",
                "aggregation": {"tsx": "count()"}
            },
            "Min Temperature": {
                "kind": "aggregate",
                "filter": null,
                "value": null,
                "interpolation": "None",
                "aggregation": {"tsx": "min($event.temperature)"}
            },
        }
    }
}
```

Yanıt:

```JSON
{
    "timestamps": [ "2016-08-01T00:00:00Z","2016-08-01T00:01:00Z","2016-08-01T00:02:00Z","2016-08-01T00:03:00Z","2016-08-01T00:04:00Z",
        "2016-08-01T00:05:00Z","2016-08-01T00:06:00Z","2016-08-01T00:07:00Z","2016-08-01T00:08:00Z","2016-08-01T00:09:00Z" ],
    "properties": [
        {
            "name": "Average Temperature",
            "type": "Double",
            "values": [ 68.699982037970059,68.889287593526234,69.089287593526919,69.28928759352759,69.489287593528246,69.689287593528917,69.8892875935296,70.089287593530273,70.289287593530929,70.4892875935316]
        },
        {
            "name": "Count Temperature",
            "type": "Double",
            "values": [ 60.0,60.0,60.0,60.0,60.0,60.0,60.0,60.0,60.0,60.0 ]
        },
        {
            "name": "Min Temperature",
            "type": "Double",
            "values": [ 68.645954260192127,68.790954260192578,68.990954260193249,69.190954260193919,69.39095426019459,69.590954260195261,69.790954260195932,69.9909542601966,70.190954260197273,70.390954260197944 ]
        }
    ]
}
```

### <a name="getseries-api"></a>getSeries API

Değişkenleri kullanarak kablo kaydedilen veri yararlanarak sorgu ve yakalanan olayları TSI verileri alma API'si sağlar getSeries modelinde tanımlayın veya satır içi sağlanan.

> [!Note]
> İlişkilendirme ve toplama yan tümcesi değişkeni sağlanır veya aralığı belirtildi, göz ardı edilir.

API getSeries aşağıdaki parametreleri alır:

* *timeSeriesId*: **zaman serisi kimliği** olay akışını ve model içindeki veriler için benzersiz bir anahtardır. Bu anahtar, TSI verileri bölümlemek için kullandığı ' dir.
* *searchSpan*: Bu toplama ifadesi için timespan ve demet boyutu.
* *Filtre*: isteğe bağlı koşul yan tümcesi.
* *ProjectedVariables*: kapsamındaki hesaplanmasını değişkenleri.
* *InlineVariables(optional)*: ya istiyoruz değişkenleri tanımları geçersiz kılmak o gelen TSM veya hesaplama için sağlanan satır içi üzerinden gelen.

Her değişken için bir TSV API getSeries döndürür sağlanan temel **zaman serisi kimliği** ve sağlanan değişkenleri kümesi. API başarır getSeries aralıkları veya değişken azaltma/ilişkilendirme desteklemez.  

#### <a name="getseries-json-request-and-response-example"></a>İstek ve yanıt getSeries JSON örneği

Bir HTTP POST isteği verilen:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/query?api-version=API_VERSION
```

| Ad | Açıklama | Örnek |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  Ortamınızın adını  | `environment123` |
| API_VERSION  |  API Belirtimi | `2018-11-01-preview` |

JSON gövdesi ile:

```JSON
{
    "getSeries": {
        "timeSeriesId": ["da2754af-cc51-46b3-b18f-6231f8781a12","PU.98"],
        "searchSpan": {
            "from": {"dateTime": "2016-08-01T00:00:00Z"},
            "to": {"dateTime": "2016-08-01T00:10:00Z"}
        },
        "interval": null, //Null or not specified
        "filter": null, //Optional
        "projectedVariables": [
            "Temperature",
            "Pressure",
        ],
        "inlineVariables": {
            "Temperature": {
                "kind": "numeric",
                "filter": null,
                "value": { "tsx": "$event.temperature.Double" },
                "interpolation": "None", // null, not specified or ignored if specified
                "aggregation": {"tsx": "avg($value)"} //null, not specified or ignored if specified
            },
            "Pressure": {
                "kind": "numeric",
                "filter": null,
                "value": { "tsx": "$event.pressure.Double" },
                "interpolation": "None", // null, not specified or ignored if specified
                "aggregation": {"tsx": "avg($value)"} //null, not specified or ignored if specified
            }
        }
    }
}
```

Yanıt:

```JSON
{
    "timestamps": [ "2016-08-01T00:00:00Z","2016-08-01T00:01:00Z","2016-08-01T00:02:00Z","2016-08-01T00:03:00Z","2016-08-01T00:04:00Z",
        "2016-08-01T00:05:00Z","2016-08-01T00:06:00Z","2016-08-01T00:07:00Z","2016-08-01T00:08:00Z","2016-08-01T00:09:00Z" ],
    "properties": [
        {
            "name": "Temperature",
            "type": "Double",
            "values": [ 68.699982037970059,68.889287593526234,69.089287593526919,69.28928759352759,69.489287593528246,69.689287593528917,69.8892875935296,70.089287593530273,70.289287593530929,70.4892875935316 ]
        },
        {
            "name": "Pressure",
            "type": "Double",
            "values": [ 68.645954260192127,68.790954260192578,68.990954260193249,69.190954260193919,69.39095426019459,69.590954260195261,69.790954260195932,69.9909542601966,70.190954260197273,70.390954260197944 ]
        }
    ]
}
```

### <a name="getevents-api"></a>getEvents API

Zaman serisi Öngörülerinde kaynak Sağlayıcısı'ndan kayıtlı sorgu ve olayları TSI verileri alma getEvents API sağlar.

API getEvents aşağıdaki parametreleri alır:

* *timeSeriesId*: **zaman serisi kimliği** olay akışını ve model içindeki veriler için benzersiz bir anahtardır. Bu anahtar, TSI verileri bölümlemek için kullandığı ' dir.
* *searchSpan*: Bu toplama ifadesi için timespan ve demet boyutu.
* *Filtre*: istediğiniz ancak sorgularınızı filtrelemek için koşul değerleri belirtmenize olanak tanır.
* *(isteğe bağlı) projectedProperties*: sütun/filtreleme özellikleri sağlar.

API döndürür ham getEvents yakalanan olayları için TSI depolanan gibi değerleri bir verilen **zaman serisi kimliği** ve zaman aralığı. Değişken tanımları (TSM ne değişken tanımları kullanılır) gerektirmez.

### <a name="getevents-json-request-and-response-example"></a>İstek ve yanıt getEvents JSON örneği

Bir HTTP POST isteği verilen:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/query?api-version=API_VERSION
```

| Ad | Açıklama | Örnek |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  Ortamınızın adını  | `environment123` |
| API_VERSION  |  API Belirtimi | `2018-11-01-preview` |

JSON gövdesi ile:

```JSON
{
    "getEvents": {
        // Required and Supports 3 Key as tsId
        "timeSeriesId": [
            "PU.123","W00158","ABN.9890"
        ],
        // Required.
        "searchSpan": {
            "from": "2018-08-01T17:01:00Z",
            "to": "2018-08-20T17:01:00Z",
        },
        // Optional.
        "filter": {
            "tsx": "($event.Value != null) OR ($event.Status.String == 'Good')"
        },
        // Optional – array of simple TSXs that refer exactly one property
        // If not specified, we will return all the properties.
        "projectedProperties": [ 
            {"propertyName" : "Volts", "type": "double" } 
        ]
    }
}
```

Yanıt:

```JSON
{
    "timestamps": [ "2016-08-01T00:00:00Z","2016-08-01T00:01:03Z","2016-08-01T00:02:05Z","2016-08-01T00:03:00Z",
        "2016-08-01T00:04:00Z","2016-08-01T00:05:05Z","2016-08-01T00:06:00Z","2016-08-01T00:07:08Z",
        "2016-08-01T00:08:00Z","2016-08-01T00:09:00Z" ],
    "properties": [
        {
            "name": "Volts",
            "type": "Double",
            "values": [ 68.699982037970059,68.889287593526234,69.089287593526919,69.28928759352759,69.489287593528246,69.689287593528917,69.8892875935296,70.089287593530273,70.289287593530929,70.4892875935316 ]
        }
    ]
}
```

### <a name="supported-query-operators"></a>Desteklenen sorgu işleçleri

Filtreleme:

* `HAS`
* `IN`
* `AND`
* `AND NOT`
* `OR`
* `>`
* `<`
* `<=`
* `>=`
* `!=`
* `<>`
* `/`
* `*`

Zamana bağlı:

* `From`
* `To`
* `First/Last` (Yalnızca API)

Toplama/dönüştürme:

* `MEASURE`
* `SUM` (bir ölçü)
* `COUNT` (olayları)
* `AVG` (bir ölçü)
* `MIN` (bir ölçü)
* `MAX` (ölçü)'
* `GROUP BY` (kategorik sütun)
* `ORDER BY ASC, DSC` (göreli zaman damgası.)
* `DateHistogram` (kova boyutu)

## <a name="tsq-api-limits"></a>TSQ API sınırları

> [!NOTE]
> Aşağıdaki tablo olarak sınırlarını belirtir **20/11/18**.

| Parametre | Sınırlar |
| --- | --- |
| TSQ istek boyutu | 32 KB |
| TSQ yanıt sınırı | 16 MB |
| TSQ paralel sorgu sınırı | ortam başına 20 |
| Etkinlikleri Al | 16 MB boyutunda veya 30 saniye süre |
| Belirtecin geçerlilik | 1 saat |
| Seri Al | 500.000 aralıkları veya zaman damgası veya 16 MB boyut |
| Min | Aralık sınırı 1 ms |
| Maks | 50 öngörülen değişkenleri |
| Toplama serisi | 500.000 aralıkları veya zaman damgası veya 16 MB boyut |
| Min | Aralık sınırı 1 ms |
| Maks | 50 öngörülen değişkenleri |

## <a name="time-series-query-tutorial"></a>Zaman serisi sorgu Öğreticisi

Tam bir TSQ öğretici gelecekte sağlanır.

[!INCLUDE [tsi-update-docs](../../includes/time-series-insights-update-documents.md)]

## <a name="next-steps"></a>Sonraki adımlar

Okuma [Azure TSI (Önizleme) depolama ve giriş](./time-series-insights-update-storage-ingress.md).

Yeni hakkında okuyun [zaman serisi modeli](./time-series-insights-update-tsm.md).

Hakkında bilgi edinin [iyi bir zaman serisi kimliği seçerken](./time-series-insights-update-how-to-id.md).

<!-- Images -->
[1]: media/v2-update-tsq/goal.png
[2]: media/v2-update-tsq/tsq.png
