---
title: Reliable Actors süreölçerler ve anımsatıcılar | Microsoft Docs
description: Service Fabric Reliable Actors süreölçerler ve anımsatıcılar için giriş.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: amanbha
ms.assetid: 00c48716-569e-4a64-bd6c-25234c85ff4f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 323de842645cced3c6f490e98112fcbcd184aa64
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58667438"
---
# <a name="actor-timers-and-reminders"></a>Aktör süreölçerler ve anımsatıcılar
Aktörler zamanlayıcılar veya anımsatıcılar kaydederek kendilerini üzerinde düzenli iş zamanlayabilirsiniz. Bu makalede, Zamanlayıcı ve anımsatıcı kullanmayı gösterir ve bunları arasındaki farklar açıklanmaktadır.

## <a name="actor-timers"></a>Aktör zamanlayıcılar
Aktör zamanlayıcılar aktörler çalışma zamanı sağlar, geri çağırma yöntemlerine sırayla oynadıkları eşzamanlılık uyduğunuzdan emin olmak için bir .NET veya Java Zamanlayıcı çevresinde basit bir sarmalayıcı garanti sağlar.

Aktörler kullanabileceğiniz `RegisterTimer`(C#) veya `registerTimer`(Java) ve `UnregisterTimer`(C#) veya `unregisterTimer`kaydetmek ve bunların zamanlayıcılar kaydını silmek için temel sınıf yöntemleri (Java). Aşağıdaki örnekte, Zamanlayıcı API'leri kullanımını gösterir. API'ler .NET Zamanlayıcı veya Java Zamanlayıcı için oldukça benzerdir. Bu örnekte, Zamanlayıcıyı zamanı geldiğinde aktörler çalışma zamanı çağıracak `MoveObject`(C#) veya `moveObject`yöntemi (Java). Yöntem sırayla oynadıkları eşzamanlılık saygı garanti edilir. Bu, bu geri çağırma yürütme tamamlanana kadar diğer aktör yöntem ya da Zamanlayıcı/anımsatıcı geri çağırmaları sürmekte olduğu anlamına gelir.

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

Sonraki dönemin Zamanlayıcının yürütme geri çağırma tamamlandıktan sonra başlar. Bu geri çağırma yürüten ve geri çağırma tamamlandığında başlatılan Zamanlayıcı durdurulduğunu gösterir.

Aktörler çalışma zamanı, geri çağırma tamamlandığında aktörün durum Yöneticisi için yapılan değişiklikleri kaydeder. Durumu kaydedilirken bir hata oluşursa, aktör nesne devre dışı bırakılır ve yeni bir örneğini etkinleştirilir.

Aktör çöp toplama işleminin bir parçası olarak devre dışı bırakıldığında tüm zamanlayıcılar durdurulur. Hiçbir Zamanlayıcı geri çağırmaları bundan sonra çağrılır. Ayrıca aktör çalışma zamanı devre dışı bırakma önce çalışmakta olan zamanlayıcılar hakkında hiçbir bilgi sürdürmez. Bu, gelecekte etkinleştirildiğinde, gereken tüm zamanlayıcılar kaydetmek için en fazla aktör olur. Daha fazla bilgi için üzerinde bölümüne bakın. [aktör çöp toplama](service-fabric-reliable-actors-lifecycle.md).

## <a name="actor-reminders"></a>Aktör anımsatıcılar
Anımsatıcıları aktörün üzerinde kalıcı geri çağırmaları tetiklemek için bir mekanizma olan belirtilen zamanlarda. Zamanlayıcılar için işlevlerine benzer. Ancak, zamanlayıcılar anımsatıcılar tüm koşullar altında aktör açıkça bunları kaydını iptal eder ya da aktör açıkça silinene kadar tetiklenir. Özellikle, aktör çalışma zamanı devam ederse aktörün anımsatıcılar aktör durumu sağlayıcısı kullanma hakkında bilgi için anımsatıcılar aktör deactivations ve yük devretme işlemleri arasında tetiklenir. Anımsatıcıları güvenilirliğini aktör durumu sağlayıcısı tarafından sağlanan durumu güvenilirlik garantisi bağlıdır unutmayın. Bu, durumu kalıcılığını hiçbiri olarak ayarlandı aktörler için bir yük devretme sonrasında anımsatıcılar tetiklenmez anlamına gelir. 

Anımsatıcı kaydetmek için aktörü çağıran `RegisterReminderAsync` taban sınıfa, aşağıdaki örnekte gösterildiği gibi sağlanan yöntemi:

```csharp
protected override async Task OnActivateAsync()
{
    string reminderName = "Pay cell phone bill";
    int amountInDollars = 100;

    IActorReminder reminderRegistration = await this.RegisterReminderAsync(
        reminderName,
        BitConverter.GetBytes(amountInDollars),
        TimeSpan.FromDays(3),    //The amount of time to delay before firing the reminder
        TimeSpan.FromDays(1));    //The time interval between firing of reminders
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

Bu örnekte, `"Pay cell phone bill"` anımsatıcı adıdır. Bu anımsatıcı benzersiz olarak tanımlanabilmesi için aktör kullanan bir dizedir. `BitConverter.GetBytes(amountInDollars)`(C#) anımsatıcısı ile ilişkili bağlam. Bu geri Aktöre anımsatıcı geri çağırma bağımsız değişkeni olarak başka bir deyişle geçirilir `IRemindable.ReceiveReminderAsync`(C#) veya `Remindable.receiveReminderAsync`(Java).

Anımsatıcıları kullanan aktörler uygulanmalı `IRemindable` aşağıdaki örnekte gösterildiği gibi arabirim.

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

Reliable Actors çalışma zamanı bir anımsatıcı tetiklendiğinde çağıracağı `ReceiveReminderAsync`(C#) veya `receiveReminderAsync`aktör yöntemi (Java). Bir aktör, birden çok anımsatıcılar kaydedebilirsiniz ve `ReceiveReminderAsync`(C#) veya `receiveReminderAsync`(Java) metodunu çağırmak bu anımsatıcılar birini ne zaman tetiklenir. Aktör için geçirilen anımsatıcı adı kullanabilirsiniz `ReceiveReminderAsync`(C#) veya `receiveReminderAsync`hangi anımsatıcı tetiklendi anlamaya yöntemi (Java).

Çalışma zamanı kaydeder aktörün aktörleri durumuna `ReceiveReminderAsync`(C#) veya `receiveReminderAsync`(Java) çağrı tamamlanır. Durumu kaydedilirken bir hata oluşursa, aktör nesne devre dışı bırakılır ve yeni bir örneğini etkinleştirilir.

Anımsatıcı kaydını kaldırmak için aktörü çağıran `UnregisterReminderAsync`(C#) veya `unregisterReminderAsync`(Java) yöntemi, aşağıdaki örneklerde gösterildiği gibi.

```csharp
IActorReminder reminder = GetReminder("Pay cell phone bill");
Task reminderUnregistration = UnregisterReminderAsync(reminder);
```
```Java
ActorReminder reminder = getReminder("Pay cell phone bill");
CompletableFuture reminderUnregistration = unregisterReminderAsync(reminder);
```

Yukarıda gösterildiği gibi `UnregisterReminderAsync`(C#) veya `unregisterReminderAsync`yöntemi (Java) kabul bir `IActorReminder`(C#) veya `ActorReminder`(Java) arabirimi. Aktör temel sınıf destekleyen bir `GetReminder`(C#) veya `getReminder`almak için kullanılan yöntemi (Java) `IActorReminder`(C#) veya `ActorReminder`(Java) arabirimi anımsatıcı adını geçirerek. Aktör kalıcı hale getirmek gerekmez çünkü bu kullanışlıdır `IActorReminder`(C#) veya `ActorReminder`öğesinden döndürülen (Java) arabirimi `RegisterReminder`(C#) veya `registerReminder`(Java) yöntem çağrısı.

## <a name="next-steps"></a>Sonraki Adımlar
Reliable Actor olayları ve yeniden giriş hakkında bilgi edinin:
* [Aktör olayları](service-fabric-reliable-actors-events.md)
* [Aktör'na yeniden giriş](service-fabric-reliable-actors-reentrancy.md)
