---
title: "CSV BLOB'lar Azure Search blob dizin oluşturucu ile dizin oluşturma | Microsoft Docs"
description: "CSV BLOB'lar ile Azure Search dizin öğrenin"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: ed3c9cff-1946-4af2-a05a-5e0b3d61eb25
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 12/28/2017
ms.author: eugenesh
ms.openlocfilehash: 40b7f1f4f75d389a64329e7d8fd3c7feb79d5e55
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Dizin oluşturma CSV BLOB'lar ile Azure Search blob dizin oluşturucu
Varsayılan olarak, [Azure Search blob dizin oluşturucu](search-howto-indexing-azure-blob-storage.md) ayrıştırıyor sınırlandırılmış metin BLOB'ları tek bir metin öbek. Ancak, CSV verileri içeren BLOB'lar ile genellikle her satır ayrı bir belge olarak blob'daki kabul istersiniz. Örneğin, aşağıdaki sınırlandırılmış metin verilen: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

2 belgelere ayrıştırma isteyebilirsiniz her "id", "datePublished" ve "etiketler" alanları içeren.

Bu makalede, CSV BLOB'lar bir Azure Search blob dizin oluşturucu ile ayrıştırmak öğreneceksiniz. 

> [!IMPORTANT]
> Bu işlev şu anda önizleme aşamasındadır. Yalnızca sürüm kullanarak REST API içinde kullanılabilir **2015-02-28-Önizleme**. Lütfen unutmayın, Önizleme API'leri sınama ve değerlendirme için tasarlanmıştır ve üretim ortamlarında kullanılmamalıdır. 
> 
> 

## <a name="setting-up-csv-indexing"></a>CSV Dizin oluşturmayı ayarlama
CSV BLOB'lar dizin, oluşturmak veya bir dizin oluşturucu tanımıyla güncelleştirmek için `delimitedText` ayrıştırma modu:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Oluşturma dizin oluşturucu API'si hakkında daha fazla ayrıntı için kullanıma [oluşturma dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

`firstLineContainsHeaders`Her bir blob ilk (boş olmayan) satırının üstbilgileri içerdiğini gösterir.
BLOB'ları ilk başlık satırı içermiyorsa, üstbilgileri dizin oluşturucu yapılandırmasında belirtilen: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Ayırıcı karakter kullanarak özelleştirebileceğiniz `delimitedTextDelimiter` yapılandırma ayarı. Örneğin:

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Şu anda yalnızca UTF-8 kodlaması desteklenir. Diğer kodlamaları için destek gerekiyorsa, lütfen bize bilmeniz [UserVoice sitemizi](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Mod ayrıştırma sınırlandırılmış metin kullandığınızda, Azure Search, veri kaynağındaki tüm BLOB'lar CSV olacağını varsayar. Aynı veri kaynağında bir karışımını CSV ve CSV olmayan BLOB desteklemeniz gerekiyorsa, lütfen bize bilmeniz [UserVoice sitemizi](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>İstek örnekleri
Bu tüm koyma birlikte tam yükü örnekler şunlardır. 

Veri kaynağı: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Dizin Oluşturucu:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Azure Search iyileştirmemize yardımcı olun
Özellik istekleri veya fikir geliştirmeleri için varsa, lütfen bize üzerinde ulaşmak bizim [UserVoice sitesinde](https://feedback.azure.com/forums/263029-azure-search/).

