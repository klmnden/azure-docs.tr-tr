---
title: Azure Time Series Insights sorgularda JSON şekillendirmek için en iyi yöntemler | Microsoft Docs
description: Azure Time Series Insights sorgu verimliliğinizi artırmak öğrenin.
services: time-series-insights
author: ashannon7
manager: cshankar
ms.service: time-series-insights
ms.topic: article
ms.date: 05/09/2019
ms.author: dpalled
ms.custom: seodec18
ms.openlocfilehash: 089285637bb740fea47f1fd07de0906dfe46662b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244471"
---
# <a name="shape-json-to-maximize-query-performance"></a>Sorgu performansını en üst düzeye çıkarmak için JSON Şekil 

Bu makalede Azure Time Series Insights sorgularınızı verimliliğini en üst düzeye çıkarmak için JSON şekle hakkında rehberlik sağlar.

## <a name="video"></a>Video

### <a name="learn-best-practices-for-shaping-json-to-meet-your-storage-needsbr"></a>Depolama ihtiyaçlarınızı karşılamak için JSON şekillendirme işlemi için en iyi uygulamaları öğrenin.</br>

> [!VIDEO https://www.youtube.com/embed/b2BD5hwbg5I]

## <a name="best-practices"></a>En iyi uygulamalar
Nasıl olayları zaman serisi öngörüleri için gönderdiğiniz hakkında düşünün. Ayrıca, her zaman:

1. verileri ağ üzerinden mümkün olduğunca verimli bir şekilde gönderin.
1. Senaryonuz için uygun toplamalar gerçekleştirebilmeleri için verilerinizin bir biçimde saklandığından emin olun.
1. Time Series Insights maksimum özelliği sınırlarına ulaştığından yoksa emin olun:
   - S1 ortamlar için 600 özellikleri (sütunları).
   - S2 ortamlar için 800 özellikleri (sütunları).

Aşağıdaki yönergeler, en iyi olası sorgu performansı sağlamaya yardımcı olur:

1. Bir etiket kimliği gibi dinamik özellikleri, özellik adı olarak kullanmayın. Bu, en fazla özellikleri sınırınıza ulaşmanız için katkı sağlar.
1. Gereksiz özellikleri göndermeyin. Bir sorgu özelliği gerekli değilse, bunu göndermemeyi en iyisidir. Bu şekilde depolama sınırlamaları kaçının.
1. Kullanım [başvuru verileri](time-series-insights-add-reference-data-set.md) ağ üzerinden statik veri göndermekten kaçınmanız.
1. Verileri ağ üzerinden daha verimli bir şekilde göndermek için birden çok olayı arasında boyut özellikleri paylaşır.
1. Kapsamlı bir dizi iç içe kullanmayın. Time Series Insights nesneleri içeren iç içe geçen diziler en fazla iki düzeyini destekler. Time Series Insights, birden çok olay ile özellik değer çiftleri halinde iletileri dizilerde düzleştirir.
1. Yalnızca birkaç ölçüleri tüm veya çoğu olaylar için mevcut, bu ölçümleri ayrı Özellikler içinde aynı nesne olarak göndermek daha iyidir. Ayrı olarak göndererek, olay sayısını azaltır ve daha az olayları işlenecek gerektiğinden sorgu performansını iyileştirebilir. Çeşitli ölçümleri olduğunda, değerler tek bir özellik olarak göndererek en fazla özellik sınırınıza ulaşmanız olasılığını en aza indirir.

## <a name="example-overview"></a>Örnek genel bakış

Aşağıdaki iki örnek, önceki öneriler vurgulamak için olayları göndermek nasıl ekleyebileceğiniz gösterilmektedir. Her bir örnekte öneri uygulanma görebilirsiniz.

Örnekleri burada birden çok cihaz ölçümleri veya sinyaller göndermek senaryoyu temel alır. Ölçümleri veya sinyalleri Akış hızı, altyapısı Petrol baskısı, sıcaklık ve nem olabilir. İlk örnekte, tüm cihazlarda birkaç ölçüler vardır. İkinci örnek çok sayıda cihaz bulunur ve birçok benzersiz ölçümleri her cihazı gönderir.

## <a name="scenario-one-only-a-few-measurements-exist"></a>Senaryo bir: Yalnızca birkaç ölçümler var

> [!TIP]
> Ayrı bir özellik ya da sütun olarak her bir ölçüm veya sinyal gönderme öneririz.

Aşağıdaki örnekte, burada dış dizi paylaşılan bir bölüm ortak boyut değerleri içeren tek bir Azure IOT Hub ileti yok. Dış dizi başvuru verilerini iletinin verimliliğini artırmak için kullanır. Başvuru verileri ile ilgili her olay değişmeyen cihaz meta verilerini içerir, ancak veri analizi için yararlı özellikleri sağlar. Ortak boyut değerleri toplu işleme ve veri kablo üzerinden gönderilen bayt kaldırır başvuru kullanan, hangi ileti daha verimli hale getirir.

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

* Başvuru veri anahtar özelliği içeren bir tabloda **DeviceID**:

   | deviceId | messageId | deviceLocation |
   | --- | --- | --- |
   | FXXX | SATIR\_VERİ | AB |
   | FYYY | SATIR\_VERİ | ABD |

* Düzleştirme sonra zaman serisi görüşleri olay tablosunu:

   | deviceId | messageId | deviceLocation | timestamp | dizi. Akış hızı ft3/sn | dizi. Petrol baskısı PSI altyapısı |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | SATIR\_VERİ | AB | 2018-01-17T01:17:00Z | 1.0172575712203979 | 34.7 |
   | FXXX | SATIR\_VERİ | AB | 2018-01-17T01:17:00Z | 2.445906400680542 | 49.2 |
   | FYYY | SATIR\_VERİ | ABD | 2018-01-17T01:18:00Z | 0.58015072345733643 | 22.2 |

Bu iki tablo ile ilgili notlar:

- **DeviceID** sütunu, sütun üst bilgisine bir fleet çeşitli cihazlar için görür. Cihaz kimliği değeri yaparak kendi özellik adı toplam cihaz sayısı (S1 ortamlarda için) 595 veya ile diğer beş sütun (için S2 ortamlarda) 795 sınırlar.
- Gereksiz özellikleri engellendi, örnek, marka ve model bilgi için. Özellikleri sorgulanan gelecekte gerekmez çünkü bunları ortadan daha iyi ağ ve depolama verimliliği sağlar.
- Başvuru verileri ağ üzerinden aktarılan bayt sayısını azaltmak için kullanılır. İki öznitelikleri **MessageID** ve **deviceLocation** anahtar özelliği kullanılarak birleştirilir **DeviceID**. Bu veriler ile telemetri verilerini giriş zamanında birleştirilir ve sorgulama için zaman serisi Öngörülerinde ardından depolanır.
- İç içe geçme zaman serisi görüşleri ile desteklenen maksimum miktarı olan iç içe geçme iki katman kullanılır. İç içe dizi önlemek için önemlidir.
- Bazı ölçüler olduğundan ölçüler ayrı Özellikler içinde aynı nesne olarak gönderilir. Burada, **serisi. Akış hızı PSI** ve **serisi. Petrol baskısı ft3/s motoru** benzersiz sütun.

## <a name="scenario-two-several-measures-exist"></a>Senaryo iki: Birden çok ölçü var

> [!TIP]
> "Yazarken," "Birim" ve "değer" diziler ölçümleri gönderme öneririz.

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

* Başvuru veri anahtar özellikleri içeren bir tabloda **DeviceID** ve **series.tagId**:

   | deviceId | series.tagId | messageId | deviceLocation | türü | Birim |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | SATIR\_VERİ | AB | Akış hızı | ft3/sn |
   | FXXX | oilPressure | SATIR\_VERİ | AB | Altyapısı Petrol baskısı | psi |
   | FYYY | pumpRate | SATIR\_VERİ | ABD | Akış hızı | ft3/sn |
   | FYYY | oilPressure | SATIR\_VERİ | ABD | Altyapısı Petrol baskısı | psi |

* Düzleştirme sonra zaman serisi görüşleri olay tablosunu:

   | deviceId | series.tagId | messageId | deviceLocation | türü | Birim | timestamp | Series.Value |
   | --- | --- | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | SATIR\_VERİ | AB | Akış hızı | ft3/sn | 2018-01-17T01:17:00Z | 1.0172575712203979 | 
   | FXXX | oilPressure | SATIR\_VERİ | AB | Altyapısı Petrol baskısı | psi | 2018-01-17T01:17:00Z | 34.7 |
   | FXXX | pumpRate | SATIR\_VERİ | AB | Akış hızı | ft3/sn | 2018-01-17T01:17:00Z | 2.445906400680542 | 
   | FXXX | oilPressure | SATIR\_VERİ | AB | Altyapısı Petrol baskısı | psi | 2018-01-17T01:17:00Z | 49.2 |
   | FYYY | pumpRate | SATIR\_VERİ | ABD | Akış hızı | ft3/sn | 2018-01-17T01:18:00Z | 0.58015072345733643 |
   | FYYY | oilPressure | SATIR\_VERİ | ABD | Altyapısı Petrol baskısı | psi | 2018-01-17T01:18:00Z | 22.2 |

Bu iki tablo ile ilgili notlar:

- Sütunları **DeviceID** ve **series.tagId** çeşitli cihazlar ve bir grubu etiketleri için sütun başlığı olarak görev yapar. Sorguyu 594 (için S1 ortamlarda) veya 794 (için S2 ortamda) kendi öznitelik sınırlar her kullanarak toplam altı diğer sütunları içeren cihaz.
- İlk örnekte Alıntı yapılan herhangi bir nedenle gereksiz özellikleri atlanma.
- Başvuru verileri tanıyarak ağ üzerinden aktarılan bayt sayısını azaltmak için kullanılan **DeviceID**, benzersiz çifti için kullanılan **MessageID** ve **deviceLocation**. Bileşik anahtarın **series.tagId** benzersiz çifti için kullanılan **türü** ve **birim**. Bileşik anahtarın sağlayan **DeviceID** ve **series.tagId** dört değerine başvurmak için kullanılacak çifti: **MessageID, deviceLocation, türü,** ve **Birim**. Bu veriler ile telemetri verilerini giriş anda katıldı. Sorgulama için zaman serisi Öngörülerinde sonra depolanır.
- İlk örnekte Alıntı yapılan herhangi bir nedenle iç içe geçme iki katman kullanılır.

### <a name="for-both-scenarios"></a>Her iki senaryo için

Çok sayıda olası değerler ile bir özellik için her bir değer için yeni bir sütun oluşturmak yerine tek bir sütunda benzersiz değerler olarak gönder en iyisidir. Önceki iki örneklerden:

  - Her bir ayrı özelliği yapmak uygun olacak şekilde ilk örnekte, çeşitli değerleri, birkaç özellik vardır.
  - İkinci örnekte, ölçüleri özellikler belirtilen değildir. Bunun yerine, değerler dizisi veya ölçüler bir ortak serisi özelliği altında demektir. Yeni anahtar **TagId** gönderilir, yeni bir sütun oluşturur **series.tagId** düzleştirilmiş tabloda. Yeni özellikleri **türü** ve **birim** özelliği sınırına değil, başvuru verilerini kullanarak oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

- Okuma [Azure Time Series Insights sorgu söz dizimi](/rest/api/time-series-insights/ga-query-syntax) Time Series Insights veri erişimi REST API'si için sorgu söz dizimi hakkında daha fazla bilgi edinmek için.
- Bilgi [Şekil olayları nasıl](./time-series-insights-send-events.md).
