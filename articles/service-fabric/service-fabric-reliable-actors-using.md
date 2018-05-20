---
title: Azure Service Fabric aktör özelliklerini uygulama | Microsoft Docs
description: Hizmet düzeyi özellikleri uygulayan StatefulService devralırken olduğu gibi kendi aktör hizmeti yazma açıklar.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 41548c3395fa0c8f56e62cfcfb7338a2d53f040f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="implementing-service-level-features-in-your-actor-service"></a>Aktör hizmetinizi hizmet düzeyi özelliklerini uygulama
Bölümünde açıklandığı gibi [hizmet katmanlarını](service-fabric-reliable-actors-platform.md#service-layering), aktör hizmeti güvenilir bir hizmettir.  Türetilen kendi hizmet yazabilirsiniz `ActorService` ve hizmet düzeyi özellikler StatefulService, gibi devralırken yaptığınız şekilde uygular:

- Hizmet yedekleme ve geri yükleme.
- İşlevselliği tüm aktörler, örneğin, bir devre kesici paylaşılan.
- Aktör hizmeti ve tek tek her aktör uzak yordam çağrıları.

## <a name="using-the-actor-service"></a>Aktör hizmeti kullanma
Aktör örnekleri bunlar çalıştırdığınız aktör hizmetine erişimi vardır. Aktör hizmeti aracılığıyla aktör örnekleri program aracılığıyla hizmet bağlamı elde edebilirsiniz. Hizmet bağlamı, bölüm kimliği, hizmet adı, uygulama adı ve diğer Service Fabric platforma özgü bilgileri vardır:

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

Service Fabric çalışma zamanında bir hizmet türü ile tüm güvenilir hizmetler gibi aktör hizmeti kayıtlı olması gerekir. Aktör hizmeti aktör örneklerinizi çalıştırmak aktör türünüz aktör hizmeti ile kaydedilmelidir. `ActorRuntime` kayıt yöntemi bu işi aktörlerin yerine gerçekleştirir. En basit durumda, yalnızca aktör türünüz kaydedebilirsiniz ve varsayılan ayarlarla aktör hizmeti örtük olarak kullanılacak:

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

Alternatif olarak, kayıt yöntemi tarafından sağlanan bir lambda aktör hizmeti kendiniz oluşturmak için kullanabilirsiniz. Aktör hizmeti yapılandırmak ve burada kurucusu aracılığıyla, aktör bağımlılıkları ekleyemezsiniz aktör örneklerinizi açıkça oluşturun:

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
Aktör hizmeti uygulayan `IActorService` (C#) veya `ActorService` (Java) sırayla uygulayan `IService` (C#) veya `Service` (Java). Bu uzaktan yordam çağrısı hizmeti yöntemleri sağlayan Reliable Services remoting tarafından kullanılan arabirimidir. Hizmet remoting uzaktan çağrılabilir ve izin hizmet düzeyi yöntemleri içeren [listeleme](service-fabric-reliable-actors-enumerate.md) ve [silmek](service-fabric-reliable-actors-delete-actors.md) aktörler.


## <a name="custom-actor-service"></a>Özel aktör hizmeti
Aktör kayıt lambda kullanarak türetilen kendi özel aktör hizmeti kaydedebilirsiniz `ActorService` (C#) ve `FabricActorService` (Java). Bu özel aktör hizmeti, kendi hizmet düzeyi işlevselliği devralan bir hizmet sınıfı yazarak uygulayabileceğiniz `ActorService` (C#) veya `FabricActorService` (Java). Bir özel aktör hizmeti tüm aktör çalışma zamanı işlevsellik devralır `ActorService` (C#) veya `FabricActorService` (Java) ve kendi hizmet yöntemleri uygulamak için kullanılabilir.

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

## <a name="implementing-actor-backup-and-restore"></a>Uygulama aktör yedekleme ve geri yükleme
Bir özel aktör hizmeti zaten mevcut remoting dinleyicisi yararlanarak aktör verileri yedeklemek için bir yöntem getirebilir `ActorService`.  Bir örnek için bkz: [yedekleme ve geri yükleme aktörler](service-fabric-reliable-actors-backup-and-restore.md).

## <a name="actor-using-remoting-v2-stack"></a>Aktör Remoting V2 yığını kullanma
2.8 nuget paketi ile kullanıcılar artık daha fazla kullanıcı olduğunu ve özel seri hale getirme gibi özellikleri sunan Remoting V2 yığını kullanabilirsiniz. Remoting V2 (biz şimdi onu V1 Remoting yığın aradığınız) mevcut uzaktan iletişim yığını ile geriye dönük uyumlu değil.

Aşağıdaki değişiklikleri Remoting V2 yığını kullanması gerekir.
 1. Şu derleme özniteliği aktör arabirimlerde ekleyin.
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListener = RemotingListener.V2Listener,RemotingClient = RemotingClient.V2Client)]
   ```

 2. V2 yığını kullanmaya başlamak için yapı ve yükseltme ActorService ve aktör istemci projeleri.

### <a name="actor-service-upgrade-to-remoting-v2-stack-without-impacting-service-availability"></a>Aktör hizmeti hizmet kullanılabilirliği etkileyen olmadan Remoting V2 yığınına yükseltin.
Bu değişikliği bir 2 adımlı yükseltme olacaktır. Listelendiği gibi aynı sırasındaki adımları izleyin.

1.  Şu derleme özniteliği aktör arabirimlerde ekleyin. Bu öznitelik iki dinleyicileri ActorService, V1 için başlar (varolan) ve V2 dinleyicisi. Bu değişiklikle ActorService yükseltin.

  ```csharp
  [assembly:FabricTransportActorRemotingProvider(RemotingListener = RemotingListener.CompatListener,RemotingClient = RemotingClient.V2Client)]
  ```

2. Yukarıdaki yükseltme işlemini tamamladıktan sonra ActorClients yükseltin.
Bu adım aktör Proxy Remoting V2 yığını kullandığından emin sağlar.

3. Bu adım isteğe bağlıdır. V1 dinleyiciyi kaldırmak için yukarıdaki özniteliğini değiştirin.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListener = RemotingListener.V2Listener,RemotingClient = RemotingClient.V2Client)]
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [Aktör durum yönetimi](service-fabric-reliable-actors-state-management.md)
* [Aktör yaşam döngüsü ve çöp toplama](service-fabric-reliable-actors-lifecycle.md)
* [Aktör API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [.NET örnek kod](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java örnek kod](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
