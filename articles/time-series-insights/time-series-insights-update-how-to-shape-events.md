---
title: Azure zaman serisi öngörüleri önizlemesi ile olayları Şekil | Microsoft Docs
description: Azure zaman serisi öngörüleri önizlemesi ile olayları şekli hakkında bilgi edinin.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: f0e1a79073596dcabfacb7163e12b33bb582b7c3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66238922"
---
# <a name="shape-events-with-azure-time-series-insights-preview"></a>Azure zaman serisi öngörüleri önizlemesi ile şekli olayları

Bu makale, Azure zaman serisi öngörüleri önizlemesi sorgularınızı verimliliğini en üst düzeye çıkarmak için JSON dosyanızı şekil yardımcı olur.

## <a name="best-practices"></a>En iyi uygulamalar

Nasıl olayları zaman serisi öngörüleri Önizleme gönderdiğiniz hakkında düşünün. Yani, her zaman bir şunları yapmalısınız:

* verileri ağ üzerinden mümkün olduğunca verimli bir şekilde gönderin.
* Senaryonuz için daha uygun bir şekilde toplama yardımcı olacak şekilde, verilerinizi Store.

En iyi olası sorgu performansı için aşağıdakileri yapın:

* Gereksiz özellikleri göndermeyin. Zaman serisi öngörüleri Önizleme, kullanımınıza düzenler. Depolamak ve işlemek, sorgulayacaksınız verileri idealdir.
* Örnek alanları statik verileri için kullanın. Bu yöntem, ağ üzerinden statik veri göndermekten kaçınmanız yardımcı olur. Time Series Insights verilerinde genellikle başvuru gibi iş örneği alanları, zaman serisi modeli'nin bir bileşeni olan kullanılabilir bir hizmet. Örnek alanları hakkında daha fazla bilgi için bkz: [zaman serisi modelleri](./time-series-insights-update-tsm.md).
* İki veya daha fazla olay arasında boyut özellikleri paylaşır. Bu uygulama, verileri ağ üzerinden daha verimli bir şekilde göndermek yardımcı olur.
* Kapsamlı bir dizi iç içe kullanmayın. Zaman serisi öngörüleri Önizleme nesneleri içeren iç içe geçen diziler en fazla iki düzeyini destekler. Zaman serisi öngörüleri Önizleme iletilerinde diziler özellik değeri çiftlerinin ile birden çok olaylara düzleştirir.
* Yalnızca birkaç ölçüleri tüm veya çoğu olaylar için mevcut, bu ölçümleri ayrı Özellikler içinde aynı nesne olarak göndermek daha iyidir. Ayrı olarak göndererek, olay sayısını azaltır ve daha az olayları işlenecek gerektiğinden sorgu performansını iyileştirebilir.

## <a name="example"></a>Örnek

Aşağıdaki örnek, burada iki veya daha fazla cihaz ölçümleri veya sinyaller göndermek bir senaryoya bağlıdır. Ölçümleri veya sinyalleri *Akış hızı*, *altyapısı Petrol baskısı*, *sıcaklık*, ve *nem*.

Aşağıdaki örnekte, burada dış dizi paylaşılan bir bölüm ortak boyut değerleri içeren tek bir Azure IOT Hub ileti yok. Dış dizi ileti verimliliğini artırmak için zaman serisi örnek verilerini kullanır. Zaman serisi örneği ile her olay değiştirmez, cihaz meta verilerini içeriyor ancak, veri analizi için faydalı özellikler sağlar. Kablo üzerinden gönderilen bayt kaydedin ve ileti daha verimli hale getirmek için ortak boyut değerleri toplu işleme ve zaman serisi örnek meta veri kullanan göz önünde bulundurun.

### <a name="example-json-payload"></a>Örnek JSON yükü

```JSON
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 1.0172575712203979,
                "Engine Oil Pressure psi ": 34.7
            },
            {
                "Flow Rate ft3/s": 2.445906400680542,
                "Engine Oil Pressure psi ": 49.2
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

### <a name="time-series-instance"></a>Zaman serisi örneği 
> [!NOTE]
> Zaman serisi kimliği *DeviceID*.

```JSON
{
    "timeSeriesId": [
      "FXXX"
    ],
    "typeId": "17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds": [
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description": null,
    "instanceFields": {
      "L1": "REVOLT SIMULATOR",
      "L2": "Battery System",
    }
  },
  {
    "timeSeriesId": [
      "FYYY"
    ],
    "typeId": "17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds": [
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description": null,
    "instanceFields": {
      "L1": "COMMON SIMULATOR",
      "L2": "Battery System",
    }
  },
```

Zaman serisi öngörüleri Önizleme sırasında sorgu süresini (düzleştirme sonra) bir tablo birleştirir. Tabloyu gibi ek sütunlar içeren **türü**. Aşağıdaki örnek nasıl gösterir [şekli](./time-series-insights-send-events.md#json) telemetri verileriniz.

| deviceId  | Tür | L1 | L2 | timestamp | dizi. Akış hızı ft3/sn | dizi. Petrol baskısı PSI altyapısı |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | SİMÜLATÖR | Pil sistem | 2018-01-17T01:17:00Z |   1.0172575712203979 |    34.7 |
| `FXXX` | Default_Type | SİMÜLATÖR |   Pil sistem |    2018-01-17T01:17:00Z | 2.445906400680542 |  49.2 |
| `FYYY` | ORTAK LINE_DATA | SİMÜLATÖR |    Pil sistem |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22.2 |

Önceki örnekte, aşağıdaki noktalara dikkat edin:

* Statik özellikler, ağ üzerinden gönderilen verilerin iyileştirmek için zaman serisi öngörüleri Önizleme depolanır.
* Zaman serisi öngörüleri Önizleme verileri, Örneğinizde tanımlanmış zaman serisi Kimliği'ni kullanarak sorgu zamanında birleştirilir.
* İç içe geçme iki katman, en çok, zaman serisi öngörüleri Preview tarafından desteklenen olduğu kullanılır. İç içe dizi önlemek için önemlidir.
* Bazı ölçüler olduğundan, ayrı Özellikler içinde aynı nesne olarak gönderildiniz. Örnekte, **serisi. Akış hızı PSI**, **serisi. Altyapısı Petrol baskısı PSI**, ve **serisi. Akış hızı ft3/sn** benzersiz sütun.

>[!IMPORTANT]
> Örnek alanları ile telemetri depolanmaz. Meta verilerle depolandıkları **zaman serisi modeli**.
> Yukarıdaki tabloda, sorgu görünümü temsil eder.

## <a name="next-steps"></a>Sonraki adımlar

- Bu yönergeleri uygulamaya koymak için bkz: [Azure zaman serisi öngörüleri önizlemesi sorgu söz dizimi](./time-series-insights-query-data-csharp.md). REST API erişimi zaman serisi öngörüleri Önizleme verileri için sorgu sözdizimi hakkında daha fazla bilgi edineceksiniz.
- Desteklenen JSON şekilleri hakkında bilgi edinmek için [desteklenen JSON şekilleri](./time-series-insights-send-events.md#json).
