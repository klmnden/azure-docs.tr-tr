---
title: "Eylem grupları SMS uyarı davranışını | Microsoft Docs"
description: "SMS ileti biçimi ve SMS iletileri aboneliğinizi iptal etmek için yanıt resubscribe veya Yardım isteyin."
author: dkamstra
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
ms.date: 02/16/2018
ms.author: dukek
ms.openlocfilehash: ce6908de0f6bcc30d1ee846fe92171a0cb589cbb
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
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
