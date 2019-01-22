---
title: Azure Active Directory’de B2B işbirliğini ve B2C’yi karşılaştırma | Microsoft Docs
description: Azure Active Directory B2B işbirliği ile Azure AD B2C arasındaki fark nedir?
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: overview
ms.date: 03/15/2017
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.openlocfilehash: 69c8e293186f955e86962a325fce2f54a2eefdc7
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54432175"
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>Azure Active Directory’de B2B işbirliğini ve B2C’yi karşılaştırma

Hem Azure Active Directory (Azure AD) B2B işbirliği hem de Azure AD B2C, Azure AD’de dış kullanıcılarla çalışmanıza olanak sağlar. O halde bunların benzerlikleri ve farkları nelerdir?

**Azure AD B2B**, işbirliği yapabilmeleri için harici kullanıcılarla dosya ve kaynakları güvenli şekilde paylaşmak isteyen işletmeler içindir. Azure yöneticisi, Azure portalda B2B’yi ayarlar ve Azure AD, işletmeniz ile şirket dışındaki iş ortağınız arasındaki federasyon işleminden yararlanır. Kullanıcılar, iş veya okul hesabı ya da herhangi bir e-posta hesabı ile basit bir davet ve kullanım işlemini kullanarak paylaşılan kaynaklarda oturum açar.
 
**Azure AD B2C**, öncelikle müşteriye yönelik uygulamalar oluşturan işletmeler ve geliştiriciler içindir. Azure AD B2C ile geliştiriciler, bir yandan müşterilerin önceden belirlediği bir kimlikle (Facebook veya Gmail gibi) oturum açmasını sağlarken diğer yandan uygulamaları için tam özellikli kimlik sistemi olarak Azure AD’yi de kullanabilir.

Aşağıdaki tabloda ayrıntılı bir karşılaştırma sağlanmıştır.


B2B işbirliği özellikleri |     Azure AD B2C bağımsız teklifi
-------- | --------
Yönelik: Kimlik sağlayıcısı bağımsız olarak bir iş ortağı kuruluştaki kullanıcıların kimliğini doğrulamak isteyen kuruluşlar. | Yönelik: Müşteriler, mobil ve web apps olup davet kişiler, Kurumsal veya Kurumsal müşterilerini Azure AD.
Desteklenen kimlikleri: Çalışanların iş veya Okul hesapları, iş ortakları ile iş veya Okul hesapları ya da herhangi bir e-posta adresi. Yakında doğrudan federasyon desteklenecektir.  | Desteklenen kimlikleri: Yerel uygulama hesapları (tüm e-posta adresi veya kullanıcı adı) veya herhangi bir tüketici kullanıcıların sosyal kimlik doğrudan Federasyon ile desteklenir.
Dizin iş ortağı kullanıcılar dahildir. İş ortağı kullanıcılar dış kuruluştan çalışanlar ile aynı dizinde yönetilen, ancak özel olarak ek açıklama. Çalışanlarla aynı şekilde yönetilebilir, aynı gruplara eklenebilir vb.  | Dizin müşteri kullanıcı varlıkları dahildir. Uygulama dizininde. Kuruluşun çalışan ve iş ortağı dizininden (varsa) ayrı şekilde yönetilir.
Tüm Azure AD ile bağlantılı uygulamalarda çoklu oturum açma (SSO) desteklenir. Örneğin, Office 365 veya şirket içi uygulamalara ve Salesforce ya da Workday gibi diğer SaaS uygulamalarına erişim sağlayabilirsiniz.  |  Azure AD B2C kiracılarında müşterilere ait uygulamalarda SSO desteklenir. Office 365 veya diğer Microsoft ve Microsoft olmayan SaaS uygulamalarında SSO desteklenmez.
İş ortağı yaşam döngüsü: Konak/davet ederek yönetilen kuruluş.  | Müşteri yaşam döngüsü: Self Servis veya uygulama tarafından yönetilir.
Güvenlik ilke ve uyumluluk: Konak/davet ederek yönetilen kuruluş (örneğin, [koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)).  | Güvenlik ilke ve uyumluluk: Uygulama tarafından yönetilir.
Marka: Kuruluşunuzun markasını konak/davet kullanılır.  |    Marka: Uygulama tarafından yönetilir. Genellikle kuruluşun arka planda soluk olduğu şekilde ürün markalı olma eğilimindedir.
Daha fazla bilgi: [Blog gönderisi](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [belgeleri](what-is-b2b.md)  | Daha fazla bilgi: [Ürün sayfası](https://azure.microsoft.com/services/active-directory-b2c/), [belgeleri](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği kullanıcı özellikleri](user-properties.md)

