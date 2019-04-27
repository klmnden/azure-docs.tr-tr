---
title: Azure Blob dizin oluşturucu için tam metin arama - Azure Search dizini belgelerden dizini BLOB'ları içeren birden çok arama
description: Azure Search Blob Dizin Oluşturucu kullanarak metin içeriği için Azure BLOB'ları gezinin. Her blob bir veya daha fazla Azure Search dizini belge içerebilir.
ms.date: 02/12/2019
author: arv100kri
manager: briansmi
ms.author: arjagann
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seofeb2018
ms.openlocfilehash: e95eff015340659b642dff800a03f615e22c1577
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60871208"
---
# <a name="indexing-blobs-producing-multiple-search-documents"></a>Birden çok arama belge üretme bloblarını dizine ekleme
Varsayılan olarak, bir blob dizin oluşturucu, bir blobun içeriklerini tek arama belgesi olarak değerlendirir. Belirli **parsingMode** değerleri tek tek bir blob birden çok arama belgeleri neden olduğu senaryoları destekler. Farklı türde **parsingMode** izin veren bir blobdan bir arama belge birden çok bir ayıklamak için dizin oluşturucu şunlardır:
+ `delimitedText`
+ `jsonArray`
+ `jsonLines`

> [!IMPORTANT]
> `jsonLines` Ayrıştırma modu, genel Önizleme aşamasındadır ve üretim ortamlarında kullanılmamalıdır. Daha fazla bilgi için [REST API Sürüm 2017-11-11-Preview =](search-api-2017-11-11-preview.md). 

## <a name="one-to-many-document-key"></a>Bire çok belge anahtarı
Azure Search dizini içinde gösterilir her belge benzersiz bir belge anahtar tarafından tanımlanır. 

Ayrıştırma modu yok belirtildiğinde ve hiçbir açık temel alan Azure Search dizini için otomatik olarak eşlemesi varsa [eşler](search-indexer-field-mappings.md) `metadata_storage_path` anahtar özelliği. Bu eşleme, her blobun benzersiz arama belgesi olarak analizinde görünmesini sağlar.

Yukarıda listelenen ayrıştırma modun herhangi birinde kullanırken, bir blob yalnızca blob meta verilerini uygun olmayan temel bir belge anahtarını yapma "many" arama belgeleri eşlenir. Bu kısıtlamayı çözmek için Azure Search bir blobun ayıklanan tek tek her varlık için bir "bir-çok" belge anahtarını oluşturma yeteneğine sahiptir. Bu özellik adlı `AzureSearch_DocumentKey` ve blob ayıklanan tek tek her varlık eklenir. Bu özelliğin değeri her bir varlık için benzersiz olması garanti _bloblar arasında_ ve varlıklar ayrı arama belgeleri olarak görüntülenir.

Anahtar dizini alan için açık alan eşlemeleri belirtilmediğinde, varsayılan olarak `AzureSearch_DocumentKey` kullanarak, eşlenmiş `base64Encode` alan eşlemesi işlevi.

## <a name="example"></a>Örnek
Bir dizin tanımı şu alanlara sahip olduğunuz varsayılmaktadır:
+ `id`
+ `temperature`
+ `pressure`
+ `timestamp`

Ve aşağıdaki yapıya sahip BLOB'lar, blob kapsayıcı vardır:

_Blob1.json_

    { "temperature": 100, "pressure": 100, "timestamp": "2019-02-13T00:00:00Z" }
    { "temperature" : 33, "pressure" : 30, "timestamp": "2019-02-14T00:00:00Z" }

_Blob2.json_

    { "temperature": 1, "pressure": 1, "timestamp": "2018-01-12T00:00:00Z" }
    { "temperature" : 120, "pressure" : 3, "timestamp": "2013-05-11T00:00:00Z" }

Bir dizin oluşturucu oluşturduğunuzda ve **parsingMode** için `jsonLines` - herhangi bir açık eşlemeleri temel alan aşağıdaki eşleme için örtük olarak uygulanacak alan belirtmeden
    
    {
        "sourceFieldName" : "AzureSearch_DocumentKey",
        "targetFieldName": "id",
        "mappingFunction": { "name" : "base64Encode" }
    }

Bu kurulum (base64 kodlu kimliği uzatmamak için kısaltılmış) aşağıdaki bilgileri içeren bir Azure Search dizini neden olur

| id | sıcaklık | basınç | timestamp |
|----|-------------|----------|-----------|
| aHR0... YjEuanNvbjsx | 100 | 100 | 2019-02-13T00:00:00Z |
| aHR0... YjEuanNvbjsy | 33 | 30 | 2019-02-14T00:00:00Z |
| aHR0... YjIuanNvbjsx | 1 | 1 | 2018-01-12T00:00:00Z |
| aHR0... YjIuanNvbjsy | 120 | 3 | 2013-05-11T00:00:00Z |

## <a name="custom-field-mapping-for-index-key-field"></a>Özel alan eşlemesi için dizin anahtar alanı

Önceki örnek olarak aynı dizin tanımını varsayıldığında, BLOB'ları aşağıdaki yapıya sahip blob kapsayıcınızın olduğunu düşünelim:

_Blob1.json_

    recordid, temperature, pressure, timestamp
    1, 100, 100,"2019-02-13T00:00:00Z" 
    2, 33, 30,"2019-02-14T00:00:00Z" 

_Blob2.json_

    recordid, temperature, pressure, timestamp
    1, 1, 1,"2018-01-12T00:00:00Z" 
    2, 120, 3,"2013-05-11T00:00:00Z" 

Bir dizin oluşturucu ile oluşturduğunuzda `delimitedText` **parsingMode**, anahtar alanının alan eşlemesi işlevine aşağıdaki şekilde ayarlamak için doğal ilerlediğini düşünebilirsiniz:

    {
        "sourceFieldName" : "recordid",
        "targetFieldName": "id"
    }

Ancak, bu eşlemeyi olacak _değil_ neden 4 belge dizinde görünmeye çünkü `recordid` alan benzersiz değil _bloblar arasında_. Yapmanızı öneririz bu nedenle, uygulamayı örtük alan eşlemeyi kullanım `AzureSearch_DocumentKey` "bir-çok" ayrıştırma modları için anahtar dizini alanına özelliği.

Bir açık alan eşlemesini ayarlamak istiyorsanız, emin _sourceField_ tek tek her varlık için farklıdır **tüm BLOB'ları arasında**.

> [!NOTE]
> Tarafından kullanılan yaklaşım `AzureSearch_DocumentKey` sağlama ayıklanan varlık başına benzersiz olup değişikliğe tabidir ve bu nedenle, uygulamanızın ihtiyaçları için değerinin güvenmemelisiniz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search'te dizin oluşturucular](search-indexer-overview.md)
+ [Azure arama ile Azure Blob Depolama dizini oluşturma](search-howto-index-json-blobs.md)
+ [Azure Search blob dizin oluşturucu ile CSV bloblarını dizine ekleme](search-howto-index-csv-blobs.md)
+ [Azure Search blob dizin oluşturucu ile JSON bloblarını dizine ekleme](search-howto-index-csv-blobs.md)

## <a name="NextSteps"></a>Sonraki adımlar
* Azure arama hakkında daha fazla bilgi edinmek için [arama hizmeti sayfasını](https://azure.microsoft.com/services/search/).