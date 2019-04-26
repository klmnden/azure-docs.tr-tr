---
title: B2B işbirliği - Azure Active Directory için Self Servis kaydolma portal | Microsoft Docs
description: Azure Active Directory B2B işbirliği, iş ortaklarının kurumsal uygulamalarınıza seçmeli olarak erişmelerini mümkün kılarak şirketler arası ilişkilerinizi destekler.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: sample
ms.date: 05/08/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c3ad424e8cab444b2405715eaa468411166ebd9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60412378"
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a>Azure AD B2B işbirliği kaydı için self servis portalı

Müşteriler, son kullanıcılar için [Azure portalı](https://portal.azure.com) ve [Uygulama Erişim Paneli](https://myapps.microsoft.com) aracılığıyla ortaya çıkan yerleşik özelliklerle birçok işlem yapabilir. Ancak B2B kullanıcıları için ekleme iş akışını, kuruluşunuzun gereksinimlerine uyacak şekilde özelleştirmeniz gerekebilir. [Davet API’si](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) ile bunu yapabilirsiniz.

Davet eden bir kuruluş olarak, kaynaklarınıza erişmesi gereken bireysel harici ortak çalışanların kim olduğunu önceden bilmeyebilirsiniz. Ortak şirketlerdeki kullanıcıların, davet eden kuruluş olarak sizin denetlediğiniz bir dizi ilke ile kendilerini kaydetmesi için bir yol gerekir. Bu senaryo, API’ler aracılığıyla mümkündür. [GitHub’da bunu yapan bir örnek proje](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web) vardır.

Bu GitHub projesi, kuruluşların, uygulamaların erişebildiği uygulamaları belirleyen kurallarla güvenilir ortaklarınız için ilke tabanlı, self servis kayıt özelliği sağlamak amacıyla API’leri nasıl kullanabildiğini gösterir. Ortak kullanıcılar, ihtiyaç duydukları anda kaynaklara erişim elde edebilir. Davet eden kuruluşun el ile eklemesi gerekmeden güvenli şekilde bunu yapabilir. Projeyi istediğiniz bir Azure aboneliğinize kolayca dağıtabilirsiniz.

## <a name="as-is-code"></a>As-is kodu

Bu kod, Azure Active Directory B2B davet API’sinin kullanımını göstermek için örnek olarak kullanılabilir duruma getirilir. Geliştirme ekibiniz veya bir ortak tarafından özelleştirilmelidir ve üretim senaryosunda dağıtmanızdan önce gözden geçirilmelidir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Azure AD B2B işbirliği lisanslaması](licensing-guidance.md)
* [Azure Active Directory B2B işbirliği hakkında sık sorulan sorular (SSS)](faq.md)
