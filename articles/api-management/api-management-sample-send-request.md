---
title: HTTP istekleri oluşturmak için API Management hizmeti kullanma
description: Dış hizmetler, API çağrısı için API Management'te istek ve yanıt ilkeleri kullanma hakkında bilgi edinin
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 4539c0fa-21ef-4b1c-a1d4-d89a38c242fa
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: d7c32e5ae02e294ee88c19f058e04249c7c9969e
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29714680"
---
# <a name="using-external-services-from-the-azure-api-management-service"></a>Azure API Management hizmetinden dış hizmetler kullanarak
Azure API Management hizmetinde kullanılabilir ilkeler çeşitli yararlı iş tamamen gelen istek, giden yanıt ve temel yapılandırma bilgileri göre yapabilirsiniz. Ancak, dış API Management hizmetlerinden etkileşimde yapamamasına ilkeleri açılır pek çok daha fazla fırsatı.

İle etkileşim kurmak nasıl daha önce gördünüz [günlüğe kaydetme, izleme ve analiz için Azure olay hub'ı hizmeti](api-management-log-to-eventhub-sample.md). Bu makalede, tüm dış HTTP tabanlı hizmetiyle etkileşime olanak tanıyan ilkeleri gösterilmektedir. Bu ilkeler, uzak olaylarını tetiklemek veya özgün istek ve yanıt bir şekilde işlemek için kullanılan bilgileri almak için kullanılabilir.

## <a name="send-one-way-request"></a>Bir şekilde İsteği Gönder
Büyük olasılıkla önemli olay çeşit bildirim almak bir dış hizmet veren istek yangın ve unut stilini basit dış etkileşim olduğu. Denetim akışı İlkesi `choose` ilgilendiğiniz koşulu her türlü algılamak için kullanılabilir.  Koşul sağlanıyorsa, dış bir HTTP isteği kullanarak yapabilirsiniz [bir şekilde İsteği Gönder](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) ilkesi. Bu ileti sistemi Hipchat veya boşluk ya da posta API SendGrid veya MailChimp gibi gibi bir istek olabilir veya kritik destek olaylar için şuna benzer PagerDuty. Bu ileti sistemlerini çağrılabilir basit HTTP API'ler sahip.

### <a name="alerting-with-slack"></a>Kayma ile uyarı
Aşağıdaki örnek, HTTP yanıtı durum kodu 500 eşit veya daha büyük ise Slack sohbet odasına bir ileti göndermek gösterilmiştir. 500 aralık hatası istemci API kendilerini çözümlenemiyor arka uç API'si bir sorun olduğunu gösterir. Genellikle, bir tür müdahale API Management bölümü gerektirir.  

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

Kayma gelen web kancaları kavramı vardır. Gelen web kancası yapılandırırken, boşluk, basit bir POST isteği yapmak için ve bir ileti Slack kanal geçmesine izin veren özel bir URL oluşturur. Oluşturduğunuz JSON gövdesi kayma tarafından tanımlanan bir biçimini temel alır.

![Slack Web kancası](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Yangın uyguluyor ve yeterli unuttunuz mu?
İstek yangın ve unut stili kullanırken belirli bileşim yoktur. Başarısız olması bildirilmedi sonra herhangi bir nedenle olduğu ise, istek başarısız olur. Bu belirli bir durumda, sistem ve yanıt bekleme ek performans maliyeti raporlama ikincil bir hataya neden karmaşıklığını garanti değil. Yanıt denetlemek için gerekli olduğu senaryolar için sonra [gönderme isteği](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) ilkedir daha iyi bir seçenek.

## <a name="send-request"></a>Gönderme isteği
`send-request` İlkesi etkinleştirir karmaşık işleme işlevleri gerçekleştirmek ve veri API Management hizmeti dönmek için bir dış hizmet kullanarak daha fazla ilke işleme için kullanılabilir.

### <a name="authorizing-reference-tokens"></a>Başvuru belirteçleri yetkilendirme
API Management ana işlevinin arka uç kaynaklarına koruyor. API tarafından kullanılan yetkilendirme sunucusu oluşturursa [JWT belirteçleri](http://jwt.io/) kendi OAuth2 akışının parçası olarak olarak [Azure Active Directory](../active-directory/active-directory-aadconnect.md) mu kullanabileceğiniz sonra `validate-jwt` İlkesi belirtecin geçerliliğini doğrulayın. Bazı yetkilendirme sunucuları ne adlı oluşturma [başvuru belirteçleri](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) , doğrulanamıyor yetkilendirme sunucusu için bir geri arama yapmadan.

### <a name="standardized-introspection"></a>Standartlaştırılmış introspection
Geçmişte, yetkilendirme sunucusu başvuru belirteciyle doğrulama hiçbir standartlaştırılmış şekilde açıldı. Ancak en son önerilen standart [RFC 7662](https://tools.ietf.org/html/rfc7662) bir kaynak sunucuda bir belirtecin geçerliliğini nasıl doğrulayabilirsiniz tanımlar IETF tarafından yayımlandı.

### <a name="extracting-the-token"></a>Belirteç ayıklanıyor
Belirteç yetkilendirme başlığından ayıklamak için ilk adımdır bakın. Üstbilgi değeri ile biçimlendirilmiş olması `Bearer` Yetkilendirme düzeni, tek bir boşluk ve ardından göre yetkilendirme belirtecini [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Ne yazık ki burada Yetkilendirme düzeni atlanmış durumlar vardır. Bu ayrıştırılırken hesap için API Management üstbilgi değeri bir alana böler ve son dizenin dizeleri döndürülen diziden seçer. Bu, hatalı biçimlendirilmiş yetkilendirme üstbilgileri için geçici bir çözüm sağlar.

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-the-validation-request"></a>Doğrulama isteği yapan
API Management yetkilendirme belirtecini olduğunda, API Management belirteci doğrulama isteği yapabilirsiniz. RFC 7662 bu işlem introspection çağırır ve gerektiren, `POST` introspection kaynak için bir HTML formu. HTML formu anahtarla en az bir anahtar/değer çifti içermelidir `token`. Bu istek için yetkilendirme sunucusu Ayrıca, kötü amaçlı istemciler için geçerli belirteçleri trawling Git olamaz emin olmak için kimliğinin doğrulanması gerekir.

```xml
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
```

### <a name="checking-the-response"></a>Yanıt denetleniyor
`response-variable-name` Özniteliği döndürülen yanıt erişmesini sağlamak için kullanılır. Bu özelliği içinde tanımlı adı bir anahtar olarak kullanılan `context.Variables` erişmek için sözlük `IResponse` nesnesi.

Yanıt nesnesinden gövdesi alabilir ve yanıt bir JSON nesnesi olmalıdır ve adlı en az bir özellik içermelidir RFC 7622 söyler API Management `active` diğer bir deyişle bir Boole değeri. Zaman `active` belirtecin geçerli kabul doğrudur.

### <a name="reporting-failure"></a>Hata Raporlama
Kullanabileceğiniz bir `<choose>` belirteci geçersiz varsa ve bu durumda, algılamak için ilke 401 yanıtı döndürür.

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

Göre [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) açıklayan nasıl `bearer` belirteçleri kullanılmalıdır, API Management ayrıca döndürür bir `WWW-Authenticate` 401 yanıt üstbilgisi. WWW-Authenticate amaçlanmıştır düzgün yetkili isteği oluşturmak nasıl bir istemcide istemek üzere. Çeşitli yaklaşımlar OAuth2 framework ile olası nedeni, gerekli tüm bilgileri iletişim kurmak zordur. Neyse ki devam yardımcı olmak üzere çabalarına vardır [istemcileri bulmak düzgün bir şekilde bir kaynak sunucuya isteklerini yetkilendirmek nasıl](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Son çözüm
Sonunda, aşağıdaki İlkesi alın:

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

Bu yalnızca, birçok örnekleri nasıl biri `send-request` İlkesi, isteklerin ve yanıtların API Management hizmet aracılığıyla akan işlemine yararlı dış hizmetler tümleştirmek için kullanılabilir.

## <a name="response-composition"></a>Yanıt oluşturma
`send-request` İlkesi, önceki örnekte gördüğünüz veya tam değiştirme için arka uç çağrının kullanılabilmesi için bir arka uç sistemi birincil isteğine geliştirme için kullanılabilir. Bu teknik kullanılarak birden çok farklı sistemlerden toplanan bileşik kaynakları kolayca oluşturabilirsiniz.

### <a name="building-a-dashboard"></a>Bir pano oluşturma
Bazen birden fazla arka uç sistemlerinde, örneğin bulunan bilgilerini kullanıma sunmak için bir Pano sürücüsüne kullanabilmek ister. Tüm farklı arka uçları, KPI'ları gelir ancak bunları doğrudan erişim sağlamak için değil tercih ve tüm bilgileri tek bir istekte alınamadı, iyi olur. Belki de arka uç bilgilerin bazıları gereken bazı dilimleme ve sağlanır ve biraz önce temizleme! Bileşik bu kaynağın önbelleğe yapamamasına kendi underperforming ölçümleri değişebilir olmadığını görmek için F5 tuşuna sözcüğüne, alýþkanlýk kullanıcınız bildiğiniz gibi arka uç yükü azaltmak bir yararlı olacaktır.    

### <a name="faking-the-resource"></a>Kaynak faking
Pano kaynak oluşturmanın ilk adımı, Azure portalında yeni bir işlem yapılandırmaktır. Dinamik kaynak oluşturmak için bir birleşim ilkesini yapılandırmak için kullanılan bir yer tutucu işlemdir.

![Pano işlemi](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>İsteği gerçekleştiren
İşlemi oluşturulduktan sonra özel olarak bu işlem için bir ilke yapılandırabilirsiniz. 

![Pano işlemi](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

Arka uç için iletme ilk adımı gelen istekte, sorgu parametreleri ayıklamak için böylelikle. Bu örnekte, pano bilgileri bir zaman aralığında göre gösteren ve bu nedenle bir `fromDate` ve `toDate` parametresi. Kullanabileceğiniz `set-variable` isteği URL'den bilgi ayıklamak için ilke.

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

Bu bilgileri olduktan sonra tüm arka uç sistemleri isteğinde bulunabilir. Her istek parametre bilgileri içeren yeni bir URL oluşturur ve ilgili sunucusuna çağırır ve yanıt içeriği değişkeninde depolar.

```xml
<send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
  <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
  <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>
```

Bu istekler ideal olmayan sırayla yürütün. 

### <a name="responding"></a>Yanıt
Bileşik yanıt oluşturmak için kullanabileceğiniz [return yanıt](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) ilkesi. `set-body` Öğesi, yeni bir oluşturmak için bir ifade kullanabilir `JObject` özellikleri olarak katıştırılmış tüm bileşen Beyanları ile.

```xml
<return-response response-variable-name="existing response variable">
  <set-status code="200" reason="OK" />
  <set-header name="Content-Type" exists-action="override">
    <value>application/json</value>
  </set-header>
  <set-body>
    @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                  new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                  new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                  new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                  ).ToString())
  </set-body>
</return-response>
```

Tam İlkesi şu şekilde görünür:

```xml
<policies>
    <inbound>

  <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
  <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
</policies>
```

Yer tutucu işlemi yapılandırmasında en az bir saat için önbelleğe alınacak Pano kaynak yapılandırabilirsiniz. 

## <a name="summary"></a>Özet
Azure API Management hizmeti, HTTP trafiği için seçmeli olarak uygulanabilir esnek ilkeler sağlar ve arka uç hizmetlerinin birleşim etkinleştirir. Uyarı İşlevler, doğrulama, doğrulama yetenekleri ile API ağ geçidi geliştirmek veya birden fazla arka uç hizmetlerini temel alarak yeni bileşik kaynakları oluşturmak isteyip istemediğinizi `send-request` ve ilgili ilkeler olanaklar dünyası açın.

