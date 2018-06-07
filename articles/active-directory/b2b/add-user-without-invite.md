---
title: B2B işbirliği kullanıcılar davetsiz Azure Active Directory'ye ekleme | Microsoft Docs
description: Azure Active Directory B2B işbirliği daveti redeeming olmadan diğer Konuk kullanıcılar için Azure AD eklemek Konuk kullanıcı izin verebilirsiniz.
services: active-directory
documentationcenter: ''
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/21/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 50c64386f1eab07c299112617283b1d8d7295295
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34591060"
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a>Davetiye olmayan B2B işbirliği Konuk kullanıcılar ekleme

Ayrıca, paylaşılan bir uygulamaya doğrudan bağlantı göndererek Konuk kullanıcılar artık davet edebilirsiniz. Bu yöntemde, Konuk kullanıcılar artık davet e-posta dışında bazı özel durumlarda kullanmanız gerekir. Konuk kullanıcı uygulama bağlantıya tıklar, gözden geçirir ve gizlilik koşullarını kabul eder ve uygulama sorunsuz bir şekilde erişir. Daha fazla bilgi için bkz: [B2B işbirliği davet kullanım](redemption-experience.md).   

Bu yeni yöntemin bulunmamaktaydı önce bir davet eden (kuruluşunuzun veya iş ortağı kuruluş) ekleyerek davet e-posta gerek kalmadan Konuk kullanıcıları davet **Konuk davet eden** dizin rolünü ve ardından Dizin, grupları veya kullanıcı arabirimini veya PowerShell aracılığıyla uygulamaları Konuk kullanıcılar eklemek davet eden sahip. (PowerShell kullanıyorsanız davet e-posta tamamen gizleyebilirsiniz). Örneğin:

1. İş ortağı kuruluştan bir kullanıcı bir kullanıcı ana bilgisayar kuruluştaki (örneğin, WoodGrove) başvurulmasını (örneğin, Sam@litware.com) konuk olarak.
2. Konak kuruluş yönetici [ilkelerini ayarlar](delegate-invitations.md) tanımlamak ve iş ortağı kuruluştan (Lıtware) diğer kullanıcıları eklemek Sam izin verin. (Sam eklenmelidir **Konuk davet eden** rol.)
3. Artık, Sam diğer kullanıcıların Lıtware WoodGrove dizini, grupları veya uygulamalar için kullanılan için davet gerek kalmadan ekleyebilirsiniz. SAM Lıtware içinde uygun numaralandırma ayrıcalıkları varsa, otomatik olarak gerçekleşir.
 
Bu özgün yöntemi hala çalışmaktadır. Ancak, davranış küçük bir fark yoktur. PowerShell'i kullanırsanız, bir davet edilen Konuk hesabı artık olduğunu fark edeceksiniz bir **PendingAcceptance** hemen gösteren yerine durumu **kabul edilen**. Durum Beklemede olsa da, Konuk kullanıcı hala oturum açın ve bir e-posta daveti bağlantı tıklatıldığında olmadan uygulama erişebilir. Kullanıcı henüz geçtiğini değil, bekleme durumu anlamına gelir [deneyimi onayı](redemption-experience.md#privacy-policy-agreement)burada bu bunlar davet kuruluşun gizlilik koşullarını kabul edin. Bunlar ilk kez oturum açtığında Konuk kullanıcıya bu izni ekran görür. 

Dizine kullanıcı davet varsa, Konuk kullanıcıya kaynak Kiracı özgü Azure portalına erişmek gerekir doğrudan URL (gibi https://portal.azure.com/ *resourcetenant*. onmicrosoft.com) görüntüleyip gizlilik koşullarını kabul ediyorsunuz.

### <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği davet kullanım](redemption-experience.md)
- [Azure Active Directory B2B işbirliği için temsilci davetleri](delegate-invitations.md)
- [Bilgi çalışanları B2B işbirliği kullanıcıların nasıl eklenir?](add-users-information-worker.md)

