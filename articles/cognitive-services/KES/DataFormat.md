---
title: Bilgi Bankası araştırması hizmeti API'si veri biçiminde | Microsoft Docs
description: İçinde bilgi araştırması hizmet (KES) API Bilişsel Hizmetleri'nde veri biçimi hakkında bilgi edinin.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: a763505ac6458d68df74ae73e71029b81202ec8b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351478"
---
# <a name="data-format"></a>Veri biçimi
Veri dosyası dizin nesneleri listesi açıklar.
Dosya her satırda bir nesnenin öznitelik değerleri belirtir [JSON biçimine](http://json.org/) UTF-8 kodlaması ile.
Tanımlanan özniteliklerin yanı sıra [şema](SchemaFormat.md), her nesnenin nesneleri arasındaki göreli günlük olasılık belirten isteğe bağlı "logprob" özniteliği vardır.
Hizmet nesneleri olasılık azalan sırada geri döndüğünde, biz "logprob" nesneleri eşleşen dönüş sırasını belirtmek için kullanabilirsiniz.
Bir olasılık verilen *p* 0 ile 1 arasında karşılık gelen günlük olasılık günlük hesaplanabilir (*p*), log() doğal günlük işlevini olduğu.
Değer için logprob belirtilmezse, varsayılan değer 0 kullanılır.

```json
{"logprob":-5.3, "Title":"latent dirichlet allocation", "Year":2003, "Author":{"Name":"david m blei", "Affiliation":"uc berkeley"}, "Author":{"Name":"andrew y ng", "Affiliation":"stanford"}, "Author":{"Name":"michael i jordan", "Affiliation":"uc berkeley"}}
{"logprob":-6.1, "Title":"probabilistic latent semantic indexing", "Year":1999, "Author":{"Name":"thomas hofmann", "Affiliation":"uc berkeley"}}
...
```

Dize, GUID ve Blob öznitelikleri için değer tırnak içine alınmış bir JSON dizesi olarak temsil edilir.  (Int32, Int64, çift) sayısal öznitelikler için değeri JSON sayı olarak temsil edilir.  Bileşik öznitelikleri için değer alt öznitelik değerleri belirten bir JSON nesnesidir.  Daha hızlı dizin derlemeler için günlük olasılık azaltarak nesneleri presort.

Genel olarak, bir öznitelik 0 veya daha fazla değer olabilir.  Bir öznitelik değeri yoksa, biz yalnızca JSON öğesinden kaldırın.  Bir öznitelik 2 veya daha fazla değerlere sahipse, biz JSON nesnesinde öznitelik değer çifti yineleyebilirsiniz.  Alternatif olarak, biz öznitelik için birden çok değer içeren bir JSON dizisi atayabilirsiniz.

```json
{"logprob":0, "Title":"0 keyword"}
{"logprob":0, "Title":"1 keyword", "Keyword":"foo"}
{"logprob":0, "Title":"2 keywords", "Keyword":"foo", "Keyword":"bar"}
{"logprob":0, "Title":"2 keywords (alt)", "Keyword":["foo", "bar"]}
```
