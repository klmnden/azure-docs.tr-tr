---
title: "Azure AD v2 ASP.NET Web sunucusu alma başlatıldı - Test | Microsoft Docs"
description: "Microsoft oturum açma Openıd Connect standardını kullanan geleneksel web tarayıcı tabanlı bir uygulama ile ASP.NET çözümünü uygulama"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: bfa2563a6a58370d9a611440017441a751b46244
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
## <a name="test-your-code"></a>Kodunuzu test

Tuşuna `F5` Visual Studio'da projeyi çalıştırın. Tarayıcı açılır ve sizden *http://localhost: {port}* burada göreceksiniz *oturum Microsoft oturum* düğmesi. Bir tane oturum açmak için tıklatın.

Test etmek hazır olduğunuzda, oturum açmak için bir iş, okul (Azure Active Directory) veya kişisel (live.com, outlook.com) hesabı kullanın. 

![Microsoft tarayıcı penceresi ile oturum aç](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Microsoft tarayıcı penceresi ile oturum aç](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Beklenen sonuçları
Oturum açma işleminden sonra kullanıcı uygulama kayıt bilgilerinizi Microsoft uygulama kayıt Portalı'nda belirtilen HTTPS URL'si olan web sitesinin giriş sayfasına yeniden yönlendirilir. Bu sayfayı artık gösterir *Hello {User}* ve bir bağlantı oturum kapatma ve Yetkilendir denetleyicisinin bağlantısını olan kullanıcı taleplerini görmek için bir bağlantıyı oluşturduğunuz.

### <a name="see-users-claims"></a>Kullanıcının talepleri bakın
Kullanıcının talepleri görmek için köprü seçin. Bu, yalnızca kimliği doğrulanan kullanıcılar tarafından kullanılabilir görünüm ve denetleyici için yol gösterir.

#### <a name="expected-results"></a>Beklenen sonuçları
 Oturum açmış olan kullanıcının temel özelliklerini içeren bir tablo görmeniz gerekir:

| Özellik | Değer | Açıklama|
|---|---|---|
| Ad | {Tam} kullanıcı adı | Kullanıcı adı ve Soyadı
|Kullanıcı adı | <span>user@domain.com</span>| Oturum açmış kullanıcıyı tanımlamak için kullanılan kullanıcı adı
| Konu| {Konu}|Kullanıcı oturum açma web üzerinden benzersiz şekilde tanımlamak için bir dize|
| Kiracı Kimliği| {GUID}| A *GUID* kullanıcının Azure Active Directory kuruluş benzersiz olarak gösterecek.|

Ayrıca, kimlik doğrulama isteğine dahil tüm kullanıcı talepleri de dahil olmak üzere bir tablo görürsünüz. Tüm talep bir belirteç kimliği ve açıklama listesi için lütfen bkz [makale](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "listesi, talepleri kimliği belirteci").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Test içeren bir yöntem erişen bir *[Authorize]* özniteliği (isteğe bağlı)
Bu adımda, bir anonim kullanıcı olarak kimlik doğrulaması yapılmış denetleyicisi erişimi test:<br/>
Kullanıcı oturum kapatma bağlantısını seçin ve oturum kapatma işlemini tamamlayın.<br/>
Şimdi, tarayıcıda http://localhost yazın: {port} / ile korunan denetleyicinizi erişmek için kimliği doğrulanmış `[Authorize]` özniteliği

#### <a name="expected-results"></a>Beklenen sonuçları
Görmek için kimlik doğrulaması gerektirmek istemi almanız gerekir.

## <a name="additional-information"></a>Ek bilgiler

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>Tüm web sitenize koruma
Tüm web sitenize korumak için add `AuthorizeAttribute` için `GlobalFilters` içinde `Global.asax` `Application_Start` yöntemi:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> **Kullanıcılar uygulamanıza oturum açmak için yalnızca bir kuruluştan kısıtlama**

> Varsayılan olarak, kişisel (outlook.com, live.com ve diğerleri dahil) hesaplarının yanı sıra iş ve Okul hesapları herhangi bir şirket veya Azure Active Directory ile tümleşik olan Kuruluş oturum uygulamanıza bileşenini. 

> Oturum açma işlemleri yalnızca bir Azure Active Directory kuruluştan kabul etmek için uygulamanızın isterseniz Değiştir `Tenant` parametresinde *web.config* gelen `Common` kuruluşun – Kiracı adı için örnek, *contoso.onmicrosoft.com*. Bundan sonra değiştirmek `ValidateIssuer` bağımsız değişkeni, *OWIN başlangıç sınıfı* için `true`.

> Yalnızca belirli kuruluşların listesi kullanıcılardan izin verecek şekilde ayarlamanız `ValidateIssuer` true ve kullanmak için `ValidIssuers` kuruluşların listesini belirtmek için parametre.

> Başka bir seçenek IssuerValidator parametresini kullanarak verenler doğrulamak için özel bir yöntem uygulamaktır. Hakkında daha fazla bilgi için `TokenValidationParameters`, lütfen bkz [bu](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "tokenvalidationparameters değerini MSDN makalesine") MSDN makalesi.

[!INCLUDE  [Help and Support Options](../../../../includes/active-directory-develop-help-support-include.md)]