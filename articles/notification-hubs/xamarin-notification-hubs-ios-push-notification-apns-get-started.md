---
title: Xamarin uygulamaları için Notification Hubs ile iOS Anında İletme Bildirimleri | Microsoft Docs
description: Bu öğreticide, bir Xamarin iOS uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını öğrenirsiniz.
services: notification-hubs
keywords: ios anında iletme bildirimleri,anında iletme iletileri,anında iletme bildirimleri,anında iletme iletisi
documentationcenter: xamarin
author: wesmc7777
manager: erikre
editor: ''

ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: wesmc

---
# Xamarin uygulamaları için Notification Hubs ile iOS Anında İletme Bildirimleri
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## Genel Bakış
> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).
> 
> 

Bu öğretici, bir iOS uygulamasına anında iletme bildirimleri göndermek için Azure Notification Hubs'ın nasıl kullanılacağını size gösterir.
[Apple Anında İletilen Bildirim Servisi'ni (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html) kullanarak anında iletme bildirimleri alan boş bir Xamarin.iOS uygulaması oluşturacaksınız. İşiniz bittiğinde, uygulamanızı çalıştıran tüm cihazlara anında iletme bildirimleri yayımlamak için bildirim hub'ınızı kullanabileceksiniz. Tamamlanan kodu [NotificationHubs uygulaması][GitHub] örneğinde bulabilirsiniz.

Bu öğretici Notification Hubs ile basit anında iletme iletisi yayımlama senaryosunu gösterir.

## Önkoşullar
Bu öğretici için aşağıdakiler gereklidir:

* [Xcode 6.0][Install Xcode]
* iOS 7.0 (veya sonraki bir sürümü) uyumlu bir cihaz
* iOS Developer Program üyeliği
* [Xamarin Studio]
  
  > [!NOTE]
  > iOS anında iletme bildirimlerinin yapılandırma gereksinimleri nedeniyle, örnek uygulamayı benzetici yerine fiziksel bir iOS cihazında (iPhone veya iPad) dağıtmanız ve test etmeniz gerekir.
  > 
  > 

Bu öğreticiyi tamamlamak Xamarin iOS uygulamalarına ilişkin diğer tüm Notification Hubs öğreticileri için önkoşuldur.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## Bildirim hub'ınızı yapılandırma
Bu bölüm, yeni bir bildirim hub'ı oluşturma ve oluşturduğunuz **.p12** anında iletme sertifikasını kullanarak APNS ile kimlik doğrulaması yapılandırma konusunda size yol gösterecektir. Önceden oluşturduğunuz bir bildirim hub'ını kullanmak istiyorsanız 5. adıma geçebilirsiniz.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li>

<p>APNS bağlantısını yapılandırmak istediğimizden, Azure Portal'da Notification Hub ayarlarınızı açın ve <b>Bildirim Hizmetleri</b>'ne tıklayın. Ardından listede <b>Apple (APNS)</b> öğesine tıklayın. Bunu yaptıktan sonra, <b>Sertifikayı Karşıya Yükle</b>'ye tıklayın. Ardından, daha önce dışarı aktardığınız <b>.p12</b> sertifikasını ve sertifika parolasını seçin.</p>

<p>Bir geliştirme ortamında anında iletme iletileri göndereceğinizden, <b>Korumalı Alan</b> modunu seçtiğinizden emin olun. <b>Üretim</b> ayarını yalnızca uygulamanızı zaten mağazadan satın almış kullanıcılara anında iletme bildirimleri göndermek istiyorsanız kullanın.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)

Bildirim hub'ınız şimdi APNS ile birlikte çalışmak üzere yapılandırıldı. Ayrıca uygulamanızı kaydetmenizi ve anlık iletme bildirimleri göndermenizi sağlayan bağlantı dizelerine sahipsiniz.

## Uygulamanızı bildirim hub'ına bağlama
#### Yeni bir proje oluşturma
1. Xamarin Studio'da yeni bir iOS projesi oluşturun ve **Unified API** (Birleşik API)  > **Single View Application** (Tek Görünüm Uygulaması) şablonunu seçin.
   
    ![Xamarin Studio - Uygulama Türünü Seçme][31]
2. Azure Mesajlaşma bileşenine bir başvuru ekleyin. Çözüm görünümünde projenizin **Components** (Bileşenler) klasörüne sağ tıklayın ve **Get More Components** (Daha Fazla Bileşen Al) seçeneğini belirleyin. **Azure Mesajlaşma** bileşeni için arama yapın ve bileşeni projenize ekleyin.
3. **AppDelegate.cs**'ye aşağıdaki using deyimini ekleyin:
   
        using WindowsAzure.Messaging;
4. Bir **SBNotificationHub** örneği bildirin:
   
        private SBNotificationHub Hub { get; set; }
5. Aşağıdaki değişkenlerle bir **Constants.cs** sınıfı oluşturun:
   
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
6. **AppDelegate.cs**'de, **FinishedLaunching()**'i aşağıdaki ile eşleşecek şekilde güncelleştirin:
   
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());
   
                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }
   
            return true;
        }
7. **AppleDelegate.cs**'de **RegisteredForRemoteNotifications()** yöntemini geçersiz kılın:
   
        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);
   
            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }
   
                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }
8. **AppleDelegate.cs**'de **ReceivedRemoteNotification()** yöntemini geçersiz kılın:
   
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
9. **AppleDelegate.cs**'de aşağıdaki **ProcessNotification()** yöntemini oluşturun:
   
        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
   
                string alert = string.Empty;
   
                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();
   
                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }
   
   > [!NOTE]
   > Ağ bağlantısının olmaması gibi durumlarla başa çıkmak için **FailedToRegisterForRemoteNotifications()**'ı geçersiz kılmayı seçebilirsiniz. Bu seçim, kullanıcı uygulamanızı çevrimdışı modda (örneğin, Uçak) başlattığında ve uygulamanıza özgü anında iletme mesajlaşması senaryoları kullanmak istediğinizde özellikle önemlidir.
   > 
   > 
10. Cihazınızda uygulamayı çalıştırın.

## Anında İletme Bildirimleri Gönderme
Aşağıdaki ekranda gösterildiği gibi, bildirim hub'ı sayfasında yer alan **Sorun Giderme** araç takımındaki **Test Gönderimi** özelliği ile [Azure Portal]'da bildirim göndererek, uygulamanızda anında iletme bildirimleri almayı test edebilirsiniz.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Anında iletme bildirimleri normalde, uyumlu bir kitaplık kullanılarak Mobile Services veya ASP.NET gibi bir arka uç hizmeti aracılığıyla gönderilir. Senaryonuzda kullanılabilir bir kitaplık yoksa anında iletme iletileri göndermek için doğrudan REST API de kullanabilirsiniz. 

Bu öğreticide konuyu basit bir şekilde işleyeceğiz ve yalnızca bir arka uç hizmeti yerine bir konsol uygulamasındaki bildirim hub'ları için .NET SDK ile bildirim göndererek istemci uygulamanızı test etmeyi göstereceğiz. Bir ASP.NET arka ucundan bildirim göndermek için sonraki adım olarak [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) öğreticisini öneririz. Bununla birlikte, bildirim göndermek için aşağıdaki yaklaşımlar kullanılabilir:

* **REST Arabirimi**: [REST arabirimini](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx) kullanarak herhangi bir arka uç platformunda anında iletme bildirimini destekleyebilirsiniz.
* **Microsoft Azure Notification Hubs .NET SDK'sı**: Visual Studio için Nuget Paket Yöneticisi'nde [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) komutunu çalıştırın.
* **Node.js**: [Node.js'den Notification Hubs'ı kullanma](notification-hubs-nodejs-push-notification-tutorial.md).
* **Azure Mobile Services**: Notification Hubs ile tümleştirilmiş Azure Mobile Services arka ucundan nasıl anında iletme bildirimleri gönderildiğinin bir örneği için bkz. "Mobile Services'de anında iletme bildirimlerini kullanmaya başlama" ([.NET arka ucu](../mobile-services/mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md) | [JavaScript arka ucu](../mobile-services/mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md)).
* **Java/PHP**: REST API'ler kullanarak nasıl anında iletme bildirimleri gönderildiğinin bir örneği için bkz. "Java/PHP'den Notification Hubs'ı kullanma" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

#### (İsteğe bağlı) .NET Konsol Uygulamasından Anında İletme Bildirimleri Gönderme
Bu bölümde, basit bir .NET konsol uygulaması kullanarak anında iletme bildirimleri göndereceğiz. Bu örneğin amaçları doğrultusunda, Visual Studio'nun zaten yüklü olduğu bir Windows geliştirme ortamına geçiş yapacağız.

1. Visual Studio'da yeni bir Visual C# konsol uygulaması oluşturun:
   
    ![Visual Studio - Yeni bir konsol uygulaması oluşturma][213]
2. Visual Studio'da **Araçlar**'a, **NuGet Paket Yöneticisi**'ne ve ardından **Paket Yöneticisi Konsolu**'na tıklayın.
   
    Paket yöneticisi konsolu, Visual Studio çalışma alanınızın altına yerleştirilmiş olarak görünmelidir.
3. Paket Yöneticisi Konsolu penceresinde, **Varsayılan projeyi** yeni konsol uygulaması projeniz olarak ayarlayın ve ardından konsol penceresinde aşağıdaki komutu yürütün:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Bu, <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet paketini</a> kullanarak Azure Notification Hubs SDK'sına bir başvuru ekler.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. `Program.cs` dosyasını açın ve ana sınıfınız içindeki Azure sınıflarını ve işlevlerini kullanabilmemizi sağlayan aşağıdaki `using` deyimini ekleyin:
   
        using Microsoft.Azure.NotificationHubs;
5. `Program` sınıfınıza aşağıdaki yöntemi ekleyin (**bağlantı dizesini** ve **hub adını** değiştirmeyi unutmayın):
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }
6. Aşağıdaki satırları `Main` yönteminize ekleyin:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Uygulamayı çalıştırmak için F5 tuşuna basın. Saniyeler içinde, cihazınızda bir anında iletme bildirimi görüntülendiğini görmeniz gerekir. Wi-Fi veya hücresel veri ağı kullanmanızdan bağımsız olarak, cihazda etkin bir İnternet bağlantınızın olduğundan emin olun.

Apple [Local and Push Notification Programming Guide](Yerel ve Anında İletilen Bildirim Programlama Kılavuzu.md) içinde tüm olası yükleri bulabilirsiniz.

#### (İsteğe bağlı) Mobil Hizmetten Bildirim Gönderme
Bu bölümde, bir node betiği aracılığıyla mobil hizmet kullanarak anında iletme bildirimleri göndereceğiz.

Mobil hizmet kullanarak bildirim göndermek için [Mobile Services'i kullanmaya başlama]'yı izleyin ve ardından:

1. [Klasik Azure Portalı]'nda oturum açın ve mobil hizmetinizi seçin.
2. Üst kısımdaki **Scheduler** sekmesini seçin.
   
    ![Klasik Azure Portalı - Scheduler][215]
3. Yeni bir zamanlanan iş oluşturun, ad ekleyin ve **İsteğe bağlı**'yı seçin.
   
    ![Klasik Azure Portalı - Yeni iş oluşturma][216]
4. İş oluşturulduğunda iş adına tıklayın. Ardından, üst çubukta **Betik** sekmesine tıklayın.
5. Zamanlayıcı işlevinizin içine aşağıdaki betiği ekleyin. Yer tutucularını daha önce edindiğiniz bildirim hub'ı adınız ve *DefaultFullSharedAccessSignature* bağlantı dizeniz ile değiştirdiğinizden emin olun. **Kaydet** düğmesine tıklayın.
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );
6. Alt çubukta **Bir Kez Çalıştır**'a tıklayın. Cihazınızda bir uyarı almanız gerekir.

## Sonraki adımlar
Bu basit örnekte, tüm iOS cihazlarınıza anında iletme bildirimleri yayımladınız. Belirli kullanıcıları hedeflemek için, [Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma] öğreticisine bakın. Kullanıcılarınızı ilgi alanı gruplarına göre segmentlere ayırmak istiyorsanız [Son dakika haberleri göndermek için Notification Hubs kullanma]'yı okuyabilirsiniz. [Notification Hubs Kılavuzu] ve [iOS için Notification Hubs'ı Kullanma]'da Notification Hubs'ı kullanma hakkında daha fazla bilgi edinin.

<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK'sı]: http://go.microsoft.com/fwLink/?LinkID=266533
[Uygulama gönderme sayfası]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[Uygulamalarım]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Windows için Live SDK]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Mobile Services'i kullanmaya başlama]: /develop/mobile/tutorials/get-started-xamarin-ios
[Klasik Azure Portalı]: https://manage.windowsazure.com/
[Notification Hubs Kılavuzu]: http://msdn.microsoft.com/library/jj927170.aspx
[iOS için Notification Hubs'ı Kullanma]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal (iOS Hazırlama Portalı)]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Kullanıcılara anında iletme bildirimleri göndermek için Notification Hubs’ı kullanma]: /manage/services/notification-hubs/notify-users-aspnet
[Son dakika haberleri göndermek için Notification Hubs kullanma]: /manage/services/notification-hubs/breaking-news-dotnet

[Local and Push Notification Programming Guide (Yerel ve Anında İletilen Bildirim Programlama Kılavuzu)]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Anında İletilen Bildirim Servisi]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Bileşeni]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure Portal]: https://portal.azure.com



<!--HONumber=Sep16_HO3-->


