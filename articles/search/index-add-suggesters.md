---
title: Bir dizine - Azure Search typeahead sorguları ekleme
description: Azure Search'te yazarken tamamlanan sorgu Eylemler, öneri araçları oluşturma ve otomatik tamamlama çağırma veya sorgu terimleriyle autosuggested istekleri formulating etkinleştirin.
ms.date: 03/22/2019
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
ms.openlocfilehash: 877294e80d655ab75be78a5aa57854a03a5f267a
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370661"
---
# <a name="add-suggesters-to-an-index-for-typeahead-in-azure-search"></a>Azure Search'te typeahead için dizin öneri Araçları eklemek

A **öneri aracı** bir yapıdır bir [Azure Search dizini](search-what-is-an-index.md) , "arama---yazarken" deneyimini destekler. Bu sorgu girişleri typeahead etkinleştirmek istediğiniz alanların listesini içerir. Typeahead iki çeşidi vardır: [ *otomatik tamamlama* ](search-autocomplete-tutorial.md) terimini veya tümceciğini yazdığınız, tamamlanan [ *otomatik öneri* ](search-autosuggest-example.md) kısa sağlar koşulları veya sorgu girdi olarak seçtiğiniz ifadeler listesi. Kuşkusuz bu davranışları ticari arama motorları önce gördünüz.

![Otomatik Tamamlama Visual karşılaştırması ve otomatik öneri](./media/index-add-suggesters/visual-comparison-suggest-complete.png "Visual karşılaştırması otomatik tamamlama ve otomatik öneri")

Azure Search'te Bu davranışların uygulamak için bir dizin ve sorgu bileşeni yoktur. 

+ Bir dizinde bir öneri aracı ekleyin. REST API'si veya .NET SDK'sı bir öneri aracı oluşturmak için portalı kullanabilirsiniz. 

+ Bir sorgu üzerinde otomatik öneri veya otomatik tamamlama eylemini belirtin. 

> [!Important]
> Otomatik Tamamlama, REST API Önizleme aşamasında kullanıma Önizleme ve .NET SDK'sı şu anda kullanılabilir ve üretim uygulamaları için desteklenmiyor. 

Typeahead destek alan başına temelinde etkinleştirilir. Ekran görüntüsünde gösterilen benzer bir deneyim istiyorsanız her iki typeahead davranışları aynı arama çözüm içinde uygulayabilirsiniz. Her iki istekleri hedef *belgeleri* belirli dizinini ve yanıtları koleksiyonu, bir kullanıcı en az üç karakter giriş dizesi ayarının sonra döndürülür.

## <a name="create-a-suggester"></a>Bir öneri aracı oluşturma

Bazı özellikler bir öneri aracı olsa da, öncelikle typeahead deneyimini etkinleştirme alanlar koleksiyonu var. Örneğin, bir seyahat uygulaması hedefler, şehir ve ilgi çekici özellikler typeahead aramasını etkinleştirmek isteyebilirsiniz. Bu nedenle, tüm üç alan alanlar koleksiyonunda gitmesi gerekiyordu.

Bir öneri aracı oluşturmak için bir dizin şemasını birine ekleyin. Bir dizinde bir öneri aracı olabilir (özellikle, öneri araçlarını koleksiyondaki bir öneri aracı). 

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

.NET SDK'da kullanan bir [öneri aracı sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet). Öneri aracı koleksiyonudur ancak yalnızca tek bir öğeyi alabilirler.

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

Önemli noktalara dikkat edin hakkında öneri araçları için bir ad (öneri araçları istek üzerine adıyla başvurulur), bir searchMode (şu anda yalnızca bir "Analyzingınfixmatching") ve typeahead etkinleştirildiği alanların listesi olduğundan emin olur. 

Bir öneri aracı oluşturulduktan sonra Ekle [öneriler API'si](https://docs.microsoft.com/rest/api/searchservice/suggestions) özellik çağırmak için sorgu mantığındaki.

### <a name="property-reference"></a>Özellik Başvurusu

Bir öneri aracı tanımlayan özellikleri şunlardır:

|Özellik      |Açıklama      |
|--------------|-----------------|
|`name`        |Öneri aracı adı. Öneri aracı adını çağırırken kullanmak [önerileri REST API](https://docs.microsoft.com/rest/api/searchservice/suggestions) veya [otomatik tamamlama REST API'si (Önizleme)](https://docs.microsoft.com/rest/api/searchservice/autocomplete).|
|`searchMode`  |Aday tümcecikleri aramak için kullanılan strateji. Şu anda desteklenen tek moddur `analyzingInfixMatching`, başında veya ortasında cümleler tümce esnek eşleştirme gerçekleştirir.|
|`sourceFields`|Öneriler için içerik kaynağı olan bir veya daha fazla alanların listesi. Türünde alanlar `Edm.String` ve `Collection(Edm.String)` kaynaklar için önerileri olabilir. Kümesi özel bir dil Çözümleyicisi olmayan alanlar kullanılabilir.<p/>Arama çubuğu veya açılır liste tamamlanmış bir dizede olup yalnızca kendilerini bir beklenen ve uygun yanıt olarak kiralamak alanları belirtin.<p/>Duyarlık sahip olduğu bir otel adı iyi bir adaydır. Ayrıntılı açıklamaları ve yorumlarına gibi çok yoğun alanlardır. Benzer şekilde, yinelenen alanlar, kategoriler ve etiketler gibi daha az etkilidir. Örneklerde "Kategori" yine de birden çok alan içerebilir göstermek için ekliyoruz. |

### <a name="index-rebuilds-for-existing-fields"></a>Dizin için varolan alanları yeniden oluşturur.

Öneri araçları alanları içerir ve bir öneri aracı mevcut bir dizine ekleme veya değiştirme, alan oluşturma, büyük olasılıkla dizini yeniden oluşturmanız gerekir.

| Eylem | Etki |
|--------|--------|
| Yeni alan oluşturun ve yeni bir öneri aracı aynı anda aynı güncelleştirmede oluşturun | En az etki. Dizin önceden eklenmiş alanlar içeriyorsa, yeni alanlar ve yeni bir öneri aracı var olan alanlarda herhangi bir etkisi yoktur. |
| Önceden mevcut olan alanlar için bir öneri aracı ekleme | Yüksek etkisi. Bir alan, alan tanımında değişiklik ekleyerek, araya bir [tam yeniden derleme](search-howto-reindex.md).|

## <a name="use-a-suggester"></a>Bir öneri aracı kullanın

Daha önce belirtildiği gibi bir öneri aracı otomatik öneri, otomatik tamamlama veya her ikisini de kullanabilirsiniz. 

İstekte işlemi ile birlikte bir öneri aracı başvuruldu. Örneğin, bir GET REST araması üzerinde seçeneklerinden birini belirtin `suggest` veya `autocomplete` belge koleksiyonu. KALAN için bir öneri aracı oluşturduktan sonra kullanma [öneriler API'si](https://docs.microsoft.com/rest/api/searchservice/suggestions) veya [otomatik tamamlama API'sini (Önizleme)](https://docs.microsoft.com/rest/api/searchservice/autocomplete) sorgu mantığınızı içinde.

.NET için kullanma [SuggestWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet-preview) veya [AutocompleteWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet-preview&viewFallbackFrom=azure-dotnet).

Her isteğin gösteren örnekler:

+ [Eklemek için açılan sorgu seçimleri otomatik öneri](search-autosuggest-example.md)

+ [Azure Search'te kısmi terimi girişleri otomatik tamamlama eklemek](search-autocomplete-tutorial.md) (önizlemede) 

## <a name="sample-code"></a>Örnek kod

[DotNetHowToAutocomplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) örnek içeren her ikisi de C# ve Java kod ve bir öneri aracı yapım otomatik öneri, otomatik tamamlama ve modeli gezinme. 

Azure Search hizmeti bir korumalı alan kullanır ve tüm yapmanız gereken çalıştırmak için F5 tuşuna önceden yüklenmiş bir dizin olduğundan. Abonelik veya oturum açma gerekli.

## <a name="next-steps"></a>Sonraki adımlar

İsteklerin nasıl ifade edilir görmek için aşağıdaki örnekleri gözden geçirin:

+ [Autosuggested sorgusu örneği](search-autosuggest-example.md) 
+ [Otomatik tamamlanan sorgu örneği (Önizleme)](search-autocomplete-tutorial.md) 
