---
title: Android uygulamaları için Azure Notification Hubs ve Firebase Cloud Messaging'i kullanmaya başlama | Microsoft Docs
description: Bu öğreticide, Android cihazlarına anında iletme bildirimleri göndermek için Azure Notification Hubs ve Firebase Cloud Messaging’in nasıl kullanılacağını öğrenirsiniz.
services: notification-hubs
documentationcenter: android
keywords: anında iletme bildirimleri,anında iletme bildirimi,android anında iletme bildirimi,fcm,firebase cloud messaging
author: jwhitedev
manager: kpiteira
editor: ''
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 0f4c766bd68227a85e2438bc68b2d61c69ce706c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="get-started-with-azure-notification-hubs-for-android-apps-and-firebase-cloud-messaging"></a>Android uygulamaları için Azure Notification Hubs ve Firebase Cloud Messaging'i kullanmaya başlama
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
> [!IMPORTANT]
> Bu makalede Google Firebase Cloud Messaging (FCM) ile anında iletme bildirimleri gösterilmektedir. Hala Google Cloud Messaging (GCM) kullanıyorsanız bkz. [Azure Notification Hubs ve GCM ile Android’e anında iletme bildirimleri gönderme](notification-hubs-android-push-notification-google-gcm-get-started.md).
> 
> 

Bu öğreticide, bir Android uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs ve Firebase Cloud Messaging'in nasıl kullanılacağı gösterilir. Bu öğreticide Firebase Cloud Messaging (FCM) kullanarak anında iletme bildirimleri alan bir Android uygulaması oluşturacaksınız.

[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Bu öğreticinin tamamlanan kodu GitHub'da [buradan](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase) indirilebilir.

## <a name="prerequisites"></a>Ön koşullar
> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).
> 
> 

* Yukarıda belirtilen etkin bir Azure hesabının yanı sıra, bu öğretici [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797)'nun en son sürümünü gerektirir.
* Firebase Cloud Messaging için Android 2.3 veya üstü.
* Firebase Cloud Messaging için Google Deposu düzeltme 27 gereklidir.
* Firebase Cloud Messaging için Google Play Services 9.0.2 veya üstü.
* Bu öğreticiyi tamamlamak Android uygulamalarına ilişkin diğer tüm Notification Hubs öğreticileri için önkoşuldur.

## <a name="create-a-new-android-studio-project"></a>Yeni bir Android Studio Projesi oluşturma
1. Android Studio'da yeni bir Android Studio projesi başlatın.
   
    ![Android Studio - yeni proje](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)
2. **Telefon ve Tablet** form faktörünü ve desteklemek istediğiniz **Minimum SDK**'yı seçin. Ardından **İleri**'ye tıklayın.
   
    ![Android Studio - proje oluşturma iş akışı](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)
3. Ana etkinlik için **Boş Etkinlik**'i seçin, **İleri**'ye tıklayın ve ardından **Son**'a tıklayın.

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Firebase Cloud Messaging'i destekleyen bir proje oluşturma
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-new-notification-hub"></a>Yeni bir bildirim hub'ı yapılandırma
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6. **Bildirim Hizmetleri**'nin altında **Google (GCM)** seçeneğini belirtin. Daha önce [Firebase konsolundan](https://firebase.google.com/console/) kopyaladığınız FCM sunucu anahtarını girin ve **Kaydet**’e tıklayın.

&emsp;&emsp;![Azure Notification Hubs - Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

Bildirim hub'ınız şimdi Firebase Cloud Messaging ile birlikte çalışmak üzere yapılandırıldı. Uygulamanızı anında iletme bildirimleri alması ve göndermesi amacıyla kaydetmek için bağlantı dizelerine sahipsiniz.

## <a id="connecting-app"></a>Uygulamanızı bildirim hub'ına bağlama
### <a name="add-google-play-services-to-the-project"></a>Projeye Google Play hizmetlerini ekleme
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Azure Notification Hubs kitaplıkları ekleme
1. **Uygulamanın** `Build.Gradle` dosyasında **bağımlılıklar** bölümüne aşağıdaki satırları ekleyin.
   
    ```java
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```

2. **Bağımlılıklar** bölümünden sonra aşağıdaki depoyu ekleyin.
   
    ```java
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }
    ```

### <a name="updating-the-androidmanifestxml"></a>AndroidManifest.xml'i güncelleştirme
1. FCM'yi desteklemek için kodunuza, [Google'ın FirebaseInstanceId API'sini](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) kullanarak [kayıt belirteçleri elde etmek](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) için kullanılan bir Örnek Kimliği dinleme hizmeti eklemeniz gerekir. Bu öğreticide sınıfın adı `MyInstanceIDService` şeklindedir. 
   
    Aşağıdaki hizmet tanımını AndroidManifest.xml dosyasında `<application>` etiketinin içine ekleyin. 
   
    ```xml
        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>
    ```

2. FirebaseInstanceId API'sinden FCM kayıt belirtecinizi aldıktan sonra, [Azure Notification Hub'a kaydolmak](notification-hubs-push-notification-registration-management.md) için bu belirteci kullanacaksınız. `RegistrationIntentService` adlı bir `IntentService` kullanarak bu kaydı arka planda destekleyeceksiniz. Bu hizmet, FCM kayıt belirtecinizi yenilemekten de sorumludur.
   
    Aşağıdaki hizmet tanımını AndroidManifest.xml dosyasında `<application>` etiketinin içine ekleyin. 
   
    ```xml
        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>
    ```

3. Bildirimleri almak için bir alıcı da tanımlamanız gerekir. Aşağıdaki alıcı tanımını AndroidManifest.xml dosyasına `<application>` etiketinin içine ekleyin. `<your package>` yer tutucusunu, `AndroidManifest.xml` dosyasının üst kısmında gösterilen asıl paket adınızla değiştirin.

    ```xml
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
    ```

4. Aşağıdaki FCM ile ilgili gerekli izinleri `</application>` etiketinin altına ekleyin. `<your package>` öğesini, `AndroidManifest.xml` dosyasının üst kısmında gösterilen paket adıyla değiştirdiğinizden emin olun.
   
    Bu izinler hakkında daha fazla bilgi için bkz. [Android için GCM İstemci uygulaması ayarlama](https://developers.google.com/cloud-messaging/android/client#manifest) ve [Android için GCM İstemci Uygulamasını Firebase Cloud Messaging’e geçirme](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).
   
    ```xml
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```

### <a name="adding-code"></a>Kod ekleme
1. Proje Görünümü'nde **app** > **src** > **main** > **java**'yı genişletin. **Java**'nın altındaki paket klasörünüze sağ tıklayın, **Yeni**'ye tıklayın ve ardından **Java Sınıfı**'na tıklayın. `NotificationSettings` adlı yeni bir sınıf ekleyin. 
   
    ![Android Studio - yeni Java sınıfı](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)
   
    Aşağıdaki kodda `NotificationSettings` sınıfı için bu üç yer tutucuyu güncelleştirdiğinizden emin olun:
   
   * **SenderId**: [Firebase konsolundaki](https://firebase.google.com/console/) proje ayarlarınızın **Cloud Messaging** sekmesinde daha önce edindiğiniz Gönderen Kimliği.
   * **HubListenConnectionString**: Hub'ınız için **DefaultListenAccessSignature** bağlantı dizesi. [Azure portalında] hub'ınızın **Erişim İlkeleri**'ne tıklayarak bağlantı dizesini kopyalayabilirsiniz.
   * **HubName**: [Azure portalında] hub dikey penceresinde görünen bildirim hub'ınızın adını kullanın.
     
     `NotificationSettings` kodu:
     
    ```java
       public class NotificationSettings {
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
       }
    ```

2. Yukarıdaki adımları kullanarak `MyInstanceIDService` adlı yeni bir sınıf daha ekleyin. Bu Örnek Kimliği dinleme hizmeti uygulamanız olacak.
   
    Bu sınıfın kodu, arka planda `IntentService`FCM belirtecini yenileme[ amacıyla ](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) hizmetinizi çağırır.
   
    ```java
        import android.content.Intent;
        import android.util.Log;
        import com.google.firebase.iid.FirebaseInstanceIdService;

        public class MyInstanceIDService extends FirebaseInstanceIdService {

            private static final String TAG = "MyInstanceIDService";

            @Override
            public void onTokenRefresh() {

                Log.d(TAG, "Refreshing GCM Registration Token");

                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };
    ```

1. `RegistrationIntentService` adlı projenize, başka bir yeni sınıf ekleyin. Bu, [FCM belirtecini yenileme](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) ve [bildirim hub'ıyla kayıt yapma](notification-hubs-push-notification-registration-management.md) işlemlerini ele alacak `IntentService` hizmetinizin uygulanması olacak.
   
    Bu sınıf için aşağıdaki kod kullanın.
   
    ```java
        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;        
        import com.google.firebase.iid.FirebaseInstanceId;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class RegistrationIntentService extends IntentService {
   
            private static final String TAG = "RegIntentService";
   
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
                    String FCM_token = FirebaseInstanceId.getInstance().getToken();
                    Log.d(TAG, "FCM Registration Token: " + FCM_token);
   
                    // Storing the registration ID that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
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
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
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

2. `MainActivity` sınıfınızda aşağıdaki `import` deyimlerini sınıf bildiriminin üstüne ekleyin.
   
    ```java
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;

3. Add the following private members at the top of the class. You use these to [check the availability of Google Play Services as recommended by Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).
   
    ```java
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
    ```

4. `MainActivity` sınıfınızda Google Play Hizmetleri'nin kullanılabilirliğine aşağıdaki yöntemi ekleyin. 
   
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

5. `MainActivity` sınıfınızda, FCM kayıt belirtecinizi almak ve bildirim hub'ınızla kaydolmak için `IntentService` hizmetinizi çağırmadan önce Google Play Hizmetleri'ni denetleyecek aşağıdaki kodu ekleyin.
   
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

6. `MainActivity` sınıfının `OnCreate` yönteminde, etkinlik oluşturulduğunda kayıt işlemini başlatmak için aşağıdaki kodu ekleyin.
   
    ```java
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
    ```

7. Uygulama durumunu doğrulamak ve uygulamanızda durumu raporlamak için bu ek yöntemleri `MainActivity` öğesine ekleyin.
   
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

8. `ToastNotify` yöntemi, uygulamada kalıcı olarak durumu ve bildirimleri raporlamak için *"Hello World"* `TextView` denetimini kullanır. activity_main.xml düzeninizde bu denetim için aşağıdaki kimliği ekleyin.
   
    ```java
       android:id="@+id/text_hello"
    ```

9. Ardından, AndroidManifest.xml'de tanımladığınız alıcı için bir alt sınıf ekleyeceksiniz. `MyHandler` adlı projenize başka bir yeni sınıf ekleyin.
10. `MyHandler.java`'in üst kısmına şu içeri aktarma deyimlerini ekleyin:
    
    ```java
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
    ```

11. `MyHandler` sınıfı için aşağıdaki kodu ekleyip bunu `com.microsoft.windowsazure.notifications.NotificationsHandler` öğesinin bir alt sınıfı haline getirin.
    
    Bu kod, `OnReceive` yöntemini geçersiz kılar; böylece işleyici alınan bildirimleri raporlar. İşleyici, `sendNotification()` yöntemini kullanarak Android bildirim yöneticisine anında iletme bildirimi gönderir. `sendNotification()` yöntemi, uygulama çalışmıyorken ve bir bildirim alındığında yürütülmelidir.
    
    ```java
        public class MyHandler extends NotificationsHandler {
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
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
    
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }
    ```

12. Android Studio'nun menü çubuğunda, kodunuzda hiçbir hata bulunmadığından emin olmak için **Oluştur** > **Projeyi Yeniden Oluştur**'a tıklayın.
13. Cihazınızda uygulamayı çalıştırın ve uygulamanın bildirim hub'ına başarıyla kaydolduğunu doğrulayın. 
    
    > [!NOTE]
    > Kayıt, örnek kimlik hizmetinin `onTokenRefresh()` yöntemi çağrılana kadar ilk başlatmada başarısız olabilir. Yenileme işlemi bildirim hub'ına başarılı bir kayıt başlatmalıdır.
    > 
    > 

## <a name="sending-push-notifications"></a>Anında iletme bildirimleri gönderme
Uygulamanızda anında iletme bildirimlerini aldığınızı, bunları [Azure portalında] aracılığıyla göndererek test edebilirsiniz; aşağıda gösterildiği gibi hub'ın **Sorun Giderme** Bölümü'nü bulun.

![Azure Notification Hubs - Test Gönderimi](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="testing-your-app"></a>Uygulamanızı test etme
#### <a name="push-notifications-in-the-emulator"></a>Öykünücüde anında iletme bildirimleri
Anında iletme bildirimlerini bir öykünücüde test etmek isterseniz öykünücü görüntünüzün, uygulamanız için seçtiğiniz Google API düzeyini desteklediğinden emin olun. Görüntünüz yerel Google API'lerini desteklemiyorsa **HİZMET\_\_KULLANILAMIYOR** özel durumuyla karşılaşırsınız.

Yukarıdakine ek olarak, **Ayarlar** > **Hesaplar**'ın altında çalışan öykünücünüze Google hesabınızı eklediğinizden emin olun. Aksi halde, GCM ile kayıt girişimleriniz **KİMLİK DOĞRULAMASI\_BAŞARISIZ** özel durumuyla sonuçlanabilir.

#### <a name="running-the-application"></a>Uygulamayı çalıştırma
1. Uygulamayı çalıştırın ve kayıt kimliğinin başarılı bir kaydı bildirdiğine dikkat edin.
   
    ![Android'de test etme - Kanal kaydı](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)
2. Hub'da kayıtlı tüm Android cihazlara gönderilecek bir bildirim iletisi girin.
   
    ![Andorid'de test etme - bir ileti gönderme](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)
3. **Bildirim Gönder**'e basın. Uygulamayı çalıştıran tüm cihazlarda, anında iletme bildirimi iletisiyle birlikte bir `AlertDialog` örneği görünür. Uygulamayı çalıştırmayan ancak anında iletme bildirimleri için daha önce kaydolan cihazlar, Android Bildirim Yöneticisi'nde bir bildirim alır. Bunlar, sol üst köşeden aşağı çekilerek görüntülenebilir.
   
    ![Android'de test etme - bildirimler](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

## <a name="next-steps"></a>Sonraki adımlar
Sonraki adım olarak [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma] öğreticisini öneririz. Bu, belirli kullanıcıları hedeflemek için etiketler kullanarak ASP.NET arka ucundan nasıl bildirim göndereceğinizi gösterir.


Kullanıcılarınızı ilgi alanı gruplarına göre segmentlere ayırmak istiyorsanız [Son dakika haberleri göndermek için Notification Hubs kullanma] öğreticisine göz atın.

Notification Hubs hakkında daha fazla genel bilgi edinmek için bkz. [Notification Hubs Kılavuzu].

<!-- Images. -->



<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Notification Hubs Kılavuzu]: notification-hubs-push-notification-overview.md
[Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Son dakika haberleri göndermek için Notification Hubs kullanma]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure portalında]: https://portal.azure.com
