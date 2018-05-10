---
title: Gruplar için dinamik üyelik sorunlarını giderme | Microsoft Docs
description: Azure AD grupları için dinamik üyelik için sorun giderme ipuçları.
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
ms.reviewer: piotrci
ms.custom: it-pro
ms.openlocfilehash: 6d8d04273e9f29b2634c8b77b0268f3c7b77b1e9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Gruplar için dinamik üyelik sorunlarını giderme
**I bir kural bir grup üzerinde yapılandırılmış ancak hiçbir üyelik grubunda güncelleştirilmesi**<br/>Kullanıcı öznitelikleri kuralda değerlerini doğrulayın: kural karşılayan kullanıcılar vardır? Her şeyi iyi görünüyorsa, Lütfen grup doldurmak biraz zaman tanıyın. Kiracınızın boyutuna bağlı olarak, ilk seferinde veya kural değişikliğinden sonra grubun doldurulması 24 saat kadar sürebilir.

**Bir kural yapılandırılmış, ancak şimdi kuralının var olan üyeleri kaldırılır**<br/>Bu beklenen bir davranıştır. Bir kural etkin veya değiştirildiğinde varolan grubun üyeleri kaldırılır. Kural değerlendirme sürümünden döndürülen kullanıcılar grubuna üye olarak eklenir.     

**Ne zaman eklemek veya bir kural neden değiştirmek instantly üyeliği değiştiğinde görmüyorum?**<br/>Ayrılmış üyeliği değerlendirmesi düzenli aralıklarla bir zaman uyumsuz arka planda gerçekleştirilir. İşlem için gereken süreyi dizininizdeki kullanıcı sayısı ve kural sonucu olarak oluşturulan grubu boyutu tarafından belirlenir. Genellikle, kullanıcıların küçük sayıda dizinlerle grup üyeliği değişikliklerinin değerinden birkaç dakika içinde görürsünüz. Çok sayıda kullanıcı dizinlerle 30 dakika alabilir veya doldurmak için daha uzun.

### <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Azure Active Directory nedir?](active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
