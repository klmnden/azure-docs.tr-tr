---
title: Azure Stream analytics'te işlem yapılandırılabilir eşik temelli kurallar
description: Bu makalede, Azure Stream Analytics'te yapılandırılabilir eşik temelli kurallar olan bir uyarı çözüm geliştirmek için başvuru verilerini kullanmayı açıklar.
services: stream-analytics
author: rockboyfor
ms.author: v-yeche
manager: digimobile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
origin.date: 04/30/2018
ms.date: 08/20/2018
ms.openlocfilehash: ce2cf6ebdfd74549114e94e4c7356e387576d3c8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60761735"
---
# <a name="process-configurable-threshold-based-rules-in-azure-stream-analytics"></a>Azure Stream analytics'te işlem yapılandırılabilir eşik temelli kurallar
Bu makalede, Azure Stream Analytics'te yapılandırılabilir eşik temelli kurallar kullanan bir uyarı çözüm elde etmek için başvuru verilerini kullanmayı açıklar.

## <a name="scenario-alerting-based-on-adjustable-rule-thresholds"></a>Senaryo: Uyarı kuralı ayarlanabilir eşiklere dayanarak
Gelen akış olayları, belirli bir değer üst sınırına ulaştınız veya bu gelen akış olaylara dayalı bir toplu değere zaman belirli bir eşiği aştığında çıktı olarak bir uyarı oluşturmak gerekebilir. Bu sabit ve önceden belirlenen statik bir eşik değerine göre bir Stream Analytics sorgu ayarlamak basit. Sabit bir eşik değeri basit sayısal karşılaştırma kullanarak akış sorgu söz dizimi içinde sabit kodlanmış olabilir (büyüktür, küçüktür ve eşitlik).

Bazı durumlarda, eşik değerlerini eşik değeri değişiklikleri her zaman sorgu söz dizimi düzenlemeden daha kolay yapılandırılabilir gerekir. Diğer durumlarda, çok sayıda cihaz veya bunların her türde bir cihazda farklı eşik değerlerine sahip her biri ile aynı sorgu tarafından işlenen kullanıcı gerekebilir. 

Bu düzen, dinamik olarak eşiklerini yapılandırma, giriş verileri filtreleyerek eşiği geçerli cihaz türünü seçerek ve hangi alanların çıktıda seçerek için kullanılabilir.

## <a name="recommended-design-pattern"></a>Önerilen tasarım deseni
Bir başvuru veri girişi için bir Stream Analytics işi bir uyarı eşiklerini arama olarak kullanın:
- Başvuru verilerinde anahtarı başına bir değer eşik değerleri Store.
- Başvuru verilerinde anahtar sütunu için akış veri giriş olaylarını katılın.
- Eşik değeri başvuru verilerini anahtarlı değerini kullanın.

## <a name="example-data-and-query"></a>Örnek veri ve sorgu
Örnekte, bir dakika uzunluğundaki penceresinde cihazlardan gelen akış veri toplama kuralında başvuru verileri sağlanan görünürlüğe değerleri eşleştiğinde uyarıları üretilir.

Sorgudaki her bir cihaz kimliği ve cihaz kimliği altında her metricName için 0 ile 5 boyutlar GROUP BY için yapılandırabilirsiniz. Yalnızca ilgili filtre değerlerine sahip olayları gruplandırılıp. Gruplandırılmış sonra pencereli toplamlar, Min, Max, Avg, 60 saniye atlayan pencere üzerinde hesaplanır. Filtrelere göre toplanan değerler, ardından uyarı çıkış olayı üretip üretmeyeceğinizi de başvurusu, yapılandırılan eşiği göre hesaplanır.

Örneğin, adlı bir başvuru veri girişi olan bir Stream Analytics işi olduğu varsayılır **kuralları**ve veri girişi adlı akış **ölçümleri**. 

## <a name="reference-data"></a>Başvuru verileri
Bu örnek başvuru verileri, eşik tabanlı bir kuralın nasıl gösterilebilir gösterir. Bir JSON dosyası başvuru verilerini tutar ve Azure blob depolama alanına kaydedilir ve bu blob depolama kapsayıcısına adlı bir başvuru veri girişi kullanılan **kuralları**. Bu JSON dosyasının üzerine ve durdurma veya akış işi başlatılıyor, zaman giden kuralı yapılandırmasını değiştirin.

- Örnek kural CPU aştığında, ayarlanabilir bir uyarıyı temsil etmek için kullanılır (ortalama büyüktür veya eşittir) değeri `90` yüzdesi. `value` Alandır gerektiği şekilde yapılandırılabilir.
- Kural olduğunu fark bir **işleci** sorgu söz dizimi daha sonra dinamik olarak yorumlanır alanı `AVGGREATEROREQUAL`. 
- Kural belirli bir boyut anahtar verileri filtreler `2` değerle `C1`. Diğer alanlar giriş akışında olay alanlara göre filtre belirten, boş bir dize. Eşleşen diğer alanları gerektiği şekilde filtrelemek için ek CPU kurallar ayarlayabilirsiniz.
- Tüm sütunları çıkış uyarı olayı dahil edilecek. Bu durumda, `includedDim` anahtar numarasını `2` açık `TRUE` temsil eden olay verilerini stream'de alan sayısı 2 uygun çıkış olayları dahil edilir. Diğer alanları uyarı çıkışında yer almaz, ancak alan listesi ayarlanabilir.

```json
{
    "ruleId": 1234, 
    "deviceId" : "978648", 
    "metricName": "CPU", 
    "alertName": "hot node AVG CPU over 90",
    "operator" : "AVGGREATEROREQUAL",
    "value": 90, 
    "includeDim": {
        "0": "FALSE", 
        "1": "FALSE", 
        "2": "TRUE", 
        "3": "FALSE", 
        "4": "FALSE"
    },
    "filter": {
        "0": "", 
        "1": "",
        "2": "C1", 
        "3": "", 
        "4": ""
    }    
}
```

## <a name="example-streaming-query"></a>Akış örnek sorgu
Bu örnek Stream Analytics sorgu birleşimler **kuralları** başvuru verileri, yukarıdaki örnekte yazılan veri adlı bir giriş akışına **ölçümleri**.

```sql
WITH transformedInput AS
(
    SELECT
        dim0 = CASE rules.includeDim.[0] WHEN 'TRUE' THEN metrics.custom.dimensions.[0].value ELSE NULL END,
        dim1 = CASE rules.includeDim.[1] WHEN 'TRUE' THEN metrics.custom.dimensions.[1].value ELSE NULL END,
        dim2 = CASE rules.includeDim.[2] WHEN 'TRUE' THEN metrics.custom.dimensions.[2].value ELSE NULL END,
        dim3 = CASE rules.includeDim.[3] WHEN 'TRUE' THEN metrics.custom.dimensions.[3].value ELSE NULL END,
        dim4 = CASE rules.includeDim.[4] WHEN 'TRUE' THEN metrics.custom.dimensions.[4].value ELSE NULL END,
        metric = metrics.metric.value,
        metricName = metrics.metric.name,
        deviceId = rules.deviceId, 
        ruleId = rules.ruleId, 
        alertName = rules.alertName,
        ruleOperator = rules.operator, 
        ruleValue = rules.value
    FROM 
        metrics
        timestamp by eventTime
    JOIN 
        rules
        ON metrics.deviceId = rules.deviceId AND metrics.metric.name = rules.metricName
    WHERE
        (rules.filter.[0] = '' OR metrics.custom.filters.[0].value = rules.filter.[0]) AND 
        (rules.filter.[1] = '' OR metrics.custom.filters.[1].value = rules.filter.[1]) AND
        (rules.filter.[2] = '' OR metrics.custom.filters.[2].value = rules.filter.[2]) AND
        (rules.filter.[3] = '' OR metrics.custom.filters.[3].value = rules.filter.[3]) AND
        (rules.filter.[4] = '' OR metrics.custom.filters.[4].value = rules.filter.[4])
)

SELECT
    System.Timestamp as time, 
    transformedInput.deviceId as deviceId,
    transformedInput.ruleId as ruleId,
    transformedInput.metricName as metric,
    transformedInput.alertName as alert,
    AVG(metric) as avg,
    MIN(metric) as min, 
    MAX(metric) as max, 
    dim0, dim1, dim2, dim3, dim4
FROM
    transformedInput
GROUP BY
    transformedInput.deviceId,
    transformedInput.ruleId,
    transformedInput.metricName,
    transformedInput.alertName,
    dim0, dim1, dim2, dim3, dim4,
    ruleOperator, 
    ruleValue, 
    TumblingWindow(second, 60)
HAVING
    (
        (ruleOperator = 'AVGGREATEROREQUAL' AND avg(metric) >= ruleValue) OR
        (ruleOperator = 'AVGEQUALORLESS' AND avg(metric) <= ruleValue) 
    )
```

## <a name="example-streaming-input-event-data"></a>Örnek giriş olayı veri akışı
Bu örnek JSON verilerini temsil eden **ölçümleri** yukarıdaki akış sorguda kullanılan veri girişi. 

- Üç örnek olaylar 1 dakikalık zaman aralığı içinde değeri listelenen `T14:50`. 
- Üç aynı olan `deviceId` değer `978648`.
- CPU ölçüm değerleri her olay, değişiklik `98`, `95`, `80` sırasıyla. Yalnızca ilk iki örnek olayların en fazla kuralda kurulan CPU uyarı kuralı.
- Uyarı kuralı includeDim alanı 2 anahtar sayısı oluştu. Olaylara karşılık gelen anahtar 2 alanında adlı `NodeName`. Üç örnek olaylar değerlere sahip `N024`, `N024`, ve `N014` sırasıyla. Çıkışta gördüğünüz yalnızca düğüm `N024` Uyarı ölçütleri için yüksek CPU eşleşen olarak diğer bir deyişle veriler yalnızca. `N014` yüksek CPU eşiği karşılamıyor.
- Uyarı kuralı ile yapılandırılmış bir `filter` karşılık gelen yalnızca anahtar sayısı 2 ' nin `cluster` alan örnek olaylar. Tüm üç örnek olaylar değere sahip `C1` ve filtre ölçütüyle eşleşir.

```json
{
    "eventTime": "2018-04-30T14:50:23.1324132Z",
    "deviceId": "978648",
    "custom": {
        "dimensions": {
            "0": {
                "name": "NodeType",
                "value": "N1"
            },
            "1": {
                "name": "Cluster",
                "value": "C1"
            },
            "2": {
                "name": "NodeName",
                "value": "N024"
            }
        },
        "filters": {
            "0": {
                "name": "application",
                "value": "A1"
            },
            "1": {
                "name": "deviceType",
                "value": "T1"
            },
            "2": {
                "name": "cluster",
                "value": "C1"
            },
            "3": {
                "name": "nodeType",
                "value": "N1"
            }
        }
    },
    "metric": {
        "name": "CPU",
        "value": 98,
        "count": 1.0,
        "min": 98,
        "max": 98,
        "stdDev": 0.0
    }
}
{
    "eventTime": "2018-04-30T14:50:24.1324138Z",
    "deviceId": "978648",
    "custom": {
        "dimensions": {
            "0": {
                "name": "NodeType",
                "value": "N2"
            },
            "1": {
                "name": "Cluster",
                "value": "C1"
            },
            "2": {
                "name": "NodeName",
                "value": "N024"
            }
        },
        "filters": {
            "0": {
                "name": "application",
                "value": "A1"
            },
            "1": {
                "name": "deviceType",
                "value": "T1"
            },
            "2": {
                "name": "cluster",
                "value": "C1"
            },
            "3": {
                "name": "nodeType",
                "value": "N2"
            }
        }
    },
    "metric": {
        "name": "CPU",
        "value": 95,
        "count": 1,
        "min": 95,
        "max": 95,
        "stdDev": 0
    }
}
{
    "eventTime": "2018-04-30T14:50:37.1324130Z",
    "deviceId": "978648",
    "custom": {
        "dimensions": {
            "0": {
                "name": "NodeType",
                "value": "N3"
            },
            "1": {
                "name": "Cluster",
                "value": "C1 "
            },
            "2": {
                "name": "NodeName",
                "value": "N014"
            }
        },
        "filters": {
            "0": {
                "name": "application",
                "value": "A1"
            },
            "1": {
                "name": "deviceType",
                "value": "T1"
            },
            "2": {
                "name": "cluster",
                "value": "C1"
            },
            "3": {
                "name": "nodeType",
                "value": "N3"
            }
        }
    },
    "metric": {
        "name": "CPU",
        "value": 80,
        "count": 1,
        "min": 80,
        "max": 80,
        "stdDev": 0
    }
}
```

## <a name="example-output"></a>Örnek çıktı
Bu örnek, tek bir uyarı olay üretilmiş JSON verilerini gösterir, başvuru verilerinde tanımlanan CPU eşik kuralı dayalı çıktı. Çıkış olayı uyarı yanı sıra toplanan (ortalama, min, Maks) olarak kabul alanların adını içerir. Çıktı olay verilerini temel alan numarası 2 içeren `NodeName` değer `N024` kuralı yapılandırması nedeniyle. (JSON okunabilirlik için satır sonlarını göstermek için değiştirildi.)

```JSON
{"time":"2018-05-01T02:03:00.0000000Z","deviceid":"978648","ruleid":1234,"metric":"CPU",
"alert":"hot node AVG CPU over 90","avg":96.5,"min":95.0,"max":98.0,
"dim0":null,"dim1":null,"dim2":"N024","dim3":null,"dim4":null}
```
<!--Update_Description: updat meta properties, wording update-->