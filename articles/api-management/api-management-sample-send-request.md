---
title: HTTP istekleri oluşturmak için API Management hizmeti kullanarak
description: Dış hizmetler, API çağrısı için API Yönetimi'nde istek ve yanıt ilkelerini kullanmayı öğrenin
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
ms.openlocfilehash: 2c4e5d0117f046343b140ef2b2c46c074c835075
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59796537"
---
# <a name="using-external-services-from-the-azure-api-management-service"></a>Azure API Management hizmetinden dış hizmetler kullanma
Azure API Management hizmetinde kullanılabilir ilkeleri faydalı iş tamamen gelen istek, giden yanıt ve temel yapılandırma bilgilerini göre çeşitli işlemleri gerçekleştirebilirsiniz. Ancak, API Yönetimi'nden dış hizmetlerle etkileşim için ilkeleri açılır daha fazla fırsatlarının.

İle etkileşim kurmak nasıl daha önce gördünüz [günlüğe kaydetme, izleme ve analiz için Azure olay hub'ı hizmet](api-management-log-to-eventhub-sample.md). Bu makalede, dış bir HTTP tabanlı hizmetle etkileşim kurmanıza imkan tanıyan ilkelerin gösterilmektedir. Bu ilkeler, uzak olaylarını tetiklemek ya da özgün istek ve yanıt bir şekilde işlemek için kullanılan bilgi almak için kullanılabilir.

## <a name="send-one-way-request"></a>Bir şekilde İsteği Gönder
Muhtemelen en basit dış tür önemli olay bildirim almak bir dış hizmet veren istek Başlat ve unut stilini etkileşimidir. Denetim akışı İlkesi `choose` ilgilendiğiniz koşul herhangi bir türden algılamak için kullanılabilir.  Koşul gerçekleştirilirse, dış bir HTTP isteği kullanarak yapabileceğiniz [bir şekilde isteği gönderme](/azure/api-management/api-management-advanced-policies#SendOneWayRequest) ilkesi. Destek gerektiren kritik olayları için şuna benzer PagerDuty veya bu istek Hipchat veya Slack veya SendGrid veya MailChimp gibi bir posta API gibi bir Mesajlaşma sistemi olabilir. Bu ileti sistemlerini çağrılabilir basit HTTP API'si sahiptir.

### <a name="alerting-with-slack"></a>Slack ile uyarı
Aşağıdaki örnek, HTTP yanıtı durum kodunun en az 500 ise, Slack sohbet odası için bir ileti göndermek nasıl gösterir. Bir 500 aralığı hata istemci API'ın kendilerine çözümlenemiyor arka uç API ile ilgili bir sorun olduğunu gösterir. Genellikle, API Management bölümünde müdahale tür gerektirir.  

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

Slack, gelen web kancaları kavramı vardır. Gelen web kancasını yapılandırırken, Slack, basit bir POST isteği yapmak ve bir Slack kanalına ileti geçmesine izin veren özel bir URL oluşturur. Oluşturduğunuz JSON gövdesi Slack tarafından tanımlanan bir biçimini temel alır.

![Slack Web kancası](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Ateş ve yeterli unuttunuz mu?
İsteği Başlat ve unut stilini kullanırken bazı ödünler vardır. Sonra da başarısız olması bildirilmedi bıraktıysanız için istek başarısız olur. Bu belirli durumda, sistem ve ek performans maliyetini yanıt bekleyen raporlama ikincil bir hataya neden karmaşıklığı izin yok. Yanıtı denetlemek için gerekli olduğu senaryolar için sonra [gönderme isteği](/azure/api-management/api-management-advanced-policies#SendRequest) ilke daha iyi bir seçenektir.

## <a name="send-request"></a>Gönderme isteği
`send-request` İlke etkinleştirir karmaşık işleme işlevleri gerçekleştiren ve verileri API management hizmet döndürmek için bir dış hizmet kullanarak daha fazla ilke işleme için kullanılabilir.

### <a name="authorizing-reference-tokens"></a>Başvuru belirteçleri yetkilendirme
API Management'ın önemli bir işlev, arka uç kaynaklarına korumaktır. API'niz tarafından kullanılan yetkilendirme sunucusu oluşturursa [JWT belirteçleri](https://jwt.io/) kendi OAuth2 akışının parçası olarak olarak [Azure Active Directory](../active-directory/hybrid/whatis-hybrid-identity.md) mu kullanabileceğiniz sonra `validate-jwt` belirtecin geçerliliğini doğrulamak için ilke. Bazı yetkilendirme sunucusu adı verilir oluşturmak [başvuru belirteçleri](https://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) , doğrulanamıyor yetkilendirme sunucusu için bir geri çağırma işlemini yapmadan.

### <a name="standardized-introspection"></a>Standartlaştırılmış iç denetim
Geçmişte, yetkilendirme sunucusu bir başvuru belirteçle doğrulama hiçbir standartlaştırılmış bir yol olmuştur. Ancak en son önerilen standart [RFC 7662](https://tools.ietf.org/html/rfc7662) nasıl bir kaynak sunucuda bir belirtecin geçerliliğini doğrulayabilirsiniz tanımlar IETF tarafından yayımlandı.

### <a name="extracting-the-token"></a>Belirteç ayıklanıyor
İlk adım yetkilendirme üst bilgisinden ayıklamaktır belirteç. Üst bilgi değeri ile biçimlendirilmelidir `Bearer` Yetkilendirme düzeni, tek bir boşluk ve yetkilendirme belirteci olarak başına [RFC 6750](https://tools.ietf.org/html/rfc6750#section-2.1). Ne yazık ki burada Yetkilendirme düzeni atlanırsa durumlar vardır. Ayrıştırılırken bu hesap için API Management üstbilgi değeri bir alana ayırır ve dizeleri döndürülen diziden son dizeyse seçer. Bu, hatalı biçimlendirilmiş yetkilendirme üstbilgileri için geçici bir çözüm sağlar.

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-the-validation-request"></a>Doğrulama isteği yapan
API yönetimi, API Management yetkilendirme belirteci aldığında, belirteci doğrulamak için istekte bulunabilirsiniz. RFC 7662 bu işlemi iç denetim çağırır ve gerektiren, `POST` iç denetim kaynak için bir HTML formu. HTML formu anahtarla en az bir anahtar/değer çifti içermelidir `token`. Bu istek için yetkilendirme sunucusu Ayrıca, kötü amaçlı istemciler için geçerli belirteçleri trawling Git olamaz emin olmak için kimliğinin doğrulanması gerekir.

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
`response-variable-name` Özniteliği erişim döndürülen yanıt vermek için kullanılır. Bu özelliği içinde tanımlı adı bir anahtar olarak kullanılan `context.Variables` erişmek için sözlük `IResponse` nesne.

Yanıt nesneden gövdesi alabilir ve RFC 7622 söyler API Management yanıt bir JSON nesnesi olmalıdır ve adlı en az bir özellik içermelidir `active` diğer bir deyişle bir Boole değeri. Zaman `active` belirtecin geçerli kabul geçerlidir.

### <a name="reporting-failure"></a>Hata Raporlama
Kullanabileceğiniz bir `<choose>` belirteç geçersiz olduğu ve bu durumda, algılamak için ilke 401 yanıtı döndürür.

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

Olarak başına [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) açıklayan nasıl `bearer` belirteçleri kullanılmalıdır, API Management ayrıca döndürür bir `WWW-Authenticate` 401 yanıt üst bilgisi. WWW-Authenticate amacı bir istemcide düzgün yetkili isteğinin nasıl oluşturulduğunun bildirin. Çeşitli yaklaşımlar OAuth2 framework ile olası nedeniyle, gerekli tüm bilgileri iletişim kurmak zordur. Neyse ki devam ettiği yardımcı olmak için çaba vardır [istemcileri bulmak nasıl düzgün bir şekilde istekleri kaynak sunucuya yetki vereceğiniz](https://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Son çözüm
Sonunda, şu ilkeyi alın:

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

Bu birçok örneği nasıl yalnızca biri olan `send-request` ilke yararlı dış hizmetlerden isteklerin ve yanıtların API Management hizmeti aracılığıyla akan sürecinizle tümleştirerek için kullanılabilir.

## <a name="response-composition"></a>Yanıt oluşturma
`send-request` İlkesi, önceki örnekte gördüğünüz veya bir tam değiştirme için arka uç çağrının kullanılabilir bir arka uç sistemine birincil bir istek geliştirme için kullanılabilir. Bu tekniği kullanarak birden çok farklı sistemlerden toplanan bileşik kaynakları kolayca oluşturabilirsiniz.

### <a name="building-a-dashboard"></a>Bir pano oluşturma
Bazen birden fazla arka uç sistemleri, örneğin mevcut bilgi göstermek için bir panoyu desteklemek üzere yönetebilmek istiyorsunuz. KPI'ları tüm farklı arka uçları, gelir, ancak bunları doğrudan erişim sağlamak için değil tercih ve tüm bilgileri tek bir istekte almışsa de iyi olurdu. Belki de arka uç bilgilerden bazılarını gereken bazı dilimleme ve ayrıntılı olarak incelemenin ve biraz önce temizlenirken! Bu bileşik kaynak önbellek için yeterli performansa sahip olmayan kullanıcıların ölçümleri değiştirirseniz görmek için F5 tuşuna sözcüğüne, bir alýþkanlýk kullanıcınız bildiğiniz gibi arka uç yükü azaltmak bir yararlı olacaktır.    

### <a name="faking-the-resource"></a>Kaynak faking
Pano kaynak oluşturmak için ilk adım, Azure portalında yeni bir işlem yapılandırmaktır. Dinamik kaynak oluşturmak için bir bileşim İlkesi yapılandırmak için kullanılan bir yer tutucu işlemdir.

![Pano işlemi](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>İstekler yapma
İşlem oluşturulduktan sonra bu işlem için özel bir ilke yapılandırabilirsiniz. 

![Pano işlemi](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

Arka uç için iletme ilk adımı gelen istekte, sorgu parametreleri ayıklamak için böylelikle. Bu örnekte, Pano bir sürede temel bilgilerini gösterir ve bu nedenle bir `fromDate` ve `toDate` parametresi. Kullanabileceğiniz `set-variable` ilke isteği URL'den bilgileri ayıklamak için.

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

Bu bilgi aldıktan sonra tüm arka uç sistemlerine isteğinde bulunabilir. Her istek parametre bilgileri içeren yeni bir URL oluşturur ve ilgili kendi sunucusuna çağırır ve yanıt bir bağlam değişkeninde depolar.

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
<set-url>@($"https://production.acme.com/accidentdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>
```

Bu istekler ideal değil sırayla yürütülür. 

### <a name="responding"></a>Yanıt
Bileşik yanıt oluşturmak için kullanabileceğiniz [döndürülecek yanıt](/azure/api-management/api-management-advanced-policies#ReturnResponse) ilkesi. `set-body` Öğesi yeni oluşturmak için bir ifade kullanabilirsiniz `JObject` özellikleri olarak katıştırılmış bileşen gösterimler ile.

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

Tüm ilke şu şekilde görünür:

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
    <set-url>@($"https://production.acme.com/accidentdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
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

Yer tutucu işlemi yapılandırmada en az bir saat için önbelleğe alınacak Pano kaynak yapılandırabilirsiniz. 

## <a name="summary"></a>Özet
Azure API Management hizmeti, HTTP trafiğini seçmeli olarak uygulanabilir esnek ilkeleri sağlar ve arka uç hizmetleri oluşturma sağlar. Uyarı işlevleri, doğrulama, doğrulama özellikleri ile API ağ geçidi geliştirmek veya birden fazla arka uç hizmetlerini temel alarak yeni bileşik kaynakları oluşturmak isteyip istemediğinizi `send-request` ve ilgili ilkeler olasılıklar oluşan bir dünyaya açın.

