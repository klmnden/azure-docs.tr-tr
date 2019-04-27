---
title: Azure Service Fabric aktör silme | Microsoft Docs
description: Service Fabric Reliable Actors ve bunların durumunu el ile silmeyi öğrenin.
services: service-fabric
documentationcenter: .net
author: amanbha
manager: chackdan
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: amanbha
ms.openlocfilehash: e297a6f42774f29e2eca4a410b695d5bbb636300
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60726613"
---
# <a name="delete-reliable-actors-and-their-state"></a>Reliable Actors ve durumlarını silme
Çöp toplama devre dışı bırakılan aktör yalnızca aktör nesnesi temizler, ancak bir aktörün durum Yöneticisi'nde depolanan verileri kaldırmaz. Aktörün etkinleştirildiğinde, verileri yeniden durum Yöneticisi için kullanılabilir hale getirilir. Burada actors durum Yöneticisi'nde veri depolamak ve devre dışı bırakıldı ancak hiçbir zaman yeniden durumlarda, kullanıcıların verileri temizlemek gerekebilir.

[Actor hizmetinin](service-fabric-reliable-actors-platform.md) aktörler uzak bir çağrıyı yapandan silmek için bir işlev sağlar:

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

Aktörün silme aktör şu anda etkin olup olmadığına bağlı olarak aşağıdaki etkileri gösterir:

* **Etkin aktör**
  * Aktör etkin aktörler listeden kaldırılır ve devre dışı bırakılır.
  * Durumunu kalıcı olarak silinir.
* **Etkin olmayan aktör**
  * Durumunu kalıcı olarak silinir.

Aktörün çağrılamıyor aktör yöntemlerinden birini kendisinden hangi çalışma zamanı elde kilit aktör çağrısı tek iş parçacıklı erişimi zorunlu etrafında bir aktör çağrısı bağlam içinde yürütülürken aktör silinemiyor çünkü silin.

Reliable Actors hakkında daha fazla bilgi için aşağıdakileri okuyun:
* [Aktör süreölçerler ve anımsatıcılar](service-fabric-reliable-actors-timers-reminders.md)
* [Aktör olayları](service-fabric-reliable-actors-events.md)
* [Aktör'na yeniden giriş](service-fabric-reliable-actors-reentrancy.md)
* [Aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# örnek kod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java örnek kodu](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
