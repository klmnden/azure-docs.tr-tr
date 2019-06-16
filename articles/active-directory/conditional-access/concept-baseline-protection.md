---
title: Koşullu erişim temel koruma ilkeleri - Azure Active Directory
description: Kuruluşların yaygın saldırılardan korumak için temel koşullu erişim ilkeleri
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
ms.openlocfilehash: 39a591a335d022ef7b2b99fdec930ddf0496cd47
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112584"
---
# <a name="what-are-baseline-policies"></a>Temel ilkeleri nelerdir?

Temel ilkeler, kuruluş pek çok ortak saldırılarına karşı korumaya yardımcı olmak önceden tanımlanmış ilkeleri kümesidir. Bu ortak saldırıları parola ilaç, yeniden yürütme ve kimlik avı içerebilir. Temel ilkeleri, Azure AD tüm sürümlerinde kullanılabilir. Kimlik tabanlı saldırılar, son birkaç yılda artarken silinmiş olduğundan Microsoft bu temel koruma ilkeleri herkesin kullanımına. Tüm kuruluşlar, güvenliği etkinleştirilmiş olarak temel düzeyde olmasını sağlamak için bu dört ilkeler amacı olan ek bir maliyet.  

Özelleştirilmiş koşullu erişim ilkelerini yönetme, bir Azure AD Premium lisansı gerektirir.

## <a name="baseline-policies"></a>Ana hat ilkeleri

![Azure portalında koşullu erişim temel ilkeleri](./media/concept-baseline-protection/conditional-access-baseline-policies.png)

Kuruluşların etkinleştirebilirsiniz dört temel ilkeleri vardır:

* [Yöneticiler için MFA gerektirme](howto-baseline-protect-administrators.md)
* [Son kullanıcı protection (Önizleme)](howto-baseline-protect-end-users.md)
* [Blok eski kimlik doğrulama (Önizleme)](howto-baseline-protect-legacy-auth.md)
* [MFA istemek için Hizmet Yönetimi (Önizleme)](howto-baseline-protect-azure.md)

Bu ilkelerin dört eski Office Masaüstü istemcileri POP ve IMAP gibi eski bir kimlik doğrulama akışları etkiler.

### <a name="require-mfa-for-admins"></a>Yöneticiler için MFA gerektirme

Güç ve Yönetici hesaplarına sahip erişim nedeniyle özel dikkatlice değerlendirmeniz gerekir. Ayrıcalıklı hesapların geliştirmek için bir ortak yöntemi oturum açmak için kullanılan hesap doğrulama daha güçlü bir form gerektirmektir. Azure Active Directory'de kaydolun ve Azure multi-Factor Authentication'ı kullanmak Yöneticiler isteyerek daha güçlü bir hesap doğrulama alabilirsiniz.

[Yöneticiler için mfa'yı gerekli](howto-baseline-protect-administrators.md) en yüksek ayrıcalıklı Azure AD rolleri kabul şu dizin rolleri için çok faktörlü kimlik doğrulaması (MFA) gerektiren bir taban çizgisi ilke olduğunu:

* Genel yönetici
* SharePoint yöneticisi
* Exchange Yöneticisi
* Koşullu Erişim Yöneticisi
* Güvenlik yöneticisi
* Yardım Masası Yöneticisi / parola Yöneticisi
* Faturalama yöneticisi
* Kullanıcı Yöneticisi

Kuruluşunuz, betikleri veya kodları kullanımda bu hesapları varsa, bunları ile değiştirmeyi göz önüne alın [yönetilen kimlikleri](../managed-identities-azure-resources/overview.md). Geçici bir çözüm, belirli kullanıcı hesaplarını temel ilkesinden hariç tutabilirsiniz.

### <a name="end-user-protection-preview"></a>Son kullanıcı protection (Önizleme)

Yüksek ayrıcalıklı yöneticilerin saldırılarında hedeflenen yalnızca olanları değil. Kötü aktörleri normal kullanıcıları hedeflemek eğilimindedir. Erişim kazandıktan sonra bu kötü aktörleri ayrıcalıklı bilgileri özgün hesap sahibi adına erişim isteğinde veya tüm dizinde indirebilir ve bunları tüm kuruluşa bir kimlik avı saldırı gerçekleştirin. Tüm kullanıcılar geliştirmek için bir ortak bir riskli oturum açma algılandığında, daha güçlü bir form Hesap doğrulama gerektirecek şekilde yöntemidir.

**Son kullanıcı protection (Önizleme)** bir dizindeki tüm kullanıcılar tarafından korunan bir taban çizgisi ilkedir. Bu ilkeyi etkinleştirmek, tüm kullanıcıların Azure multi-Factor Authentication için 14 gün içinde kaydolmasını gerektirir. Kaydedildikten sonra kullanıcılar yalnızca riskli oturum açma girişimleri sırasında MFA için istenir. Güvenliği aşılan kullanıcı hesapları, parola sıfırlama ve işten çıkarma risk kadar engellenir.

### <a name="block-legacy-authentication-preview"></a>Blok eski kimlik doğrulama (Önizleme)

Eski bir kimlik doğrulama protokollerini (örn: IMAP, SMTP, POP3) normal şekilde kimlik doğrulaması için eski e-posta istemcileri tarafından kullanılan protokoller şunlardır. Eski protokolleri, çok faktörlü kimlik doğrulamasını desteklemez. Dizininiz için çok faktörlü kimlik doğrulaması gerektiren bir ilke olsa bile, kötü bir aktör bu eski protokollerden birini kullanarak kimlik doğrulaması ve çok faktörlü kimlik doğrulamasını atla.

Eski protokolleri tarafından yapılan kötü amaçlı kimlik doğrulama istekleri hesabınızı korumak için en iyi yolu bunları engellemektir.

**Bloğu eski kimlik doğrulama (Önizleme)** temel ilke eski protokolleri kullanılarak yapılan kimlik doğrulama isteklerini engeller. Modern kimlik doğrulaması, tüm kullanıcılar için başarıyla oturum açmak için kullanılmalıdır. Diğer temel ilkeleri ile birlikte kullanıldığında, eski kurallarından gelen istekleri engellenir. Ayrıca, tüm kullanıcılar için gerekli olduğunda MFA gerekli olacaktır. Bu ilke, Exchange ActiveSync engellemez.

### <a name="require-mfa-for-service-management-preview"></a>MFA istemek için Hizmet Yönetimi (Önizleme)

Kuruluşlar, çeşitli Azure hizmetlerini kullanın ve bunları Azure Resource Manager tabanlı araçlar gibi yönetebilirsiniz:

* Azure portal
* Azure PowerShell
* Azure CLI

Kaynak yönetimi gerçekleştirmek için bu araçlardan herhangi birini kullanarak üst düzeyde ayrıcalıklı bir işlemdir. Bu araçlar, hizmet ayarları ve abonelik faturalama gibi abonelik genelindeki yapılandırmaların değiştirebilirsiniz.

Ayrıcalıklı Eylemler korumak için bu **Hizmet Yönetimi (Önizleme) için MFA gerektiren** ilke Azure portalı, Azure PowerShell veya Azure CLI erişen herhangi bir kullanıcı için çok faktörlü kimlik doğrulaması gerektirir.

## <a name="enable-a-baseline-policy"></a>Temel ilke etkinleştir

Bir taban çizgisi ilkesini etkinleştirmek için:

1. Oturum **Azure portalında** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak.
1. Gözat **Azure Active Directory** > **koşullu erişim**.
1. İlkeler listesinde, etkinleştirmek istediğiniz bir temel ilke seçin.
1. Ayarlama **ilkesini etkinleştir** için **üzerinde**.
1. Kaydet'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

* [Kimlik altyapınızın güvenliğini sağlamak için beş adım](../../security/azure-ad-secure-steps.md)
* [Azure Active Directory'de koşullu erişim nedir?](overview.md)
* [Yöneticiler için MFA gerektirme](howto-baseline-protect-administrators.md)
* [Son kullanıcı protection (Önizleme)](howto-baseline-protect-end-users.md)
* [Blok eski kimlik doğrulama (Önizleme)](howto-baseline-protect-legacy-auth.md)
* [MFA istemek için Hizmet Yönetimi (Önizleme)](howto-baseline-protect-azure.md)