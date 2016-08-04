<properties
    pageTitle="Service Bus kuyruklarını kullanma (.NET) | Microsoft Azure"
    description="Azure'da Service Bus kuyruklarını kullanmayı öğrenin. .NET API kullanarak C# dilinde yazılan kod örnekleri."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="05/09/2016"
    ms.author="sethm"/>

# Service Bus kuyruklarını kullanma

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu makalede, Service Bus kuyruklarının nasıl kullanılacağı açıklanır. Örnekler C# dilinde yazılmıştır ve .NET API kullanır. Bu makalede bulunan senaryolar, kuyruk oluşturmanın yanı sıra ileti alma ve göndermeyi içerir. Kuyruklar hakkında daha fazla bilgi edinmek için [Sonraki adımlar](#next-steps) bölümüne bakın.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## Service Bus NuGet paketi ekleme

[Service Bus NuGet paketi](https://www.nuget.org/packages/WindowsAzure.ServiceBus), Service Bus API'sini almanın ve uygulamanızı tüm Service Bus bağımlılıklarıyla yapılandırmanın en kolay yoludur. Uygulamanıza NuGet paketini yüklemek için aşağıdakileri yapın:

1.  Çözüm Gezgini'nde, **Başvurular**'a sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın.
2.  "Service Bus" için arama yapın ve **Microsoft Azure Service Bus** öğesini seçin. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından bu iletişim kutusunu kapatın.

    ![][7]

Artık Service Bus için kod yazmaya hazırsınız.

## Service Bus bağlantı dizesi ayarlama

Service Bus, uç noktaları ve kimlik bilgilerini depolamak için bir bağlantı dizesi kullanır. Sabit kodlama yerine, bağlantı dizenizi bir yapılandırma dosyasının içine koyabilirsiniz:

- Azure Cloud Services hizmetini kullanırken bağlantı dizenizi Azure hizmet yapılandırma sistemini (.csdef ve .cscfg dosyaları) kullanarak depolamanız önerilir.
- Azure web sitelerini veya Azure Virtual Machines hizmetini kullanırken bağlantı dizenizi .NET yapılandırma sistemini (örneğin, Web.config dosyası) kullanarak depolamanız önerilir.

Her iki durumda da, bağlantı dizenizi bu makalenin sonraki bölümlerinde açıklanan [CloudConfigurationManager.GetSetting][GetSetting] yöntemini kullanarak alabilirsiniz.

### Bağlantı dizenizi yapılandırma

Hizmet yapılandırma mekanizması, uygulamanızı yeniden dağıtmanıza gerek kalmadan [klasik Azure portalı][] aracılığıyla yapılandırma ayarlarınızı dinamik olarak değiştirmenize olanak sağlar. Örneğin, bir sonraki örnekte gösterildiği gibi hizmet açıklaması (.csdef) dosyanıza bir `Setting` etiketi ekleyin.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```
Ardından bir sonraki örnekte gösterildiği gibi hizmet yapılandırma (.cscfg) dosyasında değerleri belirtin.

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Önceki bölümde açıklandığı gibi klasik Azure portalından alınan Paylaşılan Erişim İmzası (SAS) anahtar adını ve anahtar değerlerini kullanın.

### Web sitelerini veya Azure Virtual Machines hizmetini kullanırken bağlantı dizenizi yapılandırma

Web sitelerini veya Virtual Machines hizmetini kullanırken, .NET yapılandırma sistemini kullanmanız önerilir (örneğin, **Web.config**). `<appSettings>` ögesini kullanarak bağlantı dizenizi depolarsınız.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Önceki bölümde açıklandığı gibi klasik Azure portalından aldığınız SAS adını ve anahtar değerlerini kullanın.

## Kuyruk oluşturma

[NamespaceManager][] sınıfını kullanarak Service Bus kuyruklarına yönelik yönetim işlemlerini gerçekleştirebilirsiniz. Bu sınıfın sağladığı yöntemlerle kuyruk oluşturabilir, kuyrukları numaralandırabilir ve silebilirsiniz.

Bu örnek, Service Bus hizmeti ad alanı taban adresini ve yönetme izinleriyle birlikte uygun SAS kimlik bilgilerinin bulunduğu bir bağlantı dizesini içeren Azure [CloudConfigurationManager][] sınıfını kullanan [NamespaceManager][] nesnesi oluşturur. Bu bağlantı dizesi, aşağıdaki örnekte gösterilen şekildedir.

````
Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedSecretValue=yourKey
````

Önceki bölümde verilen yapılandırma ayarlarıyla aşağıdaki örneği kullanın.

```
// Create the queue if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.QueueExists("TestQueue"))
{
    namespaceManager.CreateQueue("TestQueue");
}
```

Kuyruğun özelliklerini ayarlamanıza olanak sağlayan (örneğin, kuyruğa gönderilen iletilere uygulanacak varsayılan yaşam süresi (TTL) değerini ayarlama) [CreateQueue](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createqueue.aspx) yönteminde aşırı yükleme yapılmıştır. Bu ayarlar [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) sınıfı kullanılarak uygulanır. Aşağıdaki örnek, maksimum 5 GB boyuta sahip olan ve 1 dakikalık varsayılan ileti TTL'si içeren, `TestQueue` olarak adlandırılan bir kuyruğun nasıl oluşturulacağını gösterir.

```
// Configure queue settings.
QueueDescription qd = new QueueDescription("TestQueue");
qd.MaxSizeInMegabytes = 5120;
qd.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new queue with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.QueueExists("TestQueue"))
{
    namespaceManager.CreateQueue(qd);
}
```

> [AZURE.NOTE] Kuyruk için belirtilen adın zaten bir hizmet ad alanında kullanılıp kullanılmadığını kontrol etmek için [NamespaceManager][] nesnelerinde [QueueExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.queueexists.aspx) yöntemini kullanabilirsiniz.

## Kuyruğa ileti gönderme

Uygulamanız, Service Bus kuyruğuna bir ileti göndermek için bağlantı dizesi kullanarak bir [QueueClient][] nesnesi oluşturur.

Aşağıdaki kod, [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.createfromconnectionstring.aspx) API çağrısı kullanarak az önce oluşturduğunuz `TestQueue` kuyruğu için [QueueClient][] nesnesinin nasıl oluşturulacağını gösterir.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

Client.Send(new BrokeredMessage());
```

Service Bus kuyruklarına gönderilen ve bu kuyruklardan alınan iletiler [BrokeredMessage][] sınıfının örnekleridir. [BrokeredMessage][] nesneleri, bir standart özellikler kümesi ([Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) ve [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx) gibi), uygulamaya özgü özel özellikleri tutmak için kullanılan bir sözlük ve rastgele uygulama verileri gövdesi içerir. Uygulama herhangi bir seri haline getirebilir nesneyi [BrokeredMessage][] nesnesinin oluşturucusuna geçirerek ileti gövdesini ayarlayabilir ve ardından nesneyi seri haline getirmek için uygun **DataContractSerializer**'ı kullanılır. Alternatif olarak **System.IO.Stream** nesnesi sağlayabilirsiniz.

Aşağıdaki örnek, bir önceki kod örneğinde elde ettiğiniz `TestQueue` [QueueClient][] nesnesine nasıl beş test iletisi göndereceğinizi gösterir.

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set some addtional custom app-specific properties.
  message.Properties["TestProperty"] = "TestValue";
  message.Properties["Message number"] = i;

  // Send message to the queue.
  Client.Send(message);
}
```

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Bölümlendirme etkinse üst sınır daha yüksektir. Daha fazla bilgi için bkz. [Bölümlenmiş mesajlaşma varlıkları](service-bus-partitioning.md).

## Kuyruktan ileti alma

Kuyruktan iletileri almak için tavsiye edilen yöntem [QueueClient][] nesnesi kullanmaktır. [QueueClient][] nesneleri iki farklı modda çalışabilir: [ReceiveAndDelete ve PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

**ReceiveAndDelete** modunu kullanırken alma işlemi tek aşamalıdır. Service Bus kuyruktaki bir iletiye yönelik okuma isteği aldığında, iletiyi kullanılmış olarak işaretler ve uygulamaya döndürür. **ReceiveAndDelete**, en basit modeldir ve bir uygulamanın hata oluştuğunda bir iletiyi işlememeye izin verebileceği senaryolarda en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi kullanılmış olarak işaretleyeceğinden, uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında çökmenin öncesinde kullanılan iletiyi atlamış olur.

**PeekLock** modunda (varsayılan mod), atlanan iletilere izin veremeyen uygulamaları desteklemenin mümkün olması için alma işlemi iki aşamalıdır. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya iletiyi daha sonra işlemek üzere güvenli şekilde depoladıktan sonra) alınan iletide [Complete][] yöntemini çağırarak alma işleminin ikinci aşamasını tamamlar. Service Bus [Complete][] çağrısını gördüğünde iletiyi kullanılmış olarak işaretler ve kuyruktan kaldırır.

Aşağıdaki örnekte, varsayılan **PeekLock** modu kullanılarak iletilerin nasıl alınıp işlenebileceği gösterilir. Farklı bir [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) değeri belirtmek için [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.createfromconnectionstring.aspx) öğesinin başka bir aşırı yükünü kullanabilirsiniz. Bu örnekte, iletilerin `TestQueue` konumuna ulaştığında işlenmesi için [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.onmessage.aspx) geri çağrısı kullanılır.

```
string connectionString =
  CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
QueueClient Client =
  QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

// Callback to handle received messages.
Client.OnMessage((message) =>
{
    try
    {
        // Process message from queue.
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Test Property: " +
        message.Properties["TestProperty"]);

        // Remove message from queue.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in queue.
        message.Abandon();
    }
}, options);
```

Bu örnek, bir [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) nesnesini kullanarak [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.onmessage.aspx) geri çağrısını yapılandırır. Alınan ileti üzerinde [Complete][] çağrısının ne zaman yapılacağına dair kontrol sağlamanız için [AutoComplete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) **yanlış** olarak ayarlanmıştır. Çağrı süresi bitmeden önce istemcinin ileti için en fazla bir dakika beklemesini ve iletileri kontrol etmek için istemcinin yeni bir çağrı yapmasını sağlamak için [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) 1 dakika olarak ayarlanır. Bu özellik değeri sayesinde, istemci tarafından gerçekleştirilen, ileti almayan ücretlendirilebilir çağrı sayısı azaltılır.

## Uygulama çökmelerini ve okunmayan iletileri giderme

Service Bus, uygulamanızda gerçekleşen hataları ya da ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bazı nedenlerden dolayı alıcı uygulamanın iletiyi işleyememesi durumunda, alınan iletide [Abandon](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) yöntemi ([Complete][] yöntemi yerine) çağrılabilir. Bu işlem Service Bus hizmetinin kuyruktaki iletinin kilidini açmasına ve iletiyi aynı veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale getirmesine neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı vardır. Uygulama, kilitleme zaman aşımı dolmadan önce iletiyi işleyemezse (örneğin, uygulama çökerse), Service Bus otomatik olarak iletinin kilidini açar ve tekrar alınabilmesini sağlar.

Uygulamanın iletiyi işleyip [Complete][] isteği bildirilmeden önce çökmesi durumunda ise uygulama yeniden başlatıldığında ileti uygulamaya tekrar teslim edilir. Bu durum **En Az Bir Kez İşleme** olarak adlandırılır. Her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu işlem genellikle iletinin teslimat denemelerinde korunan [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) özelliği kullanılarak gerçekleştirilir.

## Sonraki adımlar

Artık Service Bus kuyruklarına ilişkin temel bilgileri öğrendiğinize göre, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

-   [Kuyruklar, konu başlıkları ve abonelikler][] bölümünde bulunan Service Bus mesajlaşma varlıkları hakkındaki bilgileri okuyun.
-   [Service Bus aracılı mesajlaşma .NET öğreticisi][] ile bir Service Bus kuyruğundan ileti alıp gönderen, çalışan bir uygulama derleyin.
-   [Azure örneklerinden][] Service Bus örneklerini indirin veya [Service Bus örneklerine genel bakış][] bölümüne bakın.

  [klasik Azure portalı]: http://manage.windowsazure.com
  [7]: ./media/service-bus-dotnet-how-to-use-queues/getting-started-multi-tier-13.png
  [Kuyruklar, konu başlıkları ve abonelikler]: service-bus-queues-topics-subscriptions.md
  [Service Bus aracılı mesajlaşma .NET öğreticisi]: service-bus-brokered-tutorial-dotnet.md
  [Azure örneklerinden]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [Service Bus örneklerine genel bakış]: service-bus-samples.md
  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [Complete]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx


<!----HONumber=Jun16_HO2-->


