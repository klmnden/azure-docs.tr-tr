---
title: Azure Active Directory B2B işbirliği sınırlamaları | Microsoft Docs
description: Azure Active Directory B2B işbirliği geçerli sınırlamalar
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/23/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 562076e9529ffeac4cb0f99c1ffd4d4866d0bd1a
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34260123"
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliği sınırlamaları
Azure Active Directory (Azure AD) B2B işbirliği şu anda bu makalede açıklanan sınırlamalara tabidir.

## <a name="possible-double-multi-factor-authentication"></a>Olası çift çok faktörlü kimlik doğrulaması
Azure AD B2B ile kaynak kuruluştaki (davet kuruluş) çok faktörlü kimlik doğrulamasını zorunlu kılabilir. Bu yaklaşım nedenlerle ayrıntıları [B2B işbirliği kullanıcılar için koşullu erişim](conditional-access.md). Bir iş ortağı ayarlama ve zorlanan çok faktörlü kimlik doğrulaması zaten varsa, ev kuruluşlarında bir kez kimlik doğrulaması yapmak, kullanıcılar olabilir ve ardından yeniden size ait.

## <a name="instant-on"></a>Anında açık
B2B işbirliği akışları içinde dizine kullanıcılar eklemek ve davet kullanım, uygulama atama ve benzeri sırasında dinamik olarak güncelleştir. Güncelleştirmeleri ve yazma işlemleri genellikle bir dizin örneğinde gerçekleşir ve tüm örneklerde çoğaltılması gerekir. Tüm örnekleri güncelleştirilir sonra çoğaltma tamamlandı. Bazen nesne yazılmış ya da bir örneğinde güncelleştirilmiş ve bu nesne almak için başka bir örneğine çağrıdır, çoğaltma gecikmeleri oluşabilir. Bu durumda, yenileme veya yardımcı olması için yeniden deneyin. Bizim API kullanarak bir uygulama yazıyorsanız, bazı geri alma ile yeniden deneme ise bu sorunu gidermek için iyi, savunma bir yöntem.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2bB işbirliği davetleri temsilci seçme](delegate-invitations.md)

