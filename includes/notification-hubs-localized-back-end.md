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
ms.openlocfilehash: c15d695e072e72c6e7be6dcf49f3ea049a9b70b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66152399"
---
Şablon bildirim gönderdiğinizde, yalnızca bir özellikler kümesini sağlamanız gerekir. Bu senaryoda, özellikler kümesini güncel haberleri yerelleştirilmiş sürümünü içerir.

```json
{
    "News_English": "World News in English!",
    "News_French": "World News in French!",
    "News_Mandarin": "World News in Mandarin!"
}
```

### <a name="send-notifications-using-a-c-console-app"></a>Bir C# konsol uygulaması kullanarak bildirim gönderme

Bu bölümde, bir konsol uygulaması kullanarak bildirim gönderme işlemi gösterilmektedir. Kod, hem Windows Store hem de iOS cihazlarına bildirimleri göndermek için yayınlar. Daha önce oluşturduğunuz konsol uygulamasındaki `SendTemplateNotificationAsync` yöntemini aşağıdaki kodla değiştirin:

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

Haberler için yerelleştirilmiş parçasını SendTemplateNotificationAsync yöntemi sunar **tüm** platform fark etmeksizin, cihazlarınızın. Bildirim hub'ınıza oluşturur ve doğru yerel yük belirli bir etikete abone olan tüm cihazlara teslim eder.

### <a name="sending-notification-with-mobile-services"></a>Mobil hizmetler ile bildirim göndererek

Mobil hizmetler Zamanlayıcısı'nda aşağıdaki betiği kullanın:

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
