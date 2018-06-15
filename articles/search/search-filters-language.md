---
title: Azure Search'te dil filtreleri | Microsoft Docs
description: Filtre ölçütü kullanıcının güvenlik kimliği, dil, coğrafi konuma veya Azure arama, Microsoft Azure üzerinde barındırılan bulut arama hizmeti sorgularda arama sonuçları azaltmak için sayısal değerleri.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.workload: search
ms.date: 10/23/2017
ms.author: heidist
ms.openlocfilehash: 6d7fa7ab6db1fe9f8e2d1530c2917f4716a38079
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31790636"
---
# <a name="how-to-filter-by-language-in-azure-search"></a>Azure Search'te dile göre filtreleme 

Çok dilde arama uygulamasında anahtar özelliği üzerinden aramak ve kullanıcının kendi dilini sonuçları almak için zorunludur. Azure Search'te çok dilli uygulama dil gereksinimlerini karşılamak için tek bir dizi dizeleri belirli bir dilde depolanması için ayrılmış alan oluşturun ve sonra tam metin araması yalnızca bu alanlara sorgu zamanında sınırlamak için yoludur.

Sorgu parametreleri istek üzerine arama işlemi kapsam hem içerik teslim etmek istediğiniz arama deneyimi ile uyumlu sağlamıyorsa herhangi bir sorgu sonuçlarını kırpma için kullanılır.

| Parametreler | Amaç |
|-----------|--------------|
| **searchFields** | Sınırları, adlandırılmış bir alanlar listesi metin aramasını tam. |
| **$select** | Yalnızca belirttiğiniz alanları içerecek şekilde yanıt kırpar. Varsayılan olarak, tüm alınabilir alanları döndürülür. **$Select** parametresi döndürülecek hangilerinin seçmenize olanak sağlar. |

Bu teknik başarısını alan içerikleri bütünlüğünü menteşeleri. Azure arama bırakmaz dizeleri Çevir veya dil algılama gerçekleştirin. Bu alanların beklediğiniz dizeleri içerdiğinden emin olmak için size bağlıdır.

## <a name="define-fields-for-content-in-different-languages"></a>Farklı dillerde içerik için alanlarını tanımlayın

Azure Search'te sorgu tek bir dizin hedefleyin. Tek arama deneyimi dile özgü dizelerde genellikle sağlamak isteyen geliştiriciler değerlerini depolamak için özel alanları tanımlamak: İngilizce için bir alan dizeleri, Fransızca vb. için bir tane. 

Bizim örneklerimizi de dahil olmak üzere içinde [Gayrimenkul örnek](search-get-started-portal.md) aşağıda gösterilen, aşağıdaki ekran görüntüsüne benzer alan tanımları açtığınız. Bu örnek dil Çözümleyicisi atamaları alanlar için bu dizinde nasıl gösteren dikkat edin. Dizeleri içeren alanlar daha iyi hedef dil dil kurallarını işlemek üzere tasarlanmış bir Çözümleyicisi ile eşlenmiş, tam metin araması gerçekleştirin.

  ![](./media/search-filters-language/lang-fields.png)

> [!Note]
> Dil Çözümleyicileri alan tanımlarını gösteren kod örnekleri için bkz: [(.NET) bir dizin tanımla](https://docs.microsoft.com/azure/search/search-create-index-dotnet#define-your-azure-search-index) ve [(REST) bir dizin tanımla](https://docs.microsoft.com/azure/search/search-create-index-rest-api#define-your-azure-search-index-using-well-formed-json).

## <a name="build-and-load-an-index"></a>Derleme ve dizin yükleme

Bir ara (ve muhtemelen belirgin) için sahip adımdır [oluşturmak ve dizinini doldurmak](https://docs.microsoft.com/azure/search/search-create-index-dotnet#create-the-index) bir sorgu formulating önce. Bu adım burada eksiksiz olması için ayarlardan söz eder. Dizin kullanılabilir olup olmadığını belirlemek için bir yoldur dizinler listesi denetleyerek [portal](https://portal.azure.com).

## <a name="constrain-the-query-and-trim-results"></a>Sorgu sınırlamak ve sonuçları kırpma

Sorgu parametreleri, belirli alanları aramayı sınırla ve senaryonuz için yararlı değildir herhangi bir sorgu sonuçlarını kırpma için kullanılır. Kullanacağınız kısıtlayan arama amacı Fransızca dizeleri içeren alanlar için verilen, **searchFields** dizelerini o dilde içeren alanlar sorgu hedeflemek için. 

Varsayılan olarak, bir arama olarak alınabilir işaretlenmiş tüm alanlar döndürür. Bu nedenle, sağlamak istediğiniz dile özgü arama deneyimi için uygun olmayan alanlar dışlamak isteyebilirsiniz. Bir alanla Fransızca dizeleri arama sınırlı, özellikle, büyük olasılıkla İngilizce dizeleri alanlarla, sonuçlardan dışlamak istediğiniz. Kullanarak **$select** sorgu denetim hangi alanların çağrı yapan uygulamaya döndürülür parametresi sağlar.

```csharp
parameters =
    new SearchParameters()
    {
        searchFields = "description_fr" 
        Select = new[] { "description_fr"  }
    };
```
> [!Note]
> Sorguya dayalı no $filter bağımsız değişken ancak biz filtreleme senaryo olarak vardır ve bu nedenle bu kullanım örneği filtre kavramlarla kesinlikle bağlı.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search'te filtreleri](search-filters.md)
+ [Dil çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support)
+ [Azure Search'te nasıl tam metin araması çalışır](search-lucene-query-architecture.md)
+ [REST API belgelerde arama](https://docs.microsoft.com/rest/api/searchservice/search-documents)

