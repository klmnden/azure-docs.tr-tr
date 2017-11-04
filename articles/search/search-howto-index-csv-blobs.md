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
ms.date: 12/15/2016
ms.author: eugenesh
ms.openlocfilehash: 60ca696a6fa8f277a13875c39b44577c4b38c92a
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Dizin oluşturma CSV BLOB'lar ile Azure Search blob dizin oluşturucu
Varsayılan olarak, [Azure Search blob dizin oluşturucu](search-howto-indexing-azure-blob-storage.md) ayrıştırıyor sınırlandırılmış metin BLOB'ları tek bir metin öbek. Ancak, CSV verileri içeren BLOB'lar ile genellikle her satır ayrı bir belge olarak blob'daki kabul istersiniz. Örneğin, aşağıdaki sınırlandırılmış metin verilen: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

2 belgelere ayrıştırma isteyebilirsiniz her "id", "datePublished" ve "etiketler" alanları içeren.

Bu makalede, CSV BLOB'lar bir Azure Search blob dizin oluşturucu ile ayrıştırmak öğreneceksiniz. 

> [!IMPORTANT]
> Bu işlevsellik şu anda önizlemede değil. Yalnızca sürüm kullanarak REST API içinde kullanılabilir **2015-02-28-Önizleme**. Lütfen unutmayın, Önizleme API'leri sınama ve değerlendirme için tasarlanmıştır ve üretim ortamlarında kullanılmamalıdır. 
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

Şu anda yalnızca UTF-8 kodlaması desteklenir. Ayrıca, yalnızca virgülle `','` karakter ayırıcısı desteklenir. Diğer Kodlamalar veya sınırlayıcıları için destek gerekiyorsa, lütfen bize bilmeniz [UserVoice sitemizi](https://feedback.azure.com/forums/263029-azure-search).

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

