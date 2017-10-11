Bu bölümde, yeni bir öğe eklenen her zaman bir anında iletme bildirimi göndermek için var olan Mobile Apps arka uç projenizde kodunu güncelleştirin. Bu tarafından desteklenmektedir [şablonu](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) Azure Notification Hubs, platformlar arası iter etkinleştirme özelliğidir. Çeşitli istemcilere şablonları kullanarak anında iletme bildirimleri için kaydedilir ve tek bir evrensel itme tüm istemci platformları için elde edebilirsiniz.

Arka uç projesi türünüzle eşleşen aşağıdaki yordamlardan birini seçin&mdash;ya da [.NET geri son](#dotnet) veya [Node.js arka ucu](#nodejs).

### <a name="dotnet"></a>.NET arka uç projesi
1. Visual Studio'da sunucu projesi sağ tıklatın ve **NuGet paketlerini Yönet**. Arama `Microsoft.Azure.NotificationHubs`ve ardından **yükleme**. Bu arka ucunuzdan bildirim göndermek için Notification Hubs kitaplığı yükler.
2. Sunucu projeyi açın **denetleyicileri** > **TodoItemController.cs**ve aşağıdaki using deyimlerini:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. İçinde **PostTodoItem** yöntemini çağırdıktan sonra aşağıdaki kodu ekleyin **InsertAsync**:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
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

    Bu öğeyi içeren bir şablon bildirim gönderir. Yeni bir öğe eklendiğinde, metin.
4. Sunucu projesi yeniden yayımlayın.

### <a name="nodejs"></a>Node.js arka uç projesi
1. Bunu zaten bunu yapmadıysanız [hızlı başlangıç arka uç projesi indirme](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), ya da başka kullanım [Azure portalında çevrimiçi düzenleyicisini](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Todoitem.js var olan kodu aşağıdakilerle değiştirin:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
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
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    Bu, yeni bir öğe eklendiğinde item.text içeren bir şablon bildirim gönderir.
3. Yerel bilgisayarınızda dosyayı düzenlerken, sunucu projesi yeniden yayımlayın.
