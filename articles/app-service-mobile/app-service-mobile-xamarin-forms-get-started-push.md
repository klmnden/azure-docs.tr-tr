---
title: "Xamarin.Forms uygulamanıza anında iletme bildirimleri ekleme | Microsoft Docs"
description: "Xamarin.Forms uygulamalarınızı birden çok platform anında iletme bildirimleri göndermek için Azure hizmetleri kullanmayı öğrenin."
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: 
ms.assetid: d9b1ba9a-b3f2-4d12-affc-2ee34311538b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: crdun
ms.openlocfilehash: a9c7c5dbbc50ccf8c5383be28e96dfb82af48559
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Xamarin.Forms uygulamanıza anında iletme bildirimleri ekleme
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, kullanmasından kaynaklanan tüm projeleri için anında iletme bildirimleri ekleme [Xamarin.Forms Hızlı Başlangıç](app-service-mobile-xamarin-forms-get-started.md). Başka bir deyişle, bir kayda eklenen her zaman bir anında iletme bildirimi tüm platformlar arası istemcilere gönderilir.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantısı paketi gerekir. Daha fazla bilgi için bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Önkoşullar
İOS için size gereken bir [Apple Developer Program üyeliği](https://developer.apple.com/programs/ios/) ve fiziksel bir iOS cihazına. [İOS simülatörü anında iletme bildirimlerini desteklemiyor](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

## <a name="configure-hub"></a>Bildirim hub'ı yapılandırma
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>Güncelleştirme anında iletme bildirimleri göndermek için sunucu projesi
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-the-android-project-optional"></a>Yapılandırma ve (isteğe bağlı) Android projesi çalıştırma
Android için Xamarin.Forms Android projesi için anında iletme bildirimleri etkinleştirmek için bu bölümü tamamlayın.

### <a name="enable-firebase-cloud-messaging-fcm"></a>Firebase Cloud Messaging'i (FCM) etkinleştirme
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm"></a>Mobile Apps arka uç FCM kullanarak anında iletme istekleri göndermek için yapılandırma
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-to-the-android-project"></a>Android projesine anında iletme bildirimleri ekleme
Arka uç FCM ile yapılandırılmış FCM ile kaydetmek için istemciye bileşenleri ve kodları ekleyebilirsiniz. Mobile Apps arka ucu aracılığıyla Azure Notification Hubs ile anında iletme bildirimleri için kaydolun ve bildirim alırsınız.

1. İçinde **Droid** projesinde **başvuruları > NuGet paketlerini Yönet...** .
1. NuGet Paket Yöneticisi penceresinde arama **Xamarin.Firebase.Messaging** paketini ve bunu projeye ekleyin.
1. İçin proje properies içinde **Droid** proje, Android sürüm 7.0 veya üzeri kullanarak derlemek için uygulamayı Ayarla.
1. Ekleme **google services.json** Firebase konsolundan köküne karşıdan dosya **Droid** proje ve yapı eylemi kümesine **GoogleServicesJson**. Daha fazla bilgi için bkz: [Google Hizmetleri JSON dosyası ekleme](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/remote-notifications-with-fcm/#Add_the_Google_Services_JSON_File).

#### <a name="registering-with-firebase-cloud-messaging"></a>Firebase ile kaydetme bulut Mesajlaşma

1. Açık **AndroidManifest.xml** dosya ve aşağıdaki Ekle `<receiver>` elemanlara `<application>` öğe:

        <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
        <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
          <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="${applicationId}" />
          </intent-filter>
        </receiver>

#### <a name="implementing-the-firebase-instance-id-service"></a>Firebase örnek kimliği hizmeti uygulama

1. Yeni bir sınıf ekleyin **Droid** adlı projesi `FirebaseRegistrationService`, emin olun aşağıdaki `using` deyimleri mevcut dosyanın üst kısmında:

        using System.Threading.Tasks;
        using Android.App;
        using Android.Util;
        using Firebase.Iid;
        using Microsoft.WindowsAzure.MobileServices;

1. Boş Değiştir `FirebaseRegistrationService` aşağıdaki kodla sınıfı:

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

    `FirebaseRegistrationService` Sınıftır FCM erişmek için uygulamayı yetkilendirmeniz güvenlik belirteçleri oluşturmak için sorumlu. `OnTokenRefresh` Yöntemi uygulama kaydı belirteci FCM aldığında çağrılır. Belirteçten yöntemi alır `FirebaseInstanceId.Instance.Token` tarafından FCM zaman uyumsuz olarak güncelleştirilen özelliği. `OnTokenRefresh` Yöntemi seyrek çağrılır, uygulamanın yüklenmesi veya kaldırılması uygulama örnek kimliği siler, kullanıcı uygulama verileri sildiğinde, belirteç yalnızca güncelleştirildiğinden ya da güvenlik belirtecinin bırakıldı güvenliği aşılmış. Ayrıca, uygulamanın kendi belirtecini düzenli aralıklarla, genellikle 6 ayda yenileme FCM örnek kimliği hizmet ister.

    `OnTokenRefresh` Yöntemini de çağırır `SendRegistrationTokenToAzureNotificationHub` Azure bildirim Hub'ıyla kullanıcının kayıt belirtecini ilişkilendirmek için kullanılan yöntem.

#### <a name="registering-with-the-azure-notification-hub"></a>Azure bildirim hub'ı ile kaydetme

1. Yeni bir sınıf ekleyin **Droid** adlı projesi `AzureNotificationHubService`, emin olun aşağıdaki `using` deyimleri mevcut dosyanın üst kısmında:

        using System;
        using System.Threading.Tasks;
        using Android.Util;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

1. Boş Değiştir `AzureNotificationHubService` aşağıdaki kodla sınıfı:

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

    `RegisterAsync` Yöntemi, JSON ve kayıtları Firebase kayıt belirteci kullanarak bildirim hub'ından, şablon bildirimleri almak için bir basit bildirim iletisi şablonu oluşturur. Bu, Azure bildirim Hub'ından gönderilen bildirimlere kayıt belirtecinizi tarafından temsil edilen aygıt hedeflediğini sağlar.

#### <a name="displaying-the-contents-of-a-push-notification"></a>Anında iletme bildirimi içeriğini görüntüleme

1. Yeni bir sınıf ekleyin **Droid** adlı projesi `FirebaseNotificationService`, emin olun aşağıdaki `using` deyimleri mevcut dosyanın üst kısmında:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Util;
        using Firebase.Messaging;

1. Boş Değiştir `FirebaseNotificationService` aşağıdaki kodla sınıfı:

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

                var notificationBuilder = new Notification.Builder(this)
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

    `OnMessageReceived` Bir uygulama FCM bir bildirim aldığında çağrılır, yöntemi, içerik ileti ayıklar ve çağırır `SendNotification` yöntemi. Bu yöntem uygulama çalışırken bildirim alanında görünen bildirim başlatılır yerel bir bildirim iletisi içeriği dönüştürür.

Artık, bir Android cihaz veya öykünücü üzerinde çalışan uygulama hazır test anında iletme bildirimleri bulunur.

### <a name="test-push-notifications-in-your-android-app"></a>Android uygulamanızda test anında iletme bildirimleri
Bir öykünücü üzerinde yalnızca test ettiğiniz zaman ilk iki adım gerekli değildir.

1. Dağıtma ve Google API'leri hedef olarak ayarlanmış Android Sanal Aygıt Yöneticisi'nde aşağıda gösterildiği gibi sanal cihaza hata ayıklama emin olun.
2. Tıklayarak Android aygıtına bir Google hesabı eklemek **uygulamaları** > **ayarları** > **hesabı eklemek**. Daha sonra cihaza Google hesabınız eklemek veya yeni bir tane oluşturmak için istemleri izleyin.
3. Visual Studio veya Xamarin Studio'da sağ **Droid** proje ve tıklatın **başlangıç projesi olarak ayarla**.
4. Tıklatın **çalıştırmak** projeyi oluşturun ve uygulamayı Android cihaz veya öykünücü üzerinde başlatın.
5. Uygulamasında bir görevi yazın ve ardından artı (**+**) simgesi.
6. Bir öğe eklendiğinde, bir bildiriminin alındığını doğrulayın.

## <a name="configure-and-run-the-ios-project-optional"></a>Yapılandırma ve (isteğe bağlı) iOS projesi çalıştırma
Bu bölüm iOS cihazları için Xamarin iOS projesi çalıştırmaya yöneliktir. iOS cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-the-notification-hub-for-apns"></a>APNS için bildirim hub'ı yapılandırma
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Ardından, Xamarin Studio veya Visual Studio iOS proje ayarı yapılandırır.

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-to-your-ios-app"></a>İOS uygulamanıza anında iletme bildirimleri ekleme
1. İçinde **iOS** proje, AppDelegate.cs açın ve aşağıdaki ifadeyi Kod dosyasının üstüne ekleyin.

        using Newtonsoft.Json.Linq;
2. İçinde **AppDelegate** sınıfı, bir geçersiz kılma için ekleyip **RegisteredForRemoteNotifications** olay bildirimleri için kaydetmek için:

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
3. İçinde **AppDelegate**, ayrıca için aşağıdaki geçersiz kılma ekleyip **DidReceiveRemoteNotification** olay işleyicisi:

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

    Uygulama çalışırken bu yöntem gelen bildirimlerini işler.
4. İçinde **AppDelegate** sınıfında, aşağıdaki kodu ekleyin **FinishedLaunching** yöntemi:

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Bu uzak bildirimler için destek sağlar ve kayıt istekleri gönderme.

Uygulamanıza anında iletme bildirimlerini desteklemek üzere güncelleştirilmiştir.

#### <a name="test-push-notifications-in-your-ios-app"></a>İOS uygulamanızı test anında iletme bildirimleri
1. İOS projesine sağ tıklayın ve **başlangıç projesi olarak ayarla**.
2. Tuşuna **çalıştırmak** düğmesini veya **F5** projeyi oluşturun ve uygulamayı bir iOS aygıtı başlatmak için Visual Studio'da. Ardından **Tamam** anında iletme bildirimleri kabul etmek için.

   > [!NOTE]
   > Anında iletme bildirimleri, uygulamanızdan açıkça kabul etmeniz gerekir. Bu istek, yalnızca uygulamayı çalıştıran ilk kez oluşur.
   >
   >
3. Uygulamasında bir görevi yazın ve ardından artı (**+**) simgesi.
4. Bir bildirim alındı ve ardından doğrulamak **Tamam** bildirim kapatılamadı.

## <a name="configure-and-run-windows-projects-optional"></a>Yapılandırma ve çalıştırma (isteğe bağlı) Windows projeleri
Bu bölüm Windows cihazları için WinPhone81 projeleri ve Xamarin.Forms WinApp çalıştırmaya yöneliktir. Bu adımları, Evrensel Windows Platformu (UWP) projeleri de destekler. Windows cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a>Windows bildirim Hizmeti'ni (WNS) Windows uygulamanızı anında iletme bildirimleri için kaydetme
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-the-notification-hub-for-wns"></a>WNS için bildirim hub'ı yapılandırma
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-to-your-windows-app"></a>Windows uygulamanıza anında iletme bildirimleri ekleme
1. Visual Studio'da açın **App.xaml.cs** Windows proje ve aşağıdaki deyimleri ekleyin.

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Değiştir `<your_TodoItemManager_portable_class_namespace>` içeren taşınabilir projenizin ad alanıyla `TodoItemManager` sınıfı.
2. App.xaml.cs dosyasında, aşağıdaki ekleyin **Initnotificationsasync** yöntemi:

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

    Bu yöntem, anında bildirim kanalı alır ve bildirim hub'ından şablon bildirimleri almak için bir şablon kaydeder. Destekleyen bir şablon bildirim *messageParam* bu istemciye teslim edilecek.
3. App.xaml.cs dosyasında, güncelleştirme **OnLaunched** ekleyerek olay işleyici yöntemi tanımının `async` değiştiricisi. Daha sonra yönteminin sonuna aşağıdaki kod satırını ekleyin:

        await InitNotificationsAsync();

    Bu, anında iletme bildirimi kaydı oluşturulur veya uygulama her başlatıldığında yenilenir sağlar. WNS anında iletme kanal her zaman etkin olduğunu güvence altına almak için bunu yapmak önemlidir.  
4. Visual Studio için Çözüm Gezgini'nde açık **Package.appxmanifest** dosya ve ayarlayın **bildirim özellikli** için **Evet** altında **bildirimleri**.
5. Uygulamayı derleyin ve hata olmadığını doğrulayın. İstemci uygulamanızın artık Mobile Apps arka ucundan şablon bildirimler için kaydetmelisiniz. Bu bölümde, çözümünüz içinde her Windows projesi için tekrarlayın.

#### <a name="test-push-notifications-in-your-windows-app"></a>Windows uygulamanızı test anında iletme bildirimleri
1. Visual Studio'da Windows projesine sağ tıklayın ve **başlangıç projesi olarak ayarla**.
2. Projeyi oluşturmak ve uygulamayı başlatmak için **Çalıştır** düğmesine basın.
3. Uygulamasındaki yeni todoıtem için bir ad yazın ve ardından artı (**+**) eklemek için simge.
4. Öğe eklendiğinde, bir bildirim alındığında doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Anında iletme bildirimleri hakkında daha fazla bilgi edinebilirsiniz:

* [Azure Mobile Apps anında iletme bildirimleri gönderme](https://developer.xamarin.com/guides/xamarin-forms/cloud-services/push-notifications/azure/)
* [Firebase bulut Mesajlaşma](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/firebase-cloud-messaging/)
* [İleti Firebase ile uzaktan bildirimleri bulut](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/remote-notifications-with-fcm/)
* [Anında iletme bildirimi sorunlarını tanılamak](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Neden bildirimleri bırakılan veya cihazlarda bitmeyen çeşitli nedenleri vardır. Bu konuda çözümlemek ve anında iletme bildirimi hataları kök nedenini anlamak nasıl gösterilmektedir.

Ayrıca aşağıdaki öğreticiler birini açın devam edebilirsiniz:

* [Uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-xamarin-forms-get-started-users.md)  
  Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.
* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Mobile Apps arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı Eşitleme ile kullanıcılar mobil uygulama ile etkileşim kurabilen&mdash;görüntüleme, ekleme veya değiştirme veri&mdash;hiçbir ağ bağlantısı olduğunda bile.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
