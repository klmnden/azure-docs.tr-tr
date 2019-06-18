---
title: Bir dizine - Azure Search typeahead sorguları ekleme
description: Azure Search'te yazarken tamamlanan sorgu Eylemler, öneri araçları oluşturma ve otomatik tamamlama çağırma veya sorgu terimleriyle autosuggested istekleri formulating etkinleştirin.
ms.date: 05/02/2019
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
ms.openlocfilehash: eb6667a1429382ed566826de64ad7ffbe83183cf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65521880"
---
# <a name="add-suggesters-to-an-index-for-typeahead-in-azure-search"></a>Azure Search'te typeahead için dizin öneri Araçları eklemek

A **öneri aracı** bir yapıdır bir [Azure Search dizini](search-what-is-an-index.md) , "arama---yazarken" deneyimini destekler. Bu sorgu girişleri typeahead etkinleştirmek istediğiniz alanların listesini içerir. İçinde bir dizin veya her ikisini bu iki typeahead çeşitleri aynı öneri aracı destekler: *otomatik tamamlama* terimini veya tümceciğini yazdığınız, tamamlanan *önerileri* sonuçların kısa listesini sağlar. 

Aşağıdaki ekran görüntüsünde iki typeahead özelliklerini gösterir. Öneriler, belirli bir oyun için bir sayfa olacak gerçek sonuçlar ise bu Xbox arama sayfasında, otomatik tamamlama öğeleri, bu sorgu için yeni bir arama sonuçları sayfası ulaşmanızı sağlar. Arama çubuğuna bir öğe için otomatik tamamlama sınırlamak veya aşağıda gösterilene benzer bir liste sağlar. Öneriler için en iyi sonucu açıklayan bir belge herhangi bir bölümünü ortaya çıkabilir.

![Otomatik Tamamlama ve önerilen sorgular Visual karşılaştırması](./media/index-add-suggesters/visual-comparison-suggest-complete.png "otomatik tamamlama ve önerilen sorgular Visual karşılaştırması")

Azure Search'te Bu davranışların uygulamak için bir dizin ve sorgu bileşeni yoktur. 

+ Bir öneri aracı dizini bileşendir. Bir öneri aracı oluşturmak için portalı, REST API'si veya .NET SDK'sını kullanabilirsiniz. 

+ Sorgu bileşeni belirtilen sorgu isteği (bir öneri veya otomatik tamamlama eylemini) bir eylemdir. 

Arama---yazarken destek alan başına temelinde etkinleştirilir. Ekran görüntüsünde gösterilen benzer bir deneyim istiyorsanız her iki typeahead davranışları aynı arama çözüm içinde uygulayabilirsiniz. Her iki istekleri hedef *belgeleri* belirli dizinini ve yanıtları koleksiyonu, bir kullanıcı en az üç karakter giriş dizesi ayarının sonra döndürülür.

## <a name="create-a-suggester"></a>Öneri oluşturucu oluşturma

Bazı özellikler bir öneri aracı olsa da, öncelikle typeahead deneyimini etkinleştirme alanlar koleksiyonu var. Örneğin, bir seyahat uygulaması hedefler, şehir ve ilgi çekici özellikler typeahead aramasını etkinleştirmek isteyebilirsiniz. Bu nedenle, tüm üç alan alanlar koleksiyonunda gitmesi gerekiyordu.

Bir öneri aracı oluşturmak için bir dizin şemasını birine ekleyin. Bir dizinde bir öneri aracı olabilir (özellikle, öneri araçlarını koleksiyondaki bir öneri aracı). 

### <a name="use-the-rest-api"></a>REST API kullanma

REST API aracılığıyla öneri araçları ekleyebilirsiniz [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) veya [dizin güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index). 

  ```json
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
Bir öneri aracı oluşturulduktan sonra Ekle [öneriler API'si](https://docs.microsoft.com/rest/api/searchservice/suggestions) veya [otomatik tamamlama API'sini](https://docs.microsoft.com/rest/api/searchservice/autocomplete) özellik çağırmak için sorgu mantığındaki.

### <a name="use-the-net-sdk"></a>.NET SDK’yı kullanma

İçinde C#, tanımlayan bir [öneri aracı nesne](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet). `Suggesters` bir koleksiyonu, ancak yalnızca tek bir öğeyi alabilirler. 

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
        Fields = FieldBuilder.BuildForType<Hotel>(),
        Suggesters = new List<Suggester>() {new Suggester()
            {
                Name = "sg",
                SourceFields = new string[] { "HotelId", "Category" }
            }}
    };

    serviceClient.Indexes.Create(definition);

}
```

## <a name="property-reference"></a>Özellik Başvurusu

Önemli noktalara dikkat edin hakkında öneri araçları için bir ad (öneri araçları istek üzerine adıyla başvurulur), bir searchMode (şu anda yalnızca bir "Analyzingınfixmatching") ve typeahead etkinleştirildiği alanların listesi olduğundan emin olur. 

Bir öneri aracı tanımlayan özellikleri şunlardır:

|Özellik      |Açıklama      |
|--------------|-----------------|
|`name`        |Öneri aracı adı. Öneri aracı adını çağırırken kullanmak [önerileri REST API](https://docs.microsoft.com/rest/api/searchservice/suggestions) veya [otomatik tamamlama REST API](https://docs.microsoft.com/rest/api/searchservice/autocomplete).|
|`searchMode`  |Aday tümcecikleri aramak için kullanılan strateji. Şu anda desteklenen tek moddur `analyzingInfixMatching`, başında veya ortasında cümleler tümce esnek eşleştirme gerçekleştirir.|
|`sourceFields`|Öneriler için içerik kaynağı olan bir veya daha fazla alanların listesi. Türünde alanlar `Edm.String` ve `Collection(Edm.String)` kaynaklar için önerileri olabilir. Kümesi özel bir dil Çözümleyicisi olmayan alanlar kullanılabilir.<p/>Arama çubuğu veya açılır liste tamamlanmış bir dizede olup yalnızca kendilerini bir beklenen ve uygun yanıt olarak kiralamak alanları belirtin.<p/>Duyarlık sahip olduğu bir otel adı iyi bir adaydır. Ayrıntılı açıklamaları ve yorumlarına gibi çok yoğun alanlardır. Benzer şekilde, yinelenen alanlar, kategoriler ve etiketler gibi daha az etkilidir. Örneklerde "Kategori" yine de birden çok alan içerebilir göstermek için ekliyoruz. |

## <a name="when-to-create-a-suggester"></a>Bir öneri aracı oluşturma zamanı

Bir dizini yeniden oluşturma, bir öneri aracı ve içinde belirtilen alanları önlemek için `sourceFields` aynı anda oluşturulmuş olması gerekir.

Burada mevcut alanların dahil edilecek varolan bir dizini için bir öneri aracı eklerseniz `sourceFields`, alan tanımı temelde değiştirir ve yeniden derleme gereklidir. Daha fazla bilgi için [Azure Search dizini yeniden oluşturmak nasıl](search-howto-reindex.md).

## <a name="how-to-use-a-suggester"></a>Bir öneri aracı kullanma

Daha önce belirtildiği gibi bir öneri aracı önerilen sorgular, otomatik tamamlama veya her ikisini de kullanabilirsiniz. 

İstekte işlemi ile birlikte bir öneri aracı başvuruldu. Örneğin, bir GET REST araması üzerinde seçeneklerinden birini belirtin `suggest` veya `autocomplete` belge koleksiyonu. KALAN için bir öneri aracı oluşturduktan sonra kullanma [öneriler API'si](https://docs.microsoft.com/rest/api/searchservice/suggestions) veya [otomatik tamamlama API'sini](https://docs.microsoft.com/rest/api/searchservice/autocomplete) sorgu mantığınızı içinde.

.NET için kullanma [SuggestWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet) veya [AutocompleteWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet).

Her iki istek gösteren bir örnek için bkz: [otomatik tamamlama ve öneriler, Azure Search'te eklemek için örnek](search-autocomplete-tutorial.md).

## <a name="sample-code"></a>Örnek kod

[DotNetHowToAutocomplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) örnek içeren her ikisi de C# ve Java kod ve öneri aracı oluşturma, önerilen sorgular, otomatik tamamlama ve model Gezinti gösterir. 

Azure Search hizmeti bir korumalı alan kullanır ve tüm yapmanız gereken çalıştırmak için F5 tuşuna önceden yüklenmiş bir dizin olduğundan. Abonelik veya oturum açma gerekli.

## <a name="next-steps"></a>Sonraki adımlar

İsteklerin nasıl ifade edilir görmek için aşağıdaki örnek öneririz.

> [!div class="nextstepaction"]
> [Öneriler ve otomatik tamamlama örnekler](search-autocomplete-tutorial.md) 
