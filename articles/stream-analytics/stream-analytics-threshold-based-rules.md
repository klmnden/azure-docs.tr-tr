---
title: Azure Stream Analytics, işlem tabanlı yapılandırılabilir eşik kuralları
description: Bu makalede, Azure akış analizi temel yapılandırılabilir eşik kuralları sahip bir uyarı çözüm elde etmek için başvuru verileri kullanmayı açıklar.
services: stream-analytics
author: zhongc
ms.author: zhongc
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/30/2018
ms.openlocfilehash: 1c131c2c9ca12556c1d2cd52e7976d2f4272a0c8
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="process-configurable-threshold-based-rules-in-azure-stream-analytics"></a>İşlem yapılandırılabilir eşik tabanlı kurallarında Azure akış analizi
Bu makalede, Azure Stream Analytics içinde yapılandırılabilir eşik tabanlı kuralları kullanan bir uyarı çözüm elde etmek için başvuru verileri kullanmayı açıklar.

## <a name="scenario-alerting-based-on-adjustable-rule-thresholds"></a>Senaryo: Uyarı ayarlanabilir kural eşiklere dayanarak
Çıkış akış gelen olayları belirli bir değere ulaştınız veya gelen akış olaylara dayanarak bir toplu değeri belirli bir eşiği aşıyor gibi bir uyarı oluşturmak gerekebilir. Bu sabit ve önceden belirlenen statik bir eşik değeriyle karşılaştırılması Stream Analytics sorgu ayarlamak basit. Sabit bir eşik değeri basit sayısal karşılaştırmaları kullanma akış sorgu söz dizimi sabit kodlanmış olabilir (daha fazla değerinden ve eşitlik).

Bazı durumlarda, eşik değerleri bir eşik değeri değişiklikleri her zaman sorgu söz dizimi düzenlemeden daha kolay yapılandırılabilir olması gerekir. Diğer durumlarda, çok sayıda cihaz veya bunların her tür bir cihaz farklı eşik değerlerine sahip her biri ile aynı sorgu tarafından işlenen kullanıcı gerekebilir. 

Bu desen dinamik olarak eşikleri yapılandırmak, seçmeli olarak hangi girdi verileri filtreleyerek eşiği geçerlidir cihaz türünü seçin ve seçmeli olarak çıktıda alanları seç için kullanılabilir.

## <a name="recommended-design-pattern"></a>Önerilen tasarım deseni
Bir başvuru veri girişi akış analizi işi bir uyarı eşikleri araması kullanın:
- Başvuru verilerinde anahtar başına bir değer eşik değerleri depolar.
- Başvuru verilerinde anahtar sütunu için veri giriş olayların akışla katılın.
- Başvuru verileri anahtarlı değerini eşik değeri olarak kullanın.

## <a name="example-data-and-query"></a>Örnek veri ve sorgu
Bir dakika uzun penceresinde aygıtlardan akış veri toplama başvuru verileri olarak sağlanan kural stipulated değerleri eşleştiğinde örnekte uyarıları üretilir.

Sorgudaki her cihaz kimliği ve her metricName DeviceID altında için 0 ile 5 boyutları GROUP BY yapılandırabilirsiniz. Yalnızca karşılık gelen filtre değerlerine sahip olayları gruplandırılır. Gruplandırılmış bir kez, Min, Max, Avg, pencerelenmiş toplamlar 60 saniye atlayan pencere hesaplanır. Toplanan değerler filtreleri sonra başvurusu uyarı çıktı olayını oluşturmak için yapılandırılan eşik göre hesaplanır.

Örnek olarak, adlandırılmış bir başvuru veri girişi sahip bir akış analizi işi varsayalım **kuralları**ve veri girişi adlı akış **ölçümleri**. 

## <a name="reference-data"></a>Başvuru verileri
Bu örnek başvuru verileri, eşik tabanlı bir kuralın nasıl gösterilebilir gösterir. Bir JSON dosyası başvuru verileri tutar ve Azure blob depolama alanına kaydedilir ve bu blob depolama kapsayıcısını adlı bir başvuru veri girişi kullanılan **kuralları**. Bu JSON dosyanın üzerine yazmak ve zaman, durdurma veya iş akışında başlatmadan devam ettiği kural yapılandırmasını değiştirin.

- Örnek kural CPU aştığında ayarlanabilir uyarı temsil etmek için kullanılır (ortalama değerinden büyük veya eşittir) değeri `90` yüzde. `value` Alandır gerektiği gibi yapılandırılabilir.
- Kural yok fark bir **işleci** sorgu sözdiziminde daha sonra dinamik olarak yorumlanır alan `AVGGREATEROREQUAL`. 
- Bir kural, belirli bir boyut anahtar üzerindeki verileri filtreler `2` değerle `C1`. Bu olay alanlara göre Giriş akışı filtre belirten boş dize diğer alanlardır. Diğer eşleşen alanları gerektiği şekilde filtrelemek için ek CPU kuralları ayarlayabilirsiniz.
- Tüm sütunları çıkış uyarı olayı dahil edilecek. Bu durumda, `includedDim` anahtar numarası `2` açık `TRUE` akış olay verileri alan sayısı 2 niteleme dahil edilecek olayları çıktı temsil etmek için. Diğer alanlar uyarı çıktıda yoktur, ancak alan listesi ayarlanabilir.


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

## <a name="example-streaming-query"></a>Örnek akış sorgu
Bu örnek Stream Analytics sorgu birleştirir **kuralları** başvuru verileri, yukarıdaki örnekte yazılan veri adlı bir giriş akışı için **ölçümleri**.

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
Bu örnek JSON verilerini temsil eden **ölçümleri** giriş yukarıdaki akış sorguda kullanılan verileri. 

- Üç örnek olaylar değeri 1 dakikalık timespan içinde listelenen `T14:50`. 
- Üçü aynı olan `deviceId` değeri `978648`.
- Her durumda, CPU ölçüm değerler farklı `98`, `95`, `80` sırasıyla. Yalnızca ilk iki örnek olaylar kuralda kurulan CPU uyarı kuralı aşıyor.
- Uyarı kuralı includeDim alanında anahtar numarası 2 oluştu. Örnek olaylar karşılık gelen anahtar 2 alanında adlı `NodeName`. Üç örnek olaylar değerlere sahip `N024`, `N024`, ve `N014` sırasıyla. Çıktıda gördüğünüz yalnızca düğüm `N024` gibi diğer bir deyişle Uyarı ölçütleri için yüksek CPU eşleşen yalnızca verileri. `N014` yüksek CPU eşiği karşılamıyor.
- Uyarı kuralı ile yapılandırılmış bir `filter` karşılık gelen yalnızca anahtar sayısı 2, `cluster` örnek olaylar alanındaki. Tüm üç örnek olaylar değerine sahip `C1` ve filtre ölçütüyle eşleşen.

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
Bu örnek, başvuru verilerinde tanımlanan CPU eşik kuralı tek bir uyarı olay üretilmiştir JSON veri gösterir dayalı çıktı. Çıkış olayı uyarı yanı sıra toplanmış (ortalama, min, max) olarak kabul alanların adını içerir. Çıktı olay verileri alan anahtar numarası 2 içeren `NodeName` değeri `N024` kuralı yapılandırması nedeniyle. (JSON okunabilirlik için satır sonu göstermek için değiştirildi.)

```JSON
{"time":"2018-05-01T02:03:00.0000000Z","deviceid":"978648","ruleid":1234,"metric":"CPU",
"alert":"hot node AVG CPU over 90","avg":96.5,"min":95.0,"max":98.0,
"dim0":null,"dim1":null,"dim2":"N024","dim3":null,"dim4":null}
```