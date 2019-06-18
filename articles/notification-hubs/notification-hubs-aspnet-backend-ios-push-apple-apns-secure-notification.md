---
title: Azure Notification hubs'ı güvenli gönderme
description: Azure'dan bir iOS uygulamasına güvenli anında iletme bildirimleri göndermeyi öğrenin. Objective-C ve C# içinde yazılan kod örneklerini.
documentationcenter: ios
author: jwargo
manager: patniko
editor: spelluru
services: notification-hubs
ms.assetid: 17d42b0a-2c80-4e35-a1ed-ed510d19f4b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: d88bdb1eaeb95413df84bf69ed4fc763b6d4901f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61458504"
---
# <a name="azure-notification-hubs-secure-push"></a>Azure Notification hubs'ı güvenli gönderme

> [!div class="op_single_selector"]
> * [Windows Evrensel](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

## <a name="overview"></a>Genel Bakış

Mobil için tüketici hem kurumsal uygulamalar için anında iletme bildirimleri yürütmesinin büyük ölçüde basitleştirir ve kullanımı kolay, çok platformlu, ölçeği genişletilmiş bir anında iletme altyapı, erişmek Microsoft azure'da anında iletme bildirimi desteği sağlar Platform.

Yasal nedeniyle veya güvenlik kısıtlamaları, bazen bir uygulama bir sorun standart bir anında iletme bildirimi altyapısı aracılığıyla aktarılan bildirim dahil olmak isteyebilirsiniz. Bu öğreticide, istemci cihaz ve uygulama arka ucu arasında güvenli, kimliği doğrulanmış bir bağlantı üzerinden hassas bilgiler göndererek aynı deneyimi elde etmek açıklar.

Yüksek bir düzeyde akışı aşağıdaki gibidir:

1. Uygulama arka ucu:
   * Arka uç veritabanı güvenli yükteki depolar.
   * Bu bildirim kimliği (hiçbir güvenli bilgiler gönderilir) cihaza gönderir.
2. Cihazın, bildirim alındığında uygulama:
   * Cihaz güvenli yükü isteme ve arka uç bağlantı kurar.
   * Uygulamanın cihaz bildirim olarak yük gösterebilirsiniz.

Önceki akış (ve Bu öğreticide), kullanıcı oturum açtığında sonra cihaz kimlik doğrulama belirteci yerel depolama alanında depolar olduğunu varsayıyoruz olduğunu unutmayın. Cihaz bildirimin güvenli yükü bu belirteci kullanarak alabilirsiniz gibi bu sorunsuz bir deneyim sağlar. Uygulamanız kimlik doğrulama belirteçlerinizi cihazda depolamaz veya bu belirteçlerin süresi, bildirim alma sırasında cihaz uygulaması uygulamayı başlatmak için kullanıcıdan genel bir bildirim görüntülenmesi gerekir. Uygulama kullanıcının kimliğini doğrular ve bildirim yükü gösterir.

Bu güvenli gönderme Öğreticisi, güvenli bir şekilde anında iletme bildirimi gönderme işlemi gösterilmektedir. Öğreticiyi yapılar [kullanıcılara bildirme](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) adımları Bu öğreticinin ilk tamamlamanız gereken şekilde öğretici.

> [!NOTE]
> Bu öğreticide oluşturduğunuz ve bildirim hub'ınıza açıklandığı gibi yapılandırılmış varsayılır [Notification hubs'ı (iOS) ile çalışmaya başlama](notification-hubs-ios-apple-push-notification-apns-get-started.md).

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>İOS projesine değiştirme

Uygulama göndermek için arka değiştirdiğiniz göre yalnızca *kimliği* ilişkin bir bildirim, size bu bildirimi tutamacı ve geri görüntülenecek güvenli ileti almak için arka uç çağırmak için iOS uygulamanız değiştirmeniz gerekir.

Bu hedefe ulaşmak için uygulama arka ucunuzdan güvenli içeriği almak için mantığı yazmak sahibiz.

1. İçinde `AppDelegate.m`, gönderilen arka ucundan bildirim kimliği işler için sessiz bildirimleri için uygulama Yazmaçları emin olun. Ekleme `UIRemoteNotificationTypeNewsstandContentAvailability` didFinishLaunchingWithOptions seçeneği:

    ```objc
    [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
    ```
2. İçinde `AppDelegate.m` üst aşağıdaki bildirimi ile bir uygulama bölümüne ekleyin:

    ```objc
    @interface AppDelegate ()
    - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
    @end
    ```
3. Ardından uygulama bölümünde yer tutucusunu değiştirerek aşağıdaki kodu ekleyin `{back-end endpoint}` için daha önce edindiğiniz arka uç nokta ile:

    ```objc
    NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

    - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
    {
        // check if authenticated
        ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
        NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
        if (!authenticationHeader) return;

        NSURLSession* session = [NSURLSession
                                    sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                    delegate:nil
                                    delegateQueue:nil];

        NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"GET"];
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

        NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
            if (!error && httpResponse.statusCode == 200)
            {
                NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                completion([json objectForKey:@"Payload"], nil);
            }
            else
            {
                NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                if (error)
                    completion(nil, error);
                else {
                    completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                }
            }
        }];
        [dataTask resume];
    }
    ```

    Bu yöntem uygulaması paylaşılan tercihlerinde depolanan kimlik bilgilerini kullanarak bildirim içeriği almak için arka çağırır.

4. Şimdi gelen bildirimini işlemek ve görüntülenecek içeriği almak için yukarıdaki yöntemi sunuyoruz. İlk olarak, iOS uygulamanızın arka planda bir anında iletme bildirimi alındığında çalışmasını etkinleştirmek sahibiz. İçinde **XCode**, uygulama projenizi Sol paneldeki seçin ve ardından ana uygulama Hedefinizde tıklayın **hedefleri** Orta bölmesinden bölümü.
5. Ardından, **özellikleri** sekmesinde, merkezi bölmesinin üst kısmında ve denetleme **uzak bildirimler** onay kutusu.

    ![][IOS1]

6. İçinde `AppDelegate.m` anında iletme bildirimleri işlemek için aşağıdaki yöntemi ekleyin:

    ```objc
    -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
    {
        NSLog(@"%@", userInfo);

        [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
            if (!error) {
                // show local notification
                UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                localNotification.alertBody = payload;
                localNotification.timeZone = [NSTimeZone defaultTimeZone];
                [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                completionHandler(UIBackgroundFetchResultNewData);
            } else {
                completionHandler(UIBackgroundFetchResultFailed);
            }
        }];

    }
    ```

    Arka uç tarafından durumlarında kimlik doğrulama üst bilgisi özelliği eksik ya da ret tercih olduğunu unutmayın. Bu gibi durumlarda belirli işlenmesini çoğunlukla hedef üzerinde kullanıcı deneyiminizin bağlıdır. Gerçek bildirim almak için kullanıcının kimliğini doğrulamak bildirim genel bir istemle görüntüle bir seçenektir.

## <a name="run-the-application"></a>Uygulamayı çalıştırın

Uygulamayı çalıştırmak için aşağıdakileri yapın:

1. Xcode'da, uygulamayı fiziksel bir iOS cihazında (anında iletme bildirimleri simülatörde çalışmaz) çalıştırın.
2. İOS uygulama kullanıcı ARABİRİMİNDEKİ'da, bir kullanıcı adı ve parola girin. Bunlar herhangi bir dize olabilir, ancak aynı değere sahip olmalıdır.
3. İOS uygulama kullanıcı Arabirimindeki, tıklayın **oturum**. Ardından **gönderin, anında iletme**. Bildirim Merkezinizde görüntülenmesini güvenli bildirim görmeniz gerekir.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
