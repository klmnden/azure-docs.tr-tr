---
title: C# kullanarak Service Fabric'te hizmet uzaktan iletişimini | Microsoft Docs
description: Service Fabric uzak istemciler ve hizmetler uzak yordam çağrısı kullanarak C# hizmetlerle iletişim sağlar.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: abfaf430-fea0-4974-afba-cfc9f9f2354b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 09/20/2017
ms.author: vturecek
ms.openlocfilehash: 7afa50484c3ebf258bbdd2b7f16c9cd051710d28
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37437901"
---
# <a name="service-remoting-in-c-with-reliable-services"></a>C# Reliable Services ile Service uzaktan iletişim
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-communication-remoting.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Belirli bir iletişim protokolü veya Webapı, Windows Communication Foundation (WCF) veya diğerleri gibi bir yığın bağlı olmayan hizmetler için Reliable Services framework uzak yordam çağrıları için hızlı ve kolay bir şekilde ayarlamak için uzaktan iletişim mekanizması sağlar Hizmetler. Bu makalede, C# ile yazılmış hizmetler için uzak yordam çağrılarını ayarlama anlatılmaktadır.

## <a name="set-up-remoting-on-a-service"></a>Bir hizmeti uzaktan iletişim ayarlamak
Bir hizmet için uzaktan iletişim kurma, iki basit adımda gerçekleştirilir:

1. Bir arabirim uygulamak hizmet oluşturun. Bu arabirim, hizmetinizde bir uzak yordam çağrısı için kullanılabilen yöntemleri tanımlar. Görev döndüren yöntemler olmalıdır zaman uyumsuz yöntemler. Arabirimi uygulamalıdır `Microsoft.ServiceFabric.Services.Remoting.IService` hizmet uzaktan iletişim arabirimi olduğunu göstermek için.
2. Uzaktan iletişim dinleyicisi hizmetinizde kullanın. RemotingListener olduğu bir `ICommunicationListener` remoting özellikleri sağlayan uygulama. `Microsoft.ServiceFabric.Services.Remoting.Runtime` Ad alanı içeren bir uzantı yöntemini`CreateServiceRemotingListener` varsayılan uzaktan iletişim aktarım protokolünü kullanarak bir uzaktan iletişim dinleyicisi oluşturmak için kullanılan durum bilgisiz ve durum bilgisi olan hizmetler için.

>[!NOTE]
>`Remoting` Ad alanını adlı ayrı bir NuGet paketi kullanılabilir `Microsoft.ServiceFabric.Services.Remoting`

Örneğin, aşağıdaki durum bilgisi olmayan hizmet uzaktan prosedür çağrılarında "Hello World" almak için tek bir yöntemi gösterir.

```csharp
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Remoting;
using Microsoft.ServiceFabric.Services.Remoting.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

public interface IMyService : IService
{
    Task<string> HelloWorldAsync();
}

class MyService : StatelessService, IMyService
{
    public MyService(StatelessServiceContext context)
        : base (context)
    {
    }

    public Task<string> HelloWorldAsync()
    {
        return Task.FromResult("Hello!");
    }

    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] { new ServiceInstanceListener(context => this.CreateServiceRemotingListener(context)) };
    }
}
```
> [!NOTE]
> Bağımsız değişkenler ve hizmet arabirimi dönüş türlerinde herhangi bir basit, karmaşık veya özel türü olabilir, ancak .NET tarafından seri hale getirilebilir olması gerekir [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).
>
>

## <a name="call-remote-service-methods"></a>Uzak Hizmet yöntemleri çağırma
Uzaktan iletişim yığını kullanarak bir hizmet yöntemleri çağırma yapılır bir yerel ara sunucu üzerinden hizmete kullanarak `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` sınıfı. `ServiceProxy` Yöntemi hizmeti uygulayan aynı arabirimi kullanarak yerel bir proxy oluşturur. Proxy ile yöntemleri arabirimi aşağıdaki uzaktan çağırabilirsiniz.

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

Uzaktan iletişimini framework istemciye hizmet tarafından oluşturulan özel durumları yayar. Kullanırken bir sonucu olarak `ServiceProxy`, istemci, hizmet tarafından oluşturulan özel durumları işlenmesinden sorumludur.

## <a name="service-proxy-lifetime"></a>Hizmet ara sunucu ömrünü
ServiceProxy oluşturma hafif bir işlem olduğundan, ihtiyacınız kadar oluşturabilirsiniz. Gerekli olan sürece hizmet proxy'si örneği yeniden kullanılabilir. Uzak yordam çağrısı, bir özel durum oluşturursa, yine de aynı proxy örneği yeniden kullanabilirsiniz. Her ServiceProxy kablo üzerinden ileti göndermek için kullanılan bir iletişim istemcisi içerir. Uzak çağrılar çağrılırken, dahili denetimler iletişim istemci geçerli olup olmadığını belirlemek için gerçekleştirilir. Bu denetimlerin sonuçlarına göre iletişim istemci gerekirse yeniden oluşturulur. Bir özel durum oluşursa, bu nedenle, yeniden oluşturmak ihtiyacınız olmayan `ServiceProxy`.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory ömrü
[ServiceProxyFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) olan farklı bir uzak arabirimler için Ara sunucu örnekleri oluşturan bir üreteci. API kullanırsanız `ServiceProxy.Create` proxy oluşturmak, sonra framework ServiceProxy tek oluşturur.
Geçersiz kılmak gerektiğinde el ile oluşturmak kullanışlıdır [IServiceRemotingClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) özellikleri.
Fabrikası oluşturma pahalı bir işlemdir. Bir iç iletişim istemci önbelleğini ServiceProxyFactory tutar.
ServiceProxyFactory olabildiğince uzun bir süre için önbelleğe en iyi uygulamadır.

## <a name="remoting-exception-handling"></a>Uzak özel durum işleme
Hizmet API'si tarafından oluşturulan tüm uzak özel durumlar AggregateException istemciye geri gönderilir. RemoteExceptions DataContract Serileştirilebilir olmalıdır; değilseniz, proxy API oluşturur [ServiceException](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) ile seri hale getirme hatası.

ServiceProxy için oluşturulan hizmet bölümü için tüm yük devretme özel durumları işler. Yük devretme özel durumlar (geçici olmayan özel durumlar) varsa, uç noktaları yeniden giderir ve doğru uç noktası ile çağrı yeniden denenir. Yük devretme özel durumlar için yeniden deneme sayısını belirsiz.
Proxy geçici özel durumlar oluşursa, çağrı yeniden dener.

Varsayılan yeniden deneme parametreleridir tarafından bulunduğunda [OperationRetrySettings](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings).

OperationRetrySettings ServiceProxyFactory oluşturucusuna geçirerek bu değerleri yapılandırabilirsiniz.

## <a name="how-to-use-the-remoting-v2-stack"></a>Uzaktan iletişim V2 yığın kullanma

NuGet uzaktan Paket sürümü itibarıyla 2.8 uzaktan iletişim V2 yığın kullanılacak seçeneğiniz vardır. Uzaktan iletişim V2 yığın daha fazla yüksek performanslı ve özel seri hale getirme ve daha takılabilir API'nin gibi özellikler sağlar.
Şablon kodunun, uzaktan iletişim V1 Stack kullanmaya devam eder.
Uzaktan iletişim V2 V1 ile uyumlu değil (önceki uzaktan iletişim yığını), bu nedenle aşağıdaki yönergeleri izleyin üzerinde [V1'den V2'ye yükseltme](#how-to-upgrade-from-remoting-v1-to-remoting-v2) hizmet kullanılabilirliği etkilemeden.

V2 yığın etkinleştirmek aşağıdaki yaklaşımlar kullanılabilir.

### <a name="using-an-assembly-attribute-to-use-the-v2-stack"></a>Bir derleme özniteliğini kullanarak V2 yığını kullanın

Bu adımları bir derleme özniteliğini kullanarak V2 yığın kullanmak için şablonu kodu değiştirin.

1. Uç nokta kaynak değiştirme `"ServiceEndpoint"` için `"ServiceEndpointV2"` hizmet bildirimindeki.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />
    </Endpoints>
  </Resources>
  ```

2. Kullanım `Microsoft.ServiceFabric.Services.Remoting.Runtime.CreateServiceRemotingInstanceListeners` uzaktan iletişim dinleyicileri (V1 ve V2 için eşittir) oluşturmak için genişletme yöntemi.

  ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
  ```

3. Uzaktan iletişimini arabirimleriyle içeren derlemeyi işaretlemek bir `FabricTransportServiceRemotingProvider` özniteliği.

  ```csharp
  [assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.V2Listener, RemotingClient = RemotingClient.V2Client)]
  ```

İstemci projede hiçbir kod değişikliği gerekir.
Yukarıda gösterilen derleme özniteliğini kullanıldığından emin olmak için arabirimi derleme ile istemci derleme.

### <a name="using-explicit-v2-classes-to-use-the-v2-stack"></a>V2 yığınını kullanmak için açık V2 sınıfları kullanma

Bir derleme özniteliğini kullanarak alternatif olarak, V2 yığın açık V2 sınıflarını kullanarak da etkinleştirilebilir.

Bu adımları açık V2 sınıflarını kullanarak V2 yığın kullanılacak şablon kodunu değiştirin.

1. Uç nokta kaynak değiştirme `"ServiceEndpoint"` için `"ServiceEndpointV2"` hizmet bildirimindeki.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2" />
    </Endpoints>
  </Resources>
  ```

2. Kullanım [FabricTransportServiceRemotingListener](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.runtime.fabrictransportserviceremotingListener?view=azure-dotnet) gelen `Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Runtime` ad alanı.

  ```csharp
  protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[]
        {
            new ServiceInstanceListener((c) =>
            {
                return new FabricTransportServiceRemotingListener(c, this);

            })
        };
    }
  ```

3. Kullanım [FabricTransportServiceRemotingClientFactory ](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.client.fabrictransportserviceremotingclientfactory?view=azure-dotnet) gelen `Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Client` istemcileri oluşturmak için ad alanı.

  ```csharp
  var proxyFactory = new ServiceProxyFactory((c) =>
          {
              return new FabricTransportServiceRemotingClientFactory();
          });
  ```

## <a name="how-to-upgrade-from-remoting-v1-to-remoting-v2"></a>Uzaktan iletişim V2'ye uzaktan iletişim V1'den yükseltme yapma.
V1'den V2'ye yükseltmek için 2 adımlı yükseltme gerekli değildir. Listelenen sırayla yürütülmesi için aşağıdaki adımları içerir.

1. V1 hizmeti V2 hizmetine CompactListener özniteliğini kullanarak yükseltin.
Bu değişiklik, hizmet V1 ve V2 dinleyicisinin dinleme emin olur.

    (a) adlı bir uç nokta kaynağı "ServiceEndpointV2" hizmet bildiriminde ekleyin.
      ```xml
      <Resources>
        <Endpoints>
          <Endpoint Name="ServiceEndpointV2" />  
        </Endpoints>
      </Resources>
      ```

    (b) genişletme yöntemi aşağıdaki uzaktan iletişim dinleyicisi oluşturmak için kullanın.

    ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
    ```

    c) uzaktan iletişimi CompatListener ve V2 istemci arabirimleri üzerinde derleme özniteliğini ekleyin.
    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.CompatListener, RemotingClient = RemotingClient.V2Client)]

      ```
2. V1 istemci V2 istemciye V2 istemci özniteliğini kullanarak yükseltin.
Bu adım, istemci V2 yığın kullandığından emin sağlar.
İstemci projesi/hizmet değişiklik gerekli değil. Güncelleştirilmiş arabirimi derleme ile istemci projeler derleme yeterli olur.

3. Bu adım isteğe bağlıdır. V2Listener özniteliğini kullanın ve ardından V2 hizmet yükseltin.
Bu adım yalnızca V2 Dinleyicide dinleyen bir hizmet emin olur.

```csharp
[assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.V2Listener, RemotingClient = RemotingClient.V2Client)]
```

## <a name="how-to-use-custom-serialization-with-remoting-v2"></a>Uzaktan iletişim V2 ile özel serileştirme kullanma
Aşağıdaki örnek, uzaktan iletişim V2 ile Json seri hale getirme kullanır.
1. Özel serileştirme için bir uygulama sağlamak için IServiceRemotingMessageSerializationProvider arabirimini uygular.
    Nasıl uygulama gibi göründüğünü kod parçacığı aşağıdadır.

 ```csharp
    public class ServiceRemotingJsonSerializationProvider : IServiceRemotingMessageSerializationProvider
    {
        public IServiceRemotingRequestMessageBodySerializer CreateRequestMessageSerializer(Type serviceInterfaceType,
            IEnumerable<Type> requestBodyTypes)
        {
            return new ServiceRemotingRequestJsonMessageBodySerializer(serviceInterfaceType, requestBodyTypes);
        }

        public IServiceRemotingResponseMessageBodySerializer CreateResponseMessageSerializer(Type serviceInterfaceType,
            IEnumerable<Type> responseBodyTypes)
        {
            return new ServiceRemotingResponseJsonMessageBodySerializer(serviceInterfaceType, responseBodyTypes);
        }

        public IServiceRemotingMessageBodyFactory CreateMessageBodyFactory()
        {
            return new JsonMessageFactory();
        }
    }

    class JsonMessageFactory : IServiceRemotingMessageBodyFactory
    {
        public IServiceRemotingRequestMessageBody CreateRequest(string interfaceName, string methodName,
            int numberOfParameters)
        {
            return new JsonRemotingRequestBody();
        }

        public IServiceRemotingResponseMessageBody CreateResponse(string interfaceName, string methodName)
        {
            return new JsonRemotingResponseBody();
        }
    }

    class ServiceRemotingRequestJsonMessageBodySerializer : IServiceRemotingRequestMessageBodySerializer
    {
        public ServiceRemotingRequestJsonMessageBodySerializer(Type serviceInterfaceType,
            IEnumerable<Type> parameterInfo)
        {
        }

        public OutgoingMessageBody Serialize(IServiceRemotingRequestMessageBody serviceRemotingRequestMessageBody)
        {
            if (serviceRemotingRequestMessageBody == null)
            {
                return null;
            }

            var writeStream = new MemoryStream();
            var jsonWriter = new JsonTextWriter(new StreamWriter(writeStream));

            var serializer = JsonSerializer.Create(new JsonSerializerSettings()
            {
                TypeNameHandling = TypeNameHandling.All
            });
            serializer.Serialize(jsonWriter, serviceRemotingRequestMessageBody);

            jsonWriter.Flush();
            var segment = new ArraySegment<byte>(writeStream.ToArray());
            var segments = new List<ArraySegment<byte>> { segment };
            return new OutgoingMessageBody(segments);
        }

        public IServiceRemotingRequestMessageBody Deserialize(IncomingMessageBody messageBody)
        {
            using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))

            using (JsonReader reader = new JsonTextReader(sr))
            {
                var serializer = JsonSerializer.Create(new JsonSerializerSettings()
                {
                    TypeNameHandling = TypeNameHandling.All
                });

                return serializer.Deserialize<JsonRemotingRequestBody>(reader);
            }
        }
    }

    class ServiceRemotingResponseJsonMessageBodySerializer : IServiceRemotingResponseMessageBodySerializer
    {
        public ServiceRemotingResponseJsonMessageBodySerializer(Type serviceInterfaceType,
            IEnumerable<Type> parameterInfo)
        {
        }

        public OutgoingMessageBody Serialize(IServiceRemotingResponseMessageBody responseMessageBody)
        {
            var json = JsonConvert.SerializeObject(responseMessageBody, new JsonSerializerSettings()
            {
                TypeNameHandling = TypeNameHandling.All
            });
            var bytes = Encoding.UTF8.GetBytes(json);
            var segment = new ArraySegment<byte>(bytes);
            var list = new List<ArraySegment<byte>> { segment };
            return new OutgoingMessageBody(list);
        }

        public IServiceRemotingResponseMessageBody Deserialize(IncomingMessageBody messageBody)
        {
            using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))

            using (var reader = new JsonTextReader(sr))
            {
                var serializer = JsonSerializer.Create(new JsonSerializerSettings()
                {
                    TypeNameHandling = TypeNameHandling.All
                });

                return serializer.Deserialize<JsonRemotingResponseBody>(reader);
            }
        }
    }

    internal class JsonRemotingResponseBody : IServiceRemotingResponseMessageBody
    {
        public object Value;

        public void Set(object response)
        {
            this.Value = response;
        }

        public object Get(Type paramType)
        {
            return this.Value;
        }
    }

    class JsonRemotingRequestBody : IServiceRemotingRequestMessageBody
    {
        public readonly Dictionary<string, object> parameters = new Dictionary<string, object>();        

        public void SetParameter(int position, string parameName, object parameter)
        {
            this.parameters[parameName] = parameter;
        }

        public object GetParameter(int position, string parameName, Type paramType)
        {
            return this.parameters[parameName];
        }
    }
 ```

2.    Varsayılan serileştirme JsonSerializationProvider sağlayıcısıyla uzaktan iletişim dinleyicisi için geçersiz kılın.

  ```csharp
  protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
   {
       return new[]
       {
           new ServiceInstanceListener((c) =>
           {
               return new FabricTransportServiceRemotingListener(c, this,
                   new ServiceRemotingJsonSerializationProvider());
           })
       };
   }
  ```

3.    Varsayılan serileştirme JsonSerializationProvider sağlayıcısıyla uzak istemci sınıf üreticisi için geçersiz kılın.

```csharp
  var proxyFactory = new ServiceProxyFactory((c) =>
            {
                return new FabricTransportServiceRemotingClientFactory(
                    serializationProvider: new ServiceRemotingJsonSerializationProvider());
            });
  ```

## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Services özelliğinde OWIN ile Web API'si](service-fabric-reliable-services-communication-webapi.md)
* [Reliable Services WCF iletişim](service-fabric-reliable-services-communication-wcf.md)
* [Reliable Services için iletişimin güvenliğini sağlama](service-fabric-reliable-services-secure-communication.md)

