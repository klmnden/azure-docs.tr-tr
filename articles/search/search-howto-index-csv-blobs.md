---
title: CSV BLOB'lar Azure Search blob dizin oluşturucu ile dizin oluşturma | Microsoft Docs
description: CSV BLOB'lar ile Azure Search dizin öğrenin
author: chaosrealm
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 12/28/2017
ms.author: eugenesh
ms.openlocfilehash: bf65ab7858ba792418e325e7a025ee1bd88bbb27
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Dizin oluşturma CSV BLOB'lar ile Azure Search blob dizin oluşturucu
Varsayılan olarak, [Azure Search blob dizin oluşturucu](search-howto-indexing-azure-blob-storage.md) ayrıştırıyor sınırlandırılmış metin BLOB'ları tek bir metin öbek. Ancak, CSV verileri içeren BLOB'lar ile genellikle her satır ayrı bir belge olarak blob'daki kabul istersiniz. Örneğin, aşağıdaki sınırlandırılmış metin verildiğinde, size iki belgelere ayrıştırma isteyebilirsiniz her "id", "datePublished" ve "etiketler" alanları içeren: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Bu makalede, CSV BLOB'lar bir Azure Search blob dizin oluşturucu ile ayrıştırmak öğreneceksiniz. 

> [!IMPORTANT]
> Bu işlevsellik şu anda genel önizlemede ve üretim ortamlarında kullanılmamalıdır. Daha fazla bilgi için bkz: [REST API sürümü 2017-11-11-Önizleme =](search-api-2017-11-11-preview.md). 
> 

## <a name="setting-up-csv-indexing"></a>CSV Dizin oluşturmayı ayarlama
CSV BLOB'lar dizin, oluşturmak veya bir dizin oluşturucu tanımıyla güncelleştirmek için `delimitedText` ayrıştırma modu:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Oluşturma dizin oluşturucu API'si hakkında daha fazla ayrıntı için kullanıma [oluşturma dizin oluşturucu](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

`firstLineContainsHeaders` Her bir blob ilk (boş olmayan) satırının üstbilgileri içerdiğini gösterir.
BLOB'ları ilk başlık satırı içermiyorsa, üstbilgileri dizin oluşturucu yapılandırmasında belirtilen: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Ayırıcı karakter kullanarak özelleştirebileceğiniz `delimitedTextDelimiter` yapılandırma ayarı. Örneğin:

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Şu anda yalnızca UTF-8 kodlaması desteklenir. Diğer kodlamaları için destek gerekiyorsa, bunun için oy [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Mod ayrıştırma sınırlandırılmış metin kullandığınızda, Azure Search, veri kaynağındaki tüm BLOB'lar CSV olacağını varsayar. Aynı veri kaynağında bir karışımını CSV ve CSV olmayan BLOB desteklemeniz gerekiyorsa, lütfen için oy [UserVoice](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>İstek örnekleri
Bu tüm koyma birlikte tam yükü örnekler şunlardır. 

Veri kaynağı: 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Dizin Oluşturucu:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Azure Search iyileştirmemize yardımcı olun
Özellik istekleri veya fikir geliştirmeleri için varsa, giriş sağlama [UserVoice](https://feedback.azure.com/forums/263029-azure-search/).

