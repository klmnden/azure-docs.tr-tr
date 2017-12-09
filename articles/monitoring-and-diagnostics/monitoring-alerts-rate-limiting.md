---
title: "Oran SMS, e-postalar, Azure uygulaması anında iletme bildirimleri ve Web kancası için sınırlama | Microsoft Docs"
description: "Azure olası SMS, e-posta, bir eylem grubundan Azure uygulaması anında iletme veya Web kancası bildirimler sayısını nasıl sınırlar anlayın."
author: dukek
manager: chrad
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/8/2017
ms.author: dukek
ms.openlocfilehash: c76bf5cf51f18a32b33060d528c64d119e31dbbd
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="rate-limiting-for-sms-messages-emails-azure-app-push-notifications-and-webhook-posts"></a>SMS iletileri, e-postalar, Azure uygulaması anında iletme bildirimleri ve Web kancası için gönderileri sınırlama oranı
Hız sınırlaması belirli telefon numarası, e-posta adresi veya aygıt çok fazla bildirimler gönderildiğinde oluşan bildirimleri ertelenmesi olur. Hız sınırlaması uyarıları yönetilebilir ve kullanılabilir olmasını sağlar.

Hızı sınırı eşikler şunlardır:

 - **SMS**: 1'den fazla SMS her 5 dakikada bir.
 - **E-posta**: 100 iletileri bir saat içinde.
 - **Azure uygulaması anında iletme bildirimleri**: anında iletme bildirimleri için sınırlama kuru yoktur.
 - **Web kancası**: Web kancası için sınırlama kuru yoktur.

## <a name="rate-limit-rules"></a>Hızı sınırı kuralları
- Belirli bir telefon numarası veya e-posta eşiği izin verdiğinden daha fazla ileti aldığında sınırlı hızıdır.
- Bir telefon numarası veya e-posta çok sayıda abonelikleri eylem gruplarının bir parçası olabilir. Hız sınırlaması abonelikler arasında geçerlidir. Eşiğe ulaşan hemen birden çok aboneliklerden gönderilen iletileri olsa bile geçerlidir.  
- Bir e-posta adresi oranı sınırlı olduğunda, hız sınırlaması iletişim kurmak için başka bir bildirim gönderilir. Hız sınırlaması sona erdiğinde bildirim durumları.

## <a name="next-steps"></a>Sonraki adımlar ##
* Daha fazla bilgi edinmek [SMS uyarı davranış](monitoring-sms-alert-behavior.md).
* Alma bir [etkinlik günlüğü uyarıları genel bakış](monitoring-overview-alerts.md)ve uyarıların nasıl alınacağını öğrenin.  
* Bilgi edinmek için nasıl [hizmeti sistem durumu bildirimi gönderilen her uyarıları yapılandırmak](monitoring-activity-log-alerts-on-service-notifications.md).
