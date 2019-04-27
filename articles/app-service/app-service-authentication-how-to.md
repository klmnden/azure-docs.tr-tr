---
title: Kullanım kimlik doğrulaması ve yetkilendirme - Azure App Service Gelişmiş | Microsoft Docs
description: Kimlik doğrulama ve yetkilendirme App Service'te özelleştirmek ve kullanıcı talepleri ve farklı bir belirteç almak nasıl gösterir.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/08/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 97764db40807214e756f119ca95fd640164f0cf2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60851432"
---
# <a name="advanced-usage-of-authentication-and-authorization-in-azure-app-service"></a>Kimlik doğrulama ve yetkilendirme Azure App Service'te özelliğinin Gelişmiş kullanımı

Bu makalede yerleşik özelleştirmek gösterilmektedir [kimlik doğrulama ve yetkilendirme App Service'te](overview-authentication-authorization.md), uygulamanızdan kimlik yönetmek için. 

Hızlıca kullanmaya başlamak için aşağıdaki öğreticilerden birine bakın:

* [Öğretici: Kimlik doğrulama ve kullanıcıları uçtan uca (Windows) Azure App Service'te yetkilendirme](app-service-web-tutorial-auth-aad.md)
* [Öğretici: Kimlik doğrulama ve kullanıcıları uçtan uca Azure App Service'te Linux için yetkilendirme](containers/tutorial-auth-aad.md)
* [Uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-aad.md)
* [Uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-facebook.md)
* [Uygulamanızı Google oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-google.md)
* [Uygulamanızı Microsoft Hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-microsoft.md)
* [Uygulamanızı Twitter oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-twitter.md)

## <a name="use-multiple-sign-in-providers"></a>Oturum açma birden çok sağlayıcı kullanma

Portal yapılandırma, çoklu oturum açma sağlayıcılarını (örneğin, Facebook ve Twitter için), kullanıcılarınıza sunmak için bir anahtar teslim yol sunmaz. Ancak, uygulamanıza işlevsellik eklemek zor değildir. Aşağıda belirtilen adımları:

İlk olarak **kimlik doğrulama / yetkilendirme** sayfasında Azure Portalı'nda, etkinleştirmek istediğiniz kimlik sağlayıcısının her yapılandırın.

İçinde **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem**seçin **izin anonim istekler (eylem yok)**.

Oturum açma sayfası veya gezinti çubuğunu veya başka bir konuma uygulamanızın oturum açma bağlantısı etkinleştirdiğiniz sağlayıcıların her birine Ekle (`/.auth/login/<provider>`). Örneğin:

```HTML
<a href="/.auth/login/aad">Log in with Azure AD</a>
<a href="/.auth/login/microsoftaccount">Log in with Microsoft Account</a>
<a href="/.auth/login/facebook">Log in with Facebook</a>
<a href="/.auth/login/google">Log in with Google</a>
<a href="/.auth/login/twitter">Log in with Twitter</a>
```

Kullanıcı bağlantılardan birini tıkladığında, kullanıcının oturum açmak için ilgili oturum açma sayfası açılır.

Kullanıcı sonrası-oturumu açma, bir özel URL'ye yeniden yönlendirmek için `post_login_redirect_url` sorgu dizesi parametresi (yeniden yönlendirme URI'si, kimlik sağlayıcısı Yapılandırması'ile karıştırılır olması değil). Örneğin, kullanıcıya gitmek için `/Home/Index` oturum açtıktan sonra aşağıdaki HTML kodu kullanın:

```HTML
<a href="/.auth/login/<provider>?post_login_redirect_url=/Home/Index">Log in</a>
```

## <a name="validate-tokens-from-providers"></a>Sağlayıcılardan gelen belirteçleri doğrulamak

Bir istemci yönelik oturum açma, uygulamayı el ile ve sağlayıcı kullanıcı oturum açar ve ardından App Service doğrulaması için kimlik doğrulama belirteci gönderir (bkz [kimlik doğrulama akışı](overview-authentication-authorization.md#authentication-flow)). Bu doğrulama gerçekten istenen uygulama kaynaklarına erişmek, ancak başarılı bir doğrulama uygulama kaynaklarına erişmek için kullanabileceğiniz bir oturum belirteci verir izni yoktur. 

Sağlayıcı belirteci doğrulamak için App Service uygulaması istenen sağlayıcı ile önce yapılandırılması gerekir. Sağlayıcınızdan kimlik doğrulama belirteci aldıktan sonra çalışma zamanında belirtece sonrası `/.auth/login/<provider>` doğrulama için. Örneğin: 

```
POST https://<appname>.azurewebsites.net/.auth/login/aad HTTP/1.1
Content-Type: application/json

{"id_token":"<token>","access_token":"<token>"}
```

Belirteç biçimi, sağlayıcıya göre biraz farklılık gösterir. Ayrıntılar için aşağıdaki tabloya bakın:

| Değer sağlayıcı | İstek gövdesinde gerekli | Yorumlar |
|-|-|-|
| `aad` | `{"access_token":"<access_token>"}` | |
| `microsoftaccount` | `{"access_token":"<token>"}` | `expires_in` Özelliği, isteğe bağlıdır. <br/>Her zaman canlı hizmetlerinden belirteç isterken istek `wl.basic` kapsam. |
| `google` | `{"id_token":"<id_token>"}` | `authorization_code` Özelliği, isteğe bağlıdır. Belirtildiğinde, isteğe bağlı olarak ayrıca eşlik `redirect_uri` özelliği. |
| `facebook`| `{"access_token":"<user_access_token>"}` | Geçerli bir kullanın [kullanıcı erişim belirteci](https://developers.facebook.com/docs/facebook-login/access-tokens) facebook'taki. |
| `twitter` | `{"access_token":"<access_token>", "access_token_secret":"<acces_token_secret>"}` | |
| | | |

Sağlayıcı belirteci başarıyla doğrulandığında ile API döndürür bir `authenticationToken` yanıt gövdesi içinde olduğu, oturum belirteci. 

```json
{
    "authenticationToken": "...",
    "user": {
        "userId": "sid:..."
    }
}
```

Bu oturum belirteci aldıktan sonra ekleyerek korumalı uygulama kaynaklara erişebilir `X-ZUMO-AUTH` , HTTP isteği üstbilgisi. Örneğin: 

```
GET https://<appname>.azurewebsites.net/api/products/1
X-ZUMO-AUTH: <authenticationToken_value>
```

## <a name="sign-out-of-a-session"></a>Dışında bir oturumu oturum

Kullanıcılar başlatabilir bir oturum kapatma göndererek bir `GET` uygulamanın isteğine `/.auth/logout` uç noktası. `GET` İsteği şunları yapar:

- Geçerli oturumun kimlik doğrulama tanımlama bilgilerini temizler.
- Geçerli kullanıcının belirteçleri, belirteci deposundan kaldırır.
- Azure Active Directory ve Google kimlik sağlayıcısını sunucu tarafı oturum kapatma gerçekleştirir.

Bir Web sayfasında basit bir oturum kapatma bağlantısına şöyledir:

```HTML
<a href="/.auth/logout">Sign out</a>
```

Varsayılan olarak başarılı bir oturum kapatma istemci URL'ye yeniden yönlendirilen `/.auth/logout/done`. Post-sign-out yönlendirme sayfası ekleyerek değiştirebileceğiniz `post_logout_redirect_uri` sorgu parametresi. Örneğin:

```
GET /.auth/logout?post_logout_redirect_uri=/index.html
```

Bırakmanız önerilir, [kodlama](https://wikipedia.org/wiki/Percent-encoding) değerini `post_logout_redirect_uri`.

Tam URL'leri kullanırken, URL gerekir aynı etki alanında barındırılan veya uygulamanız için bir izin verilen dış yönlendirme URL'si olarak yapılandırılır. Yeniden yönlendirmek için aşağıdaki örnekte `https://myexternalurl.com` aynı etki alanında bulunan değil:

```
GET /.auth/logout?post_logout_redirect_uri=https%3A%2F%2Fmyexternalurl.com
```

Aşağıdaki komutu çalıştırmanız gerekir [Azure Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp auth update --name <app_name> --resource-group <group_name> --allowed-external-redirect-urls "https://myexternalurl.com"
```

## <a name="preserve-url-fragments"></a>URL parçaları koru

Kullanıcıların uygulamanıza oturum açtıktan sonra genellikle aynı sayfanın aynı bölümüne gibi yönlendirilmesi istedikleri `/wiki/Main_Page#SectionZ`. Ancak, çünkü [URL parçaları](https://wikipedia.org/wiki/Fragment_identifier) (örneğin, `#SectionZ`) hiçbir zaman gönderilen sunucuya bunlar korunmaz varsayılan olarak OAuth oturum açma tamamlandıktan ve uygulamanıza geri yönlendirir. Kullanıcılar, ardından için istenen bağlantı yeniden gitmeniz gerektiğinde yetersiz bir deneyim alır. Bu sınırlama, tüm sunucu tarafı kimlik çözümleri için geçerlidir.

App Service kimlik doğrulaması OAuth oturum açma URL parçaları koruyabilirsiniz. Bunu yapmak için çağrılan ayarı bir uygulama kümesi `WEBSITE_AUTH_PRESERVE_URL_FRAGMENT` için `true`. Bunu yapabilirsiniz [Azure portalında](https://portal.azure.com), veya aşağıdaki komutu çalıştırmanız yeterlidir [Azure Cloud Shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group <group_name> --settings WEBSITE_AUTH_PRESERVE_URL_FRAGMENT="true"
```

## <a name="access-user-claims"></a>Erişim kullanıcı talepleri

App Service, kullanıcı talepleri uygulamanıza özel üst bilgileri kullanarak geçirir. Olmayan dış isteklerine izin mevcut olduğundan, bu üst bilgilerini ayarlayacak şekilde yalnızca App Service tarafından ayarlayın. Bazı örnek üst bilgiler şunları içerir:

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ID

Herhangi bir dil veya çerçeve yazılmış kod, bu üst bilgiler, gereken bilgileri alabilirsiniz. ASP.NET 4.6 uygulamalar için **ClaimsPrincipal** uygun değerlerle otomatik olarak ayarlanır.

Uygulamanızı Ayrıca kimliği doğrulanmış kullanıcı hakkında daha fazla ayrıntı çağırarak elde edebileceğiniz `/.auth/me`. Mobil uygulamalar sunucusu SDK'ları bu verilerle çalışmak için yardımcı yöntemler sağlar. Daha fazla bilgi için [nasıl Azure Mobile Apps Node.js SDK'sı kullanılacağını](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), ve [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="retrieve-tokens-in-app-code"></a>Uygulama kodunda belirteçleri alma

Sunucu kodunuzdan kolayca erişebilmeleri için sağlayıcıya özel belirteçler isteği üstbilgisine eklenmiş. Aşağıdaki tabloda olası belirteci üst bilgi adları gösterilmektedir:

| Sağlayıcı | Üst bilgi adları |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Facebook belirteci | `X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN` <br/> `X-MS-TOKEN-FACEBOOK-EXPIRES-ON` |
| Google | `X-MS-TOKEN-GOOGLE-ID-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-ACCESS-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-EXPIRES-ON` <br/> `X-MS-TOKEN-GOOGLE-REFRESH-TOKEN` |
| Microsoft Hesabı | `X-MS-TOKEN-MICROSOFTACCOUNT-ACCESS-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-EXPIRES-ON` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-AUTHENTICATION-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-REFRESH-TOKEN` |
| Twitter | `X-MS-TOKEN-TWITTER-ACCESS-TOKEN` <br/> `X-MS-TOKEN-TWITTER-ACCESS-TOKEN-SECRET` |
|||

İstemci kodunuz içinden (örneğin, bir mobil uygulama veya tarayıcı içinde JavaScript), bir HTTP gönderme `GET` isteği `/.auth/me`. Döndürülen JSON sağlayıcıya özgü belirteçleri sahiptir.

> [!NOTE]
> Erişim belirteci sağlayıcısı kaynakları bir gizli anahtar ile sağlayıcınız yapılandırırsanız mevcut şekilde erişmek için ' dir. Yenileme belirteçlerini almak nasıl görmek için erişim belirteçlerini yenileme bakın.

## <a name="refresh-identity-provider-tokens"></a>Kimlik sağlayıcısı belirteçleri yenileme

Zaman sağlayıcınızın erişim belirteci (değil [Oturum belirteci](#extend-session-token-expiration-grace-period)) süresi, bu belirteci yeniden kullanmadan önce kullanıcının yeniden kimlik doğrulamaya zorlayabilir gerekir. Hale getirerek belirteç süresinin dolmasını engellemek bir `GET` çağrısı `/.auth/refresh` uygulamanızın uç noktası. Çağrıldığında, App Service, erişim belirteçleri belirteç deposundaki kimliği doğrulanmış kullanıcı için otomatik olarak yenilenir. Sonraki istekleri için belirteçleri, uygulama kodunuz ile yenilenmesini belirteç alın. Ancak, çalışma belirteç yenileme işlemi için belirteç deposu içermelidir [yenileme belirteçleri](https://auth0.com/learn/refresh-tokens/) sağlayıcınız için. Yenileme belirteçleri yolu her bir sağlayıcı tarafından belgelenen, ancak liste aşağıda kısa bir özeti verilmiştir:

- **Google**: Append bir `access_type=offline` sorgu dizesi parametresi, `/.auth/login/google` API çağrısı. Mobile Apps SDK'sı kullanıyorsanız, parametre birine ekleyebileceğiniz `LogicAsync` aşırı yüklemeler (bkz [Google yenileme belirteçleri](https://developers.google.com/identity/protocols/OpenIDConnect#refresh-tokens)).
- **Facebook**: Yenileme belirteçleri sağlamaz. Uzun süreli belirteçlerin süresi 60 gün içinde (bkz [Facebook zaman aşımı ve erişim belirteçleri uzantısı](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension)).
- **Twitter**: Erişim belirteçleri olmayan süresi (bkz [Twitter OAuth SSS](https://developer.twitter.com/en/docs/basics/authentication/FAQ)).
- **Microsoft hesabı**: Zaman [Microsoft hesabı kimlik doğrulama ayarları yapılandırma](configure-authentication-provider-microsoft.md)seçin `wl.offline_access` kapsam.
- **Azure Active Directory**: İçinde [ https://resources.azure.com ](https://resources.azure.com), aşağıdaki adımları uygulayın:
    1. Sayfanın üst kısmında seçin **okuma/yazma**.
    2. Sol tarayıcıda gidin **abonelikleri** > **_\<abonelik\_adı_**   >  **resourceGroups** > _**\<kaynak\_grubu\_adı >**_   >  **sağlayıcıları** > **Microsoft.Web** > **siteleri** > _**\<uygulama \_adı >**_ > **config** > **authsettings**. 
    3. **Düzenle**’ye tıklayın.
    4. Aşağıdaki özelliğini değiştirin. Değiştirin  _\<uygulama\_kimliği >_ erişmek istediğiniz hizmeti Azure Active Directory Uygulama kimliği.

        ```json
        "additionalLoginParams": ["response_type=code id_token", "resource=<app_id>"]
        ```

    5. Tıklayın **Put**. 

Sağlayıcınız yapılandırıldıktan sonra yapabilecekleriniz [yenileme belirtecini ve erişim belirteci süre sonu](#retrieve-tokens-in-app-code) belirteç deposundaki. 

Dilediğiniz zaman erişim belirtecinizi yenilemek için çağrı `/.auth/refresh` herhangi bir dilde. Aşağıdaki kod parçacığı jQuery JavaScript istemcisi erişim belirteçlerinizi yenilemek için kullanır.

```JavaScript
function refreshTokens() {
  let refreshUrl = "/.auth/refresh";
  $.ajax(refreshUrl) .done(function() {
    console.log("Token refresh completed successfully.");
  }) .fail(function() {
    console.log("Token refresh failed. See application logs for details.");
  });
}
```

Bir kullanıcı uygulamanıza, aramanız için verilen izinler iptal eder, `/.auth/me` ile başarısız olabilir bir `403 Forbidden` yanıt. Hataları tanılamak için Ayrıntılar için uygulama günlüklerini kontrol edin.

## <a name="extend-session-token-expiration-grace-period"></a>Oturum belirteci süre sonu yetkisiz kullanım süresi

Kimliği doğrulanan oturum 8 saat sonra süresi dolar. Kimliği doğrulanmış bir oturumun sona erdikten sonra varsayılan olarak 72 saat yetkisiz kullanım süresi yoktur. Bu yetkisiz kullanım süresi içinde kullanıcının yeniden kimlik doğrulaması gerçekleştirilmesi olmadan App Service ile oturum belirteci yenileme izin verilir. Yalnızca çağırabilirsiniz `/.auth/refresh` ne zaman, oturum belirteci geçersiz hale gelir ve belirteci süre sonu kendiniz izleyin gerekmez. 72 saat yetkisiz kullanım süresi kesildiyse olduktan sonra kullanıcının yeniden geçerli bir oturum belirteci almak oturum açmanız gerekir.

72 saat sizin için yeterli zaman yoksa, bu sona erme penceresinin genişletebilirsiniz. Uzun bir süre içinde sona erme genişletme (örneğin, ne zaman bir kimlik doğrulama belirteci sızdırıldıysa veya çalınırsa) önemli güvenlik etkileri olabilir. Bu nedenle 72 saat varsayılan olarak bırakın veya uzantı süresi en küçük değere ayarlayın.

Varsayılan sona erme penceresinin genişletmek için aşağıdaki komutu çalıştırın [Cloud Shell](../cloud-shell/overview.md).

```azurecli-interactive
az webapp auth update --resource-group <group_name> --name <app_name> --token-refresh-extension-hours <hours>
```

> [!NOTE]
> Yetkisiz kullanım süresi, yalnızca değil kimlik sağlayıcılardan gelen belirteçleri App Service kimlik doğrulaması oturumu için geçerlidir. Süresi dolan sağlayıcısı belirteçleri için yetkisiz kullanım süresi yoktur. 
>

## <a name="limit-the-domain-of-sign-in-accounts"></a>Oturum Aç hesapları etki alanını sınırla

Hem Microsoft Account hem de Azure Active Directory birden çok etki alanından oturum açmanızı sağlar. Örneğin, Microsoft Account sağlar _outlook.com_, _live.com_, ve _hotmail.com_ hesaplar. Azure Active Directory oturum açma hesapları için özel etki alanları herhangi bir sayıda sağlar. Bu davranış, kimseyle istemediğiniz bir iç uygulama için istenmeyen bir _outlook.com_ hesap erişim. Oturum Aç hesapları etki alanı adını sınırlandırmak için aşağıdaki adımları izleyin.

İçinde [ https://resources.azure.com ](https://resources.azure.com), gitmek **abonelikleri** > **_\<abonelik\_adı_**   >  **resourceGroups** > _**\<kaynak\_grubu\_adı >**_   >  **sağlayıcıları** > **Microsoft.Web** > **siteleri**  >    _**\<uygulama\_adı >**_ > **config** > **authsettings**. 

Tıklayın **Düzenle**aşağıdaki özelliğini değiştirin ve ardından **Put**. Değiştirdiğinizden emin olun  _\<etki alanı\_adı >_ istediğiniz etki alanı.

```json
"additionalLoginParams": ["domain_hint=<domain_name>"]
```
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Kimlik doğrulama ve kullanıcıları uçtan uca (Windows) yetkilendirme](app-service-web-tutorial-auth-aad.md)
> [Öğreticisi: Kimlik doğrulama ve yetkilendirme kullanıcılar için uçtan uca (Linux)](containers/tutorial-auth-aad.md)
