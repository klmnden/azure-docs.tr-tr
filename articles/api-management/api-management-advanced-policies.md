---
title: "Azure API Management ilkeleri Gelişmiş | Microsoft Docs"
description: "Azure API Management'te kullanıma Gelişmiş ilkeleri hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 8a13348b-7856-428f-8e35-9e4273d94323
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: e5a658e0d20d42911870f2522f6c1bab7529ea11
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="api-management-advanced-policies"></a>API Management ilkeleri Gelişmiş
Bu konu aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AdvancedPolicies"></a>Gelişmiş ilkeleri  
  
-   [Denetim akışı](api-management-advanced-policies.md#choose) - koşullu ilke deyimleri Boole değerlendirmesi sonuçlarına dayalı uygular [ifadeleri](api-management-policy-expressions.md).  
  
-   [İstek](#ForwardRequest) -arka uç hizmetine isteği iletir.

-   [Eşzamanlılık sınırlamak](#LimitConcurrency) -engeller içine birden çok belirtilen istek sayısının tarafından aynı anda yürütülmesini ilkeleri.
  
-   [Olay Hub'ına günlük](#log-to-eventhub) -gönderdiği iletileri belirtilen biçimde Günlükçü varlık tarafından tanımlanan bir olay Hub'ına. 

-   [Yanıt mock](#mock-response) -iptalleri potansiyel satış, yürütme ve doğrudan çağırana mocked bir yanıt döndürür.
  
-   [Yeniden deneme](#Retry) -değilse ve koşul yerine getirilene kadar iliştirilmiş ilke deyimleri yürütülmesi yeniden dener. Yürütme belirtilen zaman aralıklarında yineleyin ve belirtilen en fazla yeniden deneme sayısı.  
  
-   [Yanıt](#ReturnResponse) -iptalleri potansiyel satış, yürütme ve doğrudan çağırana belirtilen yanıt verir. 
  
-   [Tek yönlü İsteği Gönder](#SendOneWayRequest) -bir istek için yanıt beklemeden belirtilen URL'ye gönderir.  
  
-   [İsteği Gönder](#SendRequest) -belirtilen URL'ye bir isteği gönderir.  

-   [HTTP proxy ayarlamak](#SetHttpProxy) -bir HTTP proxy üzerinden iletilen rota isteklere izin verir.  

-   [İstek yöntemini ayarla](#SetRequestMethod) -bir istek için HTTP yöntemini değiştirmenize izin verir.  
  
-   [Durum kodu ayarlamak](#SetStatus) -HTTP durum kodu için belirtilen değer değiştirir.  
  
-   [Değişken Ayarla](api-management-advanced-policies.md#set-variable) -adlandırılmış bir değer devam [bağlamı](api-management-policy-expressions.md#ContextVariables) sonraki erişim için değişken.  

-   [İzleme](#Trace) -bir dizeye ekler [API denetçisi](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) çıktı.  
  
-   [Bekleyin](#Wait) -içine için bekleyeceği [gönderme isteği](api-management-advanced-policies.md#SendRequest), [değeri önbellekten alma](api-management-caching-policies.md#GetFromCacheByKey), veya [kontrol akışı](api-management-advanced-policies.md#choose) devam etmeden önce tamamlanmasını ilkeleri.  
  
##  <a name="choose"></a>Denetim akışı  
 `choose` İlke deyimleri Boole ifadeleri, bir if-then-else veya bir anahtar benzer değerlendirme sonucunu dayalı bir programlama dilinde oluşturmak iliştirilmiş ilke uygulanır.  
  
###  <a name="ChoosePolicyStatement"></a>İlke bildirimi  
  
```xml  
<choose>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements to be applied if the above condition is true  -->  
    </when>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements to be applied if the above condition is true  -->  
    </when>   
    <otherwise>   
        <!— one or more policy statements to be applied if none of the above conditions are true  -->  
</otherwise>   
</choose>  
```  
  
 Denetim akışı ilke en az birini içermelidir `<when/>` öğesi. `<otherwise/>` Öğesidir isteğe bağlıdır. Koşulları `<when/>` öğeleri sırasına görünümlerini ilke içinde değerlendirilir. İlke deyim içinde ilk içine `<when/>` koşul özniteliği eşittir öğeyle `true` uygulanır. İlkeleri içine içinde `<otherwise/>` öğesi, varsa, tüm uygulanır, `<when/>` öğesi koşul öznitelikleri `false`.  
  
### <a name="examples"></a>Örnekler  
  
####  <a name="ChooseExample"></a>Örnek  
 Aşağıdaki örnekte gösterilmiştir bir [değişken Ayarla](api-management-advanced-policies.md#set-variable) İlkesi ve iki denetim akışı ilkeleri.  
  
 Değişken ilkesini ayarlama gelen bölümünde ve oluşturur bir `isMobile` Boolean [bağlamı](api-management-policy-expressions.md#ContextVariables) true ise ayarlamak değişkeni `User-Agent` üstbilgi metnini içeren istek `iPad` veya `iPhone`.  
  
 İlk denetim akışı ilke ayrıca gelen bölümde yer alan ve koşullu iki biri geçerlidir [ayarlamak sorgu dizesi parametresi](api-management-transformation-policies.md#SetQueryStringParameter) değerine bağlı olarak ilkeleri `isMobile` bağlamının.  
  
 İkinci denetim akışı ilke giden bölümünde ve koşullu geçerlidir [XML ve JSON Dönüştür](api-management-transformation-policies.md#ConvertXMLtoJSON) İlkesi zaman `isMobile` ayarlanır `true`.  
  
```xml  
<policies>  
    <inbound>  
        <set-variable name="isMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
        <base />  
        <choose>  
            <when condition="@(context.Variables.GetValueOrDefault<bool>("isMobile"))">  
                <set-query-parameter name="mobile" exists-action="override">  
                    <value>true</value>  
                </set-query-parameter>  
            </when>  
            <otherwise>  
                <set-query-parameter name="mobile" exists-action="override">  
                    <value>false</value>  
                </set-query-parameter>  
            </otherwise>  
        </choose>  
    </inbound>  
    <outbound>  
        <base />  
        <choose>  
            <when condition="@(context.GetValueOrDefault<bool>("isMobile"))">  
                <xml-to-json kind="direct" apply="always" consider-accept-header="false"/>  
            </when>  
        </choose>  
    </outbound>  
</policies>  
```  
  
#### <a name="example"></a>Örnek  
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
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|seçin|Kök öğesi.|Evet|  
|ne zaman|Koşul için kullanılacak `if` veya `ifelse` bölümlerini `choose` ilkesi. Varsa `choose` ilkesine sahip birden çok `when` bölümler, sıralı olarak değerlendirilir. Bir kez `condition` öğe ne zaman değerlendiren `true`, daha fazla `when` koşullar değerlendirilir.|Evet|  
|Aksi takdirde|Hiçbiri kullanılacak ilke parçacığı içeren `when` koşulları değerlendirmek için `true`.|Hayır|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|  
|---------------|-----------------|--------------|  
|koşul = "Boole ifadesi &#124; Boole sabiti"|Boole ifadesi veya bir sabite hesaplanan ne zaman içeren `when` ilke bildirimi değerlendirildiği.|Evet|  
  
###  <a name="ChooseUsage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen, giden arka uç, hata  
  
-   **İlke kapsamları:** tüm kapsamlar  
  
##  <a name="ForwardRequest"></a>İstek  
 `forward-request` İlkesi iletir gelen istek istekte belirtilen arka uç hizmetine [bağlamı](api-management-policy-expressions.md#ContextVariables). Arka uç hizmeti URL'si API çağrısında belirtilen [ayarları](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) ve kullanılarak değiştirilebilir [ayarlamak arka uç hizmetini](api-management-transformation-policies.md) ilkesi.  
  
> [!NOTE]
>  Bu İlkesi Sonuçları arka ucuna iletilen değil isteği kaldırma hizmeti ve giden bölümünde ilkeleri hemen gelen bölüm ilkelerinde başarılı tamamlanmasından sonra değerlendirilir.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<forward-request timeout="time in seconds" follow-redirects="true | false"/>  
```  
  
### <a name="examples"></a>Örnekler  
  
#### <a name="example"></a>Örnek  
 Aşağıdaki API düzeyi ilkesi 60 saniye cinsinden bir zaman aşımı aralığı ile arka uç hizmetine tüm istekleri iletir.  
  
```xml  
<!-- api level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="60"/>  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Örnek  
 Bu işlem düzeyi ilkeyi kullanan `base` arka uç İlkesi üst API düzeyi kapsamını devral öğesi.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <base/>  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Örnek  
 Bu işlem düzeyi ilkesi açıkça bir zaman aşımı süresi 120 ile arka uç hizmetine tüm istekleri iletir ve API düzeyi arka uç İlkesi üst devralmaz.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="120"/>   
        <!-- effective policy. note the absence of <base/> -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Örnek  
 Bu işlem düzeyi ilkesi, arka uç hizmetine isteklerini iletmek değil.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <!-- no forwarding to backend -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|İletme isteği|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|Varsayılan|  
|---------------|-----------------|--------------|-------------|  
|zaman aşımı = "tamsayı"|Arka uç hizmetine çağırmadan önce saniye cinsinden zaman aşımı aralığı başarısız olur.|Hayır|Hiçbir zaman aşımı|  
|yeniden yönlendirmeleri izleyin = "true &#124; false"|Arka uç hizmetinden yeniden yönlendirmeleri gateway tarafından izlenen veya yapana olup olmadığını belirtir.|Hayır|False|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** arka uç  
  
-   **İlke kapsamları:** tüm kapsamlar  
  
##  <a name="LimitConcurrency"></a>Sınır eşzamanlılık  
 `limit-concurrency` İlke birden çok belirtilen istek sayısının tarafından belirli bir zamanda yürütülmesini kapalı ilkeleri engeller. Eşiği aşan bağlı maksimum kuyruk uzunluğu elde kadar yeni istekler bir kuyruğa eklenir. Sıra Tükenme yeni istekleri hemen başarısız olur.
  
###  <a name="LimitConcurrencyStatement"></a>İlke bildirimi  
  
```xml  
<limit-concurrency key="expression" max-count="number" timeout="in seconds" max-queue-length="number">
        <!— nested policy statements -->  
</limit-concurrency>
``` 

### <a name="examples"></a>Örnekler  
  
####  <a name="ChooseExample"></a>Örnek  
 Aşağıdaki örnek, bir bağlam değişkeni değere göre bir arka uç iletilir isteklerinin sayısını sınırlamak gösterilmiştir.
 
```xml  
<policies>
  <inbound>…</inbound>
  <backend>
    <limit-concurrency key="@((string)context.Variables["connectionId"])" max-count="3" timeout="60">
      <forward-request timeout="120"/>
    <limit-concurrency/>
  </backend>
  <outbound>…</outbound>
</policies>
```

### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|    
|sınır eşzamanlılık|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|Varsayılan|  
|---------------|-----------------|--------------|--------------|  
|anahtar|Bir dize. İzin verilen ifade. Eşzamanlılık kapsamı belirler. Birden çok ilke tarafından paylaşılabilir.|Evet|Yok|  
|en yüksek sayısı|Bir tam sayı. En fazla bir ilke girmesine izin verilen istek sayısını belirtir.|Evet|Yok|  
|timeout|Bir tam sayı. İzin verilen ifade. Bir isteği ile başarısız olmadan önce bir kapsam girmek için beklemesi gereken saniye sayısını belirtir "429 çok fazla istek"|Hayır|Infinity|  
|en büyük sıra uzunluğu|Bir tam sayı. İzin verilen ifade. Maksimum kuyruk uzunluğu belirtir. Bu ilke girmek gelen istekleri ile sonlandırılacak "429 çok fazla istek" hemen zaman sıranın tükendi.|Hayır|Infinity|  
  
###  <a name="ChooseUsage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen, giden arka uç, hata  
  
-   **İlke kapsamları:** tüm kapsamlar  

##  <a name="log-to-eventhub"></a>Olay Hub'ına günlük  
 `log-to-eventhub` İlkesi Günlükçü varlık tarafından tanımlanan bir olay Hub'ına belirtilen biçimde iletileri gönderir. Adından da anlaşılacağı gibi ilke, çevrimiçi veya çevrimdışı analiz için seçilen istek veya yanıt bağlam bilgilerini kaydetmek için kullanılır.  
  
> [!NOTE]
>  Bir olay hub'ı ve olayları günlüğe kaydetmeyi yapılandırma hakkında adım adım yönergeler için bkz: [Azure Event Hubs ile API Management olaylarını günlüğe kaydedecek şekilde nasıl](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<log-to-eventhub logger-id="id of the logger entity" partition-id="index of the partition where messages are sent" partition-key="value used for partition assignment">  
  Expression returning a string to be logged  
</log-to-eventhub>  
  
```  
  
### <a name="example"></a>Örnek  
 Herhangi bir dize değeri olarak olay hub ' oturum açmış olmanız için kullanılabilir. Bu örnekte, tarih ve saat, dağıtım hizmet adı, istek kimliği, IP adresi ve işlem adı tüm gelen çağrıları için event hub'ına oturum Günlükçü kayıtlı `contoso-logger` kimliği.  
  
```xml  
<policies>  
  <inbound>  
    <log-to-eventhub logger-id ='contoso-logger'>  
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name) )   
    </log-to-eventhub>  
  </inbound>  
  <outbound>          
  </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|Günlük eventhub|Kök öğesi. Bu öğenin değerini event hub'ına oturum dizedir.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|  
|---------------|-----------------|--------------|  
|Günlükçü kimliği|Günlükçü kimliğini API Management hizmetiniz ile kayıtlı.|Evet|  
|bölüm kimliği|İletilerin gönderileceği bölüm dizinini belirtir.|İsteğe bağlı. Bu öznitelik, kullanılamaz. `partition-key` kullanılır.|  
|Bölüm anahtarı|İleti gönderildiğinde bölüm ataması için kullanılan değer belirtir.|İsteğe bağlı. Bu öznitelik, kullanılamaz. `partition-id` kullanılır.|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen, giden arka uç, hata  
  
-   **İlke kapsamları:** tüm kapsamlar  

##  <a name="mock-response"></a>Sahte yanıt  
`mock-response`, Adından da anlaşılacağı, API'ler ve işlemler mock için kullanılır. Normal ardışık düzen yürütmesi durdurur ve mocked yanıtını çağırana döndürür. İlke, her zaman en yüksek doğruluk yanıtları döndürmeye çalışır. Bu yanıt içerik örnekler, kullanılabilir olduğunda tercih eder. Bu örnek yanıtları şemalarına dönüştürmeyi, şemaları sağlanır ve örnekler olmadıkları zaman oluşturur. Örnek ya da şemaları bulunursa, içerik ile yanıtları döndürülür.
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<mock-response status-code="code" content-type="media type"/>  
  
```  
  
### <a name="examples"></a>Örnekler  
  
```xml  
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code. First found content type is used. If no example or schema is found, the content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code and media type. If no example or schema found, the content is empty. -->
<mock-response status-code='200' content-type='application/json'/>  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|mock yanıt|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|Varsayılan|  
|---------------|-----------------|--------------|--------------|  
|Durum kodu|Yanıt durum kodu belirtir ve karşılık gelen örnek veya şema seçmek için kullanılır.|Hayır|200|  
|içerik türü|Belirtir `Content-Type` yanıt üstbilgi değeri ve karşılık gelen örnek veya şema seçmek için kullanılır.|Hayır|None|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen, giden, hata  
  
-   **İlke kapsamları:** tüm kapsamlar

##  <a name="Retry"></a>Yeniden deneyin  
 `retry` İlkesi kendi alt ilkeleri bir kez çalıştırır ve bunların yürütme yeniden deneme kadar yeniden deneme `condition` olur `false` ya da yeniden deneme `count` tükendi olduğu.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
  
<retry  
    condition="boolean expression or literal"  
    count="number of retry attempts"  
    interval="retry interval in seconds"  
    max-interval="maximum retry interval in seconds"  
    delta="retry interval delta in seconds"  
    first-fast-retry="boolean expression or literal">  
        <!-- One or more child policies. No restrictions -->  
</retry>  
  
```  
  
### <a name="example"></a>Örnek  
 Aşağıdaki örnek istekte forewarding kadar denenir on kez üstel kullanarak yeniden algoritması. Bu yana `first-fast-retry` yanlış olarak tüm denemeleri olan tabi exponsntial yeniden deneme algoritması ayarlanmadı.  
  
```xml  
  
<retry  
    condition="@(context.Response.StatusCode == 500)"  
    count="10"  
    interval="10"  
    max-interval="100"  
    delta="10"  
    first-fast-retry="false">  
        <forward-request />  
</retry>  
  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|retry|Kök öğesi. Diğer ilkeleri ve alt öğeleri içerebilir.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|Varsayılan|  
|---------------|-----------------|--------------|-------------|  
|Koşul|Boole bir hazır değer veya [ifade](api-management-policy-expressions.md) yeniden deneme durmuş olup belirtme (`false`) veya devam (`true`).|Evet|Yok|  
|Sayısı|Denemek için yeniden deneme sayısını belirten pozitif bir sayı.|Evet|Yok|  
|interval|Yeniden deneme bekleme aralığını belirterek saniye cinsinden pozitif bir sayı çalışır.|Evet|Yok|  
|en fazla aralığı|Yeniden deneme girişimleri arasındaki aralığı saniye cinsinden maksimum belirten pozitif bir sayı bekleyin. Üstel yeniden deneme algoritması uygulamak için kullanılır.|Hayır|Yok|  
|Delta|Saniye olarak bekleme aralığı artırma belirten pozitif bir sayı. Doğrusal ve üstel yeniden deneme algoritmaları uygulamak için kullanılır.|Hayır|Yok|  
|ilk hızlı yeniden|Varsa kümesine `true` , ilk yeniden deneme girişimi hemen gerçekleştirilir.|Hayır|`false`|  
  
> [!NOTE]
>  Sadece `interval` belirtilirse, **sabit** aralığı yeniden denemeler gerçekleştirilir.  
>  Sadece `interval` ve `delta` belirtilir, bir **doğrusal** aralığı yeniden deneme algoritması kullanılır, burada denemeler arasındaki bekleme süresini hesaplanan aşağıdaki formülü - according `interval + (count - 1)*delta`.  
>  Zaman `interval`, `max-interval` ve `delta` belirtilir, **üstel** aralığı yeniden deneme algoritması uygulandığında, burada denemeler arasındaki bekleme süresini değerinden katlanarak artıyor `interval` değerine `max-interval` aşağıdaki forumula göre- `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) . Alt İlkesi kullanım kısıtlamaları Bu ilke tarafından devralınır unutmayın.  
  
-   **İlke bölümleri:** gelen, giden arka uç, hata  
  
-   **İlke kapsamları:** tüm kapsamlar  
  
##  <a name="ReturnResponse"></a>Yanıt döndürür  
 `return-response` İlkesi ardışık düzen yürütmesi durdurur ve bir varsayılan veya özel yanıtını çağırana döndürür. Varsayılan yanıt `200 OK` hiçbir gövde ile. Özel yanıtı bağlam değişkeni veya ilke deyimleri ile belirtilebilir. Her ikisi de sağlandığında bağlamının içinde yer alan yanıt çağırana döndürülen önce ilke deyimleri tarafından değiştirilir.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<return-response response-variable-name="existing context variable">  
  <set-header/>  
  <set-body/>  
  <set-status/>  
</return-response>  
  
```  
  
### <a name="example"></a>Örnek  
  
```xml  
<return-response>  
   <set-status code="401" reason="Unauthorized"/>  
   <set-header name="WWW-Authenticate" exists-action="override">  
      <value>Bearer error="invalid_token"</value>  
   </set-header>  
</return-response>  
  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|return yanıt|Kök öğesi.|Evet|  
|set üstbilgisi|A [set üstbilgisi](api-management-transformation-policies.md#SetHTTPheader) ilke bildirimi.|Hayır|  
|set-gövdesi|A [kümesi gövde](api-management-transformation-policies.md#SetBody) ilke bildirimi.|Hayır|  
|durumunu ayarla|A [durumunu ayarla](api-management-advanced-policies.md#SetStatus) ilke bildirimi.|Hayır|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|  
|---------------|-----------------|--------------|  
|yanıt değişken adı|Bağlam değişkenin adını, örneğin, Yukarı Akış başvurulan [gönderme isteği](api-management-advanced-policies.md#SendRequest) ilke ve içeren bir `Response` nesnesi|İsteğe bağlı.|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen, giden arka uç, hata  
  
-   **İlke kapsamları:** tüm kapsamlar  
  
##  <a name="SendOneWayRequest"></a>Tek yönlü İsteği Gönder  
 `send-one-way-request` İlke gönderir sağlanan isteği için belirtilen URL için bir yanıt beklemeden.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<send-one-way-request mode="new | copy">  
  <url>...</url>  
  <method>...</method>  
  <header name="" exists-action="override | skip | append | delete">...</header>  
  <body>...</body>  
</send-one-way-request>  
  
```  
  
### <a name="example"></a>Örnek  
 Bu örnek ilke kullanan bir örnek gösterilir `send-one-way-request` HTTP yanıt kodunu 500'e eşit veya daha büyük ise Slack sohbet odasına bir ileti göndermek için ilke. Bu örnek hakkında daha fazla bilgi için bkz: [Azure API Management hizmetinden dış hizmetler kullanarak](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<choose>  
    <when condition="@(context.Response.StatusCode >= 500)">  
      <send-one-way-request mode="new">  
        <set-url>https://hooks.slack.com/services/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</set-url>  
        <set-method>POST</set-method>  
        <set-body>@{  
                return new JObject(  
                        new JProperty("username","APIM Alert"),  
                        new JProperty("icon_emoji", ":ghost:"),  
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",  
                                                context.Request.Method,  
                                                context.Request.Url.Path + context.Request.Url.QueryString,  
                                                context.Request.Url.Host,  
                                                context.Response.StatusCode,  
                                                context.Response.StatusReason,  
                                                context.User.Email  
                                                ))  
                        ).ToString();  
            }</set-body>  
      </send-one-way-request>  
    </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|bir şekilde İsteği Gönder|Kök öğesi.|Evet|  
|URL|İstek URL'si.|Hiçbir IF modu kopyalama; = Aksi takdirde Evet.|  
|Yöntemi|İstek HTTP yöntemi.|Hiçbir IF modu kopyalama; = Aksi takdirde Evet.|  
|üst bilgi|İstek üstbilgisi. Birden çok üstbilgi öğeleri için birden çok istek üstbilgileri kullanın.|Hayır|  
|Gövde|İstek gövdesi.|Hayır|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|Varsayılan|  
|---------------|-----------------|--------------|-------------|  
|modu = "dize"|Yeni bir istek veya bir kopyasını geçerli istek olup olmadığını belirler. Giden modunda modu = kopyalama istek gövdesi başlatamadı.|Hayır|Yeni|  
|ad|Ayarlanacak üstbilginin adını belirtir.|Evet|Yok|  
|Mevcut eylem|Üstbilgi zaten belirtildiğinde gerçekleştirilecek eylemi belirtir. Bu öznitelik aşağıdaki değerlerden birine sahip olmalıdır.<br /><br /> -Geçersiz Kıl - varolan üstbilgisinin değerini değiştirir.<br />-skip - varolan üstbilgi değeri değiştirmez.<br />-Ekle - Varolan üstbilgi değeri değer ekler.<br />-delete - istekten üstbilgiyi kaldırır.<br /><br /> Ayarlandığında `override` aynı ada sahip birden çok girdi kaydetme sonuçları (birden çok kez listelenir) tüm girdisi göre ayarlanan üstbilgisinde; yalnızca listelenen değerler sonuç ayarlanır.|Hayır|geçersiz kılma|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen, giden arka uç, hata  
  
-   **İlke kapsamları:** tüm kapsamlar  
  
##  <a name="SendRequest"></a>İsteği Gönder  
 `send-request` İlke gönderir sağlanan istek belirtilen URL'ye zaman aşımı değerini ayarlayın bundan böyle bekleniyor.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error  
="false|true">  
  <set-url>...</set-url>  
  <set-method>...</set-method>  
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>  
  <set-body>...</set-body>  
</send-request>  
  
```  
  
### <a name="example"></a>Örnek  
 Bu örnek bir başvuru doğrulamanın bir yolu bir yetkilendirme sunucusuyla belirteci gösterir. Bu örnek hakkında daha fazla bilgi için bkz: [Azure API Management hizmetinden dış hizmetler kullanarak](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<inbound>  
  <!-- Extract Token from Authorization header parameter -->  
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />  
  
  <!-- Send request to Token Server to validate token (see RFC 7662) -->  
  <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">  
    <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>  
    <set-method>POST</set-method>  
    <set-header name="Authorization" exists-action="override">  
      <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>  
    </set-header>  
    <set-header name="Content-Type" exists-action="override">  
      <value>application/x-www-form-urlencoded</value>  
    </set-header>  
    <set-body>@($"token={(string)context.Variables["token"]}")</set-body>  
  </send-request>  
  
  <choose>  
        <!-- Check active property in response -->  
        <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">  
            <!-- Return 401 Unauthorized with http-problem payload -->  
            <return-response response-variable-name="existing response variable">  
                <set-status code="401" reason="Unauthorized" />  
                <set-header name="WWW-Authenticate" exists-action="override">  
                    <value>Bearer error="invalid_token"</value>  
                </set-header>  
            </return-response>  
        </when>  
    </choose>  
  <base />  
</inbound>  
  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|Gönderme isteği|Kök öğesi.|Evet|  
|URL|İstek URL'si.|Hiçbir IF modu kopyalama; = Aksi takdirde Evet.|  
|Yöntemi|İstek HTTP yöntemi.|Hiçbir IF modu kopyalama; = Aksi takdirde Evet.|  
|üst bilgi|İstek üstbilgisi. Birden çok üstbilgi öğeleri için birden çok istek üstbilgileri kullanın.|Hayır|  
|Gövde|İstek gövdesi.|Hayır|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|Varsayılan|  
|---------------|-----------------|--------------|-------------|  
|modu = "dize"|Yeni bir istek veya bir kopyasını geçerli istek olup olmadığını belirler. Giden modunda modu = kopyalama istek gövdesi başlatamadı.|Hayır|Yeni|  
|yanıt değişkeni adı = "dize"|Yoksa, `context.Response` kullanılır.|Hayır|Yok|  
|zaman aşımı = "tamsayı"|URL çağırmadan önce saniye cinsinden zaman aşımı aralığı başarısız olur.|Hayır|60|  
|Hatayı Yoksay|Varsa true ve istek sonuçları hata:<br /><br /> -Eğer bir null değer içerecek yanıt değişkeni adı belirtildi.<br />-Yanıt değişkeni adı belirtilmedi Eğer bağlamı. İstek güncelleştirilmez.|Hayır|False|  
|ad|Ayarlanacak üstbilginin adını belirtir.|Evet|Yok|  
|Mevcut eylem|Üstbilgi zaten belirtildiğinde gerçekleştirilecek eylemi belirtir. Bu öznitelik aşağıdaki değerlerden birine sahip olmalıdır.<br /><br /> -Geçersiz Kıl - varolan üstbilgisinin değerini değiştirir.<br />-skip - varolan üstbilgi değeri değiştirmez.<br />-Ekle - Varolan üstbilgi değeri değer ekler.<br />-delete - istekten üstbilgiyi kaldırır.<br /><br /> Ayarlandığında `override` aynı ada sahip birden çok girdi kaydetme sonuçları (birden çok kez listelenir) tüm girdisi göre ayarlanan üstbilgisinde; yalnızca listelenen değerler sonuç ayarlanır.|Hayır|geçersiz kılma|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen, giden arka uç, hata  
  
-   **İlke kapsamları:** tüm kapsamlar  
  
##  <a name="SetHttpProxy"></a>Set HTTP proxy  
 `proxy` İlkesi izin veriyor rota istekleri arka uçlarını bir HTTP proxy üzerinden iletilir. Yalnızca HTTP (HTTPS değil), proxy ve ağ geçidi arasında desteklenir. Temel ve yalnızca NTLM kimlik doğrulaması.
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<proxy url="http://hostname-or-ip:port" username="username" password="password" />  
  
```  
  
### <a name="example"></a>Örnek  
Kullanımına dikkat edin [özellikleri](api-management-howto-properties.md) kullanıcı adı ve parola ilkesi belgede hassas bilgileri depolamak önlemek için değerler olarak.  
  
```xml  
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />
  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|Proxy|Kök öğesi|Evet|  

### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|Varsayılan|  
|---------------|-----------------|--------------|-------------|  
|URL = "dize"|Proxy URL'si http://host:port biçiminde.|Evet|Yok|  
|Kullanıcı adı = "dize"|Proxy kimlik doğrulaması için kullanılacak kullanıcı adı.|Hayır|Yok|  
|parola = "dize"|Proxy kimlik doğrulaması için kullanılan parola.|Hayır|Yok|  

### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen  
  
-   **İlke kapsamları:** tüm kapsamlar  

##  <a name="SetRequestMethod"></a>Set isteği yöntemi  
 `set-method` İlke bir istek için HTTP istek yöntemini değiştirmenize izin verir.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<set-method>METHOD</set-method>  
  
```  
  
### <a name="example"></a>Örnek  
 Kullanan bu örnek ilke `set-method` ilke HTTP yanıt kodunu 500'e eşit veya daha büyük ise Slack sohbet odasına ileti gönderme örneği gösterir. Bu örnek hakkında daha fazla bilgi için bkz: [Azure API Management hizmetinden dış hizmetler kullanarak](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<choose>  
    <when condition="@(context.Response.StatusCode >= 500)">  
      <send-one-way-request mode="new">  
        <set-url>https://hooks.slack.com/services/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</set-url>  
        <set-method>POST</set-method>  
        <set-body>@{  
                return new JObject(  
                        new JProperty("username","APIM Alert"),  
                        new JProperty("icon_emoji", ":ghost:"),  
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",  
                                                context.Request.Method,  
                                                context.Request.Url.Path + context.Request.Url.QueryString,  
                                                context.Request.Url.Host,  
                                                context.Response.StatusCode,  
                                                context.Response.StatusReason,  
                                                context.User.Email  
                                                ))  
                        ).ToString();  
            }</set-body>  
      </send-one-way-request>  
    </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|Set yöntemi|Kök öğesi. Öğenin değerini HTTP yöntemini belirtir.|Evet|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen, hata  
  
-   **İlke kapsamları:** tüm kapsamlar  
  
##  <a name="SetStatus"></a>Set durum kodu  
 `set-status` İlke HTTP durum kodunu belirtilen değere ayarlar.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<set-status code="" reason=""/>  
  
```  
  
### <a name="example"></a>Örnek  
 Bu örnek, bir 401 yanıtına yetkilendirme belirteci geçersiz ise döndürülecek gösterilmiştir. Daha fazla bilgi için bkz: [Azure API Management hizmetinden dış hizmetler kullanarak](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)  
  
```xml  
<choose>  
  <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">  
    <return-response response-variable-name="existing response variable">  
      <set-status code="401" reason="Unauthorized" />  
      <set-header name="WWW-Authenticate" exists-action="override">  
        <value>Bearer error="invalid_token"</value>  
      </set-header>  
    </return-response>  
  </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|durumunu ayarla|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|Varsayılan|  
|---------------|-----------------|--------------|-------------|  
|kod = "tamsayı"|Döndürülecek HTTP durum kodu.|Evet|Yok|  
|nedeni = "dize"|Durum kodunu döndüren nedeni açıklaması.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** giden, arka uç, hata  
  
-   **İlke kapsamları:** tüm kapsamlar  

##  <a name="set-variable"></a>Değişken Ayarla  
 `set-variable` İlkesi bildirir bir [bağlamı](api-management-policy-expressions.md#ContextVariables) değişkeni aracılığıyla belirtilen bir değeri atar bir [ifade](api-management-policy-expressions.md) veya bir değişmez dize değeri. ifade bir hazır değer içeriyorsa, bir dizeye dönüştürülür ve değer türünde `System.String`.  
  
###  <a name="set-variablePolicyStatement"></a>İlke bildirimi  
  
```xml  
<set-variable name="variable name" value="Expression | String literal" />  
```  
  
###  <a name="set-variableExample"></a>Örnek  
 Aşağıdaki örnek, bir küme değişken ilkesini gelen bölümünde gösterir. Bu değişken ilkesini ayarlama oluşturur bir `isMobile` Boolean [bağlamı](api-management-policy-expressions.md#ContextVariables) true ise ayarlamak değişkeni `User-Agent` üstbilgi metnini içeren istek `iPad` veya `iPhone`.  
  
```xml  
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|değişken Ayarla|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|  
|---------------|-----------------|--------------|  
|ad|Değişken adı.|Evet|  
|değer|Değişken değeri. Bu bir deyim veya bir hazır değer olabilir.|Evet|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen, giden arka uç, hata  
  
-   **İlke kapsamları:** tüm kapsamlar  
  
###  <a name="set-variableAllowedTypes"></a>İzin verilen türleri  
 Kullanılan ifadeleri `set-variable` ilke aşağıdaki temel türlerinden birini döndürmesi gerekir.  
  
-   System.Boolean  
  
-   System.SByte  
  
-   System.Byte  
  
-   System.UInt16  
  
-   System.UInt32  
  
-   System.UInt64  
  
-   System.Int16  
  
-   System.Int32  
  
-   System.Int64  
  
-   System.Decimal  
  
-   System.Single  
  
-   System.Double  
  
-   System.Guid  
  
-   System.String  
  
-   System.Char  
  
-   System.DateTime  
  
-   System.TimeSpan  
  
-   System.Byte?  
  
-   System.UInt16?  
  
-   System.UInt32?  
  
-   System.UInt64?  
  
-   System.Int16?  
  
-   System.Int32?  
  
-   System.Int64?  
  
-   System.Decimal?  
  
-   System.Single?  
  
-   System.Double?  
  
-   System.Guid?  
  
-   System.String?  
  
-   System.Char?  
  
-   System.DateTime?  

##  <a name="Trace"></a>İzleme  
 `trace` İlkesi ekler bir dizeye [API denetçisi](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) çıktı. İlkeyi yalnızca zaman izleme, yani tetiklenir yürütecek `Ocp-Apim-Trace` istek üstbilgisi varsa ve için `true` ve `Ocp-Apim-Subscription-Key` istek üstbilgisi var ve yönetici hesabıyla ilişkilendirilmiş geçerli bir anahtar tutar.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
  
<trace source="arbitrary string literal"/>  
    <!-- string expression or literal -->  
</trace>  
  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|İzleme|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|Varsayılan|  
|---------------|-----------------|--------------|-------------|  
|Kaynak|Dize değişmez değeri izleme Görüntüleyicisi'ni ve iletinin kaynağını belirterek anlamlı.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .  
  
-   **İlke bölümleri:** gelen, giden arka uç, hata  
  
-   **İlke kapsamları:** tüm kapsamlar  
  
##  <a name="Wait"></a>Bekleme  
 `wait` İlke hemen alt ilkelerine paralel olarak yürütür ve tüm ya da hemen alt ilkelerini tamamlanmadan önce biri için bekler. Bekleme ilke hemen alt ilkelerine olabilir [gönderme isteği](api-management-advanced-policies.md#SendRequest), [değeri önbellekten alma](api-management-caching-policies.md#GetFromCacheByKey), ve [kontrol akışı](api-management-advanced-policies.md#choose) ilkeleri.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<wait for="all|any">  
  <!--Wait policy can contain send-request, cache-lookup-value,   
        and choose policies as child elements -->  
</wait>  
  
```  
  
### <a name="example"></a>Örnek  
 Aşağıdaki örnekte vardır iki `choose` ilkeleri hemen alt ilkelerini olarak `wait` ilkesi. Bunların her biri `choose` ilkeleri paralel olarak yürütür. Her `choose` ilke önbelleğe alınan değeri almayı dener. Önbellek isabetsizliği varsa, arka uç hizmeti değeri sağlamak için çağrılır. Bu örnekte `wait` İlkesi tamamlamaz hemen alt ilkeler tamamlanana kadar çünkü `for` özniteliği `all`.   Bu örnekte bağlam değişkenleri (`execute-branch-one`, `value-one`, `execute-branch-two`, ve `value-two`) Bu örnek ilke kapsamı dışında bildirilir.  
  
```xml  
<wait for="all">  
  <choose>  
    <when condition="@((bool)context.Variables["execute-branch-one="])">  
      <cache-lookup-value key="key-one" variable-name="value-one" />  
      <choose>  
        <when condition="@(!context.Variables.ContainsKey("value-one="))">  
          <send-request mode="new" response-variable-name="value-one">  
            <set-url>https://backend-one</set-url>  
            <set-method>GET</set-method>  
          </send-request>  
        </when>  
      </choose>  
    </when>  
  </choose>  
  <choose>  
    <when condition="@((bool)context.Variables["execute-branch-two="])">  
      <cache-lookup-value key="key-two" variable-name="value-two" />  
      <choose>  
        <when condition="@(!context.Variables.ContainsKey("value-two="))">  
          <send-request mode="new" response-variable-name="value-two">  
            <set-url>https://backend-two</set-url>  
            <set-method>GET</set-method>  
          </send-request>  
        </when>  
      </choose>  
    </when>  
  </choose>  
</wait>  
  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğesi|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|bekleme|Kök öğesi. Yalnızca alt öğeleri içerebilir `send-request`, `cache-lookup-value`, ve `choose` ilkeleri.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Öznitelik|Açıklama|Gerekli|Varsayılan|  
|---------------|-----------------|--------------|-------------|  
|için|Belirler olup olmadığını `wait` ilke tüm hemen alt ilkeleri tamamlanan veya yalnızca bir olmasını bekler. İzin verilen değerler:<br /><br /> -   `all`-Tüm hemen alt ilkeleri tamamlanması bekleyin<br />-any - tamamlamak hemen alt ilke bekleyin. İlk anında alt ilke tamamlandıktan sonra `wait` ilkeyi tamamladıktan ve diğer hemen alt ilkeleri yürütülmesi biter.|Hayır|Tüm|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen, giden arka uç  
  
-   **İlke kapsamları:**tüm kapsamlar  
  
## <a name="next-steps"></a>Sonraki adımlar
İlkeleriyle çalışma daha fazla bilgi için bkz:
-   [API Management ilkeleri](api-management-howto-policies.md) 
-   [İlke ifadeleri](api-management-policy-expressions.md)
