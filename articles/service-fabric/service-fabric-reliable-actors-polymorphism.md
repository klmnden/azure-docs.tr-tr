---
title: Reliable Actors Framework çok biçimlilik | Microsoft Docs
description: .NET arabirimleri ve işlevsellik ve API tanımlarını yeniden kullanmak için Reliable Actors Framework türleri hiyerarşiler
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: vturecek
ms.assetid: ef0eeff6-32b7-410d-ac69-87cba8b8fd46
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: c14b3006184f7bd6dcd1eb67be11bd0214957d72
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60725508"
---
# <a name="polymorphism-in-the-reliable-actors-framework"></a>Reliable Actors Framework çok biçimlilik
Reliable Actors framework birçok nesne yönelimli tasarım kullanacağınız aynı teknikleri kullanarak aktörler oluşturmanıza olanak tanır. Bu teknikler biri türlerine izin verir ve daha birçok devralmak için arabirimleri çok biçimlilik üst genelleştirilmiş. Reliable Actors Framework'te devralma genellikle birkaç ek kısıtlamalar ile .NET modelini izler. Java/Linux olması durumunda, Java modelini izler.

## <a name="interfaces"></a>Arabirimleri
Reliable Actors framework tarafından aktör türünüzün uygulanacak en az bir arabirim tanımlamanızı gerektirir. Bu arabirim, aktör ile iletişim kurmak için istemciler tarafından kullanılan bir proxy sınıfı oluşturmak için kullanılır. Arabirimleri IActor sonuçta bir aktör türü tarafından uygulanan her arabirim ve tüm üst öğelerinden türetilen sürece diğer arabirimlerinden devralır (C#) veya Actor(Java). IActor (C#) ve Actor(Java) .NET ve Java'dan çerçeveleri düzenindeki aktörler için platform tanımlı temel arabirimleri sırasıyla. Bu nedenle, Şekil kullanarak Klasik çok biçimlilik örnek şuna benzeyebilir:

![Şekil aktörler için hiyerarşi arabirimi][shapes-interface-hierarchy]

## <a name="types"></a>Türleri
Ayrıca platform tarafından sağlanan temel aktör sınıftan türetilen aktör türlerin hiyerarşisi oluşturabilirsiniz. Şekiller söz konusu olduğunda, bir taban sahip olabileceğiniz `Shape`(C#) veya `ShapeImpl`(Java) türü:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```
```Java
public abstract class ShapeImpl extends FabricActor implements Shape
{
    public abstract CompletableFuture<int> getVerticeCount();

    public abstract CompletableFuture<double> getAreaAsync();
}
```

' In subtypes `Shape`(C#) veya `ShapeImpl`(Java) taban yöntemleri geçersiz kılın.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```
```Java
@ActorServiceAttribute(name = "Circle")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class Circle extends ShapeImpl implements Circle
{
    @Override
    public CompletableFuture<Integer> getVerticeCount()
    {
        return CompletableFuture.completedFuture(0);
    }

    @Override
    public CompletableFuture<Double> getAreaAsync()
    {
        return (this.stateManager().getStateAsync<CircleState>("circle").thenApply(state->{
          return Math.PI * state.radius * state.radius;
        }));
    }
}
```

Not `ActorService` aktör türü özniteliği. Bu öznitelik güvenilir aktör çerçevesinde, bu tür aktörler barındırmak için bir hizmet otomatik olarak oluşturmasını söyler. Bazı durumlarda yalnızca işlevi alt türleri ile paylaşmak için tasarlanmıştır ve hiçbir zaman somut aktörler örneklemek için kullanılacak bir taban türü oluşturmak isteyebilirsiniz. Bu gibi durumlarda, kullanmanız gereken `abstract` hiçbir zaman söz konusu türüne göre bir aktör oluşturacağını göstermek için anahtar sözcüğü.

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [nasıl Reliable Actors framework Service Fabric platform yararlanır](service-fabric-reliable-actors-platform.md) güvenilirlik, ölçeklenebilirlik ve tutarlı bir duruma sağlamak için.
* Hakkında bilgi edinin [aktör yaşam döngüsü](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
