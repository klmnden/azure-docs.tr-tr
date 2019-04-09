---
title: ASP.NET Core ile iletişim hizmeti | Microsoft Docs
description: ASP.NET Core durum bilgisiz ve durum bilgisi olan Reliable Services kullanmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 8aa4668d-cbb6-4225-bd2d-ab5925a868f2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 10/12/2018
ms.author: vturecek
ms.openlocfilehash: 5a4b7514005da9e9a998dba014fa0ea6c014397a
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59268526"
---
# <a name="aspnet-core-in-service-fabric-reliable-services"></a>ASP.NET Core Service Fabric güvenilir hizmetler

ASP.NET Core, modern bulut tabanlı İnternet'e bağlı uygulamalar, web uygulamaları, IOT uygulamaları ve mobil arka uçları gibi oluşturmaya yönelik yeni bir açık kaynaklı ve platformlar arası altyapılardan biridir. 

Bu makale, Service Fabric güvenilir hizmetler kullanarak ASP.NET Core Hizmetleri barındırma için kapsamlı bir kılavuz niteliğindedir **Microsoft.ServiceFabric.AspNetCore.** * NuGet paketlerini kümesi.

Service fabric'te ASP.NET Core üzerinde giriş niteliğindeki bir eğitim ve, geliştirme ortamı Kurulumu alma hakkında yönergeler için bkz. [.NET uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md).

Bu makalenin geri kalanında, zaten ASP.NET Core ile ilgili bilgi sahibi olduğunuz varsayılır. Okumanızı öneririz, varsa [ASP.NET Core Temelleri](https://docs.microsoft.com/aspnet/core/fundamentals/index).

## <a name="aspnet-core-in-the-service-fabric-environment"></a>Service Fabric ortamında ASP.NET Core

Hem ASP.NET Core'un hem de Service Fabric uygulamaları, .NET Framework'ün tamamını yanı sıra .NET Core üzerinde çalıştırabilirsiniz. ASP.NET Core, Service Fabric iki farklı şekillerde kullanılabilir:
 - **Konuk tarafından yürütülebilir bir dosya olarak barındırılan**. Bu, öncelikli olarak mevcut ASP.NET Core uygulamaları kod değişikliği olmadan Service Fabric üzerinde çalıştırmak için kullanılır.
 - **İçinde güvenilir bir hizmet çalıştırma**. Bu, Service Fabric çalışma zamanı ile daha iyi tümleştirme sağlar ve durum bilgisi olan ASP.NET Core hizmetleri sağlar.

Bu makalenin geri kalanında, ASP.NET Core güvenilir bir Service Fabric SDK'sı ile birlikte gelen ASP.NET Core tümleştirme bileşenlerini kullanarak hizmeti içinde kullanmayı açıklar. 

## <a name="service-fabric-service-hosting"></a>Service Fabric hizmeti barındırma

Service Fabric'te, bir veya daha fazla örnekleri ve/veya hizmetinizin çoğaltmaları Çalıştır bir *hizmet ana bilgisayarı işlemi*, hizmet kodunuzun çalıştığı bir yürütülebilir dosya. Hizmet ana bilgisayarı işlemi, bir hizmet yazarı kendi ve Service Fabric etkinleştirir ve sizin yerinize izler.

(En fazla MVC 5) geleneksel ASP.NET System.Web.dll IIS için sıkı şekilde bağlı. ASP.NET Core web sunucusu ve web uygulamanızı arasında bir ayrım sağlar. Bu web uygulamaları, farklı web sunucuları arasında taşınabilmesini sağlar ve ayrıca web sunucularının olması sağlar *şirket içinde barındırılan*, edinebileceğiniz anlamına gelen web sunucusu adanmış web tarafından sahip olunan bir işlemin aksine kendi işleminizde başlayabilirsiniz sunucu yazılımı IIS gibi. 

Bir Service Fabric hizmeti ve Konuk yürütülebilir dosyası olarak veya bir güvenilir hizmet ASP.NET birleştirmek için ASP.NET içinde hizmet ana bilgisayar işlemi başlatabilmek için olmalıdır. ASP.NET Core kendi kendine barındırma, bunu yapmanızı sağlar.

## <a name="hosting-aspnet-core-in-a-reliable-service"></a>ASP.NET Core güvenilir hizmeti barındırma
Genellikle, şirket içinde barındırılan ASP.NET Core uygulamaları bir Web barındırma uygulamanın Giriş noktasında gibi oluşturma `static void Main()` yönteminde `Program.cs`. Bu durumda, yaşam döngüsünü WebHost işlem yaşam döngüsü için bağlıdır.

![Bir işlemde ASP.NET Core barındırma][0]

Bu hizmet türünün örneklerini oluşturabilir, böylece uygulama giriş noktası yalnızca bir hizmet türü Service Fabric çalışma zamanı ile kaydetmek için kullanılır çünkü ancak uygulama giriş noktası bir WebHost güvenilir bir hizmet oluşturmak için doğru yer değildir. WebHost kendisi bir güvenilir hizmet olarak oluşturulmalıdır. Hizmet barındırma işlemi içinde birden çok yaşam döngüleri hizmet örnekleri ve/veya çoğaltmaları gidebilirsiniz. 

Bir güvenilir hizmet örneği öğesinden türetme, hizmet sınıfı tarafından temsil edilen `StatelessService` veya `StatefulService`. Bir hizmet için iletişim yığını bulunan bir `ICommunicationListener` uygulamasında, hizmet sınıfı. `Microsoft.ServiceFabric.AspNetCore.*` NuGet paketlerini içeren uygulamaları `ICommunicationListener` başlatın ve ASP.NET Core WebHost Kestrel ya da güvenilir bir hizmet içinde HttpSys yönetin.

![ASP.NET Core güvenilir hizmeti barındırma][1]

## <a name="aspnet-core-icommunicationlisteners"></a>ASP.NET Core ICommunicationListeners
`ICommunicationListener` Kestrel ve içinde HttpSys uygulamaları `Microsoft.ServiceFabric.AspNetCore.*` NuGet paketlerini benzer kullanım desenleri sahip ancak her bir web sunucusuna belirli biraz farklı eylemleri gerçekleştirin. 

Her iki iletişim dinleyicileri aşağıdaki bağımsız değişken alan bir oluşturucu sağlar:
 - **`ServiceContext serviceContext`**: `ServiceContext` Çalışan hizmeti hakkında bilgi içeren nesne.
 - **`string endpointName`**: adını bir `Endpoint` ServiceManifest.xml yapılandırması. Öncelikle, burada iki iletişim dinleyicileri farklı budur: HttpSys **gerektirir** bir `Endpoint` Kestrel çalışmazken yapılandırma.
 - **`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: bir lambda, oluşturduğunuz ve dönüş uygulayan bir `IWebHost`. Bu sayede yapılandırmak `IWebHost` bir ASP.NET Core uygulaması normalde olduğu gibi. Service Fabric tümleştirmesi bağlı olarak, seçenekleri için oluşturulan bir URL kullanın, lambda sağlar ve `Endpoint` sağladığınız yapılandırma. URL daha sonra değiştirilebilir veya olarak kullanılan,-web sunucusu başlamaktır.

## <a name="service-fabric-integration-middleware"></a>Service Fabric tümleştirme ara yazılımı
`Microsoft.ServiceFabric.AspNetCore` NuGet paketini içeren `UseServiceFabricIntegration` genişletme yöntemini `IWebHostBuilder` , Service Fabric algılayan ara yazılımı ekler. Bu ara yazılımın Kestrel veya HttpSys yapılandırır `ICommunicationListener` benzersiz bir hizmet URL'si Service Fabric adlandırma hizmetine kaydetmek için ve ardından istemcileri için doğru hizmet bağlama emin olmak için istemci istekleri doğrular. Bu, birden çok web uygulamaları burada aynı fiziksel veya sanal makinede çalıştırabilirsiniz ancak istemciler yanlışlıkla yanlış hizmete bağlanmasını önlemek için benzersiz bir ana bilgisayar adları kullanmayın, Service Fabric gibi paylaşılan konak ortamında gereklidir. Bu senaryo, sonraki bölümde daha ayrıntılı açıklanmıştır.

### <a name="a-case-of-mistaken-identity"></a>Hatalı kimliğinin servis talebi
Protokol, bağımsız olarak, hizmet çoğaltmaları üzerindeki bir benzersiz IP: BağlantıNoktası birleşimini dinleyin. Bir hizmet çoğaltması, bir IP: BağlantıNoktası uç noktası üzerinde dinleme başlatıldıktan sonra Service Fabric adlandırma Burada, istemcileri veya diğer hizmetler tarafından bulunabileceğini hizmeti için bu uç nokta adresi bildirir. Hizmetleri dinamik olarak atanan uygulama bağlantı noktası kullanıyorsanız, bir hizmet çoğaltması tesadüfen aynı fiziksel veya sanal makine üzerinde önceden başka bir hizmet aynı IP: BağlantıNoktası uç noktasına kullanabilir. Bu, yanlışlıkla yanlış hizmetine bağlanmak bir istemci neden olabilir. Aşağıdaki olaylar dizisi gerçekleşir. Bu durum ortaya çıkabilir:

 1. A hizmeti üzerinde 10.0.0.1:30000 HTTP üzerinden dinler. 
 2. İstemci hizmet A giderir ve adresi 10.0.0.1:30000 alır
 3. Bir hizmet farklı bir düğüme taşır.
 4. B hizmeti üzerinde 10.0.0.1 yerleştirilir ve tesadüfen 30000 aynı bağlantı noktasını kullanır.
 5. İstemci, hizmeti bir önbelleğe alınan adresi 10.0.0.1:30000 ile bağlanmayı dener.
 6. İstemci artık taahhüdünü gerçekleştirmeye değil B yanlış hizmetine bağlı hizmetine başarıyla bağlandı.

Bu hataların tanı koymak güç olabilir rastgele zamanlarda neden olabilir. 

### <a name="using-unique-service-urls"></a>Benzersiz bir hizmet URL'leri kullanma
Bunu önlemek için hizmetleri benzersiz bir tanımlayıcı adlandırma hizmetine bir uç nokta gönderin ve sonra benzersiz tanımlayıcı istemci istekleri sırasında doğrulayın. Bu, güvenilir bir saldırgan-Kiracı olmayan ortamda hizmetler arasında işbirliği yapan bir işlemdir. Bu tehlikeli müşterili bir ortamda güvenli hizmet kimlik doğrulaması sağlamaz.

Ara yazılım tarafından eklenen güvenilir bir ortamda `UseServiceFabricIntegration` yöntem otomatik olarak ekler benzersiz bir tanımlayıcı adlandırma hizmete gönderilir ve bu tanımlayıcının her isteği doğrular adresi. Tanımlayıcı eşleşmiyorsa, ara yazılım, hemen bir HTTP 410 kaybedildi yanıtı döndürür.

Hizmetleri kullanan bir dinamik olarak atanan bağlantı yapmanız gerekir bu ara yazılımın kullanın.

Sabit bir benzersiz bağlantı noktası kullanan Hizmetleri ortak bir ortamda bu sorun yoktur. Sabit bir benzersiz bağlantı noktası genellikle harici olarak bilinen bir bağlantı noktasına bağlanmak istemci uygulamaları için gereken hizmetleriniz karşılaştığı için kullanılır. Örneğin, çoğu Internet'e yönelik web uygulamaları, web tarayıcısı bağlantıları için 80 veya 443 bağlantı noktası kullanır. Bu durumda, benzersiz tanımlayıcı etkinleştirilmemelidir.

Aşağıdaki diyagramda, etkin ile Ara yazılım istek akışı gösterilmektedir:

![Service Fabric ASP.NET Core tümleştirme][2]

Kestrel'i hem HttpSys `ICommunicationListener` uygulamaları, tam olarak aynı şekilde Bu mekanizma kullanır. HttpSys dahili olarak kullanarak temel alınan benzersiz URL yollarına göre istekleri ayırt edebilirsiniz ancak *http.sys* işlevselliği özelliği, bağlantı noktası *değil* HttpSys tarafından kullanılan `ICommunicationListener` Uygulama, daha önce açıklanan senaryoda HTTP 503 ve HTTP 404 hatası durum kodları neden. HTTP 503 ve HTTP 404 zaten genellikle diğer hataları belirtmek için kullanıldığından, sırayla hata amacı belirlemek istemcileri zorlaştırır. Bu nedenle, hem Kestrel ve HttpSys `ICommunicationListener` uygulamaları standartlaştırmak ara yazılımı tarafından sağlanan üzerinde `UseServiceFabricIntegration` genişletme yöntemi ve böylece istemciler yalnızca yapmanız gereken bir hizmet uç noktası yeniden çözümleyebilir HTTP 410 yanıt eylemi.

## <a name="httpsys-in-reliable-services"></a>Reliable Services özelliğinde HttpSys
HttpSys kullanılabilir bir güvenilir hizmet olarak içeri aktararak **Microsoft.ServiceFabric.AspNetCore.HttpSys** NuGet paketi. Bu pakette `HttpSysCommunicationListener`, uygulaması `ICommunicationListener`, bu sayede güvenilir bir hizmet içinde bir ASP.NET Core Web barındırma oluşturmak HttpSys kullanarak web sunucusu olarak.

HttpSys üzerine kurulmuştur [Windows HTTP sunucu API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx). Bu kullanır *http.sys* HTTP isteklerini işleyen ve bunları web uygulamaları çalışan işlemler için yönlendirmek için IIS tarafından kullanılan çekirdek sürücüsü. Bu, aynı fiziksel veya sanal makinenize benzersiz bir URL yolu veya ana bilgisayar adı tarafından disambiguated web uygulamalarını aynı bağlantı noktasında birden çok işlem sağlar. Bu özellikleri aynı kümenin birden fazla Web sitesi barındırmak için Service Fabric yararlıdır.

>[!NOTE]
>HttpSys uygulama yalnızca Windows platform üzerinde çalışır.

Aşağıdaki diyagram HttpSys nasıl kullandığını gösterir *http.sys* bağlantı noktası paylaşımı için çekirdek sürücüsü Windows üzerinde:

![HTTP.sys][3]

### <a name="httpsys-in-a-stateless-service"></a>Durum bilgisi olmayan hizmetin içinde HttpSys
Kullanılacak `HttpSys` bir durum bilgisi olmayan hizmet içinde geçersiz kılma `CreateServiceInstanceListeners` yöntemi ve dönüş bir `HttpSysCommunicationListener` örneği:

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

### <a name="httpsys-in-a-stateful-service"></a>Durum bilgisi olan hizmet içinde HttpSys

`HttpSysCommunicationListener` şu anda durum bilgisi olan hizmetler ile arka plandaki zorluklar nedeniyle kullanım için tasarlanmamış *http.sys* bağlantı noktası özelliği. Daha fazla bilgi için aşağıdaki bölümde dinamik bağlantı noktası ayırma ile HttpSys bakın. Durum bilgisi olan hizmetler için Kestrel önerilen web sunucusudur.

### <a name="endpoint-configuration"></a>Uç nokta yapılandırması

Bir `Endpoint` Windows HTTP sunucu HttpSys dahil olmak üzere API kullanan web sunucuları için yapılandırma gerekiyor. Windows HTTP sunucu API'si kullanan web sunucuları, URL ile ilk yedek gerekir *http.sys* (Bu normalde ile gerçekleştirilir [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) aracı). Bu eylem, Hizmetleri varsayılan olmayan yükseltilmiş ayrıcalıklar gerektiriyor. "Http" veya "https" seçeneklerini `Protocol` özelliği `Endpoint` yapılandırmasında *ServiceManifest.xml* özel bir URL ile kaydetmek için Service Fabric çalışma zamanı istemek için kullanılan  *HTTP.sys* adına kullanma [ *güçlü bir joker karakter* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL ön eki.

Örneğin, ayrılacak `http://+:80` bir hizmet için şu yapılandırmayı ServiceManifest.xml kullanılmalıdır:

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

Ve uç nokta adı geçirilmelidir `HttpSysCommunicationListener` Oluşturucusu:

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

#### <a name="use-httpsys-with-a-static-port"></a>Statik bir bağlantı noktası Httpsys'de kullanın
Httpsys'de statik bağlantı noktasını kullanmak için kullanılan bağlantı noktası numarasını sağlayın `Endpoint` yapılandırma:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-httpsys-with-a-dynamic-port"></a>Dinamik bir bağlantı noktası Httpsys'de kullanın
Dinamik olarak atanan bir bağlantı noktası Httpsys'de kullanmak için atla `Port` özelliğinde `Endpoint` yapılandırma:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

Bir dinamik bağlantı noktası tarafından ayrılan bir `Endpoint` yapılandırma yalnızca bir bağlantı noktası sağlar *ana bilgisayar işlemi başına*. Geçerli Service Fabric barındırma modeli birden çok hizmet örnekleri ve/veya her biri aynı bağlantı noktası üzerinden ayrılmış paylaşır, yani aynı işlemde barındırılan çoğaltmalar sağlayan `Endpoint` yapılandırma. Birden çok HttpSys örneği kullanarak temel alınan bir bağlantı noktası paylaşabilirsiniz *http.sys* özellik, ancak bu bağlantı noktası tarafından desteklenmiyor `HttpSysCommunicationListener` için istemci isteklerini tanıttığı zorluklar nedeniyle. Dinamik bağlantı noktası kullanımı için Kestrel önerilen web sunucusudur.

## <a name="kestrel-in-reliable-services"></a>Reliable Services özelliğinde kestrel
Kestrel'i kullanılabilir bir güvenilir hizmet olarak içeri aktararak **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet paketi. Bu pakette `KestrelCommunicationListener`, uygulaması `ICommunicationListener`, izin veren bir ASP.NET Core Web barındırma güvenilir bir hizmet içinde oluşturmanızı Kestrel kullanarak web sunucusu olarak.

Kestrel'i ASP.NET Core platformlar arası web sunucusuna libuv, platformlar arası bir zaman uyumsuz g/ç kitaplığı dayalıdır. HttpSys Kestrel merkezi uç nokta Yöneticisi gibi kullanmaz *http.sys*. Ve HttpSys Kestrel birden çok işlemler arasında bağlantı noktası paylaşımı desteklemez. Kestrel'i her örneğini benzersiz bir bağlantı noktası kullanmanız gerekir.

![Kestrel'i][4]

### <a name="kestrel-in-a-stateless-service"></a>Durum bilgisi olmayan hizmetin içinde kestrel
Kullanılacak `Kestrel` bir durum bilgisi olmayan hizmet içinde geçersiz kılma `CreateServiceInstanceListeners` yöntemi ve dönüş bir `KestrelCommunicationListener` örneği:

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

### <a name="kestrel-in-a-stateful-service"></a>Durum bilgisi olan hizmet içinde kestrel
Kullanılacak `Kestrel` bir durum bilgisi olan hizmet geçersiz kılma `CreateServiceReplicaListeners` yöntemi ve dönüş bir `KestrelCommunicationListener` örneği:

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

Bu örnekte, tekil örneğini `IReliableStateManager` WebHost bağımlılık ekleme kapsayıcısına sağlanır. Bu kesinlikle gerekli değildir, ancak kullanmanıza olanak tanır `IReliableStateManager` ve güvenilir koleksiyonlar, MVC denetleyici eylem yöntemleri.

Bir `Endpoint` yapılandırma adı **değil** sağlanan `KestrelCommunicationListener` durum bilgisi olan hizmet içinde. Bu, aşağıdaki bölümde daha ayrıntılı açıklanmıştır.

### <a name="configure-kestrel-to-use-https"></a>Kestrel’i HTTPS kullanacak şekilde yapılandırma
Kestrel'i HTTPS hizmetinizde etkinleştirirken, birkaç dinleme seçeneklerini ayarlamak gerekir.  Güncelleştirme `ServiceInstanceListener` EndpointHttps uç nokta ve (örneğin, bağlantı noktası 443) belirli bir bağlantı noktasını dinler. Web ana bilgisayarı Kestrel sunucusu kullanacak şekilde yapılandırırken, IPv6 adresleri tüm ağ arabirimleri üzerinde dinlenecek Kestrel yapılandırmanız gerekir: 

```csharp
new ServiceInstanceListener(
serviceContext =>
    new KestrelCommunicationListener(
        serviceContext,
        "EndpointHttps",
        (url, listener) =>
        {
            ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting Kestrel on {url}");

            return new WebHostBuilder()
                .UseKestrel(opt =>
                {
                    int port = serviceContext.CodePackageActivationContext.GetEndpoint("EndpointHttps").Port;
                    opt.Listen(IPAddress.IPv6Any, port, listenOptions =>
                    {
                        listenOptions.UseHttps(GetCertificateFromStore());
                        listenOptions.NoDelay = true;
                    });
                })
                .ConfigureAppConfiguration((builderContext, config) =>
                {
                    config.AddJsonFile("appsettings.json", optional: false, reloadOnChange: true);
                })

                .ConfigureServices(
                    services => services
                        .AddSingleton<HttpClient>(new HttpClient())
                        .AddSingleton<FabricClient>(new FabricClient())
                        .AddSingleton<StatelessServiceContext>(serviceContext))
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseStartup<Startup>()
                .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                .UseUrls(url)
                .Build();
        }))
```

Bir öğreticide kullanılan tam bir örnek için bkz: [Kestrel'i HTTPS kullanacak şekilde yapılandırma](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#configure-kestrel-to-use-https).


### <a name="endpoint-configuration"></a>Uç nokta yapılandırması
Bir `Endpoint` yapılandırma Kestrel kullanmak için gerekli değildir. 

Kestrel'i basit tek başına web sunucusunun adıdır; HttpSys (veya HttpListener) aksine, bu ihtiyaç duymadığı bir `Endpoint` yapılandırmasında *ServiceManifest.xml* başlatmadan önce URL kayıt gerektirmediği için. 

#### <a name="use-kestrel-with-a-static-port"></a>Kestrel'i statik bir bağlantı noktası ile kullanma
Statik bir bağlantı noktası yapılandırılabilir `Endpoint` Kestrel ile kullanmak için ServiceManifest.xml yapılandırması. Bu kesinlikle gerekli olmamakla birlikte, iki olası faydaları sağlar:
 1. Bağlantı noktası uygulama bağlantı noktası aralığı içinde kalmıyorsa, Service Fabric tarafından işletim sistemi güvenlik duvarı üzerinden açıldı.
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

Varsa bir `Endpoint` yapılandırma kullanılmazsa, adın atlamak `KestrelCommunicationListener` Oluşturucusu. Bu durumda, dinamik bir bağlantı noktası kullanılır. Daha fazla bilgi için sonraki bölüme bakın.

#### <a name="use-kestrel-with-a-dynamic-port"></a>Dinamik bir bağlantı noktasıyla Kestrel kullanın
Kestrel'i otomatik olarak bağlantı noktası atamadan kullanamaz `Endpoint` ServiceManifest.xml, yapılandırmada olduğundan otomatik olarak bağlantı noktası atamadan bir `Endpoint` başına benzersiz bir bağlantı noktası yapılandırması atar *ana işlem*, ve bir tek ana işlem Kestrel birden fazla içerebilir. Bağlantı noktası paylaşımı Kestrel desteklemediğinden, her Kestrel örneği benzersiz bir bağlantı noktasında açılmalıdır gibi bu çalışmıyor.

Dinamik bağlantı noktası atama Kestrel ile kullanmak için atla `Endpoint` ServiceManifest.xml yapılandırmasında tamamen ve bir uç nokta adına geçmeyin `KestrelCommunicationListener` Oluşturucusu:

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

Bu yapılandırmada, `KestrelCommunicationListener` kullanılmayan bir bağlantı noktası uygulama bağlantı noktası aralığından otomatik olarak seçer.

## <a name="service-fabric-configuration-provider"></a>Service Fabric yapılandırma sağlayıcısı
ASP.NET Core uygulaması yapılandırmasında okuma yapılandırma sağlayıcıları tarafından oluşturulan anahtar-değer çiftleri temel [ASP.NET Core yapılandırmasında](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/) daha fazla açık genel ASP.NET Core yapılandırma desteği anlamak için.

Bu bölümde, ASP.NET Core yapılandırmasıyla içeri aktararak tümleştirmek için Service Fabric yapılandırma sağlayıcısı açıklanmaktadır `Microsoft.ServiceFabric.AspNetCore.Configuration` NuGet paketi.

### <a name="addservicefabricconfiguration-startup-extensions"></a>AddServiceFabricConfiguration başlangıç uzantıları
İçeri aktarma sonra `Microsoft.ServiceFabric.AspNetCore.Configuration` NuGet paketi gerekir ASP.NET Core yapılandırma API'si tarafından Service Fabric yapılandırma kaynağı kaydetmek **AddServiceFabricConfiguration** uzantılarında `Microsoft.ServiceFabric.AspNetCore.Configuration` ad alanı karşı `IConfigurationBuilder`

```csharp
using Microsoft.ServiceFabric.AspNetCore.Configuration;

public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(env.ContentRootPath)
        .AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true)
        .AddServiceFabricConfiguration() // Add Service Fabric configuration settings.
        .AddEnvironmentVariables();
    Configuration = builder.Build();
}

public IConfigurationRoot Configuration { get; }
```

ASP.NET Core hizmeti, Service Fabric yapılandırma ayarları gibi herhangi bir uygulama ayarı artık erişebilirsiniz. Örneğin, türü kesin belirlenmiş nesnelerini ayarlarını yüklemek için seçenekleri desenini kullanabilirsiniz.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<MyOptions>(Configuration);  // Strongly typed configuration object.
    services.AddMvc();
}
```
### <a name="default-key-mapping"></a>Varsayılan anahtar eşleme
Varsayılan olarak, Service Fabric yapılandırma sağlayıcısı, paket adı, bölüm adı ve asp.net core yapılandırması birlikte oluşturmak için özellik adını içerir. işlevini kullanarak anahtar:
```csharp
$"{this.PackageName}{ConfigurationPath.KeyDelimiter}{section.Name}{ConfigurationPath.KeyDelimiter}{property.Name}"
```

Örneğin, adlı bir yapılandırma paketleri varsa `MyConfigPackage` altındaki içerik, ardından yapılandırma değeri üzerinde ASP.NET Core kullanıma sunulacaktır `IConfiguration` anahtarı aracılığıyla *MyConfigPackage:MyConfigSection:MyParameter*
```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">  
  <Section Name="MyConfigSection">
    <Parameter Name="MyParameter" Value="Value1" />
  </Section>  
</Settings>
```
### <a name="service-fabric-configuration-options"></a>Service Fabric yapılandırma seçenekleri
Aynı zamanda Service Fabric yapılandırma sağlayıcısını destekliyor `ServiceFabricConfigurationOptions` tuş eşlemeyi varsayılan davranışını değiştirmek için.

#### <a name="encrypted-settings"></a>Şifrelenmiş ayarları
Ayarları şifrelemek için Service Fabric destekler, bu Service Fabric yapılandırma sağlayıcısı da destekler. Varsayılan ilke, varsayılan ASP.NET core'a şifreli ayarlarla are't descrypted tarafından güvenli izlemek için `IConfiguration`, şifrelenmiş değer depolanır var. Bunun yerine. Ancak, ASP.NET Core IConfiguration içinde depolamak için değerin şifresi istiyorsanız False olarak DecryptValue bayrağı ayarlayabilirsiniz `AddServiceFabricConfiguration` aşağıdaki gibi uzantıları:

```csharp
public Startup()
{
    ICodePackageActivationContext activationContext = FabricRuntime.GetActivationContext();
    var builder = new ConfigurationBuilder()        
        .AddServiceFabricConfiguration(activationContext, (options) => options.DecryptValue = true); // set flag to decrypt the value
    Configuration = builder.Build();
}
```
#### <a name="multiple-configuration-packages"></a>Birden çok yapılandırma paketleri
Service Fabric, birden çok yapılandırma paketlerini destekler. Varsayılan olarak, yapılandırmanın anahtar paket adı dahil edilir. Ayarlayabilirsiniz `IncludePackageName` varsayılan davranışı değiştirmek için bayrak.
```csharp
public Startup()
{
    ICodePackageActivationContext activationContext = FabricRuntime.GetActivationContext();
    var builder = new ConfigurationBuilder()        
        // exclude package name from key.
        .AddServiceFabricConfiguration(activationContext, (options) => options.IncludePackageName = false); 
    Configuration = builder.Build();
}
```
#### <a name="custom-key-mapping-value-extraction-and-data-population"></a>Özel anahtar eşleme, değer ayıklama ve veri doldurma
Bunun yanında, varsayılan davranışı değiştirmek için 2 bayrakları da Service Fabric yapılandırma sağlayıcısını destekliyor daha özel anahtar eşleme aracılığıyla Gelişmiş senaryoları `ExtractKeyFunc` ve özel değerler aracılığıyla ayıklamak `ExtractValueFunc`. ASP.NET Core yapılandırma ile Service Fabric yapılandırma verilerini doldurmak için bu işlem bile değişebilir `ConfigAction`.

Aşağıdaki örnekler, kullanılacak gösterir `ConfigAction` veri doldurma özelleştirmek için.
```csharp
public Startup()
{
    ICodePackageActivationContext activationContext = FabricRuntime.GetActivationContext();
    
    this.valueCount = 0;
    this.sectionCount = 0;
    var builder = new ConfigurationBuilder();
    builder.AddServiceFabricConfiguration(activationContext, (options) =>
        {
            options.ConfigAction = (package, configData) =>
            {
                ILogger logger = new ConsoleLogger("Test", null, false);
                logger.LogInformation($"Config Update for package {package.Path} started");

                foreach (var section in package.Settings.Sections)
                {
                    this.sectionCount++;

                    foreach (var param in section.Parameters)
                    {
                        configData[options.ExtractKeyFunc(section, param)] = options.ExtractValueFunc(section, param);
                        this.valueCount++;
                    }
                }

                logger.LogInformation($"Config Update for package {package.Path} finished");
            };
        });
  Configuration = builder.Build();
}
```
### <a name="configuration-update"></a>Yapılandırma güncelleştirmesi
Service Fabric yapılandırma sağlayıcısı yapılandırmasını güncelleştirme de destekler ve ASP.NET Core kullanabileceğinizi `IOptionsMonitor` değişiklik bildirimleri almak için hem de `IOptionsSnapshot` yapılandırma verilerini yeniden yüklemek için. Daha fazla bilgi için [ASP.NET Core seçenekleri](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options).

Bu varsayılan olarak desteklenir ve başka kodlama yapılandırma güncelleştirmesi etkinleştirmek için gereklidir.

## <a name="scenarios-and-configurations"></a>Senaryolar ve yapılandırmaları
Bu bölümde, aşağıdaki senaryoları açıklar ve web sunucusu, bağlantı noktası yapılandırması, Service Fabric tümleştirme seçenekleri ve çeşitli ayarları düzgün çalışan bir hizmet elde etmek için önerilen birleşimi sağlar:
 - Durum bilgisi olmayan ASP.NET Core hizmeti harici olarak sunulan
 - Yalnızca iç durum bilgisi olmayan hizmet ASP.NET Core
 - Yalnızca iç durum bilgisi olan hizmet ASP.NET Core

Bir **harici olarak sunulan** hizmetidir, genellikle yük dengeleyici aracılığıyla küme dışında erişilebilir bir uç noktasını kullanıma sunar.

Bir **yalnızca dahili** hizmetidir biri olan uç nokta yalnızca küme içinde erişilebilir.

> [!NOTE]
> Durum bilgisi olan hizmet uç noktaları, Internet'e genellikle sunulmamalıdır. Service Fabric hizmeti çözüm, Azure Load Balancer gibi simgelerinde yük dengeleyicilerin ardındaki kümelerinin yük dengeleyici bulup uygun trafiği yönlendirmek mümkün olmayacağı için durum bilgisi olan hizmetler kullanıma sunmak mümkün olmayacaktır. durum bilgisi olan hizmet çoğaltma. 

### <a name="externally-exposed-aspnet-core-stateless-services"></a>Durum bilgisi olmayan ASP.NET Core Hizmetleri harici olarak sunulan
Kestrel'i harici, Internet'e yönelik HTTP uç noktalarını kullanıma ön uç Hizmetleri için önerilen bir web sunucusudur. Windows üzerinde birden çok web hizmetleri aynı ana bilgisayar adı veya yolu, HTTP yönlendirme sağlamak için ön uç proxy veya ağ geçidi üzerinde bağlı olmadan Ayrıştırılan aynı bağlantı noktasını kullanarak düğümleri kümesini barındırmak izin veren bir bağlantı noktası paylaşma olanağı sağlayan HttpSys kullanılabilir.
 
Internet'e açık olduğunda, durum bilgisi olmayan hizmet bir yük dengeleyici üzerinden erişilebilir olan bilinen ve kararlı bir uç kullanmanız gerekir. Bu, uygulamanızın kullanıcılara sağlayacak URL'dir. Aşağıdaki yapılandırmayı öneririz:

|  |  | **Notlar** |
| --- | --- | --- |
| Web sunucusu | Kestrel'i | Windows ve Linux'ta desteklenen kestrel tercih edilen web sunucusudur. |
| Bağlantı noktası yapılandırması | statik | İyi bilinen bir statik bağlantı noktası olarak yapılandırılmalıdır `Endpoints` yapılandırmasını ServiceManifest.xml, örneğin HTTP için 80 veya HTTPS için 443'tür. |
| ServiceFabricIntegrationOptions | None | `ServiceFabricIntegrationOptions.None` Benzersiz bir tanımlayıcı için gelen istekleri doğrulamak hizmet çalışmaz, Service Fabric tümleştirme ara yazılımı yapılandırırken seçeneği kullanılmalıdır. Uygulamanızın dış kullanıcıları ara yazılım tarafından kullanılan benzersiz tanımlayıcı bilgiler oynatacaklarını bilmez. |
| Örnek Sayısı | -1 | Böylece yük dengeleyiciden trafiği alması tüm düğümlerde örneği kullanılabilir tipik kullanım durumlarında ayarı örnek sayısı "-1" olarak ayarlanması gerekir. |

Birden çok harici olarak kullanıma sunulan hizmetlere aynı dizi düğümü paylaşıyorsanız, HttpSys benzersiz ancak kararlı bir URL yolu ile kullanılabilir. Bu, IWebHost yapılandırılırken belirtilen URL değiştirerek gerçekleştirilebilir. Bunun için HttpSys yalnızca geçerli olmadığını unutmayın.

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

### <a name="internal-only-stateless-aspnet-core-service"></a>Yalnızca iç durum bilgisi olmayan ASP.NET Core hizmeti
Yalnızca gelen küme içinde çağrılan durum bilgisi olmayan hizmetler ve birden çok hizmet arasında işbirliği sağlamak için bağlantı noktalarını dinamik olarak atanan benzersiz URL'ler kullanmalısınız. Aşağıdaki yapılandırmayı öneririz:

|  |  | **Notlar** |
| --- | --- | --- |
| Web sunucusu | Kestrel'i | HttpSys iç durum bilgisi olmayan hizmetler için kullanılabilir ancak Kestrel bir konak paylaşmak birden çok hizmeti örneği izin vermek için önerilen sunucusudur.  |
| Bağlantı noktası yapılandırması | dinamik olarak atanan | Durum bilgisi olan bir hizmet birden çok kopyasını, ana bilgisayar işlemi veya ana bilgisayar işletim sistemi paylaşabilir ve bu nedenle benzersiz bağlantı noktası gerekir. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Dinamik bağlantı noktası atama ile bu ayar, daha önce açıklanan hatalı kimlik sorununu önler. |
| Instancecount | herhangi biri | Ayarlama örneği sayısı hizmeti çalıştırmak gerekli olarak herhangi bir değere ayarlanabilir. |

### <a name="internal-only-stateful-aspnet-core-service"></a>Yalnızca iç durum bilgisi olan ASP.NET Core hizmeti
Yalnızca gelen küme içinde çağrılan durum bilgisi olan hizmetler, birden çok hizmet arasında işbirliği sağlamak için dinamik olarak atanan bağlantı noktası kullanmanız gerekir. Aşağıdaki yapılandırmayı öneririz:

|  |  | **Notlar** |
| --- | --- | --- |
| Web sunucusu | Kestrel'i | `HttpSysCommunicationListener` Çoğaltmalar bir ana bilgisayar işlemi paylaşmak durum bilgisi olan hizmetler tarafından kullanılmak üzere tasarlanmamıştır. |
| Bağlantı noktası yapılandırması | dinamik olarak atanan | Durum bilgisi olan bir hizmet birden çok kopyasını, ana bilgisayar işlemi veya ana bilgisayar işletim sistemi paylaşabilir ve bu nedenle benzersiz bağlantı noktası gerekir. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Dinamik bağlantı noktası atama ile bu ayar, daha önce açıklanan hatalı kimlik sorununu önler. |

## <a name="next-steps"></a>Sonraki adımlar
[Visual Studio kullanarak Service Fabric uygulamanızı hata ayıklama](service-fabric-debugging-your-application.md)

<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
