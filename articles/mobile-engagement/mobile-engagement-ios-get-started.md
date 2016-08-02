<properties
    pageTitle="Objective C’de iOS için Azure Mobile Engagement kullanmaya başlama"
    description="iOS uygulamaları için analizler ve anında iletme bildirimleri ile Azure Mobile Engagement kullanmayı öğrenin."
    services="mobile-engagement"
    documentationCenter="ios"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="05/03/2016"
    ms.author="piyushjo" />

# Objective C’de iOS uygulamaları için Azure Mobile Engagement kullanmaya başlama

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Bu konuda, uygulama kullanımınızı anlamak ve bir iOS uygulamasının segmentli kullanıcılarına anında iletme bildirimleri göndermek için nasıl Azure Mobile Engagement kullanılacağı gösterilmektedir.
Bu öğreticide, temel verileri toplayan ve Apple Anında İletme Bildirimi Sistemi’ni (APNS) kullanarak anında iletme bildirimleri alan boş bir iOS uygulaması oluşturursunuz.

Bu öğretici için aşağıdakiler gereklidir:

+ MAC App Store'dan yükleyebileceğiniz XCode 6 veya XCode 7
+ [Mobile Engagement iOS SDK]

Bu öğreticiyi tamamlamak iOS uygulamalarına ilişkin tüm Mobile Engagement öğreticileri için önkoşuldur.

> [AZURE.NOTE] Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).

##<a id="setup-azme"></a>iOS uygulamanız için Mobile Engagement kurma

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Uygulamanızı Mobile Engagement arka ucuna bağlama

Bu öğreticide, veri toplamak ve anında iletme bildirimi göndermek için gerekli en küçük grup olan bir "temel tümleştirme" gösterilmektedir. Tümleştirme belgelerinin tamamı [Mobile Engagement iOS SDK tümleştirmesi](mobile-engagement-ios-sdk-overview.md)’nde bulunabilir.

Tümleştirmeyi göstermek için XCode ile temel bir uygulama oluşturacağız.

###Yeni bir iOS projesi oluşturma

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###Uygulamanızı Mobile Engagement arka ucuna bağlama

1. [Mobile Engagement iOS SDK]’yı indirin.
2. .tar.gz dosyasını bilgisayarınızdaki bir klasöre ayıklayın.
3. Projeye sağ tıklayıp **Add files to** (Dosyaları şuraya ekle) seçeneğine tıklayın.

    ![][1]

4. SDK'yı ayıkladığınız klasöre gidin, `EngagementSDK` klasörünü seçip **Tamam**’a basın.

    ![][2]

5. **Build Phases** (Derleme Aşamaları) sekmesini açın, **Link Binary With Libraries** (İkiliyi Kitaplıklara Bağla) menüsünde çerçeveleri aşağıda gösterildiği gibi ekleyin:

    ![][3]

6. **XCode 7** için, `libxml2.dylib` yerine `libxml2.tbd` ekleyin.

7. Uygulamanızın**Bağlantı Bilgileri** sayfasında Azure Portalı’na geri gidin ve bağlantı dizesini kopyalayın.

    ![][4]

8. **AppDelegate.m** dosyanıza aşağıdaki kod satırını ekleyin.

        #import "EngagementAgent.h"

9. Şimdi, bağlantı dizesini `didFinishLaunchingWithOptions` temsilcisine yapıştırın.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]
            //[EngagementAgent setTestLogEnabled:YES];
   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

10. `setTestLogEnabled` sorunları belirleyebilmeniz için SDK günlüklerini etkinleştiren isteğe bağlı bir ifadedir. 

##<a id="monitor"></a>Gerçek zamanlı izlemeyi etkinleştirme

Veri göndermeye başlamak ve kullanıcıların etkin olduğundan emin olmak için, Mobile Engagement arka ucuna en az bir ekran (Etkinlik) göndermelisiniz.

1. **ViewController.h** dosyasını açıp **EngagementViewController.h** dosyasını içeri aktarın:

    `# import "EngagementViewController.h"`

2. **ViewController** arabiriminin süper sınıfını `EngagementViewController` ile değiştirin:

    `@interface ViewController : EngagementViewController`

##<a id="monitor"></a>Uygulamayı gerçek zamanlı izlemeyle bağlama

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Anında iletme bildirimlerini ve uygulama içi mesajlaşmayı etkinleştirme

Mobile Engagement, kullanıcılarınız ile etkileşim kurmanızı ve onlara kampanyalar bağlamında anında iletme bildirimleri ve uygulama içi mesajlaşma aracılığıyla erişmenizi sağlar. Mobile Engagement portalında bu modüle REACH adı verilir.
Aşağıdaki bölümler bunları almak için uygulamanızı ayarlar.

### Sessiz Anında İletme Bildirimlerini almak üzere uygulamanızı etkinleştirme

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### Reach kitaplığını projenize ekleyin.

1. Projenize sağ tıklayın.
2. **Add files to** (Dosyaları şuraya ekle) seçeneğine tıklayın.
3. SDK’yı ayıkladığınız klasöre gidin.
4. `EngagementReach` klasörünü seçin.
5. **Ekle**'ye tıklayın.

### Uygulama Temsilcinizi değiştirme

1. **AppDeletegate.m** dosyana geri dönüp Engagement Reach modülünü içeri aktarın.

        #import "AEReachModule.h"

2. `application:didFinishLaunchingWithOptions` yöntemi içerisinde bir Reach modülü oluşturun ve bu modülü mevcut Engagement başlatma satırınıza geçirin:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###APNS Anında İletme Bildirimlerini almak üzere uygulamanızı etkinleştirme

1. `application:didFinishLaunchingWithOptions` yöntemine aşağıdaki satırı ekleyin:

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

2. `application:didRegisterForRemoteNotificationsWithDeviceToken` yöntemini aşağıdaki şekilde ekleyin:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. `didFailToRegisterForRemoteNotificationsWithError` yöntemini aşağıdaki şekilde ekleyin:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. `didReceiveRemoteNotification:fetchCompletionHandler` yöntemini aşağıdaki şekilde ekleyin:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png




<!---HONumber=Jun16_HO2-->


