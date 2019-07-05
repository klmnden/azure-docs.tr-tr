---
title: Azure Active Directory'de koşullu erişim nedir? | Microsoft Docs
description: Azure Active Directory'de koşullu erişim size nasıl yardımcı olduğunu öğrenin kimin bir kaynağa erişmeye çalışır ancak bir kaynak nasıl erişilir de yalnızca dayanmayan otomatik erişim kararları uygulamak için.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: overview
ms.date: 02/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: a5ef72f1db329d04809a1069c1916d1ffcfffe65
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509391"
---
# <a name="what-is-conditional-access"></a>Koşullu Erişim nedir?

Bulutu kullanan kuruluşlar için en önemli konu güvenliktir. Bulut kaynaklarınızı yönetmek söz konusu olduğunda, bulut güvenliğinin temel etmenlerinden biri güvenlik ve kimliktir. Mobil ve bulut öncelikli bir dünyada kullanıcılar, kuruluşunuzun kaynaklarına erişmek için farklı konumlardan farklı cihazlar ve uygulamalar kullanabilir. Bunun sonucunda artık yalnızca bir kaynağa erişim sağlayabilecek kullanıcılara odaklanmak yeterli değildir. Güvenlik ve üretkenlik arasındaki dengeyi kurmak için, erişim denetimi kararı sırasında bir kaynağa erişim şeklini de dikkate almanız gerekir. Azure Active Directory (Azure AD) koşullu erişim ile bu gereksinimleri karşılayabilirsiniz. Koşullu erişim, Azure Active Directory için kullanılan bir özelliktir. Koşullu erişimle koşullara göre bulut uygulamalarınıza erişen için otomatik erişim denetimi kararları uygulayabilirsiniz.

Koşullu erişim ilkeleri, ilk-faktörlü kimlik doğrulaması tamamlandıktan sonra uygulanır. Bu nedenle, koşullu erişim, hizmet reddi (DoS) saldırıları gibi senaryolar için ilk satırı savunma olarak tasarlanmamıştır, ancak bu olaylar (örneğin, oturum açma risk düzeyini, konum isteği ve benzeri) gelen sinyalleri erişimini belirlemek için kullanabilir.  

![Denetim](./media/overview/81.png)

Bu makalede, Azure AD'de ile koşullu erişim kavramsal genel bakış sunulmaktadır.

## <a name="common-scenarios"></a>Genel senaryolar

Mobil ve bulut öncelikli bir dünyada Azure Active Directory cihazlarda, uygulamalarda ve hizmetlerde tüm konumlardan çoklu oturum açma özelliğinin kullanılmasını sağlar. Cihaz kullanımı (KCG dahil), kuruluş ağı dışında çalışma ve üçüncü taraf SaaS uygulamaları arttıkça iki hedefle karşılaşırsınız:

- Kullanıcıların her yerde ve her zaman üretken olmasını sağlama
- Kuruluş varlıklarını her zaman koruma altında tutma

Koşullu erişim ilkelerini kullanarak, gerekli koşullar altında doğru erişim denetimlerini uygulayabilirsiniz. Azure AD koşullu erişim, değilken, kullanıcınızın şekilde dışında kalır ve gerekli ek güvenlik sağlar.

Koşullu erişim ile yardımcı olabilecek bazı genel erişim konuları aşağıda verilmiştir:

- **[Oturum açma riski](conditions.md#sign-in-risk)** : Azure AD kimlik koruması, oturum açma riskleri algılar. Oturum açma riskinin kötü niyetli bir kullanıcıyı işaret ettiği durumda erişimi nasıl sınırlarsınız? Oturumun açma işleminin doğru kullanıcı tarafından gerçekleştirilmiş olduğuna dair daha kuvvetli bir kanıta ihtiyaç duyarsanız ne olur? Şüpheleriniz, belirli kullanıcıların bir uygulamaya erişimini engelleyecek kadar kuvvetliyse ne yapmalısınız?  
- **[Ağ konumu](location-condition.md)** : Azure AD her yerden erişilebilir. BT departmanınızın denetimi altında olmayan bir ağ konumundan erişim girişimi gerçekleştirilirse ne olur? Kullanıcı adı ve parola bileşimi, kuruluş ağınızdan yapılan erişim denemelerinde kimliğinizi kanıtlamaya yeterli olabilir. Peki dünya üzerindeki diğer beklenmeyen ülkelerden veya bölgelerden başlatılan erişim girişimleri için daha kuvvetli bir kimlik kanıtına ihtiyaç duyuyorsanız? Belirli konumlardan erişim girişimlerini tamamen engellemek isterseniz ne yapmalısınız?  
- **[Cihaz Yönetimi](conditions.md#device-platforms)** : Azure AD'de, bulut uygulamaları dahil olmak üzere mobil cihazları hem de kişisel cihazları geniş aralığından kullanıcılar erişebilir. Peki ya erişim denemelerinin yalnızca BT departmanınız tarafından yönetilen cihazlardan gerçekleştirilmesini isterseniz? Peki ya belirli cihaz türlerinin ortamınızdaki bulut uygulamalarına erişmesini engellemek isterseniz?
- **[İstemci uygulaması](conditions.md#client-apps)** : Günümüzde, çok sayıda bulut uygulamaları, web tabanlı uygulamalar, mobil uygulamalar ve Masaüstü uygulamaları gibi farklı uygulama türlerini kullanarak erişebilirsiniz. Peki bilinen sorunlara yol açan türde bir istemci uygulaması kullanılarak erişim girişimi gerçekleştirilirse ne olur? Belirli uygulama türleri için BT departmanınız tarafından yönetilen bir cihazı şart koşmak istiyorsanız ne yapmalısınız?

Bu soruları ve yanıtları ilgili genel erişim senaryoları için Azure AD koşullu erişim temsil eder.
Koşullu erişim, ilke tabanlı bir yaklaşım kullanarak erişimi senaryoları işlemenizi sağlayan Azure Active Directory için kullanılan bir özelliktir.

> [!VIDEO https://www.youtube.com/embed/eLAYBwjCGoA]

## <a name="conditional-access-policies"></a>Koşullu erişim ilkeleri

Koşullu erişim ilkesi şu biçimi kullanarak bir access senaryosunun bir tanımı şöyledir:

![Denetim](./media/overview/10.png)


**Bu ortaya çıktığında**, ilkenizin tetiklenme nedenini tanımlar. Neden, karşılanan bir koşul grubu ile belirlenir. Azure AD koşullu erişimi'nde, iki atama koşullar özel bir rol oynar:

- **[Kullanıcılar](conditions.md#users-and-groups)** : Erişim denemesi gerçekleştiren kullanıcıları (**kimin**).
- **[Bulut uygulamaları](conditions.md#cloud-apps-and-actions)** : Erişim denemesi hedefleri (**ne**).

Bu iki koşul, bir koşullu erişim ilkesini zorunlu değildir. Bu iki zorunlu koşula ek olarak, erişim girişiminin gerçekleştirilme şeklini açıklayan ek koşullar da kullanabilirsiniz. Mobil cihazların veya kuruluş ağınızın dışındaki konumların kullanılması, yaygın örnekler arasında sayılabilir. Daha fazla bilgi için [Azure Active Directory koşullu erişim koşulları](conditions.md).

Erişim denetimleri koşullarla bir koşullu erişim ilkesini temsil eder.

![Denetim](./media/overview/51.png)

Azure AD koşullu erişim ile nasıl yetkili kullanıcıların Denetim bulut uygulamalarınızda erişebilirsiniz. Erişim denemesi nasıl gerçekleştirildiğini tabanlı bir bulut uygulaması için erişim denemesi ek erişim denetimleri uygulamak amacı, bir koşullu erişim ilkesi var.

Bulut uygulamalarınıza erişimi korumak için ilke tabanlı bir yaklaşım kullanmak, ortamınızın ilke gereksinimlerini işin teknik boyutu konusunda kaygılanmadan bu makalede ana hatları belirlenen yapıyı kullanarak belirlemeye başlamanızı sağlar.

## <a name="azure-ad-conditional-access-and-federated-authentication"></a>Azure AD koşullu erişim ve şirket dışı kimlik doğrulaması

Koşullu erişim ilkeleri sorunsuz bir şekilde iş [şirket dışı kimlik doğrulaması](../../security/azure-ad-choose-authn.md#federated-authentication). Desteklenen tüm koşullar ve denetimleri ve ilke etkin kullanıcı oturum açma kullanarak işlemleri için nasıl uygulanacağını içine görünürlük bu desteğiyle [Azure AD raporlama](../reports-monitoring/concept-sign-ins.md).

*Azure AD ile federe kimlik doğrulaması*, Azure AD kullanıcı kimliği doğrulamalarının tümünün güvenilen bir kimlik doğrulama hizmeti tarafından halledilmesi anlamına gelir. Güvenilen bir kimlik doğrulama hizmeti; örneğin Active Directory Federasyon Hizmetleri (AD FS) ya da başka bir Federasyon Hizmetidir. Bu yapılandırmada birincil kullanıcı kimlik doğrulaması hizmette gerçekleştirilir, sonra bireysel uygulamalarda oturum için Azure AD kullanılır. Azure AD koşullu erişim, kullanıcının eriştiği uygulamaya erişim verilmeden önce uygulanır. 

Yapılandırılmış koşullu erişim ilkesi, çok faktörlü kimlik doğrulaması gerektirdiğinde, Azure mfa'yı kullanarak Azure AD varsayılan olarak. MFA için federasyon hizmetini kullanıyorsanız, [PowerShell](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainfederationsettings)'de `-SupportsMFA` değerini `$true` yaparak Azure AD'yi MFA gerektiğinde federasyon hizmetine yönlenecek şekilde yapılandırabilirsiniz. Bu ayar, Azure AD tarafından `wauth= http://schemas.microsoft.com/claims/multipleauthn` kullanılarak gönderilen MFA sınama isteğini destekleyen federe kimlik doğrulama hizmetleri içindir.

Kullanıcı federe kimlik doğrulama hizmetinde oturum açtıktan sonra Azure AD, cihaz uyumluluğu ya da uygulamanın onaylanmış olması gibi diğer ilke gereksinimlerini halleder.

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Active Directory P1 license](../../../includes/active-directory-p1-license.md)]

Müşterilerle [Microsoft 365 işletme lisanslarını](https://docs.microsoft.com/office365/servicedescriptions/microsoft-365-business-service-description) ayrıca koşullu erişim özelliklerini erişime sahip olursunuz. 

## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim, ortamınızda uygulama hakkında bilgi edinmek için bkz: [Azure Active Directory'de koşullu erişim dağıtımınızı planlama](plan-conditional-access.md).
