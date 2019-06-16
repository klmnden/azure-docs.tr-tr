---
title: Web API'sini kullanarak anında iletme bildirimleri için geçerli kullanıcıyı kaydetme | Microsoft Docs
description: ASP.NET Web API'si tarafından kaydı yapıldığında Azure Notification Hubs ile bir iOS uygulaması anında iletme bildirimi kaydı isteği öğrenin.
services: notification-hubs
documentationcenter: ios
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: ff77a955c34941d87a1f653726ab3f19e84aa440
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61458357"
---
# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>ASP.NET kullanarak anında iletme bildirimleri için geçerli kullanıcıyı kaydetme

> [!div class="op_single_selector"]
> * [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)

## <a name="overview"></a>Genel Bakış

Bu konu kayıt ASP.NET Web API'si tarafından gerçekleştirildiğinde, Azure Notification Hubs ile anında iletme bildirimi kaydı isteği gösterilmektedir. Bu konu, öğretici genişletir [Bildirim hub'larıyla kullanıcılara bildirim gönderme]. Zaten gerekli adımları kimliği doğrulanmış bir mobil hizmet oluşturmak için Bu öğreticide tamamlamış olmanız gerekir. Bildirim kullanıcılar senaryo hakkında daha fazla bilgi için bkz. [Bildirim hub'larıyla kullanıcılara bildirim gönderme].

## <a name="update-your-app"></a>Uygulamanızı güncelleştirin

1. MainStoryboard_iPhone.storyboard içinde nesne kitaplığından aşağıdaki bileşenleri ekleyin:

   * **Etiket**: "Kullanıcı Notification Hubs ile anında iletme"
   * **Etiket**: "InstallationId"
   * **Etiket**: "Kullanıcı"
   * **Metin alanı**: "Kullanıcı"
   * **Etiket**: "Password"
   * **Metin alanı**: "Password"
   * **Düğme**: "Login"

     Bu noktada, film şeridini aşağıdaki gibi görünür:

     ![][0]

2. Yardımcısı düzenleyicisinde, çıkışlar anahtarlı tüm denetimler için oluşturma ve bunları çağırın, metin alanları ile görünüm denetleyicisi (temsilci) bağlanmak ve oluşturma bir **eylem** için **oturum açma** düğmesi.

    ![][1]

    Artık BreakingNewsViewController.h dosyanıza aşağıdaki kodu içermesi gerekir:

    ```objc
    @property (weak, nonatomic) IBOutlet UILabel *installationId;
    @property (weak, nonatomic) IBOutlet UITextField *User;
    @property (weak, nonatomic) IBOutlet UITextField *Password;

    - (IBAction)login:(id)sender;
    ```
3. Adlı bir sınıf oluşturun `DeviceInfo`ve ' % s'dosyasının DeviceInfo.h arabirimi bölüme aşağıdaki kodu kopyalayın:

    ```objc
    @property (readonly, nonatomic) NSString* installationId;
    @property (nonatomic) NSData* deviceToken;
    ```
4. DeviceInfo.m dosyasının uygulama bölümünde aşağıdaki kodu kopyalayın:

    ```objc
    @synthesize installationId = _installationId;

    - (id)init {
        if (!(self = [super init]))
            return nil;

        NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
        _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
        if(!_installationId) {
            CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
            _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
            CFRelease(newUUID);

            //store the install ID so we don't generate a new one next time
            [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
            [defaults synchronize];
        }

        return self;
    }

    - (NSString*)getDeviceTokenInHex {
        const unsigned *tokenBytes = [[self deviceToken] bytes];
        NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
        return hexToken;
    }
    ```

5. Aşağıdaki özellik tekli PushToUserAppDelegate.h içinde ekleyin:

    ```objc
    @property (strong, nonatomic) DeviceInfo* deviceInfo;
    ```
6. İçinde `didFinishLaunchingWithOptions` PushToUserAppDelegate.m, yöntemine aşağıdaki kodu ekleyin:

    ```objc
    self.deviceInfo = [[DeviceInfo alloc] init];

    [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
    ```

    İlk satırı başlatır `DeviceInfo` tekil. İkinci satır zaten tamamladıysanız, var olan anında iletme bildirimleri kaydı başlar [Notification Hubs ile çalışmaya başlama] öğretici.
7. PushToUserAppDelegate.m içinde yöntemi uygulamak `didRegisterForRemoteNotificationsWithDeviceToken` AppDelegate içinde ve aşağıdaki kodu ekleyin:

    ```objc
    self.deviceInfo.deviceToken = deviceToken;
    ```

    Bu istek için cihaz belirteci ayarlar.

   > [!NOTE]
   > Bu noktada, olmamalıdır herhangi bir kodu bu yöntemi. Bir çağrı zaten varsa `registerNativeWithDeviceToken` tamamlandığında, eklediğiniz yöntemi [Notification Hubs ile çalışmaya başlama](notification-hubs-ios-apple-push-notification-apns-get-started.md) öğretici açıklama veya bu çağrı kaldırmanız gerekir.

8. İçinde `PushToUserAppDelegate.m` işleyicisi aşağıdaki yöntemi ekleyin:

    ```objc
    * (void) application:(UIApplication *) application didReceiveRemoteNotification:(NSDictionary *)userInfo {
       NSLog(@"%@", userInfo);
       UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                             [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                             @"OK" otherButtonTitles:nil, nil];
       [alert show];
    }
    ```

    Uygulamanız çalışırken bildirimleri aldığında bu yöntem kullanıcı Arabiriminde bir uyarı görüntüler.

9. Açık `PushToUserViewController.m` dosya ve içinde aşağıdaki uygulama klavye döndürür:

    ```objc
    - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
        if (theTextField == self.User || theTextField == self.Password) {
            [theTextField resignFirstResponder];
        }
        return YES;
    }
    ```

10. İçinde `viewDidLoad` yönteminde `PushToUserViewController.m` dosya, başlatma `installationId` aşağıdaki gibi:

    ```objc
    DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
    Self.installationId.text = deviceInfo.installationId;
    ```

11. Arabirimi aşağıdaki özellikleri ekleyin `PushToUserViewController.m`:

    ```objc
    @property (readonly) NSOperationQueue* downloadQueue;
    - (NSString*)base64forData:(NSData*)theData;
    ```

12. Ardından, aşağıdaki uygulama ekleyin:

    ```objc
    - (NSOperationQueue *)downloadQueue {
        if (!_downloadQueue) {
            _downloadQueue = [[NSOperationQueue alloc] init];
            _downloadQueue.name = @"Download Queue";
            _downloadQueue.maxConcurrentOperationCount = 1;
        }
        return _downloadQueue;
    }

    // base64 encoding
    - (NSString*)base64forData:(NSData*)theData
    {
        const uint8_t* input = (const uint8_t*)[theData bytes];
        NSInteger length = [theData length];

        static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

        NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
        uint8_t* output = (uint8_t*)data.mutableBytes;

        NSInteger i;
        for (i=0; i < length; i += 3) {
            NSInteger value = 0;
            NSInteger j;
            for (j = i; j < (i + 3); j++) {
                value <<= 8;

                if (j < length) {
                    value |= (0xFF & input[j]);
                }
            }

            NSInteger theIndex = (i / 3) * 4;
            output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
            output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
            output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
            output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
        }

        return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
    }
    ```

13. Aşağıdaki kodu kopyalayın `login` XCode tarafından oluşturulan işleyicisi yöntemi:

    ```objc
    DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

    // build JSON
    NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

    // build auth string
    NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

    NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
    [request setHTTPMethod:@"POST"];
    [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
    [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
    [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
    [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

    // connect with POST
    [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
        // add UIAlert depending on response.
        if (error != nil) {
            NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
            if ([httpResponse statusCode] == 200) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"status: %ld", (long)[httpResponse statusCode]);
            }
        } else {
            NSLog(@"error: %@", error);
        }
    }];
    ```

    Bu yöntem, bir yükleme kimliği ve kanal için anında iletme bildirimleri alır ve, cihaz türü ile birlikte bir kayıt Notification hubs'ı oluşturan kimliği doğrulanmış Web API yöntemine gönderir. Bu Web API tanımlandığı [Bildirim hub'larıyla kullanıcılara bildirim gönderme].

İstemci uygulaması güncelleştirildi, dönmek [Bildirim hub'larıyla kullanıcılara bildirim gönderme] ve bildirim hub'ları kullanarak bildirim göndermek için mobil hizmeti güncelleştirin.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Bildirim hub'larıyla kullanıcılara bildirim gönderme]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Notification Hubs ile çalışmaya başlama]: notification-hubs-ios-apple-push-notification-apns-get-started.md
