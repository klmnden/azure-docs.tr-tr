---
title: Azure Notification hubs'ı kullanarak, belirli kullanıcılara anında iletme bildirimleri gönderme | Microsoft Docs
description: Azure Notification Hubs kullanarak belirli kullanıcılara anında iletme bildirimleri göndermeyi öğrenin.
documentationcenter: ios
author: jwargo
manager: patniko
editor: spelluru
services: notification-hubs
ms.assetid: 1f7d1410-ef93-4c4b-813b-f075eed20082
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 9b6c0715cb85e245aba94adfb8b33d0d07ece9a9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60880493"
---
# <a name="tutorial-push-notifications-to-specific-users-using-azure-notification-hubs"></a>Öğretici: Azure Notification hubs'ı kullanarak belirli kullanıcılara anında iletme bildirimleri

[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

Bu öğreticide, belirli bir cihazdaki belirli bir uygulama kullanıcısına anında iletme bildirimleri göndermek için Azure Notification Hubs’ı nasıl kullanacağınız gösterilmektedir. Bir ASP.NET Webapı arka ucunu istemcilerin kimliğini doğrulamak ve bildirimleri oluşturmak için yönergeler konuda gösterildiği gibi kullanılan [, uygulama arka ucundan kaydetme](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * WebAPI projesi oluşturma
> * WebAPI arka ucunda istemcilerin kimliğini doğrulama
> * WebAPI arka ucunu kullanarak bildirimlere kaydolma
> * WebAPI arka ucundan bildirim gönderme
> * Yeni WebAPI arka ucunu yayımlama
> * İOS uygulamanızı değiştirme
> * Uygulamayı test etme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide oluşturduğunuz ve bildirim hub'ınıza açıklandığı gibi yapılandırılmış varsayılır [Notification hubs'ı (iOS) ile çalışmaya başlama](notification-hubs-ios-apple-push-notification-apns-get-started.md). Bu öğretici ayrıca için önkoşuldur [güvenli (iOS) anında iletme](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) öğretici.
Mobile Apps arka uç hizmetinize kullanmak istiyorsanız, bkz. [Mobile Apps ile çalışmaya başlama anında iletme](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a>İOS uygulamanızı değiştirme

1. Oluşturduğunuz tek sayfalı uygulamayı görüntüle açın [Notification hubs'ı (iOS) ile çalışmaya başlama](notification-hubs-ios-apple-push-notification-apns-get-started.md) öğretici.

   > [!NOTE]
   > Bu bölümde, projenize bir boş kuruluş adı ile yapılandırılmış olduğunu varsayar. Aksi durumda, tüm sınıf adları için kuruluşunuzun adı önüne eklediğinizden gerekir.

2. İçinde `Main.storyboard` nesne kitaplığından ekran görüntüsünde gösterilen bileşenleri ekleyin.

    ![Xcode arabirim oluşturucu görsel taslağı Düzenle][1]

   * **Kullanıcı adı**: Bir yer tutucu metinle UITextField *kullanıcı adı girin*hemen sonuçları etiket ve sol ve sağ kenar boşlukları kısıtlı Gönder altına ve gönderme sonuçları etiket altında.
   * **Parola**: Bir yer tutucu metinle UITextField *parolasını girin*, hemen altındaki kullanıcı adını metin alanı ve sol ve sağ kenar boşlukları ve username metin alanı altında kısıtlanmış. Denetleme **güvenli metin girişi** özniteliği denetçi'deki altında seçeneği *dönüş anahtar*.
   * **Oturum açma**: Bir UIButton hemen parola metin alanı etiketlenmiş ve işaretini kaldırın **etkin** öznitelikleri denetçi'deki altında seçeneği *denetim içeriği*
   * **WNS**: Etiket ve Windows bildirim hizmeti bildirim hub'ında ayarlanmış göndermeyi etkinleştirmek için geçin. Bkz: [Windows başlarken](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) öğretici.
   * **GCM**: Etiket ve Google Cloud Messaging için bildirim hub'ında ayarlanmış ise gönderilmesine izin vermek için geçiş yapabilirsiniz. Bkz: [Android Başlarken](notification-hubs-android-push-notification-google-gcm-get-started.md) öğretici.
   * **APNS**: Etiket ve Apple Platform Bildirim Hizmeti'ne bildirimi gönderilmesine izin vermek için anahtar.
   * **Alıcı Username:A** yer tutucu metinle UITextField *alıcı kullanıcı adı etiketi*hemen GCM etiket ve sol ve sağ kenar boşlukları ve GCM etiketinin altına kısıtlanmış.

     Bazı bileşenler eklenmiştir [Notification hubs'ı (iOS) ile çalışmaya başlama](notification-hubs-ios-apple-push-notification-apns-get-started.md) öğretici.

3. **CTRL** bileşenlerden görünüme sürükleyin `ViewController.h` ve bu yeni çıkışlar ekleyin.

    ```objc
    @property (weak, nonatomic) IBOutlet UITextField *UsernameField;
    @property (weak, nonatomic) IBOutlet UITextField *PasswordField;
    @property (weak, nonatomic) IBOutlet UITextField *RecipientField;
    @property (weak, nonatomic) IBOutlet UITextField *NotificationField;

    // Used to enable the buttons on the UI
    @property (weak, nonatomic) IBOutlet UIButton *LogInButton;
    @property (weak, nonatomic) IBOutlet UIButton *SendNotificationButton;

    // Used to enabled sending notifications across platforms
    @property (weak, nonatomic) IBOutlet UISwitch *WNSSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *GCMSwitch;
    @property (weak, nonatomic) IBOutlet UISwitch *APNSSwitch;

    - (IBAction)LogInAction:(id)sender;
    ```

4. İçinde `ViewController.h`, aşağıdaki `#define` içeri aktarma Deyimlerinizde sonra. Yedek `<Enter Your Backend Endpoint>` yer tutucusunu önceki bölümde, uygulamanızın arka ucuna dağıtmak için kullanılan hedef URL ile. Örneğin, `http://your_backend.azurewebsites.net`.

    ```objc
    #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
    ```

5. Projenizde, adlı yeni bir Cocoa Touch sınıfı oluşturmak `RegisterClient` oluşturduğunuz ASP.NET arka uç ile arabirim oluşturmak için. Öğesinden devralan sınıf oluşturma `NSObject`. Ardından aşağıdaki kodu ekleyin `RegisterClient.h`.

    ```objc
    @interface RegisterClient : NSObject

    @property (strong, nonatomic) NSString* authenticationHeader;

    -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
        andCompletion:(void(^)(NSError*))completion;

    -(instancetype) initWithEndpoint:(NSString*)Endpoint;

    @end
    ```

6. İçinde `RegisterClient.m`, güncelleştirme `@interface` bölümü:

    ```objc
    @interface RegisterClient ()

    @property (strong, nonatomic) NSURLSession* session;
    @property (strong, nonatomic) NSURLSession* endpoint;

    -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                andCompletion:(void(^)(NSError*))completion;
    -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                completion:(void(^)(NSString*, NSError*))completion;
    -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSString*)token
                tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion;

    @end
    ```

7. Değiştirin `@implementation` aşağıdaki kodla RegisterClient.m bölümünde:

    ```objc
    @implementation RegisterClient

    // Globals used by RegisterClient
    NSString *const RegistrationIdLocalStorageKey = @"RegistrationId";

    -(instancetype) initWithEndpoint:(NSString*)Endpoint
    {
        self = [super init];
        if (self) {
            NSURLSessionConfiguration* config = [NSURLSessionConfiguration defaultSessionConfiguration];
            _session = [NSURLSession sessionWithConfiguration:config delegate:nil delegateQueue:nil];
            _endpoint = Endpoint;
        }
        return self;
    }

    -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
                andCompletion:(void(^)(NSError*))completion
    {
        [self tryToRegisterWithDeviceToken:token tags:tags retry:YES andCompletion:completion];
    }

    -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                andCompletion:(void(^)(NSError*))completion
    {
        NSSet* tagsSet = tags?tags:[[NSSet alloc] init];

        NSString *deviceTokenString = [[token description]
            stringByTrimmingCharactersInSet:[NSCharacterSet characterSetWithCharactersInString:@"<>"]];
        deviceTokenString = [[deviceTokenString stringByReplacingOccurrencesOfString:@" " withString:@""]
                                uppercaseString];

        [self retrieveOrRequestRegistrationIdWithDeviceToken: deviceTokenString
            completion:^(NSString* registrationId, NSError *error) {
            NSLog(@"regId: %@", registrationId);
            if (error) {
                completion(error);
                return;
            }

            [self upsertRegistrationWithRegistrationId:registrationId deviceToken:deviceTokenString
                tags:tagsSet andCompletion:^(NSURLResponse * response, NSError *error) {
                if (error) {
                    completion(error);
                    return;
                }

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                if (httpResponse.statusCode == 200) {
                    completion(nil);
                } else if (httpResponse.statusCode == 410 && retry) {
                    [self tryToRegisterWithDeviceToken:token tags:tags retry:NO andCompletion:completion];
                } else {
                    NSLog(@"Registration error with response status: %ld", (long)httpResponse.statusCode);

                    completion([NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                userInfo:nil]);
                }

            }];
        }];
    }

    -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSData*)token
                tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion
    {
        NSDictionary* deviceRegistration = @{@"Platform" : @"apns", @"Handle": token,
                                                @"Tags": [tags allObjects]};
        NSData* jsonData = [NSJSONSerialization dataWithJSONObject:deviceRegistration
                            options:NSJSONWritingPrettyPrinted error:nil];

        NSLog(@"JSON registration: %@", [[NSString alloc] initWithData:jsonData
                                            encoding:NSUTF8StringEncoding]);

        NSString* endpoint = [NSString stringWithFormat:@"%@/api/register/%@", _endpoint,
                                registrationId];
        NSURL* requestURL = [NSURL URLWithString:endpoint];
        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"PUT"];
        [request setHTTPBody:jsonData];
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                self.authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];
        [request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];

        NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
            completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
        {
            if (!error)
            {
                completion(response, error);
            }
            else
            {
                NSLog(@"Error request: %@", error);
                completion(nil, error);
            }
        }];
        [dataTask resume];
    }

    -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                completion:(void(^)(NSString*, NSError*))completion
    {
        NSString* registrationId = [[NSUserDefaults standardUserDefaults]
                                    objectForKey:RegistrationIdLocalStorageKey];

        if (registrationId)
        {
            completion(registrationId, nil);
            return;
        }

        // request new one & save
        NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/api/register?handle=%@",
                                _endpoint, token]];
        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"POST"];
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                self.authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

        NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
            completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
        {
            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
            if (!error && httpResponse.statusCode == 200)
            {
                NSString* registrationId = [[NSString alloc] initWithData:data
                    encoding:NSUTF8StringEncoding];

                // remove quotes
                registrationId = [registrationId substringWithRange:NSMakeRange(1,
                                    [registrationId length]-2)];

                [[NSUserDefaults standardUserDefaults] setObject:registrationId
                    forKey:RegistrationIdLocalStorageKey];
                [[NSUserDefaults standardUserDefaults] synchronize];

                completion(registrationId, nil);
            }
            else
            {
                NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                if (error)
                    completion(nil, error);
                else {
                    completion(nil, [NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                userInfo:nil]);
                }
            }
        }];
        [dataTask resume];
    }

    @end
    ```

    Bu kod Kılavuzu makalede açıklanan mantığını uygulayan [, uygulama arka ucundan kaydetme](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) REST gerçekleştirmek için nsurlsession'ı kullanarak, uygulama arka ucunuza çağırır ve tarafından döndürülen NSUserDefaults RegistrationId yerel olarak depolamak için bildirim hub'ı.

    Bu sınıf özelliği gerektirir `authorizationHeader` düzgün çalışması için ayarlanacak. Bu özellik ayarlanır `ViewController` oturum açma sonrasında sınıfı.

8. İçinde `ViewController.h`, ekleme bir `#import` bildirimi `RegisterClient.h`. Ardından cihaz belirteci için bir bildirim Ekle ve başvuru bir `RegisterClient` örneğini `@interface` bölümü:

    ```objc
    #import "RegisterClient.h"

    @property (strong, nonatomic) NSData* deviceToken;
    @property (strong, nonatomic) RegisterClient* registerClient;
    ```

9. Özel yöntem bildiriminde ViewController.m içinde ekleme `@interface` bölümü:

    ```objc
    @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>

    // create the Authorization header to perform Basic authentication with your app back-end
    -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                    AndPassword:(NSString*)password;

    @end
    ```

    > [!NOTE]
    > Aşağıdaki kod parçacığında, bir güvenli kimlik doğrulama düzeni değildir, uygulanması yerine kullanmalısınız `createAndSetAuthenticationHeaderWithUsername:AndPassword:` örn kayıt istemci sınıfı tarafından kullanılacak bir kimlik doğrulama belirteci oluşturur, özel kimlik doğrulama mekanizması ile OAuth, Active Directory.

10. Ardından `@implementation` bölümünü `ViewController.m`, cihaz belirtecini ve kimlik doğrulama üst bilgisi ayarlamak için bir uygulama ekler aşağıdaki kodu ekleyin.

    ```objc
    -(void) setDeviceToken: (NSData*) deviceToken
    {
        _deviceToken = deviceToken;
        self.LogInButton.enabled = YES;
    }

    -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                    AndPassword:(NSString*)password;
    {
        NSString* headerValue = [NSString stringWithFormat:@"%@:%@", username, password];

        NSData* encodedData = [[headerValue dataUsingEncoding:NSUTF8StringEncoding] base64EncodedDataWithOptions:NSDataBase64EncodingEndLineWithCarriageReturn];

        self.registerClient.authenticationHeader = [[NSString alloc] initWithData:encodedData
                                                    encoding:NSUTF8StringEncoding];
    }

    -(BOOL)textFieldShouldReturn:(UITextField *)textField
    {
        [textField resignFirstResponder];
        return YES;
    }
    ```

    Cihaz belirteci ayarlama Oturum Aç düğmesini nasıl sağladığını dikkat edin. Uygulama arka ucu ile anında iletme bildirimleri için Görünüm Denetleyicisi oturum açma eyleminin bir parçası olarak kaydettirir olmasıdır. Bu nedenle, cihaz belirteci düzgün şekilde ayarlandığını gösterdiğinde kadar erişilebilir olacak şekilde oturum eylem istemezsiniz. Eski ikinci önce olur sürece anında iletme kaydı oturum açma işlemi birbirinden ayırın.

11. Eylem yöntemi için uygulamak için aşağıdaki kod parçacıklarının ViewController.m içinde kullanın, **oturum** düğmesi ve ASP.NET arka uç kullanarak bildirim iletisini göndermek için bir yöntem.

    ```objc
    - (IBAction)LogInAction:(id)sender {
        // create authentication header and set it in register client
        NSString* username = self.UsernameField.text;
        NSString* password = self.PasswordField.text;

        [self createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];

        __weak ViewController* selfie = self;
        [self.registerClient registerWithDeviceToken:self.deviceToken tags:nil
            andCompletion:^(NSError* error) {
            if (!error) {
                dispatch_async(dispatch_get_main_queue(),
                ^{
                    selfie.SendNotificationButton.enabled = YES;
                    [self MessageBox:@"Success" message:@"Registered successfully!"];
                });
            }
        }];
    }

    - (void)SendNotificationASPNETBackend:(NSString*)pns UsernameTag:(NSString*)usernameTag
                Message:(NSString*)message
    {
        NSURLSession* session = [NSURLSession
            sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration] delegate:nil
            delegateQueue:nil];

        // Pass the pns and username tag as parameters with the REST URL to the ASP.NET backend
        NSURL* requestURL = [NSURL URLWithString:[NSString
            stringWithFormat:@"%@/api/notifications?pns=%@&to_tag=%@", BACKEND_ENDPOINT, pns,
            usernameTag]];

        NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
        [request setHTTPMethod:@"POST"];

        // Get the mock authenticationheader from the register client
        NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
            self.registerClient.authenticationHeader];
        [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

        //Add the notification message body
        [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
        [request setHTTPBody:[message dataUsingEncoding:NSUTF8StringEncoding]];

        // Execute the send notification REST API on the ASP.NET Backend
        NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
            completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
        {
            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
            if (error || httpResponse.statusCode != 200)
            {
                NSString* status = [NSString stringWithFormat:@"Error Status for %@: %d\nError: %@\n",
                                    pns, httpResponse.statusCode, error];
                dispatch_async(dispatch_get_main_queue(),
                ^{
                    // Append text because all 3 PNS calls may also have information to view
                    [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];
                });
                NSLog(status);
            }

            if (data != NULL)
            {
                xmlParser = [[NSXMLParser alloc] initWithData:data];
                [xmlParser setDelegate:self];
                [xmlParser parse];
            }
        }];
        [dataTask resume];
    }
    ```

12. Güncelleştirme eylemi için **bildirim gönder** düğmesine ASP.NET arka ucu kullanan ve bir anahtar tarafından etkin bir PNS gönderin.

    ```objc
    - (IBAction)SendNotificationMessage:(id)sender
    {
        //[self SendNotificationRESTAPI];
        [self SendToEnabledPlatforms];
    }

    -(void)SendToEnabledPlatforms
    {
        NSString* json = [NSString stringWithFormat:@"\"%@\"",self.notificationMessage.text];

        [self.sendResults setText:@""];

        if ([self.WNSSwitch isOn])
            [self SendNotificationASPNETBackend:@"wns" UsernameTag:self.RecipientField.text Message:json];

        if ([self.GCMSwitch isOn])
            [self SendNotificationASPNETBackend:@"gcm" UsernameTag:self.RecipientField.text Message:json];

        if ([self.APNSSwitch isOn])
            [self SendNotificationASPNETBackend:@"apns" UsernameTag:self.RecipientField.text Message:json];
    }
    ```

13. İçinde `ViewDidLoad` işlev, oluşturmak üzere aşağıdakileri eklemeniz `RegisterClient` örneği ve, metin alanları için temsilci Ayarla.

    ```objc
    self.UsernameField.delegate = self;
    self.PasswordField.delegate = self;
    self.RecipientField.delegate = self;
    self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
    ```

14. Şimdi `AppDelegate.m`, yöntemin tüm içeriğini kaldırın `application:didRegisterForPushNotificationWithDeviceToken:` aşağıdaki (Görünüm denetleyicisi APNs alınan son cihaz belirteci içerdiğinden emin olmak için) ile değiştirin:

    ```objc
    // Add import to the top of the file
    #import "ViewController.h"

    - (void)application:(UIApplication *)application
                didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
    {
        ViewController* rvc = (ViewController*) self.window.rootViewController;
        rvc.deviceToken = deviceToken;
    }
    ```

15. Son olarak, `AppDelegate.m`, aşağıdaki yöntemi olduğundan emin olun:

    ```objc
    - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
        NSLog(@"%@", userInfo);
        [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
    }
    ```

## <a name="test-the-application"></a>Uygulamayı test etme

1. Xcode'da, uygulamayı fiziksel bir iOS cihazında (anında iletme bildirimleri simülatörde çalışmaz) çalıştırın.
2. İOS uygulama kullanıcı ARABİRİMİNDEKİ'da, kullanıcı adı ve parola için aynı değeri girin. Ardından **oturum**.

    ![iOS uygulamayı test etme][2]

3. Kayıt başarılı size bildiren bir açılır pencere görmeniz gerekir. **Tamam** düğmesine tıklayın.

    ![iOS test görüntülenen bildirimi][3]

4. İçinde **alıcı kullanıcı adı etiketi* metin alanında, başka bir CİHAZDAN kaydı ile kullanılan kullanıcı adı etiketi girin.
5. Bir bildirim iletisi girin ve tıklayın **bildirim gönder**. Bir kayıt alıcı kullanıcı adı etiketi olan cihazlar bildirim iletisini alırsınız. Bu, yalnızca bu kullanıcılara gönderilir.

    ![iOS test etiketli bildirimi][4]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kayıtlarıyla ilişkili etiketleri olan belirli kullanıcılara anında iletme bildirimleri göndermeyi öğrendiniz. Konum tabanlı anında iletme bildirimleri göndermeyi öğrenmek için aşağıdaki öğreticiye ilerleyin: 

> [!div class="nextstepaction"]
>[Konum temelli anında iletme bildirimleri gönderme](notification-hubs-push-bing-spatial-data-geofencing-notification.md)

[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
