---
title: "Xamarin.Android uygulamaları için Notification Hubs&quot;ı kullanmaya başlama | Microsoft Belgeleri"
description: "Bu öğreticide, bir Xamarin Android uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs&quot;ın nasıl kullanılacağını öğrenirsiniz."
author: ysxu
manager: erikre
editor: 
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5137e0f33497dfe7ee815bb4bc30929364f6df72


---
# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Android için Xamarin ile Notification Hubs'ı kullanmaya başlama
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, bir Xamarin.Android uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir.
Google Cloud Messaging (GCM) kullanarak anında iletme bildirimleri alan boş bir Xamarin.Android uygulaması oluşturacaksınız. İşiniz bittiğinde, uygulamanızı çalıştıran tüm cihazlara anında iletme bildirimleri yayımlamak için bildirim hub'ınızı kullanabileceksiniz. Tamamlanan kodu [NotificationHubs uygulaması][GitHub] örneğinde bulabilirsiniz.

Bu öğretici Notification Hubs kullanımında basit yayın senaryosunu gösterir.

## <a name="before-you-begin"></a>Başlamadan önce
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Bu öğreticinin tamamlanan kodu GitHub'da [buradan](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid) bulunabilir.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici için aşağıdakiler gereklidir:

* Windows'ta Xamarin ile Visual Studio veya Mac OS X'te Xamarin Studio. Tam yükleme yönergeleri [Visual Studio ve Xamarin için Kurulum ve Yükleme](https://msdn.microsoft.com/library/mt613162.aspx) sayfasında bulunabilir.
* Etkin Google hesabı
* [Azure Mesajlaşma Bileşeni]
* [Google Cloud Messaging İstemci Bileşeni]

Bu öğreticiyi tamamlamak Xamarin.Android uygulamalarına ilişkin diğer tüm Notification Hubs öğreticileri için önkoşuldur.

> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).
> 
> 

## <a name="enable-google-cloud-messaging"></a>Google Cloud Messaging'i etkinleştirme
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a>Bildirim hub'ınızı yapılandırma
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li><p>Üst kısımdaki <b>Yapılandır</b> sekmesine tıklayın, önceki bölümde edindiğiniz <b>API Anahtarı</b> değerini girin ve ardından <b>Kaydet</b>'e tıklayın.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)

Bildirim hub'ınız şimdi GCM ile birlikte çalışmak üzere yapılandırıldı. Ayrıca, uygulamanızı anında iletme bildirimleri alması ve anında iletme bildirimlerini göndermesi amacıyla kaydetmenizi sağlayan bağlantı dizelerine sahipsiniz.

## <a name="connect-your-app-to-the-notification-hub"></a>Uygulamanızı bildirim hub'ına bağlama
### <a name="create-a-new-project"></a>Yeni bir proje oluşturma
1. Xamarin Studio'da **New Solution**'a (Yeni Çözüm), **Android App **'e (Android Uygulaması) ve **Next**'e (İleri) tıklayın.
   
       ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)
2. **App Name**'i (Uygulama Adı) ve **Identifier**'ı (Tanımlayıcı) girin. Desteklemek istediğiniz **Target Plaforms**'a (Hedef Platformlar) tıklayın. Ardından, **Next** (İleri) ve **Create** (Oluştur) seçeneklerine tıklayın.
   
       ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)

    Bu, yeni bir Android projesi oluşturur.

1. Çözüm görünümünde yeni projenize sağ tıklayarak ve **Options**'ı (Seçenekler) seçerek proje özelliklerini açın. **Build** (Derleme) bölümünde **Android Application** (Android Uygulaması) öğesini seçin.
   
    **Package name**'in (Paket adı) ilk harfinin küçük harf olduğundan emin olun.
   
   > [!IMPORTANT]
   > Paket adının ilk harfi küçük olmalıdır. Aksi durumda, aşağıdaki anında iletme bildirimleri için **BroadcastReceiver** ve **IntentFilter** öğelerinizi kaydettiğinizde uygulama bildirim hataları alırsınız.
   > 
   > 
   
       ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)
2. İsteğe bağlı olarak, **Minimum Android version**'ı (Minimum Android sürümü) başka bir API Düzeyine ayarlayın.
3. İsteğe bağlı olarak, **Target Android version**'ı (Hedef Android sürümü) hedeflemek istediğiniz başka bir API sürümüne ayarlayın (API düzeyi 8 veya üzeri olmalıdır).

**Tamam**'a tıklayın ve Project Options (Proje Seçenekleri) iletişim kutusunu kapatın.

### <a name="add-the-required-components-to-your-project"></a>Projenize gerekli bileşenleri ekleme
Xamarin Bileşen Deposu'nda bulunan Google Cloud Messaging İstemcisi, Xamarin.Android'de anında iletme bildirimlerini destekleme işlemini basitleştirir.

1. Xamarin.Android uygulamasındaki Components (Bileşenler) klasörüne sağ tıklayın ve **Get More Components** (Daha Fazla Bileşen Al) seçeneğini belirleyin.
2. **Azure Mesajlaşma** bileşeni için arama yapın ve bunu projeye ekleyin.
3. **Google Cloud Messaging İstemcisi** bileşeni için arama yapın ve bunu projeye ekleyin.

### <a name="set-up-notification-hubs-in-your-project"></a>Projenizdeki bildirim hub'larını ayarlama
1. Android uygulamanız ve bildirim hub'ınız için aşağıdaki bilgileri toplayın:
   
   * **GoogleProjectNumber**: Google Developer Portal'daki uygulamanıza genel bakış bölümünden bu Proje Numarası değerini alın. Daha önce portalda uygulamayı oluştururken bu değeri not etmiştiniz.
   * **Dinleme bağlantı dizesi**: [Klasik Azure Portalı]'ndaki panoda **Bağlantı dizelerini görüntüle**'ye tıklayın. Bu değer için *DefaultListenSharedAccessSignature* bağlantı dizesini kopyalayın.
   * **Hub adı**: Bu, [Klasik Azure Portalı] hub'ınızın adıdır. Örneğin, *mynotificationhub2*.
     
     Xamarin projeniz için bir **Constants.cs** sınıfı oluşturun ve bu sınıfta aşağıdaki sabit değerleri tanımlayın. Yer tutucuları değerleriniz ile değiştirin.
     
       public static class Constants   {
     
           public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
       }
2. **MainActivity.cs**'ye aşağıdaki using deyimlerini ekleyin:
   
        using Android.Util;
        using Gcm.Client;
3. Uygulama çalışırken uyarı iletişim kutusu göstermek için kullanılacak `MainActivity` sınıfına bir örnek değişkeni ekleyin:
   
        public static MainActivity instance;
4. **MainActivity** sınıfında aşağıdaki yöntemi oluşturun:
   
        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);
   
            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }
5. **MainActivity.cs**'nin `OnCreate` yönteminde `instance` değişkenini başlatın ve `RegisterWithGCM` öğesine bir çağrı ekleyin:
   
        protected override void OnCreate (Bundle bundle)
        {
            instance = this;
   
            base.OnCreate (bundle);
   
            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);
   
            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);
   
            RegisterWithGCM();
        }
6. **MyBroadcastReceiver** adlı yeni bir sınıf oluşturun.
   
   > [!NOTE]
   > Aşağıda, sıfırdan bir **BroadcastReceiver** sınıfı oluşturma konusunda size yol göstereceğiz. Ancak manuel olarak **MyBroadcastReceiver.cs** oluşturmanın hızlı bir alternatifi, [NotificationHubs örnekleri][GitHub] içindeki örnek Xamarin.Android projesinde bulunan **GcmService.cs** dosyasına başvurmaktır. **GcmService.cs**'yi yinelemek ve sınıf adlarını değiştirmek de iyi bir başlangıç noktası olabilir.
   > 
   > 
7. **MyBroadcastReceiver.cs**'ye aşağıdaki using deyimlerini ekleyin (daha önce eklediğiniz bileşen ve derlemeye başvuran):
   
        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;
8. **MyBroadcastReceiver.cs**'de, **using** deyimleri ve **namespace** bildirimi arasına aşağıdaki izin isteklerini ekleyin:
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
9. **MyBroadcastReceiver.cs**'de, **MyBroadcastReceiver** sınıfını aşağıdaki ile eşleşecek şekilde değiştirin:
   
        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };
   
            public const string TAG = "MyBroadcastReceiver-GCM";
        }
10. **MyBroadcastReceiver.cs**'ye **GcmServiceBase**'den türetilen **PushHandlerService** adlı başka bir sınıf ekleyin. Bu sınıfa **Service** özniteliğini uyguladığınızdan emin olun:
    
         [Service] // Must use the service tag
         public class PushHandlerService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }
             private NotificationHub Hub { get; set; }
    
             public PushHandlerService() : base(Constants.SenderID)
                {
                 Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
             }
         }
11. **GcmServiceBase**; **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()** ve **OnError()** yöntemlerini uygular. **PushHandlerService** uygulama sınıfımızın bu yöntemleri geçersiz kılması gerekir ve bu yöntemler, bildirim hub'ı ile etkileşim kurulmasına yanıt olarak tetiklenir.
12. Aşağıdaki kodu kullanarak **PushHandlerService**'da **OnRegistered()** yöntemini geçersiz kılın:
    
         protected override void OnRegistered(Context context, string registrationId)
         {
             Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
             RegistrationID = registrationId;
    
             createNotification("PushHandlerService-GCM Registered...",
                                 "The device has been Registered!");
    
             Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                         context);
             try
             {
                 Hub.UnregisterAll(registrationId);
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
    
             //var tags = new List<string>() { "falcons" }; // create tags if you want
             var tags = new List<string>() {};
    
             try
             {
                 var hubRegistration = Hub.Register(registrationId, tags.ToArray());
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
         }
    
    > [!NOTE]
    > Yukarıdaki **OnRegistered()** kodunda, belirli mesajlaşma kanallarına kaydolmak için etiketler belirtme özelliğine dikkat etmeniz gerekir.
    > 
    > 
13. Aşağıdaki kodu kullanarak **PushHandlerService**'da **OnMessage** yöntemini geçersiz kılın:
    
        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");
    
            var msg = new StringBuilder();
    
            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }
    
            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }
14. Bildirim alındığında kullanıcıları bilgilendirmek için **PushHandlerService**'a **createNotification** ve **dialogNotify** yöntemlerini ekleyin.
    
    > [!NOTE]
    > Android sürüm 5.0 ve sonrasındaki bildirim tasarımı önceki sürümlerden önemli ölçüde farklıdır. Bunu Android 5.0 veya daha sonraki sürümlerinde test ederseniz bildirim almak için uygulamanın çalışıyor olması gerekir. Daha fazla bilgi için bkz. [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880) (Android Bildirimleri).
    > 
    > 
    
        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;
    
            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));
    
            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);
    
            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;
    
            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));
    
            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }
    
        protected void dialogNotify(String title, String message)
        {
    
            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }
15. **OnUnRegistered()**, **OnRecoverableError()** ve **OnError()** soyut üyelerini geçersiz kılarak kodunuzu derleyin:
    
        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);
    
            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }
    
        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);
    
            return base.OnRecoverableError (context, errorId);
        }
    
        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }

## <a name="run-your-app-in-the-emulator"></a>Uygulamanızı öykünücüde çalıştırma
Bu uygulamayı öykünücüde çalıştırırsanız Google API'lerini destekleyen bir Android Sanal Cihaz ( AVD) kullandığınızdan emin olun.

> [!IMPORTANT]
> Anında iletme bildirimleri almak için Android Sanal Cihazınızda bir Google hesabı ayarlamanız gerekir. (Öykünücüde **Settings**'e (Ayarlar) gidin ve **Add Account**'a (Hesap Ekle) tıklayın.) Ayrıca, öykünücünün İnternet'e bağlı olduğundan emin olun.
> 
> [!NOTE]
> Android sürüm 5.0 ve sonrasındaki bildirim tasarımı önceki sürümlerden önemli ölçüde farklıdır. Daha fazla bilgi için bkz. [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880) (Android Bildirimleri).
> 
> 

1. **Tools**'da (Araçlar), **Open Android Emulator Manager** (Android Öykünücüsü Yöneticisini Aç) seçeneğine tıklayın, cihazınızı açın ve ardından **Edit** (Düzenle) seçeneğine tıklayın.
   
       ![][18]
2. **Target** (Hedef) içinde **Google APIs** (Google API'leri) seçeneğini belirleyin ve ardından **Tamam**'a tıklayın.
   
       ![][19]
3. Üst araç çubuğunda **Run**'a (Çalıştır) tıklayın ve ardından uygulamanızı seçin. Bu, öykünücüyü başlatır ve uygulamayı çalıştırır.
   
   Uygulama GCM'den *registrationId* öğesini alır ve bildirim hub'ına kaydeder.

## <a name="send-notifications-from-your-backend"></a>Arka ucunuzdan bildirim gönderme
Aşağıdaki ekranda gösterildiği gibi, bildirim hub'ındaki hata ayıklama sekmesi aracılığıyla [Klasik Azure Portalı]'nda bildirim göndererek uygulamanızda bildirim almayı test edebilirsiniz.

![][30]

Anında iletme bildirimleri normalde, uyumlu bir kitaplık aracılığıyla Mobile Services veya ASP.NET gibi bir arka uç hizmetinde gönderilir. Arka ucunuz için kullanılabilir bir kitaplık yoksa bildirim iletilerini göndermek üzere doğrudan REST API de kullanabilirsiniz.

Burada, bildirim göndermek için gözden geçirmek isteyebileceğiniz diğer bazı öğreticilerin bir listesi vardır:

* ASP.NET: Bkz. [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma].
* Azure Notification Hubs Java SDK'sı: Java'dan bildirim göndermek için bkz. [Java'dan Notification Hubs kullanma](notification-hubs-java-push-notification-tutorial.md). Android Geliştirmesi için Eclipse'te test edilmiştir.
* PHP: [PHP'den Notification Hubs kullanma](notification-hubs-php-push-notification-tutorial.md).

Öğreticinin sonraki alt bölümlerinde, .NET konsol uygulaması kullanarak ve bir node betiği aracılığıyla mobil hizmet kullanarak bildirim gönderirsiniz.

#### <a name="optional-send-notifications-by-using-a-net-app"></a>(İsteğe bağlı) .NET uygulaması kullanarak bildirim gönderme
Bu bölümde, .NET konsol uygulaması kullanarak bildirim göndereceğiz

1. Yeni bir Visual C# konsol uygulaması oluşturun:
   
       ![][20]
2. Visual Studio'da **Araçlar**'a, **NuGet Paket Yöneticisi**'ne ve ardından **Paket Yöneticisi Konsolu**'na tıklayın.
   
    Bu, Visual Studio'da Paket Yöneticisi Konsolu'nu görüntüler.
3. Paket Yöneticisi Konsolu penceresinde, **Varsayılan projeyi** yeni konsol uygulaması projeniz olarak ayarlayın ve ardından konsol penceresinde aşağıdaki komutu yürütün:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Bu, <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet paketini</a> kullanarak Azure Notification Hubs SDK'sına bir başvuru ekler.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Program.cs dosyasını açın ve aşağıdaki `using` deyimini ekleyin:
   
        using Microsoft.Azure.NotificationHubs;
5. `Program` sınıfınıza aşağıdaki yöntemi ekleyin. [Klasik Azure Portalı]'ndaki *DefaultFullSharedAccessSignature* bağlantı dizenizi ve hub adınızı kullanarak yer tutucu metnini güncelleştirin.
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }
6. Aşağıdaki satırları **Main** yönteminize ekleyin:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Uygulamayı çalıştırmak için F5 tuşuna basın. Uygulamada bir bildirim almanız gerekir.
   
       ![][21]

#### <a name="optional-send-notifications-by-using-a-mobile-service"></a>(İsteğe bağlı) Mobil hizmet kullanarak bildirim gönderme
1. [Mobile Services'i kullanmaya başlama]'yı izleyin.
2. [Klasik Azure Portalı]'nda oturum açın ve mobil hizmetinizi seçin.
3. Üst kısımdaki **Scheduler** sekmesini seçin.
   
       ![][22]
4. Yeni bir zamanlanan iş oluşturun, ad ekleyin ve **İsteğe bağlı**'yı seçin.
   
       ![][23]
5. İş oluşturulduğunda iş adına tıklayın. Ardından, üst çubukta **Betik** sekmesine tıklayın.
6. Zamanlayıcı işlevinizin içine aşağıdaki betiği ekleyin. Yer tutucularını daha önce edindiğiniz bildirim hub'ı adınız ve *DefaultFullSharedAccessSignature* bağlantı dizeniz ile değiştirdiğinizden emin olun. **Kaydet** düğmesine tıklayın.
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );
7. Alt çubukta **Bir Kez Çalıştır**'a tıklayın. Bir bildirim almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Bu basit örnekte, tüm Android cihazlarınıza bildirimler yayımladınız. Belirli kullanıcıları hedeflemek için, [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma] öğreticisine bakın. Kullanıcılarınızı ilgi alanı gruplarına göre segmentlere ayırmak istiyorsanız [Son dakika haberleri göndermek için Notification Hubs kullanma]'yı okuyabilirsiniz. [Notification Hubs Kılavuzu] ve [Android İçin Notification Hubs'ı Kullanma]'da Notification Hubs'ı kullanma hakkında daha fazla bilgi edinin.

<!-- Anchors. -->
[Google Cloud Messaging'i etkinleştirme]: #register
[Bildirim Hub'ınızı yapılandırma]: #configure-hub
[Uygulamanızı Bildirim Hub'ına bağlama]: #connecting-app
[Uygulamanızı öykünücü ile çalıştırma]: #run-app
[Arka ucunuzdan bildirim gönderme]: #send
[Sonraki adımlar]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Uygulama gönderme sayfası]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[Uygulamalarım]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Windows için Live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Mobile Services'i kullanmaya başlama]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript ve HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Klasik Azure Portalı]: https://manage.windowsazure.com/
[wns nesnesi]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Kılavuzu]: http://msdn.microsoft.com/library/jj927170.aspx
[Android İçin Notification Hubs'ı Kullanma]: http://msdn.microsoft.com/library/dn282661.aspx

[Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma]: /manage/services/notification-hubs/notify-users-aspnet
[Son dakika haberleri göndermek için Notification Hubs kullanma]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Bileşen sayfası]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub sayfası]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Google Cloud Messaging İstemci Bileşeni]: http://components.xamarin.com/view/GCMClient/
[Azure Mesajlaşma Bileşeni]: http://components.xamarin.com/view/azure-messaging



<!--HONumber=Nov16_HO2-->


