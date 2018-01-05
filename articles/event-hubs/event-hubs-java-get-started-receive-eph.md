---
title: "Java kullanarak Azure Event Hubs'tan gelen olayları alma | Microsoft Docs"
description: "Java kullanarak Event Hubs'dan alma kullanmaya başlama"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 38e3be53-251c-488f-a856-9a500f41b6ca
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 94b56fdd1ad350d861b27644c6bedcc59e1cefb0
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a>Java kullanarak Azure Event Hubs'tan gelen olayları alma


## <a name="introduction"></a>Giriş
Olay hub'ları saniye başına milyonlarca olayı alma, bir uygulamaya etkinleştirme işlemek ve veri bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki analiz bir yüksek düzeyde ölçeklenebilir bir alım sistemine olur. Veriler Event Hubs hizmetinde toplandığında, herhangi bir gerçek zamanlı analitik sağlayıcısı veya depolama kümesi kullanarak bu verileri dönüştürebilir veya depolayabilirsiniz.

Daha fazla bilgi için bkz: [Event Hubs'a genel bakış][Event Hubs overview].

Bu öğretici Java'da yazılmış bir konsol uygulaması kullanarak bir event hub içine olaylarını almak nasıl gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşullar gerekir:

* Java geliştirme ortamı. Bu öğretici için varsayıyoruz [Eclipse](https://www.eclipse.org/).
* Etkin bir Azure hesabı. <br/>Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Ücretsiz Deneme Sürümü</a>.

## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Java’da EventProcessorHost bulunan iletiler alma

**EventProcessorHost** kalıcı denetim noktalarını yöneterek Event Hubs'a ait alma olaylarını basitleştiren bir Java sınıfı ve paralel alımları bu Event Hubs'a ait. EventProcessorHost kullanarak, olayları birden çok alıcı arasında farklı düğümlerde barındırıldığında bile bölebilirsiniz. Bu örnek, tek alıcı için EventProcessorHost’un nasıl kullanıldığını göstermektedir.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
EventProcessorHost kullanmak için olmalıdır bir [Azure depolama hesabı][Azure Storage account]:

1. Oturum [Azure portal][Azure portal], tıklatıp **+ yeni** ekranın sol taraftaki.
2. **Depolama** ve ardından **Depolama hesabı**’na tıklayın. **Depolama hesabı oluştur** dikey penceresinde depolama hesabı için bir ad yazın. Alanları geri kalanını tamamlamak için istediğiniz bölgeyi seçin ve ardından **oluşturma**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. Yeni oluşturulan depolama hesabına ve **Erişim Tuşlarını Yönet**’e tıklayın:
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    Birincil erişim tuşunu daha sonra Bu öğreticide kullanmak için geçici bir konuma kopyalayın.

### <a name="create-a-java-project-using-the-eventprocessor-host"></a>EventProcessor Ana Bilgisayarını kullanarak Java projesi oluşturma
Olay hub'ları için Java istemci kitaplığı Maven projelerden kullanmak için kullanılabilir [Maven merkezi bir depoya][Maven Package]ve aşağıdaki bağımlılığı bildirimi, Maven içinde kullanarak başvurulabilir Proje dosyası:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
```

Derleme ortamları farklı türleri için en son yayımlanan JAR dosyalarını açıkça edinebilirsiniz [Maven merkezi bir depoya] [ Maven Package] veya [GitHub yayın dağıtım noktasında ](https://github.com/Azure/azure-event-hubs/releases).  

1. Aşağıdaki örnek için önce en sevdiğiniz Java geliştirme ortamında bir konsol/kabuk uygulaması için yeni bir Maven projesi oluşturun. Sınıf adı verilen `ErrorNotificationHandler`.     
   
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
2. `EventProcessor` adlı yeni bir sınıf oluşturmak için aşağıdaki kodu kullanın.
   
    ```java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;
   
    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;
   
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }
   
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
   
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }
   
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
   
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```
3. Adlı bir sınıf oluşturma `EventProcessorSample`, aşağıdaki kodu kullanarak.
   
    ```java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;
   
    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";
   
            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
   
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
   
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
   
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }
   
            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
   
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
   
            System.out.println("End of sample");
        }
    }
    ```
4. Aşağıdaki alanları olay hub'ı ve depolama hesabı oluştururken kullanılan değerlerle değiştirin.
   
    ```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
   
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
   
    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [!NOTE]
> Bu öğretici, EventProcessorHost’un tek bir örneğini kullanır. Verimliliğini artırmak için birden çok örneğini EventProcessorHost, tercihen ayrı makinelerde çalıştırmanız önerilir.  Bu da artıklık sağlar. Böyle durumlarda, alınan olayların yük dengesi için çeşitli örnekler otomatik olarak birbirleriyle koordine olurlar. Birden çok alıcının her birinin *tüm* olayları işlemesini istiyorsanız **ConsumerGroup** kavramını kullanmalısınız. Olaylar farklı makinelerden alındığında, dağıtıldıkları makineleri (veya rolleri) temel alan EventProcessorHost örnekleri için ad belirtmek yararlı olabilir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
