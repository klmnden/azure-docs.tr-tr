---
title: Azure Time Series Insights sorgularda JSON şekillendirmek için en iyi yöntemler | Microsoft Docs
description: Azure Time Series Insights sorgu verimliliğinizi artırmak öğrenin.
services: time-series-insights
author: ashannon7
manager: cshankar
ms.service: time-series-insights
ms.topic: article
ms.date: 05/09/2019
ms.author: anshan
ms.custom: seodec18
ms.openlocfilehash: 535fe9a7920db89e434b4cc1b66aa38bf72a7829
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65790074"
---
# <a name="how-to-shape-json-to-maximize-query-performance"></a>Sorgu performansını en üst düzeye çıkarmak için JSON şekil nasıl 

Bu makalede, Azure Time Series Insights sorgularınızı verimliliğini en üst düzeye çıkarmak için JSON, şekillendirme yönergeleri sağlanır.

## <a name="video"></a>Video

### <a name="learn-best-practices-for-shaping-json-to-meet-your-storage-needsbr"></a>Depolama ihtiyaçlarınızı karşılamak için JSON şekillendirme işlemi için en iyi uygulamaları öğrenin.</br>

> [!VIDEO https://www.youtube.com/embed/b2BD5hwbg5I]

## <a name="best-practices"></a>En iyi yöntemler

Nasıl olayları zaman serisi öngörüleri için gönderdiğiniz hakkında düşünmeniz önemlidir. Yani, her zaman bir şunları yapmalısınız:

1. verileri ağ üzerinden mümkün olduğunca verimli bir şekilde gönderin.
1. Verileriniz toplamalar senaryonuz için uygun gerçekleştirmenize olanak sağlayan bir şekilde depolanır emin olun.
1. Olun Time Series Insights'ın en fazla özellik sınırları isabet yok:
   - S1 ortamlar için 600 özellikleri (sütunları).
   - S2 ortamlar için 800 özellikleri (sütunları).

Aşağıdaki yönergeler, en iyi olası sorgu performansı elde yardımcı olur:

1. En fazla özellikleri sınırına ulaşması için katkıda bulunan olarak, özellik adı olarak bir etiket kimliği gibi dinamik özellikler kullanmayın.
1. Gereksiz özellikleri göndermeyin. Bir sorgu özelliği gerekli değilse, değil göndermek ve depolama kısıtlamaları kaçınmak en iyisidir.
1. Kullanım [başvuru verileri](time-series-insights-add-reference-data-set.md), ağ üzerinden statik veri göndermekten kaçınmanız.
1. Verileri ağ üzerinden daha verimli bir şekilde göndermek için birden çok olayı arasında boyut özellikleri paylaşır.
1. Kapsamlı bir dizi iç içe kullanmayın. Time Series Insights nesneleri içeren iç içe geçen diziler en fazla iki düzeyini destekler. Time Series Insights, birden çok olay ile özellik değer çiftleri halinde iletileri dizilerde düzleştirir.
1. Yalnızca birkaç ölçüleri tüm veya çoğu olaylar için mevcut, bu ölçümleri ayrı Özellikler içinde aynı nesne olarak göndermek daha iyidir. Ayrı olarak göndererek, olay sayısını azaltır ve sorguları daha az olayların işlenmesi gereken daha yüksek performanslı hale getirebilir. Çeşitli ölçümleri olduğunda, değerler tek bir özellik olarak göndererek en fazla özellik limitini aştıktan olasılığını en aza indirir.

## <a name="example-overview"></a>Örnek genel bakış

Aşağıdaki iki örnek, önceki öneriler vurgulamak için gönderme olayları gösterir. Her bir örnekte önerileri nasıl uygulanmış olduğunu görebilirsiniz.

Örnekleri burada birden çok cihaz ölçümleri veya sinyaller göndermek senaryoyu temel alır. Ölçümleri veya sinyalleri Akış hızı, altyapısı Petrol baskısı, sıcaklık ve nem olabilir. İlk örnekte, tüm cihazlarda birkaç ölçüler vardır. İkinci örnekte, birçok cihaz vardır ve her birçok benzersiz ölçümleri gönderir.

## <a name="scenario-one-only-a-few-measurements-exist"></a>Senaryo bir: yalnızca birkaç ölçümler var

> [!TIP]
> **Öneri**: ayrı bir özellik/sütun her bir ölçüm/sinyali gönderir.

Aşağıdaki örnekte, yoktur tek bir IOT Hub ileti burada dış dizi boyut değerleri ortak paylaşılan bir bölüm içerir. Dış dizi başvuru verilerini iletinin verimliliğini artırmak için kullanır. Başvuru verileri ile ilgili her olay değiştirmez, ancak veri analizi için faydalı özellikler sağlar, cihaz meta verilerini içerir. Bu nedenle hem ortak boyut değerleri toplu işleme ve başvuru verileri, kablo üzerinden gönderilen bayt üzerinde kaydeder kullanan ileti daha verimli hale getirir.

Örnek JSON yükü:

```json
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

* Başvuru verileri tablo (anahtar özelliği **DeviceID**):

   | deviceId | messageId | deviceLocation |
   | --- | --- | --- |
   | FXXX | SATIR\_VERİ | AB |
   | FYYY | SATIR\_VERİ | ABD |

* (Sonra düzleştirme) zaman serisi görüşleri olay tablosu:

   | deviceId | messageId | deviceLocation | timestamp | dizi. Akış hızı ft3/sn | dizi. Petrol baskısı PSI altyapısı |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | SATIR\_VERİ | AB | 2018-01-17T01:17:00Z | 1.0172575712203979 | 34.7 |
   | FXXX | SATIR\_VERİ | AB | 2018-01-17T01:17:00Z | 2.445906400680542 | 49.2 |
   | FYYY | SATIR\_VERİ | ABD | 2018-01-17T01:18:00Z | 0.58015072345733643 | 22.2 |

Yukarıda:

- **DeviceID** sütunu, sütun üst bilgisine bir fleet çeşitli cihazlar için görür. DeviceID değer kurmaya kendi özellik adı sınırlıdır 595 (S1 ortamlarını) ya da 795 (S2 ortamlarını) toplam cihaz ile diğer beş sütun.

- Gereksiz özellikleri, engellendi örnek, marka ve model bilgileri, vb. için. Bunlar gelecekte sorgulanacağı olmaz olduğundan, bunları ortadan daha iyi ağ ve depolama verimliliği sağlar.

- Başvuru verileri ağ üzerinden aktarılan bayt sayısını azaltmak için kullanılır. İki öznitelik **MessageID** ve **deviceLocation**, anahtar özelliği birleştirilir **DeviceID**. Bu veriler, telemetri verileri giriş zamanında birleştirilmiş ve sorgulamak için zaman serisi Öngörülerinde sonradan depolanır.

- İç içe geçme zaman serisi görüşleri ile desteklenen maksimum miktarı olan iç içe geçme iki katman kullanılır. İç içe dizi önlemek için önemlidir.

- Bazı ölçüler olduğundan ölçüler ayrı Özellikler içinde aynı nesne olarak gönderilir. Burada, **serisi. Akış hızı PSI** ve **serisi. Petrol baskısı ft3/s motoru** benzersiz sütun.

## <a name="scenario-two-several-measures-exist"></a>Senaryo iki: birden çok ölçü var

> [!TIP]
> **Öneri:** ölçümleri, "type", "Birim", "value" diziler gönderin.

Örnek JSON yükü:

```json
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 1.0172575712203979
            },
            {
                "tagId": "oilPressure",
                "value": 49.2
            },
            {
                "tagId": "pumpRate",
                "value": 2.445906400680542
            },
            {
                "tagId": "oilPressure",
                "value": 34.7
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 0.58015072345733643
            },
            {
                "tagId": "oilPressure",
                "value": 22.2
            }
        ]
    }
]
```

* Başvuru verileri (anahtar özellikleri **DeviceID** ve **series.tagId**):

   | deviceId | series.tagId | messageId | deviceLocation | type | birim |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | SATIR\_VERİ | AB | Akış hızı | ft3/sn |
   | FXXX | oilPressure | SATIR\_VERİ | AB | Altyapısı Petrol baskısı | psi |
   | FYYY | pumpRate | SATIR\_VERİ | ABD | Akış hızı | ft3/sn |
   | FYYY | oilPressure | SATIR\_VERİ | ABD | Altyapısı Petrol baskısı | psi |

* (Sonra düzleştirme) zaman serisi görüşleri olay tablosu:

   | deviceId | series.tagId | messageId | deviceLocation | type | birim | timestamp | Series.Value |
   | --- | --- | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | SATIR\_VERİ | AB | Akış hızı | ft3/sn | 2018-01-17T01:17:00Z | 1.0172575712203979 | 
   | FXXX | oilPressure | SATIR\_VERİ | AB | Altyapısı Petrol baskısı | psi | 2018-01-17T01:17:00Z | 34.7 |
   | FXXX | pumpRate | SATIR\_VERİ | AB | Akış hızı | ft3/sn | 2018-01-17T01:17:00Z | 2.445906400680542 | 
   | FXXX | oilPressure | SATIR\_VERİ | AB | Altyapısı Petrol baskısı | Psi | 2018-01-17T01:17:00Z | 49.2 |
   | FYYY | pumpRate | SATIR\_VERİ | ABD | Akış hızı | ft3/sn | 2018-01-17T01:18:00Z | 0.58015072345733643 |
   | FYYY | oilPressure | SATIR\_VERİ | ABD | Altyapısı Petrol baskısı | psi | 2018-01-17T01:18:00Z | 22.2 |

Yukarıda:

- Sütunları **DeviceID** ve **series.tagId** çeşitli cihazlar ve bir grubu etiketleri için sütun başlığı olarak görev yapar. Her biri kendi özniteliği olarak kullanarak sorgu 594 (S1 ortamlarını) veya diğer altı sütunları 794 (S2 ortamlarını) toplam cihazlara sınırlı.

- İlk örnekte Alıntı yapılan herhangi bir nedenle gereksiz özellikleri atlanma.

- başvuru verileri tanıyarak ağ üzerinden aktarılan bayt sayısını azaltmak için kullanılan **DeviceID**, için benzersiz bir çift **MessageID** ve **deviceLocation**. Bir bileşik anahtarı kullanılır, **series.tagId**, benzersiz çiftinin **türü** ve **departmanla**. Bileşik anahtar verir **DeviceID** ve **series.tagId** dört değerleri belirtmek için kullanılmak üzere çifti: **MessageID, deviceLocation, türü,** ve **birim** . Bu veriler, telemetri verileri giriş zamanında birleştirilmiş ve sorgulamak için zaman serisi Öngörülerinde sonradan depolanır.

- İlk örnekte Alıntı yapılan herhangi bir nedenle iç içe geçme iki katman kullanılır.

### <a name="for-both-scenarios"></a>Her iki senaryo için

Çok sayıda olası değerler sahip bir özellik varsa, her bir değer için yeni bir sütun oluşturmak yerine tek bir sütun içinde farklı değerler olarak göndermek idealdir. Önceki iki örneklerden:

  - İlk örnekte, her bir ayrı özelliği yapmak uygun olacak şekilde, çeşitli değerleri içeren birkaç özellik vardır.
  - Ancak, ikinci örnekte, ölçüler, özellikler, ancak bunun yerine, değerler/ölçüler bir ortak serisi özelliği altında bir dizi olarak belirtilmeyen görebilirsiniz. Yeni bir anahtar gönderilir, **TagId** , yeni bir sütun oluşturur **series.tagId** düzleştirilmiş tabloda. Yeni özellikleri oluşturulur, **türü** ve **birim**başvuru verilerini kullanarak, böylece isabet gelen özellik sınırı engelliyor.

## <a name="next-steps"></a>Sonraki adımlar

- Okuma [Azure Time Series Insights sorgu söz dizimi](/rest/api/time-series-insights/ga-query-syntax) Time Series Insights veri erişimi REST API'si için sorgu söz dizimi hakkında daha fazla bilgi edinmek için.

- Bilgi [Şekil olayları nasıl](./time-series-insights-send-events.md).
