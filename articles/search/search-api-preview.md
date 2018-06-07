---
title: Azure Search Hizmeti REST API sürümü 2017-11-11-Önizleme | Microsoft Docs
description: Azure Search Hizmeti REST API sürümü 2017-11-11-Önizleme eş anlamlıları ve moreLikeThis aramaları gibi Deneysel özellikler içerir.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/17/2018
ms.author: HeidiSteen
ms.openlocfilehash: ce5771777762414a0229cf83425c2f3601cb979a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655817"
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>Azure Search Hizmeti REST API sürümü 2017-11-11-Önizleme
Bu makalede `api-version=2017-11-11-Preview` Azure Search Hizmeti REST API'si, henüz genellikle kullanılamıyor Deneysel özellikleri sunan sürümü.

> [!NOTE]
> Önizleme özellikleri, test ve deneme geri bildirimi toplama amacı ile kullanılabilir ve değiştirilebilir. Önizleme API'leri üretim uygulamalarında kullanılmamasını öneriyoruz.


## <a name="new-in-2017-11-11-preview"></a>Yeni 2017-11-11-Önizleme

[Bilişsel arama](cognitive-search-concept-intro.md), yeni bir iyileştirmesini Özelliği Azure Search'te metin olmayan kaynakları ve Azure Search'te bir tam metin aranabilir içeriğe dönüştürme protokole metin görünmeyen bilgileri bulur.

Aşağıdaki reources sunulan veya REST API önizlemede değiştirilemiyor. Genel olarak kullanılabilir çağırmak ya da sürüm Önizleme diğer tüm REST API'leri aynıdır.

+ [Skillset oluşturun (API sürümü 2017-11-11-Önizleme =)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)

+ [Dizin Oluşturucu yapın (API sürümü 2017-11-11-Önizleme =)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

+ [Önceden tanımlanmış yetenekleri](cognitive-search-predefined-skills.md)

Diğer tüm REST API'leri api-version nasıl ayarlanacağını bağımsız olarak aynıdır. Örneğin, `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11-Preview` ve `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11` (olmadan `Preview`) işlevsel olarak eşdeğerdir.

## <a name="other-preview-features"></a>Diğer Önizleme özellikleri

Önceki önizlemelerde duyurdu hala genel önizlemede özellikleridir. Bir önceki Önizleme API sürümü ile bir API arıyoruz varsa, bu sürümünü kullanın veya geçmek sürdürebilirsiniz `2017-11-11-Preview` hiçbir değişiklik beklenen davranıştır.

+ [Azure Blob dizini oluşturma CSV dosyaları](search-howto-index-csv-blobs.md), içinde sunulan `api-version=2015-02-28-Preview`, bir önizleme özelliği kalır. Bu özellik, Azure Blob dizini oluşturma bir parçası olan ve bir parametre ayarı kullanılarak çağrılır. Bir CSV dosyasındaki her satır ayrı bir belge olarak dizine alınır.

+ [Azure Blob dizini oluşturma, JSON dizileri](search-howto-index-json-blobs.md), içinde sunulan `api-version=2015-02-28-Preview`, bir önizleme özelliği kalır. Bu özellik, Azure Blob dizini oluşturma bir parçası olan ve bir parametre ayarı kullanılarak çağrılır. Burada dizideki her öğe ayrı bir belge olarak dizine alınır.

+ [moreLikeThis sorgu parametresi](search-more-like-this.md) belirli bir belgeye ilgili belgeleri bulur. Bu özellik, önceki önizlemelerde olmuştur. 


## <a name="how-to-call-a-preview-api"></a>Önizleme API çağrısı yapma

Eski önizlemeleri hala çalışır ancak zamanla eski haline gelir. Kodunuzu çağırırsa `api-version=2016-09-01-Preview` veya `api-version=2015-02-28-Preview`, bu çağrıları hala geçerli. Ancak, yalnızca en yeni önizleme sürümü geliştirmelerle yenilenir. 

Aşağıdaki örnek sözdizimi Önizleme API sürümü için bir çağrı gösterilmektedir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

Azure Search hizmeti birden çok sürümlerinde kullanılabilir. Daha fazla bilgi için bkz: [API sürümleri](search-api-versions.md).
