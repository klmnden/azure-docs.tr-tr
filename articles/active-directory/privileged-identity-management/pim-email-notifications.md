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
ms.component: protection
ms.date: 07/14/2018
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.openlocfilehash: 6c329554b5854f113fb216f874fa5a918110f9c5
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059784"
---
# <a name="email-notifications-in-azure-ad-pim"></a>Azure AD PIM e-posta bildirimleri

Azure AD Privileged Identity Management (PIM) anahtar olaylar meydana geldiğinde, ilgili yönetici veya kullanıcı için e-posta bildirimi gönderilir. Örneğin, PIM aşağıdaki olaylar için e-posta gönderir:

- Ayrıcalıklı rol Etkinleştirme Onayı Beklemede olduğunda
- Ne zaman ayrıcalıklı rol etkinleştirme isteği Onaylandı
- Ayrıcalıklı rol etkinleştirildiğinde
- Ayrıcalıklı rol atandığında
- Azure AD PIM etkinleştirildiğinde

PIM ile gönderilen e-posta bildirimleri Temmuz 2018 sonunda başlayarak, yeni bir gönderen e-posta adresi ve yeni bir görsel tasarım olacaktır. Bu güncelleştirme, Azure AD için her iki PIM etkiler ve Azure kaynakları için PIM. Daha önce bir e-posta bildirimi tetiklenen tüm olaylar bir e-posta göndermeye devam eder. Bu güncelleştirme yalnızca visual işlevselliği değişiklik yapmadan farklıdır.

## <a name="sender-email-address"></a>Gönderenin e-posta adresi

E-posta bildirimleri, Temmuz 2018 sonunda başlayarak, aşağıdaki adresi vardır:

- E-posta adresi:  **azure-noreply@microsoft.com**
- Görünen ad: Microsoft Azure

Daha önce aşağıdaki adrese e-posta bildirimleri sahipti:

- E-posta adresi:  **azureadnotifications@microsoft.com**
- Görünen ad: Microsoft Azure AD bildirim hizmeti

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
