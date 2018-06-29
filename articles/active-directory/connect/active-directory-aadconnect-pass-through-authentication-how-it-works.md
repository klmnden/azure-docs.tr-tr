---
title: 'Azure AD Connect: Doğrudan kimlik doğrulama - nasıl çalışır? | Microsoft Docs'
description: Bu makalede Azure Active Directory doğrudan kimlik doğrulaması nasıl çalıştığı açıklanır
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
ms.date: 01/24/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 5fde0ea00aacbb791836fc1076b88dafd3728454
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063485"
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Azure Active Directory doğrudan kimlik doğrulaması: Teknik derinlemesine bakış
Bu makalede hakkında genel bakış olan Azure Active directory (Azure AD) doğrudan kimlik doğrulama çalışıyor. Ayrıntılı teknik ve güvenlik bilgileri için bkz: [güvenlik derinlemesine](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) makalesi.

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Azure Active Directory doğrudan kimlik doğrulaması nasıl çalışır?

Bir kullanıcı Azure AD tarafından güvenli hale getirilmiş bir uygulamaya oturum açmaya çalıştığında ve Kiracı'geçişli kimlik doğrulaması etkinleştirilirse, aşağıdaki adımlardan oluşur:

1. Kullanıcı bir uygulama, örneğin, erişmeyi dener [Outlook Web App](https://outlook.office365.com/owa/).
2. Kullanıcı zaten oturum açmamış, Azure AD ile kullanıcının yönlendirildiği **kullanıcı oturum açma** sayfası.
3. Kullanıcı Azure AD oturum açma sayfası kullanıcı adlarını girer ve ardından seçer **sonraki** düğmesi.
4. Kullanıcı Azure AD oturum açma sayfası parolasını girer ve ardından seçer **oturum** düğmesi.
5. Oturum açma, isteği alırken azure AD kullanıcı adı ve parola (kimlik doğrulaması aracıların ortak anahtar kullanılarak şifrelenmiş) sıraya koyar.
6. Bir şirket içi kimlik doğrulama Aracısı sıradan şifrelenmiş parola ve kullanıcı adını alır. Aracı sıra gelen istekleri için sık yoklamak değil ancak istekleri önceden belirlenen kalıcı bir bağlantı alır unutmayın.
7. Aracı, özel anahtarı kullanarak parola şifresini çözer.
8. Kullanıcı Aracısı doğrular ve hangi Active Directory Federasyon Hizmetleri (AD FS) için benzer bir mekanizmadır standart Windows API'leri kullanarak parolası Active Directory karşı kullanır. Kullanıcı adı ya da şirket içi varsayılan kullanıcı adı, genellikle olabilir `userPrincipalName`, ya da Azure AD Connect içinde yapılandırılmış başka bir öznitelik (olarak bilinen `Alternate ID`).
9. Şirket içi Active Directory etki alanı denetleyicisi (DC) isteği değerlendirir ve uygun yanıtı döndürür (başarılı, başarısız, parolanın süresi doldu veya kullanıcı kilitli) aracısı.
10. Kimlik Doğrulama Aracısı, buna karşılık, Azure AD geri bu yanıt döndürür.
11. Azure AD yanıt değerlendirir ve kullanıcının uygun şekilde yanıt verir. Kullanıcı oturum açtıktan hemen veya için Azure çok faktörlü kimlik doğrulama isteklerini, örneğin, Azure AD.
12. Kullanıcı oturum açma başarılı olursa, kullanıcı uygulamasına erişebilir.

Aşağıdaki diyagramda, tüm bileşenler ve adımlar gösterilmektedir:

![Doğrudan Kimlik Doğrulama](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Sonraki adımlar
- [Geçerli sınırlamalar](active-directory-aadconnect-pass-through-authentication-current-limitations.md): hangi senaryoları desteklenir ve hangilerinin olmayan öğrenin.
- [Hızlı Başlangıç](active-directory-aadconnect-pass-through-authentication-quick-start.md): Azure AD doğrudan kimlik doğrulamasını başlamak ve çalıştırmak.
- [Akıllı kilitleme](../authentication/howto-password-smart-lockout.md): kullanıcı hesapları korumak için Kiracı akıllı kilitleme yeteneği yapılandırın.
- [Sık sorulan sorular](active-directory-aadconnect-pass-through-authentication-faq.md): Bul için sık sorulan sorulara yanıtlar.
- [Sorun giderme](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): doğrudan kimlik doğrulama özelliği ile ortak sorunları çözmeyi öğrenin.
- [Güvenlik derinlemesine](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): doğrudan kimlik doğrulama özelliği hakkında ayrıntılı teknik bilgi alın.
- [Azure AD sorunsuz SSO](active-directory-aadconnect-sso.md): tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): yeni özellik istekleri dosya için Azure Active Directory Forumu kullanın.

