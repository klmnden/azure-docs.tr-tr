---
title: Azure Active Directory ile hataları tanılamak nasıl hizmete bağlı
description: Active directory bağlı hizmet uyumsuz kimlik doğrulama türü algılandı
services: active-directory
documentationcenter: ''
author: ghogen
manager: douge
editor: ''
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev
ms.openlocfilehash: c3494102703b68d6861e8755ca9dafe19c7fa6d6
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connected-service"></a>Azure Active Directory bağlı hizmeti ile hataları tanılama

Önceki kimlik doğrulama kodu algılanırken Azure Active Directory connect sunucusu uyumsuz kimlik doğrulama türü algıladı.

Doğru projede önceki kimlik doğrulama kodu algılamak için proje oluşturulmalıdır.  Bu hata ile karşılaştı ve önceki bir kimlik doğrulama kodu projenizde sahip değilseniz, yeniden oluşturun ve yeniden deneyin.

## <a name="project-types"></a>Proje türleri

Bağlı hizmet, projeye sağ kimlik doğrulaması mantığı ekleyemezsiniz şekilde geliştirirken projesi türünü denetler. Türetilen denetleyici ise `ApiController` projede Proje Webapı proje olarak kabul edilir. Öğesinden türetilen denetleyicileri varsa `MVC.Controller` projesinde projeye MVC projesinde olarak kabul edilir. Bağlantılı hizmeti, herhangi bir proje türü desteklemiyor.

## <a name="compatible-authentication-code"></a>Uyumlu kimlik doğrulama kodu

Bağlı hizmet ayrıca, önceden yapılandırılmış veya hizmeti ile uyumlu olan kimlik doğrulama ayarlarını denetler. Tüm ayarları varsa, içe servis talebi kabul edilir ve bağlı hizmet açılır ayarları görüntüleyin.  Yalnızca bazı ayarlar mevcut bir hata durumu olarak kabul edilir.

MVC projesinde, önceki hizmet kullanımdan neden aşağıdaki ayarlardan birini bağlı hizmet denetler:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Ayrıca, bağlantılı hizmet hizmet önceki kullanımdan neden Web API projesinde, aşağıdaki ayarlardan birini denetler:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

## <a name="incompatible-authentication-code"></a>Uyumsuz kimlik doğrulama kodu

Son olarak, Visual Studio'nun önceki sürümleri ile yapılandırılan kimlik doğrulama kodu sürümlerini algılamak bağlı hizmet çalışır. Bu hatayı aldıysanız, projenizin bir uyumsuz kimlik doğrulama türünü içeren anlamına gelir. Bağlantılı hizmeti, Visual Studio'nun önceki sürümleri kimlik doğrulamasını aşağıdaki türlerini algılar:

* Windows Kimlik Doğrulaması
* Bireysel kullanıcı hesapları
* Kurumsal hesaplar

Windows kimlik doğrulaması MVC projesinde algılamak için bağlı arar `authentication` öğesinde, `web.config` dosya.

```xml
<configuration>
    <system.web>
        <span style="background-color: yellow"><authentication mode="Windows" /></span>
    </system.web>
</configuration>
```

Bağlantılı hizmeti bir Web API projesinde Windows kimlik doğrulaması algılamak için arar `IISExpressWindowsAuthentication` projenizin öğesinde `.csproj` dosyası:

```xml
<Project>
    <PropertyGroup>
        <span style="background-color: yellow"><IISExpressWindowsAuthentication>enabled</IISExpressWindowsAuthentication></span>
    </PropertyGroup>
</Project>
```

Bireysel kullanıcı hesapları kimlik doğrulaması algılamak için bağlı hizmet paketi öğesinde arar, `packages.config` dosya.

```xml
<packages>
    <span style="background-color: yellow"><package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /></span>
</packages>
```

Kurumsal hesap kimlik doğrulaması eski bir formu algılamak için aşağıdaki öğesinde bağlantılı hizmet arar`web.config`:

```xml
<configuration>
    <appSettings>
        <span style="background-color: yellow"><add key="ida:Realm" value="***" /></span>
    </appSettings>
</configuration>
```

Kimlik doğrulama türünü değiştirmek için uyumsuz kimlik doğrulama türünü kaldırmak ve bağlı hizmet yeniden eklemeyi deneyin.

Daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md).