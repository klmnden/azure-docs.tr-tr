---
title: "Azure Notification Hubs .NET arka ucu ile iOS için Kullanıcılara Bildirme"
description: "Azure kullanıcılara anında iletme bildirimleri göndermek öğrenin. Objective-C ve .NET API arka uç için yazılan kod örnekleri."
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 1f7d1410-ef93-4c4b-813b-f075eed20082
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 0fa7a886e1ecb0a90b6aebc1dbf9ef0c6ce1acf1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-notify-users-for-ios-with-net-backend"></a>Azure Notification Hubs .NET arka ucu ile iOS için Kullanıcılara Bildirme
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Genel Bakış
Azure'da anında iletme bildirimi desteği, kullanımı kolay, multiplatform ve mobil platformlar için tüketici ve kurumsal uygulama için anında iletme bildirimleri uyarlamasını büyük ölçüde basitleştirir ölçeklendirilmiş gönderim altyapısı erişmenize olanak tanır. Bu öğreticide, belirli bir cihazdaki belirli bir uygulama kullanıcısına anında iletme bildirimleri göndermek için Azure Bildirim Hub'larını nasıl kullanacağınız gösterilir. Bir ASP.NET Webapı arka istemcilerin kimliğini doğrulamak ve bildirimleri oluşturmak için Kılavuzu konusundaki gösterildiği gibi kullanılır [uygulama arka ucunuzdan kaydetme](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).

> [!NOTE]
> Bu öğreticide oluşturduğunuz ve bildirim hub'ınızı açıklandığı şekilde yapılandırılmış varsayar [bildirim hub'ları (iOS) ile çalışmaya başlama](notification-hubs-ios-apple-push-notification-apns-get-started.md). Bu öğreticinin ayrıca için ön koşuldur [güvenli itme (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) Öğreticisi.
> Mobile Apps arka uç hizmetinizin kullanmak istiyorsanız, bkz: [Mobile Apps ile çalışmaya başlama itme](../app-service-mobile/app-service-mobile-ios-get-started-push.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a>İOS uygulamanızı değiştirme
1. Oluşturduğunuz tek sayfa görünümü uygulamasını açın [bildirim hub'ları (iOS) ile çalışmaya başlama](notification-hubs-ios-apple-push-notification-apns-get-started.md) Öğreticisi.
   
   > [!NOTE]
   > Bu bölümde, projenizin bir boş kuruluş adı ile yapılandırıldığını varsayar. Aksi durumda, tüm sınıf adları için kuruluşunuzun adı başına gerekecektir.
   > 
   > 
2. Main.storyboard nesne kitaplığından ekran görüntüsünde gösterilen bileşenleri ekleyin.
   
    ![][1]
   
   * **Kullanıcı adı**: A UITextField yer tutucu metinle *kullanıcı adı girin*hemen sonuçları etiket ve sol ve sağ kenar boşlukları kısıtlı Gönder altındaki ve gönderme sonuçları etiketinin altına.
   * **Parola**: A UITextField yer tutucu metinle *parolasını girin*, kullanıcı adı hemen altındaki metin alan ve sağ ve sol kenar boşluklarına ve kullanıcı adı metin alanı altında kısıtlanmış. Denetleme **güvenli metin girişi** özniteliği Denetçisi'nde altında seçeneği *dönüş anahtar*.
   * **Oturum açma**: A UIButton hemen parola metin alanı etiketli ve işaretini **etkin** öznitelikleri Denetçisi'nde altında seçeneği *denetimi içeriğini*
   * **WNS**: Etiket ve anahtarı Windows bildirim hizmeti bildirim hub'ında Kurulum yüklediyse gönderilmesine izin vermek için. Bkz: [Windows başlarken](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) Öğreticisi.
   * **GCM**: Etiket ve anahtarı Google bulut Mesajlaşma için bildirim hub'ında Kurulum yüklediyse gönderilmesine izin vermek için. Bkz: [Android Başlarken](notification-hubs-android-push-notification-google-gcm-get-started.md) Öğreticisi.
   * **APNS**: Etiket ve anahtar için Apple Platform bildirim hizmet bildirimi gönderilmesine izin vermek için.
   * **Recipent kullanıcıadı**: A UITextField yer tutucu metinle *alıcı kullanıcı adı etiketi*, hemen GCM etiket ve sağ ve sol kenar boşluklarına ve GCM etiketinin altına kısıtlı.

    Bazı bileşenler eklenmiştir [bildirim hub'ları (iOS) ile çalışmaya başlama](notification-hubs-ios-apple-push-notification-apns-get-started.md) Öğreticisi.

1. **CTRL** görünümünde bileşenlerinden ViewController.h için sürükleyin ve bu yeni çıkışlar ekleyin.
   
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
2. ViewController.h içinde aşağıdaki ekleme `#define` içeri aktarma deyimlerini hemen altında. Yedek *< girin bilgisayarınızı arka uç nokta\>*  yer tutucusunu, uygulamanızın arka ucuna önceki bölümdeki dağıtmak için kullanılan hedef URL ile. Örneğin, *http://you_backend.azurewebsites.net*.
   
        #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
3. Projenizde, yeni bir oluşturma **Cocoa Touch sınıfı** adlı **RegisterClient** oluşturduğunuz ASP.NET arka uç ile arabirim oluşturmak için. İçinden devralma sınıfı oluşturmak `NSObject`. Ardından aşağıdaki kodu RegisterClient.h ekleyin.
   
        @interface RegisterClient : NSObject
   
        @property (strong, nonatomic) NSString* authenticationHeader;
   
        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
            andCompletion:(void(^)(NSError*))completion;
   
        -(instancetype) initWithEndpoint:(NSString*)Endpoint;
   
        @end
4. RegisterClient.m Güncelleştirmesi'nde `@interface` bölümü:
   
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
5. Değiştir `@implementation` aşağıdaki kodla RegisterClient.m bölümünde.

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

    Yukarıdaki kod Kılavuzu makalesinde açıklanan mantığını uygular [uygulama arka ucunuzdan kaydetme](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) NSURLSession kullanarak REST gerçekleştirmek için uygulamanızın arka ucuna çağırır ve RegistrationId yerel olarak depolamak için NSUserDefaults bildirim hub'ı döndürdü.

    Bu sınıf, özelliğinin gerektirdiğini Not **authorizationHeader** düzgün çalışması için ayarlanacak. Bu özelliği ayarlamak **ViewController** günlüğünde sonra sınıfı.

1. ViewController.h içinde eklemek bir `#import` RegisterClient.h bildirimi. Daha sonra cihaz belirteci için bir bildirim ekler ve başvuru bir `RegisterClient` örneğini `@interface` bölümü:
   
        #import "RegisterClient.h"
   
        @property (strong, nonatomic) NSData* deviceToken;
        @property (strong, nonatomic) RegisterClient* registerClient;
2. Bir özel yöntem bildiriminde ViewController.m içinde eklemek `@interface` bölümü:
   
        @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>
   
        // create the Authorization header to perform Basic authentication with your app back-end
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
   
        @end

> [!NOTE]
> Aşağıdaki kod parçacığında güvenli kimlik doğrulama düzeni değil, uygulaması yerine kullanmalısınız **createAndSetAuthenticationHeaderWithUsername:AndPassword:** kaydı istemci sınıfı tarafından örneğin OAuth, Active Directory kullanılması için bir kimlik doğrulama belirteci oluşturur, özel kimlik doğrulama mekanizması ile.
> 
> 

1. Ardından `@implementation` ViewController.m bölümünü cihaz belirteci ve kimlik doğrulama üstbilgisi ayarlamak için uygulama ekleyen aşağıdaki kodu ekleyin.
   
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
   
    Cihaz belirteci ayarı Oturum Aç düğmesini nasıl sağladığını unutmayın. Uygulama arka ucu ile anında iletme bildirimleri için Görünüm denetleyicisini oturum açma eylemin bir parçası olarak kaydettirir olmasıdır. Bu nedenle, biz cihaz belirteci düzgün şekilde ayarlanmış kadar erişilebilir olmasını oturum eylem istemezsiniz. Eski ikinci önce gerçekleşir sürece iletme kaydı içinde günlüğünden ayırırsınız.
2. Eylem yöntemi için uygulamak için aşağıdaki kod parçacıkları ViewController.m içinde kullanmak, **oturum** düğmesi ve ASP.NET arka ucu kullanarak bildirim iletisini göndermek için bir yöntem.
   
       - (IBAction) LogInAction: (ID) gönderen {/ / kimlik doğrulama üst bilgisi oluşturun ve kayıt istemci NSString * kullanıcı ayarlayın self =. UsernameField.text;   NSString * parola self =. PasswordField.text;
   
           [kendini createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];
   
           __weak ViewController * selfie kendini; =   [self.registerClient registerWithDeviceToken:self.deviceToken etiketler: nil andCompletion:^(NSError* error) {varsa (! hatası) {dispatch_async(dispatch_get_main_queue(), ^ {selfie. SendNotificationButton.enabled = YES;               [self MessageBox:@"Success" message:@"Registered başarıyla!"];});}}];}

        -(void) SendNotificationASPNETBackend: (NSString*) pns UsernameTag: (NSString*) usernameTag ileti: (NSString*) ileti {NSURLSession* oturum = [NSURLSession sessionWithConfiguration: [NSURLSessionConfiguration defaultSessionConfiguration] temsilci: nil delegateQueue:nil];

            Pns ve kullanıcı adı etiketi için ASP.NET arka uç NSURL * requestURL REST URL ile parametre olarak geçirmek = [NSURL URLWithString: [NSString stringWithFormat:@"%@/api/notifications? pns = % @& to_tag = % @", BACKEND_ENDPOINT, pns, usernameTag]];

            NSMutableURLRequest * isteği [NSMutableURLRequest requestWithURL:requestURL] =;    [setHTTPMethod:@"POST istek"];

            Sahte authenticationheader kayıt istemciden NSString * authorizationHeaderValue alma = [NSString stringWithFormat:@"Basic % @", self.registerClient.authenticationHeader];    [setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization istek"];

            Bildirim ileti gövdesi ekleme [setValue:@"application/json;charset=utf-8 istek" forHTTPHeaderField:@"Content-Type"];    [setHTTPBody istek: [ileti dataUsingEncoding:NSUTF8StringEncoding]];

            REST API gönderme bildirim ASP.NET arka uç NSURLSessionDataTask * dataTask üzerinde yürütme = [oturum dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) yanıt;        varsa (hata || httpResponse.statusCode! = 200) {NSString* Durum = [% NSString stringWithFormat:@"Error durum @: % d\nError: %@\n", pns, httpResponse.statusCode, hata];            dispatch_async(dispatch_get_main_queue(), ^ {/ / tüm 3 PNS çağrıları bilgileri [self.sendResults setText:[self.sendResults.text stringByAppendingString:status] görünümüne sahip olabileceği için metin Ekle];            });            NSLog(status);        }

                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];    [dataTask Sürdür]; }


1. Eylem için güncelleştirme **bildirim gönder** düğmesine ASP.NET arka kullanın ve bir anahtar tarafından etkin PNS gönderin.

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



1. İşlevde **ViewDidLoad**, RegisterClient örneği oluşturmak için aşağıdaki örneği ve metin alanları için temsilci Ayarla ekleyin.
   
       self.UsernameField.delegate = self;
       self.PasswordField.delegate = self;
       self.RecipientField.delegate = self;
       self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
2. Artık **AppDelegate.m**, yönteminin tüm içeriğini kaldırın **uygulama: didRegisterForPushNotificationWithDeviceToken:** ve görünüm denetleyicisini APNs alınan son cihaz belirteci içerdiğinden emin olmak için aşağıdakilerle değiştirin:
   
       // Add import to the top of the file
       #import "ViewController.h"
   
       - (void)application:(UIApplication *)application
                   didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
       {
           ViewController* rvc = (ViewController*) self.window.rootViewController;
           rvc.deviceToken = deviceToken;
       }
3. Son olarak, **AppDelegate.m**, aşağıdaki yöntemi olduğundan emin olun:
   
       - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
           NSLog(@"%@", userInfo);
           [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
       }

## <a name="test-the-application"></a>Uygulamayı test etme
1. Xcode'da, uygulamayı fiziksel bir iOS cihazında (anında iletme bildirimleri benzeticisinde çalışmaz) çalıştırın.
2. İOS uygulaması UI'da, bir kullanıcı adı ve parola girin. Bunlar herhangi bir dize olabilir, ancak her ikisi de aynı dize değeri olması gerekir. Ardından **oturum**.
   
    ![][2]
3. Kayıt başarılı size bildiren bir açılır pencere görmeniz gerekir. **Tamam** düğmesine tıklayın.
   
    ![][3]
4. İçinde **alıcı kullanıcı adı etiketi* metin alanında, başka bir cihaz kaydından ile kullanılan kullanıcı adı etiketi girin.
5. Bir bildirim iletisi girin ve tıklayın **bildirim gönder**.  Alıcı kullanıcı adı etiketi ile kayıt olan cihazları bildirim iletisini alırsınız.  Ayrıca, bu kullanıcılara yalnızca gönderilir.
   
    ![][4]

[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
