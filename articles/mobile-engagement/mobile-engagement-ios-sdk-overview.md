---
title: "Azure Mobile Engagement iOS SDK genel bakış | Microsoft Docs"
description: "En son güncelleştirmeler ve iOS için Azure Mobile Engagement SDK'sı için yordamlar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3a03bbd6-bcf8-436c-9775-5a8188629252
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 6acd343782a3ee07750e27ec3022ff81cedfadee
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="ios-sdk-for-azure-mobile-engagement"></a>Azure Mobile Engagement için iOS SDK’sı
Azure Mobile Engagement bir iOS uygulaması tümleştirme hakkında tüm ayrıntıları almak buradan başlayın. Try ilk vermek istiyorsanız, bunu emin olun bizim [15 dakika Öğreticisi](mobile-engagement-ios-get-started.md).

Görmek için tıklatın [SDK içerik](mobile-engagement-ios-sdk-content.md)

## <a name="integration-procedures"></a>Tümleştirme yordamları
1. Buradan başlayın: [iOS uygulamanızı Mobile Engagement tümleştirme](mobile-engagement-ios-integrate-engagement.md)
2. Bildirimlerinin: [ulaşma (bildirimleri), iOS uygulamanızın tümleştirme](mobile-engagement-ios-integrate-engagement-reach.md)
3. Etiket planı uygulama: [iOS uygulamanızı API etiketleme Gelişmiş Mobile Engagement kullanma](mobile-engagement-ios-use-engagement-api.md)

## <a name="release-notes"></a>Sürüm notları
### <a name="410-07172017"></a>4.1.0 (07/17/2017)
* Sabit rozetleri arka planda temizlendi.
* Sabit uyarılar ana sırada adlı değil API'leri hakkında XCode 9.
* Bellek sızıntısı ulaşma yoklamalarda sabit.
* İOS için destek bırakılan 6.X. Uygulamanızın dağıtım hedef bu sürümünden başlayarak olmalıdır en az iOS 7.

Önceki sürümü için lütfen bkz. [tamamlamak sürüm notları](mobile-engagement-ios-release-notes.md)

## <a name="upgrade-procedures"></a>Yükseltme yordamları
Uygulamanıza katılım daha eski bir sürümü zaten bütünleştirdiyseniz, SDK'yı yükseltirken aşağıdaki noktaları dikkate almanız gerekir.

SDK bkz: tam birçok sürümü eksik birkaç yordamları izleyin gerekebilir [yükseltme yordamları](mobile-engagement-ios-upgrade-procedure.md).

SDK'sı için her yeni sürümü, ilk değiştirmeniz gerekir (kaldırın ve yeniden xcode'da içeri aktarın) EngagementSDK ve EngagementReach klasörler.

### <a name="from-300-to-400"></a>3.0.0 4.0.0 için
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

#### <a name="usernotifications-framework"></a>UserNotifications framework
Eklemenize gerek `UserNotifications` derleme aşamaları framework.

Proje Gezgini'nde, proje bölmesini açın ve doğru hedef seçin. Ardından, açın **"Derleme aşamaları"** sekmesi ve **"Bağlantı ikiliyi kitaplıklara"** menüsünde framework eklemek `UserNotifications.framework` -bağlantı olarak ayarlayın`Optional`

#### <a name="application-push-capability"></a>Uygulamayı anında iletme yeteneği
XCode 8 uygulamanızı sıfırlama anında iletme yeteneği, lütfen tekrar gözden geçirin `capability` seçilen hedef sekmesi.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>Yeni iOS 10 bildirim kayıt kodu ekleyin
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

#### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a>UNUserNotificationCenter temsilci çakışmalarını çözme

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
