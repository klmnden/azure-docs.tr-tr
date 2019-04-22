---
title: Azure Search arama dizini - çok dilli içeriği dil filtreleri
description: Filtre ölçütü çoklu dil araması, dile özgü alanlar için kapsam belirleme sorgu yürütme desteklemek için.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.workload: search
ms.topic: conceptual
ms.date: 10/23/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 695fdfba1573ff97b05f8e8b50a05bef9dbf48de
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58846168"
---
# <a name="how-to-filter-by-language-in-azure-search"></a>Azure Search'te dile göre filtreleme 

Çok dilli arama uygulamasında anahtar üzerinde arama ve sonuçlar kullanıcının kendi dilinde alma olanağı zorunludur. Azure Search'te, çok dilli uygulama dil gereksinimlerini karşılamak için tek bir dizi içinde belirli bir dil dizeleri depolamak için ayrılmış alan oluşturun ve ardından sorgu zamanında tam metin araması yalnızca alanlara sınırlamak için yoludur.

İstek sorgu parametrelerinde hem arama işlemi kapsam ve ardından sonuçları içerik teslim etmek istediğiniz arama deneyimi ile uyumlu sağlamayan tüm alanların kırpma için kullanılır.

| Parametreler | Amaç |
|-----------|--------------|
| **searchFields** | Sınırları metin araması için adlandırılmış alanları listesi tam. |
| **$select** | Yalnızca belirttiğiniz alanları içerecek biçimde yanıt kırpar. Varsayılan olarak, tüm getirilebilir alanlar döndürülür. **$Select** parametre döndürmek için hangilerinin seçmenize olanak sağlar. |

Bu teknik başarısını alan içeriğini bütünlüğüne menteşeleri. Azure arama dizeleri çevirmek veya dil algılama desteklemez. Bu alanların beklediğiniz dizeleri içerdiğinden emin olmak için size bağlıdır.

## <a name="define-fields-for-content-in-different-languages"></a>Farklı dillerde içerik için alanlar tanımlayın

Azure Search'te sorgular tek bir dizinde hedefleyin. Genellikle dile özgü dizeleri bir tek bir arama deneyimi sağlamak isteyen geliştiriciler değerleri depolamak için özel alanlar tanımlayın: İngilizce için bir alan dizeleri, Fransızca ve benzeri. 

De dahil olmak üzere örneklerimizi [real estate and örnek](search-get-started-portal.md) aşağıda gösterilen, aşağıdaki ekran görüntüsüne benzer alan tanımları görmüş olabilirsiniz. Bu örnekte dil Çözümleyicisi atamaları alanlar için bu dizinde nasıl görüneceğini dikkat edin. Dizeleri içeren alanlar daha iyi dilsel kurallar hedef dil işlemek için tasarlanmış bir Çözümleyicisi ile birlikte kullanıldığında tam metin araması gerçekleştirin.

  ![](./media/search-filters-language/lang-fields.png)

> [!Note]
> Dil Çözümleyicileri ile alan tanımları gösteren kod örnekleri için bkz [(.NET) bir dizin tanımla](https://docs.microsoft.com/azure/search/search-create-index-dotnet) ve [(REST) bir dizin tanımla](search-create-index-rest-api.md).

## <a name="build-and-load-an-index"></a>Derleme ve dizin yükleme

Ara (ve belki de belirgin) bir adım için sahip olduğu [oluşturun ve dizini doldurma](https://docs.microsoft.com/azure/search/search-create-index-dotnet) sorgu formulating önce. Biz bu adımı burada bütünlük bahsedebilirsiniz. Dizin kullanılabilir olup olmadığını belirlemenin bir yolu olan dizinler listesi işaretleyerek [portalı](https://portal.azure.com).

## <a name="constrain-the-query-and-trim-results"></a>Sorgu kısıtlamak ve sonuçları Kes

Sorgu parametreleri, arama belirli alanlarla sınırlandırmak ve sonra senaryonuz için yararlı olmayan herhangi bir alan sonuçlarını kırpma için kullanılır. Kullanacağınız kısıtlayan arama amacı Fransızca dizeler içeren alanlar için göz önünde bulundurulduğunda, **searchFields** söz konusu dil dizeleri içeren alanlar sorgu hedeflemek için. 

Varsayılan olarak, bir arama alınabilir olarak işaretlenmiş tüm alanlardan döndürür. Bu nedenle, sunmak istediğiniz dile özgü arama deneyimi için uygun olmayan alanları dışlamak isteyebilirsiniz. Arama Fransızca dizeler bir alanla sınırlıdır, özellikle de büyük olasılıkla dizeleri İngilizce alanlarla sonuçlarınızdan çıkarmak istediğiniz. Kullanarak **$select** sorgu denetim üzerinde hangi alanların çağıran uygulamaya döndürülen parametresi sağlar.

```csharp
parameters =
    new SearchParameters()
    {
        searchFields = "description_fr" 
        Select = new[] { "description_fr"  }
    };
```
> [!Note]
> Sorguyu no $filter bağımsız değişken olsa biz filtreleme senaryo olarak vardır ve bu nedenle bu kullanım örneği filtre kavramlarla kesin bağlı.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search'te filtreler](search-filters.md)
+ [Dil çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support)
+ [Metin arama Azure Search'te tam nasıl çalışır](search-lucene-query-architecture.md)
+ [Search belgeleri REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents)

