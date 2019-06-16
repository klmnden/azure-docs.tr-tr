---
title: include dosyası
description: include dosyası
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/30/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: f0ff729084d194ff2e05e89eadc45782f775b1c5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66156732"
---
Bu bölümde, son dakika haberlerini .NET konsol uygulamasından etiketli şablon bildirimleri olarak yollarsınız. 

1. Visual Studio'da yeni bir görsel oluşturun C# konsol uygulaması: bir. Menüsünde **dosya** > **yeni** > **proje**.
    b. Genişletin **Visual C#** seçip **Windows Masaüstü**. 
    c. Seçin **konsol uygulaması (.NET Framework)** şablonları listesinde. 
    d. Girin bir **adı** uygulama için. 
    e. Seçin bir **klasör** uygulama için.
    f. Seçin **Tamam** projeyi oluşturmak için. 
2. Visual Studio ana menüden **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu** ve ardından konsol penceresinde aşağıdaki dizeyi girin:
   
    ```
    Install-Package Microsoft.Azure.NotificationHubs
    ```
   
3. **Enter** tuşunu seçin.  
    Bu eylem [Microsoft.Azure.Notification Hubs NuGet paketi] kullanarak Azure Notification Hubs SDK'sına bir başvuru ekler.

4. Program.cs dosyasını açın ve aşağıdaki `using` deyimini ekleyin:
   
    ```csharp
    using Microsoft.Azure.NotificationHubs;
    ```

5. `Program` sınıfında, aşağıdaki yöntemi ekleyin veya zaten mevcutsa değiştirin:
   
    ```csharp
    private static async void SendTemplateNotificationAsync()
    {
        // Define the notification hub.
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");

        // Create an array of breaking news categories.
        var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};

        // Send the notification as a template notification. All template registrations that contain
        // "messageParam" and the proper tags will receive the notifications.
        // This includes APNS, GCM, WNS, and MPNS template registrations.

        Dictionary<string, string> templateParams = new Dictionary<string, string>();

        foreach (var category in categories)
        {
            templateParams["messageParam"] = "Breaking " + category + " News!";
            await hub.SendTemplateNotificationAsync(templateParams, category);
        }
    }
    ```   
   
    Bu kod, dize dizisindeki altı etiketin her biri için bir şablon bildirimi gönderir. Etiketlerin kullanılması, cihazların yalnızca kayıtlı kategoriler için bildirim almasını sağlar.

5. Önceki kodda, `<hub name>` ve `<connection string with full access>` yer tutucularını bildirim hub'ı adınız ve bildirim hub’ınızın panosundaki *DefaultFullSharedAccessSignature* bağlantı dizeniz ile değiştirin.

6. **Ana** yöntemine aşağıdaki satırları ekleyin:
   
    ```csharp
    SendTemplateNotificationAsync();
    Console.ReadLine();
    ```

7. Konsol uygulamasını oluşturun.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Get started with Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Notification Hubs REST interface]: https://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[Add push notifications for Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[How to use Notification Hubs from Java or PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[Microsoft.Azure.Notification Hubs NuGet paketi]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
