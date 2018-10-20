---
title: .NET Framework kullanarak Azure Event Hubs’dan olay alma | Microsoft Belgeleri
description: .NET Framework kullanarak Azure Event Hubs’dan olay almak için bu öğreticiyi izleyin.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2018
ms.author: shvija
ms.openlocfilehash: 9e94357216690438446a738400c979d12f387df6
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471093"
---
# <a name="receive-events-from-azure-event-hubs-using-the-net-framework"></a>.NET Framework kullanarak Azure Event Hubs’dan olay alma

## <a name="introduction"></a>Giriş

Event Hubs bağlı cihaz ve uygulamalardan büyük miktarlarda olay verileri (telemetri) işleyen bir hizmettir. Verileri Event Hubs’a topladıktan sonra bir depolama kümesi kullanarak depolayabilir veya gerçek zamanlı bir analiz sağlayıcısı kullanarak dönüştürebilirsiniz. Bu büyük ölçekli olay toplama ve işleme özelliği, Nesnelerin İnterneti (IoT) gibi modern uygulama mimarilerinin temel bir bileşenidir. Event Hubs ayrıntılı bakış için bkz: [Event Hubs'a genel bakış](event-hubs-about.md) ve [Event Hubs özellikleri](event-hubs-features.md).

Bu öğreticide, bir olay hub'ı kullanarak'ından iletiler alan .NET Framework konsol uygulamasını yazma işlemi gösterilmektedir [Event Processor Host](event-hubs-event-processor-host.md). [Event Processor Host](event-hubs-event-processor-host.md) kalıcı denetim noktalarını yöneterek event hubs'a ait alma olaylarını basitleştiren bir .NET sınıfıdır ve paralel alımları bu event hubs'a ait. Olay işlemcisi konağı kullanarak olayları birden çok alıcı arasında farklı düğümlerde barındırıldığında bile bölebilirsiniz. Bu örnek, tek alıcı için olay işlemcisi konağı kullanmayı gösterir. [Ölçeği genişletilmiş olay işleme] [ Scale out Event Processing with Event Hubs] örnek ile birden çok alıcı olay işlemcisi konağı kullanmayı gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* [Microsoft Visual Studio 2017 veya daha yüksek](http://visualstudio.com).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs ad alanı ve bir olay hub’ı oluşturma
İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için verilen yordamı izleyin [bu makalede](event-hubs-create.md), ardından Bu öğreticide aşağıdaki adımlarla devam edin.

[!INCLUDE [event-hubs-create-storage](../../includes/event-hubs-create-storage.md)]

## <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

Visual Studio'da, **Konsol Uygulaması** proje şablonunu kullanarak yeni Visual C# Masaüstü Uygulaması projesi oluşturun. Proje **Alıcı** için bir ad verin.
   
![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)

## <a name="add-the-event-hubs-nuget-package"></a>Event Hubs NuGet paketini ekleme

1. Çözüm Gezgini'nde **Alıcı** projesine sağ tıklayın ve ardından **Çözüm için NuGet Paketlerini Yönet**'e tıklayın.
2. **Gözat** sekmesine tıklayıp `Microsoft Azure Service Bus Event Hub - EventProcessorHost` için arama yapın. **Yükle**'ye tıklayın ve kullanım koşullarını kabul edin.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    Visual Studio, tüm bağımlılıklarıyla birlikte [Azure Service Bus Olay Hub'ı - EventProcessorHost NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost) indirir, yükler ve ona başvuru ekler.

## <a name="implement-the-ieventprocessor-interface"></a>IEventProcessor arabirimini gerçekleştirme

1. **Alıcı** projesine sağ tıklayın, **Ekle** ve **Sınıf**’a tıklayın. Yeni sınıf **SimpleEventProcessor**’ı adlandırın ve ardından sınıfı oluşturmak için **Ekle**’ye tıklayın.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
2. Aşağıdaki deyimleri SimpleEventProcessor.cs dosyasının en üstüne ekleyin:
    
      ```csharp
      using Microsoft.ServiceBus.Messaging;
      using System.Diagnostics;
      ```
    
3. Aşağıdaki kodu sınıfın gövdesi yerine geçirin:
    
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

## <a name="update-the-main-method-to-use-simpleeventprocessor"></a>Güncelleştirme SimpleEventProcessor kullanılacak ana yöntemi

1. **Program** sınıfında aşağıdaki `using` deyimini dosyanın üst kısmına ekleyin:
    
      ```csharp
      using Microsoft.ServiceBus.Messaging;
      ```
    
2. Değiştirin `Main` yönteminde `Program` olay hub'ı adı ve daha önce kaydettiğiniz ad alanı düzeyinde bağlantı dizesiyle değiştirerek aşağıdaki kod, depolama hesabı ve önceki bölümde kopyaladığınız anahtar ile sınıfı. 
    
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
    
3. Programı çalıştırın ve herhangi bir hata olmadığından emin olun.
  
Tebrikler! Olay İşleyicisi Ana Bilgisayarı’nı kullanarak bir olay hub’ından iletiler aldınız.


> [!NOTE]
> Bu öğretici, [EventProcessorHost](event-hubs-event-processor-host.md)’un tek bir örneğini kullanır. Verimliliği artırmak için birden çok örneğini çalıştırmanızı öneririz [EventProcessorHost](event-hubs-event-processor-host.md)gösterildiği [ölçeği genişletilmiş olay işleme](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) örnek. Bu gibi durumlarda birden çok örneği otomatik olarak yük dengelemesi için birbiriyle alınan olayların koordine edin. 

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, bir olay hub'ından iletiler alan .NET Framework uygulaması oluşturdunuz. .NET Framework kullanarak olay hub'ına olay gönderme hakkında bilgi edinmek için bkz: [event hub'dan - .NET Framework olayları göndermek](event-hubs-dotnet-framework-getstarted-send.md).

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[EventProcessorHost]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Event Hubs overview]: event-hubs-about.md
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]:../storage/common/storage-create-storage-account.md
[Event Processor Host]: event-hubs-event-processor-host.md
[Azure portal]: https://portal.azure.com
