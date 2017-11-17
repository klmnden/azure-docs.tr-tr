---
title: "Bağlanma ve Azure Service Fabric hizmetleriyle iletişim | Microsoft Docs"
description: "Çözmek için bağlanmak ve Service Fabric hizmetleriyle iletişim öğrenin."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: msfussell
ms.assetid: 7d1052ec-2c9f-443d-8b99-b75c97266e6c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: d0b4ff1959465ade5f57c045d2a005e828638eb2
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="connect-and-communicate-with-services-in-service-fabric"></a>Bağlanma ve Service Fabric Hizmetleri ile iletişim
Service Fabric içinde bir hizmet birden çok VM genellikle dağıtılmış bir Service Fabric kümesindeki herhangi bir yerde çalışır. Bu tek bir yerden diğerine hizmet sahibi tarafından ya da otomatik olarak Service Fabric tarafından taşınabilir. Hizmetleri statik olarak belirli bir makine veya adresine bağlı olmak zorunda değildir.

Service Fabric uygulaması genellikle burada her hizmet özelleştirilmiş bir görev gerçekleştiren birçok farklı hizmetlerden oluşur. Bu hizmetler, diğer bir web uygulaması farklı kısımlarını işleme gibi bir tam işlevi oluşturmak için iletişim kurabilir. Bağlanmak ve Hizmetleri ile iletişim kuran istemci uygulamaları vardır. Bu belge ile ve hizmetlerinizi Service Fabric içinde arasında iletişim kurma açıklanır.

Bu Microsoft Virtual Academy video Ayrıca hizmet iletişimi ele alınmıştır:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=iYFCk76yC_6706218965">  
<img src="./media/service-fabric-connect-and-communicate-with-services/CommunicationVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="bring-your-own-protocol"></a>Kendi Protokolü Getir
Service Fabric, hizmetlerin yaşam döngüsünü yönetmenize yardımcı olur, ancak hizmetlerinizi neler hakkında kararlar yapmaz. Bu iletişim içerir. Hizmetinizi Service Fabric tarafından açıldığında, hizmetinizin istediğiniz ne olursa olsun protokolü veya iletişim yığını kullanarak gelen istekleri için bir uç nokta ayarlama fırsatı olmasıdır. Hizmetinizi normal üzerinde dinler **IP: BağlantıNoktası** bir URI gibi herhangi bir adres düzeni kullanarak adres. Birden fazla hizmet örneği veya çoğaltmaları bir ana bilgisayar işlemi, bu durumda bunlar ya da farklı bağlantı noktaları veya Windows http.sys çekirdek sürücüsü gibi bir bağlantı noktası paylaşma mekanizması kullanın gerekir paylaşabilir. Her iki durumda da, her bir hizmet örneği veya bir ana bilgisayar işlemi yinelemede benzersiz olarak adreslenebilir olmalıdır.

![Hizmet uç noktaları][1]

## <a name="service-discovery-and-resolution"></a>Hizmet bulma ve çözümleme
Dağıtılmış bir sistemde Hizmetleri bir makineden diğerine zamanla taşıyabilir. Bu kaynak Dengeleme, yükseltmeler, yük devretme veya genişleme dahil olmak üzere çeşitli nedenlerden kaynaklanabilir. Bu hizmet düğümleri farklı IP adresleriyle taşır ve hizmet dinamik olarak seçilen bağlantı noktası kullanıyorsa, farklı bağlantı noktalarını açmanız hizmeti bitiş noktası adreslerini değiştirmek anlamına gelir.

![Hizmetlerinin dağıtımı][7]

Service Fabric adlandırma hizmeti adı verilen bir bulma ve çözümleme hizmeti sağlar. Adlandırma Hizmeti, hizmet örneği üzerinde dinleme bitiş noktası adreslerine eşleyen bir tablo adında tutar. Service Fabric tüm adlandırılmış hizmet örnekleri olarak URI'ler, örneğin temsil benzersiz adlara sahip `"fabric:/MyApplication/MyService"`. Hizmetin adını hizmet ömrü boyunca değiştirmez, hizmetleri taşıdığınızda, değiştirebileceğiniz uç nokta adresleri. Bu sabit URL'leri sahip ancak IP adresi burada değişebilir Web sitelerine benzer. Ve Web sitesi URL'leri IP adresine çözümler, web üzerinde DNS'ye benzer Service Fabric hizmet adlarını kendi uç nokta adresine eşleyen bir kayıt sahiptir.

![Hizmet uç noktaları][2]

Çözümleme ve hizmetlerine bağlanan bir döngüde çalıştırılacak aşağıdaki adımları içerir:

* **Gidermek**: bir hizmeti yayımladı uç noktası adlandırma hizmetinden alın.
* **Connect**: ne olursa olsun, bu uç noktada kullandığı protokol üzerinden hizmetine bağlanın.
* **Yeniden deneme**: hizmet uç noktası adresi son tarihten itibaren taşınmışsa örnek çözümlendiği için bir bağlantı girişimi nedeniyle, herhangi bir sayı için başarısız olabilir. Bu durumda, önceki çözümlemek ve adımları denenmesi gerek bağlanmak ve bağlantı başarılı olana kadar bu döngü yinelenir.

## <a name="connecting-to-other-services"></a>Diğer hizmetlere bağlanma
Kümedeki düğümler aynı yerel ağ üzerinde olduğundan birbirlerine genellikle bir küme içinde bağlanma Hizmetleri doğrudan diğer hizmetler uç noktalarına erişebilirsiniz. Olmak için hizmetleri arasında bağlanmak kolaydır, Service Fabric adlandırma hizmeti kullanan ek hizmetleri sağlar. DNS hizmeti ve bir ters proxy hizmeti.


### <a name="dns-service"></a>DNS hizmeti
Birçok Hizmetleri bu yana özellikle kapsayıcılı Hizmetleri, var olan bir URL adı olabilir, standart DNS kullanarak bunları gidermek bölümlemeye Protokolü (adlandırma hizmeti Protokolü yerine) özellikle uygulama "kaldırın ve shift" senaryolarında çok kolay. Bu, tam olarak DNS hizmeti ne olur. DNS adları bir hizmet adı ve bu nedenle uç noktası IP adreslerini çözümlemesine olanak sağlar. 

Aşağıdaki çizimde gösterildiği gibi Service Fabric kümede çalışan DNS hizmeti, DNS adlarını ardından bağlanmak için uç nokta adresleri döndürmek için adlandırma hizmeti tarafından çözümlenen hizmet adlarını eşler. Hizmeti için DNS ad oluşturma sırasında sağlanır. 

![Hizmet uç noktaları][9]

Bkz: DNS kullanma hakkında daha fazla ayrıntı hizmetinin [Azure Service Fabric DNS hizmetinde](service-fabric-dnsservice.md) makalesi.

### <a name="reverse-proxy-service"></a>Ters proxy hizmeti
Ters proxy services HTTPS dahil olmak üzere HTTP uç noktalarını kullanıma sunar kümedeki giderir. Ters proxy büyük ölçüde belirli bir URI biçimi sağlayarak diğer hizmetler ve bunların yöntemleri çağırma basitleştirir ve çözümleme işler, bağlantı, adlandırma hizmeti kullanmada diğeriyle iletişim için bir hizmet için gerekli adımları yeniden deneyin. Diğer bir deyişle, diğer hizmetler bu URL çağrılması kadar basit hale getirerek çağrılırken sizden adlandırma hizmeti gizler.

![Hizmet uç noktaları][10]

Ters proxy hizmeti kullanma hakkında daha fazla bilgi için bkz [ters proxy Azure Service Fabric](service-fabric-reverseproxy.md) makalesi.

## <a name="connections-from-external-clients"></a>Dış istemcilerden gelen bağlantıları
Kümedeki düğümler aynı yerel ağ üzerinde olduğundan birbirlerine genellikle bir küme içinde bağlanma Hizmetleri doğrudan diğer hizmetler uç noktalarına erişebilirsiniz. Bazı ortamlarda, sınırlı bir dizi bağlantı noktası üzerinden harici giriş trafiğini yönlendiren bir yük dengeleyicinin arkasındaki bir küme ancak olabilir. Bu durumda, hizmetler, hala birbirleri ile iletişim kurmak ve adlandırma hizmeti kullanmada adresleri çözümleyecek, ancak dış istemcilerin hizmetlere bağlanmasına izin vermek için ek adımlar izlenmelidir.

## <a name="service-fabric-in-azure"></a>Azure Service Fabric
Azure Service Fabric kümesi bir Azure yük dengeleyici yer alır. Tüm dış küme trafiğine, yük dengeleyici üzerinden geçmesi gerekir. Yük Dengeleyici trafiği otomatik olarak ilet belirli bir bağlantı noktasına bir rastgele gelen trafik *düğümü* açmak aynı bağlantı noktası vardır. Azure yük dengeleyici üzerinde açık bağlantı noktalarını yalnızca bildiği *düğümleri*, bağlantı noktalarının açık kişi tarafından hakkında bilmez *Hizmetleri*.

![Azure yük dengeleyici ve Service Fabric topolojisi][3]

Örneğin, bağlantı noktasındaki dış trafiğini kabul etmek için **80**, şunları yapılandırılmış olması gerekir:

1. Bağlantı noktası 80 üzerinde dinleyen bir hizmet yazma. Hizmetin ServiceManifest.xml 80 numaralı bağlantı noktasını yapılandırın ve hizmeti, örneğin, bir kendi kendini barındıran web sunucusu bir dinleyici açın.

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
2. Service Fabric kümesi oluşturma ve bağlantı noktasını belirtin **80** hizmetini barındıran düğüm türü için bir özel uç nokta bağlantı noktası olarak. Birden fazla düğüm türü varsa, ayarlayabileceğiniz bir *yerleştirme kısıtlaması* açılmış özel uç nokta bağlantı noktası olan düğüm türü yalnızca çalışır sağlamak için hizmeti üzerinde.

    ![Düğüm türü bir bağlantı noktası açın][4]
3. Küme oluşturulduktan sonra Azure yük dengeleyici 80 numaralı bağlantı noktasında trafik iletmek için küme kaynak grubu yapılandırın. Azure Portalı aracılığıyla bir küme oluştururken, bu otomatik olarak yapılandırılan her özel uç nokta bağlantı noktası için ayarlanır.

    ![Azure yük dengeleyici trafiği ilet][5]
4. Azure yük dengeleyici bir araştırma belirli bir düğüme trafiği göndermek gerekip gerekmediğini belirlemek için kullanır. Araştırma düğüm yanıt olup olmadığını belirlemek için her düğüme bir uç nokta düzenli olarak denetler. Araştırma yapılandırılmış birkaç kez sonra bir yanıt alamazsa, yük dengeleyici bu düğüme trafiği göndermeye durdurur. Azure Portalı aracılığıyla bir küme oluştururken, bir yoklama yapılandırılmış her özel uç nokta bağlantı noktası için otomatik olarak ayarlanır.

    ![Azure yük dengeleyici trafiği ilet][8]

Azure yük dengeleyici ve araştırma yalnızca hakkında bilmeniz olduğunu unutmamak önemlidir *düğümleri*değil *Hizmetleri* düğümlerinde çalışıyor. Azure yük dengeleyici her zaman trafiği Hizmetleri için araştırma yanıt verebilir düğümler üzerinde kullanılabilir olduğundan emin olmak için dikkatli olunması gerekir böylece araştırma için yanıt düğümleri gönderir.

## <a name="reliable-services-built-in-communication-api-options"></a>Güvenilir hizmetler: Yerleşik iletişim API seçenekleri
Güvenilir hizmetler framework birkaç önceden derlenmiş iletişim seçenekleri ile birlikte gelir. Programlama modeli, iletişim framework ve hizmetlerinizi yazılmış programlama dili seçimi hakkında bir en iyi işinize karar bağlıdır.

* **Belirli bir protokol:** iletişim framework'ün belirli bir seçim yok ancak öğe ayarladığınızda hızla çalıştırabileceğinizi istediğiniz durumunda sizin için ideal bir seçenektir [hizmet remoting](service-fabric-reliable-services-communication-remoting.md), kesin türü belirtilmiş uzaktan yordam sağlayan Reliable Services ve Reliable Actors çağırır. Hizmet iletişimi ile çalışmaya başlamak için kolay ve hızlı yolu budur. Hizmet remoting service adresleri, bağlantı, yeniden deneyin ve hata işleme çözünürlüğü işler. Bu, hem C# ve Java uygulamaları için kullanılabilir.
* **HTTP**: dilden bağımsız iletişim için HTTP bir endüstri standardı seçim araçları ve HTTP sunucularıyla birçok farklı dillerde, Service Fabric tarafından desteklenen tüm kullanılabilir sağlar. Hizmetleri dahil olmak üzere, kullanılabilir tüm HTTP yığınını kullanma [ASP.NET Web API](service-fabric-reliable-services-communication-webapi.md) C# uygulamaları için. C# ile yazılmış istemcileri yararlanabilir `ICommunicationClient` ve `ServicePartitionClient` , ancak sınıfları Java için kullan `CommunicationClient` ve `FabricServicePartitionClient` sınıfları, [hizmet çözümleme, HTTP bağlantılarını ve yeniden deneme döngüsü](service-fabric-reliable-services-communication.md).
* **WCF**: WCF iletişimi çerçeve olarak kullanan var olan kodu sahip sonra kullanabileceğiniz `WcfCommunicationListener` sunucu tarafı için ve `WcfCommunicationClient` ve `ServicePartitionClient` istemci sınıfları. Bu ancak yalnızca Windows tabanlı kümelerde C# uygulamaları için kullanılabilir. Bu makalede daha fazla ayrıntı için bkz: [WCF tabanlı bir uygulama iletişim yığınının](service-fabric-reliable-services-communication-wcf.md).

## <a name="using-custom-protocols-and-other-communication-frameworks"></a>Özel protokoller ve diğer iletişim çerçeveleri kullanma
Hizmetleri kullanma herhangi bir protokolü veya framework iletişimi için özel bir ikili protokol TCP yuvaları ya da aracılığıyla akış olaylarını üzerinden olup olmadığını [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) veya [Azure IOT Hub](https://azure.microsoft.com/services/iot-hub/). Service Fabric iletişim bulmak ve bağlanmak için tüm iş sizden soyutlanır karşın, iletişim yığını, Tak API'ler sağlar. Bu makalede gördükleri hakkında [güvenilir hizmet iletişim modelini](service-fabric-reliable-services-communication.md) daha fazla ayrıntı için.

## <a name="next-steps"></a>Sonraki adımlar
Kullanılabilen API'leri ve kavramları hakkında daha fazla bilgi [Reliable Services iletişim modelini](service-fabric-reliable-services-communication.md), sonra hızlı şekilde kullanmaya başlamanızı [hizmet remoting](service-fabric-reliable-services-communication-remoting.md) veya kullanarak iletişim dinleyici yazma konusunda bilgi almak için derinlere [OWIN kendini barındırma ile Web API](service-fabric-reliable-services-communication-webapi.md).

[1]: ./media/service-fabric-connect-and-communicate-with-services/serviceendpoints.png
[2]: ./media/service-fabric-connect-and-communicate-with-services/namingservice.png
[3]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancertopology.png
[4]: ./media/service-fabric-connect-and-communicate-with-services/nodeport.png
[5]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerport.png
[7]: ./media/service-fabric-connect-and-communicate-with-services/distributedservices.png
[8]: ./media/service-fabric-connect-and-communicate-with-services/loadbalancerprobe.png
[9]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[10]: ./media/service-fabric-reverseproxy/internal-communication.png
