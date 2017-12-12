---
title: "Azure AD ile bağlandığınızda Webapı projeye yapılan değişiklikler | Microsoft Docs"
description: "Visual Studio kullanarak Azure AD'ye bağlanma ne olur Webapı projenize açıklar"
services: active-directory
documentationcenter: 
author: kraigb
manager: mtillman
editor: 
ms.assetid: 57630aee-26a2-4326-9dbb-ea2a66daa8b0
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 8a0f6e1838bcc550829c0da92dc224e1df859e9c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>My Webapı projeye ne (Visual Studio Azure Active Directory bağlı hizmeti)
> [!div class="op_single_selector"]
> * [Başlarken](vs-active-directory-webapi-getting-started.md)
> * [Ne oldu](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a>Başvuruları eklendi
### <a name="nuget-package-references"></a>NuGet paket başvuruları
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a>.NET başvuruları
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a>Kod değişiklikleri
### <a name="code-files-were-added-to-your-project"></a>Kod dosyaları projenize eklendi
Bir kimlik doğrulaması başlangıç sınıfı **App_Start/Startup.Auth.cs** Azure AD kimlik doğrulaması için başlangıç mantığı içeren projenize eklendi.

### <a name="startup-code-was-added-to-your-project"></a>Başlangıç kodu projenize eklendi
Başlangıç sınıfı projenizde, zaten sahipse **yapılandırma** yöntemi çağrısı içerecek şekilde güncelleştirildi `ConfigureAuth(app)`. Aksi takdirde, başlangıç sınıfı projenize eklendi.

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>App.config veya web.config dosyanızda yeni yapılandırma değeri içeriyor.
Aşağıdaki yapılandırma girdileri eklenmiştir.

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a>Azure AD uygulaması oluşturuldu
Azure AD uygulaması sihirbazda seçtiğiniz dizin oluşturuldu.

[Azure Active Directory hakkında daha fazla bilgi edinin](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>I işaretlediyseniz *bireysel kullanıcı hesapları kimlik doğrulamasını devre dışı*, proje için ek değişiklikler yapıldı?
NuGet paket referanslarını kaldırıldı ve dosyaları kaldırıldı ve yedeklendi. Projenizi durumuna bağlı olarak, el ile ek başvurular veya dosyaları kaldırın veya uygun şekilde kodu değiştirmeniz gerekebilir.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet paket referanslarını (olanlar için mevcut) kaldırıldı
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Kod dosyaları yedeklenebilir ve (olanlar için mevcut) kaldırıldı
Aşağıdaki dosyaların her birini yedeklendi ve projesinden kaldırılmıştır. Yedekleme dosyaları, projenin dizin kökündeki 'Yedekleme' klasöründe bulunur.

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a>(Olanlar için mevcut) yedeklenen kod dosyaları
Aşağıdaki dosyaların her birini önce değiştirilen yedeklendi. Yedekleme dosyaları, projenin dizin kökündeki 'Yedekleme' klasöründe bulunur.

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>I işaretlediyseniz *dizin verilerini okuma*, proje için ek değişiklikler yapıldı?
### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>App.config veya web.config için ek değişiklikler yapıldı
Aşağıdaki ek yapılandırma girdileri eklenmiştir.

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a>Azure Active Directory uygulamanızı güncelleştirildi
Azure Active Directory uygulamanızı içerecek şekilde güncelleştirildi *dizin verilerini okuma* izni ilave bir anahtar oluşturulduğu ve hangi ardından olarak kullanılan *IDA: parola* içinde `web.config` dosya.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Active Directory hakkında daha fazla bilgi edinin](https://azure.microsoft.com/services/active-directory/)

