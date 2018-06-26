---
title: Java kullanarak Azure Service Fabric hizmeti remoting | Microsoft Docs
description: Service Fabric uzak istemciler ve hizmetler uzaktan yordam çağrısı kullanarak Java Hizmetleri ile iletişim kurmasına olanak sağlar.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 3215ee4adf907524626b4919b637ce23b9e0e782
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750189"
---
# <a name="service-remoting-in-java-with-reliable-services"></a>Java Reliable Services ile hizmet uzaktan iletişim
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-communication-remoting.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Belirli bir iletişim protokolü veya Webapı, Windows Communication Foundation (WCF) veya diğerleri gibi yığın bağlı olmayan hizmetler için uzak yordam çağrıları için hızlı ve kolay bir şekilde ayarlamak için uzaktan iletişim mekanizması Reliable Services çerçevesi sağlar Hizmetler.  Bu makalede nasıl Java ile yazılmış hizmetler için uzaktan yordam çağrılarını ayarlanacağı açıklanmaktadır.

## <a name="set-up-remoting-on-a-service"></a>Bir hizmeti uzaktan iletişim ayarlama
Bir hizmet için uzaktan iletişim kurma iki basit adımda gerçekleştirilir:

1. Hizmetinizin uygulamak bir arabirim oluşturun. Bu arabirim, hizmeti uzaktan yordam çağrısı için kullanılabilen yöntemleri tanımlar. Görev döndüren yöntemler olmalıdır zaman uyumsuz yöntemleri. Arabirimi uygulamalıdır `microsoft.serviceFabric.services.remoting.Service` hizmet remoting arabirimi olduğunu göstermek için.
2. Remoting dinleyici hizmetinizi kullanın. Bu bir `CommunicationListener` remoting özellikleri sağlayan uygulama. `FabricTransportServiceRemotingListener` Varsayılan remoting aktarım protokolünü kullanarak bir uzak dinleyicisi oluşturmak için kullanılabilir.

Örneğin, aşağıdaki durum bilgisiz hizmeti üzerinden bir uzak yordam çağrısı "Hello World" almak için tek bir yöntemi gösterir.

```java
import java.util.ArrayList;
import java.util.concurrent.CompletableFuture;
import java.util.List;
import microsoft.servicefabric.services.communication.runtime.ServiceInstanceListener;
import microsoft.servicefabric.services.remoting.Service;
import microsoft.servicefabric.services.runtime.StatelessService;

public interface MyService extends Service {
    CompletableFuture<String> helloWorldAsync();
}

class MyServiceImpl extends StatelessService implements MyService {
    public MyServiceImpl(StatelessServiceContext context) {
       super(context);
    }

    public CompletableFuture<String> helloWorldAsync() {
        return CompletableFuture.completedFuture("Hello!");
    }

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
        listeners.add(new ServiceInstanceListener((context) -> {
            return new FabricTransportServiceRemotingListener(context,this);
        }));
        return listeners;
    }
}
```

> [!NOTE]
> Bağımsız değişkenler ve hizmet arayüzündeki dönüş türleri basit, karmaşık veya özel türleri olabilir, ancak bunlar Serileştirilebilir olmalıdır.
>
>

## <a name="call-remote-service-methods"></a>Uzak Hizmet yöntemleri çağırma
Uzaktan iletişim yığını kullanarak bir hizmet yöntemleri çağırma yapılır yerel bir proxy üzerinden hizmete kullanarak `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` sınıfı. `ServiceProxyBase` Yöntemi hizmeti uygulayan aynı arabirimini kullanarak yerel bir proxy oluşturur. Proxy ile yalnızca yöntemleri arabirimde uzaktan çağırabilirsiniz.

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

Remoting framework istemciye hizmet sırasında oluşturulan özel durumları yayar. Bu nedenle özel durum işleme mantığı kullanarak istemcide `ServiceProxyBase` doğrudan hizmet oluşturur özel durumlar işleyebilir.

## <a name="service-proxy-lifetime"></a>Hizmet Proxy ömrü
ServiceProxy oluşturma hafif bir işlem olduğundan, gereksinim duyduğunuz kadar çok oluşturabilirsiniz. Gerekli olan sürece hizmeti proxy'si örneği yeniden kullanılabilir. Uzaktan yordam çağrısı bir özel durum oluşturursa, hala aynı proxy örneği yeniden kullanabilirsiniz. Her ServiceProxy kablo üzerinden ileti göndermek için kullanılan bir iletişim istemcisi içerir. Uzak çağrılar istenirken iç denetimleri iletişimi istemci geçerli olup olmadığını belirlemek için gerçekleştirilir. Bu denetimlerini sonuçlarına bağlı olarak iletişim istemci gerektiğinde yeniden oluşturulur. Bir özel durum oluşursa, bu nedenle, yeniden oluşturmanız gerekmez `ServiceProxy`.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory yaşam süresi
[FabricServiceProxyFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) farklı remoting arabirimleri için proxy oluşturan bir üreteci değil. API kullanırsanız `ServiceProxyBase.create` proxy oluşturmak, ardından framework oluşturan bir `FabricServiceProxyFactory`.
Geçersiz kılmak gerektiğinde el ile oluşturmak kullanışlıdır [ServiceRemotingClientFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) özellikleri.
Fabrika pahalı bir işlemdir. `FabricServiceProxyFactory` iletişim istemci önbelleğini korur.
En iyi uygulamadır önbelleğine `FabricServiceProxyFactory` mümkün olduğunca uzun bir süredir.

## <a name="remoting-exception-handling"></a>Remoting özel durum işleme
Hizmeti API'si tarafından oluşturulan tüm uzak özel durum gönderilir istemciye RuntimeException veya FabricException olarak.

ServiceProxy için oluşturulan hizmet bölümü için tüm yük devretme özel durumu işler. Yük devretme Exceptions(Non-Transient Exceptions) ise uç noktaları yeniden çözümlediği ve doğru uç nokta çağrısıyla yeniden dener. Yük devretme özel durumu için yeniden deneme sayısı belirsiz.
TransientExceptions durumunda çağrı yalnızca deneme.

[OperationRetrySettings] tarafından sağlanan varsayılan yeniden deneme parametreleridir. (https://docs.microsoft.com/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) OperationRetrySettings ServiceProxyFactory oluşturucusuna geçirerek bu değerleri yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir hizmetler için iletişimin güvenliğini sağlama](service-fabric-reliable-services-secure-communication-java.md)
