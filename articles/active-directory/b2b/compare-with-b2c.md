---
title: B2B işbirliği ve Azure Active Directory B2C karşılaştırın | Microsoft Docs
description: Azure Active Directory B2B işbirliği ve Azure AD B2C arasındaki fark nedir?
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 03/15/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: b59dba541394c105370cfd3af0768a8477ddb4e6
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39348523"
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>B2B işbirliği ve Azure Active Directory B2C ile karşılaştırma

Hem Azure Active Directory (Azure AD) B2B işbirliği hem de Azure AD B2C, Azure AD'de dış kullanıcılarla çalışmanıza olanak sağlar. Ancak karşılaştırmasına?


B2B işbirliği özellikleri |     Azure AD B2C bağımsız teklif
-------- | --------
Yönelik: kimlik sağlayıcısından bağımsız olarak bir iş ortağı kuruluştaki kullanıcıların kimliğini doğrulamak isteyen kuruluşlar. | Yönelik: olup müşteriler, mobil ve web uygulamaları, davet eden kişiler, Kurumsal veya Kurumsal müşterilerini Azure AD.
Desteklenen kimlikleri: çalışan iş veya Okul hesapları, iş ortakları ile iş veya Okul hesapları ya da herhangi bir e-posta adresi. Yakında doğrudan federasyonu'nu destekleyecek şekilde.  | Desteklenen kimlikleri: yerel uygulama hesapları (tüm e-posta adresi veya kullanıcı adı) veya herhangi bir tüketici kullanıcıların doğrudan Federasyon ile sosyal kimlik desteklenir.
İş ortağı kullanıcılardır, dizin: iş ortağı kullanıcılar dış kuruluştan çalışanlar ile aynı dizinde yönetilen, ancak özel olarak ek açıklama. Bunlar çalışanlar aynı şekilde yönetilebilir, aynı gruplarına eklenebilir vb.  | Müşteri kullanıcı varlıkları, içinde dizin: uygulama dizinindeki. Kuruluşun çalışan ve iş ortağı dizininde (varsa ayrı olarak yönetilen
Çoklu oturum açma (SSO) tüm Azure AD bağlantılı uygulamalar için desteklenir. Örneğin, Office 365 veya şirket içi uygulamalar ve diğer Salesforce veya Workday gibi SaaS uygulamaları için erişim sağlayabilir.  |  Müşterinin sahip olduğu Azure AD B2C kiracıları içinde uygulamaları için SSO desteklenir. Office 365 veya diğer Microsoft ve Microsoft tarafından sunulmayan SaaS uygulamaları için SSO desteklenmiyor.
İş ortağı yaşam döngüsü: konak/davet ederek yönetilen kuruluş.  | Müşteri yaşam döngüsü: Self Servis veya uygulama tarafından yönetilir.
Güvenlik ilke ve uyumluluk: konak/davet ederek yönetilen kuruluş (örneğin, [koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)).  | Güvenlik ilke ve uyumluluk: uygulama tarafından yönetilir.
Marka: Konak ve davet kuruluşunuzun markasını kullanılır.  |    Marka: uygulama tarafından yönetilir. Genellikle arka plan içine kuruluş Soluklaşan markalı ürün olma eğilimindedir.
Daha fazla bilgi: [Blog Gönderisi](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [belgeleri](what-is-b2b.md)  | Daha fazla bilgi: [ürün sayfası](https://azure.microsoft.com/services/active-directory-b2c/), [belgeleri](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği kullanıcı özellikleri](user-properties.md)

