---
title: "Service Fabric üzerinde Reliable Actors | Microsoft Docs"
description: "Reliable Actors Reliable Services üzerinde katmanlanmış ve Service Fabric platformundan özellikleriyle nasıl açıklanmaktadır."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: vturecek
ms.openlocfilehash: ee248cb656eeb54e259ff1adf45080a207b5a866
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a>Reliable Actors hizmeti tarafından Service Fabric platformunun kullanımı
Bu makalede, Azure Service Fabric platformda Reliable Actors nasıl çalıştığı açıklanmaktadır. Durum bilgisi olan güvenilir bir hizmet uygulaması içinde barındırılan bir çerçeve çalıştırmanız reliable Actors adlı *aktör hizmeti*. Aktör hizmeti yaşam döngüsü ve ileti gönderme, aktörleri yönetmek gerekli tüm bileşenleri içerir:

* Aktör çalışma zamanı yaşam döngüsü, atık toplama yönetir ve tek iş parçacıklı erişimini zorunlu kılar.
* Aktör hizmeti remoting dinleyicisi aktörler için uzaktan erişim çağrılarını kabul eder ve bunları uygun aktör örneğine yönlendirmek için bir dağıtıcı gönderir.
* Aktör durumu sağlayıcısı (örneğin, güvenilir koleksiyonları durumu sağlayıcısı) durum sağlayıcılarının sarmalar ve aktör durum yönetimi için bir bağdaştırıcı sağlar.

Bu bileşenlerin birlikte güvenilir aktör framework oluştururlar.

## <a name="service-layering"></a>Hizmet katmanlarını
Aktör hizmeti güvenilir bir hizmet olduğundan tüm [uygulama modeli](service-fabric-application-model.md), yaşam döngüsü, [paketleme](service-fabric-package-apps.md), [dağıtım](service-fabric-deploy-remove-applications.md), yükseltme ve ölçeklendirme Reliable Services kavramlarını aktör Hizmetleri aynı şekilde geçerlidir.

![Aktör hizmeti katmanlama][1]

Önceki diyagramda Service Fabric uygulama çerçeveleri ve kullanıcı kodu arasındaki ilişkiyi gösterir. Mavi öğeleri Reliable Services uygulama çerçevesi temsil eder, turuncu güvenilir aktör framework ve yeşil kullanıcı kodu temsil eder.

Güvenilir Hizmetleri'nde hizmetinizi devralır `StatefulService` sınıfı. Bu sınıf kendisini türetilmiş, `StatefulServiceBase` (veya `StatelessService` durum bilgisi olmayan hizmetler için). Reliable Actors içinde aktör hizmeti kullanın. Aktör hizmeti farklı bir uygulamasıdır `StatefulServiceBase` , aktörler çalıştırdığı aktör deseni uygulayan sınıf. Aktör hizmeti uygulaması yalnızca olduğundan `StatefulServiceBase`, türetilen kendi hizmet yazabilirsiniz `ActorService` ve hizmet düzeyi özellikleri devralırken yaptığınız şekilde uygulamak `StatefulService`, gibi:

* Hizmet yedekleme ve geri yükleme.
* İşlevselliği tüm aktörler, örneğin, bir devre kesici paylaşılan.
* Aktör hizmeti ve tek tek her aktör uzak yordam çağrıları.

### <a name="using-the-actor-service"></a>Aktör hizmeti kullanma
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

### <a name="actor-service-methods"></a>Aktör hizmeti yöntemleri
Aktör hizmeti uygulayan `IActorService` (C#) veya `ActorService` (Java) sırayla uygulayan `IService` (C#) veya `Service` (Java). Bu uzaktan yordam çağrısı hizmeti yöntemleri sağlayan Reliable Services remoting tarafından kullanılan arabirimidir. Hizmet remoting uzaktan çağrılabilir hizmet düzeyi yöntemleri içerir.

#### <a name="enumerating-actors"></a>Aktör numaralandırma
Aktör hizmeti meta veri hizmeti barındırma aktörler hakkında numaralandırılamadı bir istemcinin sağlar. Aktör hizmeti bölümlenmiş bir durum bilgisi olan hizmet olduğundan, numaralandırma bölüm başına gerçekleştirilir. Her bölüm birçok aktörler içerebileceğinden sabit disk belleğine alınmış sonuçlar kümesi olarak döndürülür. Tüm sayfaları oku kadar üzerinden sayfaları döngüye. Aşağıdaki örnekte bir aktör hizmeti bir bölümünü tüm etkin aktörler listesini oluşturulacağını gösterir:

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```

#### <a name="deleting-actors"></a>Aktör silme
Aktör hizmeti aktörler silmek için bir işlevi de sağlar:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

Aktörlerin ve durumlarına silme hakkında daha fazla bilgi için bkz: [aktör yaşam döngüsü belgelerine](service-fabric-reliable-actors-lifecycle.md).

### <a name="custom-actor-service"></a>Özel aktör hizmeti
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

#### <a name="implementing-actor-backup-and-restore"></a>Uygulama aktör yedekleme ve geri yükleme
 Aşağıdaki örnekte, özel aktör hizmeti zaten mevcut remoting dinleyicisi yararlanarak aktör verileri yedeklemek için bir yöntem sunar. `ActorService`:

```csharp
public interface IMyActorService : IService
{
    Task BackupActorsAsync();
}

class MyActorService : ActorService, IMyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }

    public Task BackupActorsAsync()
    {
        return this.BackupAsync(new BackupDescription(PerformBackupAsync));
    }

    private async Task<bool> PerformBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store the contents of backupInfo.Directory
           return true;
        }
        finally
        {
           Directory.Delete(backupInfo.Directory, recursive: true);
        }
    }
}
```
```Java
public interface MyActorService extends Service
{
    CompletableFuture<?> backupActorsAsync();
}

class MyActorServiceImpl extends ActorService implements MyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<FabricActorService, ActorId, ActorBase> newActor)
    {
       super(context, typeInfo, newActor);
    }

    public CompletableFuture backupActorsAsync()
    {
        return this.backupAsync(new BackupDescription((backupInfo, cancellationToken) -> performBackupAsync(backupInfo, cancellationToken)));
    }

    private CompletableFuture<Boolean> performBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store the contents of backupInfo.Directory
           return true;
        }
        finally
        {
           deleteDirectory(backupInfo.Directory)
        }
    }

    void deleteDirectory(File file) {
        File[] contents = file.listFiles();
        if (contents != null) {
            for (File f : contents) {
               deleteDirectory(f);
             }
        }
        file.delete();
    }
}
```


Bu örnekte, `IMyActorService` uygulayan bir remoting sözleşme `IService` (C#) ve `Service` (Java) ve ardından tarafından uygulanan `MyActorService`. Üzerinde bu remoting sözleşme yöntemleri ekleyerek `IMyActorService` şimdi de remoting proxy aracılığıyla oluşturarak istemciye kullanılabilir `ActorServiceProxy`:

```csharp
IMyActorService myActorServiceProxy = ActorServiceProxy.Create<IMyActorService>(
    new Uri("fabric:/MyApp/MyService"), ActorId.CreateRandom());

await myActorServiceProxy.BackupActorsAsync();
```
```Java
MyActorService myActorServiceProxy = ActorServiceProxy.create(MyActorService.class,
    new URI("fabric:/MyApp/MyService"), actorId);

myActorServiceProxy.backupActorsAsync();
```

## <a name="application-model"></a>Uygulama modeli
Uygulama modeli aynı olacak şekilde aktör Reliable Services hizmetleridir. Ancak, aktör framework derleme araçları uygulama modeli dosyaların bazıları sizin için oluşturur.

### <a name="service-manifest"></a>Hizmet bildirimi
Aktör framework derleme araçları aktör hizmetinizin ServiceManifest.xml dosyasının içeriği otomatik olarak oluşturur. Bu dosya içerir:

* Aktör hizmeti türü. Tür adı aktör'ın Proje adına göre oluşturulur. Aktör Kalıcılık öznitelikte bağlı olarak, HasPersistedState bayrağı da buna uygun olarak ayarlanır.
* Kod paketi.
* Yapılandırma paketi.
* Kaynaklar ve uç noktaları.

### <a name="application-manifest"></a>Uygulama bildirimi
Aktör framework derleme araçları varsayılan hizmet tanımı aktör hizmetiniz için otomatik olarak oluşturun. Derleme araçları varsayılan hizmet özelliklerini doldurun:

* Çoğaltma kümesi sayısı, aktör Kalıcılık özniteliği tarafından belirlenir. Aktör Kalıcılık öznitelikte her değiştiğinde çoğaltma kümesi sayısı varsayılan hizmet tanımında buna uygun olarak sıfırlanır.
* Bölüm düzeni ve aralığı Tekdüzen Int64 tam Int64 anahtar aralığıyla ayarlanır.

## <a name="service-fabric-partition-concepts-for-actors"></a>Aktörler için Service Fabric bölümü kavramlar
Aktör Hizmetleri bölümlenmiş durum bilgisi olan hizmetleridir. Aktör hizmeti her bölüm aktörler kümesini içerir. Hizmet bölümlerini Service Fabric birden çok düğümler üzerinden otomatik olarak dağıtılır. Aktör örnekleri sonuç olarak dağıtılır.

![Bölümleme aktör ve dağıtım][5]

Güvenilir hizmetler farklı bir bölüm düzeni ve bölüm anahtarı aralıkları ile oluşturulabilir. Aktör hizmeti aktörler bölümlere eşlemek için tam Int64 anahtar aralığıyla Int64 bölümleme düzenini kullanır.

### <a name="actor-id"></a>Aktör kimliği
Hizmetinde oluşturulan her aktör tarafından temsil edilen ilişkili benzersiz bir Kimliğe sahip `ActorId` sınıfı. `ActorId` rastgele kimlikleri oluşturarak service bölümler aktörler Tekdüzen dağıtım için kullanılabilecek bir donuk kimliği değeridir:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Her `ActorId` bir Int64 karma haline getirilir. Aktör hizmeti bir Int64 bölümleme düzeni tam Int64 anahtar aralığıyla kullanmalısınız nedeni budur. Ancak, özel kimlik değerleri için kullanılabilecek bir `ActorID`GUID/UUID, dizeleri ve Int64s dahil olmak üzere.

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

GUID/UUID ve dizeleri kullanırken, değerlerin bir Int64 karma hale getirilir. Ancak, ne zaman, açıkça sağlayan bir Int64 bir `ActorId`, Int64 daha fazla karma olmadan doğrudan bir bölüm eşler. Aktör yerleştirilir hangi bölümünü denetlemek için bu tekniği kullanabilirsiniz.

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
