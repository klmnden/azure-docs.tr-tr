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
ms.openlocfilehash: e7652fe1b211e6811a4a3aa61bc2aa9d2f529dda
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205825"
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
     return this.CreateServiceRemotingInstanceListeners();
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
Geçersiz kılmak gerektiğinde el ile oluşturmak kullanışlıdır [IServiceRemotingClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v1.client.iserviceremotingclientfactory) özellikleri.
Fabrikası oluşturma pahalı bir işlemdir. Bir iç iletişim istemci önbelleğini ServiceProxyFactory tutar.
ServiceProxyFactory olabildiğince uzun bir süre için önbelleğe en iyi uygulamadır.

## <a name="remoting-exception-handling"></a>Uzak özel durum işleme
Hizmet API'si tarafından oluşturulan tüm uzak özel durumlar AggregateException istemciye geri gönderilir. RemoteExceptions DataContract Serileştirilebilir olmalıdır; değilseniz, proxy API oluşturur [ServiceException](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) ile seri hale getirme hatası.

ServiceProxy için oluşturulan hizmet bölümü için tüm yük devretme özel durumları işler. Yük devretme özel durumlar (geçici olmayan özel durumlar) varsa, uç noktaları yeniden giderir ve doğru uç noktası ile çağrı yeniden denenir. Yük devretme özel durumlar için yeniden deneme sayısını belirsiz.
Proxy geçici özel durumlar oluşursa, çağrı yeniden dener.

Varsayılan yeniden deneme parametreleri tarafından sağlanan [OperationRetrySettings](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings).

Kullanıcı, OperationRetrySettings ServiceProxyFactory oluşturucusuna geçirerek bu değerleri yapılandırabilirsiniz.

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
  [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2, RemotingClientVersion = RemotingClientVersion.V2)]
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

1. V1 hizmeti özniteliğini kullanarak V2 hizmetine yükseltin.
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

    c) uzaktan iletişimi V1 ve V2 dinleyici ve V2 istemci arabirimleri üzerinde derleme özniteliğini ekleyin.
    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2|RemotingListenerVersion.V1, RemotingClientVersion = RemotingClientVersion.V2)]

      ```
2. V1 istemci V2 istemciye V2 istemci özniteliğini kullanarak yükseltin.
Bu adım, istemci V2 yığın kullandığından emin sağlar.
İstemci projesi/hizmet değişiklik gerekli değil. Güncelleştirilmiş arabirimi derleme ile istemci projeler derleme yeterli olur.

3. Bu adım isteğe bağlıdır. V2Listener özniteliğini kullanın ve ardından V2 hizmet yükseltin.
Bu adım yalnızca V2 Dinleyicide dinleyen bir hizmet emin olur.

```csharp
[assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2, RemotingClientVersion = RemotingClientVersion.V2)]
```


## <a name="how-to-use-remoting-v2interfacecompatible-stack"></a>Uzaktan iletişimini V2(InterfaceCompatible) yığın kullanma
 Uzaktan iletişim V2 (InterfaceCompatible V2_1 olarak da bilinir) yığını V2 Remoting özelliklerinin yığın yanında, uzaktan iletişim V1 yığınına Arabirimi uyumlu yığınıdır ancak V2 ve V1 ile geriye dönük uyumlu olmayan tüm yoktur. Yükseltme V1'den V2_1 için hizmet kullanılabilirliğini etkilemeden yapmak için makalede izleyin[V2(InterfaceCompatible) için V1'den yükseltme](#how-to-upgrade-from-remoting-v1-to-remoting-v2interfacecompatible).


### <a name="using-assembly-attribute-to-use-remoting-v2interfacecompatible-stack"></a>Uzaktan iletişimini V2(InterfaceCompatible) yığın kullanılacak derleme özniteliğini kullanarak.

V2_1 yığınına değiştirmek için uygulayacağınız adımlar aşağıda verilmiştir.

1. Adlı bir uç nokta kaynağı "ServiceEndpointV2_1" hizmet bildiriminde ekleyin.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2_1" />  
    </Endpoints>
  </Resources>
  ```

2.  Uzaktan iletişim dinleyicisi oluşturmak için uzaktan iletişim genişletme yöntemi kullanın.

  ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
  ```

3.  Ekleme [derleme özniteliğini](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.fabrictransport.fabrictransportserviceremotingproviderattribute?view=azure-dotnet) Remoting arabirimleri üzerinde.

  ```csharp
    [assembly:  FabricTransportServiceRemotingProvider(RemotingListenerVersion=  RemotingListenerVersion.V2_1, RemotingClientVersion= RemotingClientVersion.V2_1)]

  ```
Değişiklik istemci projede gerekli değildir.
İstemci arabirimi derlemesi ile derleme, derleme özniteliği kullanıldığından emin emin yapar.

### <a name="using-explicit-remoting-classes-to-create-listener-clientfactory-for-v2interfacecompatible-version"></a>Dinleyici oluşturmak için açık uzaktan iletişim sınıfları kullanma / ClientFactory V2(InterfaceCompatible) sürümü için.
İzlenmesi gereken adımlar aşağıda verilmiştir.
1.  Adlı bir uç nokta kaynağı "ServiceEndpointV2_1" hizmet bildiriminde ekleyin.

  ```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2_1" />  
    </Endpoints>
  </Resources>
  ```

2. Kullanım [Remoting V2Listener](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.runtime.fabrictransportserviceremotinglistener?view=azure-dotnet). Kullanılan varsayılan hizmet uç noktası kaynak adı "ServiceEndpointV2_1" olduğundan hizmet bildirimi içinde tanımlanmış olması gerekir.

  ```csharp
  protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[]
        {
            new ServiceInstanceListener((c) =>
            {
                var settings = new FabricTransportRemotingListenerSettings();
                settings.UseWrappedMessage = true;
                return new FabricTransportServiceRemotingListener(c, this,settings);

            })
        };
    }
  ```

3. V2 kullanması [istemci sınıf üreticisi](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.client.fabrictransportserviceremotingclientfactory?view=azure-dotnet).
  ```csharp
  var proxyFactory = new ServiceProxyFactory((c) =>
          {
            var settings = new FabricTransportRemotingSettings();
            settings.UseWrappedMessage = true;
            return new FabricTransportServiceRemotingClientFactory(settings);
          });
  ```

## <a name="how-to-upgrade-from-remoting-v1-to-remoting-v2interfacecompatible"></a>Uzaktan iletişimini V2(InterfaceCompatible) için uzaktan iletişim V1'den yükseltme yapma.
V1'den V2'ye yükseltmek için (InterfaceCompatible V2_1 olarak da bilinir), 2 adımlı yükseltme gereklidir. Listelenen sırayla yürütülmesi için aşağıdaki adımları içerir.

1. V1 hizmet V2_1 hizmetine özniteliği aşağıdaki kullanarak yükseltin.
Bu değişiklik, hizmet V1 ve V2_1 dinleyicisinin dinleme emin olur.

    (a) adlı bir uç nokta kaynağı "ServiceEndpointV2_1" hizmet bildiriminde ekleyin.
      ```xml
      <Resources>
        <Endpoints>
          <Endpoint Name="ServiceEndpointV2_1" />  
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

    c) V1, V2_1 dinleyici ve V2_1 istemci kullanmak için uzaktan iletişim arabirimlerde derleme özniteliğini ekleyin.
    ```csharp
   [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1 | RemotingListenerVersion.V1, RemotingClientVersion = RemotingClientVersion.V2_1)]

      ```
2. V1 istemci V2_1 istemciye V2_1 istemci özniteliğini kullanarak yükseltin.
Bu adım, istemci V2_1 yığın kullandığından emin sağlar.
İstemci projesi/hizmet değişiklik gerekli değil. Güncelleştirilmiş arabirimi derleme ile istemci projeler derleme yeterli olur.

3. Bu adım isteğe bağlıdır. V1 dinleyici sürüm özniteliği kaldırın ve sonra V2 hizmet yükseltin.
Bu adım yalnızca V2 Dinleyicide dinleyen bir hizmet emin olur.

```csharp
[assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1, RemotingClientVersion = RemotingClientVersion.V2_1)]
```

### <a name="how-to-use-custom-serialization-with-remoting-wrapped-message"></a>Uzaktan iletişimini sarmalanmış ileti ile özel serileştirme kullanma
Sarmalanan uzaktan iletişim iletisinde tek Sarmalanan nesne tüm parametreleri içeren bir alanda olarak oluştururuz.
Adımlar şunlardır:

1. Özel serileştirme için bir uygulama sağlamak için IServiceRemotingMessageSerializationProvider arabirimini uygular.
    Nasıl uygulama gibi göründüğünü kod parçacığı aşağıdadır.

      ```csharp
      public class ServiceRemotingJsonSerializationProvider : IServiceRemotingMessageSerializationProvider
      {
        public IServiceRemotingMessageBodyFactory CreateMessageBodyFactory()
        {
          return new JsonMessageFactory();
        }

        public IServiceRemotingRequestMessageBodySerializer CreateRequestMessageSerializer(Type serviceInterfaceType, IEnumerable<Type> requestWrappedType, IEnumerable<Type> requestBodyTypes = null)
        {
          return new ServiceRemotingRequestJsonMessageBodySerializer();
        }

        public IServiceRemotingResponseMessageBodySerializer CreateResponseMessageSerializer(Type serviceInterfaceType, IEnumerable<Type> responseWrappedType, IEnumerable<Type> responseBodyTypes = null)
        {
          return new ServiceRemotingResponseJsonMessageBodySerializer();
        }
      }
      ```
      ```csharp
        class JsonMessageFactory : IServiceRemotingMessageBodyFactory
            {

              public IServiceRemotingRequestMessageBody CreateRequest(string interfaceName, string methodName, int numberOfParameters, object wrappedRequestObject)
              {
                return new JsonBody(wrappedRequestObject);
              }

              public IServiceRemotingResponseMessageBody CreateResponse(string interfaceName, string methodName, object wrappedRequestObject)
              {
                return new JsonBody(wrappedRequestObject);
              }
            }
      ```
      ```csharp
      class ServiceRemotingRequestJsonMessageBodySerializer : IServiceRemotingRequestMessageBodySerializer
        {
            private JsonSerializer serializer;

            public ServiceRemotingRequestJsonMessageBodySerializer()
            {
              serializer = JsonSerializer.Create(new JsonSerializerSettings()
              {
                TypeNameHandling = TypeNameHandling.All
                });
              }

              public IOutgoingMessageBody Serialize(IServiceRemotingRequestMessageBody serviceRemotingRequestMessageBody)
             {
               if (serviceRemotingRequestMessageBody == null)
               {
                 return null;
               }          
               using (var writeStream = new MemoryStream())
               {
                 using (var jsonWriter = new JsonTextWriter(new StreamWriter(writeStream)))
                 {
                   serializer.Serialize(jsonWriter, serviceRemotingRequestMessageBody);
                   jsonWriter.Flush();
                   var bytes = writeStream.ToArray();
                   var segment = new ArraySegment<byte>(bytes);
                   var segments = new List<ArraySegment<byte>> { segment };
                   return new OutgoingMessageBody(segments);
                 }
               }
              }

              public IServiceRemotingRequestMessageBody Deserialize(IIncomingMessageBody messageBody)
             {
               using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))
               {
                 using (JsonReader reader = new JsonTextReader(sr))
                 {
                   var ob = serializer.Deserialize<JsonBody>(reader);
                   return ob;
                 }
               }
             }
            }
      ```
      ```csharp
      class ServiceRemotingResponseJsonMessageBodySerializer : IServiceRemotingResponseMessageBodySerializer
       {
         private JsonSerializer serializer;

        public ServiceRemotingResponseJsonMessageBodySerializer()
        {
          serializer = JsonSerializer.Create(new JsonSerializerSettings()
          {
              TypeNameHandling = TypeNameHandling.All
            });
          }

          public IOutgoingMessageBody Serialize(IServiceRemotingResponseMessageBody responseMessageBody)
          {
            if (responseMessageBody == null)
            {
              return null;
            }

            using (var writeStream = new MemoryStream())
            {
              using (var jsonWriter = new JsonTextWriter(new StreamWriter(writeStream)))
              {
                serializer.Serialize(jsonWriter, responseMessageBody);
                jsonWriter.Flush();
                var bytes = writeStream.ToArray();
                var segment = new ArraySegment<byte>(bytes);
                var segments = new List<ArraySegment<byte>> { segment };
                return new OutgoingMessageBody(segments);
              }
            }
          }

          public IServiceRemotingResponseMessageBody Deserialize(IIncomingMessageBody messageBody)
          {

             using (var sr = new StreamReader(messageBody.GetReceivedBuffer()))
             {
               using (var reader = new JsonTextReader(sr))
               {
                 var obj = serializer.Deserialize<JsonBody>(reader);
                 return obj;
               }
             }
           }
       }
    ```
    ```csharp
    class JsonBody : WrappedMessage, IServiceRemotingRequestMessageBody, IServiceRemotingResponseMessageBody
    {
          public JsonBody(object wrapped)
          {
            this.Value = wrapped;
          }

          public void SetParameter(int position, string parameName, object parameter)
          {  //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }

          public object GetParameter(int position, string parameName, Type paramType)
          {
            //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }

          public void Set(object response)
          { //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
          }

          public object Get(Type paramType)
          {  //Not Needed if you are using WrappedMessage
            throw new NotImplementedException();
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
