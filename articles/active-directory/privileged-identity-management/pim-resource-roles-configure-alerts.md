---
title: Azure kaynakları - güvenlik uyarılarını Privileged Identity Management | Microsoft Docs
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
ms.openlocfilehash: 86c9a0f12b2598ffbd02810a11622b13b0363a1f
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---alerts"></a>Privileged Identity Management - kaynak rolleri - uyarıları
Ortamınızda kuşkulu veya güvenli olmayan bir etkinlik olduğunda PIM Azure kaynakları için uyarılar oluşturur. Bir uyarı tetiklendiğinde, uyarılar sayfasında görüntülenir. 

![](media/azure-pim-resource-rbac/RBAC-alerts-home.png)

## <a name="review-alerts"></a>Uyarıları gözden geçirin
Kullanıcıların veya uyarının rollerin listeleyen bir rapor görmek için bir uyarı seçin düzeltme önerileri yanı sıra.
![](media/azure-pim-resource-rbac/rbac-alert-info.png)

## <a name="alerts"></a>Uyarılar
| Uyarı | Önem Derecesi | Tetikleyici | Öneri |
| --- | --- | --- | --- |
| **Bir kaynağa atanan çok fazla sahipleri** |Orta |Çok sayıda kullanıcı sahibi rolüne sahip. |Listedeki kullanıcılar gözden geçirin ve bazı daha az ayrıcalıklı rollere yeniden atayın. |
| **Bir kaynağa atanan çok fazla kalıcı sahipleri** |Orta |Çok sayıda kullanıcı, bir rol kalıcı olarak atanır. |Listedeki kullanıcılar gözden geçirin ve bazı rol kullanması için etkinleştirme için yeniden atayın. |
| **Oluşturulan yinelenen rol** |Orta |Birden çok rol aynı ölçütlerine sahiptirler. |Yalnızca bu rollerden birinin kullanın. |


### <a name="severity"></a>Önem Derecesi
* **Yüksek**: bir ilke ihlali nedeniyle Acil eylem gerektirir. 
* **Orta**: Acil eylem gerekli değildir ancak olası ilke ihlalinin işaret eder.
* **Düşük**: Acil eylem gerekli değildir ancak preferrable ilke değişikliğini önerir.

## <a name="configure-security-alert-settings"></a>Güvenlik uyarı ayarlarını yapılandırma
Uyarıları sayfasından ayarlarına gidin.
![](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Ortam ve Güvenlik amaçları ile çalışmak için farklı uyarıların ayarlarını özelleştirin.
![](media/azure-pim-resource-rbac/rbac-alert-settings.png)
