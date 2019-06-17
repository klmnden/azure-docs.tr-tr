---
title: Gönder ve Java - Azure Event Hubs kullanarak olay alma | Microsoft Docs
description: Bu makalede, olayları gönderen Azure Event Hubs için Java uygulaması oluşturmayla ilgili bir kılavuz sağlar.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 04/15/2019
ms.author: shvija
ms.openlocfilehash: 0487cac6a0cf7d37befdf0d7cfab33ad6a62cf7f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60822918"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs-using-java"></a>Olayları göndermek veya Java kullanarak Azure Event Hubs'tan gelen olayları alma

Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğretici, olayları göndermek veya bir olay hub'ından olay alma için Java uygulamalarını oluşturma işlemi gösterilmektedir. 

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend)’dan örnek olarak indirebilir, `EventHubConnectionString` ve `EventHubName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- Java geliştirme ortamı. Bu öğreticide [Eclipse](https://www.eclipse.org/).
- **Bir Event Hubs ad alanı ve bir olay hub'ı oluşturma**. İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için verilen yordamı izleyin [bu makalede](event-hubs-create.md). Ardından, makaledeki yönergeleri izleyerek olay hub'ı erişim anahtarının değerini alın: [Bağlantı dizesini alma](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bu öğreticinin ilerleyen bölümlerinde yazdığınız kod erişim anahtarını kullanın. Varsayılan anahtar adı şöyledir: **RootManageSharedAccessKey**.

## <a name="send-events"></a>Olayları gönderme 
Bu bölümde, bir olay hub'ı olayları göndermek için bir Java uygulamasının nasıl oluşturulacağını gösterir. 

### <a name="add-reference-to-azure-event-hubs-library"></a>Azure Event Hubs kitaplığına bir başvuru ekleyin

Event Hubs için Java istemci kitaplığı içindeki Maven projelerinde kullanılabilir [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Bu kitaplık içinde Maven proje dosyanızda aşağıdaki bağımlılık bildirimi kullanılarak başvurabilirsiniz:

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>2.2.0</version>
</dependency>
```

Derleme ortamlarının farklı türleri için en son JAR dosyalarının açıkça edinebilirsiniz [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Basit olay yayımcısı, içeri aktarma *com.microsoft.azure.eventhubs* Event Hubs istemci sınıfları için paket ve *com.microsoft.azure.servicebus* için yardımcı sınıflar gibi paket Azure Service Bus Mesajlaşma istemcisi ile paylaşılan sık karşılaşılan özel durumlar. 

### <a name="write-code-to-send-messages-to-the-event-hub"></a>Olay hub'ına ileti göndermek için kod yazma

Aşağıdaki örnek için önce en sevdiğiniz Java geliştirme ortamında bir konsol/kabuk uygulaması için yeni bir Maven projesi oluşturun. Adlı bir sınıf ekleyin `SimpleSend`ve bu sınıfa aşağıdaki kodu ekleyin:

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;

public class SimpleSend {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
            
            
    }
 }
```

### <a name="construct-connection-string"></a>Bağlantı dizesi oluşturun

ConnectionStringBuilder sınıfı Event Hubs istemci örneğine geçirmek için bir bağlantı dizesi değerini oluşturmak için kullanın. Yer tutucuları, ad alanı ve olay hub'ı oluştururken aldığınız değerlerle değiştirin:

```java
        final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
                .setNamespaceName("speventhubns") 
                .setEventHubName("speventhub")
                .setSasKeyName("RootManageSharedAccessKey")
                .setSasKey("2+WMsyyy1XmUtEnRsfOmTTyGasfJgsVjGAOIN20J1Y8=");
```

### <a name="write-code-to-send-events"></a>Olayları göndermek için kod yazma

Bir dizeyi UTF-8 kodlamasını bayt içine dönüştürerek tekil bir olay oluşturun. Ardından, bağlantı dizesinden yeni bir olay hub'ları istemci örneği oluşturun ve ileti gönder:   

```java 
        final Gson gson = new GsonBuilder().create();

        // The Executor handles all asynchronous tasks and this is passed to the EventHubClient instance.
        // This enables the user to segregate their thread pool based on the work load.
        // This pool can then be shared across multiple EventHubClient instances.
        // The following sample uses a single thread executor, as there is only one EventHubClient instance,
        // handling different flavors of ingestion to Event Hubs here.
        final ScheduledExecutorService executorService = Executors.newScheduledThreadPool(4);

        // Each EventHubClient instance spins up a new TCP/SSL connection, which is expensive.
        // It is always a best practice to reuse these instances. The following sample shows this.
        final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);


        try {
            for (int i = 0; i < 10; i++) {

                String payload = "Message " + Integer.toString(i);
                byte[] payloadBytes = gson.toJson(payload).getBytes(Charset.defaultCharset());
                EventData sendEvent = EventData.create(payloadBytes);

                // Send - not tied to any partition
                // Event Hubs service will round-robin the events across all Event Hubs partitions.
                // This is the recommended & most reliable way to send to Event Hubs.
                ehClient.sendSync(sendEvent);
            }

            System.out.println(Instant.now() + ": Send Complete...");
            System.out.println("Press Enter to stop.");
            System.in.read();
        } finally {
            ehClient.closeSync();
            executorService.shutdown();
        }

``` 

Derleme programı çalıştırın ve hiçbir hata olmadığından emin olun.

Tebrikler! Bir olay hub'ına ileti gönderdiniz.

### <a name="appendix-how-messages-are-routed-to-eventhub-partitions"></a>Ek: EventHub bölümleri iletileri nasıl yönlendirileceğini

İleti tüketiciler tarafından alınır önce bunlar bölümlere ilk yayımcılar tarafından yayımlanan gerekir. Olay hub'ına zaman uyumlu olarak com.microsoft.azure.eventhubs.EventHubClient nesnede sendSync() yöntemi kullanarak ileti yayımlandığında, ileti belirli bir bölüme gönderilebilecek veya hepsini bir kez deneme biçimde için kullanılabilir tüm bölümleri dağıtılmış Bölüm anahtarı veya belirtilen üzerinde bağlı olarak.

Bölüm anahtarını temsil eden bir dize belirtildiğinde, anahtarı için bir olay göndermek için hangi bölümünün belirlemek için karma.

Bölüm anahtarı olarak ayarlanmadığında, iletileri ardından gidiş-robined kullanılabilen tüm bölümler için olacaktır

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

// close the client at the end of your program
eventHubClient.closeSync();

```

## <a name="receive-events"></a>Olayları alma
Bu öğreticideki kod dayanır [EventProcessorSample kodu github'da](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/EventProcessorSample), hangi tam görmek için inceleyebilirsiniz çalışan uygulama.

### <a name="receive-messages-with-eventprocessorhost-in-java"></a>Java’da EventProcessorHost bulunan iletiler alma

**EventProcessorHost** kalıcı denetim noktalarını yöneterek Event Hubs'a ait alma olaylarını basitleştiren bir Java sınıfıdır ve paralel alımları bu Event Hubs'a ait. Eventprocessorhost'u kullanarak, olayları birden çok alıcı arasında farklı düğümlerde barındırıldığında bile bölebilirsiniz. Bu örnek, tek alıcı için EventProcessorHost’un nasıl kullanıldığını göstermektedir.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Eventprocessorhost'u kullanmak için [Azure depolama hesabınız] olan [Azure depolama hesabı]:

1. Oturum [Azure portalında](https://portal.azure.com), tıklatıp **+ kaynak Oluştur** ekranın sol taraftaki.
2. **Depolama** ve ardından **Depolama hesabı**’na tıklayın. İçinde **depolama hesabı oluşturma** penceresinde, depolama hesabı için bir ad yazın. Alanları geri kalanını tamamlamak için istediğiniz bölgeyi seçin ve ardından **Oluştur**.
   
    ![Depolama hesabı oluştur](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. Yeni oluşturulan depolama hesabına tıklayın ve ardından **erişim anahtarlarını**:
   
    ![Erişim anahtarı alma](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    Key1 değeri geçici bir konuma kopyalayın. Daha sonra bu öğreticide kullanacaksınız.

### <a name="create-a-java-project-using-the-eventprocessor-host"></a>EventProcessor Ana Bilgisayarını kullanarak Java projesi oluşturma

Event Hubs için Java istemci kitaplığı içindeki Maven projelerinde kullanılabilir [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22)ve Maven proje dosyanızda aşağıdaki bağımlılık bildirimi kullanılarak başvurulabilir: 

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>2.2.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>2.4.0</version>
</dependency>
```

Derleme ortamlarının farklı türleri için açıkça [Maven Central Repository] son JAR dosyalarının edinebilirsiniz [https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22 ].  

1. Aşağıdaki örnek için önce en sevdiğiniz Java geliştirme ortamında bir konsol/kabuk uygulaması için yeni bir Maven projesi oluşturun. Sınıf `ErrorNotificationHandler`.     
   
    ```java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   
    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```
2. `EventProcessorSample` adlı yeni bir sınıf oluşturmak için aşağıdaki kodu kullanın. Yer tutucuları olay hub'ı ve depolama hesabı oluştururken kullandığınız değerlerle değiştirin:
   
   ```java
   package com.microsoft.azure.eventhubs.samples.eventprocessorsample;

   import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
   import com.microsoft.azure.eventhubs.EventData;
   import com.microsoft.azure.eventprocessorhost.CloseReason;
   import com.microsoft.azure.eventprocessorhost.EventProcessorHost;
   import com.microsoft.azure.eventprocessorhost.EventProcessorOptions;
   import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   import com.microsoft.azure.eventprocessorhost.IEventProcessor;
   import com.microsoft.azure.eventprocessorhost.PartitionContext;

   import java.util.concurrent.ExecutionException;
   import java.util.function.Consumer;

   public class EventProcessorSample
   {
       public static void main(String args[]) throws InterruptedException, ExecutionException
       {
           String consumerGroupName = "$Default";
           String namespaceName = "----NamespaceName----";
           String eventHubName = "----EventHubName----";
           String sasKeyName = "----SharedAccessSignatureKeyName----";
           String sasKey = "----SharedAccessSignatureKey----";
           String storageConnectionString = "----AzureStorageConnectionString----";
           String storageContainerName = "----StorageContainerName----";
           String hostNamePrefix = "----HostNamePrefix----";
        
           ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder()
                .setNamespaceName(namespaceName)
                .setEventHubName(eventHubName)
                .setSasKeyName(sasKeyName)
                .setSasKey(sasKey);
        
           EventProcessorHost host = new EventProcessorHost(
                EventProcessorHost.createHostName(hostNamePrefix),
                eventHubName,
                consumerGroupName,
                eventHubConnectionString.toString(),
                storageConnectionString,
                storageContainerName);
        
           System.out.println("Registering host named " + host.getHostName());
           EventProcessorOptions options = new EventProcessorOptions();
           options.setExceptionNotification(new ErrorNotificationHandler());

           host.registerEventProcessor(EventProcessor.class, options)
           .whenComplete((unused, e) ->
           {
               if (e != null)
               {
                   System.out.println("Failure while registering: " + e.toString());
                   if (e.getCause() != null)
                   {
                       System.out.println("Inner exception: " + e.getCause().toString());
                   }
               }
           })
           .thenAccept((unused) ->
           {
               System.out.println("Press enter to stop.");
               try 
               {
                   System.in.read();
               }
               catch (Exception e)
               {
                   System.out.println("Keyboard read failed: " + e.toString());
               }
           })
           .thenCompose((unused) ->
           {
               return host.unregisterEventProcessor();
           })
           .exceptionally((e) ->
           {
               System.out.println("Failure while unregistering: " + e.toString());
               if (e.getCause() != null)
               {
                   System.out.println("Inner exception: " + e.getCause().toString());
               }
               return null;
           })
           .get(); // Wait for everything to finish before exiting main!
        
           System.out.println("End of sample");
       }
    ```
3. Adlı bir sınıf oluşturma `EventProcessor`, aşağıdaki kodu kullanarak:
   
    ```java
    public static class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        // OnOpen is called when a new event processor instance is created by the host. 
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        // OnClose is called when an event processor instance is being shut down. 
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        // onError is called when an error occurs in EventProcessorHost code that is tied to this partition, such as a receiver failure.
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        // onEvents is called when events are received on this partition of the Event Hub. 
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> events) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got event batch");
            int eventCount = 0;
            for (EventData data : events)
            {
                try
                {
                    System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                            data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBytes(), "UTF8"));
                    eventCount++;
                    
                    // Checkpointing persists the current position in the event stream for this partition and means that the next
                    // time any host opens an event processor on this event hub+consumer group+partition combination, it will start
                    // receiving at the event after this one. 
                    this.checkpointBatchingCount++;
                    if ((checkpointBatchingCount % 5) == 0)
                    {
                        System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                            data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                        // Checkpoints are created asynchronously. It is important to wait for the result of checkpointing
                        // before exiting onEvents or before creating the next checkpoint, to detect errors and to ensure proper ordering.
                        context.checkpoint(data).get();
                    }
                }
                catch (Exception e)
                {
                    System.out.println("Processing failed for an event: " + e.toString());
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + eventCount + " for host " + context.getOwner());
        }
    }
    ```

Bu öğretici, EventProcessorHost’un tek bir örneğini kullanır. Verimliliği artırmak için birden çok örneğini EventProcessorHost, tercihen ayrı makinelerde çalıştırmanızı öneririz.  Bu işlem de yedeklilik sağlar. Böyle durumlarda, alınan olayların yük dengesi için çeşitli örnekler otomatik olarak birbirleriyle koordine olurlar. Birden çok alıcının her birinin *tüm* olayları işlemesini istiyorsanız **ConsumerGroup** kavramını kullanmalısınız. Olaylar farklı makinelerden alındığında, dağıtıldıkları makineleri (veya rolleri) temel alan EventProcessorHost örnekleri için ad belirtmek yararlı olabilir.

### <a name="publishing-messages-to-eventhub"></a>EventHub yayımlama iletileri

İleti tüketiciler tarafından alınır önce bunlar bölümlere ilk yayımcılar tarafından yayımlanan gerekir. Zaman uyumlu olarak com.microsoft.azure.eventhubs.EventHubClient nesnede sendSync() yöntemi kullanarak olay hub'ına ileti yayımlandığında, ileti için belirli bir bölüme gönderilen ya da tüm kullanılabilir bölümlere dağıtılan'nı hatalarının ayıklanabileceğini belirtmekte yarar bağlı olarak hepsini bir kez deneme şekilde bölüm anahtarını veya belirtilir.

Bölüm anahtarını temsil eden bir dize belirtildiğinde anahtar için bir olay göndermek için hangi bölümünün belirlemek üzere karma haline getirilir.

Bölüm anahtarı olarak ayarlanmadığında, iletileri ardından gidiş-robined için kullanılabilir tüm bölümleri

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

```

### <a name="implementing-a-custom-checkpointmanager-for-eventprocessorhost-eph"></a>Özel CheckpointManager EventProcessorHost (EPH) için uygulama

API, özel bir denetim noktası yöneticinize varsayılan uygulama, kullanım örneği ile uyumlu olmayan senaryolar uygulamak için bir mekanizma sağlar.

Varsayılan Denetim Yöneticisi'ni blob depolama kullanır ancak kendi uygulamasıyla EPH tarafından kullanılan kontrol noktası Yöneticisi geçersiz kılarsanız, kontrol noktası Yöneticisi uygulamanızı yedeklemek istediğiniz her depolama kullanabilirsiniz.

Arabirimi com.microsoft.azure.eventprocessorhost.ICheckpointManager uygulayan bir sınıf oluşturma

Uygulamanıza özel kontrol noktası Yöneticisi'ni (com.microsoft.azure.eventprocessorhost.ICheckpointManager) kullanın

Uygulamanız içinde varsayılan denetim noktası oluşturma mekanizması geçersiz kılmak ve kendi veri deposu (SQL Server, CosmosDB ve Azure önbelleği için Redis gibi) temel kendi kontrol noktaları uygulayın. Kontrol noktası Yöneticisi uygulamanızı yedeklemek için kullanılan depolama tüketici grubu için olayları işlemekte olduğu tüm EPH örnekleri erişilebilir olduğunu öneririz.

Ortamınızda herhangi bir veri deposu olarak kullanabilirsiniz.

Com.microsoft.azure.eventprocessorhost.EventProcessorHost sınıfı, EventProcessorHost için kontrol noktası Yöneticisi geçersiz kılmanıza da olanak sağlayan iki Oluşturucu sağlar.


## <a name="next-steps"></a>Sonraki adımlar
Bu makaleleri okuyun: 

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Özellikler ve Azure Event Hubs terminolojisinde](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)

