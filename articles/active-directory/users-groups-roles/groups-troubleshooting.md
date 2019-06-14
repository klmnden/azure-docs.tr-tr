---
title: Grupları - Azure Active Directory için dinamik üyelik sorunlarını giderme | Microsoft Docs
description: Azure AD'de grupları için dinamik üyelik için sorun giderme ipuçları.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0eededcc180d7652fd52c79b85ca3c34f65a22a4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60469723"
---
# <a name="troubleshoot-and-resolve-groups-issues"></a>Sorun giderme ve çözümleme ilgili sorunları gruplandırır

## <a name="troubleshooting-group-creation-issues"></a>Grup oluşturma sorunlarını giderme

**Ben Azure portalında güvenlik grubu oluşturma devre dışı ancak grupları yine de Powershell oluşturulabilir** **kullanıcı, Azure portallarında güvenlik grupları oluşturabilir** Azure portal denetimlerinde ayarlama olup olmadığını yönetici olmayan Kullanıcılar, erişim paneli veya Azure portalında güvenlik grupları oluşturabilir. Güvenlik grubu oluşturma Powershell aracılığıyla denetlemez.

Grup oluşturma Powershell yönetici olmayan kullanıcılar için devre dışı bırakmak için:
1. Yönetici olmayan kullanıcılar grupları oluşturmak için izin verildiğini doğrulayın:
   

   ```powershell
   Get-MsolCompanyInformation | Format-List UsersPermissionToCreateGroupsEnabled
   ```

  
2. Döndürürse `UsersPermissionToCreateGroupsEnabled : True`, sonra da yönetici olmayan kullanıcılar, gruplar oluşturabilirsiniz. Bu özellik devre dışı bırakmak için:
  

   ``` 
   Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False
   ```

<br/>**PowerShell'de bir dinamik grup oluşturmaya çalışırken hata izin verilen en fazla grupları aldım**<br/>
İçinde Powershell belirten bir ileti alırsanız _grupların sayısı üst sınırına en büyük izin verilen dinamik grup ilkeleri_, bu, döşemedeki sınırını dinamik gruplar için kiracınızda anlamına gelir. En fazla dinamik gruplar Kiracı başına 5.000 sayısıdır.

Tüm yeni dinamik gruplar oluşturmak için öncelikle var olan bazı dinamik grupları silme gerekecektir. Sınırı artırmak için hiçbir yolu yoktur.

## <a name="troubleshooting-dynamic-memberships-for-groups"></a>Gruplar için dinamik üyelik sorunlarını giderme

**Bir grubu üzerinde bir kural yapılandırdım ancak hiçbir üyeliklerini grubunda güncelleştirilir**<br/>
1. Kullanıcı veya cihaz özniteliklerine kuralında değerleri doğrulayın. Kural karşılayan kullanıcılar emin olun. Cihazlar için cihaz özellikleri, eşitlenen tüm öznitelikleri beklenen değerleri içeren emin olmak için kontrol edin.<br/>
2. Tam olup olmadığını onaylamak için işleme durumu üyeliğini denetleyin. Denetleyebilirsiniz [işleme durumu üyelik](groups-create-rule.md#check-processing-status-for-a-rule) ve son güncelleştirme tarihi üzerinde **genel bakış** grup için sayfa.

Her şey iyi görünüyor, lütfen grubun doldurulması için bir süre bekleyin. Kiracınızın boyutuna bağlı olarak, ilk seferinde veya kural değişikliğinden sonra grubun doldurulması 24 saat kadar sürebilir.

**Bir kural yapılandırdım ancak kuralının var olan üyeleri artık kaldırılır**<br/>Bu beklenen bir davranıştır. Kural etkin veya değiştirildiğinde grubun mevcut üyeleri kaldırılır. Kuralın değerlendirmesinden gelen döndürülen kullanıcıların gruba üye olarak eklenir.

**Ne zaman ekleyin veya neden bir kural, değiştirme instantly üyelikleri de değişir göremiyorum?**<br/>Özel üyelik değerlendirmesi zaman uyumsuz bir planda düzenli gerçekleştirilir. İşlemin ne kadar sürer, dizininizdeki kullanıcı sayısı ve kural sonucu olarak oluşturulan grubu boyutu tarafından belirlenir. Genellikle, kullanıcılar az sayıda dizinlerle grup üyeliği değişikliklerinin küçüktür birkaç dakika içinde görürsünüz. Çok sayıda kullanıcı dizinlerle 30 dakika alabilir veya doldurmak için daha uzun.

**Şimdi işlenecek grubu nasıl zorlayabilir miyim?**<br/>
Şu anda, otomatik olarak isteğe bağlı olarak işlenmek üzere grubun tetiklemek için hiçbir yolu yoktur. Ancak, sonunda bir boşluk eklemek için üyelik kuralını güncelleştirerek yeniden işlemeyerek el ile tetikleyebilirsiniz.  

**İşleme hatası bir kural karşılaştım**<br/>Aşağıdaki tabloda, sık karşılaşılan dinamik üyelik kuralı hataları ve bunların nasıl düzeltileceği listeler.

| Kural ayrıştırıcı hatası | Hata kullanımı | Düzeltilmiş kullanımı |
| --- | --- | --- |
| Hata: Özniteliği desteklenmiyor. |(user.invalidProperty - eq "Value") |(user.department - eq "value")<br/><br/>Öznitelik olduğundan emin olun üzerinde [desteklenen özellikler listesinde](groups-dynamic-membership.md#supported-properties). |
| Hata: İşleç özniteliği desteklenmiyor. |(user.accountEnabled-true içerir) |(user.accountEnabled -eq true)<br/><br/>Özellik türü için kullanılan işleci desteklenmiyor (Bu örnekte,-içeren kullanılamaz boolean türü). Doğru operators, özellik türü için kullanın. |
| Hata: Sorgu derleme hatası. | 1. (user.department - eq "Sales") (user.department - eq "Pazarlama")<br>2. (user.userPrincipalName-eşleşen "*@domain.ext") | 1. İşleç eksik. Kullanın - veya - veya iki doğrulamaları katılın<br>(user.department - eq "Sales")- veya (user.department - eq "Pazarlama")<br>2. Hata - ile kullanılan normal ifade ile eşleşen<br>(user.userPrincipalName-eşleşen ". *@domain.ext")<br>veya alternatif olarak: (user.userPrincipalName-eşleşen "@domain.ext$") |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
* [Azure Active Directory’de Uygulama Yönetimi](../manage-apps/what-is-application-management.md)
* [Azure Active Directory nedir?](../fundamentals/active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)