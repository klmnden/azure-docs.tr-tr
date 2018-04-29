---
title: Azure AD ile bağlandığınızda MVC projeye yapılan değişiklikler
description: Visual Studio bağlı hizmetleri kullanarak Azure AD'ye bağlanma ne olur MVC projenize açıklar
services: active-directory
author: ghogen
manager: douge
ms.assetid: 8b24adde-547e-4ffe-824a-2029ba210216
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev
ms.openlocfilehash: bd4a32575067b3de94935322ead9a7f0966d2969
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>MVC proje için ne (Visual Studio Azure Active Directory bağlı hizmet)?

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-dotnet-getting-started.md)
> - [Ne oldu](vs-active-directory-dotnet-what-happened.md)

Bu makalede ASP.NET MVC proje eklerken tam değişikliklerinin tanımlayan [Azure Active Directory bağlı Visual Studio kullanarak hizmet](vs-active-directory-add-connected-service.md).

Bağlantılı hizmeti ile çalışma hakkında daha fazla bilgi için bkz: [Başlarken](vs-active-directory-dotnet-getting-started.md).

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
| .NET; NuGet | Microsoft.IdentityModel.Clients.activedirectory tarafından |
| .NET        | Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms (Visual Studio 2015 yalnızca) |
| .NET; NuGet | System.Spatial |

Aşağıdaki başvuru kaldırılır (ASP.NET 4 projeleri yalnızca, Visual Studio 2015 olduğu gibi):

| Tür | Başvuru |
| --- | --- |
| .NET; NuGet | Microsoft.AspNet.Identity.Core |
| .NET; NuGet | Microsoft.AspNet.Identity.EntityFramework |
| .NET; NuGet | Microsoft.AspNet.Identity.Owin |

## <a name="project-file-changes"></a>Proje dosya değişiklikleri

- Özellik set `IISExpressSSLPort` ayrı bir sayı.
- Özellik set `WebProject_DirectoryAccessLevelKey` için 0 veya 1 seçtiyseniz **dizin verilerini okuma** seçeneği.
- Özellik set `IISUrl` için `https://localhost:<port>/` nerede `<port>` eşleşen `IISExpressSSLPort` değeri.

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

- Eklenen `<dependentAssembly>` altında öğelerin `<runtime><assemblyBinding>` düğümü için `System.IdentityModel.Tokens.Jwt` ve `Microsoft.IdentityModel.Protocol.Extensions`.

Seçtiyseniz, ek değişiklikler **dizin verilerini okuma** seçeneği:

- Aşağıdaki yapılandırma girdisi altına eklenen `<appSettings>`:

    ```xml
    <add key="ida:ClientSecret" value="<Azure AD app's new client secret>" />
    ```

- Şu öğeler altında eklenen `<configuration>`; proje mdf dosyası ve proje katalog kimliği değişir değerleri:

    ```xml
    <configSections>
      <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
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

- Eklenen `<dependentAssembly>` altında öğelerin `<runtime><assemblyBinding>` düğümü için `Microsoft.Data.Services.Client`, `Microsoft.Data.Edm`, ve `Microsoft.Data.OData`.

## <a name="code-changes-and-additions"></a>Kod değişiklikler ve eklemeler

- Eklenen `[Authorize]` özniteliğini `Controllers/HomeController.cs` ve diğer mevcut denetleyicileri.

- Bir kimlik doğrulaması başlangıç sınıfı eklenen `App_Start/Startup.Auth.cs`, Azure AD kimlik doğrulaması için başlangıç mantığı içeren. Seçtiyseniz **dizin verilerini okuma** seçeneği, bu dosya ayrıca bir OAuth kod almak ve bir erişim belirteci için exchange için kod içerir.

- Denetleyici sınıfı eklenen `Controllers/AccountController.cs`, içeren `SignIn` ve `SignOut` yöntemleri.

- Kısmi görünüm eklenen `Views/Shared/_LoginPartial.cshtml`, bir eylem bağlantısı için içeren `SignIn` ve `SignOut`.

- Kısmi görünüm eklenen `Views/Account/SignoutCallback.cshtml`, oturum kapatma kullanıcı Arabirimi HTML içeren.

- Güncelleştirilmiş `Startup.Configuration` yöntemine bir çağrı ekleyin `ConfigureAuth(app)` sınıf zaten varsa; Aksi takdirde eklenen bir `Startup` içeren sınıf yöntemini çağırır.

- Eklenen `Connected Services/AzureAD/ConnectedService.json` (Visual Studio 2017) veya `Service References/Azure AD/ConnectedService.json` bağlı hizmet eklenmesi izlemek için Visual Studio kullandığı (Visual Studio 2015), bilgi içeren.

- Seçtiyseniz **dizin verilerini okuma** eklenen seçeneğini `Models/ADALTokenCache.cs` ve `Models/ApplicationDbContext.cs` belirteç önbelleğe alma desteklemek için. Ayrıca bir ek denetleyici ve görünüm Azure grafik API'leri kullanarak erişen kullanıcı profili bilgilerini göstermek için eklendi: `Controllers/UserProfileController.cs`, `Views/UserProfile/Index.cshtml`, ve `Views/UserProfile/Relogin.cshtml`

### <a name="file-backup-visual-studio-2015"></a>Dosya yedekleme (Visual Studio 2015)

Bağlı hizmet eklerken, Visual Studio 2015 değiştirilmiş ve kaldırılan dosyaları yedekler. Tüm etkilenen dosyaların klasörüne kaydedilir `Backup/AzureAD`. Visual Studio 2017 yedeklemeler oluşturmaz.

- `Startup.cs`
- `App_Start\IdentityConfig.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ManageViewModels.cs`
- `Views\Shared\_LoginPartial.cshtml`

## <a name="changes-on-azure"></a>Azure üzerindeki değişiklikler

- Azure AD uygulaması bağlı hizmet eklerken, seçtiğiniz etki alanında oluşturulan.
- Eklenecek uygulama güncelleştirildi **dizin verilerini okuma** bu seçenek seçildiyse izni.

[Azure Active Directory hakkında daha fazla bilgi](https://azure.microsoft.com/services/active-directory/).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md)
- [Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme](guidedsetups/active-directory-aspnetwebapp-v1.md)
