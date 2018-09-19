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
ms.date: 09/11/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: e189fb8b2bc5079d1560d3b7a54fea2db7366fe7
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46293988"
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Gruplar için dinamik üyelik sorunlarını giderme

**Bir grubu üzerinde bir kural yapılandırdım ancak hiçbir üyeliklerini grubunda güncelleştirilir**<br/>Kural üzerinde kullanıcı özniteliklerine değerleri doğrulayın: kural karşılayan kullanıcılar vardır? Her şey iyi görünüyor, lütfen grubun doldurulması için bir süre bekleyin. Kiracınızın boyutuna bağlı olarak, ilk seferinde veya kural değişikliğinden sonra grubun doldurulması 24 saat kadar sürebilir.

**Bir kural yapılandırdım ancak kuralının var olan üyeleri artık kaldırılır**<br/>Bu beklenen bir davranıştır. Kural etkin veya değiştirildiğinde grubun mevcut üyeleri kaldırılır. Kuralın değerlendirmesinden gelen döndürülen kullanıcıların gruba üye olarak eklenir.

**Ne zaman ekleyin veya neden bir kural, değiştirme instantly üyelikleri de değişir göremiyorum?**<br/>Özel üyelik değerlendirmesi zaman uyumsuz bir planda düzenli gerçekleştirilir. İşlemin ne kadar sürer, dizininizdeki kullanıcı sayısı ve kural sonucu olarak oluşturulan grubu boyutu tarafından belirlenir. Genellikle, kullanıcılar az sayıda dizinlerle grup üyeliği değişikliklerinin küçüktür birkaç dakika içinde görürsünüz. Çok sayıda kullanıcı dizinlerle 30 dakika alabilir veya doldurmak için daha uzun.

**İşleme hatası bir kural karşılaştım**<br/>Aşağıdaki tabloda, sık karşılaşılan dinamik üyelik kuralı hataları ve bunların nasıl düzeltileceği listeler.

| Kural ayrıştırıcı hatası | Hata kullanımı | Düzeltilmiş kullanımı |
| --- | --- | --- |
| Hata: özniteliği desteklenmiyor. |(user.invalidProperty - eq "Value") |(user.department - eq "value")<br/><br/>Öznitelik olduğundan emin olun üzerinde [desteklenen özellikler listesinde](groups-dynamic-membership.md#supported-properties). |
| Hata: İşleç özniteliği desteklenmiyor. |(user.accountEnabled-true içerir) |(user.accountEnabled - eq true)<br/><br/>Özellik türü için kullanılan işleci desteklenmiyor (Bu örnekte,-içeren kullanılamaz boolean türü). Doğru operators, özellik türü için kullanın. |
| Hata: Sorgu derleme hatası. | 1. (user.department - eq "Sales") (user.department - eq "Pazarlama")<br>2. (user.userPrincipalName-eşleşen "*@domain.ext") | 1. İşleç eksik. Kullanın - veya - veya iki doğrulamaları katılın<br>(user.department - eq "Sales")- veya (user.department - eq "Pazarlama")<br>2. Hata - ile kullanılan normal ifade ile eşleşen<br>(user.userPrincipalName-eşleşen ". *@domain.ext")<br>veya alternatif olarak: (user.userPrincipalName-eşleşen "@domain.ext$") |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
* [Azure Active Directory'de uygulama yönetimi](../manage-apps/what-is-application-management.md)
* [Azure Active Directory nedir?](../fundamentals/active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)
