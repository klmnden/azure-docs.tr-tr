<properties
    pageTitle="Baidu kullanarak Azure Notification Hubs ile çalışmaya başlama | Microsoft Azure"
    description="Bu öğreticide, Baidu kullanarak Android cihazlarına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenirsiniz."
    services="notification-hubs"
    documentationCenter="android"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="05/05/2016"
    ms.author="wesmc"/>

# Baidu kullanarak Azure Notification Hubs ile çalışmaya başlama

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##Genel Bakış

Baidu bulut anında iletme, mobil cihazlara anında iletme bildirimleri göndermede kullanabileceğiniz bir Çin bulut hizmetidir. Farklı uygulama mağazalarının ve anında iletme hizmetlerinin varlığı ve de genellikle GCM'ye (Google Cloud Messaging) bağlı olmayan Android cihazlarının kullanılabilirliği nedeniyle, bu hizmet özellikle Android'e anında iletme bildirimleri göndermenin karmaşık olduğu Çin'de kullanışlıdır.

##Önkoşullar

Bu öğretici için aşağıdakiler gereklidir:

+ <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android sitesinden indirebileceğiniz Android SDK'sı (Eclipse kullanacağınızı varsayıyoruz)</a>
+ [Mobile Services Android SDK'sı]
+ [Baidu Anında İletme Android SDK’sını]

>[AZURE.NOTE] Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).


##Bir Baidu hesabı oluşturma

Baidu kullanmak için bir Baidu hesabınızın olması gerekir. Zaten varsa [Baidu portalında] oturum açın ve sonraki adıma atlayın. Aksi halde, bir Baidu hesabının nasıl oluşturulacağı hakkında aşağıdaki yönergelere bakın.  

1. [Baidu portalında] gidin ve **登录** (**Oturum Açma**) bağlantısına tıklayın. Hesap kayıt işlemini başlatmak için **立即注册** seçeneğine tıklayın.

    ![][1]

2. Gerekli ayrıntıları girin (telefon/e-posta adresi, parola ve doğrulama kodu) ve **Kaydol**'a tıklayın.

    ![][2]

3. Girdiğiniz e-posta adresine, Baidu hesabınızı etkinleştirmek için bir bağlantıya sahip bir e-posta gönderilir.

    ![][3]

4. E-posta hesabınızda oturum açın, Baidu etkinleştirme e-postasını açın ve Baidu hesabınızı etkinleştirmek için etkinleştirme bağlantısına tıklayın.

    ![][4]

Baidu hesabınızı etkinleştirdikten sonra, [Baidu portalında] oturum açın.

##Bir Baidu geliştiricisi olarak kaydolma

1. [Baidu portalında] oturum açtıktan sonra, **更多>>** (**daha fazlası**) öğesine tıklayın.

    ![][5]

2. **站长与开发者服务 (Web Uzmanı ve Geliştirici Hizmetleri)** bölümde aşağı kaydırın ve **百度开放云平台** (**Baidu açık bulut platformu**) öğesine tıklayın.

    ![][6]

3. Sonraki sayfada sağ üst köşedeki **开发者服务** (**Geliştirici Hizmetleri**) öğesine tıklayın.

    ![][7]

4. Sonraki sayfada sağ üst köşedeki menüden **注册开发者** (**Kayıtlı Geliştiriciler**) öğesine tıklayın.

    ![][8]

5. Bir doğrulama kısa mesajı almak için adınızı, açıklamayı ve cep telefonu numarasını girin ve ardından **送验证码** (**Doğrulama Kodu Gönder**) öğesine tıklayın. Uluslararası telefon numaraları için ülke kodunu parantez içine almanız gerektiğini unutmayın. Örneğin, bir Amerika Birleşik Devletleri numarası için bu **(1) 1234567890** şeklinde olacaktır.

    ![][9]

6. Ardından, aşağıdaki örnekte gösterildiği gibi bir doğrulama numarasına sahip bir kısa mesaj almanız gerekir:

    ![][10]

7. İletideki doğrulama numarasını **验证码** (**Doğrulama kodu**) konumuna girin.

8. Son olarak, Baidu sözleşmesini kabul edip **提交** (**Gönder**) öğesine tıklayarak geliştirici kaydını tamamlayın. Kayıt başarılı bir şekilde tamamlandığında aşağıdaki sayfayı görürsünüz:

    ![][11]

##Bir Baidu bulut anında iletme projesi oluşturma

Bir Baidu bulut anında iletme projesi oluşturduğunuzda, uygulama kimliğinizi, API anahtarınızı ve gizli anahtarınızı alırsınız.

1. [Baidu portalında] oturum açtıktan sonra, **更多>>** (**daha fazlası**) öğesine tıklayın.

    ![][5]

2. **站长与开发者服务** (**Web Uzmanı ve Geliştirici Hizmetleri**) bölümde aşağı kaydırın ve **百度开放云平台** (**Baidu açık bulut platformu**) öğesine tıklayın.

    ![][6]

3. Sonraki sayfada sağ üst köşedeki **开发者服务** (**Geliştirici Hizmetleri**) öğesine tıklayın.

    ![][7]

4. Sonraki sayfada **云服务** (**Cloud Services**) bölümünden **云推送** (**Bulut Anında İletme**) öğesine tıklayın.

    ![][12]

5. Kayıtlı bir geliştirici olduğunuzda, en üstteki menüde **管理控制台** (**Yönetim Konsolu**) öğesini görürsünüz. **开发者服务管理** (**Geliştirici Hizmeti Yönetimi**) öğesine tıklayın.

    ![][13]

6. Sonraki sayfada **创建工程** (**Proje Oluşturma**) öğesine tıklayın.

    ![][14]

7. Bir uygulama adı girin ve **创建** (**Oluştur**) öğesine tıklayın.

    ![][15]

8. Bir Baidu bulut anında iletme projesinin başarılı bir şekilde oluşturulmasından sonra, **Uygulama Kimliği**, **API Anahtarı** ve **Gizli Anahtar** içeren bir sayfa görürsünüz. Daha sonra kullanacağımız API anahtarı ve gizli anahtarı not edin.

    ![][16]

9. Sol bölmedeki **云推送** (**Bulut Anında İletme**) öğesine tıklayarak projeyi anında iletme bildirimleri için yapılandırın.

    ![][31]

10. Sonraki sayfada **推送设置** (**Anında İletme ayarları**) düğmesine tıklayın.

    ![][32]  

11. Yapılandırma sayfasında Android projenizde kullanacağınız paket adını **应用包名** (**Uygulama paketi**) alanına ekleyin ve ardından **保存设置** (**Kaydet**) öğesine tıklayın.  

    ![][33]

**保存成功！** (**Başarıyla kaydedildi!**) iletisini görürsünüz.

##Bildirim hub'ınızı yapılandırma

1. [Klasik Azure Portalı]'nda oturum açın ve ardından ekranın alt kısmındaki **+YENİ**'ye tıklayın.

2. **Uygulama Hizmetleri**'ne tıklayın, **Service Bus**'a tıklayın, **Notification Hub**'a tıklayın ve ardından **Hızlı Oluştur**'a tıklayın.

3. **Notification Hub**'ınız için bir ad sağlayın, bu bildirim hub'ının oluşturulacağı **Bölge** ve **Ad Alanı**'nı seçin ve ardından **Yeni bir Notification Hub Oluştur**'a tıklayın.  

    ![][17]

4. Bildirim hub'ınızı oluşturduğunuz ad alanına tıklayın ve ardından üst kısımdaki **Notification Hubs**'a tıklayın.

    ![][18]

5. Oluşturduğunuz bildirim hub'ını seçin ve ardından, üstteki menüden **Yapılandır**'a tıklayın.

    ![][19]

6. **Baidu bildirim ayarları** bölümüne doğru aşağı kaydırın ve Baidu bulut anında iletme projeniz için önceden Baidu konsolundan elde ettiğiniz API anahtarını ve gizli anahtarı girin. **Kaydet** düğmesine tıklayın.

    ![][20]

7. Bildirim hub'ı için en üstteki **Pano** sekmesine tıklayın ve ardından **Bağlantı Dizesini Görüntüle**'ye tıklayın.

    ![][21]

8. **Erişim bağlantı bilgileri** penceresinde **DefaultListenSharedAccessSignature** ve **DefaultFullSharedAccessSignature**'ı not edin.

    ![][22]

##Uygulamanızı bildirim hub'ına bağlama

1. Eclipse ADT'de yeni bir Android projesi (**Dosya** > **Yeni** > **Android Uygulama Projesi**) oluşturun.

    ![][23]

2. Bir **Uygulama Adı** girin ve **Gereken Minimum SDK** sürümünün **API 16: Android 4.1** olarak ayarlandığından emin olun.

    ![][24]

3. **İleri**'ye tıklayın ve **Etkinlik Oluştur** penceresi görünene kadar sihirbazı izlemeye devam edin. **Blank Activity**'nin seçildiğinden emin olun ve son olarak yeni bir Android Uygulaması oluşturmak için **Son**'u seçin.

    ![][25]

4. **Proje Derleme Hedefi**'nin doğru ayarlandığından emin olun.

    ![][26]

5. [Bintray'deki Notification-Hubs-Android-SDK'sının](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4) **Dosyalar** sekmesinden notification-hubs-0.4.jar dosyasını indirin. Dosyaları Eclipse projenizin **libs** klasörüne ekleyin ve *libs* klasörünü yenileyin.

6. [Baidu Anında İletme Android SDK’sını] indirip sıkıştırmasını açın, **libs** klasörünü açın ve ardından Android uygulamanızdaki **libs** klasörüne **pushservice-x.y.z** jar dosyasını ve **armeabi** & **mips** klasörlerini kopyalayın.

7. Android projenizin **AndroidManifest.xml** dosyasını açın ve Baidu SDK'sı için gereken izinleri ekleyin.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. **AndroidManifest.xml**'deki **uygulama** öğenize **android:name** özelliğini ekleyip *yourprojectname*'i değiştirin (ör. **com.example.BaiduTest**). Bu proje adının, Baidu konsolunda yapılandırdığınız adla eşleştiğinden emin olun.

        <application android:name="yourprojectname.DemoApplication"

9. **.MainActivity** etkinlik öğesinden sonra uygulama öğesinin içine aşağıdaki yapılandırmayı ekleyip *yourprojectname*'i değiştirin (ör. **com.example.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. Projeye **ConfigurationSettings.java** adlı yeni bir sınıf ekleyin.

    ![][28]

    ![][29]

10. Buna aşağıdaki kodu ekleyin:

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    **API_KEY**'i daha önce Baidu bulut projesinden aldığınız değerle, **NotificationHubName**'i Klasik Azure Portalı'ndaki bildirim hub'ı adınızla ve **NotificationHubConnectionString**'i de Klasik Azure Portalı'ndaki DefaultListenSharedAccessSignature ile ayarlayın.

11. **DemoApplication.java** adlı yeni bir sınıf ekleyin ve bu sınıfa aşağıdaki kodu ekleyin:

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. **MyPushMessageReceiver.java** adlı başka bir yeni sınıf ekleyin ve bu sınıfa aşağıdaki kodu ekleyin. Bu, Baidu anında iletme sunucusundan alınan anında iletme bildirimlerini işleyen sınıftır.

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. **MainActivity.java**'yı açın ve aşağıdakini **onCreate** yöntemine ekleyin:

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. En üstteki içeri aktarma deyimlerini açın:

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##Uygulamanıza bildirimler gönderme


Aşağıdaki ekranda gösterildiği [Azure Portal](https://portal.azure.com/)'da bildirim hub’ındaki **Test Gönderimi** düğmesini kullanarak uygulamanızda bildirim almayı hızlıca test edebilirsiniz.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Anında iletme bildirimleri normalde, uyumlu bir kitaplık kullanılarak Mobile Services veya ASP.NET gibi bir arka uç hizmetinde gönderilir. Arka ucunuz için uygun bir kitaplık yoksa bildirim iletilerini göndermek için doğrudan REST API de kullanabilirsiniz.

Bu öğreticide konuyu basit bir şekilde işleyeceğiz ve yalnızca bir arka uç hizmeti yerine bir konsol uygulamasındaki bildirim hub'ları için .NET SDK ile bildirim göndererek istemci uygulamanızı test etmeyi göstereceğiz. Bir ASP.NET arka ucundan bildirim göndermek için sonraki adım olarak [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) öğreticisini öneririz. Bununla birlikte, bildirim göndermek için aşağıdaki yaklaşımlar kullanılabilir:

* **REST Arabirimi**: [REST arabirimini](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx) kullanarak herhangi bir arka uç platformunda bildirimi destekleyebilirsiniz.

* **Microsoft Azure Notification Hubs .NET SDK'sı**: Visual Studio için Nuget Paket Yöneticisi'nde [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) komutunu çalıştırın.

* **Node.js**: [Node.js'den Notification Hubs'ı kullanma](notification-hubs-nodejs-push-notification-tutorial.md).

* **Azure Mobile Services**: Notification Hubs ile tümleştirilmiş Azure Mobile Services arka ucundan nasıl bildirim gönderildiğinin bir örneği için bkz. [Mobile Services uygulamanıza anında iletme bildirimleri ekleme](../mobile-services/mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md).

* **Java/PHP**: REST API'ler kullanarak nasıl bildirim gönderildiğinin bir örneği için bkz. "Java/PHP'den Notification Hubs'ı kullanma"([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

##(İsteğe bağlı) Bir .NET konsol uygulamasından bildirim gönderme

Bu bölümde, bir .NET konsol uygulaması kullanarak bildirim göndermeyi göstereceğiz.

1. Yeni bir Visual C# konsol uygulaması oluşturun:

    ![][30]

2. Paket Yöneticisi Konsolu penceresinde, **Varsayılan projeyi** yeni konsol uygulaması projeniz olarak ayarlayın ve ardından konsol penceresinde aşağıdaki komutu yürütün:

        Install-Package Microsoft.Azure.NotificationHubs

    Bu, <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet paketini</a> kullanarak Azure Notification Hubs SDK'sına bir başvuru ekler.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. **Program.cs**'yi açın ve aşağıdaki kullanım deyimini ekleyin:

        using Microsoft.Azure.NotificationHubs;

4. `Program` sınıfınızda, aşağıdaki yöntemi ekleyin ve *DefaultFullSharedAccessSignatureSASConnectionString* ve *NotificationHubName* değerlerini sizdeki değerlerle değiştirin.

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. Aşağıdaki satırları **Main** yönteminize ekleyin:

         SendNotificationAsync();
         Console.ReadLine();

##Uygulamanızı test etme

Bu uygulamayı gerçek bir telefonla test etmek için telefonu bir USB kablosu kullanarak bilgisayarınıza bağlamanız yeterlidir. Böylece uygulamanız iliştirilmiş telefona yüklenir.

Bu uygulamayı öykünücüyle test etmek için, Eclipse üst araç çubuğunda **Çalıştır**'a tıklayın ve ardından uygulamanızı seçin. Bu, öykünücüyü başlatır ve ardından uygulamayı yükleyip çalıştırır.

Uygulama, Baidu Anında iletme bildirimi hizmetinden 'userId' ve 'channelId' alır ve bildirim hub'ı ile kaydeder.

Bir test bildirimi göndermek için, Klasik Azure Portalı'nın hata ayıklama sekmesini kullanabilirsiniz. Visual Studio için .NET konsol uygulamasını oluşturduysanız uygulamayı çalıştırmak için Visual Studio'da F5 tuşuna basmanız yeterlidir. Uygulama, cihazınızın veya öykünücünüzün bildirim alanının üst kısmında görünecek bir bildirim gönderir.


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Mobile Services Android SDK'sı]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu Anında İletme Android SDK’sını]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[Klasik Azure Portalı]: https://manage.windowsazure.com/
[Baidu portalında]: http://www.baidu.com/



<!--HONumber=Aug16_HO1-->


