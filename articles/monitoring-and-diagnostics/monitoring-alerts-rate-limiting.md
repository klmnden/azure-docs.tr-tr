---
title: SMS, e-postalar, Azure uygulaması anında iletme bildirimleri ve Web kancası için sınırlama oranı
description: Azure olası SMS, e-posta, bir eylem grubundan Azure uygulaması anında iletme veya Web kancası bildirimler sayısını nasıl sınırlar anlayın.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 3/12/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 1acea47638c75971bad62f28dcd7e96823d84c1d
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35262965"
---
# <a name="rate-limiting-for-voice-sms-emails-azure-app-push-notifications-and-webhook-posts"></a>Ses, SMS, e-postalar, Azure uygulaması anında iletme bildirimleri ve Web kancası için gönderileri sınırlama oranı
Hız sınırlaması çok fazla belirli telefon numarası, e-posta adresi veya aygıt gönderildiğinde oluşan bildirimleri ertelenmesi olur. Hız sınırlaması uyarıları yönetilebilir ve kullanılabilir olmasını sağlar.

Hızı sınırı eşikler şunlardır:

 - **SMS**: 1'den fazla SMS her 5 dakikada bir.
 - **Sesli**: 1'den fazla sesli arama her 5 dakikada bir.
 - **E-posta**: 100'den fazla e-postaların bir saat içinde.
 
 Diğer Eylemler oranı sınırlı değildir.

## <a name="rate-limit-rules"></a>Hızı sınırı kuralları
- Belirli bir telefon numarası veya e-posta eşiği izin verdiğinden daha fazla ileti aldığında sınırlı hızıdır.
- Bir telefon numarası veya e-posta çok sayıda abonelikleri eylem gruplarının bir parçası olabilir. Hız sınırlaması abonelikler arasında geçerlidir. Eşiğe ulaşan hemen birden çok aboneliklerden gönderilen iletileri olsa bile geçerlidir.
- Bir e-posta adresi oranı sınırlı olduğunda, hız sınırlaması iletişim kurmak için başka bir bildirim gönderilir. E-posta, hız sınırlaması sona erdiğinde durumları.

## <a name="next-steps"></a>Sonraki adımlar ##
* Daha fazla bilgi edinmek [SMS uyarı davranış](monitoring-sms-alert-behavior.md).
* Alma bir [etkinlik günlüğü uyarıları genel bakış](monitoring-overview-alerts.md)ve uyarıların nasıl alınacağını öğrenin.  
* Bilgi edinmek için nasıl [hizmeti sistem durumu bildirimi gönderilen her uyarıları yapılandırmak](monitoring-activity-log-alerts-on-service-notifications.md).
