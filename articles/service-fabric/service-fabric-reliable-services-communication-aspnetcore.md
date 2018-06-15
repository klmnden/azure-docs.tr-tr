---
title: Hizmet ASP.NET Core ile iletişim | Microsoft Docs
description: Durum bilgisiz ve durum bilgisi olan güvenilir hizmetler ASP.NET Core kullanmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: 8aa4668d-cbb6-4225-bd2d-ab5925a868f2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: 7786e08e04d2ebce757b4c47b8ed599036c95958
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207868"
---
# <a name="aspnet-core-in-service-fabric-reliable-services"></a>ASP.NET çekirdek Service Fabric güvenilir hizmetler

ASP.NET Core modern bulut tabanlı Internet'e bağlı uygulamaları, web uygulamaları, IOT uygulamaları ve mobil arka uçlarını gibi oluşturmak için yeni bir açık kaynaklı ve çapraz platform framework kümesidir. 

Bu makalede hizmet doku güvenilir hizmetler kullanarak ASP.NET Core hizmetlerini barındıran bir ayrıntılı kılavuzdur **Microsoft.ServiceFabric.AspNetCore.** * NuGet paketlerini kümesi.

Service Fabric ASP.NET çekirdek tanıtım bir öğretici ve geliştirme ortamınızı ayarlayın alma hakkında yönergeler için bkz: [.NET uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md).

Bu makalede geri kalanı ile ASP.NET Core bilginiz varsayar. Okuma öneririz, varsa [ASP.NET Core Temelleri](https://docs.microsoft.com/aspnet/core/fundamentals/index).

## <a name="aspnet-core-in-the-service-fabric-environment"></a>Service Fabric ortamında ASP.NET Çekirdeği

Tam .NET Framework veya .NET Core üzerinde ASP.NET Core uygulamaları çalıştırabilirsiniz olsa da, Service Fabric Hizmetleri şu anda tam .NET Framework üzerinde yalnızca çalıştırabilirsiniz. Bunun anlamı, bir ASP.NET Core Service Fabric hizmet oluşturma sırasında tam .NET Framework hala hedeflemesi gerekir.

ASP.NET Core Service Fabric iki farklı şekillerde kullanılabilir:
 - **Bir konuk yürütülebilir dosya barındırılan**. Bu, öncelikle hiçbir kod değişikliklerle Service Fabric mevcut ASP.NET Core uygulamaları çalıştırmak için kullanılır.
 - **İçinde güvenilir bir hizmet Farklı Çalıştır**. Bu, Service Fabric çalışma zamanı ile daha iyi tümleştirme sağlar ve durum bilgisi olan ASP.NET Core hizmetleri sağlar.

Bu makalenin geri kalanında, ASP.NET Core güvenilir bir Service Fabric SDK ile birlikte gelen ASP.NET Core tümleştirme bileşenlerini kullanarak hizmet içinde kullanımı açıklanmaktadır. 

## <a name="service-fabric-service-hosting"></a>Service Fabric hizmeti barındırma

Service Fabric içinde bir veya daha fazla örnekleri ve/veya hizmetinizin çoğaltmaları çalıştırmak bir *hizmet ana bilgisayar işlemi*, hizmet kodunuzun çalıştığı bir yürütülebilir dosya. Kendi hizmet yazar olarak, hizmet ana bilgisayar işlemi ve Service Fabric etkinleştirir ve sizin için izler.

(En fazla MVC 5) geleneksel ASP.NET IIS System.Web.dll aracılığıyla sıkı şekilde bağlı. ASP.NET çekirdek web sunucusu ve web uygulamanızı arasında ayrım sağlar. Bu web uygulamaları farklı web sunucuları arasında taşınabilir olmasını sağlar ve ayrıca web sunucularının sağlar *kendi kendini barındıran*, ayrılmış IIS gibi web sunucusu yazılımı anlamına gelir, bir web sunucusu tarafından sahip olunan bir işlem aksine, kendi işleminde başlatabilirsiniz. 

Service Fabric hizmeti ve ASP.NET, bir konuk yürütülebilir dosya veya güvenilir bir hizmette birleştirmek için hizmet ana bilgisayar işlemi içinde ASP.NET başlatılamaz olması gerekir. Kendi kendine barındırma ASP.NET Core, bunu yapmanızı sağlar.

## <a name="hosting-aspnet-core-in-a-reliable-service"></a>ASP.NET Core güvenilir hizmetinde barındırma
Genellikle, kendi kendini barındıran ASP.NET Core uygulamaları bir WebHost uygulamanın giriş noktası gibi oluşturmasına `static void Main()` yönteminde `Program.cs`. Bu durumda, WebHost yaşam döngüsü işlemi yaşam döngüsü için bağlıdır.

![Bir işlemde ASP.NET Core barındırma][0]

Bu hizmet türünün örneklerini oluşturabilir böylece uygulama giriş noktası yalnızca bir hizmet türünün Service Fabric çalışma zamanı ile kaydetmek için kullanıldığından ancak, uygulama giriş noktası bir WebHost güvenilir bir hizmet oluşturmak için doğru yerde değil. WebHost kendisini güvenilir bir hizmet olarak oluşturulmalıdır. Hizmet barındırma işlemi içinde birden çok yaşam döngüleri hizmet örneği ve/veya çoğaltmaları gidebilirsiniz. 

Bir güvenilir hizmet örneği türetme, hizmet sınıfı tarafından temsil edilen `StatelessService` veya `StatefulService`. Bir hizmet için iletişim yığını bulunan bir `ICommunicationListener` hizmet sınıfınızı uygulamasında. `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet paketlerini içeren uygulamaları `ICommunicationListener` başlatın ve ASP.NET Core WebHost Kestrel ya da güvenilir bir hizmette HttpSys yönetin.

![ASP.NET Core güvenilir hizmetinde barındırma][1]

## <a name="aspnet-core-icommunicationlisteners"></a>ASP.NET Core ICommunicationListeners
`ICommunicationListener` Kestrel ve HttpSys içinde için uygulamaları `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet paketlerini benzer kullanım desenlerini sahip ancak her web sunucusuna belirli biraz farklı eylemleri gerçekleştirin. 

Her iki iletişim dinleyicileri şu bağımsız değişkenleri alan bir oluşturucu sağlar:
 - **`ServiceContext serviceContext`**`ServiceContext` Çalışan hizmeti hakkında bilgi içeren nesne.
 - **`string endpointName`**: adını bir `Endpoint` ServiceManifest.xml yapılandırmasında. Burada iki iletişim dinleyicileri farklı budur öncelikle: HttpSys **gerektirir** bir `Endpoint` Kestrel çalışmazken yapılandırma.
 - **`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**:, oluşturduğunuz ve dönüş uygulayan bir lambda bir `IWebHost`. Bu sayede yapılandırmak `IWebHost` ASP.NET Core uygulamada normalde olduğu gibi. Service Fabric tümleştirme bağlı olarak, seçeneklerini için oluşturulan bir URL kullanın lambda sağlar ve `Endpoint` sağladığınız yapılandırma. URL sonra değiştirilebilir veya olarak kullanılan olduğunu-web sunucusu başlatmaktır.

## <a name="service-fabric-integration-middleware"></a>Service Fabric tümleştirme Ara
`Microsoft.ServiceFabric.Services.AspNetCore` NuGet paketini içeren `UseServiceFabricIntegration` genişletme yöntemi `IWebHostBuilder` , Service Fabric algılayan ara yazılımı ekler. Bu ara yazılımın Kestrel veya HttpSys yapılandırır `ICommunicationListener` Service Fabric adlandırma hizmeti benzersiz bir hizmet URL'si için ve istemciler doğru hizmete bağlanırken emin olmak için istemci istekleri doğrular. Bu, Service Fabric, birden çok web uygulamaları burada aynı fiziksel veya sanal makine üzerinde çalıştırabilirsiniz ancak istemciler yanlışlıkla yanlış hizmete bağlanmasını önlemek için benzersiz bir ana bilgisayar adları kullanmayın gibi paylaşılan konak ortamında gereklidir. Bu senaryo, sonraki bölümde daha ayrıntılı açıklanmıştır.

### <a name="a-case-of-mistaken-identity"></a>Hatalı kimlik durumunun
Hizmet çoğaltmalar, protokol, bakılmaksızın benzersiz IP: BağlantıNoktası birleşimi üzerinde dinler. Bir IP: BağlantıNoktası uç noktada dinleme hizmeti çoğaltma başladıktan sonra o uç noktası adresi Service Fabric adlandırma Burada, istemciler veya diğer hizmetler tarafından keşfedilmesini hizmeti bildirir. Hizmet uygulaması dinamik olarak atanan bağlantı noktası kullanıyorsanız, bir hizmet çoğaltma tesadüfen aynı fiziksel veya sanal makine üzerinde öncekinden başka bir hizmet aynı IP: BağlantıNoktası uç noktası kullanabilir. Bu bir istemciye mistakely neden yanlış hizmetine bağlanın. Aşağıdaki olaylar dizisi oluşursa bu durum oluşabilir:

 1. Hizmeti bir HTTP üzerinden 10.0.0.1:30000 üzerinde dinler. 
 2. İstemci Hizmeti A çözümler ve adres 10.0.0.1:30000 alır
 3. Bir hizmet farklı bir düğüme taşır.
 4. Hizmet B üzerinde 10.0.0.1 yerleştirilir ve tesadüfen 30000 aynı bağlantı noktasını kullanır.
 5. İstemci, hizmet bir önbelleğe alınan adresi 10.0.0.1:30000 ile bağlanmaya çalışır.
 6. İstemci şimdi kullanıldıklarını değil B yanlış hizmete bağlı hizmetine başarıyla bağlandı.

Bu hataları tanılamak zor olabilir rastgele zamanlarda neden olabilir. 

### <a name="using-unique-service-urls"></a>Benzersiz bir hizmet URL'leri kullanma
Bunu önlemek için hizmetleri benzersiz bir tanımlayıcı adlandırma hizmeti için bir uç nokta sonrası ve ardından istemci istekleri sırasında bu benzersiz tanımlayıcı doğrulamak. Bu, güvenilir bir saldırgan-Kiracı olmayan ortamda hizmetleri arasında İşbirlikçi bir işlemdir. Bu, bir kiracı tehlikeli ortamda güvenli hizmetinin kimlik doğrulaması sağlamaz.

Ara yazılım tarafından eklenen güvenilir bir ortamda `UseServiceFabricIntegration` yöntemi otomatik olarak ekler benzersiz bir tanımlayıcı adlandırma hizmete gönderilen ve bu tanımlayıcı her istekte doğrular adresine. Tanımlayıcı eşleşmiyorsa, ara yazılım hemen bir HTTP 410 kaybedildi yanıt verir.

Dinamik olarak atanan bir bağlantı noktası olmalısınız kullanan Hizmetleri bu ara yazılımı kullanın.

Sabit benzersiz bağlantı noktası kullanan hizmetler işbirlikçi ortamında bu sorun yok. Sabit benzersiz bağlantı noktası tipik olarak bilinen bir bağlantı noktasına bağlanmak istemci uygulamaları için gereken dışarıya yönelik hizmetler için kullanılır. Örneğin, çoğu Internet'e yönelik web uygulamaları, web tarayıcısı bağlantıları için bağlantı noktası 80 veya 443 kullanır. Bu durumda, benzersiz tanımlayıcı etkinleştirilmemelidir.

Aşağıdaki diyagramda istek akışı etkin Ara yazılımla gösterilmektedir:

![Service Fabric ASP.NET Core tümleştirme][2]

Kestrel ve HttpSys `ICommunicationListener` uygulamaları tam olarak aynı şekilde bu mekanizması kullanır. HttpSys dahili arka plandaki kullanarak benzersiz URL yollarına bağlı istekleri ayırt edebilir ancak *http.sys* işlevselliği özelliği, bağlantı noktası *değil* HttpSys tarafından kullanılan `ICommunicationListener` Uygulama, daha önce açıklanan senaryo HTTP 503 ve HTTP 404 hata durum kodları neden. HTTP 503 ve HTTP 404 zaten genellikle diğer hataları belirtmek için kullanılan gibi sırayla çok hata amacı belirlemek istemcileri zorlaştırır. Bu nedenle, hem Kestrel ve HttpSys `ICommunicationListener` uygulamaları standartlaştırmak tarafından sağlanan ara yazılım üzerinde `UseServiceFabricIntegration` genişletme yöntemi istemcileri yalnızca gerçekleştirmeniz gerekir böylece hizmet uç noktası yeniden çözümlemek HTTP 410 yanıtları eylem.

## <a name="httpsys-in-reliable-services"></a>Güvenilir hizmetler HttpSys
HttpSys kullanılabilir güvenilir hizmetinde içeri aktararak **Microsoft.ServiceFabric.AspNetCore.HttpSys** NuGet paketi. Bu pakette `HttpSysCommunicationListener`, uygulaması `ICommunicationListener`, sağlayan bir ASP.NET Core WebHost içinde güvenilir bir hizmet oluşturmak web sunucusu olarak HttpSys kullanarak.

HttpSys üzerinde oluşturulan [Windows HTTP sunucu API'sini](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx). Bu kullanır *http.sys* HTTP isteklerini işleyen ve web uygulamaları çalışan işlemler için yönlendirmek için IIS tarafından kullanılan çekirdek sürücüsü. Bu, aynı fiziksel veya sanal makinenize benzersiz URL yolu veya ana bilgisayar adı tarafından disambiguated web uygulamalarını barındırmasını aynı bağlantı noktasında birden çok işlem sağlar. Bu özellikler aynı kümedeki birden çok Web sitesini barındırmak için Service Fabric yararlıdır.

Aşağıdaki diyagram HttpSys nasıl kullandığını gösterir *http.sys* bağlantı noktası paylaşımı için Windows çekirdek sürücüsü:

![HTTP.sys][3]

### <a name="httpsys-in-a-stateless-service"></a>Durum bilgisiz hizmetindeki HttpSys
Kullanılacak `HttpSys` durum bilgisiz hizmetinde, geçersiz kılma `CreateServiceInstanceListeners` yöntemi ve return bir `HttpSysCommunicationListener` örneği:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new HttpSysCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseHttpSys()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build()))
    };
}
```

### <a name="httpsys-in-a-stateful-service"></a>Durum bilgisi olan hizmet HttpSys

`HttpSysCommunicationListener` şu anda arka plandaki ile zorluklar nedeniyle durum bilgisi olan hizmetler kullanmak için tasarlanmamıştır *http.sys* bağlantı noktası özelliği paylaşma. Daha fazla bilgi için aşağıdaki bölümde HttpSys ile dinamik bağlantı noktası ayırma bakın. Durum bilgisi olan hizmetler için Kestrel önerilen web sunucusudur.

### <a name="endpoint-configuration"></a>Uç nokta yapılandırması

Bir `Endpoint` Windows HTTP sunucu HttpSys dahil olmak üzere API kullanan web sunucuları için yapılandırma gereklidir. Windows HTTP sunucu API'si kullanan web sunucularının URL'LERİNİ ile ilk yedek gerekir *http.sys* (Bu normalde ile gerçekleştirilir [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) aracı). Bu eylem, Hizmetleri varsayılan olmayan yükseltilmiş ayrıcalıklar gerektiriyor. "Http" veya "https" seçeneklerini `Protocol` özelliği `Endpoint` yapılandırmasında *ServiceManifest.xml* özellikle bir URL ile kaydetmek için Service Fabric çalışma zamanını istemek için kullanılan *http.sys* adına kullanma [ *güçlü joker* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL öneki.

Örneğin, ayırmak için `http://+:80` bir hizmet için aşağıdaki yapılandırma içinde ServiceManifest.xml kullanılmalıdır:

```xml
<ServiceManifest ... >
    ...
    <Resources>
        <Endpoints>
            <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>

</ServiceManifest>
```

Ve uç nokta adı için geçirilmelidir `HttpSysCommunicationListener` Oluşturucusu:

```csharp
 new HttpSysCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     return new WebHostBuilder()
         .UseHttpSys()
         .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
         .UseUrls(url)
         .Build();
 })
```

#### <a name="use-httpsys-with-a-static-port"></a>Statik bir bağlantı noktası ile HttpSys kullanın
Statik bağlantı noktası HttpSys ile kullanmak için bağlantı noktası numarası sağlayın `Endpoint` yapılandırma:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-httpsys-with-a-dynamic-port"></a>Dinamik bir bağlantı noktası ile HttpSys kullanın
Dinamik olarak atanan bir bağlantı noktasının HttpSys ile kullanmak için atlayın `Port` özelliğinde `Endpoint` yapılandırma:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

Dinamik bir bağlantı noktası tarafından ayrılmış Not bir `Endpoint` yapılandırma, yalnızca bir bağlantı noktası sağlar *ana bilgisayar işlemi başına*. Geçerli Service Fabric barındırma modeli birden fazla hizmet örneği ve/veya çoğaltmaların her biri aracılığıyla ayrılmış aynı bağlantı noktasını paylaşmak anlamına gelir aynı işlemde barındırılan verir `Endpoint` yapılandırma. Birden çok HttpSys örneği arka plandaki kullanarak bir bağlantı noktası paylaşabilirsiniz *http.sys* özellik, ancak, bağlantı noktası tarafından desteklenmiyor `HttpSysCommunicationListener` istemci isteklerini tanıttığı zorluklar nedeniyle. Dinamik bağlantı noktası kullanım için Kestrel önerilen web sunucusudur.

## <a name="kestrel-in-reliable-services"></a>Güvenilir hizmetler kestrel
Kestrel kullanılabilir güvenilir hizmetinde içeri aktararak **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet paketi. Bu pakette `KestrelCommunicationListener`, uygulaması `ICommunicationListener`, sağlayan bir ASP.NET Core WebHost içinde güvenilir bir hizmet oluşturmak web sunucusu olarak Kestrel kullanarak.

Kestrel ASP.NET Core için platformlar arası web sunucusu libuv, platformlar arası zaman uyumsuz g/ç kitaplığı dayalıdır. HttpSys Kestrel merkezi endpoint Yöneticisi gibi kullanmaz *http.sys*. Ve HttpSys farklı olarak, birden çok işlemler arasında bağlantı noktası paylaşımı Kestrel desteklemiyor. Kestrel her örneğini benzersiz bir bağlantı noktası kullanmanız gerekir.

![kestrel][4]

### <a name="kestrel-in-a-stateless-service"></a>Durum bilgisiz hizmetindeki kestrel
Kullanılacak `Kestrel` durum bilgisiz hizmetinde, geçersiz kılma `CreateServiceInstanceListeners` yöntemi ve return bir `KestrelCommunicationListener` örneği:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

### <a name="kestrel-in-a-stateful-service"></a>Durum bilgisi olan hizmet kestrel
Kullanılacak `Kestrel` durum bilgisi olan bir hizmeti geçersiz kılma `CreateServiceReplicaListeners` yöntemi ve return bir `KestrelCommunicationListener` örneği:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new ServiceReplicaListener[]
    {
        new ServiceReplicaListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                         services => services
                             .AddSingleton<StatefulServiceContext>(serviceContext)
                             .AddSingleton<IReliableStateManager>(this.StateManager))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

Bu örnekte, bir tek örneğini `IReliableStateManager` WebHost bağımlılık ekleme kapsayıcısına sağlanır. Bu kesinlikle gerekli değildir, ancak kullanmanıza olanak tanır `IReliableStateManager` ve güvenilir koleksiyonlarda MVC denetleyici eylem yöntemleri.

Unutmayın bir `Endpoint` yapılandırma adı **değil** için sağlanan `KestrelCommunicationListener` durum bilgisi olan bir hizmette. Bu aşağıdaki bölümünde daha ayrıntılı olarak açıklanmıştır.

### <a name="endpoint-configuration"></a>Uç nokta yapılandırması
Bir `Endpoint` yapılandırma Kestrel kullanmak için gerekli değildir. 

Kestrel basit tek başına web sunucusunun adıdır; HttpSys (veya HttpListener) aksine, onu ihtiyaç duymadığı bir `Endpoint` yapılandırmasında *ServiceManifest.xml* başlatmadan önce URL kayıt gerektirmediğinden. 

#### <a name="use-kestrel-with-a-static-port"></a>Statik bir bağlantı noktası ile Kestrel kullanın
Statik bağlantı noktası yapılandırılabilir `Endpoint` ServiceManifest.xml yapılandırma Kestrel ile kullanım için. Bu kesinlikle gerekli olmamakla birlikte, iki olası faydaları sağlar:
 1. Bağlantı noktası uygulama bağlantı noktası aralığı içinde uymazsa, işletim sistemi güvenlik duvarı üzerinden Service Fabric tarafından açılmış.
 2. Size sağlanan URL `KestrelCommunicationListener` Bu bağlantı noktasını kullanır.

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

Varsa bir `Endpoint` olan yapılandırılmış, adını içine geçirilmelidir `KestrelCommunicationListener` Oluşturucusu: 

```csharp
new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) => ...
```

Varsa bir `Endpoint` yapılandırması kullanılmaz, adını atla `KestrelCommunicationListener` Oluşturucusu. Bu durumda, dinamik bir bağlantı noktası kullanılır. Daha fazla bilgi için sonraki bölüme bakın.

#### <a name="use-kestrel-with-a-dynamic-port"></a>Dinamik bir bağlantı noktası ile Kestrel kullanın
Kestrel gelen otomatik olarak bağlantı noktası atamasını kullanamaz `Endpoint` ServiceManifest.xml, yapılandırmada olduğundan otomatik olarak bağlantı noktası atama bir `Endpoint` yapılandırması atar benzersiz bir bağlantı noktası başına *ana bilgisayar işlemi*, ve tek ana bilgisayar işlemi birden çok Kestrel örnekleri içerebilir. Bağlantı noktası paylaşımı Kestrel desteklemediğinden, her Kestrel örneğini benzersiz bir bağlantı noktasında açılmalıdır gibi bu çalışmaz.

Dinamik bağlantı noktası atama Kestrel ile kullanmak için yalnızca atlayın `Endpoint` ServiceManifest.xml yapılandırmasında tamamen ve bir uç nokta adı geçmeyin `KestrelCommunicationListener` Oluşturucusu:

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

Bu yapılandırmada, `KestrelCommunicationListener` kullanılmayan bir bağlantı noktası uygulama bağlantı noktası aralığından otomatik olarak seçer.

## <a name="scenarios-and-configurations"></a>Senaryolar ve yapılandırmaları
Bu bölümde aşağıdaki senaryoları açıklar ve web sunucusu, bağlantı noktası yapılandırmasını, Service Fabric tümleştirme seçeneklerini ve çeşitli ayarları düzgün çalışan bir hizmet elde etmek için önerilen birleşimi sağlar:
 - ASP.NET Core durum bilgisiz hizmetini dışarıdan kullanıma
 - Yalnızca dahili ASP.NET Core durum bilgisiz hizmeti
 - Yalnızca dahili ASP.NET Core durum bilgisi olan hizmet

Bir **dışarıdan kullanıma sunulan** hizmetidir, genellikle bir yük dengeleyici üzerinden küme dışında erişilebilir bir uç noktasını kullanıma sunar.

Bir **yalnızca iç** hizmetidir biri, uç nokta yalnızca küme içinde erişilebilir.

> [!NOTE]
> Durum bilgisi olan hizmet uç noktaları genellikle Internet'e açık olmamalıdır değil. Yük Dengeleyici bulun ve uygun durum bilgisi olan hizmet çoğaltma trafiği yönlendirmek mümkün olmayacağından durum bilgisi olan hizmetler kullanıma sunmak Service Fabric hizmeti çözümleme, Azure yük dengeleyici gibi algılamadan yük dengeleyici arkasında kümelerinin yükleyemez. 

### <a name="externally-exposed-aspnet-core-stateless-services"></a>ASP.NET Core durum bilgisi olmayan hizmetler dışarıdan kullanıma
Kestrel dış, Internet'e HTTP uç noktalarını kullanıma sunar ön uç Hizmetleri için önerilen web sunucusudur. Windows üzerinde HttpSys; böylece aynı ana bilgisayar adı veya yolu, HTTP yönlendirme sağlamak için bir ön uç proxy veya ağ geçidi kullanmadan Ayrıştırılan aynı bağlantı noktasını kullanarak düğümler kümesi üzerinde birden çok web hizmetlerini barındırmak bağlantı noktası paylaşımı yeteneği sağlamak için kullanılabilir.
 
Internet'e açık olduğunda bir durum bilgisi olmayan hizmetin bir yük dengeleyici üzerinden erişilebilen bir iyi bilinen ve kararlı uç noktası kullanmanız gerekir. Bu, uygulamanızın kullanıcılara sağlayacak URL'dir. Aşağıdaki yapılandırma önerilir:

|  |  | **Notlar** |
| --- | --- | --- |
| Web sunucusu | kestrel | Windows ve Linux desteklenenler kestrel tercih edilen web sunucusudur. |
| Bağlantı noktası yapılandırması | statik | İyi bilinen statik bağlantı noktası olarak yapılandırılmalıdır `Endpoints` ServiceManifest.xml, yapılandırmayı 80 HTTP veya HTTPS için 443 gibi. |
| ServiceFabricIntegrationOptions | Hiçbiri | `ServiceFabricIntegrationOptions.None` Hizmeti, gelen istekler için benzersiz bir kimlik doğrulama girişiminde değil böylece Service Fabric tümleştirme Ara yapılandırırken seçeneği kullanılmalıdır. Dış kullanıcılar, uygulamanızın ara yazılım tarafından kullanılan benzersiz tanımlayıcı bilgiler bilmez. |
| Örnek Sayısı | -1 | Bir örnek, bir yük dengeleyici trafiği almak tüm düğümlere kullanılabilir olmasını sağlamak, ayarı örnek sayısı tipik kullanım durumlarında, "-"1 olarak ayarlanmalıdır. |

Birden çok dışarıdan kullanıma sunulan hizmetler düğümleri aynı kümesi paylaşıyorsanız, HttpSys benzersiz ancak kararlı URL yolu ile kullanılabilir. Bu, IWebHost yapılandırırken sağlanan URL değiştirerek gerçekleştirilebilir. Bu, yalnızca HttpSys için geçerlidir unutmayın.

 ```csharp
 new HttpSysCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     url += "/MyUniqueServicePath";
 
     return new WebHostBuilder()
         .UseHttpSys()
         ...
         .UseUrls(url)
         .Build();
 })
 ```

### <a name="internal-only-stateless-aspnet-core-service"></a>Yalnızca dahili durum bilgisiz ASP.NET Core hizmeti
Yalnızca küme içinde çağrılır durum bilgisi olmayan hizmetler benzersiz URL'leri kullanmanız gerekir ve birden çok hizmetleri arasında işbirliği emin olmak için bağlantı noktalarını dinamik olarak atanır. Aşağıdaki yapılandırma önerilir:

|  |  | **Notlar** |
| --- | --- | --- |
| Web sunucusu | kestrel | HttpSys iç durum bilgisi olmayan hizmetler için kullanılabilir ancak Kestrel bir ana bilgisayar paylaşmak birden fazla hizmet örneği izin vermek için önerilen sunucusudur.  |
| Bağlantı noktası yapılandırması | dinamik olarak atanan | Bir durum bilgisi olan hizmetin birden fazla çoğaltma bir ana bilgisayar işlemi veya ana bilgisayar işletim sistemi paylaşabilir ve bu nedenle benzersiz bağlantı noktaları gerekir. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Dinamik bağlantı noktası atama ile bu ayar daha önce açıklanan hatalı kimlik sorunu önler. |
| Instancecount | herhangi biri | Ayarlama örnek sayısı hizmeti çalıştırmak gerekli olarak herhangi bir değere ayarlanabilir. |

### <a name="internal-only-stateful-aspnet-core-service"></a>Yalnızca dahili durum bilgisi olan ASP.NET Core hizmeti
Yalnızca küme içinde çağrılır durum bilgisi olan hizmetler dinamik olarak atanan bağlantı noktaları, birden çok hizmetleri arasında işbirliği sağlamak için kullanmalısınız. Aşağıdaki yapılandırma önerilir:

|  |  | **Notlar** |
| --- | --- | --- |
| Web sunucusu | kestrel | `HttpSysCommunicationListener` Çoğaltmaları bir ana bilgisayar işlemi paylaşmak durum bilgisi olan hizmet tarafından kullanılmak üzere tasarlanmamıştır. |
| Bağlantı noktası yapılandırması | dinamik olarak atanan | Bir durum bilgisi olan hizmetin birden fazla çoğaltma bir ana bilgisayar işlemi veya ana bilgisayar işletim sistemi paylaşabilir ve bu nedenle benzersiz bağlantı noktaları gerekir. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Dinamik bağlantı noktası atama ile bu ayar daha önce açıklanan hatalı kimlik sorunu önler. |

## <a name="next-steps"></a>Sonraki adımlar
[Service Fabric uygulamanızı Visual Studio kullanarak hata ayıklama](service-fabric-debugging-your-application.md)

<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
