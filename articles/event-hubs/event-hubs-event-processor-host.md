---
title: Olay işlemcisi konağı - Azure Event Hubs kullanarak olay alma | Microsoft Docs
description: Bu makalede, Azure Event denetim noktası yönetimini basitleştiren, kiralama ve olayları eçimi paralel okumaya hubs'da, olay işlemcisi konağı açıklanır.
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
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 26f0abb48ba268f79167ed5d00e4f96d8b5e5998
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60821866"
---
# <a name="receive-events-from-azure-event-hubs-using-event-processor-host"></a>Olay işlemcisi konağı kullanarak Azure Event Hubs'tan gelen olayları alma

Azure Event Hubs, milyonlarca olayı düşük bir maliyet karşılığında kullanılabilir güçlü telemetri alma hizmetidir. Bu makalede kullanarak içe alınan etkinlikleri kullanma *Event Processor Host* (EPH); akıllı bir tüketici aracıyı denetim noktası oluşturma, kiralama ve paralel olay okuyucuları yönetimini basitleştirir.  

Event Hubs için ölçeklendirmek için anahtar bölümlenmiş tüketici olur. Tersine [rakip tüketiciler](https://msdn.microsoft.com/library/dn568101.aspx) düzeni, bölünmüş bir tüketici modeli sağlar büyük ölçekli Çekişme performans sorunu kaldırma ve uçtan uca paralellik etkinleştirme.

## <a name="home-security-scenario"></a>Giriş güvenlik senaryosu

Örnek bir senaryo 100.000 havaalanlarından izleyen bir giriş güvenliği şirketi düşünün. Her dakika hareket algılayıcısı, kapı/penceresi açık algılayıcı, cam sonu algılayıcısı, her giriş sayfasında yüklü vb. gibi çeşitli sensörlerden alınan verileri alır. Şirket, kendi giriş neredeyse gerçek zamanlı etkinliğini izlemek vatandaşlar için bir web sitesi sağlar.

Her algılayıcı verileri olay hub'ına gönderir. Olay hub'ı 16 bölümleri ile yapılandırılır. Alıcı uçta ise bu etkinlikleri, birleştirmek (filtreleme, toplama, vb.) ve ardından kullanıcı dostu bir web sayfasına gsyih bir depolama blobu için toplama dökümünü bir mekanizma gerekir.

## <a name="write-the-consumer-application"></a>Tüketici uygulaması yazma

Dağıtılmış bir ortamda tüketici tasarlarken, senaryo aşağıdaki gereksinimleri ele almalısınız.

1. **Ölçek:** Birkaç Event Hubs bölümleri okumaya sahipliğini her bir tüketici içeren birden fazla tüketici oluşturun.
2. **Yük Dengeleme:** Artırın veya dinamik olarak tüketiciler azaltın. Örneğin, her giriş için yeni bir algılayıcı türü (örneğin, gizli monoxide algılayıcısı) eklendiğinde, olay sayısını artırır. Bu durumda, ' % s'işleci (insan) tüketici örneği sayısını artırır. Tüketici havuzu oldukları, bölüm sayısını dengeleyebilir sonra yeni eklenen tüketicileriyle yük paylaşma.
3. **Hatalarda sorunsuz Sürdür:** Bir tüketici, (**bir tüketici**) başarısız olursa (örneğin, sanal makine tüketici aniden kilitleniyor barındıran), diğer tüketiciler tarafından sahip olunan bölümleri yerden devam edebiliyorduk bağlanabilmelidir sonra **tüketici A** ve devam et. Ayrıca, devamlılık noktası, adlı bir *denetim noktası* veya *uzaklığı*, tam noktada olmalıdır **tüketici A** hatalı veya biraz daha önceki.
4. **Olayları kullanma:** Önceki üç nokta tüketici yönetimiyle ilgilenmenize olsa da olayları kullanma ve onunla faydalı bir şey için kod olmalıdır; Örneğin, toplama ve blob depolama alanına yükleyin.

Kendi çözümünüzü bu oluşturmak yerine, olay hub'ları aracılığıyla bu işlevselliği sağlar [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) arabirimi ve [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) sınıfı.

## <a name="ieventprocessor-interface"></a>Ieventprocessor arabirimini

İlk olarak, uygulamaların uygulama tüketen [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) arabirimi, dört yöntemleri vardır: [OpenAsync, CloseAsync, ProcessErrorAsync ve ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor?view=azure-dotnet#methods). Bu arabirim, Event Hubs gönderdiği olayları kullanma gerçek kodu içerir. Aşağıdaki kod, basit bir uygulama gösterilmektedir:

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

Ardından, örneği bir [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneği. Oluştururken aşırı yükleme, bağlı olarak [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneği oluşturucusunun içinde aşağıdaki parametreler kullanılır:

- **ana bilgisayar adı:** her tüketici örneğinin adı. Her bir örneği **EventProcessorHost** bu değişkenin içinde bu değer kod sabit olmayan biçimde bir tüketici grubu için benzersiz bir değere sahip olmalıdır.
- **eventHubPath:** Olay hub'ının adı.
- **consumerGroupName:** Event Hubs kullanan **$Default** gibi varsayılan bir tüketici grubu, ancak işlem, belirli bir boyut için bir tüketici grubu oluşturmak için iyi bir uygulama adıdır.
- **eventHubConnectionString:** Azure portaldan alınabilir olay hub'ına bağlantı dizesi. Bu bağlantı dizesi olmalıdır **dinleme** olay hub'ında izinleri.
- **StorageConnectionString:** İç kaynak yönetimi için kullanılan depolama hesabı.

Son olarak, tüketicilerin kaydetme [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneği ile Event Hubs hizmeti. Bir olay işlemcisi sınıfı EventProcessorHost örneğini kaydetme, olay işleme başlar. Kayıt talimatı verir tüketici uygulama bazı bölümleri olaylardan tükettiğini beklenir ve çağırmak için Event Hubs hizmeti [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) kullanmak için olaylar gönderir her uygulama kodu. 


### <a name="example"></a>Örnek

Örneğin, 5 sanal makineleri (VM'ler) Imagine olayları ve her VM'de bir basit bir konsol uygulaması kullanan için ayrılmış, hangi gerçek tüketim çalışır. Aşağıdakilerden her konsol uygulaması oluşturur [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) örneği ve Event Hubs hizmetine kaydeder.

Bu örnek senaryoda varsayalım 16 bölüm 5'e ayrıldığını algıladı **EventProcessorHost** örnekleri. Bazı **EventProcessorHost** örnekleri birkaç daha fazla bölüm diğerlerinden sahip. Her biri için bölüm bir **EventProcessorHost** kopyasına sahip, bir örneğini oluşturur `SimpleEventProcessor` sınıfı. Bu nedenle, 16 örnekleri vardır `SimpleEventProcessor` her bölüm için atanan bir genel.

Aşağıdaki listede, bu örnek özetlenmektedir:

- 16 event Hubs bölümler.
- 5 VM, her VM'deki 1 tüketici uygulama (örneğin, Consumer.exe).
- 5 EPH örneklerin kayıtlı, her VM Consumer.exe tarafından 1.
- 16 `SimpleEventProcessor` 5 EPH örnekleri tarafından oluşturulmuş nesneleri.
- 1 Consumer.exe uygulama 4 içerebilir `SimpleEventProcessor` nesneleri bu yana 1 EPH örnek 4 bölüm sahip.

## <a name="partition-ownership-tracking"></a>İzleme bölümü sahipliği

Bir bölüm EPH örneği (veya bir tüketici) sahipliğini, izleme için belirtilen Azure depolama hesabıyla izlenir. Basit bir tablo olarak izleme gibi görselleştirebilirsiniz. Sağlanan depolama hesabı altındaki BLOB'ları inceleyerek, gerçek uygulama görebilirsiniz:

| **Tüketici grubu adı** | **Bölüm kimliği** | **Ana bilgisayar adı (sahibi)** | **Alınan kira (veya sahipliği)** | **Uzaklık içinde bölümü (Denetim)** |
| --- | --- | --- | --- | --- |
| $Default | 0 | Tüketici\_VM3 | 2018-04-15T01:23:45 | 156 |
| $Default | 1 | Tüketici\_VM4 | 2018-04-15T01:22:13 | 734 |
| $Default | 2 | Tüketici\_VM0 | 2018-04-15T01:22:56 | 122 |
| : |   |   |   |   |
| : |   |   |   |   |
| $Default | 15 | Tüketici\_VM3 | 2018-04-15T01:22:56 | 976 |

Burada, her konağın belirli bir süre için (kiralama süresi) bir bölüm sahipliğini alır. Ardından konak başarısız olursa (VM kapatıldığında) kira süresi dolar. Diğer ana bölüm sahipliğini almayı denemek ve konaklardan birine başarılı olur. Bu işlem yeni bir sahip olan bölüm kirayı sıfırlar. Bu şekilde, aynı anda yalnızca tek bir okuyucu herhangi belirli bir bölümünün bir tüketici grubundaki okuma yapabilirsiniz.

## <a name="receive-messages"></a>İleti alma

Her çağrı [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) olaylar koleksiyonu sunar. Bu olayları işlemek için sizin sorumluluğunuzdur. İşleyicisi ana bilgisayarı her ileti en az bir kez işler emin olmak istiyorsanız, kodu yeniden deneniyor kendi Canlı yazmanıza gerek. Ancak zehirlenmiş iletileri hakkında dikkatli olun.

Görece şeyler yapmanız önerilir. diğer bir deyişle, mümkün olduğunca az miktarda işleme olarak yapın. Bunun yerine, tüketici gruplarını kullanın. Depolama alanına yazmak ve bazı üretim yapmanız gerekiyorsa, iki tüketici grupları kullanabilir ve iki tane daha iyi [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) ayrı olarak çalıştırılan uygulamalar.

İşleme sırasında belirli bir noktada ne artık okuma tamamlandı ve izlemek isteyebilirsiniz. Akış başlangıcına döndürmeyin için okuma, yeniden başlatırsanız izleme tutmak önemlidir. [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) kullanarak bu izleme basitleştirir *kontrol noktaları*. Bir konum veya uzaklık, belirli bir tüketici grubundaki belirli bir bölüm için bir denetim noktası, işlenen olduğunuz bu noktaya memnun iletileri. Bir denetim noktasında işaretleme **EventProcessorHost** çağrılarak gerçekleştirilir [CheckpointAsync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) metodunda [PartitionContext](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext) nesne. Bu işlem gerçekleştirilir [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) yöntemi de gitmeniz gerekir, ancak [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync).

## <a name="checkpointing"></a>Denetim noktası oluşturma

[CheckpointAsync](/dotnet/api/microsoft.azure.eventhubs.processor.partitioncontext.checkpointasync) yönteminin iki aşırı yüklemesi vardır: birincisi, parametre olmadan, en yüksek olay uzaklığı tarafından döndürülen koleksiyon içinde denetim noktaları [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync). Bu uzaklık, bir "yüksek su" işaretidir; Bu, çağırdığınızda, tüm son olayları işleyip varsayar. Bu şekilde bu yöntemi kullanırsanız, bir olay işleme kodunuzu döndürdü sonra çağrısından beklenir unutmayın. İkinci aşırı yükleme belirtmenizi sağlar bir [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) denetim noktası örneği. Bu yöntem, farklı türde bir filigran kontrol noktasına kullanmanıza olanak sağlar. Bu eşik ile bir "Düşük" Filigran uygulayabilir: olduğunuz belirli en düşük sıralı olay işlendi. Bu aşırı yükleme uzaklık yönetim esneklik olanağı sağlanır.

Denetim noktası işlemi yapıldığında, bir JSON dosyası (özellikle uzaklık) bölüm özgü bilgilerle yazılır oluşturucu içinde sağlanan depolama hesabına [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Bu dosya sürekli olarak güncelleştirilir. Denetim noktası oluşturma bağlamda göz önünde bulundurmanız önemlidir - kontrol noktası için uygun her ileti. Denetim noktası oluşturma için büyük olasılıkla kullanılan depolama hesabı, bu yükle başa değil, ancak denetim noktası her olay bir Service Bus kuyruğuna bir olay hub'den daha iyi bir seçenek olabilir bir kuyruğa alınan Mesajlaşma modeli simulatorda daha da önemlisi. Event Hubs arkasında büyük ölçekte "en az bir kez" teslim alma olur. Aşağı Akış sistemlerine ıdempotent yaparak bu hatalardan kurtarma kolayca veya sonucunda aynı olayları birden çok kez alınan yeniden başlatır.

## <a name="thread-safety-and-processor-instances"></a>İş parçacığı güvenliği ve işlemci örnekleri

Varsayılan olarak, [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) iş parçacığı güvenlidir ve zaman uyumlu bir şekilde örneğini göre davranışını [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor). Olayları bir bölüm için geldiğinde [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) üzerinde çağrılır **Ieventprocessor** bölüm ve sonraki çağrılar için engeller örnek **ProcessEventsAsync**bölümü için. Sonraki iletiler ve çağrıları **ProcessEventsAsync** kuyrukta arka planda ileti pompası diğer iş parçacıkları üzerinde arka planda çalışmaya devam ettikçe. Bu iş parçacığı güvenliği, iş parçacığı güvenli koleksiyonları gereksinimini ortadan kaldırır ve performansı önemli ölçüde artırır.

## <a name="shut-down-gracefully"></a>Düzgün biçimde kapatılamadı

Son olarak, [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) tüm bölüm okuyucular, temiz kapatma sağlar ve her zaman örneğini kapatılırken çağrılmalıdır [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Bunun yapılmaması diğer örneğini başlatırken gecikmelere neden olabilir **EventProcessorHost** kira sona erme ve dönem çakışmaları nedeniyle. Dönem yönetim ayrıntılı olarak ele alınmaktadır [dönem](#epoch) makalesindeki bölümüne bakın. 

## <a name="lease-management"></a>Kiralama Yönetimi
Bir olay işlemcisi sınıfı EventProcessorHost örneğini kaydetme, olay işleme başlar. Ana bilgisayar örneği, büyük olasılıkla tüm konak örneklerinde bölümlerin eşit dağıtımı üzerinde uygun sonuç verir şekilde diğer konak örneklerden bazıları kapmasını bazı bölümlerini bir olay hub'ı üzerinde kiraları alır. Kiralanmış her bölüm için ana bilgisayar örneği sağlanan olay işlemcisi sınıfının bir örneğini oluşturur, ardından bu bölümün dışında olaylarını alır ve bunları olay işlemcisi örneğine geçirir. Daha fazla örnek eklenir ve daha fazla kira yakaladı EventProcessorHost sonunda tüm tüketicileri arasında yük dengeleyen.

Daha önce açıklandığı şekilde, otomatik ölçeklendirme yapısını izleme tablosu büyük ölçüde basitleştirir. [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync). Bir örneği olarak **EventProcessorHost** başlatır, mümkün olduğunca çok kiraları alır ve olayları okumaya başlar. Kiralar, süresi dolmak üzere olarak **EventProcessorHost** rezervasyon yerleştirerek yenilemek çalışır. Kira yenileme için kullanılabilir, işlemci okumaya devam eder, ancak değilse, okuyucu kapatılana ve [CloseAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient.closeasync) çağrılır. **CloseAsync** için o bölümün son tüm temizleme işlemlerini gerçekleştirmek için iyi bir zamandır.

**EventProcessorHost** içeren bir [PartitionManagerOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.partitionmanageroptions) özelliği. Bu özellik, kiralama yönetimine üzerinde denetim sağlar. Kaydetmeden önce bu seçenekleri ayarlamak, [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) uygulaması.

## <a name="control-event-processor-host-options"></a>Olay işlemcisi konağı seçeneklerini denetleme

Ayrıca, bir aşırı yüklemesini [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) götüren bir [EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.registereventprocessorasync?view=azure-dotnet#Microsoft_Azure_EventHubs_Processor_EventProcessorHost_RegisterEventProcessorAsync__1_Microsoft_Azure_EventHubs_Processor_EventProcessorOptions_) bir parametre olarak nesne. Davranışını denetlemek için bu parametreyi kullanın [EventProcessorHost.UnregisterEventProcessorAsync](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost.unregistereventprocessorasync) kendisi. [EventProcessorOptions](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions) dört özellikleri ve bir olay tanımlar:

- [MaxBatchSize](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize): İçinde bir çağrı almak istediğiniz koleksiyonu en büyük boyutunu [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync). Bu boyut en düşük, yalnızca en büyük boyutu değil. Alınabilmesi için daha az sayıda ileti olduğunda **ProcessEventsAsync** ile kadar kullanılabilir olarak yürütür.
- [PrefetchCount](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.prefetchcount): Bir değer üst sınırını ileti sayısını belirlemek için temel alınan AMQP kanal tarafından kullanılan istemci almanız gerekir. Bu değer, büyük veya buna eşit olmalıdır [MaxBatchSize](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.maxbatchsize).
- [InvokeProcessorAfterReceiveTimeout](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.invokeprocessorafterreceivetimeout): Bu parametre **true**, [ProcessEventsAsync](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor.processeventsasync) bir bölüme olaylarını almak için temel çağrısı zaman aşımına uğradığında çağrılır. Bu yöntem, etkinlik olmadan geçen bölüm dönemlerde zamana bağlı eylemleri almak için kullanışlıdır.
- [InitialOffsetProvider](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessoroptions.initialoffsetprovider): Bir okuyucunun bölüm okuma başladığında uzaklığı ilk sağlamak için çağrılan ayarlamak bir işlev işaretçisi veya lambda ifadesi sağlar. Bir uzaklık içeren bir JSON dosyası için sağlanan depolama hesabında zaten kaydedildi sürece bu uzaklığı belirtmeden okuyucu eski etkinlikte başlatır [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) Oluşturucusu. Bu yöntem, okuyucu başlangıç davranışını değiştirmek istediğinizde kullanışlıdır. Bu yöntem çağrıldığında, nesne parametresi okuyucu başlatıldığına bölüm Kimliğini içerir.
- [ExceptionReceivedEventArgs](/dotnet/api/microsoft.azure.eventhubs.processor.exceptionreceivedeventargs): Ortaya durumların temel alınan bildirim alacak sayesinde [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost). Beklediğiniz gibi şeyler çalışmıyorsanız, bu olay bakmaya başlamak için iyi bir yerdir.

## <a name="epoch"></a>Epoch

Alma dönem çalışma şeklini aşağıda verilmiştir:

### <a name="with-epoch"></a>Dönem ile
Dönem bölüm/kira sahipliği zorlamak için hizmetin kullandığı benzersiz bir tanımlayıcıdır (dönem değer). Kullanarak bir dönem tabanlı alıcısı oluşturma [CreateEpochReceiver](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createepochreceiver?view=azure-dotnet) yöntemi. Bu yöntem, bir dönem tabanlı alıcı oluşturur. Alıcı, belirli bir event hub bölümünden belirtilen tüketici grubu için oluşturulur.

Dönem özelliği kullanıcılar olduğundan tek bir alıcı bir tüketici grubu üzerinde herhangi bir noktada aşağıdaki kurallar ile zaman içinde emin olanağı sağlar:

- Bir tüketici grubu geçerli bir alıcı varsa, kullanıcı bir alıcı herhangi bir dönem değeriyle oluşturabilirsiniz.
- Bir alıcı bir dönem değeri e1 ile yoktur ve yeni bir alıcı bir dönem değeri e2 ile oluşturulan burada e1 < = e2, alıcı e1 ile otomatik olarak kapatılacağı, alıcı e2 ile başarıyla oluşturuldu.
- Bir alıcı bir dönem değeri e1 ile yoktur ve yeni bir alıcı bir dönem değeri e2 ile oluşturulan burada e1 > e2 sonra e2 ile oluşturma işlemi şu hatayla başarısız: Dönem e1 ile bir alıcı zaten mevcut.

### <a name="no-epoch"></a>Hiçbir dönem
Kullanarak dönem tabanlı olmayan bir alıcı oluşturmak [CreateReceiver](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.eventhubclient.createreceiver?view=azure-dotnet) yöntemi. 

Akış işleme kullanıcılar tek bir tüketici grubunda birden çok alıcı oluşturmak nereye istersiniz bazı senaryolar vardır. Böyle senaryoları desteklemek için dönem olmadan bir alıcı oluşturma olanağı sunuyoruz ve bu durumda en fazla 5 eşzamanlı alıcılar bir tüketici grubu üzerinde izin veriyoruz.

### <a name="mixed-mode"></a>Karma mod
Bir alıcı dönem ile oluşturduğunuz ve ardından Hayır dönem veya tersi aynı tüketici grubu geçiş uygulama kullanımını önermemekteyiz. Ancak, bu davranış, hizmet, aşağıdaki kurallar kullanılarak işleme:

- Bir alıcı zaten dönem e1 ile oluşturulur ve etkin bir şekilde olay alıyor ve yeni bir alıcı yok dönem ile oluşturulan ise yeni alıcısı oluşturma başarısız olur. Dönem alıcılar, sistemdeki her zaman öncelik kazanır.
- Bir alıcı zaten dönem e1 ile oluşturulan ve bağlantısı kesilmiş ve yeni bir alıcı yok dönemi yeni MessagingFactory temel ile oluşturulan olduysa, yeni alıcısı oluşturma başarılı olur. Sistemimiz yaklaşık 10 dakika sonra "alıcı kesme" algılayan bir uyarı burada yoktur.
- Hiçbir dönem ile oluşturulan bir veya daha fazla alıcıya vardır ve yeni bir alıcı dönem e1 ile oluşturulan, eski tüm alıcılar bağlantısı.


## <a name="next-steps"></a>Sonraki adımlar

Artık Event Processor Host ile ilgili bilgi sahibi olduğunuza göre Event Hubs hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Event Hubs öğreticisi](event-hubs-dotnet-standard-getstarted-send.md) ile çalışmaya başlayın
* [Event Hubs programlama kılavuzu](event-hubs-programming-guide.md)
* [Event Hubs’da kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Github'da Event Hubs örnekleri](https://github.com/Azure/azure-event-hubs/tree/master/samples)
