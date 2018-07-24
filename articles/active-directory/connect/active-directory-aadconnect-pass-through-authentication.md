---
title: 'Azure AD Connect: Geçişli kimlik doğrulaması | Microsoft Docs'
description: Bu makalede, Azure Active Directory (Azure AD) geçişli kimlik doğrulaması ve nasıl kullanıcıların parolalarını şirket içi Active Directory'de doğrulayarak Azure AD oturum açma işlemleri imkan açıklanır.
services: active-directory
keywords: Azure AD Connect geçişli kimlik doğrulaması nedir, Active Directory, Azure AD SSO için gerekli bileşenleri yükleme çoklu oturum açma
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
ms.openlocfilehash: dfee42f813989da2333720ac92313344343d57a7
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214038"
---
# <a name="user-sign-in-with-azure-active-directory-pass-through-authentication"></a>Kullanıcı oturum açma ile Azure Active Directory geçişli kimlik doğrulaması

## <a name="what-is-azure-active-directory-pass-through-authentication"></a>Azure Active Directory geçişli kimlik doğrulaması nedir?

Azure Active Directory (Azure AD) geçişli kimlik doğrulaması hem şirket içi hem de bulut tabanlı uygulamalarda parolalarla oturum açmasını sağlar. Bu özellik, kullanıcılarınızın - anımsamak için daha az bir parola daha iyi bir deneyim sağlar ve kullanıcılarınızın oturum açmak nasıl oluşturabileceğinize değil daha olası olduğundan, BT Yardım Masası maliyetlerini azaltır. Bu özellik, kullanıcılar Azure AD kullanarak oturum açtığında, kullanıcıların parolalarını şirket içi Active Directory'nizde doğrudan karşı doğrular.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Bu özellik için bir alternatiftir [Azure AD parola karması eşitleme](active-directory-aadconnectsync-implement-password-hash-synchronization.md), kuruluşlar için bulut kimlik doğrulaması aynı avantajı sağlar. Ancak, bazı kuruluşlar kendi şirket içi Active Directory güvenlik ve parola ilkelerini zorlamak isteyen tercih edebilir geçişli kimlik doğrulaması kullanın. Gözden geçirme [bu kılavuzda](https://docs.microsoft.com/azure/security/azure-ad-choose-authn) bir karşılaştırma çeşitli Azure AD oturum açma yöntemleri ve kuruluşunuz için doğru oturum açma yöntemini seçin.

![Azure AD geçişli kimlik doğrulaması](./media/active-directory-aadconnect-pass-through-authentication/pta1.png)

Geçişli kimlik doğrulaması ile birleştirebilirsiniz [sorunsuz çoklu oturum açma](active-directory-aadconnect-sso.md) özelliği. Kullanıcılarınız, Kurumsal ağınızdaki Kurumsal kendi makinelerinde uygulamalarına erişirken bu şekilde oturum açmak için parola yazmak bunlar gerekmez.

## <a name="key-benefits-of-using-azure-ad-pass-through-authentication"></a>Azure AD geçişli kimlik doğrulaması kullanmanın önemli avantajları

- *Harika kullanıcı deneyimini*
  - Kullanıcılar, hem şirket içi hem de bulut tabanlı uygulamalarda oturum açmak için aynı parolayı kullanın.
  - Kullanıcılar, BT Yardım Masası çözme parolası ile ilgili sorunlar için Konuşmayı daha az zaman harcayın.
  - Kullanıcılar tamamlayabilir [Self Servis parola yönetimi](../authentication/active-directory-passwords-overview.md) bulutta görevleri.
- *Kolayca dağıtma ve yönetme*
  - Karmaşık şirket içi dağıtımları veya ağ yapılandırması için gerek yok.
  - Yüklü şirket içi olarak yalnızca bir basit Aracısı gerekir.
  - Yönetim olmamasıdır. Aracı iyileştirmeleri ve hata düzeltmeleri otomatik olarak alır.
- *Güvenlik*
  - Şirket içi parolaları, hiçbir zaman herhangi bir şekilde bulutta depolanır.
  - Aracı yalnızca ağınızdaki giden bağlantılar sağlar. Bu nedenle, aracının DMZ olarak da bilinen bir çevre ağına yüklenmesine gerek yoktur yoktur.
  - İle sorunsuz bir şekilde çalışarak kullanıcı hesaplarınızı korur [Azure AD koşullu erişim ilkeleri](../active-directory-conditional-access-azure-portal.md), multi-Factor Authentication (MFA) dahil olmak üzere [eski bir kimlik doğrulama engelleme](../active-directory-conditional-access-conditions.md) ve [ deneme yanılma parola saldırılarını filtreleme](../authentication/howto-password-smart-lockout.md).
- *Yüksek oranda kullanılabilir*
  - Ek aracılar, oturum açma isteklerinin yüksek kullanılabilirlik sağlamak üzere birden çok şirket içi sunucuya yüklenebilir.

## <a name="feature-highlights"></a>Özellik vurgular

- Kullanıcı oturum açma kullanan Microsoft Office istemci uygulamaları ve tüm web tarayıcısı tabanlı uygulamalarını destekleyen [modern kimlik doğrulaması](https://aka.ms/modernauthga).
- Oturum açma kullanıcı adları ya da şirket içi varsayılan kullanıcı adı olabilir (`userPrincipalName`) veya Azure AD Connect'e bağlanan yapılandırılmış başka bir öznitelik (olarak bilinen `Alternate ID`).
- Bu özellik ile sorunsuzca çalışır. [koşullu erişim](../active-directory-conditional-access-azure-portal.md) özellikleri gibi çok faktörlü kimlik doğrulaması (kullanıcılarınıza güvenli hale getirmek için MFA).
- Bulut tabanlı ile tümleşik [Self Servis parola yönetimi](../authentication/active-directory-passwords-overview.md), parola geri yazma için de dahil olmak üzere şirket içi Active Directory ve parola koruması engellemeyi parolaları yaygın olarak kullanılan.
- Çok ormanlı ortamları, AD ormanlar arasında orman güveni varsa desteklenir ve ad soneki yönlendirme doğru şekilde yapılandırıldıysa.
- Ücretsiz bir özelliktir ve kullanmak için Azure AD Ücretli tüm sürümleri ihtiyacınız yoktur.
- Aracılığıyla etkinleştirilebilir [Azure AD Connect](active-directory-aadconnect.md).
- Dinler ve parola doğrulaması isteklerine yanıt verip şirket basit aracı kullanır.
- Birden çok aracı yükleme, oturum açma isteklerinin yüksek kullanılabilirlik sağlar.
- Bunu [korur](../authentication/howto-password-smart-lockout.md) parola saldırılarını bulutta şirket içi hesaplarınızı karşı deneme yanılma yoluyla yapılan zorla.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı Başlangıç](active-directory-aadconnect-pass-through-authentication-quick-start.md) - getirmek ve Azure AD geçişli kimlik doğrulaması çalıştırma.
- [AD FS'den doğrudan kimlik doğrulamaya geçiş](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx) -geçişli kimlik doğrulaması için AD FS (veya diğer Federasyon teknolojileri) geçirmek için ayrıntılı bir kılavuz.
- [Akıllı kilitleme](../authentication/howto-password-smart-lockout.md) -kullanıcı hesapları korumak için kiracınızı yapılandırın akıllı kilitleme özelliği.
- [Geçerli sınırlamalar](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -hangi senaryolar desteklenir ve hangilerinin olmayan öğrenin.
- [Teknik yakından bakışın](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -bu özelliği nasıl çalıştığını anlayın.
- [Sık sorulan sorular](active-directory-aadconnect-pass-through-authentication-faq.md) -sık sorulan soruların yanıtları.
- [Sorun giderme](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) -özelliği ile ilgili yaygın sorunları çözmeyi öğrenin.
- [Güvenliğe derinlemesine bakış](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) -bu özellik hakkında daha fazla ayrıntılı teknik bilgi.
- [Azure AD sorunsuz çoklu oturum açma](active-directory-aadconnect-sso.md) -tamamlayıcı bu özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleriniz dosyalama için.
