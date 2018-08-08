---
title: E-posta bildirimleri Azure AD PIM | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) e-posta bildirimleri açıklar
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 07/24/2018
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.openlocfilehash: 7943b4fb8c2027b50ce04c30d21f1b0a58f98ace
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621591"
---
# <a name="email-notifications-in-azure-ad-pim"></a>Azure AD PIM e-posta bildirimleri

Azure AD Privileged Identity Management (PIM) anahtar olaylar meydana geldiğinde, ilgili yönetici veya kullanıcı için e-posta bildirimi gönderilir. Örneğin, PIM aşağıdaki olaylar için e-posta gönderir:

- Ayrıcalıklı rol Etkinleştirme Onayı Beklemede olduğunda
- Ne zaman ayrıcalıklı rol etkinleştirme isteği Onaylandı
- Ayrıcalıklı rol etkinleştirildiğinde
- Ayrıcalıklı rol atandığında
- Azure AD PIM etkinleştirildiğinde

PIM ile gönderilen e-posta bildirimleri Temmuz 2018 sonunda başlayarak, yeni bir gönderen e-posta adresi ve yeni bir görsel tasarım olacaktır. Bu güncelleştirme, Azure AD için her iki PIM etkiler ve Azure kaynakları için PIM. Daha önce bir e-posta bildirimi tetiklenen tüm olaylar bir e-posta göndermeye devam eder. Bazı e-posta içeriği güncelleştirdiniz mi daha hedefe yönelik bilgileri sağlama.

## <a name="sender-email-address"></a>Gönderenin e-posta adresi

E-posta bildirimleri, Temmuz 2018 sonunda başlayarak, aşağıdaki adresi vardır:

- E-posta adresi:  **azure-noreply@microsoft.com**
- Görünen ad: Microsoft Azure

Daha önce aşağıdaki adrese e-posta bildirimleri sahipti:

- E-posta adresi:  **azureadnotifications@microsoft.com**
- Görünen ad: Microsoft Azure AD bildirim hizmeti

## <a name="email-subject-line"></a>E-posta konu satırı

Temmuz 2018'den hem Azure AD için e-posta bildirimleri ve Azure kaynağı rolleri olacaktır sonunda başlangıç bir **PIM** konu satırı önek. Bir örneği aşağıda verilmiştir:

- PIM: Alain Charon kalıcı olarak yedekleme okuyucu rolü atandı.

## <a name="pim-emails-for-azure-ad-roles"></a>PIM e-postalar için Azure AD rolleri

Azure AD rolleri için PIM e-posta bildirimleri, Temmuz 2018 sonunda başlayarak, güncelleştirilmiş bir tasarım sahip. Aşağıda kurgusal Contoso kuruluş için ayrıcalıklı bir role kullanıcı etkinleştirirken, gönderilen örnek e-posta gösterilmektedir.

![Yeni PIM e-posta için Azure AD rolleri](./media/pim-email-notifications/email-directory-new.png)

Daha önce bir kullanıcı bir ayrıcalıklı rol etkinleştirildiğinde, e-posta aşağıdaki gibi görünüyordu.

![Eski PIM e-posta için Azure AD rolleri](./media/pim-email-notifications/email-directory-old.png)

## <a name="pim-emails-for-azure-resource-roles"></a>Azure kaynak rolleri için PIM postalar

Azure kaynak rolleri için PIM e-posta bildirimleri, Temmuz 2018 sonunda başlayarak, güncelleştirilmiş bir tasarım sahip. Aşağıda kurgusal Contoso kuruluş için ayrıcalıklı bir role atanmış bir kullanıcı, gönderilen örnek e-posta gösterilmektedir.

![Yeni PIM için e-posta Azure kaynağı rolleri](./media/pim-email-notifications/email-resources-new.png)

Daha önce bir kullanıcı bir ayrıcalıklı rol atandığında, e-posta aşağıdaki gibi görünüyordu.

![Eski PIM için e-posta Azure kaynağı rolleri](./media/pim-email-notifications/email-resources-old.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD PIM rol etkinleştirme ayarlarını yönetme](pim-how-to-change-default-settings.md)
- [Azure AD PIM onayları](azure-ad-pim-approval-workflow.md)
