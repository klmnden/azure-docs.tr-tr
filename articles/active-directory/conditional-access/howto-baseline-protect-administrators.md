---
title: Temel ilke yöneticilerin - Azure Active Directory MFA gerektirir
description: Yöneticiler için çok faktörlü kimlik doğrulaması gerektirmek için koşullu erişim ilkesi
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: a1ce48126c3e8867ac7f2696d8cf7db992a9a60a
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66003289"
---
# <a name="baseline-policy-require-mfa-for-admins"></a>Temel ilke: Yöneticiler için MFA gerektirme

Kullanıcılara ayrıcalıklı hesaplara erişim ortamınıza sınırsız erişimi vardır. Bu hesaplara sahip güç nedeniyle, bunları özel dikkatli düşünmelisiniz. Ayrıcalıklı hesapların geliştirmek için bir ortak oturum açma için kullanıldığında, daha güçlü bir form Hesap doğrulama gerektirecek şekilde yöntemidir. Azure Active Directory'ye multi factor authentication (MFA) zorunlu daha güçlü bir hesap doğrulama alabilirsiniz.

**Yöneticiler için mfa'yı gerekli** olduğu bir [temel ilke](concept-baseline-protection.md) ayrıcalıklı yönetici aşağıdaki rollerden birinde oturum açtığı her seferinde mfa'yı gerektirir:

* Genel yönetici
* SharePoint yöneticisi
* Exchange yöneticisi
* Koşullu erişim yöneticisi
* Güvenlik yöneticisi
* Yardım Masası Yöneticisi / parola Yöneticisi
* Faturalama yöneticisi
* Kullanıcı yöneticisi

Yöneticiler ilkesi için gerekli MFA etkinleştirme sırasında yukarıdaki dokuz yönetici rolleri kimlik doğrulayıcı uygulamasını kullanarak MFA'ya kaydetmeniz gerekir. MFA kayıt tamamlandıktan sonra Yöneticiler, oturum açma tek her seferinde MFA gerçekleştirmeniz gerekir.

![Yöneticiler için temel İlkesi MFA gerektirme](./media/howto-baseline-protect-administrators/baseline-policy-require-mfa-for-admins.png)

## <a name="deployment-considerations"></a>Dağıtma konuları

Çünkü **yöneticileri için MFA gerektiren** ilkenin geçerli tüm kritik yöneticilere, çeşitli konuları sorunsuz bir dağıtım sağlamak için yapılması gerekir. Kullanıcılar ve uygulamalar ve modern kimlik doğrulamayı desteklemeyen, kuruluşunuz tarafından kullanılan istemcilerin yanı sıra MFA'yı gerçekleştirmemelisiniz veya Azure AD'de hizmet ilkeleri tanımlayan bu konuları içerir.

### <a name="legacy-protocols"></a>Eski protokolleri

Eski bir kimlik doğrulama protokolleri (IMAP, SMTP, POP3, vb.), kimlik doğrulama istekleri için posta istemcileri tarafından kullanılır. Bu protokollerin mfa'yı desteklemez. Çoğu Microsoft tarafından görülen hesabı anladığınızda, eski protokolleri mfa'yı atlamak deneyen saldırıları gerçekleştiren kötü aktörleri kaynaklanır. Bir yönetici hesabı ile oturum açarken MFA gereklidir ve mfa'yı atlamak kötü aktörleri belirtemiyor emin olmak için bu ilkenin eski protokolleri arasından Yönetici hesaplarına yapılan tüm kimlik doğrulama isteklerini engeller.

> [!WARNING]
> Bu ilke etkinleştirmeden önce eski kimlik doğrulama protokolleri yöneticilerinize kullanmadığınız emin olun. Makaleye göz atın [nasıl yapılır: Azure AD koşullu erişim ile eski kimlik blok](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) daha fazla bilgi için.

### <a name="user-exclusions"></a>Kullanıcı dışlamaları

Bu temel ilke kullanıcılar dışında seçeneği sağlar. Kiracınız için ilke etkinleştirmeden önce aşağıdaki hesapları hariç öneririz:

* **Acil Durum erişim** veya **sonu cam** Kiracı genelinde hesap kilitleme önlemek için hesaplar. Tüm Yöneticiler, kiracınızın dışında kilitli olduğundan olası senaryoda, Acil Durum erişimi yönetici hesabınızın erişim kurtarmak için Kiracı alma adımları oturum kullanılabilir.
   * Daha fazla bilgi makalesinde bulunabilir [Azure AD'de Acil Durum erişim hesapları yönetme](../users-groups-roles/directory-emergency-access.md).
* **Hizmet hesapları** ve **servis ilkeleri**, Azure AD Connect eşitleme hesabı gibi. Hizmet, belirli bir kullanıcıya bağlı değil, etkileşimli olmayan hesaplar hesaplarıdır. Bunlar genellikle arka uç Hizmetleri tarafından kullanılan ve uygulamalar için programlı erişim izni. Hizmet hesapları, MFA programlı bir şekilde tamamlanamıyor beri hariç tutulması gerekir.
   * Kuruluşunuz, betikleri veya kodları kullanımda bu hesapları varsa, bunları ile değiştirmeyi göz önüne alın [yönetilen kimlikleri](../managed-identities-azure-resources/overview.md). Geçici bir çözüm, bu belirli hesapların temel ilkesinden hariç tutabilirsiniz.
* Sahip değil veya akıllı telefonunuz kullanmanız mümkün olmayacaktır kullanıcılar.
   * Bu ilke, Yöneticiler, Microsoft Authenticator uygulamasını kullanarak MFA'ya kaydetmeniz gerektirir.

## <a name="enable-the-baseline-policy"></a>Temel ilke etkinleştir

İlke **temel ilke: Yöneticiler için mfa'yı gerekli** önceden yapılandırılmış olarak gelir ve Azure portalında koşullu erişim dikey penceresine gittiğinizde en üstünde gösterilir.

Bu ilkeyi etkinleştirmek ve yöneticileriniz korumak için:

1. Oturum **Azure portalında** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak.
1. Gözat **Azure Active Directory** > **koşullu erişim**.
1. İlkeler listesinde seçin **temel ilke: Yöneticiler için mfa'yı gerekli**.
1. Ayarlama **ilkesini etkinleştir** için **ilkeyi hemen kullan**.
1. Herhangi bir kullanıcı özel tıklayarak Ekle **kullanıcılar** > **dışlanan kullanıcılar seçin** ve hariç tutulması gerektiğini kullanıcıları seçme. Tıklayın **seçin** ardından **Bitti**.
1. Tıklayın **Kaydet**.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

* [Koşullu erişim temel koruma ilkeleri](concept-baseline-protection.md)
* [Kimlik altyapınızın güvenliğini sağlamak için beş adım](../../security/azure-ad-secure-steps.md)
* [Azure Active Directory'de koşullu erişim nedir?](overview.md)
