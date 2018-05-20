---
title: B2B işbirliği ve Azure Active Directory B2C karşılaştırma | Microsoft Docs
description: Azure Active Directory B2B işbirliği ve Azure AD B2C arasındaki fark nedir?
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 03/15/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 19e8e9d22938fa8a18299d67aa77824aaae3f6da
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="compare-b2b-collaboration-and-b2c-in-azure-active-directory"></a>B2B işbirliği ve Azure Active Directory B2C ile karşılaştırın

Azure Active Directory (Azure AD) B2B işbirliği ve Azure AD B2C, Azure AD'de dış kullanıcılarla çalışmanıza olanak sağlar. Ancak nasıl karşılaştırmak?


B2B işbirliği özellikleri |     Azure AD B2C tek başına teklifi
-------- | --------
Yönelik: Kimlik sağlayıcısı bağımsız olarak bir iş ortağı kuruluştan kullanıcıların kimliklerini doğrulamak kullanabilmek ister kuruluş. | Yönelik: Mobil müşterileri ve web uygulamaları olup davet kişiler, Kurumsal veya Kurumsal müşteriler, Azure AD'ye.
Desteklenen kimlikleri: iş, okul hesapları, iş veya Okul hesapları iş ortaklarıyla veya herhangi bir e-posta adresi olan çalışanlar. En kısa sürede doğrudan Federasyon desteklemek için.  | Desteklenen kimlikleri: tüketici kullanıcılarla yerel uygulama hesapları (tüm e-posta adresi veya kullanıcı adı) veya herhangi bir desteklenen doğrudan Federasyon ile sosyal kimliği.
İş ortağı kullanıcılara olan dizin: iş ortağı kullanıcılar dış kuruluştan çalışanlar ile aynı dizinde yönetilen, ancak özel ek açıklama. Bunlar çalışanlar aynı şekilde yönetilebilir, aynı gruplarına eklenebilir vb.  | Müşteri kullanıcı varlıklardır içinde dizin: uygulama dizinindeki. Ayrı olarak yönetilen kuruluşun çalışan ve iş ortağı dizininden (eğer varsa.
Çoklu oturum açma (SSO) tüm Azure AD bağlı uygulamalar için desteklenir. Örneğin, Office 365 veya şirket içi uygulamalar ve Salesforce veya Workday gibi diğer SaaS uygulamaları için erişim sağlayabilir.  |  SSO uygulamaları Azure AD B2C kiracıları içinde ait müşteri için desteklenir. SSO Office 365 veya diğer Microsoft ve Microsoft olmayan SaaS uygulamaları için desteklenmiyor.
İş ortağı yaşam döngüsü: ana bilgisayar/davet tarafından yönetilen kuruluş.  | Müşteri döngüsü: Self Servis veya uygulama tarafından yönetilir.
Güvenlik ilke ve uyumluluk: ana bilgisayar/davet tarafından yönetilen kuruluş.  | Güvenlik ilke ve uyumluluk: uygulama tarafından yönetilir.
Marka: Ana bilgisayar ve davet kuruluşunuzun markası kullanılır.  |    Marka: uygulama tarafından yönetiliyor. Genellikle arka plan içine kuruluş Soluklaşan markalı ürün olma eğilimindedir.
Daha fazla bilgi: [Blog Gönderisi](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [belgeleri](what-is-b2b.md)  | Daha fazla bilgi: [ürün sayfası](https://azure.microsoft.com/services/active-directory-b2c/), [belgeleri](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği kullanıcı özellikleri](user-properties.md)

