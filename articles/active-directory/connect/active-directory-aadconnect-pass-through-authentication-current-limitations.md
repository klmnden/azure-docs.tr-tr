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
ms.date: 09/04/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: ff023812acd5e30bfec34254379431b3e620dac9
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43781851"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory geçişli kimlik doğrulaması: Geçerli sınırlamalar

>[!IMPORTANT]
>Azure Active Directory (Azure AD) geçişli kimlik doğrulaması ücretsiz bir özelliktir ve kullanmak için Azure AD Ücretli tüm sürümleri ihtiyacınız yoktur. Geçişli kimlik doğrulaması, yalnızca Azure ad ve değil dünya çapında örneğinde kullanılabilir [Microsoft Azure Almanya bulut](http://www.microsoft.de/cloud-deutschland) veya [Microsoft Azure kamu bulutundaki](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Aşağıdaki senaryolar desteklenir:

- Web tarayıcı tabanlı uygulamalar için kullanıcı oturum açma işlemleri.
- Exchange ActiveSync, EAS, SMTP, POP ve IMAP gibi eski protokolleri kullanarak Outlook istemcileri için kullanıcının oturum açma işlemleri.
- Eski Office istemci uygulamaları ve destekleyen Office uygulamaları için kullanıcı oturum açma işlemleri [modern kimlik doğrulaması](https://aka.ms/modernauthga): Office 2010, 2013 ve 2016 sürümleri.
- PowerShell sürüm 1.0 ve diğerleri gibi eski Protokolü uygulamaları için kullanıcı oturum açma işlemleri.
- Windows 10 cihazlar için Azure AD'ye katılır.
- Çok faktörlü kimlik doğrulaması için uygulama parolaları.

## <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Aşağıdaki senaryolar _değil_ desteklenir:

- Algılama sahip kullanıcıların [kimlik bilgilerinin sızdırılması](../reports-monitoring/concept-risk-events.md#leaked-credentials).
- Azure AD Domain Services parola karması Eşitleme'nın Kiracı üzerinde etkin gerekir. Bu nedenle, geçişli kimlik doğrulaması kullanan kiracılar _yalnızca_ Azure AD Domain Services gerektiren senaryolar için çalışmaz.
- Geçişli kimlik doğrulaması ile tümleşik olmayan [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md).

>[!IMPORTANT]
>Desteklenmeyen senaryolar için geçici bir çözüm olarak _yalnızca_ parola karma eşitlemesini etkinleştirin (Azure AD Connect Health tümleştirme) dışında [isteğe bağlı özellikler](active-directory-aadconnect-get-started-custom.md#optional-features) Azure AD Connect sihirbazındaki sayfası.

>[!NOTE]
Şirket içi altyapınızı kesintiye uğrarsa parola karma eşitlemesini etkinleştirme yük devretme kimlik doğrulama seçeneği sunar. Parola Karması eşitleme için geçişli kimlik doğrulaması'ndan bu yük devretme otomatik değildir. Azure AD Connect kullanarak el ile oturum açma yöntemi geçmeniz gerekir. Azure AD Connect uygulamasını çalıştıran sunucunun arıza yaparsa, geçişli kimlik doğrulamasını devre dışı açmak için Microsoft Support Yardım ihtiyacınız olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı Başlangıç](active-directory-aadconnect-pass-through-authentication-quick-start.md): Azure AD geçişli kimlik doğrulaması ile çalışır duruma alın.
- [AD FS'den doğrudan kimlik doğrulamaya geçiş](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx) -geçişli kimlik doğrulaması için AD FS (veya diğer Federasyon teknolojileri) geçirmek için ayrıntılı bir kılavuz.
- [Akıllı kilitleme](../authentication/howto-password-smart-lockout.md): kullanıcı hesapları korumak için kiracınızda akıllı kilitleme özelliğini yapılandırma hakkında bilgi edinin.
- [Teknik yakından bakışın](active-directory-aadconnect-pass-through-authentication-how-it-works.md): geçişli kimlik doğrulaması özelliği nasıl çalıştığını anlayın.
- [Sık sorulan sorular](active-directory-aadconnect-pass-through-authentication-faq.md): geçişli kimlik doğrulaması özelliği hakkında sık sorulan soruları yanıtlar bulun.
- [Sorun giderme](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): geçişli kimlik doğrulaması özelliği ile ortak sorunları çözmeyi öğrenin.
- [Güvenliğe derinlemesine bakış](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): geçişli kimlik doğrulaması özelliği hakkında ayrıntılı teknik bilgi alın.
- [Azure AD sorunsuz çoklu oturum açma](active-directory-aadconnect-sso.md): tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): dosya yeni özellik istekleri için Azure Active Directory Forumu kullanın.
