---
title: 'Azure AD Connect: Sorunsuz çoklu oturum açma | Microsoft Docs'
description: Bu konuda, Azure Active Directory (Azure AD) sorunsuz çoklu oturum açma ve nasıl true çoklu oturum açma kurumsal masaüstü kullanıcılarına Kurumsal ağınızdaki sağlamanıza imkan açıklanmaktadır.
services: active-directory
keywords: Azure AD, SSO, gerekli bileşenleri yükleme Active Directory, Azure AD Connect nedir çoklu oturum açma
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c34d8de3dfd06540dd50542ab19da0c1d9b1567
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60242264"
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory sorunsuz çoklu oturum açma

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>Azure Active Directory sorunsuz çoklu oturum açma nedir?

Azure Active Directory Sorunsuz Çoklu Oturum Açma (Azure AD Sorunsuz SSO) özelliği, kurumsal ağınıza bağlı kuruluş cihazlarını kullanan kullanıcıların otomatik olarak oturum açmasını sağlar. Etkin olduğunda, kullanıcıların Azure AD'de oturum ve genellikle bunların usernames öğesinde bile yazın parolalarını yazın gerekmez. Bu özellik kullanıcılarınızın şirket içi ek bileşenlere ihtiyaç duymadan bulut tabanlı uygulamalarınıza kolayca erişmesini sağlar.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Sorunsuz çoklu oturum açma ile birleştirilebilir [parola karması eşitleme](how-to-connect-password-hash-synchronization.md) veya [geçişli kimlik doğrulaması](how-to-connect-pta.md) oturum açma yöntemleri. Sorunsuz çoklu oturum açma olan _değil_ Active Directory Federasyon Hizmetleri (ADFS) için geçerlidir.

![Sorunsuz Çoklu Oturum Açma](./media/how-to-connect-sso/sso1.png)

>[!IMPORTANT]
>Sorunsuz çoklu oturum açma olması için kullanıcının cihazı gerekiyor **etki alanına katılmış**, cihazın olmasını gerektirmez, ancak [Azure AD katıldı](../active-directory-azureadjoin-overview.md).

## <a name="key-benefits"></a>Önemli avantajlar

- *Harika kullanıcı deneyimi*
  - Kullanıcılar, hem şirket içi ve bulut tabanlı uygulamalara otomatik olarak imzalanır.
  - Kullanıcıların parolalarını tekrar tekrar girmeniz gerekmez.
- *Kolayca dağıtma ve yönetme*
  - Hiçbir ek bileşenler, bunun çalışmasını sağlamak için şirket gerekli.
  - Bulut kimlik doğrulaması - herhangi bir yöntemle çalışır [parola karması eşitleme](how-to-connect-password-hash-synchronization.md) veya [geçişli kimlik doğrulaması](how-to-connect-pta.md).
  - Bazı alınabilir ya da Grup İlkesi kullanarak tüm kullanıcılarınız.
  - Herhangi bir AD FS altyapısı gerek kalmadan Azure AD ile Windows 10 cihazları kaydedin. Bu özellik, sürüm 2.1 veya daha sonraki bir sürümü kullanmak için gereken [çalışma alanına katılma istemcisi](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="feature-highlights"></a>Özellik vurgular

- Oturum açma kullanıcı adı ya da şirket içi varsayılan kullanıcı adı olabilir (`userPrincipalName`) veya Azure AD Connect'e bağlanan yapılandırılmış başka bir öznitelik (`Alternate ID`). Sorunsuz çoklu oturum açma kullandığı için her ikisini de durumları iş kullanmanız `securityIdentifier` Azure AD'de buna karşılık gelen kullanıcı nesnesi aramak için Kerberos anahtarındaki talep.
- Sorunsuz çoklu oturum açma fırsatçı bir özelliğidir. Herhangi bir nedenle başarısız olursa, kullanıcı oturum açma deneyimini normal davranışını - yani, kullanıcı oturum açma sayfasında parolalarını girmeleri gerekir döner.
- Bir uygulama bildirimi (örneğin, `https://myapps.microsoft.com/contoso.com`) ileten bir `domain_hint` (Openıd Connect) veya `whr` (SAML) parametre, kiracınıza belirleme - veya `login_hint` parametre - kullanıcı olup olmadığınızı belirlemek, Azure AD oturum açma isteğinde kullanıcılardır. otomatik olarak onları girme kullanıcı adlarını veya parolaları oturum açmış.
- Kullanıcıların bir uygulama bildirimi sessiz bir oturum açma deneyimi ayrıca Al (örneğin, `https://contoso.sharepoint.com`) Azure AD'nin uç noktaları kiracıları olarak - diğer bir deyişle, ayarlamak için oturum açma istekleri gönderir `https://login.microsoftonline.com/contoso.com/<..>` veya `https://login.microsoftonline.com/<tenant_ID>/<..>` - Azure AD'nin ortak uç nokta - diğer bir deyişle, yerine `https://login.microsoftonline.com/common/<...>` .
- Oturumunuzu desteklenir. Bu, otomatik olarak otomatik olarak sorunsuz SSO kullanarak oturum açmayı yerine oturum açmak için başka bir Azure AD hesabı seçmelerini sağlar.
- Etkileşimli olmayan bir akış kullanarak Office 365 Win32 istemcileri (Outlook, Word, Excel ve diğerleri) ve üstü sürümleri 16.0.8730.xxxx ile desteklenir. OneDrive için etkinleştirmeniz gerekir [OneDrive sessiz Yapılandırma özelliği](https://techcommunity.microsoft.com/t5/Microsoft-OneDrive-Blog/Previews-for-Silent-Sync-Account-Configuration-and-Bandwidth/ba-p/120894) sessiz bir oturum açma deneyimi.
- Azure AD Connect aracılığıyla etkinleştirilebilir.
- Ücretsiz bir özelliktir ve kullanmak için Azure AD Ücretli tüm sürümleri ihtiyacınız yoktur.
- Web tarayıcı tabanlı istemcileri ve destekleyen Office istemcilerinde desteklenir [modern kimlik doğrulaması](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016) Platform ve Kerberos kimlik doğrulaması özellikli tarayıcılar:

| OS\Browser |Internet Explorer|Microsoft Edge|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|Evet\*|Hayır|Evet|Evet\*\*\*|Yok
|Windows 8.1|Evet\*|Yok|Evet|Evet\*\*\*|Yok
|Windows 8|Evet\*|Yok|Evet|Evet\*\*\*|Yok
|Windows 7|Evet\*|Yok|Evet|Evet\*\*\*|Yok
|Windows Server 2012 R2 veya üzeri|Evet\*\*|Yok|Evet|Evet\*\*\*|Yok
|Mac OS X|Yok|Yok|Evet\*\*\*|Evet\*\*\*|Evet\*\*\*


\*Internet Explorer sürüm 10 gerektirir veya üzeri

\*\*Internet Explorer sürüm 10 gerektirir veya üzeri. Devre dışı bırakma geliştirilmiş korumalı mod

\*\*\*Gerektirir [ek yapılandırma](how-to-connect-sso-quick-start.md#browser-considerations)

>[!NOTE]
>Windows 10 için zamanlayıcısının [Azure AD'ye katılımı](../active-directory-azureadjoin-overview.md) en iyi çoklu oturum açma deneyimi için Azure AD ile.

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı Başlangıç** ](how-to-connect-sso-quick-start.md) - getirmek ve Azure AD sorunsuz çoklu oturum açma çalışıyor.
- [**Dağıtım planı** ](https://aka.ms/AuthenticationDeploymentPlan) -adım adım dağıtım planı.
- [**Teknik yakından bakışın** ](how-to-connect-sso-how-it-works.md) -bu özelliği nasıl çalıştığını anlayın.
- [**Sık sorulan sorular** ](how-to-connect-sso-faq.md) -sık sorulan soruların yanıtları.
- [**Sorun giderme** ](tshoot-connect-sso.md) -özelliği ile ilgili yaygın sorunları çözmeyi öğrenin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleriniz dosyalama için.

