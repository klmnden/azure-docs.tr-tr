---
title: Gruplar için dinamik üyelik sorunlarını giderme | Microsoft Docs
description: Azure AD'de grupları için dinamik üyelik için sorun giderme ipuçları.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: aaded644635ab93cef9323ad38d1d150b15fe743
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450302"
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Gruplar için dinamik üyelik sorunlarını giderme
**Bir grubu üzerinde bir kural yapılandırdım ancak hiçbir üyeliklerini grubunda güncelleştirilir**<br/>Kural üzerinde kullanıcı özniteliklerine değerleri doğrulayın: kural karşılayan kullanıcılar vardır? Her şey iyi görünüyor, lütfen grubun doldurulması için bir süre bekleyin. Kiracınızın boyutuna bağlı olarak, ilk seferinde veya kural değişikliğinden sonra grubun doldurulması 24 saat kadar sürebilir.

**Bir kural yapılandırdım ancak kuralının var olan üyeleri artık kaldırılır**<br/>Bu beklenen bir davranıştır. Kural etkin veya değiştirildiğinde grubun mevcut üyeleri kaldırılır. Kuralın değerlendirmesinden gelen döndürülen kullanıcıların gruba üye olarak eklenir.     

**Ne zaman ekleyin veya neden bir kural, değiştirme instantly üyelikleri de değişir göremiyorum?**<br/>Özel üyelik değerlendirmesi zaman uyumsuz bir planda düzenli gerçekleştirilir. İşlemin ne kadar sürer, dizininizdeki kullanıcı sayısı ve kural sonucu olarak oluşturulan grubu boyutu tarafından belirlenir. Genellikle, kullanıcılar az sayıda dizinlerle grup üyeliği değişikliklerinin küçüktür birkaç dakika içinde görürsünüz. Çok sayıda kullanıcı dizinlerle 30 dakika alabilir veya doldurmak için daha uzun.

### <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](../active-directory-apps-index.md)
* [Azure Active Directory nedir?](../fundamentals/active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../connect/active-directory-aadconnect.md)
