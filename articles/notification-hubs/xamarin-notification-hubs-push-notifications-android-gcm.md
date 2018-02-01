---
title: "Xamarin.Android uygulamaları için Notification Hubs'ı kullanmaya başlama | Microsoft Belgeleri"
description: "Bu öğreticide, bir Xamarin Android uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenirsiniz."
author: jwhitedev
manager: kpiteira
editor: 
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 1cb6fbc82c493e17815dc60ddcff183a47513bc6
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="get-started-with-notification-hubs-for-xamarinandroid-apps"></a>Xamarin.Android uygulamaları için Notification Hubs'ı kullanmaya başlama
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, bir Xamarin.Android uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir. Firebase Cloud Messaging (FCM) kullanarak anında iletme bildirimleri alan bir Xamarin.Android uygulaması oluşturacaksınız. İşiniz bittiğinde, uygulamanızı çalıştıran tüm cihazlara anında iletme bildirimleri yayımlamak için bildirim hub'ınızı kullanabileceksiniz. Tamamlanan kodu [NotificationHubs uygulaması][GitHub] örneğinde bulabilirsiniz.

Bu öğretici Notification Hubs kullanımında basit yayın senaryosunu gösterir.

## <a name="before-you-begin"></a>Başlamadan önce
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Bu öğreticinin tamamlanan kodu GitHub'da [buradan][GitHub] bulunabilir.

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici için aşağıdakiler gereklidir:

* Windows'da [Xamarin ile Visual Studio] veya OS X'te [Mac için Visual Studio].
* Etkin Google hesabı

Bu öğreticiyi tamamlamak Xamarin.Android uygulamalarına ilişkin diğer tüm Notification Hubs öğreticileri için önkoşuldur.

> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).
> 
> 

## <a name="enable-firebase-cloud-messaging"></a>Firebase Cloud Messaging'i etkinleştirme
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a>Bildirim hub'ınızı yapılandırma
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li><p>Üst kısımdaki <b>Yapılandır</b> sekmesini seçin, önceki bölümde edindiğiniz <b>API Anahtarı</b> değerini girin ve ardından <b>Kaydet</b>'i seçin.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

Bildirim hub'ınız FCM ile birlikte çalışmak üzere yapılandırıldı. Ayrıca, uygulamanızı anında iletme bildirimleri alması ve anında iletme bildirimlerini göndermesi amacıyla kaydetmenizi sağlayan bağlantı dizelerine sahipsiniz.

## <a name="connect-your-app-to-the-notification-hub"></a>Uygulamanızı bildirim hub'ına bağlama
Önce yeni bir proje oluşturun. 

1. Visual Studio'da **Yeni Çözüm** > **Android Uygulaması** yolunu izleyin ve **İleri**'ye tıklayın.
   
      ![Visual Studio- Yeni Android projesi oluşturma][22]

2. **App Name**'i (Uygulama Adı) ve **Identifier**'ı (Tanımlayıcı) girin. Desteklemek istediğiniz **Hedef Platformlar**'u seçin. Ardından, **İleri** ve **Oluştur** seçeneklerini belirleyin.
   
      ![Visual Studio- Android uygulaması yapılandırması][23]

    Bu, yeni bir Android projesi oluşturur.

3. Çözüm görünümünde yeni projenize sağ tıklayarak ve **Options**'ı (Seçenekler) seçerek proje özelliklerini açın. **Build** (Derleme) bölümünde **Android Application** (Android Uygulaması) öğesini seçin.
   
    **Package name**'in (Paket adı) ilk harfinin küçük harf olduğundan emin olun.
   
   > [!IMPORTANT]
   > Paket adının ilk harfi küçük olmalıdır. Aksi durumda, aşağıdaki anında iletme bildirimleri için uygulamanızı kaydettiğinizde uygulama bildirim hataları alırsınız.
   > 
   > 
   
      ![Visual Studio- Android projesi seçenekleri][24]
4. İsteğe bağlı olarak, **Minimum Android version**'ı (Minimum Android sürümü) başka bir API Düzeyine ayarlayın.
5. İsteğe bağlı olarak, **Hedef Android sürümü**'nü hedeflemek istediğiniz başka bir API sürümüne ayarlayın (API düzeyi 8 veya üzeri olmalıdır).
6. **Tamam**'ı seçin ve Project Options (Proje Seçenekleri) iletişim kutusunu kapatın.

### <a name="add-the-required-packages-to-your-project"></a>Gerekli paketlerini projenize ekleme

1. Projenize sağ tıklayıp **Ekle** > **NuGet Paketleri Ekle**'yi seçin
2. **Xamarin.Azure.NotificationHubs.Android** araması yapıp projeye ekleyin.
3. **Xamarin.Firebase.Messaging** girişini seçip projeye ekleyin.

### <a name="set-up-notification-hubs-in-your-project"></a>Projenizdeki bildirim hub'larını ayarlama
1. Android uygulamanız ve bildirim hub'ınız için aşağıdaki bilgileri toplayın:
   
   * **Dinleme bağlantı dizesi**: [Azure portalındaki] panoda **Bağlantı dizelerini görüntüle**'yi seçin. Bu değer için *DefaultListenSharedAccessSignature* bağlantı dizesini kopyalayın.
   * **Hub adı**: Bu, [Azure portalındaki] hub'ınızın adıdır. Örneğin, *mynotificationhub2*.
     
2. Xamarin projeniz için bir **Constants.cs** sınıfı oluşturun ve bu sınıfta aşağıdaki sabit değerleri tanımlayın. Yer tutucuları değerleriniz ile değiştirin.
    
    ```csharp
        public static class Constants
        {
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
        }
    ```

3. **MainActivity.cs**'ye aşağıdaki using deyimlerini ekleyin:
   
    ```csharp
        using Android.Util;
    ```

4. Uygulama çalışırken uyarı iletişim kutusu göstermek için kullanılacak **MainActivity.cs** dosyasına bir örnek değişkeni ekleyin:
   
    ```csharp
        public const string TAG = "MainActivity";
    ```

5. **MainActivity.cs** içinde `base.OnCreate(savedInstanceState)` sonrasındaki `OnCreate` içine aşağıdaki kodu ekleyin:

    ```csharp   
        if (Intent.Extras != null)
        {
            foreach (var key in Intent.Extras.KeySet())
            {
                if(key!=null)
                {
                    var value = Intent.Extras.GetString(key);
                    Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
                }
            }
        }
    ```

6. Projenize sağ tıklayıp daha önceden Firebase projenizden indirdiğiniz `google-services.json` öğesini ekleyin. Eklenen dosyaya sağ tıklayıp derleme eylemini `GoogleServicesJson` olarak ayarlayın

    ![Visual Studio- google-services.json yapılandırması][25]

7. **MyFirebaseIIDService** adlı yeni bir sınıf oluşturun.

8. **MyFirebaseIIDService.cs** içine aşağıdaki deyimleri ekleyin:
   
    ```csharp
        using System;
        using Android.App;
        using Firebase.Iid;
        using Android.Util;
        using WindowsAzure.Messaging;
        using System.Collections.Generic;
    ```

9. **MyFirebaseIIDService.cs** içine **class** bildiriminin üzerine aşağıdaki kodu ekleyin ve sınıfınızın **FirebaseInstanceIdService** kaynağından devralmasını sağlayın:
   
    ```csharp
        [Service]
        [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
        public class MyFirebaseIIDService : FirebaseInstanceIdService
    ```

10. **MyFirebaseIIDService.cs** içine aşağıdaki kodu ekleyin:
   
    ```csharp
        const string TAG = "MyFirebaseIIDService";
        NotificationHub hub;

        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "FCM token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }

        void SendRegistrationToServer(string token)
        {
            // Register with Notification Hubs
            hub = new NotificationHub(Constants.NotificationHubName,
                                      Constants.ListenConnectionString, this);

            var tags = new List<string>() { };
            var regID = hub.Register(token, tags.ToArray()).RegistrationId;

            Log.Debug(TAG, $"Successful registration of ID {regID}");
        }
    ```

11. Projeniz için yeni bir sınıf oluşturun ve **MyFirebaseMessagingService** olarak adlandırın.

12. Aşağıdaki deyimleri **MyFirebaseMessagingService.cs** içine ekleyin.
    
    ```csharp
        using System;
        using System.Linq;
        using Android;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Firebase.Messaging;
    ```

13. Aşağıdaki kodu sınıf bildiriminizin üzerine ekleyin ve sınıfınızın **FirebaseMessagingService** kaynağından devralmasını sağlayın:
    
    ```csharp
        [Service]
        [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
        public class MyFirebaseMessagingService : FirebaseMessagingService
    ```
    
14. Aşağıdaki kodu **MyFirebaseMessagingService.cs** içine ekleyin:
    
    ```csharp
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            if(message.GetNotification()!= null)
            {
                //These is how most messages will be received
                Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
                SendNotification(message.GetNotification().Body);
            }
            else 
            {
                //Only used for debugging payloads sent from the Azure portal
                SendNotification(message.Data.Values.First());

            }

        }

        void SendNotification(string messageBody)
        {
            var intent = new Intent(this, typeof(MainActivity));
            intent.AddFlags(ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new Notification.Builder(this)
                        .SetContentTitle("FCM Message")
                        .SetSmallIcon(Resource.Drawable.ic_launcher)
                        .SetContentText(messageBody)
                        .SetAutoCancel(true)
                        .SetContentIntent(pendingIntent);

            var notificationManager = NotificationManager.FromContext(this);

            notificationManager.Notify(0, notificationBuilder.Build());
        }
    ```

15. Uygulamayı cihazınızda veya yüklü öykünücüde çalıştırma

## <a name="send-notifications-from-the-portal"></a>Portaldan bildirim gönderme
[Azure portalındaki] *Test Gönderimi* seçeneğini kullanarak uygulamanızda bildirim alma testi gerçekleştirebilirsiniz. Bu seçenek, cihazınıza test amaçlı anında iletme bildirimi gönderir.

![Azure portalı - Test Gönderimi][30]

Anında iletme bildirimleri normalde, uyumlu bir kitaplık aracılığıyla Mobile Services veya ASP.NET gibi bir arka uç hizmetinde gönderilir. Arka ucunuz için uygun bir kitaplık yoksa bildirim iletilerini göndermek için doğrudan REST API de kullanabilirsiniz.

Burada, bildirim göndermek için gözden geçirmek isteyebileceğiniz diğer bazı öğreticilerin bir listesi vardır:

* ASP.NET: Bkz. [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma].
* Azure Notification Hubs Java SDK'sı: Java'dan bildirim göndermek için bkz. [Java'dan Notification Hubs kullanma](notification-hubs-java-push-notification-tutorial.md). Android Geliştirmesi için Eclipse'te test edilmiştir.
* PHP: [PHP'den Notification Hubs kullanma](notification-hubs-php-push-notification-tutorial.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu basit örnekte, tüm Android cihazlarınıza bildirimler yayımladınız. Belirli kullanıcıları hedeflemek için, [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma] öğreticisine bakın. Kullanıcılarınızı ilgi alanı gruplarına göre segmentlere ayırmak istiyorsanız [Son dakika haberleri göndermek için Notification Hubs kullanma]'yı okuyabilirsiniz. [Notification Hubs Kılavuzu] ve [Android İçin Notification Hubs'ı Kullanma]'da Notification Hubs'ı kullanma hakkında daha fazla bilgi edinin.

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png
[24]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-xamarin-android-app-options.png
[25]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-google-services-json.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-test-send.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js
[Xamarin ile Visual Studio]: https://docs.microsoft.com/visualstudio/install/install-visual-studio
[Mac için Visual Studio]: https://www.visualstudio.com/vs/visual-studio-mac/

[Azure portalındaki]: https://portal.azure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Kılavuzu]: http://msdn.microsoft.com/library/jj927170.aspx
[Android İçin Notification Hubs'ı Kullanma]: http://msdn.microsoft.com/library/dn282661.aspx

[Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma]: /manage/services/notification-hubs/notify-users-aspnet
[Son dakika haberleri göndermek için Notification Hubs kullanma]: /manage/services/notification-hubs/breaking-news-dotnet
[GitHub]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid
