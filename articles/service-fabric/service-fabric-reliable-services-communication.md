---
title: "Güvenilir hizmetler iletişim genel bakış | Microsoft Docs"
description: "Açılış dinleyicileri Hizmetleri dahil olmak üzere, uç noktaları çözme ve Hizmetleri iletişim güvenilir hizmetler iletişim modeline genel bakış."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: 36217988-420e-409d-b0a4-e0e875b6eac8
ms.service: service-fabric
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: 209e657678b7f300f13fc16181a14d8ef422466d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-use-the-reliable-services-communication-apis"></a>Güvenilir hizmetler iletişimi API'lerini kullanma
Bir platform olarak Azure Service Fabric Hizmetleri arasındaki iletişim hakkında tamamen bağımsızdır. Tüm protokoller ve yığınları HTTP UDP Gelen kabul edilir. Bu hizmetleri nasıl iletişim kuracağını seçmek için hizmet geliştiriciler için hazır. Güvenilir hizmetler uygulama çerçevesi yerleşik iletişim yığınları yanı sıra, özel iletişim bileşenleri oluşturmak için kullanabileceğiniz bir API sağlar.

## <a name="set-up-service-communication"></a>Hizmet iletişim kurma ayarlayın
Güvenilir hizmetler API hizmeti iletişimi için basit bir arabirim kullanır. Hizmetiniz için bir uç nokta açmak için yalnızca bu arabirimi uygular:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

```java
public interface CommunicationListener {
    CompletableFuture<String> openAsync(CancellationToken cancellationToken);

    CompletableFuture<?> closeAsync(CancellationToken cancellationToken);

    void abort();
}
```

Hizmet tabanlı sınıfı yöntemi geçersiz kılma seçeneğinde döndürerek iletişimi dinleyicisi uygulamanızı daha sonra ekleyebilirsiniz.

Durum bilgisi olmayan hizmetler için:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```
```java
public class MyStatelessService extends StatelessService {

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ...
    }
    ...
}
```

Durum bilgisi olan hizmetler için:

> [!NOTE]
> Durum bilgisi olan güvenilir hizmetler Java'da henüz desteklenmiyor.
>
>

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

Her iki durumda da dinleyicileri koleksiyonunu döndürür. Bu, büyük olasılıkla birden çok dinleyici kullanarak farklı protokoller kullanarak birden fazla uç noktası üzerinde dinleme hizmetinizi sağlar. Örneğin, bir HTTP dinleyicisi ve ayrı bir Web yuvası dinleyicisi olabilir. Her dinleyicisi bir ad ve sonuçta elde edilen koleksiyonunu alır *ad: adresi* bir istemci bir hizmet örneği veya bir bölüm için dinleme adresleri istediğinde, çiftleri bir JSON nesnesi olarak temsil.

Durum bilgisiz hizmetinde, geçersiz kılma ServiceInstanceListeners koleksiyonunu döndürür. A `ServiceInstanceListener` oluşturmak için bir işlev içeren bir `ICommunicationListener(C#) / CommunicationListener(Java)` ve bir ad sağlar. Durum bilgisi olan hizmetler için geçersiz kılma ServiceReplicaListeners koleksiyonunu döndürür. Bu durum bilgisiz kendisine karşılık gelen biraz farklı değildir çünkü bir `ServiceReplicaListener` açmak için bir seçenek olan bir `ICommunicationListener` ikincil çoğaltmalar üzerinde. Yalnızca bir hizmet birden çok iletişim dinleyicileri kullanabilirsiniz, ancak aynı zamanda hangi dinleyicileri ikincil çoğaltmalar üzerinde isteklerini kabul etmek ve hangilerinin yalnızca birincil çoğaltmalar üzerinde dinleme belirtebilirsiniz.

Örneğin, yalnızca birincil çoğaltmalar RPC çağrılarını alan bir ServiceRemotingListener sahip olabilir ve okuma geçen ikinci, özel bir dinleyici ikincil çoğaltmaların üzerine HTTP üzerinden ister:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [!NOTE]
> Hizmet için her dinleyicisi birden çok dinleyici oluştururken **gerekir** benzersiz bir ad verilmelidir.
>
>

Son olarak, hizmet için gerekli olan uç noktaları açıklayan [hizmet bildirimi](service-fabric-application-and-service-manifests.md) uç noktalarda bölümünde.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

İletişim dinleyicisi içinden ayrılan uç nokta kaynaklara erişebilir `CodePackageActivationContext` içinde `ServiceContext`. Dinleyici açıldığında isteklerini dinlemeye başlatabilirsiniz.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> Uç nokta kaynakları tüm hizmet paketi ortak olan ve hizmet paketi etkinleştirildiğinde Service Fabric tarafından ayrılır. Aynı hizmeti ana bilgisayarında barındırılan birden çok hizmet çoğaltmalar aynı bağlantı noktasını paylaşabilir. Bu iletişim dinleyicisi bağlantı noktası paylaşımı desteklemelidir anlamına gelir. Bölüm kimliği ve çoğaltma/örnek kimliği dinleme adresi oluşturduğunda kullanma iletişimi dinleyici için Bunun yapılması, önerilen yoludur.
>
>

### <a name="service-address-registration"></a>Hizmeti adresi kaydı
Bir sistem hizmeti adlı *adlandırma hizmeti* Service Fabric kümelerinde çalışır. Adlandırma Hizmeti hizmetleri ve her bir örneği veya hizmet çoğaltması dinlediği adresleri için bir kayıt var. Zaman `OpenAsync(C#) / openAsync(Java)` yöntemi bir `ICommunicationListener(C#) / CommunicationListener(Java)` tamamlar, dönüş değeri adlandırma hizmetinde kayıtlı. Adlandırma Hizmeti yayımlanabilmesi bu dönüş değeri değeri herhangi bir şey hiç olabilir bir dizedir. Bunlar hizmet için bir adres için adlandırma hizmetinden söylediğinizde istemcileri gördükleri Bu dize değeridir.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```
```java
public CompletableFuture<String> openAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.getCodePackageActivationContext.getEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.getPort();

    this.publishAddress = String.format("http://%s:%d/", FabricRuntime.getNodeContext().getIpAddressOrFQDN(), port);

    this.webApp = new WebApp(port);
    this.webApp.start();

    /* the string returned here will be published in the Naming Service.
     */
    return CompletableFuture.completedFuture(this.publishAddress);
}
```

Service Fabric istemciler ve diğer hizmetler bu adres için hizmet adına göre sonra sorulacak izin API'ler sağlar. Hizmeti adresi statik olmadığı için bu önemlidir. Hizmetleri kaynak Dengeleme ve kullanılabilirlik amaçları için kümedeki taşındı. Bir hizmet için dinleme adresini çözümlemek istemcilerin mekanizması budur.

> [!NOTE]
> İletişim dinleyici yazmak nasıl tam bir kılavuz için bkz: [OWIN kendi kendine barındırma ile Service Fabric Web API Hizmetleri](service-fabric-reliable-services-communication-webapi.md) Java için kendi HTTP sunucusu uygulamasını yazabilirsiniz gelirken, C# ' ta https://github.com/Azure-Samples/service-fabric-java-getting-started EchoServer uygulama örneğe bakın.
>
>

## <a name="communicating-with-a-service"></a>Bir hizmet ile iletişim
Güvenilir hizmetler API Hizmetleri ile iletişim kuran istemciler yazmak için aşağıdaki kitaplıkları sağlar.

### <a name="service-endpoint-resolution"></a>Hizmet uç noktası çözümleme
İlk hizmet ile iletişim için bölüm veya konuşun istediğiniz hizmet örneğini bir uç nokta adresini çözümlemek için adımdır. `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` Yardımcı programının sınıfıdır çalışma zamanında bir hizmetin uç noktası belirlemek istemcileri yardımcı olan basit bir temel tür. Service Fabric terminolojisinde hizmet uç noktası belirleme işlemi olarak adlandırılır *hizmet uç noktası çözümleme*.

Bir küme içinde hizmetlerine bağlanmak için ServicePartitionResolver varsayılan ayarları kullanarak oluşturulabilir. Bu çoğu durum için önerilen kullanım şu şekildedir:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

Farklı bir kümeye Hizmetleri'nde bağlanmak için küme ağ geçidi uç noktaları kümesiyle bir ServicePartitionResolver oluşturulabilir. Ağ geçidi uç noktalar aynı kümeye bağlanmak için yalnızca farklı uç noktalar olduğunu unutmayın. Örneğin:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Alternatif olarak, `ServicePartitionResolver` oluşturmak için bir işlev verilebilir bir `FabricClient` dahili olarak kullanmak üzere:

```csharp
public delegate FabricClient CreateFabricClientDelegate();
```
```java
public FabricServicePartitionResolver(CreateFabricClient createFabricClient) {
...
}

public interface CreateFabricClient {
    public FabricClient getFabricClient();
}
```

`FabricClient`kümede çeşitli yönetim işlemlerini Service Fabric kümesi ile iletişim kurmak için kullanılan nesne değil. Hizmet bölüm çözümleyici kümenizle nasıl etkileşim kurduğu üzerinde daha fazla denetim istediğinizde kullanışlıdır. `FabricClient`dahili olarak önbelleğe alma gerçekleştirir ve yeniden önemlidir oluşturmak, genellikle pahalı `FabricClient` mümkün olduğunca örnekleri.

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

Resolve yöntemi sonra bir hizmet veya hizmet bölüm bölümlenmiş Hizmetleri için adresini almak için kullanılır.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();

CompletableFuture<ResolvedServicePartition> partition =
    resolver.resolveAsync(new URI("fabric:/MyApp/MyService"), new ServicePartitionKey());
```

Bir hizmet adresi kolayca bir ServicePartitionResolver kullanılarak çözülebilir. ancak daha fazla iş çözümlenen adresi doğru kullanılabilir emin olmak için gereklidir. Bağlantı denemesi geçici bir hata nedeniyle başarısız oldu ve yeniden denenebilir olup olmadığını algılamak istemci gerekir (örn., hizmet taşınmış veya geçici olarak kullanım dışı), ya da kalıcı bir hata (örneğin, hizmet silindi veya istenen kaynak artık yok). Hizmet örneği veya çoğaltmaları düğümden düğüme birden çok nedeniyle herhangi bir zamanda taşıyabilirsiniz. ServicePartitionResolver çözümlenen hizmet adresini, istemci kodunuzun bağlanma girişiminde zamanına göre eski olabilir. Bu durumda adresi yeniden çözümlemek yeniden istemcinin gerekir. Önceki sağlama `ResolvedServicePartition` çözümleyici yalnızca almak yerine önbelleğe alınmış bir adresi yeniden denemeniz gerektiğini belirtir.

Genellikle, istemci kodu ServicePartitionResolver ile doğrudan işe. Oluşturulan ve güvenilir hizmetler API'sindeki iletişimi istemci üreteçlerine geçirildi. Oluşturucuları çözümleyici Hizmetleri ile iletişim kurmak için kullanılan istemci nesnesi oluşturmak için dahili olarak kullanın.

### <a name="communication-clients-and-factories"></a>İletişim istemcileri ve oluşturucuları
İletişim Fabrika kitaplığı çözümlenmiş hizmet uç noktaları için yeniden deneniyor bağlantıları kolaylaştırır tipik bir hata işleme yeniden deneme deseni uygular. Hata işleyicileri sunarken Fabrika kitaplığı yeniden deneme mekanizması sağlar.

`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`bir Service Fabric hizmeti iletişim kurabilirsiniz istemcileri üreten bir iletişim istemci sınıf üreticisi tarafından uygulanan temel arabirimi tanımlar. CommunicationClientFactory uyarlamasını nerede iletişim kurmak istemcinin istediği Service Fabric hizmeti tarafından kullanılan iletişim yığında bağlıdır. Güvenilir hizmetler API sağlayan bir `CommunicationClientFactoryBase<TCommunicationClient>`. Bu CommunicationClientFactory arabiriminin temel bir uygulama sağlar ve tüm iletişimi yığınları için ortak olan görevleri gerçekleştirir. (Hizmet uç noktası belirlemek için bir ServicePartitionResolver kullanarak bu görevleri dahil). İstemcileri, iletişim yığını belirli mantığı işlemek için Özet CommunicationClientFactoryBase sınıf genellikle uygulayın.

Communication istemcisi yalnızca bir adresi alır ve bir hizmete bağlanmak için kullanır. İstemcinin istediği ne olursa olsun protokolünü kullanabilirsiniz.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```
```java
public class MyCommunicationClient implements CommunicationClient {

    private ResolvedServicePartition resolvedServicePartition;
    private String listenerName;
    private ResolvedServiceEndpoint endPoint;

    /*
     * Getters and Setters
     */
}
```

İstemci sınıf üreticisi iletişimi istemcileri oluşturmak için öncelikle sorumludur. Bir HTTP istemcisi gibi kalıcı bir bağlantı korumak olmayan istemciler için Fabrika oluşturup istemci dönmek yeterlidir. Bazı ikili protokolleri gibi kalıcı bir bağlantı korumak diğer protokolleri ayrıca bağlantı yeniden oluşturulması gerekip gerekmediğini belirlemek için fabrika tarafından doğrulanmalıdır.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```
```java
public class MyCommunicationClientFactory extends CommunicationClientFactoryBase<MyCommunicationClient> {

    @Override
    protected boolean validateClient(MyCommunicationClient clientChannel) {
    }

    @Override
    protected boolean validateClient(String endpoint, MyCommunicationClient client) {
    }

    @Override
    protected CompletableFuture<MyCommunicationClient> createClientAsync(String endpoint) {
    }

    @Override
    protected void abortClient(MyCommunicationClient client) {
    }
}
```

Son olarak, bir özel durum oluştuğunda gerçekleştirilecek eylemi belirlemek için bir özel durum işleyici sorumludur. Özel durumlar kategorilere içine **yeniden denenebilir** ve **denenemeyen**.

* **Denenemeyen** özel durumlar basitçe çağıran işlenemezse.
* **yeniden denenebilir** özel durumlar daha fazla kategorilere ayrılabilir içine **geçici** ve **geçici olmayan**.
  * **Geçici** istisnaları, yalnızca hizmet uç noktası adresi yeniden çözümlemeden yeniden denenebilir. Bu durum geçici ağ sorunları veya hizmet hata yanıtları hizmet uç noktası adresi yok gösteren olanlar dışındaki dahil edilir.
  * **Geçici olmayan** istisnaları yeniden çözümlenmesi için hizmet uç noktası adresi gerektirenler. Bu hizmet uç noktası ulaşılamıyor belirtmek özel durumları içerecek, hizmet gösteren farklı bir düğüme taşınmıştır.

`TryHandleException` Belirli bir özel durum hakkında karar verir. Varsa, **bilmediği** döndürme zorunluluğu bir özel durum hakkında yapmak için hangi kararları **false**. Varsa, **bilen** yapmak için hangi karar sonucu uygun şekilde ayarlama ve döndürme **doğru**.

```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
```java
public class MyExceptionHandler implements ExceptionHandler {

    @Override
    public ExceptionHandlingResult handleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings) {        

        /* if exceptionInformation.getException() is known and is transient (can be retried without re-resolving)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), true, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;


        /* if exceptionInformation.getException() is known and is not transient (indicates a new service endpoint address must be resolved)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), false, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;

        /* if exceptionInformation.getException() is unknown (let the next ExceptionHandler attempt to handle it)
         */
        result = null;
        return false;

    }
}
```
### <a name="putting-it-all-together"></a>Tüm bir araya getirme
İle bir `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, ve `IExceptionHandler(C#) / ExceptionHandler(Java)` bir iletişim protokolü, yerleşik bir `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` hepsini bir araya sarmalar ve bu bileşenleri geçici hata işleme ve hizmet bölümü adres çözünürlüğü döngü sağlar.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```
```java
private MyCommunicationClientFactory myCommunicationClientFactory;
private URI myServiceUri;

FabricServicePartitionClient myServicePartitionClient = new FabricServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

CompletableFuture<?> result = myServicePartitionClient.invokeWithRetryAsync(client -> {
      /* Communicate with the service using the client.
       */
   });

```

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir hizmetler ile ASP.NET Çekirdeği](service-fabric-reliable-services-communication-aspnetcore.md)
* [Güvenilir hizmetler remoting ile uzak yordam çağrıları](service-fabric-reliable-services-communication-remoting.md)
* [Güvenilir hizmetler kullanarak WCF iletişimi](service-fabric-reliable-services-communication-wcf.md)
