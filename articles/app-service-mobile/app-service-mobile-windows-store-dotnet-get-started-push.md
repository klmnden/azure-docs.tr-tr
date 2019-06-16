---
title: Evrensel Windows Platformu (UWP) uygulamanıza anında iletme bildirimleri ekleme | Microsoft Docs
description: Evrensel Windows Platformu (UWP) uygulamasına anında iletme bildirimleri göndermek için Azure App Service Mobile Apps ve Azure Notification Hubs'ı kullanmayı öğrenin.
services: app-service\mobile,notification-hubs
documentationcenter: windows
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 6de1b9d4-bd28-43e4-8db4-94cd3b187aa3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: crdun
ms.openlocfilehash: 7efd853e7b66933cac811625d7510139864f41f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62128041"
---
# <a name="add-push-notifications-to-your-windows-app"></a>Windows uygulamanızı anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, anında iletme bildirimleri ekleme [Windows Hızlı Başlangıç](app-service-mobile-windows-store-dotnet-get-started.md) anında iletme bildirimi kayıt eklenen her zaman cihaza gönderilir, böylece proje.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantı paketi gerekir. Bkz: [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) daha fazla bilgi için.

## <a name="configure-hub"></a>Bildirim hub'ı yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-your-app-for-push-notifications"></a>Anında iletme bildirimleri için uygulamanızı kaydetme

Microsoft Store için uygulamanızı gönderin, sonra tümleştirmek için sunucu projenizi yapılandırmak gereken [Windows Bildirim Hizmetleri (WNS)](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview) anında iletme göndermek için.

1. UWP uygulaması projesini Visual Studio Çözüm Gezgini'nde sağ tıklayın, **Store** > **uygulamayı Store ile ilişkilendir...** .

    ![Uygulamayı Microsoft Store ile ilişkilendirme](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)

2. Sihirbazı'nda tıklatın **sonraki**, Microsoft hesabınızla oturum açın, uygulamanız için bir ad yazın **yeni bir uygulama adı ayrılmaya**, ardından **ayırma**.
3. Uygulama kaydı başarıyla oluşturulduktan sonra yeni bir uygulama adı seçin, **sonraki**ve ardından **ilişkilendirmek**. Bu, uygulama bildirimine gerekli Microsoft Store kayıt bilgilerini ekler.
4. Gidin [uygulama kayıt portalı](https://apps.dev.microsoft.com/) ve Microsoft hesabınızla oturum açın. Önceki adımda ilişkili Windows Store uygulamaya tıklayın.
5. Kayıt sayfanın altındaki değeri Not **uygulama gizli dizilerini** ve **paket SID'si**, mobil uygulamanızın arka ucunu yapılandırmak için sonraki olarak kullanacağınız.

    ![Uygulamayı Microsoft Store ile ilişkilendirme](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

   > [!IMPORTANT]
   > Gizli anahtar ve paket SID'si önemli güvenlik kimlik bilgileridir. Bu değerleri kimseyle paylaşmayın veya uygulamanızla birlikte dağıtmayın. **Uygulama kimliği** sahip gizli dizi Microsoft Account kimlik doğrulamasını yapılandırmak için kullanılır.

[App Center](https://docs.microsoft.com/appcenter/sdk/push/uwp#prerequisite---register-your-app-for-windows-notification-services-wns) UWP uygulamalarına anında iletme bildirimleri için yapılandırmaya yönelik yönergeler de vardır.

## <a name="configure-the-backend-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için arka uç yapılandırın

[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

## <a id="update-service"></a>Anında iletme bildirimleri göndermek sunucuyı güncelleştir

Arka uç projesi eşleşeni aşağıdaki yordamı kullanın&mdash;ya da [.NET arka ucu](#dotnet) veya [Node.js arka ucu](#nodejs).

### <a name="dotnet"></a>.NET arka uç projesi

1. Visual Studio'da sunucu projeye sağ tıklayın ve **NuGet paketlerini Yönet**, Microsoft.Azure.NotificationHubs için arama yapın ve ardından tıklayın **yükleme**. Bu, bildirim hub'ları istemci kitaplığı yükler.
2. Genişletin **denetleyicileri**TodoItemController.cs açın ve aşağıdaki using deyimlerini:

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
    ```

    Bu kod, yeni bir öğe ekleme sonra bir anında iletme bildirimi göndermek için bildirim hub'ı söyler.

4. Sunucu projesini yeniden yayımlayın.

### <a name="nodejs"></a>Node.js arka uç projesi
1. Bunu zaten bunu yapmadıysanız [hızlı başlangıç projesi indirme](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) veya başka kullanım [Azure portalında çevrimiçi düzenleyicisini](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Todoitem.js dosyasındaki mevcut kodu aşağıdakiyle değiştirin:

    ```javascript
    var azureMobileApps = require('azure-mobile-apps'),
    promises = require('azure-mobile-apps/src/utilities/promises'),
    logger = require('azure-mobile-apps/src/logger');

    var table = azureMobileApps.table();

    table.insert(function (context) {
    // For more information about the Notification Hubs JavaScript SDK,
    // see https://aka.ms/nodejshubs
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
    ```

    Bu, yeni bir todo öğesi eklendiğinde item.text içeren bir WNS bildirim gönderir.

3. Yerel bilgisayarınızda dosyası düzenlenirken, sunucu projesi yeniden yayımlayın.

## <a id="update-app"></a>Uygulamanıza anında iletme bildirimleri ekleme
Ardından, uygulamanızı anında iletme bildirimlerinin başlangıç kaydolması gerekir. Kimlik doğrulaması zaten etkinleştirdiyseniz, kullanıcı anında iletme bildirimlerini de kaydetmeniz denemeden önce oturum açtığında, emin olun.

1. Açık **App.xaml.cs** proje dosyası ve aşağıdakileri ekleyin `using` ifadeleri:

    ```csharp
    using System.Threading.Tasks;
    using Windows.Networking.PushNotifications;
    ```

2. Aynı dosyada, aşağıdaki ekleyin **Initnotificationsasync** yöntem tanımını **uygulama** sınıfı:

    ```csharp
    private async Task InitNotificationsAsync()
    {
        // Get a channel URI from WNS.
        var channel = await PushNotificationChannelManager
            .CreatePushNotificationChannelForApplicationAsync();

        // Register the channel URI with Notification Hubs.
        await App.MobileService.GetPush().RegisterAsync(channel.Uri);
    }
    ```

    Bu kod, WNS'den uygulamanın Channelurı alır ve ardından bu Channelurı, App Service mobil uygulama ile kaydeder.

3. Üst kısmındaki **OnLaunched** olay işleyicisinde **App.xaml.cs**, ekleme **zaman uyumsuz** değiştirici yöntem tanımına ve yeni aşağıdaki çağrıyı ekleyin  **Initnotificationsasync** yöntemi, aşağıdaki örnekte olduğu gibi:

    ```csharp
    protected async override void OnLaunched(LaunchActivatedEventArgs e)
    {
        await InitNotificationsAsync();

        // ...
    }
    ```

    Bu, uygulama her başlatıldığında kısa süreli Channelurı kayıtlı olduğunu garanti eder.

4. UWP uygulaması projenizi yeniden derleyin. Uygulamanız şimdi bildirim almaya hazırdır.

## <a id="test"></a>Uygulamanıza anında iletme bildirimleri test

[!INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]

## <a id="more"></a>Sonraki adımlar

Anında iletme bildirimleri hakkında daha fazla bilgi edinin:

* [Azure Mobile Apps için yönetilen istemciyi kullanma](app-service-mobile-dotnet-how-to-use-client-library.md#pushnotifications) şablonları platformlar arası bildirimler ve yerelleştirilmiş bildirimler gönderme esnekliği sağlar. Şablonları kaydetme hakkında bilgi edinin.
* [Anında iletme bildirimi sorunları tanılamak](../notification-hubs/notification-hubs-push-notification-fixer.md) neden bildirimleri bırakılan veya cihazlarda son çeşitli nedenleri vardır. Bu konuda, çözümlemek ve anında iletme bildirimi hataları kök nedenini anlamak nasıl gösterir.

Aşağıdaki öğreticilerden birine açın etmeden göz önünde bulundurun:

* [Uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-windows-store-dotnet-get-started-users.md) Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.
* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-windows-store-dotnet-get-started-offline-data.md) bir mobil uygulama arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme son kullanıcıların, ağ bağlantısı yokken dahi, mobil uygulama ile etkileşim kurmalarına &mdash;veri görüntüleme, ekleme ya da değiştirme&mdash; olanak tanır.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->
