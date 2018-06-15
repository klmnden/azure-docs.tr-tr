---
title: Azure Active Directory B2B işbirliği için Self Servis kaydolma portalı | Microsoft Docs
description: Azure Active Directory B2B işbirliği, iş ortaklarının kurumsal uygulamalarınıza seçmeli olarak erişmelerini mümkün kılarak şirketler arası ilişkilerinizi destekler.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/08/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: b16f7c040212db6d0dbeb7161a18f205cf524090
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33930444"
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Azure AD B2B işbirliği kayıt için Self Servis portalı

Müşteriler yapabilir çok aracılığıyla sunulan yerleşik özellikleri ile [Azure portal](https://portal.azure.com) ve [uygulama erişim Paneli'ne](https://myapps.microsoft.com) son kullanıcılar için. Ancak, kuruluşunuzun gereksinimlerine uyacak şekilde B2B kullanıcılar için hazırlanma iş akışını özelleştirme gerekebilir. İle yapabileceğiniz [API davet](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

Davet kuruluş olarak, önceden kaynaklarınıza erişmek için gereken tek tek dış ortak olan bilemeyebilirsiniz. Kendilerini bir davet kuruluş olarak denetleyen ilkeleri kümesiyle kaydolmak için iş ortağı şirketlerden kullanıcılar için bir yönteme ihtiyacınız vardır. Bu senaryo API'leri aracılığıyla mümkündür. Var olan bir [örnek proje github'da](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web) tam olarak bunu yapar.

Bu GitHub proje kuruluşlar erişebilecekleri uygulamalar belirleyen kuralları ile güvenilir bir iş ortakları için bir ilke tabanlı, Self Servis kaydolma olanağı sağlamak için API'ler nasıl kullanabileceğinizi gösterir. Bunları gerektiğinde iş ortağı kullanıcılara kaynaklarına erişimi elde edebilirsiniz. Bunlar güvenli bir şekilde, bunları el ile yerleşik davet kuruluşa gerektirmeden bunu yapabilirsiniz. Tercih ettiğiniz bir Azure aboneliğinize proje kolayca dağıtabilirsiniz.

## <a name="as-is-code"></a>Olarak-kodu.

Bu kod bir örnek olarak Azure Active Directory B2B davet API kullanımını göstermek için kullanılabilir hale getirilir. Geliştirme ekibiniz veya bir iş ortağı tarafından özelleştirilmelidir ve bir üretim senaryosunda dağıtmadan önce gözden geçirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Azure AD B2B işbirliği lisanslama](active-directory-b2b-licensing.md)
* [Azure Active Directory B2B işbirliği sık sorulan sorular (SSS)](active-directory-b2b-faq.md)