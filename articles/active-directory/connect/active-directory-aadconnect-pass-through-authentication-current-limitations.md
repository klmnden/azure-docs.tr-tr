---
title: "Azure AD Connect: Doğrudan kimlik doğrulama - geçerli sınırlamalar | Microsoft Docs"
description: "Bu makalede, Azure Active Directory (Azure AD) geçiş kimlik doğrulamasının geçerli sınırlamalar açıklanır."
services: active-directory
keywords: "Azure AD Connect doğrudan kimlik doğrulama, Active Directory yükleyin gerekli bileşenleri Azure AD, SSO, çoklu oturum açma"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: 4a33df43ca218545d6c684103a64f2cd1460913b
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Azure Active Directory doğrudan kimlik doğrulaması: Geçerli sınırlamalar

>[!IMPORTANT]
>Azure AD doğrudan kimlik doğrulama boş bir özelliktir ve kullanmak için Azure AD Ücretli tüm sürümleri olması gerekmez. Doğrudan kimlik doğrulama kullanılabilir yalnızca Azure ad ve değil dünya çapında örneğinde [Microsoft Cloud Almanya](http://www.microsoft.de/cloud-deutschland) ve [Microsoft Azure kamu bulut](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Desteklenen senaryolar

Aşağıdaki senaryolarda tam olarak desteklenir:

- Tüm web uygulamalarına tarayıcı tabanlı kullanıcı oturum açma işlemleri.
- Kullanıcı oturum açma işlemleri destekleyen Office 365 istemci uygulamalara [modern kimlik doğrulaması](https://aka.ms/modernauthga) -Office 2016 ve Office 2013 _ile_ modern kimlik doğrulaması.
- Windows 10 cihazlar için Azure AD katılım.
- Exchange ActiveSync desteği.

## <a name="unsupported-scenarios"></a>Desteklenmeyen senaryolar

Aşağıdaki senaryolar _değil_ desteklenir:

- Kullanıcı oturum açma işlemlerine eski Office istemci uygulamalarına - Office 2010 ve Office 2013 _olmadan_ modern kimlik doğrulaması). Kuruluşlar, modern kimlik doğrulaması için mümkünse geçmeniz önerilir. Modern kimlik doğrulama için doğrudan kimlik doğrulama desteği sağlar ancak da yardımcı olur, güvenli kullanıcı hesapları kullanarak [koşullu erişim](../active-directory-conditional-access-azure-portal.md) çok faktörlü kimlik doğrulama (MFA) gibi özellikleri.
- Kullanıcı oturum açmalarına Skype içine iş 2016 Skype dahil olmak üzere iş istemci uygulamaları için.
- Kullanıcı oturum açma işlemlerine PowerShell v1.0 içine. Bunun yerine PowerShell v2.0 kullanmanız önerilir.
- Azure AD etki alanı Hizmetleri.
- MFA için uygulama parolaları.
- Kullanıcılarla algılanması [kimlik bilgileri sızmasını](../active-directory-reporting-risk-events.md#leaked-credentials).

>[!IMPORTANT]
>Desteklenmeyen senaryolar için geçici bir çözüm olarak _yalnızca_, parola karma eşitlemesini etkinleştirmek [isteğe bağlı özellikler](active-directory-aadconnect-get-started-custom.md#optional-features) Azure AD Connect Sihirbazı sayfasında. Şirket içi altyapınızı tamamen kesintiye uğrarsa parola karma eşitlemesini etkinleştirme ayrıca yük devretme kimlik doğrulaması seçeneği sağlar. Parola karma eşitlemesi için doğrudan kimlik doğrulaması'ndan bu yük devretme otomatik değildir, ancak Microsoft Support yardımıyla tamamladıktan.

## <a name="next-steps"></a>Sonraki adımlar
- [**Hızlı Başlangıç** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - hale getirmek ve Azure AD doğrudan kimlik doğrulama çalıştıran.
- [**Akıllı kilitleme** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -Kiracı üzerinde yapılandırma akıllı kilitleme özelliğini kullanıcı hesaplarını korumak için.
- [**Teknik derinlemesine** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -bu özellik nasıl çalıştığını anlayın.
- [**Sık sorulan sorular** ](active-directory-aadconnect-pass-through-authentication-faq.md) -sık sorulan sorulara yanıtlar.
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
- [**Güvenlik derinlemesine** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -özelliği hakkında ek ayrıntılı teknik bilgi.
- [**Azure AD sorunsuz SSO** ](active-directory-aadconnect-sso.md) -tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleri dosyalama için.
