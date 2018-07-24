---
title: Azure Service Fabric aktör içinde özelliklerini uygulama | Microsoft Docs
description: Hizmet düzeyi özelliklerini StatefulService devralınırken olduğu gibi uygulayan kendi actor hizmetinin yazma açıklar.
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
ms.openlocfilehash: 6aff9e9599d31942f994f3cb4e5e9219f33dc7e1
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205529"
---
# <a name="implementing-service-level-features-in-your-actor-service"></a>Aktör hizmetinizin hizmet düzeyi özelliklerini uygulama
Bölümünde anlatıldığı gibi [hizmet katmanlarını](service-fabric-reliable-actors-platform.md#service-layering), actor hizmetinin güvenilir bir hizmettir.  Öğesinden türetilen kendi hizmet yazabilirsiniz `ActorService` ve hizmet düzeyi özelliklerini StatefulService, gibi devralınırken yaptığınız şekilde uygulayın:

- Hizmet yedekleme ve geri yükleme.
- İşlevi, tüm aktör, örneğin, devre kesici paylaşılan.
- Uzak yordam çağrıları tek tek her aktör ve aktör hizmeti üzerinde.

## <a name="using-the-actor-service"></a>Actor hizmetinin kullanma
Aktör örnekleri ama çalışıyorlar actor hizmetinin erişebilir. Aktör hizmeti aracılığıyla aktör örneklerinizi programlı olarak hizmet bağlamı elde edebilirsiniz. Hizmet bağlamı, bölüm kimliği, hizmet adı, uygulama adı ve diğer Service Fabric platforma özgü bilgileri sahiptir:

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

Tüm Reliable Services gibi actor hizmetinin ile bir Service Fabric çalışma zamanı hizmet türüne kaydedilmesi gerekir. Aktör hizmetinin aktör örneklerinizi çalıştırması için aktör türünüzün de aktör hizmetine kaydedilmesi gerekir. `ActorRuntime` kayıt yöntemi bu işi aktörlerin yerine gerçekleştirir. En basit durumda, aktör türünüzün kaydedebilir ve varsayılan ayarlarla actor hizmetinin örtük olarak kullanılır:

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

Alternatif olarak, kayıt yöntemi tarafından sağlanan bir lambda actor hizmetinin kendiniz oluşturmak için kullanabilirsiniz. Ardından aktör hizmeti yapılandırmak ve açıkça burada aktörünüz Oluşturucusu aracılığıyla bağımlılıkları ekleyebilir aktör örneklerinizi oluşturmak:

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
Aktör hizmeti uygulayan `IActorService` (C#) veya `ActorService` sırayla (Java) uygulayan `IService` (C#) veya `Service` (Java). Bu hizmet yöntemleri üzerinde uzaktan yordam çağrılarını izin veren, Reliable Services uzaktan iletişim tarafından kullanılan arabirimidir. Uzaktan hizmet iletişimini uzaktan çağrılabilir ve izin hizmet düzeyi yöntemleri içeren [listeleme](service-fabric-reliable-actors-enumerate.md) ve [Sil](service-fabric-reliable-actors-delete-actors.md) aktörler.


## <a name="custom-actor-service"></a>Özel aktör hizmeti
Aktör kaydı lambda kullanarak, türetilen kendi özel actor hizmetinin kaydedebilirsiniz `ActorService` (C#) ve `FabricActorService` (Java). Bu özel aktör hizmeti, kendi hizmet düzeyi işlevselliği devralan bir hizmet sınıfı yazarak uygulayabilirsiniz `ActorService` (C#) veya `FabricActorService` (Java). Özel actor hizmetinin tüm aktör çalışma zamanı işlevinden devralan `ActorService` (C#) veya `FabricActorService` (Java) ve kendi hizmet yöntemleri uygulamak için kullanılabilir.

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
Bir özel aktör hizmeti uzaktan iletişim dinleyicisi zaten var. avantajlarından yararlanarak aktör verileri yedeklemek için bir yöntem getirebilir `ActorService`.  Bir örnek için bkz. [aktörleri yedekleme ve geri yükleme](service-fabric-reliable-actors-backup-and-restore.md).

## <a name="actor-using-remoting-v2interfacecompatible-stack"></a>Uzaktan iletişimini V2(InterfaceCompatible) yığını kullanılarak aktör
Uzaktan iletişim V2 (InterfaceCompatible V2_1 olarak da bilinir) yığını V2 Remoting özelliklerinin yığın yanında, uzaktan iletişim V1 yığınına Arabirimi uyumlu yığınıdır ancak V2 ve V1 ile geriye dönük uyumlu olmayan tüm yoktur. Aşağıda, yükseltme V1'den V2_1 için hizmet kullanılabilirliğini etkilemeden yapmak için izleyin [makale](#actor-service-upgrade-to-remoting-v2interfacecompatible-stack-without-impacting-service-availability).

Aşağıdaki değişiklikleri uzak V2_1 yığın kullanmak için gereklidir.
 1. Aktör arabirimlerinde aşağıdaki derleme özniteliğini ekleyin.
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
   ```

 2. V2 Stack kullanmaya başlamak için derleme ve yükseltme ActorService ve aktör istemci projeleri.

#### <a name="actor-service-upgrade-to-remoting-v2interfacecompatible-stack-without-impacting-service-availability"></a>Aktör hizmeti hizmet kullanılabilirliği etkilemeden Remoting V2(InterfaceCompatible) yığınına yükseltin.
Bu değişiklik, bir 2 adımlı yükseltme olacaktır. Adımları sırayla listelendiği gibi izleyin.

1.  Aktör arabirimlerinde aşağıdaki derleme özniteliğini ekleyin. Bu öznitelik iki dinleyici için ActorService, V1 başlar (mevcut) ve V2_1 dinleyicisi. Bu değişiklikle birlikte ActorService yükseltin.

  ```csharp
  [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
  ```

2. Yukarıdaki yükseltme işlemini tamamladıktan sonra ActorClients yükseltin.
Bu adım, aktör Proxy Remoting V2_1 yığın kullandığından emin sağlar.

3. Bu adım isteğe bağlıdır. V1 dinleyiciyi kaldırmak için yukarıdaki özniteliğini değiştirin.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
    ```

## <a name="actor-using-remoting-v2-stack"></a>Uzaktan iletişim V2 yığını kullanılarak aktör
2.8 nuget paketi ile kullanıcılar artık daha fazla performansa sahip olduğundan ve özel seri hale getirme gibi özellikler sağlayan uzaktan iletişim V2 yığın kullanabilir. Uzaktan iletişim V2 (biz artık, V1 uzaktan iletişim yığını olarak aradığınız) var olan uzaktan iletişim yığını geriye dönük uyumlu değil.

Aşağıdaki değişiklikler, uzaktan iletişim V2 yığını kullanması gerekmez.
 1. Aktör arabirimlerinde aşağıdaki derleme özniteliğini ekleyin.
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
   ```

 2. V2 Stack kullanmaya başlamak için derleme ve yükseltme ActorService ve aktör istemci projeleri.

#### <a name="actor-service-upgrade-to-remoting-v2-stack-without-impacting-service-availability"></a>Aktör hizmeti hizmet kullanılabilirliği etkilemeden uzaktan iletişim V2 yığınına yükseltin.
Bu değişiklik, bir 2 adımlı yükseltme olacaktır. Adımları sırayla listelendiği gibi izleyin.

1.  Aktör arabirimlerinde aşağıdaki derleme özniteliğini ekleyin. Bu öznitelik iki dinleyici için ActorService, V1 başlar (mevcut) ve V2 dinleyicisi. Bu değişiklikle birlikte ActorService yükseltin.

  ```csharp
  [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
  ```

2. Yukarıdaki yükseltme işlemini tamamladıktan sonra ActorClients yükseltin.
Bu adım, aktör Proxy uzaktan iletişim V2 yığın kullandığından emin sağlar.

3. Bu adım isteğe bağlıdır. V1 dinleyiciyi kaldırmak için yukarıdaki özniteliğini değiştirin.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [Aktör durumu yönetimi](service-fabric-reliable-actors-state-management.md)
* [Aktör yaşam döngüsü ve atık toplama](service-fabric-reliable-actors-lifecycle.md)
* [Aktörler API başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [.NET örnek kodu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java örnek kodu](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
