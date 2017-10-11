---
title: "Azure AD .NET web API'si Başlarken | Microsoft Docs"
description: ".NET MVC web kimlik doğrulama ve yetkilendirme için Azure AD ile tümleşir API'si oluşturma."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: f44d75f45073a5d9aa9b1863ed227aba4efcf785
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="help-protect-a-web-api-by-using-bearer-tokens-from-azure-ad"></a>Azure AD'den taşıyıcı belirteçlerini kullanarak bir web API korunmasına yardımcı olma
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Korunan kaynaklara erişim sağlayan bir uygulama oluşturuyorsanız, bu kaynakları unwarranted erişimi engellemek nasıl bilmeniz gerekir.
Basit ve basit bir web API OAuth 2.0 taşıyıcı erişimi kullanarak korunmasına yardımcı olmak yalnızca birkaç kod satırıyla belirteçler azure Active Directory (Azure AD) yapar.

ASP.NET web uygulamalarında .NET Framework 4. 5 ' bulunan topluluk odaklı OWIN ara yazılımı Microsoft uyarlamasını kullanarak bu koruma gerçekleştirebilirsiniz. OWIN için burada kullanacağız bir "Yapılacaklar listesi" web API'si oluşturma:

* API olan atayan korumalı.
* Web API çağrıları geçerli erişim belirteci içeren doğrular.

Yapmak listesi API'sine oluşturmak için önce gerekir:

1. Bir uygulamayı Azure AD'ye kaydedin.
2. OWIN kimlik doğrulaması ardışık kullanmak için uygulama ayarlama.
3. Web API'sini çağırmak için bir istemci uygulaması yapılandırın.

Başlamak için [uygulama çatıyı indirmeniz](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) veya [tamamlanan örnek indirme](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Her bir Visual Studio 2013 çözümüdür. Ayrıca, uygulamanızın kaydedileceği Azure AD kiracısı gerekir. Zaten yoksa, [edinebileceğinizi öğrenin](active-directory-howto-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>1. adım: bir uygulamayı Azure AD ile kaydedin.
Güvenliğini sağlamaya yardımcı olmak için uygulamanızın, önce kiracınızda bir uygulama oluşturun ve birkaç temel bilgiler ile Azure AD sağlamak gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Üst çubuğunda hesabınızı tıklatın. İçinde **Directory** listesinde, Azure AD Kiracı uygulamanızı kaydetmek istediğiniz yeri seçin.

3. Tıklatın **daha Hizmetleri** sol bölmesinde ve seçip **Azure Active Directory**.

4. Tıklatın **uygulama kayıtlar**ve ardından **Ekle**.

5. Komut istemlerini izleyin ve yeni bir **Web uygulaması ve/veya Web API**.
  * **Ad** kullanıcılar uygulamanıza açıklar. Girin **hizmet listelemek için**.
  * **Yeniden yönlendirme URI'si** uygulamanızı istedi herhangi bir belirtece döndürmek için Azure AD kullanır ve dize şeması bir birleşimidir. Girin `https://localhost:44321/` bu değer için.

6. Gelen **ayarları** -> **özellikleri** sayfasında uygulamanız için uygulama kimliği URI'si güncelleştirin. Kiracı özgü bir tanımlayıcı girin. Örneğin, `https://contoso.onmicrosoft.com/TodoListService` girin.

7. Yapılandırmayı kaydedin. Ayrıca istemci uygulamanızın kısa bir süre sonra kaydetmeniz gerekir çünkü portal kapatmayın.

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>2. adım: uygulama OWIN kimlik doğrulaması ardışık kullanmak için ayarlama.
Gelen istekleri ve belirteçleri doğrulamak için uygulamanızı Azure AD ile iletişim kurmak için ayarlamanız gerekir.

1. Başlamak için çözümü açın ve OWIN ara yazılımı NuGet paketleri Paket Yöneticisi konsolu kullanılarak TodoListService projeye ekleyin.

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. Adlı TodoListService projesine OWIN başlangıç sınıfı ekleyin `Startup.cs`.  Projeye sağ tıklayın, **Ekle** > **yeni öğe**, arayın ve sonra **OWIN**. OWIN ara yazılımı, uygulamanız başlatıldığında `Configuration(…)` yöntemini çağırır.

3. Sınıf bildirimi değiştirme `public partial class Startup`. Zaten bu sınıfın parçası sizin için başka bir dosyaya uyguladık. İçinde `Configuration(…)` yöntemi çağrısına duruma `ConfgureAuth(…)` web uygulamanız için kimlik doğrulaması ayarlamak için.

    ```C#
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. Dosyayı açmak `App_Start\Startup.Auth.cs` ve uygulamanıza `ConfigureAuth(…)` yöntemi. Sağladığınız parametreler `WindowsAzureActiveDirectoryBearerAuthenticationOptions` uygulamanız için Azure AD ile iletişim kurmak için koordinatları olarak hizmet verecektir.

    ```C#
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
            });
    }
    ```

5. Kullanabileceğiniz artık `[Authorize]` denetleyicileri ve eylemleri JSON Web Token (JWT) taşıyıcı kimlik doğrulaması ile korumaya yardımcı olmak için öznitelikler. İşaretleme `Controllers\TodoListController.cs` bir authorize etiketiyle sınıfı. Bu sayfayı erişmeden önce oturum açmak için kullanıcının zorlar.

    ```C#
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    Ne zaman bir yetkili çağıran başarıyla çağırır birini `TodoListController` API'leri, eylem çağıran hakkında bilgilere erişimi gerekebilir. OWIN taşıyıcı belirteci içinde talep erişim sağlar `ClaimsPrincpal` nesnesi.  

6. Web API’lerine yönelik genel bir gereksinim, belirteçteki mevcut "kapsamların" doğrulanmasıdır. Bu kullanıcı yapmak listesi hizmete erişmek için gerekli izinleri seçtiği sağlar.

    ```C#
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

7. Açık `web.config` dosya TodoListService proje kök dizininde ve yapılandırma değerlerinizi girin `<appSettings>` bölümü.
  * `ida:Tenant`Azure AD kiracınıza--Örneğin, contoso.onmicrosoft.com adıdır.
  * `ida:Audience`Azure portalında girdiğiniz uygulamanın uygulama kimliği URI'si değil.

## <a name="step-3-configure-a-client-application-and-run-the-service"></a>3. adım: bir istemci uygulamasını yapılandırma ve hizmet çalıştırma
İçin yapmak listesi hizmetinde eylem görebilmeniz için öncelikle Azure AD'den belirteçleri almak ve hizmet çağrı yapmak için yapılacaklar listesi istemci yapılandırmanız gerekir.

1. Geri dönerek [Azure portal](https://portal.azure.com).

2. Azure AD kiracınızda yeni bir uygulama oluşturun ve seçin **yerel istemci uygulaması** elde edilen istemi.
  * **Ad** kullanıcılar uygulamanıza açıklar.
  * Girin `http://TodoListClient/` için **yeniden yönlendirme URI'si** değeri.

3. Kayıt işlemini tamamladıktan sonra Azure AD benzersiz uygulama kimliği uygulamanıza atar. Bu değer gerekir sonraki adımlarda bu nedenle uygulama sayfasından kopyalayın.

4. Gelen **ayarları** sayfasında, **gerekli izinler**ve ardından **Ekle**. Bulun ve için yapmak listesi hizmeti seçin, eklemeniz **erişim TodoListService** altında izni **izinlere temsilci**ve ardından **Bitti**.

5. Visual Studio'da açın `App.config` TodoListClient içinde proje ve yapılandırma değerleriniz enter `<appSettings>` bölümü.

  * `ida:Tenant`Azure AD kiracınıza--Örneğin, contoso.onmicrosoft.com adıdır.
  * `ida:ClientId`Azure portalından kopyalandığından uygulama kimliğidir.
  * `todo:TodoListResourceId`Azure portalında girdiğiniz listesi hizmeti yapmak için uygulamanın uygulama kimliği URI'si değil.

## <a name="next-steps"></a>Sonraki adımlar
Son olarak, temiz, yapı ve her proje çalıştırın. Henüz yapmadıysanız, şimdi yeni bir kullanıcı ile kiracınızda oluşturma vakti bir *. onmicrosoft.com etki alanı. Bu kullanıcının yapılacaklar listesi istemcisiyle oturum açın ve bazı görevler kullanıcının yapılacaklar listesine ekleyin.

Başvuru için (yapılandırma değerleriniz olmadan) tamamlanan örnek kullanılabilir [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Şimdi daha fazla kimlik senaryo da taşıyabilirsiniz.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
