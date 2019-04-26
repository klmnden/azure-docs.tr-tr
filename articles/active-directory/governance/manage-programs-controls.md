---
title: Programları ve denetimleri için erişim gözden geçirmeleri - Azure Active Directory yönetme | Microsoft Docs
description: Azure Active Directory erişim gözden geçirmesi denetimlerini toplayıp kuruluşunuzdaki diğer programlar için her yönetim, risk yönetimi ve uyumluluk girişim oluşturmayı öğrenin.
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
ms.subservice: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 43c0f1c041bfed1b041a9926efd869d167c6f1e9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60246028"
---
# <a name="manage-programs-and-controls-for-azure-ad-access-reviews"></a>Programları yönetir ve denetimler için Azure AD erişim gözden geçirmeleri

Azure Active Directory (Azure AD) erişim gözden geçirmeleri grup üyelerini ve uygulama erişimi içerir. Bu örnekler denetimlerin kimlerin erişebileceğini, kuruluşunuzun Grup üyeliklerini ve uygulamalar için gözetim emin olun. Kuruluşlar, verimli bir şekilde, yönetim, risk yönetimi ve uyumluluk gereksinimlerini ele almak için bu denetimleri kullanabilirsiniz.

## <a name="create-and-manage-programs-and-their-controls"></a>Oluşturma ve programları ve denetimlerini yönetme
Programlara düzenleyerek farklı amaçlara yönelik erişim gözden geçirmeleri toplamak ve izlemek nasıl basitleştirebilir. Her erişim gözden geçirmesi bir programa bağlanabilir. Raporlar için bir denetçi hazırlarken, erişim gözden geçirmeleri için belirli bir girişim kapsamda üzerinde odaklanabilirsiniz.  Programlar ve erişim gözden geçirmesi sonuçlarını genel yönetici, yönetici kullanıcı, güvenlik yöneticisi veya güvenlik okuyucusu rolü kullanıcılara görünür.

Programların listesini görmek için Git [erişim gözden geçirmeleri sayfasına](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) seçip **programlar**.

**Varsayılan Program** her zaman vardır. Bir genel yönetici veya Kullanıcı Yöneticisi rolü, ek programlar oluşturabilirsiniz. Örneğin, bir program için her uyumluluk girişim sahip olmayı seçebilirsiniz veya İş hedefi.

Bir program artık ihtiyacınız ve ona bağlı herhangi bir denetim yok, onu silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Grupları ve uygulamaları, erişim gözden geçirmesi oluştur](create-access-review.md)
- [Gruplar veya uygulamalar için erişim gözden geçirmesi sonuçlarını Al](retrieve-access-review.md)
