---
title: Dayanıklı işlevler Önizleme özellikleri - Azure işlevleri
description: Önizleme özellikleri hakkında bilgi edinmek için dayanıklı işlevler.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.date: 04/23/2019
ms.author: azfuncdf
ms.openlocfilehash: 8ceb84ab9e9c41ff6a9cbde62571fb12ae67d790
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65596084"
---
# <a name="durable-functions-20-preview-azure-functions"></a>Dayanıklı işlevler 2.0 Önizleme (Azure işlevleri)

*Dayanıklı işlevler* uzantısıdır [Azure işlevleri](../functions-overview.md) ve [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md) durum bilgisi olan işlevleri, sunucusuz bir ortamda yazmanızı sağlayan. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir. Dayanıklı işlevler ile bilginiz yok olup [genel bakış belgeleri](durable-functions-overview.md).

Dayanıklı İşlevler, Azure işlevleri, genel kullanım (genel kullanıma sunuldu) özelliğidir, ancak şu anda genel Önizleme aşamasında olan çeşitli özellikler de içerir. Bu makalede, yeni yayımlanmış Önizleme özelliklerini açıklar ve nasıl çalıştıkları ve bunları kullanarak nasıl başlatabilirsiniz ayrıntılarına geçer.

> [!NOTE]
> Bu önizleme özellikleri şu anda bir dayanıklı işlevler 2.0 sürümünün bir parçası olan bir **alfa kalite yayın** ile çeşitli önemli değişiklikler. Azure işlevleri uzantı paketi derlemeleri kalıcı biçiminde sürümleriyle nuget.org bulunabilir **2.0.0-alpha**. Bu yapılar, tüm üretim iş yükleri için uygun değildir ve sonraki sürümlerde ek bozucu değişiklikleri içerebilir.

## <a name="breaking-changes"></a>Hataya neden olan değişiklikler

Birkaç önemli değişiklikler dayanıklı işlevler 2.0 sürümünde kullanıma sunulmuştur. Mevcut uygulamaları kod değişikliği yapmadan dayanıklı işlevler 2.0 ile uyumlu olması beklenmez. Bu bölümde, bazı değişiklikler listelenmiştir:

### <a name="dropping-net-framework-support"></a>.NET Framework desteği bırakılıyor

.NET Framework (ve bu nedenle işlevleri 1.0) desteği için dayanıklı işlevler 2.0 bırakıldı. Birincil nedeni kolayca oluşturun ve dayanıklı işlevler için macOS ve Linux platformlarındaki yaptığınız değişiklikleri test etmek Windows olmayan katkıda bulunanlar etkinleştirmektir. İkincil nedeni, geliştiricilerin Azure işlevleri çalışma zamanı en son sürümüne taşımak için teşvik etmeye yardımcı olması için olmasıdır.

### <a name="hostjson-schema"></a>Host.JSON şeması

Aşağıdaki kod parçacığında, yeni şema host.json için gösterir. Dikkat edilmesi gereken ana değişiklik yenilikler `"storageProvider"` bölümünde ve `"azureStorage"` bölümü altındaki. Desteklemek için bu değişiklik yapıldı [alternatif depolama sağlayıcıları](durable-functions-preview.md#alternate-storage-providers).

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
      "maxConcurrentActivityFunctions": <int?>,
      "maxConcurrentOrchestratorFunctions": <int?>,
      "traceInputAndOutputs": <bool?>,
      "eventGridTopicEndpoint": <string?>,
      "eventGridKeySettingName": <string?>,
      "eventGridPublishRetryCount": <string?>,
      "eventGridPublishRetryInterval": <hh:mm:ss?>,
      "eventGridPublishRetryHttpStatus": <int[]?>,
      "eventgridPublishEventTypes": <string[]?>,
      "customLifeCycleNotificationHelperType"
      "extendedSessionsEnabled": <bool?>,
      "extendedSessionIdleTimeoutInSeconds": <int?>,
      "logReplayEvents": <bool?>
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

Aşağıdaki kodu tanımlayan bir varlığın işlev örneğidir bir *sayacı* varlık. İşlev üç operations tanımlar `add`, `subtract`, ve `reset`, her hangi bir tamsayı değeri güncelleştirmesi `currentValue`.

```csharp
[FunctionName("Counter")]
public static async Task Counter(
    [EntityTrigger] IDurableEntityContext ctx)
{
    int currentValue = ctx.GetState<int>();
    int operand = ctx.GetInput<int>();

    switch (ctx.OperationName)
    {
        case "add":
            currentValue += operand;
            break;
        case "subtract":
            currentValue -= operand;
            break;
        case "reset":
            await SendResetNotificationAsync();
            currentValue = 0;
            break;
    }

    ctx.SetState(currentValue);
}
```

Varlık *örnekleri* benzersiz bir tanımlayıcı erişilen *varlık kimliği*. Yalnızca bir varlık örneğini benzersiz şekilde tanımlayan bir çift dizelerden oluşan bir varlık kimliğidir. Şunlardan oluşur:

1. bir **varlık adı**: varlık türü (örneğin, "Sayaç") tanımlayan bir ad
2. bir **Varlık anahtarı**: Varlık (örneğin, bir GUID) aynı ada sahip diğer tüm varlıkları arasında benzersiz olarak tanımlayan bir dize

Örneğin, bir *sayacı* varlık işlevi çevrimiçi bir oyun puan tutmak için kullanılabilir. Her bir oyun örneğini bir benzersiz varlık kimliği gibi olacaktır `@Counter@Game1`, `@Counter@Game2`ve benzeri.

### <a name="comparison-with-virtual-actors"></a>Sanal aktörler ile karşılaştırma

Dayanıklı varlıkları tasarımını yoğun tarafından etkilenir [aktör modeli](https://en.wikipedia.org/wiki/Actor_model). Zaten Actors hizmetini kullanmaya hakkında bilginiz varsa, dayanıklı varlıkları kavramları alışık olduğunuz olmalıdır. Özellikle, dayanıklı varlıkları benzer [sanal aktörler](https://research.microsoft.com/en-us/projects/orleans/) birçok yönden:

* Aracılığıyla dayanıklı varlık adreslenebilir bir *varlık kimliği*.
* Dayanıklı entity operations seri olarak, yarış durumlarını önlemek için teker teker yürütün.
* Çağrılan veya sinyal dayanıklı varlıkları otomatik olarak oluşturulur.
* İşlemleri yürütülmüyor olduğunda dayanıklı sessizce bellekten varlıklardır.

Bazı önemli farklar vardır ancak olan çıkarabileceğini:

* Dayanıklı varlıklar saf işlevler modellenir. Bu tasarım, dile özgü destek sınıfları, özellikleri ve yöntemleri kullanarak aktörler temsil eden nesne yönelimli çoğu çerçeveleri farklıdır.
* Dayanıklı varlıkları öncelik *dayanıklılık* üzerinden *gecikme*ve bu nedenle katı gecikme süresi gereksinimlerine sahip uygulamalar için uygun olmayabilir.
* Varlıklar arasında gönderilen iletiler, güvenilir ve sipariş teslim edilir.
* Dayanıklı varlıklar dayanıklı düzenlemeleri ile birlikte kullanılabilir ve bu makalenin sonraki bölümlerinde anlatılan dağıtılmış kilitleri görebilir.
* İstek/yanıt desenleri varlıklarda düzenlemeleri için sınırlıdır. Varlık için varlık iletişim için yalnızca tek yönlü iletileri (diğer adıyla "sinyal"), olduğu gibi özgün aktör modeli izin verilir. Bu davranış, dağıtılmış kilitlenmeleri engeller.

### <a name="durable-entity-apis"></a>Dayanıklı Entity API'leri

Çeşitli API'ler varlık desteği içerir. Biri için yoktur yeni bir API varlık işlevleri tanımlamak için yukarıda gösterildiği gibi bir işlem bir varlık üzerinde çağrıldığında ne olması gerektiğini belirtin. Ayrıca, istemciler ve düzenlemeleri için mevcut API'lere varlıklarıyla etkileşimi için yeni işlevler ile güncelleştirildi.

### <a name="implementing-entity-operations"></a>Varlık işlemleri uygulama

Bir işlem bir varlığın yürütülmesi bağlam nesnesi üzerinde bu üyeleri çağırabilirsiniz (`IDurableEntityContext` . NET'te):

* **OperationName**: işlem adını alır.
* **GetInput\<T >**: işlem için giriş alır.
* **GetState\<T >**: varlığın geçerli durumunu alır.
* **SetState**: Varlık durumunu güncelleştirir.
* **SignalEntity**: bir varlık için tek yönlü bir ileti gönderir.
* **Kendi kendine**: Varlık Kimliğini alır.
* **İade**: istemci veya işlemi çağıran düzenleme için bir değer döndürür.
* **IsNewlyConstructed**: döndürür `true` varlık önce işlemi olmasaydı.
* **DestructOnExit**: işlemi bittikten sonra varlık siler.

İşlemleri düzenlemeleri daha az sınırlı şunlardır:

* İşlem, dış g/ç (yalnızca zaman uyumsuz olanları kullanılması önerilir) zaman uyumlu veya zaman uyumsuz API'leri kullanarak, çağırabilirsiniz.
* İşlemleri belirleyici olabilir. Örneğin, çağırmak güvenli `DateTime.UtcNow`, `Guid.NewGuid()` veya `new Random()`.

### <a name="accessing-entities-from-clients"></a>Varlıkları istemcilerden erişme

Dayanıklı varlıklar ile normal İşlevler'den çağrılacak `orchestrationClient` bağlama (`IDurableOrchestrationClient` .NET içinde). Aşağıdaki yöntemleri destekler:

* **ReadEntityStateAsync\<T >**: bir varlık durumunu okur.
* **SignalEntityAsync**: bir varlık için tek yönlü bir ileti gönderir ve sıraya alınan olmasını bekler.

Bu yöntemler performans tutarlılığa öncelik: `ReadEntityStateAsync` eski bir değer döndürebilir ve `SignalEntityAsync` işlemi bitmeden önce döndürebilir. Buna karşılık, varlıkları (sonraki bölümde açıklandığı gibi) düzenlemeleri çağırma kesinlikle tutarlı olur.

### <a name="accessing-entities-from-orchestrations"></a>Düzenlemeleri erişen varlıkları

Düzenlemeleri bağlam nesnesini kullanarak varlıkları erişebilirsiniz. Bunlar arasında tek yönlü iletişim seçebilirsiniz (Başlat ve unut) ve çift yönlü iletişimi (istek ve yanıt). İlgili yöntemleri

* **SignalEntity**: bir varlık için tek yönlü bir ileti gönderir.
* **CallEntityAsync**: bir varlık için bir ileti gönderir ve yanıt işleminin tamamlandığını belirten bir için bekler.
* **CallEntityAsync\<T >**: bir varlık için bir ileti gönderir ve bir sonuç t türü içeren bir yanıt bekler

Çift yönlü iletişimi kullanırken, işlemin yürütülmesi sırasında oluşturulan özel durumlar da için geri çağırma düzenleme aktarılan ve işlenemezse. Buna karşılık, Başlat ve unut kullanırken, özel durumlar gözlenmiştir değil.

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
> Redis sağlayıcı şu anda Deneysel ve yalnızca tek bir düğümde çalışan işlev uygulamaları destekler.
