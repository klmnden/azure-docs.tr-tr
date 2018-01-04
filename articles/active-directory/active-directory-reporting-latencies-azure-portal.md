---
title: Gecikme raporlama Azure Active Directory | Microsoft Docs
description: "Azure portalında göstermeyi raporlama olayları için geçen süreyi hakkında bilgi edinin"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/11/2017
ms.author: markvi;dhanyahk
ms.reviewer: dhanyahk
<<<<<<< HEAD
ms.openlocfilehash: 44e31d30cf5f6d6ca216fb7ed9f6be6e38cd8697
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
=======
ms.openlocfilehash: fa9ffa8f5380659674301f7e738879c8efb25b7f
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="azure-active-directory-reporting-latencies"></a>Gecikme raporlama Azure Active Directory

İle [raporlama](active-directory-preview-explainer.md) Azure Active Directory ortamınızı nasıl çalıştığını belirlemek için gereken tüm bilgileri alın. Veri raporlama için Azure Portalı'nda gösterilmesi için gereken süre miktarını gecikme süresi de denir. 

Bu konu Azure Portalı'ndaki tüm raporlama kategorileri gecikme bilgileri listeler. 


## <a name="activity-reports"></a>Etkinlik raporları

Etkinlik Raporlama iki alan vardır:

- **Oturum açma etkinlikleri**: Yönetilen uygulamaların kullanımı ve kullanıcıların oturum açma etkinlikleri hakkında bilgiler
- **Denetim günlükleri** - Kullanıcılar ve grup yönetimi, yönetilen uygulamalarınız ve dizin etkinlikleriniz hakkında sistem etkinliği bilgileri

Aşağıdaki tabloda, etkinlik raporları gecikme bilgileri listeler.

| Rapor | Minimum | Ortalama |
| :-- | --- | --- |
| Denetim günlükleri | 30 dakika  | 1 saat  |
| Oturum açma işlemleri | 15 dakika  | 2 saat |

Bazı kenar durumlarda bunu gerçekleştirebilirsiniz:

- 2 saat gösterilmeye etkinlik verileri denetim.
- 24 saat oturum açma etkinliği veri görünmesini sağlar. Bu, eski office uygulamalarından gelen oturum açma işlemleri etkinlik verilerini içerir. 


## <a name="security-reports"></a>Güvenlik raporları

Raporlama güvenliği, iki alan vardır:

- **Riskli oturum açma işlemleri** - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir. 
- **Riskli oldukları belirlenen kullanıcılar** - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir. 

Aşağıdaki tabloda güvenlik raporları gecikme bilgileri listeler.

| Rapor | Minimum | Ortalama | Maksimum |
| :-- | --- | --- | --- |
| Risk altındaki kullanıcılar          | 5 dakika   | 15 dakika  | 2 saat  |
| Riskli oturum açma işlemleri         | 5 dakika   | 15 dakika  | 2 saat  |

## <a name="risk-events"></a>Risk olayları

Azure Active Directory kullanıcı hesaplarınızı ilgili kuşkulu eylemleri algılamak için Uyarlamalı machine learning algoritmaları ve buluşsal yöntemler kullanır. Her kuşkulu eylem bir kayıt çağrılan risk olayı depolanan algıladı.

Aşağıdaki tabloda, risk olaylarını gecikme bilgileri listeler.

| Rapor | Minimum | Ortalama | Maksimum |
| :-- | --- | --- | --- |
| Anonim IP adreslerinden oturum açma işlemleri |5 dakika |15 dakika |2 saat |
| Alışılmadık konumlardan oturum açma işlemleri |5 dakika |15 dakika |2 saat |
| Sızan kimlik bilgilerine sahip kullanıcılar |2 saat |4 saat |8 saat |
| Alışılmadık konumlara imkansız seyahat |5 dakika |1 saat |8 saat  |
| Bulaşma olan cihazlardan oturum açma işlemleri |2 saat |4 saat |8 saat  |
| Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri |2 saat |4 saat |8 saat  |



## <a name="next-steps"></a>Sonraki adımlar

Azure portalında etkinlik raporları hakkında daha fazla bilgi edinmek istiyorsanız, bkz:

- [Azure Active Directory portalında oturum açma etkinliği raporları](active-directory-reporting-activity-sign-ins.md)
- [Azure Active Directory portalında denetim etkinlik raporları](active-directory-reporting-activity-audit-logs.md)

Azure portalında güvenlik raporları hakkında daha fazla bilgi edinmek istiyorsanız, bkz:

- [Azure Active Directory portalında risk güvenlik raporu kullanıcılar](active-directory-reporting-security-user-at-risk.md)
- [Azure Active Directory portalında riskli oturum açma işlemleri raporu](active-directory-reporting-security-risky-sign-ins.md)

Risk olaylar hakkında daha fazla bilgi edinmek istiyorsanız, bkz: [Azure Active Directory risk olaylarını](active-directory-reporting-risk-events.md).
