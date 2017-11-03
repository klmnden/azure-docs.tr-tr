---
title: "Zamanlanmış bildirimleri göndermek nasıl | Microsoft Docs"
description: "Bu konuda, Azure Notification Hubs ile zamanlanmış bildirimleri kullanmayı açıklar."
services: notification-hubs
documentationcenter: .net
keywords: "anında iletme bildirimleri, anında iletme bildirimi, anında iletme bildirimleri planlama"
author: ysxu
manager: erikre
editor: 
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: efac6e1ecc00359f1622d380333140bc055c83e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-send-scheduled-notifications"></a>Nasıl yapılır: Zamanlanmış bildirimleri gönderme
## <a name="overview"></a>Genel Bakış
Bir senaryo, belirli bir noktada gelecekte bir bildirim göndermek istiyor ancak uyandırmak için kolay bir yol arka uç kodunuzun bildirim göndermek için yoksa varsa. Standart katmanı bildirim hub'ları bildirimleri gelecekte 7 gün için zamanlamanıza olanak sağlayan bir özellik destekler.

Bir bildirim göndererek, yalnızca kullanın [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) aşağıdaki örnekte gösterildiği gibi bildirim hub'ları SDK'da sınıfı:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Ayrıca, kendi notificationId kullanarak daha önce zamanlanmış bir bildirim iptal edebilirsiniz:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Zamanlanmış bildirim gönderebilirsiniz sayısı sınırı vardır.

