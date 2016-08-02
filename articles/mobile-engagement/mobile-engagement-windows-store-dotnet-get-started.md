<properties
    pageTitle="Windows Evrensel Uygulamaları için Azure Mobile Engagement kullanmaya başlama"
    description="Windows Evrensel Uygulamaları için analizler ve anında iletme bildirimleri ile Azure Mobile Engagement kullanmayı öğrenin."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/03/2016"
    ms.author="piyushjo" />

# Windows Evrensel Uygulamaları için Azure Mobile Engagement kullanmaya başlama

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Bu konuda, uygulama kullanımınızı anlamak ve bir Windows Evrensel uygulamasının segmentli kullanıcılarına anında iletme bildirimleri göndermek için nasıl Azure Mobile Engagement kullanılacağı gösterilmektedir.
Bu öğretici, Mobile Engagement kullanarak basit bir yayın senaryosunu gösterir. Temel uygulama kullanımı verilerini toplayan ve Windows Bildirim Hizmeti’ni (WNS) kullanarak anında iletme bildirimleri alan boş bir Windows Evrensel Uygulaması oluşturursunuz.

Bu öğretici için aşağıdakiler gereklidir:

+ Visual Studio 2013
+ [MicrosoftAzure.MobileEngagement] Nuget paketi

> [AZURE.NOTE] Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-store-dotnet-get-started).

##<a id="setup-azme"></a>Windows Evrensel uygulamanız için Mobile Engagement kurma

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Uygulamanızı Mobile Engagement arka ucuna bağlama

Bu öğreticide, veri toplamak ve anında iletme bildirimi göndermek için gerekli en küçük grup olan bir "temel tümleştirme" gösterilmektedir. Tümleştirme belgelerinin tamamı [Mobile Engagement Windows Evrensel SDK tümleştirmesi](mobile-engagement-windows-store-sdk-overview.md) makalesinde bulunabilir.

Tümleştirmeyi göstermek için Visual Studio ile temel bir uygulama oluşturacağız.

###Yeni bir Windows Evrensel Uygulama projesi oluşturma

Aşağıdaki adımlarda Visual Studio 2015 kullanıldığı varsayılmaktadır ancak Visual Studio'nun önceki sürümleri için de benzer adımlar izlenir. 

1. Visual Studio'yu başlatın ve **Giriş** ekranında **Yeni Proje**’yi seçin.

2. Açılır pencerede **Windows 8** -> **Evrensel** -> **Boş Uygulama (Evrensel Windows 8.1)**’yı seçin. **Ad** ve **Çözüm adı** alanlarını doldurup **Tamam**’a tıklayın.

    ![][1]

Azure Mobile Engagement SDK’sını tümleştireceğimiz yeni bir Windows Evrensel Uygulama projesi oluşturmuş oldunuz.

###Uygulamanızı Mobile Engagement arka ucuna bağlama

1. [MicrosoftAzure.MobileEngagement] nuget paketini projenize yükleyin. Windows ve Windows Phone platformlarının ikisini de hedefliyorsanız, bu işlemi her iki proje için de gerçekleştirmeniz gerekir. Windows 8.x ve Windows Phone 8.1 ile çalışırken aynı Nuget paketi, projelerin her birine platforma özel doğru ikili dosyaları yerleştirir.

2. **Package.appxmanifest** öğesini açın ve aşağıdaki özelliğin eklenmiş olduğundan emin olun.

        Internet (Client)

    ![][2]

3. Daha önce Mobile Engagement Uygulamanız için kopyaladığınız bağlantı dizesini kopyalayın ve bu dizeyi `Resources\EngagementConfiguration.xml` dosyasında `<connectionString>` ve `</connectionString>` etiketlerinin arasına yapıştırın:

    ![][3]

    >[AZURE.TIP] Uygulamanız, Windows ve Windows Phone platformlarının ikisini de hedefleyecekse, desteklenen her platform için birer tane olmak üzere iki adet Mobile Engagement Uygulaması oluşturmanız gerekir. Böylece, hedef kitlenizin segmentlere doğru olarak ayrılmasını sağlayabilir ve her platforma uygun şekilde hedefe yönelik bildirimler gönderebilirsiniz.

4. `App.xaml.cs` dosyasında:

    a. `using` deyimini ekleyin:

            using Microsoft.Azure.Engagement;

    b. Engagement’ın başlatılmasına ve ayarlanmasına adanmış bir yöntem ekleyin:

           private void InitEngagement(IActivatedEventArgs e)
           {
             EngagementAgent.Instance.Init(e);

             //... rest of the code
           }

    c. **OnLaunched** yönteminde SDK’yı başlatın:

            protected override void OnLaunched(LaunchActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of the code
            }

    c. Aşağıdakileri **OnActivated** yöntemine ekleyin, yöntem mevcut değilse yöntemi ekleyin:

            protected override void OnActivated(IActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of the code
            }

##<a id="monitor"></a>Gerçek zamanlı izlemeyi etkinleştirme

Veri göndermeye başlamak ve kullanıcıların etkin olduğundan emin olmak için, Mobile Engagement arka ucuna en az bir ekran (Etkinlik) göndermelisiniz.

1.  **MainPage.xaml.cs** dosyasında, `using` deyimini ekleyin:

        using Microsoft.Azure.Engagement.Overlay;

2. **MainPage** temel sınıfını **Page** yerine **EngagementPageOverlay** olarak değiştirin:

        class MainPage : EngagementPageOverlay

3. `MainPage.xaml` dosyasında:

    a. Ad alanı bildirimlerinize aşağıdakini ekleyin:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

    b. XML etiket adındaki **Page** yerine **engagement:EngagementPageOverlay** koyun
    
> [AZURE.IMPORTANT] Sayfanız `OnNavigatedTo` yöntemini geçersiz kılıyorsa `base.OnNavigatedTo(e)` öğesinin çağrıldığından emin olun. Aksi takdirde etkinlik bildirilmez (`EngagementPage`, `OnNavigatedTo` yönteminin içinde `StartActivity` öğesini çağırır). Bu uyarı, varsayılan şablonda bir `OnNavigatedTo` yöntemi bulunan Windows Phone projeleri için özellikle önemlidir. 

##<a id="monitor"></a>Uygulamayı gerçek zamanlı izlemeyle bağlama

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Anında iletme bildirimlerini ve uygulama içi mesajlaşmayı etkinleştirme

Mobile Engagement, kampanyalar bağlamında anında iletme bildirimleri ve uygulama içi mesajlaşma aracılığıyla kullanıcılarınız ile etkileşim kurmanızı ve onlara erişmenizi sağlar. Mobile Engagement portalında bu modüle REACH adı verilir.
Aşağıdaki bölümler bunları almak için uygulamanızı ayarlar.

###WNS Anında İletme Bildirimlerini almak için uygulamanızı etkinleştirme

1. `Package.appxmanifest` dosyasındaki **Uygulama** sekmesinde, **Bildirimler**’in altındaki **Toast capable:** (Bildirim özellikli) seçeneğini **Evet** olarak ayarlayın

    ![][5]

###REACH SDK'sını başlatma

`App.xaml.cs` dosyasındaki **InitEngagement** işlevi içinde, aracı başlatmadan hemen sonra **EngagementReach.Instance.Init(e);** öğesini çağırın:

        private void InitEngagement(IActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

Artık bildirim göndermeye hazırsınız. Şimdi bu temel tümleştirmeyi doğru bir şekilde gerçekleştirdiğinizi onaylayacağız.

###Bildirim göndermesi için Mobile Engagement’a erişim izni verme

1. Web tarayıcınızda [Windows Mağazası Geliştirme Merkezi]’ni açın, gerekiyorsa oturum açıp bir hesap oluşturun.
2. Sağ üst köşede **Pano**’ya ve ardından sol panel menüsünde **Yeni bir uygulama oluştur**’a tıklayın. 

    ![][9]

2. Adını ayırarak uygulamanızı oluşturun. 

    ![][10]

3. Uygulama oluşturulduktan sonra sol menüden **Hizmetler -> Anında iletme bildirimleri**’ne gidin.

    ![][11]

4. Anında iletme bildirimleri bölümünde **Live Services sitesi** bağlantısına tıklayın. 

    ![][12]

5. Gönderim kimlik bilgileri bölümüne yönlendirileceksiniz. **Uygulama Ayarları** bölümünde olduğunuzdan emin olun, **Paket SID'si** ve **Client secret** (Gizli Anahtar) değerlerinizi kopyalayın.

    ![][13]

6. Mobile Engagement portalınızda **Ayarlar**’a gidip sol tarafta **Yerel Gönderim** bölümüne tıklayın. Ardından, **Düzenle** düğmesine tıklayarak aşağıda gösterildiği şekilde **Paket güvenlik tanımlayıcısı (SID)** ve **Gizli Anahtar** değerlerinizi girin:

    ![][6]

8. Son olarak, Uygulama mağazasında Visual Studio uygulamanızı oluşturulan bu uygulama ile ilişkilendirdiğinizden emin olun. Bunu yapmak için Visual Studio’da **Associate App with Store** (Uygulamayı Mağaza ile İlişkilendir) seçeneğine tıklamanız gerekir.

    ![][7]

##<a id="send"></a>Uygulamanıza bildirim gönderme

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Uygulama çalışıyorsa bir uygulama içi bildirim görürsünüz, aksi takdirde uygulama kapalıysa bir bildirim görürsünüz. Uygulama içi bildirim görüyor ancak bildirim görmüyorsanız ve uygulamayı Visual Studio'da hata ayıklama modunda çalıştırıyorsanız, uygulamanın gerçekten askıya alındığından emin olmak için araç çubuğunda **Yaşam döngüsü olayları -> Askıya al** seçeneğini deneyin. Visual Studio'da uygulamanın hatalarını ayıklarken yalnızca Giriş düğmesine tıkladıysanız, uygulama her zaman askıya alınmaz ve bildirim, uygulama içi bildirim olarak görünür, ancak bildirim olarak gösterilmez.  

![][8]

<!-- URLs. -->
[Mobile Engagement Windows Evrensel SDK belgeleri]: ../mobile-engagement-windows-store-integrate-engagement/
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592
[Windows Mağazası Geliştirme Merkezi]: https://dev.windows.com
[Windows Evrensel Uygulamaları - Yer paylaşımlı tümleştirme]: ../mobile-engagement-windows-store-integrate-engagement-reach/#overlay-integration

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-store-dotnet-get-started/universal-app-creation.png
[2]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-capabilities.png
[3]: ./media/mobile-engagement-windows-store-dotnet-get-started/add-connection-info.png
[5]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-toast.png
[6]: ./media/mobile-engagement-windows-store-dotnet-get-started/enter-credentials.png
[7]: ./media/mobile-engagement-windows-store-dotnet-get-started/associate-app-store.png
[8]: ./media/mobile-engagement-windows-store-dotnet-get-started/vs-suspend.png
[9]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_create_app.png
[10]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_app_name.png
[11]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push.png
[12]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_1.png
[13]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_creds.png





<!---HONumber=Jun16_HO2-->


