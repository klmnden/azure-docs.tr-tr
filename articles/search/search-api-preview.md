---
title: Azure arama 2017-11-11-Preview için - REST API Önizleme Azure arama
description: Azure arama hizmeti REST API Sürüm 2017-11-11-Preview eş anlamlı sözcükleri veya moreLikeThis aramaları gibi Deneysel özellikler içerir.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 06/28/2018
ms.author: HeidiSteen
ms.custom: seodec2018
ms.openlocfilehash: 524c1a6d083db02349c7dae9a0131228613dc170
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61127102"
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>Azure arama hizmeti REST API Sürüm 2017-11-11-Önizleme
Bu makalede `api-version=2017-11-11-Preview` Azure Search Hizmeti REST API, Deneysel özellikler değil henüz genel kullanıma sunan bir sürümü.

> [!NOTE]
> Önizleme özellikleri, test ve geri bildirim toplamak amacıyla bir deneme için kullanılabilir ve değiştirilebilir. Önizleme API'leri üretim uygulamalarında kullanılmasını öneriyoruz.


## <a name="new-in-2017-11-11-preview"></a>Yeni 2017-11-11-Önizleme

[**Otomatik Tamamlama** ](search-autocomplete-tutorial.md) birleştirir varolan [öneriler API'si](https://docs.microsoft.com/rest/api/searchservice/suggestions) yazarken tamamlanan tamamlayıcı karşılaştığında Arama çubuğuna eklenecek. Otomatik Tamamlama, bir kullanıcı bir sonraki arama için sorgu dizesi olarak seçebilir sorgu terimleriyle aday döndürür. Öneriler yanıt kısmi girişleri olarak gerçek belgeler döndürür: arama sonuçlarını hemen ve arama terimi girişi uzunluğu ve ayrıntısıyla büyüdükçe dinamik olarak değiştirebilirsiniz.

[**Bilişsel arama**](cognitive-search-concept-intro.md), yeni bir zenginleştirme özellik Azure Search metin olmayan kaynakları ve Azure Search'te tam metin aranabilir içeriğe dönüştürmek protokole metin görünmeyen bilgileri bulur. Aşağıdaki kaynaklar sunulan veya REST API Önizleme aşamasında değiştirdi. Genel kullanıma sunulan çağırın ya da Önizleme sürümü diğer tüm REST API'leri aynıdır.

+ [Beceri kümesi operations(api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/skillset-operations)

+ [Dizin Oluşturucu Oluşturma (API Sürüm 2017-11-11-Preview =)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

+ [Önceden tanımlanmış beceriler](cognitive-search-predefined-skills.md)

Diğer tüm REST API için api-version nasıl ayarlanacağını bağımsız olarak aynıdır. Örneğin, `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11-Preview` ve `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11` (olmadan `Preview`) işlevsel olarak eşdeğerdir.

## <a name="other-preview-features"></a>Diğer Önizleme özellikleri

Önceki önizlemelerde bildirilen özellikler hala genel Önizleme aşamasındadır. Bir önceki Önizleme api-version ile API arıyoruz ise bu sürümünü kullanın veya geçmek devam etmeden `2017-11-11-Preview` beklenen davranışın değişiklik yapmadan.

+ [CSV dosyaları Azure Blob dizin](search-howto-index-csv-blobs.md), içinde sunulan `api-version=2015-02-28-Preview`, bir önizleme özelliği olarak kalır. Bu özellik Azure Blob dizin oluşturma, bir parçasıdır ve bir parametre ayarı kullanılarak çağrılır. Bir CSV dosyasındaki her satır ayrı bir belge olarak dizine alınır.

+ [Azure Blob dizin oluşturma, JSON dizileri](search-howto-index-json-blobs.md), içinde sunulan `api-version=2015-02-28-Preview`, bir önizleme özelliği olarak kalır. Bu özellik Azure Blob dizin oluşturma, bir parçasıdır ve bir parametre ayarı kullanılarak çağrılır. Burada dizideki her öğe ayrı bir belge olarak dizine alınır.

+ [moreLikeThis sorgu parametresi](search-more-like-this.md) belirli bir belge için uygun olan belgeleri bulur. Bu özellik, önceki önizlemelerde olmuştur. 


## <a name="how-to-call-a-preview-api"></a>Bir önizleme API çağırma

Eski önizlemeler hala çalışır ancak zaman içinde eski haline gelir. Kodunuzu çağırırsa `api-version=2016-09-01-Preview` veya `api-version=2015-02-28-Preview`, bu çağrıları hala geçerli. Ancak, yalnızca en yeni önizleme sürümü ile geliştirmeleri yenilenir. 

Aşağıdaki örnek söz dizimini Önizleme API sürümü çağrıyı gösterir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

Azure arama hizmeti içinde birden çok sürümü kullanılabilir. Daha fazla bilgi için [API sürümlerini](search-api-versions.md).
