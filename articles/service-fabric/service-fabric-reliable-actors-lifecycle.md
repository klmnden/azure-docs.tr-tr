---
title: Azure Service Fabric aktör yaşam döngüsüne genel bakış | Microsoft Docs
description: Service Fabric güvenilir aktör yaşam döngüsü, çöp toplama ve el ile aktörleri ve durumlarını silme açıklar
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
ms.date: 10/06/2017
ms.author: amanbha
ms.openlocfilehash: f81fde441a2f0dc2504601f82e5b890eb6e216de
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58660995"
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a>Aktör yaşam döngüsü, otomatik çöp toplama ve el ile silin
Aktörün yöntemlerinden herhangi biri için bir çağrı yapılır ilk kez etkinleştirilir. Aktörün devre dışı bırakılmış (atık aktörler çalışma zamanı tarafından toplanan) ise yapılandırılabilir bir süre için kullanılmaz. Aktörün ve durumunu da el ile herhangi bir zamanda silinebilir.

## <a name="actor-activation"></a>Aktör etkinleştirme
Aktörün etkinleştirildiğinde, aşağıdakiler gerçekleşir:

* Yeni bir aktör, bir aktör için bir çağrı gelir ve biri etkin değilse, oluşturulur.
* Bu durum koruma aktörün durumu yüklenir.
* `OnActivateAsync` (C#) veya `onActivateAsync` (aktör uygulamasında geçersiz kılınabilir) (Java) yöntemi çağrılır.
* Aktör şimdi etkin olarak kabul edilir.

## <a name="actor-deactivation"></a>Aktör devre dışı bırakma
Aktörün devre dışı bırakıldığında, aşağıdakiler gerçekleşir:

* Bir aktör, belirli bir süre için kullanılmaz etkin aktörler tablosundan kaldırılır.
* `OnDeactivateAsync` (C#) veya `onDeactivateAsync` (aktör uygulamasında geçersiz kılınabilir) (Java) yöntemi çağrılır. Bu, bir aktör tüm zamanlayıcılar temizler. Aktör işlemleri durum değişiklikleri Bu yöntemden çağrılmamalıdır ister.

> [!TIP]
> Fabric aktörleri çalışma zamanı bazı yayan [ilgili olayları aktör etkinleştirme ve devre dışı bırakma](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters). Bunlar, tanılama ve performans izleme kullanışlıdır.
>
>

### <a name="actor-garbage-collection"></a>Aktör çöp toplama
Aktörün devre dışı bırakıldığında, aktör nesne başvurularını serbest bırakılır ve bu normalde ortak dil çalışma zamanı (CLR) veya java sanal makinesi (JVM) çöp toplayıcısı tarafından toplanan çöp olabilir. Çöp toplama yalnızca aktör nesnesi temizler; Mevcut **değil** aktörün durum Yöneticisi'nde depolanan durumu kaldırın. Aktör etkinleştirildiğinde, sonraki açışınızda yeni bir aktör nesnesi oluşturulur ve durumuna geri yüklenir.

Ne "devre dışı bırakma ve çöp toplama amacıyla kullanılan olarak" sayılır?

* Bir çağrı alma
* `IRemindable.ReceiveReminderAsync` (yalnızca aktör anımsatıcılar kullanıyorsa geçerlidir) çağrılan yöntemi

> [!NOTE]
> Aktör zamanlayıcılar kullanıyorsa ve Zamanlayıcı geri çağırma işlemi çağrıldı, mevcut **değil** "kullanılan" olarak sayısı.
>
>

Biz devre dışı bırakma ayrıntılarına geçmeden önce aşağıdaki koşulları tanımlamak önemlidir:

* *Tarama aralığı*. Bu, aktör çalışma zamanı devre dışı bırakılabilir aktörler için etkin aktörler tablosunu tarar ve atık olarak toplanmış aralığıdır. Bunun için varsayılan değer 1 dakikadır.
* *Boşta kalma zaman aşımı*. Aktörün kullanılmaması gereken süre miktarıdır (boşta) devre dışı bırakılabilir ve atık olarak toplanmış önce. Bunun için varsayılan değer 60 dakikadır.

Genellikle, bu varsayılan ayarları değiştirmek gerekmez. Ancak, gerekirse, bu aralıklar arasında değiştirilebilir `ActorServiceSettings` kaydederken, [Actor hizmetinin](service-fabric-reliable-actors-platform.md):

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        ActorRuntime.RegisterActorAsync<MyActor>((context, actorType) =>
                new ActorService(context, actorType,
                    settings:
                        new ActorServiceSettings()
                        {
                            ActorGarbageCollectionSettings =
                                new ActorGarbageCollectionSettings(10, 2)
                        }))
            .GetAwaiter()
            .GetResult();
    }
}
```

```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
    }
}
```
Etkin her aktör için de actor çalışma zamanına, (yani kullanılır) uzun zamandır boşta olan süre miktarını izler. Actor çalışma zamanına her aktör denetler her `ScanIntervalInSeconds` , atık toplanan ve boşta kaldığında topladığı olup olmadığını görmek için `IdleTimeoutInSeconds`.

Aktörün kullanılan herhangi bir zamanda, boşta kalma süresini 0 olarak ayarlanır. Bundan sonra yalnızca onu yeniden boşta kalırsa atık aktör olabilir `IdleTimeoutInSeconds`. Geri çağırma aktörün bir aktör arabirim yöntemini ya da bir oyuncu anımsatıcı geri çağırma yürütülürse kullanılan değerlendirilir. Bir aktör olur **değil** Zamanlayıcısı geri çağırma işlemi yürütülürse kullanılan düşünülür.

Aşağıdaki diyagram bu kavramları göstermek için tek bir aktör yaşam döngüsü gösterir.

![Boşta kalma süresi örneği][1]

Örnek üzerinde bu aktör ömrünü aktör yöntem çağrılarını ve anımsatıcılar zamanlayıcılar etkisini gösterir. Bu örnek hakkında aşağıdaki noktalara söz şunlardır:

* ScanInterval ve IdleTimeout 5 ve 10 sırasıyla ayarlanır. (Amacımız yalnızca konsepti açıklamak amacıyla olduğundan birim burada önemli değildir.)
* Aktörlerin toplanacak taraması tarama aralığı 5 tarafından tanımlandığı şekilde T = 0, 5, 10, 15, 20, 25 ' olmuyor.
* T = 4 8, 12, 16, 20, 24, düzenli bir süreölçer başlatılır ve kendi geri çağırmayı yürütür. Aktör boşta kalma süresi etkilemez.
* Bir aktör yöntem çağrısı T = 7, boşta kalma süresini 0 olarak sıfırlar ve aktör çöp koleksiyonu geciktirir.
* Bir aktör anımsatıcı geri çağırma T = 14 yürütür ve daha fazla aktör çöp koleksiyonu geciktirir.
* T = 25, çöp toplama tarama sırasında aktörün boşta kalma süresi son 10 boşta kalma zaman aşımını aşıyor ve çöp olarak toplanacak aktör olur.

Bir aktör, hiçbir zaman bu yöntem yürütülürken harcanan ne kadar süre ne olursa olsun, yöntemlerinden biri olan Yürütülüyor iken atık olacaktır. Daha önce bahsedildiği gibi yürütülmesi aktör arabirim yöntemlerine ve anımsatıcı geri çağırmaları aktörün boşta kalma süresi 0 olarak sıfırlama çöp toplama engeller. Zamanlayıcı geri çağırmaları yürütülmesini boşta kalma süresini 0 olarak sıfırlanmaz. Ancak, Zamanlayıcı geri çağırma yürütme tamamlanana kadar aktör çöp koleksiyonu ertelenir.

## <a name="manually-deleting-actors-and-their-state"></a>El ile aktörleri ve durumlarını silme
Çöp toplama devre dışı bırakılan aktör yalnızca aktör nesnesi temizler, ancak bir aktörün durum Yöneticisi'nde depolanan verileri kaldırmaz. Aktörün yeniden etkinleştirildiğinde, verileri yeniden durum Yöneticisi için kullanılabilir hale getirilir. Burada actors durum Yöneticisi'nde veri depolamak ve devre dışı bırakıldı ancak etkinleştirmemiştir yeniden durumlarda, kullanıcıların verileri temizlemek gerekebilir.  Aktörler silme örnekleri için okuma [aktörleri ve durumlarını silme](service-fabric-reliable-actors-delete-actors.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Aktör süreölçerler ve anımsatıcılar](service-fabric-reliable-actors-timers-reminders.md)
* [Aktör olayları](service-fabric-reliable-actors-events.md)
* [Aktör'na yeniden giriş](service-fabric-reliable-actors-reentrancy.md)
* [Aktör tanılama ve performans izleme](service-fabric-reliable-actors-diagnostics.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# örnek kod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java örnek kodu](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
