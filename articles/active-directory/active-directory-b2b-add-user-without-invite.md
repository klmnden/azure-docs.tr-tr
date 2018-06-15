---
title: B2B işbirliği kullanıcılar davetsiz Azure Active Directory'ye ekleme | Microsoft Docs
description: Azure Active Directory B2B işbirliği daveti redeeming olmadan diğer Konuk kullanıcılar için Azure AD eklemek Konuk kullanıcı izin verebilirsiniz.
services: active-directory
documentationcenter: ''
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 5a37a5a14dcb07db7e078558072f7edad7432d9b
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075632"
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a>Davetiye olmayan B2B işbirliği Konuk kullanıcılar ekleme

> [!NOTE]
> Şimdi, Konuk kullanıcılar artık davet e-posta dışında bazı özel durumlarda gerekir. Daha fazla bilgi için bkz: [B2B işbirliği davet kullanım](active-directory-b2b-redemption-experience.md).  

Kullanıcılar, kullanılan için davet gerek kalmadan kuruluşunuz için iş ortağı eklemek için bir iş ortağı temsilcisi gibi bir kullanıcı izin verebilirsiniz. Yapmanız gereken tek şey bu kullanıcı için iş ortağı Krlş. kullanmakta olduğunuz dizinde numaralandırması ayrıcalıkları vermek 

Bunlar ayrıcalıkları ne zaman verin:

1. İş ortağı kuruluştan bir kullanıcı bir kullanıcı ana bilgisayar kuruluştaki (örneğin, WoodGrove) başvurulmasını (örneğin, Sam@litware.com) konuk olarak.
2. Konak kuruluşunuzdaki Yönetim [ilkeleri oluşturur](active-directory-b2b-delegate-invitations.md) tanımlamak ve iş ortağı kuruluştan (Lıtware) diğer kullanıcıları eklemek Sam izin verin.
3. Artık Sam diğer kullanıcıların Lıtware WoodGrove dizini, grupları veya uygulamalar için kullanılan için davet gerek kalmadan ekleyebilirsiniz. SAM Lıtware içinde uygun numaralandırma ayrıcalıkları varsa, otomatik olarak gerçekleşir.

### <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Bilgi çalışanları B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-iw-add-users.md)
- [B2B işbirliği davet kullanım](active-directory-b2b-redemption-experience.md)
- [Azure Active Directory B2B işbirliği için temsilci davetleri](active-directory-b2b-delegate-invitations.md)

