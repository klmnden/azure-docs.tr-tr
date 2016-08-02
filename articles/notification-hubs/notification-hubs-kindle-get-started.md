<properties
    pageTitle="Kindle uygulamaları için Azure Notification Hubs'ı kullanmaya başlama | Microsoft Azure"
    description="Bu öğreticide, bir Kindle uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenirsiniz."
    services="notification-hubs"
    documentationCenter=""
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="02/29/2016"
    ms.author="wesmc"/>

# Kindle uygulamaları için Notification Hubs'ı kullanmaya başlama

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##Genel Bakış

Bu öğretici, bir Kindle uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir.
Amazon Device Messaging'i (ADM) kullanarak anında iletme bildirimleri alan boş bir Kindle uygulaması oluşturacaksınız.

##Önkoşullar

Bu öğretici için aşağıdakiler gereklidir:

+ <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android sitesi</a>'nden Android SDK'sını alın (Eclipse kullanacağınızı varsayıyoruz).
+ Kindle için geliştirme ortamınızı ayarlamak üzere <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Geliştirme Ortamınızı Ayarlama</a>'daki adımları izleyin.

##Geliştirici portalına yeni bir uygulama ekleme

1. Öncelikle, [Amazon geliştirici portalı]'nda bir uygulama oluşturun.

    ![][0]

2. **Application Key**'i (Uygulama Anahtarı) kopyalayın.

    ![][1]

3. Portalda, uygulamanızın adına ve ardından **Device Messaging** sekmesine tıklayın.

    ![][2]

4. **Create a New Security Profile** (Yeni Bir Güvenlik Profili Oluştur) seçeneğine tıklayın ve ardından yeni bir güvenlik profili oluşturun (örneğin, **TestAdm güvenlik profili**). Daha sonra **Kaydet**'e tıklayın.

    ![][3]

5. Az önce oluşturduğunuz güvenlik profilini görüntülemek için **Security Profiles** (Güvenlik Profilleri) seçeneğine tıklayın. Daha sonra kullanmak için **Client ID** (İstemci Kimliği) ve **Client Secret** (Gizli Anahtar) değerlerini kopyalayın.

    ![][4]

## API Anahtarı oluşturma

1. Yönetici ayrıcalıklarına sahip bir komut istemi açın.
2. Android SDK klasörüne gidin.
3. Aşağıdaki komutu girin:

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.  **keystore** parolası için **android** yazın.

5.  **MD5** parmak izini kopyalayın.
6.  Geliştirici portalına geri dönün. **Messaging** sekmesinde **Android/Kindle**'a tıklayın, uygulamanız için olan paket adını (örneğin, **com.sample.notificationhubtest**) ve **MD5** değerini girin, ardından **Generate API Key** (API Anahtarı Oluştur) seçeneğine tıklayın.

## Hub'a kimlik bilgileri ekleme

Portalda, bildirim hub'ınızın **Configure** (Yapılandır) sekmesine gizli anahtarı ve istemci kimliğini ekleyin.

## Uygulamanızı ayarlama

> [AZURE.NOTE] Bir uygulama oluştururken, en az API Düzeyi 17 kullanın.

ADM kitaplıklarını Eclipse projenize ekleyin:

1. ADM kitaplığını almak için [SDK'yı indirin]. SDK zip dosyasını ayıklayın.
2. Eclipse'te, projenize sağ tıklayın ve ardından **Properties** (Özellikler) seçeneğine tıklayın. Soldaki **Java Build Path**'ı (Java Derleme Yolu) ve ardından üstteki **Libraries **(Kitaplıklar) sekmesini seçin. **Add External Jar** (Dış Jar Ekle) seçeneğine tıklayın ve Amazon SDK'sını ayıkladığınız dizinden `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` dosyasını seçin.
3. NotificationHubs Android SDK'sını indirin (bağlantı).
4. Paketin sıkıştırmasını açın ve ardından `notification-hubs-sdk.jar` dosyasını Eclipse'te `libs` klasörünün içine sürükleyin.

ADM'yi desteklemesi için uygulama bildiriminizi düzenleyin:

1. Amazon ad alanını kök bildirim öğesine ekleyin:


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. Bildirim öğesinin altına ilk öğe olarak izinleri ekleyin. **[YOUR PACKAGE NAME]** öğesini uygulamanızı oluşturmak için kullandığınız paket adı ile değiştirin.

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

3. Uygulama öğesinin ilk alt öğesi olarak aşağıdaki öğeyi ekleyin. **[YOUR SERVICE NAME]**'i sonraki bölümde oluşturduğunuz ADM ileti işleyicisi adı ile değiştirmeyi unutmayın (paket dahil) ve **[YOUR PACKAGE NAME]**'i uygulamanızı oluşturduğunuz paket adı ile değiştirin.

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

## ADM ileti işleyicinizi oluşturma

1. Aşağıdaki şekilde gösterildiği gibi, `com.amazon.device.messaging.ADMMessageHandlerBase` öğesinden devralınan yeni bir sınıf oluşturun ve bunu `MyADMMessageHandler` olarak adlandırın:

    ![][6]

2. Aşağıdaki `import` deyimlerini ekleyin:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. Oluşturduğunuz sınıfa aşağıdaki kodu ekleyin. Hub adını ve bağlantı dizesini (dinleme) değiştirmeyi unutmayın:

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

4. `OnMessage()` yöntemine aşağıdaki kodu ekleyin:

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. `OnRegistered` yöntemine aşağıdaki kodu ekleyin:

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  `OnUnregistered` yöntemine aşağıdaki kodu ekleyin:

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. `MainActivity` yönteminde aşağıdaki içeri aktarma deyimini ekleyin:

        import com.amazon.device.messaging.ADM;

8. `OnCreate` yönteminin sonuna aşağıdaki kodu ekleyin:

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## Uygulamanıza API anahtarınızı ekleme

1. Eclipse'te, projenizin dizin varlıkları içinde **api_key.txt** adlı yeni bir dosya oluşturun.
2. Dosyayı açın ve Amazon geliştirici portalında oluşturduğunuz API anahtarını kopyalayın.

## Uygulamayı çalıştırma

1. Öykünücüyü başlatın.
2. Öykünücüde, üstten çekin ve **Settings** (Ayarlar) seçeneğine tıklayın. Ardından **My account** (Hesabım) seçeneğine tıklayın ve geçerli bir Amazon hesabıyla kaydolun.
3. Eclipse'te uygulamayı çalıştırın.

> [AZURE.NOTE] Bir sorun oluşursa öykünücü (veya cihaz) zamanını kontrol edin. Zaman değerinin doğru olması gerekir. Kindle öykünücüsü zamanını değiştirmek için, Android SDK platformunuzun araçlar dizininden aşağıdaki komutu çalıştırabilirsiniz:

        adb shell  date -s "yyyymmdd.hhmmss"

## İleti gönderme

.NET kullanarak ileti göndermek için:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

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



<!---HONumber=Jun16_HO2-->


