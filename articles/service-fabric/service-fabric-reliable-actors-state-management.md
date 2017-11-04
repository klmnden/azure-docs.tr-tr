---
title: "Güvenilir aktörler durum yönetimi | Microsoft Docs"
description: "Nasıl Reliable Actors durumu yönetilen kalıcı ve yüksek kullanılabilirlik için çoğaltılan açıklar."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: fae68c9fb40951e3f7a6fce67d75872cecfc52bd
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="reliable-actors-state-management"></a>Güvenilir aktörler durum yönetimi
Güvenilir aktörler mantığı ve durumu sarmalayabilen tek iş parçacıklı nesneleridir. Aktör Reliable Services üzerinde çalıştığından, bunlar durum güvenilir bir şekilde aynı kalıcılığını ve güvenilir hizmetler kullanan çoğaltma mekanizmalarını kullanarak koruyabilirsiniz. Bu şekilde çöp toplamadan sonra veya kaynak Dengeleme ya da yükseltme nedeniyle kümedeki düğümler arasında taşındıklarında yeniden etkinleştirme sırasında hatadan sonra durumlarına aktörler kaybetmeyin.

## <a name="state-persistence-and-replication"></a>Durum kalıcılığını ve çoğaltma
Tüm Reliable Actors kabul *durum bilgisi olan* her aktör örneği için benzersiz bir kimliği eşlendiğinden Bu, aynı aktör kimliği için yinelenen çağrıları aynı aktör örneğini yönlendirilir anlamına gelir. Durum bilgisi olmayan bir sistemde Bunun aksine, istemci çağrılarını her zaman aynı sunucuya yönlendirilecek garanti edilmez. Bu nedenle, aktör Hizmetleri zaman durum bilgisi olan hizmetleridir.

Aktörler stateful kabul edilen olsa bile, durum güvenilir bir şekilde depolamalısınız anlamına gelmez. Aktör durumunun kalıcı olmasını düzeyini seçebilirsiniz ve çoğaltma kendi veri depolama gereksinimleri dayalı:

* **Durum kalıcı**: durum kalıcı için disk ve 3 veya daha fazla çoğaltmalar için çoğaltılır. Durum tam küme kesinti burada kalıcı en sağlam durumu depolama seçenek budur.
* **Volatile durumu**: Durum 3 veya daha fazla çoğaltmalar için çoğaltılır ve yalnızca bellekte depolanır. Bu düğüm hatası ve aktör hatası karşı ve yükseltmeleri ve kaynak Dengeleme sırasında esnekliği sağlar. Ancak, durumu sürdürülmez diske. Tüm çoğaltmaları aynı anda kaybolursa, bu nedenle durumu da kaybolur.
* **Kalıcı durum yok**: durumu çoğaltılmış ya da yazılı için disk değil. Yalnızca güvenilir bir şekilde durumunu korumak üzere gerekmeyen aktörleri düzeydir.

Her Kalıcılık yalnızca farklı düzeyidir *durum sağlayıcısı* ve *çoğaltma* hizmetinizin yapılandırma. Durum desteklemediğini yazılır disk durumu sağlayıcısı--durumu depolar güvenilir bir hizmet bileşeni bağlıdır. Çoğaltma hizmeti kaç çoğaltmaları dağıtıldığından bağlıdır. Güvenilir hizmetleriyle gibi durum sağlayıcısı ve çoğaltma sayısı kolayca el ile ayarlanabilir. Aktör framework bir öznitelik, bir aktör üzerinde kullanılan, varsayılan durumu sağlayıcısı otomatik olarak seçer ve bu üç Kalıcılık ayarlarından birini elde etmek için yineleme sayısı için ayarları otomatik olarak oluşturur sağlar. StatePersistence özniteliği türetilmiş sınıf tarafından devralınır değil, her aktör türü StatePersistence düzeyiyle sağlamanız gerekir.

### <a name="persisted-state"></a>Kalıcı durum
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl  extends FabricActor implements MyActor
{
}
```  
Bu ayar, verilerin diskte depolar ve otomatik olarak hizmeti yineleme sayısı 3'e ayarlar durumu sağlayıcısı kullanır.

### <a name="volatile-state"></a>Volatile durumu
```csharp
[StatePersistence(StatePersistence.Volatile)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Volatile)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Bu ayar bir içindeki bellek-yalnızca durum sağlayıcısı kullanır ve çoğaltma sayısı 3'e ayarlar.

### <a name="no-persisted-state"></a>Kalıcı durum yok
```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.None)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
Bu ayar bir içindeki bellek-yalnızca durum sağlayıcısı kullanır ve çoğaltma sayısı 1'e ayarlar.

### <a name="defaults-and-generated-settings"></a>Varsayılanlar ve oluşturulan ayarları
Kullanırken `StatePersistence` öznitelik, bir durum sağlayıcısı otomatik olarak seçilir, çalışma zamanında aktör hizmeti başladığında. Çoğaltma sayısı, ancak, derleme zamanında Visual Studio aktör derleme araçları tarafından ayarlanır. Derleme araçları otomatik olarak oluşturacak bir *varsayılan hizmet* ApplicationManifest.xml aktör hizmeti için. Parametreler için oluşturulan **en küçük çoğaltma kümesi boyutu** ve **hedef çoğaltma kümesi boyutu**.

Bu parametreler el ile değiştirebilirsiniz. Ancak her zaman `StatePersistence` özniteliği değiştirildiğinde, varsayılan çoğaltma kümesi boyutu değerlere seçili parametrelerinin `StatePersistence` özniteliği, önceki tüm değerleri geçersiz kılma. Diğer bir deyişle, ServiceManifest.xml içinde ayarlanan değerler *yalnızca* değiştirdiğinizde derleme zamanında geçersiz kılınmış `StatePersistence` öznitelik değeri.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="MyActorService_PartitionCount" DefaultValue="10" />
      <Parameter Name="MyActorService_MinReplicaSetSize" DefaultValue="3" />
      <Parameter Name="MyActorService_TargetReplicaSetSize" DefaultValue="3" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyActorPkg" ServiceManifestVersion="1.0.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MyActorService" GeneratedIdRef="77d965dc-85fb-488c-bd06-c6c1fe29d593|Persisted">
         <StatefulService ServiceTypeName="MyActorServiceType" TargetReplicaSetSize="[MyActorService_TargetReplicaSetSize]" MinReplicaSetSize="[MyActorService_MinReplicaSetSize]">
            <UniformInt64Partition PartitionCount="[MyActorService_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
         </StatefulService>
      </Service>
   </DefaultServices>
</ApplicationManifest>
```

## <a name="state-manager"></a>Durum Yöneticisi
Kendi durum Yöneticisi her aktör örneği vardır: depoladıktan bir sözlük benzeri veri yapısı anahtar/değer çiftleri. Durum Yöneticisi durum sağlayıcısı çevresinde bir sarmalayıcı ' dir. Bağımsız olarak Kalıcılık ayarı kullanılan verileri depolamak için kullanabilirsiniz. Çalışan bir aktör hizmeti bir volatile (içindeki bellek-yalnızca) durumu ayarı verileri korurken yükseltme aracılığıyla kalıcı durum ayarı için değiştirilebilir garanti sağlamaz. Ancak, çalışan bir hizmetiniz için yineleme sayısı değiştirmek mümkündür.

Durum Yöneticisi anahtarları dize olmalıdır. Değerler genel ve herhangi bir türü olabilir özel türleri dahil olmak üzere. Diğer düğümlere ağ üzerinden çoğaltma sırasında iletilebilir ve bağlı olarak bir aktör'ın durum Kalıcılık ayarını diske yazılan çünkü durum Yöneticisi'nde depolanan değerleri veri sözleşmesi seri hale getirilebilir olmalıdır.

Durum Yöneticisi durum, güvenilir sözlükte bulunan benzer yönetmek için ortak sözlüğü yöntemlerini gösterir.

### <a name="accessing-state"></a>Erişim durumu
Durum anahtarının durum Yöneticisi aracılığıyla erişilebilir. Durum Yöneticisi yöntemlerini tüm zaman uyumsuz olduklarından aktörler durumu kalıcı olduğunda disk g/ç gerektirebilir. İlk erişim kaybedilmez durum nesneleri bellekte önbelleğe alınır. Erişim işlemleri erişimi nesneleri doğrudan bellekten yineleyin ve disk g/ç veya zaman uyumsuz bağlam geçişi yükü yansıtılmasını olmadan zaman uyumlu olarak döndürür. Bir durum nesnesi, aşağıdaki durumlarda önbellekten kaldırılır:

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

### <a name="saving-state"></a>Durum kaydetme
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

### <a name="removing-state"></a>Durum kaldırma
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
