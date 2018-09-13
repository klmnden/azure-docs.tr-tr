---
title: Self Servis kaydolma portal için Azure Active Directory B2B işbirliği | Microsoft Docs
description: Azure Active Directory B2B işbirliği, iş ortaklarının kurumsal uygulamalarınıza seçmeli olarak erişmelerini mümkün kılarak şirketler arası ilişkilerinizi destekler.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/08/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 94001b005a883c172cab279029b47ac1ad0c0de5
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647298"
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Azure AD B2B işbirliği kaydolma için Self Servis portalı

Müşteriler yapabilir çok aracılığıyla sunulan yerleşik özelliklere sahip [Azure portalında](https://portal.azure.com) ve [uygulama erişim panelinde](https://myapps.microsoft.com) son kullanıcılar için. Ancak, kuruluşunuzun gereksinimlerine uyacak şekilde B2B kullanıcıları ekleme iş akışını özelleştirmeniz gerekebilir. İle yapabileceğiniz [API davet](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

Davet eden bir kuruluş olarak kaynaklarınıza erişmesi gereken olan tek tek dış ortak çalışanlar saatin bilemeyebilirsiniz. Kendilerini bir dizi olarak davet eden kuruluştan denetleyen ilkeleri ile kaydolmak için iş ortağı şirketlerden kullanıcılar için bir yönteme ihtiyacınız vardır. Bu senaryo, API'ler aracılığıyla mümkündür. Var olan bir [GitHub üzerinde örnek proje](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web) bunu yapar.

Bu GitHub proje, kuruluşların erişebilmek uygulamaları belirleyen kuralları ile güvenilir bir iş ortakları için bir ilke tabanlı, Self Servis kaydolma olanağı sağlamak için API'leri nasıl kullanabileceğinizi gösterir. Gereksinim duydukları zaman iş ortağı kullanıcılar kaynaklara erişim elde edebilirsiniz. Bunlar güvenli bir şekilde, davet eden kuruluştan el ile eklemek için gerek kalmadan bunu yapabilirsiniz. Proje, tercih ettiğiniz bir Azure aboneliğinize kolayca dağıtabilirsiniz.

## <a name="as-is-code"></a>Olarak-kodu.

Bu kod, bir örnek olarak Azure Active Directory B2B davet API kullanımını göstermek için kullanılabilir hale getirilir. Geliştirme ekibiniz veya bir iş ortağı tarafından özelleştirilmelidir ve bir üretim senaryosunda dağıtmadan önce gözden geçirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Azure AD B2B işbirliği lisanslama](licensing-guidance.md)
* [Azure Active Directory B2B işbirliği sık sorulan sorular (SSS)](faq.md)