---
title: "Gruplar için dinamik üyelik sorunlarını giderme | Microsoft Docs"
description: "Azure AD grupları için dinamik üyelik için sorun giderme ipuçları."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 89bb04b6-a379-49c2-8465-fe386641816a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro
ms.openlocfilehash: 0bb4c294cc6a4e1c9c2f1ad405c539854b6bcf5b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
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
