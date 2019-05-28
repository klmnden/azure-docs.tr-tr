---
title: Azure Notification Hubs kullanarak Xamarin.Android uygulamalarına anında iletme bildirimleri gönderme | Microsoft Docs
description: Bu öğreticide, bir Xamarin Android uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenirsiniz.
author: jwargo
manager: patniko
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
ms.date: 05/01/2019
ms.author: jowargo
ms.openlocfilehash: 09e5f5526c2d6953c574a7d7dd2425159ad88307
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240718"
---
# <a name="tutorial-push-notifications-to-xamarinandroid-apps-using-azure-notification-hubs"></a>Öğretici: Xamarin.Android uygulamaları Azure Notification hubs'ı kullanarak anında iletme bildirimleri

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış

Bu öğretici, bir Xamarin.Android uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir. Firebase Cloud Messaging (FCM) kullanarak anında iletme bildirimleri alan boş bir Xamarin.Android uygulaması oluşturursunuz. Uygulamanızı çalıştıran tüm cihazlara anında iletme bildirimleri yayımlamak için bildirim hub’ınızı kullanırsınız. Tamamlanan kodu kullanılabilir [NotificationHubs uygulaması](https://github.com/Azure/azure-notificationhubs-dotnet/tree/master/Samples/Xamarin/GetStartedXamarinAndroid) örnek.

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Firebase projesi oluşturma ve Firebase Cloud Messaging’i etkinleştirme
> * Bildirim hub’ı oluşturma
> * Xamarin.Android uygulaması oluşturma ve bildirim hub'ına bağlama
> * Azure portalından test bildirimleri gönderme

## <a name="prerequisites"></a>Önkoşullar

* **Azure aboneliği**. Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.
* Windows'da [Xamarin ile Visual Studio] veya OS X'te [Mac için Visual Studio].
* Etkin Google hesabı

## <a name="create-a-firebase-project-and-enable-firebase-cloud-messaging"></a>Firebase projesi oluşturma ve Firebase Cloud Messaging’i etkinleştirme

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging-xamarin.md)]

## <a name="create-a-notification-hub"></a>Bildirim hub’ı oluşturma

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-gcm-settings-for-the-notification-hub"></a>Bildirim hub’ı için GCM ayarlarını yapılandırma

1. **BİLDİRİM AYARLARI** bölümünde **Google (GCM)** seçeneğini belirleyin.
2. Google Firebase Console’dan not ettiğiniz **Eski sunucu anahtarını** girin.
3. Araç çubuğunda **Kaydet**’i seçin.

    ![](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

Bildirim hub'ınız FCM ile birlikte çalışmak üzere yapılandırıldı. Ayrıca, uygulamanızı anında iletme bildirimleri alması ve anında iletme bildirimlerini göndermesi amacıyla kaydetmenizi sağlayan bağlantı dizelerine sahipsiniz.

## <a name="create-a-xamarinandroid-app-and-connect-it-to-notification-hub"></a>Bir Xamarin.Android uygulaması oluşturma ve bildirim hub'ına bağlama

### <a name="create-visual-studio-project-and-add-nuget-packages"></a>Visual Studio projesi oluşturma ve NuGet paketleri ekleme

1. Visual Studio'da açın **dosya** menüsünde **yeni**ve ardından **proje**. İçinde **yeni proje** penceresinde aşağıdaki adımları uygulayın:
    1. Genişletin **yüklü**, **Visual C#** ve ardından **Android**.
    2. Seçin **Android uygulaması (Xamarin)** listeden.
    3. Proje için bir **ad** girin.
    4. Seçin bir **konumu** projesi için.
    5. **Tamam**’ı seçin

        ![Yeni Proje iletişim kutusu](./media/partner-xamarin-notification-hubs-android-get-started/new-project-dialog-new.png)
2. Üzerinde **yeni Android uygulaması** iletişim kutusunda **boş uygulama**seçip **Tamam**.

    ![Yeni Proje iletişim kutusu](./media/partner-xamarin-notification-hubs-android-get-started/new-android-app-dialog.png)
3. **Çözüm Gezgini** penceresinde **Özellikler**’i genişletin ve **AndroidManifest.xml** dosyasına tıklayın. Paket adını, Google Firebase Console’da projenize Firebase Cloud Messaging eklerken girdiğiniz paket adıyla eşleşecek şekilde güncelleştirin.

    ![GCM’de paket adı](./media/partner-xamarin-notification-hubs-android-get-started/package-name-gcm.png)
4. Projeye sağ tıklayın ve **NuGet Paketlerini Yönet...** seçeneğini belirleyin.
5. **Gözat** sekmesini seçin. **Xamarin.GooglePlayServices.Base** öğesini arayın. Sonuç listesinden **Xamarin.GooglePlayServices.Base** öğesini seçin. Ardından **Yükle**’yi seçin.

    ![Google Play Services NuGet](./media/partner-xamarin-notification-hubs-android-get-started/google-play-services-nuget.png)
6. **NuGet Package Manager** penceresinde **Xamarin.Firebase.Messaging** öğesini arayın. Sonuç listesinden **Xamarin.Firebase.Messaging** öğesini seçin. Ardından **Yükle**’yi seçin.
7. Şimdi **Xamarin.Azure.NotificationHubs.Android** araması yapın. Sonuç listesinden **Xamarin.Azure.NotificationHubs.Android** öğesini seçin. Ardından **Yükle**’yi seçin.

### <a name="add-the-google-services-json-file"></a>Google Services JSON Dosyasını ekleme

1. Kopyalama `google-services.json` proje klasörüne Google Firebase konsolundan indirilen dosya.
2. Ekleme `google-services.json` projeye.
3. Seçin `google-services.json` içinde **Çözüm Gezgini** penceresi.
4. **Özellikler** bölmesinde, Derleme Eylemini **GoogleServicesJson** olarak ayarlayın. **GoogleServicesJson** öğesini görmezseniz, Visual Studio’yu kapatın, yeniden başlatın, projeyi yeniden açın ve yeniden deneyin.

    ![GoogleServicesJson derleme eylemi](./media/partner-xamarin-notification-hubs-android-get-started/google-services-json-build-action.png)

### <a name="set-up-notification-hubs-in-your-project"></a>Projenizdeki bildirim hub'larını ayarlama

#### <a name="registering-with-firebase-cloud-messaging"></a>Kaydetme ile Firebase Cloud Messaging

1. Açık `AndroidManifest.xml` dosyasını açıp aşağıdaki Ekle `<receiver>` elementini `<application>` öğesi:

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

2. Aşağıdaki deyimleri ekleyin **uygulama önce** öğesi.

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    ```

3. Android uygulamanız ve bildirim hub'ınız için aşağıdaki bilgileri toplayın:

   * **Dinleme bağlantı dizesi**: Panodan [Azure portalındaki], seçin **bağlantı dizelerini görüntüle**. Kopyalama `DefaultListenSharedAccessSignature` bu değer için bağlantı dizesi.
   * **Hub adı**: Hub'ınızın adını [Azure portalındaki]. Örneğin, *mynotificationhub2*.
4. İçinde **Çözüm Gezgini** penceresinde sağ tıklayın, **proje**seçin **Ekle**ve ardından **sınıfı**.
5. Oluşturma bir `Constants.cs` Xamarin projeniz için sınıf ve sınıfta aşağıdaki sabit değerleri tanımlayın. Yer tutucuları değerleriniz ile değiştirin.

    ```csharp
    public static class Constants
    {
        public const string ListenConnectionString = "<Listen connection string>";
        public const string NotificationHubName = "<hub name>";
    }
    ```

6. Aşağıdaki using deyimlerini `MainActivity.cs`:

    ```csharp
    using Android.Util;
    using Android.Gms.Common;
    ```

7. MainActivity sınıfına aşağıdaki özellikleri ekleyin. Etiket değişkeni, uygulama çalışırken uyarı iletişim kutusu göstermek için kullanılır:

    ```csharp
    public const string TAG = "MainActivity";
    internal static readonly string CHANNEL_ID = "my_notification_channel";
    ```

8. MainActivity sınıfına aşağıdaki yöntemi ekleyin. Denetlediği olmadığını **Google Play Hizmetleri** cihazda kullanılabilir.

    ```csharp
    public bool IsPlayServicesAvailable()
    {
        int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
        if (resultCode != ConnectionResult.Success)
        {
            if (GoogleApiAvailability.Instance.IsUserResolvableError(resultCode))
                Log.Debug(TAG, GoogleApiAvailability.Instance.GetErrorString(resultCode));
            else
            {
                Log.Debug(TAG, "This device is not supported");
                Finish();
            }
            return false;
        }

        Log.Debug(TAG, "Google Play Services is available.");
        return true;
    }
    ```

9. Bir bildirim kanalı oluşturan MainActivity sınıfına aşağıdaki yöntemi ekleyin.

    ```csharp
    private void CreateNotificationChannel()
    {
        if (Build.VERSION.SdkInt < BuildVersionCodes.O)
        {
            // Notification channels are new in API 26 (and not a part of the
            // support library). There is no need to create a notification
            // channel on older versions of Android.
            return;
        }

        var channelName = CHANNEL_ID;
        var channelDescription = string.Empty;
        var channel = new NotificationChannel(CHANNEL_ID, channelName, NotificationImportance.Default)
        {
            Description = channelDescription
        };

        var notificationManager = (NotificationManager)GetSystemService(NotificationService);
        notificationManager.CreateNotificationChannel(channel);
    }
    ```

10. İçinde `MainActivity.cs`, aşağıdaki kodu ekleyin `OnCreate` sonra `base.OnCreate(savedInstanceState)`:

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

    IsPlayServicesAvailable();
    CreateNotificationChannel();
    ```

11. Yeni bir sınıf oluşturun `MyFirebaseIIDService` oluşturduğunuz gibi `Constants` sınıfı.
12. Aşağıdaki using deyimlerini `MyFirebaseIIDService.cs`:

    ```csharp
    using Android.Util;
    using WindowsAzure.Messaging;
    using Firebase.Iid;
    ```

13. İçinde `MyFirebaseIIDService.cs`, aşağıdaki `class` bildirimi ve sınıfınızın devralınacak `FirebaseInstanceIdService`:

    ```csharp
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class MyFirebaseIIDService : FirebaseInstanceIdService
    ```

14. İçinde `MyFirebaseIIDService.cs`, aşağıdaki kodu ekleyin:

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

15. Projeniz için yeni bir sınıf oluşturun, adlandırın `MyFirebaseMessagingService`.
16. Aşağıdaki using deyimlerini `MyFirebaseMessagingService.cs`.

    ```csharp
    using Android.Util;
    using Firebase.Messaging;
    using Android.Support.V4.App;
    using Build = Android.OS.Build;
    ```

17. Aşağıdaki sınıf bildiriminizin ekleyin ve sınıfınızın devralınacak `FirebaseMessagingService`:

    ```csharp
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class MyFirebaseMessagingService : FirebaseMessagingService
    ```

18. Aşağıdaki kodu ekleyin `MyFirebaseMessagingService.cs`:

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

        var notificationBuilder = new NotificationCompat.Builder(this)
                    .SetContentTitle("FCM Message")
                    .SetSmallIcon(Resource.Drawable.ic_launcher)
                    .SetContentText(messageBody)
                    .SetAutoCancel(true)
                    .SetShowWhen(false)
                    .SetContentIntent(pendingIntent);

        if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
        {
            notificationBuilder.SetChannelId(MainActivity.CHANNEL_ID);
        }

        var notificationManager = NotificationManager.FromContext(this);

        notificationManager.Notify(0, notificationBuilder.Build());
    }
    ```

19. Projenizi **derleyin**.
20. Uygulamayı cihazınızda veya yüklü öykünücüde **çalıştırma**

## <a name="send-test-notification-from-the-azure-portal"></a>Azure portalından test bildirimi gönderme

[Azure portalındaki] **Test Gönderimi** seçeneğini kullanarak uygulamanızda bildirim alma testi gerçekleştirebilirsiniz. Bu, cihazınıza test amaçlı anında iletme bildirimi gönderir.

![Azure portalı - Test Gönderimi](media/partner-xamarin-notification-hubs-android-get-started/send-test-notification.png)

Anında iletme bildirimleri normalde, uyumlu bir kitaplık aracılığıyla Mobile Services veya ASP.NET gibi bir arka uç hizmetinde gönderilir. Arka ucunuz için uygun bir kitaplık yoksa bildirim iletileri göndermek için doğrudan REST API de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, arka uca kayıtlı olan tüm Android cihazlarınıza yayın bildirimleri gönderdiniz. Belirli Android cihazlara nasıl anında iletme bildirimleri gönderileceğini öğrenmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Belirli cihazlara anında iletme bildirimleri gönderme](push-notifications-android-specific-devices-firebase-cloud-messaging.md)

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
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: https://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js
[Xamarin ile Visual Studio]: https://docs.microsoft.com/visualstudio/install/install-visual-studio
[Mac için Visual Studio]: https://www.visualstudio.com/vs/visual-studio-mac/
[Azure portalındaki]: https://portal.azure.com/
[wns object]: https://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: https://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Android]: https://msdn.microsoft.com/library/dn282661.aspx
[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[GitHub]: https://github.com/Azure/azure-notificationhubs-android
