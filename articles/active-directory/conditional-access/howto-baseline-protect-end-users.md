---
title: Son kullanıcı protection (Önizleme) - Azure Active Directory temel ilkeleri
description: Kullanıcılar için çok faktörlü kimlik doğrulaması gerektirmek için koşullu erişim ilkesi
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
ms.openlocfilehash: f2644e0e35139ac470b89f6af1b95cf510f60a0a
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561013"
---
# <a name="baseline-policy-end-user-protection-preview"></a>Temel ilke: Son kullanıcı protection (Önizleme)

Yönetici hesaplarını multi-Factor authentication (MFA) ile koruma gerektiren yalnızca hesapları olduğunu düşünmek eğilimindedir. Yöneticiler, geniş hassas bilgilere erişimi ve abonelik genelindeki ayarları için değişiklik yapabilirsiniz. Ancak, hedef son kullanıcılara kötü aktörleri eğilimindedir. Erişim kazandıktan sonra bu kötü aktörleri, ayrıcalıklı bilgileri özgün hesap sahibi adına erişim isteğinde ya da kuruluş genelinde bir kimlik avı saldırı gerçekleştirmek için tüm dizin indirin. Tüm kullanıcılar geliştirmek için bir ortak yöntemi Hesap doğrulama, çok faktörlü kimlik doğrulamasını (MFA) gibi daha güçlü bir form gerektirmektir.

Güvenlik ve kullanılabilirlik makul bir denge sağlamak için oturum açma tek her zaman kullanıcılara istemde olmamalıdır. Aynı konumda aynı bir CİHAZDAN oturum açma gibi normal kullanıcı davranışı, kimlik doğrulama isteklerini tehlike düşük şansına sahip olabilirsiniz. Yalnızca oturum açma, riskli olarak kabul edilen ve kötü bir aktör özelliklerini göster işlemleri ile MFA zorluklarının sorulması gerekir.

Son kullanıcı korumadır bir risk tabanlı MFA [temel ilke](concept-baseline-protection.md) tüm yönetici rolleri dahil olmak üzere, bir dizindeki tüm kullanıcılar korur. Bu ilkeyi etkinleştirmek, tüm kullanıcıların kimlik doğrulayıcı uygulamasını kullanarak MFA'ya kaydetmeniz gerektirir. Kullanıcılar daha sonra kullanıcılar için mfa'yı kaydoluncaya kadar açmasını engellenir 14 gün için MFA kayıt istemi yoksayabilirsiniz. Kullanıcılar için mfa'yı kaydedildikten sonra MFA için yalnızca riskli oturum açma girişimleri sırasında istenir. Güvenliği aşılan kullanıcı hesapları kullanarak parolalarını sıfırlayabilir ve risk olayı kapatıldı kadar engellenir.

> [!NOTE]
> Bu ilke, Konuk hesapları dahil olmak üzere tüm kullanıcılara uygulanır ve tüm uygulamalarda oturum açtığında değerlendirilir.

## <a name="recovering-compromised-accounts"></a>Hesapları tehlikeye kurtarma

Müşterilerimizin korumaya yardımcı olmak için genel kullanıma açık kullanıcı adı/parola çiftleri Microsoft'un kimlik bilgileri sızdırılan hizmetini bulur. Kullanıcılarımızın birini Eşleşirlerse, biz o hesabın hemen güvenli hale getirin. Kullanıcıların tanımlanmış kimlik bilgileri sızdırılan sahip olacak şekilde, tehlikeye onaylanır. Bu kullanıcılar, parolalarını sıfırlama kadar açmasını engellenir.

Bir Azure AD Premium lisansı atanmış kullanıcılar, kendi dizinde özelliği etkinse, Self Servis parola sıfırlama (SSPR) aracılığıyla erişim geri yükleyebilirsiniz. Engellenmiş duruma premium lisansı olmayan kullanıcılar, bir yöneticinin el ile parola sıfırlama işlemini gerçekleştirmesine ve bayrak eklenen kullanıcı risk olayı Kapat başvurmanız gerekir.

### <a name="steps-to-unblock-a-user"></a>Bir kullanıcının engelini kaldırma adımlarını

Kullanıcının oturum açma günlükleri inceleyerek kullanıcı ilke tarafından engellendi onaylayın.

1. Yönetici oturum açmak gereken **Azure portalında** gidin **Azure Active Directory** > **kullanıcılar** > kullanıcının adına tıklayıp gidin Oturum açma işlemleri.
1. Parola sıfırlama engellenen bir kullanıcı başlatmak için yönetici gitmek gerek duyduğu **Azure Active Directory** > **risk için işaretlenen kullanıcılar**
1. Kullanıcı hesabı oturum açma kullanıcının son etkinlik hakkındaki bilgileri görüntülemek için engellendi tıklayın.
1. Sonraki oturum açtıktan sonra değiştirmesi gereken geçici bir parola atanacak parolayı Sıfırla'a tıklayın.
1. Kullanıcının risk puanını sıfırlamak için tüm olayları Kapat'ı tıklatın.

Artık kullanıcı oturum açın, kullanıcının parolasını sıfırlamak ve uygulamaya erişim.

## <a name="deployment-considerations"></a>Dağıtma konuları

Çünkü **son kullanıcı korumasını** çeşitli konuları sorunsuz bir dağıtım sağlamak için yapılması gereken, dizininizdeki tüm kullanıcılar için ilke uygulanır. Kullanıcılar ve uygulamalar ve modern kimlik doğrulamayı desteklemeyen, kuruluşunuz tarafından kullanılan istemcilerin yanı sıra MFA'yı gerçekleştirmemelisiniz veya Azure AD'de hizmet ilkeleri tanımlayan bu konuları içerir.

### <a name="legacy-protocols"></a>Eski protokolleri

Eski bir kimlik doğrulama protokolleri (IMAP, SMTP, POP3, vb.), kimlik doğrulama istekleri için posta istemcileri tarafından kullanılır. Bu protokollerin mfa'yı desteklemez.  Çoğu Microsoft tarafından görülen hesabı anladığınızda, eski protokolleri mfa'yı atlamak deneyen saldırıları gerçekleştiren kötü aktörleri kaynaklanır. Bir hesaba oturum açarken MFA gereklidir ve mfa'yı atlamak kötü aktörleri belirtemiyor emin olmak için bu ilkenin eski protokolleri arasından Yönetici hesaplarına yapılan tüm kimlik doğrulama isteklerini engeller.

> [!WARNING]
> Bu ilke etkinleştirmeden önce eski kimlik doğrulama protokolleri, kullanıcılarınızın kullanmadığınız emin olun. Makaleye göz atın [nasıl yapılır: Azure AD koşullu erişim ile eski kimlik blok](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) daha fazla bilgi için.

## <a name="enable-the-baseline-policy"></a>Temel ilke etkinleştir

İlke **temel ilke: Son kullanıcı protection (Önizleme)** önceden yapılandırılmış olarak gelir ve Azure portalında koşullu erişim dikey penceresine gittiğinizde en üstünde gösterilir.

Bu ilkeyi etkinleştirmek ve kullanıcılarınızı korumak için:

1. Oturum **Azure portalında** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak.
1. Gözat **Azure Active Directory** > **koşullu erişim**.
1. İlkeler listesinde seçin **temel ilke: Son kullanıcı protection (Önizleme)** .
1. Ayarlama **ilkesini etkinleştir** için **ilkeyi hemen kullan**.
1. Tıklayın **Kaydet**.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

* [Koşullu erişim temel koruma ilkeleri](concept-baseline-protection.md)
* [Kimlik altyapınızın güvenliğini sağlamak için beş adım](../../security/azure-ad-secure-steps.md)
* [Azure Active Directory'de koşullu erişim nedir?](overview.md)
