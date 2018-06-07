---
title: JSON Azure zaman serisi Öngörüler sorgularda şekillendirme için en iyi uygulamalar.
description: Zaman serisi Öngörüler sorgu verimliliğinizi artırmak öğrenin.
services: time-series-insights
author: ashannon7
manager: timlt
ms.service: time-series-insights
ms.topic: article
ms.date: 05/24/2018
ms.author: bryanla
ms.openlocfilehash: 29919a3ce8c18982c88f7f0e6bbd774c612e9c44
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661090"
---
# <a name="how-to-shape-json-to-maximize-query-performance"></a>Şekil sorgu performansını en üst düzeye çıkarmak için JSON nasıl 

Bu makale Azure zaman serisi Öngörüler (TSI) sorgularınızı verimliliğini en üst düzeye çıkarmak için JSON şekillendirme için yönergeler sağlar.

## <a name="best-practices"></a>En iyi uygulamalar

Nasıl, olayları zaman serisi Insights'a gönderme hakkında düşünmek önemlidir. Yani, her zaman bir yapmanız gerekir:

1. veriler ağ üzerinden mümkün olduğunca verimli bir şekilde gönderin.
2. Verileriniz toplamalar senaryonuz için uygun gerçekleştirmenize olanak sağlayan bir şekilde depolanır emin olun.
3. olun TSI'ın en fazla özellik sınırları isabet yok
   - S1 ortamlar için 600 özellikleri (sütunları).
   - S2 ortamlar için 800 özellikleri (sütunları).

Aşağıdaki yönergeler, en iyi olası sorgu performansı sağlamaya yardımcı olur:

1. Özellik adı olarak bir etiket kimliği gibi dinamik özellikleri maksimum özellikleri sınırına ulaşması için katkı olarak kullanmayın.
2. Gereksiz özellikleri göndermeyin. Bir sorgu özelliği gerekli değilse, değil göndermek ve depolama sınırlamaları önlemek en iyisidir.
3. Kullanım [başvuru verileri](time-series-insights-add-reference-data-set.md), statik verilerin ağ üzerinden gönderilmesi önlemek için.
4. Verileri daha verimli bir şekilde ağ üzerinden göndermek için birden çok olay arasında boyut özellikleri paylaşır.
5. Derin dizi iç içe kullanmayın. TSI nesneleri içeren iç içe diziler en fazla iki düzeyini destekler. TSI iletileri dizilerde birden çok olay özelliğini değer çiftleri ile içine düzleştirir.
6. Yalnızca birkaç ölçüleri tüm veya çoğu olaylar için mevcut, aynı nesne içindeki ayrı bir özellik olarak bu ölçüleri göndermek daha iyidir. Ayrı ayrı göndererek olayları sayısını azaltır ve sorguları daha az olayların işlenmesi gereken gibi daha fazla kullanıcı kalmasına neden olabilir. Birkaç ölçüleri olduğunda, en fazla özellik sınırı çıkma olasılığını değerler tek bir özellik olarak göndererek en aza indirir.

## <a name="examples"></a>Örnekler

Aşağıdaki iki örnekler önceki öneriler vurgulamak için gönderen olayları gösterir. Her örneğin önerileri nasıl uygulanmış olduğunu görebilirsiniz.

Örnekler, burada birden çok aygıt ölçümleri veya sinyalleri gönderme senaryoyu temel alır. Ölçümleri veya sinyalleri Akış hızı, altyapısı Petrol baskısı, sıcaklık ve nem olabilir. İlk örnekte cihazlara birkaç ölçümleri vardır. İkinci örnekte, birçok cihaz vardır ve her birçok benzersiz ölçümleri gönderir.

### <a name="scenario-only-a-few-measurementssignals-exist"></a>Senaryo: yalnızca birkaç ölçümleri/sinyalleri var

**Öneri:** her ölçüm ayrı bir özellik/sütun olarak gönderin.

Aşağıdaki örnekte, yoktur tek bir IOT hub'ı ileti burada dış dizi ortak boyut değerlerinin paylaşılan bir bölüm içerir. Dış dizi başvuru verileri message verimliliğini artırmak için kullanır. Başvuru verileri, her olay ile değiştirmez, ancak veri analizi için faydalı özellikleri sağlar, cihaz meta veriler içeriyor. Bu nedenle hem ortak boyut değerleri toplu işleme ve başvuru verileri, kablo üzerinden gönderilen bayt üzerinde kaydeder kullanan ileti daha verimli hale.

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
                "Flow Rate psi": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

Başvuru veri tablosu (anahtar özelliği olan DeviceID):

| deviceId | MessageID | deviceLocation |
| --- | --- | --- |
| FXXX | SATIR\_VERİ | AV |
| FYYY | SATIR\_VERİ | ABD |

(Sonra düzleştirme) zaman serisi Insights olayı tablosu:

| deviceId | MessageID | deviceLocation | timestamp | dizi. Akış hızı ft3/s | dizi. Altyapısı Petrol baskısı PSI |
| --- | --- | --- | --- | --- | --- |
| FXXX | SATIR\_VERİ | AV | 2018-01-17T01:17:00Z | 1.0172575712203979 | 34.7 |
| FXXX | SATIR\_VERİ | AV | 2018-01-17T01:17:00Z | 2.445906400680542 | 49.2 |
| FYYY | SATIR\_VERİ | ABD | 2018-01-17T01:18:00Z | 0.58015072345733643 | 22.2 |

Önceki örnekte aşağıdakileri göz önünde bulundurun:

- **DeviceID** sütun bir Donanma çeşitli aygıtlardan sütun başlığı olarak hizmet verir. DeviceID değeri kurmaya kendi özellik adı sınırlıdır 595 (S1 ortamlarda) veya 795 (S2 ortamlarda) toplam cihazların diğer beş sütunlarla.

- Gereksiz özellikleri, engellendi örnek, marka ve model bilgisi, vb. için. Bunlar gelecekte sorgulanmasını olmaz olduğundan, bunları ortadan daha iyi ağ ve depolama verimliliği sağlar.

- Başvuru verileri ağ üzerinden aktarılan bayt sayısını azaltmak için kullanılır. İki öznitelik **MessageID** ve **deviceLocation** , anahtar özelliği kullanılarak birleştirilir **DeviceID**. Bu veriler, giriş aynı anda telemetri verileri ile birleştirilmiş ve daha sonra sorgulamaya TSI içinde depolanır.

- İç içe geçme TSI tarafından desteklenen en fazla olduğu iç içe geçme iki katmandan kullanılır. İç içe diziler önlemek için önemlidir.

- Birkaç ölçüler olduğundan ölçüleri aynı nesne içindeki ayrı bir özellik olarak gönderilir. Burada, **serisi. Oranı PSI akış** ve **serisi. Altyapısını Petrol baskısı ft3/s** benzersiz sütun.

### <a name="scenario-several-measures-exist"></a>Senaryo: birkaç ölçüleri var

**Öneri:** ölçümleri, "tür", "Birim", "value" Başlık Gönder.

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

Başvuru verileri: (anahtar özellikleri DeviceID ve series.tagId'tür)

| deviceId | series.tagId | MessageID | deviceLocation | type | birim |
| --- | --- | --- | --- | --- | --- |
| FXXX | pumpRate | SATIR\_VERİ | AV | Akış hızı | ft3/s |
| FXXX | oilPressure | SATIR\_VERİ | AV | Altyapısı Petrol baskısı | psi |
| FYYY | pumpRate | SATIR\_VERİ | ABD | Akış hızı | ft3/s |
| FYYY | oilPressure | SATIR\_VERİ | ABD | Altyapısı Petrol baskısı | psi |

(Sonra düzleştirme) zaman serisi Insights olayı tablosu:

| deviceId | series.tagId | MessageID | deviceLocation | type | birim | timestamp | Series.Value |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FXXX | pumpRate | SATIR\_VERİ | AV | Akış hızı | ft3/s | 2018-01-17T01:17:00Z | 1.0172575712203979 |
| FXXX | oilPressure | SATIR\_VERİ | AV | Altyapısı Petrol baskısı | psi | 2018-01-17T01:17:00Z | 34.7 |
| FXXX | pumpRate | SATIR\_VERİ | AV | Akış hızı | ft3/s | 2018-01-17T01:17:00Z | 2.445906400680542 |
| FXXX | oilPressure | SATIR\_VERİ | AV | Altyapısı Petrol baskısı | PSI | 2018-01-17T01:17:00Z | 49.2 |
| FYYY | pumpRate | SATIR\_VERİ | ABD | Akış hızı | ft3/s | 2018-01-17T01:18:00Z | 0.58015072345733643 |
| FYYY | oilPressure | SATIR\_VERİ | ABD | Altyapısı Petrol baskısı | psi | 2018-01-17T01:18:00Z | 22.2 |

Önceki örnekte aşağıdaki ve ilk örneğe benzer şekilde dikkat edin:

- sütunları **DeviceID** ve **series.tagId** çeşitli cihazlar ve bir Donanma etiketleri için sütun başlığı olarak hizmet eder. Her biri kendi özniteliği olarak kullanarak sorgu 594 (S1 ortamlarda) veya diğer altı sütunlarla 794 (S2 ortamlarda) toplam aygıtlar için sınırlı.

- gereksiz özellikleri ilk örnekte bildirdi neden atlanma.

- başvuru verileri sunarak ağ üzerinden aktarılan bayt sayısını azaltmak için kullanılan **DeviceID**, benzersiz çifti için **MessageID** ve **deviceLocation**. Bileşik bir anahtar kullanıldığında, **series.tagId**, benzersiz çifti için **türü** ve **ölçekleyemiyorsa**. Bileşik anahtarın izin veren **DeviceID** ve **series.tagId** dört değerlere başvurmak için kullanılmak üzere çifti: **MessageID, deviceLocation, türü,** ve **birim** . Bu veriler, giriş aynı anda telemetri verileri ile birleştirilmiş ve daha sonra sorgulamaya TSI içinde depolanır.

- iç içe geçme iki katmandan ilk örnekte bildirdi neden için kullanılır.

### <a name="for-both-scenarios"></a>Her iki senaryo için

Olası değerler, çok sayıda sahip bir özellik varsa, her bir değer için yeni bir sütun oluşturmak yerine tek bir sütun içinde farklı değerler olarak göndermek en iyisidir. Önceki iki örneklerden:
  - İlk örnekte, her ayrı bir özellik yapmak uygun olması için çeşitli değerleri olan birkaç özellik vardır. 
  - Ancak, ikinci örnekte ölçüleri ayrı ayrı özellikler, ancak bunun yerine, değerleri/ölçü ortak series özelliğini altında bir dizi olarak belirtilmeyen görebilirsiniz. Yeni bir anahtar gönderilir, **TagId** , yeni bir sütun oluşturur **series.tagId** düzleştirilmiş tablosundaki. Yeni özellikleri oluşturulur, **türü** ve **birim**, başvuru verileri kullanarak, böylece isabet gelen özellik sınırı engelliyor.

## <a name="next-steps"></a>Sonraki adımlar

Bu yönergeleri uygulamaya koymanın için bkz: [Azure zaman serisi Öngörüler sorgu sözdizimi](/rest/api/time-series-insights/time-series-insights-reference-query-syntax) REST API TSI verilere erişmek için sorgu söz dizimi hakkında daha fazla bilgi edinmek için.