---
title: Azure AD'ye bağlanma sırasında bir MVC projesi için yapılan değişiklikler
description: Visual Studio bağlı hizmetlerini kullanarak Azure AD'ye bağlanma ne olur MVC projenize açıklar
services: active-directory
author: ghogen
manager: douge
ms.assetid: 8b24adde-547e-4ffe-824a-2029ba210216
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd57eadc2c42c17343eee6d813072866e8ea09e3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60249806"
---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>MVC projeme ne oldu (Visual Studio Azure Active Directory bağlı hizmet)?

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-dotnet-getting-started.md)
> - [Ne oldu](vs-active-directory-dotnet-what-happened.md)

Bu makalede ASP.NET MVC proje eklerken tam değişikliklerinin tanımlayan [Azure Active Directory bağlı Visual Studio kullanarak hizmet](vs-active-directory-add-connected-service.md).

Bağlı hizmet ile çalışma hakkında daha fazla bilgi için bkz: [Başlarken](vs-active-directory-dotnet-getting-started.md).

## <a name="added-references"></a>Ek başvurular

Proje dosyası *.NET başvuruları etkiler) ve `packages.config` (NuGet başvurularını).

| Tür | Başvuru |
| --- | --- |
| .NET; NuGet | Microsoft.IdentityModel.Protocol.Extensions |
| .NET; NuGet | Microsoft.Owin |
| .NET; NuGet | Microsoft.Owin.Host.SystemWeb |
| .NET; NuGet | Microsoft.Owin.Security |
| .NET; NuGet | Microsoft.Owin.Security.Cookies |
| .NET; NuGet | Microsoft.Owin.Security.OpenIdConnect |
| .NET; NuGet | Owın |
| .NET        | System.IdentityModel |
| .NET; NuGet | System.IdentityModel.Tokens.Jwt |
| .NET        | System.Runtime.Serialization |

Seçtiyseniz, ek başvurular **dizin verilerini okuma** seçeneği:

| Tür | Başvuru |
| --- | --- |
| .NET; NuGet | EntityFramework |
| .NET        | EntityFramework.SqlServer (Visual Studio 2015 yalnızca) |
| .NET; NuGet | Microsoft.Azure.ActiveDirectory.GraphClient |
| .NET; NuGet | Microsoft.Data.Edm |
| .NET; NuGet | Microsoft.Data.OData |
| .NET; NuGet | Microsoft.Data.Services.Client |
| .NET; NuGet | Microsoft.IdentityModel.Clients.activedirectory |
| .NET        | Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms (Visual Studio 2015 yalnızca) |
| .NET; NuGet | System.Spatial |

Aşağıdaki başvurular kaldırılır (ASP.NET 4 projeleri yalnızca, Visual Studio 2015 olduğu gibi):

| Tür | Başvuru |
| --- | --- |
| .NET; NuGet | Microsoft.AspNet.Identity.Core |
| .NET; NuGet | Microsoft.AspNet.Identity.EntityFramework |
| .NET; NuGet | Microsoft.AspNet.Identity.Owin |

## <a name="project-file-changes"></a>Proje dosya değişiklikleri

- Özelliği ayarlanmış `IISExpressSSLPort` ayrı bir sayı.
- Özelliği ayarlanmış `WebProject_DirectoryAccessLevelKey` 0 veya 1 seçtiyseniz **dizin verilerini okuma** seçeneği.
- Özelliği ayarlanmış `IISUrl` için `https://localhost:<port>/` burada `<port>` eşleşen `IISExpressSSLPort` değeri.

## <a name="webconfig-or-appconfig-changes"></a>Web.config veya app.config değişiklikleri

- Aşağıdaki yapılandırma girdileri eklendi:

    ```xml
    <appSettings>
        <add key="ida:ClientId" value="<ClientId from the new Azure AD app>" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="<your selected Azure domain>" />
        <add key="ida:TenantId" value="<the Id of your selected Azure AD tenant>" />
        <add key="ida:PostLogoutRedirectUri" value="<project start page, such as https://localhost:44335>" />
    </appSettings>
    ```

- Eklenen `<dependentAssembly>` altındaki öğeleri `<runtime><assemblyBinding>` düğümünü `System.IdentityModel.Tokens.Jwt` ve `Microsoft.IdentityModel.Protocol.Extensions`.

Seçtiyseniz, ek değişiklikler **dizin verilerini okuma** seçeneği:

- Aşağıdaki yapılandırma girdisi altına eklendi `<appSettings>`:

    ```xml
    <add key="ida:ClientSecret" value="<Azure AD app's new client secret>" />
    ```

- Şu öğeler altında eklenen `<configuration>`; değerleri proje mdf dosyası ve proje katalog kimliği farklılık gösterir:

    ```xml
    <configSections>
      <!-- For more information on Entity Framework configuration, visit https://go.microsoft.com/fwlink/?LinkID=237468 -->
      <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>

    <connectionStrings>
      <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\<project-mdf-file>.mdf;Initial Catalog=<project-catalog-id>;Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>

    <entityFramework>
      <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
        <parameters>
          <parameter value="mssqllocaldb" />
        </parameters>
      </defaultConnectionFactory>
      <providers>
        <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
      </providers>
    </entityFramework>
    ```

- Eklenen `<dependentAssembly>` altındaki öğeleri `<runtime><assemblyBinding>` düğümünü `Microsoft.Data.Services.Client`, `Microsoft.Data.Edm`, ve `Microsoft.Data.OData`.

## <a name="code-changes-and-additions"></a>Kod değişiklikleri ve eklemeler

- Eklenen `[Authorize]` özniteliğini `Controllers/HomeController.cs` ve herhangi bir mevcut denetleyicileri.

- Bir kimlik doğrulaması başlangıç sınıfı eklenen `App_Start/Startup.Auth.cs`, Azure AD kimlik doğrulaması için başlangıç mantığı içeren. Seçtiyseniz **dizin verilerini okuma** seçeneği, bu dosya ayrıca bir OAuth kodu almak için bir erişim belirteci exchange için kod içerir.

- Bir denetleyici sınıfı eklenen `Controllers/AccountController.cs`, içeren `SignIn` ve `SignOut` yöntemleri.

- Kısmi görünüm eklenen `Views/Shared/_LoginPartial.cshtml`, bir eylem bağlantısı için içeren `SignIn` ve `SignOut`.

- Kısmi görünüm eklenen `Views/Account/SignoutCallback.cshtml`, oturum kapatma UI için HTML içeren.

- Güncelleştirilmiş `Startup.Configuration` yöntemi çağrısı içerecek şekilde `ConfigureAuth(app)` sınıf zaten var; Aksi takdirde eklenen bir `Startup` içeren sınıf yöntemini çağırır.

- Eklenen `Connected Services/AzureAD/ConnectedService.json` (Visual Studio 2017) veya `Service References/Azure AD/ConnectedService.json` Visual Studio bağlı hizmeti eklenmesini izlemek için kullanır (Visual Studio 2015), içeren bilgiler.

- Seçtiyseniz **dizin verilerini okuma** eklenen seçeneğini `Models/ADALTokenCache.cs` ve `Models/ApplicationDbContext.cs` belirteç önbelleğe desteklemek için. Bir ek bir denetleyici ve görünüm Azure graph API'leri kullanarak erişen kullanıcı profili bilgilerini göstermek için de ekledik: `Controllers/UserProfileController.cs`, `Views/UserProfile/Index.cshtml`, ve `Views/UserProfile/Relogin.cshtml`

### <a name="file-backup-visual-studio-2015"></a>Dosya yedekleme (Visual Studio 2015)

Bağlı hizmet eklerken, Visual Studio 2015 değiştirilmiş ve kaldırılan dosyaları yedekler. Etkilenen tüm dosyaları klasörüne kaydedilir `Backup/AzureAD`. Visual Studio 2017, yedeklemeler oluşturmaz.

- `Startup.cs`
- `App_Start\IdentityConfig.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ManageViewModels.cs`
- `Views\Shared\_LoginPartial.cshtml`

## <a name="changes-on-azure"></a>Azure üzerindeki değişiklikler

- Azure AD uygulaması, bağlı hizmet ekleme sırasında seçtiğiniz etki alanında oluşturuldu.
- Uygulamayı içerecek şekilde güncelleştirilmiş **dizin verilerini okuma** bu seçenek seçildiyse izni.

[Azure Active Directory hakkında daha fazla bilgi](https://azure.microsoft.com/services/active-directory/).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme](quickstart-v1-aspnet-webapp.md)
