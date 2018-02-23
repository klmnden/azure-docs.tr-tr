---
title: ".NET Framework kullanarak Azure Event Hubs’dan olay alma | Microsoft Belgeleri"
description: ".NET Framework kullanarak Azure Event Hubs’dan olay almak için bu öğreticiyi izleyin."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.author: sethm
ms.openlocfilehash: 8fd70380dbb88f379789e1a4730934dcd38cac5a
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="receive-events-from-azure-event-hubs-using-the-net-framework"></a>.NET Framework kullanarak Azure Event Hubs’dan olay alma

## <a name="introduction"></a>Giriş

Event Hubs bağlı cihaz ve uygulamalardan büyük miktarlarda olay verileri (telemetri) işleyen bir hizmettir. Verileri Event Hubs’a topladıktan sonra bir depolama kümesi kullanarak depolayabilir veya gerçek zamanlı bir analiz sağlayıcısı kullanarak dönüştürebilirsiniz. Bu büyük ölçekli olay toplama ve işleme özelliği, Nesnelerin İnterneti (IoT) gibi modern uygulama mimarilerinin temel bir bileşenidir.

Bu öğreticide, **[Olay İşleyicisi Ana Bilgisayarı][EventProcessorHost]**’nı kullanarak bir olay hub’ından iletiler alan .NET Framework konsol uygulamasını yazma işlemi gösterilmektedir. .NET Framework kullanarak olayları göndermek için [.NET Framework kullanarak Azure Event Hubs’a olay gönderme](event-hubs-dotnet-framework-getstarted-send.md) makalesine bakın veya soldaki içindekiler bölümünden uygun gönderme diline tıklayın.

[Olay İşleyicisi Ana Bilgisayarı][EventProcessorHost], olay hub’larına ait kalıcı denetim noktalarını ve paralel alımları yöneterek bu olay hub’larına ait alma olaylarını basitleştiren bir .NET sınıfıdır. [Olay İşleyicisi Ana Bilgisayarı][Event Processor Host]’nı kullanarak, farklı düğümlerde barındırıldığında bile birden çok alıcı arasında olayları bölebilirsiniz. Bu örnek, tek alıcı için [Olay İşleyicisi Ana Bilgisayarı][EventProcessorHost]'nın nasıl kullanıldığını göstermektedir. [Olay işleme ölçeğini genişletme][Scale out Event Processing with Event Hubs] örneği, birden çok alıcıyla [Olay İşleyicisi Ana Bilgisayarı][EventProcessorHost]'nın nasıl kullanılacağını göstermektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* [Microsoft Visual Studio 2015 veya üzeri](http://visualstudio.com). Bu öğreticideki ekran görüntülerinde Visual Studio 2017 kullanılır.
* Etkin bir Azure hesabı. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir hesap oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/free/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs ad alanı ve bir olay hub’ı oluşturma

İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için [bu makalede](event-hubs-create.md) verilen yordamı uygulayın, ardından bu öğreticide yer alan aşağıdaki adımlarla devam edin.

## <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

[Olay İşleyicisi Ana Bilgisayarı][EventProcessorHost]'nı kullanabilmeniz için bir [Azure Depolama hesabınızın][Azure Storage account] olması gerekir:

1. Oturum [Azure portal][Azure portal], tıklatıp **kaynak oluşturma** en üst ekranın sol.
2. **Depolama** ve ardından **Depolama hesabı**’na tıklayın.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. İçinde **depolama hesabı oluşturma** bölmesinde, depolama hesabı için bir ad yazın. Bir Azure aboneliği, kaynak grubu ve kaynağın oluşturulacağı konumu seçin. Sonra **Oluştur**’a tıklayın.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. Depolama hesabı listesinde, yeni oluşturulan depolama hesabına tıklayın.
5. Depolama hesabı bölmesinde **erişim anahtarları**. **key1** değerini bu öğreticide daha sonra kullanmak üzere kopyalayın.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

## <a name="create-a-receiver-console-application"></a>Alıcı konsol uygulaması oluşturma

1. Visual Studio'da, **Konsol Uygulaması** proje şablonunu kullanarak yeni Visual C# Masaüstü Uygulaması projesi oluşturun. Proje **Alıcı** için bir ad verin.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
2. Çözüm Gezgini'nde **Alıcı** projesine sağ tıklayın ve ardından **Çözüm için NuGet Paketlerini Yönet**'e tıklayın.
3. **Gözat** sekmesine tıklayıp `Microsoft Azure Service Bus Event Hub - EventProcessorHost` için arama yapın. **Yükle**'ye tıklayın ve kullanım koşullarını kabul edin.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    Visual Studio, tüm bağımlılıklarıyla birlikte [Azure Service Bus Olay Hub'ı - EventProcessorHost NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost) indirir, yükler ve ona başvuru ekler.
4. **Alıcı** projesine sağ tıklayın, **Ekle** ve **Sınıf**’a tıklayın. Yeni sınıf **SimpleEventProcessor**’ı adlandırın ve ardından sınıfı oluşturmak için **Ekle**’ye tıklayın.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
5. Aşağıdaki deyimleri SimpleEventProcessor.cs dosyasının en üstüne ekleyin:
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  using System.Diagnostics;
  ```
    
  Sonra da, aşağıdaki kodu sınıfın gövdesi yerine geçirin:
    
  ```csharp
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
    
  Olay hub'ından alınan olayları işlemesi için bu sınıf, **EventProcessorHost** tarafından çağrılır. `SimpleEventProcessor` sınıfı, **EventProcessorHost** bağlamında düzenli olarak denetim noktası yöntemini çağırmak için bir kronometre kullanır. Bu işlem, alıcının yeniden çağrılması durumunda, işleme sürecinde beş dakikadan fazla zaman kaybedilmemesini sağlar.
6. **Program** sınıfında aşağıdaki `using` deyimini dosyanın üst kısmına ekleyin:
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  ```
    
  Ardından, `Program` sınıfındaki `Main` yöntemini aşağıdaki kodla değiştirin; bu kodda olay hub'ı adı ve daha önce kaydettiğiniz ad alanı düzeyinde bağlantı dizesi ve önceki bölümlerde kopyaladığınız depolama hesabı ve anahtarı değişecektir. 
    
  ```csharp
  static void Main(string[] args)
  {
    string eventHubConnectionString = "{Event Hubs namespace connection string}";
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

7. Programı çalıştırın ve herhangi bir hata olmadığından emin olun.
  
Tebrikler! Olay İşleyicisi Ana Bilgisayarı’nı kullanarak bir olay hub’ından iletiler aldınız.


> [!NOTE]
> Bu öğretici, [EventProcessorHost][EventProcessorHost]'un tek bir örneğini kullanır. Aktarım hızını artırmak için, birden çok [EventProcessorHost][EventProcessorHost] örneğinin [Ölçeği genişletilmiş olay işleme][Ölçeği genişletilmiş olay işleme] örneğinde gösterildiği gibi çalıştırmanız önerilir. Bu gibi durumlarda, alınan olayların yükünü dengelemek üzere çeşitli örnekler otomatik olarak birbiriyle koordine olur. Birden çok alıcının her birinin *tüm* olayları işlemesini istiyorsanız **ConsumerGroup** kavramını kullanmalısınız. Olaylar farklı makinelerden alındığında, dağıtıldıkları makineleri (veya rolleri) temel alan [EventProcessorHost][EventProcessorHost] örnekleri için ad belirtmek yararlı olabilir. Bu konu başlıkları hakkında daha fazla bilgi için [Event Hubs'a genel bakış][Event Hubs overview] ve [Event Hubs programlama kılavuzu][Event Hubs Programming Guide] konu başlıklarına bakın.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar

Olay hub’ını oluşturan ve veri gönderip alan çalışan bir uygulama oluşturduğunuza göre aşağıdaki bağlantıları ziyaret ederek daha fazla bilgi alabilirsiniz:

* [Olay İşlemcisi Konağı][Event Processor Host]
* [Event Hubs'a genel bakış][Event Hubs overview]
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[EventProcessorHost]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]:../storage/common/storage-create-storage-account.md
[Event Processor Host]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Azure portal]: https://portal.azure.com
