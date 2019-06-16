---
title: Azure Service Fabric aktör aktör temelli olayları | Microsoft Docs
description: Service Fabric Reliable Actors olayları giriş.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: aa01b0f7-8f88-403a-bfe1-5aba00312c24
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/06/2017
ms.author: amanbha
ms.openlocfilehash: 9075fc8391e8afa21e3963c1eff6a630c586d647
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60726409"
---
# <a name="actor-events"></a>Aktör olayları
Aktör olaylar istemciye aktör en yüksek çaba bildirimleri göndermek için bir yol sağlar. Aktör olayları aktör istemci iletişimi için tasarlanmıştır ve aktör aktör iletişimi için kullanılmamalıdır.

Aşağıdaki kod parçacıkları, aktör olayları, uygulamanızda kullanmayı göstermektedir.

Aktör yayımlanan olaylar açıklayan bir arabirim tanımlayın. Bu arabirim nesnesinden türetilmesi `IActorEvents` arabirimi. Yöntemlerin bağımsız değişkenlerini olmalıdır [veri sözleşme seri hale getirilebilir](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). Yöntem boş döndürmelidir, olay olarak bildirimleri bir şekilde ve en iyi çaba.

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
Aktör arabirimi, aktörün tarafından yayımlanan olaylar bildirin.

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
İstemci tarafında olay işleyicisini uygulayın.

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

İstemcisinde, olay yayımlayan aktör için bir ara sunucu oluşturabilir ve kendi olaylarına abone olma.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

Yük devretme durumunda, aktör üzerinden farklı bir işlem ya da düğümü başarısız olabilir. Aktör proxy etkin abonelikleri yöneten ve otomatik olarak yeniden abone. Yeniden abonelik aralığı aracılığıyla denetleyebilirsiniz `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API. Aboneliğinizi iptal etmek için kullanmak `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.

Gelişmelerden olayları aktör üzerinde yayımlayın. Aktörler çalışma zamanı aboneleri olay varsa, bunları bildirim gönderir.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a>Sonraki adımlar
* [Aktör'na yeniden giriş](service-fabric-reliable-actors-reentrancy.md)
* [Aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# örnek kod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [C# .NET Core örnek kodu](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [Java örnek kodu](https://github.com/Azure-Samples/service-fabric-java-getting-started)
