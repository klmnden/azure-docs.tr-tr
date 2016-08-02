<properties
    pageTitle="Windows Phone Silverlight uygulamaları için Azure Mobile Engagement kullanmaya başlama"
    description="Windows Phone Silverlight uygulamaları için analizler ve anında iletme bildirimleri ile Azure Mobile Engagement kullanmayı öğrenin."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/03/2016"
    ms.author="piyushjo" />

# Windows Phone Silverlight uygulamaları için Azure Mobile Engagement kullanmaya başlama

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Bu konuda, uygulama kullanımınızı anlamak ve bir Windows Phone Silverlight uygulamasının segmentli kullanıcılarına anında iletme bildirimleri göndermek için nasıl Azure Mobile Engagement kullanılacağı gösterilmektedir.
Bu öğretici, Mobile Engagement kullanarak basit bir yayın senaryosunu gösterir. Temel verileri toplayan ve Microsoft Anında İletme Bildirimi Hizmeti’ni (MPNS) kullanarak anında iletme bildirimleri alan boş bir Windows Phone Silverlight uygulaması oluşturursunuz.

> [AZURE.NOTE] Windows Phone 8.1'i (Silverlight olmayan) hedefliyorsanız [Windows Evrensel öğreticisine](mobile-engagement-windows-store-dotnet-get-started.md) bakın.

Bu öğretici için aşağıdakiler gereklidir:

+ Visual Studio 2013
+ [MicrosoftAzure.MobileEngagement] Nuget paketi

> [AZURE.NOTE] Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).

##<a id="setup-azme"></a>Windows Phone uygulamanız için Mobile Engagement kurma

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Uygulamanızı Mobile Engagement arka ucuna bağlama

Bu öğreticide, veri toplamak ve anında iletme bildirimi göndermek için gerekli en küçük grup olan bir "temel tümleştirme" gösterilmektedir. Tümleştirme belgelerinin tamamı [Mobile Engagement Windows Phone SDK tümleştirmesi](mobile-engagement-windows-phone-sdk-overview.md) makalesinde bulunabilir.

Tümleştirmeyi göstermek için Visual Studio ile temel bir uygulama oluşturacağız.

###Yeni bir Windows Phone Silverlight projesi oluşturma

Aşağıdaki adımlarda Visual Studio 2015 kullanıldığı varsayılmaktadır ancak Visual Studio'nun önceki sürümleri için de benzer adımlar izlenir. 

1. Visual Studio'yu başlatın ve **Giriş** ekranında **Yeni Proje**’yi seçin.

2. Açılır pencerede **Windows 8** -> **Windows Phone** -> **Boş Uygulama (Windows Phone Silverlight)**’yı seçin. **Ad** ve **Çözüm adı** alanlarını doldurup **Tamam**’a tıklayın.

    ![][1]

3. Hedeflemek için **Windows Phone 8.0** veya **Windows Phone 8.1**’i seçebilirsiniz.

Azure Mobile Engagement SDK’sını tümleştireceğimiz yeni bir Windows Phone Silverlight uygulaması oluşturmuş oldunuz.

###Uygulamanızı Mobile Engagement arka ucuna bağlama

1. [MicrosoftAzure.MobileEngagement] nuget paketini projenize yükleyin.

2. `WMAppManifest.xml` dosyasını (Özellikler klasörünün altındadır) açıp `<Capabilities />` etiketinde aşağıdakilerin bildirildiğinden emin olun (bildirilmemişlerse bunları ekleyin):

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][2]

3. Daha önce Mobile Engagement uygulamanız için kopyaladığınız bağlantı dizesini kopyalayın ve bu dizeyi `Resources\EngagementConfiguration.xml` dosyasında `<connectionString>` ve `</connectionString>` etiketlerinin arasına yapıştırın:

    ![][3]

4. `App.xaml.cs` dosyasında:

    a. `using` deyimini ekleyin:

            using Microsoft.Azure.Engagement;

    b. `Application_Launching` yönteminde SDK’yı başlatın:

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c. `Application_Activated` bölümüne aşağıdakileri ekleyin:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

##<a id="monitor"></a>Gerçek zamanlı izlemeyi etkinleştirme

Veri göndermeye başlamak ve kullanıcıların etkin olduğundan emin olmak için, Mobile Engagement arka ucuna en az bir ekran (Etkinlik) göndermelisiniz.

1. MainPage.xaml.cs dosyasında, `using` deyimini ekleyin:

        using Microsoft.Azure.Engagement;

2. **PhoneApplicationPage** olan **MainPage** temel sınıfını **EngagementPage** olarak değiştirin.

        class MainPage : EngagementPage 
    
3. `MainPage.xml` dosyanızda:

    a. Ad alanı bildirimlerinize aşağıdakini ekleyin:

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    b. XML etiket adında `phone:PhoneApplicationPage` yerine `engagement:EngagementPage` koyun.

##<a id="monitor"></a>Uygulamayı gerçek zamanlı izlemeyle bağlama

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Anında iletme bildirimlerini ve uygulama içi mesajlaşmayı etkinleştirme

Mobile Engagement, kampanyalar bağlamında Anında İletme Bildirimleri ve uygulama içi Mesajlaşma aracılığıyla kullanıcılarınız ile etkileşim kurmanızı ve onlara erişmenizi sağlar. Mobile Engagement portalında bu modüle REACH adı verilir.
Aşağıdaki bölümler bunları almak için uygulamanızı ayarlar.

###MPNS Anında İletme Bildirimlerini almak üzere uygulamanızı etkinleştirme

`WMAppManifest.xml` dosyanıza yeni Capabilities (Özellikler) ekleyin:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

###REACH SDK'sını başlatma

1. `App.xaml.cs` dosyasındaki **Application_Launching** işlevinde, aracı başlatmadan hemen sonra `EngagementReach.Instance.Init();` öğesini çağırın:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. `App.xaml.cs` dosyasındaki **Application_Activated** işlevinde, aracı başlatmadan hemen sonra `EngagementReach.Instance.OnActivated(e);` öğesini çağırın:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

İşinizi tamamlandı. Şimdi bu temel tümleştirmeyi doğru bir şekilde gerçekleştirdiğinizi onaylayacağız.

##<a id="send"></a>Uygulamanıza bildirim gönderme

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Şimdi cihazınızda, uygulama açıksa bir uygulama içi bildirim, aksi takdirde aşağıdaki gibi bir bildirim görmeniz gerekir: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK belgeleri]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png



<!--HONumber=Jun16_HO2-->


