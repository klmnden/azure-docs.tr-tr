---
title: "Azure Active Directory B2B işbirliği için Self Servis kaydolma portalı | Microsoft Docs"
description: "Azure Active Directory B2B işbirliği, iş ortaklarının kurumsal uygulamalarınıza seçmeli olarak erişmelerini mümkün kılarak şirketler arası ilişkilerinizi destekler."
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: 307373c75bbb87cec683f7a3097f8f159c9d5e61
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Azure AD B2B işbirliği kayıt için Self Servis portalı

Müşteriler yapabilir çok bizim BT yöneticisi sunulan yerleşik özellikleri ile [Azure portal](https://portal.azure.com) ve bizim [uygulama erişim Paneli'ne](https://myapps.microsoft.com) son kullanıcılar için. Ancak biz de işletmeler, kuruluşun gereksinimlerine uyacak şekilde B2B kullanıcılar için hazırlanma iş akışını özelleştirme gerektiğini fark. İle yapabileceklerini [API'mize](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

Müşterilerimizin ile tartışmalara bir ortak yukarıdaki tüm diğer hesapları yükselmeye bakın. Davet kuruluş vaktinden kaynaklarına erişmesi gereken tek tek dış ortak olan bilemeyebilirsiniz. Bunlar, kendilerini bir davet kuruluş denetleyen ilkeleri kümesiyle kaydolmak için iş ortağı şirketlerden kullanıcılar için bir yol istedik. Biz bunu vermedi Github projede yayımlanan şekilde bu senaryo bizim API'leri aracılığıyla mümkündür: [örnek Github proje](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

Github Projemizin nasıl kuruluşlar bizim API'leri ve kullanabileceğiniz erişebilecekleri uygulamalar belirleyen kuralları ile güvenilir ortakları için bir ilke tabanlı, Self Servis kaydolma yeteneği sağlamak gösterir. Bunlar, güvenli bir şekilde, bunları el ile yerleşik davet kuruluşa gerektirmeden gerek duyduğunuzda ortağı kullanıcıların kaynaklara erişimi alabilirsiniz. Tercih ettiğiniz bir Azure aboneliğinize proje kolayca dağıtabilirsiniz.

## <a name="as-is-code"></a>Olarak-kodu.

Bu kod bir örnek olarak Azure Active Directory B2B davet API kullanımını göstermek için kullanılabilir hale getirileceğini unutmayın. Geliştirme ekibiniz veya bir iş ortağı tarafından özelleştirilmelidir ve bir üretim senaryosunda dağıtılmadan önce gözden geçirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:
* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Azure Active Directory yöneticileri B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-admin-add-users.md)
* [Bilgi çalışanları B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-iw-add-users.md)
* [B2B işbirliği davet e-posta öğeleri](active-directory-b2b-invitation-email.md)
* [B2B işbirliği davet kullanım](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B işbirliği lisanslama](active-directory-b2b-licensing.md)
* [Azure Active Directory B2B işbirliği sorunlarını giderme](active-directory-b2b-troubleshooting.md)
* [Azure Active Directory B2B işbirliği sık sorulan sorular (SSS)](active-directory-b2b-faq.md)
* [B2B işbirliği kullanıcıları için çok faktörlü kimlik doğrulaması](active-directory-b2b-mfa-instructions.md)
* [B2B işbirliği kullanıcıları davet olmadan ekleme](active-directory-b2b-add-user-without-invite.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)