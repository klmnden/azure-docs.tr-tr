---
title: Azure AD'ye bağlanma sırasında bir Webapı projesi için yapılan değişiklikler
description: Visual Studio kullanarak Azure AD'ye bağlanma ne olur Webapı projenize açıklar
services: active-directory
author: ghogen
manager: douge
ms.assetid: 57630aee-26a2-4326-9dbb-ea2a66daa8b0
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7f4946251cf72d7869ec5fc2f0fd844b9c06ac34
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60353287"
---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Webapı projeme ne oldu (Visual Studio Azure Active Directory bağlı hizmet)

> [!div class="op_single_selector"]
> - [Başlarken](vs-active-directory-webapi-getting-started.md)
> - [Ne oldu](vs-active-directory-webapi-what-happened.md)

Bu makalede eklerken ASP.NET Webapı, ASP.NET tek sayfalık uygulaması ve Azure API ASP.NET projeleri için yapılan değişiklikleri tam tanımlayan [Azure Active Directory bağlı hizmetini Visual Studio kullanarak](vs-active-directory-add-connected-service.md). Visual Studio 2015'te Azure mobil hizmeti ASP.NET projeleri için de geçerlidir.

Bağlı hizmet ile çalışma hakkında daha fazla bilgi için bkz: [Başlarken](vs-active-directory-webapi-getting-started.md).

## <a name="added-references"></a>Ek başvurular

Proje dosyası *.NET başvuruları etkiler) ve `packages.config` (NuGet başvurularını).

| Tür | Başvuru |
| --- | --- |
| .NET; NuGet | Microsoft.Owin |
| .NET; NuGet | Microsoft.Owin.Host.SystemWeb |
| .NET; NuGet | Microsoft.Owin.Security |
| .NET; NuGet | Microsoft.Owin.Security.ActiveDirectory |
| .NET; NuGet | Microsoft.Owin.Security.Jwt |
| .NET; NuGet | Microsoft.Owin.Security.OAuth |
| .NET; NuGet | Owın |
| .NET; NuGet | System.IdentityModel.Tokens.Jwt |

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
| .NET        | Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms<br>(Visual Studio 2015 yalnızca) |
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
        <add key="ida:Tenant" value="<your selected Azure domain>" />
        <add key="ida:Audience" value="<your selected domain + / + project name>" />
    </appSettings>
    ```

- Yalnızca Visual Studio 2017 için: Ayrıca şu girdiyi altında eklendi `<appSettings>`"

    ```xml
    <add key="ida:MetadataAddress" value="<domain URL + /federationmetadata/2007-06/federationmetadata.xml>" />
    ```

- Eklenen `<dependentAssembly>` altındaki öğeleri `<runtime><assemblyBinding>` düğümünü `System.IdentityModel.Tokens.Jwt`.

- Seçtiyseniz **dizin verilerini okuma** seçeneğinde, aşağıdaki yapılandırma girdisi altına eklendi `<appSettings>`:

    ```xml
    <add key="ida:Password" value="<Your Azure AD app's new password>" />
    ```

## <a name="code-changes-and-additions"></a>Kod değişiklikleri ve eklemeler

- Eklenen `[Authorize]` özniteliğini `Controllers/ValueController.cs` ve herhangi bir mevcut denetleyicileri.

- Bir kimlik doğrulaması başlangıç sınıfı eklenen `App_Start/Startup.Auth.cs`, Azure AD kimlik doğrulaması için başlangıç mantığı içeren veya uygun şekilde değiştirdi. Seçtiyseniz **dizin verilerini okuma** seçeneği, bu dosya ayrıca bir OAuth kodu almak için bir erişim belirteci exchange için kod içerir.

- (ASP.NET 4 ile visual Studio 2015 yalnızca uygulama) Kaldırılan `App_Start/IdentityConfig.cs` ve eklenen `Controllers/AccountController.cs`, `Models/IdentityModel.cs`, ve `Providers/ApplicationAuthProvider.cs`.

- Eklenen `Connected Services/AzureAD/ConnectedService.json` (Visual Studio 2017) veya `Service References/Azure AD/ConnectedService.json` Visual Studio bağlı hizmeti eklenmesini izlemek için kullanır (Visual Studio 2015), içeren bilgiler.

### <a name="file-backup-visual-studio-2015"></a>Dosya yedekleme (Visual Studio 2015)

Bağlı hizmet eklerken, Visual Studio 2015 değiştirilmiş ve kaldırılan dosyaları yedekler. Etkilenen tüm dosyaları klasörüne kaydedilir `Backup/AzureAD`. Visual Studio 2017, yedeklemeler oluşturmaz.

- `Startup.cs`
- `App_Start\IdentityConfig.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ApplicationOAuthProvider.cs`

## <a name="changes-on-azure"></a>Azure üzerindeki değişiklikler

- Azure AD uygulaması, bağlı hizmet ekleme sırasında seçtiğiniz etki alanında oluşturuldu.
- Uygulamayı içerecek şekilde güncelleştirilmiş **dizin verilerini okuma** bu seçenek seçildiyse izni.

[Azure Active Directory hakkında daha fazla bilgi](https://azure.microsoft.com/services/active-directory/).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory için kimlik doğrulama senaryoları](authentication-scenarios.md)
- [Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme](quickstart-v1-aspnet-webapp.md)
