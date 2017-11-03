---
title: "Çok biçimlilik Reliable Actors Framework'te | Microsoft Docs"
description: "Hiyerarşileri .NET arabirimleri ve Reliable Actors framework türlerinde işlevselliği ve API tanımlarını yeniden oluşturun."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: vturecek
ms.assetid: ef0eeff6-32b7-410d-ac69-87cba8b8fd46
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 36a59a41b2261369a2062c76ef90aebf7e24a221
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="polymorphism-in-the-reliable-actors-framework"></a>Çok biçimlilik Reliable Actors Framework'te
Reliable Actors framework birçok nesne yönelimli tasarımında kullanacağınız aynı teknikleri kullanarak aktörler oluşturmanıza olanak sağlar. Bu teknikler biri türlerine izin verir ve'dan daha fazla devralmak için arabirimleri çok biçimlilik üst genelleştirilmiş. Reliable Actors Framework'te devralma genellikle birkaç ek kısıtlamalar .NET modeliyle izler. Java/Linux durumunda, Java modelini izler.

## <a name="interfaces"></a>Arabirimleri
Reliable Actors framework aktör türüne göre uygulanacak en az bir arabirim tanımlamanızı gerektirir. Bu arabirim, istemciler tarafından aktörler ile iletişim kurmak için kullanılan bir proxy sınıfı oluşturmak için kullanılır. Aktör türü tarafından uygulanan her arabirimi ve tüm üst öğelerinden sonuçta türetilen sürece IActor(C#) veya Actor(Java) arabirimleri diğer arabirimlerinden devralabilirsiniz. IActor(C#) ve Actor(Java) platform tarafından tanımlanan temel .NET ve Java çerçeveleri aktörler için sırasıyla arabirimlerdir. Bu nedenle, şekilleri kullanarak Klasik çok biçimlilik örnek şuna benzeyebilir:

![Hiyerarşi şekli aktörler için arabirim][shapes-interface-hierarchy]

## <a name="types"></a>Türler
Ayrıca, platform tarafından sağlanan temel aktör sınıfından türetilen aktör türlerinin hiyerarşisi oluşturabilirsiniz. Şekiller söz konusu olduğunda, bir taban olabilir `Shape`(C#) veya `ShapeImpl`(Java) türü:

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

Subtypes `Shape`(C#) veya `ShapeImpl`(Java) taban yöntemleri geçersiz kılın.

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

Not `ActorService` aktör türündeki özniteliği. Bu öznitelik güvenilir aktör framework aktörler bu tür barındırmak için bir hizmet otomatik olarak oluşturmasını söyler. Bazı durumlarda, yalnızca işlevselliği alt türleri ile paylaşmak için tasarlanmıştır ve hiçbir zaman somut aktörler örneği oluşturmak için kullanılacak bir taban türü oluşturmak isteyebilirsiniz. Bu durumda, kullanmanız gereken `abstract` hiçbir zaman bu türüne göre bir aktör oluşturacak göstermek için anahtar sözcüğü.

## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [nasıl Reliable Actors framework Service Fabric platformundan yararlanır](service-fabric-reliable-actors-platform.md) güvenilirlik, ölçeklenebilirlik ve tutarlı bir duruma sağlamak için.
* Hakkında bilgi edinin [aktör yaşam döngüsü](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
