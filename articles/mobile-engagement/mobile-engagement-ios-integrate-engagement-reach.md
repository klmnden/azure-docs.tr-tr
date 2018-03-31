---
title: Azure Mobile Engagement iOS SDK tümleştirmesi ulaşmak | Microsoft Docs
description: En son güncelleştirmeler ve iOS için Azure Mobile Engagement SDK'sı için yordamlar
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 1f5f5857-867c-40c5-9d76-675a343a0296
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 12/13/2016
ms.author: piyushjo
ms.openlocfilehash: 8d531f5850e8f7f352774f5894285402bd4cc53e
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-integrate-engagement-reach-on-ios"></a>Engagement Reach tümleştirmek için iOS hakkında
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

Açıklanan tümleştirme yordamı izlemelisiniz [tümleştirmek katılım iOS belgesinde nasıl](mobile-engagement-ios-integrate-engagement.md) bu kılavuz aşağıdaki önce.

Bu belge, XCode 8 gerektirir. XCode 7'de gerçekten bağımlı sonra kullanabilirsiniz [iOS Engagement SDK'sı v3.2.4](https://aka.ms/r6oouh). Bir bilinen hata varsa bu önceki sürüm 10 ios'de çalıştırılırken: Sistem bildirimleri işleme alınan değildir. Bu kullanım dışı API uygulamak için olacaktır sorunu gidermek için `application:didReceiveRemoteNotification:` uygulamanızda temsilci gibi:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> **Bu geçici çözüm önermiyoruz** gibi bu iOS API kullanım dışı olduğundan tüm yaklaşan (hatta küçük) iOS sürüm yükseltme Bu davranışı değiştirebilirsiniz. Mümkün olan en kısa sürede XCode 8'e geçer.
>
>

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Sessiz Anında İletme Bildirimlerini almak üzere uygulamanızı etkinleştirme
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

## <a name="integration-steps"></a>Tümleştirme adımları
### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a>Engagement Reach SDK'sı iOS projenize ekleme
* Reach SDK'sını Xcode projenize ekleyin. Xcode'da, Git **proje \> projesine Ekle** ve `EngagementReach` klasör.

### <a name="modify-your-application-delegate"></a>Uygulama Temsilcinizi değiştirme
* Uygulama dosyanızın en üstünde Engagement Reach modülünü içeri aktarın:

      [...]
      #import "AEReachModule.h"
* Yöntemi içinde `applicationDidFinishLaunching:` veya `application:didFinishLaunchingWithOptions:`, bir reach modülü oluşturun ve mevcut Engagement başlatma satırınıza geçirin:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
        [...]

        return YES;
      }
* Değiştirme **'icon.png'** dize bildirim simgesi olarak istediğiniz görüntü adı ile.
* Seçeneğini kullanmak istiyorsanız, *güncelleştirme rozet değeri* Reach kampanyaları veya yerel gönderim kullanmak isteyip istemediğinizi \<SaaS/Reach API'sini/Kampanya biçimi/yerel anında iletme\> kampanyalar, rozet yönetmek Reach modülünün izin gerekir (bunu otomatik olarak uygulama rozet temizleyin ve ayrıca uygulama Başlarken veya foregrounded her zaman Engagement tarafından depolanan değer Sıfırla) simgesi kendisi. Bu, Reach modülü başlatma sonra aşağıdaki satırı ekleyerek yapılır:

      [reach setAutoBadgeEnabled:YES];
* Reach veri gönderimi işlemek istiyorsanız, uygun uygulama temsilcinizi izin vermelisiniz `AEReachDataPushDelegate` protokolü. Reach modülü başlatma sonra aşağıdaki satırı ekleyin:

      [reach setDataPushDelegate:self];
* Yöntemleri uygulayabilirsiniz sonra `onDataPushStringReceived:` ve `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` uygulama temsilcinizi içinde:

      -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
      {
         NSLog(@"String data push message with category <%@> received: %@", category, body);
         return YES;
      }

      -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
      {
         NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
         // Do something useful with decodedBody like updating an image view
         return YES;
      }

### <a name="category"></a>Kategori
Kategori parametresi bir veri gönderme kampanya oluşturduğunuzda isteğe bağlıdır ve filtre veri iter sağlar. Farklı tür itmek istiyorsanız kullanışlıdır, `Base64` veri ve ayrıştırmadan önce kendi türünü tanımlamak istiyorsanız.

**Uygulamanızı şimdi almak ve reach içeriğini görüntülemek hazır!**

## <a name="how-to-receive-announcements-and-polls-at-any-time"></a>Duyuruları ve yoklamaları herhangi bir zamanda alma
Engagement Reach bildirimleri son kullanıcılarınız için herhangi bir zamanda Apple anında iletilen bildirim servisi kullanarak gönderebilirsiniz.

Bu işlevselliği etkinleştirmek için Apple anında iletme bildirimleri için uygulamanızı hazırlamak ve uygulama temsilcinizi değiştirme gerekir.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Uygulamanızın Apple anında iletme bildirimleri için hazırlama
Lütfen kılavuzunu izleyin: [uygulamanız Apple anında iletme bildirimleri için hazırlama](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-the-necessary-client-code"></a>Gerekli istemci kodu ekleyin
*Bu noktada, uygulamanızın kayıtlı bir Apple anında iletme sertifika katılım ön ucunda olması gerekir.*

Henüz yapmadıysanız anında iletme bildirimlerini almak üzere uygulamanızı kaydetmeniz gerekir.

* İçeri aktarma `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>
* Uygulamanız başladığında aşağıdaki satırı ekleyin (genellikle, `application:didFinishLaunchingWithOptions:`):

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

Ardından, cihaz belirteci Apple sunucuları tarafından döndürülen engagement sağlamanız gerekir. Bu yöntemin adlı yapılır `application:didRegisterForRemoteNotificationsWithDeviceToken:` uygulama temsilcinizi içinde:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Son olarak, uygulamanızın uzak bir bildirim aldığında Engagement SDK'sı hakkında bilgilendirmek sahip. Bunu yapmak için yöntemi çağırın `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` uygulama temsilcinizi içinde:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [!IMPORTANT]
> Varsayılan olarak, Engagement Reach completionHandler denetler. El ile yanıt vermek istiyorsanız `handler` kodunuzda engellemek, nil geçirmek `handler` bağımsız değişkeni ve denetim tamamlama bloğu kendiniz. Bkz: `UIBackgroundFetchResult` olası değerler listesi türü.
>
>

### <a name="full-example"></a>Tam örnek
Tümleştirme tam bir örneği burada verilmiştir:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
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

## <a name="how-to-customize-campaigns"></a>Kampanyalar özelleştirme
### <a name="notifications"></a>Bildirimler
Bildirimler iki tür vardır: Sistem ve uygulama içi bildirimler.

Sistem bildirimleri iOS tarafından işlenir ve özelleştirilemez.

Uygulama bildirimleri geçerli uygulama penceresi dinamik olarak eklenen bir görünümün yapılır. Bu bildirim bir katmana çağrılır. Bunlar gerekli olmadığı için uygulamanızda herhangi bir görünüm değiştirmenizi bildirim yer paylaşımları hızlı tümleştirme için mükemmeldir.

#### <a name="layout"></a>Düzen
Uygulama içi bildirimler görünümünü değiştirmek için yalnızca dosyasını değiştirebilirsiniz `AENotificationView.xib` gereksinimlerinize, varolan subviews türünü ve etiket değerleri tutmak sürece.

Varsayılan olarak, uygulama içi bildirimler ekranın alt kısmında sunulmuştur. Ekranın en üstünde görüntülenecek tercih ederseniz, sağlanan düzenleme `AENotificationView.xib` değiştirip `AutoSizing` kendi değerlendirmesi üstünde tutulması için ana görünümün özelliği.

#### <a name="categories"></a>Kategoriler
Sağlanan Düzen değiştirdiğinizde, tüm bildirimlerinizi görünümünü değiştirin. Kategoriler, bildirimler için çeşitli hedeflenen görünüyor (büyük olasılıkla davranışları) tanımlamanıza olanak sağlar. Bir kategori Reach kampanya oluşturduğunuzda belirtilebilir. Bu belgenin sonraki bölümlerinde açıklanan kategoriler de duyuruları ve yoklamaları, özelleştirmenize olanak tanır olduğunu aklınızda bulundurun.

Bildirimlerinizi için bir kategori işleyici kaydetmek için reach modülünün başlatıldıktan sonra bir çağrı eklemeniz gerekir.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier` protokol uyan bir nesne örneği olmalıdır `AENotifier`.

Kendi kendinize Protokolü yöntemleri uygulayabilirsiniz veya varolan bir sınıfı yeniden uygulamalı seçtiğiniz `AEDefaultNotifier` işlerin çoğunu zaten gerçekleştirir.

Örneğin, belirli bir kategorideki uyarı görünümünü yeniden tanımlamak istiyorsanız, bu örnek izleyebilirsiniz:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Bu basit örnek kategorisinin varsayalım adında bir dosyanız varsa `MyNotificationView.xib` ana uygulama paket içindeki. Yöntem karşılık gelen bulamıyor değilse `.xib`, bildirim görüntülenmeyecek ve katılım konsolunda bir ileti çıktı.

Sağlanan nib dosyası aşağıdaki kuralları dikkate:

* Yalnızca bir görünüm içermesi gerekir.
* Sağlanan nib dosyası içindeki olanlarla aynı türlerinin subviews adlı `AENotificationView.xib`
* Subviews anahtarlarla sağlanan içinde aynı etiketleri olması gerektiğini adlı nib dosyası `AENotificationView.xib`

> [!TIP]
> Yalnızca adlı sağlanan nib dosya kopyalama `AENotificationView.xib`ve buradan çalışmaya başlayın. Ancak dikkatli olun, görünüm bu nib dosyası içinde sınıfla ilişkilendirilen `AENotificationView`. Bu sınıf yöntemi yeniden tanımlandı `layoutSubViews` taşımak ve kendi subviews bağlamı göre yeniden boyutlandırmak için. Değiştirmek istediğiniz bir `UIView` ya da size özel görünüm sınıfı.
>
>

Daha derin özelleştirme bildirimlerinizi gerekiyorsa (örneğin istiyorsanız doğrudan kodunuzdan görünümünüzü yüklemek için), sağlanan kaynak kodu ve sınıf belgeleri bakmak için önerilir `Protocol ReferencesDefaultNotifier` ve `AENotifier`.

Birden çok kategori için aynı bildirim kullanabileceğinizi unutmayın.

De yeniden tanımlandı varsayılan bildirim şuna benzer:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Bildirim işleme
Varsayılan Kategori kullanırken, bazı yaşam döngüsü yöntemleri üzerinde denir `AEReachContent` nesne istatistikleri rapor ve kampanya durumunu güncelleştirin:

* Uygulamada, bildirim görüntülendiğinde `displayNotification` yöntemi (hangi istatistikleri raporları) çağrılır tarafından `AEReachModule` varsa `handleNotification:` döndürür `YES`.
* Bildirim kapatıldığında, `exitNotification` yöntemi çağrıldığında, istatistik bildirilir ve sonraki Kampanyalar şimdi işlenebilir.
* Bildirim tıkladıysanız `actionNotification` olan çağrılır, istatistik bildirilir ve ilişkili eylem gerçekleştirilir.

Uygulamanıza `AENotifier` varsayılan davranışı, atlar başınıza bu yaşam döngüsü yöntemlerini çağırmaya sahip. Aşağıdaki örneklerde varsayılan davranış olduğu atlanır bazı durumlarda gösterilmektedir:

* Genişletme yok `AEDefaultNotifier`, sıfırdan kategori işleme uygulanan örn.
* Geçersiz kılınmış `prepareNotificationView:forContent:`, en az eşlemek mutlaka `onNotificationActioned` veya `onNotificationExited` , U.I birine denetler.

> [!WARNING]
> Varsa `handleNotification:` bir özel durum, içeriği silinir atar ve `drop` olduğu çağrılır, bu istatistiklerine bildirilir ve sonraki Kampanyalar şimdi işlenebilir.
>
>

#### <a name="include-notification-as-part-of-an-existing-view"></a>Bildirim var olan bir görünümü bir parçası olarak dahil
Yer paylaşımları hızlı tümleştirme için oldukça uygundur ancak bazen uygun olamaz ya da istenmeyen yan etkileri olabilir.

Kendi görünümlerinizi bazıları katmana sistemde memnun değilseniz, bu görünümleri için özelleştirebilirsiniz.

Bizim bildirim düzeni varolan görünümlerinizi içerecek şekilde karar verebilirsiniz. Bunu yapmak için iki uygulama stili vardır:

1. Arabirim Oluşturucusu'nu kullanarak bildirim görünümü ekleme

   * Açık *arabirim Oluşturucusu*
   * 320 x 60'ı (ya da iPad cihazında varsa 768 x 60) koyun `UIView` bildirim görünmesini istediğiniz yere
   * Bu görünüm için etiket değeri ayarlanamıyor: **36822491**
2. Bildirim görünümü programlı olarak ekleyin. Görünümünüzü hazırlarken yalnızca aşağıdaki kodu ekleyin:

       UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
       notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
       [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG` Makro bulunabilir `AEDefaultNotifier.h`.

> [!NOTE]
> Varsayılan bildirim bildirim düzeni bu görünüme dahil edilmiştir ve bir katmana için eklemez otomatik olarak algılar.
>
>

### <a name="announcements-and-polls"></a>Duyuruları ve yoklamaları
#### <a name="layouts"></a>Düzenleri
Dosyalarda değişiklik yapabilir `AEDefaultAnnouncementView.xib` ve `AEDefaultPollView.xib` varolan subviews türünü ve etiket değerleri tutmak sürece.

#### <a name="categories"></a>Kategoriler
##### <a name="alternate-layouts"></a>Alternatif düzenleri
Bildirimler gibi kampanya kategori duyuruları ve yoklamaları için alternatif düzenleri sağlamak için kullanılabilir.

Duyuru için bir kategori oluşturmak için genişletmelidir **AEAnnouncementViewController** ve reach modülünün başlatıldıktan sonra kaydedin:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [!NOTE]
> Bir kullanıcı kategorisiyle duyuru için bir bildirim her tıkladığınızda "Benim\_kategori", kayıtlı görünüm denetleyicinizi (Bu durumda `MyCustomAnnouncementViewController`) yöntemi çağrılarak başlatılır `initWithAnnouncement:` ve görünüm eklenir Geçerli uygulama penceresi.
>
>

Uygulamanızda `AEAnnouncementViewController` özelliği okumaya olacaktır sınıfı `announcement` , subviews başlatılamadı. Aşağıdaki örnek düşünün, burada iki etiket başlatılır kullanarak `title` ve `body` özelliklerini `AEReachAnnouncement` sınıfı:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Kendi görünümlerinizi kendiniz yüklemek istemediğiniz ancak yalnızca varsayılan duyuru görünüm düzenini yeniden kullanmak istediğiniz, yalnızca özel görünüm denetleyicinizi yapabilirsiniz sağlanan sınıfını genişleten `AEDefaultAnnouncementViewController`. Bu durumda, nib dosyası yinelenen `AEDefaultAnnouncementView.xib` ve özel görünüm denetleyicisi tarafından yüklenebilmesi için yeniden adlandırın (adlı bir denetleyici için `CustomAnnouncementViewController`, nib dosyanızı çağırmalıdır `CustomAnnouncementView.xib`).

Duyurular varsayılan kategorisini değiştirmek için yalnızca özel görünüm denetleyicinizi tanımlanan kategori için kayıt `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Tıpkı yoklamalar özelleştirilebilir:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Bu süre, sağlanan `MyCustomPollViewController` genişletmelidir `AEPollViewController`. Veya varsayılan denetleyicisinden genişletmek seçebilirsiniz: `AEDefaultPollViewController`.

> [!IMPORTANT]
> Ya da çağırmayı unuttunuz mu `action` (`submitAnswers:` özel yoklama görünüm denetleyicileri için) veya `exit` görünüm denetleyicisini kapatıldığında önce yöntemi. Aksi takdirde istatistikleri (yani bir kampanya hiçbir analytics) gönderilmez ve uygulama işlemini yeniden başlatılana kadar daha fazla önemlisi sonraki Kampanyalar bildirilmez.
>
>

##### <a name="implementation-example"></a>Uygulama örneği
Bu uygulama özel duyuru görünümü bir dış xib dosyasından yüklenir.

Gibi gelişmiş bildirim özelleştirme için standart uygulama kaynak koduna bakmanız önerilir.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
