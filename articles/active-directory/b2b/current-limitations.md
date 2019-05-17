---
title: B2B işbirliği - Azure Active Directory sınırlamaları | Microsoft Docs
description: Azure Active Directory B2B işbirliği için geçerli sınırlamalar
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d4aae8eb29b9e90bd1cb84949e97e21ed68c04c
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65812779"
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliği sınırlamaları
Azure Active Directory (Azure AD) B2B işbirliği şu anda, bu makalede açıklanan sınırlamalara tabidir.

## <a name="possible-double-multi-factor-authentication"></a>Olası çift çok faktörlü kimlik doğrulaması
Azure AD B2B ile kaynak kuruluşta (davet eden kuruluştaki) çok faktörlü kimlik doğrulamasını zorunlu kılabilir. Bu yaklaşım nedenlerle ayrıntıları [B2B işbirliği kullanıcıları için koşullu erişim](conditional-access.md). Bir iş ortağı ayarlanan ve zorlanan çok faktörlü kimlik doğrulaması zaten varsa, kullanıcılar kimlik doğrulama bir kez giriş kuruluşlarında gerçekleştirmeniz gerekebilir ve sonra yeniden kullanacağınıza kendiniz karar verebilirsiniz.

## <a name="instant-on"></a>Anında açık
B2B işbirliği akışlardaki dizine kullanıcı ekleme ve Davetiyesi kullanımı, uygulama ataması ve benzeri sırasında dinamik olarak güncelleştir. Güncelleştirmeler ve yazma işlemleri genellikle bir dizin örneğinde gerçekleşir ve tüm örnekleri arasında çoğaltılması gerekir. Tüm örnekleri güncelleştirildikten sonra çoğaltma tamamlanır. Çoğaltma gecikmeleri, bazen nesne yazılan veya bir örneğin güncelleştirildi ve bu nesneyi almak için arama için başka bir örneği olduğunda ortaya çıkabilir. Bu durumda, yenileme veya yardımcı olması için yeniden deneyin. API'mizi kullanarak bir uygulama yazıyorsanız, bazı geri alma ile yeniden ise bu sorunu gidermeyi iyi, savunma bir yöntem.

## <a name="azure-ad-directories"></a>Azure AD dizini
Azure AD B2B olan tabi Azure AD hizmet directory sınırları. Bir kullanıcı oluşturabilirsiniz dizinlerin sayısını ve dizinleri hakkında ayrıntılı bilgi için bir kullanıcı veya konuk kullanıcıya ait olabilir için bkz: [Azure AD hizmet sınırlamaları ve kısıtlamaları](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-service-limits-restrictions).

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği hakkında aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Temsilci B2B işbirliği davetleri](delegate-invitations.md)

