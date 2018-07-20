---
title: 'Azure AD Connect: Geçişli kimlik doğrulaması - nasıl çalışır? | Microsoft Docs'
description: Bu makalede Azure Active Directory geçişli kimlik doğrulamasını nasıl çalıştığı açıklanır
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
ms.date: 07/19/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 371f3ea3b764eecbb621fc06ec483de4778e7db2
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159500"
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Azure Active Directory geçişli kimlik doğrulaması: Ayrıntılı Teknik İnceleme
Bu makalede nasıl bir genel bakıştır Azure Active directory (Azure AD) geçişli kimlik doğrulaması çalışır. Derin teknik ve güvenlik bilgileri için bkz: [güvenliğe derinlemesine bakış](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) makalesi.

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Azure Active Directory geçişli kimlik doğrulaması nasıl çalışır?

[!NOTE]
Bir önkoşul çalışmak geçişli kimlik doğrulaması, kullanıcıların Azure AD Connect kullanarak şirket içi Active Directory'den Azure AD'ye sağlanması gerekir. Geçişli kimlik doğrulaması, yalnızca bulutta yer alan kullanıcılara uygulanmaz.

Bir kullanıcı Azure AD tarafından güvenli hale getirilmiş bir uygulamaya oturum açmaya çalıştığında ve Kiracı'da geçişli kimlik doğrulaması etkinleştirilirse, aşağıdaki adımlardan oluşur:

1. Kullanıcı bir uygulama, örneğin, erişmeye [Outlook Web App](https://outlook.office365.com/owa/).
2. Kullanıcı zaten oturum açmamış, kullanıcının Azure AD'ye yeniden yönlendirilir **kullanıcı oturum açma** sayfası.
3. Kullanıcı Azure AD oturum açma sayfasında kullanıcı adlarını girer ve ardından seçer **sonraki** düğmesi.
4. Kullanıcı parolasını girer. Azure AD oturum açma sayfası ve ardından seçer **oturum** düğmesi.
5. Oturum açma isteği alırken azure AD, kullanıcı adı ve parola (şifreli kimlik doğrulama aracılarının ortak anahtarını kullanarak) bir sırada yerleştirir.
6. Bir şirket içi kimlik doğrulaması Aracısı, kullanıcı adı ve şifreli parola kuyruktan alır. Aracıyı sık kuyruğundan istekleri için yoklama değil, ancak önceden oluşturulmuş kalıcı bir bağlantı isteklerini alır unutmayın.
7. Aracı, özel anahtarı kullanarak parolanın şifresini çözer.
8. Aracı kullanıcı adı ve benzer bir mekanizma hangi Active Directory Federasyon Hizmetleri (AD FS) için standart Windows API'leri kullanarak Active Directory parolalarını kullanır. Kullanıcı adı ya da şirket içi varsayılan kullanıcı adı, genellikle olabilir `userPrincipalName`, ya da Azure AD Connect'e bağlanan yapılandırılmış başka bir öznitelik (olarak bilinen `Alternate ID`).
9. Şirket içi Active Directory etki alanı denetleyicisi (DC) bir isteği değerlendirir ve uygun bir yanıt döndürür (başarılı, hata, parolanın süresi doldu veya kullanıcı kilitli) aracısı.
10. Kimlik doğrulaması Aracısı, Azure AD geri bu yanıt döndürür.
11. Azure AD yanıt değerlendirir ve kullanıcıya uygun şekilde yanıt verir. Hemen oturum açtığında ya da Azure multi-Factor Authentication için istekleri gibi Azure AD.
12. Kullanıcı oturum açma başarılı olursa, kullanıcının uygulamaya erişebilir.

Aşağıdaki diyagramda, tüm bileşenleri ve adımlar gösterilmektedir:

![Doğrudan Kimlik Doğrulama](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Geçerli sınırlamalar](active-directory-aadconnect-pass-through-authentication-current-limitations.md): hangi senaryolar desteklenir ve hangilerinin olmayan öğrenin.
- [Hızlı Başlangıç](active-directory-aadconnect-pass-through-authentication-quick-start.md): Azure AD geçişli kimlik doğrulaması ve çalışır duruma getirin.
- [Akıllı kilitleme](../authentication/howto-password-smart-lockout.md): kullanıcı hesapları korumak için kiracınızda akıllı kilitleme özelliğini yapılandırın.
- [Sık sorulan sorular](active-directory-aadconnect-pass-through-authentication-faq.md): Bul sık sorulan soruların yanıtları.
- [Sorun giderme](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): geçişli kimlik doğrulaması özelliği ile ortak sorunları çözmeyi öğrenin.
- [Güvenliğe derinlemesine bakış](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): geçişli kimlik doğrulaması özelliği hakkında ayrıntılı teknik bilgi alın.
- [Azure AD sorunsuz çoklu oturum açma](active-directory-aadconnect-sso.md): tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): dosya yeni özellik istekleri için Azure Active Directory Forumu kullanın.

