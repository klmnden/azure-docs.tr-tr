---
title: Güvenlik Uyarıları Azure kaynak rolleri için PIM - Azure Active Directory yapılandırma | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure kaynak rolleri için güvenlik uyarıları yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: c15e4080308c3e7e2ff54312cd91fa1f3d68668a
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65602375"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-pim"></a>Güvenlik Uyarıları Azure kaynak rolleri için PIM içinde yapılandırma
Azure Active Directory (Azure AD) Privileged Identity Management (PIM), ortamınızda şüpheli veya güvenli olmayan bir etkinlik olduğunda uyarılar oluşturur. Bir uyarı tetiklendiğinde, bunu bir uyarılar sayfasında gösterilir. 

![Uyarılar sayfası](media/pim-resource-roles-configure-alerts/rbac-alerts-page.png)

## <a name="review-alerts"></a>Uyarıları gözden geçirin
Düzeltme önerileri ile birlikte, uyarıyı tetikleyen rollerin listeleyen bir rapor görmek için bir uyarı seçin.

![Uyarı raporu](media/pim-resource-roles-configure-alerts/rbac-alert-info.png)

## <a name="alerts"></a>Uyarılar
| Uyarı | Severity | Tetikleyici | Öneri |
| --- | --- | --- | --- |
| **Bir kaynağa çok fazla sahip atandı** |Ortam |Çok sayıda kullanıcı, sahip rolünün sahip. |Listedeki kullanıcıları gözden geçirin ve bazı az ayrıcalıklı roller için yeniden atayın. |
| **Bir kaynağa çok fazla kalıcı sahip atandı** |Ortam |Çok sayıda kullanıcı, bir rol kalıcı olarak atanır. |Listedeki kullanıcıları gözden geçirin ve bazı etkinleştirme rol kullanması için yeniden atayın. |
| **Yinelenen rol oluşturuldu** |Ortam |Birden çok rol aynı ölçütlerine sahiptirler. |Bu roller yalnızca birini kullanın. |


### <a name="severity"></a>Severity
* **Yüksek**: Acil eylem ilke ihlali nedeniyle gerektirir. 
* **Orta**: Acil eylem gerektirmeyen ancak olası bir ilke ihlali bildirir.
* **Düşük**: Acil eylem gerektirmeyen ancak tercih edilen ilke değişikliğini önerir.

## <a name="configure-security-alert-settings"></a>Güvenlik Uyarısı Ayarları yapılandırma
Uyarılar sayfasından Git **ayarları**.
![Ayarlar](media/pim-resource-roles-configure-alerts/rbac-navigate-settings.png)

Güvenlik hedefleri ve ortam ile çalışmak için farklı uyarıların ayarlarını özelleştirin.
![Ayarları özelleştirme](media/pim-resource-roles-configure-alerts/rbac-alert-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik Uyarıları Azure kaynak rolleri için PIM içinde yapılandırma](pim-resource-roles-configure-alerts.md)
