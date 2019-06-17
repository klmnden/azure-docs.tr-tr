---
title: Graph arama yöntemi - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Graph arama yöntemi, akademik varlıklara belirli grafik eğilimlere bağlı olarak bir dizi döndürmek için akademik bilgi API'sini kullanın.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 5d47b938560fb1bd15adfe1a1c2d35b7359d47a3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61339133"
---
# <a name="graph-search-method"></a>Graph arama yöntemi

**Grafik arama** REST API, akademik varlıklara verilen grafikle eğilimlere bağlı olarak bir dizi döndürmek için kullanılır.  Yanıt, kullanıcı tarafından belirtilen kısıtlamalara karşılamadığınızı grafik yolları kümesidir. Bir grafik yolu bir araya eklemeli graf düğümler ve kenarlar biçiminde dizisidir _v0, e0, v1, e1,..., vn_burada _v0_ yolun başlangıç düğümüdür.
<br>

**REST uç noktası:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?
```   
<br>

## <a name="request-parameters"></a>İstek Parametreleri  

Ad     | Değer | Gerekli mi?  | Açıklama
-----------|-----------|---------|--------
**Modu**       | Metin dizesi | Evet | Kullanmak istediğiniz mod adı. Değerin geçerli *json* veya *lambda*.

Graph arama yöntemi, bir HTTP POST isteği çağrılmalıdır. Post isteğinin içerik türü üst bilgisi içermelidir: **application/json**.

##### <a name="json-search"></a>JSON arama 

İçin *json* arama, POST gövdesini, bir JSON nesnesi. JSON nesnesi kullanıcı tanımlı kısıtlamalar içeren bir yol deseni açıklar (bkz [JSON nesnesinin belirtimi](JSONSearchSyntax.md) için *json* arama).


##### <a name="lambda-search"></a>Lambda Search

İçin *lambda* arama, POST gövdesini, düz metin dizesi. POST gövdesini olan tek bir C# deyimi, bir LIKQ lambda sorgu dizesi (bkz [belirtimi sorgu dizesinin](LambdaSearchSyntax.md) için *lambda* arama). 

<br>

## <a name="response-json"></a>Yanıt (JSON)

Ad | Açıklama
-------|-----   
**Sonuçları** | 0 veya sorgu ifadesi ile eşleşen daha fazla varlık dizisi. Her varlık, istenen özniteliklerinin değerlerini içerir. İstek başarıyla işlendi, bu alan mevcuttur.
**Hata** | HTTP durum kodları. İstek başarısız olursa, bu alan mevcuttur.
**İleti** | Hata iletisi. İstek başarısız olursa, bu alan mevcuttur.

Bir sorgu içinde işlenemiyor, _800 ms_, _zaman aşımı_ hata döndürülür. 

<br>

#### <a name="example"></a>Örnek:

##### <a name="json-search"></a>JSON arama
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?mode=json
```
<br>
İçin *json* arama başlıklarını içeren "altyapısı graf" incelemeler almak istiyoruz ve "tarafından bin shao" yazılmış, sorundan sorgu gibi belirtebilirsiniz.

```JSON
{
  "path": "/paper/AuthorIDs/author",
  "paper": {
    "type": "Paper",
    "NormalizedTitle": "graph engine",
    "select": [
      "OriginalTitle"
    ]
  },
  "author": {
    "return": {
      "type": "Author",
      "Name": "bin shao"
    }
  }
}
```

Bir sorgu çıktısı, grafik yolları dizisidir. Bir grafik yolu, düğüm nesneleri sorgu yolunda belirtilen düğümlerin karşılık gelen bir dizidir. Bu düğüm nesnelerin en az bir özellik olan *CellID*, varlık kimliğini temsil eder Özellik adlarını seçme işleci aracılığıyla belirterek diğer özellikler alınabilir bir [ *geçişi eylem nesnesi*](JSONSearchSyntax.md).

```JSON
{
  "Results": [
    [
      {
        "CellID": 2160459668,
        "OriginalTitle": "Trinity: a distributed graph engine on a memory cloud"
      },
      {
        "CellID": 2093502026
      }
    ],
    [
      {
        "CellID": 2171539317,
        "OriginalTitle": "A distributed graph engine for web scale RDF data"
      },
      {
        "CellID": 2093502026
      }
    ],
    [
      {
        "CellID": 2411554868,
        "OriginalTitle": "A distributed graph engine for web scale RDF data"
      },
      {
        "CellID": 2093502026
      }
    ],
    [
      {
        "CellID": 73304046,
        "OriginalTitle": "The Trinity graph engine"
      },
      {
        "CellID": 2093502026
      }
    ]
  ]
}
 ```

##### <a name="lambda-search"></a>Lambda Search 

```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?mode=lambda
```
<br>
İçin *lambda* arama, yazarın belirli bir inceleme kimliklerini almak istiyoruz, biz yazabilirsiniz aşağıdakine benzer bir sorgu.

```
MAG.StartFrom(@"{
    type  : ""Paper"",
    match : {
        NormalizedTitle : ""trinity: a distributed graph engine on a memory cloud""
    }
}").FollowEdge("AuthorIDs").VisitNode(Action.Return)
```

Çıktısını bir *lambda* arama sorgusu, da grafik yolları dizisi:

```JSON
{
  "Results": [
    [
      {
        "CellID": 2160459668
      },
      {
        "CellID": 2142490828
      }
    ],
    [
      {
        "CellID": 2160459668
      },
      {
        "CellID": 2116756368
      }
    ],
    [
      {
        "CellID": 2160459668
      },
      {
        "CellID": 2093502026
      }
    ]
  ]
}
```
 
## <a name="graph-schema"></a>Graf şeması

Graf şeması, grafik arama sorguları yazmak için yararlıdır. Aşağıdaki çizimde gösterilmektedir.

![Microsoft akademik graf şeması](./Images/AcademicGraphSchema.png)
