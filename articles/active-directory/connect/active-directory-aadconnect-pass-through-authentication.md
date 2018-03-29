---
title: 'Azure AD Connect: Doğrudan kimlik doğrulama | Microsoft Docs'
description: Bu makalede, Azure Active Directory (Azure AD) doğrudan kimlik doğrulama ve nasıl kullanıcıların parolalarını şirket içi Active Directory karşı doğrulayarak Azure AD oturum açma işlemleri sağlar açıklanmaktadır.
services: active-directory
keywords: Azure AD Connect doğrudan kimlik doğrulama nedir, Active Directory, Azure AD, SSO için gerekli bileşenleri yüklemek çoklu oturum açma
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: billmath
ms.openlocfilehash: d19e63e10f2d42d97bb6fabca9c9e47028cbaf39
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="user-sign-in-with-azure-active-directory-pass-through-authentication"></a>Kullanıcı oturum açma ile Azure Active Directory doğrudan kimlik doğrulaması

## <a name="what-is-azure-active-directory-pass-through-authentication"></a>Azure Active Directory doğrudan kimlik doğrulaması nedir?

Azure Active Directory (Azure AD) doğrudan kimlik doğrulaması, kullanıcıların şirket içi ve bulut tabanlı uygulamalar aynı parola kullanarak oturum açmak olanak tanır. Bu özellik, kullanıcılarınızın daha iyi bir deneyim - anımsaması, daha az bir parola sağlar ve kullanıcılarınızın oturum açma unuttunuz olasılığı olduğundan BT Yardım Masası maliyetlerini azaltır. Azure AD kullanarak kullanıcılar oturum açtığında, bu özellik kullanıcıların parolalarını şirket içi Active Directory'nizi karşı doğrudan doğrular.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Bu özellik için bir alternatiftir [Azure AD parola karması eşitlemesi](active-directory-aadconnectsync-implement-password-hash-synchronization.md), bulut kimlik doğrulaması kuruluşlara aynı avantajı sağlar. Ancak, belirli kuruluşlardaki güvenlik ve uyumluluk ilkeleri kullanıcıların parolalarını bile iç sınırlarının dışında bir karma form göndermek için bu kuruluşların izin vermez. Doğrudan kimlik doğrulaması gibi kuruluş için doğru çözümdür.

![Azure AD doğrudan kimlik doğrulama](./media/active-directory-aadconnect-pass-through-authentication/pta1.png)

Doğrudan kimlik doğrulaması ile birleştirebilirsiniz [sorunsuz çoklu oturum açma](active-directory-aadconnect-sso.md) özelliği. Kullanıcılarınızın şirket makinelerinin Kurumsal ağınızdaki uygulamaları erişirken bu şekilde, bunlar oturum açmak için parolalarını yazın gerek yoktur.

## <a name="key-benefits-of-using-azure-ad-pass-through-authentication"></a>Azure AD geçişli kimlik doğrulaması kullanarak avantajları

- *Mükemmel bir kullanıcı deneyimi*
  - Kullanıcılar, şirket içi ve bulut tabanlı uygulamalar imzalamak için aynı parolayı kullanın.
  - Kullanıcıların BT Yardım Masası çözümleme parola ile ilgili sorunlar için Konuşmayı daha az süre beklemesini.
  - Kullanıcıların tamamlayabilir [Self Servis parola yönetimi](../active-directory-passwords-overview.md) bulutta görevler.
- *Kolay dağıtma ve yönetme*
  - İçi karmaşık dağıtımlar veya ağ yapılandırması için gerekli.
  - İçi yüklü olması için yalnızca bir basit Aracısı gerekir.
  - Yönetim olmamasıdır. Aracı geliştirmeler ve hata düzeltmeleri otomatik olarak alır.
- *Güvenlik*
  - Şirket içi parolaları hiçbir zaman bulut herhangi bir biçimde depolanır.
  - Aracı yalnızca ağınıza giden bağlantılar sağlar. Bu nedenle, aracıyı DMZ olarak da bilinen bir çevre ağında yükleme gereksinimi yoktur.
  - Kullanıcı hesapları ile sorunsuz çalışarak korur [Azure AD koşullu erişim ilkeleri](../active-directory-conditional-access-azure-portal.md), çok faktörlü kimlik doğrulama (MFA) de dahil olmak üzere ve göre [deneme yanılma parola saldırıları filtreleme](active-directory-aadconnect-pass-through-authentication-smart-lockout.md).
- *Yüksek oranda kullanılabilir*
  - Ek aracıların, oturum açma isteklerinin yüksek kullanılabilirlik sağlamak için birden çok şirket içi sunucularda yüklenebilir.

## <a name="feature-highlights"></a>Özellik vurgular

- Kullanıcı oturum açma tüm web tarayıcı tabanlı uygulamalar ve Microsoft Office kullanan istemci uygulamaları destekleyen [modern kimlik doğrulaması](https://aka.ms/modernauthga).
- Oturum açma kullanıcı adları, şirket içi varsayılan kullanıcı olabilir (`userPrincipalName`) veya Azure AD Connect içinde yapılandırılmış başka bir öznitelik (olarak bilinen `Alternate ID`).
- Özelliği ile sorunsuz çalışır [koşullu erişim](../active-directory-conditional-access-azure-portal.md) özellikleri gibi çok faktörlü kimlik doğrulaması (kullanıcılarınızın güvenli hale getirmek için MFA).
- Bulut tabanlı ile tümleşik [Self Servis parola yönetimi](../active-directory-passwords-overview.md), parola geri yazma için de dahil olmak üzere şirket içi Active Directory ve parola koruması banning tarafından yaygın olarak kullanılan parolalar.
- Birden çok orman ortamlarına AD ormanlar arasında orman güvenleri varsa desteklenir ve ad soneki yönlendirmesi doğru yapılandırılmışsa.
- Boş bir özelliktir ve kullanmak için Azure AD Ücretli tüm sürümleri olması gerekmez.
- Aracılığıyla etkinleştirilebilir [Azure AD Connect](active-directory-aadconnect.md).
- Dinler ve parola doğrulaması isteklerine yanıt verdiğini bir basit şirket içi Aracısı kullanır.
- Birden çok aracı yükleme, oturum açma isteklerinin yüksek kullanılabilirlik sağlar.
- Bu [korur](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) parola saldırılarını bulutta şirket içi hesaplarınızı karşı deneme yanılma yoluyla yapılan zorla.

## <a name="next-steps"></a>Sonraki adımlar

- [**Hızlı Başlangıç** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) - hale getirmek ve Azure AD doğrudan kimlik doğrulama çalıştıran.
- [**Akıllı kilitleme** ](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) -Kiracı üzerinde yapılandırma akıllı kilitleme özelliğini kullanıcı hesaplarını korumak için.
- [**Geçerli sınırlamalar** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -hangi senaryoları desteklenir ve hangilerinin olmayan öğrenin.
- [**Teknik derinlemesine** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -bu özellik nasıl çalıştığını anlayın.
- [**Sık sorulan sorular** ](active-directory-aadconnect-pass-through-authentication-faq.md) -sık sorulan sorulara yanıtlar.
- [**Sorun giderme** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -özelliği ile ilgili genel sorunları çözmeyi öğrenin.
- [**Güvenlik derinlemesine** ](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -özelliği hakkında ek ayrıntılı teknik bilgi.
- [**Azure AD sorunsuz SSO** ](active-directory-aadconnect-sso.md) -tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleri dosyalama için.
