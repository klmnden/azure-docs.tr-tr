---
title: Azure Service fabric'te ReliableConcurrentQueue
description: ReliableConcurrentQueue paralel kaybolmamasının sağlar ve dequeues yüksek performanslı kuyruğudur.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: raja,tyadam,masnider,vturecek
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: aljo
ms.openlocfilehash: dbdfa4686c047fa7cf5d74cd9aca768447f9db93
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663662"
---
# <a name="introduction-to-reliableconcurrentqueue-in-azure-service-fabric"></a>Azure Service fabric'te ReliableConcurrentQueue giriş
Güvenilir eşzamanlı kuyruk, bir zaman uyumsuz işlem ve çoğaltılan kuyruğa hangi özellikleri yüksek eşzamanlılık kuyruğudur ve sıradan çıkarma işlemleri. Yüksek aktarım hızına ve düşük gecikme süreli katı FIFO tarafından sağlanan sıralama esneterek sunmak için tasarlanan [güvenilir kuyruk](https://msdn.microsoft.com/library/azure/dn971527.aspx) ve bunun yerine bir mümkün olan en iyi sıralama sağlar.

## <a name="apis"></a>API'ler

|Eşzamanlı kuyruk                |Güvenilir Eşzamanlı Kuyruk                                         |
|--------------------------------|------------------------------------------------------------------|
| void Enqueue(T item)           | Görev EnqueueAsync (ITransaction tx, T öğesi)                       |
| bool TryDequeue (out T sonuç)  | Görev < ConditionalValue < T >> TryDequeueAsync (ITransaction tx)  |
| int Count()                    | long Count()                                                     |

## <a name="comparison-with-reliable-queuehttpsmsdnmicrosoftcomlibraryazuredn971527aspx"></a>Karşılaştırma [güvenilir sırası](https://msdn.microsoft.com/library/azure/dn971527.aspx)

Güvenilir eşzamanlı kuyruk alternatif olarak sunulur [güvenilir kuyruk](https://msdn.microsoft.com/library/azure/dn971527.aspx). Katı FIFO sıralama olduğu gerekli durumlarda kullanılmalıdır olarak FIFO gerektiren bir tradeoff eşzamanlılık ile güvence altına alır.  [Güvenilir kuyruk](https://msdn.microsoft.com/library/azure/dn971527.aspx) kuyruğa izin verilen en fazla bir işlem ve aynı anda sıradan çıkarma için izin verilen en fazla bir işlem ile FIFO sıralama zorlamak için kilit kullanır. Güvenilir eşzamanlı kuyruk karşılaştırması, sıralama kısıtlamayı rahatlatır; ve kendi kuyruğa ayırma ve işlemleri sıradan çıkarma sayısı eşzamanlı işlemler sağlar. Mümkün olan en iyi sıralama ancak göreli sıralamasını güvenilir eşzamanlı kuyruk iki değerler hiçbir zaman için garanti, sağlanır.

Güvenilir eşzamanlı kuyruk daha yüksek verimlilik ve daha düşük gecikme süresi sağlayan [güvenilir kuyruk](https://msdn.microsoft.com/library/azure/dn971527.aspx) kaybolmamasının gerçekleştirme birden çok eş zamanlı işlem olduğunda ve/veya dequeues.

Örnek Kullanım örneği için ReliableConcurrentQueue [ileti kuyruğu](https://en.wikipedia.org/wiki/Message_queue) senaryo. Bu senaryoda, bir veya daha fazla ileti üreticileri oluşturun ve öğeleri kuyruğa ekleyin ve bir veya daha fazla ileti tüketiciler iletileri sıradan çıkarmak ve işlenecekleri. Kuyruğu işlemek için eş zamanlı işlem kullanarak birden çok üreticilerin ve tüketicilerin bağımsız olarak çalışabilir.

## <a name="usage-guidelines"></a>Kullanım yönergeleri
* Sıranın kuyruğundaki öğelerin düşük tutma süresine sahip bekliyor. Diğer bir deyişle, öğeleri kuyruğa uzun bir süredir kalın değil.
* Sıranın katı FIFO sıralama garanti etmez.
* Sıranın kendi yazma okumaz. Sıraya alınan bir işlem içinde bir öğe ise aynı işlem içindeki bir dequeuer için görünür olmaz.
* Dequeues birbirinden yalıtılmış değildir. Öğesi, *A* işlemde sıradan çıkarılan *txnA*rağmen *txnA* öğe yüklü durumda değil *A* eşzamanlı için görünür olmaz işlem *txnB*.  Varsa *txnA* durdurur, *A* için görünür olacak *txnB* hemen.
* *TryPeekAsync* davranışını kullanarak uygulanabilir bir *TryDequeueAsync* ve sonra işlem iptal ediliyor. Buna örnek olarak Programming Patterns bölümünde bulunabilir.
* İşlem olmayan sayısıdır. Kuyrukta öğe sayıları hakkında bir fikir edinmek için kullanılabilir ancak bir-belirli bir noktaya temsil eder ve bağlı yararlandı olamaz.
* İşlem sistemde bir performans etkisi sahip olabilecek uzun süre çalışan işlemleri önlemek için etkin durumdayken dequeued öğelerde pahalı işleme gerçekleştirilmemelidir.

## <a name="code-snippets"></a>Kod Parçacıkları
Bize birkaç kod parçacıkları ve onların beklenen çıkış bakın. Bu bölümde, özel durum işleme göz ardı edilir.

### <a name="enqueueasync"></a>EnqueueAsync
Birkaç kod parçacıkları için kendi beklenen çıktı tarafından takip EnqueueAsync kullanarak aşağıda verilmiştir.

- *1. durum: Göreve sıraya alma*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}
```

Görev başarıyla tamamlandı ve bu orada sıranın değiştirme eşzamanlı işlem olduğunu varsayalım. Kullanıcı sıranın öğelerini aşağıdaki sıralardan birine içermesi için geçerli olacaktır:

> 10, 20
> 
> 20, 10


- *2. durum: Paralel sıraya alma görevi*

```
// Parallel Task 1
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}

// Parallel Task 2
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 30, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 40, cancellationToken);

    await txn.CommitAsync();
}
```

Görevlerin sırası değiştirme diğer eş zamanlı işlem başlatılmalı ve görevleri paralel olarak çalışan işleminin başarıyla tamamlandığını varsayılır. Hiçbir çıkarımı kuyruğundaki öğelerin sırasını hakkında yapılabilir. Bu kod parçacığı için öğeleri 4 hiçbirinde görünebilir! olası sıralamalarının.  Sıranın özgün (sıraya) sırada öğeleri tutmak deneyecek, ancak eşzamanlı işlem veya hatalar nedeniyle yeniden sıralamak için zorlanabilir.


### <a name="dequeueasync"></a>DequeueAsync
Beklenen çıktı tarafından takip TryDequeueAsync kullanmak için birkaç kod parçacıklarını aşağıda verilmiştir. Sıranın sırasındaki şu öğeler önceden doldurulur varsayın:
> 10, 20, 30, 40, 50, 60

- *1. durum: Tek görev sıradan çıkarma*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    await txn.CommitAsync();
}
```

Görev başarıyla tamamlandı ve bu orada sıranın değiştirme eşzamanlı işlem olduğunu varsayalım. Sıradaki öğelerin sırasını hakkında hiçbir çıkarımı yapılan bu yana herhangi üç öğe, herhangi bir sırada sıradan çıkarılan. Sıranın özgün (sıraya) sırada öğeleri tutmak deneyecek, ancak eşzamanlı işlem veya hatalar nedeniyle yeniden sıralamak için zorlanabilir.  

- *2. durum: Paralel görev sıradan çıkarma*

```
// Parallel Task 1
List<int> dequeue1;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;
    dequeue1.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;

    await txn.CommitAsync();
}

// Parallel Task 2
List<int> dequeue2;
using (var txn = this.StateManager.CreateTransaction())
{
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;
    dequeue2.Add(await this.Queue.TryDequeueAsync(txn, cancellationToken)).val;

    await txn.CommitAsync();
}
```

Görevlerin sırası değiştirme diğer eş zamanlı işlem başlatılmalı ve görevleri paralel olarak çalışan işleminin başarıyla tamamlandığını varsayılır. Listeleri kuyruğundaki öğelerin sırasını hakkında hiçbir çıkarımı yapılan bu yana *dequeue1* ve *dequeue2* her herhangi bir sırada herhangi iki öğe içerir.

Aynı öğe olacak *değil* her iki listede de görünür. Bu nedenle, dequeue1 varsa *10*, *30*, dequeue2 olacaktır *20*, *40*.

- *3. durum: Sıradan çıkarma işlemi iptal ile sıralama*

Yürütülen ile bir işlem iptal ediliyor, kuyruğun head üzerinde öğeleri geri koyar dequeues. Hangi öğelerin kuyruğun head üzerinde geri yerleştirilir sırasını garanti edilmez. Bize aşağıdaki koda bakın:

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    // Abort the transaction
    await txn.AbortAsync();
}
```
Öğeler şu sırayla sıradan çıkarılan varsayın:
> 10, 20

İşlem iptal ediyoruz, öğeleri aşağıdaki sıralardan birine kuyruğun baş dön eklenir:
> 10, 20
> 
> 20, 10

Aynı işlem olduğu değil başarıyla tüm durumlarda geçerlidir *kabul edilen*.

## <a name="programming-patterns"></a>Programlama desenleri
Bu bölümde, bize birkaç programlama ara ReliableConcurrentQueue kullanarak yardımcı olabilecek desenler.

### <a name="batch-dequeues"></a>Batch Dequeues
Önerilen bir programlama modelidir tüketici göreve batch için kendi birini gerçekleştirmek yerine dequeues teker teker sıradan çıkarma. Kullanıcı, her batch veya toplu iş boyutu arasında gecikmeler azaltma seçebilirsiniz. Aşağıdaki kod parçacığını bu programlama modelini göstermektedir.  Bu örnekte unutmayın, işlem kaydedildikten sonra bir hata işleme sırasında ortaya çıkacaksa, işlenmemiş öğe işlenen kalmadan kaybolacak işlem yapılmaz.  Alternatif olarak, ancak bu performansı üzerinde olumsuz bir etkiye sahip ve önceden işlenmiş öğelerin gerektirip gerektirmediği hareketin kapsamında, işlem yapılabilir.

```
int batchSize = 5;
long delayMs = 100;

while(!cancellationToken.IsCancellationRequested)
{
    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        for(int i = 0; i < batchSize; ++i)
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if (ret.HasValue)
            {
                // If an item was dequeued, add to the buffer for processing
                processItems.Add(ret.Value);
            }
            else
            {
                // else break the for loop
                break;
            }
        }

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }

    int delayFactor = batchSize - processItems.Count;
    await Task.Delay(TimeSpan.FromMilliseconds(delayMs * delayFactor), cancellationToken);
}
```

### <a name="best-effort-notification-based-processing"></a>En yüksek çaba bildirim tabanlı işleme
Başka bir ilgi çekici programlama deseni sayısı API kullanır. Burada, size en yüksek çaba bildirim tabanlı sıra için işleme uygulayabilirsiniz. Kuyruk sayısı, bir kuyruğa veya kuyruktan alma görevi kısıtlama için kullanılabilir.  İşlemin dışında işlemesi bu yana işlenirken bir hata oluşursa, önceki örnekte olduğu gibi işlenmemiş öğeleri kayıp olabileceğine dikkat edin.

```
int threshold = 5;
long delayMs = 1000;

while(!cancellationToken.IsCancellationRequested)
{
    while (this.Queue.Count < threshold)
    {
        cancellationToken.ThrowIfCancellationRequested();

        // If the queue does not have the threshold number of items, delay the task and check again
        await Task.Delay(TimeSpan.FromMilliseconds(delayMs), cancellationToken);
    }

    // If there are approximately threshold number of items, try and process the queue

    // Buffer for dequeued items
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        ConditionalValue<int> ret;

        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if (ret.HasValue)
            {
                // If an item was dequeued, add to the buffer for processing
                processItems.Add(ret.Value);
            }
        } while (processItems.Count < threshold && ret.HasValue);

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
}
```

### <a name="best-effort-drain"></a>En yüksek çaba boşaltma
Sıranın bir boşaltma veri yapısı eşzamanlı niteliği nedeniyle garanti edilemez.  Bu sıranın hiçbir kullanıcı işlemlerinde uçuşan olsa bile TryDequeueAsync belirli bir çağrısına sıraya önceden var olan bir öğe döndürebilir değil, olası ve taahhüt olur.  Sıraya alınan öğe garanti *sonunda* sıradan çıkarma için bir bant dışı iletişim mekanizması bağımsız bir tüketici sıranın kararlı bir duruma bile tüm Üreticiler ulaştı ancak bilemezsiniz görünür hale gelir işlemlerine izin durduruldu ve yeni kuyruğa alındı. Bu nedenle, aşağıda uygulanan en yüksek çaba boşaltma işlemi.

Kullanıcı tüm başka üretici ve tüketici görevleri durdurun ve tamamlama veya iptal, sıranın boşaltma çalışmadan önce yürütülen işlemler için beklemeniz.  Kullanıcı kuyruğundaki öğe sayısı beklenen biliyorsa tüm öğeleri sıradan çıkarılan olmadığını bildiren bir bildirim ayarlayabilirler.

```
int numItemsDequeued;
int batchSize = 5;

ConditionalValue ret;

do
{
    List<int> processItems = new List<int>();

    using (var txn = this.StateManager.CreateTransaction())
    {
        do
        {
            ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);

            if(ret.HasValue)
            {
                // Buffer the dequeues
                processItems.Add(ret.Value);
            }
        } while (ret.HasValue && processItems.Count < batchSize);

        await txn.CommitAsync();
    }

    // Process the dequeues
    for (int i = 0; i < processItems.Count; ++i)
    {
        Console.WriteLine("Value : " + processItems[i]);
    }
} while (ret.HasValue);
```

### <a name="peek"></a>Göz At
ReliableConcurrentQueue sağlamaz *TryPeekAsync* API. Kullanıcıların elde gözlem anlam kullanarak bir *TryDequeueAsync* ve sonra işlem iptal ediliyor. Bu örnekte, dequeues yalnızca öğenin değeri büyükse işlenen *10*.

```
using (var txn = this.StateManager.CreateTransaction())
{
    ConditionalValue ret = await this.Queue.TryDequeueAsync(txn, cancellationToken);
    bool valueProcessed = false;

    if (ret.HasValue)
    {
        if (ret.Value > 10)
        {
            // Process the item
            Console.WriteLine("Value : " + ret.Value);
            valueProcessed = true;
        }
    }

    if (valueProcessed)
    {
        await txn.CommitAsync();    
    }
    else
    {
        await txn.AbortAsync();
    }
}
```

## <a name="must-read"></a>Okuma olmalıdır
* [Reliable Services hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [Reliable Services bildirimleri](service-fabric-reliable-services-notifications.md)
* [Reliable Services yedekleme ve geri yükleme (olağanüstü durum kurtarma)](service-fabric-reliable-services-backup-restore.md)
* [Güvenilir durum Yöneticisi'ni yapılandırma](service-fabric-reliable-services-configuration.md)
* [Service Fabric Web API Hizmetleri ile çalışmaya başlama](service-fabric-reliable-services-communication-webapi.md)
* [Reliable Services programlama modelinin Gelişmiş kullanımı](service-fabric-reliable-services-advanced-usage.md)
* [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
