---
title: "Azure AD ile bağlandığınızda bir MVC projede yapılan değişiklikleri | Microsoft Docs"
description: "Visual Studio bağlı hizmetleri kullanarak Azure AD'ye bağlanma ne olur MVC projenize açıklar"
services: active-directory
documentationcenter: na
author: kraigb
manager: mtillman
editor: 
ms.assetid: 8b24adde-547e-4ffe-824a-2029ba210216
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: eccff00847968b4293b6e7142af0cceff0476c46
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>MVC proje için ne (Visual Studio Azure Active Directory bağlı hizmet)?
> [!div class="op_single_selector"]
> * [Başlarken](vs-active-directory-dotnet-getting-started.md)
> * [Ne oldu](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a>Başvuruları eklendi
### <a name="nuget-package-references"></a>NuGet paket başvuruları
* **Microsoft.IdentityModel.Protocol.Extensions**
* **Microsoft.Owin**
* **Microsoft.Owin.Host.SystemWeb**
* **Microsoft.Owin.Security**
* **Microsoft.Owin.Security.Cookies**
* **Microsoft.Owin.Security.OpenIdConnect**
* **Owın**
* **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>.NET başvuruları
* **Microsoft.IdentityModel.Protocol.Extensions**
* **Microsoft.Owin**
* **Microsoft.Owin.Host.SystemWeb**
* **Microsoft.Owin.Security**
* **Microsoft.Owin.Security.Cookies**
* **Microsoft.Owin.Security.OpenIdConnect**
* **Owın**
* **System.IdentityModel**
* **System.IdentityModel.Tokens.Jwt**
* **System.Runtime.Serialization**

## <a name="code-has-been-added"></a>Kod eklendi
### <a name="code-files-were-added-to-your-project"></a>Kod dosyaları projenize eklendi
Bir kimlik doğrulaması başlangıç sınıfı **App_Start/Startup.Auth.cs** Azure AD kimlik doğrulaması için başlangıç mantığı içeren projenize eklendi. Ayrıca, denetleyici sınıfını, Controllers/AccountController.cs içeren eklendi **SignIn()** ve **SignOut()** yöntemleri. Son olarak, kısmi Görünüm **Views/Shared/_LoginPartial.cshtml** Signın/SignOut için bir eylem bağlantısı içeren eklendi.

### <a name="startup-code-was-added-to-your-project"></a>Başlangıç kodu projenize eklendi
Başlangıç sınıfı projenizde, zaten sahipse **yapılandırma** yöntemi çağrısı içerecek şekilde güncelleştirildi **ConfigureAuth(app)**. Aksi takdirde, başlangıç sınıfı projenize eklendi.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>App.config veya web.config yeni yapılandırma değeri var.
Aşağıdaki yapılandırma girdileri eklenmiştir.

    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Bir Azure Active Directory (AD) uygulama oluşturuldu
Azure AD uygulaması sihirbazda seçtiğiniz dizin oluşturuldu.

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>I işaretlediyseniz *bireysel kullanıcı hesapları kimlik doğrulamasını devre dışı*, proje için ek değişiklikler yapıldı?
NuGet paket referanslarını kaldırıldı ve dosyaları kaldırıldı ve yedeklendi. Projenizi durumuna bağlı olarak, el ile ek başvurular veya dosyaları kaldırın veya uygun şekilde kodu değiştirmeniz gerekebilir.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet paket referanslarını (olanlar için mevcut) kaldırıldı
* **Microsoft.ASPNET.Identity.Core**
* **Microsoft.ASPNET.Identity.entityframework**
* **Microsoft.ASPNET.Identity.owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Kod dosyaları yedeklenebilir ve (olanlar için mevcut) kaldırıldı
Aşağıdaki dosyaların her birini yedeklendi ve projesinden kaldırılmıştır. Yedekleme dosyaları, projenin dizin kökündeki 'Yedekleme' klasöründe bulunur.

* **App_Start\IdentityConfig.cs**
* **Controllers\ManageController.cs**
* **Models\IdentityModels.cs**
* **Models\ManageViewModels.cs**

### <a name="code-files-backed-up-for-those-present"></a>(Olanlar için mevcut) yedeklenen kod dosyaları
Aşağıdaki dosyaların her birini önce değiştirilen yedeklendi. Yedekleme dosyaları, projenin dizin kökündeki 'Yedekleme' klasöründe bulunur.

* **Haline**
* **App_Start\Startup.auth.cs**
* **Controllers\AccountController.cs**
* **Görünümler/paylaşılan\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>I işaretlediyseniz *dizin verilerini okuma*, proje için ek değişiklikler yapıldı?
Ek başvurular eklenmiştir.

### <a name="additional-nuget-package-references"></a>Ek NuGet paketi başvurular
* **EntityFramework**
* **Microsoft.Azure.ActiveDirectory.GraphClient**
* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.IdentityModel.Clients.ActiveDirectory**
* **System.Spatial**

### <a name="additional-net-references"></a>Ek .NET başvurular
* **EntityFramework**
* **EntityFramework.SqlServer**
* **Microsoft.Azure.ActiveDirectory.GraphClient**
* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.IdentityModel.Clients.ActiveDirectory**
* **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
* **System.Spatial**

### <a name="additional-code-files-were-added-to-your-project"></a>Ek kod dosyaları projenize eklendi
Belirteç önbelleğe alma desteklemek için iki dosya eklendi: **Models\ADALTokenCache.cs** ve **Models\ApplicationDbContext.cs**.  Bir ek denetleyici ve görünüm Azure grafik API'leri kullanarak erişen kullanıcı profili bilgilerini göstermek için eklenmiştir.  Bu dosyalar **Controllers\UserProfileController.cs** ve **Views\UserProfile\Index.cshtml**.

### <a name="additional-startup-code-was-added-to-your-project"></a>Ek başlatma kod projenize eklendi
İçinde **startup.auth.cs** dosya, yeni bir **OpenIdConnectAuthenticationNotifications** nesne eklenmiş **bildirimleri** üyesi  **OpenIdConnectAuthenticationOptions**.  Bu OAuth kod alırken ve için bir erişim belirteci değişimi etkinleştirmek için yapılır.

### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>App.config veya web.config için ek değişiklikler yapıldı
Aşağıdaki ek yapılandırma girdileri eklenmiştir.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

Aşağıdaki yapılandırma bölümlerini ve bağlantı dizesi eklenmiştir.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
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


### <a name="your-azure-active-directory-app-was-updated"></a>Azure Active Directory uygulamanızı güncelleştirildi
Azure Active Directory uygulamanızı içerecek şekilde güncelleştirildi *dizin verilerini okuma* izni ilave bir anahtar oluşturulduğu ve hangi ardından olarak kullanılan *IDA: ClientSecret* içinde  **Web.config** dosya.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Active Directory hakkında daha fazla bilgi edinin](https://azure.microsoft.com/services/active-directory/)

