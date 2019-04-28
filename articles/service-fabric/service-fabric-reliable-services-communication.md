---
title: Reliable Services iletişimine genel bakış | Microsoft Docs
description: Açılış dinleyicileri Hizmetleri dahil olmak üzere, uç noktaları çözme ve Hizmetleri iletişim güvenilir hizmetler iletişimi modeline genel bakış.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: BharatNarasimman
ms.assetid: 36217988-420e-409d-b0a4-e0e875b6eac8
ms.service: service-fabric
ms.devlang: multiple
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: 15b45cadc69830827952d87ffc2315b06b07b02c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125001"
---
# <a name="how-to-use-the-reliable-services-communication-apis"></a>Reliable Services iletişimi API'leri kullanma
Azure Service Fabric platform olarak hizmetler arasındaki iletişimi hakkında tamamen bağımsızdır. Tüm protokoller ve Yığınlar HTTP UDP Gelen kabul edilebilir. Bu hizmetleri nasıl iletişim kuracağını seçmek için hizmeti Geliştirici aittir. Reliable Services uygulaması çerçevesi yerleşik iletişim yığınlarının yanı sıra, özel iletişim bileşenleri oluşturmak için kullanabileceğiniz API'ler sağlar.

## <a name="set-up-service-communication"></a>Hizmet iletişim kurma ayarlayın
Reliable Services API, hizmet iletişimi için basit bir arabirim kullanır. Hizmetiniz için bir uç noktası'nı açmak için bu arabirimi uygulayan yeterlidir:

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

Sonra hizmet tabanlı sınıfı yöntemi geçersiz kılma seçeneğinde döndürerek iletişim dinleyicisi uygulamanıza ekleyebilirsiniz.

Durum bilgisi olmayan hizmetler için:

```csharp
public class MyStatelessService : StatelessService
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

```java
    @Override
    protected List<ServiceReplicaListener> createServiceReplicaListeners() {
        ...
    }
    ...
```

```csharp
public class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

Her iki durumda da dinleyicileri koleksiyonunu döndürür. Bu, büyük olasılıkla birden çok dinleyici kullanarak farklı protokoller kullanarak birden fazla uç noktası dinleyecek şekilde hizmetinizi sağlar. Örneğin, bir HTTP dinleyicisi ve ayrı bir WebSocket dinleyicisi olabilir. Her dinleyici bir ad ve sonuçta elde edilen koleksiyonunu alır *adı: adresi* çiftleri bir istemci bir hizmet örneği veya bir bölümü için dinleme adresleri istediğinde bir JSON nesnesi olarak temsil edilir.

Bir durum bilgisi olmayan hizmet içinde geçersiz kılma ServiceInstanceListeners koleksiyonunu döndürür. A `ServiceInstanceListener` oluşturmak için bir işlev içeren bir `ICommunicationListener(C#) / CommunicationListener(Java)` ve bir ad verir. Durum bilgisi olan hizmetler için geçersiz kılma ServiceReplicaListeners koleksiyonunu döndürür. Bu durum bilgisi olmayan çözümlemesiyle biraz daha farklı olmasına çünkü bir `ServiceReplicaListener` açmak için bir seçenek sahip bir `ICommunicationListener` ikincil çoğaltmalarındaki. Yalnızca bir hizmet birden fazla iletişim dinleyicileri kullanabilirsiniz, ancak hangi dinleyicileri ikincil çoğaltmalarındaki isteklerini kabul etmek ve hangilerinin yalnızca birincil çoğaltmalara üzerinde dinleme de belirtebilirsiniz.

Örneğin, yalnızca birincil çoğaltma RPC çağrıları alan ServiceRemotingListener olabilir ve salt okunur alan ikinci, özel bir dinleyicisi, HTTP üzerinden ikincil çoğaltmalarda ister:

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
> Bir hizmet, her dinleyici için birden çok dinleyici oluştururken **gerekir** benzersiz bir ad verilmelidir.
>
>

Son olarak, hizmet için gerekli olan uç noktaları açıklayan [hizmet bildirimi](service-fabric-application-and-service-manifests.md) uç noktalarda bölümünün altında.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

İletişim dinleyicisini içinden ayrılan uç nokta kaynaklara erişebilir `CodePackageActivationContext` içinde `ServiceContext`. Dinleyici açıldığında isteklerini dinlemeye başlayabilirsiniz.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> Uç nokta kaynakları tüm hizmet paketi için ortak olan ve hizmet paketi etkin olduğunda Service Fabric tarafından ayrılır. Aynı hizmeti ana bilgisayarında barındırılan birden çok hizmet çoğaltmaları aynı bağlantı noktasını paylaşabiliriz. Bu iletişim dinleyicisi bağlantı noktası paylaşımı desteklemelidir anlamına gelir. Bölüm kimliği ve çoğaltma/örnek kimliği dinleme adresi oluşturduğunda kullanmak için iletişim dinleyicisini bu önerilen yoludur.
>
>

### <a name="service-address-registration"></a>Hizmeti adresi kaydı
Bir sistem hizmeti adlı *adlandırma hizmeti* Service Fabric kümelerinde çalıştırılır. Adlandırma Hizmetleri ve her bir örneği veya hizmet çoğaltması dinlediği adresleri için bir kayıt şirketi hizmetidir. Zaman `OpenAsync(C#) / openAsync(Java)` yöntemi bir `ICommunicationListener(C#) / CommunicationListener(Java)` tamamlandıktan, dönüş değeri adlandırma hizmetinde kayıtlı. Adlandırma Hizmeti yayımlanabilmesi bu dönüş değeri, değeri herhangi bir şey hiç olabilir bir dizedir. Hizmet için bir adres için adlandırma hizmetinden yakalıyorum istemcileri görür Bu dize değeridir.

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

Service Fabric, istemciler ve diğer hizmetler bu adres için hizmet adından sonra istemeniz sağlayan API'ler sağlar. Hizmet adresinin statik olduğundan, bu önemlidir. Hizmetleri kaynak Dengeleme ve kullanılabilirlik amaçları için kümedeki taşındı. Bir hizmet için dinleme adresini çözümlemek istemcilerin mekanizması budur.

> [!NOTE]
> Bir iletişim dinleyicisini yazma tam talimatları için bkz [OWIN kendi kendine barındırma ile Service Fabric Web API Hizmetleri](service-fabric-reliable-services-communication-webapi.md) için C#Java için kendi HTTP sunucusu uygulamasını yazabileceğiniz bilgileriyse EchoServer bakın Uygulama örneğe https://github.com/Azure-Samples/service-fabric-java-getting-started.
>
>

## <a name="communicating-with-a-service"></a>Bir hizmeti ile iletişim
Reliable Services API Hizmetleri ile iletişim kuran istemciler yazmak için aşağıdaki kitaplıklar sağlar.

### <a name="service-endpoint-resolution"></a>Hizmet uç noktası çözümleme
Bir hizmeti ile iletişimi için ilk adımı, bölüm veya konuşabilir istediğiniz hizmeti örneğini bir uç nokta adresini çözümlemek sağlamaktır. `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` Yardımcı sınıfı, temel temel nesne çalışma zamanında bir hizmet uç noktası belirlemek istemciler yardımcı olur. Service Fabric terminolojisinde, bir hizmet uç noktasını belirleme işlemi olarak adlandırılır *hizmet uç noktası çözümleme*.

Bir küme içindeki hizmetlerine bağlanmak için ServicePartitionResolver varsayılan ayarları kullanarak oluşturulabilir. Bu, çoğu durum için önerilen kullanımdır:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

Farklı bir kümede hizmetlerin bağlanmak için küme ağ geçidi uç noktaları bir dizi bir ServicePartitionResolver oluşturulabilir. Ağ geçidi uç noktalar aynı kümeye bağlanmak için yalnızca farklı uç noktalar olduğunu unutmayın. Örneğin:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Alternatif olarak, `ServicePartitionResolver` bir işlev oluşturmak için verilebilir. bir `FabricClient` dahili olarak kullanmak için:

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

`FabricClient` küme üzerinde çeşitli yönetim işlemlerini için Service Fabric kümesi ile iletişim kurmak için kullanılan nesnedir. Hizmet bölüm çözümleyici kümenizle nasıl etkileşim kurduğunu üzerinde daha fazla denetim istediğinizde bu kullanışlıdır. `FabricClient` dahili olarak önbelleğe alma gerçekleştirir ve yeniden geçirmeniz önemlidir oluşturmak, genellikle pahalıdır `FabricClient` mümkün olduğunca örnekleri.

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

Resolve yöntemi, ardından bir hizmet ya da hizmet bölüm bölümlenmiş Hizmetleri adresini almak için kullanılır.

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

Bir hizmet adresi ServicePartitionResolver bir kolayca kullanarak çözülebilir. ancak daha fazla iş çözümlenen adresini doğru kullanılabilir emin olmak için gereklidir. İstemci bağlantı denemesi nedeniyle geçici bir hatayla başarısız oldu ve yeniden denenebilir algılaması gerekir (örneğin, hizmet taşınmış veya geçici olarak kullanılamıyor) ya da kalıcı bir hatayla (örneğin, hizmeti silindi veya istenen kaynak artık yok). Hizmet örneği veya çoğaltmaları düğümden düğüme birden çok nedeniyle herhangi bir zamanda taşıyabilirsiniz. İstemci kodunuz bağlanmaya zaman ServicePartitionResolver çözümlenen hizmet adresini eski olabilir. Bu durumda yeniden adresini çözümlemek yeniden istemcinin gerekir. Önceki sağlama `ResolvedServicePartition` çözümleyici almak yerine yalnızca bir önbelleğe alınan adresi yeniden denemeniz gerektiğini belirtir.

Genellikle, istemci kodu ile ServicePartitionResolver doğrudan işe. Oluşturulur ve güvenilir hizmetler API'si iletişim istemci Fabrikalar geçirilen. Fabrikaları çözümleyici Hizmetleri ile iletişim kurmak için kullanılan istemci nesnesi oluşturmak için dahili olarak kullanın.

### <a name="communication-clients-and-factories"></a>İletişim istemcileri ve fabrikaları
İletişim Fabrika kitaplığı çözümlenen hizmet uç noktalarına deneniyor bağlantıları kolaylaştırır tipik bir hata işleme yeniden deneme düzeni uygular. Hata işleyicilerini sunarken Fabrika kitaplığı yeniden deneme mekanizması sağlar.

`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)` temel bir Service Fabric hizmeti için konuşabilirsiniz istemciler üreten bir iletişim istemci sınıf üreticisi tarafından uygulanan arabirimi tanımlar. CommunicationClientFactory uygulamasını nerede istemci iletişim kurmak istediği Service Fabric hizmeti tarafından kullanılan iletişim yığını üzerinde bağlıdır. Reliable Services API sağlayan bir `CommunicationClientFactoryBase<TCommunicationClient>`. Bu CommunicationClientFactory arabiriminin temel bir uygulamasını sağlar ve tüm iletişim yığınları için ortak olan görevleri gerçekleştirir. (Bu görevler, hizmet uç noktasını belirlemek için bir ServicePartitionResolver kullanarak içerir). İstemcileri, iletişim yığını için belirli mantığı işlemek için soyut CommunicationClientFactoryBase sınıf genellikle uygulama.

İstemci iletişimi yalnızca bir adresi alır ve bir hizmete bağlanmak için kullanır. İstemcinin istediği ne olursa olsun protokolünü kullanabilirsiniz.

```csharp
public class MyCommunicationClient : ICommunicationClient
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

İstemci sınıf üreticisi iletişim istemciler oluşturmak için öncelikli olarak sorumludur. Bir HTTP istemcisi gibi kalıcı bir bağlantı sürdürmenizi olmayan istemciler için Fabrika oluşturma ve istemciye döndürmek yeterlidir. Bazı ikili protokolleri gibi kalıcı bir bağlantı sürdürmenizi diğer protokolleri ayrıca bağlantı yeniden oluşturulması gerekip gerekmediğini belirlemek için Fabrika doğrulanması gerekir.  

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

Son olarak, özel durum işleyicisi bir özel durum oluştuğunda yapılacak eylemi belirlemekten sorumludur. Özel durumlar halinde kategorilere ayrılmış **yeniden denenebilir** ve **denenemeyen**.

* **Denenemeyen** özel durumları yeterlidir çağırana geri döndükten.
* **yeniden denenebilir** özel durumları ek halinde kategorilere **geçici** ve **geçici olmayan**.
  * **Geçici** özel durumları olan, yalnızca Hizmeti uç noktası adresi çözümlemeden yeniden denenebilir. Bu durum geçici ağ sorunları veya hizmet hata yanıtları hizmet uç noktası adresi yok gösteren dışındaki dahil edilir.
  * **Geçici olmayan** özel durumları olanlardır yeniden çözümlenmesi için hizmet uç noktası adresi gerektirir. Hizmet uç noktası ulaşılamıyor belirten özel durumlar bunlar, hizmet belirten farklı bir düğüme taşınmıştır.

`TryHandleException` Belirli bir özel durum hakkında karar verir. Varsa, **bilmediği** döndürmesi gereken bir özel durum hakkında yapmak için hangi kararları **false**. Varsa, **bilen** yapmak için hangi karar ve buna göre sonuç kümesi döndürür **true**.

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
### <a name="putting-it-all-together"></a>Hepsini bir araya getirme
İle bir `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, ve `IExceptionHandler(C#) / ExceptionHandler(Java)` bir iletişim protokolü, yerleşik bir `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` tümünü bir araya sarmalar ve bu bileşenler geçici hata işleme ve hizmet bölümü adresi çözümleme döngü sağlar.

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
* [Reliable Services ile ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)
* [Reliable Services uzaktan iletişimi ile uzak yordam çağrıları](service-fabric-reliable-services-communication-remoting.md)
* [Reliable Services kullanarak WCF iletişim](service-fabric-reliable-services-communication-wcf.md)
