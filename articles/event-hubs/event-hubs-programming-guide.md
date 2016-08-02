<properties 
    pageTitle="Azure Event Hubs için programlama kılavuzu | Microsoft Azure"
    description="Azure .NET SDK’sını kullanarak Azure Event Hubs ile programlamayı açıklar."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="04/15/2016"
    ms.author="sethm" />

# Event Hubs programlama kılavuzu

Bu konu Azure .NET SDK’sı kullanılarak Azure Event Hubs ile programlamayı açıklamaktadır. Burada Event Hubs’ın önceden bilindiği varsayılır. Event Hubs’a kavramsal genel bakış için bkz. [Event Hubs’a genel bakış](event-hubs-overview.md).

## Olay yayımcıları

Bir Event Hub'ına olayların gönderilmesi HTTP POST kullanılarak veya bir AMQP 1.0 bağlantısı üzerinden gerçekleştirilir. Hangisinin kullanılacağına ilişkin seçim, ele alınan belirli senaryoya bağlıdır. AMQP 1.0 bağlantıları Service Bus içinde aracılı bağlantılar olarak ölçülür ve sıklıkla daha yüksek ileti hacimlerine ve düşük gecikme gereksinimlerine sahip senaryolar kalıcı bir mesajlaşma kanalı sağladığından bu senaryolarda daha uygundur.

Event Hubs [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) sınıfı kullanılarak oluşturulur ve yönetilir. .NET ile yönetilen API’ler kullanılırken Event Hubs’a veri yayımlamaya yönelik birincil yapılar [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) ve [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) sınıflarıdır. [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx), olayların Event Hub'ına gönderildiği AMQP iletişim kanalını sağlar. [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) sınıfı bir olayı temsil eder ve bir Event Hub'ına iletileri yayımlamak için kullanılır. Bu sınıf, olayla ilgili gövde bilgileri, bazı meta verileri ve üst bilgileri içerir. Diğer özellikler bir Event Hub'ından geçtikçe [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) nesnesine eklenir.

## Başlarken

Event Hubs’ı destekleyen .NET sınıfları Microsoft.ServiceBus.dll bütünleştirilmiş kodunda sağlanır. Service Bus API'sini almanın ve uygulamanızı tüm Service Bus bağımlılıklarıyla yapılandırmanın en kolay yolu [Service Bus NuGet paketi](https://www.nuget.org/packages/WindowsAzure.ServiceBus)’nin indirilmesidir. Alternatif olarak, Visual Studio’daki [Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)'nu kullanabilirsiniz. Bunu yapmak için [Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) penceresinde aşağıdaki komutu yürütün:

```
Install-Package WindowsAzure.ServiceBus
```

## Event Hub'ı oluşturma

Event Hubs oluşturmak için [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) sınıfını kullanabilirsiniz. Örneğin:

```
var manager = new Microsoft.ServiceBus.NamespaceManager("mynamespace.servicebus.windows.net");
var description = manager.CreateEventHub("MyEventHub");
```

Çoğu durumda hizmetin yeniden başlatılması halinde özel durumların oluşmasını engellemek için [CreateEventHubIfNotExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createeventhubifnotexists.aspx) yöntemlerini kullanmanız önerilir. Örneğin:

```
var description = manager.CreateEventHubIfNotExists("MyEventHub");
```

[CreateEventHubIfNotExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createeventhubifnotexists.aspx) dahil tüm Event Hubs’ı oluşturma işlemleri söz konusu ad alanında **Manage** izinlerini gerektirir. Yayımcı veya tüketici uygulamanızın izinlerini kısıtlamak istiyorsanız, sınırlı izinlere sahip kimlik bilgileri kullanırken üretim kodunda bu oluşturma işlemi çağrılarından kaçınabilirsiniz.

[EventHubDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubdescription.aspx) sınıfı yetkilendirme kuralları, ileti elde tutma aralığı, bölüm kimlikleri, durum ve yol gibi bir Event Hub'ı ile ilgili ayrıntılar içerir. Bu sınıfı kullanarak bir Event Hub'ındaki meta verileri güncelleştirebilirsiniz.

## Event Hubs istemcisi oluşturma

Event Hubs ile etkileşim kurmaya yönelik birincil sınıf [Microsoft.ServiceBus.Messaging.EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) sınıfıdır. Bu sınıf hem gönderen hem de alıcı özellikleri sağlar. Aşağıdaki örnekte gösterildiği gibi [Create](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx) yöntemini kullanarak bu sınıfın bir örneğini oluşturabilirsiniz.

```
var client = EventHubClient.Create(description.Path);
```

Bu yöntem `appSettings` bölümündeki App.config dosyasında bulunan Service Bus bağlantı bilgilerini kullanır. Service Bus bağlantı bilgilerini depolamak için kullanılan `appSettings` XML örneği için [Microsoft.ServiceBus.Messaging.EventHubClient.Create(System.String)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx) yönteminin belgelerine bakın.

Başka bir seçenek ise istemcinin bir bağlantı dizesinden oluşturulmasıdır. Dizeyi çalışanın yapılandırma özelliklerine depolayabileceğiniz için, bu seçenek Azure çalışan rolleri kullanılarak iyi çalışır. Örneğin:

```
EventHubClient.CreateFromConnectionString("your_connection_string");
```

Bağlantı dizesi önceki yöntemler için App.config dosyasında göründüğü biçimde olacaktır:

```
Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=Manage;SharedAccessKey=[key]
```

Son olarak, aşağıdaki örnekte gösterildiği gibi bir [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx) örneğinden [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) nesnesinin oluşturulması da mümkündür.

```
var factory = MessagingFactory.CreateFromConnectionString("your_connection_string");
var client = factory.CreateEventHubClient("MyEventHub");
```

Bir mesajlaşma altyapısı örneğinden oluşturulan ek [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) nesneleri temel alınan aynı TCP bağlantısını yeniden kullanacaktır. Bu nedenle, bu nesneler işlemede istemci tarafı sınırlamasına sahiptir. [Create](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx) yöntemi tek bir mesajlaşma altyapısını yeniden kullanır. Tek bir gönderenden çok yüksek işleme gerekiyorsa birden fazla ileti altyapısı ve her mesajlaşma altyapısından bir [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) nesnesi oluşturabilirsiniz.

## Bir Event Hub'ına olay gönderme

Bir [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) örneği oluşturup [Send](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.send.aspx) yöntemi ile göndererek olayları bir Event Hub'ına gönderebilirsiniz. Bu yöntem tek bir [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) örnek parametresini alır ve bir Event Hub'ına zaman uyumlu olarak gönderir.

## Olayı seri hale getirme

[EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) sınıfında nesne ve seri hale getirici, bayt dizisi veya akış gibi çeşitli parametreler kullanan [dört aşırı yüklü oluşturucu](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.eventdata.aspx) bulunur. [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) sınıfının bir örneğini oluşturup gövde akışını daha sonra ayarlamak da mümkündür. JSON’u [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) ile kullanırken JSON ile kodlanmış bir dize için bayt dizisini almak üzere **Encoding.UTF8.GetBytes()** kullanabilirsiniz.

## Bölüm anahtarı

[EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) sınıfında gönderenin bir bölüm ataması oluşturmak üzere karma hale getirilmiş değer belirtmesini sağlayan bir [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx) özelliği bulunur. Bir bölüm anahtarının kullanılması aynı anahtara sahip tüm olayların Event Hub'ı içinde aynı bölüme gönderilmesini sağlar. Yaygın bölüm anahtarları kullanıcı oturum kimlikleri ve benzersiz gönderen kimlikleridir. [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx) özelliği isteğe bağlıdır ve [Microsoft.ServiceBus.Messaging.EventHubClient.Send(Microsoft.ServiceBus.Messaging.EventData)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) veya [Microsoft.ServiceBus.Messaging.EventHubClient.SendAsync(Microsoft.ServiceBus.Messaging.EventData)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) yöntemleri kullanılırken sağlanabilir. [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx) için bir değer belirtmezseniz gönderilen olaylar genel bir model kullanılarak bölümlere dağıtılır.

## Toplu olay gönderme işlemleri

Olayların toplu olarak gönderilmesi üretilen işi çarpıcı biçimde artırabilir. [SendBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.sendbatch.aspx) yöntemi [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) türünde bir **IEnumerable** parametresini alır ve toplu işin tamamını atomik bir işlem olarak Event Hub'ına gönderir.

```
public void SendBatch(IEnumerable<EventData> eventDataList);
```

Tek bir toplu iş olayın 256 KB’lik sınırını aşmamalıdır. Ayrıca, toplu işteki her bir ileti aynı yayımcı kimliğini kullanır. Toplu işin en büyük olay boyutu aşmamasını sağlamak gönderenin sorumluluğundadır. Aşması durumunda bir istemci **Gönderme** hatası oluşturulur.

## Zaman uyumsuz olarak gönderme ve ölçekli gönderme

Olayları bir Event Hub'ına zaman uyumsuz olarak da gönderebilirsiniz. Zaman uyumsuz gönderme bir istemcinin olayları gönderebildiği hızı artırabilir. Bir [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) nesnesi döndüren zaman uyumsuz sürümlerde hem [Send](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.send.aspx) hem de [SendBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.sendbatch.aspx) yöntemleri kullanılabilir. Bu teknik üretilen işi artırabilse de, istemcinin Event Hubs hizmeti tarafından kısıtlandığında bile olay göndermeye devam etmesine neden olabilir ve düzgün uygulanmaması durumunda istemcinin hata veya kayıp iletilerle karşılaşmasına yol açabilir. Ayrıca, istemci yeniden deneme seçeneklerini denetlemek için istemci üzerindeki [RetryPolicy](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.cliententity.retrypolicy.aspx) özelliğini kullanabilirsiniz.

## Bölüm göndereni oluşturma

Olayları bir Event Hub'a göndermenin en yaygın yolu bir bölüm anahtarının kullanılmasıdır, ancak bazı durumlarda olayları doğrudan belirli bir bölüme göndermek isteyebilirsiniz. Örneğin:

```
var partitionedSender = client.CreatePartitionedSender(description.PartitionIds[0]);
```

[CreatePartitionedSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.createpartitionedsender.aspx), olayları belirli bir Event Hub’ı bölümünde yayımlamak için kullanabileceğiniz bir [EventHubSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubsender.aspx) nesnesi döndürür.

## Olay tüketicileri

Event Hubs olay tüketimi için iki adet birincil modele sahiptir: doğrudan alıcılar ve [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) gibi üst düzey soyutlamalar. Doğrudan alıcılar bir tüketici grubundaki bölümlere erişimin eşgüdümünden kendileri sorumludur.

### Doğrudan tüketici

Bir tüketici grubundaki bölümden okuma yapmanın en doğrudan yolu [EventHubReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubreceiver.aspx) sınıfının kullanılmasıdır. Bu sınıfın bir örneğini oluşturmak için [EventHubConsumerGroup](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubconsumergroup.aspx) sınıfının bir örneğini kullanmanız gerekir. Aşağıdaki örnekte tüketici grubu için alıcı oluşturulurken bölüm kimliği belirtilmelidir.

```
EventHubConsumerGroup group = client.GetDefaultConsumerGroup();
var receiver = group.CreateReceiver(client.GetRuntimeInformation().PartitionIds[0]);
```

[CreateReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubconsumergroup.createreceiver.aspx) yöntemi oluşturulmakta olan okuyucu üzerinde denetimi kolaylaştıran birkaç aşırı yüke sahiptir. Bu yöntemler bir uzaklığı dize veya zaman damgası olarak belirtmeyi ve belirtilen bu uzaklığın döndürülen akışa dahil edileceğini ya da ondan sonra başlayacağını belirtebilmeyi içerir. Alıcıyı oluşturduktan sonra döndürülen nesne üzerinde olayları almaya başlayabilirsiniz. [Receive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubreceiver.receive.aspx) yönteminde toplu işlem boyutu ve bekleme süresi gibi alma işlemi parametrelerini denetleyen dört aşırı yük bulunur. Bir tüketicinin verimliliğini artırmak için bu yöntemlerin zaman uyumsuz sürümlerini kullanabilirsiniz. Örneğin:

```
bool receive = true;
string myOffset;
while(receive)
{
    var message = receiver.Receive();
    myOffset = message.Offset;
    string body = Encoding.UTF8.GetString(message.GetBytes());
    Console.WriteLine(String.Format("Received message offset: {0} \nbody: {1}", myOffset, body));
}
```

Belirli bir bölüme göre iletiler Event Hub'ına gönderildikleri sırayla alınır. Uzaklık bir bölümdeki bir iletiyi tanımlamak için kullanılan bir dize belirtecidir.

Bir tüketici grubundaki tek bir bölüm aynı anda 5'ten fazla eşzamanlı okuyucunun bağlanmasına izin vermez. Okuyucular bağlandığında veya bağlantıları kesildiğinde hizmetin bağlantı kesilmesini algılamasından önce oturumları birkaç dakika boyunca etkin kalabilir. Bu süre boyunca bir bölüme yeniden bağlanılması başarısız olabilir. Event Hubs için doğrudan alıcı yazmaya ilişkin tam bir örnek için [Service Bus Event Hubs Doğrudan Alıcıları](https://code.msdn.microsoft.com/Event-Hub-Direct-Receivers-13fa95c6) örneğine bakın.

### Olay işlemcisi konağı

[EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) sınıfı Event Hubs verilerini işler. .NET platformu üzerinde olay okuyucuları oluştururken bu uygulamayı kullanmanız gerekir. [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) aynı zamanda denetim noktası oluşturma ve bölüm kiralama yönetimi sağlayan olay işlemcisi uygulamaları için iş parçacığı güvenli, çok işlemli, güvenli bir çalışma zamanı ortamı sağlar.

[EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) sınıfını kullanmak için [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx) uygulayabilirsiniz. Bu arabirim üç yöntem içerir:

- [OpenAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.openasync.aspx)

- [CloseAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.closeasync.aspx)

- [ProcessEventsAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.processeventsasync.aspx)

Olay işlemeyi başlatmak için Event Hub'ınıza uygun parametreleri sağlayarak bir [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) örneği oluşturun. Ardından [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx) uygulamanızı çalışma zamanına kaydetmek için [RegisterEventProcessorAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.registereventprocessorasync.aspx) çağrısı yapın. Bu noktada konak bir "hızlı" algoritma kullanarak Event Hub'ındaki her bölüm üzerinde bir kira elde etmeye çalışır. Bu kiralar belirli bir zaman çerçevesi boyunca sürer ve sonrasında yenilenmelidir. Bu örnekte çalışan örnekleri olan yeni düğümler çevrimiçi oldukça kiralama ayırmaları yapar ve zaman içerisinde yük daha fazla kira elde etmeye çalıştığından düğümler arasında kayar.

![Olay İşlemcisi Konağı](./media/event-hubs-programming-guide/IC759863.png)

Zaman içerisinde bir denge sağlanır. Bu dinamik özellik hem ölçek artırma hem de ölçek azaltma için tüketicilere CPU tabanlı otomatik ölçeklendirmenin uygulanmasını sağlar. Event Hubs doğrudan ileti sayısı kavramına sahip olmadığından ortalama CPU kullanımı genellikle arka uç veya tüketici ölçeğini ölçmeye yönelik en iyi mekanizmadır. Yayımcılar tüketicilerin işleyebileceğinden daha fazla olay yayımlamaya başlarsa, tüketiciler üzerindeki CPU artışı çalışan örnek sayısı üzerinde otomatik ölçeklendirmeye neden olmak için kullanılabilir.

[EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) sınıfı ayrıca bir Azure depolama tabanlı denetim noktası oluşturma mekanizması kullanır. Bu mekanizma uzaklığı bölüm başına temelinde depolar, böylece her tüketici önceki tüketicinin son denetim noktasının ne olduğunu belirleyebilir. Bölümler kiralamalar aracılığıyla düğümler arasında geçiş yaptığında yük kaymasını kolaylaştıran eşitleme mekanizması budur.

## Yayımcı iptali

[EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx)’un gelişmiş çalışma zamanı özelliklerine ek olarak Event Hubs, belirli yayımcıların bir Event Hub'ına olay göndermesini engellemek üzere yayımcı iptalini sağlar. Bu özellikleri özellikle bir yayımcı belirteci tehlike girdiğinde veya bir yazılım güncelleştirmesi yayımcının uygunsuz şekilde davranmasına yol açtığında yararlıdır. Bu durumlarda SAS belirtecinin bir parçası olan yayımcı kimliğinin olayları yayımlaması engellenebilir.

Yayımcı iptali ve yayımcı olarak Event Hubs’a gönderme hakkında daha fazla bilgi için [Service Bus Event Hubs Büyük Ölçekli Güvenli Yayımlama](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab) örneğine bakın.

## Sonraki adımlar

Event Hubs senaryoları hakkında daha fazla bilgi almak için aşağıdaki bağlantıları ziyaret edin:

- [Event Hubs API’sine genel bakış](event-hubs-api-overview.md)
- [Event Hubs’a genel bakış](event-hubs-overview.md)
- [Event Hubs kod örnekleri](http://code.msdn.microsoft.com/site/search?query=event hub&f[0].Value=event hub&f[0].Type=SearchText&ac=5)
- [Olay işleyicisi konağı API başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx)



<!--HONumber=Jun16_HO2-->


