---
title: "Öğretici: Tek sayfa web uygulaması - Bing Web araması API'si oluşturma"
titleSuffix: Azure Cognitive Services
description: Bu tek sayfalı uygulama, Bing Web Araması API'si kullanılarak tek sayfalı bir uygulamada ilgili arama sonuçlarının nasıl alınabileceği, ayrıştırılabileceği ve görüntülenebileceğini gösterir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: tutorial
ms.date: 09/12/2018
ms.author: aahi
ms.openlocfilehash: 6c28b02d68239bac658954caf447b6ff738c1b65
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122532"
---
# <a name="tutorial-create-a-single-page-app-using-the-bing-web-search-api"></a>Öğretici: Bing Web araması API'si kullanarak tek sayfalı uygulama oluşturma

Bu tek sayfalı uygulama Bing Web Araması API'sinden arama sonuçlarını almayı, ayrıştırmayı ve görüntülemeyi gösterir. Öğretici standart HTML ile CSS kullanır ve JavaScript koduna odaklanır. HTML, CSS ve JS dosyaları, hızlı başlangıç yönergeleriyle birlikte [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/Tutorials/Bing-Web-Search)'da sağlanır.

Bu örnek uygulama şunları yapabilir:

> [!div class="checklist"]
> * Arama seçenekleriyle Bing Web Araması API'sini çağırma
> * Web, resim, haber ve video sonuçlarını görüntüleme
> * Sonuçları sayfalandırma
> * Abonelik anahtarlarını yönetme
> * Hataları işleme

Bu uygulamayı kullanmak için Bing Arama API'lerine sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerekir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Uygulamayı çalıştırmak için ihtiyacınız olacak birkaç şey:

* Node.js 8 veya üstü
* Abonelik anahtarı

## <a name="get-the-source-code-and-install-dependencies"></a>Kaynak kodu alma ve bağımlılıkları yükleme

İlk adım, örnek uygulamanın kaynak koduyla depoyu kopyalamaktır.

```console
git clone https://github.com/Azure-Samples/cognitive-services-REST-api-samples.git
```

Ardından `npm install` komutunu çalıştırın. Bu öğreticide tek bağımlılık Express.js'dir.

```console
cd <path-to-repo>/cognitive-services-REST-api-samples/Tutorials/Bing-Web-Search
npm install
```

## <a name="app-components"></a>Uygulama bileşenleri

Derlediğimiz örnek uygulama dört bölümden oluşur:

* `bing-web-search.js` - Express.js uygulamamız. İstek/yanıt mantığını ve yönlendirmeyi işler.
* `public/index.html` - Uygulamamızın çatısı; verilerin kullanıcıya nasıl gösterileceğini tanımlar.
* `public/css/styles.css` - Yazı tipi, renk, metin boyutu gibi sayfa stillerini tanımlar.
* `public/js/scripts.js` - Bing Web Araması API'sine istek göndermek, abonelik anahtarlarını yönetmek, yanıtları işlemek, ayrıştırmak ve sonuçları görüntülemek için gereken mantığı içerir.

Bu öğretici `scripts.js` dosyasına ve Bing Web Araması API'sini çağırıp yanıtı işlemek için gereken mantığa odaklanır.

## <a name="html-form"></a>HTML formu

`index.html`, kullanıcıların arama yapmasına ve arama seçeneklerini belirtmesine olanak sağlayan bir form içerir. Form gönderilip `scripts.js` dosyasında tanımlanan `bingWebSearch()` yöntemi çağrıldığında, `onsubmit` özniteliği çağrılır. Üç bağımsız değişken alır:

* Arama sorgusu
* Belirtilen seçenekler
* Abonelik anahtarı

```html
<form name="bing" onsubmit="return bingWebSearch(this.query.value,
    bingSearchOptions(this), getSubscriptionKey())">
```

## <a name="query-options"></a>Sorgu seçenekleri

HTML formu, [Bing Web Araması API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query-parameters)'deki sorgu parametrelerine eşlenen seçenekler içerir. Bu tabloda, örnek uygulamayı kullanarak kullanıcıların arama sonuçlarını nasıl filtreleyebileceğini gösteren bir döküm sağlanır:

| Parametre | Açıklama |
|-----------|-------------|
| `query` | Sorgu dizesinin girileceği metin alanı. |
| `where` | Pazarın (konum ve dil) seçileceği açılan menü. |
| `what` | Belirli sonuç türlerini yükseltmek için onay kutuları. Örneğin görüntülerin yükseltilmesi, arama sonuçlarında görüntülerin derecelendirmesini yükseltir. |
| `when` | Kullanıcının arama sonuçlarını bugün, bu hafta veya bu ay ile sınırlandırmasına olanak tanıyan bir açılan menü. |
| `safe` | Yetişkinlere yönelik içeriği filtreleyip dışarıda bırakan Bing Güvenli Araması'nı etkinleştirmek için bir onay kutusu. |
| `count` | Gizli alan. Her istekte döndürülecek arama sonuçlarının sayısı. Sayfada daha az veya daha fazla sonuç görüntülemek için bu değeri değiştirin. |
| `offset` | Gizli alan. İstekteki ilk arama sonucunun göreli konumu; sayfalama için kullanılır. Her yeni istekle birlikte `0` değerine sıfırlanır. |

> [!NOTE]
> Bing Web Araması API'si arama sonuçlarını daraltmak için ek sorgu parametreleri sağlar. Bu örnekte yalnızca birkaç parametre kullanılır. Sağlanan parametrelerin tam listesi için bkz. [Bing Web Araması API'si v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query-parameters).

`bingSearchOptions()` işlevi, Bing Arama API'sinin gerektirdiği biçime uyacak şekilde bu seçenekleri dönüştürür.

```javascript
// Build query options from selections in the HTML form.
function bingSearchOptions(form) {

    var options = [];
    // Where option.
    options.push("mkt=" + form.where.value);
    // SafeSearch option.
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    // Freshness option.
    if (form.when.value.length) options.push("freshness=" + form.when.value);
    var what = [];
    for (var i = 0; i < form.what.length; i++)
        if (form.what[i].checked) what.push(form.what[i].value);
    // Promote option.
    if (what.length) {
        options.push("promote=" + what.join(","));
        options.push("answerCount=9");
    }
    // Count option.
    options.push("count=" + form.count.value);
    // Offset option.
    options.push("offset=" + form.offset.value);
    // Hardcoded text decoration option.
    options.push("textDecorations=true");
    // Hardcoded text format option.
    options.push("textFormat=HTML");
    return options.join("&");
}
```

`SafeSearch` `strict`, `moderate` veya `off` olarak ayarlanabilir; Bing Web Araması'nın varsayılan ayarı `moderate` ayarıdır. Bu formda iki durumu olan bir onay kutusu kullanılıyor. Bu kod parçacığında, SafeSearch `strict` veya `off` olarak ayarlanmıştır; `moderate` kullanılmaz.

**Yükselt** onay kutularından herhangi bir seçildiyse, sorguya `answerCount` parametresi eklenir. `promote` parametresi kullanıldığında `answerCount` gereklidir. Bu kod parçacığında, tüm kullanılabilir sonuç türlerinin döndürülmesi için değer `9` olarak ayarlanmıştır.
> [!NOTE]
> Sonuç türünün yükseltilmesi, bunun arama sonuçlarına eklenmesini *garanti* etmez. Bunun yerine, yükseltme işlemi bu tür sonuçların derecelendirmesini kendi normal derecelerine göre yükseltir. Aramaları belirli sonuç türleriyle sınırlandırmak için, `responseFilter` sorgu parametresini kullanın ya da Bing Resim Arama veya Bing Haber Arama gibi daha belirgin bir uç noktayı çağırın.

`textDecoration` ve `textFormat` sorgu parametreleri betiğe sabit kodlanmıştır ve arama teriminin arama sonuçlarında kalın gösterilmesine neden olur. Bu parametreler gerekli değildir.

## <a name="manage-subscription-keys"></a>Abonelik anahtarlarını yönetme

Bing Arama API'si abonelik anahtarının basit kodlanmasını önlemek için, bu örnek uygulama tarayıcının kalıcı depolamasını kullanarak abonelik anahtarını depolar. Hiçbir abonelik anahtarı depolanmazsa, kullanıcıdan bir anahtar girmesi istenir. Abonelik anahtarı API tarafından reddedilirse, kullanıcının abonelik anahtarını yeniden girmesi istenir.

`getSubscriptionKey()` işlevi, `storeValue` ve `retrieveValue` işlevlerini kullanarak kullanıcının abonelik anahtarını depolar ve alır. Bu işlevler, destekleniyorsa `localStorage` nesnesini veya tanımlama bilgilerini kullanır.

```javascript
// Cookie names for stored data.
API_KEY_COOKIE   = "bing-search-api-key";
CLIENT_ID_COOKIE = "bing-search-client-id";

BING_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/search";

// See source code for storeValue and retrieveValue definitions.

// Get stored subscription key, or prompt if it isn't found.
function getSubscriptionKey() {
    var key = retrieveValue(API_KEY_COOKIE);
    while (key.length !== 32) {
        key = prompt("Enter Bing Search API subscription key:", "").trim();
    }
    // Always set the cookie in order to update the expiration date.
    storeValue(API_KEY_COOKIE, key);
    return key;
}
```

Daha önce gördüğümüz gibi, form gönderildiğinde `onsubmit` çağrılarak `bingWebSearch` çağrısı yapılır. Bu işlev isteği başlatır ve gönderir. Her gönderimde isteğin kimliğini doğrulamak için `getSubscriptionKey` çağrılır.

## <a name="call-bing-web-search"></a>Bing Web Araması'nı çağırma

Sorgu, seçenekler dizesi ve abonelik anahtarı verili durumdayken, `BingWebSearch` işlevi Bing Web Araması uç noktasını çağırmak için bir `XMLHttpRequest` nesnesi oluşturur.

```javascript
// Perform a search constructed from the query, options, and subscription key.
function bingWebSearch(query, options, key) {
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;

    showDiv("noresults", "Working. Please wait.");
    hideDivs("pole", "mainline", "sidebar", "_json", "_http", "paging1", "paging2", "error");

    var request = new XMLHttpRequest();
    var queryurl = BING_ENDPOINT + "?q=" + encodeURIComponent(query) + "&" + options;

    // Initialize the request.
    try {
        request.open("GET", queryurl);
    }
    catch (e) {
        renderErrorMessage("Bad request (invalid URL)\n" + queryurl);
        return false;
    }

    // Add request headers.
    request.setRequestHeader("Ocp-Apim-Subscription-Key", key);
    request.setRequestHeader("Accept", "application/json");
    var clientid = retrieveValue(CLIENT_ID_COOKIE);
    if (clientid) request.setRequestHeader("X-MSEdge-ClientID", clientid);

    // Event handler for successful response.
    request.addEventListener("load", handleBingResponse);

    // Event handler for errors.
    request.addEventListener("error", function() {
        renderErrorMessage("Error completing request");
    });

    // Event handler for an aborted request.
    request.addEventListener("abort", function() {
        renderErrorMessage("Request aborted");
    });

    // Send the request.
    request.send();
    return false;
}
```

Başarılı bir isteğin ardından, `load` olay işleyicisi çağrılır ve `handleBingResponse` işlevini çağırır. `handleBingResponse`, sonuç nesnesini ayrıştırır, sonuçları görüntüler ve başarısız istekler için hata mantığını içerir.

```javascript
function handleBingResponse() {
    hideDivs("noresults");

    var json = this.responseText.trim();
    var jsobj = {};

    // Try to parse results object.
    try {
        if (json.length) jsobj = JSON.parse(json);
    } catch(e) {
        renderErrorMessage("Invalid JSON response");
        return;
    }

    // Show raw JSON and the HTTP request.
    showDiv("json", preFormat(JSON.stringify(jsobj, null, 2)));
    showDiv("http", preFormat("GET " + this.responseURL + "\n\nStatus: " + this.status + " " +
        this.statusText + "\n" + this.getAllResponseHeaders()));

    // If the HTTP response is 200 OK, try to render the results.
    if (this.status === 200) {
        var clientid = this.getResponseHeader("X-MSEdge-ClientID");
        if (clientid) retrieveValue(CLIENT_ID_COOKIE, clientid);
        if (json.length) {
            if (jsobj._type === "SearchResponse" && "rankingResponse" in jsobj) {
                renderSearchResults(jsobj);
            } else {
                renderErrorMessage("No search results in JSON response");
            }
        } else {
            renderErrorMessage("Empty response (are you sending too many requests too quickly?)");
        }
    }

    // Any other HTTP response is considered an error.
    else {
        // 401 is unauthorized; force a re-prompt for the user's subscription
        // key on the next request.
        if (this.status === 401) invalidateSubscriptionKey();

        // Some error responses don't have a top-level errors object, if absent
        // create one.
        var errors = jsobj.errors || [jsobj];
        var errmsg = [];

        // Display the HTTP status code.
        errmsg.push("HTTP Status " + this.status + " " + this.statusText + "\n");

        // Add all fields from all error responses.
        for (var i = 0; i < errors.length; i++) {
            if (i) errmsg.push("\n");
            for (var k in errors[i]) errmsg.push(k + ": " + errors[i][k]);
        }

        // Display Bing Trace ID if it isn't blocked by CORS.
        var traceid = this.getResponseHeader("BingAPIs-TraceId");
        if (traceid) errmsg.push("\nTrace ID " + traceid);

        // Display the error message.
        renderErrorMessage(errmsg.join("\n"));
    }
}
```

> [!IMPORTANT]
> Başarılı bir HTTP isteği, aramanın kendisinin başarılı olduğu anlamına *gelmez*. Arama işleminde hata oluşursa, Bing Web Araması API'si 200 olmayan bir HTTP durum kodu döndürür ve JSON yanıtına hata bilgilerini ekler. İstekte hız sınırlaması varsa API boş yanıt döndürür.

Önceki işlevlerin ikisinde de kodun büyük bölümü hata işlemeye ayrılmıştır. Şu aşamalarda hata oluşabilir:

| Aşama | Olası hatalar | İşleyen |
|-------|--------------------|------------|
| İstek nesnesini oluşturma | Geçersiz URL | `try` / `catch` bloğu |
| İstekte bulunma | Ağ hataları, durdurulan bağlantılar | `error` ve `abort` olay işleyicileri |
| Aramayı gerçekleştirme | Geçersiz istek, geçersiz JSON, hız sınırları | `load` olay işleyicisindeki testler |

Hatalar `renderErrorMessage()` çağrısı yapılarak işlenir. Yanıt tüm hata testlerinden geçerse, arama sonuçlarını görüntülemek için `renderSearchResults()` çağrılır.

## <a name="display-search-results"></a>Arama sonuçlarını görüntüleme

Bing Web Araması API'si tarafından döndürülen sonuçlarla ilişkili [kullanım ve görüntüleme gereksinimleri](useanddisplayrequirements.md) vardır. Yanıtta çeşitli yanıt türleri bulunabileceğinden, en üst düzey `WebPages` koleksiyonunu yinelemek yeterli olmaz. Bunun yerine, örnek uygulama sonuçları belirtimlere göre sıralamak için `RankingResponse` kullanır.

> [!NOTE]
> Tek bir sonuç türü istiyorsanız, `responseFilter` sorgu parametresini kullanın veya Bing Resim Araması gibi diğer Bing Araması uç noktalarını kullanmayı göz önünde bulundurun.

Her yanıtta bir `RankingResponse` nesnesi vardır ve bu nesne en çok üç koleksiyon içerebilir: `pole`, `mainline` ve `sidebar`. `pole` (varsa) en ilgili arama sonucudur ve belirgin bir şekilde görüntülenmelidir. `mainline` arama sonuçlarının büyük bölümünü içerir ve hemen `pole` koleksiyonundan sonra görüntülenir. `sidebar` ikincil arama sonuçlarını içerir. Mümkünse, bu sonuçların kenar çubuğunda görüntülenmesi gerekir. Ekran sınırlarından dolayı kenar çubuğunu görüntülemek pratik değilse, bu sonuçlar `mainline` sonuçlarından sonra gösterilmelidir.

Her `RankingResponse`, sonuçların nasıl sıralanacağını belirten bir `RankingItem` dizisi içerir. Örnek uygulamamızda, sonucu tanımlamak için `answerType` ve `resultIndex` parametreleri kullanılır.

> [!NOTE]
> Sonuçları tanımlamanın ve derecelendirmenin başka yolları da vardır. Daha fazla bilgi için bkz. [Sonuçları görüntülemek için derecelendirmeyi kullanma](rank-results.md).

Şimdi koda bir göz atalım:

```javascript
// Render the search results from the JSON response.
function renderSearchResults(results) {

    // If spelling was corrected, update the search field.
    if (results.queryContext.alteredQuery)
        document.forms.bing.query.value = results.queryContext.alteredQuery;

    // Add Prev / Next links with result count.
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);

    // Render the results for each section.
    for (section in {pole: 0, mainline: 0, sidebar: 0}) {
        if (results.rankingResponse[section])
            showDiv(section, renderResultsItems(section, results));
    }
}
```

`renderResultsItems()` işlevi her `RankingResponse` koleksiyonundaki öğelerde yinelenir, `answerType` ve `resultIndex` değerlerini kullanarak her derecelendirme sonucunu bir arama sonucuna eşler ve HTML'yi oluşturmak için uygun işleme işlevini çağırır. Öğe için `resultIndex` belirtilmediyse, `renderResultsItems()` bu türdeki tüm sonuçlarda yinelenir ve her öğe için işleme işlevini çağırır. Sonuçta elde edilen HTML, `index.html` dosyasındaki uygun `<div>` öğesine eklenir.

```javascript
// Render search results from the RankingResponse object per rank response and
// use and display requirements.
function renderResultsItems(section, results) {

    var items = results.rankingResponse[section].items;
    var html = [];
    for (var i = 0; i < items.length; i++) {
        var item = items[i];
        // Collection name has lowercase first letter while answerType has uppercase
        // e.g. `WebPages` RankingResult type is in the `webPages` top-level collection.
        var type = item.answerType[0].toLowerCase() + item.answerType.slice(1);
        if (type in results && type in searchItemRenderers) {
            var render = searchItemRenderers[type];
            // This ranking item refers to ONE result of the specified type.
            if ("resultIndex" in item) {
                html.push(render(results[type].value[item.resultIndex], section));
            // This ranking item refers to ALL results of the specified type.
            } else {
                var len = results[type].value.length;
                for (var j = 0; j < len; j++) {
                    html.push(render(results[type].value[j], section, j, len));
                }
            }
        }
    }
    return html.join("\n\n");
}
```

## <a name="review-renderer-functions"></a>İşleyici işlevlerini gözden geçirme

Örnek uygulamamızdaki `searchItemRenderers` nesnesinde, her tür arama sonucu için HTML oluşturan işlevler vardır.

```javascript
// Render functions for each result type.
searchItemRenderers = {
    webPages: function(item) { ... },
    news: function(item) { ... },
    images: function(item, section, index, count) { ... },
    videos: function(item, section, index, count) { ... },
    relatedSearches: function(item, section, index, count) { ... }
}
```

> [!IMPORTANT]
> Örnek uygulamanın web sayfaları, haberler, görüntüler, videolar ve ilgili aramalar için işleyicileri vardır. Uygulamanızın alabileceği her tür sonuç için işleyicilere ihtiyacı olacaktır. Bunlar hesaplamaları, yazım önerilerini, varlıkları, saat dilimlerini ve tanımları içerebilir.

İşleme işlevlerinden bazıları yalnızca `item` parametresini kabul eder. Diğerleri, bağlama göre öğeleri farklı işlemek için kullanılabilen ek parametreler de kabul eder. Bu bilgileri kullanmayan bir işleyicinin bu parametreleri kabul etmesi gerekmez.

Bağlam bağımsız değişkenleri şunlardır:

| Parametre  | Açıklama |
|------------|-------------|
| `section` | Öğelerin gösterildiği sonuç bölümü (`pole`, `mainline` veya `sidebar`). |
| `index`<br>`count` | `RankingResponse` öğesi verili bir koleksiyondaki tüm sonuçların görüntüleneceğini belirtirse sağlanır; aksi takdirde `undefined` olur. Öğenin koleksiyonu içindeki dizin ve bu koleksiyondaki öğelerin toplam sayısı. Bu bilgileri kullanarak sonuçları numaralandırabilir, ilk veya son sonuç için farklı bir HTML oluşturabilir ve başka işlemler yapabilirsiniz. |

Örnek uygulamada, hem `images` hem de `relatedSearches` işleyicileri oluşturulan HTML'yi özelleştirmek için bağlam bağımsız değişkenlerini kullanır. `images` işleyicisine daha yakından bakalım:

```javascript
searchItemRenderers = {
    // Render image result with thumbnail.
    images: function(item, section, index, count) {
        var height = 60;
        var width = Math.round(height * item.thumbnail.width / item.thumbnail.height);
        var html = [];
        if (section === "sidebar") {
            if (index) html.push("<br>");
        } else {
            if (!index) html.push("<p class='images'>");
        }
        html.push("<a href='" + item.hostPageUrl + "'>");
        var title = escape(item.name) + "\n" + getHost(item.hostPageDisplayUrl);
        html.push("<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width +
            "' height=" + height + " width=" + width + " title='" + title + "' alt='" + title + "'>");
        html.push("</a>");
        return html.join("");
    },
    // Other renderers are omitted from this sample...
}
```

Görüntü işleyicisi:

* Görüntünün küçük resim boyutunu hesaplar (genişlik değişiklik gösterir, yükseklik ise 60 piksele sabitlenmiştir).
* Bağlama göre görüntü sonucunun önüne gelen HTML'yi ekler.
* Resmi içeren sayfaya bağlanan HTML `<a>` etiketini oluşturur.
* Resmin küçük resmini görüntülemek için HTML `<img>` etiketini oluşturur.

Görüntü işleyicisi, bulundukları yere bağlı olarak sonuçları farklı görüntülemek için `section` ve `index` bağımsız değişkenlerini kullanır. Kenar çubuğunda görüntü sonuçlarının arasına satır sonu (`<br>` etiketi) eklenerek, kenar çubuğunun bir görüntü sütunu göstermesi sağlanır. Diğer bölümlerde, ilk görüntü sonucunun (`(index === 0)`) önüne bir `<p>` etiketi gelir.

Küçük resim boyutu hem `<img>` etiketinde hem de küçük resmin URL'sindeki `h` ve `w` alanlarında kullanılır. `title` ve `alt` öznitelikleri (görüntünün metin açıklaması), görüntü adından ve URL'deki ana bilgisayar adından oluşturulur.

Burada, örnek uygulamada görüntülerin nasıl gösterildiğine ilişkin bir örnek verilmiştir:

![[Bing görüntü sonuçları]](media/cognitive-services-bing-web-api/web-search-spa-images.png)

## <a name="persist-the-client-id"></a>İstemci kimliğinin kalıcı olmasını sağlama

Bing arama API'lerinden gelen yanıtlar, birbirini izleyen her başarılı birlikte API'ye geri gönderilmesi gereken bir `X-MSEdge-ClientID` üst bilgisi içerir. Uygulamanız birden çok Bing Arama API'si kullanıyorsa, her istekle birlikte hizmetler arasında aynı istemci kimliğinin gönderildiğinden emin olun.

Böylelikle `X-MSEdge-ClientID` üst bilgisi sayesinde Bing API'leri kullanıcının aramalarını ilişkilendirebilir. İlk olarak, Bing arama alt yapısının geçmiş bağlamı aramalara uygulayarak isteğe daha uygun sonuçlar bulabilmesini sağlar. Kullanıcı daha önce yelkencilikle ilgili terim aramaları yaptıysa, daha sonra yapılan "düğümler" araması tercihen yelkencilikte kullanılan düğümler hakkında bilgi döndürebilir. İkincisi, Bing yeni özellikleri geniş ölçekte kullanıma sunmadan önce bunları denemesi için rastgele kullanıcılar seçebilir. Her istekle birlikte aynı istemci kimliğinin sağlanması, özelliği görmesi tercih edilen kullanıcıların bunu sürekli görebilmesini sağlar. İstemci kimliği olmadan, özellik kullanıcıya arama sonuçlarında rastgele gösterilebilir ve gösterilmeyebilir.

Çıkış Noktaları Arası Kaynak Paylaşımı (CORS) gibi tarayıcı güvenlik ilkeleri, örnek uygulamanın `X-MSEdge-ClientID` üst bilgisine erişmesini engelleyebilir. Bu sınırlama, arama sonucunun kaynağı istekte bulunan sayfadan farklı olduğunda ortaya çıkar. Üretim ortamında, Web sayfasıyla aynı etki alanında API çağrısı yapan bir sunucu tarafı betiği barındırarak bu ilkeye uymalısınız. Betiğin kaynağı Web sayfasıyla aynı olduğundan, `X-MSEdge-ClientID` üst bilgisi JavaScript'in kullanımına sunulur.

> [!NOTE]
> Üretim ortamındaki bir Web uygulamasında, isteği sunucu tarafından gerçekleştirmeniz gerekir. Aksi takdirde, Bing Arama API'si abonelik anahtarınız web sayfasına eklenmelidir ve bu durumda kaynağı görüntüleyen herkes tarafından görülebilir. API abonelik anahtarınız altında gerçekleştirilen tüm kullanım, yetkisiz tarafların yaptığı istekler bile size faturalandırılır; dolayısıyla anahtarınızı açıklamamanız önemlidir.

Geliştirme amacıyla, CORS ara sunucusu aracılığıyla bir istek gönderebilirsiniz. Bu tür bir ara sunucudan gelen yanıtta, yanıt üst bilgilerini beyaz listeye alan ve JavaScript'in kullanımına sunan `Access-Control-Expose-Headers` üst bilgisi bulunur.

Örnek uygulamamızın istemci kimliği üst bilgisine erişebilmesi için CORS ara sunucusu kolayca yüklenebilir. Şu komutu çalıştırın:

```console
npm install -g cors-proxy-server
```

Sonra, `script.js` dosyasındaki Bing Web Araması uç noktasını şöyle değiştirin:

```javascript
http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search
```

Şu komutla CORS ara sunucusunu başlatın:

```console
cors-proxy-server
```

Örnek uygulamayı kullanırken komut penceresini açık bırakın; pencere kapatılırsa ara sunucu durdurulur. Arama sonuçlarının alt kısmındaki genişletilebilir HTTP Üst Bilgileri bölümünde `X-MSEdge-ClientID` üst bilgisi görünür durumda olmalıdır. Bunun her istekte aynı olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing Web Araması API'si v7 başvurusu](//docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
