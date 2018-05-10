---
title: Zamanlanmış bildirimleri göndermek nasıl | Microsoft Docs
description: Bu konuda, Azure Notification Hubs ile zamanlanmış bildirimleri kullanmayı açıklar.
services: notification-hubs
documentationcenter: .net
keywords: anında iletme bildirimleri, anında iletme bildirimi, anında iletme bildirimleri planlama
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 0f4055a11d22604c0936685a7a2be3d56b259a5b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-send-scheduled-notifications"></a>Nasıl yapılır: Zamanlanmış bildirimleri gönderme
## <a name="overview"></a>Genel Bakış
Bir senaryo, belirli bir noktada gelecekte bir bildirim göndermek istiyor ancak uyandırmak için kolay bir yol arka uç kodunuzun bildirim göndermek için yoksa varsa. Standart katmanı bildirim hub'ları bildirimleri gelecekte yedi gün için zamanlamanıza olanak sağlayan bir özellik destekler.

Bir bildirim göndererek, yalnızca kullanın [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) aşağıdaki örnekte gösterildiği gibi bildirim hub'ları SDK'da sınıfı:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Ayrıca, kendi notificationId kullanarak daha önce zamanlanmış bir bildirim iptal edebilirsiniz:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Zamanlanmış bildirim gönderebilirsiniz sayısı sınırı vardır.

