---
title: 'Azure AD Connect: Sorunsuz çoklu oturum açma | Microsoft Docs'
description: Bu konuda, Azure Active Directory (Azure AD) sorunsuz çoklu oturum açma ve nasıl Kurumsal ağınızdaki Kurumsal Masaüstü Kullanıcıları için doğru çoklu oturum açma sağlamak üzere tanır açıklanmaktadır.
services: active-directory
keywords: Azure AD, SSO, gerekli bileşenleri yükleme Active Directory, Azure AD Connect nedir çoklu oturum açma
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: billmath
ms.openlocfilehash: b1c82727e97b85fae5f315ceb1cd79cfdd111b45
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory sorunsuz çoklu oturum açma

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory sorunsuz çoklu oturum açma nedir?

Azure Active Directory sorunsuz çoklu oturum açma (Azure AD sorunsuz SSO) otomatik olarak oturum açtığında şirket cihazlarından şirket ağınıza bağlı olduklarında kullanıcıların. Etkin olduğunda, kullanıcılar parolalarını Azure AD ile oturum açma ve genellikle, kendi usernames öğesinde bile yazın yazın gerek yoktur. Bu özellik herhangi bir şirket içinde ek bileşenini gerek kalmadan, kullanıcılarınızın bulut tabanlı uygulamalarınızı kolay erişim sağlar.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Sorunsuz SSO ile birleştirilebilir [parola karması eşitlemesi](active-directory-aadconnectsync-implement-password-synchronization.md) veya [doğrudan kimlik doğrulama](active-directory-aadconnect-pass-through-authentication.md) oturum açma yöntemleri.

![Sorunsuz çoklu oturum açma](./media/active-directory-aadconnect-sso/sso1.png)

>[!IMPORTANT]
>Sorunsuz SSO olan _değil_ Active Directory Federasyon Hizmetleri (ADFS) için geçerlidir.

## <a name="key-benefits"></a>Önemli avantajlar

- *Mükemmel bir kullanıcı deneyimi*
  - Kullanıcılar, şirket içi ve bulut tabanlı uygulamalar otomatik olarak imzalanmıştır.
  - Kullanıcıların parolalarını tekrar tekrar girmeniz gerekmez.
- *Kolay dağıtma ve yönetme*
  - Herhangi bir ek bileşeni bunun çalışmasını sağlamak için şirket içi gerekli.
  - Bulut kimlik doğrulaması - herhangi bir yöntemle çalışır [parola karması eşitlemesi](active-directory-aadconnectsync-implement-password-synchronization.md) veya [doğrudan kimlik doğrulama](active-directory-aadconnect-pass-through-authentication.md).
  - Bazı alınabilmesi için veya tüm kullanıcılar Grup İlkesi kullanarak.
  - Herhangi bir AD FS altyapı gerek kalmadan Azure AD ile Windows 10 cihazları kaydedin. Bu özellik, 2.1 veya sonraki bir sürümü kullanmak için gereken [çalışma alanına katılma istemcisi](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="feature-highlights"></a>Özellik vurgular

- Oturum açma kullanıcı adı ya da şirket içi varsayılan kullanıcı adı olabilir (`userPrincipalName`) veya Azure AD Connect içinde yapılandırılmış başka bir öznitelik (`Alternate ID`). Sorunsuz SSO kullandığı için her ikisini de durumlarda iş kullanmak `securityIdentifier` Azure AD içinde karşılık gelen kullanıcı nesnesi aramak için Kerberos bileti talep.
- Sorunsuz SSO fırsatçılıktan bir özelliktir. Herhangi bir nedenle başarısız olursa, kullanıcı oturum açma deneyimi normal davranışını - yani, kullanıcı oturum açma sayfasında parolalarını girmeleri gerekir geri gider.
- Bir uygulama varsa (örneğin, https://myapps.microsoft.com/contoso.com) iletir bir `domain_hint` (Openıd Connect) veya `whr` (SAML) parametre - kiracınız, tanımlama veya `login_hint` kullanıcı tanımlama parametre - kendi Azure AD oturum açma isteğinde kullanıcılardır otomatik olarak onları girme kullanıcı adlarını veya parolaları oturum.
- Kullanıcılar ayrıca alırsanız sessiz bir oturum açma deneyimi uygulama (örneğin, https://contoso.sharepoint.com) diğer bir deyişle, Azure AD kiralanan Uç noktalara - oturum açma istekleri gönderir https://login.microsoftonline.com/contoso.com/<..> veya https://login.microsoftonline.com/<tenant_ID>/<..> - Azure AD ortak bir uç - diğer bir deyişle, yerine https://login.microsoftonline.com/common/<...>.
- Oturum kapatma desteklenir. Bu, otomatik olarak otomatik olarak kullanarak sorunsuz SSO imzalanmakta yerine oturum açmak için başka bir Azure AD hesabının seçmelerini sağlar.
- Office 365 istemcileri (16.0.8730.xxxx ve üstü) etkileşimli olmayan bir akış kullanarak desteklenir.
- Azure AD Connect üzerinden etkinleştirilebilir.
- Boş bir özelliktir ve kullanmak için Azure AD Ücretli tüm sürümleri olması gerekmez.
- Web tarayıcısı tabanlı istemciler ve Destek Office istemcilerinde desteklenen [modern kimlik doğrulaması](https://aka.ms/modernauthga) platformlarında ve Kerberos kimlik doğrulaması özellikli tarayıcılar:

| OS\Browser |Internet Explorer|Edge|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|Evet|Hayır|Evet|Evet\*|Yok
|Windows 8.1|Evet|Yok|Evet|Evet\*|Yok
|Windows 8|Evet|Yok|Evet|Evet\*|Yok
|Windows 7|Evet|Yok|Evet|Evet\*|Yok
|Mac OS X|Yok|Yok|Evet\*|Evet\*|Evet\*

\*Gerektirir [ek yapılandırma](active-directory-aadconnect-sso-quick-start.md#browser-considerations)

>[!NOTE]
>Windows 10 için öneri kullanmaktır [Azure AD katılım](../active-directory-azureadjoin-overview.md) en iyi tek oturum açma deneyimi için Azure AD ile.

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı Başlangıç** ](active-directory-aadconnect-sso-quick-start.md) - hale getirmek ve Azure AD sorunsuz SSO çalışıyor.
- [**Teknik derinlemesine** ](active-directory-aadconnect-sso-how-it-works.md) -bu özellik nasıl çalıştığını anlayın.
- [**Sık sorulan sorular** ](active-directory-aadconnect-sso-faq.md) -sık sorulan sorulara yanıtlar.
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-sso.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleri dosyalama için.
