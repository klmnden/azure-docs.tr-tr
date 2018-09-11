---
title: E-posta bildirimleri PIM - Azure | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) e-posta bildirimleri tanımlar.
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
ms.date: 09/07/2018
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.openlocfilehash: de1d29d3ab1b370257c3a2d6b6ff9f677197fc2a
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44303073"
---
# <a name="email-notifications-in-pim"></a>PIM e-posta bildirimleri

Azure AD Privileged Identity Management (PIM) anahtar olaylar meydana geldiğinde, e-posta bildirimi gönderilir. Örneğin, PIM aşağıdaki olaylar için e-posta gönderir:

- Ayrıcalıklı rol Etkinleştirme Onayı Beklemede olduğunda
- Ayrıcalıklı rol etkinleştirme isteği tamamlandığında
- Ayrıcalıklı rol etkinleştirildiğinde
- Ayrıcalıklı rol atandığında
- Azure AD PIM etkinleştirildiğinde

Aşağıdaki yöneticilere e-posta bildirimi gönderilir:

- Ayrıcalıklı Rol Yöneticisi
- Güvenlik Yöneticisi

Aşağıdaki olaylar için ayrıcalıklı rol olan son kullanıcıya e-posta bildirimleri de gönderilir:

- Ayrıcalıklı rol etkinleştirme isteği tamamlandığında
- Ayrıcalıklı rol atandığında

PIM e-posta bildirimi, Temmuz 2018 sonunda başlayarak, yeni bir gönderen e-posta adresi ve yeni bir görsel tasarım sahip. Bu güncelleştirme, Azure AD için her iki PIM etkiler ve Azure kaynakları için PIM. Daha önce bir e-posta bildirimi tetiklenen tüm olaylar bir e-posta göndermeye devam eder. Bazı e-posta içeriği güncelleştirdiniz mi daha hedefe yönelik bilgileri sağlama.

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

- [PIM'de Azure AD dizini rol ayarlarını yapılandırma](pim-how-to-change-default-settings.md)
- [Onaylayın veya Azure AD dizin rollerini PIM isteklerini reddedin](azure-ad-pim-approval-workflow.md)
