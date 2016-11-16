## <a name="receive-messages-with-eventprocessorhost"></a>EventProcessorHost bulunan iletiler alma
[EventProcessorHost][EventProcessorHost], Event Hubs’a ait kalıcı denetim noktalarını ve paralel alımları yöneterek bu Event Hubs’a ait alma olaylarını basitleştiren bir .NET sınıfıdır. [EventProcessorHost][EventProcessorHost]’u kullanarak, farklı düğümlerde barındırıldığında bile birden çok alıcı arasında olayları bölebilirsiniz. Bu örnek, tek alıcı için [EventProcessorHost][EventProcessorHost]’un nasıl kullanıldığını göstermektedir. [Ölçeği genişletilmiş olay işleme][Ölçeği genişletilmiş olay işleme] örneği birden çok alıcıyla [EventProcessorHost][EventProcessorHost]’un nasıl kullanılacağını göstermektedir.

[EventProcessorHost][EventProcessorHost]’u kullanmak için [Azure Depolama hesabınız][Azure Depolama hesabınız] olmalıdır:

1. [Azure portal][Azure portal] üzerinde oturum açın ve ekranın sol üst köşesindeki **Yeni**’ye tıklayın.
2. **Veri + Depolama** ve ardından **Depolama hesabı**’na tıklayın.
   
    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png)
3. **Depolama hesabı oluştur** dikey penceresinde depolama hesabı için bir ad yazın. Bir Azure aboneliği, kaynak grubu ve kaynağın oluşturulacağı konumu seçin. Sonra **Oluştur**’a tıklayın.
   
    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png)
4. Depolama hesabı listesinde yeni oluşturulan depolama hesabına tıklayın.
5. Depolama hesabı dikey penceresinde **Erişim anahtarları**’na tıklayın. **key1** değerini bu öğreticide daha sonra kullanmak üzere kopyalayın.
   
    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png)
6. Visual Studio'da, **Konsol Uygulaması** proje şablonunu kullanarak yeni Visual C# Masaüstü Uygulaması projesi oluşturun. Proje **Alıcı** için bir ad verin.
   
    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png)
7. Çözüm Gezgini'nde çözüme sağ tıklayın ve ardından **Çözüm için NuGet Paketlerini Yönet**'e tıklayın.
8. **Gözat** sekmesine tıklayıp `Microsoft Azure Service Bus Event Hub - EventProcessorHost` aramasını gerçekleştirin. Proje adının (**Alıcı**), **Sürüm(ler)** kutusunda belirtildiğinden emin olun. **Yükle**'ye tıklayın ve kullanım koşullarını kabul edin.
   
    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png)
   
    Visual Studio, tüm bağımlılıklarıyla birlikte [Azure Service Bus Olay Hub'ı - EventProcessorHost NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost) indirir, yükler ve ona başvuru ekler.
9. **Alıcı** projesine sağ tıklayın, **Ekle** ve **Sınıf**’a tıklayın. Yeni sınıf **SimpleEventProcessor**’ı adlandırın ve ardından sınıfı oluşturmak için **Ekle**’ye tıklayın.
   
    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png)
10. Aşağıdaki deyimleri SimpleEventProcessor.cs dosyasının en üstüne ekleyin:
    
     ```
     using Microsoft.ServiceBus.Messaging;
     using System.Diagnostics;
     ```
    
     Sonra da, aşağıdaki kodu sınıfın gövdesi yerine geçirin:
    
     ```
     class SimpleEventProcessor : IEventProcessor
     {
         Stopwatch checkpointStopWatch;
    
         async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
         {
             Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
             if (reason == CloseReason.Shutdown)
             {
                 await context.CheckpointAsync();
             }
         }
    
         Task IEventProcessor.OpenAsync(PartitionContext context)
         {
             Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
             this.checkpointStopWatch = new Stopwatch();
             this.checkpointStopWatch.Start();
             return Task.FromResult<object>(null);
         }
    
         async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
         {
             foreach (EventData eventData in messages)
             {
                 string data = Encoding.UTF8.GetString(eventData.GetBytes());
    
                 Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                     context.Lease.PartitionId, data));
             }
    
             //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
             if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
             {
                 await context.CheckpointAsync();
                 this.checkpointStopWatch.Restart();
             }
         }
     }
     ```
    
     Olay Hub’ından alınan olayları işlemek için bu sınıf **EventProcessorHost** tarafından çağrılır. `SimpleEventProcessor` sınıfının, **EventProcessorHost** bağlamında düzenli olarak denetim noktası yöntemini çağırmak için bir kronometre kullandığını unutmayın. Alıcı yeniden başlatılırsa, çalışmayı sürdürmek için beş dakikadan fazla kaybedilmemesi sağlanır.
11. **Program** sınıfında aşağıdaki `using` deyimini dosyanın üst kısmına ekleyin:
    
     ```
     using Microsoft.ServiceBus.Messaging;
     ```
    
     Ardından, `Program` sınıfındaki `Main` yöntemini aşağıdaki kodla değiştirin; bu kodda Olay Hub'ı adı ve daha önce kaydettiğiniz ad alanı düzeyinde bağlantı dizesi ve önceki bölümlerde kopyaladığınız depolama hesabı ve anahtarı değişecektir. 
    
     ```
     static void Main(string[] args)
     {
       string eventHubConnectionString = "{Event Hub connection string}";
       string eventHubName = "{Event Hub name}";
       string storageAccountName = "{storage account name}";
       string storageAccountKey = "{storage account key}";
       string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);
    
       string eventProcessorHostName = Guid.NewGuid().ToString();
       EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
       Console.WriteLine("Registering EventProcessor...");
       var options = new EventProcessorOptions();
       options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
       eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();
    
       Console.WriteLine("Receiving. Press enter key to stop worker.");
       Console.ReadLine();
       eventProcessorHost.UnregisterEventProcessorAsync().Wait();
     }
     ```

> [!NOTE]
> Bu öğretici, [EventProcessorHost][EventProcessorHost]’un tek bir örneğini kullanır. Verimliliği artırmak için, birden çok [EventProcessorHost][EventProcessorHost] örneğinin [Ölçeği genişletilmiş olay işleme][Ölçeği genişletilmiş olay işleme] örneğinde gösterildiği gibi çalıştırmanız önerilir. Bu gibi durumlarda, alınan olayların yükünü dengelemek üzere çeşitli örnekler otomatik olarak birbiriyle koordine olur. Birden çok alıcının her birinin *tüm* olayları işlemesini istiyorsanız **ConsumerGroup** kavramını kullanmalısınız. Olaylar farklı makinelerden alındığında, dağıtıldıkları makineleri (veya rolleri) temel alan [EventProcessorHost][EventProcessorHost] örnekleri için ad belirtmek yararlı olabilir. Bu konular hakkında daha fazla bilgi için bkz. [Event Hubs’a Genel Bakış][Event Hubs’a Genel Bakış] ve [Event Hubs Programlama Kılavuzu][Event Hubs Programlama Kılavuzu] konuları.
> 
> 

<!-- Links -->
[Event Hubs’a Genel Bakış]: ../articles/event-hubs/event-hubs-overview.md
[Event Hubs Programlama Kılavuzu]: ../articles/event-hubs/event-hubs-programming-guide.md
[Ölçeği genişletilmiş olay işleme]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Azure Depolama hesabınız]: ../articles/storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure portal]: https://portal.azure.com

<!--HONumber=Nov16_HO2-->


