---
title: "Azure Notification Hubs ile iOS&quot;a anında iletme bildirimleri gönderme | Microsoft Belgeleri"
description: "Bu öğreticide, bir iOS uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs&quot;ın nasıl kullanılacağını öğrenirsiniz."
services: notification-hubs
documentationcenter: ios
keywords: "anında iletme bildirimi,anında iletme bildirimleri,ios anında iletme bildirimleri"
author: ysxu
manager: erikre
editor: 
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 968e24b0441575be7ef17aac8ffaddb8fd16d3c6


---
# <a name="sending-push-notifications-to-ios-with-azure-notification-hubs"></a>Azure Notification Hubs ile iOS'a anında iletme bildirimleri gönderme
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
> [!NOTE]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).
> 
> 

Bu öğretici, bir iOS uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir. [Apple Anında İletilen Bildirim servisini (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html) kullanarak anında iletme bildirimleri alan boş bir iOS uygulaması oluşturacaksınız. 

İşiniz bittiğinde, uygulamanızı çalıştıran tüm cihazlara anında iletme bildirimleri yayımlamak için bildirim hub'ınızı kullanabileceksiniz.

## <a name="before-you-begin"></a>Başlamadan önce
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Bu öğreticinin tamamlanan kodu [GitHub'da](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted) bulunabilir. 

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici için aşağıdakiler gereklidir:

* [Mobile Services iOS SDK'sı sürüm 1.2.4]
* [Xcode]'un en son sürümü
* iOS 8 (veya sonraki bir sürümü) uyumlu bir cihaz
* [Apple Developer Program](https://developer.apple.com/programs/) üyeliği.
  
  > [!NOTE]
  > Anında iletme bildirimlerinin yapılandırma gereksinimleri nedeniyle, anında iletme bildirimlerini iOS Simülatörü'nün yerine fiziksel bir iOS cihazında (iPhone veya iPad) dağıtmanız ve test etmeniz gerekir.
  > 
  > 

Bu öğreticiyi tamamlamak iOS uygulamalarına ilişkin diğer tüm Notification Hubs öğreticileri için önkoşuldur.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>iOS anında iletme bildirimleri için Notification Hub'ınızı yapılandırma
Bu bölüm, yeni bir bildirim hub'ı oluşturma ve oluşturduğunuz **.p12** anında iletme sertifikasını kullanarak APNS ile kimlik doğrulaması yapılandırma konusunda size yol gösterecektir. Önceden oluşturduğunuz bir bildirim hub'ını kullanmak istiyorsanız 5. adıma geçebilirsiniz.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p><b>Ayarlar</b> dikey penceresinde, <b>Bildirim Hizmetleri</b> düğmesine tıklayın, ardından <b>Apple'ı (APNS)</b> seçin. <b>Sertifikayı Karşıya Yükle</b>'ye tıklayıp daha önce dışarı aktardığınız <b>.p12</b> dosyasını seçin. Ayrıca, doğru parolayı belirttiğinizden emin olun.</p>

<p><b>Korumalı Alan</b> modu geliştirme içindir, bu nedenle bu seçeneği belirlediğinizden emin olun. <b>Üretim</b> seçeneğini yalnızca uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek istiyorsanız kullanın.</p>
</li>
</ol>
&emsp;&emsp;![Azure Portal'da APNS'yi yapılandırma](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;![Azure Portal'da APNS sertifikası yapılandırma](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

Bildirim hub'ınız şimdi APNS ile birlikte çalışmak üzere yapılandırıldı. Ayrıca uygulamanızı kaydetmenizi ve anlık iletme bildirimleri göndermenizi sağlayan bağlantı dizelerine sahipsiniz.

## <a name="connect-your-ios-app-to-notification-hubs"></a>iOS uygulamanızı Notification Hubs'a bağlama
1. Xcode'da yeni bir iOS projesi oluşturun ve **Single View Application** (Tek Görünüm Uygulaması) şablonunu seçin.
   
       ![Xcode - Single View Application][8]
2. Yeni projeniz için seçenekleri ayarlarken, daha önce Apple Developer portalında paket kimliğini açarken kullandığınız **Product Name** (Ürün Adı) ve **Organization Identifier**'nı (Kuruluş Tanımlayıcısı) kullandığınızdan emin olun.
   
    ![Xcode - proje seçenekleri][11]
3. **Targets** (Hedefler) altında projenizin adına tıklayın, **Build Settings** (Derleme Ayarları) sekmesine tıklayın ve **Code Signing Identity**'yi (Kod İmzalama Kimliği) genişletin. Ardından **Debug** (Hata Ayıklama) altında kod imzalama kimliğinizi ayarlayın. **Levels**'i (Düzeyler) **Basic**'ten (Temel) **All**'a (Tüm) geçirin ve **Provisioning Profile**'ı (Hazırlama Profili) daha önce oluşturduğunuz hazırlama profiline ayarlayın.
   
    Xcode'da oluşturduğunuz yeni hazırlama profilini göremiyorsanız imzalama kimliğiniz için profilleri yenilemeyi deneyin. Menü çubuğunda **Xcode**'a tıklayın, **Preferences**'a (Tercihler) tıklayın, **Account** (Hesap) sekmesine tıklayın, **View Details** (Ayrıntıları Görüntüle) düğmesine tıklayın, imzalama kimliğinize tıklayın ve ardından sağ alt köşedeki yenile düğmesine tıklayın.
   
       ![Xcode - provisioning profile][9]
4. [Mobile Services iOS SDK'sı sürüm 1.2.4]'ü indirin ve dosyanın sıkıştırmasını açın. Xcode'da projenize sağ tıklayın ve **WindowsAzureMessaging.framework** klasörünü Xcode projenize eklemek için **Add Files to** (Dosyaları Şuraya Ekle) seçeneğine tıklayın. **Copy items if needed** (Gerekirse öğeleri kopyala) seçeneğini belirleyin ve ardından **Add** (Ekle) seçeneğine tıklayın.
   
   > [!NOTE]
   > Bildirim hub'ları SDK'sı şu anda Xcode 7'de bitcode'u desteklemiyor.  **Build Options** (Derleme Seçenekleri) içinde, projeniz için **Enable Bitcode** (Bitcode'u Etkinleştir) seçeneğini **No** (Hayır) olarak ayarlamanız gerekir.
   > 
   > 
   
       ![Unzip Azure SDK][10]
5. Projenize `HubInfo.h` adlı yeni bir üst bilgi dosyası ekleyin. Bu dosya, bildirim hub'ınız için sabitleri tutar.  Aşağıdaki tanımları ekleyin ve dize sabiti yer tutucularını, daha önce not ettiğiniz *hub adınız* ve *DefaultListenSharedAccessSignature* ile değiştirin.
   
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
6. `AppDelegate.h` dosyanızı açıp aşağıdaki içeri aktarma yönergelerini ekleyin:
   
         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
7. `AppDelegate.m file` dosyanızda, iOS sürümünüze bağlı olarak `didFinishLaunchingWithOptions` yöntemine aşağıdaki kodu ekleyin. Bu kod, cihaz tanıtıcınızı APNs'ye kaydeder:
   
    iOS 8 için:
   
         UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
   
    8'den önceki iOS sürümleri için:
   
         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
8. Aynı dosyada, aşağıdaki yöntemleri ekleyin. Bu kod, HubInfo.h içinde belirttiğiniz bağlantı bilgilerini kullanarak bildirim hub'ına bağlanır. Ardından, cihaz belirtecini bildirim hub'ına verir. Böylece bildirim hub'ı bildirim gönderebilir:
   
        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];
   
            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }
   
        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }
9. Aynı dosyaya, uygulama etkinken bildirim alınırsa **UIAlert** görüntülenmesi için aşağıdaki yöntemi ekleyin:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

1. Herhangi bir hata olmadığını doğrulamak için cihazınızda uygulamayı derleyin ve çalıştırın.

## <a name="send-test-push-notifications"></a>Test amaçlı anında iletme bildirimleri gönderme
Hub dikey penceresindeki **Sorun Giderme** bölümü aracılığıyla [Azure Portal]'da anında iletme bildirimleri göndererek uygulamanızda bildirim almayı test edebilirsiniz (*Test Gönderimi* seçeneğini kullanın).

![Azure Portal - Test Gönderimi][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-from-the-app"></a>(İsteğe bağlı) Uygulamadan anında iletme bildirimleri gönderme
> [!IMPORTANT]
> İstemci uygulamasından bildirim göndermeye yönelik bu örnek yalnızca öğrenme amacıyla verilmiştir. Bu işlem istemci uygulamada `DefaultFullSharedAccessSignature` gerektireceğinden, bildirim hub’ınızı bir kullanıcının istemcilerinize yetkisiz bildirimler göndermek üzere erişim kazanabilmesi riskine maruz bırakır.
> 
> 

Bir uygulama içinden anında iletme bildirimleri göndermek isterseniz bu bölümde REST arabirimini kullanarak bunu nasıl yapacağınız konusunda bir örnek sağlanmaktadır.

1. Xcode'da `Main.storyboard` öğesini açın ve kullanıcının uygulama içinde anında iletme bildirimleri göndermesine izin vermek için nesne kitaplığından aşağıdaki kullanıcı arabirimi bileşenlerini ekleyin:
   
   * Etiket metni olmayan bir etiket. Bildirim gönderme hatalarını raporlamak için kullanılır. **Lines** özelliği **0** olarak ayarlanmalıdır. Böylece, otomatik olarak sağ ve sol kenar boşluklarına ve üst görünüme göre kısıtlandırılarak otomatik olarak boyutlandırılır.
   * **Placeholder** (Yer Tutucu) metninin **Enter Notification Message** (Bildirim İletisi Girin) olarak ayarlandığı bir metin alanı. Aşağıda gösterildiği gibi, bu alanı tam etiketin altında kısıtlayın. Görünüm Denetleyicisi'ni çıkış temsilcisi olarak ayarlayın.
   * Tam metin alanı altında ve yatay ortada kısıtlanmış **Send Notification** (Bildirim Gönder) adlı bir düğme.
     
     Görünüm aşağıdaki gibi olmalıdır:
     
     ![Xcode tasarımcısı][32]
2. Görünümünüze bağlı etiket ve metin alanı için [çıkışlar ekleyin](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html); `interface` tanımınızı, `UITextFieldDelegate` ve `NSXMLParserDelegate` yöntemlerini desteklemesi için güncelleştirin. REST API çağırmayı ve yanıt ayrıştırmayı desteklemeye yardımcı olmak için aşağıda gösterilen üç özellik bildirimini ekleyin.
   
    ViewController.h dosyanız aşağıdaki gibi görünmelidir:
   
        #import <UIKit/UIKit.h>
   
        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }
   
        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;
   
        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;
   
        @end
3. `HubInfo.h` öğesini açın ve hub'ınıza bildirimler göndermek için kullanılacak olan aşağıdaki sabitleri ekleyin. Yer tutucu dize sabitini, gerçek *DefaultFullSharedAccessSignature* bağlantı dizeniz ile değiştirin.
   
        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"
4. Aşağıdaki `#import` deyimlerini `ViewController.h` dosyanıza ekleyin.
   
        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"
5. `ViewController.m` içinde, arabirim uygulamasına aşağıdaki kodu ekleyin. Bu kod, *DefaultFullSharedAccessSignature* bağlantı dizenizi ayrıştırır. [REST API başvurusu](http://msdn.microsoft.com/library/azure/dn495627.aspx)'nda belirtildiği gibi, bu ayrıştırılmış bilgiler **Authorization** (Yetkilendirme) istek üst bilgisi için bir SaS belirteci oluşturmak üzere kullanılır.
   
        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;
   
        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;
   
            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];
   
                @throw parseException;
            }
   
            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }
6. `ViewController.m` içinde, görünüm yüklenirken bağlantı dizesini ayrıştırmak için `viewDidLoad` yöntemini güncelleştirin. Ayrıca, aşağıda gösterilen yardımcı program yöntemlerini arabirim uygulamasına ekleyin.  

        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





1. `ViewController.m` içinde, [REST API Başvurusu](http://msdn.microsoft.com/library/azure/dn495627.aspx)'nda belirtildiği gibi **Authorization** (Yetkilendirme) üst bilgisinde sağlanacak olan SaS yetkilendirme belirtecini oluşturmak için, aşağıdaki kodu arabirim uygulamasına ekleyin.
   
        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;
   
            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];
   
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];
   
                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }
   
            return token;
        }
2. **Touch Down** olayı için **SendNotificationMessage** adlı bir eylem eklemek amacıyla, **Send Notification** (Bildirim Gönder) düğmesinden `ViewController.m` öğesine Ctrl tuşunu basılı tutup sürükleyin. REST API kullanarak bildirim göndermek için aşağıdaki kod ile yöntemi güncelleştirin.
   
        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }
   
        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];
   
            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];
   
            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];
   
            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];
   
            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
   
            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];
   
            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];
   
            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
   
            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
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
3. `ViewController.m` içinde, metin alanı için klavye kapatmayı desteklemek üzere aşağıdaki temsilci yöntemini ekleyin. Görünüm denetleyicisini çıkış temsilcisi olarak ayarlamak için, metin alanından arabirim tasarımcısındaki Görünüm Denetleyicisi'ne Ctrl tuşunu basılı tutup sürükleyin.
   
        //===[ Implement UITextFieldDelegate methods ]===
   
        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }
4. `ViewController.m` içinde, `NSXMLParser` kullanarak yanıt ayrıştırmayı desteklemek için aşağıdaki temsilci yöntemlerini ekleyin.
   
       //===[ Implement NSXMLParserDelegate methods ]===
   
       -(void)parserDidStartDocument:(NSXMLParser *)parser
       {
           self.statusResult = @"";
       }
   
       -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
           namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
           attributes:(NSDictionary *)attributeDict
       {
           NSString * element = [elementName lowercaseString];
           NSLog(@"*** New element parsed : %@ ***",element);
   
           if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
           {
               self.currentElement = element;
           }
       }
   
       -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
       {
           self.statusResult = [self.statusResult stringByAppendingString:
               [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
       }
   
       -(void)parserDidEndDocument:(NSXMLParser *)parser
       {
           // Set the status label text on the UI thread
           dispatch_async(dispatch_get_main_queue(),
           ^{
               [self.sendResults setText:self.statusResult];
           });
       }
5. Projeyi derleyin ve hata olmadığını doğrulayın.

> [!NOTE]
> Xcode7'de bitcode desteğine ilişkin bir derleme hatası ile karşılaşırsanız Xcode'da **Build Settings** (Derleme Ayarları) > **Enable Bitcode (ENABLE_BITCODE)** (Bitcode'u Etkinleştir) seçeneğini **NO** (HAYIR) olarak değiştirmeniz gerekir. Notification Hubs SDK'sı şu anda bitcode'u desteklemiyor. 
> 
> 

Apple [Local and Push Notification Programming Guide] (Yerel ve Anında İletilen Bildirim Programlama Kılavuzu) içinde tüm olası bildirim yüklerini bulabilirsiniz.

## <a name="checking-if-your-app-can-receive-push-notifications"></a>Uygulamanızın anında iletme bildirimleri alıp almadığını denetleme
iOS'ta anında iletme bildirimlerini test etmek için, uygulamayı fiziksel bir iOS cihazına dağıtmanız gerekir. iOS Simülatörü'nü kullanarak Apple anında iletme bildirimleri gönderemezsiniz.

1. Uygulamayı çalıştırın ve kaydın başarılı olduğunu doğrulayın. Ardından, **Tamam**'a basın.
   
    ![iOS Uygulaması Anında İletme Bildirimi Kayıt Testi][33]
2. Yukarıda açıklandığı gibi, [Azure Portal]'dan test amaçlı anında iletme bildirimi gönderebilirsiniz. Uygulamada anında iletme bildirimleri göndermek için kod eklediyseniz bildirim iletisi girmek için metin alanı içine dokunun. Ardından, bildirim iletisini göndermek için klavyede **Send** (Gönder) düğmesine veya görünümdeki **Send Notification** (Bildirim Gönder) düğmesine basın.
   
    ![iOS Uygulaması Anında İletilen Bildirim Gönderme Testi][34]
3. Belirli Bildirim Hub'ından bildirimleri almak için kaydedilen tüm cihazlara anında iletme bildirimi gönderilir.
   
    ![iOS Uygulaması Anında İletilen Bildirim Alma Testi][35]

## <a name="next-steps"></a>Sonraki adımlar
Bu basit örnekte, tüm kayıtlı iOS cihazlarınıza anında iletme bildirimleri yayımladınız. Öğrenmenizde bir sonraki adım olarak, etiketleri kullanarak anında iletme bildirimleri göndermek için arka uç oluşturmada size yol gösterecek [Azure Notification Hubs .NET arka ucu ile iOS için Kullanıcılara Bildirme] öğreticisine devam etmenizi öneririz. 

Kullanıcılarınızı ilgi alanı gruplarına göre segmentlere ayırmak istiyorsanız buna ek olarak, [Son dakika haberleri göndermek için Notification Hubs kullanma] öğreticisine gidebilirsiniz. 

Notification Hubs hakkında genel bilgi için bkz. [Notification Hubs Kılavuzu].

<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobile Services iOS SDK'sı sürüm 1.2.4]: http://aka.ms/kymw2g
[Mobile Services iOS SDK'sı]: http://go.microsoft.com/fwLink/?LinkID=266533
[Uygulama sayfası gönderme]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[Uygulamalarım]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Windows için Live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Mobile Services'i kullanmaya başlama]: /develop/mobile/tutorials/get-started-ios
[Klasik Azure Portalı]: https://manage.windowsazure.com/
[Notification Hubs Kılavuzu]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Hazırlama Portalı]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Mobile Services'de anında iletme bildirimlerini kullanmaya başlama]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure Notification Hubs .NET arka ucu ile iOS için Kullanıcılara Bildirme]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Son dakika haberleri göndermek için Notification Hubs kullanma]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Yerel ve Anında İletme Bildirimi Programlama Kılavuzu]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com



<!--HONumber=Nov16_HO2-->


