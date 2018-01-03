---
title: "Azure Notification Hubs güvenli bildirme"
description: "Azure'da güvenli anında iletme bildirimleri göndermek öğrenin. .NET API kullanarak C# dilinde yazılan kod örnekleri."
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 5aef50f4-80b3-460e-a9a7-7435001273bd
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 9c626ec1534c4899588150a58c0da57b9d963f6f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
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

Bu güvenli itme öğretici güvenli bir şekilde bir anında iletme bildirimi göndermek nasıl gösterir. Öğretici derlemeler [kullanıcılara bildirme](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) önce bu öğreticide adımları tamamlanmalıdır şekilde öğretici.

> [!NOTE]
> Bu öğreticide oluşturduğunuz ve bildirim hub'ınızı açıklandığı şekilde yapılandırılmış varsayar [bildirim hub'ları (Windows mağazası) ile çalışmaya başlama](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
> Ayrıca, Windows Phone 8.1 (Windows Phone değil) Windows kimlik bilgileri gerektirir ve arka plan görevleri Windows Phone 8.0 veya Silverlight 8.1 çalışmaz unutmayın. Windows mağazası uygulamaları için yalnızca uygulama ekran kilitleme etkin ise bir arka plan görevi aracılığıyla bildirimleri alabilirsiniz (Appmanifest, onay kutusuna tıklayın).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Windows Phone proje değiştirme
1. İçinde **NotifyUserWindowsPhone** projesi, App.xaml.cs için anında iletme arka plan görevi kaydetmek için aşağıdaki kodu ekleyin. Sonuna aşağıdaki kod satırını ekleyin `OnLaunched()` yöntemi:
   
        RegisterBackgroundTask();
2. Hala App.xaml.cs dosyasında, aşağıdaki ekleyin hemen sonra kod `OnLaunched()` yöntemi:
   
        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();
   
                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }
3. Aşağıdakileri ekleyin `using` deyimleri App.xaml.cs dosyasını üstündeki:
   
        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;
4. Visual Studio'daki **Dosya** menüsünde **Tümünü Kaydet**'e tıklayın.

## <a name="create-the-push-background-component"></a>Anında iletme arka plan bileşeni oluşturma
Sonraki adım, anında iletme arka plan bileşeni oluşturmaktır.

1. Çözüm Gezgini'nde çözüme üst düzey düğümünü sağ tıklayın (**çözüm SecurePush** bu durumda), ardından **Ekle**, ardından **yeni proje**.
2. Genişletme **mağazası uygulamaları**, ardından **Windows Phone uygulamaları**, ardından **Windows çalışma zamanı bileşeni (Windows Phone)**. Proje adı **PushBackgroundComponent**ve ardından **Tamam** projesi oluşturmak için.
   
    ![][12]
3. Çözüm Gezgini'nde sağ **PushBackgroundComponent (Windows Phone 8.1)** proje ve ardından **Ekle**, ardından **sınıfı**. Yeni sınıf **PushBackgroundTask.cs**. Tıklatın **Ekle** sınıfı oluşturmak için.
4. Tüm içeriğini değiştirin **PushBackgroundComponent** ad alanı tanımını aşağıdaki kodla yer tutucu değiştirerek `{back-end endpoint}` , arka uç dağıtırken elde arka uç uç noktası ile:
   
        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }
   
            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";
   
                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;
   
                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);
   
                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);
   
                    ShowToast(notification);
   
                    deferral.Complete();
                }
   
                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }
5. Çözüm Gezgini'nde sağ **PushBackgroundComponent (Windows Phone 8.1)** proje ve ardından **NuGet paketlerini Yönet**.
6. Sol taraftaki tıklatın **çevrimiçi**.
7. İçinde **arama** kutusuna **Http istemcisi**.
8. Sonuçlar listesinde tıklatın **Microsoft HTTP istemci kitaplıkları**ve ardından **yükleme**. Yüklemeyi tamamlayın.
9. NuGet edilene **arama** kutusuna **Json.net**. Yükleme **Json.NET** paketini sonra NuGet Paket Yöneticisi penceresini kapatın.
10. Aşağıdakileri ekleyin `using` deyimleri en üstündeki **PushBackgroundTask.cs** dosyası:
    
        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;
11. Çözüm Gezgini'nde, içinde **NotifyUserWindowsPhone (Windows Phone 8.1)** projesinde **başvuruları**, ardından **Başvuru Ekle...** . Başvuru Yöneticisi iletişim kutusunda yanındaki kutuyu işaretleyin **PushBackgroundComponent**ve ardından **Tamam**.
12. Çözüm Gezgini'nde çift tıklayarak **Package.appxmanifest** içinde **NotifyUserWindowsPhone (Windows Phone 8.1)** projesi. Altında **bildirimleri**ayarlayın **bildirim özellikli** için **Evet**.
    
    ![][3]
13. Hala **Package.appxmanifest**, tıklatın **bildirimleri** menü üstüne yakın. İçinde **kullanılabilir bildirimleri** açılan listesinde, tıklatın **arka plan görevleri**ve ardından **Ekle**.
14. İçinde **Package.appxmanifest**altında **özellikleri**, denetleme **anında bildirim**.
15. İçinde **Package.appxmanifest**altında **uygulama ayarları**, türü **PushBackgroundComponent.PushBackgroundTask** içinde **giriş noktası** alan.
    
    ![][13]
16. **Dosya** menüsünden **Tümünü Kaydet**'e tıklayın.

## <a name="run-the-application"></a>Uygulamayı çalıştırın
Uygulamayı çalıştırmak için aşağıdakileri yapın:

1. Visual Studio'da çalıştırın **AppBackend** Web API uygulaması. Bir ASP.NET web sayfası görüntülenir.
2. Visual Studio'da çalıştırın **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone Uygulama. Windows Phone öykünücüsü çalışır ve uygulamayı otomatik olarak yükler.
3. İçinde **NotifyUserWindowsPhone** uygulama kullanıcı Arabirimi, bir kullanıcı adı ve parola girin. Bunlar herhangi bir dize olabilir, ancak aynı değeri olması gerekir.
4. İçinde **NotifyUserWindowsPhone** uygulama kullanıcı Arabirimi, tıklatın **oturum açma ve kaydetme**. Ardından **Gönder itme**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
