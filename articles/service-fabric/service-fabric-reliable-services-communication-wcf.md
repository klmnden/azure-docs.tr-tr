---
title: Reliable Services WCF iletişim yığını | Microsoft Docs
description: Service fabric'te yerleşik WCF iletişim yığını istemci hizmeti için Reliable Services WCF iletişim sağlar.
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: chackdan
editor: vturecek
ms.assetid: 75516e1e-ee57-4bc7-95fe-71ec42d452b2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/07/2017
ms.author: bharatn
ms.openlocfilehash: ae8a0ab0382083ebfca0834d2238403668efa71d
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670585"
---
# <a name="wcf-based-communication-stack-for-reliable-services"></a>Reliable Services WCF tabanlı iletişim yığını
Reliable Services framework servisine kullanmak istedikleri iletişim yığını seçmek hizmet yazarlar sağlar. İletişim yığını kendi seçtikleri takabilirsiniz **ICommunicationListener** döndürüldüğü [CreateServiceReplicaListeners veya Createserviceınstancelisteners](service-fabric-reliable-services-communication.md) yöntemleri. Framework'te iletişim WCF tabanlı kullanmak istediğiniz hizmet yazarları için Windows Communication Foundation (WCF) tabanlı iletişim yığını bir uygulamasını sağlar.

## <a name="wcf-communication-listener"></a>WCF iletişim dinleyicisi
WCF özel uygulanışı **ICommunicationListener** tarafından sağlanan **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** sınıfı.

Türünde bir hizmet anlaşmasını sunuyoruz deyin ekleyin `ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Aşağıdaki şekilde hizmetinde bir WCF iletişim dinleyicisini oluşturabiliriz.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-the-wcf-communication-stack"></a>İstemciler için WCF iletişim yığını yazma
WCF kullanarak hizmetlerle iletişim kurmak için istemcilerin yazmak için framework sağlar **WcfClientCommunicationFactory**, WCF özel uygulanışı olduğu [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

WCF iletişim kanalı erişilebilir **WcfCommunicationClient** tarafından oluşturulan **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

İstemci kodu kullanabileceğiniz **WcfCommunicationClientFactory** ile birlikte **WcfCommunicationClient** uygulayan **ServicePartitionClient** belirlemek için Hizmet uç noktası ve hizmetiyle iletişim kurar.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
> [!NOTE]
> ' % S'varsayılan ServicePartitionResolver istemci hizmetiyle aynı kümede çalıştığını varsayar. Diğer bir deyişle Aksi halde, bir ServicePartitionResolver nesnesi oluşturur ve küme bağlantısı uç noktaların geçirin.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Services uzaktan iletişimi ile uzak yordam çağrısı](service-fabric-reliable-services-communication-remoting.md)
* [Reliable Services özelliğinde OWIN ile Web API'si](service-fabric-reliable-services-communication-webapi.md)
* [Reliable Services için iletişimin güvenliğini sağlama](service-fabric-reliable-services-secure-communication-wcf.md)

