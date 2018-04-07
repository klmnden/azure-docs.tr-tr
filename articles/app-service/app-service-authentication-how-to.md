---
title: Kimlik doğrulama ve yetkilendirme Azure App Service'te özelleştirme | Microsoft Docs
description: Kimlik doğrulama ve yetkilendirme App Service'te özelleştirmek ve kullanıcı talepleri ve farklı belirteçleri almak nasıl gösterir.
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
ms.date: 03/14/2018
ms.author: cephalin
ms.openlocfilehash: 10decbd5291e2054e373bfef266b64eae36ea1cf
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="customize-authentication-and-authorization-in-azure-app-service"></a>Kimlik doğrulama ve yetkilendirme Azure App Service'te özelleştirme

Bu makalede nasıl özelleştirileceğini gösteren [kimlik doğrulama ve yetkilendirme App Service'te](app-service-authentication-overview.md)ve kimlik uygulamanızdan yönetmek için. 

Hızlıca başlamak için şu eğitimlerden birine bakın:

* [Öğretici: Kimlik doğrulaması ve kullanıcıların uçtan uca Azure App Service'te yetkilendirme](app-service-web-tutorial-auth-aad.md)
* [Uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma](app-service-mobile-how-to-configure-active-directory-authentication.md)
* [Uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma](app-service-mobile-how-to-configure-facebook-authentication.md)
* [Uygulamanızı Google oturum açma bilgilerini kullanacak şekilde yapılandırma](app-service-mobile-how-to-configure-google-authentication.md)
* [Uygulamanızı Microsoft Hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma](app-service-mobile-how-to-configure-microsoft-authentication.md)
* [Uygulamanızı Twitter oturum açma bilgilerini kullanacak şekilde yapılandırma](app-service-mobile-how-to-configure-twitter-authentication.md)

## <a name="configure-multiple-sign-in-options"></a>Çoklu oturum açma seçeneklerini yapılandırma

Portal yapılandırma (örneğin, Facebook ve Twitter için), kullanıcılara çoklu oturum açma seçenekleri sunmak için bir anahtar teslim yol sunmaz. Ancak, web uygulamanıza işlevsellik ekleme zor değildir. Adımlar aşağıda belirtilen:

İlk olarak **kimlik doğrulama / yetkilendirme** sayfasında Azure Portalı'nda, her etkinleştirmek istediğiniz kimlik sağlayıcısı yapılandırın.

İçinde **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem**seçin **izin anonim istekler (hiçbir işlem)**.

Oturum açma sayfası veya gezinti çubuğunda veya başka bir konuma, web uygulamanızın her etkinleştirdiğiniz sağlayıcıları için oturum açma bağlantı eklemek (`/.auth/login/<provider>`). Örneğin:

```HTML
<a href="/.auth/login/aad">Log in with Azure AD</a>
<a href="/.auth/login/microsoft">Log in with Microsoft Account</a> 
<a href="/.auth/login/facebook">Log in with Facebook</a>
<a href="/.auth/login/google">Log in with Google</a>
<a href="/.auth/login/twitter">Log in with Twitter</a>
```

Kullanıcı bağlantılardan birini tıkladığında, kullanıcıyla oturum açmak için ilgili oturum açma sayfası açılır.

## <a name="access-user-claims"></a>Access kullanıcı talepleri

Uygulama hizmeti özel üstbilgileri kullanarak uygulamanız için kullanıcı talepleri geçirir. Dış istekleri izin verilmiyor mevcut; bu nedenle bu üstbilgileri ayarlamak için yalnızca App Service tarafından ayarlayın. Bazı örnek üst bilgiler içerir:

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ID

Tüm dil veya framework ile yazılan kod, gereken bilgileri bu üstbilgileri elde edebilirsiniz. ASP.NET 4.6 uygulamalar için **ClaimsPrincipal** uygun değerlerle otomatik olarak ayarlanır.

Uygulamanızı Ayrıca kimliği doğrulanmış kullanıcının hakkında daha fazla ayrıntı çağırarak elde edebileceğiniz `/.auth/me`. Mobil uygulamalar sunucusu SDK'ları bu verilerle çalışmak için yardımcı yöntemler sağlar. Daha fazla bilgi için bkz: [nasıl Azure Mobile Apps Node.js SDK'ın](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), ve [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="retrieve-tokens-in-app-code"></a>Uygulama kodu belirteçleri almak

Bunları kolayca erişebilmesi için sunucu kodunuzdan isteği üstbilgisine sağlayıcıya özgü belirteçleri eklenmiş. Aşağıdaki tabloda olası belirteci üstbilgi adlarını gösterir:

| | |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Facebook simgesi | `X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN` <br/> `X-MS-TOKEN-FACEBOOK-EXPIRES-ON` |
| Google | `X-MS-TOKEN-GOOGLE-ID-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-ACCESS-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-EXPIRES-ON` <br/> `X-MS-TOKEN-GOOGLE-REFRESH-TOKEN` |
| Microsoft Hesabı | `X-MS-TOKEN-MICROSOFTACCOUNT-ACCESS-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-EXPIRES-ON` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-AUTHENTICATION-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-REFRESH-TOKEN` |
| Twitter | `X-MS-TOKEN-TWITTER-ACCESS-TOKEN` <br/> `X-MS-TOKEN-TWITTER-ACCESS-TOKEN-SECRET` |
|||

İstemci kodunuzdan (örneğin, bir mobil uygulama veya tarayıcı içi JavaScript), bir HTTP gönderme `GET` isteği `/.auth/me`. Döndürülen JSON sağlayıcıya özgü belirteçleri sahiptir.

> [!NOTE]
> Erişim belirteçleri, bir istemci parolası ile sağlayıcınız yapılandırırsanız mevcut; bu nedenle sağlayıcısı kaynaklara erişmek için ' dir. Yenileme belirteçleri alınacağı hakkında bilgi için bkz: [yenileme erişim belirteçleri](#refresh-access-tokens).

## <a name="refresh-access-tokens"></a>Erişim belirteçleri Yenile

Sağlayıcınızın erişim belirtecinin süresi dolduğunda, kullanıcının yeniden kimlik doğrulamaya gerekir. Yaparak belirteci süre sonu önleyebilirsiniz bir `GET` çağrısı `/.auth/refresh` uygulamanızın uç noktası. Çağrıldığında, uygulama hizmeti kimliği doğrulanmış kullanıcı için belirteç deposundaki erişim belirteçleri otomatik olarak yenilenir. Uygulama kodu tarafından belirteçleri sonraki istekleri yenilendi belirteçleri alın. Çalışmak belirteç yenileme için belirteç deposu ancak içermelidir [yenileme belirteçleri](https://auth0.com/learn/refresh-tokens/) sağlayıcınız için. Yenileme belirteçleri almak için yöntem, her sağlayıcı tarafından belgelenmiştir, ancak kısa bir özeti liste aşağıda verilmiştir:

- **Google**: Append bir `access_type=offline` sorgu dizesi parametresi, `/.auth/login/google` API çağrısı. Mobile Apps SDK'sı kullanıyorsanız, parametre birine ekleyebileceğiniz `LogicAsync` aşırı (bkz [Google yenileme belirteçleri](https://developers.google.com/identity/protocols/OpenIDConnect#refresh-tokens)).
- **Facebook**: yenileme belirteçleri sağlamaz. Uzun süreli belirteçler 60 gün içinde sona (bkz [Facebook geçerlilik süresi ve erişim belirteçleri uzantısı](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension)).
- **Twitter**: erişim belirteçleri verme süresi (bkz [Twitter OAuth SSS](https://developer.twitter.com/docs/basics/authentication/guides/oauth-faq)).
- **Microsoft Account**: zaman [Microsoft hesap kimlik doğrulama ayarlarını yapılandırma](app-service-mobile-how-to-configure-microsoft-authentication.md)seçin `wl.offline_access` kapsam.
- **Azure Active Directory**: içinde [ https://resources.azure.com ](https://resources.azure.com), aşağıdaki adımları uygulayın:
    1. Sayfanın en üstünde seçin **okuma/yazma**.
    1. Sol tarayıcıda gidin **abonelikleri** > **_\<abonelik\_adı_**   >  **resourceGroups** > _**\<kaynak\_grup\_adı >**_   >  **sağlayıcıları** > **Microsoft.Web** > **siteleri** > _**\<uygulama \_adı >**_ > **config** > **authsettings**. 
    1. **Düzenle**’ye tıklayın.
    1. Aşağıdaki özelliğini değiştirin. Değiştir  _\<uygulama\_kimliği >_ erişmek istediğiniz hizmet Azure Active Directory Uygulama kimliği.

        ```json
        "additionalLoginParams": ["response_type=code id_token", "resource=<app_id>"]
        ```

    1. Tıklatın **Put**. 

Sağlayıcınız yapılandırıldıktan sonra yenileme belirteçleri çağırarak belirteci deposunda olup olmadığını görebilirsiniz `/.auth/me`. 

Erişim belirteci zaman yenilemek için yalnızca çağrı `/.auth/refresh` herhangi bir dilde. Aşağıdaki kod parçacığında jQuery JavaScript istemcisinden, erişim belirteçleri yenilemek için kullanır.

```JavaScript
function refreshTokens() {
  var refreshUrl = "/.auth/refresh";
  $.ajax(refreshUrl) .done(function() {
    console.log("Token refresh completed successfully.");
  }) .fail(function() {
    console.log("Token refresh failed. See application logs for details.");
  });
}
```

Bir kullanıcı uygulamanıza aramanız için verilen izinleri iptal eder, `/.auth/me` ile başarısız olabilir bir `403 Forbidden` yanıt. Hataları tanılamak için Ayrıntılar için uygulama günlüklerini denetleyin.

## <a name="extend-session-expiration-grace-period"></a>Oturum sona erme yetkisiz kullanım süresi

Kimliği doğrulanmış bir oturumun sona erdikten sonra varsayılan olarak 72 saat yetkisiz kullanım süresi yoktur. Bu yetkisiz kullanım süresi içinde kullanıcı yeniden doğrularken olmadan oturum tanımlama bilgisi veya uygulama hizmeti ile oturum belirteci yenilemeye izin. Yalnızca çağırabilirsiniz `/.auth/refresh` ne zaman oturum tanımlama bilgisi veya oturum belirteci geçersiz hale gelir ve belirteç süre sonu kendiniz izlemenize gerek yoktur. 72 saat yetkisiz kullanım süresi geçene olduktan sonra kullanıcının yeniden geçerli bir oturum tanımlama bilgisi veya oturum belirteci almak kaydolmalısınız.

72 saat yeterli zamanı değilse, bu süre sonu pencere genişletebilirsiniz. Uzun bir süre içinde sona erme genişletme (örneğin, ne zaman bir kimlik doğrulama belirteci sızmasını veya çalınırsa) önemli güvenlik etkileri olabilir. Bu nedenle 72 saat varsayılan olarak bırakın veya uzantı süresi en küçük değere ayarlayın.

Varsayılan süre sonu penceresini genişletmek için aşağıdaki komutu çalıştırın [bulut Kabuk](../cloud-shell/overview.md).

```azurecli-interactive
az webapp auth update --resource-group <group_name> --name <app_name> --token-refresh-extension-hours <hours>
```

> [!NOTE]
> Yetkisiz kullanım süresi yalnızca uygulama hizmeti kimliği doğrulanmış oturum olmayan kimlik sağlayıcılardan gelen belirteçleri geçerlidir. Süresi dolan sağlayıcısı belirteçleri için hiçbir yetkisiz kullanım süresi yoktur. 
>

## <a name="limit-the-domain-of-sign-in-accounts"></a>Oturum açma hesapları etki alanını sınırla

Microsoft Account ve Azure Active Directory birden çok etki alanından oturum açmanızı sağlar. Örneğin, Microsoft Account sağlar _outlook.com_, _live.com_, ve _hotmail.com_ hesaplar. Azure Active Directory oturum açma hesapları için özel etki alanlarını herhangi bir sayıda sağlar. Bu davranış kimseyle istemediğiniz bir iç uygulama için istenmeyen bir _outlook.com_ hesap erişim. Oturum açma hesapları etki alanı adını sınırlandırmak için aşağıdaki adımları izleyin.

İçinde [ https://resources.azure.com ](https://resources.azure.com), gitmek **abonelikleri** > **_\<abonelik\_adı_**   >  **resourceGroups** > _**\<kaynak\_grup\_adı >**_   >  **sağlayıcıları** > **Microsoft.Web** > **siteleri**  >    _**\<uygulama\_adı >**_ > **config** > **authsettings**. 

Tıklatın **Düzenle**, aşağıdaki özelliğini değiştirin ve ardından **Put**. Değiştirdiğinizden emin olun  _\<etki alanı\_adı >_ istediğiniz etki alanı ile.

```json
"additionalLoginParams": ["domain_hint=<domain_name>"]
```
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Kimlik doğrulaması ve kullanıcıların uçtan uca yetkilendirme](app-service-web-tutorial-auth-aad.md)
