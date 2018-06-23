---
title: Bing varlık arama tek sayfalı web uygulaması | Microsoft Docs
description: Bir tek sayfalı Web uygulamasında Bing varlık arama API kullanmayı gösterir.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 12/08/2017
ms.author: v-jerkin
ms.openlocfilehash: 91c60913cd806baf100e5511cbf59299bf9a84f0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354922"
---
# <a name="tutorial-single-page-web-app"></a>Öğretici: Tek sayfa web uygulaması

Bing varlık arama API hakkında bilgi için Web arama sağlar *varlıklar* ve *yerleştirir.* Sonuç türü ya da her ikisi de belirli bir sorgu içinde isteyebilir. Yerler ve varlıkları tanımlarını aşağıda verilmiştir.

|||
|-|-|
|Varlıklar|İyi bilinen kişiler, yerler ve ada göre bulma noktalar|
|Yerler|Restoran, Oteller ve ada göre bulma diğer yerel işletmeler *veya* türüne (İtalyanca Restoran) göre|

Bu öğreticide, biz arama sonuçlarını görüntülemek için Bing varlık arama API'sini kullanan bir tek sayfalı Web uygulaması oluşturma sayfasındaki sağ. Uygulama, HTML, CSS ve JavaScript bileşenleri içerir.

API, sonuçları konuma göre öncelik sağlar. Mobil uygulamada, cihaz, kendi konumunu için sorabilirsiniz. Bir Web uygulamasında kullandığınız `getPosition()` işlevi. Ancak bu çağrı yalnızca güvenli bağlamlarda çalışır ve kesin bir konum sağlamayabilir. Ayrıca, kullanıcının kendi dışındaki bir konum yakın varlıklar için arama yapmak isteyebilirsiniz.

Bizim uygulama, bu nedenle enlem ve boylam kullanıcı tarafından girilen bir konumdan elde için Bing Haritalar hizmetini bağlı çağırır. Kullanıcı daha sonra yer işareti ("alanı iğnenin") veya tam veya kısmi bir adres ("New York şehrinde") adını girin ve Bing Haritalar API'si koordinatları sağlar.

<!-- Remove until we can replace with a sanitized version.
![[Single-page Bing Entity Search app]](media/entity-search-spa-demo.png)
-->

> [!NOTE]
> Sayfanın altındaki JSON ve HTTP başlıkları tıklatıldığında HTTP istek bilgileri ve JSON yanıt ortaya. Bu ayrıntılar hizmetini keşfetmeye yararlı olur.

Eğitmen uygulama gösterilmektedir nasıl yapılır:

> [!div class="checklist"]
> * Bir Bing varlık arama API çağrısı JavaScript'te gerçekleştirin
> * Bing Haritalar gerçekleştirmek `locationQuery` JavaScript API çağrısı
> * API çağrıları için arama seçeneklerini geçirin
> * Görüntü arama sonuçları
> * Bing istemci kimliği ve API Abonelik anahtarları işleme
> * Oluşabilecek hataları ile Dağıt

Öğretici sayfası tamamen bağımsızdır; herhangi bir dış çerçeveleri, stil sayfaları veya hatta resim dosyalarını kullanmaz. Yalnızca yaygın olarak desteklenen JavaScript dil özellikleri kullanır ve tüm ana Web tarayıcıları geçerli sürümlerinde çalışır.

Bu öğretici kapsamında, kaynak kodunu yalnızca seçili bölümlerini tartışın. Tam kaynak kodunu kullanılabilir [ayrı bir sayfaya](tutorial-bing-entities-search-single-page-app-source.md). Kopyalayın ve bu kodu bir metin düzenleyicisine yapıştırın ve kaydedileceği `bing.html`.

> [!NOTE]
> Bu öğretici önemli ölçüde benzer [tek sayfalı Bing Web arama uygulaması Öğreticisi](../Bing-Web-Search/tutorial-bing-web-search-single-page-app.md), ancak yalnızca varlık arama sonuçları ile ilgilidir.

## <a name="app-components"></a>Uygulama bileşenleri

Herhangi bir tek sayfalı Web uygulamasına gibi öğretici uygulama üç bölümleri içerir:

> [!div class="checklist"]
> * HTML - tanımlar sayfasının içeriği ve yapısı
> * Sayfa görünümünü tanımlayan CSS-
> * JavaScript - sayfanın davranışını tanımlar

Basit oldukları gibi Bu öğretici HTML veya CSS çoğunu ayrıntılı, kapsamaz.

HTML kullanıcı bir sorgu girer ve arama seçenekleri seçer arama formu içerir. Formun gerçekte göre arama gerçekleştirir JavaScript bağlı `<form>` etiketinin `onsubmit` özniteliği:

```html
<form name="bing" onsubmit="return newBingEntitySearch(this)">
```

`onsubmit` İşleyicisini döndürür `false`, hangi tutar formu bir sunucuya gönderildi. JavaScript kodu, aslında formdan gerekli bilgileri toplama ve arama gerçekleştirilirken çalışır.

Arama iki aşamada yapılır. İlk olarak, bir konum kısıtlaması kullanıcının girdiği, koordinatları dönüştürmek için Bing Haritalar sorgu yapılır. Bu sorgu için geri çağırma daha sonra devre dışı Bing varlık arama sorgusu başlatır.

HTML bölümleri de içerir (HTML `<div>` etiketleri) burada arama sonuçları görüntülenir.

## <a name="managing-subscription-keys"></a>Abonelik anahtarlarını yönetme

> [!NOTE]
> Bu uygulama, Bing arama API ve Bing Haritalar API'si için Abonelik anahtarları gerekiyor. Kullanabileceğiniz bir [deneme Bing arama anahtarı](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ve [temel Bing Haritalar anahtarı](https://www.microsoft.com/maps/create-a-bing-maps-key).

Bing arama ve Bing Haritalar API'si Abonelik anahtarları kodda dahil etmek zorunda kalmamak için tarayıcının kalıcı depolama bunları depolamak için kullanırız. İki anahtarı depolanmaz, biz de sor ve daha sonra kullanmak üzere saklayın. Anahtar daha sonra API tarafından reddedilirse, kullanıcı için sonraki aramalarına istenir böylece biz saklı anahtarı geçersiz.

Tanımlarız `storeValue` ve `retrieveValue` kullanın ya da işlevleri `localStorage` (tarayıcı destekliyorsa) nesnesi veya bir tanımlama bilgisi. Bizim `getSubscriptionKey()` işlevi depolamak ve kullanıcının anahtarı almak için bu işlevleri kullanır.

```javascript
// cookie names for data we store
SEARCH_API_KEY_COOKIE = "bing-search-api-key";
MAPS_API_KEY_COOKIE   = "bing-maps-api-key";
CLIENT_ID_COOKIE      = "bing-search-client-id";

// API endpoints
SEARCH_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/entities";
MAPS_ENDPOINT   = "http://dev.virtualearth.net/REST/v1/Locations";

// ... omitted definitions of storeValue() and retrieveValue()

// get stored API subscription key, or prompt if it's not found
function getSubscriptionKey(cookie_name, key_length, api_name) {
    var key = retrieveValue(cookie_name);
    while (key.length !== key_length) {
        key = prompt("Enter " + api_name + " API subscription key:", "").trim();
    }
    // always set the cookie in order to update the expiration date
    storeValue(cookie_name, key);
    return key;
}

function getMapsSubscriptionKey() {
    return getSubscriptionKey(MAPS_API_KEY_COOKIE, 64, "Bing Maps");
}

function getSearchSubscriptionKey() {
    return getSubscriptionKey(SEARCH_API_KEY_COOKIE, 32, "Bing Search");
}
```

HTML `<body>` etiketi de içeren bir `onload` çağırır özniteliği `getSearchSubscriptionKey()` ve `getMapsSubscriptionKey()` sayfa bittiği yükleniyor. Bunlar henüz bunları girmediyseniz hemen kendi anahtarları kullanıcıdan istemek için bu çağrılardan hizmet.

```html
<body onload="document.forms.bing.query.focus(); getSearchSubscriptionKey(); getMapsSubscriptionKey();">
```

## <a name="selecting-search-options"></a>Arama Seçenekleri

![[Bing varlık arama formu]](media/entity-search-spa-form.png)

HTML formu aşağıdaki denetimleri içerir:

| | |
|-|-|
|`where`|Arama için kullanılan Pazar (konumu ve dil) seçmek için açılır menü.|
|`query`|Metin alanı, arama terimlerini girin.|
|`safe`|Güvenli arama açık olup olmadığını belirten bir onay kutusu ("yetişkin" sonuçları kısıtlayan)|
|`what`|Varlıklar, yerler veya her ikisini de aramak seçmeye yönelik bir menü.|
|`mapquery`|Hangi kullanıcı girebilirsiniz tam veya kısmi bir adresi, bir yer işareti vb. Bing varlık arama dönüş daha ilgili sonuçları yardımcı olmak için metin alanı.|

> [!NOTE]
> Yerler sonuçları yalnızca Amerika Birleşik Devletleri'nde şu anda kullanılabilir. `where` Ve `what` Menülerine sahip bu kısıtlamayı zorlamak için kod. Yerler seçildiğinde, ancak bir ABD Pazar seçerseniz `what` menüsünde `what` için herhangi bir şey değiştirir. ABD dışındaki Pazar seçildiğinde yerler seçerseniz `where` menüsünde `where` ABD değiştirir.

Bizim JavaScript işlevi `bingSearchOptions()` bu alanlar için Bing arama API kısmi sorgu dizeye dönüştürür.

```javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.what.selectedIndex) options.push("responseFilter=" + form.what.value);
    return options.join("&");
}
```

Örneğin, güvenli arama özellik yüklenebilir `strict`, `moderate`, veya `off`, ile `moderate` varsayılan bırakılıyor. Ancak yalnızca iki durumlu sahip bir onay kutusu formumuzun kullanır. Bu çok ya da ayarı JavaScript kodu dönüştürür `strict` veya `off` (biz kullanmayan `moderate`).

`mapquery` Alan değil ele `bingSearchOptions()` değil Bing varlık arama için Bing Haritalar konum sorgu için kullanıldığından.

## <a name="obtaining-a-location"></a>Bir konum alma

Bing Haritalar API'si sunan bir [ `locationQuery` yöntemi](//msdn.microsoft.com/library/ff701711.aspx), hangi enlem bulmak için kullanırız ve kullanıcı konumunun boylam girer. Bu koordinatları sonra kullanıcının isteği ile Bing varlık arama API geçirilir. Arama sonuçlarını varlıkları ve belirtilen konum yakın olan yerlerde öncelik.

Sıradan kullanarak Bing Haritalar API'si erişemiyoruz `XMLHttpRequest` hizmet çıkış noktaları arası sorguları desteklemediği için bir Web uygulamasında sorgu. Neyse ki, JSONP ("P" "doldurulan için"'dır) destekler. Bir JSONP yanıt bir işlev çağrısında Sarmalanan sıradan bir JSON yanıtı ' dir. İstek kullanarak ekleyerek yapılan bir `<script>` belgeye etiketi. (Komut dosyaları yükleme tarayıcı güvenlik ilkelerini tabi değildir.)

`bingMapsLocate()` İşlev oluşturur ve ekler `<script>` sorgu için etiketi. `jsonp=bingMapsCallback` Sorgu dizesi kesimini Yanıtla çağrılacak işlevin adını belirtir.

```javascript
function bingMapsLocate(where) {

    where = where.trim();
    var url = MAPS_ENDPOINT + "?q=" + encodeURIComponent(where) + 
                "&jsonp=bingMapsCallback&maxResults=1&key=" + getMapsSubscriptionKey();

    var script = document.getElementById("bingMapsResult")
    if (script) script.parentElement.removeChild(script);

    // global variable holds reference to timer that will complete the search if the maps query fails
    timer = setTimeout(function() {
        timer = null;
        var form = document.forms.bing;
        bingEntitySearch(form.query.value, "", bingSearchOptions(form), getSearchSubscriptionKey());
    }, 5000);

    script = document.createElement("script");
    script.setAttribute("type", "text/javascript");
    script.setAttribute("id", "bingMapsResult");
    script.setAttribute("src", url);
    script.setAttribute("onerror", "BingMapsCallback(null)");
    document.body.appendChild(script);

    return false;
}
```

> [!NOTE]
> Bing Haritalar API'si yanıt vermezse `bingMapsCallBack()` işlevi hiçbir zaman çağrılır. Normalde, diğer bir deyişle, `bingEntitySearch()` olarak adlandırılmaz ve varlık arama sonuçlarında görünmez. Bu senaryonun olmaması için `bingMapsLocate()` ayrıca çağırmak için bir zamanlayıcı ayarlar `bingEntitySearch()` beş saniye sonra. Varlık iki kez aramadan önlemek için geri çağırma işlevi mantığı vardır.

Sorgu tamamladığında, `bingMapsCallback()` işlevi çağrıldığında, istendiği gibi.

```javascript
function bingMapsCallback(response) {

    if (timer) {    // we beat the timer; stop it from firing
        clearTimeout(timer);
        timer = null;
    } else {        // the timer beat us; don't do anything
        return; 
    }

    var location = "";
    var name = "";
    var radius = 1000;

    if (response) {
        try {
            if (response.statusCode === 401) {
                invalidateMapsKey();
            } else if (response.statusCode === 200) {
                var resource = response.resourceSets[0].resources[0];
                var coords   = resource.point.coordinates;
                name         = resource.name;

                // the radius is the largest of the distances between the location and the corners
                // of its bounding box (in case it's not in the center) with a minimum of 1 km
                try {
                    var bbox    = resource.bbox;
                    radius  = Math.max(haversineDistance(bbox[0], bbox[1], coords[0], coords[1]),
                                       haversineDistance(coords[0], coords[1], bbox[2], bbox[1]),
                                       haversineDistance(bbox[0], bbox[3], coords[0], coords[1]),
                                       haversineDistance(coords[0], coords[1], bbox[2], bbox[3]), 1000);
                } catch(e) {  }
                var location = "lat:" + coords[0] + ";long:" + coords[1] + ";re:" + Math.round(radius);
            }
        }
        catch (e) { }   // response is unexpected. this isn't fatal, so just don't provide location
    }

    var form = document.forms.bing;
    if (name) form.mapquery.value = name;
    bingEntitySearch(form.query.value, location, bingSearchOptions(form), getSearchSubscriptionKey());

}
```

Enlem ve boylam birlikte Bing varlık arama sorgusu gerektiren bir *RADIUS* konum bilgileri kesinliğini gösterir. RADIUS kullanan hesaplamak *sınırlayıcı kutu* Bing Haritalar yanıt verdi. Sınırlama kutusu konumu tümüyle çevreleyen dikdörtgen şeklinde. Örneğin, kullanıcının girdiği `NYC`, sonuç New York Şehir ve Semt kapsayan bir sınırlayıcı kutu kabaca merkezi koordinatlarını içerir. 

Biz öncelikle birincil koordinatları uzaklıkta her işlevini kullanarak sınırlama kutusu dört köşelerinde hesaplar `haversineDistance()` (gösterilmez). Bu dört uzaklıklar en büyük RADIUS kullanırız. Minimum RADIUS kilometre ' dir. Sınırlama kutusu yanıtı sağlanırsa bu değer varsayılan olarak da kullanılır.

Koordinatları ve RADIUS elde, ardından diyoruz `bingEntitySearch()` gerçek arama yapmak üzere.

## <a name="performing-the-search"></a>Arama gerçekleştirme

Verilen sorgu, bir konum, seçenekleri dizisi ve API anahtarını `BingEntitySearch()` işlevi Bing varlık arama isteği yapar.

```javascript
// perform a search given query, location, options string, and API keys
function bingEntitySearch(query, latlong, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("pole", "mainline", "sidebar", "_json", "_http", "error");

    var request = new XMLHttpRequest();
    var queryurl = SEARCH_ENDPOINT + "?q=" + encodeURIComponent(query) + "&" + options;

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

    if (latlong) request.setRequestHeader("X-Search-Location", latlong);

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
            if (jsobj._type === "SearchResponse") {
                renderSearchResults(jsobj);
            } else {
                renderErrorMessage("No search results in JSON response");
            }
        } else {
            renderErrorMessage("Empty response (are you sending too many requests too quickly?)");
        }
    if (divHidden("pole") && divHidden("mainline") && divHidden("sidebar")) 
        showDiv("noresults", "No results.<p><small>Looking for restaurants or other local businesses? Those currently areen't supported outside the US.</small>");
    }

    // Any other HTTP status is an error
    else {
        // 401 is unauthorized; force re-prompt for API key for next request
        if (this.status === 401) invalidateSearchKey();

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
> Başarılı bir HTTP isteği mu *değil* arama kendisini başarılı mutlaka anlamına. Arama işlemi bir hata oluşursa, Bing varlık arama API 200 HTTP durum kodu döndürür ve JSON yanıtında hata bilgilerini içerir. Ayrıca, API istek oranı sınırlı ise, boş bir yanıt döndürür.

Hata işleme kodu hem de önceki işlevlerin çoğunu ayrılır. Aşağıdaki aşamalarda hatalar oluşabilir:

|Aşama|Olası hatalar|Tarafından işlenen|
|-|-|-|
|Yapı JavaScript istek nesnesi|Geçersiz URL|`try`/`catch` engelle|
|İsteği yapan|Ağ hataları, iptal edilen bağlantıları|`error` ve `abort` olay işleyicileri|
|Arama gerçekleştirme|Geçersiz istek, geçersiz JSON oran sınırları|içinde testleri `load` olay işleyicisi|

Hataları çağırarak işlenir `renderErrorMessage()` hata hakkında bilinen herhangi bir ayrıntıyı ile. Yanıt hata testlerinin tam gauntlet geçerse, diyoruz `renderSearchResults()` arama sonuçları sayfasında görüntülenecek.

## <a name="displaying-search-results"></a>Arama Sonuçları görüntüleme

Bing varlık arama API [belirli bir sırada sonuçları görüntülemek gerektiren](use-display-requirements.md). API iki farklı türde yanıtlar döndürebilir olduğundan, bu üst düzey yinelemek yeterli değil `Entities` veya `Places` JSON yanıt koleksiyonunda ve bu sonuçları görüntüler. (Yalnızca bir sonuç türü istiyorsanız kullanın `responseFilter` sorgu parametresi.)

Bunun yerine, kullanırız `rankingResponse` görüntü için sonuçları sıralamak için arama sonuçlarında koleksiyonu. Öğe bu nesnenin başvurduğu `Entitiess` ve/veya `Places` koleksiyonları.

`rankingResponse` Belirtilen arama sonuçlarının en çok üç koleksiyonları içerebilir `pole`, `mainline`, ve `sidebar`. 

`pole`, varsa, en uygun arama sonuç ve görüntülenmelidir. `mainline` Arama sonuçlarını toplu ifade eder. Mainline sonuçları hemen sonra görüntülenmesi gereken `pole` (veya ilk `pole` mevcut değil). 

Son olarak. `sidebar` yardımcı arama sonuçlarını gösterir. Bunlar, gerçek bir kenar veya yalnızca mainline sonuçları sonra görüntülenebilir. Biz ikinci öğretici uygulamamıza seçtiniz.

Her öğe bir `rankingResponse` koleksiyonu iki farklı, ancak eşdeğer şekilde gerçek arama sonucu öğelerini başvuruyor.

| | |
|-|-|
|`id`|`id` Bir URL gibi görünüyor, ancak bağlantıları için kullanılmamalıdır. `id` Derecelendirme sonuç türü ile eşleşen `id` öğesinin ya da bir arama sonucu bir yanıt koleksiyonundaki *veya* tüm yanıt koleksiyonu (gibi `Entities`).
|`answerType`<br>`resultIndex`|`answerType` Sonucu içeren üst düzey yanıt koleksiyona ifade eder (örneğin, `Entities`). `resultIndex` Sonucunun dizin, koleksiyondaki başvuruyor. Varsa `resultIndex` olan atlanırsa, tüm koleksiyon derecelendirme sonuç başvuruyor.

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

    // for each possible section, render the resuts from that section
    for (section in {pole: 0, mainline: 0, sidebar: 0}) {
        if (results.rankingResponse[section])
            showDiv(section, renderResultsItems(section, results));
    }
}
```

## <a name="rendering-result-items"></a>Sonuç öğeleri oluşturma

Bizim JavaScript kodu bir nesne `searchItemRenderers`, içeren *Oluşturucu:* her biri için tür HTML'i işlevleri arama sonucu.

```javascript
searchItemRenderers = { 
    entities: function(item) { ... },
    places: function(item) { ... }
}
```

Oluşturucu işlevi aşağıdaki parametreleri kabul edebilir:

| | |
|-|-|
|`item`|Öğenin özelliklerini içeren JavaScript nesne URL'sini ve açıklamasını gibi.|
|`index`|Kendi koleksiyonundaki sonuç öğenin dizini.|
|`count`|Arama sonucu öğesi'nin koleksiyondaki öğe sayısı.|

`index` Ve `count` parametreleri sayı sonuçları başına veya sonuna bir koleksiyon için özel HTML öğeleri, belirli bir sayıda sonra satır sonları eklemek için oluşturmak için kullanılır ve benzeri. Bu işlev bir işleyici gerekmez, bu iki parametre kabul etmek gerekmez. Aslında, biz bunları işleyicilerde öğretici uygulamamıza için kullanmayın.

Daha yakın bir göz atalım `entities` Oluşturucu:

```javascript
    entities: function(item) {
        var html = [];
        html.push("<p class='entity'>");
        if (item.image) {
            var img = item.image;
            if (img.hostPageUrl) html.push("<a href='" + img.hostPageUrl + "'>");
            html.push("<img src='" + img.thumbnailUrl +  "' title='" + img.name + "' height=" + img.height + " width= " + img.width + ">");
            if (img.hostPageUrl) html.push("</a>");
            if (img.provider) {
                var provider = img.provider[0];
                html.push("<small>Image from ");
                if (provider.url) html.push("<a href='" + provider.url + "'>");
                html.push(provider.name ? provider.name : getHost(provider.url));
                if (provider.url) html.push("</a>");
                html.push("</small>");
            }
        }
        html.push("<p>");
        if (item.entityPresentationInfo) {
            var pi = item.entityPresentationInfo;
            if (pi.entityTypeHints || pi.entityTypeDisplayHint) {
                html.push("<i>");
                if (pi.entityTypeDisplayHint) html.push(pi.entityTypeDisplayHint);
                else if (pi.entityTypeHints) html.push(pi.entityTypeHints.join("/"));
                html.push("</i> - ");
            }
        }
        html.push(item.description);
        if (item.webSearchUrl) html.push("&nbsp;<a href='" + item.webSearchUrl + "'>More</a>")
        if (item.contractualRules) {
            html.push("<p><small>");
            var rules = [];
            for (var i = 0; i < item.contractualRules.length; i++) {
                var rule = item.contractualRules[i];
                var link = [];
                if (rule.license) rule = rule.license;
                if (rule.url) link.push("<a href='" + rule.url + "'>");
                link.push(rule.name || rule.text || rule.targetPropertyName + " source");
                if (rule.url) link.push("</a>");
                rules.push(link.join(""));
            }
            html.push("License: " + rules.join(" - "));
            html.push("</small>");
        }
        return html.join("");
    }, // places renderer omitted
```

Bizim varlık oluşturucu işlevi:

> [!div class="checklist"]
> * HTML derlemeler `<img>` varsa ve görüntü küçük görüntülenecek etiket. 
> * HTML derlemeler `<a>` görüntüsünü içeren sayfa bağlanan etiketi.
> * Görüntü ve üzerinde site hakkındaki bilgileri görüntüler açıklama oluşturur.
> * Görüntü ipuçlarını kullanarak varlığın sınıflandırma varsa içerir.
> * Bing arama varlığı hakkında daha fazla bilgi almak için bir bağlantı içerir.
> * Veri kaynakları tarafından gerekli lisans ya da attribution bilgileri görüntüler.

## <a name="persisting-client-id"></a>Kalıcı istemci kimliği

Bing arama API'leri yanıtlarının içerebilir bir `X-MSEdge-ClientID` geri API art arda gelen istekleri ile gönderilmesi gereken üstbilgi. Birden çok Bing arama API'leri kullanılıyorsa, aynı istemci kimliği, bunların tümünün ile mümkünse kullanılmalıdır.

Sağlama `X-MSEdge-ClientID` üstbilgi iki önemli faydası vardır kullanıcının aramaları ilişkilendirilecek Bing API'ler sağlar.

İlk olarak, kullanıcının daha iyi karşılamak sonuçları bulmak için arama için bağlam uygulamak arama motoru Bing sağlar. Örneğin, bir kullanıcı daha önce Yelkenli için ilgili koşulları için aradı, "noktalarını" sonraki Ara tercihen bir BalıkçıTeknesi sabitlemek için yerler hakkında bilgi döndürebilir.

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
> [Bing varlık arama API Başvurusu](//docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference)

> [!div class="nextstepaction"]
> [Bing Haritalar API'si belgeleri](//msdn.microsoft.com/library/dd877180.aspx)
