---
title: Azure arama 2019-05-06-Preview için - REST API Önizleme Azure arama
description: Azure arama hizmeti REST API sürümü 2019-05-06-Preview bilgi deposu ve müşteri tarafından yönetilen bir şifreleme anahtarları gibi Deneysel özellikler içerir.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/02/2019
ms.author: HeidiSteen
ms.custom: seodec2018
ms.openlocfilehash: 5374ff896613dd8f8563a2054be8a92103e63fbb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65523916"
---
# <a name="azure-search-service-rest-api-version-2019-05-06-preview"></a>Azure arama hizmeti REST API sürümü 2019-05-06-Önizleme
Bu makalede `api-version=2019-05-06-Preview` Azure Search Hizmeti REST API, Deneysel özellikler değil henüz genel kullanıma sunan bir sürümü.

> [!NOTE]
> Önizleme özellikleri, test ve geri bildirim toplamak amacıyla bir deneme için kullanılabilir ve değiştirilebilir. Önizleme API'leri üretim uygulamalarında kullanılmasını öneriyoruz.


## <a name="new-in-2019-05-06-preview"></a>Yeni 2019-05-06-Önizleme

[**Bilgi deposunu** ](knowledge-store-concept-intro.md) bir işlem hattının zenginleştirme yapay ZEKA tabanlı yeni bir hedef. Bir dizin ek olarak Azure depolama dizini oluşturma sırasında oluşturulan doldurulmuş veri yapılarını artık kalıcı hale getirebilirsiniz. Verilerinizi verileri tablo depolama veya Blob Depolama içinde depolanmasından bağımsız veri şeklinde nasıl ve birden çok görünüm olup olmadığı dahil olmak üzere bir beceri kümesi öğeleri aracılığıyla fiziksel yapıları denetim.

[**Müşteri tarafından yönetilen bir şifreleme anahtarları** ](search-security-manage-encryption-keys.md) için hizmet tarafı şifreleme bekleyen ayrıca yeni bir önizleme özelliğidir. Yerleşik şifreleme Microsoft tarafından yönetilen bekleyen ek olarak, şifreleme anahtarları tek sahibi olduğu bir ek katmanı uygulayabilirsiniz.

## <a name="other-preview-features"></a>Diğer Önizleme özellikleri

Önceki önizlemelerde bildirilen özellikler hala genel Önizleme aşamasındadır. Bir önceki Önizleme api-version ile API arıyoruz ise bu sürümünü kullanın veya geçmek devam etmeden `2019-05-06-Preview` beklenen davranışın değişiklik yapmadan.

+ [moreLikeThis sorgu parametresi](search-more-like-this.md) belirli bir belge için uygun olan belgeleri bulur. Bu özellik, önceki önizlemelerde olmuştur. 
* [CSV blob dizin](search-howto-index-csv-blobs.md) metin blob başına tek bir belge aksine, satır başına bir belgeyi oluşturur.
* [Cosmos DB dizin oluşturucular için MongoDB API'si desteği](search-howto-index-cosmosdb.md) Önizleme aşamasındadır.


## <a name="how-to-call-a-preview-api"></a>Bir önizleme API çağırma

Eski önizlemeler hala çalışır ancak zaman içinde eski haline gelir. Kodunuzu çağırırsa `api-version=2016-09-01-Preview` veya `api-version=2017-11-11-Preview`, bu çağrıları hala geçerli. Ancak, yalnızca en yeni önizleme sürümü ile geliştirmeleri yenilenir. 

Aşağıdaki örnek söz dizimini Önizleme API sürümü çağrıyı gösterir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Azure arama hizmeti içinde birden çok sürümü kullanılabilir. Daha fazla bilgi için [API sürümlerini](search-api-versions.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure Search Hizmeti REST API başvuru belgelerini gözden geçirin. Sorunlarla karşılaşırsanız, bize yardım üzerinde isteyin [StackOverflow](https://stackoverflow.com/) veya [desteğe](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Arama hizmeti REST API Başvurusu](https://docs.microsoft.com/rest/api/searchservice/)