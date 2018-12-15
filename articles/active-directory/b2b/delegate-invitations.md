---
title: Temsilci davetleri Azure Active Directory B2B işbirliği | Microsoft Docs
description: Azure Active Directory B2B işbirliği kullanıcı özellikleri yapılandırılabilir
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 8abbbba0cdac783dd315cd1f0965e03c4e5a1a05
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2018
ms.locfileid: "53434554"
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B işbirliği için temsilci davetleri

Azure Active Directory (Azure AD) ile işletmeden işletmeye (B2B) işbirliği, davet göndermek için genel yönetici olması gerekmez. Bunun yerine, ilkeleri kullanın ve temsilci davetleri kullanıcıları, rolleri davet göndermek izin vermek için. Konuk kullanıcı davetlerini temsilci seçmek için önemli bir yeni yol ile Konuk davet eden rolüdür.

## <a name="guest-inviter-role"></a>Konuk davet eden rolü
Biz, kullanıcı davet göndermek için konuk davet eden rolü atayabilirsiniz. Davet göndermek için genel yönetici rolünün üyesi olmanız gerekmez. Varsayılan olarak, normal kullanıcı genel yönetici davetleri normal kullanıcılar için devre dışı bırakılmamışsa davet API de çağırabilirsiniz. Bir kullanıcı, Azure portal veya PowerShell kullanarak API de çağırabilirsiniz.

Konuk davet eden rolüne kullanıcı eklemek için PowerShell'i kullanmayı gösteren bir örnek aşağıda verilmiştir:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Davet edebilir denetimi

Azure Active Directory'de seçin **kullanıcı ayarları**. Altında **dış kullanıcılar**seçin **dış işbirliği ayarlarını Yönet**.

> [!NOTE]
> **Dış işbirliği ayarları** ayrıca kullanılabilir **kuruluş ilişkileri** sayfası. Azure Active Directory'de altında **Yönet**Git **kuruluş ilişkileri** > **ayarları**.

![Dış işbirliği ayarları](./media/delegate-invitations/control-who-to-invite.png)

Azure AD B2B işbirliği ile bir kiracı Yöneticisi aşağıdaki davet ilkeleri ayarlayabilirsiniz:

- Davet Kapat
- Yalnızca Yöneticiler ve Konuk davet eden rolündeki kullanıcılar davet edebilir
- Yöneticiler, Konuk davet eden rolü ve üyeler davet edebilir
- Konuklar, dahil olmak üzere tüm kullanıcıları davet edebilir

Varsayılan olarak, kiracılar #4'e ayarlanır. (Tüm kullanıcılar, Konuklar, dahil olmak üzere B2B kullanıcıları davet edebilirsiniz.)

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği hakkında aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği Konuk kullanıcıları davet etmeden ekleme](add-user-without-invite.md)
- [Bir role B2B işbirliği kullanıcısı ekleme](add-guest-to-role.md)


