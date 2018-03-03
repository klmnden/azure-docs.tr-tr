---
title: "B2B işbirliği ve Azure Active Directory B2C karşılaştırma | Microsoft Docs"
description: "Azure Active Directory B2B işbirliği ve Azure AD B2C arasındaki fark nedir?"
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 03/15/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: ae3ebdceb65c04b98965f81f52997da457bd7845
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
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
Daha fazla bilgi: [Blog Gönderisi](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/01/azure-ad-b2b-new-updates-make-cross-business-collab-easy/), [belgeleri](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)  | Daha fazla bilgi: [ürün sayfası](https://azure.microsoft.com/en-us/services/active-directory-b2c/), [belgeleri](https://docs.microsoft.com/azure/active-directory-b2c/)


### <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B işbirliği kullanıcı özellikleri](active-directory-b2b-user-properties.md)
* [Bir role B2B işbirliği kullanıcı ekleme](active-directory-b2b-add-guest-to-role.md)
* [B2B işbirliği davetleri temsilci seçme](active-directory-b2b-delegate-invitations.md)
* [Dinamik gruplar ve B2B işbirliği](active-directory-b2b-dynamic-groups.md)
* [SaaS uygulamaları B2B işbirliği için yapılandırma](active-directory-b2b-configure-saas-apps.md)
* [B2B işbirliği kullanıcı belirteçleri](active-directory-b2b-user-token.md)
* [B2B işbirliği kullanıcı taleplerini eşleme](active-directory-b2b-claims-mapping.md)
* [Office 365 dış paylaşım](active-directory-b2b-o365-external-user.md)
* [B2B işbirliği geçerli sınırlamalar](active-directory-b2b-current-limitations.md)
* [B2B işbirliği için destek alma](active-directory-b2b-support.md)
