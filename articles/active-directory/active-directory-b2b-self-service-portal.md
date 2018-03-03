---
title: "Azure Active Directory B2B işbirliği için Self Servis kaydolma portalı | Microsoft Docs"
description: "Azure Active Directory B2B işbirliği, iş ortaklarının kurumsal uygulamalarınıza seçmeli olarak erişmelerini mümkün kılarak şirketler arası ilişkilerinizi destekler."
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
ms.date: 05/24/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: bb63a3b23bdcaac5c94d43bb8e7294a82b0c3fa0
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Azure AD B2B işbirliği kayıt için Self Servis portalı

Müşteriler yapabilir çok BT yöneticisi sunulan yerleşik özellikleri ile [Azure portal](https://portal.azure.com) ve [uygulama erişim Paneli'ne](https://myapps.microsoft.com) son kullanıcılar için. Ancak biz de işletmeler, kuruluşun gereksinimlerine uyacak şekilde B2B kullanıcılar için hazırlanma iş akışını özelleştirme gerektiğini fark. İle yapabileceklerini [API davet](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

Müşteri ile tartışmalara yukarıdaki tüm diğer hesapları miktarı artar ve ortak bir gereksinimi yoktur. Davet kuruluş vaktinden kaynaklarına erişmesi gereken tek tek dış ortak olan bilemeyebilirsiniz. Bunlar, kendilerini bir davet kuruluş denetleyen ilkeleri kümesiyle kaydolmak için iş ortağı şirketlerden kullanıcılar için bir yol istedik. Biz bunu vermedi Github projede yayımlanan şekilde bu senaryo API'leri aracılığıyla mümkündür: [örnek Github proje](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).

Bu Github proje nasıl kuruluşlar API'ler ve kullanabileceğiniz erişebilecekleri uygulamalar belirleyen kuralları ile güvenilir ortakları için bir ilke tabanlı, Self Servis kaydolma olanağı sağlamak gösterir. Bunlar, güvenli bir şekilde, bunları el ile yerleşik davet kuruluşa gerektirmeden gerek duyduğunuzda ortağı kullanıcıların kaynaklara erişimi alabilirsiniz. Tercih ettiğiniz bir Azure aboneliğinize proje kolayca dağıtabilirsiniz.

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