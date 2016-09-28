<properties
    pageTitle="Xamarin.iOS için Azure Mobile Engagement kullanmaya başlama"
    description="Xamarin.iOS Uygulamaları için Analizler ve Anında İletme Bildirimleri ile Azure Mobile Engagement kullanmayı öğrenin."
    services="mobile-engagement"
    documentationCenter="xamarin"
    authors="piyushjo"
    manager=""
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


# Xamarin.iOS Uygulamaları için Azure Mobile Engagement kullanmaya başlama

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Bu konu size uygulama kullanımınızı anlamak ve Xamarin.iOS uygulamasının kesimli kullanıcılarına anında iletme bildirimleri göndermek için Azure Mobile Engagement kullanmayı gösterir.
Bu öğreticide, temel verileri toplayan ve Apple Anında İletme Bildirimi Sistemi’ni (APNS) kullanarak anında iletme bildirimleri alan boş bir Xamarin.iOS uygulaması oluşturursunuz.

Bu öğretici için aşağıdakiler gereklidir:

+ [Xamarin Studio](http://xamarin.com/studio). Xamarin ile Visual Studio’yu da kullanabilirsiniz, ancak bu öğretici Xamarin Studio'yu kullanır. Yükleme yönergeleri için bkz. [Visual Studio ve Xamarin için Kurulum ve Yükleme](https://msdn.microsoft.com/library/mt613162.aspx). 
+ [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [AZURE.NOTE] Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-ios-get-started).

##<a id="setup-azme"></a>iOS uygulamanız için Mobile Engagement kurma

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Uygulamanızı Mobile Engagement arka ucuna bağlama

Bu öğreticide, veri toplamak ve anında iletme bildirimi göndermek için gerekli en küçük grup olan bir "temel tümleştirme" gösterilmektedir.

Tümleştirmeyi göstermek için Xamarin ile temel bir uygulama oluşturacağız.

###Yeni bir Xamarin.iOS projesi oluşturma

1. Xamarin Studio’yu başlatın. **Dosya** -> **Yeni** -> **Çözüm**’e gidin. 

    ![][1]

2. **Single View Uygulaması**’nı seçin ve ardından seçilen dilin **C#** olduğundan emin olun ve tıklatıp **Sonraki**’ye tıklayın.

    ![][2]

3. **Uygulama Adı** ve **Kuruluş Tanımlayıcı**’yı doldurun ve ardından **Sonraki**’ye tıklayın. 

    ![][3]

    > [AZURE.IMPORTANT] Sonunda iOS uygulamanızı dağıtmak için kullandığınız yayımlama profilinin, burada sahip olduğunuz Paket Tanımlayıcısı ile tam olarak eşleşen bir Uygulama Kimliği kullandığından emin olun. 

4. Gerekliyse **Proje Adı**, **Çözüm Adı** ve **Konumu** güncelleştirin ve **Oluştur**’a tıklayın.

    ![][4]
 
Xamarin Studio, Mobile Engagement’ı tümleştireceğimiz demo uygulamayı oluşturur. 

###Uygulamanızı Mobile Engagement arka ucuna bağlama

1. Çözüm penceresinde **Paketler**’e sağ tıklayın ve **Paketleri Ekle...** öğesini seçin.

    ![][5]

2. **Microsoft Azure Mobile Engagement Xamarin SDK**’yı arayın ve çözümünüze ekleyin.  

    ![][6]
   
3. **AppDelegate.cs**’yi açın ve şu deyimleri kullanarak aşağıdakileri ekleyin:

        using Microsoft.Azure.Engagement.Xamarin;

4. **FinishedLaunching** yönteminde, Mobile Engagement arka ucuyla bağlantıyı başlatmak için aşağıdakileri ekleyin. **ConnectionString**’inizi eklediğinizden emin olun. Bu kodu aynı zamanda, değiştirmek isteyebileceğiniz Mobile Engagement SDK tarafından eklenen kukla bir **NotificationIcon** kullanır. 

        EngagementConfiguration config = new EngagementConfiguration {
                        ConnectionString = "YourConnectionStringFromAzurePortal",
                        NotificationIcon = UIImage.FromBundle("close")
                    };
        EngagementAgent.Init (config);

##<a id="monitor"></a>Gerçek zamanlı izlemeyi etkinleştirme

Verileri göndermeye başlamak ve kullanıcıların etkin olduğundan emin olmak için, Mobile Engagement arka ucuna en az bir ekran göndermelisiniz.

1. **ViewController.cs**’yi açın ve şu deyimleri kullanarak aşağıdakileri ekleyin:

        using Microsoft.Azure.Engagement.Xamarin;

2. `ViewController` öğesinin `UIViewController` öğesinden devraldığı sınıfı `EngagementViewController` olarak değiştirin. 

##<a id="monitor"></a>Uygulamayı gerçek zamanlı izlemeyle bağlama

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Anında iletme bildirimlerini ve uygulama içi mesajlaşmayı etkinleştirme

Mobile Engagement, kullanıcılarınız ile etkileşim kurmanızı ve onlara kampanyalar bağlamında anında iletme bildirimleri ve uygulama içi mesajlaşma aracılığıyla erişmenizi sağlar. Mobile Engagement portalında bu modüle REACH adı verilir.
Aşağıdaki bölümler bunları almak için uygulamanızı ayarlar.

### Uygulama Temsilcinizi değiştirme

1. **AppDelegate.cs**’yi açın ve şu deyimleri kullanarak aşağıdakileri ekleyin:

        using System; 

2. Şimdi, anında iletileri kaydetmek için şunun ardından, `FinishedLaunching` yöntemine aşağıdakileri ekleyin `EngagementAgent.init(...)`

        if (UIDevice.CurrentDevice.CheckSystemVersion(8,0))
        {
            var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                (UIUserNotificationType.Badge |
                    UIUserNotificationType.Sound |
                    UIUserNotificationType.Alert),
                null);
            UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications ();
        }
        else
        {
            UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (
                UIRemoteNotificationType.Badge |
                UIRemoteNotificationType.Sound |
                UIRemoteNotificationType.Alert);
        }

3. Son olarak - güncelleştirin veya aşağıdaki yöntemleri ekleyin:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, 
            Action<UIBackgroundFetchResult> completionHandler)
        {
            EngagementAgent.ApplicationDidReceiveRemoteNotification(userInfo, completionHandler);
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            // Register device token on Engagement
            EngagementAgent.RegisterDeviceToken(deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications(UIApplication application, NSError error)
        {
            Console.WriteLine("Failed to register for remote notifications: Error '{0}'", error);
        }

4. Çözümdeki **Info.plist** dosyanızda, **Paket Tanımlayıcısı**’nın Apple Geliştirici Merkezi’ndeki sağlama profilinizde sahip olduğunuz **Uygulama Kimliği** ile eşleştiğini doğrulayın. 

    ![][7]

5. Aynı **Info.plist** dosyasında, **Arka Plan Modlarını Etkinleştir** ve **Uzak Bildirimler** seçeneklerini etkinleştirdiğinizden emin olun. 

    ![][8]

6. Bu yayımlama profiliyle ilişkilendirdiğiniz cihazda uygulamayı çalıştırın. 

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[1]: ./media/mobile-engagement-xamarin-ios-get-started/new-solution.png
[2]: ./media/mobile-engagement-xamarin-ios-get-started/app-type.png
[3]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-name.png
[4]: ./media/mobile-engagement-xamarin-ios-get-started/configure-project-confirm.png
[5]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget.png
[6]: ./media/mobile-engagement-xamarin-ios-get-started/add-nuget-azme.png
[7]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-confirm-bundle.png
[8]: ./media/mobile-engagement-xamarin-ios-get-started/info-plist-configure-push.png



<!--HONumber=Sep16_HO3-->


