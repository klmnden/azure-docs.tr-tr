---
title: Azure Notification hubs'ı güvenli gönderme
description: Azure'da güvenli bir anında iletme bildirimleri göndermeyi öğrenin. .NET API kullanarak C# dilinde yazılan kod örnekleri.
documentationcenter: windows
author: jwargo
manager: patniko
editor: spelluru
services: notification-hubs
ms.assetid: 5aef50f4-80b3-460e-a9a7-7435001273bd
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: windows
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: cf23ef5df3bdcaad23841da111fa06cc36b4cd57
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61459253"
---
# <a name="securely-push-notifications-from-azure-notification-hubs"></a>Azure Notification Hubs güvenli bir şekilde anında bildirimler

> [!div class="op_single_selector"]
> * [Windows Evrensel](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

## <a name="overview"></a>Genel Bakış

Mobil için tüketici hem kurumsal uygulamalar için anında iletme bildirimleri yürütmesinin büyük ölçüde basitleştirir ve kullanımı kolay, çok platformlu, ölçeği genişletilmiş bir anında iletme altyapı, erişmek Microsoft azure'da anında iletme bildirimi desteği sağlar Platform.

Yasal nedeniyle veya güvenlik kısıtlamaları, bazen bir uygulama bir sorun standart bir anında iletme bildirimi altyapısı aracılığıyla aktarılan bildirim dahil olmak isteyebilirsiniz. Bu öğreticide, istemci cihaz ve uygulama arka ucu arasında güvenli, kimliği doğrulanmış bir bağlantı üzerinden hassas bilgiler göndererek aynı deneyimi elde etmek açıklar.

Yüksek bir düzeyde akışı aşağıdaki gibidir:

1. Uygulama arka ucu:
   * Arka uç veritabanı güvenli yükteki depolar.
   * Bu bildirim kimliği (hiçbir güvenli bilgiler gönderilir) cihaza gönderir.
2. Cihazın, bildirim alındığında uygulama:
   * Cihaz güvenli yükü isteme ve arka uç bağlantı kurar.
   * Uygulamanın cihaz bildirim olarak yük gösterebilirsiniz.

Önceki akış (ve Bu öğreticide), kullanıcı oturum açtığında sonra cihaz kimlik doğrulama belirteci yerel depolama alanında depolar olduğunu varsayıyoruz olduğunu unutmayın. Cihaz Bu belirteci kullanarak güvenli bildirimin yükü alabilirsiniz gibi bu tamamen sorunsuz bir deneyim garanti eder. Uygulamanız kimlik doğrulama belirteçlerinizi cihazda depolamaz veya bu belirteçlerin süresi, bildirim alma sırasında cihaz uygulaması uygulamayı başlatmak için kullanıcıdan genel bir bildirim görüntülenmesi gerekir. Uygulama kullanıcının kimliğini doğrular ve bildirim yükü gösterir.

Bu güvenli gönderme Öğreticisi, güvenli bir şekilde anında iletme bildirimi gönderme işlemi gösterilmektedir. Öğreticiyi yapılar [kullanıcılara bildirme](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) adımları Bu öğreticinin ilk tamamlamanız gereken şekilde öğretici.

> [!NOTE]
> Bu öğreticide oluşturduğunuz ve bildirim hub'ınıza açıklandığı gibi yapılandırılmış varsayılır [Notification hubs'ı (Windows Store) ile çalışmaya başlama](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
> Ayrıca, Windows Phone 8.1 (Windows Phone değil) Windows kimlik bilgileri gerektirir ve arka plan görevleri Silverlight 8.1 veya Windows Phone 8.0 çalışmadığını unutmayın. Windows Store uygulamaları için yalnızca uygulama kilit ekranı etkin ise bir arka plan görevi aracılığıyla bildirim alabilir (onay kutusu Appmanifest tıklayın).

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Windows Phone projesi değiştirme

1. İçinde **NotifyUserWindowsPhone** projesinde, App.xaml.cs anında iletme arka plan görevinin kaydetmek için'için aşağıdaki kodu ekleyin. `OnLaunched()` yönteminin sonuna aşağıdaki kod satırını ekleyin:

    ```c#
    RegisterBackgroundTask();
    ```
2. Hala App.xaml.cs dosyasında, aşağıdaki ekleyin hemen sonra kod `OnLaunched()` yöntemi:

    ```c#
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
    ```
3. Aşağıdaki `using` App.xaml.cs dosyasının en üstüne deyimlerini:

    ```c#
    using Windows.Networking.PushNotifications;
    using Windows.ApplicationModel.Background;
    ```
4. Visual Studio'daki **Dosya** menüsünde **Tümünü Kaydet**'e tıklayın.

## <a name="create-the-push-background-component"></a>Anında iletme arka plan bileşeni oluşturma

Sonraki adım, anında iletme arka plan bileşeni oluşturmaktır.

1. Çözüm Gezgini'nde çözümün en üst düzey düğümü (**çözüm SecurePush** bu durumda), ardından **Ekle**, ardından **yeni proje**.
2. Genişletin **Store uygulamaları**, ardından **Windows Phone uygulamaları**, ardından **Windows çalışma zamanı bileşeni (Windows Phone)** . Projeyi adlandırın **PushBackgroundComponent**ve ardından **Tamam** projeyi oluşturmak için.

    ![][12]
3. Çözüm Gezgini'nde sağ **PushBackgroundComponent (Windows Phone 8.1)** proje'a tıklayın **Ekle**, ardından **sınıfı**. Yeni bir sınıf adı `PushBackgroundTask.cs`. Tıklayın **Ekle** sınıfı oluşturun.
4. Tüm içeriğini değiştirin `PushBackgroundComponent` ad alanı tanımını aşağıdaki kodla yer tutucusunu değiştirerek `{back-end endpoint}` arka ucunuz dağıtırken elde arka uç noktası ile:

    ```csharp
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
    ```
5. Çözüm Gezgini'nde sağ **PushBackgroundComponent (Windows Phone 8.1)** proje ve ardından **NuGet paketlerini Yönet**.
6. Sol taraftaki **Çevrimiçi** öğesine tıklayın.
7. **Arama** kutusuna **Http İstemcisi** yazın.
8. Sonuçlar listesinde **Microsoft HTTP istemcisi kitaplıkları**ve ardından **yükleme**. Yüklemeyi tamamlayın.
9. NuGet **Arama** kutusuna **Json.net** yazın. Yükleme **Json.NET** paketini ve ardından NuGet Paket Yöneticisi penceresini kapatın.
10. Aşağıdaki `using` deyimleri en üstündeki `PushBackgroundTask.cs` dosyası:

    ```c#
    using Windows.ApplicationModel.Background;
    using Windows.Networking.PushNotifications;
    using System.Net.Http;
    using Windows.Storage;
    using System.Net.Http.Headers;
    using Newtonsoft.Json;
    using Windows.UI.Notifications;
    using Windows.Data.Xml.Dom;
    ```
11. Çözüm Gezgini'nde, **NotifyUserWindowsPhone (Windows Phone 8.1)** projesinde **başvuruları**, ardından **Başvuru Ekle...** . Başvuru Yöneticisi iletişim kutusunda yanındaki kutuyu işaretleyin **PushBackgroundComponent**ve ardından **Tamam**.
12. Çözüm Gezgini'nde çift tıklayarak **Package.appxmanifest** içinde **NotifyUserWindowsPhone (Windows Phone 8.1)** proje. Altında **bildirimleri**ayarlayın **bildirim özellikli** için **Evet**.

    ![][3]
13. Hala **Package.appxmanifest**, tıklayın **bildirimleri** üst menü. İçinde **kullanılabilir bildirimler** açılır listesinde, tıklayın **arka plan görevleri**ve ardından **Ekle**.
14. İçinde **Package.appxmanifest**altında **özellikleri**, kontrol **anında iletme bildirimi**.
15. İçinde **Package.appxmanifest**altında **uygulama ayarları**, türü **PushBackgroundComponent.PushBackgroundTask** içinde **giriş noktası** alan.

    ![][13]
16. **Dosya** menüsünden **Tümünü Kaydet**'e tıklayın.

## <a name="run-the-application"></a>Uygulamayı çalıştırın

Uygulamayı çalıştırmak için aşağıdakileri yapın:

1. Visual Studio'da çalıştırma **AppBackend** Web API uygulaması. Bir ASP.NET web sayfası görüntülenir.
2. Visual Studio'da çalıştırma **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone uygulaması. Windows Phone öykünücü çalışır ve uygulamayı otomatik olarak yükler.
3. İçinde **NotifyUserWindowsPhone** uygulama kullanıcı Arabirimi, bir kullanıcı adı ve parola girin. Bunlar herhangi bir dize olabilir, ancak aynı değere sahip olmalıdır.
4. İçinde **NotifyUserWindowsPhone** kullanıcı Arabirimi, uygulamaya tıklayın **oturum açma ve kaydetme**. Ardından **gönderin, anında iletme**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
