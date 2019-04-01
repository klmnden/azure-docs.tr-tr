---
title: Azure API Management Gelişmiş ilkeleri | Microsoft Docs
description: Kullanılmak üzere Azure API Management Gelişmiş ilkeleri hakkında daha fazla bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: 43cbeea554f43e4db7d5440af83a9b414741d2f6
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756608"
---
# <a name="api-management-advanced-policies"></a>API Management Gelişmiş ilkeleri

Bu konu aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](https://go.microsoft.com/fwlink/?LinkID=398186).

## <a name="AdvancedPolicies"></a> Gelişmiş ilkeler

-   [Denetim akışı](api-management-advanced-policies.md#choose) - koşullu ilke bildirimlerine Boolean değerlendirme sonuçlarına göre uygulanır [ifadeleri](api-management-policy-expressions.md).
-   [İstek](#ForwardRequest) -isteği arka uç hizmetine iletir.
-   [Eşzamanlılık sınırlamak](#LimitConcurrency) -engeller içine birden çok istekleri belirtilen sayıda tarafından aynı anda yürütülmesini ilkeleri.
-   [Olay Hub'ına](#log-to-eventhub) -gönderdiği iletileri belirtilen biçimde bir Günlükçü varlık tarafından tanımlanan olay Hub'ına.
-   [Sahte yanıt](#mock-response) -iptalleri işlem hattı yürütme ve sahte yanıt doğrudan çağırana döner.
-   [Yeniden deneme](#Retry) -varsa ve koşul yerine getirilene kadar iliştirilmiş ilke bildirimlerine yürütülmesi yeniden dener. Yürütme, belirtilen zaman aralıklarında yineleyin ve belirtilen en fazla yeniden deneme sayısı.
-   [Yanıt](#ReturnResponse) -iptalleri işlem hattı yürütme ve belirtilen yanıtını çağırana doğrudan döndürür.
-   [One-way isteğini göndermek](#SendOneWayRequest) -bir istek için yanıt beklemeden belirtilen URL'ye gönderir.
-   [İsteği Gönder](#SendRequest) -bir istek belirtilen URL'ye gönderir.
-   [HTTP proxy ayarlamak](#SetHttpProxy) -bir HTTP Ara sunucusu üzerinden iletilen istekleri sağlar.
-   [İstek yöntemini ayarla](#SetRequestMethod) -bir isteğin HTTP yöntemi değiştirmenize izin verir.
-   [Durum kodu ayarlamak](#SetStatus) -HTTP durum kodunu belirtilen değere değişir.
-   [Değişken Ayarla](api-management-advanced-policies.md#set-variable) -adlandırılmış bir değer devam ederse [bağlam](api-management-policy-expressions.md#ContextVariables) daha sonra erişmek için değişken.
-   [İzleme](#Trace) -bir dizeye ekler [API denetçisi](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) çıktı.
-   [Bekleyin](#Wait) -içine için bekleyeceği [gönderme isteği](api-management-advanced-policies.md#SendRequest), [değeri önbellekten alma](api-management-caching-policies.md#GetFromCacheByKey), veya [denetim akışı](api-management-advanced-policies.md#choose) devam etmeden önce tamamlanması ilkeleri.

## <a name="choose"></a> Denetim akışı

`choose` İlke, değerlendirme ve Boolean ifadelerin bir if-then-else veya bir anahtar benzer sonucuna göre deyimleri bir programlama dili oluşturmada iliştirilmiş ilke uygulanır.

### <a name="ChoosePolicyStatement"></a> İlke bildirimi

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

Denetim akışı ilkesi en az birini içermelidir `<when/>` öğesi. `<otherwise/>` Öğesi, isteğe bağlıdır. Koşulları `<when/>` öğeleri ilke içinde görünme sırasına göre değerlendirilir. İlke deyim içinde ilk içine `<when/>` koşul özniteliği eşittir öğeyle `true` uygulanır. İlkeleri içine içinde `<otherwise/>` öğesi, varsa, tüm uygulanır, `<when/>` öğesi koşul öznitelikler `false`.

### <a name="examples"></a>Örnekler

#### <a name="ChooseExample"></a> Örnek

Aşağıdaki örnek, gösterir bir [değişken Ayarla](api-management-advanced-policies.md#set-variable) İlkesi ve iki denetim akışı ilkeleri.

Değişken İlkesi ayarlama gelen bölümünde ve oluşturur bir `isMobile` Boole [bağlam](api-management-policy-expressions.md#ContextVariables) true olarak ayarlanır değişkeni `User-Agent` üst bilgi metnini içeren istek `iPad` veya `iPhone`.

İlk denetim akışı ilke ayrıca gelen bölümünde ve koşullu olarak iki birini uygular [ayarlamak sorgu dizesi parametresi](api-management-transformation-policies.md#SetQueryStringParameter) değerine göre ilkeleri `isMobile` bağlam değişkeni.

İkinci denetim akışı ilke giden bölümünde ve koşullu olarak geçerlidir [XML'i Dönüştür](api-management-transformation-policies.md#ConvertXMLtoJSON) ilke olduğunda `isMobile` ayarlanır `true`.

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
            <when condition="@(context.Variables.GetValueOrDefault<bool>("isMobile"))">
                <xml-to-json kind="direct" apply="always" consider-accept-header="false"/>
            </when>
        </choose>
    </outbound>
</policies>
```

#### <a name="example"></a>Örnek

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

### <a name="elements"></a>Öğeler

| Öğe   | Açıklama                                                                                                                                                                                                                                                               | Gerekli |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| Seçin    | Kök öğe.                                                                                                                                                                                                                                                             | Evet      |
| zaman      | İçin kullanılacak koşulu `if` veya `ifelse` bölümlerini `choose` ilkesi. Varsa `choose` ilkeye sahip birden çok `when` bölümleri sırayla değerlendirilir. Bir kez `condition` öğesi bir zaman değerlendiren `true`, başka `when` koşullar değerlendirilir. | Evet      |
| Aksi takdirde | Hiçbiri kullanılacak ilke kod parçacığının içerdiği `when` koşullar değerlendirmek için `true`.                                                                                                                                                                               | Hayır       |

### <a name="attributes"></a>Öznitelikler

| Öznitelik                                              | Açıklama                                                                                               | Gerekli |
| ------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- | -------- |
| koşul = "Boole ifadesi &#124; Boole sabiti" | Boole ifadesi veya sabit değer için değerlendirilen ne zaman içeren `when` ilke ifadesi değerlendirilir. | Evet      |

### <a name="ChooseUsage"></a> Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden arka uç, hata

-   **İlke kapsamları:** tüm kapsamlar

## <a name="ForwardRequest"></a> İstek

`forward-request` İlke isteğinde belirtilen arka uç hizmeti gelen isteği iletir [bağlam](api-management-policy-expressions.md#ContextVariables). Arka uç hizmeti URL'si API'SİNDE belirtilen [ayarları](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) ve kullanılarak değiştirilebilir [ayarlayın, arka uç hizmeti](api-management-transformation-policies.md) ilkesi.

> [!NOTE]
> Arka uca iletilen değil isteği bu ilkeyi kaldırarak hizmet ve giden bölümünde ilkeleri hemen ilkeleri gelen bölümündeki başarıyla tamamlanmasından sonra değerlendirilir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<forward-request timeout="time in seconds" follow-redirects="true | false" buffer-request-body="true | false" />
```

### <a name="examples"></a>Örnekler

#### <a name="example"></a>Örnek

Aşağıdaki API düzeyi ilke, tüm API isteklerinin 60 saniyelik bir zaman aşımı aralığı ile arka uç hizmetine iletir.

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

Bu işlem düzeyi ilkeyi kullanan `base` üst düzey kapsamındaki API arka uç İlkesi devralmak için öğesi.

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

Bu işlem düzeyi ilke açıkça tüm istekleri bir zaman aşımı süresi 120 ile arka uç hizmetine iletir ve API düzeyi arka uç İlkesi üst devralmaz.

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

Bu işlem düzeyi ilke istekleri arka uç hizmetine iletmez.

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

### <a name="elements"></a>Öğeler

| Öğe         | Açıklama   | Gerekli |
| --------------- | ------------- | -------- |
| İleri-istek | Kök öğe. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik                               | Açıklama                                                                                                      | Gerekli | Varsayılan     |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | -------- | ----------- |
| timeout="integer"                       | Bir zaman aşımı hatası önce arka uç hizmeti tarafından döndürülecek HTTP yanıtı üstbilgileri için beklenecek saniye cinsinden süreyi ortaya çıkar. Minimum değer 0 saniyedir. 240 saniye temel ağ altyapısını ödenmiş olmayabilir daha büyük değerler bu tarihten sonra boşta kalan bağlantıların bırakabilirsiniz. | Hayır       | None |
| follow-redirects="true &#124; false"    | Arka uç hizmetinden yeniden yönlendirmeleri gateway tarafından izlenen veya çağırana döndürülen belirtir.      | Hayır       | false       |
| buffer-request-body="true &#124; false" | Ayarlandığında, "true" istek yürütülmeden ve üzerinde yeniden [yeniden](api-management-advanced-policies.md#Retry). | Hayır       | false       |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** arka uç
-   **İlke kapsamları:** tüm kapsamlar

## <a name="LimitConcurrency"></a> Eşzamanlılık sınırı

`limit-concurrency` İlke kapalı ilkeleri tarafından belirtilen istek sayısının birden fazla herhangi bir zamanda yürütülmesini engeller. Bu sayıyı aşan üzerine yeni istekler hemen 429 çok fazla istek durum kodu ile başarısız olur.

### <a name="LimitConcurrencyStatement"></a> İlke bildirimi

```xml
<limit-concurrency key="expression" max-count="number">
        <!— nested policy statements -->
</limit-concurrency>
```

### <a name="examples"></a>Örnekler

#### <a name="example"></a>Örnek

Aşağıdaki örnek, bir bağlam değişkeninin değerine göre bir arka uca iletilen istek sayısını sınırlamak gösterilmektedir.

```xml
<policies>
  <inbound>…</inbound>
  <backend>
    <limit-concurrency key="@((string)context.Variables["connectionId"])" max-count="3">
      <forward-request timeout="120"/>
    <limit-concurrency/>
  </backend>
  <outbound>…</outbound>
</policies>
```

### <a name="elements"></a>Öğeler

| Öğe           | Açıklama   | Gerekli |
| ----------------- | ------------- | -------- |
| Eşzamanlılık sınırı | Kök öğe. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik | Açıklama                                                                                        | Gerekli | Varsayılan |
| --------- | -------------------------------------------------------------------------------------------------- | -------- | ------- |
| anahtar       | Bir dize. Verilen ifade. Eşzamanlılık kapsamı belirtir. Birden çok ilke tarafından paylaşılabilir. | Evet      | Yok     |
| en yüksek sayısı | Bir tamsayı. İstekleri ilkeyi girmek için izin verilen en fazla sayısını belirtir.           | Evet      | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden arka uç, hata

-   **İlke kapsamları:** tüm kapsamlar

## <a name="log-to-eventhub"></a> Olay Hub'ına

`log-to-eventhub` İlke Günlükçü varlık tarafından tanımlanan olay Hub'ına belirtilen biçimde iletileri gönderir. Adından da anlaşılacağı gibi ilke, çevrimiçi veya çevrimdışı analiz için seçili istek veya yanıtı bağlam bilgilerini kaydetmek için kullanılır.

> [!NOTE]
> Bir olay hub'ı ve olayları günlüğe kaydetmeyi yapılandırma hakkında adım adım yönergeler için bkz. [Azure Event Hubs ile API Management olaylarını günlüğe kaydedecek şekilde nasıl](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).

### <a name="policy-statement"></a>İlke bildirimi

```xml
<log-to-eventhub logger-id="id of the logger entity" partition-id="index of the partition where messages are sent" partition-key="value used for partition assignment">
  Expression returning a string to be logged
</log-to-eventhub>

```

### <a name="example"></a>Örnek

Herhangi bir dize değeri olarak olay hub'ları, oturum açmış olmanız için kullanılabilir. Tarih ve saat, dağıtım hizmet adı, istek kimliği, IP adresi ve işlem adı tüm gelen çağrılar için olay hub'ına günlüğe kaydedilir Bu örnekte Günlükçü kayıtlı `contoso-logger` kimliği.

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

### <a name="elements"></a>Öğeler

| Öğe         | Açıklama                                                                     | Gerekli |
| --------------- | ------------------------------------------------------------------------------- | -------- |
| Günlük eventhub | Kök öğe. Bu öğenin değeri, olay hub'ınıza bağlanmanız dizedir. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik     | Açıklama                                                               | Gerekli                                                             |
| ------------- | ------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| Günlükçü kimliği     | Günlükçü kimliğini API Yönetimi hizmetiniz ile kayıtlı.         | Evet                                                                  |
| bölüm kimliği  | İletilerin gönderileceği bölümün dizinini belirtir.             | İsteğe bağlı. Bu öznitelik, kullanılamaz `partition-key` kullanılır. |
| Bölüm anahtarı | Bölüm ataması için ileti gönderilirken kullanılan değeri belirtir. | İsteğe bağlı. Bu öznitelik, kullanılamaz `partition-id` kullanılır.  |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden arka uç, hata

-   **İlke kapsamları:** tüm kapsamlar

## <a name="mock-response"></a> Sahte yanıt

`mock-response`, Adından da anlaşılacağı, API ve işlemleri sahte için kullanılır. Bu, normal işlem hattı yürütme iptal eder ve çağırana sahte yanıt döndürmesini. İlke, her zaman en yüksek güvenilirliğe sahip yanıtların döndürmeye çalışır. Bu yanıt içerik örnekler, kullanılabilir olduğunda tercih eder. Bu örnek yanıtlar, şemalardan şemaları sağlanır ve örnekler olmayan oluşturur. Örnek ya da şemaları bulunursa, içeriği olmayan yanıt döndürülür.

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

### <a name="elements"></a>Öğeler

| Öğe       | Açıklama   | Gerekli |
| ------------- | ------------- | -------- |
| sahte yanıt | Kök öğe. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik    | Açıklama                                                                                           | Gerekli | Varsayılan |
| ------------ | ----------------------------------------------------------------------------------------------------- | -------- | ------- |
| Durum kodu  | Yanıt durum kodu belirtir ve karşılık gelen örnek veya şema seçmek için kullanılır.                 | Hayır       | 200     |
| içerik türü | Belirtir `Content-Type` yanıt üst bilgisi değeri ve karşılık gelen örnek veya şema seçmek için kullanılır. | Hayır       | None    |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden, hata

-   **İlke kapsamları:** tüm kapsamlar

## <a name="Retry"></a> yeniden deneme

`retry` İlkesi kendi alt ilkeleri bir kez çalıştırır ve ardından yürütülmesi kadar yeniden deneme `condition` olur `false` veya yeniden deneme `count` tükendi olduğunu.

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

Aşağıdaki örnekte, on kez bir üstel yeniden deneme algoritmasını kullanarak en fazla istek iletme denenir. Bu yana `first-fast-retry` false, tüm yeniden deneme girişimleri olan tabi üstel yeniden deneme algoritma ayarlanmış.

```xml

<retry
    condition="@(context.Response.StatusCode == 500)"
    count="10"
    interval="10"
    max-interval="100"
    delta="10"
    first-fast-retry="false">
        <forward-request buffer-request-body="true" />
</retry>

```

### <a name="elements"></a>Öğeler

| Öğe | Açıklama                                                         | Gerekli |
| ------- | ------------------------------------------------------------------- | -------- |
| retry   | Kök öğe. Diğer ilkelerle onun alt öğeleri içerebilir. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik        | Açıklama                                                                                                                                           | Gerekli | Varsayılan |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| koşul        | Boole sabit veya [ifade](api-management-policy-expressions.md) yeniden denemeleri durdurulur belirtme (`false`) veya devam (`true`).      | Evet      | Yok     |
| count            | Denemek için yeniden deneme sayısını belirten pozitif bir sayı.                                                                                | Evet      | Yok     |
| interval         | Saniye cinsinden yeniden deneme bekleme aralığını belirten pozitif bir sayı çalışır.                                                                 | Evet      | Yok     |
| en büyük aralık     | Saniye cinsinden maksimum belirten pozitif bir sayı, yeniden deneme girişimleri arasındaki süreyi bekleyin. Üstel yeniden deneme algoritma uygulamak için kullanılır. | Hayır       | Yok     |
| delta            | Saniye bekleme aralığı artışı belirten pozitif bir sayı. Doğrusal ve üstel yeniden deneme algoritmalar uygulamak için kullanılır.             | Hayır       | Yok     |
| ilk hızlı yeniden | Varsa kümesine `true` , ilk yeniden deneme girişimi hemen gerçekleştirilir.                                                                                  | Hayır       | `false` |

> [!NOTE]
> Yalnızca `interval` belirtilen **sabit** aralığı yeniden denemeler gerçekleştirilir.
> Yalnızca `interval` ve `delta` belirtilir, bir **doğrusal** aralığı yeniden deneme algoritma kullanılır, bekleme süresi yeniden denemeler arasında nerede hesaplanır aşağıdaki formülü - uygun `interval + (count - 1)*delta`.
> Zaman `interval`, `max-interval` ve `delta` belirtilir, **üstel** aralığı yeniden deneme algoritması uygulandığında, burada yeniden denemeler arasındaki bekleme süresi değerinden katlanarak artıyor `interval` için değer `max-interval` aşağıdaki formülü göre- `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) . Alt ilke kullanım kısıtlamaları Bu ilke tarafından devralınır unutmayın.

-   **İlke bölümler:** gelen, giden arka uç, hata

-   **İlke kapsamları:** tüm kapsamlar

## <a name="ReturnResponse"></a> Dönüş yanıtındaki

`return-response` İlkesi işlem hattı yürütme iptal eder ve bir varsayılan veya özel yanıtını çağırana döner. Varsayılan yanıt `200 OK` hiçbir gövdesi olan. Özel yanıtı bir bağlam değişkeni veya ilke deyimleri ile belirtilebilir. Her ikisi de sağlandığında, bağlam değişkeni içinde yer alan yanıt çağırana döndürülmeden önce ilke bildirimlerine tarafından değiştirilir.

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

### <a name="elements"></a>Öğeler

| Öğe         | Açıklama                                                                               | Gerekli |
| --------------- | ----------------------------------------------------------------------------------------- | -------- |
| döndürülecek yanıt | Kök öğe.                                                                             | Evet      |
| üst bilgi ayarlama      | A [üst bilgi ayarlama](api-management-transformation-policies.md#SetHTTPheader) ilke bildirimi. | Hayır       |
| gövdeyi Ayarla        | A [gövdeyi Ayarla](api-management-transformation-policies.md#SetBody) ilke bildirimi.         | Hayır       |
| durumunu ayarla      | A [durumunu ayarla](api-management-advanced-policies.md#SetStatus) ilke bildirimi.           | Hayır       |

### <a name="attributes"></a>Öznitelikler

| Öznitelik              | Açıklama                                                                                                                                                                          | Gerekli  |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------- |
| yanıt değişken adı | Bağlam değişkeni adını, örneğin, bir Yukarı Akış başvurulan [gönderme isteği](api-management-advanced-policies.md#SendRequest) İlkesi ve içeren bir `Response` nesnesi | İsteğe bağlı. |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden arka uç, hata

-   **İlke kapsamları:** tüm kapsamlar

## <a name="SendOneWayRequest"></a> One-way isteğini Gönder

`send-one-way-request` İlke gönderir sağlanan istek belirtilen URL'ye yanıt beklemeden.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<send-one-way-request mode="new | copy">
  <url>...</url>
  <method>...</method>
  <header name="" exists-action="override | skip | append | delete">...</header>
  <body>...</body>
  <authentication-certificate thumbprint="thumbprint" />
</send-one-way-request>

```

### <a name="example"></a>Örnek

Bu örnek ilke kullanan bir örnek gösterilmektedir `send-one-way-request` HTTP yanıt kodunu büyüktür veya eşittir 500 ise, Slack sohbet odası için bir ileti göndermek için ilke. Bu örnek hakkında daha fazla bilgi için bkz. [Azure API Management hizmetinden dış hizmetler kullanma](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).

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

### <a name="elements"></a>Öğeler

| Öğe                    | Açıklama                                                                                                 | Gerekli                        |
| -------------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------- |
| bir şekilde İsteği Gönder       | Kök öğe.                                                                                               | Evet                             |
| url                        | İsteğin URL'si.                                                                                     | Hiçbir if modu kopyaya = Aksi takdirde Evet. |
| method                     | İsteğin HTTP yöntemi.                                                                            | Hiçbir if modu kopyaya = Aksi takdirde Evet. |
| üst bilgi                     | İstek üstbilgisi. Birden çok üstbilgi öğeleri için birden çok istek üst bilgileri kullanın.                                  | Hayır                              |
| body                       | İstek gövdesi.                                                                                           | Hayır                              |
| kimlik doğrulama sertifikası | [İstemci kimlik doğrulaması için kullanılacak sertifika](api-management-authentication-policies.md#ClientCertificate) | Hayır                              |

### <a name="attributes"></a>Öznitelikler

| Öznitelik     | Açıklama                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan  |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | -------- |
| mod = "string" | Geçerli isteğin bir kopyasını ya da yeni bir istek olup olmadığını belirler. Giden modunda, mod = kopyalama istek gövdesi başlatamadı.                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Hayır       | Yeni      |
| ad          | Ayarlanacak üstbilginin adı belirtir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Evet      | Yok      |
| Mevcut eylem | Üstbilgi zaten belirtildiğinde gerçekleştirilecek eylemi belirtir. Bu öznitelik aşağıdaki değerlerden birine sahip olmalıdır.<br /><br /> -override - mevcut üstbilgisinin değerini değiştirir.<br />-skip - var olan üstbilgi değeri yerini almaz.<br />-ekleme - değeri var olan üstbilgi değerine ekler.<br />-delete - üstbilgi istekten kaldırır.<br /><br /> Ayarlandığında `override` göre (Bu, birden çok kez listelenir), tüm girişleri ayarlanan üst bilgisindeki sonuçları aynı ada sahip birden çok girişi kaydetme; yalnızca listelenen değerler sonuç ayarlanır. | Hayır       | geçersiz kılma |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden arka uç, hata

-   **İlke kapsamları:** tüm kapsamlar

## <a name="SendRequest"></a> İsteği Gönder

`send-request` İlke gönderir sağlanan istek belirtilen URL'ye kümesi zaman aşımı değerinden daha uzun bekleniyor.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error
="false|true">
  <set-url>...</set-url>
  <set-method>...</set-method>
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>
  <set-body>...</set-body>
  <authentication-certificate thumbprint="thumbprint" />
</send-request>

```

### <a name="example"></a>Örnek

Bu örnek başvuru doğrulamanın bir yolu belirteç bir yetkilendirme sunucusu olarak gösterir. Bu örnek hakkında daha fazla bilgi için bkz. [Azure API Management hizmetinden dış hizmetler kullanma](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).

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
            <return-response>
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

### <a name="elements"></a>Öğeler

| Öğe                    | Açıklama                                                                                                 | Gerekli                        |
| -------------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------- |
| Gönderme isteği               | Kök öğe.                                                                                               | Evet                             |
| url                        | İsteğin URL'si.                                                                                     | Hiçbir if modu kopyaya = Aksi takdirde Evet. |
| method                     | İsteğin HTTP yöntemi.                                                                            | Hiçbir if modu kopyaya = Aksi takdirde Evet. |
| üst bilgi                     | İstek üstbilgisi. Birden çok üstbilgi öğeleri için birden çok istek üst bilgileri kullanın.                                  | Hayır                              |
| body                       | İstek gövdesi.                                                                                           | Hayır                              |
| kimlik doğrulama sertifikası | [İstemci kimlik doğrulaması için kullanılacak sertifika](api-management-authentication-policies.md#ClientCertificate) | Hayır                              |

### <a name="attributes"></a>Öznitelikler

| Öznitelik                       | Açıklama                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Gerekli | Varsayılan  |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | -------- |
| mod = "string"                   | Geçerli isteğin bir kopyasını ya da yeni bir istek olup olmadığını belirler. Giden modunda, mod = kopyalama istek gövdesi başlatamadı.                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Hayır       | Yeni      |
| response-variable-name="string" | Yanıt nesnesini alacak bağlam değişkeninin adı. Değişkeni mevcut değilse, ilkenin başarılı yürütme sonrasında oluşturulur ve aracılığıyla erişilebilir olacak [ `context.Variable` ](api-management-policy-expressions.md#ContextVariables) koleksiyonu.                                                                                                                                                                                                                                                                                                                          | Evet      | Yok      |
| timeout="integer"               | URL'ye yapılan çağrının saniye cinsinden zaman aşımı aralığı başarısız olur.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Hayır       | 60       |
| Hatayı Yoksay                    | Varsa true ve istek sonuçları hata:<br /><br /> -Eğer null değeri içerecek yanıt değişkeni adı belirtildi.<br />-Yanıt değişken adı belirtilmedi, bağlam. İstek güncelleştirilmez.                                                                                                                                                                                                                                                                                                                                                                                   | Hayır       | false    |
| ad                            | Ayarlanacak üstbilginin adı belirtir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Evet      | Yok      |
| Mevcut eylem                   | Üstbilgi zaten belirtildiğinde gerçekleştirilecek eylemi belirtir. Bu öznitelik aşağıdaki değerlerden birine sahip olmalıdır.<br /><br /> -override - mevcut üstbilgisinin değerini değiştirir.<br />-skip - var olan üstbilgi değeri yerini almaz.<br />-ekleme - değeri var olan üstbilgi değerine ekler.<br />-delete - üstbilgi istekten kaldırır.<br /><br /> Ayarlandığında `override` göre (Bu, birden çok kez listelenir), tüm girişleri ayarlanan üst bilgisindeki sonuçları aynı ada sahip birden çok girişi kaydetme; yalnızca listelenen değerler sonuç ayarlanır. | Hayır       | geçersiz kılma |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden arka uç, hata

-   **İlke kapsamları:** tüm kapsamlar

## <a name="SetHttpProxy"></a> HTTP proxy Ayarla

`proxy` İlke istekleri arka uçları bir HTTP Ara sunucusu üzerinden iletilen size verir. Yalnızca HTTP (HTTPS değil), proxy ve ağ geçidi arasında desteklenir. Temel ve yalnızca NTLM kimlik doğrulaması.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<proxy url="http://hostname-or-ip:port" username="username" password="password" />

```

### <a name="example"></a>Örnek

Kullanımına dikkat edin [özellikleri](api-management-howto-properties.md) kullanıcı adı ve parola ilkesi belgede hassas bilgi depolanmasını önlemek için değerler olarak.

```xml
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />

```

### <a name="elements"></a>Öğeler

| Öğe | Açıklama  | Gerekli |
| ------- | ------------ | -------- |
| Proxy   | Kök öğe | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik         | Açıklama                                            | Gerekli | Varsayılan |
| ----------------- | ------------------------------------------------------ | -------- | ------- |
| URL = "string"      | Proxy URL'si biçiminde http://host:port.             | Evet      | Yok     |
| Kullanıcı adı = "string" | Proxy kimlik doğrulaması için kullanılacak kullanıcı adı. | Hayır       | Yok     |
| parola = "string" | Proxy kimlik doğrulaması için kullanılacak parola. | Hayır       | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen

-   **İlke kapsamları:** tüm kapsamlar

## <a name="SetRequestMethod"></a> İstek yöntemini ayarla

`set-method` İlke bir istek için HTTP istek yöntemi değiştirmenize izin verir.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-method>METHOD</set-method>

```

### <a name="example"></a>Örnek

Bu örnek ilke kullanan `set-method` ilkesini gösteren bir örnek HTTP yanıt kodunu büyüktür veya eşittir 500 ise Slack sohbet odası için ileti gönderme. Bu örnek hakkında daha fazla bilgi için bkz. [Azure API Management hizmetinden dış hizmetler kullanma](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).

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

### <a name="elements"></a>Öğeler

| Öğe    | Açıklama                                                       | Gerekli |
| ---------- | ----------------------------------------------------------------- | -------- |
| Set yöntemi | Kök öğe. Öğenin değeri HTTP yöntemi belirtir. | Evet      |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, hata

-   **İlke kapsamları:** tüm kapsamlar

## <a name="SetStatus"></a> Küme durum kodu

`set-status` İlkesini HTTP durum kodunu belirtilen değere ayarlar.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<set-status code="" reason=""/>

```

### <a name="example"></a>Örnek

Bu örnek, bir 401 yanıt yetkilendirme belirteci geçersiz ise döndürülecek gösterilmektedir. Daha fazla bilgi için [Azure API Management hizmetinden dış hizmetler kullanma](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)

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

### <a name="elements"></a>Öğeler

| Öğe    | Açıklama   | Gerekli |
| ---------- | ------------- | -------- |
| durumunu ayarla | Kök öğe. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik       | Açıklama                                                | Gerekli | Varsayılan |
| --------------- | ---------------------------------------------------------- | -------- | ------- |
| code="integer"  | Döndürülecek HTTP durum kodu.                            | Evet      | Yok     |
| neden = "string" | Durum kodunu döndürerek nedeni açıklaması. | Evet      | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** giden, arka uç, hata
-   **İlke kapsamları:** tüm kapsamlar

## <a name="set-variable"></a> Değişken Ayarla

`set-variable` İlke bildirir bir [bağlam](api-management-policy-expressions.md#ContextVariables) değişkeni aracılığıyla belirtilen bir değer atar bir [ifade](api-management-policy-expressions.md) veya bir dize. ifade bir sabit değer içeriyorsa, bir dizeye dönüştürülür ve değer türünde `System.String`.

### <a name="set-variablePolicyStatement"></a> İlke bildirimi

```xml
<set-variable name="variable name" value="Expression | String literal" />
```

### <a name="set-variableExample"></a> Örnek

Aşağıdaki örnek, bir değişken İlkesi ayarlama gelen bölümündeki gösterir. Bu küme değişken ilkesini oluşturur bir `isMobile` Boole [bağlam](api-management-policy-expressions.md#ContextVariables) true olarak ayarlanır değişkeni `User-Agent` üst bilgi metnini içeren istek `iPad` veya `iPhone`.

```xml
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />
```

### <a name="elements"></a>Öğeler

| Öğe      | Açıklama   | Gerekli |
| ------------ | ------------- | -------- |
| değişken Ayarla | Kök öğe. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik | Açıklama                                                              | Gerekli |
| --------- | ------------------------------------------------------------------------ | -------- |
| ad      | Değişkenin adı.                                                | Evet      |
| değer     | Değişken değeri. Bu, bir ifade veya değişmez değer olabilir. | Evet      |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden arka uç, hata
-   **İlke kapsamları:** tüm kapsamlar

### <a name="set-variableAllowedTypes"></a> İzin verilen türler

Kullanılan ifadeler `set-variable` ilke aşağıdaki temel türlerinden birini döndürmesi gerekir.

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

## <a name="Trace"></a> İzleme

`trace` İlke ekler bir dizeye [API denetçisi](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) çıktı. İlkeyi yalnızca izleme, yani tetiklendiğinde yürütülür `Ocp-Apim-Trace` varsa ve istek üst bilgisi için `true` ve `Ocp-Apim-Subscription-Key` istek üst bilgisi var ve yönetici hesabıyla ilişkilendirilmiş geçerli bir anahtar tutar.

### <a name="policy-statement"></a>İlke bildirimi

```xml

<trace source="arbitrary string literal">
    <!-- string expression or literal -->
</trace>

```

### <a name="elements"></a>Öğeler

| Öğe | Açıklama   | Gerekli |
| ------- | ------------- | -------- |
| İzleme   | Kök öğe. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik | Açıklama                                                                             | Gerekli | Varsayılan |
| --------- | --------------------------------------------------------------------------------------- | -------- | ------- |
| source    | Dize sabit değeri anlamlı izleme görüntüleyicisini ve iletinin kaynağını belirtme. | Evet      | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .

-   **İlke bölümler:** gelen, giden arka uç, hata

-   **İlke kapsamları:** tüm kapsamlar

## <a name="Wait"></a> bekleme

`wait` İlkesi ilk alt öğe ilkelerine paralel olarak yürütür ve tüm veya ilk alt öğe ilkelerini tamamlanmadan önce birini bekler. Bekleme İlkesi, ilk alt öğe ilkelere sahip [gönderme isteği](api-management-advanced-policies.md#SendRequest), [değeri önbellekten alma](api-management-caching-policies.md#GetFromCacheByKey), ve [denetim akışı](api-management-advanced-policies.md#choose) ilkeleri.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<wait for="all|any">
  <!--Wait policy can contain send-request, cache-lookup-value,
        and choose policies as child elements -->
</wait>

```

### <a name="example"></a>Örnek

Aşağıdaki örnekte var olan iki `choose` ilkeleri ilk alt öğe ilkeleri olarak `wait` ilkesi. Bunların her biri `choose` ilkeleri paralel olarak yürütür. Her `choose` İlkesi bir önbelleğe alınan değer almayı dener. Önbellek isabetsizliği varsa, bir arka uç hizmeti değeri sağlamak için çağrılır. Bu örnekte `wait` ilke tamamlamaz, ilk alt öğe ilkeler tamamlanana kadar çünkü `for` özniteliği `all`. Bu örnekte bağlam değişkenlerini (`execute-branch-one`, `value-one`, `execute-branch-two`, ve `value-two`) Bu örnek ilke kapsamı dışında bildirilir.

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

### <a name="elements"></a>Öğeler

| Öğe | Açıklama                                                                                                   | Gerekli |
| ------- | ------------------------------------------------------------------------------------------------------------- | -------- |
| bekleme    | Kök öğe. Alt öğeleri olarak içerebilir `send-request`, `cache-lookup-value`, ve `choose` ilkeleri. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Öznitelik | Açıklama                                                                                                                                                                                                                                                                                                                                                                                                            | Gerekli | Varsayılan |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| için       | Belirler olmadığını `wait` İlkesi, tamamlanmış ya da yalnızca bir tane olacak şekilde ilk alt öğe yönelik tüm ilkeleri bekler. İzin verilen değerler şunlardır:<br /><br /> - `all` -Tüm ilk alt öğe ilkeleri tamamlanması bekle<br />-Tüm - tamamlamak ilk alt ilke bekleyin. İlk ilk alt ilke tamamlandıktan sonra `wait` ilke tamamlar ve diğer ilk alt öğe ilkeleri yürütülmesini sonlandırılır. | Hayır       | tümü     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden arka uç
-   **İlke kapsamları:** tüm kapsamlar

## <a name="next-steps"></a>Sonraki adımlar

İlkeleriyle çalışma hakkında bilgi için bkz:

-   [API Management ilkeleri](api-management-howto-policies.md)
-   [İlke ifadeleri](api-management-policy-expressions.md)
-   [İlke başvurusu](api-management-policy-reference.md) ilke bildirimlerine ve ayarlarının tam listesi için
-   [İlke örnekleri](policy-samples.md)
