---
title: "Evrensel Windows Platformu (UWP) uygulamanıza anında iletme bildirimleri ekleme | Microsoft Docs"
description: "Evrensel Windows Platformu (UWP) uygulamanıza anında iletme bildirimleri göndermek için Azure App Service Mobile Apps ve Azure bildirim hub'ları kullanmayı öğrenin."
services: app-service\mobile,notification-hubs
documentationcenter: windows
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: 6de1b9d4-bd28-43e4-8db4-94cd3b187aa3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: a14bb0320c1f6a563f766a6a0fad5cf556fe7b70
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-push-notifications-to-your-windows-app"></a>Windows uygulamanıza anında iletme bildirimleri ekleme
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, anında iletme bildirimleri ekleme [Windows Hızlı Başlangıç](app-service-mobile-windows-store-dotnet-get-started.md) proje böylece bir kayda eklenen her zaman bir anında iletme bildirimi cihaza gönderilir.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantısı paketi gerekir. Bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) daha fazla bilgi için.

## <a name="configure-hub"></a>Bildirim hub'ı yapılandırma
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-your-app-for-push-notifications"></a>Anında iletme bildirimleri için uygulamanızı kaydetme
Uygulamanızı Windows Mağazası'na gönderin ve ardından Windows Bildirim Hizmetleri (anında iletme göndermek için WNS ile) tümleştirmek için sunucu projenizi yapılandırmanız gerekir.

1. UWP uygulaması projesini Visual Studio Çözüm Gezgini'nde sağ tıklayın, **deposu** > **uygulamayı mağaza ile ilişkilendir...** .

    ![Uygulamayı Windows mağazası ile ilişkilendirme](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
2. Sihirbazı'nda tıklatın **sonraki**, Microsoft hesabınızla oturum açın, uygulamanız için bir ad yazın **yeni bir uygulama adı yedek**, ardından **ayırma**.
3. Uygulama kaydı başarıyla oluşturulduktan sonra yeni uygulama adı seçin, **sonraki**ve ardından **ilişkilendirmek**. Bu, uygulama bildirimine gerekli Windows Mağazası kayıt bilgilerini ekler.  
4. Gidin [Windows Geliştirme Merkezi](https://dev.windows.com/en-us/overview), oturum açma Microsoft hesabınızla, yeni uygulama kaydında tıklatın **uygulamalarım**, ardından **Hizmetleri**  >   **Anında iletme bildirimleri**.
5. İçinde **anında iletme bildirimleri** sayfasında, **Live Services sitesi** altında **Microsoft Azure Mobile Services**.
6. Kayıt sayfanın altında bulunan değerini not **uygulama parolaları** ve **paket SID'si**, sonraki, mobil uygulamanızın arka ucuna yapılandırmak için kullanacağınız.

    ![Uygulamayı Windows mağazası ile ilişkilendirme](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

   > [!IMPORTANT]
   > Gizli anahtar ve paket SID'si önemli güvenlik kimlik bilgileridir. Bu değerleri kimseyle paylaşmayın veya uygulamanızla birlikte dağıtmayın. **Uygulama kimliği** gizli anahtarı ile Microsoft Account kimlik doğrulamasını yapılandırmak için kullanılır.
   >
   >

## <a name="configure-the-backend-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için arka uç yapılandırın
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

## <a id="update-service"></a>Anında iletme bildirimleri göndermek için sunucusunu güncelleştir
Arka uç projesi türünüzle eşleşen aşağıdaki yordamı kullanın&mdash;ya da [.NET arka ucu](#dotnet) veya [Node.js arka ucu](#nodejs).

### <a name="dotnet"></a>.NET arka uç projesi
1. Visual Studio'da sunucu projesi sağ tıklatın ve **NuGet paketlerini Yönet**, Microsoft.Azure.NotificationHubs için arama yapın ve ardından **yükleme**. Bu, bildirim hub'ları istemci kitaplığı yükler.
2. Genişletin **denetleyicileri**TodoItemController.cs açın ve aşağıdaki using deyimlerini:

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

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Bu kod, yeni bir öğe ekleme tamamlandıktan sonra bir anında iletme bildirimi göndermek için bildirim hub'ı söyler.
4. Sunucu projesi yeniden yayımlayın.

### <a name="nodejs"></a>Node.js arka uç projesi
1. Bunu zaten bunu yapmadıysanız [hızlı başlangıç projesi indirme](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) veya başka kullanım [Azure portalında çevrimiçi düzenleyicisini](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Todoitem.js dosyasındaki var olan kodu aşağıdakilerle değiştirin:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
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

    Bu, yeni bir Yapılacaklar öğesi eklendiğinde item.text içeren WNS bildirim gönderir.
3. Yerel bilgisayarınızda dosyayı düzenlerken, sunucu projesi yeniden yayımlayın.

## <a id="update-app"></a>Uygulamanıza anında iletme bildirimleri ekleme
Ardından, uygulamanızı anında iletme bildirimleri başlatma üzerinde için kaydetmeniz gerekir. Kimlik doğrulaması zaten etkinleştirdiyseniz, kullanıcı için anında iletme bildirimleri kaydetmeyi denemeden önce oturum açtığında olduğundan emin olun.

1. Açık **App.xaml.cs** proje dosyası ve aşağıdakileri ekleyin `using` deyimleri:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
2. Aynı dosyada aşağıdakileri ekleyin **Initnotificationsasync** yöntemi tanımına **uygulama** sınıfı:

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Bu kod, WNS'den uygulamanın ChannelURI alır ve ardından bu ChannelURI, mobil uygulama hizmeti ile kaydeder.
3. Üstündeki **OnLaunched** olay işleyicisini **App.xaml.cs**, ekleme **zaman uyumsuz** değiştirici yöntem tanımına ve yeni aşağıdaki çağrıyı ekleyin  **Initnotificationsasync** aşağıdaki örnekteki gibi yöntemi:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Bu kısa süreli ChannelURI uygulama başlatılan her zaman kayıtlı olduğunu güvence altına alır.
4. UWP uygulama projenizi yeniden derleyin. Uygulamanız şimdi bildirim almaya hazırdır.

## <a id="test"></a>Test, uygulamanızda anında iletme bildirimleri
[!INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]

## <a id="more"></a>Sonraki adımlar
Anında iletme bildirimleri hakkında daha fazla bilgi edinin:

* [Azure Mobile Apps için yönetilen istemciyi kullanma](app-service-mobile-dotnet-how-to-use-client-library.md#pushnotifications)  
  Şablonları platformlar arası gönderim ve yerelleştirilmiş bildirim göndermek için esneklik sağlar. Şablonlarını kaydetmek öğrenin.
* [Anında iletme bildirimi sorunlarını tanılamak](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Neden bildirimleri bırakılan veya cihazlarda bitmeyen çeşitli nedenleri vardır. Bu konuda çözümlemek ve anında iletme bildirimi hataları kök nedenini anlamak nasıl gösterilmektedir.

Aşağıdaki öğreticiler birini açın etmeden göz önünde bulundurun:

* [Uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.
* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Mobil Uygulama arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme son kullanıcıların, ağ bağlantısı yokken dahi, mobil uygulama ile etkileşim kurmalarına &mdash;veri görüntüleme, ekleme ya da değiştirme&mdash; olanak tanır.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->
