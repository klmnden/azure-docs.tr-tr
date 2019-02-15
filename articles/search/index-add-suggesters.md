---
title: Öneri Araçları eklemek için Azure Search dizini
description: Önerilen sorgular Azure Search dizini alanlarındaki metin burada oluşur alanları yazarken tamamlanan sorgu eylemler için etkinleştirir.
ms.date: 02/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 7128e4d3b0675775dc713451ef672b28a4991499
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56269935"
---
# <a name="add-suggesters-to-an-azure-search-index"></a>Öneri Araçları eklemek için Azure Search dizini

A **öneri aracı** olan "arama---yazarken" destekleyen bir Azure Search yapısı [önerileri](https://docs.microsoft.com/rest/api/searchservice/suggestions) özellik ve [(Önizleme) otomatik tamamlama](search-autocomplete-tutorial.md) özelliği. Öneriler API'sine çağrı yapmadan önce tanımlamalıdır bir **öneri aracı** belirli alanlarda önerilerini etkinleştirmek için bir dizinde.

Ancak bir **öneri aracı** birçok özelliğe sahiptir, öncelikle bir alan için sağlayarak koleksiyonu [öneriler API'si](https://docs.microsoft.com/rest/api/searchservice/suggestions). Örneğin, bir seyahat uygulaması hedefler, şehir ve ilgi çekici özellikler typeahead aramasını etkinleştirmek isteyebilirsiniz. Bu nedenle, tüm üç alanı alanı koleksiyonda gitmesi gerekiyordu.

Tek sahip **öneri aracı** kaynak her dizin için (özellikle bir **öneri aracı** içinde **öneri Araçları** koleksiyonu).

## <a name="creating-a-suggester"></a>Bir öneri aracı oluşturma 

Oluşturabileceğiniz bir **öneri aracı** herhangi bir zamanda ancak dizininizi üzerindeki etkiyi alanlarda göre değişir. 

+ Yeni alanlar için bir öneri aracı aynı güncelleştirmenin bir parçası eklenmiş olan en az etkili içeren hiçbir dizinin yeniden oluşturulması gerekiyor.
+ Var olan bir öneri aracı için alanları eklendi ancak tam bir dizini yeniden araya alan tanımı değiştirir.

 **Öneri Araçları** belirli belgeleri yerine gevşek koşulları ya da tümcelere önermek için kullanıldığında en iyi şekilde çalışır. En iyi aday başlıklar, adları ve öğeyi tanımlayan diğer görece kısa deyimlerin alanlardır. Kategoriler ve etiketler gibi yinelenen alanlar veya çok uzun alanlar açıklamaları ya da yorumlarınız alanları gibi daha az etkilidir.  

Bir öneri aracı oluşturulduktan sonra Ekle [öneriler API'si](https://docs.microsoft.com/rest/api/searchservice/suggestions) özellik çağırmak için sorgu mantığındaki.  

Tanımlayan özellikleri bir **öneri aracı** şunları içerir:  

|Özellik|Açıklama|  
|--------------|-----------------|  
|`name`|Adını **öneri aracı**. Adını kullandığınız **öneri aracı** çağırırken [önerileri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/suggestions).|  
|`searchMode`|Aday tümcecikleri aramak için kullanılan strateji. Şu anda desteklenen tek moddur `analyzingInfixMatching`, başında veya ortasında cümleler tümce esnek eşleştirme gerçekleştirir.|  
|`sourceFields`|Öneriler için içerik kaynağı olan bir veya daha fazla alanların listesi. Türünde alanlar `Edm.String` ve `Collection(Edm.String)` kaynaklar için önerileri olabilir. Kümesi özel bir dil Çözümleyicisi olmayan alanlar kullanılabilir. |  

## <a name="suggester-example"></a>Öneri aracı örneği  
 A **öneri aracı** dizin tanımını bir parçasıdır. Yalnızca bir **öneri aracı** içinde bulunabilir **öneri Araçları** koleksiyon geçerli sürümünde yanı sıra **alanları** koleksiyonu ve **scoringProfiles**.  

```  
{  
  "name": "hotels",  
  "fields": [  
     . . .   
   ],  
  "suggesters": [  
    {  
    "name": "sg",  
    "searchMode": "analyzingInfixMatching",  
    "sourceFields": ["hotelName", "category"]  
    }  
  ],  
  "scoringProfiles": [  
     . . .   
  ]  
}  

```  

## <a name="see-also"></a>Ayrıca bkz.  
 [Dizin oluşturma &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)   
 [Dizin güncelleştirme &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/update-index)   
 [Öneriler &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/suggestions)   
 [Dizin işlemleri &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/index-operations)   
 [Azure Search Service REST](https://docs.microsoft.com/rest/api/searchservice/)   
 [Azure Search .NET SDK'sı](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)  
