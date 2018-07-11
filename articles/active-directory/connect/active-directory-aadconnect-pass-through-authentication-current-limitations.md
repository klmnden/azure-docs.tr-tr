---
title: 'Azure AD Connect: Geçişli kimlik doğrulaması - geçerli sınırlamalar | Microsoft Docs'
description: Bu makalede, Azure Active Directory (Azure AD) geçişli kimlik doğrulaması geçerli sınırlamalar açıklanır.
services: active-directory
keywords: Azure AD, SSO, Azure AD Connect geçişli kimlik doğrulaması, Active Directory Yükleme gerekli bileşenleri çoklu oturum açma
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 34b83c54e31ed73af3f776a6add8f218dda35cf7
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37918929"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory geçişli kimlik doğrulaması: Geçerli sınırlamalar

>[!IMPORTANT]
>Azure Active Directory (Azure AD) geçişli kimlik doğrulaması ücretsiz bir özelliktir ve kullanmak için Azure AD Ücretli tüm sürümleri ihtiyacınız yoktur. Geçişli kimlik doğrulaması, yalnızca Azure ad ve değil dünya çapında örneğinde kullanılabilir [Microsoft Azure Almanya bulut](http://www.microsoft.de/cloud-deutschland) veya [Microsoft Azure kamu bulutundaki](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Aşağıdaki senaryolarda tam olarak desteklenir:

- Tüm web tarayıcı tabanlı uygulamalar için kullanıcı oturum açma işlemleri.
- Kullanıcı oturum açma işlemleri destekleyen Office uygulamalarına [modern kimlik doğrulaması](https://aka.ms/modernauthga): Office 2016 ve Office 2013 _ile_ modern kimlik doğrulaması.
- Exchange ActiveSync, SMTP, POP ve IMAP gibi eski protokolleri kullanarak Outlook istemcileri için kullanıcının oturum açma işlemleri.
- Kullanıcı oturum açma işlemleri için Skype Kurumsal çevrimiçi dahil olmak üzere, modern kimlik doğrulaması ve karma topolojiler destekleyen. Desteklenen topolojiler hakkında daha fazla bilgi [burada](https://technet.microsoft.com/library/mt803262.aspx).
- Windows 10 cihazlar için Azure AD etki alanına katılır.
- Çok faktörlü kimlik doğrulaması için uygulama parolaları.

## <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Aşağıdaki senaryolar _değil_ desteklenir:

- Kullanıcı oturum açma işlemleri Outlook dışında eski Office istemci uygulamaları için (bkz **desteklenen senaryoları** yukarıda): Office 2010 ve Office 2013 _olmadan_ modern kimlik doğrulaması. Kuruluşlar, modern kimlik doğrulaması için mümkünse geçmeniz önerilir. Modern kimlik doğrulama için geçişli kimlik doğrulaması desteği sağlar. Ayrıca, kullanıcı hesaplarınızı kullanarak güvenli hale getirmenize yardımcı olur [koşullu erişim](../active-directory-conditional-access-azure-portal.md) Azure multi-Factor Authentication gibi özellikleri.
- Takvim Paylaşımı ve ücretsiz/meşgul bilgileri yalnızca karma ortamlarda Office 2010 Exchange erişim.
- Kullanıcı oturum açma Skype kurumsal iş istemci uygulamaları için _olmadan_ modern kimlik doğrulaması.
- PowerShell sürüm 1.0 için kullanıcı oturum açma işlemleri. PowerShell sürüm 2.0 kullanmanızı öneririz.
- Algılama sahip kullanıcıların [kimlik bilgilerinin sızdırılması](../active-directory-reporting-risk-events.md#leaked-credentials).
- Azure AD Domain Services parola karması Eşitleme'nın Kiracı üzerinde etkin gerekir. Bu nedenle, geçişli kimlik doğrulaması kullanan kiracılar _yalnızca_ Azure AD Domain Services gerektiren senaryolar için çalışmaz.
- Geçişli kimlik doğrulaması ile tümleşik olmayan [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md).
- Apple cihaz kaydı iOS Kurulum Yardımcısı'nı kullanarak programı'nı (Apple DEP), modern kimlik doğrulamasını desteklemez. Bu, geçişli kimlik doğrulaması kullanarak yönetilen etki alanı için Intune'a Apple DEP cihazlarını kaydetmek başarısız olur. Kullanmayı [Şirket portalı uygulamasını](https://blogs.technet.microsoft.com/intunesupport/2018/02/08/support-for-multi-token-dep-and-authentication-with-company-portal/) alternatif olarak.

>[!IMPORTANT]
>Desteklenmeyen senaryolar için geçici bir çözüm olarak _yalnızca_, parola karma eşitlemesini etkinleştirin [isteğe bağlı özellikler](active-directory-aadconnect-get-started-custom.md#optional-features) Azure AD Connect sihirbazındaki sayfası. Uygulamalar kullanıcıların oturum içinde "senaryoları desteklenmeyen" bölümünde listelenen belirli bu oturum açma istekleri konusunda _değil_ geçişli kimlik doğrulaması aracıları tarafından işlenir ve bu nedenle, kaydedilmez [ Geçişli kimlik doğrulaması oturum](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#collecting-pass-through-authentication-agent-logs).

>[!NOTE]
Şirket içi altyapınızı kesintiye uğrarsa parola karma eşitlemesini etkinleştirme yük devretme kimlik doğrulama seçeneği sunar. Bu yük geçişli kimlik doğrulaması için Active Directory parola karması eşitleme otomatik değildir. Azure AD Connect kullanarak el ile oturum açma yöntemi geçmeniz gerekir. Azure AD Connect uygulamasını çalıştıran sunucunun arıza yaparsa, geçişli kimlik doğrulamasını devre dışı açmak için Microsoft Support Yardım ihtiyacınız olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı Başlangıç](active-directory-aadconnect-pass-through-authentication-quick-start.md): Azure AD geçişli kimlik doğrulaması ile çalışır duruma alın.
- [Akıllı kilitleme](../authentication/howto-password-smart-lockout.md): kullanıcı hesapları korumak için kiracınızda akıllı kilitleme özelliğini yapılandırma hakkında bilgi edinin.
- [Teknik yakından bakışın](active-directory-aadconnect-pass-through-authentication-how-it-works.md): geçişli kimlik doğrulaması özelliği nasıl çalıştığını anlayın.
- [Sık sorulan sorular](active-directory-aadconnect-pass-through-authentication-faq.md): geçişli kimlik doğrulaması özelliği hakkında sık sorulan soruları yanıtlar bulun.
- [Sorun giderme](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): geçişli kimlik doğrulaması özelliği ile ortak sorunları çözmeyi öğrenin.
- [Güvenliğe derinlemesine bakış](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): geçişli kimlik doğrulaması özelliği hakkında ayrıntılı teknik bilgi alın.
- [Azure AD sorunsuz çoklu oturum açma](active-directory-aadconnect-sso.md): tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): dosya yeni özellik istekleri için Azure Active Directory Forumu kullanın.
