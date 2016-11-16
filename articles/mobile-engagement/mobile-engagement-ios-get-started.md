---
title: "Objective C&quot;de iOS için Azure Mobile Engagement ile Çalışmaya Başlama | Microsoft Belgeleri"
description: "iOS uygulamaları için analizler ve anında iletme bildirimleri ile Azure Mobile Engagement kullanmayı öğrenin."
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 117b5742-522b-41de-98c5-f432da2d98f8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/05/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1dc9885e4cdbad1153ac476e3f0c0068ec391374


---
# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Objective C’de iOS uygulamaları için Azure Mobile Engagement kullanmaya başlama
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Bu konuda, uygulama kullanımınızı anlamak ve bir iOS uygulamasının segmentli kullanıcılarına anında iletme bildirimleri göndermek için nasıl Azure Mobile Engagement kullanılacağı gösterilmektedir.
Bu öğreticide, temel verileri toplayan ve Apple Anında İletme Bildirimi Sistemi’ni (APNS) kullanarak anında iletme bildirimleri alan boş bir iOS uygulaması oluşturursunuz.

Bu öğretici için aşağıdakiler gereklidir:

* MAC App Store'dan yükleyebileceğiniz XCode 8
* [Mobile Engagement iOS SDK]

Bu öğreticiyi tamamlamak iOS uygulamalarına ilişkin tüm Mobile Engagement öğreticileri için önkoşuldur.

> [!NOTE]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).
> 
> 

## <a name="a-idsetupazmeasetup-mobile-engagement-for-your-ios-app"></a><a id="setup-azme"></a>iOS uygulamanız için Mobile Engagement kurma
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="a-idconnectingappaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Uygulamanızı Mobile Engagement arka ucuna bağlama
Bu öğreticide, veri toplamak ve anında iletme bildirimi göndermek için gerekli en küçük grup olan bir "temel tümleştirme" gösterilmektedir. Tümleştirme belgelerinin tamamı [Mobile Engagement iOS SDK tümleştirmesi](mobile-engagement-ios-sdk-overview.md)’nde bulunabilir.

Tümleştirmeyi göstermek için XCode ile temel bir uygulama oluşturacağız.

### <a name="create-a-new-ios-project"></a>Yeni bir iOS projesi oluşturma
[!INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

### <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Uygulamanızı Mobile Engagement arka ucuna bağlama
1. [Mobile Engagement iOS SDK]’yı indirin.
2. .tar.gz dosyasını bilgisayarınızdaki bir klasöre ayıklayın.
3. Projeye sağ tıklayıp **Add files to** (Dosyaları şuraya ekle) seçeneğine tıklayın.
   
    ![][1]
4. SDK'yı ayıkladığınız klasöre gidin, `EngagementSDK` klasörünü seçip **Tamam**’a basın.
   
    ![][2]
5. **Build Phases** (Derleme Aşamaları) sekmesini açın, **Link Binary With Libraries** (İkiliyi Kitaplıklara Bağla) menüsünde çerçeveleri aşağıda gösterildiği gibi ekleyin:
   
    ![][3]
6. Uygulamanızın**Bağlantı Bilgileri** sayfasında Azure Portalı’na geri gidin ve bağlantı dizesini kopyalayın.
   
    ![][4]
7. **AppDelegate.m** dosyanıza aşağıdaki kod satırını ekleyin.
   
        #import "EngagementAgent.h"
8. Şimdi, bağlantı dizesini `didFinishLaunchingWithOptions` temsilcisine yapıştırın.
   
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
              [...]   
              [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
              [...]
        }
9. `setTestLogEnabled`, sorunları belirleyebilmeniz için SDK günlüklerini etkinleştiren isteğe bağlı bir ifadedir. 

## <a name="a-idmonitoraenable-realtime-monitoring"></a><a id="monitor"></a>Gerçek zamanlı izlemeyi etkinleştirme
Veri göndermeye başlamak ve kullanıcıların etkin olduğundan emin olmak için, Mobile Engagement arka ucuna en az bir ekran (Etkinlik) göndermelisiniz.

1. **ViewController.h** dosyasını açıp **EngagementViewController.h** dosyasını içeri aktarın:
   
    `# import "EngagementViewController.h"`
2. **ViewController** arabiriminin süper sınıfını `EngagementViewController` ile değiştirin:
   
    `@interface ViewController : EngagementViewController`

## <a name="a-idmonitoraconnect-app-with-realtime-monitoring"></a><a id="monitor"></a>Uygulamayı gerçek zamanlı izlemeyle bağlama
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="a-idintegratepushaenable-push-notifications-and-inapp-messaging"></a><a id="integrate-push"></a>Anında iletme bildirimlerini ve uygulama içi mesajlaşmayı etkinleştirme
Mobile Engagement, kullanıcılarınız ile etkileşim kurmanızı ve onlara kampanyalar bağlamında anında iletme bildirimleri ve uygulama içi mesajlaşma aracılığıyla erişmenizi sağlar. Mobile Engagement portalında bu modüle REACH adı verilir.
Aşağıdaki bölümler bunları almak için uygulamanızı ayarlar.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Sessiz Anında İletme Bildirimlerini almak üzere uygulamanızı etkinleştirme
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>Reach kitaplığını projenize ekleyin.
1. Projenize sağ tıklayın.
2. **Add files to** (Dosyaları şuraya ekle) seçeneğine tıklayın.
3. SDK’yı ayıkladığınız klasöre gidin.
4. `EngagementReach` klasörünü seçin.
5. **Ekle**'ye tıklayın.

### <a name="modify-your-application-delegate"></a>Uygulama Temsilcinizi değiştirme
1. **AppDeletegate.m** dosyana geri dönüp Engagement Reach modülünü içeri aktarın.
   
        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>
2. `application:didFinishLaunchingWithOptions` yöntemi içerisinde bir Reach modülü oluşturun ve bu modülü mevcut Engagement başlatma satırınıza geçirin:
   
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

### <a name="enable-your-app-to-receive-apns-push-notifications"></a>APNS Anında İletme Bildirimlerini almak üzere uygulamanızı etkinleştirme
1. `application:didFinishLaunchingWithOptions` yöntemine aşağıdaki satırı ekleyin:
   
        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
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

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png




<!--HONumber=Nov16_HO2-->


