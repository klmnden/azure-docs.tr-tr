---
title: Akademik bilgi API'si JSON arama söz dizimi | Microsoft Docs
description: Akademik bilgi API'si Microsoft Bilişsel Hizmetleri'ndeki kullanabileceğiniz JSON arama söz dizimi hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: a4b9cf535dae60258d71c43bba6f9eec1444bd41
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351365"
---
# <a name="json-search-syntax"></a>JSON arama söz dizimi

```javascript
/* Query Object:
   Suppose we have a query path /v0/e0/v1/e1/...
   A query object contains constraints to be applied to graph nodes in a path.
   Constraints are given in <"node identifier", {constraint object}> key-value pairs: 
*/
{
    /* query path can also be set in the query object */
    path: "/v0/e0/v1/e1/.../vn/",
    v0: { /* A Constraint Object */ },
    v1: { /* A Constraint Object */ },
}
```

Bir sorgu yolunda düğüm adı (_v0, v1,..._ ) sorgu nesnesinde; başvurulabilir düğümü tanımlayıcıları olarak hizmet edge adları (_e0, e1,..._ ) karşılık gelen kenarları türlerini yolu temsil eder. Bir yıldız işareti kullanırız _*_ (dışında hangi verilmelidir başlangıç düğümü,) düğüm veya kenar adı olarak bildirmek için var olan bu tür bir öğe üzerinde hiç bir kısıtlama. Örneğin, bir sorgu yolu `/v0/*/v1/e1/*/` kenar türünü kısıtlamadan grafikten yolları alır _(v0, v1)_. Bu sırada, sorgu kısıtlamaları yolun hedef (son düğümü) ya da yok.

Bir yol yalnızca tek bir düğüm içerdiğinde söyleyin _v0_, sorgu sadece kısıtlamaları karşılayan tüm varlıkları döndürür. Başlangıç düğüme uygulanan bir kısıtlama nesnesi olarak adlandırılan bir *Başlangıç sorgu nesnesi*, olan belirtimi gibi verilen.

```javascript
/* Starting Query Object:
   Specifies constraints on the starting node
*/
{
    /* "match" operator searches for entities with the specified properties. 
       All properties defined in the graph schema be queried such as "Name" and "NormalizedTitle".
     */
    "match": { 
        "Name" : "some name",
        ...
    },
    /* "id" operator directly specifies the IDs of the starting nodes. 
       When it is present, the "match" operator is ignored. 
     */
    "id": [ id1, id2, id3... ],
    /* "type" operator limits results to a certain type of entities,
       for example, "Author" or "Paper".
     */
    "type": "Author",
    /* "select" operator pulls properties from the database and 
       returns them to the client.
     */
    "select": ["PaperRank", ...]
}
```

Bir yol daha fazlasını başlangıç düğümü içeriyorsa, Sorgu işlemcisi belirtilen yol deseni izleyen bir grafik geçişi gerçekleştirir. Bir düğümde geldiğinde, kullanıcı tarafından belirtilen geçişi Eylemler, diğer bir deyişle, tetiklenecek mi, yoksa geçerli düğümde durdurup dönmek için grafik keşfetmeye devam etmek için. Herhangi bir çapraz geçişi eylemi belirtildiğinde, varsayılan eylemleri ulaşabilirsiniz. Bir ara için varsayılan eylem grafiği keşfetmeye devam etmek için düğümdür. Son bir yolu için varsayılan eylem durdurup dönmek için düğümdür. Çapraz Geçişi eylemleri belirten bir kısıtlama nesnesi olarak adlandırılan bir *geçişi eylem nesnesi*. Belirtimindekini aşağıda verilmiştir:

```javascript
/* Traversal Action Object:
   Specifies graph traversal actions on a node (except for the starting node).
 */
{
    /* Set the continue condition here. */
    continue: { 
        /* simple property exact match */
        "property_key" : "property_value", 
        /* Advanced query operators */
        /* -- Numerical comparisons */
        "property_key_2" : { "gt" /* or simply ">" */ : 123 },
        /* -- Substring query */
        "property_key_3" : { "substring" : "456" },
        /* -- CellID query */
        "id": [ 111, 222, 333... ],
        /* -- Entity type query */
        "type": "cell_type",
        /* -- Property existance query */
        "has" : "property_key_4",
        /* -- Logical operators */
        /* ---- Note that, by default the operators are combined with AND semantics */
        
        /* -- OR operator */
        "or": {
          /* Query operators... */
        },
        /* -- NOT operator */
        "not": {
            /* Query operators... */
        }
    },
    /* Set the return condition here. */
    return: {
        /* Same operators as the continue object */
    },
    /* The selected properties to return. */
    select: ["property_key_1", ...]
}
```

POST gövdesinde bir *json* arama sorgusu bulunması gereken en az bir *yolu* düzeni. Çapraz eylem nesneleri isteğe bağlıdır. Aşağıda, iki örnek verilmiştir.

```JSON
{
  "path": "/series/ConferenceInstanceIDs/conference/FieldOfStudyIDs/field",
  "series": {
    "type": "ConferenceSeries",
    "FullName": "graph",
    "select": [ "FullName", "ShortName" ]
  },
  "conference": {
    "type": "ConferenceInstance",
    "select": [ "FullName" ]
  },
  "field": {
    "type": "FieldOfStudy",
    "select": [ "Name" ],
    "return": { "Name": { "substring" : "World Wide Web" } }
  }
}
```

```JSON
{
  "path": "/author/PaperIDs/paper",
  "author": {
    "type": "Author",
    "select": [ "DisplayAuthorName" ],
    "match": { "Name": "leslie lamport" }
  },
  "paper": {
    "type": "Paper",
    "select": [ "OriginalTitle", "CitationCount" ],
    "return": { "CitationCount": { "gt": 100 } }
  }
}
```

