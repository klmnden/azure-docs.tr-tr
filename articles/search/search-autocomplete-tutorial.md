---
title: Otomatik Tamamlama örneği - Azure Search arama kutusuna typeahead eklemek için
description: Azure Search'te sorgu Eylemler typeahead öneri araçları oluşturarak ve tamamlanan koşulları ya da tümcelere bir arama kutusu doldurun istekleri formulating etkinleştirin.
manager: pablocas
author: mrcarter8
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: mcarter
ms.custom: seodec2018
ms.openlocfilehash: b78fdf0c493e4631e4cdd7e26b154570b6226d1f
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369590"
---
# <a name="example-add-autocomplete-to-partial-term-inputs-in-azure-search"></a>Örnek: Azure Search'te kısmi terimi girişleri otomatik tamamlama ekleyin

Bu önizleme özelliğini "kısmi terimi giriş tamamlanmış bir Azure Search dizini belgelerde terimden sağlanarak biter". Bu özelliği ticari arama motorları fark etmiş olabilirsiniz. Bu özellik şu anda genel önizlemede olan bir sorgu girişi basitleştirmek için bir Azure arama çözümü artık ekleyebilirsiniz.

Bu örnekte, nasıl kullanılacağını öğreneceksiniz [önerileri](https://docs.microsoft.com/rest/api/searchservice/suggestions), [otomatik tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete) ve [modelleri](search-faceted-navigation.md) içinde [Azure Search REST API'sine](https://docs.microsoft.com/rest/api/searchservice/) ve [.NET SDK'sı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions?view=azure-dotnet) derleme güçlü bir arama kutusu için. 

+ *Öneriler*, kullanıcının o ana kadar girdiği metne göre gerçek sonuçlardan öneriler sunar. 
+ *Otomatik Tamamlama*, [yeni bir önizleme özelliği](search-api-preview.md) hangi kullanıcı şu anda yazıyor tamamlanması dizinden koşulları Azure Search'te sağlar. 

Biz yazarken arama zenginliğine doğrudan kullanıcıya getirerek kullanıcı üretkenliğini artırmak için birden çok teknikleri karşılaştıracağız.

Bu örnekte kullanan bir ASP.NET MVC tabanlı uygulama size C# çağrılacak [Azure Search .NET istemci kitaplıkları](https://aka.ms/search-sdk)ve Azure Search REST API'sini doğrudan çağırmak için JavaScript. Bu örnek uygulamayı doldurulmuş bir dizin hedefleyen [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) örnek veriler. NYCJobs örneğindeki önceden yapılandırılmış dizini kullanabilir veya NYCJobs örnek çözümündeki veri yükleyiciyi kullanarak kendi dizininizi oluşturabilirsiniz. Örnek kullanır [jQuery kullanıcı Arabirimi](https://jqueryui.com/autocomplete/) ve [XDSoft](https://xdsoft.net/jqplugins/autocomplete/) JavaScript kitaplıklarını otomatik tamamlama destekleyen bir arama kutusu oluşturmak için. Bu bileşenler Azure Search ile birlikte kullanarak, birden çok örnek olarak, arama kutusuna yazarken tamamlanan ile otomatik tamamlama desteklemek nasıl görürsünüz.

Aşağıdaki görevleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Çözümü indirme ve yapılandırma
> * Uygulama ayarlarına arama hizmeti bilgilerini ekleme
> * Arama giriş kutusu ekleme
> * Bir uzak kaynaktan çeken bir otomatik tamamlama listesi için destek eklendi 
> * Öneriler ve otomatik tamamlama .NET SDK'sını ve REST API kullanarak alma
> * Performansı iyileştirmek için istemci tarafında önbelleğe almayı destekleme 

## <a name="prerequisites"></a>Önkoşullar

* Visual Studio 2017. Ücretsiz [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)'ı kullanabilirsiniz. 

* Örneği indirmek [kaynak kodu](https://github.com/azure-samples/search-dotnet-getting-started) örneğin.

* (İsteğe bağlı) Etkin Azure hesabı ve Azure Search hizmeti. Azure hesabınız yoksa [ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz. Hizmeti sağlama konusunda yardım almak için bkz. [Arama hizmeti oluşturma](search-create-service-portal.md). Bu örnek, bir barındırılan NYCJobs dizini zaten farklı bir tanıtım için yerinde kullanarak tamamlanabilir olduğundan hizmet ve hesabı isteğe bağlıdır.

* (İsteğe bağlı) NYCJobs verilerini kendi Azure Search hizmetinizdeki bir dizine aktarmak için [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) örnek kodunu indirin.

> [!Note]
> Ücretsiz Azure Search hizmetini kullanıyorsanız, üç dizin sınırlandırmanız vardır. NYCJobs veri yükleyici iki dizin oluşturur. Hizmetinizde yeni dizinleri kabul edecek kadar yer olduğundan emin olun.

### <a name="set-up-azure-search-optional"></a>Azure Search'ü ayarlama (İsteğe bağlı)

NYCJobs örnek uygulamasındaki verileri kendi dizininize aktarmak istiyorsanız bu bölümdeki adımları uygulayın. Bu adım isteğe bağlıdır.  Verilen örnek dizini kullanmak istiyorsanız bir sonraki bölüme geçerek örneği çalıştırın.

1. NYCJobs örnek kodunun DataLoader klasöründeki DataLoader.sln çözüm dosyasını Visual Studio'da açın.

1. Azure Search hizmetinizin bağlantı bilgileriyle güncelleştirin.  DataLoader projesindeki App.config dosyasını açın ve TargetSearchServiceName ile TargetSearchServiceApiKey appSettings değerlerini Azure Search hizmetiniz ve Azure Search Service API Anahtarı değeriyle değiştirin.  Bu bilgilere Azure portaldan ulaşabilirsiniz.

1. Uygulamayı çalıştırmak için F5'e basın.  2 dizin oluşturulur ve NYCJobs örnek verileri içeri aktarılır.

1. Örnek örnek kodda AutocompleteTutorial.sln çözüm dosyasını Visual Studio'da açın.  AutocompleteTutorial projesindeki Web.config dosyasını açın ve SearchServiceName ile SearchServiceApiKey yerine yukarıdaki değerleri girin.

### <a name="running-the-sample"></a>Örneği çalıştırma

Şimdi örnek örnek uygulamayı çalıştırmaya hazırsınız.  Örneği çalıştırmak için Visual Studio'da AutocompleteTutorial.sln çözüm dosyasını açın.  Çözümde bir ASP.NET MVC projesi bulunur.  Projeyi çalıştırmak ve sayfayı istediğiniz tarayıcıda yüklemek için F5'e basın.  En üstte C# ve JavaScript seçeneklerini göreceksiniz.  C# Seçeneği tarayıcıdan HomeController çağırır ve sonuçları almak için Azure Search .NET SDK'sını kullanır.  JavaScript seçeneği Azure Search REST API'sini doğrudan tarayıcıdan çağırır.  Bu akışın dışında denetleyicisi gerektirdiğinden bu seçenek genellikle fark edilir derecede daha iyi performans sahip olacaktır.  İhtiyaçlarınıza ve dil tercihlerinize uygun seçeneği tercih edebilirsiniz.  Sayfasında her biri için bazı yönergeler ile birden fazla otomatik tamamlama örnekleri vardır.  Her örnekte deneyebileceğiniz önerilen örnek metin vardır.  Gerçekleştirilen işlemleri görmek için her arama kutusuna birkaç harf yazmayı deneyin.

## <a name="how-this-works-in-code"></a>Bu kodda nasıl çalışır?

Örnekleri tarayıcıda gördünüz, şimdi ilk örneği ayrıntılı bir şekilde inceleyerek bileşenleri ve çalışma şekillerini gözden geçirelim.

### <a name="search-box"></a>Arama kutusu

Arama kutusu tüm dil seçeneklerinde aynıdır.  \Views\Home klasöründeki Index.cshtml dosyasını açın. Arama kutusu oldukça basittir:

```html
<input class="searchBox" type="text" id="example1a" placeholder="search">
```

Stil, JavaScript ve yer tutucu metnini başvuru için bir kimlik için bir sınıf ile basit bir giriş metin kutusu budur.  Sihrin gerçekleştiği yer JavaScript kodudur.

### <a name="javascript-code-c"></a>JavaScript kodu (C#)

C# dili örneği, Index.cshtml dosyasındaki JavaScript kodunu kullanarak jQuery UI Autocomplete kitaplığından faydalanmaktadır.  Bu kitaplık, otomatik tamamlama deneyimi MVC denetleyicisi öneriler almak için zaman uyumsuz çağrıları yaparak arama kutusuna ekler.  İlk örneğin JavaScript koduna bakalım:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: "/home/suggest?highlights=false&fuzzy=false&",
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

Bu kod sayfası yükleme "example1a" giriş kutusu otomatik tamamlama yapılandırmak için tarayıcı içinde çalışır.  `minLength: 3`, önerilerin yalnızca arama kutusuna en az üç karakter girildiğinde gösterilmesini sağlar.  Kaynak değeri önemlidir:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

Bu satırı arama kutusu altında görüntülenen öğe listesini almak nereye otomatik tamamlama API'sini bildirir.  Bu bir MVC projesi olduğundan HomeController.cs içindeki Suggest işlevini çağırır.  Bir sonraki bölümde bu konuya ayrıntılı olarak değineceğiz.  Ayrıca vurgulamaları, benzer öğe eşleştirmelerini ve süresi denetlemek için birkaç parametre iletir.  JavaScript API'si otomatik tamamlama dönem parametresi ekler.

#### <a name="extending-the-sample-to-support-fuzzy-matching"></a>Örneği benzer öğe eşleştirmeyi destekleyecek şekilde genişletme

Benzer öğe araması, kullanıcı bir kelimeyi arama kutusuna yanlış yazsa bile yakın eşleşmelere göre sonuçlara ulaşmanızı sağlar.  Şimdi kaynak satırını benzer öğe eşleştirmeyi etkinleştirecek şekilde değiştirerek bir deneme yapalım.

Aşağıdaki satırı değiştirin:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

şu şekilde:

```javascript
source: "/home/suggest?highlights=false&fuzzy=true&",
```

F5 tuşuna basarak uygulamayı başlatın.

"execative" gibi bir terim yazmayı deneyin. Yazdığınız harflerle tam olarak eşleşmediği halde "executive" terimi sonuçlarının döndürüldüğünü göreceksiniz.

### <a name="homecontrollercs-c"></a>HomeController.cs (C#)

Örneğin JavaScript kodunu gözden geçirdik. Şimdi Azure Search .NET SDK'sını kullanarak önerileri alan C# denetleyicisi koduna göz atalım.

1. Controllers dizinindeki HomeController.cs dosyasını açın. 

1. İlk fark edeceğiniz şey sınıfın en üstünde InitSearch adlı bir metot olduğudur.  Bu, Azure Search hizmeti için kimliği doğrulanmış bir HTTP dizini istemcisi oluşturur.  Bunun işleyişi hakkında daha fazla bilgi edinmek istiyorsanız, aşağıdaki örnekte ziyaret edin: [Bir .NET Uygulamasından Azure Search kullanma](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

1. Suggest işlevine geçelim.

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

    DocumentSuggestResult resp = _indexClient.Documents.Suggest(term, "sg", sp);

    // Convert the suggest query results to a list that can be displayed in the client.
    List<string> suggestions = resp.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = suggestions
    };
}
```

Suggest işlevi, arama terimi girişine ek olarak isabet vurgularının veya benzer öğe eşleştirme özelliğinin kullanılıp kullanılmadığını belirleyen iki parametre alır.  Metot, Suggest API'sine iletilen bir SuggestParameters nesnesi oluşturur. Ardından alınan sonuç istemcide gösterilebilmesi için JSON biçimine dönüştürülür.
(İsteğe bağlı) Suggest işlevinin başlangıcına bir kesme noktası ekleyin ve kodu adım adım inceleyin.  SDK tarafından döndürülen yanıta ve metot tarafından döndürülen sonuçları nasıl dönüştürdüğüne bakın.

Sayfadaki diğer örnekler, isabet vurgulama, yazarken tamamlanan otomatik tamamlama sonuçlarını istemci tarafı önbelleğe alma desteklemek üzere otomatik tamamlama önerileri ve modelleri için eklemek için aynı düzeni uygular.  Her birini gözden geçirerek nasıl çalıştıklarını ve arama deneyiminizde nasıl kullanabileceğinizi görebilirsiniz.

### <a name="javascript-language-example"></a>JavaScript dili örneği

JavaScript dili örneği için, IndexJavaScript.cshtml sayfasındaki JavaScript kodu jQuery UI Autocomplete kitaplığını kullanmaktadır.  Bu kitaplık, güzel görünen bir arama kutusu oluşturma konusundaki işlemlerin çoğunu gerçekleştirir ve öneri alma amacıyla Azure Search'e asenkron çağrı yapmayı kolaylaştırır.  İlk örneğin JavaScript koduna bakalım:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: function (request, response) {
        $.ajax({
            type: "POST",
            url: suggestUri,
            dataType: "json",
            headers: {
                "api-key": searchServiceApiKey,
                "Content-Type": "application/json"
            },
            data: JSON.stringify({
                top: 5,
                fuzzy: false,
                suggesterName: "sg",
                search: request.term
            }),
                success: function (data) {
                    if (data.value && data.value.length > 0) {
                        response(data.value.map(x => x["@@search.text"]));
                    }
                }
            });
        },
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

Bunu yukarıdaki Home denetleyicisini çağıran örnekle karşılaştırdığınızda birçok benzerlik göreceksiniz.  Otomatik Tamamlama yapılandırmasını `minLength` ve `position` tam olarak aynıdır.  Buradaki önemli değişiklik kaynaktır.  Giriş denetleyicide Öner yöntemini çağırmak, yerine REST isteği içinde JavaScript işlevi oluşturulur ve Ajax kullanarak.  Ardından gelen yanıt "success" parametresinde işlenir ve kaynak olarak kullanılır.

## <a name="takeaways"></a>Paketler

Bu örnekte, bir arama kutusu otomatik tamamlama ve öneriler destekleyen oluşturmaya yönelik temel adımlar gösterilmektedir.  Gördüğünüz bir ASP.NET MVC uygulaması oluşturma ve öneriler almak için Azure Search .NET SDK veya REST API'sini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Öneriler ve otomatik tamamlama'ya arama deneyiminizi tümleştirin.  Nasıl doğrudan .NET SDK veya REST API'yi kullanarak daha üretken hale getirmek için yazarken, kullanıcılarınız için Azure Search gücünü katın yardımcı olabileceğini göz önünde bulundurun.

> [!div class="nextstepaction"]
> [Autocomplete REST API'si](https://docs.microsoft.com/rest/api/searchservice/autocomplete)
> [Suggestions REST API'si](https://docs.microsoft.com/rest/api/searchservice/suggestions)
> [Create Index REST API'sindeki modeller dizini özniteliği](https://docs.microsoft.com/rest/api/searchservice/create-index)

