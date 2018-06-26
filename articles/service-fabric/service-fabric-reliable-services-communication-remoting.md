---
title: C# kullanarak Service Fabric hizmeti remoting | Microsoft Docs
description: Service Fabric uzak istemciler ve hizmetler uzaktan yordam çağrısı kullanarak C# Hizmetleri ile iletişim kurmasına olanak sağlar.
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
ms.openlocfilehash: ad56580e73c06acff95b3146f6dc2d83ab2ba3ae
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36945981"
---
# <a name="service-remoting-in-c-with-reliable-services"></a>C# Reliable Services ile hizmet uzaktan iletişim
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-communication-remoting.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Belirli bir iletişim protokolü veya Webapı, Windows Communication Foundation (WCF) veya diğerleri gibi yığın bağlı olmayan hizmetler için uzak yordam çağrıları için hızlı ve kolay bir şekilde ayarlamak için uzaktan iletişim mekanizması Reliable Services çerçevesi sağlar Hizmetler. Bu makalede nasıl C# ile yazılmış hizmetler için uzaktan yordam çağrılarını ayarlanacağı açıklanmaktadır.

## <a name="set-up-remoting-on-a-service"></a>Bir hizmeti uzaktan iletişim ayarlama
Bir hizmet için uzaktan iletişim kurma iki basit adımda gerçekleştirilir:

1. Hizmetinizin uygulamak bir arabirim oluşturun. Bu arabirim, hizmeti uzaktan yordam çağrısı için kullanılabilen yöntemleri tanımlar. Görev döndüren yöntemler olmalıdır zaman uyumsuz yöntemleri. Arabirimi uygulamalıdır `Microsoft.ServiceFabric.Services.Remoting.IService` hizmet remoting arabirimi olduğunu göstermek için.
2. Remoting dinleyici hizmetinizi kullanın. RemotingListener olan bir `ICommunicationListener` remoting özellikleri sağlayan uygulama. `Microsoft.ServiceFabric.Services.Remoting.Runtime` Ad alanı içeren bir genişletme yöntemi`CreateServiceRemotingListener` varsayılan remoting aktarım protokolünü kullanarak bir uzak dinleyicisi oluşturmak için kullanılan durum bilgisiz ve durum bilgisi olan hizmetler için.

>[!NOTE]
>`Remoting` Ad alanı adlı ayrı bir NuGet paketi kullanılabilir `Microsoft.ServiceFabric.Services.Remoting`

Örneğin, aşağıdaki durum bilgisiz hizmeti üzerinden bir uzak yordam çağrısı "Hello World" almak için tek bir yöntemi gösterir.

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
> Bağımsız değişkenler ve hizmet arabirimi dönüş türler basit, karmaşık veya özel türleri olabilir, ancak .NET tarafından Serileştirilebilir olmalıdır [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).
>
>

## <a name="call-remote-service-methods"></a>Uzak Hizmet yöntemleri çağırma
Uzaktan iletişim yığını kullanarak bir hizmet yöntemleri çağırma yapılır yerel bir proxy üzerinden hizmete kullanarak `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` sınıfı. `ServiceProxy` Yöntemi hizmeti uygulayan aynı arabirimini kullanarak yerel bir proxy oluşturur. Bu proxy ile yöntemleri arabirimde uzaktan çağırabilirsiniz.

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

Remoting framework istemciye hizmeti tarafından oluşturulan özel durumları yayar. Kullanırken, sonuç olarak, `ServiceProxy`, istemci hizmeti tarafından oluşturulan özel durumları işlemekten sorumludur.

## <a name="service-proxy-lifetime"></a>Hizmet Proxy ömrü
ServiceProxy oluşturma hafif bir işlem olduğundan, gereksinim duyduğunuz kadar çok oluşturabilirsiniz. Gerekli olan sürece hizmeti proxy'si örneği yeniden kullanılabilir. Uzaktan yordam çağrısı bir özel durum oluşturursa, hala aynı proxy örneği yeniden kullanabilirsiniz. Her ServiceProxy kablo üzerinden ileti göndermek için kullanılan bir iletişim istemcisi içerir. Uzak çağrılar istenirken iç denetimleri iletişimi istemci geçerli olup olmadığını belirlemek için gerçekleştirilir. Bu denetimlerini sonuçlarına bağlı olarak iletişim istemci gerektiğinde yeniden oluşturulur. Bir özel durum oluşursa, bu nedenle, yeniden oluşturmanız gerekmez `ServiceProxy`.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory yaşam süresi
[ServiceProxyFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) farklı remoting arabirimleri için proxy örnekleri oluşturan bir üreteci değil. API kullanırsanız `ServiceProxy.Create` proxy oluşturmak için daha sonra framework ServiceProxy tek oluşturur.
Geçersiz kılmak gerektiğinde el ile oluşturmak kullanışlıdır [IServiceRemotingClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) özellikleri.
Fabrika oluşturma pahalı bir işlemdir. ServiceProxyFactory iletişimi istemci, dahili bir önbelleğini korur.
Mümkün olduğunca uzun bir süredir ServiceProxyFactory önbelleğe en iyi uygulamadır.

## <a name="remoting-exception-handling"></a>Remoting özel durum işleme
Hizmeti API'si tarafından oluşturulan tüm uzak özel durumları AggregateException istemcisine geri gönderilir. RemoteExceptions DataContract Serileştirilebilir olmalıdır; değilse, API proxy oluşturur [ServiceException](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) ile seri hale getirme hatası.

ServiceProxy için oluşturulan hizmet bölümü için tüm yük devretme özel durumları işler. Yük devretme özel durumları (geçici olmayan özel durumları) varsa uç noktaları yeniden çözümler ve doğru uç nokta çağrısıyla yeniden dener. Yük devretme özel durumlar için yeniden deneme sayısı belirsiz.
Geçici özel durumlar meydana gelirse, proxy'ye çağrı yeniden dener.

Varsayılan yeniden deneme parametreleridir tarafından sağlanan [OperationRetrySettings](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings).

OperationRetrySettings ServiceProxyFactory oluşturucusuna geçirerek, bu değerleri yapılandırabilirsiniz.

## <a name="how-to-use-the-remoting-v2-stack"></a>Remoting V2 yığını kullanma

NuGet Remoting Paket sürümü itibariyle 2.8 Remoting V2 yığını kullanma seçeneğiniz vardır. Remoting V2 yığını daha fazla kullanıcı ve özel serileştirme ve daha eklenebilir API'nin gibi özellikleri sağlar.
Şablon kodu Remoting V1 yığını kullanmaya devam eder.
Remoting V2 V1 ile uyumlu değil (önceki Remoting yığını), aşağıdaki yönergeleri izleyin üzerinde [V1 ile V2'ye yükseltme](#how-to-upgrade-from-remoting-v1-to-remoting-v2) hizmet kullanılabilirliği etkileyen olmadan.

Aşağıdaki yaklaşımlardan V2 yığını etkinleştirmek kullanılabilir.

### <a name="using-an-assembly-attribute-to-use-the-v2-stack"></a>V2 yığını kullanmak için bir derleme özniteliğini kullanma

Bu adımları bir derleme özniteliği kullanılarak V2 yığını kullanmak için şablonu kodu değiştirin.

1. Uç nokta kaynağının değiştirme `"ServiceEndpoint"` için `"ServiceEndpointV2"` hizmet bildiriminde.

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

3. Remoting arabirimleriyle içeren derlemenin işaretlemek bir `FabricTransportServiceRemotingProvider` özniteliği.

  ```csharp
  [assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.V2Listener, RemotingClient = RemotingClient.V2Client)]
  ```

Kod değişiklik istemci projesinde gerekli değildir.
Yukarıda gösterilen derleme özniteliği kullanıldığından emin olmak için arabirimi derlemeyle istemci derleme.

### <a name="using-explicit-v2-classes-to-use-the-v2-stack"></a>V2 yığını kullanmak için açık V2 sınıflarını kullanma

Bir derleme özniteliği kullanılarak alternatif olarak, V2 yığın açık V2 sınıflarını kullanarak da etkinleştirilebilir.

Bu adımları açık V2 sınıflarını kullanarak V2 yığını kullanmak için şablonu kodu değiştirin.

1. Uç nokta kaynağının değiştirme `"ServiceEndpoint"` için `"ServiceEndpointV2"` hizmet bildiriminde.

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

## <a name="how-to-upgrade-from-remoting-v1-to-remoting-v2"></a>Remoting V1 ile Remoting V2'ye yükseltme yapma.
V1 ile V2'ye yükseltmek için 2 adımlı yükseltmeler gereklidir. Listelenen sırayla yürütülmek üzere aşağıdaki adımları.

1. V1 hizmet V2 hizmetine CompactListener özniteliğini kullanarak yükseltin.
Bu değişiklik hizmetinin V1 ve V2 dinleyicisi dinlediğini emin olur.

    bir) adlı bir uç nokta kaynağının hizmet bildiriminde "ServiceEndpointV2" ekleyin.
      ```xml
      <Resources>
        <Endpoints>
          <Endpoint Name="ServiceEndpointV2" />  
        </Endpoints>
      </Resources>
      ```

    b) genişletme yöntemi aşağıdaki Remoting dinleyicisi oluşturmak için kullanın.

    ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
    ```

    c) CompatListener ve V2 istemci kullanmak için uzaktan iletişim arabirimlerde derleme özniteliğini ekleyin.
    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.CompatListener, RemotingClient = RemotingClient.V2Client)]

      ```
2. V1 istemci V2 istemciye V2 istemci özniteliğini kullanarak yükseltin.
Bu adımı istemci V2 yığını kullandığından emin hale getirir.
Herhangi bir değişiklik istemci proje/hizmet gereklidir. Güncelleştirilmiş arabirimi derleme istemci projeler derleme yeterli olur.

3. Bu adım isteğe bağlıdır. V2Listener özniteliğini kullanın ve ardından V2 hizmet yükseltin.
Bu adım yalnızca V2 Dinleyicisi'ni dinleyen bir hizmet emin olur.

```csharp
[assembly: FabricTransportServiceRemotingProvider(RemotingListener = RemotingListener.V2Listener, RemotingClient = RemotingClient.V2Client)]
```

## <a name="how-to-use-custom-serialization-with-remoting-v2"></a>Özel serileştirme Remoting V2 ile kullanma
Aşağıdaki örnek, Json seri hale getirme Remoting V2 ile kullanır.
1. Özel serileştirme için uygulama sunmak amacıyla IServiceRemotingMessageSerializationProvider arabirimini uygular.
    Kod parçacığı aşağıda verilmiştir-nasıl uygulaması gibi görünüyor.

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

2.    Varsayılan seri hale getirme JsonSerializationProvider sağlayıcısıyla Remoting dinleyici için geçersiz kılar.

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

3.    Varsayılan seri hale getirme JsonSerializationProvider sağlayıcısıyla Remoting istemci sınıf üreticisi için geçersiz kılar.

```csharp
  var proxyFactory = new ServiceProxyFactory((c) =>
            {
                return new FabricTransportServiceRemotingClientFactory(
                    serializationProvider: new ServiceRemotingJsonSerializationProvider());
            });
  ```

## <a name="next-steps"></a>Sonraki adımlar
* [Web API OWIN güvenilir Hizmetleri'ndeki ile](service-fabric-reliable-services-communication-webapi.md)
* [Güvenilir hizmetler ile WCF iletişim](service-fabric-reliable-services-communication-wcf.md)
* [Güvenilir hizmetler için iletişimin güvenliğini sağlama](service-fabric-reliable-services-secure-communication.md)

