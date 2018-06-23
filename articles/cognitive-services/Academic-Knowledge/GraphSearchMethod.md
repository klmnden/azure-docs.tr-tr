---
title: Akademik bilgi API'si arama yönteminde graph | Microsoft Docs
description: Grafik Search yöntemini akademik bilgi API'si Microsoft Bilişsel Hizmetleri'ndeki belirli grafik desenleri dayalı akademik varlık kümesi döndürmek için kullanın.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: d811db117c934c0d41fbfea1220d241cc022e4a8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351389"
---
# <a name="graph-search-method"></a>Grafik arama yöntemi

**Grafik arama** REST API verilen grafikle düzenlerini esas alarak akademik varlık kümesi döndürmek için kullanılır.  Yanıt, kullanıcı tanımlı kısıtlamalar çağıran grafik yolları kümesidir. Bir grafik yolu bir araya eklemeli grafik düğümleri ve kenarları biçiminde dizisidir _v0, e0, v1, e1,..., vn_, burada _v0_ yolun başlangıç düğümdür.
<br>

**REST uç noktası:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?
```   
<br>

## <a name="request-parameters"></a>İstek Parametreleri  
Ad     | Değer | Gerekli mi?  | Açıklama
-----------|-----------|---------|--------
**modu**       | Metin dizesi | Evet | Kullanmak istediğiniz mod adı. Ya da değerdir *json* veya *lambda*.

Bir HTTP POST isteği grafik arama yöntemi çağrılmalıdır. Post isteğini içerik türü üstbilgisi içermelidir: **uygulama/json**.

##### <a name="json-search"></a>JSON arama 

İçin *json* arama, posta gövdesi bir JSON nesnesi değil. JSON nesnesi kullanıcı tanımlı kısıtlamaları olan bir yol deseni açıklar (bkz [belirtimi JSON nesnesinin](JSONSearchSyntax.md) için *json* arama).


##### <a name="lambda-search"></a>Lambda arama

İçin *lambda* arama, POST gövdenin düz metin dizesi olur. POSTA gövdesi tek bir C# deyimi bir LIKQ lambda sorgu dizesi olan (bkz [sorgu dizesi belirtimi](LambdaSearchSyntax.md) için *lambda* arama). 

<br>
## <a name="response-json"></a>Yanıt (JSON)
Ad | Açıklama
-------|-----   
**Sonuçları** | 0 veya sorgu ifadesi eşleşen daha fazla varlık dizisi. Her varlığın istenen özniteliklerinin değerlerini içerir. İstek başarıyla işlendi, bu alan mevcuttur.
**hata** | HTTP durum kodları. Bu alan, istek başarısız olursa mevcuttur.
**İleti** | Hata iletisi. Bu alan, istek başarısız olursa mevcuttur.

İçinde bir sorgu işlenemiyor, _800 ms_, _zaman aşımı_ hata döndürülecek. 

<br>
#### <a name="example"></a>Örnek:

##### <a name="json-search"></a>JSON arama
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?mode=json
```
<br>
İçin *json* arama başlıkları içeren "altyapısı graph" yazıları almak istiyoruz ve "bin shao" tarafından yazılmış, biz sorgu şu şekilde belirtebilirsiniz.

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

Bir sorgu çıktısı, grafik yolları dizisidir. Bir grafik yolu sorgu yolunda belirtilen düğümleri karşılık gelen düğüm nesneleri dizisidir. Bu düğüm nesneleri en az bir özellik olan *CellID*, varlık kimliğini temsil eder Diğer özellikler select işlecini aracılığıyla özellik adları belirterek alınabilir bir [ *geçişi eylem nesnesi*](JSONSearchSyntax.md).

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

##### <a name="lambda-search"></a>Lambda arama 

```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?mode=lambda
```
<br>
İçin *lambda* arama, biz Yazar verilen kağıt kimliklerini almak istiyorsanız, biz yazabilirler aşağıdakine benzer bir sorgu.

```
MAG.StartFrom(@"{
    type  : ""Paper"",
    match : {
        NormalizedTitle : ""trinity: a distributed graph engine on a memory cloud""
    }
}").FollowEdge("AuthorIDs").VisitNode(Action.Return)
```

Çıktısını bir *lambda* arama sorgusu olduğunu da grafik yolları dizisi:

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
 
## <a name="graph-schema"></a>Grafik şeması

Grafik şema grafik arama sorguları yazmak için yararlıdır. Aşağıdaki çizimde gösterilmiştir.

![Microsoft akademik grafik şeması](./Images/AcademicGraphSchema.png)
