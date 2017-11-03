---
title: "Azure Mobile Engagement iOS SDK Yükseltme yordamı | Microsoft Docs"
description: "En son güncelleştirmeler ve iOS için Azure Mobile Engagement SDK'sı için yordamlar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 72a9e493-3f14-4e52-b6e2-0490fd04b184
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 12/13/2016
ms.author: piyushjo
ms.openlocfilehash: 37c7f133d079186f828d58cabce0d2a259efd085
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="upgrade-procedures"></a>Yükseltme yordamları
Uygulamanıza katılım daha eski bir sürümü zaten bütünleştirdiyseniz, SDK'yı yükseltirken aşağıdaki noktaları dikkate almanız gerekir.

SDK'sı için her yeni sürümü, ilk değiştirmeniz gerekir (kaldırın ve yeniden xcode'da içeri aktarın) EngagementSDK ve EngagementReach klasörler.

## <a name="from-300-to-400"></a>3.0.0 4.0.0 için
### <a name="xcode-8"></a>XCode 8
XCode 8 SDK 4.0.0 sürümünden başlayarak zorunludur.

> [!NOTE]
> XCode 7'de gerçekten bağımlı sonra kullanabilirsiniz [iOS Engagement SDK'sı v3.2.4](https://aka.ms/r6oouh). Bir bilinen hata varsa bu önceki sürüm reach modülünü 10 ios'de çalıştırılırken: Sistem bildirimleri işleme alınan değildir. Bu kullanım dışı API uygulamak için olacaktır sorunu gidermek için `application:didReceiveRemoteNotification:` uygulamanızda temsilci gibi:
> 
> 

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> **Bu geçici çözüm önermiyoruz** gibi bu iOS API kullanım dışı olduğundan tüm yaklaşan (hatta küçük) iOS sürüm yükseltme Bu davranışı değiştirebilirsiniz. Mümkün olan en kısa sürede XCode 8'e geçer.
> 
> 

### <a name="usernotifications-framework"></a>UserNotifications framework
Eklemenize gerek `UserNotifications` derleme aşamaları framework.

Proje Gezgini'nde, proje bölmesini açın ve doğru hedef seçin. Ardından, açın **"Derleme aşamaları"** sekmesi ve **"Bağlantı ikiliyi kitaplıklara"** menüsünde framework eklemek `UserNotifications.framework` -bağlantı olarak ayarlayın`Optional`

### <a name="application-push-capability"></a>Uygulamayı anında iletme yeteneği
XCode 8 uygulamanızı sıfırlama anında iletme yeteneği, lütfen tekrar gözden geçirin `capability` seçilen hedef sekmesi.

### <a name="add-the-new-ios-10-notification-registration-code"></a>Yeni iOS 10 bildirim kayıt kodu ekleyin
Uygulama bildirimleri kaydetmek için eski kod parçacığını hala çalışır ancak iOS 10 çalıştırılırken kullanım dışı API'lerini kullanarak.

İçeri aktarma `User Notification` framework:

        #import <UserNotifications/UserNotifications.h> 

Uygulama temsilcinizi içinde `application:didFinishLaunchingWithOptions` yöntemi Değiştir:

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

tarafından:

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

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a>UNUserNotificationCenter temsilci çakışmalarını çözme

*Uygulamanızı ya da üçüncü taraf Kitaplıklarınızı biri uygulayan bir `UNUserNotificationCenterDelegate` sonra da bu bölümü atlayabilirsiniz.*

A `UNUserNotificationCenter` temsilci SDK tarafından katılım bildirimleri 10 veya daha büyük iOS çalıştıran cihazlarda yaşam döngüsünü izlemek için kullanılır. SDK, kendi uygulamanızda `UNUserNotificationCenterDelegate` protokol ancak yalnızca bir olabilir `UNUserNotificationCenter` temsilci uygulama başına. Eklenen herhangi bir temsilci `UNUserNotificationCenter` nesne katılım bir çakışma. SDK veya herhangi diğer üçüncü bir tarafın temsilci algılarsa sonra kendi uygulama çakışmaları olanağı vermek için kullanmaz. Çakışmaları çözümlemek amacıyla kendi temsilciye katılım mantığı eklemeniz gerekir.

Bunu başarmak için iki yolu vardır.

SDK çağrıları temsilciniz iletme tarafından yalnızca 1, Teklif:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Veya içinden devralma tarafından 2, teklif `AEUserNotificationHandler` sınıfı

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [!NOTE]
> Bir bildirim geçirerek değil veya katılım üzerinden gelen olup olmadığını belirlemek, `userInfo` aracı sözlüğe `isEngagementPushPayload:` sınıf yöntemi.

Olduğundan emin olun `UNUserNotificationCenter` nesnenin temsilci temsilciniz ya da içinde ayarlanmış `application:willFinishLaunchingWithOptions:` veya `application:didFinishLaunchingWithOptions:` uygulama temsilcinizi yöntemi.
Örneğin, yukarıdaki teklifi 1 uygulanırsa:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code
  
        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="from-200-to-300"></a>2.0.0 3.0.0 için
İOS için destek bırakılan 4.X. Uygulamanızın dağıtım hedef bu sürümünden başlayarak olmalıdır en az iOS 6.

Reach uygulamanızda kullanıyorsanız, eklemelisiniz `remote-notification` değeri `UIBackgroundModes` dizi Info.plist dosyanızdaki uzak bildirimleri almak için.

Yöntem `application:didReceiveRemoteNotification:` tarafından değiştirilmesi gereken `application:didReceiveRemoteNotification:fetchCompletionHandler:` uygulama temsilcinizi içinde.

"AEPushDelegate.h" kullanım dışı arabirimi ve tüm başvurularını kaldırmanız gerekir. Bu kaldırma içerir `[[EngagementAgent shared] setPushDelegate:self]` ve uygulama temsilcinizi temsilci yöntemleri:

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

## <a name="from-1160-to-200"></a>1.16.0 2.0.0 için
Aşağıdaki nasıl Azure Mobile Engagement tarafından desteklenen bir uygulamaya Capptain SAS tarafından sunulan Capptain hizmetinden bir SDK tümleştirmesi geçirileceğini açıklar.
Önceki bir sürümünden geçiş yapıyorsanız, Lütfen 1.16 için önce geçirmenizi sonra aşağıdaki yordamı uygulamak için Capptain web sitesine bakın.

> [!IMPORTANT]
> Capptain ve Mobile Engagement aynı Hizmetleri değildir ve aşağıda verilen yordamı yalnızca istemci uygulaması geçirmek nasıl vurgular. Uygulama SDK'yı geçirme verilerinizi Capptain sunucularından Mobile Engagement sunucuya geçişi YAPILMAZ
> 
> 

### <a name="agent"></a>Aracı
Yöntem `registerApp:` yeni yöntemi tarafından değiştirildiğini `init:`. Uygulama temsilcinizi uygun şekilde güncelleştirilmesi gerekir ve bağlantı dizesi kullanın:

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

SmartAd izleme yalnızca sahip tüm örneklerini kaldırmak için SDK'dan kaldırıldı `AETrackModule` sınıfı

### <a name="class-name-changes"></a>Sınıf adı değişikliği
Rebranding bir parçası olarak değiştirilmesi gereken sınıf/dosya adları birkaç vardır.

Tüm sınıflar "CP" öneki "AE" önekiyle adlandırılır.

Örnek:

* `CPModule.h`yeniden adlandırılır `AEModule.h`.

Tüm sınıflar "İle Capptain" öneki "Katılım" önekiyle adlandırılır.

Örnekler:

* Sınıf `CapptainAgent` adlandırılır `EngagementAgent`.
* Sınıf `CapptainTableViewController` adlandırılır `EngagementTableViewController`.
* Sınıf `CapptainUtils` adlandırılır `EngagementUtils`.
* Sınıf `CapptainViewController` adlandırılır `EngagementViewController`.

