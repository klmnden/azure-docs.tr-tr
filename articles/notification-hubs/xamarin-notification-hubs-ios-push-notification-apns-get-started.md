---
title: Azure Notification Hubs kullanarak Xamarin.iOS uygulamalarına anında iletme bildirimleri gönderme | Microsoft Docs
description: Bu öğreticide, bir Xamarin.iOS uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenirsiniz.
services: notification-hubs
keywords: ios anında iletme bildirimleri,anında iletme iletileri,anında iletme bildirimleri,anında iletme iletisi
documentationcenter: xamarin
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 4d4dfd42-c5a5-4360-9d70-7812f96924d2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 94268d47eaf23e1bac54bb9791ec149bb5cccacb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60426681"
---
# <a name="tutorial-push-notifications-to-xamarinios-apps-using-azure-notification-hubs"></a>Öğretici: Xamarin.iOS uygulamaları Azure Notification hubs'ı kullanarak anında iletme bildirimleri

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış

Bu öğretici, bir iOS uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir. [Apple Anında İletilen Bildirim Servisi'ni (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html) kullanarak anında iletme bildirimleri alan boş bir Xamarin.iOS uygulaması oluşturursunuz.

İşiniz bittiğinde, uygulamanızı çalıştıran tüm cihazlara anında iletme bildirimleri yayımlamak için bildirim hub’ınızı kullanabilirsiniz. Tamamlanan kodu [NotificationHubs uygulaması][GitHub] örneğinde bulabilirsiniz.

Bu öğreticide, aşağıdaki görevleri gerçekleştirmek için kod oluşturur/güncelleştirirsiniz:

> [!div class="checklist"]
> * Sertifika imzalama isteği dosyası oluşturma
> * Anında iletme bildirimleri için uygulamanızı kaydetme
> * Uygulama için bir sağlama profili oluşturun
> * iOS anında iletme bildirimleri için bildirim hub’ınızı yapılandırma
> * Test amaçlı anında iletme bildirimleri gönderme

## <a name="prerequisites"></a>Önkoşullar

* **Azure aboneliği**. Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.
* [Xcode][Install Xcode]'un en son sürümü
* iOS 10 (veya sonraki bir sürümü) uyumlu bir cihaz
* [Apple Developer Program](https://developer.apple.com/programs/) üyeliği.
* [Mac için Visual Studio]
  
  > [!NOTE]
  > iOS anında iletme bildirimlerinin yapılandırma gereksinimleri nedeniyle, örnek uygulamayı benzetici yerine fiziksel bir iOS cihazında (iPhone veya iPad) dağıtmanız ve test etmeniz gerekir.

Bu öğreticiyi tamamlamak Xamarin.iOS uygulamalarına ilişkin diğer tüm Notification Hubs öğreticileri için önkoşuldur.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>iOS anında iletme bildirimleri için bildirim hub’ınızı yapılandırma

Bu bölüm, yeni bir bildirim hub'ı oluşturma ve önceden oluşturduğunuz **.p12** anında iletme sertifikasını kullanarak APNs ile kimlik doğrulaması yapılandırma adımlarında size yol gösterecektir. Önceden oluşturduğunuz bir bildirim hub'ını kullanmak istiyorsanız 5. adıma geçebilirsiniz.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-ios-settings-for-the-notification-hub"></a>Bildirim hub’ı için iOS ayarlarını yapılandırma

1. **BİLDİRİM AYARLARI** grubunda **Apple (APNS)** seçeneğini belirleyin.
2. **Sertifika**’yı seçin, **dosya** simgesine tıklayın ve önceden dışarı aktardığınız **.p12** dosyasını seçin.
3. Sertifika için **parola** belirtin.
4. **Korumalı alan** modunu seçin. Yalnızca uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek istiyorsanız **Üretim** modunu kullanın.

    ![Azure portal'da APNs'yi yapılandırma][6]

    ![Azure portal'da APNs sertifikasını yapılandırma][7]

Bildirim hub'ınız şimdi APNs ile birlikte çalışmak üzere yapılandırıldı. Ayrıca uygulamanızı kaydetmenizi ve anlık iletme bildirimleri göndermenizi sağlayan bağlantı dizelerine sahipsiniz.

## <a name="connect-your-app-to-the-notification-hub"></a>Uygulamanızı bildirim hub'ına bağlama

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

1. Visual Studio'da yeni bir iOS projesi oluşturup **Tek Görünüm Uygulaması** şablonunu seçin ve **İleri**'ye tıklayın

     ![Visual Studio - Uygulama Türünü Seçme][31]

2. Uygulama adı ve kuruluş tanımlayıcınızı girin, ardından tıklatın **sonraki**, ardından **oluştur**

3. Çözüm görünümünde **Kimlik** bölümündeki *Info.plist* dosyasına çift tıklayarak Paket Tanımlayıcısının sağlama profili oluştururken kullandığınızla eşleştiğinden emin olun. **İmzalama** bölümünde **Ekip** altında Geliştirici hesabınızın seçili olduğundan, "İmzalamayı otomatik olarak yönet" seçeneğinin belirlendiğinden ve İmza Sertifikası ile Sağlama Profili bilgilerinizin otomatik olarak seçildiğinden emin olun.

    ![Visual Studio- iOS Uygulaması Yapılandırması][32]

4. Çözüm görünümünde çift `Entitlements.plist` olduğundan emin olun **anında iletme bildirimlerini etkinleştirme**"** denetlenir.

    ![Visual Studio- iOS Destek Hakları Yapılandırması][33]

5. Azure Messaging paketini ekleyin. Çözüm görünümünde projeye sağ tıklayıp **Ekle** > **NuGet Paketleri Ekle**'yi seçin. **Xamarin.Azure.NotificationHubs.iOS** araması yapıp projeyi pakete ekleyin.

6. Sınıfınıza yeni bir dosya ekleyin, adlandırın `Constants.cs` ile dize sabiti yer tutucularını değiştirin ve aşağıdaki değişkenleri ekleyip `hubname` ve `DefaultListenSharedAccessSignature` daha önce not ettiğiniz.

    ```csharp
    // Azure app-specific connection string and hub path
    public const string ListenConnectionString = "<Azure DefaultListenSharedAccess Connection String>";
    public const string NotificationHubName = "<Azure Notification Hub Name>";
    ```

7. İçinde `AppDelegate.cs`, aşağıdaki using deyimi:

    ```csharp
    using WindowsAzure.Messaging;
    ```

8. Bir örneğini bildirmeniz `SBNotificationHub`:

    ```csharp
    private SBNotificationHub Hub { get; set; }
    ```

9. İçinde `AppDelegate.cs`, güncelleştirme `FinishedLaunching()` aşağıdaki kodu eşleştirmek için:

    ```csharp
    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        if (UIDevice.CurrentDevice.CheckSystemVersion(10, 0))
        {
            UNUserNotificationCenter.Current.RequestAuthorization(UNAuthorizationOptions.Alert | UNAuthorizationOptions.Sound | UNAuthorizationOptions.Sound,
                                                                    (granted, error) =>
            {
                if (granted)
                    InvokeOnMainThread(UIApplication.SharedApplication.RegisterForRemoteNotifications);
            });
        } else if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                    new NSSet ());

            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        } else {
            UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
        }

        return true;
    }
    ```

10. İçinde `AppDelegate.cs`, geçersiz kılma `RegisteredForRemoteNotifications()` yöntemi:

    ```csharp
    public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
    {
        Hub = new SBNotificationHub(Constants.ListenConnectionString, Constants.NotificationHubName);

        Hub.UnregisterAllAsync (deviceToken, (error) => {
            if (error != null)
            {
                System.Diagnostics.Debug.WriteLine("Error calling Unregister: {0}", error.ToString());
                return;
            }

            NSSet tags = null; // create tags if you want
            Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                if (errorCallback != null)
                    System.Diagnostics.Debug.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
            });
        });
    }
    ```

11. İçinde `AppDelegate.cs`, geçersiz kılma `ReceivedRemoteNotification()` yöntemi:

    ```csharp
    public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
    {
        ProcessNotification(userInfo, false);
    }
    ```

12. İçinde `AppDelegate.cs`, oluşturma `ProcessNotification()` yöntemi:

    ```csharp
    void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
    {
        // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
        if (null != options && options.ContainsKey(new NSString("aps")))
        {
            //Get the aps dictionary
            NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;

            //Extract the alert text
            // NOTE: If you're using the simple alert by just specifying
            // "  aps:{alert:"alert msg here"}  ", this will work fine.
            // But if you're using a complex alert with Localization keys, etc.,
            // your "alert" object from the aps dictionary will be another NSDictionary.
            // Basically the JSON gets dumped right into a NSDictionary,
            // so keep that in mind.
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //If this came from the ReceivedRemoteNotification while the app was running,
            // we of course need to manually process things like the sound, badge, and alert.
            if (!fromFinishedLaunching)
            {
                //Manually show an alert
                if (!string.IsNullOrEmpty(alert))
                {
                    UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                    avAlert.Show();
                }
            }
        }
    }
    ```

    > [!NOTE]
    > Geçersiz kılmayı seçebilirsiniz `FailedToRegisterForRemoteNotifications()` ağ bağlantısının olmaması gibi durumlarda işlemek için. Bu seçim, kullanıcı uygulamanızı çevrimdışı modda (örneğin, Uçak) başlattığında ve uygulamanıza özgü anında iletme mesajlaşması senaryoları kullanmak istediğinizde özellikle önemlidir.

13. Cihazınızda uygulamayı çalıştırın.

## <a name="send-test-push-notifications"></a>Test amaçlı anında iletme bildirimleri gönderme

[Azure portalındaki] *Test Gönderimi* seçeneğini kullanarak uygulamanızda bildirim alma testi gerçekleştirebilirsiniz. Bu, cihazınıza test amaçlı anında iletme bildirimi gönderir.

![Azure portalı - Test Gönderimi][30]

Anında iletme bildirimleri normal olarak, uyumlu bir kitaplık kullanılarak Mobile Apps veya ASP.NET gibi bir arka uç hizmetine gönderilir. Arka ucunuz için uygun bir kitaplık yoksa bildirim iletileri göndermek için doğrudan REST API de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, arka uca kayıtlı olan tüm iOS cihazlarınıza yayın bildirimleri gönderdiniz. Belirli iOS cihazlarına nasıl anında iletme bildirimleri gönderileceğini öğrenmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Belirli cihazlara anında iletme bildirimleri gönderme](notification-hubs-ios-xplat-segmented-apns-push-notification.md)

<!-- Images. -->
[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png
[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png
[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png
[32]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-app-settings.png
[33]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-entitlements-settings.png

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: https://go.microsoft.com/fwlink/p/?LinkId=272456
[Mac için Visual Studio]: https://visualstudio.microsoft.com/vs/mac/
[Local and Push Notification Programming Guide]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1
[Apple Push Notification Service]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html
[Apple Push Notification Service fwlink]: https://go.microsoft.com/fwlink/p/?LinkId=272584
[GitHub]: https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs
[Azure portalındaki]: https://portal.azure.com