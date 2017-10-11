---
title: "Aktör tabanlı Azure mikro olayları | Microsoft Docs"
description: "Service Fabric Reliable Actors olayları için giriş."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: aa01b0f7-8f88-403a-bfe1-5aba00312c24
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: amanbha
ms.openlocfilehash: d936670c548ff709fc2e935d3f28d94e4bde8a04
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="actor-events"></a>Aktör olayları
Aktör olayları en yüksek çaba bildirim oyuncusu istemcilere göndermek için bir yol sağlar. Aktör olayları aktör istemci iletişimi için tasarlanmıştır ve aktör aktör iletişimi için kullanılmaması gerekir.

Aşağıdaki kod parçacıkları, uygulamanızda aktör olayları kullanmayı gösterir.

Aktör tarafından yayımlanan olayları tanımlayan bir arabirim tanımlayın. Bu arabirim öğesinden türetilmelidir `IActorEvents` arabirimi. Bağımsız değişkenler yöntemlerin olmalıdır [veri sözleşmesi seri hale getirilebilir](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). Yöntemleri boş döndürmeleri gerektiği, bir yolu ve en iyi çaba bildirimleri olayı olarak olur.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```
```Java
public interface GameEvents implements ActorEvents
{
    void gameScoreUpdated(UUID gameId, String currentScore);
}
```
Aktör arabiriminde aktör tarafından yayımlanan olayları bildirme.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```
```Java
public interface GameActor extends Actor, ActorEventPublisherE<GameEvents>
{
    CompletableFuture<?> updateGameStatus(GameStatus status);

    CompletableFuture<String> getGameScore();
}
```
İstemci tarafında olay işleyicisi uygulayın.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

```Java
class GameEventsHandler implements GameEvents {
    public void gameScoreUpdated(UUID gameId, String currentScore)
    {
        System.out.println("Updates: Game: "+gameId+" ,Score: "+currentScore);
    }
}
```

İstemci üzerindeki olay yayımlar aktör proxy'sine oluşturun ve kendi olaylarına abone olma.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

Yük devretme durumunda aktör üzerinden farklı işlem ya da düğüm başarısız olabilir. Aktör proxy etkin abonelikleri yönetir ve otomatik olarak yeniden abone. Aracılığıyla yeniden abonelik aralığı kontrol edebilirsiniz `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API. Aboneliğinizi iptal etmek için kullanmak `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.

Sırada aktör üzerinde olayları yalnızca yayımlayın. Aktör çalışma zamanı olay abonelere varsa, bunları bildirim gönderir.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a>Sonraki adımlar
* [Aktör yeniden giriş](service-fabric-reliable-actors-reentrancy.md)
* [Aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# örnek kod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [C# .NET Core örnek kod](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [Java örnek kod](http://github.com/Azure-Samples/service-fabric-java-getting-started)
