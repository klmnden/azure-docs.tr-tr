---
title: "Aktör tabanlı Azure mikro yaşam döngüsüne genel bakış | Microsoft Docs"
description: "Service Fabric güvenilir aktör yaşam döngüsü, atık toplama ve aktörler ve durumlarına el ile silinmesi açıklanmaktadır"
services: service-fabric
documentationcenter: .net
author: amanbha
manager: timlt
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/06/2017
ms.author: amanbha
ms.openlocfilehash: d49afd9e5cfe80ddc2d919c76eaa0cb168280c15
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2017
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a>Aktör yaşam döngüsü, otomatik çöp toplama ve el ile silme
Bir oyuncu yöntemlerinden herhangi biri için bir çağrı yapılır ilk kez etkinleştirilir. Bir oyuncu devre dışı bırakılmış (Çöp aktörler çalışma zamanı tarafından toplanan) ise yapılandırılabilir bir süre için kullanılmaz. Bir aktör ve durumu da el ile herhangi bir zamanda silinebilir.

## <a name="actor-activation"></a>Aktör etkinleştirme
Bir oyuncu etkinleştirildiğinde, aşağıdakiler gerçekleşir:

* Bir oyuncu için bir çağrı gelir ve bir etkin değilse, yeni aktör oluşturulur.
* Durum koruma aktör'in durumu yüklenir.
* `OnActivateAsync` (C#) veya `onActivateAsync` (hangi aktör uygulamasında kılınabilir) (Java) yöntemi çağrılır.
* Aktör şimdi etkin olarak kabul edilir.

## <a name="actor-deactivation"></a>Aktör devre dışı bırakma
Bir oyuncu devre dışı bırakıldığında, aşağıdakiler gerçekleşir:

* Belirli bir süre için bir aktör kullanılmadığı zaman etkin aktörler tablosundan kaldırılır.
* `OnDeactivateAsync` (C#) veya `onDeactivateAsync` (hangi aktör uygulamasında kılınabilir) (Java) yöntemi çağrılır. Aktör için tüm zamanlayıcılar temizler. Aktör işlemleri değişiklikler bu yönteminden çağrılmamalıdır durumu ister.

> [!TIP]
> Fabric aktör çalışma zamanı bazı yayar [olayları ilgili aktör etkinleştirme ve devre dışı bırakma](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters). Bunlar, tanılama ve performans izlemesi kullanışlıdır.
>
>

### <a name="actor-garbage-collection"></a>Aktör çöp toplama
Bir oyuncu devre dışı bırakıldığında aktör nesne başvuruları yayımlanan ve normalde ortak dil çalışma zamanı (CLR) veya java sanal makinesi (JVM) atık toplayıcı tarafından toplanacak olabilir. Çöp toplama yalnızca aktör nesnesini temizler; Mevcut **değil** aktör ait durum Yöneticisi'nde depolanan durumunu kaldırın. Aktör etkinleştirilir, sonraki açışınızda yeni bir aktör nesnesi oluşturulur ve durumu geri.

Ne "devre dışı bırakma ve atık toplama amacıyla kullanılan olarak" sayar?

* Çağrı Alma
* `IRemindable.ReceiveReminderAsync`(yalnızca aktör anımsatıcıları kullanıyorsa geçerlidir) çağrılan yöntemi

> [!NOTE]
> Aktör zamanlayıcılar kullanıyorsa ve Zamanlayıcı geri çağırma çağrılır, mevcut **değil** "kullanılan" olarak sayısı.
>
>

Biz devre dışı bırakma ayrıntıları geçmeden önce aşağıdaki koşulları tanımlamak önemlidir:

* *Tarama aralığı*. Withintext aktörler çalışma zamanı, etkin aktörler tablosunu devre dışı bırakılabilir aktörler için tarar ve atık toplanan aralığını budur. Bu varsayılan değeri 1 dakikadır.
* *Boşta kalma zaman aşımı*. Bu bir aktör kullanılmayan kalması gereken süreyi belirtir (boş) devre dışı bırakılabilir ve atık toplanan önce. Bunun için varsayılan değer 60 dakikadır.

Genellikle, bu varsayılan değişiklik gerekmez. Ancak, gerekirse, bu aralıklar üzerinden değiştirilebilir `ActorServiceSettings` kaydederken, [aktör hizmeti](service-fabric-reliable-actors-platform.md):

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        ActorRuntime.RegisterActorAsync<MyActor>((context, actorType) =>
                new ActorService(context, actorType,
                    settings:
                        new ActorServiceSettings()
                        {
                            ActorGarbageCollectionSettings =
                                new ActorGarbageCollectionSettings(10, 2)
                        }))
            .GetAwaiter()
            .GetResult();
    }
}
```

```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
    }
}
```
Her etkin aktör aktör çalışma zamanı, (yani kullanılır) boşta olduğu süre miktarını izler. Aktör çalışma zamanı her aktörler denetler her `ScanIntervalInSeconds` , çöp toplanır ve boşta olması durumunda topladığı olup olmadığını görmek için `IdleTimeoutInSeconds`.

Bir oyuncu kullanılan zaman boşta kalma süresini 0 olarak sıfırlanır. Bundan sonra yalnızca onu yeniden boşta kalırsa toplanacak aktör olabilir `IdleTimeoutInSeconds`. Bir oyuncu aktör arabirim yöntemi veya aktör anımsatıcı geri çağırma yürütülürse kullanılan değerlendirilir çağırma. Bir oyuncu olan **değil** Zamanlayıcı geri çağırma çalıştırıldığında kullanılan düşünülür.

Aşağıdaki diyagramda bu kavramları göstermek için tek bir aktör yaşam döngüsü gösterilmektedir.

![Boşta kalma süresi örneği][1]

Örnek bu aktör lifetime öğesine aktör yöntem çağrıları, anımsatıcı ve zamanlayıcılar etkisini gösterir. Aşağıdaki noktaları örnek hakkında tümleştirilmediği şunlardır:

* ScanInterval ve IdleTimeout 5 ve 10 sırasıyla ayarlanır. (Yalnızca kavramı göstermek için bu konudaki Hedefimiz olduğundan birimleri burada önemli değildir.)
* Toplanacak aktörler için tarama T = 0, 5, 10, 15, 20, 25 5 tarama aralığı tarafından tanımlandığı şekilde gerçekleşir.
* T = konumundaki 4, 8, 12, 16, 20, 24, düzenli bir süreölçeri başlatılır ve kendi geri çağırmayı yürütür. Aktör boşta kalma süresi etkilemez.
* Aktör yöntem çağrısı T = 7, boşta kalma süresi 0 olarak sıfırlar ve aktör çöp koleksiyonu geciktirir.
* Aktör anımsatıcı geri çağırma T = 14 yürütür ve daha fazla aktör çöp koleksiyonu geciktirir.
* Çöp toplama tarama T = 25 adresindeki sırasında aktör'ın boşta kalma süresi son 10 boşta kalma zaman aşımı aşıyor ve aktör toplanacak olan.

Bir oyuncu hiçbir zaman bu yöntemin yürütülmesi için ne kadar süre olsun yöntemlerinden birini yürütülürken toplanacak olmaz. Daha önce belirtildiği gibi aktör arabirim yöntemleri ve anımsatıcı geri aramalar yürütülmesini aktör'ın boşta kalma süresi 0 olarak sıfırlayarak çöp toplama engeller. Zamanlayıcı geri aramalar yürütülmesi boşta kalma süresi 0 olarak sıfırlamaz. Ancak, aktör çöp koleksiyonu Zamanlayıcı geri yürütme tamamlanana kadar ertelenir.

## <a name="deleting-actors-and-their-state"></a>Aktör ve durumlarına silme
Çöp toplama devre dışı bırakılan aktör yalnızca aktör nesnesini temizler, ancak bir aktör ait durum Yöneticisi'nde depolanan verileri kaldırmaz. Bir aktör yeniden etkinleştirildiğinde, verileri yeniden durum Yöneticisi aracılığıyla için kullanılabilir hale getirilir. Burada aktörler durum Yöneticisi'nde veri depolamak ve devre dışı ancak hiçbir zaman yeniden etkinleştirilmiş durumda, kendi verilerini temizle gerekebilir.

[Aktör hizmeti](service-fabric-reliable-actors-platform.md) aktörler uzak çağrıyı yapandan silmek için bir işlev sağlar:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

Bir oyuncu silme aktör şu anda etkin olan olup olmadığına bağlı olarak aşağıdaki etkileri gösterir:

* **Etkin aktör**
  * Aktör etkin aktörler listesinden kaldırılır ve devre dışı bırakılır.
  * Durumu kalıcı olarak silinir.
* **Etkin olmayan aktör**
  * Durumu kalıcı olarak silinir.

Bir oyuncu çağrılamıyor Not Sil aktör yöntemlerinden birini kendisinden aktör çalışma zamanı tek iş parçacıklı erişim uygulamaya aktör çağrısı geçici bir kilidi elde bir aktör çağrısı bağlamı içinde yürütülürken silinemez çünkü.

## <a name="next-steps"></a>Sonraki adımlar
* [Aktör zamanlayıcılar ve anımsatıcıları](service-fabric-reliable-actors-timers-reminders.md)
* [Aktör olayları](service-fabric-reliable-actors-events.md)
* [Aktör yeniden giriş](service-fabric-reliable-actors-reentrancy.md)
* [Aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# örnek kod](https://github.com/Azure/servicefabric-samples)
* [Java örnek kod](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
