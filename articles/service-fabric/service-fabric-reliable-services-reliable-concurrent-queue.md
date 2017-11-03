---
title: Azure Service Fabric ReliableConcurrentQueue
description: "ReliableConcurrentQueue paralel enqueues sağlar ve dequeues yüksek verimlilik sırasıdır."
services: service-fabric
documentationcenter: .net
author: sangarg
manager: timlt
editor: raja,tyadam,masnider,vturecek
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: sangarg
ms.openlocfilehash: 122cb48149477f295a65b8ee623c647b6db10a86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-reliableconcurrentqueue-in-azure-service-fabric"></a>Azure Service Fabric ReliableConcurrentQueue giriş
Güvenilir eşzamanlı sırası bir zaman uyumsuz işlem ve çoğaltılmış hangi özellikleri yüksek eşzamanlılık sıraya alma sıradır ve işlemleri dequeue. Tarafından sağlanan sıralama, katı FIFO gevşetme tarafından yüksek verimlilik ve düşük gecikme süresi sunacak şekilde tasarlanan [güvenilir sıra](https://msdn.microsoft.com/library/azure/dn971527.aspx) ve bunun yerine bir en yüksek çaba sıralama sağlar.

## <a name="apis"></a>API'ler

|Eşzamanlı sırası                |Güvenilir eşzamanlı sırası                                         |
|--------------------------------|------------------------------------------------------------------|
| void Enqueue(T item)           | Görev EnqueueAsync (ITransaction tx, T öğe)                       |
| bool TryDequeue (out T sonuç)  | Görev < ConditionalValue < T >> TryDequeueAsync (ITransaction tx)  |
| int Count()                    | uzun Count()                                                     |

## <a name="comparison-with-reliable-queuehttpsmsdnmicrosoftcomlibraryazuredn971527aspx"></a>Karşılaştırma [güvenilir sırası](https://msdn.microsoft.com/library/azure/dn971527.aspx)

Alternatif olarak güvenilir eşzamanlı sıra sunulan [güvenilir sıra](https://msdn.microsoft.com/library/azure/dn971527.aspx). Burada katı FIFO sıralama gerekli değildir, durumlarda kullanılmalıdır olarak FIFO gerektiren bir kolaylığını eşzamanlılık ile güvence altına alır.  [Güvenilir sıra](https://msdn.microsoft.com/library/azure/dn971527.aspx) sıraya alma için izin verilen en fazla bir işlem ve aynı anda dequeue izin verilen en fazla bir işlem ile FIFO sıralama zorlamak için kilitler kullanır. Buna karşılık, güvenilir eşzamanlı sıra sıralama kısıtlaması rahatlatır ve bunların enqueue Interleave ve işlemleri dequeue sayı eşzamanlı işlemler sağlar. En yüksek çaba sıralama ancak güvenilir eşzamanlı sıradaki iki değer göreli sıralamasını hiçbir zaman olabilir garanti sağlanmış.

Güvenilir eşzamanlı sıra daha yüksek verimlilik ve daha düşük gecikme sağlar [güvenilir sıra](https://msdn.microsoft.com/library/azure/dn971527.aspx) enqueues gerçekleştirme birden çok eşzamanlı işlem olduğunda ve/veya dequeues.

Örnek Kullanım örneği ReliableConcurrentQueue için [Message Queue](https://en.wikipedia.org/wiki/Message_queue) senaryo. Bu senaryoda, bir veya daha fazla ileti üreticileri oluşturun ve öğeleri kuyruğa ekleyin ve bir veya daha fazla ileti tüketiciye sırasından ileti çekmek ve işlenecekleri. Birden çok üreticileri ve tüketicileri sırasını işlemek için eş zamanlı işlemleri kullanarak bağımsız olarak çalışabilir.

## <a name="usage-guidelines"></a>Kullanım yönergeleri
* Sıranın kuyruğundaki öğelerin düşük saklama dönemi olmasını bekliyor. Diğer bir deyişle, öğeleri sıraya uzun bir süredir kalmak değil.
* Sıranın katı FIFO sıralama garanti etmez.
* Sıranın kendi yazma okumaz. Bir öğe sıraya alınan bir işlem içinde ise, aynı işlem içinde dequeuer için görünür olmaz.
* Dequeues birbirinden yalıtılmış değildir. Öğesi, *A* işlemde kuyruktan çıkarıldı *txnA*rağmen *txnA* öğe kaydedilmiş, değil *A* eşzamanlı bir işlem için görünür olmaz *txnB*.  Varsa *txnA* durdurur, *A* için görünür olacak *txnB* hemen.
* *TryPeekAsync* davranışı kullanarak uygulanabilir bir *TryDequeueAsync* ve işlem iptal ediliyor. Buna örnek olarak programlama desenleri bölümünde bulunabilir.
* İşlem olmayan sayısıdır. Kuyrukta, öğelerin sayısı hakkında bir fikir edinmek için kullanılabilir, ancak bir nokta zaman temsil eder ve bağlı dayanıyordu olamaz.
* İşlem bir performans etkisi sistemde olabilir uzun süre çalışan işlemleri önlemek için etkin durumdayken dequeued öğeleri üzerindeki pahalı işleme gerçekleştirilmemelidir.

## <a name="code-snippets"></a>Kod parçacıkları
Bize birkaç kod parçacıkları ve bunların beklenen çıkış bakın. Özel durum işleme, bu bölümde göz ardı edilir.

### <a name="enqueueasync"></a>EnqueueAsync
Burada, kendi beklenen çıktı tarafından takip EnqueueAsync kullanmak için bazı kod parçacıkları bulunmaktadır.

- *Durum 1: Tek bir Sıraya alma görevi*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.EnqueueAsync(txn, 10, cancellationToken);
    await this.Queue.EnqueueAsync(txn, 20, cancellationToken);

    await txn.CommitAsync();
}
```

Görevi başarıyla tamamlandı ve o orada sıranın değiştirme eşzamanlı işlem olduğunu varsayalım. Kullanıcı öğelerinin herhangi birinde aşağıdaki siparişleri bulunduğu için kuyruğa bekleyebilirsiniz:

> 10, 20

> 20, 10


- *Durum 2: Sıraya alma görev paralel*

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

Görevleri görevleri paralel olarak çalışan ve sıranın değiştirilmesi diğer eşzamanlı işlem vardı işleminin başarıyla tamamlandığını varsayalım. Hiçbir çıkarım kuyruğundaki öğelerin sırasını hakkında yapılabilir. Bu kod parçacığını için öğeleri 4 hiçbirinde görünebilir! olası sıralamalarını.  Sıranın özgün (sıradaki) sırayla öğeleri tutmak deneyecek, ancak bunları eşzamanlı işlem veya hataları nedeniyle yeniden sıralamak için zorlanabilir.


### <a name="dequeueasync"></a>DequeueAsync
Burada, beklenen çıktı tarafından takip TryDequeueAsync kullanmak için bazı kod parçacıkları bulunmaktadır. Sıranın sırasındaki aşağıdaki öğelerin ile önceden doldurulur varsayın:
> 10, 20, 30, 40, 50, 60

- *Durum 1: Tek Dequeue görevi*

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    await txn.CommitAsync();
}
```

Görevi başarıyla tamamlandı ve o orada sıranın değiştirme eşzamanlı işlem olduğunu varsayalım. Kuyruğundaki öğelerin sırasını hakkında hiçbir çıkarımı yapılan olduğundan, tüm üç öğe, herhangi bir sırada kuyruktan çıkarıldı. Sıranın özgün (sıradaki) sırayla öğeleri tutmak deneyecek, ancak bunları eşzamanlı işlem veya hataları nedeniyle yeniden sıralamak için zorlanabilir.  

- *Durum 2: Paralel görev Dequeue*

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

Görevleri görevleri paralel olarak çalışan ve sıranın değiştirilmesi diğer eşzamanlı işlem vardı işleminin başarıyla tamamlandığını varsayalım. Listeler kuyruğundaki öğelerin sırasını hakkında hiçbir çıkarımı yapılan bu yana *dequeue1* ve *dequeue2* her herhangi bir sırada iki tüm öğeleri içerir.

Aynı öğe *değil* hem listelerde görüntülenir. Bu nedenle, dequeue1 varsa *10*, *30*, dequeue2 olacaktır *20*, *40*.

- *Durum 3: İşlem iptali ile sıralama Dequeue*

Yürütülen olan bir işlem durduruluyor sıranın head üzerinde öğeleri geri koyar dequeues. Hangi öğelerin sıra head üzerinde geri getirilme sırasını garanti edilmez. Bize aşağıdaki koda bakın:

```
using (var txn = this.StateManager.CreateTransaction())
{
    await this.Queue.TryDequeueAsync(txn, cancellationToken);
    await this.Queue.TryDequeueAsync(txn, cancellationToken);

    // Abort the transaction
    await txn.AbortAsync();
}
```
Öğeleri aşağıdaki sırayla kuyruktan çıkarıldı varsayın:
> 10, 20

Biz hareketi iptal, öğeleri aşağıdaki siparişleri hiçbirinde sırasının head dön eklenir:
> 10, 20

> 20, 10

Aynı işlem bulunduğu değil başarıyla tüm durumlarda geçerlidir *kabul edilen*.

## <a name="programming-patterns"></a>Programlama desenleri
Bu bölümde, bize en birkaç programlama ara desenleri ReliableConcurrentQueue kullanarak faydalı olabilir.

### <a name="batch-dequeues"></a>Toplu Dequeues
A önerilir programlama düzeni toplu tüketici görevi için bir gerçekleştirmek yerine dequeues aynı anda dequeue. Kullanıcının her toplu iş veya toplu iş boyutu arasındaki gecikmelerini kısıtlama seçebilirsiniz. Aşağıdaki kod parçacığını bu programlama modeli gösterir.  Bu örnekte unutmayın, işlem kaydedildikten sonra bir arıza işlenirken meydana olsaydı, işlenmemiş öğeleri işlenmemiş olmadan kaybolacak işlem yapılmaz.  Alternatif olarak, bu performansı üzerinde olumsuz bir etkisi olabilir ve önceden işlenmiş öğelerinin işleme gerektirir ancak işleme işlemdeki kapsamında yapılabilir.

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
Başka bir ilginç programlama modeli sayısı API kullanır. Burada, size en yüksek çaba bildirim tabanlı sıra için işlem uygulayabilirsiniz. Sıra sayısı bir sıraya veya bir dequeue görev kısıtlama için kullanılabilir.  İşlem dışında işleneceğini beri işleme sırasında bir hata oluşursa, önceki örnekte olduğu gibi işlenmemiş öğeleri kayıp olabileceğini unutmayın.

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
Sıranın boşaltmayı veri yapısı eşzamanlı yapısı nedeniyle garanti edilemez.  Bu olası TryDequeueAsync belirli bir çağrı, etkinliklerini sırada hiçbir kullanıcı işlemleri yürütülen olsa bile, sıraya alınan önceden olan bir öğeyi döndürmeyebilir ve kaydedilmiş olur.  Sıraya alınan öğe garanti *sonunda* dequeue için bir bant dışı iletişim mekanizması bağımsız bir tüketici sıranın kararlı durum tüm üreticileri durduruldu ve hiç yeni sıraya alma işlemlerine izin verildiğinden olsa bile ulaştı ancak bilemezsiniz görünür hale gelmiştir. Bu nedenle, boşaltma en altına uygulandığı gibi yüksek çaba işlemdir.

Kullanıcı tüm başka üretici ve tüketici görevleri durdurmak ve tamamlanmaya veya iptal, sıranın boşaltmak denemeden önce yürütülen işlemler için beklemeniz gerekir.  Kullanıcı beklenen kuyruğundaki öğelerin sayısı biliyorsa, bunlar tüm öğeleri kuyruktan çıkarıldı sinyalleri bir bildirim ayarlayalım ayarlayabilirsiniz.

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
ReliableConcurrentQueue sağlamaz *TryPeekAsync* API. Kullanıcıların alabilirsiniz gözlem anlamsal kullanarak bir *TryDequeueAsync* ve işlem iptal ediliyor. Bu örnekte, dequeues yalnızca öğenin değeri büyükse işlenen *10*.

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
* [Güvenilir hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [Güvenilir hizmetler bildirimleri](service-fabric-reliable-services-notifications.md)
* [Güvenilir hizmetler yedekleme ve geri yükleme (olağanüstü durum kurtarma)](service-fabric-reliable-services-backup-restore.md)
* [Güvenilir durum Yöneticisi yapılandırması](service-fabric-reliable-services-configuration.md)
* [Service Fabric Web API Hizmetleri'ni kullanmaya başlama](service-fabric-reliable-services-communication-webapi.md)
* [Gelişmiş kullanımını programlama modeli güvenilir hizmetler](service-fabric-reliable-services-advanced-usage.md)
* [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
