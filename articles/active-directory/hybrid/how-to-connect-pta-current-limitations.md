---
title: 'Azure AD Connect: Geçişli kimlik doğrulaması - geçerli sınırlamalar | Microsoft Docs'
description: Bu makalede, Azure Active Directory (Azure AD) geçişli kimlik doğrulaması geçerli sınırlamalar açıklanır.
services: active-directory
keywords: Azure AD, SSO, Azure AD Connect geçişli kimlik doğrulaması, Active Directory Yükleme gerekli bileşenleri çoklu oturum açma
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/04/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97dc67d46b08bf5765c59806b45edd82f38720cd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60347762"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory geçişli kimlik doğrulaması: Geçerli sınırlamalar

>[!IMPORTANT]
>Azure Active Directory (Azure AD) geçişli kimlik doğrulaması ücretsiz bir özelliktir ve kullanmak için Azure AD Ücretli tüm sürümleri ihtiyacınız yoktur. Geçişli kimlik doğrulaması, yalnızca Azure ad ve değil dünya çapında örneğinde kullanılabilir [Microsoft Azure Almanya bulut](https://www.microsoft.de/cloud-deutschland) veya [Microsoft Azure kamu bulutundaki](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Aşağıdaki senaryolar desteklenir:

- Web tarayıcı tabanlı uygulamalar için kullanıcı oturum açma işlemleri.
- Exchange ActiveSync, EAS, SMTP, POP ve IMAP gibi eski protokolleri kullanarak Outlook istemcileri için kullanıcının oturum açma işlemleri.
- Eski Office istemci uygulamaları ve destekleyen Office uygulamaları için kullanıcı oturum açma işlemleri [modern kimlik doğrulaması](https://www.microsoft.com/en-us/microsoft-365/blog/2015/11/19/updated-office-365-modern-authentication-public-preview): Office 2013 ve 2016 sürümleri.
- PowerShell sürüm 1.0 ve diğerleri gibi eski Protokolü uygulamaları için kullanıcı oturum açma işlemleri.
- Windows 10 cihazlar için Azure AD'ye katılır.
- Çok faktörlü kimlik doğrulaması için uygulama parolaları.

## <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Aşağıdaki senaryolar _değil_ desteklenir:

- Algılama sahip kullanıcıların [kimlik bilgilerinin sızdırılması](../reports-monitoring/concept-risk-events.md#leaked-credentials).
- Azure AD Domain Services parola karması Eşitleme'nın Kiracı üzerinde etkin gerekir. Bu nedenle, geçişli kimlik doğrulaması kullanan kiracılar _yalnızca_ Azure AD Domain Services gerektiren senaryolar için çalışmaz.
- Geçişli kimlik doğrulaması ile tümleşik olmayan [Azure AD Connect Health](whatis-hybrid-identity-health.md).

> [!IMPORTANT]
> Desteklenmeyen senaryolar için geçici bir çözüm olarak _yalnızca_ parola karma eşitlemesini etkinleştirin (Azure AD Connect Health tümleştirme) dışında [isteğe bağlı özellikler](how-to-connect-install-custom.md#optional-features) Azure AD Connect sihirbazındaki sayfası.
> 
> [!NOTE]
> Şirket içi altyapınızı kesintiye uğrarsa parola karma eşitlemesini etkinleştirme yük devretme kimlik doğrulama seçeneği sunar. Parola Karması eşitleme için geçişli kimlik doğrulaması'ndan bu yük devretme otomatik değildir. Azure AD Connect kullanarak el ile oturum açma yöntemi geçmeniz gerekir. Azure AD Connect uygulamasını çalıştıran sunucunun arıza yaparsa, geçişli kimlik doğrulamasını devre dışı açmak için Microsoft Support Yardım ihtiyacınız olacaktır.

## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı Başlangıç](how-to-connect-pta-quick-start.md): Azure AD geçişli kimlik doğrulaması ile çalışır duruma alın.
- [AD FS'den doğrudan kimlik doğrulamaya geçiş](https://aka.ms/ADFSTOPTADPDownload) -geçişli kimlik doğrulaması için AD FS (veya diğer Federasyon teknolojileri) geçirmek için ayrıntılı bir kılavuz.
- [Akıllı kilitleme](../authentication/howto-password-smart-lockout.md): Akıllı kilitleme özelliğini kiracınızda kullanıcı hesapları korumak için yapılandırmayı öğrenin.
- [Teknik yakından bakışın](how-to-connect-pta-how-it-works.md): Geçişli kimlik doğrulaması özelliği nasıl çalıştığını anlayın.
- [Sık sorulan sorular](how-to-connect-pta-faq.md): Geçişli kimlik doğrulaması özelliği hakkında sık sorulan soruların yanıtlarını bulun.
- [Sorun giderme](tshoot-connect-pass-through-authentication.md): Geçişli kimlik doğrulaması özelliği olan yaygın sorunların nasıl çözümleneceğini öğrenin.
- [Güvenliğe derinlemesine bakış](how-to-connect-pta-security-deep-dive.md): Geçişli kimlik doğrulaması özelliği hakkında ayrıntılı teknik bilgilerini edinin.
- [Azure AD sorunsuz çoklu oturum açma](how-to-connect-sso.md): Tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Yeni özellik istekleriniz dosya için Azure Active Directory Forumu kullanın.
