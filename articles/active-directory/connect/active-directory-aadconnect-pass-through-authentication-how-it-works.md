---
title: "Azure AD Connect: Doğrudan kimlik doğrulama - nasıl çalışır? | Microsoft Belgeleri"
description: "Bu makalede, Azure Active Directory doğrudan kimlik doğrulaması nasıl çalıştığı açıklanmaktadır."
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
ms.openlocfilehash: c7863e38671349b6424ee08330da8aaa49cb2a70
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-technical-deep-dive"></a>Azure Active Directory doğrudan kimlik doğrulaması: Teknik derinlemesine bakış
Aşağıdaki makalede Azure AD doğrudan kimlik doğrulama nasıl çalıştığını bir genel bakıştır.  Ayrıntılı teknik ve güvenlik bilgileri için bkz: [ **güvenlik derinlemesine** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) makalesi.

## <a name="how-does-azure-active-directory-pass-through-authentication-work"></a>Azure Active Directory doğrudan kimlik doğrulaması nasıl çalışır?

Bir kullanıcı Azure Active Directory (Azure AD) tarafından güvenli hale getirilmiş bir uygulamaya oturum girişiminde bulunduğunda ve Kiracı'geçişli kimlik doğrulaması etkinleştirilirse, aşağıdaki adımlardan oluşur:

1. Kullanıcı bir uygulamaya erişmeye çalıştığında (örneğin, Outlook Web App - https://outlook.office365.com/owa/).
2. Kullanıcı zaten oturum açmamış, kullanıcının Azure AD oturum açma sayfasına yönlendirilir.
3. Kullanıcının kullanıcı adı ve parola Azure AD oturum açma sayfasına girer ve "Oturum" düğmesine tıklar.
4. Oturum açma isteği alırken azure AD kullanıcı adı ve parola (bir ortak anahtar kullanılarak şifrelenmiş) bulunan bir sıra yerleştirir.
5. Bir şirket içi kimlik doğrulama Aracısı sıradan şifrelenmiş parola ve kullanıcı adını alır.
6. Aracı özel anahtarını kullanarak parola şifresini çözer.
7. Aracıyı sonra kullanıcı adı ve parola standart Windows API'larını (Active Directory Federasyon Hizmetleri tarafından kullanılan benzer mekanizması) kullanarak Active Directory karşı doğrular. Kullanıcı adı ya da şirket içi varsayılan kullanıcı adı olabilir (genellikle `userPrincipalName`) veya Azure AD Connect içinde yapılandırılmış başka bir öznitelik (olarak bilinen `Alternate ID`).
8. Şirket içi Active Directory etki alanı denetleyicisi (DC) sonra isteği değerlendirir ve uygun yanıtı döndürür (başarılı, başarısız, parolanın süresi doldu veya kullanıcı kilitli) aracısı.
9. Kimlik Doğrulama Aracısı, buna karşılık, Azure AD geri bu yanıt döndürür.
10. Azure AD yanıt değerlendirir ve kullanıcının uygun - Örneğin, kullanıcı oturum açtıktan hemen ya da çok faktörlü kimlik doğrulama (MFA) için istekleri yanıtlar.
11. Kullanıcı oturum açma başarılı olursa, kullanıcı uygulamaya erişim mümkün değil.

Aşağıdaki diyagram, tüm bileşenler ve adımlarını gösterir.

![Doğrudan Kimlik Doğrulama](./media/active-directory-aadconnect-pass-through-authentication/pta2.png)

## <a name="next-steps"></a>Sonraki adımlar
- [**Geçerli sınırlamalar** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -hangi senaryoları desteklenir ve hangilerinin olmayan öğrenin.
- [**Hızlı Başlangıç** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - hale getirmek ve Azure AD doğrudan kimlik doğrulama çalıştıran.
- [**Akıllı kilitleme** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -Kiracı üzerinde yapılandırma akıllı kilitleme özelliğini kullanıcı hesaplarını korumak için.
- [**Sık sorulan sorular** ](active-directory-aadconnect-pass-through-authentication-faq.md) -sık sorulan sorulara yanıtlar.
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
- [**Güvenlik derinlemesine** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -özelliği hakkında ek ayrıntılı teknik bilgi.
- [**Azure AD sorunsuz SSO** ](active-directory-aadconnect-sso.md) -tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleri dosyalama için.
