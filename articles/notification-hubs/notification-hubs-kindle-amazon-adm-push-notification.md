---
title: Azure Notification Hubs kullanarak Kindle uygulamalarına anında iletme bildirimleri gönderme | Microsoft Docs
description: Bu öğreticide, bir Kindle uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenirsiniz.
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 346fc8e5-294b-4e4f-9f27-7a82d9626e93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-kindle
ms.devlang: Java
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 7dc969fdcf8b819f5223bdbff3cc1b9d4439c370
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60873623"
---
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Kindle uygulamaları için Notification Hubs'ı kullanmaya başlama

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Bu öğretici, bir Kindle uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir. Amazon Device Messaging (ADM) kullanarak anında iletme bildirimleri alan boş bir Kindle uygulaması oluşturursunuz.

Bu öğreticide, aşağıdaki görevleri gerçekleştirmek için kod oluşturur/güncelleştirirsiniz:

> [!div class="checklist"]
> * Geliştirici portalına yeni bir uygulama ekleme
> * API Anahtarı oluşturma
> * Hub'a kimlik bilgileri ekleme
> * Uygulamanızı ayarlama
> * ADM ileti işleyicinizi oluşturma
> * Uygulamanıza API anahtarınızı ekleme
> * Uygulamayı çalıştırma
> * Test bildirimi gönderme

## <a name="prerequisites"></a>Önkoşullar

* Android (Eclipse kullandığınız varsayılarak) SDK'sını alın [Android Geliştirici Portalı](https://go.microsoft.com/fwlink/?LinkId=389797).
* Kindle için geliştirme ortamınızı ayarlamak üzere [Geliştirme Ortamınızı Ayarlama](https://developer.amazon.com/docs/fire-tablets/ft-set-up-your-development-environment.html)'daki adımları izleyin.

## <a name="add-a-new-app-to-the-developer-portal"></a>Geliştirici portalına yeni bir uygulama ekleme

1. Öncelikle, [Amazon geliştirici portalı]'nda bir uygulama oluşturun.

    ![][0]

2. **Application Key**'i (Uygulama Anahtarı) kopyalayın.

    ![][1]

3. Portalda, uygulamanızın adına ve ardından **Device Messaging** sekmesine tıklayın.

    ![][2]

4. **Create a New Security Profile** (Yeni Bir Güvenlik Profili Oluştur) seçeneğine tıklayın ve ardından yeni bir güvenlik profili oluşturun (örneğin, **TestAdm güvenlik profili**). Daha sonra **Kaydet**'e tıklayın.

    ![][3]

5. Oluşturduğunuz güvenlik profilini görüntülemek için **Güvenlik Profilleri**’ne tıklayın. Daha sonra kullanmak için **Client ID** (İstemci Kimliği) ve **Client Secret** (Gizli Anahtar) değerlerini kopyalayın.

    ![][4]

## <a name="create-an-api-key"></a>API Anahtarı oluşturma

1. Yönetici ayrıcalıklarına sahip bir komut istemi açın.
2. Android SDK klasörüne gidin.
3. Aşağıdaki komutu girin:

    ```shell
    keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
    ```

    ![][5]

4. **keystore** parolası için **android** yazın.
5. **MD5** parmak izini kopyalayın.
6. Geliştirici portalına geri dönün. **Messaging** sekmesinde **Android/Kindle**'a tıklayın, uygulamanız için olan paket adını (örneğin, **com.sample.notificationhubtest**) ve **MD5** değerini girin, ardından **Generate API Key** (API Anahtarı Oluştur) seçeneğine tıklayın.

## <a name="add-credentials-to-the-hub"></a>Hub'a kimlik bilgileri ekleme

Portalda, bildirim hub'ınızın **Configure** (Yapılandır) sekmesine gizli anahtarı ve istemci kimliğini ekleyin.

## <a name="set-up-your-application"></a>Uygulamanızı ayarlama

> [!NOTE]
> Bir uygulama oluştururken, en az API Düzeyi 17 kullanın.

ADM kitaplıklarını Eclipse projenize ekleyin:

1. ADM kitaplığını almak için [SDK’yı indirin]. SDK zip dosyasını ayıklayın.
2. Eclipse'te, projenize sağ tıklayın ve ardından **Properties** (Özellikler) seçeneğine tıklayın. Seçin **Java oluşturma yolu** solda ve ardından **kitaplıkları** en üstteki sekmedeki. Tıklayın **dış Jar Ekle**seçip `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` Amazon SDK'sını ayıkladığınız dizinden dosya.
3. NotificationHubs Android SDK'sını indirin (bağlantı).
4. Paketin sıkıştırmasını açın ve ardından `notification-hubs-sdk.jar` dosyasını Eclipse'te `libs` klasörünün içine sürükleyin.

ADM'yi desteklemesi için uygulama bildiriminizi düzenleyin:

1. Amazon ad alanını kök bildirim öğesine ekleyin:

    ```xml
    xmlns:amazon="http://schemas.amazon.com/apk/res/android"
    ```

2. Bildirim öğesinin altına ilk öğe olarak izinleri ekleyin. **[YOUR PACKAGE NAME]** öğesini uygulamanızı oluşturmak için kullandığınız paket adı ile değiştirin.

    ```xml
    <permission
        android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
        android:protectionLevel="signature" />

    <uses-permission android:name="android.permission.INTERNET"/>

    <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

    <!-- This permission allows your app access to receive push notifications
    from ADM. -->
    <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

    <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    ```
3. Uygulama öğesinin ilk alt öğesi olarak aşağıdaki öğeyi ekleyin. **[YOUR SERVICE NAME]**'i sonraki bölümde oluşturduğunuz ADM ileti işleyicisi adı ile değiştirmeyi unutmayın (paket dahil) ve **[YOUR PACKAGE NAME]**'i uygulamanızı oluşturduğunuz paket adı ile değiştirin.

    ```xml
    <amazon:enable-feature
        android:name="com.amazon.device.messaging"
        android:required="true"/>
    <service
        android:name="[YOUR SERVICE NAME]"
        android:exported="false" />

    <receiver
        android:name="[YOUR SERVICE NAME]$Receiver" />

        <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
        android:permission="com.amazon.device.messaging.permission.SEND" >

        <!-- To interact with ADM, your app must listen for the following intents. -->
        <intent-filter>
        <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
        <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

        <!-- Replace the name in the category tag with your app's package name. -->
        <category android:name="[YOUR PACKAGE NAME]" />
        </intent-filter>
    </receiver>
    ```

## <a name="create-your-adm-message-handler"></a>ADM ileti işleyicinizi oluşturma

1. Aşağıdaki şekilde gösterildiği gibi, `com.amazon.device.messaging.ADMMessageHandlerBase` öğesinden devralınan yeni bir sınıf oluşturun ve bunu `MyADMMessageHandler` olarak adlandırın:

    ![][6]

2. Aşağıdaki `import` deyimlerini ekleyin:

    ```java
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;
    import android.support.v4.app.NotificationCompat;
    import com.amazon.device.messaging.ADMMessageReceiver;
    import com.microsoft.windowsazure.messaging.NotificationHub
    ```

3. Oluşturduğunuz sınıfa aşağıdaki kodu ekleyin. Hub adını ve bağlantı dizesini (dinleme) değiştirmeyi unutmayın:

    ```java
    public static final int NOTIFICATION_ID = 1;
    private NotificationManager mNotificationManager;
    NotificationCompat.Builder builder;
        private static NotificationHub hub;
    public static NotificationHub getNotificationHub(Context context) {
        Log.v("com.wa.hellokindlefire", "getNotificationHub");
        if (hub == null) {
            hub = new NotificationHub("[hub name]", "[listen connection string]", context);
        }
        return hub;
    }

    public MyADMMessageHandler() {
            super("MyADMMessageHandler");
        }

        public static class Receiver extends ADMMessageReceiver
        {
            public Receiver()
            {
                super(MyADMMessageHandler.class);
            }
        }

        private void sendNotification(String msg) {
            Context ctx = getApplicationContext();

            mNotificationManager = (NotificationManager)
                ctx.getSystemService(Context.NOTIFICATION_SERVICE);

        PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

        NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                        .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
    }
    ```
4. `OnMessage()` yöntemine aşağıdaki kodu ekleyin:

    ```java
    String nhMessage = intent.getExtras().getString("msg");
    sendNotification(nhMessage);
    ```
5. `OnRegistered` yöntemine aşağıdaki kodu ekleyin:

    ```java
    try {
        getNotificationHub(getApplicationContext()).register(registrationId);
    } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
    }
    ```
6. `OnUnregistered` yöntemine aşağıdaki kodu ekleyin:

    ```java
    try {
        getNotificationHub(getApplicationContext()).unregister();
    } catch (Exception e) {
        Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
    }
    ```
7. `MainActivity` yönteminde aşağıdaki içeri aktarma deyimini ekleyin:

    ```java
    import com.amazon.device.messaging.ADM;
    ```
8. `OnCreate` yönteminin sonuna aşağıdaki kodu ekleyin:

    ```java
    final ADM adm = new ADM(this);
    if (adm.getRegistrationId() == null)
    {
        adm.startRegister();
    } else {
        new AsyncTask() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                    } catch (Exception e) {
                        Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
    }
    ```

## <a name="add-your-api-key-to-your-app"></a>Uygulamanıza API anahtarınızı ekleme

1. Eclipse'te, adlı yeni bir dosya oluşturmak `api_key.txt` projenizin dizin varlıkları içinde.
2. Dosyayı açın ve Amazon geliştirici portalında oluşturduğunuz API anahtarını kopyalayın.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Öykünücüyü başlatın.
2. Öykünücüde, üstten çekin ve **Settings** (Ayarlar) seçeneğine tıklayın. Ardından **My account** (Hesabım) seçeneğine tıklayın ve geçerli bir Amazon hesabıyla kaydolun.
3. Eclipse'te uygulamayı çalıştırın.

> [!NOTE]
> Bir sorun oluşursa öykünücü (veya cihaz) zamanını kontrol edin. Zaman değerinin doğru olması gerekir. Kindle öykünücüsü zamanını değiştirmek için, Android SDK platformunuzun araçlar dizininden aşağıdaki komutu çalıştırabilirsiniz:

```shell
adb shell  date -s "yyyymmdd.hhmmss"
```

## <a name="send-a-notification-message"></a>Bildirim iletisi gönderme

.NET kullanarak ileti göndermek için:

```csharp
static void Main(string[] args)
{
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

    hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
}
```

![][7]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, arka uca kayıtlı olan tüm Kindle cihazlarınıza yayın bildirimleri gönderdiniz. Kindle belirli cihazlara anında iletme bildirimleri öğrenmek için aşağıdaki öğreticiye geçin:  Aşağıdaki öğreticide belirli Android cihazlarına anında iletme bildirimleri gösterilmektedir, ancak belirli Kindle cihazlarına bildirim göndermek için aynı mantığı kullanabilirsiniz.

> [!div class="nextstepaction"]
>[Belirli cihazlara anında iletme bildirimleri gönderme](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)

<!-- URLs. -->
[Amazon geliştirici portalı]: https://developer.amazon.com/home.html
[SDK’yı indirin]: https://developer.amazon.com/public/resources/development-tools/sdk
[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
