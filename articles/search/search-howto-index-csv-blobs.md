---
title: Azure Search blob dizin oluşturucu ile CSV bloblarını dizine ekleme | Microsoft Docs
description: Azure Search ile CSV bloblarını dizine öğrenin
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.openlocfilehash: b1f97b5e9542e32096bb060bce40e7b9620d0f49
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406084"
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Azure Search blob dizin oluşturucu ile CSV bloblarını dizine ekleme
Varsayılan olarak, [Azure Search blob dizin oluşturucu](search-howto-indexing-azure-blob-storage.md) ayrıştırıyor sınırlandırılmış metin BLOB'ları tek bir metin parçası. Ancak, CSV verileri içeren BLOB'ları ile genellikle her satır ayrı bir belge olarak blob işlemesi gerektiğini istersiniz. Örneğin, aşağıdaki sınırlandırılmış metin göz önünde bulundurulduğunda, iki belgelere ayrıştırmak isteyebilirsiniz "id", "datePublished" ve "tags" alanlar içeren her: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Bu makalede, bir Azure Search blob dizin oluşturucu ile CSV bloblarını ayrıştırma öğreneceksiniz. 

> [!IMPORTANT]
> Bu işlevsellik şu anda genel Önizleme aşamasındadır ve üretim ortamlarında kullanılmamalıdır. Daha fazla bilgi için [REST API Sürüm 2017-11-11-Preview =](search-api-2017-11-11-preview.md). 
> 

## <a name="setting-up-csv-indexing"></a>CSV Dizin oluşturma ayarlama
CSV bloblarını dizine oluşturun veya bir dizin oluşturucu tanımı ile güncelleştirmek için `delimitedText` ayrıştırma modu:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Dizin Oluşturucu Oluşturma API'si hakkında daha fazla ayrıntı için kullanıma [dizin oluşturucu oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

`firstLineContainsHeaders` Her blobun ilk (boş olmayan) satır üst bilgileri içerdiğini gösterir.
Blobları ilk üst bilgi satırı içermiyorsa, üst bilgiler dizin oluşturucu yapılandırmasında belirtilen: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Sınırlayıcı karakter kullanarak özelleştirebileceğiniz `delimitedTextDelimiter` yapılandırma ayarı. Örneğin:

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Şu anda yalnızca UTF-8 kodlaması desteklenir. Diğer kodlamaları için destek gerekiyorsa, bunun için oy [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Ayrıştırma modu sınırlandırılmış metin kullandığınızda, Azure Search veri kaynağındaki tüm blobları CSV olacağını varsayar. CSV ve CSV olmayan bloblar bir karışımını aynı veri kaynağında desteklemeniz gerekiyorsa, lütfen için oy verin [UserVoice](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>Örnek istek
Bu tüm koyarak birlikte eksiksiz yükü örnekler aşağıdadır. 

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
Özellik istekleri veya geliştirmeleri için fikriniz varsa, girdi sağlayın [UserVoice](https://feedback.azure.com/forums/263029-azure-search/).

