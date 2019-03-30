---
title: Java kullanarak Azure Service Fabric'te hizmet uzaktan iletişimini | Microsoft Docs
description: Service Fabric uzak istemciler ve hizmetler uzak yordam çağrısı kullanarak Java hizmetlerle iletişim sağlar.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: chackdan
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 51c8c689bd3fe3e8967bab77e776ad02f9cb59f1
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58667975"
---
# <a name="service-remoting-in-java-with-reliable-services"></a>Reliable Services ile Java'da Service uzaktan iletişim
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-communication-remoting.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-communication-remoting-java.md)
>
>

Belirli bir iletişim protokolü veya Webapı, Windows Communication Foundation (WCF) veya diğerleri gibi bir yığın bağlı olmayan hizmetler için Reliable Services framework uzak yordam çağrıları için hızlı ve kolay bir şekilde ayarlamak için uzaktan iletişim mekanizması sağlar Hizmetler.  Bu makalede nasıl Java ile yazılmış hizmetler için uzak yordam çağrılarını ayarlanacağı açıklanmaktadır.

## <a name="set-up-remoting-on-a-service"></a>Bir hizmeti uzaktan iletişim ayarlamak
Bir hizmet için uzaktan iletişim kurma, iki basit adımda gerçekleştirilir:

1. Bir arabirim uygulamak hizmet oluşturun. Bu arabirim, hizmetinizde bir uzak yordam çağrısı için kullanılabilen yöntemleri tanımlar. Görev döndüren yöntemler olmalıdır zaman uyumsuz yöntemler. Arabirimi uygulamalıdır `microsoft.serviceFabric.services.remoting.Service` hizmet uzaktan iletişim arabirimi olduğunu göstermek için.
2. Uzaktan iletişim dinleyicisi hizmetinizde kullanın. Bu bir `CommunicationListener` remoting özellikleri sağlayan uygulama. `FabricTransportServiceRemotingListener` Varsayılan uzaktan iletişim aktarım protokolünü kullanarak uzak bir dinleyici oluşturmak üzere kullanılabilir.

Örneğin, aşağıdaki durum bilgisi olmayan hizmet uzaktan prosedür çağrılarında "Hello World" almak için tek bir yöntemi gösterir.

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
> Bağımsız değişkenler ve hizmet arabirimi dönüş türlerinde herhangi bir basit, karmaşık veya özel türü olabilir, ancak seri hale getirilebilir olması gerekir.
>
>

## <a name="call-remote-service-methods"></a>Uzak Hizmet yöntemleri çağırma
Uzaktan iletişim yığını kullanarak bir hizmet yöntemleri çağırma yapılır bir yerel ara sunucu üzerinden hizmete kullanarak `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` sınıfı. `ServiceProxyBase` Yöntemi hizmeti uygulayan aynı arabirimi kullanarak yerel bir proxy oluşturur. Proxy ile yalnızca yöntemleri arabirimde uzaktan çağırabilirsiniz.

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

Uzaktan iletişimini framework hizmetin istemciye oluşturulan özel durumları yayar. Bu nedenle özel durum işleme mantığı kullanılarak istemcide `ServiceProxyBase` doğrudan hizmet oluşturduğu özel durumları işleyebilir.

## <a name="service-proxy-lifetime"></a>Hizmet ara sunucu ömrünü
ServiceProxy oluşturma hafif bir işlem olduğundan, ihtiyacınız kadar oluşturabilirsiniz. Gerekli olan sürece hizmet proxy'si örneği yeniden kullanılabilir. Uzak yordam çağrısı, bir özel durum oluşturursa, yine de aynı proxy örneği yeniden kullanabilirsiniz. Her ServiceProxy kablo üzerinden ileti göndermek için kullanılan bir iletişim istemcisi içerir. Uzak çağrılar çağrılırken, dahili denetimler iletişim istemci geçerli olup olmadığını belirlemek için gerçekleştirilir. Bu denetimlerin sonuçlarına göre iletişim istemci gerekirse yeniden oluşturulur. Bir özel durum oluşursa, bu nedenle, yeniden oluşturmak ihtiyacınız olmayan `ServiceProxy`.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory ömrü
[FabricServiceProxyFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client.fabricserviceproxyfactory) olan farklı bir uzak arabirimler için proxy oluşturan bir üreteci. API kullanırsanız `ServiceProxyBase.create` proxy oluşturmak, sonra framework oluşturur bir `FabricServiceProxyFactory`.
Geçersiz kılmak gerektiğinde el ile oluşturmak kullanışlıdır [ServiceRemotingClientFactory](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.remoting.client.serviceremotingclientfactory) özellikleri.
Fabrika pahalı bir işlemdir. `FabricServiceProxyFactory` Önbellek iletişim istemcilerin tutar.
En iyi uygulamadır önbelleğine `FabricServiceProxyFactory` olabildiğince uzun bir süre için.

## <a name="remoting-exception-handling"></a>Uzak özel durum işleme
Hizmet API'si tarafından oluşturulan tüm uzak özel durumu gönderildiğinde istemciye RuntimeException veya FabricException olarak.

ServiceProxy, için oluşturulan hizmet bölümü için tüm yük devretme özel durumu işle. Yük devretme Exceptions(Non-Transient Exceptions) ise uç noktaları yeniden çözer ve doğru uç noktası ile çağrı yeniden denenir. Yeniden deneme özel durum yük devretme için belirsiz.
TransientExceptions durumunda çağrı yalnızca deneme.

Varsayılan yeniden deneme parametreleridir tarafından bulunduğunda [OperationRetrySettings](https://docs.microsoft.com/java/api/microsoft.servicefabric.services.communication.client.operationretrysettings).
OperationRetrySettings ServiceProxyFactory oluşturucusuna geçirerek bu değerleri yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Services için iletişimin güvenliğini sağlama](service-fabric-reliable-services-secure-communication-java.md)
