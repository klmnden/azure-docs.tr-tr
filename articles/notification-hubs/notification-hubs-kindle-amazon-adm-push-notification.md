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
ms.date: 04/29/2019
ms.author: jowargo
ms.openlocfilehash: abc77ad4d06dc719ee1a89cd8fcf29d42d96b483
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147626"
---
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Kindle uygulamaları için Notification Hubs'ı kullanmaya başlama

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Bu öğretici, bir Kindle uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir. Amazon Device Messaging (ADM) kullanarak anında iletme bildirimleri alan boş bir Kindle uygulaması oluşturursunuz.

Bu öğreticide, aşağıdaki görevleri gerçekleştirmek için kod oluşturur/güncelleştirirsiniz:

> [!div class="checklist"]
> * Geliştirici portalına yeni bir uygulama ekleme
> * API Anahtarı oluşturma
> * Oluşturma ve bir bildirim hub'ı yapılandırma
> * Uygulamanızı ayarlama
> * ADM ileti işleyicinizi oluşturma
> * Uygulamanıza API anahtarınızı ekleme
> * Uygulamayı çalıştırma
> * Test bildirimi gönderme

## <a name="prerequisites"></a>Önkoşullar

- [Android Studio](https://developer.android.com/studio/?gclid=CjwKCAjwwZrmBRA7EiwA4iMzBPZ9YYmR0pbb5LtjnWhWCxe8PWrmjmeaR6ad5ksCr_j2mmkVj_-o6hoCAqwQAvD_BwE).
- Kindle için geliştirme ortamınızı ayarlamak üzere [Geliştirme Ortamınızı Ayarlama](https://developer.amazon.com/docs/fire-tablets/ft-set-up-your-development-environment.html)'daki adımları izleyin.

## <a name="add-a-new-app-to-the-developer-portal"></a>Geliştirici portalına yeni bir uygulama ekleme

1. Oturum [Amazon Geliştirici Portalı](https://developer.amazon.com/apps-and-games/console/apps/list.html).
2. Seçin **yeni uygulama Ekle**ve ardından **Android**.  

    ![Yeni uygulama Ekle düğmesi](./media/notification-hubs-kindle-get-started/add-new-app-button.png)
1. Üzerinde **yeni uygulama gönderme** sayfasında, almak için aşağıdaki adımları izleyin **uygulama anahtarı**:
    1. İçin bir ad girin **uygulama başlığı**.
    2. Seçin **kategori** (örneğin: eğitim)
    4. Bir e-posta adresi girin **Müşteri Destek e-posta adresi** alan. 
    5. **Kaydet**’i seçin.

        ![Yeni uygulama gönderme sayfası](./media/notification-hubs-kindle-get-started/new-app-submission-page.png) 
2.  Üstünde geçiş **uygulama hizmetleri** sekmesi.

    ![Uygulama Hizmetleri sekmesi](./media/notification-hubs-kindle-get-started/app-services-tab.png)
1. Üzerinde **uygulama hizmetleri** sekmesinde, aşağı kaydırın ve seçin **görünümü Mobile Ads** içinde **Mobile Ads** bölümü. Gördüğünüz **Mobile Ads** yeni bir web tarayıcısı sekmesinde sayfa. 

    ![Mobile Ads bölüm - görünüm Mobile Ads bağlantı](./media/notification-hubs-kindle-get-started/view-mobile-ads-link.png)
1. Üzerinde **Mobile Ads** sayfasında, aşağıdaki adımları uygulayın: 
    1. Uygulamanızı öncelikle çocukların 13 yaşından yönlendirilmiş olup olmadığını belirtin. Bu öğreticide, seçin **Hayır**.
    1. Seçin **gönderme**. 

        ![Mobile Ads sayfası](./media/notification-hubs-kindle-get-started/mobile-ads-page.png)
    3. Kopyalama **uygulama anahtarı** gelen **Mobile Ads** sayfası. 

        ![Uygulama anahtarı](./media/notification-hubs-kindle-get-started/application-key.png)
3.  Şimdi, sahip web tarayıcı sekmesine geçiş **uygulama hizmetleri** sekmesini açın ve aşağıdaki adımları uygulayın:
    1. Kaydırma **Device Messaging** bölümü.     
    1. Genişletin **mevcut güvenlik profili seçin veya Yeni Oluştur**ve ardından **güvenlik profili oluştur**. 

        ![Güvenlik profili düğme oluşturma](./media/notification-hubs-kindle-get-started/create-security-profile-button.png)
    1. Girin bir **adı** güvenliği profiliniz için. 
    2. Girin **açıklama** güvenliği profiliniz için. 
    3. **Kaydet**’i seçin. 

        ![Güvenlik profilini Kaydet](./media/notification-hubs-kindle-get-started/save-security-profile.png)
    1. Seçin **etkinleştirme Device Messaging** cihaz üzerinde bu güvenlik profili Mesajlaşma olanağı. 

        ![Cihaz mesajlaşmayı etkinleştirebilirsiniz](./media/notification-hubs-kindle-get-started/enable-device-messaging.png)
    1. Ardından, **Güvenlik Profili Görüntüle** sonucu sayfasında. 
1. Şimdi, **güvenlik profili** sayfasında, aşağıdaki adımları uygulayın: 
    1. Geçiş **Web ayarları** sekmesini tıklatın ve Kopyala **istemci kimliği** ve **gizli** daha sonra kullanmak için değer. 

        ![İstemci Kimliğini ve parolasını alın](./media/notification-hubs-kindle-get-started/client-id-secret.png) 
    2. Geçiş **Android/Kindle ayarları** sayfasında ve sayfayı açık tutun. Sonraki bölümde bu değerleri girmenizi isteriz. 

## <a name="create-an-api-key"></a>API Anahtarı oluşturma
1. Yönetici ayrıcalıklarına sahip bir komut istemi açın.
2. Android SDK klasörüne gidin.
3. Aşağıdaki komutu girin:

    ```shell
    keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
    ```
4. **keystore** parolası için **android** yazın.
5. Kopyalama **MD5** ve **SHA256** parmak izleri. 
6. Geliştirici Portalı'nda geri **Android/Kindle ayarları** sekmesinde, aşağıdaki adımları uygulayın: 
    1. Girin bir **API anahtarı adı**. 
    2. girin **paket adını** uygulamanız için (örneğin, **com.fabrikam.mykindleapp**) ve **MD5** değeri.
        
        >[!IMPORTANT]
        > Android Studio'da bir uygulama oluşturduğunuzda, belirttiğiniz aynı paket adı burada kullanın. 
    1. Yapıştırma **MD5 imzasını** daha önce kopyaladığınız. 
    2. Yapıştırma **SHA256 imza** daha önce kopyaladığınız.  
    3. Seçin **yeni anahtar oluştur**.

        ![Android/Kindle ayarları - anahtar oluşturma](./media/notification-hubs-kindle-get-started/android-kindle-settings.png)
    4. Şimdi, seçtiğiniz **Göster** API anahtarını görmek için listedeki. 

        ![Android/Kindle ayarları - API anahtarı Göster](./media/notification-hubs-kindle-get-started/show-api-key-button.png) 
    5. İçinde **API anahtarı ayrıntılarını** penceresinde API anahtarını kopyalayıp bir yere kaydedin. Ardından, **X** penceresini kapatmak için sağ üst köşedeki. 


## <a name="create-and-configure-a-notification-hub"></a>Oluşturma ve bir bildirim hub'ı yapılandırma

1. İzleyeceğiniz adımlar [Azure portalında bir Azure bildirim hub'ı oluşturma](create-notification-hub-portal.md) makalede bildirim hub'ı oluşturmak için. 
2. Seçin **Amazon (ADM)** altında **ayarları** menüsü.
3. Yapıştırma **istemci kimliği** ve **gizli** daha önce kaydedildi. 
4. Araç çubuğunda **Kaydet**’i seçin. 

    ![Bildirim hub'ında ADM ayarlarını yapılandırma](./media/notification-hubs-kindle-get-started/configure-notification-hub.png)
5. Seçin **erişim ilkeleri** seçin ve soldaki menüden **kopyalama** düğmesi için bağlantı dizesi için **DefaultListenSharedAccessSignature** ilkesi. Başka bir yere kaydedin. Daha sonra kaynak kodunda kullanacaksınız. 

    ![Bildirim hub'ı - dinleme bağlantı dizesi](./media/notification-hubs-kindle-get-started/event-hub-listen-connection-string.png)    

## <a name="set-up-your-application"></a>Uygulamanızı ayarlama

1. Android Studio'yu başlatın. 
2. Seçin **dosya**, işaret **yeni**ve ardından **yeni proje**. 
3. İçinde **projenizi seçin** penceresi, **telefon ve Tablet** sekmesinde **boş etkinlik**seçip **sonraki**. 
4. İçinde **Nakonfigurovat projekt** penceresinde aşağıdaki adımları uygulayın:
    1. Girin bir **uygulamanızın adı**. Amazon Geliştirici Portalı'nda oluşturulan uygulama adıyla eşleşen isteyebilirsiniz. 
    2. Girin bir **paket adı**. 
        
        >[!IMPORTANT]
        >Paket adı Amazon Geliştirici Portalı'nda belirtilen paket adı eşleşmelidir.
    3. Gözden geçirin ve kalan değerleri uygun şekilde güncelleştirin. 
    4. **Son**’u seçin. 

        ![Android projesi yapılandırma](./media/notification-hubs-kindle-get-started/new-android-studio-project.png)
5. İndirme [Amazon Geliştirici SDK'sını Android](https://developer.amazon.com/sdk-download) sabit diskinize kitaplığı. SDK zip dosyasını ayıklayın.
6. Android Studio'da klasör yapısından değiştirme **Android** için **proje** Bunu zaten ayarlanmadıysa **proje**. 

    ![Android Studio - Proje yapısı geçin](./media/notification-hubs-kindle-get-started/android-studio-project-view.png)
7. Genişletin **uygulama** görmek için **libs** ağaç görünümünde klasörü.     
8. Dosya Gezgini penceresinde, Amazon Android SDK'ları karşıdan yüklediğiniz klasöre gidin.
9. Tuşuna **CTRL** ve sürükle ve bırak **amazon-cihaz-Mesajlaşma-1.0.1.jar** dosyasını **LIB** ağaç görünümünde düğümü. 

    ![Android Studio - Amazon cihaz Mesajlaşma JAR Ekle](./media/notification-hubs-kindle-get-started/drag-drop-amazon-device-messaging-jar.png)
9. İçinde **kopyalama** penceresinde **Tamam**. Görürseniz **taşıma** penceresi yerine **kopyalama** penceresini kapatın ve sürükle ve bırak işlemi deneyin **CTRL** düğmeye basıldığını. 

    ![Android Studio - JAR kopyalama](./media/notification-hubs-kindle-get-started/copy-jar-window.png)
10. Şu deyimi ekleyin **uygulamanın build.gradle** dosyası **bağımlılıkları** bölüm: `implementation files('libs/amazon-device-messaging-1.0.1.jar')`. 

    ![Android Studio - ADM için uygulamanın build.gradle Ekle](./media/notification-hubs-kindle-get-started/adm-build-gradle.png)
11. İçinde `Build.Gradle` dosya **uygulama**, aşağıdaki satırları ekleyin **bağımlılıkları** bölümü: 

    ```gradle
    implementation 'com.microsoft.azure:notification-hubs-android-sdk:0.6@aar'
    implementation 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```
12. Aşağıdaki depoyu ekleyin **sonra** **bağımlılıkları** bölümü:

    ```gradle
    repositories {
        maven {
            url "https://dl.bintray.com/microsoftazuremobile/SDK"
        }
    }
    ```
13. Düzenleyicisinde **build.gradle** dosya **uygulama**seçin **Şimdi Eşitle** araç. 

    ![Android Studio - uygulama eşitleme build.gradle](./media/notification-hubs-kindle-get-started/gradle-sync-now.png)
14. Ağaç görünümünde Android yapısı için dönün.  Amazon ad alanını kök bildirim öğesine ekleyin:

    ```xml
    xmlns:amazon="http://schemas.amazon.com/apk/res/android"
    ```
   
    ![Amazon ad alanı bildiriminde](./media/notification-hubs-kindle-get-started/amazon-namespace-manifest.png)
2. Bildirim öğesinin altına ilk öğe olarak izinleri ekleyin. Değiştirin **[YOUR PACKAGE NAME]** uygulamanızı oluşturmak için kullanılan paket.

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
3. Uygulama öğesinin ilk alt öğesi olarak aşağıdaki öğeyi ekleyin. Değiştirin **[YOUR PACKAGE NAME]** uygulamanızı oluşturduğunuz paket adı ile. Sonraki adımda MyADMMessageHandler sınıfı oluşturacaksınız. 

    ```xml
        <amazon:enable-feature
            android:name="com.amazon.device.messaging"
            android:required="true"/>
        <service
            android:name="[YOUR PACKAGE NAME].MyADMMessageHandler"
            android:exported="false" />
        <receiver
            android:name="[YOUR PACKAGE NAME].MyADMMessageHandler$Receiver"
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

1. Yeni bir sınıfa eklemek `com.fabrikam.mykindleapp` devraldığı proje paketinde `com.amazon.device.messaging.ADMMessageHandlerBase` ve adlandırın `MyADMMessageHandler`, aşağıdaki görüntüde gösterildiği gibi:

    ![MyADMMessageHandler sınıfı oluşturma](./media/notification-hubs-kindle-get-started/create-adm-message-handler.png)

2. Aşağıdaki `import` deyimleriyle `MyADMMessageHandler` sınıfı:

    ```java
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;
    import android.support.v4.app.NotificationCompat;
    import android.util.Log;
    import com.amazon.device.messaging.ADMMessageReceiver;
    import com.microsoft.windowsazure.messaging.NotificationHub;
    ```

3. Oluşturduğunuz sınıfa aşağıdaki kodu ekleyin. Unutmayın `[HUB NAME]` ve `[LISTEN CONNECTION STRING]` bildirim hub'ı ve dinleme bağlantı dizesi adı: 

    ```java
    public static final int NOTIFICATION_ID = 1;
    private NotificationManager mNotificationManager;
    NotificationCompat.Builder builder;
    private static NotificationHub hub;

    public static NotificationHub getNotificationHub(Context context) {
        Log.v("com.wa.hellokindlefire", "getNotificationHub");
        if (hub == null) {
            hub = new NotificationHub("[HUB NAME]", "[HUB NAMESPACE CONNECTION STRING]", context);
        }
        return hub;
    }

    public MyADMMessageHandler() {
        super("MyADMMessageHandler");
    }

    @Override
    protected void onMessage(Intent intent) {
        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);
    }

    @Override
    protected void onRegistrationError(String s) {

    }

    @Override
    protected void onRegistered(String s) {
        try {
            getNotificationHub(getApplicationContext()).register(s);
        } catch (Exception e) {
            Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
        }
    }

    @Override
    protected void onUnregistered(String s) {
        try {
            getNotificationHub(getApplicationContext()).unregister();
        } catch (Exception e) {
            Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
        }
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
## <a name="create-an-adm-object"></a>ADM nesne oluşturma
1 içinde `MainActivity.java` içeri aktarma deyimlerini ekleyin:

    ```java
    import android.os.AsyncTask;
    import android.util.Log;
    import com.amazon.device.messaging.ADM;
    ```
2. `OnCreate` yönteminin sonuna aşağıdaki kodu ekleyin:

    ```java
    final ADM adm = new ADM(this);
    if (adm.getRegistrationId() == null)
    {
        adm.startRegister();
    } else {
        new AsyncTask() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {                         MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
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
1. Varlıklar klasöründe projeye eklemek için aşağıdaki adımları izleyin. 
    1. Geçiş **proje** görünümü. 
    2. Sağ **uygulama**.
    3. **Yeni**'yi seçin.
    4. Seçin **klasör**. 
    5. Ardından, **varlıklar klasör**. 

        ![Varlıklar klasör Menüsü Ekle](./media/notification-hubs-kindle-get-started/add-assets-folder-menu.png)    
    6. Üzerinde **yapılandırma bileşen** sayfasında, aşağıdaki adımları uygulayın:
        1. Seçin **klasör konumunu değiştirme**
        2. Klasör değerine ayarlandığını doğrulayın: `src/main/assets`.
        3. **Son**’u seçin. 
        
            ![Varlık klasörünü yapılandırmak](./media/notification-hubs-kindle-get-started/configure-asset-folder.png)
2. Adlı bir dosya ekleyin **api_key.txt** için **varlıklar** klasör. Ağaç görünümünde genişletin **uygulama**, genişletin **src**, genişletin **ana**, sağ tıklayın ve **varlıklar**, işaret **yeni**ve ardından **dosya**. Girin **api_key.txt** dosya adı. 3. 
5. Amazon Geliştirici Portalı api_key.txt dosya için oluşturulan API anahtarını kopyalayın. 
6. Projeyi oluşturun. 

## <a name="run-the-app"></a>Uygulamayı çalıştırma
1. Kindle cihazında üstten çekin ve tıklayın **ayarları**ve ardından **Hesabımı** ve geçerli bir Amazon hesabıyla kaydolun.
2. Uygulamayı bir Kindle cihazında Android Studio'da çalıştırın. 

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

Örnek kod için bkz: [github'da Bu örnek](https://github.com/Azure/azure-notificationhubs-dotnet/blob/master/Samples/SendPushSample/SendPushSample/Program.cs).

![][7]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, arka uca kayıtlı olan tüm Kindle cihazlarınıza yayın bildirimleri gönderdiniz. Kindle belirli cihazlara anında iletme bildirimleri öğrenmek için aşağıdaki öğreticiye geçin:  Aşağıdaki öğreticide belirli Android cihazlarına anında iletme bildirimleri gösterilmektedir, ancak belirli Kindle cihazlarına bildirim göndermek için aynı mantığı kullanabilirsiniz.

> [!div class="nextstepaction"]
>[Belirli cihazlara anında iletme bildirimleri gönderme](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)

<!-- URLs. -->
[Amazon developer portal]: https://developer.amazon.com/home.html
[download the SDK]: https://developer.amazon.com/public/resources/development-tools/sdk
[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
