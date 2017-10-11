---
title: "Azure bildirim hub'ları zengin gönderme"
description: "Azure'dan bir iOS uygulamasının zengin anında iletme bildirimleri göndermek öğrenin. Objective-C ve C# içinde yazılan kod örnekleri."
documentationcenter: ios
services: notification-hubs
author: ysxu
manager: erikre
editor: 
ms.assetid: 590304df-c0a4-46c5-8ef5-6a6486bb3340
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 394efdc2dfaff0666bc23d8a448b0a00d414da99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-rich-push"></a>Azure bildirim hub'ları zengin gönderme
## <a name="overview"></a>Genel Bakış
Anlık Zengin içeriği kullanıcılarla kullanmak için bir uygulama düz metin göndermek isteyebilirsiniz. Bu bildirimler kullanıcı etkileşimleri ve URL'ler, ses, görüntüler/kuponlar ve daha fazlasını gibi mevcut içeriği yükseltin. Bu öğretici derlemeler [kullanıcılara bildirme](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) konusuna ve yüklerini (örneğin, görüntü) dahil anında iletme bildirimleri göndermeyi gösterir.

Bu öğretici, iOS 7 ve 8 ile uyumludur.

  ![][IOS1]

Yüksek düzeyde:

1. Uygulama arka ucu:
   * Zengin yükü depolar (Bu durumda, görüntü) arka uç veritabanı/yerel depolama
   * Bu zengin bildirim Kimliğini cihaza gönderir
2. Uygulama cihazın:
   * Zengin yükü aldığı kimlikli isteyen arka uç bağlantı kurar
   * Veri alma işlemi tamamlandığında ve hemen kullanıcılara daha fazla bilgi için dokunduğunuzda yükü gösterir cihazda kullanıcılara bildirim gönderme

## <a name="webapi-project"></a>Webapı proje
1. Visual Studio'da açın **AppBackend** oluşturduğunuz proje [kullanıcılara bildirme](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) Öğreticisi.
2. İstediğiniz kullanıcılarla bilgilendirin ve bunu koymak bir görüntü elde bir **img** proje dizininizde klasör.
3. Tıklatın **tüm dosyaları göster** Çözüm Gezgini'nde ve klasöre sağ tıklayarak **projeye dahil et**.
4. Yapı Eylemi Özellikleri penceresinde seçili görüntüsüyle değiştirme **katıştırılmış kaynak**.
   
    ![][IOS2]
5. İçinde **Notifications.cs**, aşağıdaki ekleme deyimini kullanarak:
   
        using System.Reflection;
6. Tüm güncelleştirme **bildirimleri** aşağıdaki kodla sınıfı. Yer tutucuları bildirim hub'ı kimlik bilgilerinizi ve görüntü dosya adı ile değiştirdiğinizden emin olun.
   
        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }
   
        public class Notifications {
            public static Notifications Instance = new Notifications();
   
            private List<Notification> notifications = new List<Notification>();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }
   
            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };
   
                notifications.Add(notification);
   
                return notification;
            }
   
            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }
   
   > [!NOTE]
   > (isteğe bağlı) Başvurmak [katıştırmak ve Visual C# kullanarak kaynaklara erişmek nasıl](http://support.microsoft.com/kb/319292) ekleyin ve proje kaynakları elde hakkında daha fazla bilgi için.
   > 
   > 
7. İçinde **NotificationsController.cs**, yeniden tanımlamanız **NotificationsController** aşağıdaki kod parçacıkları ile. Bu bir başlangıç sessiz zengin bildirim kimliği cihaza gönderir ve görüntü istemci-tarafı alınmasına izin:
   
        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);
   
            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");
   
            return result;
        }
   
        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");
   
            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";
   
            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
8. Şimdi Biz bu uygulamayı bir Azure Web sitesine tüm cihazlar üzerinden erişilebilir olması için yeniden dağıtır. **AppBackend** projesine sağ tıklayıp **Yayımla**’yı seçin.
9. Azure Web sitesi yayımlama hedefi olarak seçin. Azure hesabınızla oturum açın ve mevcut veya yeni bir Web sitesini seçin ve Not **hedef URL** özelliğinde **bağlantı** sekmesi. Bu URL'ye bu öğreticinin sonraki bölümlerinde *arka uca ait uç nokta* olarak başvuracağız. **Yayımla**’ta tıklayın.

## <a name="modify-the-ios-project"></a>İOS projesine değiştirme
Göndermek için uygulamanızın arka ucuna değiştirdiniz. artık, yalnızca *kimliği* ilişkin bir bildirim, bu kimliği işlemek ve arka ucunuzdan zengin ileti almak için iOS uygulamanızın değiştirir.

1. İOS projenizi açın ve ana uygulama hedefiniz giderek uzak bildirimlerini etkinleştirin **hedefleri** bölümü.
2. Tıklayın **yetenekleri**, Aç **arka plan modları**ve denetleme **uzak bildirimler** onay kutusu.
   
    ![][IOS3]
3. Git **Main.storyboard**ve gelen View Controller (refered için giriş View Controller olarak bu öğreticideki) sahip olduğunuzdan emin olun [bildir kullanıcı](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) Öğreticisi.
4. Ekleme bir **Gezinti denetleyicisi** film şeridi ve control-sürükleyin giriş görünüme sağlamak için denetleyiciye **kök görünümü** gezinti. Emin olun **ilk görünüm denetleyicisinin** öznitelikleri denetçisi yalnızca Gezinti denetleyici için seçilir.
5. Ekleme bir **View Controller** film şeridi ve eklemek için bir **görüntü Görünüm**. Bu, üzerinde notifiication tıklayarak daha fazla bilgi tercih ettikleri sonra kullanıcıların göreceği sayfasıdır. Şeridinizin aşağıdaki gibi görünmelidir:
   
    ![][IOS4]
6. Tıklayın **giriş View Controller** film şeridi ve emin olun sahip **homeViewController** olarak kendi **özel sınıf** ve **film şeridi kimliği**kimlik denetçisi altında.
7. Aynı görüntü görünüm denetleyicisi olarak için yaptığınız **imageViewController**.
8. Ardından, başlıklı yeni bir görünüm denetleyici sınıfını oluşturmak **imageViewController** oluşturduğunuz UI işlemek için.
9. İçinde **imageViewController.h**, denetleyicinin arabirimi bildirimleri için aşağıdakileri ekleyin. İki bağlamak için bu özellikleri film şeridi görüntü görünümünden denetimi Sürükle emin olun:
   
        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;
10. İçinde **imageViewController.m**, aşağıdaki sonuna Ekle **viewDidload**:
    
        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];
11. İçinde **AppDelegate.m**, oluşturduğunuz görüntü denetleyicisi içeri aktarın:
    
        #import "imageViewController.h"
12. Aşağıdaki bildirimiyle arabirimi bölümüne ekleyin:
    
        @interface AppDelegate ()
    
        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;
    
        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;
    
        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;
    
        @end
13. İçinde **AppDelegate**, emin olun, uygulamanızın sessiz bildirimleri için kayıtları **uygulama: didFinishLaunchingWithOptions**:
    
        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];
    
        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");
    
            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";
    
            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];
    
            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];

            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

1. Aşağıdaki uygulamasını DEVICEHIGH **uygulama: didRegisterForRemoteNotificationsWithDeviceToken** UI değişiklikleri hesaba film şeridi almak için:
   
       // Access navigation controller which is at the root of window
       UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
       // Get home view controller from stack on navigation controller
       homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
       hvc.deviceToken = deviceToken;
2. Ardından, aşağıdaki yöntemi ekleyin **AppDelegate.m** uç noktasından görüntüyü almak ve alma işlemi tamamlandığında, yerel bir bildirim göndermek için. Yer tutucu değiştirdiğinizden emin olun `{backend endpoint}` arka uç noktası ile:
   
       NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";
   
       // Helper: retrieve notification content from backend with rich notification id
       - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
           UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
           homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
           NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
           // Check if authenticated
           if (!authenticationHeader) return;
   
           NSURLSession* session = [NSURLSession
                                    sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                    delegate:nil
                                    delegateQueue:nil];
   
           NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
           NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
           [request setHTTPMethod:@"GET"];
           NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
           [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];
   
           NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
   
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
               if (!error && httpResponse.statusCode == 200) {
                   // From NSData to UIImage
                   self.imagePayload = [UIImage imageWithData:data];
   
                   completion(nil);
               }
               else {
                   NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                   if (error)
                       completion(error);
                   else {
                       completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                   }
               }
           }];
           [dataTask resume];
       }
   
       // Handle silent push notifications when id is sent from backend
       - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
           self.userInfo = userInfo;
           int richId = [[self.userInfo objectForKey:@"richId"] intValue];
           NSString* richType = [self.userInfo objectForKey:@"richType"];
   
           // Retrieve image data
           if ([richType isEqualToString:@"img"]) {  
               [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                   if (!error){
                       // Send local notification
                       UILocalNotification* localNotification = [[UILocalNotification alloc] init];
   
                       // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                       localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                       localNotification.userInfo = self.userInfo;
                       localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                       localNotification.timeZone = [NSTimeZone defaultTimeZone];
   
                       // iOS8 categories
                       if (self.iOS8) {
                           localNotification.category = @"richPush";
                       }
   
                       [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];
   
                       handler(UIBackgroundFetchResultNewData);
                   }
                   else{
                       handler(UIBackgroundFetchResultFailed);
                   }
               }];
           }
           // Add "else if" here to handle more types of rich content such as url, sound files, etc.
       }
3. Görüntü görünüm denetleyicisini açarak yukarıda yerel bildirimi tutamacı **AppDelegate.m** aşağıdaki yöntemlerle:
   
       // Helper: redirect users to image view controller
       - (void)redirectToImageViewWithImage: (UIImage *)img {
           UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
           UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                    bundle: nil];
           imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
           // Pass data/image to image view controller
           imgViewController.imagePayload = img;
   
           // Redirect
           [navigationController pushViewController:imgViewController animated:YES];
       }
   
       // Handle local notification sent above in didReceiveRemoteNotification
       - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
           if (application.applicationState == UIApplicationStateActive) {
               // Show in-app alert with an extra "more" button
               UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];
   
               [alert show];
           }
           // App becomes active from user's tap on notification
           else {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
       }
   
       // Handle buttons in in-app alerts and redirect with data/image
       - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
           // Handle "more" button
           if (buttonIndex == 1)
           {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
           // Add "else if" here to handle more buttons
       }
   
       // Handle notification setting actions in iOS8
       - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
           // Handle richPush related buttons
           if ([identifier isEqualToString:@"richPushMore"]) {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
           completionHandler();
       }

## <a name="run-the-application"></a>Uygulamayı çalıştırın
1. Xcode'da, uygulamayı fiziksel bir iOS cihazında (anında iletme bildirimleri benzeticisinde çalışmaz) çalıştırın.
2. Kimlik doğrulama ve tıklatın iOS uygulaması UI'da, bir kullanıcı adı ve parola aynı değeri girin **oturum**.
3. Tıklatın **Gönder itme** ve bir uygulama uyarı görmeniz gerekir. Üzerinde tıklatırsanız **daha fazla**, uygulama arka ucunda eklemek için seçtiğiniz görüntüsüne güncelleştirilecektir.
4. Tıklatarak **Gönder itme** ve hemen aygıtınızın giriş düğmesine basın. Birkaç dakika sonra bir anında iletme bildirimi alırsınız. Üzerinde dokunun veya daha fazla tıklatın, uygulamanızı ve zengin görüntü içeriğini duruma getirilir.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
