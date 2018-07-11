---
title: 'Öğretici: Azure Search kullanarak arama kutunuza otomatik tamamlama özelliği ekleme | Microsoft Docs'
description: Azure Search otomatik tamamlama ve öneriler API'lerini kullanarak veri merkezli uygulamalarınızda son kullanıcı deneyimini geliştirme örnekleri.
manager: pablocas
author: mrcarter8
services: search
ms.service: search
ms.devlang: NA
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: mcarter
ms.openlocfilehash: 90e99e7d44183d70f4e348c7b9070001fa3c6329
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37101162"
---
# <a name="tutorial-add-auto-complete-to-your-search-box-using-azure-search"></a>Öğretici: Azure Search kullanarak arama kutunuza otomatik tamamlama özelliği ekleme

Bu öğreticide güçlü bir arama kutusu oluşturmak için [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/)'sindeki [öneriler](https://docs.microsoft.com/rest/api/searchservice/suggestions), [otomatik tamamlama](https://docs.microsoft.com/en-us/rest/api/searchservice/autocomplete) ve [modeller](search-faceted-navigation.md) özellikleriyle [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions?view=azure-dotnet)'sını kullanmayı öğreneceksiniz. *Öneriler*, kullanıcının o ana kadar girdiği metne göre gerçek sonuçlardan öneriler sunar. Azure Search'teki [yeni bir önizleme seçeneği](search-api-preview.md) olan *otomatik tamamlama* özelliği, dizinden kullanıcının yazmakta olduğu girişi tamamlayan terimler önerir. Gelişmiş arama özelliklerini metin girişi sırasında kullanıcıya sunarak kullanıcının üretkenliğini artırmanın yanı sıra aradığını hızla ve kolayca bulmasını sağlayan birden fazla tekniği karşılaştıracağız.

Bu öğreticide [Azure Search .NET istemci kitaplıklarını](https://aka.ms/search-sdk) çağırmak için C# kullanan bir ASP.NET MVC tabanlı uygulama ile Azure Search REST API'sini doğrudan çağırmak için kullanılan JavaScript uygulamasının işleyişi anlatılmaktadır. Bu öğreticide kullanılan uygulama, dizini oluşturulmuş [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) örnek verilerini hedefler. NYCJobs örneğindeki önceden yapılandırılmış dizini kullanabilir veya NYCJobs örnek çözümündeki veri yükleyiciyi kullanarak kendi dizininizi oluşturabilirsiniz. Örnekte otomatik tamamlama destekli bir arama kutusu oluşturmak için [jQuery UI](https://jqueryui.com/autocomplete/) ve [XDSoft](https://xdsoft.net/jqplugins/autocomplete/) JavaScript kitaplıkları kullanılmaktadır. Bu bileşenleri Azure Search ile birlikte kullanarak arama kutunuza yazarken tamamlama özellikli otomatik tamamlama desteği eklemek için birden fazla örnek göreceksiniz.

Aşağıdaki görevleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Çözümü indirme ve yapılandırma
> * Uygulama ayarlarına arama hizmeti bilgilerini ekleme
> * Arama giriş kutusu ekleme
> * Uzak kaynaktan veri alan bir otomatik tamamlama listesi için destek ekleme 
> * .Net SDK'sını ve REST API'sini kullanarak önerileri alma ve otomatik tamamlama
> * Performansı iyileştirmek için istemci tarafında önbelleğe almayı destekleme 

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio 2017. Ücretsiz [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)'ı kullanabilirsiniz. 

* Öğretici için örnek [kaynak kodunu](https://github.com/azure-samples/search-dotnet-getting-started) indirin.

* (İsteğe bağlı) Etkin Azure hesabı ve Azure Search hizmeti. Azure hesabınız yoksa [ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz. Hizmeti sağlama konusunda yardım almak için bkz. [Arama hizmeti oluşturma](search-create-service-portal.md). Bu öğretici farklı bir örnek için oluşturulmuş olan barındırılmış NYCJobs dizini kullanılarak tamamlanabileceğinden hesap ve hizmet oluşturma adımları isteğe bağlıdır.

* (İsteğe bağlı) NYCJobs verilerini kendi Azure Search hizmetinizdeki bir dizine aktarmak için [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) örnek kodunu indirin.

> [!Note]
> Ücretsiz Azure Search hizmetini kullanıyorsanız, üç dizin sınırlandırmanız vardır. NYCJobs veri yükleyici iki dizin oluşturur. Hizmetinizde yeni dizinleri kabul edecek kadar yer olduğundan emin olun.

### <a name="set-up-azure-search-optional"></a>Azure Search'ü ayarlama (İsteğe bağlı)

NYCJobs örnek uygulamasındaki verileri kendi dizininize aktarmak istiyorsanız bu bölümdeki adımları uygulayın. Bu adım isteğe bağlıdır.  Verilen örnek dizini kullanmak istiyorsanız bir sonraki bölüme geçerek örneği çalıştırın.

1. NYCJobs örnek kodunun DataLoader klasöründeki DataLoader.sln çözüm dosyasını Visual Studio'da açın.

1. Azure Search hizmetinizin bağlantı bilgileriyle güncelleştirin.  DataLoader projesindeki App.config dosyasını açın ve TargetSearchServiceName ile TargetSearchServiceApiKey appSettings değerlerini Azure Search hizmetiniz ve Azure Search Service API Anahtarı değeriyle değiştirin.  Bu bilgilere Azure portaldan ulaşabilirsiniz.

1. Uygulamayı çalıştırmak için F5'e basın.  2 dizin oluşturulur ve NYCJobs örnek verileri içeri aktarılır.

1. Öğreticideki örnek kodundaki AutocompleteTutorial.sln çözüm dosyasını Visual Studio'da açın.  AutocompleteTutorial projesindeki Web.config dosyasını açın ve SearchServiceName ile SearchServiceApiKey yerine yukarıdaki değerleri girin.

### <a name="running-the-sample"></a>Örneği çalıştırma

Artık öğreticideki örnek uygulamayı çalıştırmaya hazırsınız.  Öğreticiyi çalıştırmak için AutocompleteTutorial.sln çözüm dosyasını Visual Studio'da açın.  Çözümde bir ASP.NET MVC projesi bulunur.  Projeyi çalıştırmak ve sayfayı istediğiniz tarayıcıda yüklemek için F5'e basın.  En üstte C# ve JavaScript seçeneklerini göreceksiniz.  C# seçeneği tarayıcıdan HomeController çağrısı yapar ve sonuçları almak için Azure Search .Net SDK'sını kullanır.  JavaScript seçeneği Azure Search REST API'sini doğrudan tarayıcıdan çağırır.  Bu seçenek denetleyiciyi akış dışında bıraktığından daha iyi performans sunacaktır.  İhtiyaçlarınıza ve dil tercihlerinize uygun seçeneği tercih edebilirsiniz.  Sayfada birden fazla otomatik tamamlama örneği ve her biri için yönergeler bulunur.  Her örnekte deneyebileceğiniz önerilen örnek metin vardır.  Gerçekleştirilen işlemleri görmek için her arama kutusuna birkaç harf yazmayı deneyin.

## <a name="how-this-works-in-code"></a>Bu kodda nasıl çalışır?

Örnekleri tarayıcıda gördünüz, şimdi ilk örneği ayrıntılı bir şekilde inceleyerek bileşenleri ve çalışma şekillerini gözden geçirelim.

### <a name="search-box"></a>Arama kutusu

Arama kutusu tüm dil seçeneklerinde aynıdır.  \Views\Home klasöründeki Index.cshtml dosyasını açın. Arama kutusu oldukça basittir:

```html
<input class="searchBox" type="text" id="example1a" placeholder="search">
```

Bu stil için bir sınıfa, JavaScript tarafından başvurulacak bir kimliğe ve yer tutucu metnine sahip olan basit bir giriş metin kutusudur.  Sihrin gerçekleştiği yer JavaScript kodudur.

### <a name="javascript-code-c"></a>JavaScript kodu (C#)

C# dili örneği, Index.cshtml dosyasındaki JavaScript kodunu kullanarak jQuery UI Autocomplete kitaplığından faydalanmaktadır.  Bu kitaplık öneri almak üzere MVC denetleyicisine asenkron çağrı yaparak arama kutusuna otomatik tamamlama deneyimi ekler.  İlk örneğin JavaScript koduna bakalım:

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

Bu kod sayfa yüklendiğinde tarayıcıda çalışarak "example1a" giriş kutusu için otomatik tamamlama özelliğini yapılandırır.  `minLength: 3`, önerilerin yalnızca arama kutusuna en az üç karakter girildiğinde gösterilmesini sağlar.  Kaynak değeri önemlidir:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

Bu satır, otomatik tamamlama API'sine arama kutusunun altında gösterilecek öğelerin listesini alacağı yeri belirtir.  Bu bir MVC projesi olduğundan HomeController.cs içindeki Suggest işlevini çağırır.  Bir sonraki bölümde bu konuya ayrıntılı olarak değineceğiz.  Ayrıca vurgulamaları, benzer öğe eşleştirmelerini ve süresi denetlemek için birkaç parametre iletir.  Otomatik tamamlama JavaScript API'si süre parametresini ekler.

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

1. İlk fark edeceğiniz şey sınıfın en üstünde InitSearch adlı bir metot olduğudur.  Bu, Azure Search hizmeti için kimliği doğrulanmış bir HTTP dizini istemcisi oluşturur.  Bunun çalışması hakkında daha fazla bilgi almak isterseniz şu öğreticiyi ziyaret edin: [Bir .NET Uygulamasından Azure Search kullanma](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

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

Sayfadaki diğer örnekler isabet vurgulama, otomatik tamamlama önerileri için yazarken tamamlama ve otomatik tamamlama sonuçlarının istemci tarafında önbelleğe alınmasını destekleyen modeller konusunda aynı düzeni kullanmaktadır.  Her birini gözden geçirerek nasıl çalıştıklarını ve arama deneyiminizde nasıl kullanabileceğinizi görebilirsiniz.

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

Bunu yukarıdaki Home denetleyicisini çağıran örnekle karşılaştırdığınızda birçok benzerlik göreceksiniz.  `minLength` ve `position` otomatik tamamlama yapılandırmaları tamamen aynıdır.  Buradaki önemli değişiklik kaynaktır.  Home denetleyicisindeki Suggest metodunu çağırmak yerine JavaScript işlevinde bir REST isteği oluşturulur ve Ajax kullanılarak yürütülür.  Ardından gelen yanıt "success" parametresinde işlenir ve kaynak olarak kullanılır.

## <a name="takeaways"></a>Paketler

Bu öğreticide otomatik tamamlamayı ve önerileri destekleyen bir arama kutusu oluşturmanın temel adımları gösterilmektedir.  Bir ASP.NET MVC uygulaması derledikten sonra önerileri almak için Azure Search .Net SDK'sını veya REST API'sini kullanabileceğinizi gördünüz.

## <a name="next-steps"></a>Sonraki adımlar

Önerileri ve otomatik tamamlamayı arama deneyiminizle tümleştirin.  .Net SDK'sını veya doğrudan REST API'sini kullanmanın Azure Search'ün gücünü arama kutusuna metin giren kullanıcılarınıza sunmanızı nasıl sağlayabileceğini inceleyin.

> [!div class="nextstepaction"]
> [Autocomplete REST API'si](https://docs.microsoft.com/en-us/rest/api/searchservice/autocomplete)
> [Suggestions REST API'si](https://docs.microsoft.com/en-us/rest/api/searchservice/suggestions)
> [Create Index REST API'sindeki modeller dizini özniteliği](https://docs.microsoft.com/en-us/rest/api/searchservice/create-index)

