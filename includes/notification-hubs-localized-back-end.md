---
title: include dosyası
description: include dosyası
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 04/02/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 752feca30fdca663aaf8bd88e6686781b9065682
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33836690"
---
Şablon bildirimleri gönderdiğinizde, yalnızca bir özellikler kümesi sağlamanız gerekir. Bu senaryoda, özellikler kümesini güncel haberleri yerelleştirilmiş sürümünü içerir.

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }



### <a name="send-notifications-using-a-c-console-app"></a>Bir C# konsol uygulaması kullanarak bildirim gönderme
Bu bölümde, bir konsol uygulaması kullanarak bildirim göndermek gösterilmiştir. Kod Windows mağazası ve iOS cihazlar için bildirimler yayınlar. Değiştirme `SendTemplateNotificationAsync` aşağıdaki kod ile daha önce oluşturduğunuz konsol uygulaması yöntemi:

```csharp
private static async void SendTemplateNotificationAsync()
{
    // Define the notification hub.
    NotificationHubClient hub = 
        NotificationHubClient.CreateClientFromConnectionString(
            "<connection string with full access>", "<hub name>");

    // Sending the notification as a template notification. All template registrations that contain 
    // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications. 
    // This includes APNS, GCM, WNS, and MPNS template registrations.
    Dictionary<string, string> templateParams = new Dictionary<string, string>();

    // Create an array of breaking news categories.
    var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
    var locales = new string[] { "English", "French", "Mandarin" };

    foreach (var category in categories)
    {
        templateParams["messageParam"] = "Breaking " + category + " News!";

        // Sending localized News for each tag too...
        foreach( var locale in locales)
        {
            string key = "News_" + locale;

            // Your real localized news content would go here.
            templateParams[key] = "Breaking " + category + " News in " + locale + "!";
        }

        await hub.SendTemplateNotificationAsync(templateParams, category);
    }
}
```

Haberleri ile yerelleştirilmiş parçası SendTemplateNotificationAsync yöntemi sunar **tüm** platform yedeklemiş aygıtlarınızı. Bildirim hub'ınızı oluşturur ve belirli bir tag abone olan tüm cihazlar için doğru yerel yükü sunar.

### <a name="sending-notification-with-mobile-services"></a>Mobile Services bildirim gönderme
Mobile Services Zamanlayıcısı'nda aşağıdaki komut dosyasını kullanın:

```csharp
var azure = require('azure');
var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
var notification = {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin", "World News in Mandarin!"
}
notificationHubService.send('World', notification, function(error) {
    if (!error) {
        console.warn("Notification successful");
    }
});
```

