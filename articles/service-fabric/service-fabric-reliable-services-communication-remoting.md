---
title: C# kullanarak Service Fabric'te hizmet uzaktan iletişimini | Microsoft Docs
description: Service Fabric uzak istemciler ve hizmetler uzak yordam çağrısı kullanarak C# hizmetlerle iletişim sağlar.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: BharatNarasimman
ms.assetid: abfaf430-fea0-4974-afba-cfc9f9f2354b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 09/20/2017
ms.author: vturecek
ms.openlocfilehash: f9cd6e2fee738d2d42c790b4eb7b9a876a44b01d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60772984"
---
# <a name="service-remoting-in-c-with-reliable-services"></a>C# Reliable Services ile Service uzaktan iletişim

> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-communication-remoting.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Belirli bir iletişim protokolü veya bir web API'si, Windows Communication Foundation veya diğerleri gibi bir yığın bağlı olmayan hizmetler için Reliable Services framework uzak yordam çağrıları için hızlı ve kolay bir şekilde ayarlamak için uzaktan iletişim mekanizması sağlar Hizmetler. Bu makalede, C# ile yazılmış hizmetler için uzak yordam çağrılarını ayarlama anlatılmaktadır.

## <a name="set-up-remoting-on-a-service"></a>Bir hizmeti uzaktan iletişim ayarlamak

Uzaktan iletişim için bir hizmeti iki basit adımda ayarlayabilirsiniz:

1. Bir arabirim uygulamak hizmet oluşturun. Bu arabirim, hizmetinizde bir uzak yordam çağrısı için kullanılabilen yöntemleri tanımlar. Görev döndüren yöntemler olmalıdır zaman uyumsuz yöntemler. Arabirimi uygulamalıdır `Microsoft.ServiceFabric.Services.Remoting.IService` hizmet uzaktan iletişim arabirimi olduğunu göstermek için.
2. Uzaktan iletişim dinleyicisi hizmetinizde kullanın. Uzaktan iletişim dinleyicisi olan bir `ICommunicationListener` remoting özellikleri sağlayan uygulama. `Microsoft.ServiceFabric.Services.Remoting.Runtime` Ad alanı genişletme yöntemini içeren `CreateServiceRemotingListener` varsayılan uzaktan iletişim aktarım protokolünü kullanarak bir uzaktan iletişim dinleyicisi oluşturmak için kullanılan durum bilgisiz ve durum bilgisi olan hizmetler için.

>[!NOTE]
>`Remoting` Ad alanını adlı ayrı bir NuGet paketi kullanılabilir `Microsoft.ServiceFabric.Services.Remoting`.

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
> Bağımsız değişkenler ve hizmet arabirimi dönüş türlerinde herhangi bir basit, karmaşık veya özel türü olabilir, ancak .NET tarafından serileştirilecek çözebilmeleri gerekir [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).
>
>

## <a name="call-remote-service-methods"></a>Uzak Hizmet yöntemleri çağırma

Uzaktan iletişim yığını kullanarak bir hizmet yöntemleri çağırma yapılır bir yerel ara sunucu üzerinden hizmete kullanarak `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` sınıfı. `ServiceProxy` Yöntemi hizmeti uygulayan aynı arabirimi kullanarak yerel bir proxy oluşturur. Proxy ile yöntemleri arabirimi aşağıdaki uzaktan çağırabilirsiniz.

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

Uzaktan iletişimini framework istemciye hizmet tarafından oluşturulan özel durumları yayar. Sonuç olarak, `ServiceProxy`olan kullanıldığında, istemci hizmeti tarafından oluşturulan özel durumları işlenmesinden sorumludur.

## <a name="service-proxy-lifetime"></a>Hizmet ara sunucu ömrünü

Hizmeti proxy oluşturma hafif bir işlem olduğundan, ihtiyacınız kadar oluşturabilirsiniz. Gerekli olan sürece için hizmet proxy örneği yeniden kullanılabilir. Uzak yordam çağrısı, bir özel durum oluşturursa, yine de aynı proxy örneği yeniden kullanabilirsiniz. Her hizmet proxy'si kablo üzerinden ileti göndermek için kullanılan bir iletişim istemcisi içerir. Uzak çağrılar çağrılırken, dahili denetimler iletişim istemci geçerli olup olmadığını belirlemek için gerçekleştirilir. Bu denetimlerin sonuçlarına göre iletişim istemci gerekirse yeniden oluşturulur. Bir özel durum oluşursa, bu nedenle, yeniden oluşturmak ihtiyacınız olmayan `ServiceProxy`.

### <a name="service-proxy-factory-lifetime"></a>Hizmet ara sunucu üreteci ömrü

[ServiceProxyFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) olan farklı bir uzak arabirimler için Ara sunucu örnekleri oluşturan bir üreteci. API kullanırsanız `ServiceProxyFactory.CreateServiceProxy` bir ara sunucu oluşturmak için bir singleton hizmeti proxy'si framework oluşturur.
Geçersiz kılmak gerektiğinde el ile oluşturmak kullanışlıdır [IServiceRemotingClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v1.client.iserviceremotingclientfactory) özellikleri.
Fabrikası oluşturma pahalı bir işlemdir. Bir hizmeti proxy fabrikası bir iç iletişim istemci önbelleğini korur.
Önbellek hizmeti proxy Fabrika olabildiğince uzun bir süre için iyi bir uygulamadır.

## <a name="remoting-exception-handling"></a>Uzak özel durum işleme

Hizmet API'si tarafından oluşturulan tüm uzak özel durumlar AggregateException istemciye geri gönderilir. Uzak özel durum DataContract tarafından serileştirilecek başlatabilmeniz gerekir. Değilseniz, proxy API oluşturur [ServiceException](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) ile seri hale getirme hatası.

Hizmet proxy'si için oluşturulan hizmet bölümü için tüm yük devretme özel durumları işler. Yük devretme özel durumlar (geçici olmayan özel durumlar) varsa, uç noktaları yeniden giderir ve doğru uç noktası ile çağrı yeniden denenir. Yük devretme özel durumlar için yeniden deneme sayısını sonsuzdur.
Proxy geçici özel durumlar oluşursa, çağrı yeniden dener.

Varsayılan yeniden deneme parametreleri tarafından sağlanan [OperationRetrySettings](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings).

Bir kullanıcı bu değerleri OperationRetrySettings ServiceProxyFactory oluşturucusuna geçirerek yapılandırabilirsiniz.

## <a name="use-the-remoting-v2-stack"></a>Uzaktan iletişim V2 yığın kullanma

NuGet remoting paketin sürümü itibarıyla 2.8, uzaktan iletişim V2 yığın kullanılacak seçeneğiniz vardır. Uzaktan iletişim V2 yığın daha iyi gerçekleştirir. Özel seri hale getirme ve daha takılabilir API'leri gibi özellikler de sağlar.
Şablon kodunun, uzaktan iletişim V1 stack kullanmaya devam eder.
Uzaktan iletişim V2 V1 ile uyumlu değil (önceki uzaktan iletişim yığını). Makaledeki yönergeleri [V1'den V2'ye yükseltme](#upgrade-from-remoting-v1-to-remoting-v2) hizmet kullanılabilirliği etkileri önlemek için.

V2 yığın etkinleştirmek aşağıdaki yaklaşımlar kullanılabilir.

### <a name="use-an-assembly-attribute-to-use-the-v2-stack"></a>Bir derleme özniteliğini V2 yığını kullanın

Bu adımları bir derleme özniteliğini kullanarak V2 yığın kullanılacak şablon kodunu değiştirin.

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
İstemci bütünleştirilmiş kod daha önce gösterilen derleme özniteliğini kullanıldığından emin olmak için arabirimi derleme ile oluşturun.

### <a name="use-explicit-v2-classes-to-use-the-v2-stack"></a>V2 yığın açık V2 sınıfları kullanın

Bir derleme özniteliğini kullanarak alternatif olarak, V2 yığın açık V2 sınıflarını kullanarak da etkinleştirilebilir.

Bu adımları V2 yığın açık V2 sınıfları kullanarak şablon kodunu değiştirin.

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

3. Kullanım [FabricTransportServiceRemotingClientFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.client.fabrictransportserviceremotingclientfactory?view=azure-dotnet) gelen `Microsoft.ServiceFabric.Services.Remoting.V2.FabricTransport.Client` istemcileri oluşturmak için ad alanı.

   ```csharp
   var proxyFactory = new ServiceProxyFactory((c) =>
          {
              return new FabricTransportServiceRemotingClientFactory();
          });
   ```

## <a name="upgrade-from-remoting-v1-to-remoting-v2"></a>Uzaktan iletişim V2 Remoting V1 ' yükseltme

V1'den V2'ye yükseltmek için iki aşamalı yükseltme gerekli değildir. Bu sıradaki adımları izleyin.

1. V1 hizmetini, bu özniteliği kullanarak V2 hizmetine yükseltin.
Bu değişiklik, hizmetin V1 ve V2 Dinleyicide bekleyen emin olur.

    a. Hizmet bildiriminde "ServiceEndpointV2" adlı bir uç nokta kaynağı ekleyin.
      ```xml
      <Resources>
        <Endpoints>
          <Endpoint Name="ServiceEndpointV2" />  
        </Endpoints>
      </Resources>
      ```

    b. Uzak bir dinleyici oluşturmak üzere aşağıdaki genişletme yöntemini kullanın.

    ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
    ```

    c. V1 ve V2 dinleyici ve V2 istemci kullanmak için uzaktan iletişim arabirimleri üzerinde bir derleme özniteliğini ekleyin.
    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2|RemotingListenerVersion.V1, RemotingClientVersion = RemotingClientVersion.V2)]

      ```
2. V1 istemci V2 istemci özniteliğini kullanarak bir V2 istemciye yükseltin.
Bu adım, istemcinin V2 yığın kullandığından emin sağlar.
İstemci projesi/hizmet değişiklik gereklidir. Güncelleştirilmiş arabirimi derleme ile istemci projeler derleme yeterli olur.

3. Bu adım isteğe bağlıdır. V2 dinleyici özniteliğini kullanın ve ardından V2 hizmet yükseltin.
Bu adım yalnızca V2 Dinleyicide dinleyen bir hizmet emin olur.

    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2, RemotingClientVersion = RemotingClientVersion.V2)]
    ```


## <a name="use-the-remoting-v2-interface-compatible-stack"></a>Uzaktan iletişim V2 (Arabirimi uyumlu) yığını kullanın

 Uzaktan iletişim V2 (V2_1 bilinen uyumlu arabirimi) yığını V2 remoting yığınının tüm özelliklere sahiptir. Arabirimi yığınını remoting V1 yığın ile uyumludur, ancak V2 ve V1 ile geriye dönük olarak uyumlu değil. Yükseltme makaledeki adımları v1'den V2_1 için hizmet kullanılabilirliğini etkilemeden yükseltmek için V2'ye V1'den izleyin (uyumlu arabirimi).


### <a name="use-an-assembly-attribute-to-use-the-remoting-v2-interface-compatible-stack"></a>Bir derleme özniteliğini uzaktan iletişim V2 (Arabirimi uyumlu) yığını kullanın

Bir V2_1 yığınına değiştirmek için aşağıdaki adımları izleyin.

1. Hizmet bildiriminde "ServiceEndpointV2_1" adlı bir uç nokta kaynağı ekleyin.

   ```xml
   <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2_1" />  
    </Endpoints>
   </Resources>
   ```

2. Uzaktan iletişim dinleyicisi oluşturmak için uzaktan iletişim genişletme yöntemini kullanın.

   ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
   ```

3. Ekleme bir [derleme özniteliğini](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.fabrictransport.fabrictransportserviceremotingproviderattribute?view=azure-dotnet) remoting arabirimleri üzerinde.

   ```csharp
    [assembly:  FabricTransportServiceRemotingProvider(RemotingListenerVersion=  RemotingListenerVersion.V2_1, RemotingClientVersion= RemotingClientVersion.V2_1)]

   ```

Değişiklik istemci projede gerekli değildir.
Önceki derleme özniteliğini kullanıldığından emin emin olmak için arabirimi derleme ile istemci derleme.

### <a name="use-explicit-remoting-classes-to-create-a-listenerclient-factory-for-the-v2-interface-compatible-version"></a>V2 (Arabirimi uyumlu) sürümü için bir dinleyici/istemci fabrikası oluşturmak için açık uzaktan iletişim sınıfları kullanın

Şu adımları uygulayın:

1. Hizmet bildiriminde "ServiceEndpointV2_1" adlı bir uç nokta kaynağı ekleyin.

   ```xml
   <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpointV2_1" />  
    </Endpoints>
   </Resources>
   ```

2. Kullanım [uzaktan iletişim V2 dinleyici](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.services.remoting.v2.fabrictransport.runtime.fabrictransportserviceremotinglistener?view=azure-dotnet). Kullanılan varsayılan hizmet uç noktası kaynak adı olduğundan "ServiceEndpointV2_1." Hizmet bildiriminde tanımlanması gerekir.

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

## <a name="upgrade-from-remoting-v1-to-remoting-v2-interface-compatible"></a>Yükseltme remoting V1 uzaktan iletişim V2 (uyumlu arabirimi)

V1'den V2'ye yükseltmek için (V2_1 bilinen uyumlu arabirimi) iki aşamalı yükseltme gereklidir. Bu sıradaki adımları izleyin.

1. V1 hizmeti aşağıdaki özniteliği kullanılarak V2_1 hizmetine yükseltin.
Bu değişiklik hizmetinin V1 ve V2_1 dinleyici dinlediğini emin olur.

    a. Hizmet bildiriminde "ServiceEndpointV2_1" adlı bir uç nokta kaynağı ekleyin.
      ```xml
      <Resources>
        <Endpoints>
          <Endpoint Name="ServiceEndpointV2_1" />  
        </Endpoints>
      </Resources>
      ```

    b. Uzak bir dinleyici oluşturmak üzere aşağıdaki genişletme yöntemini kullanın.

    ```csharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return this.CreateServiceRemotingInstanceListeners();
    }
    ```

    c. Bir derleme özniteliğini remoting arabirimlerde kullanım V1, V2_1 dinleyici ve V2_1 istemci ekleyin.
    ```csharp
   [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1 | RemotingListenerVersion.V1, RemotingClientVersion = RemotingClientVersion.V2_1)]

      ```
2. V1 istemci V2_1 istemciye V2_1 istemci özniteliğini kullanarak yükseltin.
Bu adım, istemcinin V2_1 yığının kullandığı emin olur.
İstemci projesi/hizmet değişiklik gereklidir. Güncelleştirilmiş arabirimi derleme ile istemci projeler derleme yeterli olur.

3. Bu adım isteğe bağlıdır. V1 dinleyici sürüm özniteliği kaldırın ve ardından V2 hizmet yükseltin.
Bu adım yalnızca V2 Dinleyicide dinleyen bir hizmet emin olur.

    ```csharp
    [assembly: FabricTransportServiceRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1, RemotingClientVersion = RemotingClientVersion.V2_1)]
    ```
  
### <a name="use-custom-serialization-with-a-remoting-wrapped-message"></a>Uzaktan iletişimini Sarmalanan ileti ile özel serileştirme kullanın

Uzaktan iletişimini Sarmalanan ileti için tek bir Sarmalanan nesne tüm parametreleri içeren bir alanda olarak oluştururuz.
Şu adımları uygulayın:

1. Uygulama `IServiceRemotingMessageSerializationProvider` özel serileştirme için bir uygulama sağlamak için arabirim.
    Bu kod parçacığı uygulamasını nasıl göründüğünü gösterir.

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

2. Geçersiz Kıl ve varsayılan seri hale getirme sağlayıcı ile `JsonSerializationProvider` uzaktan iletişim dinleyicisi için.

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

3. Geçersiz Kıl ve varsayılan seri hale getirme sağlayıcı ile `JsonSerializationProvider` için bir uzak istemci sınıf üreticisi.

    ```csharp
    var proxyFactory = new ServiceProxyFactory((c) =>
    {
        return new FabricTransportServiceRemotingClientFactory(
        serializationProvider: new ServiceRemotingJsonSerializationProvider());
      });
      ```

## <a name="next-steps"></a>Sonraki adımlar

* [Reliable Services özelliğinde OWIN ile Web API'si](service-fabric-reliable-services-communication-webapi.md)
* [Reliable Services ile Windows Communication Foundation iletişimi](service-fabric-reliable-services-communication-wcf.md)
* [Reliable Services için güvenli iletişim](service-fabric-reliable-services-secure-communication.md)
