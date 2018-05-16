---
title: Azure Notification Hubs kullanarak Xamarin.Android uygulamalarına anında iletme bildirimleri gönderme | Microsoft Docs
description: Bu öğreticide, bir Xamarin Android uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenirsiniz.
author: dimazaid
manager: kpiteira
editor: spelluru
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: f75671e2e5511054f3db550a8c24e62d031492c3
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-push-notifications-to-xamarinandroid-apps-using-azure-notification-hubs"></a>Öğretici: Azure Notification Hubs kullanarak Xamarin.Android uygulamalarına anında iletme bildirimleri gönderme
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, bir Xamarin.Android uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir. Firebase Cloud Messaging (FCM) kullanarak anında iletme bildirimleri alan boş bir Xamarin.Android uygulaması oluşturursunuz. Uygulamanızı çalıştıran tüm cihazlara anında iletme bildirimleri yayımlamak için bildirim hub’ınızı kullanırsınız. Tamamlanan kodu [NotificationHubs uygulaması][GitHub] örneğinde bulabilirsiniz.

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Firebase projesi oluşturma ve Firebase Cloud Messaging’i etkinleştirme
> * Bildirim hub’ı oluşturma
> * Xamarin.Android uygulaması oluşturma ve bildirim hub'ına bağlama
> * Azure portalından test bildirimleri gönderme

## <a name="prerequisites"></a>Ön koşullar

- **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
- Windows'da [Xamarin ile Visual Studio] veya OS X'te [Mac için Visual Studio].
- Etkin Google hesabı

## <a name="create-a-firebase-project-and-enable-firebase-cloud-messaging"></a>Firebase projesi oluşturma ve Firebase Cloud Messaging’i etkinleştirme
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="create-a-notification-hub"></a>Bildirim hub’ı oluşturma
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-gcm-settings-for-the-notification-hub"></a>Bildirim hub’ı için GCM ayarlarını yapılandırma

1. **BİLDİRİM AYARLARI** bölümünde **Google (GCM)** seçeneğini belirleyin. 
2. Google Firebase Console’dan not ettiğiniz **Eski sunucu anahtarını** girin. 
3. Araç çubuğunda **Kaydet**’i seçin. 

    ![](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

Bildirim hub'ınız FCM ile birlikte çalışmak üzere yapılandırıldı. Ayrıca, uygulamanızı anında iletme bildirimleri alması ve anında iletme bildirimlerini göndermesi amacıyla kaydetmenizi sağlayan bağlantı dizelerine sahipsiniz.

## <a name="create-xamainandroid-app-and-connect-it-to-notification-hub"></a>Xamain.Android uygulaması oluşturma ve bildirim hub’ına bağlama

### <a name="create-visual-studio-project-and-add-nuget-packages"></a>Visual Studio projesi oluşturma ve NuGet paketleri ekleme
1. Visual Studio’da imleci **Dosya**’nın üzerine getirin, **Yeni**’yi seçin ve sonra **Proje** seçeneğini belirleyin. 
   
      ![Visual Studio- Yeni Android projesi oluşturma](./media/partner-xamarin-notification-hubs-android-get-started/new-project-dialog.png)
2. **Çözüm Gezgini** penceresinde **Özellikler**’i genişletin ve **AndroidManifest.xml** dosyasına tıklayın. Paket adını, Google Firebase Console’da projenize Firebase Cloud Messaging eklerken girdiğiniz paket adıyla eşleşecek şekilde güncelleştirin. 

      ![GCM’de paket adı](./media/partner-xamarin-notification-hubs-android-get-started/package-name-gcm.png)
3. Projeye sağ tıklayın ve **NuGet Paketlerini Yönet...** seçeneğini belirleyin. 
4. **Gözat** sekmesini seçin. **Xamarin.GooglePlayServices.Base** öğesini arayın. Sonuç listesinden **Xamarin.GooglePlayServices.Base** öğesini seçin. Ardından **Yükle**’yi seçin. 

      ![Google Play Services NuGet](./media/partner-xamarin-notification-hubs-android-get-started/google-play-services-nuget.png)
5. **NuGet Package Manager** penceresinde **Xamarin.Firebase.Messaging** öğesini arayın. Sonuç listesinden **Xamarin.Firebase.Messaging** öğesini seçin. Ardından **Yükle**’yi seçin. 
6. Şimdi **Xamarin.Azure.NotificationHubs.Android** araması yapın. Sonuç listesinden **Xamarin.Azure.NotificationHubs.Android** öğesini seçin. Ardından **Yükle**’yi seçin. 

### <a name="add-the-google-services-json-file"></a>Google Services JSON Dosyasını ekleme

1. Google Firebase Console’dan indirdiğiniz **google-services.json** öğesini proje klasörüne kopyalayın.
2. **google-services.json** öğesini projeye ekleyin.
3. **Çözüm Gezgini** penceresinde **google-services.json** öğesini seçin.
4. **Özellikler** bölmesinde, Derleme Eylemini **GoogleServicesJson** olarak ayarlayın. **GoogleServicesJson** öğesini görmezseniz, Visual Studio’yu kapatın, yeniden başlatın, projeyi yeniden açın ve yeniden deneyin. 

      ![GoogleServicesJson derleme eylemi](./media/partner-xamarin-notification-hubs-android-get-started/google-services-json-build-action.png)

### <a name="set-up-notification-hubs-in-your-project"></a>Projenizdeki bildirim hub'larını ayarlama

#### <a name="registering-with-firebase-cloud-messaging"></a>Firebase Cloud Messaging ile kaydolma

**AndroidManifest.xml** dosyasını açın ve aşağıdaki `<receiver>` öğelerini `<application>` öğesine ekleyin:

        <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
        <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
          <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="${applicationId}" />
          </intent-filter>
        </receiver>

1. Android uygulamanız ve bildirim hub'ınız için aşağıdaki bilgileri toplayın:
   
   * **Dinleme bağlantı dizesi**: [Azure portalındaki] panoda **Bağlantı dizelerini görüntüle**'yi seçin. Bu değer için *DefaultListenSharedAccessSignature* bağlantı dizesini kopyalayın.
   * **Hub adı**: [Azure portalındaki] hub’ınızın adı. Örneğin, *mynotificationhub2*.
     
2. **Çözüm Gezgini** penceresinde **projenize** sağ tıklayın, **Ekle**’nin üzerine gelip **Sınıf**’ı seçin. 
4. Xamarin projeniz için bir **Constants.cs** sınıfı oluşturun ve bu sınıfta aşağıdaki sabit değerleri tanımlayın. Yer tutucuları değerleriniz ile değiştirin.
    
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
7. **Constants** sınıfında oluşturduğunuz gibi yeni bir sınıf (**MyFirebaseIIDService**) oluşturun. 
8. **MyFirebaseIIDService.cs** içine aşağıdaki deyimleri ekleyin:
   
    ```csharp
    using Android.Util;
    using WindowsAzure.Messaging;
    using Firebase.Iid;
    ```

9. **MyFirebaseIIDService.cs** içine aşağıdaki **class** bildirimini ekleyin ve sınıfınızın **FirebaseInstanceIdService** kaynağından devralmasını sağlayın:
   
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
15. Projenizi **derleyin**. 
16. Uygulamayı cihazınızda veya yüklü öykünücüde **çalıştırma**

## <a name="send-test-notification-from-the-azure-portal"></a>Azure portalından test bildirimi gönderme
[Azure portalındaki] *Test Gönderimi* seçeneğini kullanarak uygulamanızda bildirim alma testi gerçekleştirebilirsiniz. Bu, cihazınıza test amaçlı anında iletme bildirimi gönderir.

![Azure portalı - Test Gönderimi](media/partner-xamarin-notification-hubs-android-get-started/send-test-notification.png)

Anında iletme bildirimleri normalde, uyumlu bir kitaplık aracılığıyla Mobile Services veya ASP.NET gibi bir arka uç hizmetinde gönderilir. Arka ucunuz için uygun bir kitaplık yoksa bildirim iletileri göndermek için doğrudan REST API de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, arka uca kayıtlı olan tüm Android cihazlarınıza yayın bildirimleri gönderdiniz. Belirli Android cihazlara nasıl anında iletme bildirimleri gönderileceğini öğrenmek için aşağıdaki öğreticiye ilerleyin: 

> [!div class="nextstepaction"]
>[Belirli cihazlara anında iletme bildirimleri gönderme](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)


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
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Use Notification Hubs to push notifications to users]: /manage/services/notification-hubs/notify-users-aspnet
[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet
[GitHub]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid
