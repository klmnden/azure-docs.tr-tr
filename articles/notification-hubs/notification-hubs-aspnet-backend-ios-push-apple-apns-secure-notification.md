---
title: "Azure Notification Hubs güvenli bildirme"
description: "Azure'dan bir iOS uygulamasının güvenli anında iletme bildirimleri göndermek öğrenin. Objective-C ve C# içinde yazılan kod örnekleri."
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 17d42b0a-2c80-4e35-a1ed-ed510d19f4b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: e5f09fb3716303bb21fe7442aa6fa8832174838e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-secure-push"></a>Azure Notification Hubs güvenli bildirme
> [!div class="op_single_selector"]
> * [Windows Evrensel](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Microsoft Azure anında iletme bildirimi desteği, mobil platformlar için tüketici ve kurumsal uygulama için anında iletme bildirimleri uyarlamasını büyük ölçüde basitleştirir bir kullanımı kolay, çok platformlu, ölçeği gönderim altyapısı erişmenize olanak tanır.

Yasal nedeniyle veya güvenlik kısıtlamaları, bazen bir uygulama bir şey standart anında iletme bildirimi altyapısı iletilen bildirimi dahil olmak isteyebilirsiniz. Bu öğretici, hassas bilgileri istemci cihaz ve uygulama arka ucu arasında güvenli, kimliği doğrulanmış bir bağlantı aracılığıyla göndererek aynı deneyimi elde etmek açıklar.

Yüksek bir düzeyde akışı aşağıdaki gibidir:

1. Uygulama arka ucu:
   * Arka uç veritabanı güvenli yükünde depolar.
   * Bu bildirim Kimliğini (güvenli hiçbir bilgi gönderilmez) cihaza gönderir.
2. Bildirim alırken cihaza uygulamanın:
   * Cihaz güvenli yükü isteyen arka uç bağlantı kurar.
   * Uygulama yükü cihaz bildirim olarak gösterebilir.

Önceki akış (ve Bu öğreticide), kullanıcı oturum açtığında sonra cihaz kimlik doğrulama belirtecini yerel depoda sakladığı varsayıyoruz olduğunu dikkate almak önemlidir. Cihaz Bu belirteci kullanarak bildirim 's güvenli yükü alabilir gibi tamamen sorunsuz bir deneyim daha güvence altına alır. Uygulamanızın kimlik doğrulama belirteçleri cihazda depolamaz veya bu belirteçleri süresi, bildirim alma sırasında cihaz uygulaması uygulamayı başlatmak için kullanıcıdan genel bir bildirim görüntülemelidir. Uygulama kullanıcının kimliğini doğrular ve bildirim yükü gösterir.

Bu güvenli itme öğretici güvenli bir şekilde bir anında iletme bildirimi göndermek nasıl gösterir. Öğretici derlemeler [kullanıcılara bildirme](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) önce bu öğreticide adımları tamamlanmalıdır şekilde öğretici.

> [!NOTE]
> Bu öğreticide oluşturduğunuz ve bildirim hub'ınızı açıklandığı şekilde yapılandırılmış varsayar [bildirim hub'ları (iOS) ile çalışmaya başlama](notification-hubs-ios-apple-push-notification-apns-get-started.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>İOS projesine değiştirme
Uygulama göndermek için uç değiştiren göre yalnızca *kimliği* ilişkin bir bildirim, iOS uygulamanızı bu bildirim işlemek ve görüntülenecek güvenli ileti almak için arka uç geri arama için değiştirmeniz gerekir.

Bu hedefe ulaşmak için şu uygulama arka ucunu güvenli içeriği almak için mantığı yazmak zorunda.

1. İçinde **AppDelegate.m**, emin olun uygulama kaydeder sessiz bildirimleri için gönderilen arka ucundan bildirim kimliği işler şekilde. Ekleme **UIRemoteNotificationTypeNewsstandContentAvailability** didFinishLaunchingWithOptions seçeneği:
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
2. İçinde **AppDelegate.m** en üstünde şu bildirimi ile bir uygulama bölümüne ekleyin:
   
        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end
3. Sonra uygulama bölümünde yer tutucu değiştirerek aşağıdaki kodu ekleyin `{back-end endpoint}` daha önce edindiğiniz, arka uç için uç noktaya sahip:

```
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

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

1. Şimdi biz gelen bildirim işlemek ve görüntülemek için içeriği almak için yukarıdaki yöntemini kullanın gerekmez. İlk olarak, biz, iOS uygulamanızın arka planda bir anında iletme bildirimi alınırken yürütmek etkinleştirmeniz gerekir. İçinde **XCode**, uygulama projenizi Sol paneldeki seçin ve ardından ana uygulama hedefiniz tıklayın **hedefleri** merkezi bölmesinden bölümü.
2. Ardından, **yetenekleri** sekmesinde, merkezi bölmenin en üstünde ve denetleyin **uzak bildirimler** onay kutusu.
   
    ![][IOS1]
3. İçinde **AppDelegate.m** anında iletme bildirimleri işlemek için aşağıdaki yöntemi ekleyin:
   
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
   
    Arka uç tarafından durumlarında eksik kimlik doğrulama üstbilgisi özelliği ya da reddetme tercih olduğuna dikkat edin. Bu durumlarda belirli işleme çoğunlukla, hedef kullanıcı deneyimi bağlıdır. Gerçek bildirim almak için kullanıcının kimliğini doğrulamak bildirim genel istemiyle görüntüle bir seçenektir.

## <a name="run-the-application"></a>Uygulamayı çalıştırın
Uygulamayı çalıştırmak için aşağıdakileri yapın:

1. Xcode'da, uygulamayı fiziksel bir iOS cihazında (anında iletme bildirimleri benzeticisinde çalışmaz) çalıştırın.
2. İOS uygulaması UI'da, bir kullanıcı adı ve parola girin. Bunlar herhangi bir dize olabilir, ancak aynı değeri olması gerekir.
3. İOS uygulaması UI'da, tıklatın **oturum**. Ardından **Gönder itme**. Bildirim Merkezinize görüntülenmesini güvenli bildirim görmeniz gerekir.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
