---
title: Örnek - Azure Search arama kutusuna açılan koşulları eklemek için otomatik öneri
description: Öneri araçları oluşturup, hem Azure Search'te önerilen sorgular istekleri formulating önerilen sorgu girişler ekleyin.
manager: cgronlun
author: heidisteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: heidist
ms.openlocfilehash: 1db085d61c60d303aaff5c3b0d16998805fcda90
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58373083"
---
# <a name="example-add-autosuggest-for-dropdown-query-selections"></a>Örnek: Eklemek için açılan sorgu seçimleri otomatik öneri

Arama terimi girişleri önerilen sorgu terimleriyle aşağı açılan listesini içerebilir. Bu özelliği ticari arama motorları gördünüz ve Azure Search kullanarak, benzer bir deneyim uygulayabileceğiniz bir [öneri aracı yapısı](index-add-suggesters.md) ve sorgu istek üzerine bir öneri işlemi. Bu makalede örnekler zaten tanımlamış olduğunuz bir öneri aracı kullanarak otomatik öneri sorgusu, oluşumunu göstermek için kullanır. 

## <a name="rest-api"></a>REST API

Kullanabileceğiniz [Postman](search-fiddler.md) veya [PowerShell](search-create-index-rest-api.md) ve [REST API](https://docs.microsoft.com/rest/api/searchservice/) Bu örnek, ancak sonuçları denemek için JSON belgeleri olarak döndürülür. Daha gerçekçi ve görsel bir deneyim kullanarak bulunabilir [örnek kod için otomatik tamamlama](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete).

### <a name="1---index-with-a-suggester-construct"></a>1 - dizin ile bir öneri aracı yapısı

Otomatik öneri ile başlayan bir [öneri aracı yapısı](index-add-suggesters.md) açılır değerleri katkıda bulunan alanları oluşan bir dizine eklenir. Aşağıdaki şema göz önünde bulundurulduğunda, önerilen sorgular hotelName ve kategori alanları değerleri kullanarak formüle edilebilir.

Hızlı başlangıçlar, bulunan en az bir örnek veri kümelerini varsayıldığında, "Başvurmaktan kalın" ve "Roach motel" otel adlarında ve "Lüks" ve "Budget" kategorilerini içerir.

Oteller dizinini zaten varsa, bırakın ve aşağıdaki şemayı kullanarak yeniden oluşturun. Bu şema için bir öneri aracı ekler. Verileri de yeniden unutmayın.

```json
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ],
  "suggesters": [
    {
      "name": "sg",
      "searchMode": "analyzingInfixMatching",
      "sourceFields": ["hotelName", "category"]
    }
  ]
}

```

### <a name="2---query-with-suggestions-operator"></a>2 - önerileri işleci ile sorgulama

Bir öneri aracı kullanın ve çağırmak için otomatik öneri, göndermeniz gerekir bir [öneriler API'si](https://docs.microsoft.com/rest/api/searchservice/suggestions) GET veya POST kullanarak isteyin. İlk üç karakterine alındığında bu tür bir istek üzerinde arama hizmeti için olası eşleşme tarar. 

İstek üstbilgisinde ayarlanan **api anahtarını** bir yönetici veya sorgu anahtarına ve **Content-Type** uygulama/JSON. 

```http
GET https://mydemo.search.windows.net/indexes/hotels/docs/suggest?search=fan&&suggesterName=sg
```

Öneri aracı içinde (hotelName ve kategorisi), tüm alanları dahil isteği taramalar şu yanıtı döndüren:

```
{
    "@odata.context": "https://mydemo.search.windows.net/indexes('hotels')/$metadata#docs(*)",
    "value": [
        {
            "@search.text": "Fancy Stay",
            "hotelId": "1"
        }
    ]
}
```

Beklenen sonucu sunmak için istemci kodunuz sonuçları arama çubuğu altındaki açılır listede olarak oluşturacak.

## <a name="net-sdk-c"></a>.NET SDK'SI (C#)

### <a name="1---index-with-a-suggester-construct"></a>1 - dizin ile bir öneri aracı yapısı

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

### <a name="2---query-with-suggest-method"></a>2 - Öner yöntemi ile sorgulama

[DocumentsOperationsExtensions.Suggest yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.suggest?view=azure-dotnet) önerilen sorgu dizeleri döndürmek için kullanılır.

```csharp
public ActionResult Suggest(bool highlights, bool fuzzy, string term)
{
    InitSearch();

    // Call suggest API and return results
    SuggestParameters sp = new SuggestParameters()
    {
        UseFuzzyMatching = fuzzy,
        Top = 5
    };

    if (highlights)
    {
        sp.HighlightPreTag = "<b>";
        sp.HighlightPostTag = "</b>";
    }

    DocumentSuggestResult suggestResult = _indexClient.Documents.Suggest(term, "sg",sp);

    // Convert the suggest query results to a list that can be displayed in the client.
    List<string> suggestions = suggestResult.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = suggestions
    };
}
```

## <a name="see-also"></a>Ayrıca bkz.

+ [Postman kullanarak REST API'sini keşfedin](search-fiddler.md)
+ [Örnek: Otomatik Tamamlama](search-autocomplete-tutorial.md)
+ [Öneri Araçları eklemek için bir dizin](index-add-suggesters.md)
+ [.NET kullanarak bir öneri araçları sınıfı Ekle](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet)
+ [GET veya POST (REST API'si) kullanarak arama önerileri](https://docs.microsoft.com/rest/api/searchservice/suggestions)
+ [Arama önerileri SuggestWithHttpMessagesAsync (.NET) kullanılarak](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet-preview) veya 