---
title: Azure AD .NET Web API'si başlangıç | Microsoft Docs
description: .NET MVC web API'si Azure AD kimlik doğrulaması ve yetkilendirme ile tümleşen oluşturmak nasıl.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: celested
ms.reviewer: hirsin, dastrock
ms.custom: aaddev
ms.openlocfilehash: ca506d821fe3534468c0d370dd51464e5df90f79
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504670"
---
# <a name="azure-ad-net-web-api-getting-started"></a>Başlarken Azure AD .NET Web API'si
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Korunan kaynaklara erişim sağlayan bir uygulama oluşturuyorsanız, bu kaynaklara unwarranted erişimi engellemek nasıl bilmeniz gerekir.
Azure Active Directory (Azure AD) sağlar yalnızca birkaç kod satırıyla, OAuth 2.0 taşıyıcı erişimi'ni kullanarak bir web API'sini korumak sade ve belirteçler.

ASP.NET web uygulamaları, .NET Framework 4. 5 ' bulunan topluluk odaklı OWIN ara yazılımı Microsoft uygulaması kullanarak bu koruma gerçekleştirebilirsiniz. OWIN için burayı kullanacağız, bir "Yapılacaklar listesi" web API'si oluşturma:

* API'leri gösteren korumalı.
* Web API çağrıları için geçerli bir erişim belirteciyle içerdiğini doğrular.

Yapmak listesi API oluşturmak için önce yapmanız:

1. Bir uygulamayı Azure AD'ye kaydedin.
2. OWIN kimlik doğrulaması işlem hattı kullanılacak uygulamasını ayarlama.
3. Web API'sini çağırmak için bir istemci uygulaması yapılandırın.

Başlamak için [uygulama çatıyı indirmeniz](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) veya [tamamlanan örnek indirme](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Her bir Visual Studio 2013 çözümüdür. Ayrıca, uygulamanızı kaydetmek Azure AD kiracısı gerekir. Zaten yoksa, [edinebileceğinizi öğrenin](quickstart-create-new-tenant.md).

## <a name="step-1-register-an-application-with-azure-ad"></a>1. adım: bir uygulamayı Azure AD'ye kaydetme
Güvenliğini sağlamaya yardımcı olmak için uygulamanızın, öncelikle kiracınızda bir uygulama oluşturun ve birkaç temel bilgiler ile Azure AD sağlamak ihtiyacınız.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Üzerine tıklayarak ve ardından sayfanın sağ üst köşesinde hesabınıza tıklayarak Azure AD kiracınızı seçin **dizini Değiştir** gezinti ve uygun bir kiracı seçin.
 * Hesabınızın altında yalnızca bir Azure AD kiracısı getirdik veya zaten uygun seçtiyseniz, bu adımı atlayın. Azure AD kiracısı.

3. Sol taraftaki gezinti bölmesinde, **Azure Active Directory**’ye tıklayın.

4. Tıklayın **uygulama kayıtları**ve ardından **Ekle**.

5. Komut istemlerini izleyin ve yeni bir **Web uygulaması ve/veya Web API**.
  * **Adı** uygulamanızı kullanıcılara açıklar. Girin **hizmet Yapılacaklar listesi**.
  * **Yeniden yönlendirme URI'si** uygulamanızı istediği tarafından istenen belirteçleri döndürmek için Azure AD kullanan düzeni ve dize bir birleşimidir. Girin `https://localhost:44321/` bu değer için.

6. Gelen **ayarları** -> **özellikleri** sayfasında uygulamanız için uygulama kimliği URI'si güncelleştirin. Bir kiracıya özgü tanımlayıcısı girin. Örneğin, `https://contoso.onmicrosoft.com/TodoListService` girin.

7. Yapılandırmayı kaydedin. Portal, kısa bir süre sonra istemci uygulamanızı kaydetmek gerekeceği için açık bırakın.

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>2. adım: OWIN kimlik doğrulaması işlem hattı kullanılacak uygulamasını ayarlama
Gelen istekleri ve belirteçleri doğrulamak için uygulamanızı Azure AD ile iletişim kurmak için ayarlamanız gerekir.

1. Başlamak için çözümü açın ve Paket Yöneticisi Konsolu'nu kullanarak OWIN ara yazılımı NuGet paketlerini TodoListService projeye ekleyin.

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. Bir OWIN başlangıç sınıfı olarak adlandırılan TodoListService projeye eklemek `Startup.cs`.  Projeye sağ tıklayın, **Ekle** > **yeni öğe**ve ardından arama **OWIN**. OWIN ara yazılımı, uygulamanız başlatıldığında `Configuration(…)` yöntemini çağırır.

3. Sınıf bildirimine değiştirme `public partial class Startup`. Zaten bu sınıfın parçası sizin için başka bir dosyaya uyguladık. İçinde `Configuration(…)` yöntemi çağrısı duruma `ConfgureAuth(…)` web uygulamanız için kimlik doğrulaması ayarlamak için.

    ```csharp
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. Dosyayı açmak `App_Start\Startup.Auth.cs` ve uygulama `ConfigureAuth(…)` yöntemi. Sağladığınız parametreler `WindowsAzureActiveDirectoryBearerAuthenticationOptions` uygulamanızı Azure AD ile iletişim kurmak için koordinatları olarak hizmet verecektir.

    ```csharp
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

5. Artık `[Authorize]` denetleyicileri ve eylemleri JSON Web Token (JWT) taşıyıcı kimlik doğrulaması ile korumaya yardımcı olmak için öznitelikler. Süslemek `Controllers\TodoListController.cs` authorize etiketi sınıfı. Bu sayfayı erişmeden önce oturum açmak için kullanıcıyı zorlar.

    ```csharp
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    Ne zaman bir yetkili arayan başarıyla çağırır birini `TodoListController` API'leri, eylem çağrıda bulunanı hakkında bilgilere erişimi gerekebilir. OWIN aracılığıyla taşıyıcı belirteç içindeki talep erişim sağlar `ClaimsPrincpal` nesne.  

6. Web API’lerine yönelik genel bir gereksinim, belirteçteki mevcut "kapsamların" doğrulanmasıdır. Bu kullanıcı için yapmak listesi hizmetine erişmesi için gereken izinleri onaylamasını sağlar.

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

7. Açık `web.config` dosya TodoListService proje kök dizininde ve yapılandırma değerlerinize girin `<appSettings>` bölümü.
  * `ida:Tenant` Azure AD kiracınıza--Örneğin, contoso.onmicrosoft.com adıdır.
  * `ida:Audience` Azure portalında girdiğiniz uygulamanın uygulama kimliği URI'si ' dir.

## <a name="step-3-configure-a-client-application-and-run-the-service"></a>3. adım: bir istemci uygulaması yapılandırma ve hizmet çalıştırma
İçin yapmak listesi hizmette eylem görebilmek için önce Azure AD'den belirteçleri almak ve hizmetine çağrı yapmak için yapılacaklar listesi istemciyi yapılandırmak gerekir.

1. Geri Git [Azure portalında](https://portal.azure.com).

2. Azure AD kiracınıza yeni bir uygulama oluşturun ve seçin **yerel istemci uygulaması** elde edilen istemi.
  * **Adı** uygulamanızı kullanıcılara açıklar.
  * Girin `http://TodoListClient/` için **yeniden yönlendirme URI'si** değeri.

3. Kayıt işlemini tamamladıktan sonra Azure AD uygulamanız için benzersiz uygulama Kimliğidir atar. Bu değer gerekir sonraki adımlarda bu nedenle uygulama sayfasından kopyalayın.

4. Gelen **ayarları** sayfasında **gerekli izinler**ve ardından **Ekle**. Bulun ve için yapmak listesi hizmeti seçin, ekleyin **erişim TodoListService** altında izin **Temsilcili izinler**ve ardından **Bitti**.

5. Visual Studio'da açın `App.config` TodoListClient içinde proje ve yapılandırma değerlerinize enter `<appSettings>` bölümü.

  * `ida:Tenant` Azure AD kiracınıza--Örneğin, contoso.onmicrosoft.com adıdır.
  * `ida:ClientId` Azure portaldan kopyaladığınız uygulama kimliğidir.
  * `todo:TodoListResourceId` Azure portalında girdiğiniz listesi hizmeti yapmak için uygulamanın uygulama kimliği URI'si ' dir.

## <a name="next-steps"></a>Sonraki adımlar
Son olarak, temizleme, yapı ve her projeyi çalıştırın. Henüz yapmadıysanız, yeni bir kullanıcı ile Kiracı oluşturmak için zaman sunulmuştur bir *. onmicrosoft.com etki alanı. Yapılacaklar listesi istemciye bu kullanıcı ile oturum açın ve bazı görevler kullanıcının yapılacaklar listesine ekleyin.

Başvuru için tamamlanmış bir örnek (olmadan yapılandırma değerlerinize) kullanılabilir [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip). Artık daha fazla kimlik senaryo da taşıyabilirsiniz.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
