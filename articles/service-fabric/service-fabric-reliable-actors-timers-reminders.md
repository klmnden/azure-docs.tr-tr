---
title: "Güvenilir aktörler zamanlayıcılar ve anımsatıcıları | Microsoft Docs"
description: "Service Fabric Reliable Actors zamanlayıcılar ve anımsatıcıları için giriş."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 00c48716-569e-4a64-bd6c-25234c85ff4f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: f61743a7925c26767118dcb2d18799c26f880156
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="actor-timers-and-reminders"></a>Aktör zamanlayıcılar ve anımsatıcıları
Aktör zamanlayıcılar veya anımsatıcıları kaydederek kendilerini düzenli çalışma zamanlayabilirsiniz. Bu makalede zamanlayıcılar ve anımsatıcıları nasıl kullanılacağını gösterir ve bunları arasındaki farklar açıklanmaktadır.

## <a name="actor-timers"></a>Aktör zamanlayıcılar
Aktör zamanlayıcılar aktörler çalışma zamanı sağladığını geri arama yöntemleri Aç tabanlı eşzamanlılık saygı emin olmak için .NET veya Java bir zamanlayıcı çevresinde basit bir sarmalayıcı garanti sağlar.

Aktör kullanabileceğiniz `RegisterTimer`(C#) veya `registerTimer`(Java) ve `UnregisterTimer`(C#) veya `unregisterTimer`(Java) yöntemlere kaydetmek ve bunların zamanlayıcılar kaydı için temel sınıf. Aşağıdaki örnek, süreölçer API'lerini kullanımını gösterir. API'ler .NET Zamanlayıcı veya Java Zamanlayıcı çok benzer. Bu örnekte, Zamanlayıcı zamanı geldiğinde aktörler çalışma zamanı çağıracak `MoveObject`(C#) veya `moveObject`(Java) yöntemi. Yöntemi, dönüş tabanlı eşzamanlılık cevaben sağlanır. Bu yürütme bu geri çağırma işlemi tamamlanana kadar başka bir aktör yöntemin veya Zamanlayıcı/anımsatıcı geri aramalar ediyor olacağı anlamına gelir.

```csharp
class VisualObjectActor : Actor, IVisualObject
{
    private IActorTimer _updateTimer;

    public VisualObjectActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    protected override Task OnActivateAsync()
    {
        ...

        _updateTimer = RegisterTimer(
            MoveObject,                     // Callback method
            null,                           // Parameter to pass to the callback method
            TimeSpan.FromMilliseconds(15),  // Amount of time to delay before the callback is invoked
            TimeSpan.FromMilliseconds(15)); // Time interval between invocations of the callback method

        return base.OnActivateAsync();
    }

    protected override Task OnDeactivateAsync()
    {
        if (_updateTimer != null)
        {
            UnregisterTimer(_updateTimer);
        }

        return base.OnDeactivateAsync();
    }

    private Task MoveObject(object state)
    {
        ...
        return Task.FromResult(true);
    }
}
```
```Java
public class VisualObjectActorImpl extends FabricActor implements VisualObjectActor
{
    private ActorTimer updateTimer;

    public VisualObjectActorImpl(FabricActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    @Override
    protected CompletableFuture onActivateAsync()
    {
        ...

        return this.stateManager()
                .getOrAddStateAsync(
                        stateName,
                        VisualObject.createRandom(
                                this.getId().toString(),
                                new Random(this.getId().toString().hashCode())))
                .thenApply((r) -> {
                    this.registerTimer(
                            (o) -> this.moveObject(o),                        // Callback method
                            "moveObject",
                            null,                                             // Parameter to pass to the callback method
                            Duration.ofMillis(10),                            // Amount of time to delay before the callback is invoked
                            Duration.ofMillis(timerIntervalInMilliSeconds));  // Time interval between invocations of the callback method
                    return null;
                });
    }

    @Override
    protected CompletableFuture onDeactivateAsync()
    {
        if (updateTimer != null)
        {
            unregisterTimer(updateTimer);
        }

        return super.onDeactivateAsync();
    }

    private CompletableFuture moveObject(Object state)
    {
        ...
        return this.stateManager().getStateAsync(this.stateName).thenCompose(v -> {
            VisualObject v1 = (VisualObject)v;
            v1.move();
            return (CompletableFuture<?>)this.stateManager().setStateAsync(stateName, v1).
                    thenApply(r -> {
                      ...
                      return null;});
        });
    }
}
```

Geri çağırma yürütme tamamlandıktan sonra sonraki süreyi süreölçer başlatır. Bu geri arama yürütüyor ve geri arama tamamlandığında başlatılan Zamanlayıcı durdurulduğunda anlamına gelir.

Aktörler çalışma zamanı geri araması bitirdiğinde aktör ait durum Yöneticisi için yapılan değişiklikleri kaydeder. Durum kaydetme işleminde bir hata meydana gelirse, söz konusu aktör nesne devre dışı bırakılır ve yeni bir örneğini etkinleştirilir.

Aktör çöp toplama bir parçası olarak devre dışı bırakıldığında tüm zamanlayıcılar durdurulur. Bir zamanlayıcı geri aramalar bundan sonra çağrılır. Ayrıca, aktörler çalışma zamanı devre dışı bırakma önce çalışmakta olan zamanlayıcılar hakkında hiçbir bilgi sürdürmez. Gelecekte etkinleştirildiğinde, gereken tüm zamanlayıcılar kaydetmek için aktör kadar olur. Daha fazla bilgi için bölümüne bakarak [aktör çöp toplama](service-fabric-reliable-actors-lifecycle.md).

## <a name="actor-reminders"></a>Aktör anımsatıcıları
Anımsatıcıları olan bir aktör üzerinde kalıcı geri aramalar tetiklemek için bir mekanizma kez belirtilebilir. İşlevleri için zamanlayıcılar benzer. Ancak zamanlayıcılar anımsatıcıları tüm koşullar altında aktör açıkça bunları kaydını siler veya aktör açıkça silinene kadar tetiklenir. Özellikle, aktörler çalışma zamanı aktör'ın anımsatıcıları aktör durumu sağlayıcısı kullanma hakkında bilgi devam ederse çünkü anımsatıcıları aktör deactivations ve yük devretme işlemlerini arasında tetiklenir. Anımsatıcıları güvenilirliğini aktör durumu sağlayıcısı tarafından sağlanan durumu güvenilirlik garanti bağlıdır unutmayın. Bu, durumu kalıcılığını None olarak ayarlanmış aktörler için anımsatıcılar yük devretme sonrasında harekete geçmeyecektir olduğunu anlamına gelir. 

Bir anımsatıcı kaydetmek için bir aktör çağırır `RegisterReminderAsync` temel sınıf üzerinde aşağıdaki örnekte gösterildiği gibi sağlanan yöntemi:

```csharp
protected override async Task OnActivateAsync()
{
    string reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    IActorReminder reminderRegistration = await this.RegisterReminderAsync(
        reminderName,
        BitConverter.GetBytes(amountInDollars),
        TimeSpan.FromDays(3),
        TimeSpan.FromDays(1));
}
```

```Java
@Override
protected CompletableFuture onActivateAsync()
{
    String reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    ActorReminder reminderRegistration = this.registerReminderAsync(
            reminderName,
            state,
            dueTime,    //The amount of time to delay before firing the reminder
            period);    //The time interval between firing of reminders
}
```

Bu örnekte, `"Pay cell phone bill"` anımsatıcı adıdır. Bu aktör anımsatıcısı benzersiz şekilde tanımlamak için kullandığı bir dizedir. `BitConverter.GetBytes(amountInDollars)`(C#) olduğundan anımsatıcı'yla ilişkili bağlam. Bu geri aktör anımsatıcı geri çağırma bağımsız değişken olarak yani geçirilir `IRemindable.ReceiveReminderAsync`(C#) veya `Remindable.receiveReminderAsync`(Java).

Anımsatıcıları kullanmak aktörler uygulanmalı `IRemindable` , aşağıdaki örnekte gösterildiği gibi arabirim.

```csharp
public class ToDoListActor : Actor, IToDoListActor, IRemindable
{
    public ToDoListActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task ReceiveReminderAsync(string reminderName, byte[] context, TimeSpan dueTime, TimeSpan period)
    {
        if (reminderName.Equals("Pay cell phone bill"))
        {
            int amountToPay = BitConverter.ToInt32(context, 0);
            System.Console.WriteLine("Please pay your cell phone bill of ${0}!", amountToPay);
        }
        return Task.FromResult(true);
    }
}
```
```Java
public class ToDoListActorImpl extends FabricActor implements ToDoListActor, Remindable
{
    public ToDoListActor(FabricActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture receiveReminderAsync(String reminderName, byte[] context, Duration dueTime, Duration period)
    {
        if (reminderName.equals("Pay cell phone bill"))
        {
            int amountToPay = ByteBuffer.wrap(context).getInt();
            System.out.println("Please pay your cell phone bill of " + amountToPay);
        }
        return CompletableFuture.completedFuture(true);
    }

```

Bir anımsatıcı tetiklendiğinde Reliable Actors çalışma zamanı çağıracağı `ReceiveReminderAsync`(C#) veya `receiveReminderAsync`aktör yöntemi (Java). Bir oyuncu birden çok anımsatıcıları kaydedebilirsiniz ve `ReceiveReminderAsync`(C#) veya `receiveReminderAsync`(Java) yöntem çağrıldığında bu anımsatıcılar birini ne zaman tetiklenir. Aktör için geçirilen anımsatıcı adı kullanabilirsiniz `ReceiveReminderAsync`(C#) veya `receiveReminderAsync`hangi anımsatıcı tetiklendi şekilde yöntemi (Java).

Çalışma zamanı kaydeder aktör 's aktörler duruma `ReceiveReminderAsync`(C#) veya `receiveReminderAsync`(Java) çağrı tamamlanır. Durum kaydetme işleminde bir hata meydana gelirse, söz konusu aktör nesne devre dışı bırakılır ve yeni bir örneğini etkinleştirilir.

Bir aktör anımsatıcısı kaydı çağrılar `UnregisterReminderAsync`(C#) veya `unregisterReminderAsync`(Java) yöntemi, aşağıdaki örneklerde gösterildiği gibi.

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

Yukarıda gösterildiği gibi `UnregisterReminderAsync`(C#) veya `unregisterReminderAsync`(Java) yöntemi kabul eden bir `IActorReminder`(C#) veya `ActorReminder`(Java) arabirimi. Aktör temel sınıf destekleyen bir `GetReminder`(C#) veya `getReminder`almak için kullanılır (Java) yöntemi `IActorReminder`(C#) veya `ActorReminder`anımsatıcı adı geçirerek (Java) arabirimi. Aktör kalıcı gerek yoktur çünkü bu uygundur `IActorReminder`(C#) veya `ActorReminder`döndürüldü (Java) arabirimi `RegisterReminder`(C#) veya `registerReminder`(Java) yöntem çağrısı.

## <a name="next-steps"></a>Sonraki Adımlar
Güvenilir aktör olayları ve yeniden giriş hakkında bilgi edinin:
* [Aktör olayları](service-fabric-reliable-actors-events.md)
* [Aktör yeniden giriş](service-fabric-reliable-actors-reentrancy.md)
