---
title: Şekil olayları Azure Time Series Insights (Önizleme) ile nasıl | Microsoft Docs
description: Azure Time Series Insights (Önizleme) ile olayları şekil nasıl anlama
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: edc1dac05a8ab4281eee3ee0eb4c5e6b7571b404
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856919"
---
# <a name="shaping-events-with-azure-time-series-insights-preview"></a>Azure Time Series Insights (Önizleme) ile olayları şekillendirme

Bu makalede JSON şekillendirme yönergeleri sağlanır, verimliliğini en üst düzeye çıkarmak için Azure Time Series Insights (Önizleme) sorguları demektir.

## <a name="best-practices"></a>En iyi uygulamalar

> [!NOTE]
> S1/S2 600 800 özelliği sınırları Azure TSI (Önizleme) için geçerli değildir.

Nasıl olayları Azure TSI gönderdiğiniz hakkında düşünmeniz önemlidir. Yani, her zaman bir şunları yapmalısınız:

1. verileri ağ üzerinden mümkün olduğunca verimli bir şekilde gönderin.
1. Verileriniz toplamalar senaryonuz için uygun gerçekleştirmenize olanak sağlayan bir şekilde depolanır emin olun.

Aşağıdaki yönergeler, en iyi olası sorgu performansı elde yardımcı olur:

1. Gereksiz özellikleri göndermeyin. TSI (Önizleme), kullanımınıza faturalandırır ve bu depolamak için en iyi ve sorgular, işlem verileri.
1. Ağ üzerinden statik veri göndermekten kaçınmanız statik veriler için örnek alanları kullanın. İş örneği alanları, zaman serisi modeli'nin bir bileşeni gibi başvuru verileri TSI hizmetinde kullanıma sunuldu. Örnek alanı hakkında daha fazla bilgi edinmek için [zaman serisi modelleri](./time-series-insights-update-tsm.md).
1. Verileri ağ üzerinden daha verimli bir şekilde göndermek için birden çok olayı arasında boyut özellikleri paylaşır.
1. Kapsamlı bir dizi iç içe kullanmayın. TSI nesneleri içeren iç içe geçen diziler en fazla iki düzeyini destekler. TSI, birden çok olay ile özellik değer çiftleri halinde iletileri dizilerde düzleştirir.
1. Yalnızca birkaç ölçüleri tüm veya çoğu olaylar için mevcut, bu ölçümleri ayrı Özellikler içinde aynı nesne olarak göndermek daha iyidir. Ayrı olarak göndererek, olay sayısını azaltır ve sorguları daha az olayların işlenmesi gereken daha yüksek performanslı hale getirebilir.

## <a name="example"></a>Örnek

Örneğin, burada birden çok cihaz ölçümleri veya sinyaller göndermek senaryoyu temel alır. Ölçümleri veya sinyalleri olabilir **Akış hızı**, **altyapısı Petrol baskısı**, **sıcaklık**, ve **nem**.

Aşağıdaki örnekte, yoktur tek bir IOT Hub ileti burada dış dizi boyut değerleri ortak paylaşılan bir bölüm içerir. Dış dizi ileti verimliliğini artırmak için zaman serisi örnek verilerini kullanır. Zaman serisi örneği ile her olay değiştirmez, ancak veri analizi için faydalı özellikler sağlar, cihaz meta verilerini içerir. Bu nedenle ileti daha verimli hale getirir ve kablo üzerinden gönderilen bayt sayısı üzerinde hem ortak boyut değerleri toplu işleme ve zaman serisi örnek meta veri, kullanan kaydeder.

Örnek JSON yükü:

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
                "Flow Rate psi": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

Zaman serisi örnek (Not: **zaman serisi kimliği** olduğu *DeviceID*):

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

TSI, sorgu süre boyunca (sonra düzleştirme) tablo katıldı. Tablonun türü gibi ek sütunlar dahil edilir. Bu örnek, telemetri verilerinizi nasıl şekillendirebileceğinize göstermektedir:

| deviceId  | Tür | L1 | L2 | timestamp | dizi. Akış hızı ft3/sn | dizi. Petrol baskısı PSI altyapısı |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | SİMÜLATÖR REVOLT | Pil sistem | 2018-01-17T01:17:00Z |    1.0172575712203979 |    34.7 |
| `FXXX` | LINE_DATA REVOLT | SİMÜLATÖR |    Pil sistem |    2018-01-17T01:17:00Z | 2.445906400680542 |  49.2 |
| `FYYY` | ORTAK LINE_DATA | SİMÜLATÖR |    Pil sistem |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22.2 |

Önceki örnekte aşağıdakilere dikkat edin:

* Statik özellikler, ağ üzerinden gönderilen verilerin en iyi duruma getirme TSI olarak depolanır.
* TSI veri kullanarak sorgu zamanında katılırsa *timeSeriesId* Örneğinizde tanımlanmış.
* İç içe geçme TSI tarafından desteklenen maksimum miktarı olan iç içe geçme iki katman kullanılır. İç içe dizi önlemek için önemlidir.
* Bazı ölçüler olduğundan ölçüler ayrı Özellikler içinde aynı nesne olarak gönderilir. Burada, **serisi. Akış hızı PSI**, serisi **altyapısı Petrol baskısı**, ve **ft3/sn** benzersiz sütun.

>[!IMPORTANT]
> Örnek alanları ile telemetri depolanmaz, meta verilerle depolanan **zaman serisi modeli**.
> Yukarıdaki tabloda, sorgu görünümü temsil eder.

## <a name="next-steps"></a>Sonraki adımlar

Bu yönergeleri uygulamaya koymak için bkz: [Azure TSI sorgu söz dizimi](./time-series-insights-query-data-csharp.md) TSI veri erişimi REST API'si için sorgu söz dizimi hakkında daha fazla bilgi edinmek için.
