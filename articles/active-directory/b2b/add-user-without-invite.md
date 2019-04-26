---
title: B2B Konukları davet bağlantısı veya e-posta - Azure Active Directory olmayan Ekle | Microsoft Docs
description: Diğer Konuk kullanıcıları davet Azure Active Directory B2B işbirliği içinde çalışan kuponumu kullanmakta olmadan Azure AD'nize ekleyin. Konuk kullanıcı izin verebilirsiniz.
services: active-directory
documentationcenter: ''
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1dca03f84b821d20ee6fecbaec24a1aa840836b5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60357146"
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation-link-or-email"></a>B2B işbirliği Konuk kullanıcıları davet bağlantısı veya e-posta olmayan Ekle

Artık paylaşılan bir uygulamanın doğrudan bağlantısını göndererek Konuk kullanıcılar davet edebilirsiniz. Bu yöntem ile Konuk kullanıcılar artık davet e-posta dışında bazı özel durumlarda kullanmanız gerekir. Konuk kullanıcı uygulama bağlantıya tıklar, inceler ve gizlilik koşullarını kabul eder ve uygulama sorunsuz bir şekilde erişir. Daha fazla bilgi için [B2B işbirliği Davetiyesi kullanımı](redemption-experience.md).   

Bu yeni bir yöntem yoktu önce bir davet eden (Kuruluşunuz veya bir iş ortağı kuruluş) ekleyerek davet e-posta gerek kalmadan Konuk kullanıcıları davet **Konuk davet eden** dizin rolü ve ardından Dizin, grupları ve uygulamaları kullanıcı Arabirimi veya PowerShell aracılığıyla Konuk kullanıcıları eklemek davet eden sahip. (PowerShell kullanıyorsanız, davet e-posta bütünüyle gizleyebilirsiniz). Örneğin:

1. İş ortağı kuruluştan bir kullanıcı (örneğin, WoodGrove) konak kuruluşunuzdaki bir kullanıcı davet eder (örneğin, Sam@litware.com) konuk olarak.
2. Konak kuruluşun Yöneticisi [ilkeleri ayarlar](delegate-invitations.md) Sam tanımlamak ve iş ortağı kuruluştan (Litware) diğer kullanıcıları eklemek izin verin. (Sam eklenmelidir **Konuk davet eden** rol.)
3. Artık, Sam diğer kullanıcıların Litware WoodGrove dizini, grupları veya uygulamalar için davet kullanılamadı gerek kalmadan ekleyebilirsiniz. SAM Litware uygun numaralandırma ayrıcalıkları varsa, otomatik olarak gerçekleşir.
 
Bu özgün yöntem çalışır. Bununla birlikte, küçük bir davranışı fark yoktur. PowerShell kullanıyorsanız, davet edilen Konuk hesabı artık olduğunu fark edeceksiniz bir **PendingAcceptance** durumu hemen gösteren yerine **kabul edilen**. Durum bekleyen olsa da, Konuk kullanıcı hala oturum açabilir ve bir e-posta davetiyesi bağlantıya tıklamak olmadan uygulamayı erişebilir. Kullanıcı henüz geçtiğini değil, bekleme durumu anlamına gelir [deneyimi onay](redemption-experience.md#privacy-policy-agreement), burada bu kullanıcılar davet eden kuruluştan gizlilik koşullarını kabul edin. İlk kez oturum açtığında Konuk kullanıcı, bu onay ekranında görür. 

Dizine kullanıcı davet et varsa Konuk kullanıcı kaynak kiracıya özgü Azure portalına erişmek gerekir doğrudan URL (gibi https://portal.azure.com/ *resourcetenant*. onmicrosoft.com) görüntüleyebilir ve gizlilik koşullarını kabul etmiş olursunuz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği Davetiyesi kullanımı](redemption-experience.md)
- [Azure Active Directory B2B işbirliği için temsilci davetleri](delegate-invitations.md)
- [Bilgi çalışanları B2B işbirliği kullanıcılarını nasıl ekleyebilir?](add-users-information-worker.md)

