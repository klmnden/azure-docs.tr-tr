<properties
    pageTitle="Android Uygulamaları Azure Mobile Engagement kullanmaya başlama"
    description="Android uygulamaları için analizler ve anında iletme bildirimleri ile Azure Mobile Engagement kullanmayı öğrenin."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />


# Android uygulamaları için Azure Mobile Engagement kullanmaya başlama

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Bu konuda, uygulama kullanımınızı anlamak için nasıl Azure Mobile Engagement kullanılacağı ve bir Android uygulamasının segmentli kullanıcılarına nasıl anında iletme bildirimleri gönderileceği gösterilmektedir.
Bu öğretici, Mobile Engagement kullanarak basit bir yayın senaryosunu gösterir. Öğreticide, temel bilgiler toplayan ve Google Cloud Messaging (GCM) kullanarak anında iletme bildirimleri alan, boş bir Android uygulaması oluşturursunuz.

## Ön koşullar

Bu öğreticiyi tamamlamak için Android Studio tümleşik geliştirme ortamını ve en son Android platformunu içeren [Android Geliştirici Araçları](https://developer.android.com/sdk/index.html) gerekir.

Ayrıca [Mobile Engagement Android SDK](https://aka.ms/vq9mfn) gereklidir.

> [AZURE.IMPORTANT] Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).

## Android uygulamanız için Mobile Engagement kurma

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## Uygulamanızı Mobile Engagement arka ucuna bağlama

Bu öğreticide, veri toplamak ve anında iletme bildirimi göndermek için gerekli en küçük grup olan bir "temel tümleştirme" gösterilmektedir. Tümleştirmeyi göstermek için Android Studio ile temel bir uygulama oluşturursunuz.

Tümleştirme belgelerinin tamamı [Mobile Engagement Android SDK tümleştirmesi](mobile-engagement-android-sdk-overview.md)’nde bulunabilir.

### Android projesi oluşturma

1. **Android Studio**’yu başlatın ve açılır menüde **Yeni bir Android Studio projesi başlat**’ı seçin.

    ![][1]

2. Uygulama adı ve şirket etki alanı belirtin. Girdiğiniz bilgileri daha sonra kullanmanız gerekeceğinden bunları not edin. **Next (İleri)** düğmesine tıklayın.

    ![][2]

3. Hedef form faktörünü ve API düzeyini seçip **İleri**'ye tıklayın.

    >[AZURE.NOTE] Mobile Engagement, en az API düzey 10 (Android 2.3.3) gerektirir.

    ![][3]

4. Burada, bu uygulama için tek ekran olan **Boş Etkinlik**’i seçip **İleri**'ye tıklayın.

    ![][4]

5. Son olarak, varsayılan değerleri olduğu gibi bırakıp **Son**'a tıklayın.

    ![][5]

Şimdi Android Studio, Mobile Engagement’ı tümleştirdiğimiz tanıtım uygulamasını oluşturur.

### SDK kitaplığını projenize ekleme

1. [Mobile Engagement Android SDK](https://aka.ms/vq9mfn)’yı indirin.
2. Arşiv dosyasını bilgisayarınızdaki bir klasöre ayıklayın.
3. Bu SDK'nın geçerli sürümüne ait .jar kitaplığını belirleyin ve Pano’ya kopyalayın.

      ![][6]

4. **Proje** bölümüne gidin (1) ve .jar kitaplığını libs klasörüne yapıştırın (2).

      ![][7]

5. Kitaplığı yüklemek için projeyi eşitleyin.

      ![][8]

### Uygulamanızı Bağlantı Dizesi ile Mobile Engagement arka ucuna bağlama

1. Aşağıdaki kod satırlarını etkinlik oluşturmaya kopyalayın (uygulamanızın tek bir noktasında yapılmalıdır, bu nokta genellikle ana etkinliktir). Bu örnek uygulama için, src -> main -> java klasörü altında MainActivity dosyasını açın ve aşağıdakileri ekleyin:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. Alt + Enter tuşlarına basarak veya aşağıdaki içeri aktarma deyimlerini ekleyerek başvuruları çözümleyin:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Uygulamanızın**Bağlantı Bilgileri** sayfasında Klasik Azure Portalı’na geri gidin ve **Bağlantı Dizesi**’ni kopyalayın.

      ![][9]

4. `setConnectionString` parametresine yapıştırarak aşağıdaki kodda gösterilen dizenin tamamını değiştirin:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### İzinler ve bir hizmet bildirimi ekleme

1. Bu izinleri, projenizin Manifest.xml dosyasında `<application>` etiketinin hemen önüne ya da arkasına ekleyin:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. Aracı hizmetini bildirmek için `<application>` ve `</application>` etiketleri arasına şu kodu ekleyin:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. Yapıştırdığınız kodda, cihazda çalışmakta olan hizmetleri görebileceğiniz **Ayarlar** menüsünde görüntülenen etiketteki `"<Your application name>"` değerini değiştirin. Örneğin bu etikete "Hizmet" sözcüğünü ekleyebilirsiniz.

### Bir ekranı Mobile Engagement’a gönderme

Veri göndermeye başlamak ve kullanıcıların etkin olduğundan emin olmak için, Mobile Engagement arka ucuna en az bir ekran (Etkinlik) göndermelisiniz.

**MainActivity.java** dosyasına gidip aşağıdakileri ekleyerek **MainActivity** temel sınıfını **EngagementActivity** olarak değiştirin:

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Temel sınıfınız *Etkinlik* değilse farklı sınıflardan nasıl devralınacağı konusunda [Gelişmiş Android Raporlama](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes)’ya başvurun.


Bu basit örnek senaryo için aşağıdaki satırı açıklama satırı yapın:

    // setSupportActionBar(toolbar);

Uygulamanızda `ActionBar` öğesini tutmak istiyorsanız [Gelişmiş Android Raporlama](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) konusuna bakın.

## Uygulamayı gerçek zamanlı izlemeyle bağlama

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## Anında iletme bildirimlerini ve uygulama içi mesajlaşmayı etkinleştirme

Mobile Engagement, bir kampanya sırasında anında iletme bildirimleri ve uygulama içi mesajlaşma ile kullanıcılarınızla etkileşim kurmanızı ve REACH ile kullanıcılarınıza ulaşmanızı sağlar. Mobile Engagement portalında bu modüle REACH adı verilir.
Aşağıdaki bölüm, uygulamanızı bu bildirim ve mesajları alacak şekilde ayarlar.

### SDK kaynaklarını projenize kopyalama

1. İndirilen SDK içeriğinize geri gidip **res** klasörünü kopyalayın.

    ![][10]

2. Android Studio’ya geri gidip proje dosyalarınıza ait **ana** dizini seçin ve kopyaladığınız klasörü buraya yapıştırarak kaynakları projenize ekleyin.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## Sonraki adımlar

SDK tümleştirmesi hakkında ayrıntılı bilgi edinmek için [Android SDK](mobile-engagement-android-sdk-overview.md) sayfasına gidin.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png



<!--HONumber=Oct16_HO3-->


