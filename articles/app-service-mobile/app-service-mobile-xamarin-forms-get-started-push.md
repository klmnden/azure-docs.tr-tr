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
ms.openlocfilehash: 124c36063482aa3b36844104c0b83b8a6e9598cb
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
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

1. İçinde **Droid** projesinde **bileşenleri** klasörü ve tıklatın **daha almak bileşenleri...** . İçin arama **Google Cloud Messaging istemcisi** bileşeni ve bunu projeye ekleyin. Bu bileşen, bir Xamarin Android projesi için anında iletme bildirimleri destekler.
2. MainActivity.cs proje dosyasını açın ve dosyanın üst kısmında aşağıdaki deyimini ekleyin:

        using Gcm.Client;
3. Aşağıdaki kodu ekleyin **OnCreate** yöntemini çağırdıktan sonra **LoadApplication**:

        try
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }
4. Yeni bir ekleme **CreateAndShowDialog** yardımcı yöntemi, aşağıdaki gibi:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }
5. Aşağıdaki kodu ekleyin **MainActivity** sınıfı:

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    Bu geçerli sunan **MainActivity** örnek biz ana kullanıcı Arabirimi iş parçacığı üzerinde çalıştırabilirsiniz.
6. Initialize `instance` başındaki değişken **OnCreate** şekilde yöntemi.

        // Set the current instance of MainActivity.
        instance = this;
7. Yeni bir sınıf dosyası ekleyin **Droid** adlı projesi `GcmService.cs`ve aşağıdakilerden emin olun **kullanarak** deyimleri mevcut dosyanın üst kısmında:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;
8. Dosyanın üst aşağıdaki izin isteklerini ekleyin **kullanarak** deyimleri ve önce **ad alanı** bildirimi.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
9. Aşağıdaki sınıf tanımını ad alanına ekleyin.

       [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
       public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
       {
           public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
       }

   > [!NOTE]
   > Değiştir **< PROJECT_NUMBER >** not ettiğiniz daha önce proje numarası ile.    
   >
   >
10. Boş Değiştir **GcmService** yeni yayın alıcı kullanır aşağıdaki kod ile sınıfı:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }
11. Aşağıdaki kodu ekleyin **GcmService** sınıfı. Bu geçersiz kılmaları **OnRegistered** olay işleyicisi ve uygulayan bir **kaydetmek** yöntemi.

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

    Bu kodu kullanır Not `messageParam` şablonu kayıt parametresi.
12. Uygulayan aşağıdaki kodu ekleyin **Onmessageoptions**:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    Bu gelen bildirimlerini işleme ve görüntülenecek bildirim Yöneticisi gönderir.
13. **GcmServiceBase** uygulamak gerektiren **OnUnRegistered** ve **OnError** şu şekilde yapabilirsiniz işleyici yöntemleri:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

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
