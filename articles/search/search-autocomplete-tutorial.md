---
title: Öneriler ve otomatik tamamlama'ya - Azure Search arama kutusuna ekleyin.
description: Azure Search'te sorgu Eylemler typeahead öneri araçları oluşturarak ve tamamlanan koşulları ya da tümcelere bir arama kutusu doldurun istekleri formulating etkinleştirin.
manager: pablocas
author: mrcarter8
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: mcarter
ms.custom: seodec2018
ms.openlocfilehash: ed2e0bd352823a932cfea719c18e05ae6c913621
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59495745"
---
# <a name="example-add-suggestions-or-autocomplete-to-your-azure-search-application"></a>Örnek: Önerileriniz veya otomatik tamamlama, Azure Search uygulamanıza ekleyin

Bu makalede, kullanmayı öğrenin [önerileri](https://docs.microsoft.com/rest/api/searchservice/suggestions) ve [otomatik tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete) arama---yazarken Davranışları destekleyen güçlü bir arama kutusu oluşturmak için.

+ *Öneriler* önerilen sonuç her öneri şu ana kadar yazdığınız eşleşen dizinden tek bir sonuç olduğu türü olarak oluşturulur. 

+ *Otomatik Tamamlama*, [bir önizleme özelliği](search-api-preview.md), "sözcük veya bir kullanıcı şu anda yazarak tümcecik tamamlandıktan". Sonuçları döndürmek yerine, bir sorgu sonuçları döndürmek için sonra yürütebilir tamamlar. Önerileriniz gibi bir tamamlanmış sözcük veya tümcecik sorguda bir eşleşen dizini içindeki predicated. Hizmet, dizin sıfır sonuçları döndüren sorgular sunmak olmaz.

İndirme ve örnek kodu çalıştırma **DotNetHowToAutocomplete** bu özellikler değerlendirilemedi. Örnek kod ile doldurulmuş önceden oluşturulmuş bir dizin hedefleyen [NYCJobs tanıtım verileri](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs). NYCJobs dizini içeren bir [öneri aracı yapısı](index-add-suggesters.md), öneri veya otomatik tamamlama kullanma gereksinimi olan. Bir korumalı alan hizmette barındırılan hazırlanmış dizini kullanabilir veya [kendi dizininizi doldurmak](#configure-app) NYCJobs örnek çözümde bir veri yükleyici kullanılarak. 

**DotNetHowToAutocomplete** örnek gösterir hem öneriler ve otomatik tamamlama, hem de C# ve JavaScript dil sürümleri. C#Geliştiriciler kullanan bir ASP.NET MVC tabanlı uygulama adım [Azure Search .NET SDK'sı](https://aka.ms/search-sdk). Otomatik Tamamlama ve önerilen sorgu çağrıları yapmak için mantıksal HomeController.cs dosyasında bulunabilir. JavaScript geliştiricileri, doğrudan çağrıları içeren IndexJavaScript.cshtml içinde eşdeğer sorgu mantığının bulacaksınız [Azure Search REST API'sine](https://docs.microsoft.com/rest/api/searchservice/). 

Ön uç kullanıcı deneyimini dayanır her iki dil sürümleri için [jQuery kullanıcı Arabirimi](https://jqueryui.com/autocomplete/) ve [XDSoft](https://xdsoft.net/jqplugins/autocomplete/) kitaplıkları. Öneriler ve otomatik tamamlama'ya hem destekleyen arama kutusu oluşturmak için bu kitaplıkları kullanın. Arama kutusuna toplanan girişleri HomeController.cs veya IndexJavaScript.cshtml tanımlandığı şekilde, öneriler ve olanlar gibi otomatik tamamlama eylemleri ile eşlenirler.

Bu alıştırmada, aşağıdaki görevleri size yardımcı olur:

> [!div class="checklist"]
> * Arama giriş kutusunu eşleşmeler veya otomatik tamamlanan terimler için JavaScript ve sorunu isteklerinin uygulayın
> * İçinde C#, öneriler ve otomatik tamamlama eylemleri HomeController.cs tanımlayın
> * JavaScript'te, doğrudan aynı işlevselliği sağlamak için REST API'lerini çağırma

## <a name="prerequisites"></a>Önkoşullar

Azure Search hizmeti, çözüm bir hazırlanmış NYCJobs tanıtım dizini barındıran bir dinamik sanal hizmet kullandığından bu alıştırma için isteğe bağlıdır. Bu örnekte, kendi Arama hizmeti üzerinde çalıştırmak istiyorsanız, bkz. [NYC işleri yapılandırma dizini](#configure-app) yönergeler için.

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), herhangi bir sürümü. Örnek kodu ve yönergeleri ücretsiz Community edition üzerinde test edilmiştir.

* İndirme [DotNetHowToAutoComplete örnek](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete).

, Kapsamlı, kapsayan öneriler, otomatik tamamlama, çok yönlü gezinme ve istemci tarafı önbelleğe alma örnektir. Ne örnek sunan, tam açıklama için yorum ve Benioku dosyasını gözden geçirmelisiniz.

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Açık **AutocompleteTutorial.sln** Visual Studio'da. Çözüm NYC işleri demo dizin bağlantısı olan bir ASP.NET MVC projesi içeriyor.

2. Projeyi çalıştırmak ve sayfayı istediğiniz tarayıcıda yüklemek için F5'e basın.

En üstte C# ve JavaScript seçeneklerini göreceksiniz. C# Seçeneği tarayıcıdan HomeController çağırır ve sonuçları almak için Azure Search .NET SDK'sını kullanır. 

JavaScript seçeneği Azure Search REST API'sini doğrudan tarayıcıdan çağırır. Bu akışın dışında denetleyicisi gerektirdiğinden bu seçenek genellikle fark edilir derecede daha iyi performans sahip olacaktır. İhtiyaçlarınıza ve dil tercihlerinize uygun seçeneği tercih edebilirsiniz. Sayfasında her biri için bazı yönergeler ile birden fazla otomatik tamamlama örnekleri vardır. Her örnekte deneyebileceğiniz önerilen örnek metin vardır.  

Gerçekleştirilen işlemleri görmek için her arama kutusuna birkaç harf yazmayı deneyin.

## <a name="search-box"></a>Arama kutusu

Her ikisi için de C# ve JavaScript sürümleri, arama kutusuna uygulama tam olarak aynı. 

Açık **Index.cshtml** altındaki kodu görmek için klasör \Views\Home dosyası:

```html
<input class="searchBox" type="text" id="example1a" placeholder="search">
```

Stil, JavaScript ve yer tutucu metnini başvuru için bir kimlik için bir sınıf ile basit bir giriş metin kutusu budur.  Magic katıştırılmış JavaScript'te ' dir.

C# Dil örnek yararlanmak için Index.cshtml içinde JavaScript kullanan [jQuery kullanıcı Arabirimi otomatik tamamlama Kitaplığı](https://jqueryui.com/autocomplete/). Bu kitaplık, otomatik tamamlama deneyimi MVC denetleyicisi öneriler almak için zaman uyumsuz çağrıları yaparak arama kutusuna ekler. JavaScript dil IndexJavaScript.cshtml sürümüdür. Bu, Azure Search REST API çağrıları yanı sıra arama çubuğu için aşağıdaki betiği içerir.

JQuery kullanıcı Arabirimi Otomatik Tamamlama işlevi, bir istek için öneriler geçirme çağıran ilk örnek JavaScript kodunu göz atalım:

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

Yukarıdaki kodu tarayıcıda Otomatik Tamamla "example1a" giriş kutusuna jQuery kullanıcı Arabirimi yapılandırmak için sayfa yükleme çalışır.  `minLength: 3` Arama kutusuna en az üç karakter olduğunda önerileri yalnızca gösterilmesini sağlar.  Kaynak değeri önemlidir:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

Yukarıdaki satırı jQuery kullanıcı Arabirimi Otomatik Tamamlama işlevi arama kutusu altında görüntülenen öğe listesini almak nereye bildirir. Bu bir MVC projesi olduğundan, Sorgu önerileri (hakkında daha fazla sonraki bölümde Öner) döndürmek için mantığı içeren HomeController.cs Öner işlevi çağırır. Bu işlev, ayrıca Denetim vurgular, benzer öğe eşleştirmesi ve terimi birkaç parametrelerini geçirir. JavaScript API'si otomatik tamamlama dönem parametresi ekler.

### <a name="extending-the-sample-to-support-fuzzy-matching"></a>Örneği benzer öğe eşleştirmeyi destekleyecek şekilde genişletme

Benzer öğe araması, kullanıcı bir kelimeyi arama kutusuna yanlış yazsa bile yakın eşleşmelere göre sonuçlara ulaşmanızı sağlar. Gerekli olmasa da, sağlamlık typeahead deneyiminin önemli ölçüde artırır. Şimdi kaynak satırını benzer öğe eşleştirmeyi etkinleştirecek şekilde değiştirerek bir deneme yapalım.

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

### <a name="jquery-autocomplete--backed-by-azure-search-autocomplete"></a>jQuery Azure Search otomatik tamamlama tarafından desteklenen otomatik tamamlama

Şu ana kadar UX kod arama önerileri ortalanmış. Azure Search otomatik tamamlama için bir istekte geçirme jQuery kullanıcı Arabirimi Otomatik Tamamlama işlevi (satır 91 içinde Index.cshtml), sonraki kod bloğu gösterir:

```javascript
$(function () {
    // using modified jQuery Autocomplete plugin v1.2.6 http://xdsoft.net/jqplugins/autocomplete/
    // $.autocomplete -> $.autocompleteInline
    $("#example2").autocompleteInline({
        appendMethod: "replace",
        source: [
            function (text, add) {
                if (!text) {
                    return;
                }

                $.getJSON("/home/autocomplete?term=" + text, function (data) {
                    if (data && data.length > 0) {
                        currentSuggestion2 = data[0];
                        add(data);
                    }
                });
            }
        ]
    });

    // complete on TAB and clear on ESC
    $("#example2").keydown(function (evt) {
        if (evt.keyCode === 9 /* TAB */ && currentSuggestion2) {
            $("#example2").val(currentSuggestion2);
            return false;
        } else if (evt.keyCode === 27 /* ESC */) {
            currentSuggestion2 = "";
            $("#example2").val("");
        }
    });
});
```

## <a name="c-example"></a>C# örneği

Biz web sayfası için JavaScript kodunu gözden geçirdikten sonra göz atalım C# gerçekte Azure Search .NET SDK'sını kullanarak eşleşmeler alır denetleyicisi sunucu tarafı kodu.

Açık **HomeController.cs** denetleyicileri dizin altında dosya. 

İlk şey yapabilirsiniz adlı sınıfı üst kısmındaki bir yöntemdir `InitSearch`. Bu, Azure Search hizmeti için kimliği doğrulanmış bir HTTP dizini istemcisi oluşturur. Daha fazla bilgi için [bir .NET uygulamasından Azure Search kullanma](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).

41 satırında Öner işlevi dikkat edin. Dayanır [DocumentsOperationsExtensions.Suggest yöntemi](/dotnet/api/microsoft.azure.search.documentsoperationsextensions.suggest?view=azure-dotnet-preview).

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

Suggest işlevi, arama terimi girişine ek olarak isabet vurgularının veya benzer öğe eşleştirme özelliğinin kullanılıp kullanılmadığını belirleyen iki parametre alır. Yöntemi oluşturur bir [SuggestParameters nesne](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggestparameters?view=azure-dotnet), hangi sonra geçirilen Öner API için. Ardından alınan sonuç istemcide gösterilebilmesi için JSON biçimine dönüştürülür.

69. satırda, Otomatik Tamamlama işlevi dikkat edin. Dayanır [DocumentsOperationsExtensions.Autocomplete yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.autocomplete?view=azure-dotnet-preview).

```csharp
public ActionResult AutoComplete(string term)
{
    InitSearch();
    //Call autocomplete API and return results
    AutocompleteParameters ap = new AutocompleteParameters()
    {
        AutocompleteMode = AutocompleteMode.OneTermWithContext,
        UseFuzzyMatching = false,
        Top = 5
    };
    AutocompleteResult autocompleteResult = _indexClient.Documents.Autocomplete(term, "sg", ap);

    // Conver the Suggest results to a list that can be displayed in the client.
    List<string> autocomplete = autocompleteResult.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = autocomplete
    };
}
```

Otomatik Tamamlama işlevi arama terimi girişleri alır. Yöntemi oluşturur bir [AutoCompleteParameters nesne](https://docs.microsoft.com/rest/api/searchservice/autocomplete). Ardından alınan sonuç istemcide gösterilebilmesi için JSON biçimine dönüştürülür.

(İsteğe bağlı) Suggest işlevinin başlangıcına bir kesme noktası ekleyin ve kodu adım adım inceleyin. SDK ve yönteminden döndürülen sonuç nasıl dönüştürülür tarafından döndürülen yanıt dikkat edin.

Sayfadaki diğer örnekler, isabet vurgulama eklemek için aynı deseni ve istemci tarafı önbelleğe alma otomatik tamamlama sonuçlarını desteklemek için modelleri izleyin. Her birini gözden geçirerek nasıl çalıştıklarını ve arama deneyiminizde nasıl kullanabileceğinizi görebilirsiniz.

## <a name="javascript-example"></a>JavaScript örneği

Bir Javascript uygulamasını otomatik tamamlama ve öneriler işlemi ve dizini belirtmek için kaynak olarak bir URI kullanarak REST API'sini çağırır. 

JavaScript uygulamasını gözden geçirmek için açık **IndexJavaScript.cshtml**. JQuery kullanıcı Arabirimi Otomatik Tamamlama işlevi ayrıca arama kutusuna arama terimi girişleri toplamak için kullanılır ve eşleşme önerilen veya koşulları tamamlandı almak için Azure Search için zaman uyumsuz çağrı yapmaya dikkat edin. 

İlk örneğin JavaScript koduna bakalım:

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

Bunu yukarıdaki Home denetleyicisini çağıran örnekle karşılaştırdığınızda birçok benzerlik göreceksiniz.  Otomatik Tamamlama yapılandırmasını `minLength` ve `position` tam olarak aynıdır. 

Buradaki önemli değişiklik kaynaktır. Giriş denetleyicide Öner yöntemini çağırmak, yerine REST isteği içinde JavaScript işlevi oluşturulur ve Ajax kullanarak. Ardından gelen yanıt "success" parametresinde işlenir ve kaynak olarak kullanılır.

REST çağrılarını URI'leri belirtmek için kullanın olup olmadığını bir [otomatik tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete) veya [önerileri](https://docs.microsoft.com/rest/api/searchservice/suggestions) API Çağrının yapıldığı. Aşağıdaki bir URI'leri satırlarda 9 ve 10, sırasıyla şunlardır.

```javascript
var suggestUri = "https://" + searchServiceName + ".search.windows.net/indexes/" + indexName + "/docs/suggest?api-version=" + apiVersion;
var autocompleteUri = "https://" + searchServiceName + ".search.windows.net/indexes/" + indexName + "/docs/autocomplete?api-version=" + apiVersion;
```

148 satırında çağıran bir komut dosyası bulabilirsiniz `autocompleteUri`. İlk çağrıda `suggestUri` 39 satırdadır.

> [!Note]
> JavaScript'te hizmet REST çağrıları yapma sunulur burada REST API'sinin, kullanışlı bir örnek olarak, ancak en iyi yöntem veya öneri olarak yorumlanmamalıdır. Bir API anahtarını ve uç nokta bir betikte dahilini hizmetinizin hizmet reddi saldırılarını betik bu değerleri okuyabilirsiniz herkes kadar açılır. JavaScript öğrenme amacıyla kullanmak için kendi güvenli, belki de ücretsiz hizmette barındırılan dizinlerin Java kullanmanızı öneririz veya C# üretim kodunda dizin oluşturma ve sorgu işlemleri için. 

<a name="configure-app"></a>

## <a name="configure-nycjobs-to-run-on-your-service"></a>Hizmetinizde çalıştırılacak NYCJobs yapılandırın

Şimdiye kadar barındırılan NYCJobs tanıtım dizini kullandınız. Tüm dizin de dahil olmak üzere kodun tam görünürlük istiyorsanız oluşturmak ve kendi arama hizmetinizi dizin yüklemek için bu yönergeleri izleyin.

1. [Azure Search hizmeti oluşturma](search-create-service-portal.md) veya [mevcut bir hizmet bulma](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) geçerli aboneliğinizdeki. Bu örnek için ücretsiz bir hizmet kullanabilirsiniz. 

   > [!Note]
   > Ücretsiz Azure Search hizmetini kullanıyorsanız, üç dizin sınırlandırmanız vardır. NYCJobs veri yükleyici iki dizin oluşturur. Hizmetinizde yeni dizinleri kabul edecek kadar yer olduğundan emin olun.

1. İndirme [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) örnek kodu.

1. NYCJobs örnek kodu DataLoader klasöründe açın **DataLoader.sln** Visual Studio'da.

1. Azure Search hizmetiniz için bağlantı bilgilerini ekleyin. DataLoader projesindeki App.config dosyasını açın ve TargetSearchServiceName ile TargetSearchServiceApiKey appSettings değerlerini Azure Search hizmetiniz ve Azure Search Service API Anahtarı değeriyle değiştirin. Bu bilgilere Azure portaldan ulaşabilirsiniz.

1. İki dizin oluşturma ve NYCJob örnek verileri içeri aktarma uygulamayı başlatmak için F5 tuşuna basın.

1. Açık **AutocompleteTutorial.sln** ve Web.config dosyasında Düzenle **AutocompleteTutorial** proje. SearchServiceName ve SearchServiceApiKey değerlerini arama hizmetiniz için geçerli olan değerlerle değiştirin.

1. Uygulamayı çalıştırmak için F5'e basın. Örnek web uygulamasını varsayılan tarayıcınızda açılır. Deneyimi korumalı alan sürümü ile aynıdır, yalnızca dizin ve verileri hizmetinizde barındırılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekte, bir arama kutusu otomatik tamamlama ve öneriler destekleyen oluşturmaya yönelik temel adımlar gösterilmektedir. Gördüğünüz bir ASP.NET MVC uygulaması oluşturma ve öneriler almak için Azure Search .NET SDK veya REST API'sini kullanın.

Sonraki adım olarak, arama deneyiminizi tümleştirme öneriler ve otomatik tamamlama'ya çalışıyor. Aşağıdaki başvuru makalelerimize yardımcı olmalıdır.

> [!div class="nextstepaction"]
> [Autocomplete REST API'si](https://docs.microsoft.com/rest/api/searchservice/autocomplete)
> [Suggestions REST API'si](https://docs.microsoft.com/rest/api/searchservice/suggestions)
> [Create Index REST API'sindeki modeller dizini özniteliği](https://docs.microsoft.com/rest/api/searchservice/create-index)

