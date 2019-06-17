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
ms.openlocfilehash: 638c06e1854504dcb7ff34b1d9df56694556c421
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64939787"
---
# <a name="aspnet-core-in-azure-service-fabric-reliable-services"></a>ASP.NET Core, Azure Service Fabric güvenilir hizmetler

ASP.NET Core açık kaynaklı ve platformlar arası bir çerçevedir. Mobil arka uçlar ve bu çerçeve, web uygulamaları, IOT uygulamaları gibi bulut tabanlı, internet bağlantılı uygulamalar oluşturmak için tasarlanmıştır.

Bu makale kullanarak Service Fabric güvenilir hizmetler ASP.NET Core hizmetlerde barındırılması için kapsamlı bir kılavuz niteliğindedir **Microsoft.ServiceFabric.AspNetCore.** NuGet paketlerini kümesi.

Giriş niteliğindeki bir eğitim üzerinde Service fabric'te ASP.NET Core ve geliştirme ortamınızı ayarlayın alma hakkında yönergeler için bkz [Öğreticisi: Bir ASP.NET Core Web API'si ön uç hizmeti ve durum bilgisi olan bir arka uç hizmeti ile bir uygulama oluşturup](service-fabric-tutorial-create-dotnet-app.md).

Bu makalenin geri kalanında, zaten ASP.NET Core ile ilgili bilgi sahibi olduğunuz kabul edilmektedir. Aksi takdirde, lütfen okumak [ASP.NET Core Temelleri](https://docs.microsoft.com/aspnet/core/fundamentals/index).

## <a name="aspnet-core-in-the-service-fabric-environment"></a>Service Fabric ortamında ASP.NET Core

Hem ASP.NET Core'un hem de Service Fabric uygulamaları, .NET Core veya .NET Framework'ün tamamını çalıştırabilirsiniz. İki farklı şekilde Service fabric'te ASP.NET Core kullanabilirsiniz:
 - **Konuk tarafından yürütülebilir bir dosya olarak barındırılan**. Bu şekilde, öncelikle varolan ASP.NET Core uygulamaları kod değişikliği olmadan Service Fabric üzerinde çalıştırmak için kullanılır.
 - **İçinde güvenilir bir hizmet çalıştırma**. Bu şekilde, Service Fabric çalışma zamanı ile daha iyi tümleştirme sağlar ve durum bilgisi olan ASP.NET Core hizmetleri sağlar.

Bu makalenin geri kalanında, Service Fabric SDK'sı ile birlikte gelen ASP.NET Core tümleştirme bileşenleri arasında güvenilir bir hizmet içinde ASP.NET Core kullanmayı açıklar.

## <a name="service-fabric-service-hosting"></a>Service Fabric hizmeti barındırma

Service Fabric'te, bir veya daha fazla örnekleri ve/veya hizmetinizin çoğaltmaları Çalıştır bir *hizmet ana bilgisayarı işlemi*: hizmet kodunuzun çalıştığı bir yürütülebilir dosya. Hizmet ana bilgisayarı işlemi, bir hizmet yazarı kendi ve Service Fabric etkinleştirir ve sizin yerinize izler.

(En fazla MVC 5) geleneksel ASP.NET System.Web.dll IIS için sıkı şekilde bağlı. ASP.NET Core web sunucusu ve web uygulamanızı arasında bir ayrım sağlar. Bu ayrım, web uygulamaları, farklı web sunucuları arasında taşınabilmesini sağlar. Ayrıca web sunucuları olmasını sağlar *şirket içinde barındırılan*. Başka bir deyişle, bir web sunucusu IIS gibi özel web sunucusu yazılımı tarafından sahip olunan bir işlemin aksine kendi işleminizde başlayabilirsiniz.

Bir Service Fabric hizmeti ve Konuk yürütülebilir dosyası olarak veya bir güvenilir hizmet ASP.NET birleştirmek için ASP.NET içinde hizmet ana bilgisayar işlemi başlatabilmek için olmalıdır. ASP.NET Core kendi kendine barındırma, bunu yapmanızı sağlar.

## <a name="hosting-aspnet-core-in-a-reliable-service"></a>ASP.NET Core güvenilir bir hizmet barındırma
Genellikle, şirket içinde barındırılan ASP.NET Core uygulamaları bir Web barındırma uygulamanın Giriş noktasında gibi oluşturma `static void Main()` yönteminde `Program.cs`. Bu durumda, WebHost yaşam döngüsünü işlem yaşam döngüsünü bağlıdır.

![Bir işlemde ASP.NET Core barındırma][0]

Ancak uygulama giriş noktası bir WebHost içinde güvenilir bir hizmet oluşturmak için doğru yer değildir. Bu hizmet türünün örneklerini oluşturabilmesi uygulama giriş noktası yalnızca bir hizmet türü Service Fabric çalışma zamanı ile kaydetmek için kullanılan olmasıdır. WebHost güvenilir hizmet kendisini oluşturulmalıdır. Hizmet barındırma işlemi içinde birden çok yaşam döngüleri hizmet örnekleri ve/veya çoğaltmaları gidebilirsiniz. 

Bir güvenilir hizmet örneği öğesinden türetme, hizmet sınıfı tarafından temsil edilen `StatelessService` veya `StatefulService`. Bir hizmet için iletişim yığını bulunan bir `ICommunicationListener` uygulamasında, hizmet sınıfı. `Microsoft.ServiceFabric.AspNetCore.*` NuGet paketlerini içeren uygulamaları `ICommunicationListener` başlatın ve ASP.NET Core WebHost Kestrel veya HTTP.sys için güvenilir bir hizmet olarak yönetme.

![ASP.NET Core güvenilir bir hizmet barındırma için diyagramı][1]

## <a name="aspnet-core-icommunicationlisteners"></a>ASP.NET Core ICommunicationListeners
`ICommunicationListener` Kestrel ve HTTP.sys içinde uygulamaları `Microsoft.ServiceFabric.AspNetCore.*` NuGet paketleri benzer kullanım desenleri sahiptir. Ancak bunlar her web sunucusuna belirli biraz farklı eylemleri gerçekleştirin. 

Her iki iletişim dinleyicileri aşağıdaki bağımsız değişken alan bir oluşturucu sağlar:
 - **`ServiceContext serviceContext`** : Bu `ServiceContext` çalışan hizmeti hakkında bilgi içeren nesne.
 - **`string endpointName`** : Bu adı, bir `Endpoint` ServiceManifest.xml yapılandırması. Öncelikle, burada iki iletişim dinleyicileri farklı olduğu. HTTP.sys *gerektirir* bir `Endpoint` yapılandırması sırasında Kestrel değil.
 - **`Func<string, AspNetCoreCommunicationListener, IWebHost> build`** : Bu oluşturup dönmek de uygulayan bir lambda, bir `IWebHost`. Yapılandırmanıza olanak sağlayan `IWebHost` bir ASP.NET Core uygulaması normalde olduğu gibi. Lambda sizin için oluşturulan bir URL sağlar, Service Fabric tümleştirmesi seçeneklere bağlı olarak kullanırsınız ve `Endpoint` sağladığınız yapılandırma. Ardından, değiştirin veya web sunucusunu başlatmak için bu URL'yi kullanın.

## <a name="service-fabric-integration-middleware"></a>Service Fabric tümleştirme ara yazılımı
`Microsoft.ServiceFabric.AspNetCore` NuGet paketini içeren `UseServiceFabricIntegration` genişletme yöntemini `IWebHostBuilder` , Service Fabric tanıyan ara yazılımı ekler. Bu ara yazılımın Kestrel veya HTTP.sys yapılandırır `ICommunicationListener` benzersiz bir hizmet URL'si Service Fabric adlandırma hizmetine kaydetmek için. Sonra istemciler doğru hizmetine bağlandığınızdan emin olmak için istemci istekleri doğrular. 

İstemciler yanlışlıkla yanlış hizmete bağlanmasını önlemek Bu adım gereklidir. Service Fabric gibi paylaşılan konak ortamı, birden çok web uygulaması, aynı fiziksel veya sanal makinede çalıştırabilirsiniz ancak benzersiz ana bilgisayar adlarını kullanmayın çünkü olmasıdır. Bu senaryo, sonraki bölümde daha ayrıntılı açıklanmıştır.

### <a name="a-case-of-mistaken-identity"></a>Hatalı kimliğinin servis talebi
Protokol, bağımsız olarak, hizmet çoğaltmaları üzerindeki bir benzersiz IP: BağlantıNoktası birleşimini dinleyin. Bir hizmet çoğaltması, bir IP: BağlantıNoktası uç noktası üzerinde dinleme başlatıldıktan sonra Service Fabric adlandırma ağ geçidi için bu uç nokta adresi bildirir. Burada, istemcileri veya diğer hizmetler bu bulabilir. Hizmetleri dinamik olarak atanan uygulama bağlantı noktası kullanıyorsanız, bir hizmet çoğaltması tesadüfen aynı IP: BağlantıNoktası uç noktasına başka bir hizmete daha önce aynı fiziksel veya sanal makine kullanabilirsiniz. Bu, yanlışlıkla yanlış hizmetine bağlanmak bir istemci neden olabilir. Aşağıdaki olaylar dizisi gerçekleşir, bu senaryo ortaya çıkabilir:

 1. A hizmeti üzerinde 10.0.0.1:30000 HTTP üzerinden dinler. 
 2. İstemci, hizmeti bir çözümler ve adresi 10.0.0.1:30000 alır.
 3. Bir hizmet farklı bir düğüme taşır.
 4. B hizmeti üzerinde 10.0.0.1 yerleştirilir ve tesadüfen 30000 aynı bağlantı noktasını kullanır.
 5. İstemci, hizmeti bir önbelleğe alınan adresi 10.0.0.1:30000 ile bağlanmayı dener.
 6. İstemcisi artık başarıyla B hizmetine bağlıysa, taahhüdünü gerçekleştirmeye değil yanlış hizmete bağlı.

Bu hataların tanı koymak güç olabilir rastgele zamanlarda neden olabilir.

### <a name="using-unique-service-urls"></a>Benzersiz bir hizmet URL'leri kullanma
Bu hataları önlemek için hizmetleri benzersiz bir tanımlayıcı adlandırma hizmetine bir uç nokta gönderin ve sonra benzersiz tanımlayıcı istemci istekleri sırasında doğrulayın. Bu, güvenilir bir saldırgan-Kiracı olmayan ortamda hizmetler arasında işbirliği yapan bir işlemdir. Bu tehlikeli müşterili bir ortamda güvenli hizmet kimlik doğrulaması sağlamaz.

Ara yazılım tarafından eklenen güvenilir bir ortamda `UseServiceFabricIntegration` yöntem otomatik olarak ekler benzersiz bir tanımlayıcı adlandırma hizmetine gönderilen adresine. Bu tanımlayıcının her isteği doğrular. Tanımlayıcı eşleşmiyorsa, ara yazılım, hemen bir HTTP 410 kaybedildi yanıtı döndürür.

Hizmetleri kullanan bir dinamik olarak atanan bağlantı yapmanız gerekir bu ara yazılımın kullanın.

Sabit bir benzersiz bağlantı noktası kullanan Hizmetleri ortak bir ortamda bu sorun yoktur. Sabit bir benzersiz bağlantı noktası genellikle harici olarak bilinen bir bağlantı noktasına bağlanmak istemci uygulamaları için gereken hizmetleriniz karşılaştığı için kullanılır. Örneğin, çoğu internet'e yönelik web uygulamaları, web tarayıcısı bağlantıları için 80 veya 443 bağlantı noktası kullanır. Bu durumda, benzersiz tanımlayıcı etkin olmamalıdır.

Aşağıdaki diyagramda, etkin ile Ara yazılım istek akışı gösterilmektedir:

![Service Fabric ASP.NET Core tümleştirme][2]

Kestrel'i hem HTTP.sys `ICommunicationListener` uygulamaları, tam olarak aynı şekilde Bu mekanizma kullanır. HTTP.sys dahili olarak istekleri arka plandaki kullanarak benzersiz URL yollarına göre ayırt edebilirsiniz ancak **HTTP.sys** işlevselliği özelliği, bağlantı noktası *değil* HTTP.systarafındankullanılan`ICommunicationListener`uygulaması. Daha önce açıklanan senaryoda HTTP 503 ve HTTP 404 hatası durum kodları ile sonuçlanır olmasıdır. HTTP 503 ve HTTP 404 genellikle diğer hataları belirtmek için kullanıldığından, sırayla hata amacı belirlemek istemcileri zorlaştırır. 

Bu nedenle, hem Kestrel ve HTTP.sys `ICommunicationListener` uygulamaları standartlaştırmak ara yazılımı tarafından sağlanan üzerinde `UseServiceFabricIntegration` genişletme yöntemi. Bu nedenle, istemcileri yalnızca HTTP 410 yanıtları yeniden çözümleyebilir bir hizmet uç noktası eylemi gerçekleştirmek gerekir.

## <a name="httpsys-in-reliable-services"></a>Reliable Services özelliğinde HTTP.sys
İçeri aktararak güvenilir hizmetlerin HTTP.sys kullanabilirsiniz **Microsoft.ServiceFabric.AspNetCore.HttpSys** NuGet paketi. Bu pakette `HttpSysCommunicationListener`, uygulaması `ICommunicationListener`. `HttpSysCommunicationListener` web sunucusu olarak HTTP.sys kullanarak bir ASP.NET Core Web barındırma güvenilir bir hizmet içinde oluşturmanıza olanak sağlar.

HTTP.sys üzerine kurulmuştur [Windows HTTP sunucu API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx). Bu API kullanan **HTTP.sys** HTTP isteklerini işleyen ve bunları web uygulamaları çalışan işlemler için yönlendirmek için çekirdek sürücüsü. Bu, aynı fiziksel veya sanal makine konak web uygulamalarına disambiguated ya da bir benzersiz URL yolu veya ana bilgisayar adıyla aynı bağlantı noktasında birden çok işlemi sağlar. Bu özellikleri aynı kümenin birden fazla Web sitesi barındırmak için Service Fabric yararlıdır.

>[!NOTE]
>HTTP.sys uygulama yalnızca Windows platform üzerinde çalışır.

Aşağıdaki diyagram, HTTP.sys nasıl kullandığını gösterir **HTTP.sys** bağlantı noktası paylaşımı için çekirdek sürücüsü Windows üzerinde:

![HTTP.sys diyagramı][3]

### <a name="httpsys-in-a-stateless-service"></a>HTTP.sys içinde bir durum bilgisi olmayan hizmet
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

### <a name="httpsys-in-a-stateful-service"></a>HTTP.sys içinde bir durum bilgisi olan hizmet

`HttpSysCommunicationListener` şu anda kullanılmak üzere durum bilgisi olan hizmetler ile arka plandaki zorluklar nedeniyle tasarlanmamıştır **HTTP.sys** bağlantı noktası özelliği. Daha fazla bilgi için aşağıdaki bölümde dinamik bağlantı noktası ayırma ile HTTP.sys bakın. Durum bilgisi olan hizmetler için Kestrel önerilen web sunucusudur.

### <a name="endpoint-configuration"></a>Uç nokta yapılandırması

Bir `Endpoint` Windows HTTP sunucu HTTP.sys dahil olmak üzere API kullanan web sunucuları için yapılandırma gerekiyor. Windows HTTP sunucu API'si kullanan web sunucularının URL'LERİNİ HTTP.sys ile ilk yedek gerekir (Bu normalde ile gerçekleştirilir [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) aracı). 

Bu eylem, Hizmetleri varsayılan olmayan yükseltilmiş ayrıcalıklar gerektiriyor. "Http" veya "https" seçeneklerini `Protocol` özelliği `Endpoint` ServiceManifest.xml yapılandırmasında, özellikle bir URL, sizin adınıza HTTP.sys ile kaydetmek için Service Fabric çalışma zamanı istemek için kullanılır. Bunu kullanarak yapar [ *güçlü bir joker karakter* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL ön eki.

Örneğin, ayrılacak `http://+:80` bir hizmet için ServiceManifest.xml şu yapılandırmayı kullanın:

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

#### <a name="use-httpsys-with-a-static-port"></a>HTTP.sys ile statik bağlantı noktasını kullanın.
HTTP.sys ile statik bağlantı noktasını kullanmak için kullanılan bağlantı noktası numarasını sağlayın `Endpoint` yapılandırma:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-httpsys-with-a-dynamic-port"></a>HTTP.sys dinamik bir bağlantı noktası ile kullanma
HTTP.sys ile dinamik olarak atanan bir bağlantı noktası kullanmak için atla `Port` özelliğinde `Endpoint` yapılandırma:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

Bir dinamik bağlantı noktası tarafından ayrılan bir `Endpoint` yapılandırma yalnızca bir bağlantı noktası sağlar *ana bilgisayar işlemi başına*. Geçerli Service Fabric barındırma modeli, çoklu hizmet örnekleri ve/veya aynı işlem içinde barındırılması için çoğaltmaları sağlar. Yani her biri aynı bağlantı noktası üzerinden ayrılmış paylaşmak `Endpoint` yapılandırma. Birden çok **HTTP.sys** örnekleri, temel alınan'ı kullanarak bir bağlantı noktası paylaşabilir **HTTP.sys** bağlantı noktası özelliği. Ancak tarafından desteklenmeyen `HttpSysCommunicationListener` için istemci isteklerini tanıttığı zorluklar nedeniyle. Dinamik bağlantı noktası kullanımı için Kestrel önerilen web sunucusudur.

## <a name="kestrel-in-reliable-services"></a>Reliable Services özelliğinde kestrel
İçeri aktararak güvenilir hizmetlerin Kestrel kullanabilirsiniz **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet paketi. Bu pakette `KestrelCommunicationListener`, uygulaması `ICommunicationListener`. `KestrelCommunicationListener` web sunucusu olarak Kestrel kullanarak bir ASP.NET Core Web barındırma güvenilir bir hizmet içinde oluşturmanıza olanak sağlar.

Kestrel'i ASP.NET Core platformlar arası web sunucusuna libuv, platformlar arası bir zaman uyumsuz g/ç kitaplığı dayalıdır. HTTP.sys, bir merkezi uç nokta Yöneticisi Kestrel kullanmaz. Ayrıca HTTP.sys, birden çok işlemler arasında bağlantı noktası paylaşımı Kestrel desteklemiyor. Kestrel'i her örneğini benzersiz bir bağlantı noktası kullanmanız gerekir.

![Kestrel'i diyagramı][4]

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

Bir `Endpoint` yapılandırma adı *değil* sağlanan `KestrelCommunicationListener` durum bilgisi olan hizmet içinde. Bu, aşağıdaki bölümde daha ayrıntılı açıklanmıştır.

### <a name="configure-kestrel-to-use-https"></a>Kestrel’i HTTPS kullanacak şekilde yapılandırma
Kestrel'i HTTPS hizmetinizde etkinleştirirken, birkaç dinleme seçeneklerini ayarlamak gerekir. Güncelleştirme `ServiceInstanceListener` kullanmak için bir *EndpointHttps* uç noktası ve (örneğin, bağlantı noktası 443) belirli bir bağlantı noktasını dinler. Web ana bilgisayarı Kestrel web sunucusunu kullanacak şekilde yapılandırırken, IPv6 adresleri tüm ağ arabirimleri üzerinde dinlenecek Kestrel yapılandırmanız gerekir: 

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

Bir öğretici tam bir örnek için bkz: [Kestrel'i HTTPS kullanacak şekilde yapılandırma](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#configure-kestrel-to-use-https).


### <a name="endpoint-configuration"></a>Uç nokta yapılandırması
Bir `Endpoint` yapılandırma Kestrel kullanmak için gerekli değildir. 

Kestrel'i basit bir tek başına web sunucusudur. HTTP.sys (veya HttpListener) aksine, gerekli olmayan bir `Endpoint` ServiceManifest.xml yapılandırmasında URL kayıt başlatmadan önce verilmesini gerektirmediği. 

#### <a name="use-kestrel-with-a-static-port"></a>Kestrel'i statik bir bağlantı noktası ile kullanma
Statik bir bağlantı yapılandırabilirsiniz `Endpoint` Kestrel ile kullanmak için ServiceManifest.xml yapılandırması. Bu kesinlikle gerekli olmasa da, iki olası avantajları sunar:
 - Bağlantı noktası uygulama bağlantı noktası aralığında değil ise işletim sistemi güvenlik duvarı üzerinden Service Fabric tarafından açıldı.
 - Size sağlanan URL `KestrelCommunicationListener` Bu bağlantı noktasını kullanır.

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

ServiceManifest.xml kullanmıyorsa bir `Endpoint` Yapılandırması adı atlamak `KestrelCommunicationListener` Oluşturucusu. Bu durumda, dinamik bir bağlantı noktası kullanır. Bu konu hakkında daha fazla bilgi için sonraki bölüme bakın.

#### <a name="use-kestrel-with-a-dynamic-port"></a>Dinamik bir bağlantı noktasıyla Kestrel kullanın
Kestrel'i otomatik olarak bağlantı noktası atamadan kullanamaz `Endpoint` ServiceManifest.xml yapılandırması. Otomatik olduğundan bağlantı noktası atamadan bir `Endpoint` başına benzersiz bir bağlantı noktası yapılandırması atar *ana işlem*, ve tek ana işlem Kestrel birden fazla içerebilir. Bağlantı noktası paylaşımı desteklemediğinden bu Kestrel ile çalışmaz. Bu nedenle, her Kestrel örneği benzersiz bir bağlantı noktasında açılması gerekir.

Dinamik bağlantı noktası atama Kestrel ile kullanmak için atla `Endpoint` ServiceManifest.xml yapılandırmasında tamamen ve bir uç nokta adına geçirmezseniz `KestrelCommunicationListener` oluşturucusu, aşağıdaki gibi:

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

Bu yapılandırmada, `KestrelCommunicationListener` kullanılmayan bir bağlantı noktası uygulama bağlantı noktası aralığından otomatik olarak seçer.

## <a name="service-fabric-configuration-provider"></a>Service Fabric yapılandırma sağlayıcısı
ASP.NET Core uygulaması yapılandırmasında yapılandırma sağlayıcısı tarafından oluşturulan anahtar-değer çiftleri temel alır. Okuma [ASP.NET Core yapılandırmasında](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/) daha fazla açık genel ASP.NET Core yapılandırma desteği anlamak için.

Bu bölüm, Service Fabric ile ilgili yapılandırma sağlayıcısı içeri aktararak ASP.NET Core yapılandırması ile nasıl tümleştirildiğini açıklar `Microsoft.ServiceFabric.AspNetCore.Configuration` NuGet paketi.

### <a name="addservicefabricconfiguration-startup-extensions"></a>AddServiceFabricConfiguration başlangıç uzantıları
İçeri aktardığınız sonra `Microsoft.ServiceFabric.AspNetCore.Configuration` NuGet paketi gerekir ASP.NET Core yapılandırma API'si ile Service Fabric yapılandırma kaynağı kaydedilecek. Denetleyerek bunu **AddServiceFabricConfiguration** uzantılarında `Microsoft.ServiceFabric.AspNetCore.Configuration` ad alanı karşı `IConfigurationBuilder`.

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

Artık ASP.NET Core hizmeti Service Fabric yapılandırma ayarlarını, herhangi bir uygulama ayarları gibi erişebilirsiniz. Örneğin, türü kesin belirlenmiş nesnelerini ayarlarını yüklemek için seçenekleri desenini kullanabilirsiniz.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<MyOptions>(Configuration);  // Strongly typed configuration object.
    services.AddMvc();
}
```
### <a name="default-key-mapping"></a>Varsayılan anahtar eşleme
Paket adı, bölüm adı ve özellik adı, varsayılan olarak, Service Fabric ile ilgili yapılandırma sağlayıcısı içerir. Bu arada, ASP.NET Core yapılandırma anahtarı gibi gelerek:
```csharp
$"{this.PackageName}{ConfigurationPath.KeyDelimiter}{section.Name}{ConfigurationPath.KeyDelimiter}{property.Name}"
```

Örneğin, adlı bir yapılandırma paketi varsa `MyConfigPackage` aşağıdaki içerikle, ardından yapılandırma değeri üzerinde ASP.NET Core kullanılabilir `IConfiguration` aracılığıyla *MyConfigPackage:MyConfigSection:MyParameter*.
```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">  
  <Section Name="MyConfigSection">
    <Parameter Name="MyParameter" Value="Value1" />
  </Section>  
</Settings>
```
### <a name="service-fabric-configuration-options"></a>Service Fabric yapılandırma seçenekleri
Service Fabric ile ilgili yapılandırma Sağlayıcısı'nı da destekler `ServiceFabricConfigurationOptions` tuş eşlemeyi varsayılan davranışını değiştirmek için.

#### <a name="encrypted-settings"></a>Şifrelenmiş ayarları
Service Fabric, Service Fabric ile ilgili yapılandırma sağlayıcısı gibi şifrelenmiş ayarlarını destekler. Şifreli ayarları için ASP.NET Core şifresi olmayan `IConfiguration` varsayılan olarak. Şifrelenmiş değerler yerine burada depolanır. Ancak ASP.NET Core IConfiguration depolamak için değerin şifresi istiyorsanız ayarlayabilirsiniz *DecryptValue* bayrağı false olarak `AddServiceFabricConfiguration` uzantısı, aşağıdaki gibi:

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
Service Fabric, birden çok yapılandırma paketlerini destekler. Varsayılan olarak, yapılandırma anahtarı paket adı dahil edilir. Ancak ayarlayabilirsiniz `IncludePackageName` false olarak şu şekilde işaretle:
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
Service Fabric ile ilgili yapılandırma sağlayıcısı ile tuş eşlemeyi özelleştirmek için daha gelişmiş senaryoları da destekler. `ExtractKeyFunc` ve değerleri ile birlikte özel-extract `ExtractValueFunc`. Tüm kullanarak Service Fabric yapılandırma verileri için ASP.NET Core yapılandırmasına doldurma işleminin bile değiştirebilirsiniz `ConfigAction`.

Aşağıdaki örnek nasıl kullanılacağını örneklendiren `ConfigAction` veri doldurma özelleştirmek için:
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

### <a name="configuration-updates"></a>Yapılandırma güncelleştirmeleri
Service Fabric ile ilgili yapılandırma sağlayıcısı de yapılandırma güncelleştirmeleri destekler. ASP.NET Core kullanabileceğiniz `IOptionsMonitor` değişiklik bildirimleri alabilir ve ardından `IOptionsSnapshot` yapılandırma verilerini yeniden yüklemek için. Daha fazla bilgi için [ASP.NET Core seçenekleri](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/options).

Bu seçenekleri varsayılan olarak desteklenir. Başka kodlama, yapılandırma güncelleştirmelerini etkinleştirmek için gereklidir.

## <a name="scenarios-and-configurations"></a>Senaryolar ve yapılandırmaları
Bu bölümde, web sunucusu, bağlantı noktası yapılandırması, Service Fabric tümleştirme seçenekleri ve çeşitli ayarları aşağıdaki senaryolarla ilgili sorunları gidermek için önerdiğimiz birleşimi sağlar:
 - Durum bilgisi olmayan ASP.NET Core Hizmetleri harici olarak sunulan
 - Yalnızca iç durum bilgisi olmayan hizmetler ASP.NET Core
 - Yalnızca iç durum bilgisi olan hizmetler ASP.NET Core

Bir **harici olarak kullanıma sunulan hizmet** biri genellikle yük dengeleyici aracılığıyla küme dışında çağrılır bir uç noktasını kullanıma sunar.

Bir **yalnızca dahili** hizmet uç noktası, yalnızca çağrılır küme içinde olduğu.

> [!NOTE]
> Durum bilgisi olan hizmet uç noktaları genellikle internet'e açık olmamalıdır. Yük Dengeleyiciler gibi Azure Load Balancer'ı Service Fabric hizmeti çözümleme habersiz arkasında kümeleri, durum bilgisi olan hizmetler kullanıma sunmak mümkün olmayacaktır. Yük Dengeleyici bulup uygun durum bilgisi olan hizmet çoğaltma trafiği yönlendirmek mümkün olmayacaktır olmasıdır. 

### <a name="externally-exposed-aspnet-core-stateless-services"></a>Durum bilgisi olmayan ASP.NET Core Hizmetleri harici olarak sunulan
Kestrel'i harici, internet'e yönelik HTTP uç noktalarını kullanıma ön uç Hizmetleri için önerilen bir web sunucusudur. Windows üzerinde HTTP.sys, aynı bağlantı noktasını kullanarak aynı kümesindeki birden çok web hizmetleri barındırmanızı sağlar bağlantı noktası paylaşımı yeteneği sağlar. Bu senaryoda, web hizmetleri tarafından ana bilgisayar adı veya yolu, HTTP yönlendirme sağlamak için ön uç proxy veya ağ geçidi üzerinde bağlı olmadan ayrılır.
 
İnternet'e açık olduğunda, durum bilgisi olmayan hizmet bir yük dengeleyici üzerinden erişilebilir olan bilinen ve kararlı bir uç kullanmanız gerekir. Bu URL, uygulamanızın kullanıcılara sağlarız. Aşağıdaki yapılandırmayı öneririz:

|  |  | **Notlar** |
| --- | --- | --- |
| Web sunucusu | Kestrel'i | Windows ve Linux'ta desteklenen kestrel tercih edilen web sunucusudur. |
| Bağlantı noktası yapılandırması | Statik | İyi bilinen bir statik bağlantı noktası olarak yapılandırılmalıdır `Endpoints` yapılandırmasını ServiceManifest.xml, örneğin HTTP için 80 veya HTTPS için 443'tür. |
| ServiceFabricIntegrationOptions | None | Kullanım `ServiceFabricIntegrationOptions.None` seçeneğine Service Fabric tümleştirme ara yazılımı, yapılandırırken hizmete gelen istekler için benzersiz bir tanımlayıcı doğrulamak çalışmaz. Uygulamanızın dış kullanıcıları ara yazılım kullanan benzersiz tanımlayıcı bilgiler bilemezsiniz. |
| Örnek Sayısı | -1 | Tipik kullanım durumlarında ayarı örnek sayısı ayarlanmalıdır *-1*. Bu, böylece yük dengeleyiciden trafiği alması tüm düğümlerde örneği kullanılabilir gerçekleştirilir. |

Birden çok harici olarak kullanıma sunulan hizmetlere aynı dizi düğümü paylaşıyorsanız, HTTP.sys ile benzersiz ancak kararlı bir URL yolu kullanabilirsiniz. Bunu IWebHost yapılandırılırken belirtilen URL değiştirerek gerçekleştirebilirsiniz. Bu HTTP.sys için yalnızca geçerli olduğunu unutmayın.

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
| Web sunucusu | Kestrel'i | HTTP.sys iç durum bilgisi olmayan hizmetler için kullanabilirsiniz, ancak Kestrel bir konak paylaşmak birden çok hizmeti örneği izin vermek için en iyi sunucusudur.  |
| Bağlantı noktası yapılandırması | dinamik olarak atanan | Durum bilgisi olan bir hizmet birden çok kopyasını, bir ana bilgisayar işlemi veya ana bilgisayar işletim sistemi paylaşabilir ve bu nedenle benzersiz bağlantı noktası gerekir. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Dinamik bağlantı noktası atama ile bu ayar, daha önce açıklanan hatalı kimlik sorununu önler. |
| Instancecount | Tüm | Ayarlama örneği sayısı hizmeti çalıştırmak gerekli olarak herhangi bir değere ayarlanabilir. |

### <a name="internal-only-stateful-aspnet-core-service"></a>Yalnızca iç durum bilgisi olan ASP.NET Core hizmeti
Yalnızca gelen küme içinde çağrılan durum bilgisi olan hizmetler, birden çok hizmet arasında işbirliği sağlamak için dinamik olarak atanan bağlantı noktası kullanmanız gerekir. Aşağıdaki yapılandırmayı öneririz:

|  |  | **Notlar** |
| --- | --- | --- |
| Web sunucusu | Kestrel'i | `HttpSysCommunicationListener` Çoğaltmalar bir ana bilgisayar işlemi paylaşmak durum bilgisi olan hizmetler tarafından kullanılmak üzere tasarlanmamıştır. |
| Bağlantı noktası yapılandırması | dinamik olarak atanan | Durum bilgisi olan bir hizmet birden çok kopyasını, bir ana bilgisayar işlemi veya ana bilgisayar işletim sistemi paylaşabilir ve bu nedenle benzersiz bağlantı noktası gerekir. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Dinamik bağlantı noktası atama ile bu ayar, daha önce açıklanan hatalı kimlik sorununu önler. |

## <a name="next-steps"></a>Sonraki adımlar
[Visual Studio'yu kullanarak Service Fabric uygulamanızda hata ayıklama](service-fabric-debugging-your-application.md)


<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
