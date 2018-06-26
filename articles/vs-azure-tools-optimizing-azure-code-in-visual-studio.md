---
title: Visual Studio'da Azure kodunuzu en iyi duruma getirme | Microsoft Docs
description: Kodunuzu daha sağlam ve çökmelerin yapmak en iyi duruma getirme araçları Visual Studio hakkında nasıl Azure Kod yardımcı öğrenin.
services: visual-studio-online
documentationcenter: na
author: cawa
manager: paulyuk
editor: ''
ms.assetid: ed48ee06-e2d2-4322-af22-07200fb16987
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: cawa
ms.openlocfilehash: 3ee2cc3ac5098ebf205331167faffa2b5f9b6d56
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937566"
---
# <a name="optimizing-your-azure-code"></a>Azure kodunuzu iyileştirme
Microsoft Azure kullanan uygulamalar programlama, uygulama ölçeklenebilirlik, davranış ve bulut ortamında bulunan performans sorunlarını önlemek için izlemeniz gereken bazı kodlama uygulamalarını vardır. Microsoft, tanır ve bu sık karşılaşılan sorunları çeşitli tanımlar ve bunları çözmenize yardımcı olacak bir Azure Kod Analizi aracı sağlar. Visual Studio'da NuGet aracılığıyla aracı yükleyebilirsiniz.

## <a name="azure-code-analysis-rules"></a>Azure Kod Analizi kuralları
Azure Kod Analizi aracı performansı etkileyen bilinen sorunlar bulduğunda otomatik olarak Azure kodunuzu bayrak için aşağıdaki kuralları kullanır. Sorunları görünen uyarıları olarak algılanan veya derleyici hataları. Genellikle kod düzeltmeleri veya uyarı veya hata çözümlemek için öneriler ampul simgesi üzerinden sağlanır.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Varsayılan (işlemdeki) oturum durumu modunu kullanmaktan kaçının
### <a name="id"></a>Kimlik
AP0000

### <a name="description"></a>Açıklama
Bulut uygulamaları için varsayılan (işlemdeki) oturum durumu modunu kullanıyorsanız, oturum durumu kaybedebilir.

Lütfen fikir ve geri bildirim sırasında paylaşımı [Azure Kod Analizi geri bildirim](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Neden
Varsayılan olarak, işlem içi oturum durumu modunu web.config dosyasında belirtilen. Ayrıca, yapılandırma dosyasında belirtilen giriş varsa, oturum durumu modu işlemdeki varsayılan olarak ayarlanır. İşlem içi modu oturum durumu web sunucusu üzerindeki bellekte depolar. Örneği yeniden ya da yeni bir örneğini Yük Dengeleme veya yük devretme desteği için kullanılan bellek web sunucusu üzerinde depolanan oturum durumu kaydedilmez. Bu durum, ölçeklenebilir bulut üzerinde engeller uygulama engeller.

ASP.NET oturum durumu için oturum durumu verilerini birkaç farklı depolama seçeneklerini destekler: InProc, StateServer, SQLServer, özel ve kapalı. Özel modu verileri barındırmak için bir dış oturum durumu deposunda gibi kullanmanız önerilir [Redis için Azure oturum durumu sağlayıcısı](http://go.microsoft.com/fwlink/?LinkId=401521).

### <a name="solution"></a>Çözüm
Bir önerilen bir yönetilen önbellek hizmetinde oturum durumunu depolamak için bir çözümdür. Nasıl kullanacağınızı öğrenin [Redis için Azure oturum durumu sağlayıcısı](http://go.microsoft.com/fwlink/?LinkId=401521) oturum durumunu depolamak için. Oturum durumu uygulamanızı bulutta ölçeklenebilir olduğundan emin olmak için diğer yerler depolayabilirsiniz. Lütfen okuyun alternatif çözümleri hakkında daha fazla bilgi edinmek için [oturum durumu modu](https://msdn.microsoft.com/library/ms178586).

## <a name="run-method-should-not-be-async"></a>Run yöntemini zaman uyumsuz olmamalıdır
### <a name="id"></a>Kimlik
AP1000

### <a name="description"></a>Açıklama
Zaman uyumsuz yöntemleri oluşturun (gibi [await](https://msdn.microsoft.com/library/hh156528.aspx)) dışında [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) yöntemi ve zaman uyumsuz yöntemleri çağırma [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Bildirme [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) yöntemi zaman uyumsuz olarak bir yeniden başlatma döngü girmek çalışan rolü neden olur.

Lütfen fikir ve geri bildirim sırasında paylaşımı [Azure Kod Analizi geri bildirim](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Neden
İçinde zaman uyumsuz yöntemleri çağırma [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) yöntemi çalışan rolü geri dönüştürmek bulut hizmeti çalışma zamanı neden olur. Çalışan rolü başladığında, tüm program yürütme içinde gerçekleşir [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) yöntemi. Çıkma [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) yöntemi yeniden başlatmak çalışan rolü neden olur. Zaman uyumsuz yöntem çalışan rolü çalışma zamanı geldiğinde, tüm işlemleri sonra async yöntemi gönderir ve ardından döndürür. Bu çalışan rolü'ndan çıkmak neden [ [ [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) yöntemi ve yeniden başlatın. Yinelemede sonraki yürütme, çalışan rolü zaman uyumsuz yöntem yeniden İsabetli ve çalışan rolü yeniden de geri dönüşüm yapmasına neden yeniden başlatır.

### <a name="solution"></a>Çözüm
Tüm zaman uyumsuz işlemleri dışında yerleştirin [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) yöntemi. Ardından, içinde işlenmiş zaman uyumsuz yöntemden çağırın [ [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) ](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) RunAsync () .wait gibi yöntemi. Azure Kod Analizi aracı, bu sorunu gidermenize yardımcı olabilir.

Aşağıdaki kod parçacığını bu sorunla ilgili kod düzeltme gösterir:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Hizmet veri yolu paylaşılan erişim imzası kimlik doğrulamasını kullan
### <a name="id"></a>Kimlik
AP2000

### <a name="description"></a>Açıklama
Paylaşılan erişim imzası (SAS) kimlik doğrulaması için kullanın. Erişim denetimi Hizmeti'nden (ACS) hizmet veri yolu kimlik doğrulaması için kullanım dışıdır.

Lütfen fikir ve geri bildirim sırasında paylaşımı [Azure Kod Analizi geri bildirim](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Neden
Gelişmiş güvenlik için Azure Active Directory ACS kimlik doğrulaması SAS kimlik doğrulaması ile değiştirmektir. Bkz: [Azure Active Directory olduğu ACS geleceği](https://cloudblogs.microsoft.com/enterprisemobility/2013/06/22/azure-active-directory-is-the-future-of-acs/) geçiş planı hakkında bilgi için.

### <a name="solution"></a>Çözüm
SAS kimlik doğrulaması uygulamalarınızı kullanın. Aşağıdaki örnek, bir hizmet veri yolu ad alanı veya varlık erişmek için var olan bir SAS belirteci kullanmayı gösterir.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Daha fazla bilgi için aşağıdaki konulara bakın.

* Genel bir bakış için bkz: [paylaşılan erişim imzası kimlik doğrulaması Service Bus ile](https://msdn.microsoft.com/library/dn170477.aspx)
* [Service Bus ile paylaşılan erişim imzası kimlik doğrulaması kullanma](https://msdn.microsoft.com/library/dn205161.aspx)
* Örnek proje için bkz: [hizmet veri yolu abonelikleri ile kullanarak paylaşılan erişim imzası (SAS) kimlik doğrulaması](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>"Döngü alırsınız" önlemek için Onmessageoptions yöntemini kullanmayı deneyin
### <a name="id"></a>Kimlik
AP2002

### <a name="description"></a>Açıklama
Bir "alma döngü," uygulamasına giderek önlemek için çağırma **Onmessageoptions** yöntemdir arama daha iletileri almak için daha iyi bir çözüm **alma** yöntemi. Ancak, kullanmanız gerekiyorsa **alma** yöntemi ve varsayılan olmayan sunucu bekleme süresini belirtin, sunucu bekleme süresi bir dakikadan fazla olduğundan emin olun.

Lütfen fikir ve geri bildirim sırasında paylaşımı [Azure Kod Analizi geri bildirim](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Neden
Çağrılırken **Onmessageoptions**, istemci sürekli sıra veya abonelik yokladığı bir dahili ileti Pompalama başlatır. Bu ileti Pompalama iletileri almak için bir çağrı sorunları sonsuz bir döngüde içerir. Arama zaman aşımına uğrarsa, yeni bir çağrı verir. Zaman aşımı aralığı değeri tarafından belirlenir [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) özelliği [Eventhubclient](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)kullanılan.

Kullanmanın avantajı **Onmessageoptions** karşılaştırılan **alma** kullanıcıları el ile iletileri için yoklama, özel durumları işleme, birden fazla ileti paralel işlem ve iletiler tamamlama gerekmemesidir.

Çağırırsanız **alma** emin olun, varsayılan değer kullanmadan *ServerWaitTime* değerdir bir dakikadan fazla. Ayarı *ServerWaitTime* bir dakikadan için sunucu ileti tamamen alınmadan önce zaman aşımına uğruyor engeller.

### <a name="solution"></a>Çözüm
Aşağıdaki kod örnekleri önerilen kullanımlar için bkz. Daha fazla ayrıntı için bkz: [QueueClient.OnMessage yöntemi (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)ve [QueueClient.Receive yöntemi (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

Azure Mesajlaşma altyapısı performansını geliştirmek için tasarım deseni bkz [zaman uyumsuz Mesajlaşma Primer](https://msdn.microsoft.com/library/dn589781.aspx).

Kullanarak bir örnek verilmiştir **Onmessageoptions** iletileri almak için.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

Kullanarak bir örnek verilmiştir **alma** varsayılan sunucusuyla bekleme süresi.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

Kullanarak bir örnek verilmiştir **alma** varsayılan olmayan sunucusuyla bekleme süresi.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Zaman uyumsuz hizmet veri yolu yöntemleri kullanmayı düşünün
### <a name="id"></a>Kimlik
AP2003

### <a name="description"></a>Açıklama
Aracılı Mesajlaşma ile performansını artırmak için zaman uyumsuz hizmet veri yolu yöntemlerini kullanın.

Lütfen fikir ve geri bildirim sırasında paylaşımı [Azure Kod Analizi geri bildirim](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Neden
Her çağrısının ana iş parçacığı engellemesini devre dışı bırakmak için zaman uyumsuz yöntemleri kullanarak uygulama programı eşzamanlılık sağlar. Service Bus Mesajlaşma yöntemleri, bir işlemi gerçekleştirilirken kullanırken (gönderme, alma, silme, vb.) zaman alır. Bu süre, istek ve yanıt gecikmesi ek olarak Service Bus hizmeti tarafından işlemi işlenmesini içerir. Saat başına işlem sayısını artırmak için işlemler aynı anda yürütmeniz gerekir. Daha fazla bilgi için lütfen [performans iyileştirmeleri kullanarak Service Bus aracılı Mesajlaşma için en iyi uygulamaları](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Çözüm
Bkz: [QueueClient sınıfı (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) önerilen zaman uyumsuz yönteminin nasıl kullanılacağı hakkında bilgi için.

Azure Mesajlaşma altyapısı performansını geliştirmek için tasarım deseni bkz [zaman uyumsuz Mesajlaşma Primer](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Bölümleme Service Bus kuyrukları ve konuları göz önünde bulundurun
### <a name="id"></a>Kimlik
AP2004

### <a name="description"></a>Açıklama
Bölüm Service Bus kuyrukları ve konularından Service Bus Mesajlaşma ile daha iyi performans için.

Lütfen fikir ve geri bildirim sırasında paylaşımı [Azure Kod Analizi geri bildirim](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Neden
Bölümlenmiş kuyruk veya konu, genel üretilen işi artık tek ileti Aracısı ya da ileti deposu performans ile sınırlı olduğundan, Service Bus kuyrukları ve konularından bölümleme performans verimlilik ve hizmet kullanılabilirliğini artırır. Ayrıca, bir Mesajlaşma deposu geçici bir kesinti bölümlenmiş kuyruk veya konu kullanılamaz hale değil. Daha fazla bilgi için bkz: [Mesajlaşma varlıkları bölümleme](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Çözüm
Aşağıdaki kod parçacığında, Mesajlaşma varlıkları bölüm gösterilmektedir.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Daha fazla bilgi için bkz: [bölümlenmiş Service Bus kuyrukları ve konularından | Microsoft Azure blogu](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) ve kullanıma [Microsoft Azure hizmet veri yolu kuyruğu bölümlenmiş](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) örnek.

## <a name="do-not-set-sharedaccessstarttime"></a>SharedAccessStartTime ayarlı değil
### <a name="id"></a>Kimlik
AP3001

### <a name="description"></a>Açıklama
Paylaşılan Erişim İlkesi hemen başlatmak için geçerli zamanın SharedAccessStartTimeset kullanarak kaçınmalısınız. Yalnızca paylaşılan erişim ilkesi daha sonraki bir zamanda başlatmak istiyorsanız bu özelliği ayarlamanız gerekir.

Lütfen fikir ve geri bildirim sırasında paylaşımı [Azure Kod Analizi geri bildirim](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Neden
Saati eşitleme veri merkezleri arasında küçük zaman farkı neden olur. Örneğin, mantıksal olarak DateTime.Now kullanarak geçerli zaman olarak başlangıç zamanı depolama SAS İlkesi ayarlama düşündüğünüz veya benzer bir yöntem hemen etkinleşmesini SAS İlkesi neden olur. Ancak, veri merkezi zamanlarda diğerlerinin bu öncesinde başlangıç saatinden sonraki biraz olabileceğinden veri merkezleri arasında hafif saat farklılıklarını bu sorunlara neden olabilir. Sonuç olarak, SAS İlkesi hızla (veya hatta hemen) dolabilir İlkesi yaşam süresi çok kısa olarak ayarlanmışsa.

Üzerinde Azure storage paylaşılan erişim imzası kullanma konusunda daha fazla yönergeler için bkz: [giriş tablosu SAS (paylaşılan erişim imzası) kuyruğu SAS ve Blob SAS - Microsoft Azure depolama ekibi blogu - güncelleştirmeye Site giriş - MSDN Bloglarında](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Çözüm
Paylaşılan erişim ilkesinin başlangıç saatini ayarlar deyimi kaldırın. Azure Kod Analizi aracı bu sorun için düzeltme sağlar. Güvenlik yönetimi hakkında daha fazla bilgi için lütfen bkz tasarım deseni [Valet anahtar düzeni](https://msdn.microsoft.com/library/dn568102.aspx).

Aşağıdaki kod parçacığını bu sorunla ilgili kod düzeltme gösterir.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Paylaşılan Erişim İlkesi sona erme saati beş dakikadan fazla olmalıdır
### <a name="id"></a>Kimlik
AP3002

### <a name="description"></a>Açıklama
Beş dakikalık fark kadar "saat eğme." bilinen bir koşul nedeniyle farklı konumlardaki veri merkezleri arasında saatleri içinde olabilir SAS önlemek için zaman aşımına uğramak gelen ilke belirteci planlanan daha erken sona erme saati beş dakikadan fazla olacak şekilde ayarlayın.

Lütfen fikir ve geri bildirim sırasında paylaşımı [Azure Kod Analizi geri bildirim](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Neden
Dünyanın dört farklı konumlardaki veri merkezlerine saati sinyali ile eşitleyin. Farklı bir konuma seyahat saat sinyal zaman alır çünkü her şeyin beklendiği gibi eşitlenir ancak farklı coğrafi konumlardaki veri merkezlerine arasındaki zaman farkı olabilir. Bu zaman farkı, paylaşılan erişim ilkesi başlangıç saati ve sona erme aralığını etkileyebilir. Bu nedenle, paylaşılan erişim ilkesi hemen etkinleşir emin olmak için başlangıç saati belirtmezsiniz. Ayrıca, sona erme zamanı erken zaman aşımını önlemek için birden fazla 5 dakika olduğundan emin olun.

Azure depolama alanında paylaşılan erişim imzası kullanma hakkında daha fazla bilgi için bkz: [giriş tablosu SAS (paylaşılan erişim imzası) kuyruğu SAS ve Blob SAS - Microsoft Azure depolama ekibi blogu - güncelleştirmeye Site giriş - MSDN Bloglarında](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Çözüm
Güvenlik yönetimi hakkında daha fazla bilgi için bkz: tasarım deseni [Valet anahtar düzeni](https://msdn.microsoft.com/library/dn568102.aspx).

Paylaşılan Erişim İlkesi başlangıç zamanı belirtmeden bir örnek verilmiştir.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Bir ilke sona erme süresi beş dakikadan fazla ile bir paylaşılan erişim ilkesi başlangıç saati belirten bir örnek verilmiştir.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Daha fazla bilgi için bkz: [oluşturmak ve paylaşılan erişim imzası kullanmak](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>CloudConfigurationManager kullanın
### <a name="id"></a>Kimlik
AP4000

### <a name="description"></a>Açıklama
Kullanarak [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) sınıf projelerde gibi Azure Web sitesine gidin ve Azure mobil hizmetler çalışma zamanı sorunlarına neden olmaz. En iyi uygulama, ancak bunu bulut kullanmak için iyi bir fikirdir[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager\(v=vs.110\).aspx) tüm Azure bulut uygulamaları için yapılandırmaları yönetme birleşik bir yolu olarak.

Lütfen fikir ve geri bildirim sırasında paylaşımı [Azure Kod Analizi geri bildirim](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Neden
CloudConfigurationManager uygulama ortamınız için uygun yapılandırma dosyasını okur.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Çözüm
Kullanmak için kodunuzu yeniden düzenlemeniz [CloudConfigurationManager sınıfı](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx). Bu sorun için bir kod düzeltme Azure Kod Analizi aracı tarafından sağlanır.

Aşağıdaki kod parçacığını bu sorunla ilgili kod düzeltme gösterir. Değiştir

`var settings = ConfigurationManager.AppSettings["mySettings"];`

with

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Yapılandırma ayarı App.config veya Web.config dosyasında depolamak nasıl bir örneği burada verilmiştir. Ayarları yapılandırma dosyasının appSettings bölümünü ekleyin. Önceki kod örneğinde için Web.config dosyasının verilmiştir.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Sabit kodlanmış bağlantı dizelerini kullanmaktan kaçının
### <a name="id"></a>Kimlik
AP4001

### <a name="description"></a>Açıklama
Sabit kodlanmış bağlantı dizelerini kullanın ve daha sonra güncelleştirmek ihtiyacınız varsa, kaynak kodunuzu değişiklikleri yapın ve uygulamayı yeniden derlemenize gerekir. Yapılandırma dosyasında bağlantı dizelerinizi depolarsanız, ancak bunu daha sonra yapılandırma dosyasını güncelleştirerek değiştirebilirsiniz.

Lütfen fikir ve geri bildirim sırasında paylaşımı [Azure Kod Analizi geri bildirim](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Neden
Sabit kodlama bağlantı dizeleri nedeni hatalı bir uygulama bağlantı dizeleri hızlı bir şekilde değiştirilmesi gerektiğinde sorunları tanıtır. Ayrıca, proje kaynak denetimine iade edilmesi gerekirse, sabit kodlanmış bağlantı dizeleri güvenlik açıkları dizeleri kaynak kodunda görüntülenebilir beri tanıtmaktadır.

### <a name="solution"></a>Çözüm
Bağlantı dizelerini yapılandırma dosyaları ya da Azure ortamı depolar.

* Bağımsız uygulamalar için app.config bağlantı dizesi ayarlarını depolamak için kullanın.
* IIS tarafından barındırılan web uygulamaları için web.config bağlantı dizeleri depolamak için kullanın.
* ASP.NET vNext uygulamaları için bağlantı dizelerini depolamak için configuration.json kullanın.

Web.config veya app.config gibi yapılandırma dosyalarını kullanma hakkında daha fazla bilgi için bkz: [ASP.NET Web yapılandırma yönergeleri](https://msdn.microsoft.com/library/vstudio/ff400235\(v=vs.100\).aspx). Nasıl Azure ortam değişkenleri çalışma hakkında daha fazla bilgi için bkz: [Azure Web siteleri: nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Bağlantı dizesi kaynak denetiminde depolama hakkında daha fazla bilgi için bkz: [bağlantı dizeleri gibi hassas bilgileri kaynak kodu deposunda saklanır dosyalarında Geçirilmemesi](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Tanılama yapılandırma dosyası kullanın
### <a name="id"></a>Kimlik
AP5000

### <a name="description"></a>Açıklama
Tanılama ayarları gibi kodunuzda API programlama Microsoft.WindowsAzure.Diagnostics kullanarak yapılandırmak yerine, diagnostics.wadcfg dosyasında tanılama ayarlarını yapılandırmanız gerekir. (Ya da Azure SDK 2.5 kullanıyorsanız diagnostics.wadcfgx). Bunu yaparak, kodunuzu yeniden derlemenize gerek kalmadan tanılama ayarlarını değiştirebilirsiniz.

Lütfen fikir ve geri bildirim sırasında paylaşımı [Azure Kod Analizi geri bildirim](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Neden
Azure SDK 2.5 (hangi Azure tanılama 1.3 kullanır), Azure tanılama (WAD) birkaç farklı yöntemler kullanarak yapılandırılabilir önce: Bu yapılandırma blob depolama, kesinlik temelli kodu, bildirim temelli yapılandırma veya varsayılan kullanarak ekleme yapılandırma. Ancak, tanılama yapılandırmak için tercih edilen yöntem bir XML yapılandırma dosyasında (diagnostics.wadcfg veya diagnositcs.wadcfgx SDK 2.5 ve sonrası) uygulama projesi kullanmaktır. Bu yaklaşımda, diagnostics.wadcfg dosya tamamen yapılandırmasını tanımlar ve güncelleştirilebilir ve gerçekleştirilse imzalanmasını. Diagnostics.wadcfg yapılandırma dosyasının kullanımını kullanarak yapılandırmalarını ayarlama programlama yöntemleri ile karıştırma [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)veya [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)sınıfları olabilir karışıklığa yol açar. Bkz: [başlatılamadı ya da değişiklik Azure tanılama Yapılandırması](https://msdn.microsoft.com/library/azure/hh411537.aspx) daha fazla bilgi için.

WAD 1.3 (Azure SDK 2.5 ile dahil) itibaren artık kod tanılama yapılandırmak mümkündür. Sonuç olarak, yalnızca uygulama ya da tanılama uzantısını güncelleştirilirken yapılandırması sağlayabilir.

### <a name="solution"></a>Çözüm
Tanılama ayarları tanılama yapılandırma dosyasına (diagnositcs.wadcfg veya diagnositcs.wadcfgx SDK 2.5 ve sonrası) taşımak için tanılama yapılandırma Tasarımcısı'nı kullanın. Ayrıca, yüklemenizi tavsiye edilir [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) ve en son Tanılama özelliğini kullanın.

1. Yapılandırmak istediğiniz rolü için kısayol menüsünde, Özellikler'i seçin ve ardından yapılandırma sekmesini seçin.
2. İçinde **tanılama** bölümünde, olduğundan emin olun **tanılamayı etkinleştir** onay kutusu seçilidir.
3. Seçin **yapılandırma** düğmesi.

   ![Tanılamayı etkinleştir seçeneğine erişme](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

   Bkz: [yapılandırma tanılama Azure Cloud Services ve sanal makineler için](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) daha fazla bilgi için.

## <a name="avoid-declaring-dbcontext-objects-as-static"></a>DbContext nesneleri statik olarak bildirme kaçının
### <a name="id"></a>Kimlik
AP6000

### <a name="description"></a>Açıklama
Bellek kaydetmek için statik olarak DBContext nesne bildirme kaçının.

Lütfen fikir ve geri bildirim sırasında paylaşımı [Azure Kod Analizi geri bildirim](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Neden
DBContext nesneleri her çağrı sorgu sonuçlarından barındırır. Uygulama etki alanı bellekten kaldırılana kadar statik DBContext nesneler atıldı değil. Bu nedenle, bir statik DBContext nesnesi, büyük miktarlarda bellek kullanabilir.

### <a name="solution"></a>Çözüm
Bir yerel değişken ya da statik olmayan örnek alanı olarak DBContext bildirme, bir görev için kullanacağınız ve sonra kullanıldıktan sonra elden sağlayabilirsiniz.

Aşağıdaki örnek MVC denetleyicisi sınıfı DBContext nesnesinin nasıl kullanılacağını gösterir.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla hakkında en iyi duruma getirme ve Azure uygulamaları sorunlarını giderme öğrenmek için bkz: [Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme](app-service/web-sites-dotnet-troubleshoot-visual-studio.md).
