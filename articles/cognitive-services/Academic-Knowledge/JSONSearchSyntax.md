---
title: JSON arama söz dizimi - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si kullanabileceğiniz JSON arama söz dizimi hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: fddd2291fe7fbb46c57d31e9aebc7fc6244df971
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61336975"
---
# <a name="json-search-syntax"></a>JSON Arama Söz Dizimi

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

Bir sorgu yolda düğüm adları (_v0, v1,..._ ) sorgu nesnesinde; başvurulabilir düğüm tanımlayıcıları görür edge adları (_e0, e1,..._ ) yolu ilgili kenarları türlerini temsil eder. Bir yıldız işareti kullanabiliriz _*_ (hariç, verilmesi gereken başlangıç düğümü) bir düğüm veya kenar adı olarak bildirmek için vardır hiçbir kısıtlamaları bu tür bir öğe. Örneğin, bir sorgu yolu `/v0/*/v1/e1/*/` edge türünü kısıtlamadan grafikten yolları alır _(v0, v1)_. Bu arada, sorguya yolun (son düğüm) hedef kısıtlamaları ya da yok.

Bir yolu, tek bir düğüm içeriyorsa, söyleyin _v0_, sorgu sadece kısıtlamalar karşılayan tüm varlıkları döndürür. Başlangıç düğüme uygulanan bir kısıtlama nesnesi olarak adlandırılan bir *Başlangıç sorgu nesnesi*, olan belirtimi aşağıda verilmiştir.

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

Sorgu işlemcisi, bir yol yalnızca bir başlangıç düğümü içeriyorsa, belirtilen yol deseni izleyen bir graf geçişi gerçekleştirir. Bir düğümde geldiğinde, kullanıcı tarafından belirtilen geçişi eylemlerini, diğer bir deyişle, tetiklenip geçerli düğümde durdurup dönmek için veya graph incelemeye devam edin. Varsayılan Eylemler, herhangi bir çapraz geçiş eylemi belirtildiğinde alınır. Ara bir düğüm için grafik keşfetmeye devam etmek için varsayılan eylem vardır. Son bir yol için varsayılan eylem durdurup dönmek için düğümüdür. Geçişi Eylemler belirten bir kısıtlama nesnesi olarak adlandırılan bir *geçişi eylem nesnesi*. Kendi belirtimi aşağıda verilmiştir:

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

POST gövdesini bir *json* arama sorgusu bulunması gereken en az bir *yolu* deseni. Çapraz eylem nesneleri isteğe bağlıdır. İki örnek aşağıda verilmiştir.

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

