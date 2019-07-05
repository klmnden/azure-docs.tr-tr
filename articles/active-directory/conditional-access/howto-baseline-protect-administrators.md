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
ms.openlocfilehash: 4474283b9a233e39497cd05f0f04ea0984f02401
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67560945"
---
# <a name="baseline-policy-require-mfa-for-admins-preview"></a>Temel ilke: Mfa'yı (Önizleme) yöneticileri için gerekli

Kullanıcılara ayrıcalıklı hesaplara erişim ortamınıza sınırsız erişimi vardır. Bu hesaplara sahip güç nedeniyle, bunları özel dikkatli düşünmelisiniz. Ayrıcalıklı hesapların geliştirmek için bir ortak oturum açma için kullanıldığında, daha güçlü bir form Hesap doğrulama gerektirecek şekilde yöntemidir. Azure Active Directory'ye multi factor authentication (MFA) zorunlu daha güçlü bir hesap doğrulama alabilirsiniz.

**Mfa'yı (Önizleme) yöneticileri için gerekli** olduğu bir [temel ilke](concept-baseline-protection.md) ayrıcalıklı yönetici aşağıdaki rollerden birinde oturum açtığı her seferinde mfa'yı gerektirir:

* Genel yönetici
* SharePoint yöneticisi
* Exchange Yöneticisi
* Koşullu Erişim Yöneticisi
* Güvenlik yöneticisi
* Yardım Masası Yöneticisi / parola Yöneticisi
* Faturalama yöneticisi
* Kullanıcı Yöneticisi

Yöneticiler ilkesi için gerekli MFA etkinleştirme sırasında yukarıdaki dokuz yönetici rolleri kimlik doğrulayıcı uygulamasını kullanarak MFA'ya kaydetmeniz gerekir. MFA kayıt tamamlandıktan sonra Yöneticiler, oturum açma tek her seferinde MFA gerçekleştirmeniz gerekir.

## <a name="deployment-considerations"></a>Dağıtma konuları

Çünkü **yöneticileri (Önizleme) için MFA gerektiren** ilkenin geçerli tüm kritik yöneticilere, çeşitli konuları sorunsuz bir dağıtım sağlamak için yapılması gerekir. Kullanıcılar ve uygulamalar ve modern kimlik doğrulamayı desteklemeyen, kuruluşunuz tarafından kullanılan istemcilerin yanı sıra MFA'yı gerçekleştirmemelisiniz veya Azure AD'de hizmet ilkeleri tanımlayan bu konuları içerir.

### <a name="legacy-protocols"></a>Eski protokolleri

Eski bir kimlik doğrulama protokolleri (IMAP, SMTP, POP3, vb.), kimlik doğrulama istekleri için posta istemcileri tarafından kullanılır. Bu protokollerin mfa'yı desteklemez. Çoğu Microsoft tarafından görülen hesabı anladığınızda, eski protokolleri mfa'yı atlamak deneyen saldırıları gerçekleştiren kötü aktörleri kaynaklanır. Bir yönetici hesabı ile oturum açarken MFA gereklidir ve mfa'yı atlamak kötü aktörleri belirtemiyor emin olmak için bu ilkenin eski protokolleri arasından Yönetici hesaplarına yapılan tüm kimlik doğrulama isteklerini engeller.

> [!WARNING]
> Bu ilke etkinleştirmeden önce eski kimlik doğrulama protokolleri yöneticilerinize kullanmadığınız emin olun. Makaleye göz atın [nasıl yapılır: Azure AD koşullu erişim ile eski kimlik blok](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) daha fazla bilgi için.

## <a name="enable-the-baseline-policy"></a>Temel ilke etkinleştir

İlke **temel ilke: Mfa'yı (Önizleme) yöneticileri için gerekli** önceden yapılandırılmış olarak gelir ve Azure portalında koşullu erişim dikey penceresine gittiğinizde en üstünde gösterilir.

Bu ilkeyi etkinleştirmek ve yöneticileriniz korumak için:

1. Oturum **Azure portalında** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak.
1. Gözat **Azure Active Directory** > **koşullu erişim**.
1. İlkeler listesinde seçin **temel ilke: Mfa'yı (Önizleme) yöneticileri için gerekli**.
1. Ayarlama **ilkesini etkinleştir** için **ilkeyi hemen kullan**.
1. Tıklayın **Kaydet**.

> [!WARNING]
> Bir seçenek oluştu **ilke gelecekte otomatik olarak etkinleştir** bu ilkesinin ne zaman önizlemede. Ani kullanıcı etkisini en aza indirmek için bu seçeneği kaldırdık. Kullanılabilir olduğunda bu seçeneği işaretlediyseniz **ilke kullanmayın** otomatik olarak artık seçili. Bunlar bu temel ilke kullanmak istiyorsanız, bunu etkinleştirmek için yukarıdaki adımları bakın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

* [Koşullu erişim temel koruma ilkeleri](concept-baseline-protection.md)
* [Kimlik altyapınızın güvenliğini sağlamak için beş adım](../../security/azure-ad-secure-steps.md)
* [Azure Active Directory'de koşullu erişim nedir?](overview.md)
