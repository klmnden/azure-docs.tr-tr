---
title: Azure Active Directory raporlama gecikmeleri | Microsoft Docs
description: Raporlama olayları, Azure Portalı'nda görünmesi geçen süreyi hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 12/15/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: b81c66acc0a90ba9b74cf1f4fb34ef7a545837f9
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736615"
---
# <a name="azure-active-directory-reporting-latencies"></a>Azure Active Directory raporlama gecikmeleri

İle [raporlama](../active-directory-preview-explainer.md) Azure Active Directory ortamınızı nasıl çalıştığını belirlemek için gereken tüm bilgileri alın. Veri raporlama için Azure Portalı'nda görünmesi için gereken süreyi gecikme süresi de denir. 

Bu konu, Azure portalında tüm raporlama kategorileri gecikme bilgileri listeler. 


## <a name="activity-reports"></a>Etkinlik raporları

Etkinlik Raporlama iki alan vardır:

- **Oturum açma etkinlikleri**: Yönetilen uygulamaların kullanımı ve kullanıcıların oturum açma etkinlikleri hakkında bilgiler
- **Denetim günlükleri** - Kullanıcılar ve grup yönetimi, yönetilen uygulamalarınız ve dizin etkinlikleriniz hakkında sistem etkinliği bilgileri

Aşağıdaki tabloda, etkinlik raporları gecikme bilgileri listeler.

| Rapor | Gecikme süresi (%95) |Gecikme süresi (% 99)|
| :-- | --- | --- | 
| Denetim günlükleri | 2 dk.  | 5 dk.  |
| Oturum açma işlemleri | 2 dk.  | 5 dk. |


## <a name="security-reports"></a>Güvenlik raporları

Güvenlik raporlama iki alan vardır:

- **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. 
- **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. 

Aşağıdaki tabloda, güvenlik raporları gecikme bilgileri listeler.

| Rapor | Minimum | Ortalama | Maksimum |
| :-- | --- | --- | --- |
| Risk altındaki kullanıcılar          | 5 dakika   | 15 dakika  | 2 saat  |
| Riskli oturum açma işlemleri         | 5 dakika   | 15 dakika  | 2 saat  |

## <a name="risk-events"></a>Risk olayları

Azure Active Directory, kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılamak için Uyarlamalı makine öğrenimi algoritmaları ve buluşsal yöntemler kullanır. Her bir kayıt adı verilen risk olayı şüpheli eylem depolanır algılandı.

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

Azure portalında Etkinlik raporlarını hakkında daha fazla bilgi edinmek istiyorsanız, bkz:

- [Azure Active Directory portalındaki oturum açma etkinlik raporları](concept-sign-ins.md)
- [Azure Active Directory portalındaki denetim etkinlik raporları](concept-audit-logs.md)

Azure portalında güvenlik raporları hakkında daha fazla bilgi edinmek istiyorsanız, bkz:

- [Risk altındaki kullanıcılar güvenlik raporu Azure Active Directory portalındaki](concept-user-at-risk.md)
- [Azure Active Directory portalındaki riskli oturum açma işlemleri raporu](concept-risky-sign-ins.md)

Risk olayları hakkında daha fazla bilgi edinmek istiyorsanız bkz [Azure Active Directory risk olayları](concept-risk-events.md).
