---
title: Azure Active Directory B2B işbirliği sınırlamaları | Microsoft Docs
description: Azure Active Directory B2B işbirliği için geçerli sınırlamalar
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 4e997e33eaccc6938209937d3157498db8bf4781
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982349"
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

