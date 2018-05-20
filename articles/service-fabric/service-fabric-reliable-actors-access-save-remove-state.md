---
title: Azure Service Fabric durumunu yönetme | Microsoft Docs
description: Erişim, kaydedin ve Service Fabric Reliable Actors durumu kaldırma hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: ac3afe144b9cf9e2fb307087edb175a603ffe4e9
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="access-save-and-remove-reliable-actors-state"></a>Erişim, kaydetme ve Reliable Actors durumunu kaldırın
[Güvenilir aktörler](service-fabric-reliable-actors-introduction.md) mantığı ve durumunu saklayan ve durumunu güvenilir bir şekilde korumak tek iş parçacıklı nesneleridir. Her aktör örneği kendi sahip [durum Yöneticisi](service-fabric-reliable-actors-state-management.md): depoladıktan bir sözlük benzeri veri yapısı anahtar/değer çiftleri. Durum Yöneticisi durum sağlayıcısı çevresinde bir sarmalayıcı ' dir. Hangi bağımsız olarak veri depolamak için kullanmak [Kalıcılık ayarı](service-fabric-reliable-actors-state-management.md#state-persistence-and-replication) kullanılır.

Durum Yöneticisi anahtarları dize olmalıdır. Değerler genel ve herhangi bir türü olabilir özel türleri dahil olmak üzere. Diğer düğümlere ağ üzerinden çoğaltma sırasında iletilebilir ve bağlı olarak bir aktör'ın durum Kalıcılık ayarını diske yazılan çünkü durum Yöneticisi'nde depolanan değerleri veri sözleşmesi seri hale getirilebilir olmalıdır.

Durum Yöneticisi durum, güvenilir sözlükte bulunan benzer yönetmek için ortak sözlüğü yöntemlerini gösterir.

Bilgi için bkz: [en iyi uygulamalar aktör durumunu yönetme](service-fabric-reliable-actors-state-management.md#best-practices).

## <a name="access-state"></a>Erişim durumu
Durum anahtarının durum Yöneticisi aracılığıyla erişilir. Durum Yöneticisi yöntemlerini tüm zaman uyumsuz olduklarından aktörler durumu kalıcı olduğunda disk g/ç gerektirebilir. İlk erişim kaybedilmez durum nesneleri bellekte önbelleğe alınır. Erişim işlemleri erişimi nesneleri doğrudan bellekten yineleyin ve disk g/ç veya zaman uyumsuz bağlam geçişi yükü yansıtılmasını olmadan zaman uyumlu olarak döndürür. Bir durum nesnesi, aşağıdaki durumlarda önbellekten kaldırılır:

* Durum Yöneticisi'nden bir nesneyi alır sonra aktör yöntemi işlenmeyen bir özel durum oluşturur.
* Bir aktör, devre dışı bırakılan sonra veya hatadan sonra yeniden başlatılır.
* Disk durumu sağlayıcısı sayfaları durumu. Bu davranışı üzerinde durumu sağlayıcısı uygulaması bağlıdır. Varsayılan durum sağlayıcısı için `Persisted` ayarının bu davranışı.

Standart bir kullanarak durumu alabilirsiniz *almak* oluşturur işlemi `KeyNotFoundException`(C#) veya `NoSuchElementException`(bir girdi için anahtar mevcut değilse Java):

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

Kullanarak durumu alabilirsiniz bir *TryGet* bir girdi için bir anahtar mevcut değilse oluşturmadığını yöntemi:

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

## <a name="save-state"></a>Durumu kaydedin
Durum Yöneticisi Alma yöntemlerini yerel bellekte bir nesneye başvuru döndürür. Bu nesne yerel bellek tek başına değiştirme işlemi kaydedilmesi için neden olmaz. Bir nesnenin durumu Yöneticisi'nden alınan ve değişiklik olduğunda işlemi kaydedilecek durumu Manager'a yeniden gerekir.

Bir koşulsuz kullanarak durumu ekleyebilirsiniz *ayarlamak*, denk olduğu `dictionary["key"] = value` sözdizimi:

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

Kullanarak durumu ekleyebilirsiniz bir *Ekle* yöntemi. Bu yöntem oluşturulur `InvalidOperationException`(C#) veya `IllegalStateException`(anahtar zaten eklemek çalıştığında Java) bulunmaktadır.

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

Kullanarak durumu ekleyebilirsiniz bir *TryAdd* yöntemi. Zaten bir anahtar eklemek çalıştığında, bu yöntem oluşturmadığını.

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

Aktör yöntemi sonunda, durum Yöneticisi'ni otomatik olarak eklenen veya bir INSERT veya update işlem tarafından değiştirilmiş tüm değerleri kaydeder. "Kaydet" diske ve kullanılan ayarlarına bağlı olarak çoğaltma kalıcı içerebilir. Değiştirilmemiş değerleri kalıcı veya çoğaltılan değil. Hiçbir değer değiştirildi, kaydetme işlemi hiçbir şey yapmaz. Başarısız kaydetme, değiştirilmiş durum atılır ve özgün duruma geri yüklenir.

Ayrıca durumu el ile çağırarak kaydedebilirsiniz `SaveStateAsync` aktör tabanında yöntemi:

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

## <a name="remove-state"></a>Durum Kaldır
Durum kalıcı olarak bir aktör'ın durum Yöneticisi'nden çağırarak kaldırabilirsiniz *kaldırmak* yöntemi. Bu yöntem oluşturulur `KeyNotFoundException`(C#) veya `NoSuchElementException`(mevcut olmayan bir anahtarı kaldırmak çalıştığında Java).

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

Ayrıca durumu kalıcı olarak kullanarak kaldırabilirsiniz *TryRemove* yöntemi. Mevcut bir anahtarı kaldırmak çalıştığında, bu yöntem oluşturmadığını.

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

Reliable Actors içinde depolanan durumu serileştirilmiş, diske yazılan ve yüksek kullanılabilirlik için çoğaltılmış önce. Daha fazla bilgi edinmek [aktör türü seri hale getirme](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

Ardından, daha fazla bilgi edinmek [aktör tanılama ve performans izlemesi](service-fabric-reliable-actors-diagnostics.md).
