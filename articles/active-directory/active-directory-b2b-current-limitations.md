---
title: "Azure Active Directory B2B işbirliği sınırlamaları | Microsoft Docs"
description: "Azure Active Directory B2B işbirliği geçerli sınırlamalar"
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 2136015eeb3d8ccfc58bf7a94b423144b9aed52e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliği sınırlamaları
Azure Active Directory (Azure AD) B2B işbirliği şu anda bu makalede açıklanan sınırlamalara tabidir.

## <a name="possible-double-multi-factor-authentication"></a>Olası çift çok faktörlü kimlik doğrulaması
Azure AD B2B ile kaynak kuruluştaki (davet kuruluş) çok faktörlü kimlik doğrulamasını zorunlu kılabilir. Bu yaklaşım nedenlerle ayrıntıları [B2B işbirliği kullanıcılar için koşullu erişim](active-directory-b2b-mfa-instructions.md). Bir iş ortağı ayarlama ve zorlanan çok faktörlü kimlik doğrulaması zaten varsa, ev kuruluşlarında bir kez kimlik doğrulaması yapmak, kullanıcılar olabilir ve ardından yeniden size ait.

## <a name="instant-on"></a>Anında açık
B2B işbirliği akışları içinde dizine kullanıcılar eklemek ve davet kullanım, uygulama atama ve benzeri sırasında dinamik olarak güncelleştir. Güncelleştirmeleri ve yazma işlemleri genellikle bir dizin örneğinde gerçekleşir ve tüm örneklerde çoğaltılması gerekir. Tüm örnekleri güncelleştirilir sonra çoğaltma tamamlandı. Bazen nesne yazılmış ya da bir örneğinde güncelleştirilmiş ve bu nesne almak için başka bir örneğine çağrıdır, çoğaltma gecikmeleri oluşabilir. Bu durumda, yenileme veya yardımcı olması için yeniden deneyin. Bizim API kullanarak bir uygulama yazıyorsanız, bazı geri alma ile yeniden deneme ise bu sorunu gidermek için iyi, savunma bir yöntem.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B işbirliği kullanıcı özellikleri](active-directory-b2b-user-properties.md)
* [Bir role B2B işbirliği kullanıcı ekleme](active-directory-b2b-add-guest-to-role.md)
* [B2bB işbirliği davetleri temsilci seçme](active-directory-b2b-delegate-invitations.md)
* [Dinamik gruplar ve B2B işbirliği](active-directory-b2b-dynamic-groups.md)
* [B2B işbirliği kodu ve PowerShell örnekleri](active-directory-b2b-code-samples.md)
* [SaaS uygulamaları B2B işbirliği için yapılandırma](active-directory-b2b-configure-saas-apps.md)
* [B2B işbirliği kullanıcı belirteçleri](active-directory-b2b-user-token.md)
* [B2B işbirliği kullanıcı taleplerini eşleme](active-directory-b2b-claims-mapping.md)
* [Office 365 dış paylaşım](active-directory-b2b-o365-external-user.md)
