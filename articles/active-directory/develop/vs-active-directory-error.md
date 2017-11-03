---
title: "Azure Active Directory Bağlantı Sihirbazı ile hataları tanılama"
description: "Active directory Bağlantı Sihirbazı'nı uyumsuz kimlik doğrulama türü algılandı"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/05/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 4f29f62b2996cae98b02c1ed5fcb59eca09301ef
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connection-wizard"></a>Azure Active Directory Bağlantı Sihirbazı ile hataları tanılama
Sihirbaz, önceki kimlik doğrulama kodu algılanırken uyumsuz kimlik doğrulama türü algıladı.   

## <a name="what-is-being-checked"></a>Ne denetlenen?
**Not:** doğru projede önceki kimlik doğrulama kodu algılamak için proje oluşturulmalıdır.  Bu hata ile karşılaştı ve önceki bir kimlik doğrulama kodu projenizde sahip değilseniz, yeniden oluşturun ve yeniden deneyin.

### <a name="project-types"></a>Proje türleri
Sihirbaz projeye sağ kimlik doğrulaması mantığı ekleyemezsiniz şekilde geliştirirken projesi türünü denetler.  Türetilen denetleyici ise `ApiController` projede Proje Webapı proje olarak kabul edilir.  Öğesinden türetilen denetleyicileri varsa `MVC.Controller` projesinde projeye MVC projesinde olarak kabul edilir.  Başka bir şey, sihirbaz tarafından desteklenmiyor.

### <a name="compatible-authentication-code"></a>Uyumlu kimlik doğrulama kodu
Sihirbaz aynı zamanda daha önce Sihirbazı ile yapılandırılmış veya Sihirbazı ile uyumlu olan kimlik doğrulama ayarlarını denetler.  Tüm ayarları varsa, içe servis talebi kabul edilir ve Sihirbazı açılır ayarları görüntüleyin.  Yalnızca bazı ayarlar mevcut bir hata durumu olarak kabul edilir.

MVC projesinde, sihirbazın önceki kullanımdan neden aşağıdaki ayarlardan birini Sihirbazı'nı denetler:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Ayrıca, herhangi bir Web API projesi Sihirbazı'nın önceki kullanımdan sonucunda aşağıdaki ayarları Sihirbazı'nı denetler:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

### <a name="incompatible-authentication-code"></a>Uyumsuz kimlik doğrulama kodu
Son olarak, sihirbaz, Visual Studio'nun önceki sürümleri ile yapılandırılan kimlik doğrulama kodu sürümleri algılamaya çalışır. Bu hatayı aldıysanız, projenizin bir uyumsuz kimlik doğrulama türünü içeren anlamına gelir. Sihirbaz, Visual Studio'nun önceki sürümleri kimlik doğrulamasını aşağıdaki türlerini algılar:

* Windows Kimlik Doğrulaması 
* Bireysel kullanıcı hesapları 
* Kurumsal hesaplar 

Sihirbaz Windows kimlik doğrulaması MVC projesinde algılamak için arar `authentication` öğesinden, **web.config** dosya.

<pre>
    &lt;configuration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode="Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

Sihirbaz Windows kimlik doğrulaması Web API projesinde algılamak için arar `IISExpressWindowsAuthentication` projenizin öğesinden **.csproj** dosyası:

<pre>
    &lt;Project&gt;
        &lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;enabled&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup>
    &lt;/Project&gt;
</pre>

Bireysel kullanıcı hesapları kimlik doğrulaması algılamak için paket öğesinden sihirbaz arar, **Packages.config** dosya.

<pre>
    &lt;packages&gt;
        <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /&gt;</span>
    &lt;/packages&gt;
</pre>

Kurumsal hesap kimlik doğrulaması eski bir formu algılamak için aşağıdaki öğeyi sihirbaz arar **web.config**:

<pre>
    &lt;configuration&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

Kimlik doğrulama türünü değiştirmek için uyumsuz kimlik doğrulama türünü kaldırmak ve sihirbazı yeniden çalıştırın.

Daha fazla bilgi için bkz: [Azure AD için kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md).

#<a name="next-steps"></a>Sonraki adımlar
- [Azure AD için Kimlik Doğrulama Senaryoları](active-directory-authentication-scenarios.md)