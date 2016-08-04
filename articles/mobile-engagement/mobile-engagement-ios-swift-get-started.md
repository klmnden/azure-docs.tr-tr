<properties
    pageTitle="Swift’te iOS için Azure Mobile Engagement Kullanmaya Başlama"
    description="iOS Uygulamaları için Analizler ve Anında İletme Bildirimleri ile Azure Mobile Engagement kullanmayı öğrenin."
    services="mobile-engagement"
    documentationCenter="ios"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="05/03/2016"
    ms.author="piyushjo" />

# Swift’te iOS Uygulamaları için Azure Mobile Engagement Kullanmaya Başlama

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Bu konuda, uygulama kullanımınızı anlamak ve bir iOS uygulamasının segmentli kullanıcılarına anında iletme bildirimleri göndermek için nasıl Azure Mobile Engagement kullanılacağı gösterilmektedir.
Bu öğreticide, temel verileri toplayan ve Apple Anında İletme Bildirimi Sistemi’ni (APNS) kullanarak anında iletme bildirimleri alan boş bir iOS uygulaması oluşturursunuz.

Bu öğretici için aşağıdakiler gereklidir:

+ MAC App Store'dan yükleyebileceğiniz XCode 6 veya XCode 7
+ [Mobile Engagement iOS SDK]
+ Apple Dev Center'ınızda edinebileceğiniz anında iletme bildirimi sertifikası (.p12)

> [AZURE.NOTE] Bu öğreticide, Swift sürüm 2.0 kullanılmaktadır. 

Bu öğreticiyi tamamlamak iOS uygulamalarına ilişkin tüm Mobile Engagement öğreticileri için önkoşuldur.

> [AZURE.NOTE] Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).

##<a id="setup-azme"></a>iOS uygulamanız için Mobile Engagement kurma

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Uygulamanızı Mobile Engagement arka ucuna bağlama

Bu öğreticide, veri toplamak ve anında iletme bildirimi göndermek için gerekli en küçük grup olan bir "temel tümleştirme" gösterilmektedir. Tümleştirme belgelerinin tamamı [Mobile Engagement iOS SDK tümleştirmesi](mobile-engagement-ios-sdk-overview.md)’nde bulunabilir.

Tümleştirmeyi göstermek için XCode ile temel bir uygulama oluşturacağız:

###Yeni bir iOS projesi oluşturma

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###Uygulamanızı Mobile Engagement arka ucuna bağlama

1. [Mobile Engagement iOS SDK]’yı indirin
2. .tar.gz dosyasını bilgisayarınızdaki bir klasöre ayıklayın
3. Projeye sağ tıklayıp "Add files to ..." (Dosyaları şuraya ekle) seçeneğine tıklayın.

    ![][1]

4. SDK'yı ayıkladığınız klasöre gidin, `EngagementSDK` klasörünü seçip Tamam’a basın.

    ![][2]

5. `Build Phases` sekmesini açıp `Link Binary With Libraries` menüsünde aşağıda gösterildiği gibi çerçeveleri ekleyin. **NOT** `CoreLocation, CFNetwork, CoreTelephony, and SystemConfiguration` dahil edilmelidir:

    ![][3]

6. **XCode 7** için, `libxml2.dylib` yerine `libxml2.tbd` ekleyin.

7. SDK'nın Objective C API'lerini kullanabilmek için Dosya > Yeni > Dosya > iOS > Kaynak > Üst Bilgi Dosyası’nı seçerek bir Köprü Oluşturma üst bilgisi oluşturun.

    ![][4]

8. Mobile Engagement Objective-C kodunu Swift kodunuzun kullanımına sunmak için köprü oluşturma üst bilgi dosyasını düzenleyin, aşağıdaki içeri aktarmaları ekleyin:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEIdfaProvider.h"

9. Derleme Ayarları’nda Swift Derleyicisi - Kod Oluşturma altındaki Objective-C Köprü Oluşturma Üst Bilgisi derleme ayarının bu üst bilgiye bir yolu bulunduğundan emin olun. Yol için bir örnek şudur: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (yola bağlı olarak)**

    ![][6]

10. Uygulamanızın*Bağlantı Bilgileri* sayfasında Azure Portalı’na geri gidin ve Bağlantı Dizesini kopyalayın.

    ![][5]

11. Şimdi, bağlantı dizesini `didFinishLaunchingWithOptions` temsilcisine yapıştırın

        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a id="monitor"></a>Gerçek zamanlı izlemeyi etkinleştirme

Veri göndermeye başlamak ve kullanıcıların etkin olduğundan emin olmak için, Mobile Engagement arka ucuna en az bir ekran (Etkinlik) göndermelisiniz.

1. **ViewController.swift** dosyasını açıp **ViewController** temel sınıfını **EngagementViewController** olarak değiştirin:

    `class ViewController : EngagementViewController {`

##<a id="monitor"></a>Uygulamayı gerçek zamanlı izlemeyle bağlama

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Anında İletme Bildirimlerini ve uygulama içi mesajlaşmayı etkinleştirme

Mobile Engagement, kampanyalar bağlamında anında iletme bildirimleri ve uygulama içi mesajlaşma aracılığıyla kullanıcılarınız ile etkileşim kurmanızı ve onlara erişmenizi sağlar. Mobile Engagement portalında bu modüle REACH adı verilir.
Aşağıdaki bölümler, uygulamanızı bu bildirim ve mesajları alacak şekilde ayarlar.

### Sessiz Anında İletme Bildirimlerini almak üzere uygulamanızı etkinleştirme

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### Reach kitaplığını projenize ekleyin.

1. Projenize sağ tıklayın
2. Şunu seçin: `Add file to ...`
3. SDK’yı ayıkladığınız klasöre gidin
4. `EngagementReach` klasörünü seçin
5. Ekle'ye tıklayın.
6. Mobile Engagement Objective-C Reach üst bilgilerini kullanıma sunmak için köprü oluşturma üst bilgi dosyasını düzenleyin ve aşağıdaki içeri aktarmaları ekleyin:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### Uygulama Temsilcinizi değiştirme

1. `didFinishLaunchingWithOptions` içerisinde bir erişim modülü oluşturun ve bu modülü mevcut Engagement başlatma satırınıza geçirin:

        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
            let reach = AEReachModule.moduleWithNotificationIcon(UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###APNS Anında İletme Bildirimlerini almak üzere uygulamanızı etkinleştirme
1. `didFinishLaunchingWithOptions` yöntemine aşağıdaki satırı ekleyin:

        /* Ask user to receive push notifications */
        if #available(iOS 8.0, *)
        {
           let settings = UIUserNotificationSettings(forTypes: [UIUserNotificationType.Alert, UIUserNotificationType.Badge, UIUserNotificationType.Sound], categories: nil)
           application.registerUserNotificationSettings(settings)
           application.registerForRemoteNotifications()
        }
        else
        {
           application.registerForRemoteNotificationTypes([UIRemoteNotificationType.Alert, UIRemoteNotificationType.Badge, UIRemoteNotificationType.Sound])
        }

2. `didRegisterForRemoteNotificationsWithDeviceToken` yöntemini aşağıdaki şekilde ekleyin:

        func application(application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: NSData)
        {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. `didReceiveRemoteNotification:fetchCompletionHandler:` yöntemini aşağıdaki şekilde ekleyin:

        func application(application: UIApplication, didReceiveRemoteNotification userInfo: [NSObject : AnyObject], fetchCompletionHandler completionHandler: (UIBackgroundFetchResult) -> Void)
        {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png



<!----HONumber=Jun16_HO2-->


