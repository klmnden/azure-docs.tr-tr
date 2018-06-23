---
title: Bing tek sayfa haber arama uygulaması | Microsoft Docs
description: Bir tek sayfalı Web uygulamasında Bing Haberler arama API kullanımı açıklanmaktadır.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 10/30/2017
ms.author: v-gedod
ms.openlocfilehash: fb8cd24dfdfb03500cc86ee1b1f0126ec044a873
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354905"
---
# <a name="tutorial-single-page-news-search-app"></a>Öğretici: Tek sayfalı haber arama uygulama
Bing Haberler arama API Web araması ve bir arama sorgusu ilgili haberleri türünde sonuçlar elde etmenizi sağlar. Bu öğreticide, sayfada arama sonuçlarını görüntülemek için Bing Haberler arama API'sini kullanan bir tek sayfalı Web uygulaması oluşturun. Uygulama, HTML, CSS ve JavaScript bileşenleri içerir.

<!-- Remove until we can replace it with sanitized copy
![Single-page Bing News Search app](media/news-search-singlepage.png)
-->

> [!NOTE]
> JSON ve HTTP başlıkları tıklatıldığında sayfanın sonundaki JSON yanıt ve HTTP isteği bilgilerini gösterir. Bu ayrıntılar hizmetini keşfetmeye kullanışlı olabilir.

Eğitmen uygulama gösterilmektedir nasıl yapılır:
> [!div class="checklist"]
> * Bing Haberler arama API çağrısı JavaScript'te gerçekleştirin
> * Bing Haberler arama API geçişi arama seçenekleri
> * Dört kategoriden haber arama sonuçları görüntüler: türü herhangi, iş, sistem durumu veya aralıklarına 24 saat, geçen hafta, ay veya tüm kullanılabilir saat gelen siyaset
> * Arama sonuçları sayfasını
> * Tanıtıcı Bing istemci kimliği ve API abonelik anahtarı
> * Oluşabilecek hataları işleme

Öğretici sayfası tamamen bağımsızdır; herhangi bir dış çerçeveleri, stil sayfaları veya resim dosyalarını kullanmaz. Yalnızca yaygın olarak desteklenen JavaScript dil özellikleri kullanır ve tüm ana Web tarayıcıları geçerli sürümlerinde çalışır.

Bu öğretici kapsamında, kaynak kodu seçili bölümlerini tartışın. Tam [kaynak kodu](tutorial-bing-news-search-single-page-app-source.md) kullanılabilir. Örneği çalıştırmak için kopyalamak ve kaynak kodu bir metin düzenleyicisine yapıştırın ve kaydedileceği `bing.html`.

## <a name="app-components"></a>Uygulama bileşenleri
Herhangi bir tek sayfalı Web uygulamasına gibi Bu öğretici uygulama üç bölümleri içerir:

> [!div class="checklist"]
> * HTML - tanımlar sayfasının içeriği ve yapısı
> * Sayfa görünümünü tanımlayan CSS-
> * JavaScript - sayfanın davranışını tanımlar

HTML ve CSS çoğunu olduğundan Geleneksel, öğreticiyi ele alınmamaktadır. HTML kullanıcı bir sorgu girer ve arama seçenekleri seçer arama formu içerir. Formun gerçekleştiren gerçekte arama'yı kullanarak JavaScript bağlı `onsubmit` özniteliği `<form>` etiketi:

```html
<form name="bing" onsubmit="return newBingNewsSearch(this)">
```
`onsubmit` İşleyicisini döndürür `false`, hangi tutar formu bir sunucuya gönderildi. JavaScript kodu formdan gerekli bilgileri toplama ve arama gerçekleştirilirken çalışır.

HTML bölümleri de içerir (HTML `<div>` etiketleri) burada arama sonuçları görüntülenir.

## <a name="managing-subscription-key"></a>Abonelik anahtarı yönetme

Bing arama API abonelik anahtarı kodda dahil etmek zorunda kalmamak için tarayıcının kalıcı depolama anahtarını depolamak için kullanırız. Anahtar depolanan önce biz için kullanıcının anahtar ister. Anahtar daha sonra API tarafından reddedilirse, kullanıcı yeniden istenir böylece biz saklı anahtarı geçersiz.

Tanımlarız `storeValue` ve `retrieveValue` kullanın ya da işlevleri `localStorage` nesnesi (tüm tarayıcılar desteklemez) veya bir tanımlama bilgisi. `getSubscriptionKey()` İşlevi depolamak ve kullanıcının anahtarı almak için bu işlevleri kullanır.

``` javascript
// Cookie names for data we store
API_KEY_COOKIE   = "bing-search-api-key";
CLIENT_ID_COOKIE = "bing-search-client-id";

// Bing Search API endpoint
BING_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/news";

// ... omitted definitions of storeValue() and retrieveValue()
// Browsers differ in their support for persistent storage by 
// local HTML files. See the source code for browser-specific
// options.

// Get stored API subscription key, or 
// prompt if it's not found.
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
HTML `<form>` etiketi `onsubmit` çağrıları `bingWebSearch` arama sonuçları döndürmek için işlevi. `bingWebSearch` kullanan `getSubscriptionKey()` her sorgu kimliğini doğrulamak için. Önceki tanımında gösterildiği gibi `getSubscriptionKey` anahtar girdiyseniz taşınmadığından, anahtar kullanıcıya sorar. Anahtar sonra kullanım sürdürdüğünüz için uygulama tarafından depolanır.

```html
<form name="bing" onsubmit="this.offset.value = 0; return bingWebSearch(this.query.value, 
    bingSearchOptions(this), getSubscriptionKey())">
```
## <a name="selecting-search-options"></a>Arama Seçenekleri
Aşağıdaki şekil sorgu metin kutusu ve Okul fon hakkında haberleri araması tanımlamak seçenekleri gösterir.

![Bing Haberler arama seçenekleri](media/news-search-categories.png)

HTML formu aşağıdaki adlara sahip öğeleri içerir:

|Öğe|Açıklama|
|-|-|
| `where` | Arama için kullanılan Pazar (konumu ve dil) seçmek için açılır menü. |
| `query` | Arama terimleri girmek için metin alanı. |
| `category` | Belirli tür sonuç yükseltmek için onay kutularını. Sistem durumu yükseltme, örneğin, sistem durumu haber sıralamasını artırır. |
| `when` | İsteğe bağlı olarak en son gün, hafta veya ay aramayı sınırlamak için aşağı açılır menüden. |
| `safe` | Bing güvenli arama özelliği "yetişkin" sonuçları filtrelemek için kullanılıp kullanılmayacağını belirten bir onay kutusu. |
| `count` | Gizli alan. Her istekte döndürmek için arama sonuçları sayısı. Sayfa başına daha az veya daha fazla sonuçları görüntülemek için değiştirin. |
| `offset`|  Gizli alan. İstek ilk arama sonucu uzaklığını; disk belleği için kullanılır. İçin Sıfırla `0` yeni bir istek üzerinde. |

> [!NOTE]
> Bing Web araması diğer sorgu parametreleri sunar. Biz yalnızca birkaç tanesi kullanıyorsunuz.

``` javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.when.value.length) options.push("freshness=" + form.when.value);

    for (var i = 0; i < form.category.length; i++) {
        if (form.category[i].checked) {
            category = form.category[i].value;
            break;
        }
    }
    if (category.valueOf() != "all".valueOf()) { 
        options.push("category=" + category); 
        }
    options.push("count=" + form.count.value);
    options.push("offset=" + form.offset.value);
    return options.join("&");
}
```

Örneğin, `SafeSearch` gerçek bir API çağrısında parametre olabilir `strict`, `moderate`, veya `off`, ile `moderate` varsayılan bırakılıyor. Formumuzun, ancak yalnızca iki durumlu sahip bir onay kutusu kullanır. Bu çok ya da ayarı JavaScript kodu dönüştürür `strict` veya `off` (`moderate` kullanılmaz).

## <a name="performing-the-request"></a>İsteği gerçekleştirme
Verilen sorgu, seçenekleri dize ve API anahtarını `BingNewsSearch` işlev kullanan bir `XMLHttpRequest` Bing Haberler arama uç noktasına istek yapmak için nesne.

```javascript
// perform a search given query, options string, and API key
function bingNewsSearch(query, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("results", "related", "_json", "_http", "paging1", "paging2", "error");

    var request = new XMLHttpRequest();
     if (category.valueOf() != "all".valueOf()) {
        var queryurl = BING_ENDPOINT + "/search?" + "?q=" + encodeURIComponent(query) + "&" + options;
    }
    else
    {
        if (query){
        var queryurl = BING_ENDPOINT + "?q=" + encodeURIComponent(query) + "&" + options;
        }
        else {
            var queryurl = BING_ENDPOINT + "?" + options;
        }
    }

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
HTTP isteği başarıyla tamamlandıktan sonra JavaScript çağrılarını `load` olay işleyicisi `handleBingResponse()` API, başarılı bir HTTP GET isteği işlemek için işlevi. 

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
            if (jsobj._type === "News") {
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
> Başarılı bir HTTP isteği mu *değil* arama kendisini başarılı mutlaka anlamına. Arama işlemi bir hata meydana gelirse, Bing Haberler arama API 200 HTTP durum kodu döndürür ve JSON yanıtında hata bilgilerini içerir. Ayrıca, API istek oranı sınırlı ise, boş bir yanıt döndürür.

Hata işleme kodu hem de önceki işlevlerin çoğunu ayrılır. Aşağıdaki aşamalarda hatalar oluşabilir:

|Aşama|Olası hatalar|Tarafından işlenen|
|-|-|-|
|JavaScript istek nesnesi oluşturma|Geçersiz URL|`try`/`catch` engelle|
|İsteği yapan|Ağ hataları, iptal edilen bağlantıları|`error` ve `abort` olay işleyicileri|
|Arama gerçekleştirme|Geçersiz istek, geçersiz JSON oran sınırları|içinde testleri `load` olay işleyicisi|

Hataları çağırarak işlenir `renderErrorMessage()` hata hakkında bilinen herhangi bir ayrıntıyı ile. Yanıt hata testlerinin tam gauntlet geçerse, diyoruz `renderSearchResults()` arama sonuçları sayfasında görüntülenecek.

## <a name="displaying-search-results"></a>Arama Sonuçları görüntüleme
Arama sonuçlarını görüntülemek için ana işlevi `renderSearchResults()`. Bu işlev Bing Haberler arama hizmeti tarafından döndürülen JSON alır ve haber sonuçları ve ilişkili aramaları varsa işler.

```javascript
// render the search results given the parsed JSON response
    function renderSearchResults(results) {

    // add Prev / Next links with result count
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);

    showDiv("results", renderResults(results.value));
    if (results.relatedSearches)
        showDiv("sidebar", renderRelatedItems(results.relatedSearches));
}
```
Ana arama sonuçları en üst düzey döndürülen `value` JSON yanıt nesnesi. Biz bizim işlevi geçirmek `renderResults()`, aralarında yineler ve her bir öğeyi HTML'e işlemek için ayrı bir işlevi çağırır. Sonuçta elde edilen HTML döndürülen `renderSearchResults()`, içine eklenen burada `results` sayfasındaki bölme.

```javascript
function renderResults(items) {
    var len = items.length;
    var html = [];
    if (!len) {
        showDiv("noresults", "No results.");
        hideDivs("paging1", "paging2");
        return "";
    }
    for (var i = 0; i < len; i++) {
        html.push(searchItemRenderers.news(items[i], i, len));
    }
    return html.join("\n\n");
}
```
Bing Haberler arama API her kendi üst düzey nesnesindeki ilgili sonuçları dört farklı türde kadar döndürür. Bunlar:

|İlişkisi|Açıklama|
|-|-|
|`pivotSuggestions`|Özgün arama Özet Word'de farklı bir tarihle sorgular. Örneğin, "için kırmızı çiçekler" arıyorsanız, bir Özet sözcük "red" olabilir ve bir Özet öneri "sarı çiçekler." olabilir|
|`queryExpansions`|Daha fazla koşulları ekleyerek özgün arama daraltmak sorgular. Örneğin, "Microsoft Surface" için arama yaparsanız, sorgu genişletme olabilir "Microsoft Surface Pro."|
|`relatedSearches`|Ayrıca özgün araması girilen diğer kullanıcılar tarafından girilen sorgular. Örneğin, "Bağlama Sunucu1" için arama yaparsanız, ilgili arama "yüksekliğindeki olabilir Saint Helens."|
|`similarTerms`|Özgün arama anlamı benzer sorgular. Örneğin, "okullar" için arama yaparsanız, "eğitim." benzer bir terim olabilir|

Daha önce görünen `renderSearchResults()`, biz yalnızca işleme `relatedItems` önerileri ve Yerleştir sayfanın Kenar çubuğunda bağlantılar sonuç.

## <a name="rendering-result-items"></a>Sonuç öğeleri oluşturma

JavaScript nesne kodu `searchItemRenderers`, içeren *Oluşturucu:* her biri için tür HTML'i işlevleri arama sonucu.

```javascript
searchItemRenderers = {
    news: function(item) { ... },
    webPages: function (item) { ... }, 
    images: function(item, index, count) { ... },
    relatedSearches: function(item) { ... }
}
```
Oluşturucu işlevi aşağıdaki parametreleri kabul edebilir:

|Parametre|Açıklama|
|-|-|
|`item`| Öğenin özelliklerini içeren JavaScript nesne URL'sini ve açıklamasını gibi.|
|`index`| Kendi koleksiyonundaki sonuç öğenin dizini.|
|`count`| Arama sonucu öğesi'nin koleksiyondaki öğe sayısı.|

`index` Ve `count` parametreleri sayı sonuçları başına veya sonuna bir koleksiyon için özel HTML öğeleri, belirli bir sayıda sonra satır sonları eklemek için oluşturmak için kullanılır ve benzeri. Bu işlev bir işleyici gerekmez, bu iki parametre kabul etmek gerekmez.

`news` Oluşturucu aşağıdaki javascript alıntı gösterilir:
```javascript
    // render news story
    news: function (item) {
        var html = [];
        html.push("<p class='news'>");
        if (item.image) {
            width = 60;
            height = Math.round(width * item.image.thumbnail.height / item.image.thumbnail.width);
            html.push("<img src='" + item.image.thumbnail.contentUrl +
                "&h=" + height + "&w=" + width + "' width=" + width + " height=" + height + ">");
        }
        html.push("<a href='" + item.url + "'>" + item.name + "</a>");
        if (item.category) html.push(" - " + item.category);
        if (item.contractualRules) {    // MUST display source attributions
            html.push(" (");
            var rules = [];
            for (var i = 0; i < item.contractualRules.length; i++)
                rules.push(item.contractualRules[i].text);
                html.push(rules.join(", "));
                html.push(")");
            }
        html.push(" (" + getHost(item.url) + ")");
        html.push("<br>" + item.description);
        return html.join("");
    },
```
Haber oluşturucu işlevi:
> [!div class="checklist"]
> * Bir paragraf etiketinin oluşturur, atar `news` sınıfı ve html diziye iter.
> * Görüntü küçük resim boyutu hesaplar (genişlik sabit 60 piksel cinsinden yüksekliği orantılı olarak hesaplanan).
> * HTML derlemeler `<img>` görüntü küçük görüntülenecek etiket. 
> * HTML derlemeler `<a>` bağlantı görüntü ve içerdiği sayfa etiketler.
> * Görüntü ve üzerinde site hakkındaki bilgileri görüntüler açıklama oluşturur.

Küçük resimlerin boyutunu hem de kullanılan `<img>` etiketi ve `h` ve `w` alanları küçük resim ait URL. [Bing küçük resim hizmet](resize-and-crop-thumbnails.md) tam olarak bu boyut, bir küçük resim sunar.

## <a name="persisting-client-id"></a>Kalıcı istemci kimliği
Bing arama API'leri yanıtlarının içerebilir bir `X-MSEdge-ClientID` geri API art arda gelen istekleri ile gönderilmesi gereken üstbilgi. Birden çok Bing arama API'leri kullanılıyorsa, aynı istemci kimliği, bunların tümünün ile mümkünse kullanılmalıdır.

Sağlama `X-MSEdge-ClientID` üstbilgi iki önemli faydası vardır kullanıcının aramaları ilişkilendirilecek Bing API'ler sağlar.

İlk olarak, kullanıcının daha iyi karşılamak sonuçları bulmak için arama için bağlam uygulamak arama motoru Bing sağlar. Örneğin, bir kullanıcı daha önce Yelkenli için ilgili koşulları için aradı, "düğümü" sonraki Ara tercihen Yelkenli içinde kullanılan düğüm hakkında bilgi döndürebilir.

İkinci olarak, Bing rastgele yaygın olarak kullanılabilir hale getirilmeden önce yeni özellikleri denemek için kullanıcıların seçebilirsiniz. Her istek ile aynı istemci kimliği sağlama özellik her zaman bakın kullanıcılar bunu görmenizi sağlar. İstemci kimliği kullanıcı görünür ve, görünen rastgele, kendi arama sonuçlarında kaybolur bir özellik görebilirsiniz.

Tarayıcı güvenlik ilkelerini (CORS) engelleyebilir `X-MSEdge-ClientID` üstbilgi JavaScript için kullanılabilir olmasını durduracak. Bu sınırlama arama yanıt, istenen sayfa farklı bir kaynaktan olduğunda oluşur. Bir üretim ortamında, bu ilke aynı etki alanındaki Web sayfası olarak API çağrısı yapan bir sunucu tarafı komut dosyası barındırarak giderilmelidir. Komut dosyasını Web sayfası olarak aynı kaynak olduğundan `X-MSEdge-ClientID` başlığıdır sonra JavaScript için kullanılabilir.

> [!NOTE]
> Bir üretim Web uygulaması, isteği sunucu tarafı gerçekleştirmeniz gerekir. Aksi halde, kaynak görünümleri herkes için kullanılabilir olduğu Web sayfasındaki Bing arama API anahtarınıza eklenmesi gerekir. API abonelik anahtarınızı altında anahtarınızı kullanıma sunmak değil önemlidir yetkisiz kişiler tarafından bile isteklerini tüm kullanım için faturalandırılır.

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
> [Bing Haberler arama API Başvurusu](//docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference)