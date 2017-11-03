---
title: "Azure Active Directory B2B işbirliği için davet temsilci | Microsoft Docs"
description: "Azure Active Directory B2B işbirliği kullanıcı özellikleri yapılandırılabilir"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 78613cc978b585a98d235245194c02371f7f3849
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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

![Davet etme denetleme](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

Azure AD B2B işbirliği ile bir kiracı Yöneticisi aşağıdaki davet ilkeleri ayarlayabilirsiniz:

- Davetiye devre dışı bırakma
- Yalnızca Yöneticiler ve kullanıcılar Konuk davet eden rolündeki davet edebilirsiniz
- Yöneticiler, Konuk davet eden rolü ve üyeleri davet edebilirsiniz
- Konuklar, dahil tüm kullanıcıları davet edebilir

Varsayılan olarak, kiracılar #4'e ayarlanır. (Tüm kullanıcılar, Konuklar, dahil B2B kullanıcıları davet edebilirsiniz.)

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B işbirliği kullanıcı özellikleri](active-directory-b2b-user-properties.md)
* [Bir role B2B işbirliği kullanıcı ekleme](active-directory-b2b-add-guest-to-role.md)
* [Dinamik gruplar ve B2B işbirliği](active-directory-b2b-dynamic-groups.md)
* [B2B işbirliği kodu ve PowerShell örnekleri](active-directory-b2b-code-samples.md)
* [SaaS uygulamaları B2B işbirliği için yapılandırma](active-directory-b2b-configure-saas-apps.md)
* [B2B işbirliği kullanıcı belirteçleri](active-directory-b2b-user-token.md)
* [B2B işbirliği kullanıcı taleplerini eşleme](active-directory-b2b-claims-mapping.md)
* [Office 365 dış paylaşım](active-directory-b2b-o365-external-user.md)
* [B2B işbirliği geçerli sınırlamalar](active-directory-b2b-current-limitations.md)
