---
title: Azure Service Fabric aktör'na yeniden giriş | Microsoft Docs
description: Service Fabric Reliable Actors yeniden giriş giriş.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: amanbha
ms.assetid: be23464a-0eea-4eca-ae5a-2e1b650d365e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 46682787bac2d60d188384a4078ca2fa1f46ae7a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60725423"
---
# <a name="reliable-actors-reentrancy"></a>Reliable Actors yeniden giriş
Reliable Actors çalışma zamanı varsayılan olarak, mantıksal çağrı Bağlam tabanlı yeniden giriş sağlar. Bu aktörler ve aynı çağrı bağlamı zincirine olmaları durumunda yeniden girilen olmasını sağlar. Örneğin, bir aktör aktör C'ye ileti gönderen aktör B için bir ileti gönderir Aktör C aktör A çağırırsa izin verecek şekilde ileti işleme işleminin bir parçası olarak yeniden girilen, iletisidir. İşlem tamamlanana kadar farklı bir çağrı bağlamı parçası olan herhangi bir iletiyi aktör A engellenir.

Aktör yönetmezseniz tanımlanan kullanılabilir iki seçenek vardır `ActorReentrancyMode` sabit listesi:

* `LogicalCallContext` (varsayılan davranış)
* `Disallowed` -yeniden giriş devre dışı bırakır

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
Yeniden giriş yapılandırılabilir bir `ActorService`ait kayıt sırasında ayarları. Ayar actor hizmetinin içinde oluşturulmuş tüm aktör örnekleri için geçerlidir.

Yeniden giriş modu ayarlar bir aktör hizmeti aşağıdaki örnekte `ActorReentrancyMode.Disallowed`. Bu durumda, bir aktör başka bir aktör, bir özel durum türü, yeniden girilen iletisi gönderirse `FabricException` oluşturulur.

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
* ' Na yeniden giriş hakkında daha fazla bilgi [aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
