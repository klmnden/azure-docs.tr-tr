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
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 42fbb8b08a2dc24ced436c4a6104f03ae3bca1e9
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982819"
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>Azure Active Directory’de B2B işbirliğini ve B2C’yi karşılaştırma

Hem Azure Active Directory (Azure AD) B2B işbirliği hem de Azure AD B2C, Azure AD’de dış kullanıcılarla çalışmanıza olanak sağlar. O halde bunların benzerlikleri ve farkları nelerdir?

**Azure AD B2B**, işbirliği yapabilmeleri için harici kullanıcılarla dosya ve kaynakları güvenli şekilde paylaşmak isteyen işletmeler içindir. Azure yöneticisi, Azure portalda B2B’yi ayarlar ve Azure AD, işletmeniz ile şirket dışındaki iş ortağınız arasındaki federasyon işleminden yararlanır. Kullanıcılar, iş veya okul hesabı ya da herhangi bir e-posta hesabı ile basit bir davet ve kullanım işlemini kullanarak paylaşılan kaynaklarda oturum açar.
 
**Azure AD B2C**, öncelikle müşteriye yönelik uygulamalar oluşturan işletmeler ve geliştiriciler içindir. Azure AD B2C ile geliştiriciler, bir yandan müşterilerin önceden belirlediği bir kimlikle (Facebook veya Gmail gibi) oturum açmasını sağlarken diğer yandan uygulamaları için tam özellikli kimlik sistemi olarak Azure AD’yi de kullanabilir.

Aşağıdaki tabloda ayrıntılı bir karşılaştırma sağlanmıştır.


B2B işbirliği özellikleri |     Azure AD B2C bağımsız teklifi
-------- | --------
Hedeflenen: Kimlik sağlayıcısına bakılmaksızın bir üst kuruluştaki kullanıcıların kimliğini doğrulayabilmek isteyen kuruluşlar. | Hedeflenen: İster birey, isterse kurumsal müşteriler olsun, mobil ve web uygulamalarınızın müşterilerini Azure AD’ye davet etme.
Desteklenen kimlikler: İş veya okul hesapları olan çalışanlar, iş ya da okul hesapları veya herhangi bir e-posta adresi olan iş ortakları. Yakında doğrudan federasyon desteklenecektir.  | Desteklenen kimlikler: Yerel uygulama hesapları (herhangi bir e-posta adresi veya kullanıcı adı) ya da doğrudan federasyon özellikli herhangi bir desteklenen sosyal kimliği olan tüketici kullanıcılar.
Ortak kullanıcılarının bulunduğu dizin: Harici kuruluştaki ortak kullanıcıları, çalışanlarla aynı dizinde yönetilir, ancak bunlara özel olarak ek açıklama eklenir. Çalışanlarla aynı şekilde yönetilebilir, aynı gruplara eklenebilir vb.  | Müşteri kullanıcı varlıklarının bulunduğu dizin: Uygulama dizini. Kuruluşun çalışan ve iş ortağı dizininden (varsa) ayrı şekilde yönetilir.
Tüm Azure AD ile bağlantılı uygulamalarda çoklu oturum açma (SSO) desteklenir. Örneğin, Office 365 veya şirket içi uygulamalara ve Salesforce ya da Workday gibi diğer SaaS uygulamalarına erişim sağlayabilirsiniz.  |  Azure AD B2C kiracılarında müşterilere ait uygulamalarda SSO desteklenir. Office 365 veya diğer Microsoft ve Microsoft olmayan SaaS uygulamalarında SSO desteklenmez.
İş ortağı yaşam döngüsü: Ana bilgisayar/davet eden kuruluş tarafından yönetilir.  | Müşteri yaşam döngüsü: Self servistir veya uygulama tarafından yönetilir.
Güvenlik ilkesi ve uyumluluk: Ana bilgisayar/davet eden kuruluş tarafından yönetilir (örneğin, [koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access) ile).  | Güvenlik ilkesi ve uyumluluk: Uygulama tarafından yönetilir.
Marka: Ana bilgisayar/davet eden kuruluşun markası kullanılır.  |    Marka: Uygulama tarafından yönetilir. Genellikle kuruluşun arka planda soluk olduğu şekilde ürün markalı olma eğilimindedir.
Daha fazla bilgi: [Blog gönderisi](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [Belgeler](what-is-b2b.md)  | Daha fazla bilgi: [Ürün sayfası](https://azure.microsoft.com/services/active-directory-b2c/), [Belgeler](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği kullanıcı özellikleri](user-properties.md)

