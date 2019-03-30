---
title: Bağlanmak ve Azure Service fabric'te hizmetlerle iletişim kuran | Microsoft Docs
description: Service fabric'te hizmetlerle iletişim kuran çözmek ve bağlanma hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: msfussell
ms.assetid: 7d1052ec-2c9f-443d-8b99-b75c97266e6c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: 55a0a1a8097ea46c7a3407b5f42824973edcf1a2
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666129"
---
# <a name="connect-and-communicate-with-services-in-service-fabric"></a>Service fabric'te hizmetlerle iletişim kuran ve bağlanın
Service Fabric'te hizmet genellikle birden çok VM arasında dağıtılmış bir Service Fabric kümesindeki herhangi bir yerde çalışır. Bunu tek bir yerden diğerine hizmet sahibi tarafından ya da otomatik olarak Service Fabric taşınabilir. Hizmetleri statik olarak belirli bir makine ya da adres bağlı olmak zorunda değildir.

Service Fabric uygulaması, burada her hizmeti özel bir görev gerçekleştirir birçok farklı Hizmetleri, genellikle oluşur. Bu hizmetler, diğer bir web uygulamanın farklı kısımlarını işleme gibi tam bir işlev oluşturmak için iletişim kurabilir. Bağlanmak ve hizmetlerle iletişim kurma istemci uygulamaları vardır. Bu belge ile ve hizmetlerinizi Service fabric'te arasında iletişim kurmak nasıl ele alınmaktadır.

## <a name="bring-your-own-protocol"></a>Kendi Protokolü Getir
Service Fabric, hizmetlerinizi yaşam döngüsünü yönetmenize yardımcı olur, ancak hizmetlerinizin neler hakkında kararlar yapmaz. Bu iletişim içerir. Service Fabric tarafından hizmetinizi açıldığında, hizmetinizin istediğiniz ne olursa olsun protokolü veya iletişim yığını kullanılarak, gelen istekler için bir uç nokta ayarlama fırsatı olmasıdır. Hizmetinizi normal üzerinde dinleyecek **IP: BağlantıNoktası** adresini kullanarak bir URI gibi tüm adres düzeni. Birden fazla hizmet örneği veya çoğaltmalar bir ana bilgisayar işlemi, bu durumda bunlar ya da farklı bağlantı noktalarını kullanacak ya da Windows http.sys çekirdek sürücü gibi bir bağlantı noktası paylaşımı mekanizmasının kullanılması gerekir paylaşabiliriz. Her iki durumda da, her bir hizmet örneği veya yineleme içinde bir ana bilgisayar işlemi benzersiz olarak adreslenebilir olmalıdır.

![Hizmet uç noktaları][1]

## <a name="service-discovery-and-resolution"></a>Hizmet bulma ve çözümleme
Dağıtılmış bir sistemde Hizmetleri bir makineden diğerine zamanla taşıyabilir. Bu Dengeleme, yükseltmeler, yük devretme veya genişleme kaynak dahil olmak üzere çeşitli nedenlerden ötürü oluşabilir. Bu hizmet, farklı IP adreslerine sahip düğümleri taşır ve hizmet dinamik olarak seçilen bir bağlantı noktası kullanıyorsa, farklı bağlantı noktalarını açmanız hizmet uç noktası adreslerini değiştirme anlamına gelir.

![Dağıtım Hizmetleri][7]

Service Fabric adlandırma hizmetine adlı bir bulma ve çözümleme hizmeti sağlar. Adlandırma Hizmeti hizmet örnekleri bunlar üzerinde dinleme bitiş noktası adreslerini eşleştiren bir tablo adlı tutar. Tüm Service Fabric adlandırılmış hizmet örnekleri olarak URI'ler, örneğin, gösterilen benzersiz adlara sahip `"fabric:/MyApplication/MyService"`. Adı hizmetin hizmet ömrü boyunca değiştirmez, hizmetleri taşıdığınızda, değiştirebileceğiniz uç nokta adresleri. Bu sabit URL'leri sahip olan ancak burada IP adresi değişebilir Web sitelerine benzer. Ve benzer DNS Web sitesinin URL'leri, IP adresleri için çözümler, Web Service Fabric hizmet adları kendi uç nokta adresine eşleyen bir kayıt şirketi sahiptir.

![Hizmet uç noktaları][2]

Çözümleme ve hizmetlerine bağlanan bir döngüde çalışmasına aşağıdaki adımları içerir:

* **Çözmek**: Hizmet yayınladığı uç noktayı adlandırma hizmetinden alın.
* **Connect**: Bu uç noktada kullanan ne olursa olsun protokolü üzerinden hizmete bağlanır.
* **Yeniden deneme**: Hizmet uç noktası adresi son sorgulandığından beri taşınmışsa örnek çözümlendiği için bir bağlantı girişimi nedeniyle, herhangi bir sayıda için başarısız olabilir. Bu durumda, önceki çözümleyin ve adımları denenmesi gerek bağlanmak ve bağlantı başarılı olana kadar bu döngü tekrarlanır.

## <a name="connecting-to-other-services"></a>Diğer hizmetlere bağlanma
Kümedeki düğümler aynı yerel ağda olduğundan Hizmetleri birbirine kümesi içinde genel olarak bağlanan diğer hizmetleri uç noktalarına doğrudan erişebilirsiniz. Hizmetler arasında bağlantı daha kolay yapmak için Service Fabric adlandırma hizmetine kullanan ek hizmetleri sağlar. Bir DNS hizmeti ve bir ters proxy hizmeti.


### <a name="dns-service"></a>DNS hizmeti
Birçok hizmet, bu yana özellikle kapsayıcı Hizmetleri, var olan bir URL adı olabilir, bunları çözmek mümkün olan standart DNS kullanarak Protokolü (adlandırma hizmeti Protokolü yerine) özellikle uygulama "kaldırma ve kaydırma" senaryolarında çok kullanışlı. DNS hizmeti yaptığı tam olarak budur. DNS adlarını bir hizmet adına eşlemenize ve bu nedenle bitiş IP adreslerini sağlar. 

Aşağıdaki diyagramda gösterildiği gibi Service Fabric kümesinde çalışan DNS hizmeti, DNS adları daha sonra bağlanmak için uç nokta adresleri döndürülecek adlandırma hizmeti tarafından çözülen hizmet adlarını eşler. Hizmetin DNS adı, oluşturma sırasında sağlanır. 

![Hizmet uç noktaları][9]

DNS kullanma hakkında daha fazla ayrıntı bkz: hizmet için [Azure Service Fabric DNS hizmetinde](service-fabric-dnsservice.md) makalesi.

### <a name="reverse-proxy-service"></a>Ters proxy hizmeti
Ters proxy, HTTPS dahil olmak üzere HTTP uç noktalarını kullanıma sunar kümedeki hizmetleri çözüyor. Ters proxy büyük ölçüde basitleştirir belirli bir URI biçimi sağlayarak diğer hizmetler ve kendi yöntemlerini çağırmak ve çözümleme işleme, bağlan, adlandırma hizmeti kullanarak birbiriyle iletişim kurmak için bir hizmet için gerekli adımları yeniden deneyin. Diğer bir deyişle, diğer hizmetlerin bu URL çağrılması kadar basit hale getirerek çağrılırken, adlandırma hizmeti gizler.

![Hizmet uç noktaları][10]

Ters proxy hizmetini kullanma hakkında daha fazla ayrıntı için bkz: [ters proxy Azure Service fabric'te](service-fabric-reverseproxy.md) makalesi.

## <a name="connections-from-external-clients"></a>Dış istemcilerden gelen bağlantıları
Kümedeki düğümler aynı yerel ağda olduğundan Hizmetleri birbirine kümesi içinde genel olarak bağlanan diğer hizmetleri uç noktalarına doğrudan erişebilirsiniz. Bazı ortamlarda ancak sınırlı bir bağlantı noktası üzerinden dış giriş trafiği yönlendiren bir yük dengeleyicinin arkasına bir kümesi olabilir. Bu gibi durumlarda, hizmetleri, yine de birbirleri ile iletişim kurmak ve adresleri adlandırma hizmeti kullanarak çözümlemek, ancak dış istemcilere hizmetlere bağlanmasına izin vermek için ek adımlar atılmalıdır.

## <a name="service-fabric-in-azure"></a>Azure Service Fabric'te
Azure Service Fabric kümesinde Azure Load Balancer yerleştirilir. Tüm dış trafiği kümeye, yük dengeleyici üzerinden geçmesi gerekir. Yük Dengeleyici, trafiği otomatik olarak iletir rastgele bir sıra için belirli bir bağlantı noktasında gelen *düğüm* sahip aynı bağlantı noktasını açın. Azure Load Balancer üzerinde açık bağlantı noktaları yalnızca bildiği *düğümleri*, kişi tarafından açık bağlantı noktaları hakkında bilmez *Hizmetleri*.

![Azure Load Balancer ve Service Fabric topolojisi][3]

Örneğin, bağlantı noktasındaki dış trafiği kabul etmek için **80**, şunları yapılandırılması gerekir:

1. 80 numaralı bağlantı noktasında dinleyen bir hizmet yazın. Hizmetin ServiceManifest.xml 80 numaralı bağlantı noktasını yapılandırın ve hizmeti, örneğin, şirket içinde barındırılan web sunucusu bir dinleyici açın.

    ```xml
    <Resources>
        <Endpoints>
            <Endpoint Name="WebEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>
    ```
    ```csharp
        class HttpCommunicationListener : ICommunicationListener
        {
            ...

            public Task<string> OpenAsync(CancellationToken cancellationToken)
            {
                EndpointResourceDescription endpoint =
                    serviceContext.CodePackageActivationContext.GetEndpoint("WebEndpoint");

                string uriPrefix = $"{endpoint.Protocol}://+:{endpoint.Port}/myapp/";

                this.httpListener = new HttpListener();
                this.httpListener.Prefixes.Add(uriPrefix);
                this.httpListener.Start();

                string publishUri = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
                return Task.FromResult(publishUri);
            }

            ...
        }

        class WebService : StatelessService
        {
            ...

            protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
            {
                return new[] { new ServiceInstanceListener(context => new HttpCommunicationListener(context))};
            }

            ...
        }
    ```
    ```java
        class HttpCommunicationlistener implements CommunicationListener {
            ...

            @Override
            public CompletableFuture<String> openAsync(CancellationToken arg0) {
                EndpointResourceDescription endpoint =
                    this.serviceContext.getCodePackageActivationContext().getEndpoint("WebEndpoint");
                try {
                    HttpServer server = com.sun.net.httpserver.HttpServer.create(new InetSocketAddress(endpoint.getPort()), 0);
                    server.start();

                    String publishUri = String.format("http://%s:%d/",
                        this.serviceContext.getNodeContext().getIpAddressOrFQDN(), endpoint.getPort());
                    return CompletableFuture.completedFuture(publishUri);
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }

            ...
        }

        class WebService extends StatelessService {
            ...

            @Override
            protected List<ServiceInstanceListener> createServiceInstanceListeners() {
                <ServiceInstanceListener> listeners = new ArrayList<ServiceInstanceListener>();
                listeners.add(new ServiceInstanceListener((context) -> new HttpCommunicationlistener(context)));
                return listeners;       
            }

            ...
        }
    ```
2. Azure'da bir Service Fabric kümesi oluşturma ve bağlantı noktasını belirtin **80** hizmeti barındıracak düğüm türü için bir özel uç nokta bağlantı noktası olarak. Birden fazla düğüm türü varsa, ayarlayabileceğiniz bir *yerleştirme kısıtlaması* açılmış özel uç nokta bağlantı noktası olan düğüm türü yalnızca çalışır sağlamak için hizmeti üzerinde.

    ![Düğüm türü bir bağlantı noktasını açın][4]
3. Küme oluşturulduktan sonra Azure Load Balancer 80 numaralı bağlantı noktasında trafiği iletmek için küme kaynak grubu yapılandırın. Azure Portal'dan bir küme oluştururken, bu otomatik olarak yapılandırılan her özel uç nokta bağlantı noktası için ayarlanır.

    ![Azure Load Balancer, trafiği][5]
4. Azure Load Balancer, trafiği göndermek için belirli bir düğüm gerekip gerekmediğini belirlemek için araştırmaları kullanır. Araştırma, düğüm yanıt olup olmadığını belirlemek için her düğüme bir uç nokta düzenli olarak denetler. Yapılandırılmış birkaç kez sonra bir yanıt almak yoklama başarısız olursa, yük dengeleyici trafik bu düğüme göndermeyi durdurur. Azure Portal'dan bir küme oluştururken, bir yoklama yapılandırılan her özel uç nokta bağlantı noktası için otomatik olarak ayarlanır.

    ![Azure Load Balancer, trafiği][8]

Azure Load Balancer ve araştırma yalnızca bilmeniz olduğunu unutmamak önemlidir *düğümleri*değil *Hizmetleri* düğümlerinde çalışıyor. Azure Load Balancer, her zaman Hizmetleri araştırmasına yanıt verebilir düğümlerinde kullanılabilir olduğundan emin olmak için dikkatli olunması gerekir, böylece araştırmasına yanıt düğümlerine trafiği gönderir.

## <a name="reliable-services-built-in-communication-api-options"></a>Güvenilir hizmetler: Yerleşik iletişim API seçenekleri
Reliable Services framework birkaç önceden oluşturulmuş iletişim seçenekleri ile birlikte gelir. Programlama modeli, communication framework ve hizmetlerinizi yazıldığı programlama dili seçimi hakkında bir en iyi sizin için işe karar bağlıdır.

* **Hiçbir özel protokolü:**  Communication framework'ün belirli bir seçim yoksa ancak öğe ayarladığınızda hızla çalışmaya başlamanızı istediğiniz durumunda sizin için ideal bir seçenektir [uzaktan hizmet iletişimini](service-fabric-reliable-services-communication-remoting.md), için sağlayan kesin türü belirtilmiş uzak yordam çağrıları Reliable Services ve Reliable Actors. Bu hizmet iletişimi ile kullanmaya başlamak için kolay ve en hızlı yoludur. Uzaktan hizmet iletişimini hizmeti adresleri, bağlantı, yeniden deneme ve hata işleme, çözümleme gerçekleştirir. Bu hem kullanılabilir C# ve Java uygulamaları.
* **HTTP**: Dilden iletişim için HTTP araçları ve HTTP sunucularına birçok farklı dillerde, Service Fabric tarafından desteklenen tüm kullanılabilir bir endüstri standardı seçim sağlar. Hizmetler dahil olmak üzere, kullanılabilir herhangi bir HTTP yığını kullanabilir [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) için C# uygulamalar. Yazılan istemciler C# yararlanabilir `ICommunicationClient` ve `ServicePartitionClient` sınıfları, oysa Java için kullanma `CommunicationClient` ve `FabricServicePartitionClient` sınıfları [hizmet çözümleme, HTTP bağlantılarını ve yeniden deneme döngüleri](service-fabric-reliable-services-communication.md).
* **WCF**: WCF iletişim Çerçevenizi kullanan mevcut kodunuz sonra kullanabileceğiniz `WcfCommunicationListener` için sunucu tarafı ve `WcfCommunicationClient` ve `ServicePartitionClient` istemci sınıfları. Bu ancak yalnızca kullanılabilir C# tabanlı uygulamaları Windows üzerindeki kümeler. Bu makalede daha fazla ayrıntı için bkz: [WCF tabanlı bir uygulama, iletişim yığını](service-fabric-reliable-services-communication-wcf.md).

## <a name="using-custom-protocols-and-other-communication-frameworks"></a>Özel protokoller ve diğer iletişim çerçeveleri kullanarak
Hizmetleri kullanabilir herhangi bir protokolü veya framework iletişimi için özel bir ikili protokolü üzerinden TCP yuvaları veya akış olayları aracılığıyla olup olmadığını [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) veya [Azure IOT hub'ı](https://azure.microsoft.com/services/iot-hub/). Service Fabric, iletişim, bulmak ve bağlanmak için tüm iş karmaşıklığının sırada, iletişim yığını, takılabilir API'ler sağlar. Bu makalede görecekleri [güvenilir hizmet iletişim modelini](service-fabric-reliable-services-communication.md) daha fazla ayrıntı için.

## <a name="next-steps"></a>Sonraki adımlar
Ndaki mevcut API'lere ve kavramları hakkında daha fazla bilgi [Reliable Services iletişim modelini](service-fabric-reliable-services-communication.md), sonra hızlı şekilde kullanmaya başlamanızı [uzaktan hizmet iletişimini](service-fabric-reliable-services-communication-remoting.md) veya bir iletişim yazmayı öğrenin derinlere Dinleyici kullanarak [OWIN barındırma ile Web API](service-fabric-reliable-services-communication-webapi.md).

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
