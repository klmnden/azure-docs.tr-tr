Arka uç projesi türünüzle eşleşen yordamı&mdash;ya da [.NET son geri](#dotnet) veya [Node.js arka ucu](#nodejs).

### <a name="dotnet"></a>.NET arka uç projesi
1. Visual Studio'da sunucu projesi sağ tıklayın ve **NuGet paketlerini Yönet**. Arama `Microsoft.Azure.NotificationHubs`ve ardından **yükleme**. Bu, bildirim hub'ları istemci kitaplığı yükler.
2. Denetleyicileri klasöründe TodoItemController.cs açın ve aşağıdakileri ekleyin `using` deyimleri:

        using Microsoft.Azure.Mobile.Server.Config;
        using Microsoft.Azure.NotificationHubs;
3. Değiştir `PostTodoItem` aşağıdaki kod ile yöntemi:  

        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
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

            // Android payload
            var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";

            try
            {
                // Send the push notification and log the results.
                var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);

                // Write the success result to the logs.
                config.Services.GetTraceWriter().Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                // Write the failure result to the logs.
                config.Services.GetTraceWriter()
                    .Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

4. Sunucu projesi yeniden yayımlayın.

### <a name="nodejs"></a>Node.js arka uç projesi
1. Bunu zaten bunu yapmadıysanız [hızlı başlangıç projesi indirme](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), ya da başka kullanım [Azure portalında çevrimiçi düzenleyicisini](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Todoitem.js dosyasındaki var olan kodu aşağıdakilerle değiştirin:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the GCM payload.
        var payload = {
            "data": {
                "message": context.item.text
            }
        };   

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a GCM native notification.
                    context.push.gcm.send(null, payload, function (error) {
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

    Bu, yeni bir Yapılacaklar öğesi eklendiğinde item.text içeren bir GCM bildirim gönderir.
3. Yerel bilgisayarınızda dosyayı düzenlerken, sunucu projesi yeniden yayımlayın.
