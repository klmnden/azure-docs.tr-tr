---
title: Programları yönetir ve denetimler için Azure AD erişim gözden geçirmeleriyle | Microsoft Docs
description: Azure Active Directory erişim gözden geçirmesi denetimlerini toplayıp kuruluşunuzdaki diğer programlar için her yönetim, risk yönetimi ve uyumluluk girişim oluşturabilirsiniz.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: b62a150160daa1d6708dbf5edaa6772688e2ffa1
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45607818"
---
# <a name="manage-programs-and-their-controls"></a>Programları ve denetimlerini yönetme 

Azure Active Directory (Azure AD) erişim gözden geçirmeleri grup üyelerini ve uygulama erişimi içerir. Bu örnekler denetimlerin kimlerin erişebileceğini, kuruluşunuzun Grup üyeliklerini ve uygulamalar için gözetim emin olun. Kuruluşlar, verimli bir şekilde, yönetim, risk yönetimi ve uyumluluk gereksinimlerini ele almak için bu denetimleri kullanabilirsiniz.

## <a name="create-and-manage-programs-and-their-controls"></a>Oluşturma ve programları ve denetimlerini yönetme
Programlara düzenleyerek farklı amaçlara yönelik erişim gözden geçirmeleri toplamak ve izlemek nasıl basitleştirebilir. Her erişim gözden geçirmesi bir programa bağlanabilir. Raporlar için bir denetçi hazırlarken, erişim gözden geçirmeleri için belirli bir girişim kapsamda üzerinde odaklanabilirsiniz.  Programlar ve erişim gözden geçirmesi sonuçlarını genel yönetici, kullanıcı hesabı yöneticisi, güvenlik yöneticisi veya güvenlik okuyucusu rolündeki kullanıcılar için görünür durumdadır.

Programların listesini görmek için Git [erişim gözden geçirmeleri sayfasına](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) seçip **programlar**.

**Varsayılan Program** her zaman vardır. Bir genel yönetici veya kullanıcı hesabı yönetici rolü varsa, ek programlar oluşturabilirsiniz. Örneğin, bir program için her uyumluluk girişim sahip olmayı seçebilirsiniz veya İş hedefi.

Bir program artık ihtiyacınız ve ona bağlı herhangi bir denetim yok, onu silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir grubun üyeleri veya bir uygulamaya erişim için erişim gözden geçirmesi oluşturma](create-access-review.md)
- [Erişim gözden geçirmesi sonuçlarını Al](retrieve-access-review.md)
