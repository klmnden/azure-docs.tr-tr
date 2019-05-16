---
title: Azure Search Blob dizin oluşturucu - Azure Search ile dizin CSV BLOB'ları
description: CSV blobları Azure Search dizini kullanarak tam metin araması için Azure Blob Depolama alanında gezinin. Dizin oluşturucular veri alımı Azure Blob Depolama gibi seçili veri kaynakları için otomatik hale getirin.
ms.date: 05/02/2019
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: e7d959e77d27fb04b18f402e4056d4dea1607039
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65522909"
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Azure Search blob dizin oluşturucu ile CSV bloblarını dizine ekleme

> [!Note]
> Ayrıştırma modu delimitedText olduğunu Önizleme ve amaçlayan üretim kullanımı için değildir. [2019-05-06-Önizleme REST API sürümü](search-api-preview.md) bu özelliği sağlar. .NET SDK'sı desteği şu anda yoktur.
>

Varsayılan olarak, [Azure Search blob dizin oluşturucu](search-howto-indexing-azure-blob-storage.md) ayrıştırıyor sınırlandırılmış metin BLOB'ları tek bir metin parçası. Ancak, CSV verileri içeren BLOB'ları ile genellikle her satır ayrı bir belge olarak blob işlemesi gerektiğini istersiniz. Örneğin, aşağıdaki sınırlandırılmış metin göz önünde bulundurulduğunda, iki belgelere ayrıştırmak isteyebilirsiniz "id", "datePublished" ve "tags" alanlar içeren her: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Bu makalede, bir Azure Search blob indexerby ayarıyla CSV bloblarını ayrıştırmayı öğreneceksiniz `delimitedText` ayrıştırma modu. 

> [!NOTE]
> Dizin Oluşturucu yapılandırma önerileri izleyin [bire çok dizin](search-howto-index-one-to-many-blobs.md) birden çok arama belgeden bir Azure blob çıktı olarak.

## <a name="setting-up-csv-indexing"></a>CSV Dizin oluşturma ayarlama
CSV bloblarını dizine oluşturun veya bir dizin oluşturucu tanımı ile güncelleştirmek için `delimitedText` üzerinde modu ayrıştırılırken bir [dizin oluşturucu oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer) isteği:

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

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

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Dizin Oluşturucu:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06-Preview
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

