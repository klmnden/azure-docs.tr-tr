---
title: "Bildirim hub'ları yerelleştirilmiş çiğnemekten haber öğretici için iOS"
description: "Yerelleştirilmiş son dakika haberi bildirimleri (iOS) göndermek için Azure hizmet veri yolu bildirim hub'ları kullanmayı öğrenin."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: fd2b7d9dfd4f432bbcbaa3ed76f8bec0b9677e17
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>İOS cihazları için yerelleştirilmiş son dakika haberleri göndermek için Notification Hubs kullanma
> [!div class="op_single_selector"]
> * [Windows mağazası C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Bu konuda nasıl kullanılacağını gösterir [şablonları](notification-hubs-templates-cross-platform-push-messages.md) dil ve cihaz tarafından yerelleştirilmiş son dakika haberi bildirimleri yayınlamak için Azure Notification Hubs özelliğidir. Bu öğreticide oluşturduğunuz iOS uygulaması ile başlamanız [son dakika haberleri göndermek için Notification Hubs kullanma]. Tamamlandığında, ilgilendiğiniz kategorileri için kaydetme, hangi bildirimleri almak ve o dilde yalnızca seçili kategorileri için anında iletme bildirimleri almak bir dil belirtin mümkün olacaktır.

Bu senaryo iki bölümü vardır:

* iOS uygulamanızın istemci aygıtlar bir dil belirtin ve farklı son dakika haberleri kategorilere abone olmak için olanak tanır;
* arka uç kullanarak bildirimleri yayınlar **etiketi** ve **şablonu** Azure bildirim hub'larını feautres.

## <a name="prerequisites"></a>Ön koşullar
Önceden tamamlamış olmalıdır [son dakika haberleri göndermek için Notification Hubs kullanma] öğretici ve bu öğreticinin bu kodu doğrudan derlemeler için kullanılabilir, koda sahip.

Visual Studio 2012 veya sonraki sürümünü isteğe bağlıdır.

## <a name="template-concepts"></a>Şablon kavramları
İçinde [son dakika haberleri göndermek için Notification Hubs kullanma] kullanılan bir uygulama yerleşik **etiketleri** farklı haber kategorileri için Bildirimlere abone olma.
Birçok uygulama, ancak, birden çok pazarda hedef ve yerelleştirme gerektirir. Bildirimler içeriğe sahip yerelleştirilmiş ve aygıtların doğru kümesine teslim anlamına gelir.
Bu konudaki nasıl kullanılacağını göstereceğiz **şablonu** kolayca yerelleştirilmiş son dakika haberi bildirimleri göndermek için Notification Hubs özelliğidir.

Not: yerelleştirilmiş bildirimleri göndermek için bir yol her etiket birden fazla sürümünü oluşturmaktır. Örneğin, İngilizce, Fransızca ve Mandarin desteklemek için üç farklı etiketler world haberler için ihtiyacımız: "world_en", "world_fr" ve "world_ch". Biz sonra world haber yerelleştirilmiş bir sürümünde bu etiketlerin her biri için göndermesi gerekir. Bu konudaki etiketleri artışı ve birden fazla ileti gönderme gereksinimi önlemek için şablonlar kullanın.

Yüksek bir düzeyde şablonları belirli bir aygıt bir bildirim nasıl alacağını belirtmek için bir yoldur. Şablon, uygulama arka ucu tarafından gönderilen ileti parçası olan özellikler için bakarak tam yük biçimi belirtir. Örneğimizde, biz tüm desteklenen diller içeren bir yerel ayar belirsiz ileti gönderir:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Ardından aygıtları doğru özelliğine başvuran bir şablon ile kaydetmeye sağlayacaktır. Kaydetmek için Fransızca haber isteyen bir iOS uygulaması örneği için aşağıdaki kaydeder:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Şablonlar, daha fazla bilgi bulabilir hakkında içinde çok güçlü bir özellik olan bizim [şablonları](notification-hubs-templates-cross-platform-push-messages.md) makalesi.

## <a name="the-app-user-interface"></a>Uygulama kullanıcı arabirimi
Biz şimdi konu başlığı altında oluşturulan yeni haber uygulama değiştirecek [son dakika haberleri göndermek için Notification Hubs kullanma] son dakika haberleri şablonları kullanarak göndermek için yerelleştirilmiş.

MainStoryboard_iPhone.storyboard, biz destekleyen üç dilleri ile bölümlenmiş bir denetim ekleyin: İngilizce, Fransızca ve Mandarin.

![][13]

Ardından aşağıda gösterildiği gibi ViewController.h bir IBOutlet eklediğinizden emin olun:

![][14]

## <a name="building-the-ios-app"></a>İOS uygulaması oluşturma
1. Notification.h ekleme *retrieveLocale* yöntemi, mağaza değiştirmek ve yöntemleri aşağıda gösterildiği gibi abone:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    Notification.m içinde değişiklik *storeCategoriesAndSubscribe* yerel ayar parametresi ekleme ve kullanıcı varsayılan ayarlarında depolayarak yöntemi:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
   
    Ardından değiştirin *abone* sınırlarsınız yöntemi:
   
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
   
    Nasıl yöntemi şimdi kullanıyoruz Not *registerTemplateWithDeviceToken*, yerine *registerNativeWithDeviceToken*. Biz için bir şablon kaydederken (uygulamamıza farklı şablonlarını kaydetmek isteyebilirsiniz gibi) için json şablonunu ve ayrıca şablon için bir ad sağlayın sahip. Bu haber notifciations almaya emin olmak istiyoruz gibi kategoriler etiketler kaydetmek emin olun.
   
    Yerel kullanıcı varsayılan ayarlardan almak için bir yöntem ekleyin:
   
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
2. Biz bizim bildirimleri sınıfı değiştirilmiş, bizim ViewController yeni UISegmentControl kullanmak yapar emin olmak gerekir. Aşağıdaki satırda ekleme *viewDidLoad* şu anda seçili yerel göstermek emin olmak için yöntem:
   
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
   
    Ardından, *abone* yöntemi, aramanız için değiştirme *storeCategoriesAndSubscribe* şu:
   
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
3. Son olarak, güncelleştirmeye sahip *didRegisterForRemoteNotificationsWithDeviceToken* , AppDelegate.m yönteminde böylece uygulamanız başladığında kaydınızı doğru yenileyebilirsiniz. Aramanız için değiştirme *abone* aşağıdaki bildirimler yöntemi:
   
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a>(isteğe bağlı) .NET konsol uygulamasından yerelleştirilmiş şablon bildirimleri gönderin.
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-the-device"></a>(isteğe bağlı) Aygıttan yerelleştirilmiş şablon bildirimleri gönderme
Visual Studio erişimi veya yalnızca istiyorsanız yok cihaza uygulamanın doğrudan yerelleştirilmiş şablon bildirimleri gönderme sınayın.  Basit yapabilecekleriniz yerelleştirilmiş şablon parametreleri ekleme `SendNotificationRESTAPI` önceki öğreticide tanımladığınız yöntemi.

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




## <a name="next-steps"></a>Sonraki Adımlar
Şablonları kullanma hakkında daha fazla bilgi için bkz:

* [Notification Hubs ile kullanıcılara bildirin: ASP.NET]
* [Notification Hubs ile kullanıcılara bildirin: Mobil hizmetler]

<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[son dakika haberleri göndermek için Notification Hubs kullanma]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs ile kullanıcılara bildirin: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notification Hubs ile kullanıcılara bildirin: Mobil hizmetler]: /manage/services/notification-hubs/notify-users
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
