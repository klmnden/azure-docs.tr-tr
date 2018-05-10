---
title: Azure Notification Hubs'ı kullanarak iOS cihazlarını yerelleştirilmiş bildirimleri gönderme | Microsoft Docs
description: Azure Notification Hubs'ı kullanarak iOS cihazları için yerelleştirilmiş anında iletme bildirimleri kullanmayı öğrenin.
services: notification-hubs
documentationcenter: ios
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: b3d74086ee233da50138aff00d8da78aa0243a75
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-push-localized-notifications-to-ios-devices-using-azure-notification-hubs"></a>Öğretici: Anında iletme bildirimleri göndermek için Azure Notification Hubs'ı kullanarak iOS cihazlarını yerelleştirilmiş 
> [!div class="op_single_selector"]
> * [Windows mağazası C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 

Bu öğretici nasıl kullanılacağını gösterir [şablonları](notification-hubs-templates-cross-platform-push-messages.md) dil ve cihaz tarafından yerelleştirilmiş son dakika haberi bildirimleri yayınlamak için Azure Notification Hubs özelliğidir. Bu öğreticide oluşturduğunuz iOS uygulamasıyla Başlat [son dakika haberleri göndermek için Notification Hubs kullanma]. Tamamlandığında, ilgilendiğiniz kategorileri için kaydetme, hangi bildirimleri almak bir dil belirtin ve yalnızca anında iletme bildirimlerini seçili kategorileri için o dilde alırsınız.

Bu senaryo iki bölümü vardır:

* iOS uygulamanızın istemci aygıtlar bir dil belirtin ve farklı son dakika haberleri kategorilere abone olmak için olanak tanır;
* Arka uç kullanarak bildirimleri yayınlar **etiketi** ve **şablonu** Azure Notification Hubs özellikleri.

Bu öğreticide, aşağıdaki adımları uygulayın:

> [!div class="checklist"]
> * Güncelleştirme uygulama kullanıcı arabirimi
> * İOS uygulaması oluşturma
> * .NET konsol uygulamasından yerelleştirilmiş şablon bildirimleri gönderme
> * Aygıttan yerelleştirilmiş şablon bildirimleri gönderme


## <a name="overview"></a>Genel Bakış
İçinde [son dakika haberleri göndermek için Notification Hubs kullanma], kullanılan bir uygulama yerleşik **etiketleri** farklı haber kategorileri için Bildirimlere abone olma. Birçok uygulama, ancak, birden çok pazarda hedef ve yerelleştirme gerektirir. İçerik anlamına bildirimleri kendilerini yerelleştirilmiş ve aygıtların doğru kümesine teslim gerekir. Bu öğretici nasıl kullanılacağını gösterir **şablonu** kolayca yerelleştirilmiş son dakika haberi bildirimleri göndermek için Notification Hubs özelliğidir.

> [!NOTE]
> Yerelleştirilmiş bildirimleri göndermek için bir yolu, her bir etiketin birden fazla sürümünü oluşturmaktır. Örneğin, İngilizce, Fransızca ve Mandarin desteklemek için üç farklı etiketler world haberler için gerekir: "world_en", "world_fr" ve "world_ch". Bu etiketlerin her biri için yerelleştirilmiş bir sürümün world Haberler göndermek gerekir. Bu konuda, etiketleri artışı ve birden fazla ileti gönderme gereksinimi önlemek için şablonlar kullanın.

Yüksek bir düzeyde şablonları belirli bir aygıt bir bildirim nasıl alacağını belirtmek için bir yoldur. Şablon, uygulama arka ucu tarafından gönderilen ileti parçası olan özellikler için bakarak tam yük biçimi belirtir. Sizin durumunuzda, tüm desteklenen diller içeren bir yerel ayar belirsiz ileti gönder:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Ardından aygıtları doğru özelliğine başvuran bir şablon ile kaydetmeye emin olun. Örneğin, aşağıdaki sözdizimini kullanarak kaydetmek için Fransızca haber isteyen bir iOS uygulaması kaydeder:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Şablonlar hakkında daha fazla bilgi için bkz: [şablonları](notification-hubs-templates-cross-platform-push-messages.md) makalesi.

## <a name="prerequisites"></a>Önkoşullar

- Tamamlamak [anında iletme bildirimleri belirli iOS cihazlarına](notification-hubs-ios-xplat-segmented-apns-push-notification.md) öğretici ve bu öğreticinin bu kodu doğrudan derlemeler için kullanılabilir, koda sahip.
- Visual Studio 2012 veya sonraki sürümünü isteğe bağlıdır.

## <a name="update-the-app-user-interface"></a>Güncelleştirme uygulama kullanıcı arabirimi
Bu bölümde, konu başlığı altında oluşturulan yeni haber uygulamayı değiştirmek [son dakika haberleri göndermek için Notification Hubs kullanma] son dakika haberleri şablonları kullanarak göndermek için yerelleştirilmiş.

MainStoryboard_iPhone.storyboard içinde üç dilleri ile bölümlenmiş bir denetim ekleyin: İngilizce, Fransızca ve Mandarin.

![][13]

Ardından aşağıdaki resimde gösterildiği gibi ViewController.h bir IBOutlet eklediğinizden emin olun:

![][14]

## <a name="build-the-ios-app"></a>İOS uygulaması oluşturma
1. Notification.h ekleme *retrieveLocale* yöntemi, deposu değiştirin ve aşağıdaki kodda gösterildiği gibi yöntemleri abone:
   
    ```obj-c
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    ```
    Notification.m içinde değişiklik *storeCategoriesAndSubscribe* yerel ayar parametresi ekleme ve kullanıcı varsayılan ayarlarında depolayarak yöntemi:
   
    ```obj-c
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
    ````
    Ardından değiştirin *abone* sınırlarsınız yöntemi:
   
    ```obj-c
        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];
   
            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }
   
            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];
   
            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }
       ```
    You use the method *registerTemplateWithDeviceToken*, instead of *registerNativeWithDeviceToken*. When you register for a template, you have to provide the json template and also a name for the template (as the app might want to register different templates). Make sure to register your categories as tags, as you want to make sure to receive the notifciations for those news.
   
    Add a method to retrieve the locale from the user default settings:
   
    ```obj-c
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
    ```
2. Bildirimleri sınıfı değiştirilmiş, ViewController yeni UISegmentControl kullanmak yapar emin yapmanız gerekir. Aşağıdaki satırda ekleme *viewDidLoad* şu anda seçili yerel göstermek emin olmak için yöntem:
   
    ```obj-c
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
     ```  
    Ardından, *abone* yöntemi, aramanız için değiştirme *storeCategoriesAndSubscribe* aşağıdaki kodu için:
   
    ```obj-c
        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];
    ```
3. Son olarak, güncelleştirmeye sahip *didRegisterForRemoteNotificationsWithDeviceToken* , AppDelegate.m yönteminde böylece uygulamanız başladığında kaydınızı doğru yenileyebilirsiniz. Aramanız için değiştirme *abone* bildirimler aşağıdaki kod ile yöntemi:
   
    ```obj-c
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];
    ```

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a>(isteğe bağlı) .NET konsol uygulamasından yerelleştirilmiş şablon bildirimleri gönderme
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-the-device"></a>(isteğe bağlı) Aygıttan yerelleştirilmiş şablon bildirimleri gönderme
Visual Studio erişimi veya yalnızca istiyorsanız yok cihaza uygulamanın doğrudan yerelleştirilmiş şablon bildirimleri gönderme sınayın. Yerelleştirilmiş şablon parametreleri ekleyebilirsiniz `SendNotificationRESTAPI` önceki öğreticide tanımladığınız yöntemi.

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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

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


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, iOS cihazlarına yerelleştirilmiş bildirimler gönderilir. İOS uygulamalarını belirli kullanıcılara anında iletme bildirimleri öğrenmek için aşağıdaki öğreticide ilerlemek: 

> [!div class="nextstepaction"]
>[Belirli kullanıcılara anında iletme bildirimleri](notification-hubs-aspnet-backend-ios-apple-apns-notification.md)

<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[son dakika haberleri göndermek için Notification Hubs kullanma]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
