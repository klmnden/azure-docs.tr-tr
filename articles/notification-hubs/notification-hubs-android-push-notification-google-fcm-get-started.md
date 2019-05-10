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
ms.openlocfilehash: 0a344e4a068ac6791403f686fa728530b3c4f17e
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65209363"
---
# <a name="tutorial-push-notifications-to-android-devices-by-using-azure-notification-hubs-and-google-firebase-cloud-messaging"></a>Öğretici: Azure Notification Hubs ve Google Firebase Cloud Messaging kullanarak Android cihazlarına anında iletme bildirimleri

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Bu öğreticide Azure Notification Hubs ve Firebase Cloud Messaging (FCM) için bir Android uygulamasına anında iletme bildirimleri nasıl kullanılacağını gösterir. Bu öğreticide Firebase Cloud Messaging (FCM) kullanarak anında iletme bildirimleri alan bir Android uygulaması oluşturacaksınız.

Bu öğreticinin tamamlanan kodu GitHub'da [buradan](https://github.com/Azure/azure-notificationhubs-android/tree/master/samples/FCMTutorialApp) indirilebilir.

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Android Studio projesi oluşturma.
> * Firebase Cloud Messaging’i destekleyen bir Firebase projesi oluşturma.
> * Bildirim hub’ı oluşturma.
> * Uygulamanızı bildirim hub’ına bağlama.
> * Uygulamayı test etme.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/).

* Yukarıda belirtilen etkin bir Azure hesabının yanı sıra, bu öğretici [Android Studio](https://go.microsoft.com/fwlink/?LinkId=389797)'nun en son sürümünü gerektirir.
* Firebase Cloud Messaging için Android 2.3 veya üstü.
* Firebase Cloud Messaging için Google Deposu düzeltme 27 gereklidir.
* Firebase Cloud Messaging için Google Play Services 9.0.2 veya üstü.
* Bu öğreticiyi tamamlamak Android uygulamalarına ilişkin diğer tüm Notification Hubs öğreticileri için önkoşuldur.

## <a name="create-an-android-studio-project"></a>Android Studio Projesi oluşturma

1. Android Studio'yu başlatın.
2. Seçin **dosya** menüde işaret **yeni**seçip **yeni proje**. 
2. Üzerinde **projenizi seçin** sayfasında **boş etkinlik**seçip **sonraki**. 
3. Üzerinde **Nakonfigurovat projekt** sayfasında, aşağıdaki adımları uygulayın: 
    1. Girin bir **adı** uygulama için.
    2. Proje dosyalarını kaydetmek için bir konum belirtin. 
    3. **Son**’u seçin. 

        ![Nakonfigurovat projekt.)](./media/notification-hubs-android-push-notification-google-fcm-get-started/configure-project.png)

## <a name="create-a-firebase-project-that-supports-fcm"></a>FCM’yi destekleyen bir Firebase projesi oluşturma

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Bildirim hub’ını yapılandırma

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-firebase-cloud-messaging-settings-for-the-hub"></a>Hub için Firebase Cloud Messaging ayarlarını yapılandırma

1. Seçin **Google (GCM/FCM)** altında **ayarları** sol menüsünde. 
2. Yapıştırma **sunucu anahtarı** daha önce kaydettiğiniz FCM projesi. 
3. Araç çubuğunda **Kaydet**’i seçin. 

    ![Azure Notification Hubs - Google (FCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)
4. Notification hubs'ı başarıyla güncelleştirildiğini uyarılar bir ileti görürsünüz. **Kaydet** düğmesi devre dışıdır. 

Bildirim hub'ınız şimdi Firebase Cloud Messaging ile birlikte çalışmak üzere yapılandırıldı. Uygulamanızı anında iletme bildirimleri alması ve göndermesi amacıyla kaydetmek için bağlantı dizelerine sahipsiniz.

## <a id="connecting-app"></a>Uygulamanızı bildirim hub'ına bağlama

### <a name="add-google-play-services-to-the-project"></a>Projeye Google Play hizmetlerini ekleme

[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Azure Notification Hubs kitaplıkları ekleme

1. **Uygulamanın** `Build.Gradle` dosyasında **bağımlılıklar** bölümüne aşağıdaki satırları ekleyin.

    ```gradle
    implementation 'com.microsoft.azure:notification-hubs-android-sdk:0.6@aar'
    implementation 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```

2. **Bağımlılıklar** bölümünden sonra aşağıdaki depoyu ekleyin.

    ```gradle
    repositories {
        maven {
            url "https://dl.bintray.com/microsoftazuremobile/SDK"
        }
    }
    ```

### <a name="add-google-firebase-support"></a>Google Firebase desteği ekleme

1. İçinde `Build.Gradle` dosya **uygulama**, aşağıdaki satırları ekleyin **bağımlılıkları** zaten mevcut değilse bölüm. 

    ```gradle
    implementation 'com.google.firebase:firebase-core:16.0.8'
    implementation 'com.google.firebase:firebase-messaging:17.3.4'
    ```

2. Zaten mevcut değilse şu eklenti dosyanın sonuna ekleyin. 

    ```gradle
    apply plugin: 'com.google.gms.google-services'
    ```
3. Seçin **Şimdi Eşitle** araç.

### <a name="updating-the-androidmanifestxml"></a>Androidmanifest.XML'i güncelleştirme

1. FCM kayıt belirtecinizi aldıktan sonra bunu kullanın [Azure bildirim Hub'ınızla kaydolmak](notification-hubs-push-notification-registration-management.md). `RegistrationIntentService` adlı bir `IntentService` kullanarak bu kaydı arka planda desteklersiniz. Bu hizmet, FCM kayıt belirtecinizi yenilemekten de sorumludur.

    Aşağıdaki hizmet tanımını AndroidManifest.xml dosyasında `<application>` etiketinin içine ekleyin.

    ```xml
    <service
        android:name=".RegistrationIntentService"
        android:exported="false">
    </service>
    ```

2. Bildirimleri almak için bir alıcı da tanımlamanız gerekir. Aşağıdaki alıcı tanımını AndroidManifest.xml dosyasına `<application>` etiketinin içine ekleyin. 

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
    > `<your package NAME>` yer tutucusunu, `AndroidManifest.xml` dosyasının üst kısmında gösterilen asıl paket adınızla değiştirin.
3. FCM ile ilgili aşağıdaki gerekli izinleri Ekle **aşağıda** `</application>` etiketi.

    ```xml
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```

### <a name="adding-code"></a>Kod ekleme

1. Proje Görünümü'nde **app** > **src** > **main** > **java**'yı genişletin. **Java**'nın altındaki paket klasörünüze sağ tıklayın, **Yeni**'ye tıklayın ve ardından **Java Sınıfı**'na tıklayın. Girin `NotificationSettings` adını ve seçin için **Tamam**.

    Aşağıdaki kodda `NotificationSettings` sınıfı için bu üç yer tutucuyu güncelleştirdiğinizden emin olun:

   * **HubListenConnectionString**: **DefaultListenAccessSignature** hub'ınızın bağlantı dizesi. [Azure portal] hub'ınızın **Erişim İlkeleri**'ne tıklayarak bağlantı dizesini kopyalayabilirsiniz.
   * **hubName**: Hub sayfasında görünen bildirim hub'ınızın adını kullanın [Azure portal].

     `NotificationSettings` kodu:

        ```java
        public class NotificationSettings {
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
        }
        ```

     > [!IMPORTANT]
     > Girin **adı** ve **DefaultListenSharedAccessSignature** proceding önce bildirim hub'ınızın daha fazla. 

3. `RegistrationIntentService` adlı projenize, başka bir yeni sınıf ekleyin. Bu sınıf, `IntentService` arabirimini uygular ve [FCM belirtecini yenileme](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) ve [bildirim hub’ıyla kayıt yapma](notification-hubs-push-notification-registration-management.md) işlemlerini ele alır.

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
                // sent to your server. If it is not stored, send the token to your server,
                // otherwise your server should have already received the token.
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

                // Check if the token may have been compromised and needs refreshing.
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
                // If an exception happens while fetching the new token or updating our registration data
                // on a third-party server, this ensures that we'll attempt the update at a later time.
            }

            // Notify UI that registration has completed.
            if (MainActivity.isVisible) {
                MainActivity.mainActivity.ToastNotify(resultString);
            }
        }
    }
    ```

4. `MainActivity` sınıfınızda aşağıdaki `import` deyimlerini sınıf bildiriminin üstüne ekleyin.

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
    * it doesn't, display a dialog that allows users to download the APK from
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

7. `MainActivity` sınıfınızda, FCM kayıt belirtecinizi almak ve bildirim hub'ınızla kaydolmak için `IntentService` hizmetinizi çağırmadan önce Google Play Hizmetleri'ni denetleyecek aşağıdaki kodu ekleyin.

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

8. `MainActivity` sınıfının `OnCreate` yönteminde, etkinlik oluşturulduğunda kayıt işlemini başlatmak için aşağıdaki kodu ekleyin.

    ```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mainActivity = this;
        registerWithNotificationHubs();
        MyHandler.createChannelAndHandleNotifications(getApplicationContext());
    }
    ```

9. Uygulama durumunu doğrulamak ve uygulamanızda durumu raporlamak için bu ek yöntemleri `MainActivity` öğesine ekleyin.

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

10. `ToastNotify` yöntemi, uygulamada kalıcı olarak durumu ve bildirimleri raporlamak için *"Hello World"* `TextView` denetimini kullanır. İçinde **res** -> **Düzen** -> **activity_main.xml** düzeni, bu denetim için aşağıdaki kimliği ekleyin.

    ```java
    android:id="@+id/text_hello"
    ```

11. Ardından, AndroidManifest.xml'de tanımladığınız alıcı için bir alt sınıf ekleyeceksiniz. `MyHandler` adlı projenize başka bir yeni sınıf ekleyin.

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

13. `MyHandler` sınıfı için aşağıdaki kodu ekleyip bunu `com.microsoft.windowsazure.notifications.NotificationsHandler` öğesinin bir alt sınıfı haline getirin.

    Bu kod, `OnReceive` yöntemini geçersiz kılar; böylece işleyici alınan bildirimleri raporlar. İşleyici, `sendNotification()` yöntemini kullanarak Android bildirim yöneticisine anında iletme bildirimi gönderir. `sendNotification()` yöntemi, uygulama çalışmıyorken ve bir bildirim alındığında yürütülmelidir.

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

14. Android Studio'nun menü çubuğunda, kodunuzda hiçbir hata bulunmadığından emin olmak için **Oluştur** > **Projeyi Yeniden Oluştur**'a tıklayın. İlgili bir hata alırsanız `ic_launcher` simgesi, aşağıdaki deyim AndroidManifest.xml dosyasından kaldırın. 

    ```
        android:icon="@mipmap/ic_launcher"
    ```
15. Cihazınızda uygulamayı çalıştırın ve uygulamanın bildirim hub'ına başarıyla kaydolduğunu doğrulayın.

    > [!NOTE]
    > Kayıt, örnek kimlik hizmetinin `onTokenRefresh()` yöntemi çağrılana kadar ilk başlatmada başarısız olabilir. Yenileme işlemi bildirim hub'ına başarılı bir kayıt başlatmalıdır.

    ![Cihaz kaydı başarılı](./media/notification-hubs-android-push-notification-google-fcm-get-started/device-registration.png)

## <a name="test-send-notification-from-the-notification-hub"></a>Bildirim hub’ından bildirim göndermeyi test edin

Aşağıdaki eylemleri uygulayarak [Azure portal] anında iletme bildirimleri gönderebilirsiniz:

1. Azure portalında, üzerinde **bildirim hub'ı** sayfası için bildirim hub'ınızın **Test gönderimi** içinde **sorun giderme** bölümü.
3. **Platformlar** için **Android**’i seçin.
4. **Gönder**’i seçin.  Mobil uygulamada çalıştırmadığınızdan Android cihazda bir bildirim görmezsiniz. Mobil uygulamayı çalıştırdıktan sonra, bildirim iletisini görmek için **Gönder** düğmesini tekrar seçin.
5. En alttaki listede işlemin **sonucunu** görürsünüz.

    ![Azure Notification Hubs - Test Gönderimi](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)
6. Cihazınızda bildirim iletisini görürsünüz. 

    ![Cihazda bildirim iletisi](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-on-device.png)
    

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

### <a name="run-the-mobile-app-on-emulator"></a>Mobil uygulama emulator'da çalıştırma
Anında iletme bildirimlerini bir öykünücüde test etmek isterseniz öykünücü görüntünüzün, uygulamanız için seçtiğiniz Google API düzeyini desteklediğinden emin olun. Görüntünüz yerel Google API'lerini desteklemiyorsa, alabilirsiniz **hizmet\_değil\_kullanılabilir** özel durum.

Ek olarak, **Ayarlar** > **Hesaplar**'ın altında çalışan öykünücünüze Google hesabınızı eklediğinizden emin olun. Aksi takdirde, FCM ile kayıt girişimleriniz sonuçlanabilir **kimlik doğrulaması\_başarısız** özel durum.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Firebase Cloud Messaging hizmete kayıtlı tüm Android cihazlar için bildirimleri için kullanılır. Belirli cihazlara nasıl anında iletme bildirimleri gönderileceğini öğrenmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Öğretici: Belirli Android cihazlarına anında iletme bildirimleri gönderme](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)

<!-- Images. -->

<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: https://go.microsoft.com/fwlink/?LinkId=389800
[Notification Hubs Guidance]: notification-hubs-push-notification-overview.md
[Azure portal]: https://portal.azure.com
