---
title: Azure Notification Hubs zengin gönderme
description: Azure'dan bir iOS uygulamasına zengin anında iletme bildirimleri göndermeyi öğrenin. Objective-C ve C# içinde yazılan kod örneklerini.
documentationcenter: ios
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 590304df-c0a4-46c5-8ef5-6a6486bb3340
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: dd808a04dff77388248bf7309f5ff804e6dd065c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60873112"
---
# <a name="azure-notification-hubs-rich-push"></a>Azure Notification Hubs zengin gönderme

## <a name="overview"></a>Genel Bakış

Anlık Zengin içeriği kullanıcılarla etkileşim kurmak için bir uygulama düz metin itmek isteyebilirsiniz. Bu bildirimleri, Kullanıcı etkileşimlerine ve URL'leri, ses, görüntüler/kuponlar ve daha fazlası gibi mevcut içerik tanıtın. Bu öğreticide yapılar [kullanıcılara bildirme](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) konusuna ve yükleri (örneğin, görüntü) bir araya getiren anında iletme bildirimleri gönderme işlemini gösterir.

Bu öğreticide, iOS 7 ve 8 ile uyumludur.

  ![][IOS1]

Yüksek düzeyde:

1. Uygulama arka ucu:
   * Zengin yükünün depolar (Bu durumda, görüntü) arka uç veritabanı/yerel depolama
   * Bu zengin bildirim Kimliğini cihaza gönderir
2. Cihazdaki uygulama:
   * Arka uç aldığı Kimliğine sahip zengin yükünün isteyen kişiler
   * Veri alma işlemi tamamlandığında ve hemen kullanıcılara daha fazla bilgi için dokunduğunuzda yükü gösterir cihazda kullanıcı bildirimleri gönderir.

## <a name="webapi-project"></a>Webapı projesi

1. Visual Studio'da açın **AppBackend** oluşturduğunuz proje [kullanıcılara bildirme](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) öğretici.
2. Kullanıcılarla bildirir ve bunu koymak istediğiniz görüntüyü almak bir **img** proje dizininizde klasör.
3. Tıklayın **tüm dosyaları göster** Çözüm Gezgini'nde, klasöre sağ tıklayın ve **projeye dahil et**.
4. Görüntü seçildi değiştirmek için Özellikler penceresinde derleme eylemi **gömülü kaynak**.

    ![][IOS2]
5. İçinde `Notifications.cs`, aşağıdaki using deyimi:

    ```c#
    using System.Reflection;
    ```
6. Tüm güncelleştirme `Notifications` aşağıdaki kodla sınıfı. Yer tutucuları, bildirim hub'ı kimlik bilgileri ve görüntü dosya adı ile değiştirdiğinizden emin olun.

    ```c#
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
    ```

   > [!NOTE]
   > (isteğe bağlı) Başvurmak [ekleme ve görsel kullanarak kaynaklara nasıl C# ](https://support.microsoft.com/kb/319292) ekleyin ve proje kaynaklarını edinme hakkında daha fazla bilgi için.

7. İçinde `NotificationsController.cs`, bulunabileceğini ' NotificationsController aşağıdaki kod parçacıkları ile. Bu ilk sessiz zengin bildirim kimliği cihaza gönderir ve istemci tarafı görüntüsü alınmasını sağlar:

    ```c#
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
    ```
8. Artık bir Azure Web sitesi için bu uygulamayı tüm cihazlardan erişilebilir olması için yeniden dağıtacağız. **AppBackend** projesine sağ tıklayıp **Yayımla**’yı seçin.
9. Azure Web sitesi, yayımlama hedefi seçin. Azure hesabınızla oturum açın ve mevcut veya yeni bir Web sitesini seçin ve Not **hedef URL** özelliğinde **bağlantı** sekmesi. Bu URL'ye bu öğreticinin sonraki bölümlerinde *arka uca ait uç nokta* olarak başvuracağız. **Yayımla**’ta tıklayın.

## <a name="modify-the-ios-project"></a>İOS projesine değiştirme

Göndermek için bir uygulama arka ucunuzu değiştirdiniz, artık yalnızca *kimliği* ilişkin bir bildirim, bu kimliği işlemek ve arka ucunuzdan zengin ileti almak için iOS uygulamanız değişir.

1. İOS projenizi açın ve uzak bildirimler ana uygulama Hedefinizde ekranına giderek etkinleştirirsiniz **hedefleri** bölümü.
2. Tıklayın **özellikleri**, açma **arka plan modları**ve **uzak bildirimler** onay kutusu.

    ![][IOS3]
3. Açık `Main.storyboard`, ve emin bir görünüm denetleyicisi (giriş görünümü denetleyicisi olarak Bu öğreticide adlandırılır) gelen [bildir kullanıcı](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) öğretici.
4. Ekleme bir **Gezinti denetleyicisi** film şeridi ve denetim Sürükle giriş görünüme sağlamak için denetleyiciye **kök görünümü** gezinti. Emin **olan ilk görünüm denetleyicisi** öznitelikleri yalnızca Gezinti denetleyicisi denetçisi seçilir.
5. Ekleme bir **görünüm denetleyicisi** film şeridi eklemek için bir **resim görünümü**. Bu bildirime tıklayarak daha fazla bilgi seçtikleri sonra kullanıcıların göreceği sayfasıdır. Film şeridini şu şekilde görünmelidir:

    ![][IOS4]
6. Tıklayarak **giriş görünüm denetleyicisi** görsel taslak ve emin olun, sahip **homeViewController** olarak kendi **özel sınıf** ve **film şeridi kimliği**kimlik Inspector'ı altında.
7. Görüntü görünüm denetleyicisi aynı şeyi **imageViewController**.
8. Başlıklı yeni bir görünüm denetleyicisi sınıfını oluşturup **imageViewController** oluşturduğunuz kullanıcı Arabirimi işlemek için.
9. İçinde **imageViewController.h**, denetleyicinin arabirimi bildirimi ekleyin. Film şeridi görüntü görünümünden ikisini bağlamak için bu özellikleri denetimi sürüklemek emin olun:

    ```objc
    @property (weak, nonatomic) IBOutlet UIImageView *myImage;
    @property (strong) UIImage* imagePayload;
    ```
10. İçinde `imageViewController.m`, sonunda aşağıdaki ekleyin `viewDidload`:

    ```objc
    // Display the UI Image in UI Image View
    [self.myImage setImage:self.imagePayload];
    ```
11. İçinde `AppDelegate.m`, oluşturduğunuz görüntü denetleyicisi içeri aktarın:

    ```objc
    #import "imageViewController.h"
    ```
12. Bir arabirimi bölümüne aşağıdaki bildirimi ekleyin:

    ```objc
    @interface AppDelegate ()

    @property UIImage* imagePayload;
    @property NSDictionary* userInfo;
    @property BOOL iOS8;

    // Obtain content from backend with notification id
    - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

    // Redirect to Image View Controller after notification interaction
    - (void)redirectToImageViewWithImage: (UIImage *)img;

    @end
    ```
13. İçinde `AppDelegate`, uygulamanızı emin kaydeder sessiz bildirimleri için `application: didFinishLaunchingWithOptions`:

    ```objc
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
    ```

14. Aşağıdaki uygulamada için yedek `application:didRegisterForRemoteNotificationsWithDeviceToken` kullanıcı Arabirimi değişiklikleri hesaba film şeridi almak için:

    ```objc
    // Access navigation controller which is at the root of window
    UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
    // Get home view controller from stack on navigation controller
    homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
    hvc.deviceToken = deviceToken;
    ```
15. Ardından, aşağıdaki yöntemi ekleyin `AppDelegate.m` uç görüntüyü almak ve alma tamamlandığında yerel bir bildirim gönderin. Yer tutucu değiştirdiğinizden emin olun `{backend endpoint}` arka uç noktanız ile:

    ```objc
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
    ```
16. Görüntü görünüm denetleyicisi açarak yukarıda yerel bildirimi tutamacı `AppDelegate.m` aşağıdaki yöntemlerle:

    ```objc
    // Helper: redirect users to image view controller
    - (void)redirectToImageViewWithImage: (UIImage *)img {
        UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
        UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main" bundle: nil];
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
    ```

## <a name="run-the-application"></a>Uygulamayı çalıştırın

1. Xcode'da, uygulamayı fiziksel bir iOS cihazında (anında iletme bildirimleri simülatörde çalışmaz) çalıştırın.
2. Kimlik doğrulama ve tıklatın için iOS uygulama kullanıcı ARABİRİMİNDEKİ'da, bir kullanıcı adı ve parola aynı değeri girin **oturum**.
3. Tıklayın **gönderin, anında iletme** ve uygulama içi uyarı görmeniz gerekir. Tıklarsanız **daha fazla**, uygulamanızın arka ucunu eklemek için seçtiğiniz görüntüye güncelleştirilecektir.
4. Ayrıca **gönderin, anında iletme** hemen giriş düğmesi cihazınızın tuşuna basın. Birkaç dakika sonra bir anında iletme bildirimi alırsınız. Üzerinde dokunun ya da daha fazla tıklayın, uygulamanızı ve zengin görüntü içeriğini yönlendirilirsiniz.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
