---
title: Azure AD PIM - Azure Active Directory rolleri Güvenlik Sihirbazı | Microsoft Docs
description: Dönüştürme kalıcı ayrıcalıklı Azure AD rol atamaları için Azure AD Privileged Identity Management (PIM) kullanarak uygun için kullanabileceğiniz Güvenlik Sihirbazı'nı açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: aa4fd850ac2116dc7f353eea87845501fff020bb
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476231"
---
# <a name="azure-ad-roles-security-wizard-in-pim"></a>Azure AD PIM rolleri Güvenlik Sihirbazı

Kuruluşunuz için Azure Active Directory (Azure AD) Privileged Identity Management (PIM) çalıştırmak için ilk kişi siz, sihirbaz ile sunulur. Sihirbaz ayrıcalıklı kimlikleri ve bu riskleri azaltmak için PIM'i kullanma güvenlik risklerini anlamanıza yardımcı olur. Daha sonra yapmayı tercih ediyorsanız sihirbazında, mevcut rol atamaları herhangi bir değişiklik yapmak gerekmez.

## <a name="wizard-overview"></a>Sihirbazına genel bakış

PIM kullanarak kuruluşunuzun başlamadan önce tüm rol atamalarını kalıcı: şu anda ayrıcalıklarını gerekmeyen olsa bile her zaman bu rollere kullanıcılardır. Sihirbazın ilk adımı, yüksek ayrıcalıklı rollerin bir listesini ve kaç kullanıcının şu anda bu rollerdeki olduğunu gösterir. Daha fazla bilginiz ya da varsa kullanıcılar hakkında daha fazla bilgi edinmek için belirli bir rol için detaya gidebilirsiniz.

Sihirbazın ikinci adım, yöneticinin rol atamalarını değiştirme olanağı sağlar.  

> [!WARNING]
> En az bir genel yönetici ve ayrıcalıklı Rol Yöneticisi bir kurumsal hesap (Microsoft hesabı değil) ile birden fazla olması önemlidir. Kuruluş yalnızca bir ayrıcalıklı Rol Yöneticisi varsa, o hesabı silinirse, PIM yönetmek mümkün olmayacaktır.
> Ayrıca, rol atamalarını bir kullanıcının bir Microsoft hesabı (Skype ve Outlook.com gibi Microsoft hizmetlerinde oturum açarken kullandıkları bir hesap) varsa kalıcı olarak tut. Bu rol için etkinleştirme için mfa'yı gerekli planlıyorsanız, bu kullanıcı kilitlenir.

## <a name="run-the-wizard"></a>Sihirbazı çalıştırma

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD rolleri** ve ardından **Sihirbazı**.

    ![Azure AD rolleri - Sihirbaz Sayfası Sihirbazı çalıştırmak için 3 adımları gösteren](./media/pim-security-wizard/wizard-start.png)

1. Tıklayın **1 bulma ayrıcalıklı rolleri**.

1. Kalıcı veya uygun kullanıcıları görmek için ayrıcalıklı rolleri listesini gözden geçirin.

    ![Ayrıcalıklı roller - kalıcı ve uygun üyelerine gösteren rolü bölmesi keşfedin](./media/pim-security-wizard/discover-privileged-roles-users.png)

1. Tıklayın **sonraki** uygun hale getirmek istediğiniz üyeleri seçin.

    ![Uygun rolleri için uygun hale getirmek istediğiniz üyeleri seçmek için Seçenekleri sayfası üyeleri Dönüştür](./media/pim-security-wizard/convert-members-eligible.png)

1. Üyelerini seçtikten sonra tıklayın **sonraki**.

    ![Dönüştürülecek kalıcı rol atamaları üyeleriyle gösteren değişiklikler sayfasını gözden geçirin](./media/pim-security-wizard/review-changes.png)

1. Tıklayın **Tamam** kalıcı atamaları uygun hale getirme için.

    Dönüştürme tamamlandığında bir bildirim görürsünüz.

    ![Dönüştürme durumunu gösteren bir bildirim](./media/pim-security-wizard/notification-completion.png)

Diğer ayrıcalıklı rol atamaları için uygun dönüştürmek gerekirse Sihirbazı yeniden çalıştırabilirsiniz. PIM arabirimi yerine Sihirbazı'nı kullanmak istiyorsanız, bkz. [Azure AD PIM Rolleri Ata](pim-how-to-add-role-to-user.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD PIM Rolleri Ata](pim-how-to-add-role-to-user.md)
- [PIM yönetmek için diğer yöneticilere erişim izni ver](pim-how-to-give-access-to-pim.md)
