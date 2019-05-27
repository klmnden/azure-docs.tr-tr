---
title: Kimlik doğrulaması ve yetkilendirme için Azure AD ile tümleştirilen bir .NET web API'si oluşturma | Microsoft Docs
description: Kimlik doğrulaması ve yetkilendirme için Azure AD ile tümleştirilen bir .NET MVC web API'si nasıl oluşturulur?
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/21/2019
ms.author: ryanwi
ms.reviewer: jmprieur, andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5e2eca253bc5d1495d26506e0e6f8a83762e8bc5
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66001100"
---
# <a name="quickstart-build-a-net-web-api-that-integrates-with-azure-ad-for-authentication-and-authorization"></a>Hızlı Başlangıç: .NET web API'si Azure AD kimlik doğrulaması ve yetkilendirme ile tümleşen oluşturma

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

Korumalı kaynaklara erişim sağlayan bir uygulama derliyorsanız, söz konusu kaynaklara istenmeyen erişimi nasıl önleyeceğinizi bilmeniz gerekir. Azure Active Directory (Azure AD), yalnızca birkaç kod satırıyla OAuth 2.0 taşıyıcı erişim belirteçleri kullanarak web API'sini korumayı basitleştirir ve rahatlatır.

ASP.NET web uygulamalarında, bu korumayı .NET Framework 4.5'e eklenmiş topluluk tarafından yönlendirilen OWIN ara yazılımının Microsoft uyarlamasını kullanarak gerçekleştirebilirsiniz. Burada, OWIN kullanarak aşağıdakileri gerçekleştiren bir "To Do List" web API'si oluşturacağız:

* Hangi API'lerin korunacağını belirleme.
* Web API'si çağrılarının geçerli bir erişim belirteci içerdiğini doğrulama.

Bu hızlı başlangıçta, To Do List API'sini oluşturacak ve şunları yapmayı öğreneceksiniz:

1. Bir uygulamayı Azure AD'ye kaydedin.
2. Uygulamayı OWIN kimlik doğrulaması işlem hattını kullanacak şekilde ayarlama.
3. Web API'sini çağırmak için bir istemci uygulaması yapılandırma.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için şu önkoşulları tamamlayın:

* [Uygulama çatısını indirin](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) veya [tamamlanmış örneği indirin](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Bunların hepsi Visual Studio 2013 çözümüdür.
* Uygulamanızı kaydedeceğiniz bir Azure AD kiracınız olmalıdır. Henüz kiracınız yoksa, [nasıl alacağınızı öğrenin](quickstart-create-new-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>1. Adım: Bir uygulamayı Azure AD'ye kaydetme

Uygulamanızın güvenliğini sağlamaya yardımcı olmak için, ilk olarak kiracınızda bir uygulama oluşturmalı ve Azure AD'de birkaç önemli bilgi parçası sağlamalısınız.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sayfanın sağ üst köşesinden hesabınızı seçtikten sonra **Dizin değiştir** gezintisini seçerek ve sonra da uygun kiracıyı belirterek Azure AD kiracınızı seçin.
    * Hesabınızın altında tek bir Azure AD kiracısı varsa veya uygun Azure AD kiracısını zaten seçtiyseniz, bu adımı atlayın.

3. Sol taraftaki gezinti bölmesinde **Azure Active Directory**'yi seçin.
4. Seçin **uygulama kayıtları**ve ardından **yeni kayıt**.
5. Zaman **bir uygulamayı kaydetme** sayfası görüntülenirse, uygulamanız için bir ad girin.
Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**.
6. Seçin **Web** platform altında **yeniden yönlendirme URI'si** bölümünde ve değerine `https://localhost:44321/` (istediğiniz Azure AD belirteçleri döndürecektir konum).
7. Bittiğinde **Kaydet**’i seçin. Uygulamasında **genel bakış** sayfa, Not **uygulama (istemci) kimliği** değeri.
6. Seçin **bir API'yi kullanıma sunmak**, ardından tıklayarak uygulama kimliği URI'si güncelleştirin **ayarlamak**. Kiracıya özel bir tanımlayıcı girin. Örneğin, `https://contoso.onmicrosoft.com/TodoListService` girin.
7. Yapılandırmayı kaydedin. Portalı açık bırakın, çünkü kısa süre sonra istemcinizi de kaydetmeniz gerekecektir.

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>2. Adım: OWIN kimlik doğrulaması işlem hattı kullanılacak uygulamasını ayarlama

Gelen istekleri ve belirteçleri doğrulamak için, uygulamanızı Azure AD ile iletişim kuracak şekilde ayarlamanız gerekir.

1. Başlangıç olarak, çözümü açın ve Paket Yöneticisi Konsolu'nu kullanarak TodoListService projesine OWIN ara yazılımı NuGet paketlerini ekleyin.

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. TodoListService projesine `Startup.cs` adlı bir OWIN Startup sınıfı ekleyin.  Projeye sağ tıklayın, **Ekle > Yeni Öğe**'yi seçin, ardından **OWIN**'i aratın. OWIN ara yazılımı, uygulamanız başlatıldığında `Configuration(…)` yöntemini çağırır.

3. Sınıf bildirimini `public partial class Startup` olarak değiştirin. Başka bir dosyada bu sınıfın bir kısmını sizin için zaten uygulamıştır. `Configuration(…)` yönteminde, web uygulamanızda kimlik doğrulamasını ayarlamak için `ConfgureAuth(…)` çağrısı yapın.

    ```csharp
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. `App_Start\Startup.Auth.cs` dosyasını açın ve `ConfigureAuth(…)` yöntemini uygulayın. `WindowsAzureActiveDirectoryBearerAuthenticationOptions` içinde sağladığınız parametreler, uygulamanızın Azure AD ile iletişim kurması için koordinat işlevi görecektir. Bunları kullanmak için sınıfları kullanma gerekecektir `System.IdentityModel.Tokens` ad alanı.

    ```csharp
    using System.IdentityModel.Tokens;
    ```

    ```csharp
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                 Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
                 TokenValidationParameters = new TokenValidationParameters
                 {
                    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"]
                 }
            });
    }
    ```

5. Denetleyicilerinizi ve eylemlerinizi JSON Web Token (JWT) taşıyıcı kimlik doğrulamasıyla korumaya yardımcı olmak için `[Authorize]` özniteliklerini kullanın. `Controllers\TodoListController.cs` sınıfına bir yetkilendirme etiketi ekleyin; bu etiket kullanıcıyı bu sayfaya erişmeden önce oturum açmaya zorlar.

    ```csharp
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    Yetkili bir kullanıcı `TodoListController` API'lerinden birini başarıyla çağırdığında, eylemin çağrıyı yapanla ilgili bilgilere erişmesi gerekebilir. OWIN, `ClaimsPrincpal` nesnesi yoluyla taşıyıcı belirtecinin içindeki taleplere erişim sağlar.  

6. Web API'leriyle ilgili yaygın bir gereksinim de, kullanıcının To Do List Service uygulamasına erişmek için gereken izinlere onay verdiğinden emin olmak üzere belirteçte bulunan "kapsamların" doğrulanmasıdır.

    ```csharp
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is the default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. TodoListService projesinin kökündeki `web.config` dosyasını açın ve `<appSettings>` bölümüne yapılandırma değerlerinizi girin.
    * `ida:Tenant`, Azure AD kiracınızın adıdır (örneğin, contoso.onmicrosoft.com).
    * `ida:Audience`, Azure portalına girdiğiniz uygulamanın Uygulama Kimliği URI'sidir.

## <a name="step-3-configure-a-client-application-and-run-the-service"></a>3. adım: Bir istemci uygulaması yapılandırma ve hizmet çalıştırma

To Do List Service'i iş başında görebilmek için önce To Do List istemcisini Azure AD'den belirteçleri alacak ve hizmete çağrı yapabilecek şekilde yapılandırmanız gerekir.

1. [Azure portalına](https://portal.azure.com) geri dönün.
1. Azure AD kiracınıza yeni bir uygulama kaydı oluşturun.  Girin bir **adı** girin, uygulamanızı kullanıcılara açıklayan `http://TodoListClient/` için **yeniden yönlendirme URI'si** değeri ve seçin **genel istemci (Mobil ve Masaüstü)** içinde açılır.
1. Kaydı bitirdikten sonra, Azure AD uygulamanıza benzersiz bir uygulama kimliği atar. Bu değeri sonraki adımlarda kullanacağınız için, uygulama sayfasından kopyalayın.
1. Seçin **API izinleri**, ardından **bir izin eklemek**.  Bulun ve için yapmak listesi hizmeti seçin, ekleyin **user_impersonation erişim TodoListService** altında izin **Temsilcili izinler**ve ardından **izinlerieklemek**.
1. Visual Studio'da, TodoListClient projesinde `App.config` dosyasını açın ve `<appSettings>` bölümüne kendi yapılandırma değerlerinizi girin.

    * `ida:Tenant`, Azure AD kiracınızın adıdır (örneğin, contoso.onmicrosoft.com).
    * `ida:ClientId`, Azure portalından kopyaladığınız uygulama kimliğidir.
    * `todo:TodoListResourceId`, Azure portalına girdiğiniz To Do List Service uygulamasının Uygulama Kimliği URI'sidir.

1. Her projeyi temizleyin, derleyin ve çalıştırın.
1. Henüz yapmadıysanız, kiracınızda *.onmicrosoft.com etki alanıyla yeni bir kullanıcı oluşturun.
1. Bu kullanıcıyla To Do List istemcisinde oturum açın ve kullanıcının yapılacaklar listesine birkaç görev ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Tamamlanan örneği, başvuru için (yapılandırma değerleriniz olmadan) [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip)'dan indirin. Artık diğer kimlik senaryolarına ilerleyebilirsiniz.
