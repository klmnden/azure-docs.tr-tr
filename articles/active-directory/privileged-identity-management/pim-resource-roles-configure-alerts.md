---
title: Privileged Identity Management'ı kullanarak Azure kaynakları için güvenlik uyarılarını yönetme | Microsoft Docs
description: PIM güvenlik uyarıları açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: c6c057541b3e3067de6331bab6ca9cccfa092710
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="manage-security-alerts-for-azure-resources-by-using-privileged-identity-management"></a>Privileged Identity Management'ı kullanarak Azure kaynakları için güvenlik uyarılarını yönetme
Ortamınızda kuşkulu veya güvenli olmayan bir etkinlik olduğunda ayrıcalıklı Kimlik Yönetimi (PIM) Azure kaynakları için uyarılar oluşturur. Bir uyarı tetiklendiğinde, uyarılar sayfasında görüntülenir. 

![Uyarılar sayfası](media/azure-pim-resource-rbac/RBAC-alerts-home.png)

## <a name="review-alerts"></a>Uyarıları gözden geçirin
Kullanıcılar ya da düzeltme önerileri yanı sıra uyarıyı tetikleyen roller listeleyen bir rapor görmek için bir uyarı seçin.

![Uyarı raporu](media/azure-pim-resource-rbac/rbac-alert-info.png)

## <a name="alerts"></a>Uyarılar
| Uyarı | Önem Derecesi | Tetikleyici | Öneri |
| --- | --- | --- | --- |
| **Bir kaynağa atanan çok fazla sahipleri** |Orta |Çok sayıda kullanıcı sahibi rolüne sahip. |Listedeki kullanıcılar gözden geçirin ve bazı daha az ayrıcalıklı rollere yeniden atayın. |
| **Bir kaynağa atanan çok fazla kalıcı sahipleri** |Orta |Çok sayıda kullanıcı, bir rol kalıcı olarak atanır. |Listedeki kullanıcılar gözden geçirin ve bazı rol kullanması için etkinleştirme için yeniden atayın. |
| **Oluşturulan yinelenen rol** |Orta |Birden çok rol aynı ölçütlerine sahiptirler. |Bu rolleri yalnızca birini kullanın. |


### <a name="severity"></a>Önem Derecesi
* **Yüksek**: bir ilke ihlali nedeniyle Acil eylem gerektirir. 
* **Orta**: Acil eylem gerekli değildir ancak olası ilke ihlalinin işaret eder.
* **Düşük**: Acil eylem gerekli değildir ancak tercih edilen ilke değişikliğini önerir.

## <a name="configure-security-alert-settings"></a>Güvenlik uyarı ayarlarını yapılandırma
Uyarıları sayfasından Git **ayarları**.
![Ayarlar](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Ortam ve Güvenlik amaçları ile çalışmak için farklı uyarıların ayarlarını özelleştirin.
![Ayarları özelleştirme](media/azure-pim-resource-rbac/rbac-alert-settings.png)
