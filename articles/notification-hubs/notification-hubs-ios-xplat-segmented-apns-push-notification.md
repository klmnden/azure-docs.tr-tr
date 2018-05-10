---
title: Azure bildirim hub'ları kullanarak belirli iOS cihazları için anında iletme bildirimleri | Microsoft Docs
description: Bu öğreticide, Azure Notification Hubs belirli iOS cihazlara anında iletme bildirimleri göndermek için nasıl kullanılacağını öğrenin.
services: notification-hubs
documentationcenter: ios
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 6ead4169-deff-4947-858c-8c6cf03cc3b2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: f6096238deb2186edfac2eb9d1c9a9e76db07553
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-push-notifications-to-specific-ios-devices-using-azure-notification-hubs"></a>Öğretici: Azure bildirim hub'ları kullanarak belirli iOS cihazları için anında iletme bildirimleri
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, Azure Notification Hubs son dakika haberi bildirimleri bir iOS uygulamasına yayını için nasıl kullanılacağını gösterir. Tamamlandığında, ilgilendiğiniz haber kategorileri dökümü için kaydettirebilir ve yalnızca bu kategorilerin anında iletme bildirimlerinin alırsınız. Bu sayıda uygulama için genel bir desen bildirimleri daha önce bunları ilgi örneğin bildirilen kullanıcılar, RSS Okuyucu, müzik fanlar, vb. için uygulama gruplarının gönderilmesi sahip olduğu bir senaryodur.

Yayın senaryoları etkin bir veya daha fazla dahil ederek *etiketleri* bir kayıt bildirim hub'ı oluştururken. Bir etiket için bildirimler gönderildiğinde kaydettiğiniz cihazlar etiketi için bildirim alırsınız. Etiketleri yalnızca dizeleri olduğundan, bunlar önceden hazırlanması gerekmez. Etiketler hakkında daha fazla bilgi için bkz: [bildirim hub'ları Yönlendirme ve etiket ifadeleri](notification-hubs-tags-segment-push-message.md).

Bu öğreticide, aşağıdaki adımları uygulayın:

> [!div class="checklist"]
> * Bir kategori seçimi için uygulama ekleme
> * Etiketli bildirimleri gönderme
> * Aygıttan bildirimleri gönderme
> * Uygulamayı çalıştırın ve bildirimler oluşturma

## <a name="prerequisites"></a>Önkoşullar
Bu konu, oluşturduğunuz uygulama inşa edilmiştir [Öğreticisi: anında iletme bildirimleri Azure bildirim hub'ları kullanarak iOS uygulamaları için][get-started]. Bu öğreticiye başlamadan önce önce tamamlamış olmalıdır [Öğreticisi: anında iletme bildirimleri Azure bildirim hub'ları kullanarak iOS uygulamaları için][get-started].

## <a name="add-category-selection-to-the-app"></a>Kategori seçimi için uygulama ekleme
İlk adım kullanıcının kaydetmek için kategoriler seçmesini sağlayan varolan şeridinizin için kullanıcı Arabirimi öğeleri eklemektir. Bir kullanıcı tarafından seçilen kategoriler cihazda depolanır. Uygulama başlatıldığında bir aygıt kaydı bildirim hub'ınıza seçili kategorileri etiketler oluşturulur.

1. MainStoryboard_iPhone.storyboard nesne kitaplığından aşağıdaki bileşenleri ekleyin:
   
   * Bir etiket "Haber sonu" metni ile
   * Kategori metinleri ile etiketler "World", "Siyaset", "İş", "Teknolojisinin", "Bilim", "Spor",
   * Her kategori, altı anahtarları ayarlamak her anahtar **durumu** olmasını **kapalı** varsayılan olarak.
   * Bir düğme "Abonelik" olarak etiketlenmiş
     
     Şeridinizin aşağıdaki gibi görünmelidir:
     
     ![][3]
2. Yardımcısı düzenleyicisinde çıkışlar tüm anahtarları oluşturmak ve "WorldSwitch", çağrı "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"
3. Adlandırılan, bir düğme için bir eylem oluşturun **abone**. Aşağıdaki kod, ViewController.h içermelidir:
   
    ```obj-c
        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;
   
        - (IBAction)subscribe:(id)sender;
    ```
4. Yeni bir **Cocoa Touch sınıfı** adlı `Notifications`. Aşağıdaki kodu dosyanın Notifications.h arabirimi bölümünde kopyalayın:
   
    ```obj-c
        @property NSData* deviceToken;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;
   
        - (NSSet*)retrieveCategories;
   
        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
    ```
5. Aşağıdaki içeri aktarma yönergesi için Notifications.m ekleyin:
   
    ```obj-c
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
    ```
6. Aşağıdaki kodu dosyanın Notifications.m uygulama bölümünde kopyalayın.
   
    ```obj-c
        SBNotificationHub* hub;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{
   
            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];
   
            return self;
        }
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
   
            [self subscribeWithCategories:categories completion:completion];
        }

        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }

        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }
    ```

    Bu sınıf, depolamak ve bu aygıt alır haber kategorilerini almak için yerel depolama kullanır. Ayrıca, bu kategorileri kullanarak kaydetmek için bir yöntem içeren bir [şablonu](notification-hubs-templates-cross-platform-push-messages.md) kayıt.

1. AppDelegate.h dosyasında Notifications.h için içeri aktarma deyimini ekleyin ve bildirimleri sınıfının bir örneği için bir özellik ekleyin:
   
    ```obj-c
        #import "Notifications.h"
   
        @property (nonatomic) Notifications* notifications;
    ```
2. İçinde **didFinishLaunchingWithOptions** AppDelegate.m, yönteminde yönteminin başında bildirimleri örneği başlatmak için kodu ekleyin.  
   
    `HUBNAME` ve `HUBLISTENACCESS` (hubınfo.h içinde tanımlanmıştır) zaten yüklü olmalıdır `<hub name>` ve `<connection string with listen access>` yer tutucular yerine, bildirim hub'ı adı ve bağlantı dizesini *DefaultListenSharedAccessSignature* daha önce aldığınız
   
    ```obj-c
        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
    ```
   
   > [!NOTE]
   > Bir istemci uygulaması ile dağıtılmış kimlik bilgileri genellikle güvenli olmadığı için istemci uygulamanızı dinleme erişim için anahtar yalnızca dağıtmanız. Bildirimler, ancak var olan kayıtlar için kaydetmek için uygulamanızın değiştirilemez erişimi etkinleştirir dinleyin ve bildirim gönderilemiyor. Tam erişim anahtarı, bir güvenli arka uç hizmetinde bildirimleri gönderme ve var olan kayıtlar değiştirmek için kullanılır.
   > 
   > 
3. İçinde **didRegisterForRemoteNotificationsWithDeviceToken** AppDelegate.m, yönteminde yöntemindeki kod cihaz belirteci bildirimleri sınıfına geçirmek için aşağıdaki kod ile değiştirin. Bildirimleri sınıf kategorileri ile bildirimleri için kaydediliyor gerçekleştirir. Kullanıcı kategori seçimlerinin değiştirirse, çağrı `subscribeWithCategories` yöntemi yanıt olarak **abone** bunları güncelleştirmek için düğmesini.
   
   > [!NOTE]
   > Apple anında iletilen bildirim servisi (APNS) tarafından atanan cihaz belirteci herhangi bir anda fırsat çünkü bildirimlerinin sık bildirim hataları önlemek için kaydetmeniz. Bu örnek uygulama başladıktan her zaman bir bildirime kaydolur. Sık çalıştırılan uygulamalar için günde bir kereden fazla, büyük olasılıkla bir günden az itibaren önceki kayıt aktarılırsa bant genişliğinden tasarruf etmek kayıt atlayabilirsiniz.
   > 
   > 
   
    ```obj-c
        self.notifications.deviceToken = deviceToken;
   
        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.
   
        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];
    ```

    Bu noktada, olması gerekir, başka bir kod **didRegisterForRemoteNotificationsWithDeviceToken** yöntemi.

1. Aşağıdaki yöntemlerden zaten tamamlamanızı AppDelegate.m içinde bulunması gereken [Notification Hubs ile çalışmaya başlama] [ get-started] Öğreticisi. Yoksa, bunları ekleyin.
   
    ```obj-c    
    -(void)MessageBox:(NSString *)title message:(NSString *)messageText
    {
   
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
        [alert show];
    }
   
   * (void)application:(UIApplication *)application didReceiveRemoteNotification:
       (NSDictionary *)userInfo {
       NSLog(@"%@", userInfo);
       [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
     }
    ```
   
   Bu yöntem uygulama basit bir görüntüleyerek çalışırken alınan bildirimlerini işleme **Uıalert**.
2. ViewController.m, AppDelegate.h için içeri aktarma deyimini ekleyin ve XCode üretilmiş aşağıdaki kodu kopyalayın **abone** yöntemi. Bu kod, kullanıcının kullanıcı arabiriminde seçtiği yeni kategori etiketleri kullanmak için bildirimi kaydı güncelleştirir.
   
    ```obj-c
       #import "Notifications.h"
   
       NSMutableArray* categories = [[NSMutableArray alloc] init];
   
       if (self.WorldSwitch.isOn) [categories addObject:@"World"];
       if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
       if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
       if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
       if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
       if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];
   
       Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];
   
       [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
           if (!error) {
               [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
           } else {
               NSLog(@"Error subscribing: %@", error);
           }
       }];
    ```

   Bu yöntem oluşturur bir **NSMutableArray** kategorileri kullanır ve **bildirimleri** listesi karşılık gelen etiketleri bildirim hub'ınıza yerel depolama ve kayıtları depolamak için sınıf. Kategoriler değiştirildiğinde kaydı yeni kategoriler yeniden oluşturulur.
3. Aşağıdaki kodda ViewController.m içinde eklemek **viewDidLoad** önceden kaydedilmiş kategorilerindeki tabanlı kullanıcı arabirimi ayarlamak için yöntemini.

    ```obj-c    
        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;
    ```


Uygulamayı şimdi kategorileri kümesi uygulama başlattığında bildirim hub'ınızla kaydolmak için kullanılan aygıt yerel depolama alanına depolar. Kullanıcı kategorileri çalışma zamanı ve tıklatın seçimini değiştirebilir **abone** yöntemi cihazın kaydını güncelleştirin. Ardından, uygulama kendisini doğrudan son dakika haberi bildirimleri göndermek için uygulamayı güncelleştirme.

## <a name="optional-send-tagged-notifications"></a>(isteğe bağlı) Etiketli bildirimleri gönderme
Visual Studio erişimi yoksa, sonraki bölüme geçin ve uygulamasından bildirimleri gönderin. Uygun şablon bildirimden da gönderebilirsiniz [Azure portal] bildirim hub'ınız için hata ayıklama sekmesini kullanarak. 

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-the-device"></a>(isteğe bağlı) Aygıttan bildirimleri gönderme
Normalde bildirimleri bir arka uç hizmeti tarafından gönderilir ancak doğrudan uygulamadan son dakika haberi bildirimleri gönderebilir. Bunu yapmak için güncelleştirme `SendNotificationRESTAPI` tanımlanan yöntemi [Notification Hubs ile çalışmaya başlama] [ get-started] Öğreticisi.

1. İçinde `ViewController.m`, güncelleştirme `SendNotificationRESTAPI` yöntemi olarak, böylece kategori etiketi için bir parametre kabul eder ve uygun gönderir izleyen [şablonu](notification-hubs-templates-cross-platform-push-messages.md) bildirim.
   
    ```obj-c
        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];
   
            NSString *json;
   
            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];
   
            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];
   
            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
   
            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];
   
            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];
   
            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];
   
            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
   
            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];
   
            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
   
            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];
   
            [dataTask resume];
        }
    ```
2. İçinde `ViewController.m`, güncelleştirme **bildirim gönder** aşağıdaki kodda gösterildiği gibi eylem. Her etiket kullanarak tek tek bildirimleri gönderir ve birden çok platform gönderir böylece.

    ```obj-c
        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }
    ```


1. Projenizi yeniden derleyin ve hiçbir derleme hataları olduğundan emin olun.

## <a name="run-the-app-and-generate-notifications"></a>Uygulamayı çalıştırın ve bildirimler oluşturma
1. Projeyi oluşturmak ve uygulamayı başlatmak için Çalıştır düğmesine basın. Abone tuşuna basarak bazı en son haberleri seçeneklerini belirleyin ve **abone ol** düğmesi. Bildirimler için abone olunmuş belirten bir iletişim kutusu görmeniz gerekir.
   
    ![][1]
   
    Seçtiğinizde **abone ol**, uygulama seçili kategorileri etiketlerine dönüştürür ve bildirim hub'ından seçili etiketleri için yeni bir cihaz kaydı ister.
2. Son dakika haberleri tuşuna olarak gönderilecek bir ileti girin **bildirim gönder** düğmesi. Alternatif olarak, bildirim oluşturmak amacıyla .NET konsol uygulamasını çalıştırın.
   
    ![][2]
3. Son dakika haberleri abone olan her bir cihaz yalnızca gönderilen son dakika haberi bildirimleri alır.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, yayın bildirimleri kaydettiğiniz belirli iOS cihazları kategorilerde gönderdi. Anında iletme yerelleştirilmiş bildirimleri öğrenmek için aşağıdaki öğreticide ilerlemek: 

> [!div class="nextstepaction"]
>[Yerelleştirilmiş bildirimleri gönderme](notification-hubs-ios-xplat-localized-apns-push-notification.md)


<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Use Notification Hubs to broadcast localized breaking news]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Azure portal]: https://portal.azure.com
