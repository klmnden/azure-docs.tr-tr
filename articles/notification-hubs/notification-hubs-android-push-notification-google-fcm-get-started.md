---
title: Azure Notification Hubs ve Firebase Cloud Messaging kullanarak Android uygulamalarına anında iletme bildirimleri gönderme | Microsoft Docs
description: Bu öğreticide, Android cihazlarına anında iletme bildirimleri göndermek için Azure Notification Hubs ve Google Firebase Cloud Messaging’in nasıl kullanılacağını öğrenirsiniz.
services: notification-hubs
documentationcenter: android
keywords: anında iletme bildirimleri,anında iletme bildirimi,android anında iletme bildirimi,fcm,firebase cloud messaging
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/30/2019
ms.author: jowargo
ms.openlocfilehash: f2efa9b7e1e534f93e4ea01ba52740c8c5ac7b02
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67653873"
---
# <a name="tutorial-push-notifications-to-android-devices-by-using-azure-notification-hubs-and-google-firebase-cloud-messaging"></a>Öğretici: Azure Notification Hubs ve Google Firebase Cloud Messaging kullanarak Android cihazlarına anında iletme bildirimleri

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Bu öğreticide Azure Notification Hubs ve Firebase Cloud Messaging (FCM) için bir Android uygulamasına anında iletme bildirimleri nasıl kullanılacağını gösterir. Bu öğreticide Firebase Cloud Messaging (FCM) kullanarak anında iletme bildirimleri alan bir Android uygulaması oluşturacaksınız.

Bu öğreticinin tamamlanan kodu indirilebilir [github'dan](https://github.com/Azure/azure-notificationhubs-android/tree/master/samples/FCMTutorialApp).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Android Studio projesi oluşturma.
> * Firebase Cloud Messaging’i destekleyen bir Firebase projesi oluşturma.
> * Bir hub'ı oluşturun.
> * Uygulamanızı hub'a bağlayın.
> * Uygulamayı test etme.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/). 

Ayrıca aşağıdaki öğeler gerekir: 

* En son sürümünü [Android Studio](https://go.microsoft.com/fwlink/?LinkId=389797)
* Android 2.3 veya üstü Firebase Cloud Messaging için
* Firebase Cloud Messaging için Google deposu düzeltme 27 veya daha yüksek
* Google Play Services 9.0.2 veya üstü Firebase Cloud Messaging için

Bu öğreticide yapmak Android uygulamaları için diğer tüm Notification Hubs öğreticileri için ön koşuldur.

## <a name="create-an-android-studio-project"></a>Android Studio Projesi oluşturma

1. Android Studio'yu başlatın.
2. Seçin **dosya**, işaret **yeni**ve ardından **yeni proje**. 
2. Üzerinde **projenizi seçin** sayfasında **boş etkinlik**ve ardından **sonraki**. 
3. Üzerinde **Nakonfigurovat projekt** sayfasında, aşağıdaki adımları uygulayın: 
    1. Uygulama için bir ad girin.
    2. Proje dosyalarının kaydedileceği bir konum belirtin. 
    3. **Son**’u seçin. 

        ![Nakonfigurovat projekt.)](./media/notification-hubs-android-push-notification-google-fcm-get-started/configure-project.png)

## <a name="create-a-firebase-project-that-supports-fcm"></a>FCM’yi destekleyen bir Firebase projesi oluşturma

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-hub"></a>Bir hub'ı yapılandırma

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-firebase-cloud-messaging-settings-for-the-hub"></a>Hub için Firebase Cloud Messaging ayarlarını yapılandırma

1. Sol bölmede altında **ayarları** seçin **Google (GCM/FCM)** . 
2. Girin **sunucu anahtarı** daha önce kaydettiğiniz FCM projesi. 
3. Araç çubuğunda **Kaydet**. 

    ![Azure bildirim hub'ı - Google (FCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)
4. Azure portalında hub'ı başarıyla güncelleştirildiğini uyarıları bir ileti görüntüler. **Kaydet** düğmesi devre dışıdır. 

Hub'ınız şimdi Firebase Cloud Messaging ile birlikte çalışacak şekilde yapılandırılır. Bildirimleri almak için bir uygulamayı kaydetme ve bir cihaz için bildirimleri göndermek gerekli olan bağlantı dizelerini de vardır.

## <a id="connecting-app"></a>Uygulamanızı bildirim hub'ına bağlama

### <a name="add-google-play-services-to-the-project"></a>Projeye Google Play hizmetlerini ekleme

1. Android Studio'da **Araçları** menüsünü ve ardından **SDK Yöneticisi**. 
2. Projenizde kullanılan Android SDK'sının hedef sürümü seçin. Ardından **Paket ayrıntılarını göster**. 

    ![Android SDK Yöneticisi - select hedef sürümü](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Seçin **Google API'leri**, zaten yüklü değilse.

    ![Android SDK Yöneticisi - Google API'leri seçili](./media/notification-hubs-android-studio-add-google-play-services/googole-apis-selected.png)
4. Geçiş **SDK Tools** sekmesi. Google Play Hizmetleri henüz yüklemediyseniz, seçin **Google Play Hizmetleri** aşağıdaki görüntüde gösterildiği gibi. Ardından **Uygula** yüklemek için. SDK yolunun sonraki bir adım için olduğunu unutmayın.

    ![Android SDK Yöneticisi - seçili olan Google Play Hizmetleri](./media/notification-hubs-android-studio-add-google-play-services/google-play-services-selected.png)
3. Görürseniz **değişikliği onaylayın** iletişim kutusunda **Tamam**. Bileşen yükleyici istenen bileşenleri yükler. Seçin **son** bileşenler yüklendikten sonra.
4. Seçin **Tamam** kapatmak için **yeni projeler için ayarları** iletişim kutusu.  
5. Seçin **Şimdi Eşitle** araç çubuğunda simge.
1. AndroidManifest.xml dosyasını açın ve ardından aşağıdaki etiketine ekleyin *uygulama* etiketi.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```


### <a name="add-azure-notification-hubs-libraries"></a>Azure Notification Hubs kitaplıklarını ekleyin.

1. Uygulama için Build.Gradle dosyasında bağımlılıklar bölümüne aşağıdaki satırları ekleyin.

    ```gradle
    implementation 'com.microsoft.azure:notification-hubs-android-sdk:0.6@aar'
    implementation 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```

2. Bağımlılıklar bölümünden sonra aşağıdaki depoyu ekleyin.

    ```gradle
    repositories {
        maven {
            url "https://dl.bintray.com/microsoftazuremobile/SDK"
        }
    }
    ```

### <a name="add-google-firebase-support"></a>Google Firebase desteği ekleme

1. Uygulama için Build.Gradle dosyasında aşağıdaki satırları ekleyin **bağımlılıkları** zaten mevcut olmaması durumunda bölümü. 

    ```gradle
    implementation 'com.google.firebase:firebase-core:16.0.8'
    implementation 'com.google.firebase:firebase-messaging:17.3.4'
    ```

2. Zaten yoksa şu eklenti dosyasının sonuna ekleyin. 

    ```gradle
    apply plugin: 'com.google.gms.google-services'
    ```
3. Seçin **Şimdi Eşitle** araç.

### <a name="update-the-androidmanifestxml-file"></a>AndroidManifest.xml dosyasını güncelleştirme

1. FCM kayıt belirtecinizi aldıktan sonra bunu kullanın [Azure Notification Hubs'a kaydedilmek](notification-hubs-push-notification-registration-management.md). Kullanarak bu kaydı arka planda destekleyen bir `IntentService` adlı `RegistrationIntentService`. Bu hizmet, FCM kayıt belirtecinizi ayrıca yeniler.

    Aşağıdaki hizmet tanımını AndroidManifest.xml dosyasında `<application>` etiketinin içine ekleyin.

    ```xml
    <service
        android:name=".RegistrationIntentService"
        android:exported="false">
    </service>
    ```

2. Bildirimleri almak için bir alıcı tanımlamak gerekir. Aşağıdaki alıcı tanımını AndroidManifest.xml dosyasına `<application>` etiketinin içine ekleyin. 

    ```xml
    <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
        android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <category android:name="<your package name>" />
        </intent-filter>
    </receiver>
    ```

    > [!IMPORTANT]
    > Değiştirin `<your package NAME>` AndroidManifest.xml dosyanın üst kısmında gösterilen asıl Paket adınızla ile yer tutucu.
3. Aşağıdaki gerekli FCM ile ilgili aşağıdaki izinleri ekleyin `</application>` etiketi.

    ```xml
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```

### <a name="add-code"></a>Kod ekleme

1. Proje Görünümü'nde **app** > **src** > **main** > **java**'yı genişletin. Altındaki paket klasörünüze sağ **java**seçin **yeni**ve ardından **Java sınıfı**. Girin **NotificationSettings** adı ve ardından **Tamam**.

    Aşağıdaki kodda `NotificationSettings` sınıfı için bu üç yer tutucuyu güncelleştirdiğinizden emin olun:

   * **HubListenConnectionString**: **DefaultListenAccessSignature** hub'ınızın bağlantı dizesi. Tıklayarak bağlantı dizesini kopyalayabilirsiniz **erişim ilkeleri** hub'ınızdaki içinde [Azure portal].
   * **hubName**: Hub'ı sayfasında görünen hub'ınızın adını kullanın [Azure portal].

     `NotificationSettings` kodu:

        ```java
        public class NotificationSettings {
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
        }
        ```

     > [!IMPORTANT]
     > Girin **adı** ve **DefaultListenSharedAccessSignature** hub'ınızın devam etmeden önce. 

3. `RegistrationIntentService` adlı projenize başka bir yeni sınıf ekleyin. Bu sınıfın uyguladığı `IntentService` arabirimi. Aynı zamanda işleyen [FCM belirtecini yenileme](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) ve [bildirim hub'ıyla](notification-hubs-push-notification-registration-management.md).

    Bu sınıf için aşağıdaki kod kullanın.

    ```java
    import android.app.IntentService;
    import android.content.Intent;
    import android.content.SharedPreferences;
    import android.preference.PreferenceManager;
    import android.util.Log;
    import com.google.android.gms.tasks.OnSuccessListener;
    import com.google.firebase.iid.FirebaseInstanceId;
    import com.google.firebase.iid.InstanceIdResult;
    import com.microsoft.windowsazure.messaging.NotificationHub;
    import java.util.concurrent.TimeUnit;

    public class RegistrationIntentService extends IntentService {

        private static final String TAG = "RegIntentService";
        String FCM_token = null;

        private NotificationHub hub;

        public RegistrationIntentService() {
            super(TAG);
        }

        @Override
        protected void onHandleIntent(Intent intent) {

            SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
            String resultString = null;
            String regID = null;
            String storedToken = null;

            try {
                FirebaseInstanceId.getInstance().getInstanceId().addOnSuccessListener(new OnSuccessListener<InstanceIdResult>() { 
                    @Override 
                    public void onSuccess(InstanceIdResult instanceIdResult) { 
                        FCM_token = instanceIdResult.getToken(); 
                        Log.d(TAG, "FCM Registration Token: " + FCM_token); 
                    } 
                }); 
                TimeUnit.SECONDS.sleep(1);

                // Storing the registration ID that indicates whether the generated token has been
                // sent to your server. If it is not stored, send the token to your server.
                // Otherwise, your server should have already received the token.
                if (((regID=sharedPreferences.getString("registrationID", null)) == null)){

                    NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                            NotificationSettings.HubListenConnectionString, this);
                    Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                    regID = hub.register(FCM_token).getRegistrationId();

                    // If you want to use tags...
                    // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                    // regID = hub.register(token, "tag1,tag2").getRegistrationId();

                    resultString = "New NH Registration Successfully - RegId : " + regID;
                    Log.d(TAG, resultString);

                    sharedPreferences.edit().putString("registrationID", regID ).apply();
                    sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                }

                // Check to see if the token has been compromised and needs refreshing.
                else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {

                    NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                            NotificationSettings.HubListenConnectionString, this);
                    Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                    regID = hub.register(FCM_token).getRegistrationId();

                    // If you want to use tags...
                    // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                    // regID = hub.register(token, "tag1,tag2").getRegistrationId();

                    resultString = "New NH Registration Successfully - RegId : " + regID;
                    Log.d(TAG, resultString);

                    sharedPreferences.edit().putString("registrationID", regID ).apply();
                    sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                }

                else {
                    resultString = "Previously Registered Successfully - RegId : " + regID;
                }
            } catch (Exception e) {
                Log.e(TAG, resultString="Failed to complete registration", e);
                // If an exception happens while fetching the new token or updating registration data
                // on a third-party server, this ensures that we'll attempt the update at a later time.
            }

            // Notify UI that registration has completed.
            if (MainActivity.isVisible) {
                MainActivity.mainActivity.ToastNotify(resultString);
            }
        }
    }
    ```

4. İçinde `MainActivity` sınıfında, aşağıdaki `import` deyimlerini sınıf bildiriminin üstüne.

    ```java
    import com.google.android.gms.common.ConnectionResult;
    import com.google.android.gms.common.GoogleApiAvailability;
    import com.microsoft.windowsazure.notifications.NotificationsManager;
    import android.content.Intent;
    import android.util.Log;
    import android.widget.TextView;
    import android.widget.Toast;
    ```

5. Aşağıdaki üyeleri sınıfının üstüne ekleyin. [Google Play Hizmetleri'nin kullanılabilirliğini Google tarafından önerildiği şekilde denetlemek](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk) için bu alanları kullanırsınız.

    ```java
    public static MainActivity mainActivity;
    public static Boolean isVisible = false;
    private static final String TAG = "MainActivity";
    private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
    ```

6. İçinde `MainActivity` sınıfı, Google Play Hizmetleri'nin kullanılabilirliğini denetlemek için aşağıdaki yöntemi ekleyin.

    ```java
    /**
    * Check the device to make sure it has the Google Play Services APK. If
    * it doesn't, display a dialog box that enables  users to download the APK from
    * the Google Play Store or enable it in the device's system settings.
    */

    private boolean checkPlayServices() {
        GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
        int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
        if (resultCode != ConnectionResult.SUCCESS) {
            if (apiAvailability.isUserResolvableError(resultCode)) {
                apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                        .show();
            } else {
                Log.i(TAG, "This device is not supported by Google Play Services.");
                ToastNotify("This device is not supported by Google Play Services.");
                finish();
            }
            return false;
        }
        return true;
    }
    ```

7. İçinde `MainActivity` çağırmadan önce Google Play Hizmetleri'ni denetleyen aşağıdaki kodu ekleyin, sınıf `IntentService` , FCM kayıt belirtecinizi almak ve hub'ınıza kaydetmek için:

    ```java
    public void registerWithNotificationHubs()
    {
        if (checkPlayServices()) {
            // Start IntentService to register this application with FCM.
            Intent intent = new Intent(this, RegistrationIntentService.class);
            startService(intent);
        }
    }
    ```

8. İçinde `OnCreate` yöntemi `MainActivity` sınıf, etkinlik oluşturulduğunda kayıt işlemini başlatmak için aşağıdaki kodu ekleyin:

    ```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mainActivity = this;
        registerWithNotificationHubs();
    }
    ```

9. Uygulamanızda uygulama durumunu ve rapor durumu doğrulamak için bu ek yöntemleri ekleme `MainActivity`:

    ```java
    @Override
    protected void onStart() {
        super.onStart();
        isVisible = true;
    }

    @Override
    protected void onPause() {
        super.onPause();
        isVisible = false;
    }

    @Override
    protected void onResume() {
        super.onResume();
        isVisible = true;
    }

    @Override
    protected void onStop() {
        super.onStop();
        isVisible = false;
    }

    public void ToastNotify(final String notificationMessage) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                TextView helloText = (TextView) findViewById(R.id.text_hello);
                helloText.setText(notificationMessage);
            }
        });
    }
    ```

10. `ToastNotify` yöntemi, uygulamada kalıcı olarak durumu ve bildirimleri raporlamak için *"Hello World"* `TextView` denetimini kullanır. İçinde **res** > **Düzen** > **activity_main.xml** düzeni, bu denetim için aşağıdaki kimliği ekleyin.

    ```java
    android:id="@+id/text_hello"
    ```

11. Ardından, Androidmanifest.XML'de tanımladığınız alıcı için bir alt sınıfı ekleyin. `MyHandler` adlı projenize başka bir yeni sınıf ekleyin.

12. `MyHandler.java`'in üst kısmına şu içeri aktarma deyimlerini ekleyin:

    ```java
    import android.app.NotificationChannel;
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;
    import android.media.RingtoneManager;
    import android.net.Uri;
    import android.os.Build;
    import android.os.Bundle;
    import android.support.v4.app.NotificationCompat;
    import com.microsoft.windowsazure.notifications.NotificationsHandler;    
    import com.microsoft.windowsazure.notifications.NotificationsManager;
    ```

13. İçin aşağıdaki kodu ekleyin `MyHandler` sınıfı, bunu bir alt sınıfı haline `com.microsoft.windowsazure.notifications.NotificationsHandler`.

    Bu kod, `OnReceive` yöntemini geçersiz kılar; böylece işleyici alınan bildirimleri raporlar. İşleyici, `sendNotification()` yöntemini kullanarak Android bildirim yöneticisine anında iletme bildirimi gönderir. Çağrı `sendNotification()` uygulama çalışmıyorken ve bir bildirim alındığında yöntemi.

    ```java
    public class MyHandler extends NotificationsHandler {
        public static final String NOTIFICATION_CHANNEL_ID = "nh-demo-channel-id";
        public static final String NOTIFICATION_CHANNEL_NAME = "Notification Hubs Demo Channel";
        public static final String NOTIFICATION_CHANNEL_DESCRIPTION = "Notification Hubs Demo Channel";
    
        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        Context ctx;
    
        @Override
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String nhMessage = bundle.getString("message");
            sendNotification(nhMessage);
            if (MainActivity.isVisible) {
                MainActivity.mainActivity.ToastNotify(nhMessage);
            }
        }
    
        private void sendNotification(String msg) {
    
            Intent intent = new Intent(ctx, MainActivity.class);
            intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
    
            mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);
    
            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                    intent, PendingIntent.FLAG_ONE_SHOT);
    
            Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
            NotificationCompat.Builder notificationBuilder = new NotificationCompat.Builder(
                    ctx,
                    NOTIFICATION_CHANNEL_ID)
                    .setContentText(msg)
                    .setPriority(NotificationCompat.PRIORITY_HIGH)
                    .setSmallIcon(android.R.drawable.ic_popup_reminder)
                    .setBadgeIconType(NotificationCompat.BADGE_ICON_SMALL);
    
            notificationBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, notificationBuilder.build());
        }
    
        public static void createChannelAndHandleNotifications(Context context) {
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
                NotificationChannel channel = new NotificationChannel(
                        NOTIFICATION_CHANNEL_ID,
                        NOTIFICATION_CHANNEL_NAME,
                        NotificationManager.IMPORTANCE_HIGH);
                channel.setDescription(NOTIFICATION_CHANNEL_DESCRIPTION);
                channel.setShowBadge(true);
    
                NotificationManager notificationManager = context.getSystemService(NotificationManager.class);
                notificationManager.createNotificationChannel(channel);
                NotificationsManager.handleNotifications(context, "", MyHandler.class);
            }
        }
    }
    ```

14. Android Studio'da, menü çubuğunda, seçin **derleme** > **projeyi** kodunuzdaki herhangi bir hata olmadığından emin olmak için. İlgili bir hata alırsanız `ic_launcher` simgesi, aşağıdaki deyim AndroidManifest.xml dosyasından kaldırın: 

    ```
        android:icon="@mipmap/ic_launcher"
    ```
15. Cihazınızda uygulamayı çalıştırın ve onu hub ile başarıyla kaydolduğunu doğrulayın.

    > [!NOTE]
    > Kayıt sırasında kadar ilk başlatma başarısız olabilir `onTokenRefresh()` , örnek kimlik hizmetinin yöntemi çağrılır. Yenileme işlemi bildirim hub'ına başarılı bir kayıt başlatmalıdır.

    ![Cihaz kaydı başarılı](./media/notification-hubs-android-push-notification-google-fcm-get-started/device-registration.png)

## <a name="test-send-notification-from-the-notification-hub"></a>Bildirim hub’ından bildirim göndermeyi test edin

Anında iletme bildirimleri gönderebilirsiniz [Azure portal] aşağıdaki adımların atılmasından tarafından:

1. Azure portalında, bildirim hub'ı sayfasında, hub ' ınızı seçin **Test gönderimi** içinde **sorun giderme** bölümü.
3. **Platformlar** için **Android**’i seçin.
4. **Gönder**’i seçin.  Mobil uygulama çalıştığı henüz için bir bildirim Android cihazında henüz görmezsiniz. Mobil uygulamayı çalıştırdıktan sonra seçin **Gönder** düğmesine tekrar bildirim iletisini görürsünüz.
5. Alttaki listede işleminin sonucu bakın.

    ![Azure Notification Hubs - Test Gönderimi](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)
6. Cihazınızda bildirim iletisini görürsünüz. 

    ![Cihazda bildirim iletisi](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-on-device.png)
    

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

### <a name="run-the-mobile-app-on-emulator"></a>Mobil uygulama emulator'da çalıştırma
Anında iletme bildirimlerini bir öykünücüde test etmeden önce öykünücü görüntünüzün, uygulamanız için seçtiğiniz Google API düzeyini desteklediğinden emin olun. Görüntünüz yerel Google API'lerini desteklemiyorsa, alabilirsiniz **hizmet\_değil\_kullanılabilir** özel durum.

Ayrıca, altında çalışan öykünücünüze Google hesabınızı eklediğinizden emin olun **ayarları** > **hesapları**. Aksi takdirde, FCM ile kayıt girişimleriniz neden olabilir **kimlik doğrulaması\_başarısız** özel durum.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Firebase Cloud Messaging hizmete kayıtlı tüm Android cihazlar için bildirimleri için kullanılır. Belirli cihazlara nasıl anında iletme bildirimleri gönderileceğini öğrenmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Öğretici: Belirli Android cihazlarına anında iletme bildirimleri gönderme](push-notifications-android-specific-devices-firebase-cloud-messaging.md)

<!-- Images. -->

<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: https://go.microsoft.com/fwlink/?LinkId=389800
[Notification Hubs Guidance]: notification-hubs-push-notification-overview.md
[Azure portal]: https://portal.azure.com
