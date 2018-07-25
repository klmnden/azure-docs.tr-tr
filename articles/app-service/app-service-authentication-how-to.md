---
title: Kimlik doğrulama ve yetkilendirme Azure App Service'te özelleştirme | Microsoft Docs
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
ms.date: 03/14/2018
ms.author: cephalin
ms.openlocfilehash: 191d42f43e500c7f8041a02aeba2fbcb7dfd5379
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39226535"
---
# <a name="customize-authentication-and-authorization-in-azure-app-service"></a>Kimlik doğrulama ve yetkilendirme Azure App Service'te özelleştirme

Bu makalede nasıl özelleştirileceğini gösterir [kimlik doğrulama ve yetkilendirme App Service'te](app-service-authentication-overview.md), uygulamanızdan kimlik yönetmek için. 

Hızlıca kullanmaya başlamak için aşağıdaki öğreticilerden birine bakın:

* [Öğretici: Kimliğini doğrulama ve kullanıcıları uçtan uca (Windows) Azure App Service'te yetkilendirme](app-service-web-tutorial-auth-aad.md)
* [Öğretici: Kimliğini doğrulama ve kullanıcıları uçtan uca Azure App Service'te Linux için yetkilendirin](containers/tutorial-auth-aad.md)
* [Uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma](app-service-mobile-how-to-configure-active-directory-authentication.md)
* [Uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma](app-service-mobile-how-to-configure-facebook-authentication.md)
* [Uygulamanızı Google oturum açma bilgilerini kullanacak şekilde yapılandırma](app-service-mobile-how-to-configure-google-authentication.md)
* [Uygulamanızı Microsoft Hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma](app-service-mobile-how-to-configure-microsoft-authentication.md)
* [Uygulamanızı Twitter oturum açma bilgilerini kullanacak şekilde yapılandırma](app-service-mobile-how-to-configure-twitter-authentication.md)

## <a name="configure-multiple-sign-in-options"></a>Çoklu oturum açma seçeneklerini yapılandırın

Portal yapılandırma (örneğin, Facebook ve Twitter için), kullanıcılara çoklu oturum açma seçenekleri sunmak için kullanıma hazır bir yol sunmaz. Ancak, web uygulamanıza işlevsellik eklemek zor değildir. Aşağıda belirtilen adımları:

İlk olarak **kimlik doğrulama / yetkilendirme** sayfasında Azure Portalı'nda, etkinleştirmek istediğiniz kimlik sağlayıcısının her yapılandırın.

İçinde **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem**seçin **izin anonim istekler (eylem yok)**.

Oturum açma sayfası veya gezinti çubuğunu veya web uygulamanızın başka bir konuma bir oturum açma bağlantısı etkinleştirdiğiniz sağlayıcıların her birine Ekle (`/.auth/login/<provider>`). Örneğin:

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

## <a name="access-user-claims"></a>Erişim kullanıcı talepleri

App Service, kullanıcı talepleri uygulamanıza özel üst bilgileri kullanarak geçirir. Olmayan dış isteklerine izin mevcut olduğundan, bu üst bilgilerini ayarlayacak şekilde yalnızca App Service tarafından ayarlayın. Bazı örnek üst bilgiler şunları içerir:

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ID

Herhangi bir dil veya çerçeve yazılmış kod, bu üst bilgiler, gereken bilgileri alabilirsiniz. ASP.NET 4.6 uygulamalar için **ClaimsPrincipal** uygun değerlerle otomatik olarak ayarlanır.

Uygulamanızı Ayrıca kimliği doğrulanmış kullanıcı hakkında daha fazla ayrıntı çağırarak elde edebileceğiniz `/.auth/me`. Mobil uygulamalar sunucusu SDK'ları bu verilerle çalışmak için yardımcı yöntemler sağlar. Daha fazla bilgi için [nasıl Azure Mobile Apps Node.js SDK'sı kullanılacağını](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), ve [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="retrieve-tokens-in-app-code"></a>Uygulama kodunda belirteçleri alma

Sunucu kodunuzdan kolayca erişebilmeleri için sağlayıcıya özel belirteçler isteği üstbilgisine eklenmiş. Aşağıdaki tabloda olası belirteci üst bilgi adları gösterilmektedir:

| | |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Facebook belirteci | `X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN` <br/> `X-MS-TOKEN-FACEBOOK-EXPIRES-ON` |
| Google | `X-MS-TOKEN-GOOGLE-ID-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-ACCESS-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-EXPIRES-ON` <br/> `X-MS-TOKEN-GOOGLE-REFRESH-TOKEN` |
| Microsoft Hesabı | `X-MS-TOKEN-MICROSOFTACCOUNT-ACCESS-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-EXPIRES-ON` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-AUTHENTICATION-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-REFRESH-TOKEN` |
| Twitter | `X-MS-TOKEN-TWITTER-ACCESS-TOKEN` <br/> `X-MS-TOKEN-TWITTER-ACCESS-TOKEN-SECRET` |
|||

İstemci kodunuz içinden (örneğin, bir mobil uygulama veya tarayıcı içinde JavaScript), bir HTTP gönderme `GET` isteği `/.auth/me`. Döndürülen JSON sağlayıcıya özgü belirteçleri sahiptir.

> [!NOTE]
> Erişim belirteci sağlayıcısı kaynakları bir gizli anahtar ile sağlayıcınız yapılandırırsanız mevcut şekilde erişmek için ' dir. Yenileme belirteçlerini almak hakkında bilgi için bkz. [erişim belirteçlerini yenileme](#refresh-access-tokens).

## <a name="refresh-access-tokens"></a>Erişim belirteci yenileyin

Sağlayıcınızın erişim belirtecinin süresi dolduğunda, kullanıcının yeniden kimlik doğrulamaya zorlayabilir gerekir. Hale getirerek belirteç süresinin dolmasını engellemek bir `GET` çağrısı `/.auth/refresh` uygulamanızın uç noktası. Çağrıldığında, App Service, erişim belirteçleri belirteç deposundaki kimliği doğrulanmış kullanıcı için otomatik olarak yenilenir. Sonraki istekleri için belirteçleri, uygulama kodunuz ile yenilenmesini belirteç alın. Ancak, çalışma belirteç yenileme işlemi için belirteç deposu içermelidir [yenileme belirteçleri](https://auth0.com/learn/refresh-tokens/) sağlayıcınız için. Yenileme belirteçleri yolu her bir sağlayıcı tarafından belgelenen, ancak liste aşağıda kısa bir özeti verilmiştir:

- **Google**: ekleme bir `access_type=offline` sorgu dizesi parametresi, `/.auth/login/google` API çağrısı. Mobile Apps SDK'sı kullanıyorsanız, parametre birine ekleyebileceğiniz `LogicAsync` aşırı yüklemeler (bkz [Google yenileme belirteçleri](https://developers.google.com/identity/protocols/OpenIDConnect#refresh-tokens)).
- **Facebook**: yenileme belirteçleri sağlamaz. Uzun süreli belirteçlerin süresi 60 gün içinde (bkz [Facebook zaman aşımı ve erişim belirteçleri uzantısı](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension)).
- **Twitter**: olmayan erişim belirteçlerin süresi (bkz [Twitter OAuth SSS](https://developer.twitter.com/en/docs/basics/authentication/guides/oauth-faq)).
- **Microsoft Account**: zaman [Microsoft hesabı kimlik doğrulama ayarları yapılandırma](app-service-mobile-how-to-configure-microsoft-authentication.md)seçin `wl.offline_access` kapsam.
- **Azure Active Directory**: içinde [ https://resources.azure.com ](https://resources.azure.com), aşağıdaki adımları uygulayın:
    1. Sayfanın üst kısmında seçin **okuma/yazma**.
    1. Sol tarayıcıda gidin **abonelikleri** > **_\<abonelik\_adı_**   >  **resourceGroups** > _**\<kaynak\_grubu\_adı >**_   >  **sağlayıcıları** > **Microsoft.Web** > **siteleri** > _**\<uygulama \_adı >**_ > **config** > **authsettings**. 
    1. **Düzenle**’ye tıklayın.
    1. Aşağıdaki özelliğini değiştirin. Değiştirin  _\<uygulama\_kimliği >_ erişmek istediğiniz hizmeti Azure Active Directory Uygulama kimliği.

        ```json
        "additionalLoginParams": ["response_type=code id_token", "resource=<app_id>"]
        ```

    1. Tıklayın **Put**. 

Sağlayıcınız yapılandırıldıktan sonra yapabilecekleriniz [yenileme belirtecini ve erişim belirteci süre sonu](#retrieve-tokens-in-app-code) belirteç deposundaki. 

Dilediğiniz zaman erişim belirtecinizi yenilemek için çağrı `/.auth/refresh` herhangi bir dilde. Aşağıdaki kod parçacığı jQuery JavaScript istemcisi erişim belirteçlerinizi yenilemek için kullanır.

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

Bir kullanıcı uygulamanıza, aramanız için verilen izinler iptal eder, `/.auth/me` ile başarısız olabilir bir `403 Forbidden` yanıt. Hataları tanılamak için Ayrıntılar için uygulama günlüklerini kontrol edin.

## <a name="extend-session-expiration-grace-period"></a>Oturum sona erme yetkisiz kullanım süresi

Kimliği doğrulanmış bir oturumun sona erdikten sonra varsayılan olarak 72 saat yetkisiz kullanım süresi yoktur. Bu yetkisiz kullanım süresi içinde kullanıcının yeniden kimlik doğrulaması gerçekleştirilmesi olmadan oturum tanımlama bilgisi veya App Service ile oturum belirteci yenileme izin verilir. Yalnızca çağırabilirsiniz `/.auth/refresh` ne zaman oturum tanımlama bilgisi veya oturum belirteci geçersiz hale gelir ve belirteci süre sonu kendiniz izleyin gerekmez. 72 saat yetkisiz kullanım süresi kesildiyse olduktan sonra kullanıcının yeniden geçerli bir oturum tanımlama bilgisi veya oturum belirteci almak oturum açmanız gerekir.

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
> [Öğretici: Kimlik doğrulama ve kullanıcıları uçtan uca (Windows)'ı yetkilendirme](app-service-web-tutorial-auth-aad.md)
> [Öğreticisi: kimlik doğrulama ve yetkilendirme kullanıcılar için uçtan uca (Linux)](containers/tutorial-auth-aad.md)