---
title: Bildirim hub'ları - Kurumsal gönderme mimari
description: Azure Notification hubs'ı bir Kurumsal ortamda kullanma yönergeleri
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 903023e9-9347-442a-924b-663af85e05c6
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 938801148b175456553865b54d59271021811401
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60873401"
---
# <a name="enterprise-push-architectural-guidance"></a>Kurumsal gönderim mimari kılavuzu

Kuruluşların bugün son kullanıcılarının (Dış) ya da mobil uygulamalar oluşturmaya yönelik veya (iç) çalışanlar için kademeli olarak geçiş yapıyor. Ana bilgisayarlar veya mobil uygulama mimarisi ile tümleştirilmelidir bazı LoB uygulamaları olması mevcut arka uç sistemlerine yerinde sahiptirler. Bu kılavuzda Bu tümleştirme senaryoları için olası çözüm öneren yapmak en iyi nasıl hakkında konuşuyor.

Sık kullanılan arka uç sistemleri ilgilendiğiniz bir olayı meydana geldiğinde anında iletme bildirimi, mobil uygulama üzerinden kullanıcılara göndermek için zorunludur. Örneğin, bir İphone'da bank'ın bankacılık uygulamasını yükleyen bir banka müşterisi, bir banka hesabı ya da burada bir Windows Phone üzerinde bir bütçe onay uygulamasını yükleyen bir çalışan Finans departmanından istediği intranet senaryosu belirli bir miktar yukarıda yapıldığında bildirim almak isteyen  onay isteği alındığında bildirilmesi için.

Banka hesabı ya da onay işleme kullanıcı için bir anında iletme başlatmalıdır bazı arka uç sistemi yapılması olasıdır. Tüm mantıksal bir olay bildirim tetiklendiğinde göndermek için aynı türde bir derleme birden çok tür arka uç sistemler, olabilir. Burada son kullanıcılar farklı bildirimlerine abone ve ayrıca birden çok mobil uygulama bile olabilir bir tek bir anında iletme sistemi ile birlikte çeşitli arka uç sistemleri tümleştirme karmaşıklığı burada arasındadır. Bir mobil uygulama bildirimleri gibi birden fazla arka uç sistemlerden almak için nereye isteyebilirsiniz, intranet mobil uygulamalar. Arka uç sistemlerine bilmiyorsanız veya anında iletme semantiği/teknolojisi burada genel bir çözüm ilgilendiğiniz herhangi bir olayı için arka uç sistemlerine yoklar ve anında iletme iletileri göndermek için sorumlu olduğu bir bileşen tanıtmak için geleneksel olarak olmuştur için bilmeniz gereken İstemci.

Azure Service Bus - çözüm ölçeklenebilir yaparken karmaşıklığı azaltan konu/abonelik modeli, kullanımı daha iyi bir çözümdür.

İşte çözüm mimarisine genel (birden çok mobil uygulamalarla genelleştirilmiş ancak yalnızca bir mobil uygulama olduğunda eşit oranda geçerlidir)

## <a name="architecture"></a>Mimari

![][1]

Bu mimari diyagramında SQL'ye programlama modeli konular/abonelikler sağlayan Azure Service Bus olan (adresinden hakkında daha fazla [Service Bus Pub/Sub programlama]). Bu durumda mobil arka uç, alıcı (genellikle [Azure mobil hizmeti], mobil uygulamalar için bir anında iletme başlatır) iletileri doğrudan arka uç sistemlerden ancak bunun yerine, almaz Ara bir Özet Katman tarafından sağlanan [Azure Service Bus], bir veya daha fazla arka uç sistemlerine iletileri almak mobil arka uç sağlar. Bir Service Bus konusu, her bir arka uç sistemi, örneğin, hesap, SA, temel anında iletme bildirimi gönderilecek iletilerin başlatan ilgilendiğiniz "konu" olan Finans oluşturulması gerekir. Arka uç sistemlerine bu konulara ileti gönderin. Bir mobil arka ucu, bir veya daha fazla gibi konuları Service Bus aboneliği oluşturarak abone olabilirsiniz. Mobil arka uç karşılık gelen arka uç sistemi bir bildirim almak için de yetki verir. Mobil arka uç aboneliklerini iletileri için dinleyecek şekilde devam eder ve bir ileti gelirse hemen sonra geri bırakır ve bildirim, bildirim hub'ına olarak gönderir. Notification hubs'ı sonra sonuçta ileti için mobil uygulama sunun. Anahtar bileşenleri listesi aşağıda verilmiştir:

1. Arka uç sistemlerine (iş kolu/eski sistemleri)
   * Hizmet veri yolu konusu oluşturur
   * İleti gönderir
1. Mobil arka uç
   * Hizmet aboneliği oluşturur.
   * İleti (arka uç sistemden) alır
   * İstemciler (aracılığıyla Azure bildirim hub'ı) için bildirim gönderir
1. Mobil uygulama
   * Alır ve bildirim görüntüleyin

### <a name="benefits"></a>Avantajlar

1. Alıcı (mobil uygulama/hizmet bildirim hub'ı aracılığıyla) ve gönderen (arka uç sistemleri) bağlantıyı kesmenize çok az değişiklikle tümleştirilmektedir ek arka uç sistemlerine sağlar.
1. Ayrıca, bir veya daha fazla arka uç sistemlerine olaylarını almak için birden çok mobil uygulama senaryosu kılar.  

## <a name="sample"></a>Örnek

### <a name="prerequisites"></a>Önkoşullar

Kavramları yanı sıra ortak oluşturma ve yapılandırma adımları hakkında bilgi için aşağıdaki öğreticilerde tamamlayın:

1. [Service Bus Pub/Sub programlama] -Bu öğreticide, hizmet veri yolu konuları/Abonelikleri, ile çalışma ayrıntılarını açıklayan konular/abonelikler, içermesi için bir ad alanı oluşturma nasıl bunları iletilerini & Gönder.
2. [Notification Hubs - Windows Evrensel Öğreticisi] -Bu öğreticide, bir Windows Store uygulaması ayarlama ve kaydetme ve ardından bildirimleri almak için Notification Hubs'ı kullanma açıklanmaktadır.

### <a name="sample-code"></a>Örnek kod

Tam örnek kodu kullanılabilir [bildirim hub'ı örnekleri]. Bu üç bileşene bölünür:

1. **EnterprisePushBackendSystem**

    a. Bu proje kullanan **WindowsAzure.ServiceBus** NuGet paketi ve dayanır [Service Bus Pub/Sub programlama].

    b. Mobil uygulamaya gönderilecek iletinin başlatan bir LoB sistemi benzetimini yapmak için basit bir C# konsol uygulaması uygulamasıdır.

    ```csharp
    static void Main(string[] args)
    {
        string connectionString =
            CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

        // Create the topic
        CreateTopic(connectionString);

        // Send message
        SendMessage(connectionString);
    }
    ```

    c. `CreateTopic` Service Bus konu oluşturmak için kullanılır.

    ```csharp
    public static void CreateTopic(string connectionString)
    {
        // Create the topic if it does not exist already

        var namespaceManager =
            NamespaceManager.CreateFromConnectionString(connectionString);

        if (!namespaceManager.TopicExists(sampleTopic))
        {
            namespaceManager.CreateTopic(sampleTopic);
        }
    }
    ```

    d. `SendMessage` Bu Service Bus konusuna ileti göndermek için kullanılır. Bu kod yalnızca konuya örnek amacıyla düzenli olarak bir dizi rastgele bir ileti gönderir. Normalde bir olay meydana geldiğinde, iletiler gönderen bir arka uç sistemi yoktur.

    ```csharp
    public static void SendMessage(string connectionString)
    {
        TopicClient client =
            TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

        // Sends random messages every 10 seconds to the topic
        string[] messages =
        {
            "Employee Id '{0}' has joined.",
            "Employee Id '{0}' has left.",
            "Employee Id '{0}' has switched to a different team."
        };

        while (true)
        {
            Random rnd = new Random();
            string employeeId = rnd.Next(10000, 99999).ToString();
            string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

            // Send Notification
            BrokeredMessage message = new BrokeredMessage(notification);
            client.Send(message);

            Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

            System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
        }
    }
    ```
2. **ReceiveAndSendNotification**

    a. Bu proje kullanan *WindowsAzure.ServiceBus* ve **Microsoft.Web.WebJobs.Publish** NuGet paketleri ve dayanır [Service Bus Pub/Sub programlama].

    b. Aşağıdaki konsol uygulaması olarak çalışan bir [Azure WebJob] LoB/arka uç sistemlerine gelen iletileri dinlemek için sürekli olarak çalıştırmak sahip olduğundan. Bu uygulama, mobil arka ucunuzdaki bir parçasıdır.

    ```csharp
    static void Main(string[] args)
    {
        string connectionString =
                 CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

        // Create the subscription that receives messages
        CreateSubscription(connectionString);

        // Receive message
        ReceiveMessageAndSendNotification(connectionString);
    }
    ```

    c. `CreateSubscription` konu için bir Service Bus aboneliği oluşturmak için arka uç sistemi iletileri göndereceği yeri kullanılır. İş senaryosuna bağlı olarak bu bileşen bir veya daha fazla abonelik ilgili konulara oluşturur. (örneğin, bazı iletilerin bazı Finans sistem ve benzeri ik sisteminden alıyor olabilir)

    ```csharp
    static void CreateSubscription(string connectionString)
    {
        // Create the subscription if it does not exist already
        var namespaceManager =
            NamespaceManager.CreateFromConnectionString(connectionString);

        if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
        {
            namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
        }
    }
    ```

    d. `ReceiveMessageAndSendNotification` ileti, aboneliği kullanarak konu başlığından okumak ve okuma başarılı olursa sonra bir bildirim (örnek senaryo bir Windows yerel bildirim), Azure Notification hubs'ı kullanarak mobil uygulamaya gönderilecek ustalıkla işleyin için kullanılır.

    ```csharp
    static void ReceiveMessageAndSendNotification(string connectionString)
    {
        // Initialize the Notification Hub
        string hubConnectionString = CloudConfigurationManager.GetSetting
                ("Microsoft.NotificationHub.ConnectionString");
        hub = NotificationHubClient.CreateClientFromConnectionString
                (hubConnectionString, "enterprisepushservicehub");

        SubscriptionClient Client =
            SubscriptionClient.CreateFromConnectionString
                    (connectionString, sampleTopic, sampleSubscription);

        Client.Receive();

        // Continuously process messages received from the subscription
        while (true)
        {
            BrokeredMessage message = Client.Receive();
            var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

            if (message != null)
            {
                try
                {
                    Console.WriteLine(message.MessageId);
                    Console.WriteLine(message.SequenceNumber);
                    string messageBody = message.GetBody<string>();
                    Console.WriteLine("Body: " + messageBody + "\n");

                    toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                    SendNotificationAsync(toastMessage);

                    // Remove message from subscription
                    message.Complete();
                }
                catch (Exception)
                {
                    // Indicate a problem, unlock message in subscription
                    message.Abandon();
                }
            }
        }
    }
    static async void SendNotificationAsync(string message)
    {
        await hub.SendWindowsNativeNotificationAsync(message);
    }
    ```

    e. Bu uygulama olarak yayımlamak için bir **WebJob**, Visual Studio'da çözüme sağ tıklayın ve seçin **WebJob olarak Yayımla**

    ![][2]

    f. Yayımlama profilinizi seçin ve bunu zaten bu webjob'ı barındıran mevcut değilse ve ardından Web sitesini oluşturduktan sonra yeni bir Azure Web sitesi oluşturma **Yayımla**.

    ![][3]

    g. İş için oturum açtığınızda, "Sürekli Çalıştır" olmasını yapılandırma [Azure portal] aşağıdaki gibi bir şey görmeniz gerekir:

    ![][4]

3. **EnterprisePushMobileApp**

    a. Bu uygulama, mobil arka ucunuzdaki bir parçası olarak çalışan bir WebJob kutlama bildirimleri aldığında bir Windows Store uygulaması ve görüntüleyin. Bu kod dayanır [Notification Hubs - Windows Evrensel Öğreticisi].  

    b. Uygulamanızı bildirim almaya etkin olduğundan emin olun.

    c. Aşağıdaki Notification Hubs kayıt kodu uygulamaya çağrılmakta olan zaman başlatıldığından emin olun (değiştirdikten sonra `HubName` ve `DefaultListenSharedAccessSignature` değerleri:

    ```csharp
    private async void InitNotificationsAsync()
    {
        var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
        var result = await hub.RegisterNativeAsync(channel.Uri);

        // Displays the registration ID so you know it was successful
        if (result.RegistrationId != null)
        {
            var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
    ```

### <a name="running-the-sample"></a>Örneği çalıştırma

1. WebJob'ınıza başarıyla çalışıyor ve sürekli olarak çalıştırılması zamanlanan emin olun.
2. Çalıştırma **EnterprisePushMobileApp**, Windows Store uygulaması başlatır.
3. Çalıştırma **EnterprisePushBackendSystem** LoB arka uç benzetimini yapar ve göndermeye başlar ve konsol uygulaması iletileri ve aşağıdaki gibi görünen kutlama bildirimleri görmeniz gerekir:

    ![][5]

4. İletileri ilk olarak Web işinizin Service Bus abonelikleri tarafından izlenen Service Bus konu başlıkları için gönderildi. Bir ileti alındığında bir bildirim oluşturulmuş ve mobil uygulamaya gönderilir. İşlem günlükleri bağlantısında olduğunuzda onaylamak için WebJob günlükleri aracılığıyla bakabilirsiniz [Azure portal] Web işiniz için:

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Bildirim hub'ı örnekleri]: https://github.com/Azure/azure-notificationhubs-samples
[Azure mobil hizmeti]: https://azure.microsoft.com/documentation/services/mobile-services/
[Azure Service Bus]: https://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Service Bus Pub/Sub programlama]: https://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: ../app-service/webjobs-create.md
[Notification Hubs - Windows Evrensel Öğreticisi]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure portal]: https://portal.azure.com/
