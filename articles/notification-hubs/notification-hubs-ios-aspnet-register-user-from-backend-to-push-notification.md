---
title: Web API kullanarak anında iletme bildirimleri için geçerli kullanıcı kaydı | Microsoft Docs
description: Kayda ASP.NET Web API tarafından gerçekleştirildiğinde bir iOS uygulaması Azure Notification Hubs ile anında iletme bildirimi kaydı isteği öğrenin.
services: notification-hubs
documentationcenter: ios
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: c43c15131afb5fbf346b0137dac566f5331c65a2
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>ASP.NET kullanarak anında iletme bildirimleri için geçerli kullanıcı kaydı
> [!div class="op_single_selector"]
> * [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Bu konu kayıt ASP.NET Web API tarafından gerçekleştirildiğinde Azure Notification Hubs ile anında iletme bildirimi kaydı isteği gösterilmiştir. Bu konuda öğretici genişletir [Notification Hubs kullanıcılara bildirme]. Zaten gerekli adımları kimliği doğrulanmış bir mobil hizmet oluşturmak için Bu öğreticide tamamlamış olmanız gerekir. Bildirim kullanıcılar senaryo hakkında daha fazla bilgi için bkz: [Notification Hubs kullanıcılara bildirme].

## <a name="update-your-app"></a>Uygulamanızı güncelleştirme
1. MainStoryboard_iPhone.storyboard içinde nesne kitaplığından aşağıdaki bileşenleri ekleyin:
   
   * **Etiket**: "Push Notification Hubs ile kullanıcıya"
   * **Etiket**: "InstallationId"
   * **Etiket**: "Kullanıcı"
   * **Metin alanı**: "Kullanıcı"
   * **Etiket**: "Parola"
   * **Metin alanı**: "Parola"
   * **Düğme**: "Oturum Aç"
     
     Bu noktada, şeridinizin aşağıdaki gibi görünür:
     
      ![][0]
2. Yardımcısı düzenleyicisinde çıkışlar anahtarlı tüm denetimler için oluşturma ve bunları çağrısı, metin alanları View Controller (temsilci) bağlanmak ve oluşturma bir **eylem** için **oturum açma** düğmesi.
   
       ![][1]
   
       Your BreakingNewsViewController.h file should now contain the following code:
   
        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;
   
        - (IBAction)login:(id)sender;
3. Adlı bir sınıf oluşturun **Deviceınfo**ve aşağıdaki kodu dosyanın DeviceInfo.h arabirimi bölüme kopyalayın:
   
        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;
4. Aşağıdaki kodu DeviceInfo.m dosya uygulama bölümünde kopyalayın:
   
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
5. PushToUserAppDelegate.h içinde aşağıdaki özellik singleton ekleyin:
   
        @property (strong, nonatomic) DeviceInfo* deviceInfo;
6. İçinde **didFinishLaunchingWithOptions** PushToUserAppDelegate.m, yöntemine aşağıdaki kodu ekleyin:
   
        self.deviceInfo = [[DeviceInfo alloc] init];
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
   
    İlk satırı başlatır **Deviceınfo** singleton. İkinci satır, zaten tamamladıysanız zaten kayıt anında iletme bildirimleri için başlar [Notification Hubs ile çalışmaya başlama] Öğreticisi.
7. PushToUserAppDelegate.m içinde yöntemi uygulaması **didRegisterForRemoteNotificationsWithDeviceToken** AppDelegate içinde ve aşağıdaki kodu ekleyin:
   
        self.deviceInfo.deviceToken = deviceToken;
   
    Bu istek için cihaz belirteci ayarlar.
   
   > [!NOTE]
   > Bu noktada, olmamalıdır başka bir kodu bu yöntem. Çağrı zaten varsa **registerNativeWithDeviceToken** tamamlandığında, eklediğiniz yöntemi [Notification Hubs ile çalışmaya başlama](/manage/services/notification-hubs/get-started-notification-hubs-ios/) eğitmen, yorum genişletme veya o çağrının kaldırmanız gerekir.
   > 
   > 
8. PushToUserAppDelegate.m dosyasında aşağıdaki işleyici yöntemi ekleyin:
   
   * (void) uygulama:(UIApplication *) uygulama didReceiveRemoteNotification:(NSDictionary *) kullanıcı bilgisi {NSLog (@"% @", kullanıcı bilgisi);   UIAlertView * uyarı = [[UIAlertView ayırma] initWithTitle:@"Notification" ileti: [kullanıcı bilgisi objectForKey:@"inAppMessage"] temsilci: nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];   [uyarı göster]; }
   
   Uygulamanızı çalışırken bildirimi aldığında bu yöntem kullanıcı Arabiriminde bir uyarı görüntüler.
9. PushToUserViewController.m dosyasını açın ve aşağıdaki uygulama klavye döndürür:
   
        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }
10. İçinde **viewDidLoad** InstallationID etiketini aşağıdaki gibi Initialize yöntemi PushToUserViewController.m dosyasında:
    
         DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
         Self.installationId.text = deviceInfo.installationId;
11. Aşağıdaki özellikler PushToUserViewController.m arabiriminde ekleyin:
    
        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;
12. Ardından, aşağıdaki uygulama ekleyin:
    
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
13. Aşağıdaki kodu kopyalayın **oturum açma** XCode tarafından oluşturulan işleyici yöntemi:
    
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
    
    Bu yöntem anında iletme bildirimleri için bir yükleme kimliği ve kanal alır ve bunu, Notification Hubs ' bir kayıt oluşturur kimliği doğrulanmış Web API yöntemine cihaz türü ile birlikte gönderir. Bu Web API tanımlanan [Notification Hubs kullanıcılara bildirme].

İstemci uygulaması güncelleştirildi, geri dönüp [Notification Hubs kullanıcılara bildirme] ve bildirim hub'ları kullanarak bildirim göndermek için mobil hizmeti güncelleştirin.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Notification Hubs kullanıcılara bildirme]: /manage/services/notification-hubs/notify-users-aspnet

[Notification Hubs ile çalışmaya başlama]: /manage/services/notification-hubs/get-started-notification-hubs-ios
