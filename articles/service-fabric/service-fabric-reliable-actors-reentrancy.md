---
title: "Aktör tabanlı Azure mikro yeniden girişi | Microsoft Docs"
description: "Service Fabric Reliable Actors yeniden giriş için giriş"
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: be23464a-0eea-4eca-ae5a-2e1b650d365e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 00fcccb379bf1ba3875fbaba57a05b00fa228622
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="reliable-actors-reentrancy"></a>Güvenilir aktörler yeniden giriş
Reliable Actors çalışma zamanı varsayılan olarak, mantıksal çağrısı Bağlam tabanlı yeniden giriş sağlar. Bu aynı çağrı bağlam zincirinde olmaları durumunda desteklemeyeceğini olmasını aktörler sağlar. Örneğin, A aktör aktör C'ye ileti gönderen aktör B'ye ileti gönderir Aktör C aktör A çağırırsa izin verecek şekilde ileti işleme bir parçası olarak desteklemeyeceğini, iletisidir. İşlem tamamlanana kadar farklı çağrı içeriği parçası olan diğer tüm iletileri aktör A engellenir.

İki seçenek tanımlanan aktör yeniden giriş için kullanılabilir `ActorReentrancyMode` enum:

* `LogicalCallContext`(varsayılan davranış)
* `Disallowed`-yeniden giriş devre dışı bırakır

```csharp
public enum ActorReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```
```Java
public enum ActorReentrancyMode
{
    LogicalCallContext(1),
    Disallowed(2)
}
```
Yeniden giriş yapılandırılabilir bir `ActorService`'s kayıt sırasında ayarları. Aktör hizmeti oluşturulan tüm aktör örnekleri ayar uygulanır.

Aşağıdaki örnek, yeniden giriş modunu ayarlar için bir aktör hizmeti gösterir `ActorReentrancyMode.Disallowed`. Bu durumda, bir aktör başka bir aktör, türünde bir özel durum desteklemeyeceğini iletisi gönderirse `FabricException` oluşturulur.

```csharp
static class Program
{
    static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<Actor1>(
                (context, actorType) => new ActorService(
                    context,
                    actorType, () => new Actor1(),
                    settings: new ActorServiceSettings()
                    {
                        ActorConcurrencySettings = new ActorConcurrencySettings()
                        {
                            ReentrancyMode = ActorReentrancyMode.Disallowed
                        }
                    }))
                .GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}
```
```Java
static class Program
{
    static void Main()
    {
        try
        {
            ActorConcurrencySettings actorConcurrencySettings = new ActorConcurrencySettings();
            actorConcurrencySettings.setReentrancyMode(ActorReentrancyMode.Disallowed);

            ActorServiceSettings actorServiceSettings = new ActorServiceSettings();
            actorServiceSettings.setActorConcurrencySettings(actorConcurrencySettings);

            ActorRuntime.registerActorAsync(
                Actor1.getClass(),
                (context, actorType) -> new FabricActorService(
                    context,
                    actorType, () -> new Actor1(),
                    null,
                    stateProvider,
                    actorServiceSettings, timeout);

            Thread.sleep(Long.MAX_VALUE);
        }
        catch (Exception e)
        {
            throw e;
        }
    }
}
```


## <a name="next-steps"></a>Sonraki adımlar
* Yeniden girişi hakkında daha fazla bilgi [aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
