---
title: Eylem grupları SMS uyarısı davranışı
description: SMS ileti biçimi ve iptal etme, SMS mesajları yanıt yeniden abone olmak veya Yardım isteyin.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 74666149824627308b6c5b026e0c9ba7a7750ada
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59523622"
---
# <a name="sms-alert-behavior-in-action-groups"></a>SMS uyarı Eylem grupları davranışı
## <a name="overview"></a>Genel Bakış ##
Eylem grupları eylemlerin bir listesini yapılandırmak etkinleştirin. Bu gruplar, uyarılar tanımlarken kullanılır; uyarı tetiklendiğinde, belirli bir eylem grubu haberdar olma. Desteklenen eylemlerin SMS biridir; SMS bildirimleri çift yönlü iletişimi destekler. Bir kullanıcı için bir SMS yanıt:

- **Uyarılardan Aboneliğimi:** Bir kullanıcının tüm Eylem grupları veya bir tek bir eylem grubu için tüm SMS Uyarılardan aboneliği.
- **Uyarı yeniden abone olmak:** Bir kullanıcı için tüm Eylem grupları veya tek bir eylem grubu için tüm SMS uyarıları yeniden abone olmak.  
- **Yardım iste:** Bir kullanıcı SMS hakkında daha fazla bilgi için isteyebilir. Bunlar, bu makalede yönlendirilir.

Bu makalede, SMS uyarısı davranışı yer almaktadır ve yanıt kullanıcının kullanıcı yerel ayarı üzerinde temel eylemleri gerçekleştirebilirsiniz:

## <a name="receiving-an-sms-alert"></a>SMS uyarı alma
Bir uyarı tetiklendiğinde bir eylem grubu bir parçası olarak yapılandırılmış bir SMS alıcı SMS alır. SMS, aşağıdaki bilgileri içerir:
* Bu uyarının gönderildiği Shortname eylem grubu
* Uyarı başlığı

| YANITLA | Açıklama |
| ----- | ----------- |
| DEVRE DIŞI BIRAK `<Action Group Short name>` | Eylem grubu başka SMS devre dışı bırakır |
| ETKİNLEŞTİRME `<Action Group Short name>` | SMS eylemi grubundan yeniden etkinleştirir |
| DURDUR | Tüm eylem gruplarından ek SMS devre dışı bırakır |
| BAŞLANGIÇ | Tüm eylem gruplarından SMS yeniden etkinleştirir |
| YARDIM | Yanıt, bu makalede bir bağlantı ile kullanıcıya gönderilir. |

>[!NOTE]
>Gelen bir kullanıcı iptal etti, SMS uyarır, ancak daha sonra yeni bir eylem grubuna eklenir; Bunlar, yeni eylem grubu SMS uyarıları almak, ancak önceki tüm eylem gruplarından aboneliği kalır.

## <a name="next-steps"></a>Sonraki Adımlar
Alma bir [etkinlik günlüğü uyarılarına genel bakış](alerts-overview.md) ve uyarı alma hakkında bilgi edinin  
Daha fazla bilgi edinin [SMS oran sınırlandırma](alerts-rate-limiting.md)  
Daha fazla bilgi edinin [Eylem grupları](../../azure-monitor/platform/action-groups.md)

