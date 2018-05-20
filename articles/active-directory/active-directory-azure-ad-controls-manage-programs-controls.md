---
title: Programları yönetir ve denetimler için Azure AD erişim değerlendirmeleri | Microsoft Docs
description: Toplamak ve Azure Active Directory erişimi incelemeler denetimleri olarak düzenlemek için kuruluşunuzdaki her idare, risk yönetimi ve uyumluluk Initiative ek programlar oluşturabilirsiniz.
services: active-directory
documentationcenter: ''
author: markwahl-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/14/2018
ms.author: billmath
ms.openlocfilehash: a3fb812623b490e27907f63c1f7c6610ae754fb8
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="manage-programs-and-their-controls"></a>Programlar ve bunların denetimleri yönetme 

Azure Active Directory (Azure AD) Grup üyeleri ve uygulama erişimi erişim değerlendirmelerini içerir. Bu örnekler denetimlerinin, kuruluşunuzun Grup üyelikleri ve uygulamalara kimlerin erişebileceğini gözetim emin olun. Kuruluşlar, verimli bir şekilde, idare, risk yönetimi ve uyumluluk gereksinimlerine yönelik olarak bu denetimleri kullanabilirsiniz.

## <a name="create-and-manage-programs-and-their-controls"></a>Programları ve bunların denetimleri oluşturma ve yönetme
İzleme ve programlara düzenleyerek farklı amaçlar için erişim incelemeler toplamak nasıl basitleştirebilirsiniz. Her erişim gözden geçirme bir programa bağlanabilir. Rapor için bir denetçi hazırladığınızda belirli girişimi kapsamında erişim incelemeler üzerinde odaklanabilirsiniz.  Programlar ve erişim gözden geçirme sonuçları genel yönetici, güvenlik veya güvenlik okuyucu rolündeki kullanıcılara görünür.

Programların listesini görmek için Git [erişim incelemeleri sayfa](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) seçip **programlar**.

**Varsayılan Program** her zaman vardır. Bir genel Yönetici rolüne değilseniz, ek programlar oluşturabilirsiniz. Örneğin, bir program her uyumluluk girişimi sahip olmayı seçebilirsiniz veya İş hedefi.

Bir program artık ihtiyaç duymadığınız ve bağlı herhangi bir denetim yok, silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir grubun üyeleri veya bir uygulamaya erişim için erişim gözden geçirmesi oluşturma](active-directory-azure-ad-controls-create-access-review.md)
- [Bir erişim gözden geçirme sonuçları Al](active-directory-azure-ad-controls-retrieve-access-review.md)
