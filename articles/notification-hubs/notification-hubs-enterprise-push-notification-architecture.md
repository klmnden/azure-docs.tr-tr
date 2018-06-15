---
title: Notification Hubs - kuruluş anında iletme mimarisi
description: Bir kuruluş ortamında Azure Notification Hubs kullanma yönergeleri
services: notification-hubs
documentationcenter: ''
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 903023e9-9347-442a-924b-663af85e05c6
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: d7066b58330d35e5dba66cfe6ed5cfaddff4b68a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33778071"
---
# <a name="enterprise-push-architectural-guidance"></a>Kurumsal gönderim mimari kılavuzu
Kuruluşlar bugün kademeli olarak mobil uygulamaları ya da kendi son kullanıcıları (harici) oluşturmak için doğru veya (iç) çalışanlar için geçiyor. Ana bilgisayarlar veya mobil uygulama mimariye tümleşik bazı iş kolu uygulamaları olması varolan arka uç sistemleri yerinde sahiptirler. Bu kılavuz, bu tümleştirme senaryoları için olası çözüm öneren yapmak en iyi nasıl hakkında alınmaktadır.

Arka uç sistemleri ilgi bir olay meydana geldiğinde anında iletme bildirimi kendi mobil uygulama aracılığıyla kullanıcılara göndermek için sık gerekli değildir. Örneğin, bir İphone'da bankanın bankacılık uygulaması olan bir banka müşteri hesabı veya bir Windows Phone bir bütçe onay uygulaması olan bir çalışanın Finans departmanından burada istediği intranet senaryosu belirli bir miktar yukarıda borç yapıldığında bildirim almak isteyen  onay isteği alındığında bildirim almak için.

Banka hesabı veya onay işleme kullanıcı için bir itme başlatmalıdır bazı arka uç sistem yapılması olasıdır. Tüm bildirim bir olay tetikler push mantığı aynı türde oluşturmalısınız birden çok tür arka uç sistemleri, olabilir. Burada son kullanıcıların farklı bildirimlerine abone ve ayrıca birden çok mobil uygulama bile olabilir tek itme sistemiyle birlikte birkaç arka uç sistemleri tümleştirme karmaşıklık burada arasındadır. Burada bir mobil uygulama bildirimleri gibi birden fazla arka uç sistemlerden almak isteyebilirsiniz örneğin, intranet mobil uygulamalar. Arka uç sistemleri bilmiyorsanız veya burada ortak bir çözüm geleneksel herhangi olaylarının için arka uç sistemleri yoklar ve anında iletme iletileri göndermek için sorumlu olan bir bileşen tanıtmak için bırakıldı şekilde itme semantiği/teknolojisini bilmeniz gerekir İstemci.

Azure Service Bus - çözüm ölçeklenebilir yaparken karmaşıklığını azaltır konu başlığının/aboneliğinin modelini kullanmak daha iyi bir çözümdür.

İşte çözümünün genel mimari (birden çok mobil uygulamaları ile genelleştirilmiş ancak yalnızca bir mobil uygulama olduğunda eşit oranda geçerlidir)

## <a name="architecture"></a>Mimari
![][1]

Anahtar mimarisi Bu diyagramda programlama modeli konuları/abonelikleri sağlayan Azure Service Bus parçasıdır (adresinden hakkında daha fazla [Service Bus Pub/alt programlama]). Bu durumda, mobil arka uç olduğundan alıcı (genellikle [Azure mobil hizmeti], mobil uygulamaları için bir itme başlatan) iletileri doğrudan arka uç sistemlerden ancak bunun yerine, almaz bir ara Özet [Azure hizmet bir veya daha fazla arka uç sistemlerinden iletileri almak mobil arka uç sağlayan veri yolu,. tarafından sağlanan katmanı Hizmet veri yolu konusu her arka uç sistemleri, örneğin, hesap, SA, neredeyse anında iletme bildirimi gönderilecek iletilerin başlatır ilginizi çeken "konuları" olan Finans oluşturulması gerekir. Arka uç sistemleri bu konulara iletileri gönderir. Bir mobil arka uç, bir hizmet veri yolu abonelik oluşturarak bir veya daha fazla gibi konular için abone olabilirsiniz. Bu, karşılık gelen arka uç sistemden bir bildirim almak için mobil arka uç hakkı verir. Aboneliği iletilerde dinlemek mobil arka uç devam eder ve bir ileti ulaşır ulaşmaz geri kapatır ve kendi bildirim hub'ına bildirimi olarak gönderir. Bildirim hub'ları sonra sonunda ileti mobil uygulamaya sunar. Anahtar bileşenleri listesi aşağıdadır:

1. Arka uç sistemleri (LoB/eski sistemler için)
   * Hizmet veri yolu konusu oluşturur
   * İleti gönderir
2. Mobil arka uç
   * Hizmet aboneliği oluşturur
   * İleti (arka uç sistemden) alır
   * İstemciler (aracılığıyla Azure bildirim hub'ı) bildirim gönderir
3. Mobil uygulama
   * Alır ve bildirim göster

### <a name="benefits"></a>Yararları:
1. Alıcı (mobil uygulama/hizmet bildirim hub'ı aracılığıyla) ve gönderen (arka uç sistemleri) arasında kesilmesi en az değişiklikle tümleştirilmekte ek arka uç sistemleri sağlar.
2. Ayrıca, bir veya daha fazla arka uç sistemlerden olaylarını almak için birden çok mobil uygulama senaryo sağlar.  

## <a name="sample"></a>Örnek:
### <a name="prerequisites"></a>Önkoşullar
Kavramları yanı sıra ortak oluşturma ve yapılandırma adımları hakkında bilgi edinmek için aşağıdaki öğreticileri tamamlayın:

1. [Service Bus Pub/alt programlama] -Bu öğretici hizmet veri yolu konuları/abonelikleri ile çalışma ayrıntılarını açıklayan konuları/abonelikleri içerecek şekilde bir ad alanı oluşturmak nasıl göndermek ve bunları iletileri almak nasıl.
2. [Notification Hubs - Windows Evrensel Öğreticisine] -Bu öğretici bir Windows mağazası uygulama ayarlama ve kaydetme ve bildirimleri almak için Notification Hubs'ı kullanma açıklanmaktadır.

### <a name="sample-code"></a>Örnek kod
Tam örnek kod şu adresten edinilebilir [bildirim Hub örnekleri]. Üç bileşenlerine ayrılır:

1. **EnterprisePushBackendSystem**
   
    a. Bu proje kullanıyor *WindowsAzure.ServiceBus* NuGet paketi ve dayanır [Service Bus Pub/alt programlama].
   
    b. Mobil uygulamaya teslim edilecek ileti başlatan bir LoB sistem benzetimini yapmak için bir basit C# konsol uygulaması uygulamasıdır.
   
        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the topic
            CreateTopic(connectionString);
   
            // Send message
            SendMessage(connectionString);
        }
   
    c. `CreateTopic` Service Bus konu oluşturmak için kullanılır.
   
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
   
    d. `SendMessage` Bu hizmet veri yolu konusu iletileri göndermek için kullanılır. Bu kod konu örnek amacıyla düzenli olarak yalnızca bir dizi rastgele ileti gönderir. Normalde bir olay gerçekleştiğinde, iletileri gönderen bir arka uç sistemi yoktur.
   
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
2. **ReceiveAndSendNotification**
   
    a. Bu proje kullanıyor *WindowsAzure.ServiceBus* ve *Microsoft.Web.WebJobs.Publish* NuGet paketleri ve dayanır [Service Bus Pub/alt programlama].
   
    b. Aşağıdaki konsol uygulaması olarak çalışan bir [Azure WebJob] LoB/arka uç sistemleri iletilerden dinlemek için sürekli çalışacak şekilde olduğundan. Bu uygulamayı mobil arka bir parçasıdır.
   
        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the subscription that receives messages
            CreateSubscription(connectionString);
   
            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }
   
    c. `CreateSubscription` konu için bir hizmet veri yolu aboneliği oluşturmak için burada arka uç sistemi iletiler gönderir kullanılır. İş senaryosu bağlı olarak bu bileşen bir veya daha fazla abonelik ilgili konulara oluşturur (örneğin, bazı iletileri sisteminden HR bazı Finans sistem ve benzeri alıyor olabilir)
   
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
   
    d. ReceiveMessageAndSendNotification aboneliğini kullanarak konusundan iletisini okuyun ve okuma başarılı olursa sonra bir bildirim (senaryoda örnek Windows yerel bildirim) Azure bildirim hub'ları kullanarak mobil uygulamaya gönderilmek üzere oluşturabilir için kullanılır.
   
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
   
    e. Bu uygulama olarak yayımlamak için bir **WebJob**, Visual Studio çözümüne sağ tıklayın ve seçin **Web işi olarak Yayımla**
   
    ![][2]
   
    f. Yayımlama profilinizi seçin ve bunu zaten, bu Web işi barındıran mevcut değilse ve ardından Web sitesini bulduktan sonra yeni bir Azure Web sitesi oluşturma **Yayımla**.
   
    ![][3]
   
    g. Böylece zaman oturum "Sürekli Çalıştır" olarak iş yapılandırma [Azure portal] aşağıdaki gibi bir şey görmeniz gerekir:
   
    ![][4]
3. **EnterprisePushMobileApp**
   
    a. Bu uygulamayı mobil arka bir parçası olarak çalışan Web işi bildirimleri alan bir Windows mağazası uygulaması ve görüntüleyin. Bu kod dayanır [Notification Hubs - Windows Evrensel Öğreticisine].  
   
    b. Uygulamanızı bildirim almaya etkinleştirildiğinden emin olun.
   
    c. Kayıt kodu uygulamaya çağrılma aşağıdaki Notification Hubs zaman başlatıldığından emin olun (değiştirildikten sonra *HubName* ve *DefaultListenSharedAccessSignature*:
   
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

### <a name="running-sample"></a>Örnek çalıştırma:
1. Web işi başarıyla çalıştığından ve sürekli olarak çalışmak üzere zamanlanmış emin olun.
2. Çalıştırma ** Windows mağazası uygulaması başlatır EnterprisePushMobileApp.
3. Çalıştırma **EnterprisePushBackendSystem** LoB arka canlandırır ve göndermeye başlar konsol uygulaması iletileri ve aşağıdaki görüntü gibi görünen bildirimleri görmeniz gerekir:
   
    ![][5]
4. İletileri, ilk olarak Web işinizin Service Bus Aboneliklerde tarafından izlenen Service Bus konu başlıklarını gönderildi. Bir ileti alındığında bir bildirim oluşturuldu ve mobil uygulamaya gönderilir. İşlem günlükleri bağlantıyı gittiğinizde onaylamak için Web işi günlükleri aracılığıyla bakabilirsiniz [Azure portal] Web işi için:
   
    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[bildirim Hub örnekleri]: https://github.com/Azure/azure-notificationhubs-samples
[Azure mobil hizmeti]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Service Bus Pub/alt programlama]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: ../app-service/web-sites-create-web-jobs.md
[Notification Hubs - Windows Evrensel Öğreticisine]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure portal]: https://portal.azure.com/
