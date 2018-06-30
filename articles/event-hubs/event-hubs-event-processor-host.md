---
title: Hangi Azure olay hub'ları olay işleyicisi konağı olduğunu ve neden kullanın | Microsoft Docs
description: Genel bakış ve Azure olay hub'ları olay işleyicisi konağı giriş
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: shvija
ms.openlocfilehash: 4907004f4bb88cf0fe9fb799cab236ebf98bba7a
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133116"
---
# <a name="azure-event-hubs-event-processor-host-overview"></a>Azure olay hub'ları olay işleyicisi konağı genel bakış

Azure Event Hubs akış milyonlarca olayların düşük maliyetle için kullanılan bir güçlü telemetri alım hizmetidir. Bu makalede kullanarak alınan olayları kullanma *olay işleyicisi konağı* (EPH); akıllı bir tüketici aracıyı denetim noktası oluşturma, kiralama ve paralel olay okuyucuları yönetimini basitleştirir.  

Olay hub'larına ölçeklendirmek için bölümlenmiş tüketici fikrini anahtardır. Tersine için [rakip tüketiciye](http://msdn.microsoft.com/en-us/library/dn568101.aspx) düzeni, bölümlenmiş tüketici düzeni sağlar yüksek ölçekli Çekişme sıkışıklık kaldırarak ve uçtan uca paralellik kolaylaştırmanın.

## <a name="home-security-scenario"></a>Giriş güvenlik senaryosu

Örnek bir senaryo 100.000 ev izler bir ev güvenlik şirket göz önünde bulundurun. Dakikada, bu verileri bir hareket algılayıcısı, kapı/penceresi açık algılayıcı, cam sonu algılayıcısı, her giriş yüklü vb. gibi çeşitli algılayıcılar alır. Şirket Satışlar kendi giriş yakın gerçek zamanlı etkinliğini izlemek için bir web sitesi sağlar.

Her algılayıcı verileri olay hub'ına iter. Olay hub'ı 16 bölümleri ile yapılandırılır. Alan tarafta, bu olayları okuyabilir, birleştirmek (filtresi, toplama, vb.) ve ardından kullanıcı dostu bir web sayfasına öngörülen bir depolama blob için toplama dökümü bir mekanizma gerekir.

## <a name="write-the-consumer-application"></a>Tüketici uygulaması yazma

Senaryo, dağıtılmış bir ortama tüketici tasarlarken, aşağıdaki gereksinimleri işlemesi gerekir:

1. **Ölçek:** birkaç olay hub'ları bölümleri okunurken sahipliğini her bir tüketici ile birden çok tüketiciye oluşturun.
2. **Yük Dengeleme:** artırın veya tüketicileri dinamik olarak azaltın. Örneğin, her giriş için yeni bir algılayıcı türü (örneğin, gizli monoxide algılayıcısı) eklendiğinde, olayların sayısını artırır. Bu durumda, işleci (insan) tüketici örneklerinin sayısını artırır. Tüketiciler havuzu oldukları, bölüm sayısı yeniden dengelemeniz sonra yeni eklenen tüketicileri ile yükü paylaştırmak için.
3. **Hatalarda sorunsuz Sürdür:** bir tüketici varsa (**tüketici A**) başarısız olursa (örneğin, sanal makine tüketici aniden çöküyor barındırma), diğer tüketicilerin tarafındansahipolunanbölümleriçekmemümkünolmasıgerekir**tüketici A** ve devam edin. Ayrıca, devamlılık noktası, adlı bir *denetim noktası* veya *uzaklığı*, kesiştiği tam noktada olmalıdır **tüketici A** başarısız, veya biraz daha önceki.
4. **Consume Events:** önceki üç nokta tüketici yönetimi ile ilgilidir, ancak olayları kullanma ile yararlı bir şeyler; Örneğin, bu toplama ve blob depolama alanına yükleme kodu olmalıdır.

Kendi çözüm bu oluşturmak yerine, olay hub'ları aracılığıyla bu işlevselliği sağlar [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) arabirimi ve [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) sınıfı.

## <a name="ieventprocessor-interface"></a>Ieventprocessor arabirimi

İlk olarak, uygulamaların uygulama tüketen [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) dört yöntemlerine sahiptir arabirimi: [OpenAsync, CloseAsync, ProcessErrorAsync ve ProcessEventsAsnyc](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor?view=azure-dotnet#methods). Bu arabirim Event Hubs'a gönderir olayları kullanmak için gerçek bir kod içerir. Aşağıdaki kod, bir basit uygulama gösterir:

```csharp
public class SimpleEventProcessor : IEventProcessor
{
    public Task CloseAsync(PartitionContext context, CloseReason reason)
    {
       Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
       return Task.CompletedTask;
    }

    public Task OpenAsync(PartitionContext context)
    {
       Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
       return Task.CompletedTask;
     }

    public Task ProcessErrorAsync(PartitionContext context, Exception error)
    {
       Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
       return Task.CompletedTask;
    }

    public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
       foreach (var eventData in messages)
       {
          var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
             Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
       }
       return context.CheckpointAsync();
    }
}
```

Ardından, örneği bir [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneği. Oluştururken aşırı bağlı olarak [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneği oluşturucuda aşağıdaki parametreleri kullanılır:

- **ana bilgisayar adı:** her tüketici örneğinin adı. Her örneği **EventProcessorHost** bu değer kod olmayan sabit en iyi şekilde bir tüketici grubundaki Bu değişken için benzersiz bir değer olmalıdır.
- **eventHubPath:** olay hub'ın adı.
- **consumerGroupName:** Event Hubs kullanan **$Default** varsayılan bir tüketici grubu, ancak adını işleme belirli yönü için bir tüketici grubu oluşturmak için iyi bir uygulama olarak.
- **eventHubConnectionString:** Azure portalından alınabilir olay hub'ına bağlantı dizesi. Bu bağlantı dizesi olmalıdır **dinleme** olay hub'ı izinleri.
- **storageConnectionString:** iç kaynak yönetimi için kullanılan depolama hesabı.

Son olarak, Tüketicileri kaydetmek [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) Event Hubs hizmeti örneğiyle. Kaydetme bildirir tüketici uygulama bazı bölümleri olaylarından tüketir beklediğiniz ve çağırmak için Event Hubs hizmeti [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) tüketmeye olayları iter her uygulama kodu.

### <a name="example"></a>Örnek

Örnek olarak, 5 sanal makineleri (VM'ler) olduğunuzu varsayalım olayları ve basit bir konsol uygulaması her VM'deki tüketen için ayrılmış, hangi gerçek tüketim çalışır. Her konsol uygulaması tane oluşturur [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneği ve Event Hubs hizmetine kaydolur.

Bu örnek senaryoda düşünelim 16 bölümleri 5 olarak ayrılır **EventProcessorHost** örnekleri. Bazı **EventProcessorHost** örnekleri diğerlerinden daha fazla birkaç bölüme sahip. Her biri için bölüm bir **EventProcessorHost** kopyasına sahip, bir örneğini oluşturur `SimpleEventProcessor` sınıfı. Bu nedenle, 16 örneklerini vardır `SimpleEventProcessor` her bölüm için atanan biriyle genel.

Aşağıdaki listede, bu örnek özetlenmektedir:

- 16 olay hub'ları bölüm.
- 5 VM, her VM 1 tüketici uygulama (örneğin, Consumer.exe).
- Kayıtlı 5 EPH örnekleri Consumer.exe tarafından her bir VM 1.
- 16 `SimpleEventProcessor` 5 EPH örnekleri tarafından oluşturulan nesneler.
- 1 Consumer.exe uygulama 4 içerebilir `SimpleEventProcessor` nesneleri bu yana 1 EPH örnek 4 bölümleri sahip.

## <a name="partition-ownership-tracking"></a>İzleme bölümü sahipliği

Bir bölüm EPH örneği (veya bir tüketici) sahipliğini izleme için sağlanan Azure depolama hesabıyla izlenir. Basit bir tablo olarak izleme gibi görselleştirebilirsiniz. Sağlanan depolama hesabı altında BLOB'ları inceleyerek gerçek uygulamayı görebilirsiniz:

| **Tüketici grubu adı** | **Bölüm kimliği** | **Ana bilgisayar adı (sahibi)** | **Alınan kira (veya sahipliği) süresi** | **Bölüm (Denetim) uzaklığı** |
| --- | --- | --- | --- | --- |
| $Default | 0 | Tüketici\_VM3 | 2018-04-15T01:23:45 | 156 |
| $Default | 1 | Tüketici\_VM4 | 2018-04-15T01:22:13 | 734 |
| $Default | 2 | Tüketici\_VM0 | 2018-04-15T01:22:56 | 122 |
| : |   |   |   |   |
| : |   |   |   |   |
| $Default | 15 | Tüketici\_VM3 | 2018-04-15T01:22:56 | 976 |

Burada, her ana bilgisayarın bir bölüm sahipliğini belirli bir süre için (kira süresi) alır. Bir ana bilgisayar başarısız olursa (VM kapatır) kira süresi dolar. Diğer ana bölüm sahipliğini almayı deneyin ve bir ana bilgisayar başarılı olur. Bu işlem yeni bir sahip olan bölüm kira sıfırlar. Bu şekilde, bir seferde yalnızca tek bir okuyucu herhangi bir tüketici grubundaki belirli bölümünden okuyabilir.

## <a name="receive-messages"></a>İleti alma

Her çağrı [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) olayları koleksiyonu sunar. Bu olayları işlemek için sizin sorumluluğunuzdadır değil. Oldukça hızlı bir şey yapmanız önerilir; diğer bir deyişle, mümkün olduğunca az miktarda işleme olarak yapın. Bunun yerine, tüketici grupları kullanın. Depolama alanına yazmak ve bazı üretim yapmanız gerekiyorsa, iki tüketici grupları kullanıyorsanız ve iki genellikle daha iyi [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) ayrı olarak çalıştırmanız uygulamaları.

İşleme sırasında bir noktada ne okuma tamamlandı ve izlemek isteyebilirsiniz. İzleme tutma akış başlangıcından döndürmeyen için okuma, yeniden başlatırsanız kritik öneme sahiptir. [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) kullanarak bu izleme basitleştirir *kontrol noktaları*. Bir denetim noktası konumu ya da belirli bir tüketici grubundaki belirli bir bölüm için uzaklık, işlenen olduğunuz hangi noktası yerine iletileri. Bir denetim noktası olarak işaretleme **EventProcessorHost** çağırarak gerçekleştirilir [CheckpointAsync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) yöntemi [PartitionContext](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext) nesnesi. Bu işlem genellikle içinde yapılır [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) yöntemi ancak de yapılabilir [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync).

## <a name="checkpointing"></a>Denetim noktası oluşturma

[CheckpointAsync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) yöntemi iki aşırı sahiptir: en yüksek olay uzaklık tarafından döndürülen koleksiyon içinde denetim noktaları, ilk olarak, hiçbir parametre [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync). "Yüksek su" işareti uzaklığı; Bu, çağırdığınızda tüm son olayları işlenmiş varsayar. Bu şekilde bu yöntemi kullanırsanız, bir olay işleme kodunuzu döndürdü sonra çağrı beklenir unutmayın. İkinci aşırı belirtmenize olanak sağlar. bir [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) denetim noktası örneğine. Bu yöntem, farklı türde bir filigran denetim kullanmanıza olanak sağlar. Bu Filigran ile bir "düşük su" işareti uygulayabilirsiniz: olduğunuz belirli en düşük sıralı olay işlenir. Bu aşırı uzaklık yönetim esneklik sağlamak için sağlanır.

Denetim noktası gerçekleştirildiğinde, bir JSON dosyası bölüm özgü bilgiler (özellikle, uzaklık) ile yazılmış oluşturucu içinde sağlanan depolama hesabı [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Bu dosya sürekli olarak güncelleştirilir. Denetim noktası oluşturma bağlamda dikkate alınması gereken kritik - denetim uygun her ileti. Denetim noktası oluşturma için büyük olasılıkla kullanılan depolama hesabı, bu yükü işlemek değil, ancak daha da önemlisi denetim noktası oluşturma her olay Service Bus kuyruğuna bir event hub'den daha iyi bir seçenek olabilir sıraya alınmış bir Mesajlaşma düzeni göstergesi olduğu. Olay hub'ları arkasında büyük ölçekte "en az bir kez" teslim alma olur. Aşağı Akış sistemleri ıdempotent yaparak, bu arızalardan kurtarmak kolay veya birden çok kez alınan olayları sonucunda ortaya çıkan yeniden başlatır.

## <a name="thread-safety-and-processor-instances"></a>İş parçacığı güvenliği ve işlemci örnekleri

Varsayılan olarak, [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) iş parçacığı güvenlidir ve örneğini göre zaman uyumlu bir şekilde davranır [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor). Olaylar için bir bölüm, geldiğinde [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) üzerinde adlı **Ieventprocessor** bölüm ve sonraki çağrılar engeller için örnek **ProcessEventsAsync**bölümü için. Sonraki iletiler ve çağrıları **ProcessEventsAsync** sıra arka planda ileti Pompalama diğer iş parçacıklarında arka planda çalışmaya devam ettikçe. Bu iş parçacığı güvenliği iş parçacığı koleksiyonları gereksinimini ortadan kaldırır ve performansı önemli ölçüde artırır.

## <a name="shut-down-gracefully"></a>Düzgün biçimde kapatılamadı

Son olarak, [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) tüm bölüm okuyucuların temiz kapatma sağlar ve her zaman bir örneğini kapanırken çağrılmalıdır [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Bunun Sağlanamaması diğer örneklerini başlatırken gecikmelere neden olabilir **EventProcessorHost** kira geçerlilik süresi ve dönem çakışmaları nedeniyle. Bu ayrıntılı dönem yönetim kapsamına [blog gönderisi](https://blogs.msdn.microsoft.com/gyan/2014/09/02/event-hubs-receiver-epoch/)

## <a name="lease-management"></a>Kiralama Yönetimi

Daha önce açıklandığı gibi izleme tablosunu otomatik ölçek yapısını büyük ölçüde basitleştirir. [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync). Bir örneği olarak **EventProcessorHost** başlatır, mümkün olduğu kadar kiraları alır ve olayları okumaya başlar. Kira süresi, dolmak üzere olarak **EventProcessorHost** bir ayırma koyarak yenilemek çalışır. Kira yenileme için kullanılabilir, işlemci okuma devam eder, ancak değilse, okuyucu kapatılana ve [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync) olarak adlandırılır. **CloseAsync** Bu bölüm için tüm son temizleme gerçekleştirmek için iyi bir zamandır.

**EventProcessorHost** içeren bir [PartitionManagerOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.partitionmanageroptions) özelliği. Bu özelliği, kiralama yönetimi üzerinde denetim sağlar. Kaydetmeden önce bu seçenekleri ayarlamak, [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) uygulaması.

## <a name="control-event-processor-host-options"></a>Olay işleyicisi konağı seçeneklerini denetleme

Ayrıca, bir aşırı yüklemesini [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) geçen bir [EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) nesnesini parametre olarak. Davranışını denetlemek için bu parametreyi kullanın [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) kendisi. [EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions) dört özellikleri ve bir olay tanımlar:

- [MaxBatchSize](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize): çağrısından almak istediğiniz koleksiyonu en büyük boyutunu [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync). Bu boyut yalnızca en büyük boyutu en az değil. Alınacak, daha az ileti varsa **ProcessEventsAsync** ile kadar bulunmamaktaydı olarak yürütür.
- [PrefetchCount](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.prefetchcount): temel alınan AMQP kanal tarafından kaç istemci iletileri, üst sınır belirlemek için kullanılan bir değer almanız gerekir. Bu değer sıfırdan büyük veya eşit olmalıdır [MaxBatchSize](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize).
- [InvokeProcessorAfterReceiveTimeout](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.invokeprocessorafterreceivetimeout): Bu parametre ise **true**, [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) bir bölüme olaylarını almak için temel alınan çağrısı zaman aşımına uğradığında çağrılır. Bu yöntem, bölümdeki kaldıktan dönemlerde zamana dayalı eylemleri almak için kullanışlıdır.
- [InitialOffsetProvider](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.initialoffsetprovider): bir okuyucu bir bölüm okuma başladığında uzaklığı ilk sağlamak için çağrılan ayarlanması bir işlev işaretçisi veya lambda ifadesi sağlar. Bir JSON dosyası uzaklığı olan için sağlanan depolama hesabında zaten kaydedildi sürece bu uzaklık belirtmeden okuyucu eski etkinlikte başlatır [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) Oluşturucusu. Bu yöntem, okuyucu başlangıç davranışını değiştirmek istediğinizde kullanışlıdır. Bu yöntem çağrıldığında, nesne parametresi okuyucu başlatıldığına bölüm kimliği içerir.
- [ExceptionReceivedEventArgs](/dotnet/api/microsoft.azure.eventhubs.processor.exceptionreceivedeventargs): oluşan temel özel durumlar bildirim almak sağlar [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Beklediğiniz gibi şeyler çalışmıyorsanız, bu olay aramaya başlamak için uygun bir yerdir.

## <a name="next-steps"></a>Sonraki adımlar

Olay işleyicisi konağı ile tanıdık, Event Hubs hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs programlama kılavuzu](event-hubs-programming-guide.md)
* [Event Hubs’da kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Olay hub'ları örnekler github'da](https://github.com/Azure/azure-event-hubs/tree/master/samples)