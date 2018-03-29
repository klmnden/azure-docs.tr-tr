---
title: 'Azure AD Connect: Doğrudan kimlik doğrulama - geçerli sınırlamalar | Microsoft Docs'
description: Bu makalede Azure Active Directory (Azure AD) doğrudan kimlik doğrulaması'nın geçerli kısıtlamaları
services: active-directory
keywords: Azure AD Connect doğrudan kimlik doğrulama, Active Directory yükleyin gerekli bileşenleri Azure AD, SSO, çoklu oturum açma
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
ms.openlocfilehash: 680e9967010771b8e3651c6f4eed81237f8fb4c3
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory doğrudan kimlik doğrulaması: Geçerli sınırlamalar

>[!IMPORTANT]
>Azure Active Directory (Azure AD) doğrudan kimlik doğrulaması boş bir özelliktir ve kullanmak için Azure AD Ücretli tüm sürümleri olması gerekmez. Doğrudan kimlik doğrulama kullanılabilir yalnızca Azure ad ve değil dünya çapında örneğinde [Microsoft Azure Almanya bulut](http://www.microsoft.de/cloud-deutschland) veya [Microsoft Azure kamu bulut](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Aşağıdaki senaryolarda tam olarak desteklenir:

- Kullanıcı oturum açma işlemleri tüm web tarayıcı tabanlı uygulamalar için.
- Kullanıcı oturum açma işlemleri destekleyen Office uygulamaları için [modern kimlik doğrulaması](https://aka.ms/modernauthga): Office 2016 ve Office 2013 _ile_ modern kimlik doğrulaması.
- Kullanıcı oturum açma işlemlerine Exchange ActiveSync, SMTP, POP ve IMAP gibi eski protokolleri kullanarak Outlook istemcileri.
- Kullanıcı oturum açma işlemlerine Skype Kurumsal çevrimiçi dahil modern kimlik doğrulamasını ve karma topolojiler destekleyen. Desteklenen topolojiler hakkında daha fazla bilgi [burada](https://technet.microsoft.com/library/mt803262.aspx).
- Windows 10 cihazlar için Azure AD etki alanı birleştirir.
- Çok faktörlü kimlik doğrulaması için uygulama parolaları.

## <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Aşağıdaki senaryolar _değil_ desteklenir:

- Kullanıcı oturum açma işlemlerine Outlook dışında eski Office istemci uygulamalarında (bkz **desteklenen senaryoları** yukarıda): Office 2010 ve Office 2013 _olmadan_ modern kimlik doğrulaması. Kuruluşlar, modern kimlik doğrulaması için mümkünse geçmeniz önerilir. Modern kimlik doğrulama için doğrudan kimlik doğrulama desteği sağlar. Ayrıca, kullanıcı hesaplarını kullanarak güvenliğini sağlamanıza yardımcı olur [koşullu erişim](../active-directory-conditional-access-azure-portal.md) Azure çok faktörlü kimlik doğrulaması gibi özellikleri.
- Takvim Paylaşımı hem de serbest/meşgul bilgilerini Exchange yalnızca karma ortamlar Office 2010 üzerinde erişebilirsiniz.
- Kullanıcı oturum açma işlemlerine Skype iş istemci uygulamaları için _olmadan_ modern kimlik doğrulaması.
- Kullanıcı oturum açma işlemlerine PowerShell 1.0 sürümü. PowerShell sürüm 2.0 kullanmanızı öneririz.
- Kullanıcılarla algılanması [kimlik bilgileri sızmasını](../active-directory-reporting-risk-events.md#leaked-credentials).
- Azure AD etki alanı Hizmetleri parola karması eşitlemesi Kiracı'etkinleştirilmesi gerekir. Bu nedenle, geçişli kimlik doğrulaması kullanan kiracılar _yalnızca_ Azure AD etki alanı Hizmetleri gerektiren senaryolar için çalışmıyor.
- Doğrudan kimlik doğrulaması ile tümleşik değildir [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md).
- Apple cihaz kayıt iOS Kurulum Yardımcısı'nı kullanarak programı'nı (Apple DEP), modern kimlik doğrulamasını desteklemez. Bu Apple DEP cihazları doğrudan kimlik doğrulama kullanarak yönetilen etki alanları için Intune'a kaydetmek başarısız olur. Kullanmayı [Şirket portalı uygulamasını](https://blogs.technet.microsoft.com/intunesupport/2018/02/08/support-for-multi-token-dep-and-authentication-with-company-portal/) alternatif olarak.

>[!IMPORTANT]
>Desteklenmeyen senaryolar için geçici bir çözüm olarak _yalnızca_, parola karma eşitlemesini etkinleştirmek [isteğe bağlı özellikler](active-directory-aadconnect-get-started-custom.md#optional-features) Azure AD Connect Sihirbazı sayfasında. Kullanıcılar oturum uygulamalara içinde "senaryoları desteklenmeyen" bölümünde listelenen, belirli bu oturum açma istekleri alındığında _değil_ doğrudan kimlik doğrulama aracı tarafından işlenen ve bu nedenle kaydedilecek değil [ Doğrudan kimlik doğrulama oturum](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#collecting-pass-through-authentication-agent-logs).

>[!NOTE]
Şirket içi altyapınızı kesintiye uğrarsa parola karma eşitlemesini etkinleştirmek için yük devretme kimlik doğrulama seçeneği sunar. Active Directory parola karma eşitlemesi için doğrudan kimlik doğrulaması'ndan bu yük devretme otomatik değildir. Azure AD Connect kullanarak el ile oturum açma yöntemi geçmeniz gerekir. Azure AD Connect çalıştıran sunucunun kullanılamaz hale gelirse Microsoft doğrudan kimlik doğrulamasını devre dışı bırakma Support yardım almanız.

## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı Başlangıç](active-directory-aadconnect-pass-through-authentication-quick-start.md): Azure AD geçişli kimlik doğrulaması ile başlamak ve çalıştırmak.
- [Akıllı kilitleme](active-directory-aadconnect-pass-through-authentication-smart-lockout.md): kullanıcı hesapları korumak için Kiracı akıllı kilitleme özelliği yapılandırmayı öğrenin.
- [Teknik derinlemesine](active-directory-aadconnect-pass-through-authentication-how-it-works.md): doğrudan kimlik doğrulaması özelliğinin nasıl çalıştığını anlayın.
- [Sık sorulan sorular](active-directory-aadconnect-pass-through-authentication-faq.md): Bul doğrudan kimlik doğrulama özelliği hakkında sık sorulan soruları yanıtlar.
- [Sorun giderme](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): doğrudan kimlik doğrulama özelliği ile ortak sorunları çözmeyi öğrenin.
- [Güvenlik derinlemesine](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): doğrudan kimlik doğrulama özelliği hakkında ayrıntılı teknik bilgi alın.
- [Azure AD sorunsuz SSO](active-directory-aadconnect-sso.md): tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): yeni özellik istekleri dosya için Azure Active Directory Forumu kullanın.
