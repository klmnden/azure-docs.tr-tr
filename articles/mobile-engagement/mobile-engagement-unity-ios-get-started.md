<properties
    pageTitle="Unity iOS dağıtımı için Azure Mobile Engagement kullanmaya başlama"
    description="Unity uygulamalarını iOS cihazlarına dağıtmak için Analizler ve Anında İletme Bildirimleri ile Azure Mobile Engagement kullanmayı öğrenin."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager=""
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="03/25/2016"
    ms.author="piyushjo" />

# Unity iOS dağıtımı için Azure Mobile Engagement kullanmaya başlama

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Bu konu size uygulama kullanımınızı anlamak için Azure Mobile Engagement kullanmayı ve iOS cihazına dağıtırken bir Unity uygulamasının kesimli kullanıcılarına anında iletme bildirimleri göndermeyi gösterir.
Bu öğretici, başlangıç noktası olarak bir Küre öğretici olarak klasik Unity Roll kullanır. Aşağıdaki öğreticide gösterdiğimiz Mobile Engagement tümleştirmesine geçmeden önce bu [öğreticideki](mobile-engagement-unity-roll-a-ball.md) adımları izlemelisiniz. 

Bu öğretici için aşağıdakiler gereklidir:

+ [Unity Düzenleyicisi](http://unity3d.com/get-unity)
+ [Mobile Engagement Unity SDK](https://aka.ms/azmeunitysdk)
+ XCode Düzenleyicisi

> [AZURE.NOTE] Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).

##<a id="setup-azme"></a>iOS uygulamanız için Mobile Engagement kurma

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Uygulamanızı Mobile Engagement arka ucuna bağlama

###Unity paketini içeri aktarma

1. [Mobile Engagement Unity paketini](https://aka.ms/azmeunitysdk) indirin ve yerel makinenize kaydedin. 

2. **Varlıklar -> Paketi İçeri Aktar -> Özel Paket**’e gidin ve yukarıdaki adımda indirdiğiniz paketi seçin. 

    ![][70] 

3. Tüm dosyaların seçildiğinden emin olun ve **İçeri Aktar** düğmesine tıklayın. 

    ![][71] 

4. İçeri aktarma başarılı olduktan sonra, içeri aktarılan SDK dosyalarını projenizde görürsünüz.  

    ![][72] 

###EngagementConfiguration’ı güncelleştirme

1. SDK klasöründe **EngagementConfiguration** betik dosyasını açın ve daha önce Azure portaldan aldığınız bağlantı dizesiyle **IOS\_CONNECTION\_STRING**’i güncelleştirin.  

    ![][73]

2. Dosyayı kaydedin. 

###Uygulamayı temel izleme için yapılandırma

1. Düzenlemek için Player nesnesine ekli olan **PlayerController** betiğini açın. 

2. Şu deyimi kullanarak aşağıdakileri ekleyin:

        using Microsoft.Azure.Engagement.Unity;

3. Aşağıdakileri `Start()` yöntemine ekleyin.
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###Uygulamayı dağıtma ve çalıştırma

1. Bir iOS cihazı makinenize bağlayın. 

2. **Dosya -> Yapı Ayarları**’nı açın 

    ![][40]

3. **iOS**’u seçin ve ardından **Anahtar Platformu**’na tıklayın

    ![][41]

    ![][42]

4. **Player ayarları**’na tıklayın ve geçerli bir Paket Tanımlayıcısı girin. 

    ![][53]

5. Son olarak, **Derle ve Çalıştır**’a tıklayın.

    ![][54]

6. iOS paketini depolamak için bir klasör adı sağlamanız istenebilir. 

    ![][43]

7. Her şey yolunda giderse, proje derlenir ve XCode uygulamanızda açabilmelisiniz. 

8. **Paket Tanımlayıcısı**’nın projede doğru olduğundan emin olun.  

    ![][75]

10. Şimdi uygulamayı XCode’^da çalıştırın, böylece paket bağlı cihazınıza dağıtılır ve telefonunuzda Unity oyununuzu görmelisiniz. 

##<a id="monitor"></a>Uygulamayı gerçek zamanlı izlemeyle bağlama

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Anında iletme bildirimlerini ve uygulama içi mesajlaşmayı etkinleştirme

Mobile Engagement, kampanyalar bağlamında anında iletme bildirimleri ve uygulama içi mesajlaşma ile kullanıcılarınız ve REACH ile etkileşim kurmanızı sağlar. Mobile Engagement portalında bu modüle REACH adı verilir.
Bildirimleri almak için uygulamanızda herhangi bir ek yapılandırma yapmanız gerekmez ve zaten ayarlıdır.

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png


<!----HONumber=Jun16_HO2-->


