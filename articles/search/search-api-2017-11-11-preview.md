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
ms.openlocfilehash: afcc8ac31c45c1a43d3d759ef6f5a1cee8166c0a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>Azure Search Hizmeti REST API'si: Sürüm 2017-11-11-Önizleme
Bu makalede `api-version=2017-11-11-Preview` Azure Search Hizmeti REST API'si, henüz genellikle kullanılamıyor Deneysel özellikleri sunan sürümü.

> [!NOTE]
> Önizleme özellikleri, test ve deneme geri bildirimi toplama amacı ile kullanılabilir ve değiştirilebilir. Önizleme API'leri üretim uygulamalarında kullanılmamasını öneriyoruz.


## <a name="new-in-this-preview"></a>Bu Önizleme'de yeni

+ [Bilişsel arama](cognitive-search-concept-intro.md), yeni bir iyileştirmesini Özelliği Azure Search'te metin olmayan kaynakları ve Azure Search'te bir tam metin aranabilir içeriğe dönüştürme protokole metin görünmeyen bilgileri bulur.

Aşağıdaki iki işlem sunulan veya REST API önizlemede değiştirilemiyor. Genel olarak kullanılabilir çağırmak ya da sürüm (örneğin,. Önizleme diğer tüm REST API'leri aynıdır

+ [Skillset oluşturun (API sürümü 2017-11-11-Önizleme =)](ref-create-skillset.md)

+ [Dizin Oluşturucu yapın (API sürümü 2017-11-11-Önizleme =)](ref-create-indexer.md)

Genel olarak kullanılabilir çağırmak ya da sürüm Önizleme diğer tüm REST API'leri aynıdır. Örneğin, `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11-Preview` ve `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11` (olmadan `Preview`) işlevsel olarak eşdeğerdir.

## <a name="other-preview-features"></a>Diğer Önizleme özellikleri

Önceki önizlemeler özelliklerinden hala genel önizlemede.

+ [Azure Blob dizini oluşturma CSV dosyaları](search-howto-index-csv-blobs.md), CSV dosyasındaki her satır ayrı bir belge olarak burada dizinlenir.

+ [Azure Blob dizini oluşturma, JSON dizileri](search-howto-index-json-blobs.md), dizideki her öğe ayrı bir belge olarak burada dizinlenir.

+ [`moreLikeThis` sorgu parametresi](search-more-like-this.md) belirli bir belge için uygun olan belgeleri bulmak için. Bu özellik, önceki önizlemelerde olmuştur. Bu API önceki bir API sürümü ile arıyoruz varsa, bu sürümü kullanmaya devam edebilirsiniz.


## <a name="how-to-call-a-preview-api"></a>Önizleme API çağrısı yapma

Eski önizlemeleri hala çalışır ancak zamanla eski haline gelir. Kodunuzu çağırırsa `api-version=2016-09-01-Preview` veya `api-version=2015-02-25-Preview`, bu çağrıları hala geçerli. Ancak, yalnızca en yeni önizleme sürümü geliştirmelerle yenilenir. 

Aşağıdaki örnek sözdizimi Önizleme API sürümü için bir çağrı gösterilmektedir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

Azure Search hizmeti birden çok sürümlerinde kullanılabilir. Daha fazla bilgi için bkz: [API sürümleri](search-api-versions.md).
