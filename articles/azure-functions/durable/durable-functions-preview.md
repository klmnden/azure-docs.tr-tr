---
title: Dayanıklı işlevler Önizleme özellikleri - Azure işlevleri
description: Önizleme özellikleri hakkında bilgi edinmek için dayanıklı işlevler.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: article
ms.date: 07/08/2019
ms.author: azfuncdf
ms.openlocfilehash: 7101519aa4a87995dac3a7f11046eed84a2c09b6
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812773"
---
# <a name="durable-functions-20-preview-azure-functions"></a>Dayanıklı işlevler 2.0 Önizleme (Azure işlevleri)

*Dayanıklı işlevler* uzantısıdır [Azure işlevleri](../functions-overview.md) ve [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md) durum bilgisi olan işlevleri, sunucusuz bir ortamda yazmanızı sağlayan. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir. Dayanıklı işlevler ile bilginiz yok olup [genel bakış belgeleri](durable-functions-overview.md).

Dayanıklı işlevler 1.x Azure işlevleri, genel kullanım (genel kullanıma sunuldu) özelliğidir, ancak şu anda genel Önizleme aşamasında olan çeşitli özellikler de içerir. Bu makalede, yeni yayımlanmış Önizleme özelliklerini açıklar ve nasıl çalıştıkları ve bunları kullanarak nasıl başlatabilirsiniz ayrıntılarına geçer.

> [!NOTE]
> Bu önizleme özellikleri şu anda bir dayanıklı işlevler 2.0 sürümünün bir parçası olan bir **Önizleme kalite sürümü** ile çeşitli önemli değişiklikler. Azure işlevleri uzantı paketi derlemeleri kalıcı biçiminde sürümleriyle nuget.org bulunabilir **2.0.0-betaX**. Bu yapıları üretim iş yükleri için tasarlanmamıştır ve sonraki sürümlerde ek bozucu değişiklikleri içerebilir.

## <a name="breaking-changes"></a>Yeni değişiklikler

Birkaç önemli değişiklikler dayanıklı işlevler 2.0 sürümünde kullanıma sunulmuştur. Mevcut uygulamaları kod değişikliği yapmadan dayanıklı işlevler 2.0 ile uyumlu olması beklenmez. Bu bölümde, bazı değişiklikler listelenmiştir:

### <a name="hostjson-schema"></a>Host.JSON şeması

Aşağıdaki kod parçacığında, yeni şema host.json için gösterir. Dikkat edilmesi gereken ana değişiklikleri yeni alt bölümleri şunlardır:

* `"storageProvider"` (ve `"azureStorage"` alt) için depolama özgü yapılandırma
* `"tracking"` İzleme ve günlük kaydı yapılandırması
* `"notifications"` (ve `"eventGrid"` alt) için event grid bildirim yapılandırması

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "azureStorage": {
          "connectionStringName": <string>,
          "controlQueueBatchSize": <int?>,
          "partitionCount": <int?>,
          "controlQueueVisibilityTimeout": <hh:mm:ss?>,
          "workItemQueueVisibilityTimeout": <hh:mm:ss?>,
          "trackingStoreConnectionStringName": <string?>,
          "trackingStoreNamePrefix": <string?>,
          "maxQueuePollingInterval": <hh:mm:ss?>
        }
      },
      "tracking": {
        "traceInputsAndOutputs": <bool?>,
        "traceReplayEvents": <bool?>,
      },
      "notifications": {
        "eventGrid": {
          "topicEndpoint": <string?>,
          "keySettingName": <string?>,
          "publishRetryCount": <string?>,
          "publishRetryInterval": <hh:mm:ss?>,
          "publishRetryHttpStatus": <int[]?>,
          "publishEventTypes": <string[]?>,
        }
      },
      "maxConcurrentActivityFunctions": <int?>,
      "maxConcurrentOrchestratorFunctions": <int?>,
      "extendedSessionsEnabled": <bool?>,
      "extendedSessionIdleTimeoutInSeconds": <int?>,
      "customLifeCycleNotificationHelperType": <string?>
  }
}
```

İçin kararlı dayanıklı işlevler 2.0 devam ettikçe daha fazla değişiklik görülecektir `durableTask` host.json bölümü. Bu değişiklikler hakkında daha fazla bilgi için bkz. [bu GitHub sorunu](https://github.com/Azure/azure-functions-durable-extension/issues/641).

### <a name="public-interface-changes"></a>Ortak arabirim değişiklikleri

Dayanıklı işlevler tarafından desteklenen çeşitli "bağlam" nesneleri birim testi kullanımda yönelik olarak soyut taban sınıfları vardı. Dayanıklı işlevler 2.0 bir parçası olarak, bu soyut taban sınıfları, arabirimleri ile değiştirilmiştir. Doğrudan somut türleri kullanan işlev kodu etkilenmez.

Aşağıdaki tabloda ana değişiklikleri temsil eder:

| Eski türü | Yeni tür |
|----------|----------|
| DurableOrchestrationClientBase | IDurableOrchestrationClient |
| DurableOrchestrationContextBase | IDurableOrchestrationContext |
| DurableActivityContextBase | IDurableActivityContext |

Soyut bir temel sınıf sanal yöntemler bulunduğu durumlarda, bu sanal yöntemler içinde tanımlanan genişletme yöntemleri tarafından değiştirilmiştir `DurableContextExtensions`.

## <a name="entity-functions"></a>Varlık işlevleri

Varlık işlevleri tanımlayın okuma ve küçük parçaları olarak bilinen durumunu güncelleştirmek için işlemleri *dayanıklı varlıkları*. Bir özel tetikleyici türü işlevleriyle orchestrator işlevler gibi varlık işlevlerdir *varlık tetikleyici*. Orchestrator işlevleri farklı olarak, belirli bir kod kısıtlamalardan varlık işlevleri yoktur. Varlık işlevlerini de yönetmek durumu açıkça örtük olarak durumu aracılığıyla denetim akışını temsil eden yerine.

### <a name="net-programing-models"></a>.NET ölçeklenebilirliğinden modelleri

Dayanıklı varlıkları yazmak için iki isteğe bağlı programlama modeli vardır. Aşağıdaki kod, basit bir örnektir *sayacı* varlık standart bir işlev uygulanır. Bu işlev, üç tanımlar *işlemleri*, `add`, `reset`, ve `get`, her bir tamsayı durum değeri üzerinde çalıştığınız, `currentValue`.

```csharp
[FunctionName("Counter")]
public static void Counter([EntityTrigger] IDurableEntityContext ctx)
{
    int currentValue = ctx.GetState<int>();

    switch (ctx.OperationName.ToLowerInvariant())
    {
        case "add":
            int amount = ctx.GetInput<int>();
            currentValue += operand;
            break;
        case "reset":
            currentValue = 0;
            break;
        case "get":
            ctx.Return(currentValue);
            break;
    }

    ctx.SetState(currentValue);
}
```

Bu model, varlığın uygulamaları veya dinamik bir dizi işlemlerini sahip uygulamaları en iyi şekilde çalışır. Ancak, var. Ayrıca statik olan, ancak daha karmaşık uygulamalara sahip varlıklar için kullanışlı olan bir sınıf tabanlı programlama modeli Aşağıdaki örnek eşdeğer uygulamasıdır `Counter` varlık .NET sınıflar ve yöntemler kullanma.

```csharp
public class Counter
{
    [JsonProperty("value")]
    public int CurrentValue { get; set; }

    public void Add(int amount) => this.CurrentValue += amount;
    
    public void Reset() => this.CurrentValue = 0;
    
    public int Get() => this.CurrentValue;

    [FunctionName(nameof(Counter))]
    public static Task Run([EntityTrigger] IDurableEntityContext ctx)
        => ctx.DispatchAsync<Counter>();
}
```

Sınıf tabanlı model olarak popüler bir programlama modeli benzer [Orleans](https://www.microsoft.com/research/project/orleans-virtual-actors/). Bu modelde, bir varlık türü bir .NET sınıfı tanımlanır. Sınıfının her bir yöntemi, bir dış istemci tarafından çağrılabilen bir işlemdir. Orleans, ancak .NET arabirimleri isteğe bağlıdır. Önceki *sayacı* örnek bir arabirim kullanmayan ancak HTTP API çağrıları veya diğer işlevleri aracılığıyla yine de çağrılabilir.

Varlık *örnekleri* benzersiz bir tanımlayıcı erişilen *varlık kimliği*. Yalnızca bir varlık örneğini benzersiz şekilde tanımlayan bir çift dizelerden oluşan bir varlık kimliğidir. Şunlardan oluşur:

* Bir **varlık adı**: varlık türü (örneğin, "Sayaç") tanımlayan bir ad.
* Bir **Varlık anahtarı**: Varlık (örneğin, bir GUID) aynı ada sahip diğer tüm varlıkları arasında benzersiz olarak tanımlayan bir dize.

Örneğin, bir *sayacı* varlık işlevi çevrimiçi bir oyun puan tutmak için kullanılabilir. Her bir oyun örneğini bir benzersiz varlık kimliği gibi olacaktır `@Counter@Game1`, `@Counter@Game2`ve benzeri.

### <a name="comparison-with-virtual-actors"></a>Sanal aktörler ile karşılaştırma

Dayanıklı varlıkları tasarımını yoğun tarafından etkilenir [aktör modeli](https://en.wikipedia.org/wiki/Actor_model). Zaten Actors hizmetini kullanmaya hakkında bilginiz varsa, dayanıklı varlıkları kavramları alışık olduğunuz olmalıdır. Özellikle, dayanıklı varlıkları benzer [sanal aktörler](https://research.microsoft.com/projects/orleans/) birçok yönden:

* Aracılığıyla dayanıklı varlık adreslenebilir bir *varlık kimliği*.
* Dayanıklı entity operations seri olarak, yarış durumlarını önlemek için teker teker yürütün.
* Çağrılan veya sinyal dayanıklı varlıkları otomatik olarak oluşturulur.
* İşlemleri yürütülmüyor olduğunda dayanıklı sessizce bellekten varlıklardır.

Bazı önemli farklar vardır ancak olan çıkarabileceğini:

* Dayanıklı varlıkları öncelik *dayanıklılık* üzerinden *gecikme*ve bu nedenle katı gecikme süresi gereksinimlerine sahip uygulamalar için uygun olmayabilir.
* Varlıklar arasında gönderilen iletiler, güvenilir ve sipariş teslim edilir.
* Dayanıklı varlıklar dayanıklı düzenlemeleri ile birlikte kullanılabilir ve bu makalenin sonraki bölümlerinde anlatılan dağıtılmış kilitleri görebilir.
* İstek/yanıt desenleri varlıklarda düzenlemeleri için sınırlıdır. Varlık için varlık iletişim için yalnızca tek yönlü iletileri (diğer adıyla "sinyal"), olduğu gibi özgün aktör modeli izin verilir. Bu davranış, dağıtılmış kilitlenmeleri engeller.

### <a name="durable-entity-net-apis"></a>Dayanıklı Entity .NET API'leri

Çeşitli API'ler varlık desteği içerir. Biri için yoktur yeni bir API varlık işlevleri tanımlamak için yukarıda gösterildiği gibi bir işlem bir varlık üzerinde çağrıldığında ne olması gerektiğini belirtin. Ayrıca, istemciler ve düzenlemeleri için mevcut API'lere varlıklarıyla etkileşimi için yeni işlevler ile güncelleştirildi.

#### <a name="implementing-entity-operations"></a>Varlık işlemleri uygulama

Bir işlem bir varlığın yürütülmesi bağlam nesnesi üzerinde bu üyeleri çağırabilirsiniz (`IDurableEntityContext` . NET'te):

* **OperationName**: işlem adını alır.
* **GetInput\<TInput >** : işlem için giriş alır.
* **GetState\<TState >** : varlığın geçerli durumunu alır.
* **SetState**: Varlık durumunu güncelleştirir.
* **SignalEntity**: bir varlık için tek yönlü bir ileti gönderir.
* **Kendi kendine**: Varlık Kimliğini alır.
* **İade**: istemci veya işlemi çağıran düzenleme için bir değer döndürür.
* **IsNewlyConstructed**: döndürür `true` varlık önce işlemi olmasaydı.
* **DestructOnExit**: işlemi bittikten sonra varlık siler.

İşlemleri düzenlemeleri daha az sınırlı şunlardır:

* İşlem, dış g/ç (yalnızca zaman uyumsuz olanları kullanılması önerilir) zaman uyumlu veya zaman uyumsuz API'leri kullanarak, çağırabilirsiniz.
* İşlemleri belirleyici olabilir. Örneğin, çağırmak güvenli `DateTime.UtcNow`, `Guid.NewGuid()` veya `new Random()`.

#### <a name="accessing-entities-from-clients"></a>Varlıkları istemcilerden erişme

Dayanıklı varlıklar ile normal İşlevler'den çağrılacak `orchestrationClient` bağlama (`IDurableOrchestrationClient` .NET içinde). Aşağıdaki yöntemleri destekler:

* **ReadEntityStateAsync\<T >** : bir varlık durumunu okur.
* **SignalEntityAsync**: bir varlık için tek yönlü bir ileti gönderir ve sıraya alınan olmasını bekler.
* **SignalEntityAsync\<T >** : aynı `SignalEntityAsync` ancak bir oluşturulan proxy nesnesi türü kullanan `T`.

Önceki `SignalEntityAsync` çağrı gerektirir. varlık işlem adını belirterek bir `string` ve işlem yükünü bir `object`. Aşağıdaki örnek kod, bu düzen bir örneğidir:

```csharp
EntityId id = // ...
object amount = 5;
context.SignalEntityAsync(id, "Add", amount);
```

Tür kullanımı uyumlu erişim için bir proxy nesnesi oluşturmak mümkündür. Varlık türü, tür kullanımı uyumlu proxy oluşturmak için bir arabirim uygulamalıdır. Örneğin, varsayalım `Counter` varlık daha önce bahsedilen uygulanan bir `ICounter` arabirimi gibi tanımlanır:

```csharp
public interface ICounter
{
    void Add(int amount);
    void Reset();
    int Get();
}

public class Counter : ICounter
{
    // ...
}
```

İstemci kodu daha sonra kullanabileceğiniz `SignalEntityAsync<T>` belirtin `ICounter` tür parametresi tür kullanımı uyumlu proxy oluşturmak için arabirim. Bu tür kullanımı uyumlu proxy'leri kullanımı aşağıdaki kod örneğinde gösterilmiştir:

```csharp
[FunctionName("UserDeleteAvailable")]
public static async Task AddValueClient(
    [QueueTrigger("my-queue")] string message,
    [OrchestrationClient] IDurableOrchestrationClient client)
{
    int amount = int.Parse(message);
    var target = new EntityId(nameof(Counter), "MyCounter");
    await client.SignalEntityAsync<ICounter>(target, proxy => proxy.Add(amount));
}
```

Önceki örnekte, `proxy` parametresi, dinamik olarak üretilen bir örneğini `ICounter`, hangi dahili olarak çeviren çağrısı `Add` (türsüz) eşdeğer çağrısı `SignalEntityAsync`.

> [!NOTE]
> Dikkat etmeniz önemlidir `ReadEntityStateAsync` ve `SignalEntityAsync` yöntemlerinin `IDurableOrchestrationClient` performans tutarlılığa öncelik verin. `ReadEntityStateAsync` eski bir değer döndürebilir ve `SignalEntityAsync` işlemi bitmeden önce döndürebilir.

#### <a name="accessing-entities-from-orchestrations"></a>Düzenlemeleri erişen varlıkları

Düzenlemeleri kullanarak varlıkları erişebilir `IDurableOrchestrationContext` nesne. Bunlar arasında tek yönlü iletişim seçebilirsiniz (Başlat ve unut) ve çift yönlü iletişimi (istek ve yanıt). İlgili yöntemler şunlardır:

* **SignalEntity**: bir varlık için tek yönlü bir ileti gönderir.
* **CallEntityAsync**: bir varlık için bir ileti gönderir ve yanıt işleminin tamamlandığını belirten bir için bekler.
* **CallEntityAsync\<T >** : bir varlık için bir ileti gönderir ve bir sonuç t türü içeren bir yanıt bekler

Çift yönlü iletişimi kullanırken, işlemin yürütülmesi sırasında oluşturulan özel durumlar da için geri çağırma düzenleme aktarılan ve işlenemezse. Buna karşılık, Başlat ve unut kullanırken, özel durumlar gözlenmiştir değil.

Tür kullanımı uyumlu erişim için proxy tabanlı bir arabirimde düzenleme işlevler oluşturabilirsiniz. `CreateEntityProxy` Genişletme yöntemi bu amaç için kullanılabilir:

```csharp
public interface IAsyncCounter
{
    Task AddAsync(int amount);
    Task ResetAsync();
    Task<int> GetAsync();
}

[FunctionName("CounterOrchestration)]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    // ...
    IAsyncCounter proxy = context.CreateEntityProxy<IAsyncCounter>("MyCounter");
    await proxy.AddAsync(5);
    int newValue = await proxy.GetAsync();
    // ...
}
```

Önceki örnekte, "sayaç" varlık uygulayan mevcut varsayıldı `IAsyncCounter` arabirimi. Orchestration ardından kullanabilmek için `IAsyncCounter` zaman uyumlu bir varlıkla etkileşim kurmak için bir proxy tür oluşturma için tanım yazın.

### <a name="locking-entities-from-orchestrations"></a>Kilitleme varlıklardan düzenlemeleri

Düzenlemeleri varlıkları kilitleyebilirsiniz. Bu özelliği kullanarak istenmeyen yarışa hazır önlemek için basit bir yol sunar *kritik bölümler*.

Bağlam nesnesini aşağıdaki yöntemleri sağlar:

* **LockAsync**: bir veya daha fazla varlık kilitler alır.
* **IsLocked**: false olursa kritik bölüm, şu anda, true döndürür Aksi takdirde.

Kritik bölüm sona erer ve tüm kilitleri yayımlandığında, orchestration sona erdiğinde. . NET'te, `LockAsync` döndürür bir `IDisposable` ile birlikte kullanılabilecek atıldı, kritik bölüm biten bir `using` yan kritik bölüm söz dizimi bir gösterimini alır.

Örneğin, iki oyuncuların kullanılabilir olup olmadığını test etmek için gereken düzenleme göz önünde bulundurun ve bunları hem de oyun atayabilirsiniz. Bu görevi, kullanarak aşağıdaki gibi kritik bir bölüm uygulanabilir:

```csharp
[FunctionName("Orchestrator")]
public static async Task RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext ctx)
{
    EntityId player1 = /* ... */;
    EntityId player2 = /* ... */;

    using (await ctx.LockAsync(player1, player2))
    {
        bool available1 = await ctx.CallEntityAsync<bool>(player1, "is-available");
        bool available2 = await ctx.CallEntityAsync<bool>(player2, "is-available");

        if (available1 && available2)
        {
            Guid gameId = ctx.NewGuid();

            await ctx.CallEntityAsync(player1, "assign-game", gameId);
            await ctx.CallEntityAsync(player2, "assign-game", gameId);
        }
    }
}
```

Kritik bölüm içinde her iki player varlıklar, kritik bölüm içinde çağrılan olanlar dışındaki herhangi bir işlem yürütülürken yok anlamına gelir kilitlenir). Bu davranış, çakışan işlemleri yarışa hazır engeller, farklı bir atanan oynatıcıları gibi oyun ya da imzalama kapalı.

Biz birkaç kısıtlamaları dayatır ne kadar kritik bölümler üzerinde kullanılabilir. Bu kısıtlamalar, Kilitlenmeler ve yeniden giriş önlemek için hizmet.

* Kritik bölümler iç içe olamaz.
* Kritik bölümler suborchestrations oluşturulamıyor.
* Kritik bölümler yalnızca kilitli varlıklar çağırabilirsiniz.
* Kritik bölümler birden çok paralel çağrıları kullanarak aynı varlık çağrılamıyor.
* Kritik bölümler yalnızca kilitli varlıklar sinyal verebilirsiniz.

## <a name="alternate-storage-providers"></a>Alternatif depolama sağlayıcıları

Dayanıklı görev Framework dahil olmak üzere birden çok depolama sağlayıcıları bugün desteklemektedir. [Azure depolama](https://github.com/Azure/durabletask/tree/master/src/DurableTask.AzureStorage), [Azure Service Bus](https://github.com/Azure/durabletask/tree/master/src/DurableTask.ServiceBus)e [bellek içi öykünücü](https://github.com/Azure/durabletask/tree/master/src/DurableTask.Emulator)ve bir Deneysel [Redis](https://github.com/Azure/durabletask/tree/redis/src/DurableTask.Redis) sağlayıcısı. Ancak, şimdiye kadar Azure işlevleri için sürekli görev uzantısı yalnızca Azure depolama alanı Sağlayıcısı'desteklenir. Alternatif depolama sağlayıcıları için destek, dayanıklı işlevler 2.0 ile başlayarak, Redis sağlayıcı ile başlayan ekleniyor.

> [!NOTE]
> Dayanıklı işlevler 2.0, yalnızca .NET Standard 2.0 ile uyumlu sağlayıcılarını destekler. Yazma sırasında Azure Service Bus sağlayıcısı .NET Standard 2.0 desteklemiyor ve bu nedenle bir alternatif depolama sağlayıcısı olarak kullanılamaz.

### <a name="emulator"></a>Öykünücü

[DurableTask.Emulator](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Emulator/) sağlayıcısıdır yerel bellek, yerel test senaryoları için uygun olan dayanıklı olmayan depolama sağlayıcısı. Aşağıdaki en düşük kullanılarak yapılandırılabilir **host.json** şema:

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "emulator": { }
      }
    }
  }
}
```

### <a name="redis-experimental"></a>Redis (Deneysel)

[DurableTask.Redis](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Redis/) sağlayıcısı devam ederse, tüm düzenleme durumuna Redis küme yapılandırılır.

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "redis": {
          "connectionStringName": <string>,
        }
      }
    }
  }
}
```

`connectionStringName` Bir uygulama ayarı veya ortam değişkeni adı başvurmalıdır. Bu uygulama ayarı veya ortam değişkeni biçiminde bir Redis bağlantı dizesi değeri içermelidir *sunucu: BağlantıNoktası*. Örneğin, `localhost:6379` için yerel bir Redis kümesine bağlanma.

> [!NOTE]
> Redis sağlayıcı şu anda Deneysel ve yalnızca tek bir düğümde çalışan işlev uygulamaları destekler. Bu Redis sağlayıcısı hiç olmadığı kadar genel olarak kullanılabilir hale getirilir ve gelecekteki bir sürümde kaldırılabilir garanti edilmez.
