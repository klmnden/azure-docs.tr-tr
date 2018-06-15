---
title: Azure API Management dönüştürme ilkelerini | Microsoft Docs
description: Azure API Management'te kullanıma dönüştürme ilkeleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: miaojiang
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 3eb9d6851c30f11980d47d4e48b158217e41995d
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
ms.locfileid: "30233794"
---
# <a name="api-management-transformation-policies"></a>API Management dönüştürme ilkelerini
Bu konu aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](http://go.microsoft.com/fwlink/?LinkID=398186).

##  <a name="TransformationPolicies"></a> Dönüştürme ilkelerini

-   [XML ve JSON Dönüştür](api-management-transformation-policies.md#ConvertJSONtoXML) - dönüştürür isteği veya yanıtı XML ve JSON öğesinden gövde.

-   [XML JSON biçimine Dönüştür](api-management-transformation-policies.md#ConvertXMLtoJSON) - dönüştürür isteği veya yanıtı için JSON XML'den gövde.

-   [Bulma ve değiştirme dizesi gövdesi içinde](api-management-transformation-policies.md#Findandreplacestringinbody) - bir istek veya yanıt alt dizeyi bulur ve farklı bir dizeyle değiştirir.

-   [İçeriği URL'leri maske](api-management-transformation-policies.md#MaskURLSContent) -yanıta bağlantı (maskeleri)'yeniden yazar gövde böylece bunlar ağ geçidi aracılığıyla eşdeğer bağlantı üzerine gelin.

-   [Arka uç hizmetini ayarlamak](api-management-transformation-policies.md#SetBackendService) -gelen bir istek için arka uç hizmeti değiştirir.

-   [Gövde ayarlamak](api-management-transformation-policies.md#SetBody) -ileti gövdesi gelen ve giden istekleri için ayarlar.

-   [HTTP üstbilgisi kümesi](api-management-transformation-policies.md#SetHTTPheader) - değer atayan bir varolan yanıt ve/veya istek üstbilgisi veya yeni bir yanıt ve/veya istek üstbilgisi ekler.

-   [Sorgu dizesi parametresi kümesi](api-management-transformation-policies.md#SetQueryStringParameter) - ekler, değiştirir değerini veya istek sorgu dizesi parametresi siler.

-   [URL yeniden yazma](api-management-transformation-policies.md#RewriteURL) -bir istek URL'sini ortak formundan web hizmeti tarafından beklenen biçime dönüştürür.

-   [XSLT kullanarak XML dönüştürme](api-management-transformation-policies.md#XSLTransform) -XSL dönüşümü istek veya yanıt gövdesinde XML için geçerlidir.

##  <a name="ConvertJSONtoXML"></a> XML ve JSON Dönüştür
 `json-to-xml` İlkesi JSON öğesinden bir istek veya yanıt gövdesi XML'e dönüştürür.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false" parse-date="true | false"/>
```

### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <base />
    </inbound>
    <outbound>
        <base />
        <json-to-xml apply="always" consider-accept-header="false" parse-date="false"/>
    </outbound>
</policies>
```

### <a name="elements"></a>Öğeleri

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|json-to-xml|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|uygula|Özniteliği aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> -her zaman - her zaman dönüştürme uygulanır.<br />Yalnızca yanıt Content-Type üstbilgisi JSON varlığını gösteriyorsa - içerik türü json - dönüştürülemiyor.|Evet|Yok|
|consider-accept-header|Özniteliği aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> JSON istek Accept üstbilgisi isteniyorsa - true - dönüştürme uygulanır.<br />-false - her zaman dönüştürme uygulayın.|Hayır|true|
|Ayrıştırma tarihi|Ayarlandığında `false` tarih değerlerini dönüştürme sırasında yalnızca kopyalanır|Hayır|true|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümleri:** gelen, giden, hata

-   **İlke kapsamları:** genel, ürün, API işlemi

##  <a name="ConvertXMLtoJSON"></a> XML JSON biçimine Dönüştür
 `xml-to-json` İlkesi XML'den bir istek veya yanıt gövdesi JSON değerine dönüştürür. Bu ilke, yalnızca XML arka uç web hizmetlerini temel alarak API'leri modernize için kullanılabilir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>
```

### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <base />
    </inbound>
    <outbound>
        <base />
        <xml-to-json kind="direct" apply="always" consider-accept-header="false" />
    </outbound>
</policies>
```

### <a name="elements"></a>Öğeleri

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|xml-to-json|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|türü|Özniteliği aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> javascript-kolay - dönüştürülen JSON JavaScript geliştiricileri için kolay bir form sahiptir.<br />-doğrudan - dönüştürülen JSON özgün XML belge yapısını yansıtır.|Evet|Yok|
|uygula|Özniteliği aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> -her zaman - her zaman dönüştürün.<br />Yalnızca yanıt Content-Type üstbilgisi XML varlığını gösteriyorsa - içerik türü xml - dönüştürülemiyor.|Evet|Yok|
|consider-accept-header|Özniteliği aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> XML isteği Accept üstbilgisi isteniyorsa - true - dönüştürme uygulanır.<br />-false - her zaman dönüştürme uygulayın.|Hayır|true|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümleri:** gelen, giden, hata

-   **İlke kapsamları:** genel, ürün, API işlemi

##  <a name="Findandreplacestringinbody"></a> Bulma ve gövde dizesinde değiştirme
 `find-and-replace` İlke isteği veya yanıtı alt dizeyi bulur ve farklı bir dizeyle değiştirir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<find-and-replace from="what to replace" to="replacement" />
```

### <a name="example"></a>Örnek

```xml
<find-and-replace from="notebook" to="laptop" />
```

### <a name="elements"></a>Öğeleri

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|Bul ve Değiştir|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|başlangıç|Aranacak dize.|Evet|Yok|
|-|Değiştirme dizesi. Arama dizesi kaldırmak için bir sıfır uzunluk değiştirme dizesini belirtin.|Evet|Yok|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümleri:** gelen, giden arka uç, hata

-   **İlke kapsamları:** genel, ürün, API işlemi

##  <a name="MaskURLSContent"></a> İçerik maskesi URL'leri
 `redirect-content-urls` İlkesi böylece bunlar ağ geçidi aracılığıyla eşdeğer bağlantı noktası yanıt gövdesi (maskeleri) bağlantıları yeniden yazar. Giden bölümünde yanıt gövdesi bağlantılar için ağ geçidi'nin üzerine olmalarını yeniden yazmak için kullanın. Gelen bölümünde ters efekt için kullanın.

> [!NOTE]
>  Bu ilke tüm üstbilgi değerleri gibi değiştirmez `Location` üstbilgileri. Üstbilgi değerlerini değiştirmek için kullanın [set üstbilgisi](api-management-transformation-policies.md#SetHTTPheader) ilkesi.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<redirect-content-urls />
```

### <a name="example"></a>Örnek

```xml
<redirect-content-urls />
```

### <a name="elements"></a>Öğeleri

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|redirect-content-urls|Kök öğesi.|Evet|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümleri:** gelen, giden

-   **İlke kapsamları:** genel, ürün, API işlemi

##  <a name="SetBackendService"></a> Arka uç hizmet belirleme
 Kullanım `set-backend-service` gelen bir istek bu işlem için API ayarlarında belirtilenden farklı bir arka uç yeniden yönlendirmek için ilke. Bu ilke, ilkede belirtilen karmayla gelen isteği arka uç hizmeti temel URL'sini değiştirir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-backend-service base-url="base URL of the backend service" />
```

### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <choose>
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2013-05")">
                <set-backend-service base-url="http://contoso.com/api/8.2/" />
            </when>
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2014-03")">
                <set-backend-service base-url="http://contoso.com/api/9.1/" />
            </when>
        </choose>
        <base />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```
Bu örnekte, farklı arka uç hizmetine bir API çağrısında belirtilen sorgu dizesinde geçirilen sürüm değere göre isteklerini kümesi arka uç hizmet ilkesi yönlendirir.

Başlangıçta arka uç hizmeti temel URL API ayarlarından türetilir. Bu nedenle istek URL'si `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` hale `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` burada `http://contoso.com/api/10.4/` API ayarlarında belirtilen arka uç hizmeti URL.

Zaman [< seçin\> ](api-management-advanced-policies.md#choose) ilke bildirimi uygulanır arka uç hizmeti temel URL ya da yeniden değişebilir `http://contoso.com/api/8.2` veya `http://contoso.com/api/9.1`sürüm istek sorgu parametresinin değeri bağlı olarak. Örneğin, değer ise `"2013-15"` URL hale son isteği `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.

Daha fazla dönüştürme isteği olup olmadığını istenen, diğer [dönüştürme ilkelerini](api-management-transformation-policies.md#TransformationPolicies) kullanılabilir. Örneğin, istek sürüm belirli arka ucuna yönlendirilen artık version sorgu parametresi kaldırmak için [ayarlamak sorgu dizesi parametresi](api-management-transformation-policies.md#SetQueryStringParameter) İlkesi, şimdi yedek sürüm özniteliğini kaldırmak için kullanılabilir.

### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <set-backend-service backend-id="my-sf-service" sf-partition-key="@(context.Request.Url.Query.GetValueOrDefault("userId","")" sf-replica-type="primary" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```
Bu örnekte, bölüm anahtarı olarak UserID sorgu dizesi kullanılarak ve birincil çoğaltmasını kullanarak bir service fabric arka ucuna, isteği ilkesi yönlendirir.

### <a name="elements"></a>Öğeleri

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|set-backend-service|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|Taban URL'si|Yeni arka uç hizmeti temel URL'si.|Hayır|Yok|
|backend-id|Yönlendirmek için arka uç tanımlayıcısı.|Hayır|Yok|
|BT bölüm anahtarı|Yalnızca arka uç Service Fabric hizmeti ve 'arka uç-ID' kullanılarak belirtilen olduğunda geçerlidir. Belirli bir ad çözümleme hizmeti bölümünden çözmek için kullanılır.|Hayır|Yok|
|BT çoğaltma türü|Yalnızca arka uç Service Fabric hizmeti ve 'arka uç-ID' kullanılarak belirtilen olduğunda geçerlidir. İstek bir bölüm birincil veya ikincil çoğaltmasının görünmeliyse denetler. |Hayır|Yok|
|sf-resolve-condition|Yalnızca arka uç Service Fabric hizmeti olduğunda geçerlidir. Service Fabric arka uç çağrısı ile yeni çözünürlüğü yinelenmesini varsa tanımlama koşulu.|Hayır|Yok|
|sf-service-instance-name|Yalnızca arka uç Service Fabric hizmeti olduğunda geçerlidir. Hizmet örnekleri çalışma zamanında değiştirmesine olanak verir. |Hayır|Yok|
|sf-listener-name|Yalnızca arka uç Service Fabric hizmeti ve 'arka uç-ID' kullanılarak belirtilen olduğunda geçerlidir. Service Fabric güvenilir hizmetler, bir hizmet olarak birden çok dinleyici oluşturmanızı sağlar. Bu öznitelik, bir arka uç güvenilir hizmet birden çok dinleyici olduğunda belirli bir dinleyici seçmek için kullanılır. Bu özniteliği belirtilmezse, API Management bir ad olmadan bir dinleyici kullanmayı dener. Adı olmayan bir dinleyici güvenilir hizmetler için tek bir dinleyicisi olması normaldir. |Hayır|Yok|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümleri:** gelen, arka uç

-   **İlke kapsamları:** genel, ürün, API işlemi

##  <a name="SetBody"></a> Set gövdesi
 Kullanım `set-body` gelen ve giden istekleri için ileti gövdesi ayarlamak için ilke. Kullanabileceğiniz ileti gövdesi erişmek için `context.Request.Body` özelliği veya `context.Response.Body`ilkesi gelen veya giden bölümünde olmasına bağlı olarak.

> [!IMPORTANT]
>  Varsayılan olarak, ileti eriştiğinizde kullanarak gövde unutmayın `context.Request.Body` veya `context.Response.Body`, özgün ileti gövdesi kaybolur ve geri ifadesinde gövdesi döndürerek ayarlamanız gerekir. Gövde içeriği korumak üzere ayarlanmış `preserveContent` parametresi `true` ileti erişirken. Varsa `preserveContent` ayarlanır `true` ve döndürülen gövde kullanılır farklı gövde ifade tarafından döndürülür.
>
>  Lütfen kullanırken aşağıdaki noktaları unutmayın `set-body` ilkesi.
>
>  -   Kullanıyorsanız `set-body` ayarlamak gerekmez yeni veya güncelleştirilmiş bir gövde döndürmek için ilke `preserveContent` için `true` olduğundan yeni gövdesi içeriği açıkça sağlamış olursunuz.
> -   Gelen ardışık düzeninde bir yanıt içeriği koruma, olmadığından yanıt henüz doesn't make Sense.
> -   Giden ardışık düzeninde bir isteğin içerik koruma, çünkü istek zaten arka ucuna bu noktada gönderildi doesn't make Sense.
> -   Örneğin ileti gövdesi yok olduğunda bu ilkeyi kullandıysanız, gelen bir GET içinde bir özel durum oluşur.

 Daha fazla bilgi için bkz: `context.Request.Body`, `context.Response.Body`ve `IMessage` bölümler [bağlamının](api-management-policy-expressions.md#ContextVariables) tablo.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-body>new body value as text</set-body>
```

### <a name="examples"></a>Örnekler

#### <a name="literal-text-example"></a>Değişmez değer metni örneği

```xml
<set-body>Hello world!</set-body>
```

#### <a name="example-accessing-the-body-as-a-string-note-that-we-are-preserving-the-original-request-body-so-that-we-can-access-it-later-in-the-pipeline"></a>Örnek, dize olarak gövdesi erişme. Böylece biz ardışık düzeninde erişebilir biz özgün istek gövdesi koruma olduğunu unutmayın.

```xml
<set-body>
@{ 
    string inBody = context.Request.Body.As<string>(preserveContent: true); 
    if (inBody[0] =='c') { 
        inBody[0] = 'm'; 
    } 
    return inBody; 
}
</set-body>
```

#### <a name="example-accessing-the-body-as-a-jobject-note-that-since-we-are-not-reserving-the-original-request-body-accesing-it-later-in-the-pipeline-will-result-in-an-exception"></a>Örnek, gövde bir JObject olarak erişme. Biz özgün istek gövdesi, bunu daha sonra düzenindeki bir özel durumla sonuçlanır belirtilmemelidir ayırma değil bu yana unutmayın.

```xml
<set-body> 
@{ 
    JObject inBody = context.Request.Body.As<JObject>(); 
    if (inBody.attribute == <tag>) { 
        inBody[0] = 'm'; 
    } 
    return inBody.ToString(); 
} 
</set-body>

```

#### <a name="filter-response-based-on-product"></a>Ürüne göre filtre yanıtı
 Bu örnek veri öğeleri kullanırken arka uç hizmetinden alınan yanıtı kaldırarak içerik filtreleme gerçekleştirmek nasıl gösterir `Starter` ürün. Yapılandırma ve bu ilkeyi kullanan bir örnek için bkz: [bulut kapak bölüm 177: daha Vlad Vinogradsky ile yönetim özelliklerini API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 34:30 için ileri sarma. Genel bir bakış görmek 31:50 Başlat [tahmin koyu Sky API](https://developer.forecast.io/) bu tanıtım için kullanılır.

```xml
<!-- Copy this snippet into the outbound section to remove a number of data elements from the response received from the backend service based on the name of the api product -->
<choose>
  <when condition="@(context.Response.StatusCode == 200 && context.Product.Name.Equals("Starter"))">
    <set-body>@{
        var response = context.Response.Body.As<JObject>();
        foreach (var key in new [] {"minutely", "hourly", "daily", "flags"}) {
          response.Property (key).Remove ();
        }
        return response.ToString();
      }
    </set-body>
  </when>
</choose>
```

### <a name="using-liquid-templates-with-set-body"></a>Set gövde ile sıvı şablonlarını kullanma
`set-body` İlkesi kullanmak üzere yapılandırılabilir [sıvı](https://shopify.github.io/liquid/basics/introduction/) transfom bir istek veya yanıt gövdesi için şablon dili. Bu, ileti biçimi tamamen yeniden şekillendirmek gerekiyorsa çok etkili olabilir.

> [!IMPORTANT]
> Sıvı uyarlamasını kullanılan `set-body` ilke ', C# modu' yapılandırılır. Bu filtreleme gibi şeyler yaparken özellikle önemlidir. Örnek olarak, bir tarih filtresi kullanarak Pascal kullanılmasını gerektiren büyük/küçük harf ve C# tarih biçimlendirme örn:
>
> {{body.foo.startDateTime| Date:"yyyyMMddTHH:mm:ddZ"}}

> [!IMPORTANT]
> Doğru sıvı şablonu kullanarak bir XML gövdesi bağlamak amacıyla kullanmak bir `set-header` Content-Type ayarlamak için ilke ya da uygulama/xml, text/xml (veya tüm ile biten türü + xml) için; JSON gövdesi, uygulama/json olmalıdır metin/json (veya ile biten herhangi bir türü + JSON).

#### <a name="convert-json-to-soap-using-a-liquid-template"></a>JSON sıvı bir şablon kullanarak SOAP Dönüştür
```xml
<set-body template="liquid">
    <soap:Envelope xmlns="http://tempuri.org/" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
        <soap:Body>
            <GetOpenOrders>
                <cust>{{body.getOpenOrders.cust}}</cust>
            </GetOpenOrders>
        </soap:Body>
    </soap:Envelope>
</set-body>
```

#### <a name="tranform-json-using-a-liquid-template"></a>Dönüşüm sıvı bir şablon kullanarak JSON
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a>Öğeleri

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|set-gövdesi|Kök öğesi. Gövde metin ya da body döndüren bir ifade içeriyor.|Evet|

### <a name="properties"></a>Özellikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|şablon|Gövde ilkesini ayarlama çalışacak şablon modunu değiştirmek için kullanılır. Şu anda desteklenen tek değerdir:<br /><br />-Sıvı - gövde ilkesini ayarlama sıvı şablon altyapısı da kullanır |Hayır|Sıvı|

İstek ve yanıt bilgilerine erişmek için sıvı şablon aşağıdaki özelliklere sahip bir bağlam nesnesi bağlayabilirsiniz: <br />
<pre>context.
    Request.
        Url
        Method
        OriginalMethod
        OriginalUrl
        IpAddress
        MatchedParameters
        HasBody
        ClientCertificates
        Headers

    Response.
        StatusCode
        Method
        Headers
Url.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString

OriginalUrl.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString
</pre>



### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümleri:** gelen, giden arka uç

-   **İlke kapsamları:** genel, ürün, API işlemi

##  <a name="SetHTTPheader"></a> HTTP üstbilgisi kümesi
 `set-header` İlke değer atayan bir varolan yanıt ve/veya istek üstbilgisi veya yeni bir yanıt ve/veya istek üstbilgisi ekler.

 HTTP üstbilgilerin listesi HTTP iletisine ekler. Bir gelen ardışık düzeninde yerleştirildiğinde, bu ilkeyi hedef hizmete geçirilen İstek HTTP üstbilgilerini ayarlar. Giden ardışık düzeninde yerleştirildiğinde, bu ilke ağ geçidi'nin istemciye gönderilen yanıtı için HTTP üstbilgilerini ayarlar.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-header name="header name" exists-action="override | skip | append | delete">
    <value>value</value> <!--for multiple headers with the same name add additional value elements-->
</set-header>
```

### <a name="examples"></a>Örnekler

#### <a name="example"></a>Örnek

```xml
<set-header name="some header name" exists-action="override">
    <value>20</value>
</set-header>
```

#### <a name="forward-context-information-to-the-backend-service"></a>Arka uç hizmetine bağlam bilgilerini ilet
 Bu örnek, arka uç hizmetine bağlam bilgilerini sağlamak için API düzeyinde ilkeyi uygulamak gösterilmiştir. Yapılandırma ve bu ilkeyi kullanan bir örnek için bkz: [bulut kapak bölüm 177: daha Vlad Vinogradsky ile yönetim özelliklerini API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 10:30 için ileri sarma. 12:10 işyerindeki İlkesi görebileceğiniz bir işlem Geliştirici Portalı'nda arama demo yoktur.

```xml
<!-- Copy this snippet into the inbound element to forward some context information, user id and the region the gateway is hosted in, to the backend service for logging or evaluation -->
<set-header name="x-request-context-data" exists-action="override">
  <value>@(context.User.Id)</value>
  <value>@(context.Deployment.Region)</value>
</set-header>
```

 Daha fazla bilgi için bkz: [ilke ifadelerini](api-management-policy-expressions.md) ve [bağlamının](api-management-policy-expressions.md#ContextVariables).

### <a name="elements"></a>Öğeleri

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|set üstbilgisi|Kök öğesi.|Evet|
|değer|Ayarlanacak üstbilgi değerini belirtir. Aynı ada sahip birden çok üstbilgi ek eklemek için `value` öğeleri.|Evet|

### <a name="properties"></a>Özellikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|Mevcut eylem|Üstbilgi zaten belirtildiğinde gerçekleştirilecek eylemi belirtir. Bu öznitelik aşağıdaki değerlerden birine sahip olmalıdır.<br /><br /> -Geçersiz Kıl - varolan üstbilgisinin değerini değiştirir.<br />-skip - varolan üstbilgi değeri değiştirmez.<br />-Ekle - Varolan üstbilgi değeri değer ekler.<br />-delete - istekten üstbilgiyi kaldırır.<br /><br /> Ayarlandığında `override` aynı ada sahip birden çok girdi kaydetme sonuçları (birden çok kez listelenir) tüm girdisi göre ayarlanan üstbilgisinde; yalnızca listelenen değerler sonuç ayarlanır.|Hayır|override|
|ad|Ayarlanacak üstbilginin adını belirtir.|Evet|Yok|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümleri:** gelen, giden arka uç, hata

-   **İlke kapsamları:** genel, ürün, API işlemi

##  <a name="SetQueryStringParameter"></a> Set sorgu dizesi parametresi
 `set-query-parameter` İlkesi ekler, değiştirir değeri, veya silme isteği sorgu dizesi parametresi. Hiçbir zaman istekte mevcut veya sorgu parametreleri, isteğe bağlı olan arka uç hizmeti tarafından beklenen geçirmek için kullanılabilir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-query-parameter name="param name" exists-action="override | skip | append | delete">
    <value>value</value> <!--for multiple parameters with the same name add additional value elements-->
</set-query-parameter>
```

### <a name="examples"></a>Örnekler

#### <a name="example"></a>Örnek

```xml

<set-query-parameter>
  <parameter name="api-key" exists-action="skip">
    <value>12345678901</value>
  </parameter>
  <!-- for multiple parameters with the same name add additional value elements -->
</set-query-parameter>

```

#### <a name="forward-context-information-to-the-backend-service"></a>Arka uç hizmetine bağlam bilgilerini ilet
 Bu örnek, arka uç hizmetine bağlam bilgilerini sağlamak için API düzeyinde ilkeyi uygulamak gösterilmiştir. Yapılandırma ve bu ilkeyi kullanan bir örnek için bkz: [bulut kapak bölüm 177: daha Vlad Vinogradsky ile yönetim özelliklerini API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 10:30 için ileri sarma. 12:10 işyerindeki İlkesi görebileceğiniz bir işlem Geliştirici Portalı'nda arama demo yoktur.

```xml
<!-- Copy this snippet into the inbound element to forward a piece of context, product name in this example, to the backend service for logging or evaluation -->
<set-query-parameter name="x-product-name" exists-action="override">
  <value>@(context.Product.Name)</value>
</set-query-parameter>

```

 Daha fazla bilgi için bkz: [ilke ifadelerini](api-management-policy-expressions.md) ve [bağlamının](api-management-policy-expressions.md#ContextVariables).

### <a name="elements"></a>Öğeleri

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|set-query-parameter|Kök öğesi.|Evet|
|değer|Ayarlanacak sorgu parametresinin değeri belirtir. Aynı ada sahip birden çok sorgu parametreleri ek eklemek için `value` öğeleri.|Evet|

### <a name="properties"></a>Özellikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|Mevcut eylem|Sorgu parametresi zaten belirtildiğinde gerçekleştirilecek eylemi belirtir. Bu öznitelik aşağıdaki değerlerden birine sahip olmalıdır.<br /><br /> -Geçersiz Kıl - varolan parametresinin değerini değiştirir.<br />-skip - varolan sorgusu parametre değeri değiştirmez.<br />-Ekle - Varolan sorgu parametresi değeri değer ekler.<br />-delete - sorgu parametresi istekten kaldırır.<br /><br /> Ayarlandığında `override` aynı ada sahip birden çok girdi kaydetme sonuçlanıyor (birden çok kez listelenir) tüm girdisi göre ayarlanan sorgu parametresi; yalnızca listelenen değerler sonuç ayarlanır.|Hayır|override|
|ad|Ayarlanacak sorgu parametresi adını belirtir.|Evet|Yok|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümleri:** gelen, arka uç

-   **İlke kapsamları:** genel, ürün, API işlemi

##  <a name="RewriteURL"></a> URL yeniden yazma
 `rewrite-uri` İlkesi dönüştürür istek URL'si ortak formundan web hizmeti tarafından beklenen form aşağıdaki örnekte gösterildiği gibi.

-   Ortak URL- `http://api.example.com/storenumber/ordernumber`

-   İstek URL- `http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`

 Bu ilke, İnsan ve/veya tarayıcı kolay URL web hizmeti tarafından beklenen URL biçimine dönüştürülmesi gereken olduğunda kullanılabilir. Bu ilke yalnızca temiz URL'leri, RESTful URL'leri, kullanıcı dostu URL'leri veya bir sorgu dizesi içermiyor ve bunun yerine yalnızca kaynak (yolunu içeren zamanıyla ilgili yapısal URL'leri SEO dostu URL'leri gibi alternatif bir URL biçimi gösterme ulaştığında uygulanacak gerekiyor Şema ve yetkili sonra). Bu genellikle estetik, kullanılabilirlik veya arama motoru iyileştirmesi (SEO) amacıyla yapılır.

> [!NOTE]
>  İlkeyi kullanarak sorgu dizesi parametreleri yalnızca ekleyebilirsiniz. Ek şablon yol parametreleri yeniden yazma URL'de ekleyemezsiniz.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<rewrite-uri template="uri template" copy-unmatched-params="true | false" />
```

### <a name="example"></a>Örnek

```xml
<policies>
    <inbound>
        <base />
        <rewrite-uri template="/v2/US/hardware/{storenumber}&{ordernumber}?City=city&State=state" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
```
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
<policies>
    <inbound>
        <base />
        <rewrite-uri template="/put" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
<!-- Resulting URL will be /put?c=d -->
```
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
<policies>
    <inbound>
        <base />
        <rewrite-uri template="/put" copy-unmatched-params="false" />
    </inbound>
    <outbound>
        <base />
    </outbound>
</policies>
<!-- Resulting URL will be /put -->
```

### <a name="elements"></a>Öğeleri

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|rewrite-uri|Kök öğesi.|Evet|

### <a name="attributes"></a>Öznitelikler

|Öznitelik|Açıklama|Gerekli|Varsayılan|
|---------------|-----------------|--------------|-------------|
|şablon|Herhangi bir sorgu dizesi parametre ile gerçek web hizmeti URL'si. İfadeler kullanırken, tam değeri bir ifade olmalıdır.|Evet|Yok|
|copy-unmatched-params|Özgün URL şablonunda mevcut değil gelen istekte sorgu parametreleri yeniden yazma şablon tarafından tanımlanan URL'ye eklenmiş olup olmadığını belirtir|Hayır|true|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümleri:** gelen

-   **İlke kapsamları:** ürün, API işlemi

##  <a name="XSLTransform"></a> XSLT kullanarak XML dönüştürme
 `Transform XML using an XSLT` İlke istek veya yanıt gövdesinde XML XSL dönüşümünü uygular.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<xsl-transform>
    <parameter name="User-Agent">@(context.Request.Headers.GetValueOrDefault("User-Agent","non-specified"))</parameter>
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
        <xsl:output method="xml" indent="yes" />
        <xsl:param name="User-Agent" />
        <xsl:template match="* | @* | node()">
            <xsl:copy>
                <xsl:if test="self::* and not(parent::*)">
                    <xsl:attribute name="User-Agent">
                        <xsl:value-of select="$User-Agent" />
                    </xsl:attribute>
                </xsl:if>
                <xsl:apply-templates select="* | @* | node()" />
            </xsl:copy>
        </xsl:template>
    </xsl:stylesheet>
  </xsl-transform>
```

### <a name="example"></a>Örnek

```xml
<policies>
  <inbound>
      <base />
  </inbound>
  <outbound>
      <base />
      <xsl-transform>
        <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
            <xsl:output omit-xml-declaration="yes" method="xml" indent="yes" />
            <!-- Copy all nodes directly-->
            <xsl:template match="node()| @*|*">
                <xsl:copy>
                    <xsl:apply-templates select="@* | node()|*" />
                </xsl:copy>
            </xsl:template>
        </xsl:stylesheet>
    </xsl-transform>
  </outbound>
</policies>
```

### <a name="elements"></a>Öğeleri

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|xsl-transform|Kök öğesi.|Evet|
|parametre|Dönüşüm dosyasında kullanılan değişkenler tanımlamak için kullanılır|Hayır|
|xsl:stylesheet|Kök stil öğesi. Tüm öğeleri ve özniteliklerinin içinde tanımlanan standarda [XSLT belirtimi](http://www.w3.org/TR/xslt)|Evet|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümleri:** gelen, giden

-   **İlke kapsamları:** genel, ürün, API işlemi

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [Grup İlkesi başvurusu](api-management-policy-reference.md) ilke deyimleri ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)
