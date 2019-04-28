---
title: Azure Notification hubs'ı kullanarak belirli iOS cihazlarına anında iletme bildirimleri gönderme | Microsoft Docs
description: Bu öğreticide, belirli iOS cihazlarına anında iletme bildirimleri göndermek için Azure Notification hubs'ı kullanmayı öğrenin.
services: notification-hubs
documentationcenter: ios
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 6ead4169-deff-4947-858c-8c6cf03cc3b2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: dd625dba0e125ccf993af524a0ab0c0cc66555fb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60873181"
---
# <a name="tutorial-push-notifications-to-specific-ios-devices-using-azure-notification-hubs"></a>Öğretici: Azure Notification hubs'ı kullanarak belirli iOS cihazlarına anında iletme bildirimleri gönderme

[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide flaş haber bildirimlerini bir iOS uygulamasını yayınlamak için Azure Notification hubs'ı kullanmayı gösterir. Tamamlandığında, ilgilendiğiniz haber kategorileri önemli için kayıt olanağına sahip olursunuz ve sadece bu kategorileri için anında iletme bildirimleri alın. Bu senaryo, daha önce ilgisini belirtmiş kullanıcı gruplarına bildirim gönderilmesi gereken RSS okuyucu, müzik hayranlarına yönelik uygulamalar vb. birçok uygulama için ortak düzendir.

Yayın senaryoları, bildirim hub’ında bir kayıt oluştururken bir veya daha fazla *etiket* dahil edilerek etkinleştirilir. Etiket bildirimleri gönderilirken, kayıtlı cihazları etiket için bildirimleri alır. Etiketler basitçe birer dize olduğundan, önceden hazırlanması gerekmez. Etiketler hakkında daha fazla bilgi için bkz. [Notification Hubs Yönlendirme ve Etiket İfadeleri](notification-hubs-tags-segment-push-message.md).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Bir kategori seçimi uygulamaya ekleme
> * Etiketli bildirimler gönderme
> * CİHAZDAN bildirimler gönderme
> * Uygulamayı çalıştırma ve bildirimler oluşturma

## <a name="prerequisites"></a>Önkoşullar

Bu konu başlığı altında oluşturduğunuz uygulama geliştirir [Öğreticisi: Anında iletme bildirimleri için Azure Notification hubs'ı kullanarak iOS uygulamaları][get-started]. Bu öğreticiye başlamadan önce zaten tamamlamış olmanız gerekir [Öğreticisi: Anında iletme bildirimleri için Azure Notification hubs'ı kullanarak iOS uygulamaları][get-started].

## <a name="add-category-selection-to-the-app"></a>Uygulamaya kategori seçimi ekleme

İlk adım, kullanıcı Arabirimi öğeleri kaydetmek için kategorileri seçmesini etkinleştirmek var olan film şeridini eklemektir. Bir kullanıcı tarafından seçilen kategoriler cihazda depolanır. Uygulama başlatıldığında, etiketler olarak seçilen kategorilerle bildirim hub’ınızda bir cihaz kaydı oluşturulur.

1. İçinde **MainStoryboard_iPhone.storyboard** nesne kitaplığından aşağıdaki bileşenlerini ekleyin:

   * "Yeni haber" yazan etiket,
   * Kategori metinlerle "World", "Siyaset", "İş", "Teknoloji", "Bilimsel", "Spor", etiketler
   * Kategori başına altı anahtarlar her anahtar kümesi **durumu** olmasını **kapalı** varsayılan olarak.
   * Bir düğme "Subscribe" olarak etiketlenmiş

     Film şeridini şu şekilde görünmelidir:

     ![Xcode arabirim Oluşturucu][3]

2. Yardımcısı düzenleyicideki tüm anahtarlarda çıkışlar oluşturmak ve "WorldSwitch" çağırmaya "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"
3. Adlandırılan, bir düğme için eylem oluşturma `subscribe`; `ViewController.h` aşağıdaki kodu içermesi gerekir:

    ```objc
    @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

    - (IBAction)subscribe:(id)sender;
    ```

4. Yeni bir **Cocoa Touch sınıfı** adlı `Notifications`. ' % S'dosyasının Notifications.h arabirimi bölümünde aşağıdaki kodu kopyalayın:

    ```objc
    @property NSData* deviceToken;

    - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

    - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                completion:(void (^)(NSError* error))completion;

    - (NSSet*)retrieveCategories;

    - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
    ```

5. Aşağıdaki içeri aktarma yönergesi için Notifications.m ekleyin:

    ```objc
    #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
    ```

6. ' % S'dosyasının Notifications.m uygulama bölümünde aşağıdaki kodu kopyalayın.

    ```objc
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

    Bu sınıf, depolamak ve bu cihazda alan haber kategorilerini almak için yerel depolama kullanır. Ayrıca, bir yöntem kullanarak bu kategoriler için kaydedilecek içerdiği bir [şablon](notification-hubs-templates-cross-platform-push-messages.md) kayıt.

7. İçinde `AppDelegate.h` için bir içeri aktarma deyimini ekleyin `Notifications.h` ve özellik için bir örneğini ekleme `Notifications` sınıfı:

    ```objc
    #import "Notifications.h"

    @property (nonatomic) Notifications* notifications;
    ```

8. İçinde `didFinishLaunchingWithOptions` yönteminde `AppDelegate.m`, yöntemin başında bildirimleri örneği başlatmak için kodu ekleyin.  
    `HUBNAME` ve `HUBLISTENACCESS` (tanımlanan `hubinfo.h`) zaten olmalıdır `<hub name>` ve `<connection string with listen access>` yer tutucular yerine, bildirim hub'ı adı ve bağlantı dizesini *DefaultListenSharedAccessSignature*daha önce edindiğiniz

    ```objc
    self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
    ```

    > [!NOTE]
    > Bir istemci uygulaması ile dağıtılmış kimlik bilgileri genellikle güvenli olmadığından yalnızca istemci uygulamanızla dinleme erişimi için anahtarı dağıtmanız gerekir. Dinleme erişimi, uygulamanızın bildirimlere kaydolmasını sağlar, ancak mevcut kayıtlar değiştirilemez ve bildirimler gönderilemez. Tam erişim anahtarı, güvenli bir arka uç hizmetinde bildirimler göndermek ve mevcut kayıtları değiştirmek için kullanılır.

9. İçinde `didRegisterForRemoteNotificationsWithDeviceToken` yönteminde `AppDelegate.m`, yöntem için cihaz belirteci geçirmek için aşağıdaki kodla değiştirin `notifications` sınıfı. `notifications` Sınıfı gerçekleştirir kategorileri ile bildirimleri için kaydediliyor. Kullanıcı kategori seçimlerinin değişirse, çağrı `subscribeWithCategories` yanıt olarak yöntemi **abone** düğmesini bunları güncelleştirin.

    > [!NOTE]
    > Apple anında iletilen bildirim servisi (APNS) tarafından atanan cihaz belirteci dilediğiniz zaman fırsat çünkü sık bildirim hatalarını önlemek, bildirimlerini de kaydetmeniz. Bu örnek, uygulama her başlatıldığında bildirimlere kaydolur. Sık sık çalıştırılan uygulamalar için, önceki kayıttan bu yana bir günden az zaman geçtiyse bant genişliğini korumak için günde birkaç kere kaydı atlayabilirsiniz.

    ```objc
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

    Bu noktada, bulunmamalıdır başka herhangi bir kod `didRegisterForRemoteNotificationsWithDeviceToken` yöntemi.

10. Aşağıdaki yöntemlerden zaten mevcut olmalıdır `AppDelegate.m` tamamlanmasını [Notification Hubs ile çalışmaya başlama] [ get-started] öğretici. Aksi durumda, bunları ekleyebilirsiniz.

    ```objc
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

    Bu yöntem uygulama basit bir görüntüleyerek çalışırken alınan bildirimleri işleme **Uıalert**.

11. İçinde `ViewController.m`, ekleme bir `import` bildirimi `AppDelegate.h` ve XCode tarafından oluşturulan aşağıdaki kodu kopyalayın `subscribe` yöntemi. Bu kod kullanıcı arabiriminin kullanıcının seçtiği yeni kategori etiketlerini kullanmak için bildirimi kaydı güncelleştirir.

    ```objc
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

    Bu yöntem, oluşturur bir `NSMutableArray` kategoriler kullanır ve `Notifications` listesinde karşılık gelen etiketleri bildirim hub'ınızla yerel depolama ve kayıtları depolamak için sınıf. Kategoriler değiştirildiğinde kayıt yeni kategorilerle yeniden oluşturulur.

12. İçinde `ViewController.m`, aşağıdaki kodu ekleyin `viewDidLoad` tabanlı kullanıcı arabirimi ayarlama yöntemini önceden kaydedilmiş kategorilere göre.

    ```objc
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

Uygulama, uygulama başlatıldığında, bildirim hub'ınızla kaydolmak için kullanılan cihazın yerel depolama alanına kategorileri kümesi artık depolayabilirsiniz. Kullanıcı kategorileri çalışma zamanı ve tıklatın seçimini değiştirebilir `subscribe` cihaz kaydı güncelleştirmek için yöntemi. Ardından, uygulamayı doğrudan uygulamanın içinde flaş haber bildirimlerini göndermek için güncelleştirin.

## <a name="optional-send-tagged-notifications"></a>(isteğe bağlı) Etiketli bildirimler gönderme

Visual Studio için erişiminiz yoksa sonraki bölüme atlayın ve uygulamadan bildirimleri gönderin. Ayrıca, gelen uygun şablon bildirim gönderebilirsiniz [Azure portal] bildirim hub'ınız için hata ayıklama sekmesini kullanarak.

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-the-device"></a>(isteğe bağlı) CİHAZDAN bildirimler gönderme

Normalde bildirimleri bir arka uç hizmeti tarafından gönderilir ancak doğrudan uygulamadan flaş haber bildirimlerini gönderebilirsiniz. Bunu yapmak için güncelleştirme `SendNotificationRESTAPI` içinde tanımlanan yöntem [Notification Hubs ile çalışmaya başlama] [ get-started] öğretici.

1. İçinde `ViewController.m`, güncelleştirme `SendNotificationRESTAPI` yöntemi olarak izler kategori etiket için bir parametreyi kabul eden ve uygun gönderir [şablon](notification-hubs-templates-cross-platform-push-messages.md) bildirim.

    ```objc
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

2. İçinde `ViewController.m`, güncelleştirme `Send Notification` aşağıdaki kodda gösterildiği gibi bir eylem. Her etiket kullanarak tek tek bildirimleri gönderir ve birden çok platforma gönderir böylece.

    ```objc
    - (IBAction)SendNotificationMessage:(id)sender
    {
        self.sendResults.text = @"";

        NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                @"Technology", @"Science", @"Sports", nil];

        // Lets send the message as breaking news for each category to WNS, FCM, and APNS
        // using a template.
        for(NSString* category in categories)
        {
            [self SendNotificationRESTAPI:category];
        }
    }
    ```

3. Projenizi yeniden derleyin ve herhangi bir yapı hatası olduğundan emin olun.

## <a name="run-the-app-and-generate-notifications"></a>Uygulamayı çalıştırma ve bildirimler oluşturma

1. Projeyi oluşturmak ve uygulamayı başlatmak için Çalıştır düğmesine basın. Abone olmak ve ENTER tuşuna basın bazı flaş haber seçenekleri belirleyin **abone ol** düğmesi. Bildirimler için abone olunmuş belirten bir iletişim kutusu görmeniz gerekir.

    ![İos'ta örnek bildirim][1]

    Seçeneğini belirlediğinizde **abone ol**, uygulama etiketi ekleyerek seçili kategorilerdeki dönüştürür ve bildirim hub'ından seçili etiketler için yeni bir cihaz kaydı ister.

2. Son dakika haberleri tuşuna olarak gönderilecek bir ileti girin **bildirim gönder** düğmesi. Alternatif olarak, bildirimleri oluşturmak için .NET konsol uygulaması çalıştırın.

    ![İOS bildirim tercihlerinizi değiştirin][2]

3. Son dakika haberleri abone her bir cihaz gönderdiğiniz flaş haber bildirimlerini alır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kayıtlı belirli iOS cihazlara kategorileri için yayın bildirimleri gönderdiniz. Yerelleştirilmiş bildirimler gönderme öğrenmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Yerelleştirilmiş anında iletme bildirimleri gönderme](notification-hubs-ios-xplat-localized-apns-push-notification.md)

<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png

<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: https://msdn.microsoft.com/library/jj927168.aspx
[Use Notification Hubs to broadcast localized breaking news]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: https://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: https://msdn.microsoft.com/library/jj927168.aspx
[get-started]: notification-hubs-ios-apple-push-notification-apns-get-started.md
[Azure portal]: https://portal.azure.com
