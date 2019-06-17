---
title: Azure API Management dönüştürme ilkeleri | Microsoft Docs
description: Azure API Yönetimi'nde kullanıma dönüştürme ilkeleri hakkında bilgi edinin.
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
ms.date: 03/11/2019
ms.author: apimpm
ms.openlocfilehash: 28720098206c7afdefacbd47de283b2ef8d5a606
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66243233"
---
# <a name="api-management-transformation-policies"></a>API Management dönüştürme ilkeleri
Bu konu aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](https://go.microsoft.com/fwlink/?LinkID=398186).

##  <a name="TransformationPolicies"></a> Dönüştürme ilkeleri

-   [JSON XML'ye dönüştürür](api-management-transformation-policies.md#ConvertJSONtoXML) - dönüştürür istek veya yanıt gövdesi JSON'dan XML.

-   [XML, JSON biçimine Dönüştür](api-management-transformation-policies.md#ConvertXMLtoJSON) - dönüştürür istek veya yanıt gövdesi XML'den JSON'a.

-   [Bul ve Değiştir gövdedeki dizeyi](api-management-transformation-policies.md#Findandreplacestringinbody) - istek veya yanıtı alt dizeyi bulur ve farklı bir dizeyle değiştirir.

-   [Maske URL'leri içeriğinde](api-management-transformation-policies.md#MaskURLSContent) -yanıta bağlantı (maskeleri)'yeniden yazar, böylece eşdeğer bağlantı ağ geçidi üzerinden işaret gövde.

-   [Arka uç Hizmeti'ni](api-management-transformation-policies.md#SetBackendService) -arka uç hizmeti gelen bir istek için değiştirir.

-   [Gövde ayarlamak](api-management-transformation-policies.md#SetBody) -ileti gövdesini gelen ve giden istekler için ayarlar.

-   [Set HTTP üstbilgisi](api-management-transformation-policies.md#SetHTTPheader) - bir var olan yanıt ve/veya istek üst bilgisi için bir değer atar veya yeni bir yanıt ve/veya istek üstbilgisi ekler.

-   [Sorgu dizesi parametresini ayarlayın](api-management-transformation-policies.md#SetQueryStringParameter) - ekler, değerini değiştirir veya istek sorgu dizesi parametresi siler.

-   [URL yeniden yazma](api-management-transformation-policies.md#RewriteURL) -istek URL'si, genel formu web hizmeti tarafından beklenen biçime dönüştürür.

-   [Bir XSLT kullanarak XML dönüştürme](api-management-transformation-policies.md#XSLTransform) -XSL dönüşümü için XML istek veya yanıt gövdesi içinde geçerlidir.

##  <a name="ConvertJSONtoXML"></a> JSON XML'ye dönüştürür.
 `json-to-xml` İlkeyi JSON'dan istek veya yanıt gövdesi XML biçimine dönüştürür.

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

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|JSON xml|Kök öğe.|Evet|

### <a name="attributes"></a>Öznitelikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|Uygula|Öznitelik aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> -her zaman - her zaman dönüştürme uygulanır.<br />yanıt Content-Type üst bilgisi JSON varlığını gösteriyorsa - içerik-türü-json - Dönüştür.|Evet|Yok|
|göz önünde bulundurun kabul-üstbilgisi|Öznitelik aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> -true - JSON Accept üst bilgisi istekte istenirse dönüştürme uygulanır.<br />-yanlış - dönüştürme her zaman geçerlidir.|Hayır|true|
|Ayrıştırma tarihi|Ayarlandığında `false` tarih değerlerini dönüştürme sırasında yalnızca kopyalanır|Hayır|true|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden, hata

-   **İlke kapsamları:** genel, ürün, API, işlemi

##  <a name="ConvertXMLtoJSON"></a> XML, JSON biçimine Dönüştür
 `xml-to-json` İlkeyi XML'den bir istek veya yanıt gövdesi JSON değerine dönüştürür. Bu ilke, yalnızca XML arka uç web hizmetlerini temel alarak API'leri modernize etme kullanılabilir.

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

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|XML-json|Kök öğe.|Evet|

### <a name="attributes"></a>Öznitelikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|tür|Öznitelik aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> javascript-dostu - dönüştürülen JSON, JavaScript geliştiricileri için kolay bir forma sahiptir.<br />-doğrudan - özgün XML belgesinin yapısına dönüştürülen JSON yansıtır.|Evet|Yok|
|Uygula|Öznitelik aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> -her zaman - her zaman Dönüştür.<br />yanıt Content-Type üst bilgisi XML varlığını gösteriyorsa - içerik-türü-xml - Dönüştür.|Evet|Yok|
|göz önünde bulundurun kabul-üstbilgisi|Öznitelik aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> Accept üst bilgisi istekte XML istenirse - true - dönüştürme uygulanır.<br />-yanlış - dönüştürme her zaman geçerlidir.|Hayır|true|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden, hata

-   **İlke kapsamları:** genel, ürün, API, işlemi

##  <a name="Findandreplacestringinbody"></a> Bul ve Değiştir gövdedeki dizeyi
 `find-and-replace` İlke istek veya yanıtı alt dizeyi bulur ve farklı bir dizeyle değiştirir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<find-and-replace from="what to replace" to="replacement" />
```

### <a name="example"></a>Örnek

```xml
<find-and-replace from="notebook" to="laptop" />
```

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|Bul ve Değiştir|Kök öğe.|Evet|

### <a name="attributes"></a>Öznitelikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|from|Aranacak dize.|Evet|Yok|
|-|Yeni dize. Arama dizesini kaldırmak için bir sıfır uzunluk değiştirme dizesini belirtin.|Evet|Yok|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden arka uç, hata

-   **İlke kapsamları:** genel, ürün, API, işlemi

##  <a name="MaskURLSContent"></a> İçerik maskesi URL'leri
 `redirect-content-urls` İlke eşdeğer bağlantı ağ geçidi üzerinden üzerine böylece yanıt gövdesi (maskeleri) bağlantıları yeniden yazar. Giden bölümünde, yanıt gövdesi bağlantılar ağ geçidine noktası olacak şekilde yeniden yazmak için kullanın. Gelen bölümünde karşı efekt için kullanın.

> [!NOTE]
>  Bu ilkeyi herhangi bir üst bilgi değeri gibi değiştirmez `Location` üstbilgileri. Üstbilgi değerlerini değiştirmek için kullanın [üst bilgi ayarlama](api-management-transformation-policies.md#SetHTTPheader) ilkesi.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<redirect-content-urls />
```

### <a name="example"></a>Örnek

```xml
<redirect-content-urls />
```

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|yeniden yönlendirme içerik URL'leri|Kök öğe.|Evet|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden

-   **İlke kapsamları:** genel, ürün, API, işlemi

##  <a name="SetBackendService"></a> Arka uç hizmet belirleme
 Kullanım `set-backend-service` bu işlem için API ayarlarında belirtilenden farklı bir arka uca gelen bir isteği yönlendirmek için ilke. Bu ilke, ilkesinde belirtilen bir gelen istek arka uç hizmeti temel URL'sini değiştirir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-backend-service base-url="base URL of the backend service" />
```

or

```xml
<set-backend-service backend-id="identifier of the backend entity specifying base URL of the backend service" />
```

> [!NOTE]
> Arka uç varlık yönetimi yönetilebilir [API](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/backend) ve [PowerShell](https://www.powershellgallery.com/packages?q=apimanagement).

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
Bu örnekte, arka uç hizmet ilkesi ayarlama istekleri farklı arka uç hizmeti bir API'de belirtilen sorgu dizesinde geçirilen sürüm değere göre yönlendirir.

Başlangıçta arka uç hizmeti temel URL'si, API ayarlarından elde edilir. Bu nedenle istek URL'si `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` olur `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` burada `http://contoso.com/api/10.4/` API ayarlarında belirtilen arka uç hizmeti URL'si.

Zaman [< seçin\> ](api-management-advanced-policies.md#choose) ilke bildirimi uygulandığı arka uç hizmeti temel URL'si ya da yeniden değişebilir `http://contoso.com/api/8.2` veya `http://contoso.com/api/9.1`sürüm istek sorgu parametresi değeri bağlı olarak. Örneğin, değer ise `"2013-15"` son istek URL'si olur `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.

Daha fazla dönüştürme isteği olup olmadığını istenen, diğer [dönüştürme ilkeleri](api-management-transformation-policies.md#TransformationPolicies) kullanılabilir. Örneğin, istek sürüm belirli arka uca yönlendirilmesini artık sürümü sorgu parametresi kaldırmak için [ayarlamak sorgu dizesi parametresi](api-management-transformation-policies.md#SetQueryStringParameter) İlkesi artık yedekli version özniteliği kaldırmak için kullanılabilir.

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
Bu örnekte, ilke UserID sorgu dizesi bölüm anahtarı olarak ve birincil çoğaltmasını kullanarak bir service fabric arka ucu için istek yönlendirir.

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|set-backend-service|Kök öğe.|Evet|

### <a name="attributes"></a>Öznitelikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|temel url|Yeni arka uç hizmeti temel URL'si.|Aşağıdakilerden birini `base-url` veya `backend-id` mevcut olması gerekir.|Yok|
|arka uç kimliği|Yönlendirmek için arka uç tanımlayıcısı. (Arka uç varlıkları aracılığıyla yönetilir [API](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/backend) ve [PowerShell](https://www.powershellgallery.com/packages?q=apimanagement).)|Aşağıdakilerden birini `base-url` veya `backend-id` mevcut olması gerekir.|Yok|
|SF bölüm anahtarı|Yalnızca arka uç Service Fabric hizmeti ve 'backend-id' kullanarak belirtilen olduğunda geçerlidir. Ad çözümleme hizmeti belirli bir bölümünden çözmek için kullanılır.|Hayır|Yok|
|SF çoğaltma türü|Yalnızca arka uç Service Fabric hizmeti ve 'backend-id' kullanarak belirtilen olduğunda geçerlidir. Denetimler bir bölüm için birincil veya ikincil çoğaltma isteği tamamlamalıdır. |Hayır|Yok|
|SF çözümleme durumu|Yalnızca arka uç Service Fabric hizmeti olduğunda geçerlidir. Service Fabric arka uç çağrısı ile yeni çözüm yinelenmesi varsa tanımlama koşulu.|Hayır|Yok|
|SF-service-örnek-name|Yalnızca arka uç Service Fabric hizmeti olduğunda geçerlidir. Hizmet örnekleri zamanında değiştirmesine olanak verir. |Hayır|Yok|
|SF dinleyici adı|Yalnızca arka uç Service Fabric hizmeti ve 'backend-id' kullanarak belirtilen olduğunda geçerlidir. Service Fabric güvenilir hizmetler, bir hizmet birden çok dinleyici oluşturmanıza olanak sağlar. Bu öznitelik, bir güvenilir hizmet arka ucu birden fazla dinleyici sahip olduğunda, belirli bir dinleyici seçmek için kullanılır. Bu öznitelik belirtilmezse, API Management'ın adı olmadan bir dinleyici kullanmayı dener. Adı olmayan bir dinleyici Reliable Services için yalnızca bir dinleyicisi olması normaldir. |Hayır|Yok|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, arka uç

-   **İlke kapsamları:** genel, ürün, API, işlemi

##  <a name="SetBody"></a> Gövdeyi Ayarla
 Kullanım `set-body` gelen ve giden istekler için ileti gövdesi ayarlamak için ilke. İleti gövdesi kullanabileceğiniz erişmeye `context.Request.Body` özelliği veya `context.Response.Body`ilke gelen veya giden bölümünde olmasına bağlı olarak.

> [!IMPORTANT]
>  Varsayılan olarak, ileti eriştiğinizde kullanarak gövde unutmayın `context.Request.Body` veya `context.Response.Body`, özgün ileti gövdesi kaybolur ve gövdesine ifadede geri döndürerek ayarlamanız gerekir. Gövde içeriği korumak için ayarlanmış `preserveContent` parametresi `true` ileti erişirken. Varsa `preserveContent` ayarlanır `true` ve farklı bir gövde döndürülen ifade tarafından döndürülen gövde kullanılır.
> 
>  Lütfen kullanılırken aşağıdaki önemli noktalar unutmayın `set-body` ilkesi.
> 
> - Kullanıyorsanız `set-body` İlkesi ayarlamak için gerekmez yeni veya güncelleştirilmiş bir gövde dönüş `preserveContent` için `true` çünkü yeni gövde içeriğini açıkça sağlamış olursunuz.
>   -   Olduğundan yanıt henüz gelen işlem hattındaki bir yanıt içeriği koruma mantıklı değildir.
>   -   Giden işlem hattındaki bir isteğin içerik koruma, çünkü istek zaten arka ucuna bu noktada gönderildi doesn't make Sense.
>   -   İleti gövdesi yok ise bu ilkeyi kullandıysanız, bir gelen alın, örneğin bir özel durum oluşturulur.

 Daha fazla bilgi için `context.Request.Body`, `context.Response.Body`ve `IMessage` bölümlerine [bağlam değişkeni](api-management-policy-expressions.md#ContextVariables) tablo.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-body>new body value as text</set-body>
```

### <a name="examples"></a>Örnekler

#### <a name="literal-text-example"></a>Örnek metin

```xml
<set-body>Hello world!</set-body>
```

#### <a name="example-accessing-the-body-as-a-string-note-that-we-are-preserving-the-original-request-body-so-that-we-can-access-it-later-in-the-pipeline"></a>Örnek, dize olarak gövdesi erişme. Biz bunu daha sonra işlem hattında erişebilmesi için biz özgün istek gövdesi koruma olduğunu unutmayın.

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

#### <a name="example-accessing-the-body-as-a-jobject-note-that-since-we-are-not-reserving-the-original-request-body-accessing-it-later-in-the-pipeline-will-result-in-an-exception"></a>Örnek bir JObject olarak gövde erişme. Biz özgün istek gövdesi rezerve edersiniz değil olduğundan, daha sonra işlem hattında erişen bir özel durum sonuçlanacağını unutmayın.

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

#### <a name="filter-response-based-on-product"></a>Yanıt ürüne göre filtrele
 Bu örnekte, veri öğeleri kullanırken arka uç hizmetinden alınan yanıtı kaldırarak içerik filtreleme yapma işlemi açıklanır `Starter` ürün. Yapılandırma ve bu ilkeyi kullanan bir gösterimi için bkz. [Cloud Cover bölümü 177: Daha fazla API yönetimi özellikleri ile Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 34:30 için ileri sarma. Başlangıç 31:50 özetini görmek [koyu Gök tahmin API](https://developer.forecast.io/) bu tanıtım için kullanılır.

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

### <a name="using-liquid-templates-with-set-body"></a>Liquid şablonları kümesi gövde ile kullanma
`set-body` İlkesi kullanmak için yapılandırılabilir [Liquid](https://shopify.github.io/liquid/basics/introduction/) bir istek veya yanıt gövdesi dönüştürmek için şablon oluşturma dili. Bu, ileti biçimi tamamen yeniden şekillendirmeniz gerekiyorsa çok etkili olabilir.

> [!IMPORTANT]
> Sıvı uygulaması içinde kullanılan `set-body` İlkesi ', C# modu' yapılandırılır. Bu filtreleme gibi şeyler olduğunda özellikle önemlidir. Örnek olarak, bir tarih filtresi kullanarak Pascal kullanılmasını gerektiren büyük/küçük harf ve C# tarih biçimlendirme örn:
>
> {{body.foo.startDateTime| Date:"yyyyMMddTHH:mm:ddZ"}}

> [!IMPORTANT]
> Liquid şablonu kullanarak bir XML gövdesi doğru bir şekilde bağlamak için kullanılması bir `set-header` Content-Type ayarlamak için ilke veya uygulama/xml, metin/xml (veya tüm türü ile biten + xml); bir JSON gövdesi için uygulama/json olmalıdır metin/json (veya herhangi bir türü ile biten + JSON için).

#### <a name="convert-json-to-soap-using-a-liquid-template"></a>JSON için sıvı şablon kullanarak SOAP Dönüştür
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

#### <a name="transform-json-using-a-liquid-template"></a>Liquid şablon kullanarak JSON dönüştürmesi Uygula
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|gövdeyi Ayarla|Kök öğe. Gövde metni veya bir gövde döndüren bir ifade içeriyor.|Evet|

### <a name="properties"></a>Özellikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|şablon|İlkesi ayarlama gövdesi içinde çalışacağı şablon oluşturma modunu değiştirmek için kullanılır. Şu anda desteklenen tek değerdir:<br /><br />-liquid - gövdesi İlkesi ayarlama liquid şablon oluşturma altyapısı da kullanır |Hayır||

İstek ve yanıt hakkında bilgi erişmek için sıvı şablon aşağıdaki özelliklere sahip bir bağlam nesnesi bağlayabilirsiniz: <br />
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
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden arka uç

-   **İlke kapsamları:** genel, ürün, API, işlemi

##  <a name="SetHTTPheader"></a> Set HTTP üstbilgisi
 `set-header` İlke bir var olan yanıt ve/veya istek üst bilgisi için bir değer atar veya yeni bir yanıt ve/veya istek üstbilgisi ekler.

 HTTP üstbilgilerin listesi HTTP iletisine ekler. Bu ilke, gelen bir işlem hattında yerleştirildiğinde, hedef hizmete geçirilen istek için HTTP üstbilgilerini ayarlar. Bu ilke, bir giden işlem hattında yerleştirildiğinde, ağ geçidinin istemciye gönderilen yanıt için HTTP üstbilgilerini ayarlar.

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

#### <a name="forward-context-information-to-the-backend-service"></a>Bağlam bilgileri arka uç hizmetine iletmek
 Bu örnekte, arka uç hizmetine bağlam bilgilerini sağlamak için API düzeyinde ilkenin uygulanacağı gösterilmektedir. Yapılandırma ve bu ilkeyi kullanan bir gösterimi için bkz. [Cloud Cover bölümü 177: Daha fazla API yönetimi özellikleri ile Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 10:30 için ileri sarma. 12: 10'da bir tanıtım İlkesi iş görebileceğiniz Geliştirici Portalı'nda bir işlemi çağırma yoktur.

```xml
<!-- Copy this snippet into the inbound element to forward some context information, user id and the region the gateway is hosted in, to the backend service for logging or evaluation -->
<set-header name="x-request-context-data" exists-action="override">
  <value>@(context.User.Id)</value>
  <value>@(context.Deployment.Region)</value>
</set-header>
```

 Daha fazla bilgi için [ilke ifadeleri](api-management-policy-expressions.md) ve [bağlam değişkeni](api-management-policy-expressions.md#ContextVariables).

> [!NOTE]
> Birden çok üst bilgi değeri bir CSV dize, örneğin bitiştirilir: `headerName: value1,value2,value3`
>
> Özel durumlar, standartlaştırılmış üst bilgiler, hangi değerleri şunlardır:
> - virgül içerebilir (`User-Agent`, `WWW-Authenticate`, `Proxy-Authenticate`),
> - tarih içerebilir (`Cookie`, `Set-Cookie`, `Warning`),
> - tarih içeren (`Date`, `Expires`, `If-Modified-Since`, `If-Unmodified-Since`, `Last-Modified`, `Retry-After`).
>
> Bu özel durumlar olması durumunda birden çok üstbilgi değerlerini bir dizeye birleştirilmiş değil ve ayrı üst bilgi olarak örneğin geçirilir: `User-Agent: value1`
>`User-Agent: value2`
>`User-Agent: value3`

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|üst bilgi ayarlama|Kök öğe.|Evet|
|value|Ayarlanacak üstbilgi değerini belirtir. Aynı ada sahip birden çok üst bilgi ek eklemek için `value` öğeleri.|Evet|

### <a name="properties"></a>Özellikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|Mevcut eylem|Üstbilgi zaten belirtildiğinde gerçekleştirilecek eylemi belirtir. Bu öznitelik aşağıdaki değerlerden birine sahip olmalıdır.<br /><br /> -override - mevcut üstbilgisinin değerini değiştirir.<br />-skip - var olan üstbilgi değeri yerini almaz.<br />-ekleme - değeri var olan üstbilgi değerine ekler.<br />-delete - üstbilgi istekten kaldırır.<br /><br /> Ayarlandığında `override` göre (Bu, birden çok kez listelenir), tüm girişleri ayarlanan üst bilgisindeki sonuçları aynı ada sahip birden çok girişi kaydetme; yalnızca listelenen değerler sonuç ayarlanır.|Hayır|geçersiz kılma|
|name|Ayarlanacak üstbilginin adı belirtir.|Evet|Yok|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden arka uç, hata

-   **İlke kapsamları:** genel, ürün, API, işlemi

##  <a name="SetQueryStringParameter"></a> Sorgu dizesi parametresi kümesi
 `set-query-parameter` İlke ekler, değiştirir değeri, ya da silme isteği sorgu dizesi parametresi. Hiçbir zaman istekteki var veya sorgu parametreleri, isteğe bağlı olan arka uç hizmeti tarafından beklenen geçirmek için kullanılabilir.

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

#### <a name="forward-context-information-to-the-backend-service"></a>Bağlam bilgileri arka uç hizmetine iletmek
 Bu örnekte, arka uç hizmetine bağlam bilgilerini sağlamak için API düzeyinde ilkenin uygulanacağı gösterilmektedir. Yapılandırma ve bu ilkeyi kullanan bir gösterimi için bkz. [Cloud Cover bölümü 177: Daha fazla API yönetimi özellikleri ile Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 10:30 için ileri sarma. 12: 10'da bir tanıtım İlkesi iş görebileceğiniz Geliştirici Portalı'nda bir işlemi çağırma yoktur.

```xml
<!-- Copy this snippet into the inbound element to forward a piece of context, product name in this example, to the backend service for logging or evaluation -->
<set-query-parameter name="x-product-name" exists-action="override">
  <value>@(context.Product.Name)</value>
</set-query-parameter>

```

 Daha fazla bilgi için [ilke ifadeleri](api-management-policy-expressions.md) ve [bağlam değişkeni](api-management-policy-expressions.md#ContextVariables).

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|kümesi sorgu parametresi|Kök öğe.|Evet|
|value|Ayarlanacak sorgu parametresi'ünün değerini belirtir. Aynı ada sahip birden çok sorgu parametreleri ek eklemek için `value` öğeleri.|Evet|

### <a name="properties"></a>Özellikler

|Ad|Açıklama|Gerekli|Varsayılan|
|----------|-----------------|--------------|-------------|
|Mevcut eylem|Sorgu parametresi zaten belirtilmiş gerçekleştirilecek eylemi belirtir. Bu öznitelik aşağıdaki değerlerden birine sahip olmalıdır.<br /><br /> -override - mevcut parametresinin değerini değiştirir.<br />-skip - var olan sorgu parametresi değerini değiştirmez.<br />-ekleme - var olan sorgu parametresi değeri değeri ekler.<br />-delete - sorgu parametresi istekte kaldırır.<br /><br /> Ayarlandığında `override` göre (Bu, birden çok kez listelenir), tüm girişleri ayarlanan sorgu parametresi sonuçlarının aynı ada sahip birden çok girişi kaydetme; yalnızca listelenen değerler sonuç ayarlanır.|Hayır|geçersiz kılma|
|name|Ayarlanacak sorgu parametresi adını belirtir.|Evet|Yok|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, arka uç

-   **İlke kapsamları:** genel, ürün, API, işlemi

##  <a name="RewriteURL"></a> URL yeniden yazma
 `rewrite-uri` İlkeyi dönüştürür istek URL'si, genel formu web hizmeti tarafından beklenen biçime aşağıdaki örnekte gösterildiği gibi.

- Genel URL- `http://api.example.com/storenumber/ordernumber`

- İstek URL'si- `http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`

  Bu ilke, bir insan ve/veya tarayıcı kullanımı kolay URL web hizmeti tarafından beklenen URL biçimine dönüştürülmesi gereken olduğunda kullanılabilir. Bu ilke yalnızca temiz URL'leri, RESTful URL'leri, kullanımı kolay URL'ler veya bir sorgu dizesi içermemelidir ve bunun yerine yalnızca kaynak (yolunu içeren zamanıyla ilgili yapısal URL'ler SEO dostu URL'leri gibi alternatif bir URL biçimi gösterme zaman uygulanması gerekiyor Şema ve yetkilisi sonra). Bu genellikle estetik, kullanılabilirlik veya arama motoru iyileştirmesi (SEO) amacıyla yapılır.

> [!NOTE]
>  İlkeyi kullanan bir sorgu dizesi parametreleri yalnızca ekleyebilirsiniz. Ek şablon yol parametreleri yeniden yazma URL ekleyemezsiniz.

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

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|URI yeniden yazma|Kök öğe.|Evet|

### <a name="attributes"></a>Öznitelikler

|Öznitelik|Açıklama|Gerekli|Varsayılan|
|---------------|-----------------|--------------|-------------|
|şablon|Herhangi bir sorgu dizesi parametreleri ile gerçek web hizmeti URL'si. İfadeleri kullanırken, tam değeri bir ifade olmalıdır.|Evet|Yok|
|kopyalama eşleşmeyen-params|Özgün URL şablonunda yok gelen istekteki sorgu parametreleri için URL yeniden yazma şablon tarafından tanımlanan eklenip eklenmeyeceğini belirtir|Hayır|true|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen

-   **İlke kapsamları:** genel, ürün, API, işlemi

##  <a name="XSLTransform"></a> Bir XSLT kullanarak XML dönüştürme
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

### <a name="elements"></a>Öğeler

|Ad|Açıklama|Gerekli|
|----------|-----------------|--------------|
|XSL Dönüştürme|Kök öğe.|Evet|
|Parametre|Dönüşüm kullanılan değişkenleri tanımlamak için kullanılır|Hayır|
|xsl:stylesheet|Kök stil sayfası öğesi. Tüm öğeleri ve öznitelikleri içinde tanımlanan standarda [XSLT belirtimi](https://www.w3.org/TR/xslt)|Evet|

### <a name="usage"></a>Kullanım
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden

-   **İlke kapsamları:** genel, ürün, API, işlemi

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki konulara bakın:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [İlke başvurusu](api-management-policy-reference.md) ilke bildirimlerine ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)
