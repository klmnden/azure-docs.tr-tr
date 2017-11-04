---
title: "Dizin oluşturma JSON BLOB'ları ile Azure Search blob dizin oluşturucu"
description: "Dizin oluşturma JSON BLOB'ları ile Azure Search blob dizin oluşturucu"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 57e32e51-9286-46da-9d59-31884650ba99
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 09/07/2017
ms.author: eugenesh
ms.openlocfilehash: bf4d3a517e1308a142d21cffff64f3c6e104eb62
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Dizin oluşturma JSON BLOB'ları ile Azure Search blob dizin oluşturucu
Bu makalede, JSON içeren bloblarından yapılandırılmış içeriği ayıklamak için Azure Search dizin oluşturucusunu blob yapılandırma gösterilmektedir.

## <a name="scenarios"></a>Senaryolar
Varsayılan olarak, [Azure Search blob dizin oluşturucu](search-howto-indexing-azure-blob-storage.md) JSON BLOB'ları tek bir metin öbek ayrıştırır. Genellikle, JSON belgelerinin yapısını korumak istiyorsunuz. Örneğin, JSON belgesini verilen

    {
        "article" : {
             "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

"metin", "datePublished" ve "etiketler" alanları ile bir Azure Search belgesine ayrıştırma isteyebilirsiniz.

Alternatif olarak, ne zaman, BLOB'ları içeren bir **dizi JSON nesnesi**, her öğe ayrı bir Azure Search belge olmasını dizinin isteyebilirsiniz. Örneğin, bir blob bu JSON ile verilen:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Azure Search dizininizi üç ayrı belgelerle, her "id" ve "metin" alanları ile doldurabilirsiniz.

> [!IMPORTANT]
> İşlevselliği ayrıştırma JSON dizisi şu anda önizlemede değil. Yalnızca sürüm kullanarak REST API içinde kullanılabilir **2016-09-01-Önizleme**. Unutmayın, Önizleme API'leri sınama ve değerlendirme için tasarlanmıştır ve üretim ortamlarında kullanılmamalıdır.
>
>

## <a name="setting-up-json-indexing"></a>JSON dizin oluşturmayı ayarlama
JSON BLOB'ları dizin oluşturma, normal belge ayıklama benzer. Tam olarak normal olarak ilk olarak, veri kaynağı oluşturun: 

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Zaten yoksa, hedef arama dizini oluşturun. 

Son olarak bir dizin oluşturucu oluşturma ve ayarlama `parsingMode` parametresi `json` (tek bir belge olarak her bir blob dizini oluşturmak için) veya `jsonArray` (bloblarınızın JSON dizileri içerir ve ayrı bir belge olarak kabul edilmesi için bir dizinin her bir öğesine ihtiyacınız varsa):

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Gerekirse, kullanın **alan eşlemelerini** sonraki bölümde gösterildiği gibi hedef search dizininizi doldurmak için kullanılan kaynak JSON belgesi özelliklerini seçmek için.

> [!IMPORTANT]
> Kullandığınızda `json` veya `jsonArray` modu ayrıştırma, Azure Search, veri kaynağındaki tüm BLOB'lar JSON içeren varsayar. Üzerinde bir karışımını JSON ve JSON olmayan BLOB aynı veri kaynağına desteklemeniz gerekiyorsa,'ın bize bildirin [UserVoice sitemizi](https://feedback.azure.com/forums/263029-azure-search).
>
>

## <a name="using-field-mappings-to-build-search-documents"></a>Search belgeleri oluşturmak için alan eşlemelerini kullanma
Yalnızca ilkel veri türleri, dize dizileri ve GeoJSON noktaları desteklediğinden, şu anda, Azure Search rastgele JSON belgelerinin doğrudan dizin oluşturulamıyor. Ancak, kullanabileceğiniz **alan eşlemelerini** JSON belgenizi bölümlerini seçin ve "bunları arama belge üst düzey alanlarına Yükselt". Alan eşlemeleri temel kavramları hakkında bilgi edinmek için [Azure Search dizin oluşturucu alan eşlemelerini köprü veri kaynakları ve arama dizinlerini arasındaki farkları](search-indexer-field-mappings.md).

Bizim örnek JSON belgesi gelmeye:

    {
        "article" : {
             "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Bir arama dizinini aşağıdaki alanlarla sahip varsayalım: `text` türü `Edm.String`, `date` türü `Edm.DateTimeOffset`, ve `tags` türü `Collection(Edm.String)`. İstenen şekle, JSON eşlemek için aşağıdaki alan eşlemelerini kullanın:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

Eşlemeleri kaynak alan adlarını kullanarak belirtilen [JSON işaretçi](http://tools.ietf.org/html/rfc6901) gösterimi. JSON belgesi kökündeki başvurmak için eğik çizgiyle başlayın, ardından İleri eğik ayrılmış yolu kullanarak ve istenen özelliğine (iç içe geçme düzeyi rastgele) seçin.

Sıfır tabanlı dizini kullanılarak tek tek dizi öğeleri de başvurabilir. Örneğin, yukarıdaki örnekteki "etiketler" dizinin ilk öğesi seçmek için bu gibi bir alan eşleme kullanın:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Bir kaynak alan adı bir alan eşleme yolunda JSON içinde mevcut olmayan bir özelliğe başvuruyor, bu eşlemeyi hatasız atlanır. Bu, böylece (ortak kullanım örneği olan) farklı bir şema belgelerle destekliyoruz gerçekleştirilir. Hiçbir doğrulama olduğundan ilgilenebilmek alan eşleme belirtimi yazım hatalarını önlemek için gerekir.
>
>

JSON belgeleri yalnızca basit en üst düzey özellikler içeriyorsa, alan eşlemelerini hiç gerekmeyebilir. Örneğin, JSON gibi görünüyorsa, üst düzey özellikleri "metin", "datePublished" ve "etiketler" doğrudan eşler arama dizini karşılık gelen alanlara:

    {
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13"
       "tags" : [ "search", "storage", "howto" ]    
     }

Alan eşlemelerini içeren bir tam dizin oluşturucu yükü şöyledir:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }

## <a name="indexing-nested-json-arrays"></a>İç içe JSON diziler dizin oluşturma
Ne JSON nesnelerinin bir dizisi, ancak bu dizi dizini oluşturmak istediğiniz yere belge içinde iç içe yerleştirilmiş? Hangi özelliği kullanarak dizi içerir çekme `documentRoot` Yapılandırma özelliği. Örneğin, BLOB'ları şöyle görünür:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

içinde yer alan dizi dizini oluşturmak için bu yapılandırmayı kullanmak `level2` özelliği:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

## <a name="help-us-make-azure-search-better"></a>Azure Search iyileştirmemize yardımcı olun
Özellik istekleri veya fikir geliştirmeleri için varsa, bize üzerinde ulaşmak bizim [UserVoice sitesinde](https://feedback.azure.com/forums/263029-azure-search/).
