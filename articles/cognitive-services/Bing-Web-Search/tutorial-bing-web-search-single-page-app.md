---
title: Bing Web araması tek sayfa Web uygulaması | Microsoft Docs
description: Bir tek sayfalı Web uygulamasında Bing Web arama API kullanmayı gösterir.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-jerkin
ms.openlocfilehash: f22e38a1d6ee4042684b9822b58669bed6fe29a0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354604"
---
# <a name="tutorial-single-page-web-app"></a>Öğretici: Tek sayfalı Web uygulaması

Bing Web arama API Web ara ve değişken türleri için bir arama sorgusu ilgili sonuçlarını elde etmenizi sağlar. Bu öğreticide, biz arama sonuçlarını görüntülemek için Bing Web arama API'sini kullanan bir tek sayfalı Web uygulaması oluşturma sayfasındaki sağ. Uygulama, HTML, CSS ve JavaScript bileşenleri içerir.

<!-- Remove until this can be replaced with a sanitized version.
![[Single-page Bing Web Search app]](media/cognitive-services-bing-web-api/web-search-spa-demo.png)
-->

> [!NOTE]
> Sayfanın altındaki JSON ve HTTP başlıkları tıklatıldığında HTTP istek bilgileri ve JSON yanıt ortaya. Bu ayrıntılar hizmetini keşfetmeye yararlı olur.

Eğitmen uygulama gösterilmektedir nasıl yapılır:

> [!div class="checklist"]
> * Bir Bing Web arama API çağrısı JavaScript'te gerçekleştirin
> * Arama Seçenekleri Bing Web arama API'sine geçirin
> * Web, haber, görüntü ve video arama sonuçları görüntüleme
> * Arama sonuçları sayfasını
> * Tanıtıcı Bing istemci kimliği ve API abonelik anahtarı
> * Oluşabilecek hataları işleme

Öğretici sayfası tamamen bağımsızdır; herhangi bir dış çerçeveleri, stil sayfaları veya hatta resim dosyalarını kullanmaz. Yalnızca yaygın olarak desteklenen JavaScript dil özellikleri kullanır ve tüm ana Web tarayıcıları geçerli sürümlerinde çalışır.

Bu öğretici kapsamında, kaynak kodunu yalnızca seçili bölümlerini tartışın. Tam kaynak kodunu kullanılabilir [ayrı bir sayfaya](tutorial-bing-web-search-single-page-app-source.md). Kopyalayın ve bu kodu bir metin düzenleyicisine yapıştırın ve kaydedileceği `bing.html`.

## <a name="app-components"></a>Uygulama bileşenleri

Herhangi bir tek sayfalı Web uygulamasına gibi öğretici uygulama üç bölümleri içerir:

> [!div class="checklist"]
> * HTML - tanımlar sayfasının içeriği ve yapısı
> * Sayfa görünümünü tanımlayan CSS-
> * JavaScript - sayfanın davranışını tanımlar

Basit oldukları gibi Bu öğretici HTML veya CSS çoğunu ayrıntılı, kapsamaz.

HTML kullanıcı bir sorgu girer ve arama seçenekleri seçer arama formu içerir. Formun gerçekte göre arama gerçekleştirir JavaScript bağlı `<form>` etiketinin `onsubmit` özniteliği:

```html
<form name="bing" onsubmit="return newBingWebSearch(this)">
```

`onsubmit` İşleyicisini döndürür `false`, hangi tutar formu bir sunucuya gönderildi. JavaScript kodu, aslında formdan gerekli bilgileri toplama ve arama gerçekleştirilirken çalışır.

HTML bölümleri de içerir (HTML `<div>` etiketleri) burada arama sonuçları görüntülenir.

## <a name="managing-subscription-key"></a>Abonelik anahtarı yönetme

Bing arama API abonelik anahtarı kodda dahil etmek zorunda kalmamak için tarayıcının kalıcı depolama anahtarını depolamak için kullanırız. Hiçbir anahtar depolanıyorsa, biz kullanıcının anahtarının istendiği ve daha sonra kullanmak üzere saklayın. Anahtar daha sonra API tarafından reddedilirse, kullanıcı yeniden istenir böylece biz saklı anahtarı geçersiz.

Tanımlarız `storeValue` ve `retrieveValue` kullanın ya da işlevleri `localStorage` (tarayıcı destekliyorsa) nesnesi veya bir tanımlama bilgisi. Bizim `getSubscriptionKey()` işlevi depolamak ve kullanıcının anahtarı almak için bu işlevleri kullanır.

```javascript
// cookie names for data we store
API_KEY_COOKIE   = "bing-search-api-key";
CLIENT_ID_COOKIE = "bing-search-client-id";

BING_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/search";

// ... omitted definitions of storeValue() and retrieveValue()

// get stored API subscription key, or prompt if it's not found
function getSubscriptionKey() {
    var key = retrieveValue(API_KEY_COOKIE);
    while (key.length !== 32) {
        key = prompt("Enter Bing Search API subscription key:", "").trim();
    }
    // always set the cookie in order to update the expiration date
    storeValue(API_KEY_COOKIE, key);
    return key;
}
```

HTML `form` etiketi `onsubmit` çağrıları `bingWebSearch` arama sonuçları döndürmek için işlevi. `bingWebSearch` kullanan `getSubscriptionKey` her sorgu kimliğini doğrulamak için. Önceki tanımında gösterildiği gibi `getSubscriptionKey` anahtar girdiyseniz taşınmadığından, anahtar kullanıcıya sorar. Anahtar sonra kullanım sürdürdüğünüz için uygulama tarafından depolanır.

```html
<form name="bing" onsubmit="this.offset.value = 0; return bingWebSearch(this.query.value, 
    bingSearchOptions(this), getSubscriptionKey())">
```

## <a name="selecting-search-options"></a>Arama Seçenekleri

![[Bing Web araması form]](media/cognitive-services-bing-web-api/web-search-spa-form.png)

HTML formu aşağıdaki adlara sahip öğeleri içerir:

| | |
|-|-|
| `where` | Arama için kullanılan Pazar (konumu ve dil) seçmek için açılır menü. |
| `query` | Metin alanı, arama terimlerini girin. |
| `what` | Belirli tür sonuç yükseltmek için onay kutularını. Görüntüleri, yükseltme, örneğin, görüntüleri sıralamasını artırır. |
| `when` | İsteğe bağlı olarak en son gün, hafta veya ay aramayı sınırlamak için aşağı açılır menüden. |
| `safe` | Bing'ın güvenli arama özelliği "yetişkin" sonuçları filtrelemek için kullanılıp kullanılmayacağını belirten bir onay kutusu. |
| `count` | Gizli alan. Her istekte döndürmek için arama sonuçları sayısı. Sayfa başına daha az veya daha fazla sonuçları görüntülemek için değiştirin. |
| `offset` | Gizli alan. İstek ilk arama sonucu uzaklığını; disk belleği için kullanılır. İçin Sıfırla `0` yeni bir istek üzerinde. |

> [!NOTE]
> Bing Web araması çok daha fazla sorgu parametreleri sunar. Biz yalnızca birkaç tanesi aşağıda kullanıyorsunuz.

JavaScript işlevinin `bingSearchOptions()` bu alanlar Bing arama API'si tarafından gerekli biçime dönüştürür.

```javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.when.value.length) options.push("freshness=" + form.when.value);
    var what = [];
    for (var i = 0; i < form.what.length; i++) 
        if (form.what[i].checked) what.push(form.what[i].value);
    if (what.length) {
        options.push("promote=" + what.join(","));
        options.push("answerCount=9");
    }
    options.push("count=" + form.count.value);
    options.push("offset=" + form.offset.value);
    options.push("textDecorations=true");
    options.push("textFormat=HTML");
    return options.join("&");
}
```

Örneğin, `SafeSearch` gerçek bir API çağrısında parametre olabilir `strict`, `moderate`, veya `off`, ile `moderate` varsayılan bırakılıyor. Formumuzun, ancak yalnızca iki durumlu sahip bir onay kutusu kullanır. Bu çok ya da ayarı JavaScript kodu dönüştürür `strict` veya `off` (`moderate` kullanılmaz).

Varsa **Yükselt** onay kutularını işaretlenir, ayrıca eklediğimiz bir `answerCount` sorgu parametresi. `answerCount` kullanılırken gereklidir `promote` parametresi. Biz yalnızca ayarlayın `9` (Bing Web arama API'si tarafından desteklenen sonuç türleri sayısı) biz almak olası üst sınırını sonuç türleri emin olmak için.

> [!NOTE]
> Sonuç türü yükseltme yok *garanti* arama sonuçları, bu tür bir sonuç içerir. Bunun yerine, bu tür bir sonuç kendi normal derecelendirme göreli sıralamasını yükseltme artırır. Belirli tür Sonuç aramaları sınırlandırmak için kullanmak `responseFilter` sorgu parametresi veya Bing görüntü arama veya Bing Haberler arama gibi daha belirli bir uç çağırın.

Biz de Gönder `textDecoration` ve `textFormat` sorgu parametreleri arama terimi arama sonuçlarında kalın neden olacak. Bu komut dosyasında kodlanmış değerlerdir.

## <a name="performing-the-request"></a>İsteği gerçekleştirme

Verilen sorgu, seçenekleri dize ve API anahtarını `BingWebSearch` işlev kullanan bir `XMLHttpRequest` Bing Web araması uç noktasına istek yapmak için nesne.

```javascript
// perform a search given query, options string, and API key
function bingWebSearch(query, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("pole", "mainline", "sidebar", "_json", "_http", "paging1", "paging2", "error");

    var request = new XMLHttpRequest();
    var queryurl = BING_ENDPOINT + "?q=" + encodeURIComponent(query) + "&" + options;

    // open the request
    try {
        request.open("GET", queryurl);
    } 
    catch (e) {
        renderErrorMessage("Bad request (invalid URL)\n" + queryurl);
        return false;
    }

    // add request headers
    request.setRequestHeader("Ocp-Apim-Subscription-Key", key);
    request.setRequestHeader("Accept", "application/json");
    var clientid = retrieveValue(CLIENT_ID_COOKIE);
    if (clientid) request.setRequestHeader("X-MSEdge-ClientID", clientid);
    
    // event handler for successful response
    request.addEventListener("load", handleBingResponse);
    
    // event handler for erorrs
    request.addEventListener("error", function() {
        renderErrorMessage("Error completing request");
    });

    // event handler for aborted request
    request.addEventListener("abort", function() {
        renderErrorMessage("Request aborted");
    });

    // send the request
    request.send();
    return false;
}
```

HTTP isteği başarıyla tamamlandıktan sonra JavaScript çağrılarını bizim `load` olay işleyicisi `handleBingResponse()` API, başarılı bir HTTP GET isteği işlemek için işlevi. 

```javascript
// handle Bing search request results
function handleBingResponse() {
    hideDivs("noresults");

    var json = this.responseText.trim();
    var jsobj = {};

    // try to parse JSON results
    try {
        if (json.length) jsobj = JSON.parse(json);
    } catch(e) {
        renderErrorMessage("Invalid JSON response");
        return;
    }

    // show raw JSON and HTTP request
    showDiv("json", preFormat(JSON.stringify(jsobj, null, 2)));
    showDiv("http", preFormat("GET " + this.responseURL + "\n\nStatus: " + this.status + " " + 
        this.statusText + "\n" + this.getAllResponseHeaders()));

    // if HTTP response is 200 OK, try to render search results
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

    // Any other HTTP response is an error
    else {
        // 401 is unauthorized; force re-prompt for API key for next request
        if (this.status === 401) invalidateSubscriptionKey();

        // some error responses don't have a top-level errors object, so gin one up
        var errors = jsobj.errors || [jsobj];
        var errmsg = [];

        // display HTTP status code
        errmsg.push("HTTP Status " + this.status + " " + this.statusText + "\n");

        // add all fields from all error responses
        for (var i = 0; i < errors.length; i++) {
            if (i) errmsg.push("\n");
            for (var k in errors[i]) errmsg.push(k + ": " + errors[i][k]);
        }

        // also display Bing Trace ID if it isn't blocked by CORS
        var traceid = this.getResponseHeader("BingAPIs-TraceId");
        if (traceid) errmsg.push("\nTrace ID " + traceid);

        // and display the error message
        renderErrorMessage(errmsg.join("\n"));
    }
}
```

> [!IMPORTANT]
> Başarılı bir HTTP isteği mu *değil* arama kendisini başarılı mutlaka anlamına. Arama işlemi bir hata meydana gelirse, Bing Web arama API 200 HTTP durum kodu döndürür ve JSON yanıtında hata bilgilerini içerir. Ayrıca, API istek oranı sınırlı ise, boş bir yanıt döndürür.

Hata işleme kodu hem de önceki işlevlerin çoğunu ayrılır. Aşağıdaki aşamalarda hatalar oluşabilir:

|Aşama|Olası hatalar|Tarafından işlenen|
|-|-|-|
|Yapı JavaScript istek nesnesi|Geçersiz URL|`try`/`catch` engelle|
|İsteği yapan|Ağ hataları, iptal edilen bağlantıları|`error` ve `abort` olay işleyicileri|
|Arama gerçekleştirme|Geçersiz istek, geçersiz JSON oran sınırları|içinde testleri `load` olay işleyicisi|

Hataları çağırarak işlenir `renderErrorMessage()` hata hakkında bilinen herhangi bir ayrıntıyı ile. Yanıt hata testlerinin tam gauntlet geçerse, diyoruz `renderSearchResults()` arama sonuçları sayfasında görüntülenecek.

## <a name="displaying-search-results"></a>Arama Sonuçları görüntüleme

Bing Web arama API [belirli bir sırada sonuçları görüntülemek gerektiren](useanddisplayrequirements.md). API değişik yanıtlar döndürebilir olduğundan, bu üst düzey yinelemek yeterli değil `WebPages` JSON yanıt koleksiyonunda ve bu sonuçları görüntüler. (Yalnızca bir tür sonuçları istiyorsanız kullanın `responseFilter` sorgu parametresi veya başka bir Bing arama uç.)

Bunun yerine, kullanırız `rankingResponse` görüntü için sonuçları sıralamak için arama sonuçlarında. Öğe bu nesnenin başvurduğu `WebPages` `News`, `Images`, ve/veya `Videos` koleksiyonları veya diğer üst düzey yanıt koleksiyonlar JSON yanıt.

`rankingResponse` Belirtilen arama sonuçlarının en çok üç koleksiyonları içerebilir `pole`, `mainline`, ve `sidebar`. 

`pole`, varsa, en uygun arama sonuç ve görüntülenmelidir. `mainline` Arama sonuçlarını toplu ifade eder. Mainline sonuçları hemen sonra görüntülenmesi gereken `pole` (veya ilk `pole` mevcut değil). 

Son olarak. `sidebar` yardımcı arama sonuçlarını gösterir. Genellikle, bu sonuçlar ilgili aramalar veya görüntüleri olur. Mümkünse, bu sonuçların gerçek bir Kenar çubuğunda görüntülenmesi gerekir. Ekran sınırları kenar çubuğu pratik (örneğin, bir mobil cihazda) yaparsanız, bunlar sonra görünmelidir `mainline` sonuçları.

Her öğe bir `rankingResponse` koleksiyonu iki farklı, ancak eşdeğer şekilde gerçek arama sonucu öğelerini başvuruyor.

| | |
|-|-|
|`id`|`id` Bir URL gibi görünüyor, ancak bağlantıları için kullanılmamalıdır. `id` Derecelendirme sonuç türü ile eşleşen `id` öğesinin ya da bir arama sonucu bir yanıt koleksiyonundaki *veya* tüm yanıt koleksiyonu (gibi `Images`).
|`answerType`, `resultIndex`|`answerType` Sonucu içeren üst düzey yanıt koleksiyona ifade eder (örneğin, `WebPages`). `resultIndex` Sonucunun dizin, koleksiyondaki başvuruyor. Varsa `resultIndex` olan atlanırsa, tüm koleksiyon derecelendirme sonuç başvuruyor.

> [!NOTE]
> Bu arama yanıtı parçası hakkında daha fazla bilgi için bkz: [derece sonuçları](rank-results.md).

Başvurulan arama sonucu öğesinin bulma, hangi yöntemi uygulamanız için en uygun kullanabilir. Eğitmen kodumuza kullanırız `answerType` ve `resultIndex` her arama sonucu bulunamadı.

Bizim işlevini aramak için zamanı son olarak, `renderSearchResults()`. Bu işlev üç tekrarlanan `rankingResponse` arama sonuçlarını üç bölümlerini temsil koleksiyonları. Her bölüm için diyoruz `renderResultsItems()` Bu bölüm için sonuçları işlenecek.

```javascript
// render the search results given the parsed JSON response
function renderSearchResults(results) {

    // if spelling was corrected, update search field
    if (results.queryContext.alteredQuery) 
        document.forms.bing.query.value = results.queryContext.alteredQuery;

    // add Prev / Next links with result count
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);
    
    // for each possible section, render the resuts from that section
    for (section in {pole: 0, mainline: 0, sidebar: 0}) {
        if (results.rankingResponse[section])
            showDiv(section, renderResultsItems(section, results));
    }
}
```

`renderResultsItems()` her öğe üzerinde sırayla tekrarlanan `rankingResponse` bölümünde, her bir derecelendirme sonucu kullanarak bir arama sonucu eşlemeleri `answerType` ve `resultIndex` alanları ve sonucunun HTML oluşturmak için uygun işleme işlevi çağırır. 

Varsa `resultIndex` verilen derecelendirme öğesi için belirtilmemiş `renderResultsItems()` bu türdeki tüm sonuçları tekrarlanan ve her öğe için işleme işlevi çağırır. 

Her iki durumda da, sonuçta elde edilen HTML uygun eklenen `<div>` sayfasındaki öğe.

```javascript
// render search results from rankingResponse object in specified order
function renderResultsItems(section, results) {

    var items = results.rankingResponse[section].items;
    var html = [];
    for (var i = 0; i < items.length; i++) {
        var item = items[i];
        // collection name has lowercase first letter while answerType has uppercase
        // e.g. `WebPages` rankingResult type is in the `webPages` top-level collection
        var type = item.answerType[0].toLowerCase() + item.answerType.slice(1);
        // must have results of the given type AND a renderer for it
        if (type in results && type in searchItemRenderers) {
            var render = searchItemRenderers[type];
            // this ranking item refers to ONE result of the specified type
            if ("resultIndex" in item) {
                html.push(render(results[type].value[item.resultIndex], section));
            // this ranking item refers to ALL results of the specified type
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

## <a name="rendering-result-items"></a>Sonuç öğeleri oluşturma

Bizim JavaScript kodu bir nesne `searchItemRenderers`, içeren *Oluşturucu:* her biri için tür HTML'i işlevleri arama sonucu.

```javascript
// render functions for various types of search results
searchItemRenderers = { 
    webPages: function(item) { ... },
    news: function(item) { ... },
    images: function(item, section, index, count) { ... },
    videos: function(item, section, index, count) { ... },
    relatedSearches: function(item, section, index, count) { ... }
}
```

> [!NOTE]
> Eğitmen uygulamamıza Web sayfaları, haber öğeleri, görüntüler, videolar ve ilişkili aramaları için işleyiciler sahiptir. Kendi uygulama Oluşturucu hesaplamalar, yazım önerileri, varlıklar, saat dilimleri ve tanımları içerebilir alabilir, sonuçları herhangi türde için gerekir.

Bazı bizim işleme işlevleri yalnızca kabul `item` parametresi: tek arama sonucu temsil eden bir JavaScript nesnesi. Başkalarının farklı bağlamlarda farklı öğeleri işlemek için kullanılan ek parametreleri kabul edin. (Bu bilgileri kullanmaz bir işleyici bu parametreler kabul etmek gerekmez.)

Bağlam bağımsız değişkenleri şunlardır:

| | |
|-|-|
|`section`|Sonuçları bölümü (`pole`, `mainline`, veya `sidebar`) öğesi göründüğü içinde.
|`index`<br>`count`|Kullanılabilir olduğunda `rankingResponse` öğesi görüntülenecek; belirli bir koleksiyondaki tüm sonuçları olduğunu belirtir `undefined` Aksi takdirde. Bu parametreler, toplama ve toplam öğe sayısını içinde öğenin dizini koleksiyonda alırsınız. İlk veya son sonucu için farklı HTML oluşturur ve benzeri için sonuçları numaralandırmak için bu bilgileri kullanabilirsiniz.|

Bizim öğretici uygulamasında hem `images` ve `relatedSearches` Oluşturucu bağlamı bağımsız değişkenleri oluşturdukları HTML özelleştirmek için kullanın. Daha yakın bir göz atalım `images` Oluşturucu:

```javascript
searchItemRenderers = { 
    // render image result using thumbnail
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
    }, // other renderers omitted
}
```

Bizim Görüntü Oluşturucu işlevi:

> [!div class="checklist"]
> * Görüntü küçük resimlerin boyutunu hesaplar (genişlik farklılık gösterir, 60 piksel çözünürlükte sabit bir yükseklik).
> * Görüntü sonuç bağlamı bağlı olarak önündeki HTML ekler.
> * HTML derlemeler `<a>` görüntü içeren sayfasını bağlanan etiketi.
> * HTML derlemeler `<img>` görüntü küçük görüntülenecek etiket. 

Görüntü Oluşturucu kullanan `section` ve `index` farklı bağlı olarak göründüğü sonuçları görüntülemek için değişkenleri. Satır sonu (`<br>` etiketi) görüntülerinin bir sütunu kenar görüntüler böylece resim kenar sonuçlarında arasında eklenir. Diğer bölümlerinde ilk resmi neden `(index === 0)` öncesinde bir `<p>` etiketi. Aksi takdirde küçük resimleri birbirleri ve kaydırma sunmayı tarayıcı penceresinde gerektiğinde alın.

Küçük resimlerin boyutunu hem de kullanılan `<img>` etiketi ve `h` ve `w` alanları küçük resim ait URL. [Bing küçük resim hizmet](resize-and-crop-thumbnails.md) tam olarak bu boyut, bir küçük resim sunar. `title` Ve `alt` öznitelikleri (görüntü metinsel açıklaması) görüntünün adı ve URL ana bilgisayar adı oluşturulur.

Görüntüleri mainline arama sonuçlarında aşağıda gösterildiği gibi görünür.

![[Bing resim sonuçları]](media/cognitive-services-bing-web-api/web-search-spa-images.png)

## <a name="persisting-client-id"></a>Kalıcı istemci kimliği

Bing arama API'leri yanıtlarının içerebilir bir `X-MSEdge-ClientID` geri API art arda gelen istekleri ile gönderilmesi gereken üstbilgi. Birden çok Bing arama API'leri kullanılıyorsa, aynı istemci kimliği, bunların tümünün ile mümkünse kullanılmalıdır.

Sağlama `X-MSEdge-ClientID` üstbilgi iki önemli faydası vardır kullanıcının aramaları ilişkilendirilecek Bing API'ler sağlar.

İlk olarak, kullanıcının daha iyi karşılamak sonuçları bulmak için arama için bağlam uygulamak arama motoru Bing sağlar. Örneğin, bir kullanıcı daha önce Yelkenli için ilgili koşulları için aradı, "düğümü" sonraki Ara tercihen Yelkenli içinde kullanılan düğüm hakkında bilgi döndürebilir.

İkinci olarak, Bing rastgele yaygın olarak kullanılabilir hale getirilmeden önce yeni özellikleri denemek için kullanıcıların seçebilirsiniz. Her istek ile aynı istemci Kimliğini sağlayan bir özellik her zaman görmek için seçilen kullanıcılar, görmenizi sağlar. İstemci kimliği kullanıcı görünür ve, görünen rastgele, kendi arama sonuçlarında kaybolur bir özellik görebilirsiniz.

Tarayıcı güvenlik ilkelerini (CORS) engelleyebilir `X-MSEdge-ClientID` üstbilgi JavaScript için kullanılabilir olmasını durduracak. Bu sınırlama arama yanıt, istenen sayfa farklı bir kaynaktan olduğunda oluşur. Bir üretim ortamında, bu ilke aynı etki alanındaki Web sayfası olarak API çağrısı yapan bir sunucu tarafı komut dosyası barındırarak giderilmelidir. Komut dosyasını Web sayfası olarak aynı kaynak olduğundan `X-MSEdge-ClientID` başlığıdır sonra JavaScript için kullanılabilir.

> [!NOTE]
> Bir üretim Web uygulaması, isteği sunucu tarafı yine de gerçekleştirmeniz gerekir. Aksi halde, kaynak görünümleri herkes için kullanılabilir olduğu Web sayfasındaki Bing arama API anahtarınıza eklenmesi gerekir. API abonelik anahtarınızı altında anahtarınızı kullanıma sunmak değil önemlidir yetkisiz kişiler tarafından bile isteklerini tüm kullanım için faturalandırılır.

Geliştirme amaçlı bir CORS Ara sunucu aracılığıyla Bing Web arama API isteği yapabilirsiniz. Bu tür bir proxy yanıttan sahip bir `Access-Control-Expose-Headers` üstbilgi bu whitelists yanıt üstbilgilerini ve JavaScript için kullanılabilir hale getirir.

İstemci erişimi için öğretici uygulamamıza kimliği üstbilgisi izin vermek için bir CORS Ara sunucusunun yükleneceği kolaydır. Zaten yoksa, birinci [Node.js yüklemek](https://nodejs.org/en/download/). Ardından bir komut penceresinde aşağıdaki komutu yürütün:

    npm install -g cors-proxy-server

Ardından, HTML dosyasına Bing Web araması uç değiştirin:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Son olarak, aşağıdaki komutla CORS proxy başlatın:

    cors-proxy-server

Eğitmen uygulama kullanırken komut penceresini açık bırakın; pencereyi proxy durdurur. Arama sonuçları altında Genişletilebilir HTTP üstbilgileri bölümünde şimdi görebilirsiniz `X-MSEdge-ClientID` üstbilgi (Diğerlerinin yanında) ve her istek için aynı olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görsel arama mobil uygulama Öğreticisi](computer-vision-web-search-tutorial.md)

> [!div class="nextstepaction"]
> [Bing Web arama API Başvurusu](//docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
