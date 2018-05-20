---
title: Güvenilir hizmetler WCF iletişim yığını | Microsoft Docs
description: Service Fabric yerleşik WCF iletişimi yığınında güvenilir hizmetler için İstemci-hizmet WCF iletişimi sağlar.
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 75516e1e-ee57-4bc7-95fe-71ec42d452b2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/07/2017
ms.author: bharatn
ms.openlocfilehash: f5ca579b446e5d3608d53cea73fa9392cd00db06
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="wcf-based-communication-stack-for-reliable-services"></a>Güvenilir hizmetler için WCF tabanlı iletişim yığını
Güvenilir hizmetler altyapısına hizmet yazarların kendi hizmet için kullanmak istedikleri iletişim yığını seçin izin verir. Kendi seçtikleri iletişim yığınındaki ekleyebilirsiniz **ICommunicationListener** döndürülen [CreateServiceReplicaListeners veya CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) yöntemleri. Çerçeve WCF tabanlı iletişim kullanmak istediğiniz hizmet yazarlar için Windows Communication Foundation (WCF) tabanlı iletişim yığını uygulaması sağlar.

## <a name="wcf-communication-listener"></a>WCF iletişimi dinleyicisi
WCF özel uyarlamasını **ICommunicationListener** tarafından sağlanan **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** sınıfı.

Bir hizmet sözleşmesini türü deyin sahibiz ekleyin `ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Aşağıdaki şekilde hizmetinde bir WCF iletişimi dinleyici oluşturabiliriz.

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
WCF kullanarak Hizmetleri ile iletişim kurmak için istemcileri yazmak için bir çerçeve sağlar **WcfClientCommunicationFactory**, WCF özgü uyarlamasını olduğu [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

WCF iletişim kanalını erişilebilen **WcfCommunicationClient** tarafından oluşturulan **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

İstemci kodu kullanabileceğiniz **WcfCommunicationClientFactory** ile birlikte **WcfCommunicationClient** hangi uygulayan **ServicePartitionClient** hizmet uç noktası belirlemek ve hizmeti ile iletişim için.

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
> ServicePartitionResolver varsayılan istemci hizmeti olarak aynı kümedeki çalıştığını varsayar. Diğer bir deyişle Aksi halde, bir ServicePartitionResolver nesnesi oluşturur ve küme bağlantı uç noktalardan geçirin.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir hizmetler remoting ile uzaktan yordam çağrısı](service-fabric-reliable-services-communication-remoting.md)
* [Web API OWIN güvenilir Hizmetleri'ndeki ile](service-fabric-reliable-services-communication-webapi.md)
* [Güvenilir hizmetler için iletişimin güvenliğini sağlama](service-fabric-reliable-services-secure-communication.md)

