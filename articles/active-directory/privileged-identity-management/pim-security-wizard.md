---
title: Güvenlik Sihirbazı PIM - Azure | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) kullandığınız ilk kez Güvenlik Sihirbazı'nı açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 02/27/2017
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 178a4c5e978075f2a59b22a1cccf462138527964
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189088"
---
# <a name="security-wizard-in-pim"></a>PIM Güvenlik Sihirbazı
Kuruluşunuz için Azure Privileged Identity Management (PIM) çalıştırmak için ilk kişi siz, sihirbaz ile sunulur. Sihirbaz ayrıcalıklı kimlikleri ve bu riskleri azaltmak için PIM'i kullanma güvenlik risklerini anlamanıza yardımcı olur. Daha sonra yapmayı tercih ediyorsanız sihirbazında, mevcut rol atamaları herhangi bir değişiklik yapmak gerekmez.

## <a name="what-to-expect"></a>Sizi neler bekliyor
PIM kullanarak kuruluşunuzun başlamadan önce tüm rol atamalarını kalıcı: şu anda ayrıcalıklarını gerekmeyen olsa bile her zaman bu rollere kullanıcılardır.  Sihirbazın ilk adımı, yüksek ayrıcalıklı rollerin bir listesini ve kaç kullanıcının şu anda bu rollerdeki olduğunu gösterir. Daha fazla bilginiz ya da varsa kullanıcılar hakkında daha fazla bilgi edinmek için belirli bir rol için detaya gidebilirsiniz.

Sihirbazın ikinci adım, yöneticinin rol atamalarını değiştirme olanağı sağlar.  

> [!WARNING]
> En az bir genel yönetici ve bir kurumsal hesap (Microsoft hesabı değil) ile birden fazla ayrıcalıklı Rol Yöneticisi olması önemlidir. Kuruluş yalnızca bir ayrıcalıklı Rol Yöneticisi varsa, o hesabı silinirse, PIM yönetmek mümkün olmayacaktır.
> Ayrıca, rol atamalarını bir kullanıcının bir Microsoft hesabı (Skype ve Outlook.com gibi Microsoft hizmetlerinde oturum açarken kullandıkları bir hesap) varsa kalıcı olarak tut. Bu rol için etkinleştirme için mfa'yı gerekli planlıyorsanız, bu kullanıcı kilitlenir.
> 
> 

Değişiklikleri yaptıktan sonra Sihirbazı artık görünür. Siz veya başka bir ayrıcalıklı Rol Yöneticisi PIM, sonraki açışınızda, PIM Pano görürsünüz.  

* Eklemek veya kullanıcıların rollerini kaldırın veya atamaları kalıcı uygun duruma getirmek için değiştirmek istiyorsanız, daha fazla bilgi okuyun [nasıl kullanıcı rolü ekleme veya kaldırma](pim-how-to-add-role-to-user.md).
* Daha fazla kullanıcının PIM yönetmek için erişim vermek istiyorsanız, daha fazla bilgi okuyun [PIM'de yönetmek için erişim vermek nasıl](pim-how-to-give-access-to-pim.md).

## <a name="next-steps"></a>Sonraki adımlar

- [PIM kullanmaya başlayın](pim-getting-started.md)
- [Azure AD dizin rollerini PIM atayın](pim-how-to-add-role-to-user.md)
- [PIM yönetmek için diğer yöneticilere erişim izni ver](pim-how-to-give-access-to-pim.md)
