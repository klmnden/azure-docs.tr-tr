---
title: Azure Cosmos DB Gremlin API için yürütme profilini işlevi içeren sorgularınıza değerlendir
description: Yürütme profili adımı kullanarak, Gremlin sorguları iyileştirmek ve sorun giderme hakkında bilgi edinin.
services: cosmos-db
author: luisbosquez
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: lbosq
ms.openlocfilehash: 2f3967c64e79b2bc7b01b35eff26f5ac0d4e3db4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60888426"
---
# <a name="how-to-use-the-execution-profile-step-to-evaluate-your-gremlin-queries"></a>Yürütme profili adımı, Gremlin sorgularını değerlendirmek için kullanma

Bu makalede, Azure Cosmos DB Gremlin API graf veritabanları için yürütme profili adım kullanmak nasıl bir genel bakış sağlar. Bu adım sorun giderme için ilgili bilgi sağlar; sorgu iyileştirmeleri ve uyumlu bir Cosmos DB Gremlin API hesabı karşı yürütülen herhangi bir Gremlin sorgu.

Bu adım kullanmak için basitçe ekleme `executionProfile()` işlev çağrısı, Gremlin sorgu sonunda. **Gremlin sorgunuzu yürütülecek** ve sorgu yürütme profiline sahip bir JSON yanıt nesnesi işleminin sonucunu döndürür.

Örneğin:

```java
    // Basic traversal
    g.V('mary').out()

    // Basic traversal with execution profile call
    g.V('mary').out().executionProfile()
```

Arama sonra `executionProfile()` adım, yanıt yürütülen Gremlin adımı içeren bir JSON nesnesi, geçen toplam süreyi ve bir dizi deyim içinde sonuçlanan Cosmos DB çalışma zamanı işleçleri olacaktır.

> [!NOTE]
> Bu uygulama için yürütme profilini Apache Tinkerpop belirtimi içinde tanımlı değil. Azure Cosmos DB Gremlin API'si özeldir.


## <a name="response-example"></a>Örnek yanıt

Döndürülen çıktının Açıklama eklenmiş bir örnek verilmiştir:

> [!NOTE]
> Bu örnekte yanıt genel yapısını açıklayan yorumlar ile açıklanıyor. Gerçek executionProfile yanıt yorumları içermez.

```json
[
  {
    // The Gremlin statement that was executed.
    "gremlin": "g.V('mary').out().executionProfile()",

    // Amount of time in milliseconds that the entire operation took.
    "totalTime": 28,

    // An array containing metrics for each of the steps that were executed. Each Gremlin step will translate to one or more of these steps.
    // This list is sorted in order of execution.
    "metrics": [
      {
        // This operation obtains a set of Vertex objects.
        // The metrics include: time, percentTime of total execution time, resultCount, fanoutFactor, count, size (in bytes) and time.
        "name": "GetVertices",
        "time": 24,
        "annotations": {
          "percentTime": 85.71
        },
        "counts": {
          "resultCount": 2
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 2,
            "size": 696,
            "time": 0.4
          }
        ]
      },
      {
        // This operation obtains a set of Edge objects. Depending on the query, these might be directly adjacent to a set of vertices, or separate, in the case of an E() query.
        // The metrics include: time, percentTime of total execution time, resultCount, fanoutFactor, count, size (in bytes) and time.
        "name": "GetEdges",
        "time": 4,
        "annotations": {
          "percentTime": 14.29
        },
        "counts": {
          "resultCount": 1
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 1,
            "size": 419,
            "time": 0.67
          }
        ]
      },
      {
        // This operation obtains the vertices that a set of edges point at.
        // The metrics include: time, percentTime of total execution time and resultCount.
        "name": "GetNeighborVertices",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 1
        }
      },
      {
        // This operation represents the serialization and preparation for a result from the preceding graph operations.
        // The metrics include: time, percentTime of total execution time and resultCount.
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 1
        }
      }
    ]
  }
]
```

> [!NOTE]
> ExecutionProfile adım Gremlin sorgu yürütülür. Bu içerir `addV` veya `addE`oluşturulmasında neden olur ve sorguda belirtilen değişiklikleri adımları. Sonuç olarak, Gremlin sorgu tarafından oluşturulan istek birimi ayrıca ücretlendirilir.

## <a name="execution-profile-response-objects"></a>Yürütme profil yanıt nesneleri

Aşağıdaki yapıya sahip JSON nesnelerin bir hiyerarşisini executionProfile() işlevin yanıt verecek olan:
  - **Gremlin işlem nesnesi**: Yürütülen tüm Gremlin işlemi temsil eder. Aşağıdaki özellikleri içerir.
    - `gremlin`: Yürütülmesi açık Gremlin deyimi.
    - `totalTime`: İçinde adımının yürütülmesi sonucunda süre, milisaniye cinsinden. 
    - `metrics`: Her sorgu karşılamak için yürütüldü Cosmos DB çalışma zamanı işleçleri içeren bir dizi. Bu liste, yürütme sırasına göre sıralanır.
    
  - **Cosmos DB çalışma zamanı işleçleri**: Tüm Gremlin işlemin bileşenlerin her birini temsil eder. Bu liste, yürütme sırasına göre sıralanır. Her nesne aşağıdaki özellikleri içerir:
    - `name`: İşleç adı. Değerlendirilme ve yürütülme adımı türüdür. Aşağıdaki tabloda daha fazlasını okuyun.
    - `time`: Belirli bir işleci geçen milisaniye cinsinden süre miktarı.
    - `annotations`: Yürütülen işleci özel olarak, ek bilgiler içerir.
    - `annotations.percentTime`: Belirli işleci yürütmek için harcanan toplam zamanı yüzdesi.
    - `counts`: Bu işleç tarafından döndürülen depolama katmanından nesnelerinin sayısı. Bu yer alan `counts.resultCount` içinde skaler değer.
    - `storeOps`: Bir veya birden çok bölüme yayılan bir depolama işlemi temsil eder.
    - `storeOps.fanoutFactor`: Bu belirli depolama işlemi erişilen bölümlerini sayısını temsil eder.
    - `storeOps.count`: Bu depolama işlemi döndürülen sonuç sayısını temsil eder.
    - `storeOps.size`: Boyutu bayt cinsinden belirtilen depolama işleminin sonucunu temsil eder.

Cosmos DB Gremlin çalışma zamanı işleci|Açıklama
---|---
`GetVertices`| Bu adım, Kalıcılık katmandan predicated bir nesneler kümesini alır. 
`GetEdges`| Bu adım, bir dizi köşeler için bitişik kenarları alır. Bu adım, bir veya daha çok depolama işlemleri neden olabilir.
`GetNeighborVertices`| Bu adım, kenarlar kümesine bağlı olan köşeler alır. Kenarları içeren bölüm anahtarları ve kimliğin hem kaynak hem de hedef köşelerin.
`Coalesce`| Bu adım, iki işlem değerlendirmesi için hesapları her `coalesce()` Gremlin adımı yürütülür.
`CartesianProductOperator`| Bu adım, iki veri kümesi arasında bir Kartezyen ürün hesaplar. Genellikle her yürütülen koşullarına `to()` veya `from()` kullanılır.
`ConstantSourceOperator`| Bu adım hesaplar. sonuç olarak bir sabit değer üretmek için bir ifade.
`ProjectOperator`| Bu adım hazırlar ve önceki işlem sonucunu kullanarak bir yanıtı serileştirir.
`ProjectAggregation`| Bu adım hazırlar ve bir toplama işlemi için bir yanıt serileştirir.

> [!NOTE]
> Bu liste, yeni işleç eklendikçe güncelleştirilmesi devam eder.

## <a name="examples-on-how-to-analyze-an-execution-profile-response"></a>Örnekler üzerinde bir yürütme profili yanıt analiz etme

Yürütme profili kullanılarak anlaþýlmaz genel iyileştirmeler örnekleri şunlardır:
  - Görme engelli yaygın sorgu.
  - Filtrelenmemiş sorgu.

### <a name="blind-fan-out-query-patterns"></a>Görme engelli yayma sorgu desenleri

Aşağıdaki yürütme profili yanıttan varsayar bir **bölümlenmiş graf**:

```json
[
  {
    "gremlin": "g.V('tt0093640').executionProfile()",
    "totalTime": 46,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 46,
        "annotations": {
          "percentTime": 100
        },
        "counts": {
          "resultCount": 1
        },
        "storeOps": [
          {
            "fanoutFactor": 5,
            "count": 1,
            "size": 589,
            "time": 75.61
          }
        ]
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 1
        }
      }
    ]
  }
]
```

Aşağıdaki sonuçları ondan hale getirilebilir:
- Gremlin deyim şu yapıdadır tek bir kimliği arama sorgu olduğu `g.V('id')`.
- Öğesinden yüksek `time` ölçüm, bu sorgunun gecikme süresi gibi görünüyor, olduğundan yüksek olmasını [10ms tek bir noktadan okuma işlemi için birden fazla](https://docs.microsoft.com/azure/cosmos-db/introduction#guaranteed-low-latency-at-99th-percentile-worldwide).
- İçine bakarsanız `storeOps` nesnesi, biz olduğunu görüyorsunuz `fanoutFactor` olduğu `5`, anlamına [5 bölümler](https://docs.microsoft.com/azure/cosmos-db/partition-data) bu işlem tarafından erişilen.

Bu analiz, sonuç, ilk sorgu gerekenden daha fazla bölüm erişmekte olduğunu belirleyebilirsiniz. Bu, bir koşul olarak sorgu bölümleme anahtarı belirterek çözülebilir. Bu daha az gecikme ve sorgu başına maliyeti daha az neden. Daha fazla bilgi edinin [grafik bölümleme](graph-partitioning.md). Daha iyi bir sorgu olacaktır `g.V('tt0093640').has('partitionKey', 't1001')`.

### <a name="unfiltered-query-patterns"></a>Filtrelenmemiş sorgu desenleri

Aşağıdaki iki yürütme profili yanıtları karşılaştırın. Kolaylık olması için bu örnekler, tek bir bölünmüş grafik kullanın.

Bu ilk sorgu etikete sahip tüm köşeleri alır `tweet` ve ardından, komşu köşeler alır:

```json
[
  {
    "gremlin": "g.V().hasLabel('tweet').out().executionProfile()",
    "totalTime": 42,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 31,
        "annotations": {
          "percentTime": 73.81
        },
        "counts": {
          "resultCount": 30
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 13,
            "size": 6819,
            "time": 1.02
          }
        ]
      },
      {
        "name": "GetEdges",
        "time": 6,
        "annotations": {
          "percentTime": 14.29
        },
        "counts": {
          "resultCount": 18
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 20,
            "size": 7950,
            "time": 1.98
          }
        ]
      },
      {
        "name": "GetNeighborVertices",
        "time": 5,
        "annotations": {
          "percentTime": 11.9
        },
        "counts": {
          "resultCount": 20
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 4,
            "size": 1070,
            "time": 1.19
          }
        ]
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 20
        }
      }
    ]
  }
]
```

Profili, aynı sorgu ancak bir ek filtre ile artık fark `has('lang', 'en')`, bitişik köşeler keşfetmeye önce:

```json
[
  {
    "gremlin": "g.V().hasLabel('tweet').has('lang', 'en').out().executionProfile()",
    "totalTime": 14,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 14,
        "annotations": {
          "percentTime": 58.33
        },
        "counts": {
          "resultCount": 11
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 11,
            "size": 4807,
            "time": 1.27
          }
        ]
      },
      {
        "name": "GetEdges",
        "time": 5,
        "annotations": {
          "percentTime": 20.83
        },
        "counts": {
          "resultCount": 18
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 18,
            "size": 7159,
            "time": 1.7
          }
        ]
      },
      {
        "name": "GetNeighborVertices",
        "time": 5,
        "annotations": {
          "percentTime": 20.83
        },
        "counts": {
          "resultCount": 18
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 4,
            "size": 1070,
            "time": 1.01
          }
        ]
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 18
        }
      }
    ]
  }
]
```

Bu iki sorguları aynı sonucu ulaştı, ancak, daha büyük bir ilk veri kümesi bitişik öğelerin sorgulama önce yineleme yapmak gerekli olduğundan daha fazla istek birimi ilk gerekir. Bu davranış göstergeleri hem de yanıtlar aşağıdaki parametreler karşılaştırılırken görebilirsiniz:
- `metrics[0].time` Değerdir bu tek adımda çözmek için uzun sürdüğünü belirten ilk yanıt olarak, daha yüksek.
- `metrics[0].counts.resultsCount` Değerdir ilk çalışma veri kümesini daha büyük olduğunu gösteren de ilk yanıt olarak, daha yüksek.

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [desteklenen Gremlin özellikleri](gremlin-support.md) Azure Cosmos DB'de. 
* Daha fazla bilgi edinin [Azure Cosmos DB Gremlin API](graph-introduction.md).
