---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: c664b089f316255fabc4c8dc36b291d7d63e6280
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891118"
---
Bu bölümde, yeni bir öğe eklendiğinde bir anında iletme bildirimi göndermek için mevcut Mobile Apps arka uç projesi içinde kod güncelleştirin. Bu işlem tarafından desteklenen [şablon](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) platformlar arası sağlayan Azure Notification Hubs'ın özellik gönderir. Çeşitli istemcilere şablonlarını kullanarak anında iletme bildirimleri için kaydedilir ve tüm istemci platformları için tek bir evrensel anında iletme alabilirsiniz.

Arka uç proje türünüzü eşleşen aşağıdaki yordamlardan birini&mdash;ya da [.NET arka ucu](#dotnet) veya [Node.js arka ucu](#nodejs).

### <a name="dotnet"></a>.NET arka uç projesi

1. Visual Studio'da sunucu projeye sağ tıklayın. Ardından **NuGet paketlerini Yönet**. Arama `Microsoft.Azure.NotificationHubs`ve ardından **yükleme**. Bu işlem arka ucundan bildirim göndermek için Notification Hubs kitaplığına yükler.
2. Sunucu projeyi **denetleyicileri** > **TodoItemController.cs**. Ardından aşağıdakileri ekleyin using deyimlerini:

    ```csharp
    using System.Collections.Generic;
    using Microsoft.Azure.NotificationHubs;
    using Microsoft.Azure.Mobile.Server.Config;
    ```

3. İçinde **PostTodoItem** yöntem çağrısından sonra aşağıdaki kodu ekleyin **InsertAsync**:  

    ```csharp
    // Get the settings for the server project.
    HttpConfiguration config = this.Configuration;
    MobileAppSettingsDictionary settings =
        this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

    // Get the Notification Hubs credentials for the mobile app.
    string notificationHubName = settings.NotificationHubName;
    string notificationHubConnection = settings
        .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

    // Create a new Notification Hub client.
    NotificationHubClient hub = NotificationHubClient
    .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

    // Send the message so that all template registrations that contain "messageParam"
    // receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
    Dictionary<string,string> templateParams = new Dictionary<string,string>();
    templateParams["messageParam"] = item.Text + " was added to the list.";

    try
    {
        // Send the push notification and log the results.
        var result = await hub.SendTemplateNotificationAsync(templateParams);

        // Write the success result to the logs.
        config.Services.GetTraceWriter().Info(result.State.ToString());
    }
    catch (System.Exception ex)
    {
        // Write the failure result to the logs.
        config.Services.GetTraceWriter()
            .Error(ex.Message, null, "Push.SendAsync Error");
    }
    ```

    Bu işlem öğeyi içeren bir şablon bildirim gönderir. Yeni bir öğe eklendiğinde, metin.

4. Sunucu projesini yeniden yayımlayın.

### <a name="nodejs"></a>Node.js arka uç projesi

1. Bunu zaten bunu yapmadıysanız [hızlı başlangıç arka uç projesi indirme](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), veya başka kullanım [Azure portalında çevrimiçi düzenleyicisini](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Todoitem.js mevcut kodu şu kodla değiştirin:

    ```javascript
    var azureMobileApps = require('azure-mobile-apps'),
    promises = require('azure-mobile-apps/src/utilities/promises'),
    logger = require('azure-mobile-apps/src/logger');

    var table = azureMobileApps.table();

    table.insert(function (context) {
    // For more information about the Notification Hubs JavaScript SDK,
    // see https://aka.ms/nodejshubs.
    logger.info('Running TodoItem.insert');

    // Define the template payload.
    var payload = '{"messageParam": "' + context.item.text + '" }';  

    // Execute the insert. The insert returns the results as a promise.
    // Do the push as a post-execute action within the promise flow.
    return context.execute()
        .then(function (results) {
            // Only do the push if configured.
            if (context.push) {
                // Send a template notification.
                context.push.send(null, payload, function (error) {
                    if (error) {
                        logger.error('Error while sending push notification: ', error);
                    } else {
                        logger.info('Push notification sent successfully!');
                    }
                });
            }
            // Don't forget to return the results from the context.execute().
            return results;
        })
        .catch(function (error) {
            logger.error('Error while running context.execute: ', error);
        });
    });

    module.exports = table;  
    ```

    Bu işlem, yeni bir öğe eklendiğinde item.text içeren bir şablon bildirim gönderir.

3. Yerel bilgisayarınızda bir dosyayı düzenlerken, sunucu projesi yeniden yayımlayın.
