---
title: Güvenlik Uyarıları Azure kaynak rolleri için PIM içinde yapılandırma | Microsoft Docs
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
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 2fa63cf2fa05f2cde4558f0bea38bfd7f17df3ae
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43342071"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-pim"></a>Güvenlik Uyarıları Azure kaynak rolleri için PIM içinde yapılandırma
Azure kaynakları için privileged Identity Management (PIM), ortamınızda şüpheli veya güvenli olmayan bir etkinlik olduğunda uyarılar oluşturur. Bir uyarı tetiklendiğinde, bunu bir uyarılar sayfasında gösterilir. 

![Uyarılar sayfası](media/azure-pim-resource-rbac/RBAC-alerts-home.png)

## <a name="review-alerts"></a>Uyarıları gözden geçirin
Düzeltme önerileri ile birlikte, uyarıyı tetikleyen rollerin listeleyen bir rapor görmek için bir uyarı seçin.

![Uyarı raporu](media/azure-pim-resource-rbac/rbac-alert-info.png)

## <a name="alerts"></a>Uyarılar
| Uyarı | Severity | Tetikleyici | Öneri |
| --- | --- | --- | --- |
| **Bir kaynağa çok fazla sahip atandı** |Orta |Çok sayıda kullanıcı, sahip rolünün sahip. |Listedeki kullanıcıları gözden geçirin ve bazı az ayrıcalıklı roller için yeniden atayın. |
| **Bir kaynağa çok fazla kalıcı sahip atandı** |Orta |Çok sayıda kullanıcı, bir rol kalıcı olarak atanır. |Listedeki kullanıcıları gözden geçirin ve bazı etkinleştirme rol kullanması için yeniden atayın. |
| **Yinelenen rol oluşturuldu** |Orta |Birden çok rol aynı ölçütlerine sahiptirler. |Bu roller yalnızca birini kullanın. |


### <a name="severity"></a>Severity
* **Yüksek**: ilke ihlali nedeniyle Acil eylem gerektirir. 
* **Orta**: Acil eylem gerektirmeyen ancak olası bir ilke ihlali bildirir.
* **Düşük**: Acil eylem gerektirmeyen ancak tercih edilen ilke değişikliğini önerir.

## <a name="configure-security-alert-settings"></a>Güvenlik Uyarısı Ayarları yapılandırma
Uyarılar sayfasından Git **ayarları**.
![Ayarlar](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Güvenlik hedefleri ve ortam ile çalışmak için farklı uyarıların ayarlarını özelleştirin.
![Ayarları özelleştirme](media/azure-pim-resource-rbac/rbac-alert-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik Uyarıları Azure kaynak rolleri için PIM içinde yapılandırma](pim-resource-roles-configure-alerts.md)
