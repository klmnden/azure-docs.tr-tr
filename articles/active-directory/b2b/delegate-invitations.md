---
title: Azure Active Directory B2B işbirliği için davet temsilci | Microsoft Docs
description: Azure Active Directory B2B işbirliği kullanıcı özellikleri yapılandırılabilir
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/23/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 0c7b0e3dd4d2ab98bc0f0bedc06424b7838fcf9e
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34267519"
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B işbirliği için temsilci davetleri

Azure Active Directory (Azure AD) işletmeden işletmeye (B2B) işbirliğiyle Davetleri Gönder için genel yönetici olmanız gerekmez. Bunun yerine, ilkeleri kullanın ve kullanıcıları, rolleri Davetleri Gönder izin davetleri atayabilirsiniz. Konuk kullanıcı davetleri temsilci için önemli bir yeni yol Konuk davet eden rolüdür.

## <a name="guest-inviter-role"></a>Konuk davet eden rolü
Biz kullanıcı davet göndermek için konuk davet eden rolüne atayabilirsiniz. Davetiye göndermek için genel yönetici rolünün üyesi olması gerekmez. Varsayılan olarak, genel yönetici davetleri normal kullanıcılar için devre dışı bırakılmamışsa normal kullanıcıların davet API de çalıştırabilirsiniz. Bir kullanıcı Azure portal veya PowerShell kullanarak API de çalıştırabilirsiniz.

PowerShell Konuk davet eden role bir kullanıcı eklemek için nasıl kullanılacağını gösteren örnek aşağıda verilmiştir:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Davet edebilirsiniz denetimi

![Davet etme denetleme](media/delegate-invitations/control-who-to-invite.png)

Azure AD B2B işbirliği ile bir kiracı Yöneticisi aşağıdaki davet ilkeleri ayarlayabilirsiniz:

- Davetiye devre dışı bırakma
- Yalnızca Yöneticiler ve kullanıcılar Konuk davet eden rolündeki davet edebilirsiniz
- Yöneticiler, Konuk davet eden rolü ve üyeleri davet edebilirsiniz
- Konuklar, dahil tüm kullanıcıları davet edebilir

Varsayılan olarak, kiracılar #4'e ayarlanır. (Tüm kullanıcılar, Konuklar, dahil B2B kullanıcıları davet edebilirsiniz.)

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Davetiye olmayan B2B işbirliği Konuk kullanıcılar ekleme](add-user-without-invite.md)
- [Bir role B2B işbirliği kullanıcı ekleme](add-guest-to-role.md)


