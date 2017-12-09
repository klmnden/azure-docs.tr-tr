---
title: "Oran SMS, e-postalar ve Web kancalarını sınırlama | Microsoft Docs"
description: "Bir eylem grubundan olası SMS, e-posta veya Web kancası bildirim sayısı Azure nasıl sınırlar anlayın."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: bde645624ab1860d19ba18470f55845855a7d1fb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="rate-limiting-for-sms-messages-emails-and-webhook-posts"></a>SMS iletileri, e-postalar ve Web kancası için gönderileri sınırlama oranı
Hız sınırlaması belirli telefon numarası veya e-posta adresi çok fazla bildirimler gönderildiğinde oluşan bildirimleri ertelenmesi olur. Hız sınırlaması uyarıları yönetilebilir ve kullanılabilir olmasını sağlar.

SMS ve e-posta için kuralları aynıdır. Hızı sınırı eşik ise:

 - **SMS**: bir saat içinde 10 iletileri.
 - **E-posta**: 100 iletileri bir saat içinde.

## <a name="rate-limit-rules"></a>Hızı sınırı kuralları
- Belirli bir telefon numarası veya e-posta eşiği izin verdiğinden daha fazla ileti aldığında sınırlı hızıdır.
- Bir telefon numarası veya e-posta çok sayıda abonelikleri eylem gruplarının bir parçası olabilir. Hız sınırlaması abonelikler arasında geçerlidir. Eşiğe ulaşan hemen birden çok aboneliklerden gönderilen iletileri olsa bile geçerlidir.  
- Bir telefon numarası veya e-posta oranı sınırlı olduğunda, hız sınırlaması iletişim kurmak için başka bir bildirim gönderilir. Hız sınırlaması sona erdiğinde bildirim durumları.

## <a name="rate-limit-of-webhooks"></a>Web kancası hızı sınırı ##
Web kancası için yerinde sınırlama kuru yoktur.

## <a name="next-steps"></a>Sonraki adımlar ##
* Daha fazla bilgi edinmek [SMS uyarı davranış](monitoring-sms-alert-behavior.md).
* Alma bir [etkinlik günlüğü uyarıları genel bakış](monitoring-overview-alerts.md)ve uyarıların nasıl alınacağını öğrenin.  
* Bilgi edinmek için nasıl [hizmeti sistem durumu bildirimi gönderilen her uyarıları yapılandırmak](monitoring-activity-log-alerts-on-service-notifications.md).
