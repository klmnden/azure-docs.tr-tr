---
title: Azure Notification hubs'ı kullanarak iOS cihazlarına yerelleştirilmiş bildirimler gönderme | Microsoft Docs
description: Azure Notification Hubs'ı kullanarak iOS cihazlarını yerelleştirilmiş anında iletme bildirimleri kullanmayı öğrenin.
services: notification-hubs
documentationcenter: ios
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: a293f0b656c075ae3b21ccf98e602e43ed761958
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428447"
---
# <a name="tutorial-push-localized-notifications-to-ios-devices-using-azure-notification-hubs"></a>Öğretici: Azure Notification Hubs'ı kullanarak iOS cihazlarını yerelleştirilmiş anında iletme bildirimleri

> [!div class="op_single_selector"]
> * [Windows Mağazası C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

Bu öğreticide nasıl kullanılacağını gösterir [şablonları](notification-hubs-templates-cross-platform-push-messages.md) dil ve cihaz tarafından yerelleştirilmiş flaş haber bildirimlerini yayınlamak için bildirim hub'ları Azure özelliğidir. Bu öğreticide oluşturduğunuz iOS uygulaması ile başlamanız [Son dakika haberleri göndermek için Notification Hubs’ı kullanma]. Tamamlandığında, ilgilendiğiniz kategorileri için kaydetme, hangi bildirimleri almak bir dil belirtin ve o dilde yalnızca seçilen kategorilerdeki için anında iletme bildirimleri alın.

Bu senaryo iki bölümü vardır:

* iOS uygulaması, cihazların bir dil belirtin ve farklı flaş haber kategorileri için abone olmak için istemci sağlar;
* Arka uç kullanarak bildirimleri yayınlar **etiketi** ve **şablon** Azure Notification Hubs'ın özellikleri.

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Uygulama kullanıcı arabirimini güncelleştirme
> * İOS uygulaması oluşturma
> * .NET konsol uygulamasından şablon yerelleştirilmiş bildirimler gönderme
> * CİHAZDAN şablon yerelleştirilmiş bildirimler gönderme

## <a name="overview"></a>Genel Bakış

İçinde [Son dakika haberleri göndermek için Notification Hubs’ı kullanma], kullanılan uygulama yerleşik **etiketleri** farklı haber kategorileri için Bildirimlere abone olma. Birçok uygulama, ancak birden çok pazarları hedeflemek ve yerelleştirme gerektirir. İçerik anlamına bildirimleri kendilerini yerelleştirilir ve cihazların doğru kümesine teslim gerekir. Bu öğreticide nasıl kullanılacağını gösterir **şablon** yerelleştirilmiş flaş haber bildirimlerini kolayca sağlamak için Notification hubs'ın bir özelliğidir.

> [!NOTE]
> Yerelleştirilmiş bildirimler gönderme yollarından biri, her bir etiketin birden çok sürümünü oluşturmaktır. Örneğin, İngilizce, Fransızca ve Mandarin desteklemek için üç farklı etiketler dünya Haberleri için gerekir: "world_en", "world_fr" ve "world_ch". Ardından, dünya haberleri yerelleştirilmiş bir sürümünü her etiketlerin göndermek gerekir. Bu konu başlığında, işyerinde etiketleri ve birden çok ileti gönderme gereksinimi önlemek için şablonları kullanın.

Şablonları, belirli bir cihaza bir bildirim nasıl alacağını belirtmek için kullanılan bir yoludur. Şablon, uygulama arka ucunuz tarafından gönderilen iletinin parçası olan özelliklere başvurarak tam yük biçimini belirtir. Sizin durumunuzda, desteklenen tüm dilleri içeren bir yerel ayardan bağımsız mesaj gönder:

```json
{
    "News_English": "...",
    "News_French": "...",
    "News_Mandarin": "..."
}
```

Ardından cihazların doğru özelliğine başvuran bir şablon ile kayıt olun. Örneğin, aşağıdaki sözdizimini kullanarak kaydetmek için Fransızca haber isteyen bir iOS uygulamasını kaydeder:

```json
{
    aps: {
        alert: "$(News_French)"
    }
}
```

Şablonlar hakkında daha fazla bilgi için bkz. [şablonları](notification-hubs-templates-cross-platform-push-messages.md) makalesi.

## <a name="prerequisites"></a>Önkoşullar

* Tamamlamak [anında iletme bildirimleri belirli iOS cihazlarına](notification-hubs-ios-xplat-segmented-apns-push-notification.md) öğretici ve bu Öğreticide bu kodu doğrudan oluşturur çünkü kullanılabilir koda sahip.
* Visual Studio 2019 isteğe bağlıdır.

## <a name="update-the-app-user-interface"></a>Uygulama kullanıcı arabirimini güncelleştirme

Bu bölümde, konu başlığı altında oluşturduğunuz bozucu News uygulamasının değiştirme [Son dakika haberleri göndermek için Notification Hubs’ı kullanma] son dakika haberleri şablonlarını kullanarak göndermek için yerelleştirilmiş.

İçinde `MainStoryboard_iPhone.storyboard`, üç dilleri ile bölümlenmiş bir denetim ekleyin: İngilizce, Fransızca ve Mandarin.

![İOS kullanıcı Arabirimi görsel taslak oluşturma][13]

Ardından aşağıdaki görüntüde gösterildiği gibi ViewController.h içinde bir IBOutlet eklediğinizden emin olun:

![Çıkışlar anahtarları oluşturma][14]

## <a name="build-the-ios-app"></a>İOS uygulaması oluşturma

1. İçinde `Notification.h`, ekleme `retrieveLocale` yöntemi, deponun değiştirin ve aşağıdaki kodda gösterildiği gibi abone olma yöntemlerini:

    ```objc
    - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

    - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

    - (NSSet*) retrieveCategories;

    - (int) retrieveLocale;
    ```
    İçinde `Notification.m`, değişiklik `storeCategoriesAndSubscribe` ekleyerek yöntemi `locale` parametresi ve kullanıcı varsayılan ayarlarında Depolama:

    ```objc
    - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
        NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
        [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
        [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
        [defaults synchronize];

        [self subscribeWithLocale: locale categories:categories completion:completion];
    }
    ```

    Ardından değiştirin *abone* sınırlarsınız yöntemi:

    ```objc
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

    Yöntem `registerTemplateWithDeviceToken`, yerine `registerNativeWithDeviceToken`. Bir şablon için kaydolduğunuzda, (uygulama farklı şablonlarını kaydetmek isteyebilirsiniz gibi) için json şablonunu ve şablon için bir ad sağlayın gerekir. Bu haberler için bildirimleri almak emin olmak istediğiniz kadar kategorileriniz etiketler kaydetmek emin olun.

    Kullanıcı varsayılan ayarlardan yerel ayarı almak için bir yöntem ekleyin:

    ```objc
    - (int) retrieveLocale {
        NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

        int locale = [defaults integerForKey:@"BreakingNewsLocale"];

        return locale < 0?0:locale;
    }
    ```

2. Değiştirdiğiniz göre `Notifications` sınıfı sahip emin olmak `ViewController` kullanır yeni `UISegmentControl`. Aşağıdaki satırı ekleyin `viewDidLoad` yöntemi şu anda seçili olan yerel ayar gösterilecek emin olmak için:

    ```objc
    self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
    ```

    Ardından, `subscribe` yöntem çağrınız değiştirme `storeCategoriesAndSubscribe` aşağıdaki koda:

    ```objc
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

3. Son olarak, güncelleştirilecek sahip `didRegisterForRemoteNotificationsWithDeviceToken` , AppDelegate.m yönteminde böylece uygulamanız başlatıldığında kaydınız doğru şekilde yenileyebilirsiniz. Çağrınız değiştirme `subscribe` yöntemini aşağıdaki kodla bildirimler:

    ```obj-c
    NSSet* categories = [self.notifications retrieveCategories];
    int locale = [self.notifications retrieveLocale];
    [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
        if (error != nil) {
            NSLog(@"Error registering for notifications: %@", error);
        }
    }];
    ```

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a>(isteğe bağlı) .NET konsol uygulamasından şablon yerelleştirilmiş bildirimler gönderme

[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-the-device"></a>(isteğe bağlı) CİHAZDAN şablon yerelleştirilmiş bildirimler gönderme

Visual Studio erişimi veya yalnızca istediğiniz yoksa doğrudan cihazda uygulama içinden yerelleştirilmiş şablon bildirimleri göndermeyi test edin. Yerelleştirilmiş şablon parametreleri ekleyebilirsiniz `SendNotificationRESTAPI` önceki öğreticide tanımlanan yöntemi.

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

Bu öğreticide, iOS cihazları için yerelleştirilmiş bildirimleri gönderdiniz. İOS uygulamaları, belirli kullanıcılara anında iletme bildirimleri öğrenmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Belirli kullanıcılara anında iletme bildirimleri gönderme](notification-hubs-aspnet-backend-ios-apple-apns-notification.md)

<!-- Images. -->
[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png

<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: https://msdn.microsoft.com/library/jj927168.aspx
[Son dakika haberleri göndermek için Notification Hubs’ı kullanma]: notification-hubs-ios-xplat-segmented-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Notify users with Notification Hubs: Mobile Services]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: https://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md
[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: https://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: https://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: https://msdn.microsoft.com/library/jj927168.aspx
