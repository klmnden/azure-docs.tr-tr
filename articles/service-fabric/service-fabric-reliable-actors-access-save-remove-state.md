---
title: Azure Service Fabric durumunu yönetme | Microsoft Docs
description: Kaydet ve Service Fabric Reliable Actors durum kaldırma erişimi öğrenin.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 7c10d00916ef65767c98616c7337bfa444c339a9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60725406"
---
# <a name="access-save-and-remove-reliable-actors-state"></a>Kaydet ve Reliable Actors durum kaldırma erişimi
[Reliable Actors](service-fabric-reliable-actors-introduction.md) hem mantıksal hem de durum kapsüllemek ve güvenilir bir şekilde durumunu korumak tek iş parçacıklı nesnelerdir. Her aktör örnek kendi bölümüne sahiptir [durum Yöneticisi](service-fabric-reliable-actors-state-management.md): güvenilir bir şekilde depolayan bir sözlüğü benzeri veri yapısı anahtar/değer çiftleri. Durum Yöneticisi durum sağlayıcısı çevresinde bir sarmalayıcı ' dir. Hangi verileri depolamak için kullanabileceğiniz [Kalıcılık ayarı](service-fabric-reliable-actors-state-management.md#state-persistence-and-replication) kullanılır.

Durum Yöneticisi anahtarları dize olmalıdır. Değerleri geneldir ve herhangi bir tür olabilir özel türler dahil. Durum Yöneticisi'nde depolanan değerleri, diğer düğümlerle ağ üzerinden çoğaltma sırasında iletilebilir ve, bir aktörün durumu Kalıcılık ayarlarına bağlı olarak diske yazılan çünkü veri sözleşme seri hale getirilebilir olması gerekir.

Durum Yöneticisi durum, güvenilir bir sözlükte bulunan benzer yönetmek için ortak sözlüğü yöntemleri sunar.

Bilgi için [en iyi uygulamalar aktör durumu yönetiminde](service-fabric-reliable-actors-state-management.md#best-practices).

## <a name="access-state"></a>Erişim durumu
Durum anahtarıyla durum Yöneticisi erişilir. Durum Yöneticisi yöntemlerini tüm zaman uyumsuz olduklarından durumu aktörlerini kalıcı, disk g/ç gerektirebilir. İlk erişim kaybedilmez durum nesneleri bellekte önbelleğe alınır. Doğrudan bellekten işlemleri erişim nesnelere erişme yinelemek ve disk g/ç veya zaman uyumsuz bağlam geçişi yükü olmaksızın zaman uyumlu olarak döndürür. Durum nesnesi, aşağıdaki durumlarda önbellekten kaldırılır:

* Durum Yöneticisi'nden nesneyi aldıktan sonra bir aktör yöntemi işlenmeyen özel durum oluşturur.
* Bir aktör, sonra devre dışı bırakılan veya hatadan sonra yeniden başlatılır.
* Disk durumu sağlayıcısı sayfaları durumu. Bu davranış durum sağlayıcısı uygulamasının bağlıdır. İçin varsayılan durumu sağlayıcısı `Persisted` ayarının bu davranışı.

Standart'ı kullanarak durumu alabilirsiniz *alma* oluşturan işlem `KeyNotFoundException`(C#) veya `NoSuchElementException`(Java) bir giriş için anahtar mevcut değilse:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<int> GetCountAsync()
    {
        return this.StateManager.GetStateAsync<int>("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().getStateAsync("MyState");
    }
}
```

Kullanarak durumu alabilirsiniz bir *TryGet* bir giriş için bir anahtar mevcut değilse oluşturmaz yöntemi:

```csharp
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task<int> GetCountAsync()
    {
        ConditionalValue<int> result = await this.StateManager.TryGetStateAsync<int>("MyState");
        if (result.HasValue)
        {
            return result.Value;
        }

        return 0;
    }
}
```
```Java
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().<Integer>tryGetStateAsync("MyState").thenApply(result -> {
            if (result.hasValue()) {
                return result.getValue();
            } else {
                return 0;
            });
    }
}
```

## <a name="save-state"></a>Durumu Kaydet
Durum Yöneticisi Alma yöntemlerini yerel belleğinde bir nesneye bir başvuru döndürür. Bu nesne tek başına yerel bellek değiştirme arızaya kaydedilmesi için neden olmaz. Bir nesne durum Yöneticisi'nden alınan ve değişiklik olduğunda arızaya kaydedilecek durum Yöneticisi'ne yeniden gerekir.

Bir koşulsuz kullanarak durumu ekleyebilirsiniz *ayarlamak*, denk olan `dictionary["key"] = value` söz dizimi:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task SetCountAsync(int value)
    {
        return this.StateManager.SetStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture setCountAsync(int value)
    {
        return this.stateManager().setStateAsync("MyState", value);
    }
}
```

Kullanarak durumu ekleyebilirsiniz bir *Ekle* yöntemi. Bu yöntem `InvalidOperationException`(C#) veya `IllegalStateException`(Java) zaten bir anahtar eklemek çalıştığında bulunmaktadır.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task AddCountAsync(int value)
    {
        return this.StateManager.AddStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().addOrUpdateStateAsync("MyState", value, (key, old_value) -> old_value + value);
    }
}
```

Kullanarak durumu ekleyebilirsiniz bir *TryAdd* yöntemi. Zaten bir anahtar eklemek çalıştığında, bu yöntem oluşturmaz.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task AddCountAsync(int value)
    {
        bool result = await this.StateManager.TryAddStateAsync<int>("MyState", value);

        if (result)
        {
            // Added successfully!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().tryAddStateAsync("MyState", value).thenApply((result)->{
            if(result)
            {
                // Added successfully!
            }
        });
    }
}
```

Durum Yöneticisi, bir aktör yönteminin sonunda, eklenen veya bir ekleme veya güncelleştirme işlemi tarafından değiştiren herhangi bir değeri otomatik olarak kaydeder. "Kaydet" diske ve kullanılan ayarlarına bağlı olarak, çoğaltma kalıcı içerebilir. Değiştirilmemiş değerleri kalıcı veya çoğaltılan değil. Değer değiştirildi, kaydetme işlemi hiçbir şey yapmaz. Kaydetme başarısız, değiştirilen durum atılır ve özgün durumuna geri yüklenir.

Ayrıca durumu el ile çağırarak kaydedebilirsiniz `SaveStateAsync` aktör taban yöntemi:

```csharp
async Task IMyActor.SetCountAsync(int count)
{
    await this.StateManager.AddOrUpdateStateAsync("count", count, (key, value) => count > value ? count : value);

    await this.SaveStateAsync();
}
```
```Java
interface MyActor {
    CompletableFuture setCountAsync(int count)
    {
        this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value).thenApply();

        this.stateManager().saveStateAsync().thenApply();
    }
}
```

## <a name="remove-state"></a>Durumu Kaldır
Durumu kalıcı olarak bir aktörün durum Yöneticisi'nden çağırarak kaldırabilirsiniz *Kaldır* yöntemi. Bu yöntem `KeyNotFoundException`(C#) veya `NoSuchElementException`mevcut olmayan bir anahtar kaldırmaya çalıştığında (Java).

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task RemoveCountAsync()
    {
        return this.StateManager.RemoveStateAsync("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().removeStateAsync("MyState");
    }
}
```

Ayrıca durumu kalıcı olarak kullanarak kaldırabilirsiniz *TryRemove* yöntemi. Bu yöntem, var olmayan bir anahtar kaldırmaya çalıştığında oluşturmaz.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task RemoveCountAsync()
    {
        bool result = await this.StateManager.TryRemoveStateAsync("MyState");

        if (result)
        {
            // State removed!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().tryRemoveStateAsync("MyState").thenApply((result)->{
            if(result)
            {
                // State removed!
            }
        });
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Reliable Actors içinde depolanan durumu serileştirilmiş önce diske yazılan ve yüksek kullanılabilirlik için çoğaltılır. Daha fazla bilgi edinin [aktör türü serileştirme](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

Ardından, daha fazla bilgi edinin [aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md).
