---
title: Xamarin.Forms uygulamanızı anında iletme bildirimleri ekleme | Microsoft Docs
description: Xamarin.Forms uygulamalarınızı çoklu platform anında iletme bildirimleri göndermek için Azure services'ı kullanmayı öğrenin.
services: app-service\mobile
documentationcenter: xamarin
author: elamalani
manager: crdun
editor: ''
ms.assetid: d9b1ba9a-b3f2-4d12-affc-2ee34311538b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: b7e2ff63211ec5891a48a585e4f69e18116cdeb3
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446565"
---
# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Xamarin.Forms uygulamanızı anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirme merkezi hizmetlerinde yeni ve tümleşik yatırım yapıyor. Geliştiriciler **derleme**, **Test** ve **Dağıt** hizmetlerinin sürekli tümleştirme ve teslim işlem hattı ayarlayın. Uygulama dağıtıldığında, geliştiriciler kendi uygulamasını kullanarak kullanımı ve durumu izleyebilirsiniz **Analytics** ve **tanılama** kullanarak kullanıcılarla etkileşim kurun ve hizmetlerini **anında iletme** hizmeti. Geliştiriciler de yararlanabilir **Auth** , kullanıcıların kimliğini doğrulamak ve **veri** kalıcı hale getirmek ve uygulama verilerini bulutta eşitleme hizmeti. Kullanıma [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-xamarin-forms-get-started-push) bugün.
>

## <a name="overview"></a>Genel Bakış

Bu öğreticide, kullanmasından tüm projeler için anında iletme bildirimleri ekleme [Xamarin.Forms Hızlı Başlangıç](app-service-mobile-xamarin-forms-get-started.md). Başka bir deyişle, bir kayıt eklenir her zaman tüm platformlar arası istemcileri için anında iletme bildirimi gönderilir.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantı paketi gerekir. Daha fazla bilgi için [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Önkoşullar

İOS için ihtiyacınız olacak bir [Apple Developer Program üyeliği](https://developer.apple.com/programs/ios/) ve fiziksel bir iOS cihazına. [Anında iletme bildirimlerini iOS simülatörü desteklemiyor](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

## <a name="configure-hub"></a>Bildirim hub'ı yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için sunucu projesi güncelleştirme

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-the-android-project-optional"></a>Yapılandırma ve (isteğe bağlı) Android projesi çalıştırma

Android için Xamarin.Forms Droid projesini için anında iletme bildirimleri etkinleştirmek için bu bölümü tamamlayın.

### <a name="enable-firebase-cloud-messaging-fcm"></a>Firebase Cloud Messaging (FCM) etkinleştirme

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm"></a>Mobile Apps arka ucu FCM kullanarak anında iletme istekleri göndermek için yapılandırma

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-to-the-android-project"></a>Android projesine anında iletme bildirimleri ekleme

Arka uç FCM ile yapılandırılmış FCM ile kaydetmek için istemciye bileşenleri ve kodları ekleyebilirsiniz. Mobile Apps arka ucu aracılığıyla Azure Notification Hubs ile anında iletme bildirimleri için kaydolun ve bildirimler alırsınız.

1. İçinde **Droid** projesinde **başvuruları > NuGet paketlerini Yönet...** .
1. NuGet Paket Yöneticisi penceresi içinde arama **Xamarin.Firebase.Messaging** paketini ve projeye ekleyin.
1. Proje özelliklerinde **Droid** proje, uygulamanın Android 7.0 veya üzeri sürümünü kullanarak derle ayarlayın.
1. Ekleme **google-services.json** köküne Firebase konsolundan indirilen dosya, **Droid** proje ve derleme eylemi kümesine **GoogleServicesJson**. Daha fazla bilgi için [Google Hizmetleri JSON dosyası ekleme](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/remote-notifications-with-fcm/#Add_the_Google_Services_JSON_File).

#### <a name="registering-with-firebase-cloud-messaging"></a>Kaydetme ile Firebase Cloud Messaging

1. **AndroidManifest.xml** dosyasını açın ve aşağıdaki `<receiver>` öğelerini `<application>` öğesine ekleyin:

    ```xml
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
        </intent-filter>
    </receiver>
    ```

#### <a name="implementing-the-firebase-instance-id-service"></a>Firebase örnek kimlik hizmetinin uygulama

1. Yeni bir sınıfa eklemek **Droid** adlı proje `FirebaseRegistrationService`, emin olun aşağıdaki `using` deyimlerini dosyanın üstüne bulunduğunu:

    ```csharp
    using System.Threading.Tasks;
    using Android.App;
    using Android.Util;
    using Firebase.Iid;
    using Microsoft.WindowsAzure.MobileServices;
    ```

1. Boş değiştirin `FirebaseRegistrationService` aşağıdaki kodla sınıfı:

    ```csharp
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class FirebaseRegistrationService : FirebaseInstanceIdService
    {
        const string TAG = "FirebaseRegistrationService";

        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "Refreshed token: " + refreshedToken);
            SendRegistrationTokenToAzureNotificationHub(refreshedToken);
        }

        void SendRegistrationTokenToAzureNotificationHub(string token)
        {
            // Update notification hub registration
            Task.Run(async () =>
            {
                await AzureNotificationHubService.RegisterAsync(TodoItemManager.DefaultManager.CurrentClient.GetPush(), token);
            });
        }
    }
    ```

    `FirebaseRegistrationService` Sınıfı, FCM erişmek için uygulamayı yetkilendirme güvenlik belirteçleri oluşturmak için sorumludur. `OnTokenRefresh` Yöntemi uygulama FCM kayıt belirtecinizi aldığında çağrılır. Belirteçten yöntemi alır `FirebaseInstanceId.Instance.Token` özelliği FCM ile zaman uyumsuz olarak güncelleştirilir. `OnTokenRefresh` Yöntemi nadiren çağrılır, uygulamanın yüklenmesi veya kaldırılması, kullanıcı uygulama verileri sildiğinde uygulamanın örnek kimliği vuruşunu sildiğinde, belirteç yalnızca güncelleştirildiğinden veya güvenlik belirtecinin olduğunda gizliliği. Ayrıca, uygulama, belirteci düzenli olarak, genellikle her 6 ayda bir yenileme FCM örnek kimliği hizmeti ister.

    `OnTokenRefresh` Yöntemini de çağırır `SendRegistrationTokenToAzureNotificationHub` Azure bildirim Hub'ınızla kullanıcının kayıt belirtecini ilişkilendirmek için kullanılan yöntem.

#### <a name="registering-with-the-azure-notification-hub"></a>Azure bildirim Hub'ıyla

1. Yeni bir sınıfa eklemek **Droid** adlı proje `AzureNotificationHubService`, emin olun aşağıdaki `using` deyimlerini dosyanın üstüne bulunduğunu:

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Android.Util;
    using Microsoft.WindowsAzure.MobileServices;
    using Newtonsoft.Json.Linq;
    ```

1. Boş değiştirin `AzureNotificationHubService` aşağıdaki kodla sınıfı:

    ```csharp
    public class AzureNotificationHubService
    {
        const string TAG = "AzureNotificationHubService";

        public static async Task RegisterAsync(Push push, string token)
        {
            try
            {
                const string templateBody = "{\"data\":{\"message\":\"$(messageParam)\"}}";
                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBody}
                };

                await push.RegisterAsync(token, templates);
                Log.Info("Push Installation Id: ", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                Log.Error(TAG, "Could not register with Notification Hub: " + ex.Message);
            }
        }
    }
    ```

    `RegisterAsync` Yöntemi olarak JSON ve kayıtları Firebase kayıt belirtecinizi kullanarak bildirim hub'ından, şablon bildirim almak için basit bir bildirim iletisi şablonu oluşturur. Bu, Azure bildirim Hub'ından gönderilen tüm bildirimler kayıt belirtecinizi tarafından temsil edilen cihaz hedeflediğiniz sağlar.

#### <a name="displaying-the-contents-of-a-push-notification"></a>Anında iletme bildirimi içeriğini görüntüleme

1. Yeni bir sınıfa eklemek **Droid** adlı proje `FirebaseNotificationService`, emin olun aşağıdaki `using` deyimlerini dosyanın üstüne bulunduğunu:

    ```csharp
    using Android.App;
    using Android.Content;
    using Android.Media;
    using Android.Support.V7.App;
    using Android.Util;
    using Firebase.Messaging;
    ```

1. Boş değiştirin `FirebaseNotificationService` aşağıdaki kodla sınıfı:

    ```csharp
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class FirebaseNotificationService : FirebaseMessagingService
    {
        const string TAG = "FirebaseNotificationService";

        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);

            // Pull message body out of the template
            var messageBody = message.Data["message"];
            if (string.IsNullOrWhiteSpace(messageBody))
                return;

            Log.Debug(TAG, "Notification message body: " + messageBody);
            SendNotification(messageBody);
        }

        void SendNotification(string messageBody)
        {
            var intent = new Intent(this, typeof(MainActivity));
            intent.AddFlags(ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new NotificationCompat.Builder(this)
                .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
                .SetContentTitle("New Todo Item")
                .SetContentText(messageBody)
                .SetContentIntent(pendingIntent)
                .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))
                .SetAutoCancel(true);

            var notificationManager = NotificationManager.FromContext(this);
            notificationManager.Notify(0, notificationBuilder.Build());
        }
    }
    ```

    `OnMessageReceived` Uygulama FCM bir bildirim aldığında çağrılır, yöntemi, ileti içeriği ayıklar ve çağırır `SendNotification` yöntemi. Bu yöntem, uygulama çalışırken bildirim alanında görüntülenen bildirim ile başlatılan bir yerel bildirim iletisi içeriği dönüştürür.

Artık, bir Android cihaz veya öykünücü üzerinde çalışan uygulama hazır test anında iletme bildirimleri olursunuz.

### <a name="test-push-notifications-in-your-android-app"></a>Android uygulamanızı test anında iletme bildirimleri

Yalnızca bir öykünücü üzerinde test ederken ilk iki adım gereklidir.

1. Dağıtma ve bir cihaz veya Google Play Hizmetleri yapılandırıldı öykünücü üzerinde hata ayıklama emin olun. Bu, kontrol ederek doğrulanabilir **Play** uygulamalar, cihaz veya öykünücü üzerinde yüklenir.
2. Bir Google hesabı tıklayarak Android cihaza ekleyin **uygulamaları** > **ayarları** > **Hesap Ekle**. Daha sonra cihaza var olan bir Google hesabı ekleyin veya yeni bir tane oluşturmak için istemleri izleyin.
3. Visual Studio veya Xamarin Studio'da, sağ **Droid** projesine **başlangıç projesi olarak ayarla**.
4. Tıklayın **çalıştırma** projeyi oluşturun ve uygulamayı Android cihaz veya öykünücü üzerinde başlatın.
5. Uygulamasında, bir görev yazın ve ardından artı ( **+** ) simgesi.
6. Bir öğe eklendiğinde bir bildiriminin alındığını doğrulayın.

## <a name="configure-and-run-the-ios-project-optional"></a>Yapılandırma ve (isteğe bağlı) iOS projesi çalıştırma

Bu bölüm iOS cihazları için Xamarin iOS projesi çalıştırmaya yöneliktir. iOS cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

[!INCLUDE [Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

#### <a name="configure-the-notification-hub-for-apns"></a>APNS için bildirim hub'ı yapılandırma

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Ardından, Xamarin Studio veya Visual Studio'da iOS projesi ayarını yapılandıracaksınız.

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-to-your-ios-app"></a>İOS uygulamanıza anında iletme bildirimleri ekleme

1. İçinde **iOS** proje, AppDelegate.cs açın ve aşağıdaki deyimi kod dosyasının en üstüne ekleyin.

    ```csharp
    using Newtonsoft.Json.Linq;
    ```

2. İçinde **AppDelegate** sınıfı, bir geçersiz kılma için **RegisteredForRemoteNotifications** olay bildirimlerini de kaydetmeniz için:

    ```csharp
    public override void RegisteredForRemoteNotifications(UIApplication application,
        NSData deviceToken)
    {
        const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

        JObject templates = new JObject();
        templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

        // Register for push with your mobile app
        Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
        push.RegisterAsync(deviceToken, templates);
    }
    ```

3. İçinde **AppDelegate**, ayrıca aşağıdaki geçersiz kılma için ekleme **DidReceiveRemoteNotification** olay işleyicisi:

    ```csharp
    public override void DidReceiveRemoteNotification(UIApplication application,
        NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
    {
        NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

        string alert = string.Empty;
        if (aps.ContainsKey(new NSString("alert")))
            alert = (aps[new NSString("alert")] as NSString).ToString();

        //show alert
        if (!string.IsNullOrEmpty(alert))
        {
            UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
            avAlert.Show();
        }
    }
    ```

    Bu yöntem, uygulama çalışırken gelen bildirimler işler.

4. İçinde **AppDelegate** sınıfında, aşağıdaki kodu ekleyin **FinishedLaunching** yöntemi:

    ```csharp
    // Register for push notifications.
    var settings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert
        | UIUserNotificationType.Badge
        | UIUserNotificationType.Sound,
        new NSSet());

    UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications();
    ```

    Bu uzak bildirimler için destek sağlar ve kayıt isteği gönderin.

Uygulamanızı anında iletme bildirimlerini desteklemek için güncelleştirilmiştir.

#### <a name="test-push-notifications-in-your-ios-app"></a>Test iOS uygulamanıza anında iletme bildirimleri

1. İOS projesine sağ tıklayın ve **başlangıç projesi olarak ayarla**.
2. Basın **çalıştırın** düğmesini veya **F5** Visual Studio projeyi oluşturun ve uygulamayı bir iOS cihazının başlatın. Ardından **Tamam** anında iletme bildirimleri kabul etmek için.

   > [!NOTE]
   > Anında iletme bildirimleri uygulamanızdan açıkça kabul etmeniz gerekir. Bu istek, yalnızca uygulamayı çalıştıran ilk kez oluşur.

3. Uygulamasında, bir görev yazın ve ardından artı ( **+** ) simgesi.
4. Bildirim alınırsa ve ardından doğrulamak **Tamam** bildirimi kapatmak için.

## <a name="configure-and-run-windows-projects-optional"></a>Yapılandırma ve çalıştırma (isteğe bağlı) Windows projeleri

Bu bölüm Windows cihazları için WinPhone81 projeleri ve Xamarin.Forms WinApp çalıştırmaya yöneliktir. Bu adımlar da evrensel Windows Platformu (UWP) projeleri destekler. Windows cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a>Windows bildirim Hizmeti'ni (WNS) Windows uygulamanızı anında iletme bildirimleri için kaydetme

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-the-notification-hub-for-wns"></a>WNS için bildirim hub'ı yapılandırma

[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-to-your-windows-app"></a>Windows uygulamanızı anında iletme bildirimleri ekleme

1. Visual Studio'da açın **App.xaml.cs** bir Windows içinde proje ve aşağıdaki deyimleri ekleyin.

    ```csharp
    using Newtonsoft.Json.Linq;
    using Microsoft.WindowsAzure.MobileServices;
    using System.Threading.Tasks;
    using Windows.Networking.PushNotifications;
    using <your_TodoItemManager_portable_class_namespace>;
    ```

    Değiştirin `<your_TodoItemManager_portable_class_namespace>` içeren taşınabilir proje ad alanıyla `TodoItemManager` sınıfı.

2. App.xaml.cs dosyasında, aşağıdaki ekleyin **Initnotificationsasync** yöntemi:

    ```csharp
    private async Task InitNotificationsAsync()
    {
        var channel = await PushNotificationChannelManager
            .CreatePushNotificationChannelForApplicationAsync();

        const string templateBodyWNS =
            "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

        JObject headers = new JObject();
        headers["X-WNS-Type"] = "wns/toast";

        JObject templates = new JObject();
        templates["genericMessage"] = new JObject
        {
            {"body", templateBodyWNS},
            {"headers", headers} // Needed for WNS.
        };

        await TodoItemManager.DefaultManager.CurrentClient.GetPush()
            .RegisterAsync(channel.Uri, templates);
    }
    ```

    Bu yöntem, anında iletme bildirim kanalı alır ve bildirim hub'ından şablon bildirimleri almak için bir şablon kaydeder. Destekleyen bir şablon bildirim *messageParam* bu istemciye teslim edilir.

3. App.xaml.cs dosyasında, güncelleştirme **OnLaunched** ekleyerek olay işleyici yöntemi tanımının `async` değiştiricisi. Ardından yönteminin sonunda aşağıdaki kod satırını ekleyin:

    ```csharp
    await InitNotificationsAsync();
    ```

    Bu anında iletme bildirimi kaydı oluşturan veya uygulama her başlatıldığında yenilenmesini sağlar. WNS anında iletme kanal her zaman etkin olacağını garanti etmek için bunu yapmak önemlidir.  

4. Visual Studio Çözüm Gezgini'nde açın **Package.appxmanifest** dosya ve ayarlama **bildirim özellikli** için **Evet** altında **bildirimleri**.
5. Uygulamayı oluşturun ve hatasız doğrulayın. İstemci uygulamanızın artık Mobile Apps arka ucu gelen şablon bildirimlere kaydolması. Bu bölümde, çözümünüzdeki her Windows projesi için tekrarlayın.

#### <a name="test-push-notifications-in-your-windows-app"></a>Windows uygulamanızı test anında iletme bildirimleri

1. Visual Studio'da bir Windows projesini sağ tıklayın ve **başlangıç projesi olarak ayarla**.
2. Projeyi oluşturmak ve uygulamayı başlatmak için **Çalıştır** düğmesine basın.
3. Uygulamada, yeni bir todoıtem için'bir ad yazın ve ardından artı ( **+** ) simgesi ekleyin.
4. Bir öğe eklendiğinde bir bildiriminin alındığını doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Anında iletme bildirimleri hakkında daha fazla bilgi edinebilirsiniz:

* [Azure mobil uygulamalardan anında iletme bildirimleri gönderme](https://developer.xamarin.com/guides/xamarin-forms/cloud-services/push-notifications/azure/)
* [Firebase Cloud Messaging](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/firebase-cloud-messaging/)
* [Uzak bildirimler Firebase ile bulut Mesajlaşma](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/remote-notifications-with-fcm/)
* [Anında iletme bildirimi sorunlarını tanılayın](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Neden bildirimleri bırakılan veya cihazlarda son çeşitli nedenleri vardır. Bu konuda, çözümlemek ve anında iletme bildirimi hataları kök nedenini anlamak nasıl gösterir.

Ayrıca aşağıdaki öğreticilerden birine açın devam edebilirsiniz:

* [Uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-xamarin-forms-get-started-users.md)  
  Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.
* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Mobile Apps arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı Eşitleme ile kullanıcılar mobil uygulama ile etkileşim kurabilir&mdash;görüntüleme, ekleme veya verileri değiştirme&mdash;hiçbir ağ bağlantısı olduğunda bile.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: https://go.microsoft.com/fwlink/p/?LinkId=272333
