---
title: Uygulama özellikleri Azure Service Fabric aktör | Microsoft Docs
description: StatefulService devraldığı durumlarda, aynı şekilde hizmet düzeyi özellikleri uygulayan kendi actor hizmetinin yazılacağını açıklar.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 57894770ad9d27430d5803c9a93ce6973355878a
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662984"
---
# <a name="implement-service-level-features-in-your-actor-service"></a>Aktör hizmetinizin hizmet düzeyi özelliklerini uygulama

Bölümünde anlatıldığı gibi [hizmet katmanlarını](service-fabric-reliable-actors-platform.md#service-layering), actor hizmetinin güvenilir bir hizmettir. Öğesinden türetilen kendi hizmet yazabilirsiniz `ActorService`. Durum bilgisi olan bir hizmet gibi devraldığı durumlarda aynı şekilde hizmet düzeyi özellikleri de uygulayabilirsiniz:

- Hizmet yedekleme ve geri yükleme.
- İşlevi, tüm aktör, örneğin, devre kesici paylaşılan.
- Uzak yordam çağrıları tek tek her aktör ve aktör hizmeti üzerinde.

## <a name="use-the-actor-service"></a>Actor hizmetinin kullanın

Aktör örnekleri ama çalışıyorlar actor hizmetinin erişebilir. Aktör hizmeti aracılığıyla aktör örneklerinizi programlı olarak hizmet bağlamı elde edebilirsiniz. Hizmet bağlamı, bölüm kimliği, hizmet adı, uygulama adı ve diğer Azure Service Fabric platforma özgü bilgileri sahiptir.

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```
```Java
CompletableFuture<?> MyActorMethod()
{
    UUID partitionId = this.getActorService().getServiceContext().getPartitionId();
    String serviceTypeName = this.getActorService().getServiceContext().getServiceTypeName();
    URI serviceInstanceName = this.getActorService().getServiceContext().getServiceName();
    String applicationInstanceName = this.getActorService().getServiceContext().getCodePackageActivationContext().getApplicationName();
}
```

Tüm Reliable Services gibi actor hizmetinin ile bir Service Fabric çalışma zamanı hizmet türüne kaydedilmesi gerekir. Aktör hizmetinin aktör örneklerinizi çalıştırması için aktör türünüzün de aktör hizmetine kaydedilmesi gerekir. `ActorRuntime` kayıt yöntemi bu işi aktörlerin yerine gerçekleştirir. En basit durumda, aktör türünüzün kaydedebilirsiniz ve actor hizmetinin sonra varsayılan ayarları kullanır.

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>().GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```

Alternatif olarak, kayıt yöntemi tarafından sağlanan bir lambda actor hizmetinin kendiniz oluşturmak için kullanabilirsiniz. Ardından actor hizmetinin yapılandırabilir ve açıkça aktör örneklerinizi oluşturun. Oluşturucusu aracılığıyla aktörünüz için bağımlılıklar ekleyebilir.

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new ActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
static class Program
{
    private static void Main()
    {
      ActorRuntime.registerActorAsync(
              MyActor.class,
              (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
              timeout);

        Thread.sleep(Long.MAX_VALUE);
    }
}
```

## <a name="actor-service-methods"></a>Aktör hizmeti yöntemleri

Aktör hizmeti uygulayan `IActorService` (C#) veya `ActorService` sırayla (Java) uygulayan `IService` (C#) veya `Service` (Java). Bu arabirim, uzaktan yordam çağrısı hizmeti yöntemleri sağlayan, Reliable Services uzaktan iletişim tarafından kullanılır. Bu hizmet uzaktan iletişimini uzaktan çağrılabilir hizmet düzeyi yöntemleri içerir. Bunu kullanabilirsiniz [listeleme](service-fabric-reliable-actors-enumerate.md) ve [Sil](service-fabric-reliable-actors-delete-actors.md) aktörler.


## <a name="custom-actor-service"></a>Özel aktör hizmeti

Aktör kaydı lambda kullanarak, türetilen kendi özel actor hizmetinin kaydedebilirsiniz `ActorService` (C#) ve `FabricActorService` (Java). Devralınan bir hizmet sınıfı yazarak, kendi hizmet düzeyi işlevselliği ardından uygulayabileceğiniz `ActorService` (C#) veya `FabricActorService` (Java). Özel actor hizmetinin tüm aktör çalışma zamanı işlevinden devralan `ActorService` (C#) veya `FabricActorService` (Java). Kendi hizmet yöntemleri uygulamak için kullanılabilir.

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```
```Java
class MyActorService extends FabricActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, BiFunction<FabricActorService, ActorId, ActorBase> newActor)
    {
         super(context, typeInfo, newActor);
    }
}
```

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new MyActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
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
        Thread.sleep(Long.MAX_VALUE);
    }
}
```

## <a name="implement-actor-backup-and-restore"></a>Uygulama aktör yedekleme ve geri yükleme

Bir özel aktör hizmeti uzaktan iletişim dinleyicisi zaten var. avantajlarından yararlanarak aktör verileri yedeklemek için bir yöntem getirebilir `ActorService`. Bir örnek için bkz. [aktörleri yedekleme ve geri yükleme](service-fabric-reliable-actors-backup-and-restore.md).

## <a name="actor-that-uses-a-remoting-v2-interface-compatible-stack"></a>Uzaktan iletişim V2 (Arabirimi uyumlu) yığını kullanan aktör

Uzaktan iletişim V2 (V2_1 bilinen uyumlu arabirimi) yığını V2 remoting yığınının tüm özelliklere sahiptir. Arayüz remoting V1 yığın ile uyumludur, ancak V2 ve V1 ile geriye dönük olarak uyumlu değil. V1'den V2_1 için hizmet kullanılabilirliği hakkında herhangi bir etkisi ile yükseltmek için sonraki bölümde yer alan adımları izleyin.

Aşağıdaki değişiklikleri uzak V2_1 yığın kullanmak için gereklidir:

1. Aktör arabirimlerinde aşağıdaki derleme özniteliğini ekleyin.
  
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
   ```

2. Derleme ve aktör hizmeti ve V2 stack kullanmaya başlamak için aktör istemci projeleri yükseltebilirsiniz.

### <a name="actor-service-upgrade-to-remoting-v2-interface-compatible-stack-without-affecting-service-availability"></a>Hizmet kullanılabilirliği etkilemeden uzaktan iletişim V2 (Arabirimi uyumlu) yığına aktör hizmeti yükseltmesi

İki aşamalı yükseltme değişikliktir. Bu sıradaki adımları izleyin.

1. Aktör arabirimlerinde aşağıdaki derleme özniteliğini ekleyin. Bu öznitelik iki dinleyiciden actor hizmetinin, V1 başlar (mevcut) ve V2_1 dinleyicisi. Bu değişiklik actor hizmetiyle yükseltin.

   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
   ```

2. Önceki yükseltme tamamlandıktan sonra aktör istemcileri yükseltin.
   Bu adım, aktör proxy remoting V2_1 yığın kullandığından emin sağlar.

3. Bu adım isteğe bağlıdır. V1 dinleyiciyi kaldırmak için önceki özniteliğini değiştirin.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
    ```

## <a name="actor-that-uses-the-remoting-v2-stack"></a>Uzaktan iletişim V2 yığınını kullanır aktör

Sürüm 2.8 NuGet paketi ile kullanıcılar artık daha iyi gerçekleştirir ve özel seri hale getirme gibi özellikler sağlayan uzaktan iletişim V2 yığın kullanabilir. Uzaktan iletişim V2 (artık V1 uzaktan iletişim yığını olarak da bilinir) mevcut uzaktan iletişim yığını geriye dönük uyumlu değil.

Aşağıdaki değişiklikler, uzaktan iletişim V2 yığını kullanması gerekmez.

1. Aktör arabirimlerinde aşağıdaki derleme özniteliğini ekleyin.

   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
   ```

2. Derleme ve aktör hizmeti ve V2 stack kullanmaya başlamak için aktör istemci projeleri yükseltebilirsiniz.

### <a name="upgrade-the-actor-service-to-the-remoting-v2-stack-without-affecting-service-availability"></a>Hizmet kullanılabilirliği etkilemeden actor hizmetinin uzaktan iletişim V2 yığınına yükseltin

İki aşamalı yükseltme değişikliktir. Bu sıradaki adımları izleyin.

1. Aktör arabirimlerinde aşağıdaki derleme özniteliğini ekleyin. Bu öznitelik iki dinleyiciden actor hizmetinin, V1 başlar (mevcut) ve V2 dinleyicisi. Bu değişiklik actor hizmetiyle yükseltin.

   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
   ```

2. Önceki yükseltme tamamlandıktan sonra aktör istemcileri yükseltin.
   Bu adım, aktör proxy uzaktan iletişim V2 yığın kullandığından emin sağlar.

3. Bu adım isteğe bağlıdır. V1 dinleyiciyi kaldırmak için önceki özniteliğini değiştirin.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Aktör durumu yönetimi](service-fabric-reliable-actors-state-management.md)
* [Aktör yaşam döngüsü ve atık toplama](service-fabric-reliable-actors-lifecycle.md)
* [Aktörler API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [.NET örnek kodu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java örnek kodu](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
