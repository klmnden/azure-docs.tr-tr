<properties
    pageTitle="Windows Evrensel Uygulamaları Azure Mobile Engagement kullanmaya başlayın"
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
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# Windows Evrensel Uygulamaları için Azure Mobile Engagement kullanmaya başlama

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Bu konuda, uygulama kullanımınızı anlamak ve bir Windows Evrensel uygulamasının segmentli kullanıcılarına anında iletme bildirimleri göndermek için nasıl Azure Mobile Engagement kullanılacağı gösterilmektedir.
Bu öğretici, Mobile Engagement kullanarak basit bir yayın senaryosunu gösterir. Temel uygulama kullanımı verilerini toplayan ve Windows Bildirim Hizmeti’ni (WNS) kullanarak anında iletme bildirimleri alan boş bir Windows Evrensel Uygulaması oluşturursunuz.

## Ön koşullar

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]


## Windows Evrensel uygulamanız için Mobile Engagement kurma

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Uygulamanızı Mobile Engagement arka ucuna bağlama

Bu öğreticide, veri toplamak ve anında iletme bildirimi göndermek için gerekli en küçük grup olan bir "temel tümleştirme" gösterilmektedir. Tümleştirme belgelerinin tamamı [Mobile Engagement Windows Evrensel SDK tümleştirmesi](mobile-engagement-windows-store-sdk-overview.md) makalesinde bulunabilir.

Tümleştirmeyi göstermek için Visual Studio ile temel bir uygulama oluşturursunuz.

###Bir Windows Evrensel Uygulaması projesi oluşturma

Aşağıdaki adımlarda Visual Studio 2015 kullanıldığı varsayılmaktadır ancak Visual Studio'nun önceki sürümleri için de benzer adımlar izlenir.

1. Visual Studio'yu başlatın ve **Giriş** ekranında **Yeni Proje**’yi seçin.

2. Açılır pencerede**Windows** -> **Evrensel** -> **Boş Uygulama (Evensel Windows)** seçeneğini belirleyin. **Ad** ve **Çözüm adı** alanlarını doldurup **Tamam**’a tıklayın.

    ![][1]

Bir sonraki adımda Azure Mobile Engagement SDK’sını tümleştireceğiniz bir Windows Evrensel Uygulama projesi oluşturmuş oldunuz.

###Uygulamanızı Mobile Engagement arka ucuna bağlama

1. [MicrosoftAzure.MobileEngagement] Nuget paketini projenize yükleyin. Windows ve Windows Phone platformlarının ikisini de hedefliyorsanız, bu işlemi her iki proje için de gerçekleştirmeniz gerekir. Windows 8.x ve Windows Phone 8.1 ile çalışırken aynı Nuget paketi tarafından projelerin her birine platforma özel doğru ikili dosyalar yerleştirilir.

2. **Package.appxmanifest** öğesini açın ve aşağıdaki özelliğin eklenmiş olduğundan emin olun.

        Internet (Client)

    ![][2]

3. Daha önce Mobile Engagement Uygulamanız için kopyaladığınız bağlantı dizesini kopyalayın ve bu dizeyi `Resources\EngagementConfiguration.xml` dosyasında `<connectionString>` ve `</connectionString>` etiketlerinin arasına yapıştırın:

    ![][3]

    >[AZURE.TIP] Uygulamanız, Windows ve Windows Phone platformlarının ikisini de hedefliyorsa, desteklenen her platform için birer adet olmak üzere iki adet Mobile Engagement Uygulaması oluşturmanız gerekir. İki uygulamanızın olması, hedef kitlenizi doğru bir şekilde bölümlendirebildiğinizden ve her platform için uygun şekilde hedefe yönelik bildirimler gönderebildiğinizden emin olmanızı sağlar.

4. `App.xaml.cs` dosyasında:

    a. `using` deyimini ekleyin:

            using Microsoft.Azure.Engagement;

    b. Engagement’ı başlatan bir yöntem ekleyin:

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

2. **MainPage** öğesinin **Page** temel sınıfını **EngagementPageOverlay** olarak değiştirin:

        class MainPage : EngagementPageOverlay

3. `MainPage.xaml` dosyasında:

    a. Ad alanı bildirimlerinize aşağıdakini ekleyin:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

    b. XML etiket adındaki **Page** yerine **engagement:EngagementPageOverlay** koyun

> [AZURE.IMPORTANT] Sayfanız `OnNavigatedTo` yöntemini geçersiz kılıyorsa `base.OnNavigatedTo(e)` öğesini çağırdığınızdan emin olun. Aksi takdirde, etkinlik bildirilmez (`EngagementPage`, `OnNavigatedTo` yönteminin içinde `StartActivity` öğesini çağırır). Bu uyarı, varsayılan şablonda bir `OnNavigatedTo` yöntemi bulunan Windows Phone projeleri için özellikle önemlidir.

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

Bir bildirim göndermeye hazırsınız. Şimdi bu temel tümleştirmeyi doğru bir şekilde gerçekleştirdiğinizi doğrulayacağız.

###Bildirim göndermesi için Mobile Engagement’a erişim izni verme

1. Web tarayıcınızda [Windows Mağazası Geliştirme Merkezi]’ni açın, oturum açın ve gerekiyorsa bir hesap oluşturun.
2. Sağ üst köşede **Pano**’ya ve ardından sol panel menüsünde **Yeni bir uygulama oluştur**’a tıklayın.

    ![][9]

2. Adını ayırarak uygulamanızı oluşturun.

    ![][10]

3. Uygulama oluşturulduktan sonra sol menüden **Hizmetler -> Anında iletme bildirimleri**’ne gidin.

    ![][11]

4. Anında iletme bildirimleri bölümünde **Live Services sitesi** bağlantısına tıklayın.

    ![][12]

5. Gönderim kimlik bilgileri bölümüne gidin. **Uygulama Ayarları** bölümünde olduğunuzdan emin olun, **Paket SID'si** ve **Client secret** (Gizli Anahtar) değerlerinizi kopyalayın.

    ![][13]

6. Mobile Engagement portalınızda **Ayarlar**’a gidip sol tarafta **Yerel Gönderim** bölümüne tıklayın. Ardından, **Düzenle** düğmesine tıklayarak burada gösterildiği şekilde **Paket güvenlik tanımlayıcısı (SID)** ve **Gizli Anahtar** değerlerinizi girin:

    ![][6]

8. Son olarak, Uygulama mağazasında Visual Studio uygulamanızı oluşturulan bu uygulama ile ilişkilendirdiğinizden emin olun. Visual Studio’da **Uygulamayı Mağaza ile İlişkilendir**’e tıklayın.
    ![][7]

##<a id="send"></a>Uygulamanıza bildirim gönderme

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Uygulama çalışıyorsa uygulama içi bir bildirim görürsünüz. Uygulama kapalıysa bir bildirim görürsünüz.
Uygulama içi bildirim görür ancak bildirim görmezseniz ve uygulamayı Visual Studio'da hata ayıklama modunda çalıştırıyorsanız, uygulamanın askıya alındığından emin olmak için araç çubuğunda **Yaşam döngüsü olayları -> Askıya al** seçeneğini deneyin. Visual Studio'da uygulamanın hatalarını ayıklarken Giriş düğmesine tıkladıysanız, uygulama her zaman askıya alınmaz ve bildirimi uygulama içi bildirim olarak görmenize rağmen bildirim olarak göremezsiniz.  

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



<!--HONumber=Aug16_HO4-->


