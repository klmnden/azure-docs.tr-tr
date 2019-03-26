---
title: Azure Active Directory raporlama gecikmeleri | Microsoft Docs
description: Raporlama olayları, Azure Portalı'nda görünmesi geçen süreyi hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64bd2247a3437a2cc960da1820d9be417eedff8e
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58438658"
---
# <a name="azure-active-directory-reporting-latencies"></a>Azure Active Directory raporlama gecikmeleri

Gecikme süresi, Azure Active raporlama verilerini Directory (Azure AD) görünmesini süresini miktarıdır [Azure portalında](https://portal.azure.com). Bu makale, beklenen gecikme sürelerini farklı rapor türlerini listeler. 

## <a name="activity-reports"></a>Etkinlik raporları

İki tür Etkinlik Raporu vardır:

- [Oturum açma işlemleri](concept-sign-ins.md) – kullanımı hakkında bilgi yönetilen uygulamalar ve kullanıcı oturum açma etkinlikleri sağlar.
- [Denetim günlükleri](concept-audit-logs.md) -kullanıcılar ve grupları, yönetilen uygulamalar ve dizin etkinlikleriniz hakkında sistem etkinliği bilgileri sağlar

Aşağıdaki tabloda, etkinlik raporları gecikme bilgileri listeler. 

> [!NOTE]
> **Gecikme süresi (yüzde 95'lik dilim)** olarak %95 günlükler bildirilir, süresi başvurur ve **gecikme (99. yüzdebirlik dilimde)** olarak %99 günlükler bildirilir süresini gösterir. 
>

| Rapor | Gecikme süresi (yüzde 95'lik dilim) |Gecikme (99. yüzdebirlik dilimde)|Zaman aralığı içinde günlükleri raporlanır.|
| :-- | --- | --- | --- |
| Denetim günlükleri | 2 dk.  | 5 dk.  | 2-60 dakika |
| Oturum açma işlemleri | 2 dk.  | 5 dk. | 2-120 dakika |

### <a name="how-soon-can-i-see-activities-data-after-getting-a-premium-license"></a>Etkinlikler verileri bir premium lisansı aldıktan sonra en kısa sürede nasıl görebilirim?

Etkinlikleri veriler, ücretsiz lisansa zaten varsa, daha sonra bunu hemen yükseltmeyi görebilirsiniz. Herhangi bir veri yoksa, raporlarda görünmesi için bir premium lisansı yükselttikten sonra verilerin bir veya iki gün sürebilir.

## <a name="security-reports"></a>Güvenlik raporları

İki tür güvenlik raporu vardır:

- [Riskli oturum açma işlemleri](concept-risky-sign-ins.md) - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. 
- [Riskli oldukları belirlenen kullanıcılar](concept-user-at-risk.md) - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. 

Aşağıdaki tabloda, güvenlik raporları gecikme bilgileri listeler.

| Rapor | Minimum | Ortalama | Maksimum |
| :-- | --- | --- | --- |
| Risk altındaki kullanıcılar          | 5 dakika   | 15 dakika  | 2 saat  |
| Riskli oturum açma işlemleri         | 5 dakika   | 15 dakika  | 2 saat  |

## <a name="risk-events"></a>Risk olayları

Azure AD, kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılamak için Uyarlamalı makine öğrenimi algoritmaları ve buluşsal yöntemler kullanır. Her kuşkulu eylem adlı bir kayıt depolanır algılanan bir **risk olayı**.

Aşağıdaki tabloda, risk olayları gecikme bilgileri listeler.

| Rapor | Minimum | Ortalama | Maksimum |
| :-- | --- | --- | --- |
| Anonim IP adreslerinden oturum açma işlemleri |5 dakika |15 dakika |2 saat |
| Alışılmadık konumlardan oturum açma işlemleri |5 dakika |15 dakika |2 saat |
| Sızan kimlik bilgilerine sahip kullanıcılar |2 saat |4 saat |8 saat |
| Alışılmadık konumlara imkansız seyahat |5 dakika |1 saat |8 saat  |
| Bulaşma olan cihazlardan oturum açma işlemleri |2 saat |4 saat |8 saat  |
| Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri |2 saat |4 saat |8 saat  |


## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD raporlarına genel bakış](overview-reports.md)
* [Azure AD raporlar programlı erişim](concept-reporting-api.md)
* [Azure Active Directory risk olayları](concept-risk-events.md)
