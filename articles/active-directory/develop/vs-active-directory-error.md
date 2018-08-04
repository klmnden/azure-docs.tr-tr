---
title: Bağlı hizmet Azure Active Directory ile hataları tanılama
description: Active directory bağlı hizmet uyumsuz bir kimlik doğrulama türü algılandı
services: active-directory
author: ghogen
manager: douge
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev
ms.openlocfilehash: f4fc7d11516d2ebf5e091d519333b3cf1fd09c47
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495164"
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connected-service"></a>Azure Active Directory bağlı hizmetini ile hataları tanılama

Azure Active Directory connect önceki kimlik doğrulama kodu algılanırken sunucu uyumlu bir kimlik doğrulama türü algılandı.

Önceki kimlik doğrulama kodu bir projede doğru bir şekilde algılamak için proje oluşturulmalıdır.  Bu hata ile karşılaştı ve önceki bir doğrulama kodu projenizde yoksa, yeniden derleyip tekrar deneyin.

## <a name="project-types"></a>Proje türleri

Bağlı hizmet için projeye sağ kimlik doğrulaması mantığı ekleyebilir, geliştirmekte olduğunuz proje türünü denetler. Öğesinden türetilen herhangi bir denetleyici yoksa `ApiController` proje proje Webapı projesi olarak kabul edilir. Öğesinden türetilen denetleyicileri varsa `MVC.Controller` projede, projeye bir MVC projesi olarak kabul edilir. Bağlı hizmet, herhangi bir proje türünü desteklemiyor.

## <a name="compatible-authentication-code"></a>Uyumlu bir kimlik doğrulama kodu

Bağlı hizmet, daha önce yapılandırılmamış olması veya hizmeti ile uyumlu olan kimlik doğrulama ayarları denetler. Tüm ayarları varsa, içe çalışması olarak kabul edilir ve bağlı hizmet açılır ayarları görüntüleyin.  Yalnızca bazı ayarlar varsa, bir hata durumu olarak kabul edilir.

MVC projesinde, bağlı hizmet için önceki ve hizmetin kullanımını neden aşağıdaki ayarlardan birini denetler:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Ayrıca, önceki ve hizmetin kullanımını neden Web API projesinde, aşağıdaki ayarlardan birini için bağlı hizmet denetler:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

## <a name="incompatible-authentication-code"></a>Uyumsuz bir kimlik doğrulama kodu

Son olarak, bağlı hizmet kimlik doğrulama kodu, Visual Studio'nun önceki sürümleri ile yapılandırılmış sürümleri saptamaya çalışır. Bu hatayı aldıysanız, uyumsuz bir kimlik doğrulama türü projenizi içeren anlamına gelir. Bağlı hizmet kimlik doğrulaması Visual Studio'nun önceki sürümlerinden aşağıdaki türde algılar:

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

Bağlı hizmet bir Web API projesinde Windows kimlik doğrulaması algılamak için arar `IISExpressWindowsAuthentication` projenizin öğesinde `.csproj` dosyası:

```xml
<Project>
    <PropertyGroup>
        <span style="background-color: yellow"><IISExpressWindowsAuthentication>enabled</IISExpressWindowsAuthentication></span>
    </PropertyGroup>
</Project>
```

Bireysel kullanıcı hesapları kimlik doğrulaması algılamak için bağlı hizmet paketi öğeyi arar, `packages.config` dosya.

```xml
<packages>
    <span style="background-color: yellow"><package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /></span>
</packages>
```

Eski bir kurumsal hesap kimlik doğrulaması biçimi algılamak için şu öğede bağlı hizmet arar`web.config`:

```xml
<configuration>
    <appSettings>
        <span style="background-color: yellow"><add key="ida:Realm" value="***" /></span>
    </appSettings>
</configuration>
```

Kimlik doğrulama türünü değiştirmek için uyumsuz kimlik doğrulaması türünü kaldırın ve bağlı hizmet yeniden eklemeyi deneyin.

Daha fazla bilgi için [Azure AD için kimlik doğrulama senaryoları](authentication-scenarios.md).