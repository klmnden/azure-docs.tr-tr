---
title: Eylem grupları SMS uyarı davranışı
description: SMS ileti biçimi ve SMS iletileri aboneliğinizi iptal etmek için yanıt resubscribe veya Yardım isteyin.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: f2f463f6c428ce6c72e2640472376fa17a2bfe5a
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263016"
---
# <a name="sms-alert-behavior-in-action-groups"></a>SMS eylemi gruplarındaki davranışı uyar
## <a name="overview"></a>Genel Bakış ##
Eylem grupları eylemlerin bir listesini yapılandırmanızı sağlar. Bu gruplar, uyarıları tanımlarken kullanılır; uyarı tetiklendiğinde, belirli bir eylem grubu bildirim sağlama. Desteklenen eylemler SMS biridir; SMS bildirim çift yönlü iletişimi destekler. Bir kullanıcı bir SMS yanıt verebilir:

- **Uyarılardan aboneliği:** bir kullanıcı tüm Eylem grupları ya da tek eylem grubu için tüm SMS uyarıları aboneliğinizi.
- **Uyarılar resubscribe:** bir kullanıcı için tüm Eylem grupları ya da tek eylem grubu için tüm SMS uyarıları resubscribe.  
- **Yardım isteğinde:** bir kullanıcı SMS hakkında daha fazla bilgi isteyebilir. Bunlar, bu makalede yönlendirilir.

Bu makalede, SMS uyarıları davranışını yer almaktadır ve yanıt kullanıcıya kullanıcının bölgesel ayarına göre eylemleri gerçekleştirebilirsiniz:

## <a name="receiving-an-sms-alert"></a>SMS uyarı alma
Bir uyarı tetiklendiğinde bir eylem grubunun bir parçası olarak yapılandırılmış bir SMS alıcı SMS alır. SMS aşağıdaki bilgileri içerir:
* Bu uyarı gönderildiği eylem grubunun kısaad
- Uyarı başlığı

| YANITLA | Açıklama |
| ----- | ----------- |
| DEVRE DIŞI BIRAK <Action Group Short name> | Eylem grubundan başka SMS devre dışı bırakır |
| ETKİNLEŞTİR <Action Group Short name> | SMS eylemi grubundan yeniden etkinleştirir |
| DURDUR | Tüm eylem gruplarından başka SMS devre dışı bırakır |
| BAŞLAT | Tüm eylem gruplarından SMS yeniden etkinleştirir |
| YARDIM | Yanıt, bu makalede bir bağlantıyla kullanıcıya gönderilir. |

>[!NOTE]
>Bir kullanıcı iptal etti SMS uyarır, ancak daha sonra yeni bir eylem grubuna eklenir; Bunlar yeni o eylemi grubu SMS uyarı almak, ancak tüm önceki eylem gruplarından aboneliği kalır.

## <a name="next-steps"></a>Sonraki Adımlar
Alma bir [etkinlik günlüğü uyarıları genel bakış](monitoring-overview-alerts.md) ve uyarı öğrenin  
Daha fazla bilgi edinmek [SMS hız sınırı](monitoring-alerts-rate-limiting.md)  
Daha fazla bilgi edinmek [Eylem grupları](monitoring-action-groups.md)
