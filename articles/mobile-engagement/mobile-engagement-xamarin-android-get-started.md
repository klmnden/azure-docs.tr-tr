<properties
    pageTitle="Xamarin.Android için Azure Mobile Engagement kullanmaya başlama"
    description="Xamarin.Android Uygulamaları için Analizler ve Anında İletme Bildirimleri ile Azure Mobile Engagement kullanmayı öğrenin."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager=""
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/16/2016"
    ms.author="piyushjo" />


# Xamarin.Android Uygulamaları için Azure Mobile Engagement kullanmaya başlama

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Bu konu size uygulama kullanımınızı anlamak için Azure Mobile Engagement kullanmayı ve Xamarin.Android uygulamasının kesimli kullanıcılarına anında iletme bildirimleri göndermeyi gösterir.
Bu öğretici, Mobile Engagement kullanarak basit bir yayın senaryosunu gösterir. Öğreticide, temel bilgiler toplayan ve Google Cloud Messaging (GCM) kullanarak anında iletme bildirimleri gönderen, boş bir Xamarin.Android uygulaması oluşturursunuz.

Bu öğretici için aşağıdakiler gereklidir:

+ [Xamarin Studio](http://xamarin.com/studio). Xamarin ile Visual Studio’yu da kullanabilirsiniz, ancak bu öğretici Xamarin Studio'yu kullanır. Yükleme yönergeleri için bkz. [Visual Studio ve Xamarin için Kurulum ve Yükleme](https://msdn.microsoft.com/library/mt613162.aspx).
+ [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).

##<a id="setup-azme"></a>Android uygulamanız için Mobile Engagement kurma

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Uygulamanızı Mobile Engagement arka ucuna bağlama

Bu öğreticide, veri toplamak ve anında iletme bildirimi göndermek için gerekli en küçük grup olan bir "temel tümleştirme" gösterilmektedir. 

Tümleştirmeyi göstermek için Xamarin Studio ile temel bir uygulama oluşturacağız.

###Yeni bir Xamarin.Android projesi oluşturma

1. **Xamarin Studio**’yu başlatın **Dosya** -> **Yeni** -> **Çözüm**’e gidin. 

    ![][1]

2. **Android Uygulaması**’nı seçin ve ardından seçilen dilin **C#** olduğundan emin olun ve tıklatıp **Sonraki**’ye tıklayın.

    ![][2]

3. **Uygulama Adı** ve **Kuruluş Tanımlayıcı**’yı doldurun. **Google Play Hizmetleri**’ni işaretlediğinizden emin olun ve ardından **Sonraki**’ye tıklayın. 

    ![][3]
    
4. Gerekliyse **Proje Adı**, **Çözüm Adı** ve **Konumu** güncelleştirin ve **Oluştur**’a tıklayın.

    ![][4]
 
Xamarin Studio, Mobile Engagement’ı tümleştireceğimiz uygulamayı oluşturur. 

###Uygulamanızı Mobile Engagement arka ucuna bağlama

1. Çözüm penceresinde **Paketler**’e sağ tıklayın ve **Paketleri Ekle...** öğesini seçin.

    ![][5]

2. **Microsoft Azure Mobile Engagement Xamarin SDK**’yı arayın ve çözümünüze ekleyin.  

    ![][6]
   
3. **MainActivity.cs**’yi açın ve şu deyimleri kullanarak aşağıdakileri ekleyin:

        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;

4. `OnCreate` yönteminde, Mobile Engagement arka ucuyla bağlantıyı başlatmak için aşağıdakileri ekleyin. **ConnectionString**’inizi eklediğinizden emin olun. 

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

###İzinler ve bir hizmet bildirimi ekleme

1. Özellikleri klasörü altında **Manifest.xml** dosyasını açın. XML kaynağını doğrudan güncelleştirecek şekilde Kaynak sekmesini seçin.
 
2. Bu izinleri projenizin Manifest.xml dosyasına (**Özellikler** klasörü altında bulunabilir) `<application>` etiketinin önüne ya da arkasına ekleyin:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

3. `<application>` ve `</application>` etiketleri arasına aşağıdakileri ekleyerek aracı hizmetini bildirin:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

4. Yapıştırdığınız kodda, etiketteki `"<Your application name>"` öğesini değiştirin. Bu etiket, kullanıcıların cihazda çalışan hizmetleri görebileceği **Ayarlar** menüsünde görüntülenir. Örneğin bu etikete "Hizmet" sözcüğünü ekleyebilirsiniz.

###Bir ekranı Mobile Engagement’a gönderme

Verileri göndermeye başlamak ve kullanıcıların etkin olduğundan emin olmak için, Mobile Engagement arka ucuna en az bir ekran göndermelisiniz. Bunu yapmak için, `MainActivity` öğesinin `Activity` yerine `EngagementActivity` öğesinden devraldığından emin olun.

    public class MainActivity : EngagementActivity
    
Alternatif olarak, `EngagementActivity` konumundan devralamıyorsanız `.StartActivity` ve `.EndActivity` yöntemlerini sırasıyla `OnResume` ve `OnPause` içine eklemeniz gerekir.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }
    
            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

##<a id="monitor"></a>Uygulamayı gerçek zamanlı izlemeyle bağlama

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Anında iletme bildirimlerini ve uygulama içi mesajlaşmayı etkinleştirme

Mobile Engagement, kampanyalar bağlamında anında iletme bildirimleri ve uygulama içi mesajlaşma ile kullanıcılarınız ve REACH ile etkileşim kurmanızı sağlar. Mobile Engagement portalında bu modüle REACH adı verilir.
Aşağıdaki bölümler, uygulamanızı bu bildirim ve mesajları alacak şekilde ayarlar.

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png



<!--HONumber=Sep16_HO3-->


