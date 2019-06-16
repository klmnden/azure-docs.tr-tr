---
title: Planlanmış bildirimler gönderme | Microsoft Docs
description: Bu konuda, Azure Notification Hubs ile zamanlanmış bildirimler kullanmayı açıklar.
services: notification-hubs
documentationcenter: .net
keywords: anında iletme bildirimleri, anında iletme bildirimi, anında iletme bildirimleri zamanlama
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: af0de9e8c18644f4ae200f6546c0dd0a41320f9f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61457694"
---
# <a name="how-to-send-scheduled-notifications"></a>Nasıl Yapılır: Planlanmış bildirimler gönderme

Bir senaryo, gelecekte bir noktada bir bildirim göndermesini istiyor ancak arka uç kodunuzu bildirim göndermek için uyandırmak için kolay bir yolu olmayan varsa. Standart katmandaki bildirim hub'ları bildirimleri gelecekte yedi gün zamanlayın olanak tanıyan bir özelliği destekler.


## <a name="schedule-your-notifications"></a>Bildirimlerinizi zamanlama
Bildirim gönderilirken kullanmanız yeterlidir [ `ScheduledNotification` sınıfı](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) bildirim hub'ları aşağıdaki örnekte gösterildiği gibi SDK:

```c#
Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));
```

## <a name="cancel-scheduled-notifications"></a>Planlanmış bildirimler iptal et
Ayrıca, kendi notificationId kullanarak önceden zamanlanmış bir bildirim iptal edebilirsiniz:

```c#
await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);
```

Planlanmış bildirimler göndermek için sayısı sınırı yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki öğreticilere bakın:

 - [Tüm kayıtlı cihazlara anında iletme bildirimleri gönderme](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
 - [Belirli cihazlara anında iletme bildirimleri gönderme](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md)
 - [Yerelleştirilmiş anında iletme bildirimleri gönderme](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
 - [Belirli kullanıcılara anında iletme bildirimleri gönderme](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) 
 - [Konum temelli anında iletme bildirimleri gönderme](notification-hubs-push-bing-spatial-data-geofencing-notification.md)
