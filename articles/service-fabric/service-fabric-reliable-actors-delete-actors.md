---
title: Azure Service Fabric aktör Sil | Microsoft Docs
description: Service Fabric Reliable Actors ve durumlarına el ile silmeniz öğrenin.
services: service-fabric
documentationcenter: .net
author: amanbha
manager: timlt
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: amanbha
ms.openlocfilehash: fa4fe018a9e6b32158f5bbd13c44ff57069cb1cf
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="delete-reliable-actors-and-their-state"></a>Reliable Actors ve durumlarına Sil
Çöp toplama devre dışı bırakılan aktör yalnızca aktör nesnesini temizler, ancak bir aktör ait durum Yöneticisi'nde depolanan verileri kaldırmaz. Bir oyuncu etkinleştirildiğinde, verileri yeniden durum Yöneticisi aracılığıyla için kullanılabilir hale getirilir. Burada aktörler durum Yöneticisi'nde veri depolamak ve devre dışı ancak hiçbir zaman yeniden durumlarda kendi verilerini temizle gerekebilir.

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

Bir oyuncu çağrılamıyor aktör yöntemlerinden birini kendisinden üzerinde aktör, çalışma zamanı elde kilit tek iş parçacıklı erişim uygulamaya aktör çağrısı geçici bir aktör çağrısı bağlamı içinde yürütülürken silinemez olduğundan silinemiyor.

Reliable Actors hakkında daha fazla bilgi için aşağıdaki okuyun:
* [Aktör zamanlayıcılar ve anımsatıcıları](service-fabric-reliable-actors-timers-reminders.md)
* [Aktör olayları](service-fabric-reliable-actors-events.md)
* [Aktör yeniden giriş](service-fabric-reliable-actors-reentrancy.md)
* [Aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# örnek kod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java örnek kod](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
