---
title: Bing görüntü arama tek sayfa Web uygulaması | Microsoft Docs
description: Bir tek sayfalı Web uygulamasında Bing görüntü arama API kullanmayı gösterir.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-jerkin
ms.openlocfilehash: d0e1dc24513c8fc3a405cf1c18f531a0c58fad13
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354599"
---
# <a name="tutorial-single-page-web-app"></a>Öğretici: Tek sayfalı Web uygulaması

Bing görüntü arama API Web araması ve resim sonuçları arama sorgu ile ilgili elde etmenizi sağlar. Bu öğreticide, biz arama sonuçlarını görüntülemek için Bing görüntü arama API'sini kullanan bir tek sayfalı Web uygulaması oluşturma sayfasındaki sağ. Uygulama, HTML, CSS ve JavaScript bileşenleri içerir.

<!-- Remove until we can sanitize images
![[Single-page Bing Image Search app]](media/cognitive-services-bing-images-api/image-search-spa-demo.png)
-->

> [!NOTE]
> Sayfanın altındaki JSON ve HTTP başlıkları tıklatıldığında HTTP istek bilgileri ve JSON yanıt ortaya. Bu ayrıntılar hizmetini keşfetmeye yararlı olur.

Eğitmen uygulama gösterilmektedir nasıl yapılır:

> [!div class="checklist"]
> * Bir Bing görüntü arama API çağrısı JavaScript'te gerçekleştirin
> * Arama Seçenekleri Bing görüntü arama API'sine geçirin
> * Görüntü arama sonuçları
> * Arama sonuçları sayfasını
> * Tanıtıcı Bing istemci kimliği ve API abonelik anahtarı
> * Oluşabilecek hataları işleme

Öğretici sayfası tamamen bağımsızdır; herhangi bir dış çerçeveleri, stil sayfaları veya hatta resim dosyalarını kullanmaz. Yalnızca yaygın olarak desteklenen JavaScript dil özellikleri kullanır ve tüm ana Web tarayıcıları geçerli sürümlerinde çalışır.

Bu öğretici kapsamında, kaynak kodunu yalnızca seçili bölümlerini tartışın. Tam kaynak kodunu kullanılabilir [ayrı bir sayfaya](tutorial-bing-image-search-single-page-app-source.md). Kopyalayın ve bu kodu bir metin düzenleyicisine yapıştırın ve kaydedileceği `bing.html`.

> [!NOTE]
> Bu öğretici önemli ölçüde benzer [tek sayfalı Bing Web arama uygulaması Öğreticisi](../Bing-Web-Search/tutorial-bing-web-search-single-page-app.md), ancak yalnızca resim arama sonuçları ile ilgilidir.

## <a name="app-components"></a>Uygulama bileşenleri

Herhangi bir tek sayfalı Web uygulamasına gibi öğretici uygulama üç bölümleri içerir:

> [!div class="checklist"]
> * HTML - tanımlar sayfasının içeriği ve yapısı
> * Sayfa görünümünü tanımlayan CSS-
> * JavaScript - sayfanın davranışını tanımlar

Basit oldukları gibi Bu öğretici HTML veya CSS çoğunu ayrıntılı, kapsamaz.

HTML kullanıcı bir sorgu girer ve arama seçenekleri seçer arama formu içerir. Formun gerçekte göre arama gerçekleştirir JavaScript bağlı `<form>` etiketinin `onsubmit` özniteliği:

```html
<form name="bing" onsubmit="return newBingImageSearch(this)">
```

`onsubmit` İşleyicisini döndürür `false`, hangi tutar formu bir sunucuya gönderildi. JavaScript kodu, aslında formdan gerekli bilgileri toplama ve arama gerçekleştirilirken çalışır.

HTML bölümleri de içerir (HTML `<div>` etiketleri) burada arama sonuçları görüntülenir.

## <a name="managing-subscription-key"></a>Abonelik anahtarı yönetme

Bing arama API abonelik anahtarı kodda dahil etmek zorunda kalmamak için tarayıcının kalıcı depolama anahtarını depolamak için kullanırız. Hiçbir anahtar depolanıyorsa, biz kullanıcının anahtarının istendiği ve daha sonra kullanmak üzere saklayın. Anahtar daha sonra API tarafından reddedilirse, kullanıcı yeniden böylece biz saklı anahtarı geçersiz.

Tanımlarız `storeValue` ve `retrieveValue` kullanın ya da işlevleri `localStorage` (tarayıcı destekliyorsa) nesnesi veya bir tanımlama bilgisi. Bizim `getSubscriptionKey()` işlevi depolamak ve kullanıcının anahtarı almak için bu işlevleri kullanır.

```javascript
// cookie names for data we store
API_KEY_COOKIE   = "bing-search-api-key";
CLIENT_ID_COOKIE = "bing-search-client-id";

BING_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/images/search";

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

HTML `<form>` etiketi `onsubmit` çağrıları `bingWebSearch` arama sonuçları döndürmek için işlevi. `bingWebSearch` kullanan `getSubscriptionKey` her sorgu kimliğini doğrulamak için. Önceki tanımında gösterildiği gibi `getSubscriptionKey` anahtar girdiyseniz taşınmadığından, anahtar kullanıcıya sorar. Anahtar sonra kullanım sürdürdüğünüz için uygulama tarafından depolanır.

```html
<form name="bing" onsubmit="this.offset.value = 0; return bingWebSearch(this.query.value, 
    bingSearchOptions(this), getSubscriptionKey())">
```

## <a name="selecting-search-options"></a>Arama Seçenekleri

![[Bing görüntü arama formu]](media/cognitive-services-bing-images-api/image-search-spa-form.png)

HTML formu aşağıdaki denetimleri içerir:

| | |
|-|-|
|`where`|Arama için kullanılan Pazar (konumu ve dil) seçmek için açılır menü.|
|`query`|Metin alanı, arama terimlerini girin.|
|`aspect`|Radyo düğmeleri bulunan görüntüleri oranlarını seçme: kabaca, geniş veya yükle kare.|
|`color`|Renk veya beyaz veya baskın renk seçer.
|`when`|İsteğe bağlı olarak en son gün, hafta veya ay aramayı sınırlamak için aşağı açılır menüden.|
|`safe`|Bing'ın güvenli arama özelliği "yetişkin" sonuçları filtrelemek için kullanılıp kullanılmayacağını belirten bir onay kutusu.|
|`count`|Gizli alan. Her istekte döndürmek için arama sonuçları sayısı. Sayfa başına daha az veya daha fazla sonuçları görüntülemek için değiştirin.|
|`offset`|Gizli alan. İstek ilk arama sonucu uzaklığını; disk belleği için kullanılır. İçin Sıfırla `0` yeni bir istek üzerinde.|
|`nextoffset`|Gizli alan. Arama sonucu alındıktan sonra bu alan değerine ayarlanır `nextOffset` yanıt. Bu alan kullanılarak çakışan art arda sayfaları sonuçlarına önler.|
|`stack`|Gizli alan. Önceki arama sonuçları, önceki sayfalara gezinme sayfaları uzaklıklarını JSON olarak kodlanmış listesi.|

> [!NOTE]
> Bing görüntü arama çok daha fazla sorgu parametreleri sunar. Biz yalnızca birkaç tanesi aşağıda kullanıyorsunuz.

Bizim JavaScript işlevi `bingSearchOptions()` kısmi sorgu biçiminde bir dize olarak Bing arama API'si tarafından gerekli bu alanlar dönüştürür.

```javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.when.value.length) options.push("freshness=" + form.when.value);
    var aspect = "all";
    for (var i = 0; i < form.aspect.length; i++) {
        if (form.aspect[i].checked) {
            aspect = form.aspect[i].value;
            break;
        }
    }
    options.push("aspect=" + aspect);
    if (form.color.value) options.push("color=" + form.color.value);
    options.push("count=" + form.count.value);
    options.push("offset=" + form.offset.value);
    return options.join("&");
}
```

Örneğin, güvenli arama özellik yüklenebilir `strict`, `moderate`, veya `off`, ile `moderate` varsayılan bırakılıyor. Ancak yalnızca iki durumlu sahip bir onay kutusu formumuzun kullanır. Bu çok ya da ayarı JavaScript kodu dönüştürür `strict` veya `off` (biz kullanmayan `moderate`).

## <a name="performing-the-request"></a>İsteği gerçekleştirme

Verilen sorgu, seçenekleri dize ve API anahtarını `BingImageSearch` işlev kullanan bir `XMLHttpRequest` Bing görüntü arama uç noktasına istek yapmak için nesne.

```javascript
// perform a search given query, options string, and API key
function bingImageSearch(query, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("results", "related", "_json", "_http", "paging1", "paging2", "error");

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
            if (jsobj._type === "Images") {
                if (jsobj.nextOffset) document.forms.bing.nextoffset.value = jsobj.nextOffset;
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
> Başarılı bir HTTP isteği mu *değil* arama kendisini başarılı mutlaka anlamına. Arama işlemi bir hata oluşursa, Bing görüntü arama API 200 HTTP durum kodu döndürür ve JSON yanıtında hata bilgilerini içerir. Ayrıca, API istek oranı sınırlı ise, boş bir yanıt döndürür.

Hata işleme kodu hem de önceki işlevlerin çoğunu ayrılır. Aşağıdaki aşamalarda hatalar oluşabilir:

|Aşama|Olası hatalar|Tarafından işlenen|
|-|-|-|
|Yapı JavaScript istek nesnesi|Geçersiz URL|`try`/`catch` engelle|
|İsteği yapan|Ağ hataları, iptal edilen bağlantıları|`error` ve `abort` olay işleyicileri|
|Arama gerçekleştirme|Geçersiz istek, geçersiz JSON oran sınırları|içinde testleri `load` olay işleyicisi|

Hataları çağırarak işlenir `renderErrorMessage()` hata hakkında bilinen herhangi bir ayrıntıyı ile. Yanıt hata testlerinin tam gauntlet geçerse, diyoruz `renderSearchResults()` arama sonuçları sayfasında görüntülenecek.

## <a name="displaying-search-results"></a>Arama Sonuçları görüntüleme

Arama sonuçlarını görüntülemek için ana işlevi `renderSearchResults()`. Bu işlev Bing görüntü arama hizmeti tarafından döndürülen JSON alır ve görüntüler ve ilişkili aramaları varsa işler.

```javascript
function renderSearchResults(results) {

    // add Prev / Next links with result count
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);
    
    showDiv("results", renderImageResults(results.value));
    if (results.relatedSearches)
        showDiv("sidebar", renderRelatedItems(results.relatedSearches));
}
```

Ana görüntü arama sonuçları en üst düzey döndürülen `value` JSON yanıt nesnesi. Biz bizim işlevi geçirmek `renderImageResults()`, aralarında yineler ve her bir öğeyi HTML'e işlemek için ayrı bir işlevi çağırır. Sonuçta elde edilen HTML döndürülen `renderSearchResults()`, içine eklenen burada `results` sayfasındaki bölme.

```javascript
function renderImageResults(items) {
    var len = items.length;
    var html = [];
    if (!len) {
        showDiv("noresults", "No results.");
        hideDivs("paging1", "paging2");
        return "";
    }
    for (var i = 0; i < len; i++) {
        html.push(searchItemRenderers.images(items[i], i, len));
    }
    return html.join("\n\n");
}
```

Her kendi üst düzey nesnesindeki ilgili sonuçları dört farklı türde kadar Bing görüntü arama API döndürür. Bunlar:

|||
|-|-|
|`pivotSuggestions`|Özgün arama Özet Word'de farklı bir tarihle sorgular. Örneğin, "için kırmızı çiçekler" arıyorsanız, bir Özet sözcük "red" olabilir ve bir Özet öneri "sarı çiçekler." olabilir|
|`queryExpansions`|Daha fazla koşulları ekleyerek özgün arama daraltmak sorgular. Örneğin, "Microsoft Surface" için arama yaparsanız, sorgu genişletme olabilir "Microsoft Surface Pro."|
|`relatedSearches`|Ayrıca özgün araması girilen diğer kullanıcılar tarafından girilen sorgular. Örneğin, "Bağlama Sunucu1" için arama yaparsanız, ilgili arama "yüksekliğindeki olabilir Saint Helens."|
|`similarTerms`|Özgün arama anlamı benzer sorgular. "Kediler" için arama yaparsanız, örneğin, benzer bir terim "şirin." olabilir|

Daha önce görünen `renderSearchResults()`, biz yalnızca işleme `relatedItems` önerileri ve Yerleştir sayfanın Kenar çubuğunda bağlantılar sonuç.

## <a name="rendering-result-items"></a>Sonuç öğeleri oluşturma

Bizim JavaScript kodu bir nesne `searchItemRenderers`, içeren *Oluşturucu:* her biri için tür HTML'i işlevleri arama sonucu.

```javascript
searchItemRenderers = { 
    images: function(item, index, count) { ... },
    relatedSearches: function(item) { ... }
}
```

Oluşturucu işlevi aşağıdaki parametreleri kabul edebilir:

| | |
|-|-|
|`item`|Öğenin özelliklerini içeren JavaScript nesne URL'sini ve açıklamasını gibi.|
|`index`|Kendi koleksiyonundaki sonuç öğenin dizini.|
|`count`|Arama sonucu öğesi'nin koleksiyondaki öğe sayısı.|

`index` Ve `count` parametreleri sayı sonuçları başına veya sonuna bir koleksiyon için özel HTML öğeleri, belirli bir sayıda sonra satır sonları eklemek için oluşturmak için kullanılır ve benzeri. Bu işlev bir işleyici gerekmez, bu iki parametre kabul etmek gerekmez.

Daha yakın bir göz atalım `images` Oluşturucu:

```javascript
    images: function (item, index, count) {
        var height = 120;
        var width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
        var html = [];
        if (index === 0) html.push("<p class='images'>");
        var title = escape(item.name) + "\n" + getHost(item.hostPageDisplayUrl);
        html.push("<p class='images' style='max-width: " + width + "px'>");
        html.push("<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + 
            "' height=" + height + " width=" + width + "'>");
        html.push("<br>");
        html.push("<nobr><a href='" + item.contentUrl + "'>Image</a> - ");
        html.push("<a href='" + item.hostPageUrl + "'>Page</a></nobr><br>");
        html.push(title.replace("\n", " (").replace(/([a-z0-9])\.([a-z0-9])/g, "$1.<wbr>$2") + ")</p>");
        return html.join("");
    }, // relatedSearches renderer omitted
```

Bizim Görüntü Oluşturucu işlevi:

> [!div class="checklist"]
> * Görüntü küçük resimlerin boyutunu hesaplar (genişlik farklılık gösterir, en az 120 piksel ile 90 piksel çözünürlükte sabit bir yükseklik).
> * HTML derlemeler `<img>` görüntü küçük görüntülenecek etiket. 
> * HTML derlemeler `<a>` bağlantı görüntü ve içerdiği sayfa etiketler.
> * Görüntü ve üzerinde site hakkındaki bilgileri görüntüler açıklama oluşturur.

Biz test `index` eklemek için değişken bir `<p>` ilk resim sonucu önce etiketi. Aksi takdirde küçük resimleri birbirleri ve kaydırma sunmayı tarayıcı penceresinde gerektiğinde alın.

Küçük resimlerin boyutunu hem de kullanılan `<img>` etiketi ve `h` ve `w` alanları küçük resim ait URL. [Bing küçük resim hizmet](resize-and-crop-thumbnails.md) tam olarak bu boyut, bir küçük resim sunar.

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
> [Bing görüntü arama API Başvurusu](//docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)

