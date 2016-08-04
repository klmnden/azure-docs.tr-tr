<properties 
    pageTitle="Azure Redis Önbelleğini Kullanma | Microsoft Azure" 
    description="Azure Redis Önbelleği ile Azure, uygulamalarınızın performansını artırmayı öğrenin" 
    services="redis-cache,app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="05/31/2016" 
    ms.author="sdanie"/>

# Azure Redis Önbelleğini kullanma

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Bu kılavuz **Azure Redis Önbelleğini** kullanmaya başlamayı gösterir. Microsoft Azure Redis Önbelleği popüler açık kaynak Redis Önbelleğini temel alır. Microsoft tarafından yönetilen güvenli, ayrılmış bir Redis önbelleğine erişmenizi sağlar. Azure Redis Önbelleği kullanılarak oluşturulan bir önbelleğe Microsoft Azure’daki her uygulamadan erişilebilir.

Microsoft Azure Redis Önbelleği aşağıdaki katmanlarda kullanılabilir:

-   **Temel** – Tek düğümlü. 53 GB'a kadar birden çok boyut.
-   **Standart** – İki düğümlü Birincil/Çoğaltma. 53 GB'a kadar birden çok boyut. %99,9SLA.
-   **Premium** – İki düğümlü Birincil/Çoğaltma, En fazla 10 parça. 6 GB ila 530 GB birden çok boyut (daha fazlası için bizimle iletişime geçin). [Redis kümesi](cache-how-to-premium-clustering.md), [Redis kalıcılığı](cache-how-to-premium-persistence.md), and [Azure Virtual Network](cache-how-to-premium-vnet.md) dahil tüm Standart katman özellikleri ve fazlası. %99,9SLA.

Her katman özellikler ve fiyatlandırma açısından farklıdır. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Önbellek Fiyatlandırma Ayrıntıları][].

Bu kılavuz C\# kodu kullanarak [StackExchange.Redis][] istemcisi kullanmayı gösterir. Ele alınan senaryolar **bir önbellek oluşturma ve yapılandırma **, **önbellek istemcilerini yapılandırma** ve **önbelleğe nesne ekleme ve nesneleri önbellekten kaldırma** konularını içerir. Azure Redis Önbelleğini kullanma hakkında daha fazla bilgi için bkz [Sonraki Adımlar][] bölümü. Redis Önbelleği ile ASP.NET MVC web uygulaması oluşturmaya ilişkin adım bir öğretici için, bkz. [Redis Önbelleği ile Web Uygulaması Oluşturma](cache-web-app-howto.md)

<a name="getting-started-cache-service"></a>
## Azure Redis Önbelleğini kullanmaya başlama

Azure Redis Önbelleğini kullanmaya başlamak kolaydır. Başlamak için, bir önbellek hazırlayın ve yapılandırın. Ardından, önbelleğe erişebilmeleri için önbellek istemcilerini yapılandırın. Önbellek istemcileri yapılandırıldıktan sonra, bunlarla çalışmaya başlayabilirsiniz.

-   [Önbelleği oluşturma][]
-   [Önbellek istemcilerini yapılandırma][]

<a name="create-cache"></a>
## Bir önbellek oluşturma

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### Oluşturulduktan sonra önbelleğinize erişmek için

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Önbelleğinizi yapılandırma hakkında daha fazla bilgi için, bkz. [Azure Redis Önbelleğini yapılandırma](cache-configure.md).

<a name="NuGet"></a>
## Önbellek istemcilerini yapılandırma

Azure Redis Önbelleği kullanılarak oluşturulan bir önbelleğe her Azure uygulamasından erişilebilir. Visual Studio’da geliştirilen .NET uygulamaları, önbellek istemcisi uygulamalarının yapılandırmasını basitleştiren bir NuGet paketi kullanarak yapılandırılabilecek **StackExchange.Redis** önbellek istemcisi kullanabilir. 

>[AZURE.NOTE] Daha fazla bilgi için, bkz. [StackExchange.Redis][] github sayfası ve [StackExchange.Redis önbelleği istemcisi belgeleri][].

Visual Studio’da StackExchange.Redis NuGet paketi kullanarak bir istemci uygulamasını yapılandırmak için, **Çözüm Gezgini**’nde sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin. 

![NuGet paketlerini yönetme][NuGetMenu]

Arama metin kutusuna **StackExchange.Redis** ya da **StackExchange.Redis.StrongName** yazın, sonuçlardan istediğiniz sürümü seçin ve **Yükle**’ye tıklayın.

>[AZURE.NOTE] **StackExchange.Redis** istemci kitaplığının kesin adlandırılmış bir sürümünü kullanmak isterseniz, **StackExchange.Redis.StrongName** seçeneğin seçin; istemezseniz **StackExchange.Redis** seçeneğin seçin.

![StackExchange.Redis NuGet paketi][StackExchangeNuget]

NuGet paketi, StackExchange.Redis önbellek istemcisiyle Azure Redis Önbelleğine erişmek üzere istemci uygulamanız için gerekli derleme başvurularını ekler.

İstemci projeniz önbelleğe almak üzere yapılandırıldığında, önbelleğinizle çalışmak için aşağıdaki bölümlerde açıklanan teknikleri kullanabilirsiniz.

<a name="working-with-caches"></a>
## Önbelleklerle Çalışma

Bu bölümdeki adımlar Önbellek ile ortak görevler gerçekleştirmeyi açıklar.

-   [Önbelleğe bağlanma][]
-   [Önbelleğe nesneler ekleme ve nesneleri önbellekten alma][]
-   [Önbellekte .NET nesneleriyle çalışma](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## Önbelleğe bağlanma

Program aracılığıyla bir önbellekle çalışmak için, önbelleğe başvuru gerekir. Azure Redis Önbelleğine erişmek üzere StackExchange.Redis istemcisini kullanmak istediğiniz bir dosyanın en üstüne aşağıdakileri ekleyin.

    using StackExchange.Redis;

>[AZURE.NOTE] StackExchange.Redis istemcisi .NET Framework 4 veya üst sürümünü gerektirir.

Azure Redis Önbelleği bağlantısı `ConnectionMultiplexer` sınıfı tarafından yönetilir. Bu sınıf istemci uygulamanız genelinde paylaşılmak ve yeniden kullanılmak zere tasarlanmıştır ve işlem bazında oluşturulmasına gerek yoktur. 

Bir Azure Redis Önbelleğine bağlanmak ve bağlı bir `ConnectionMultiplexer` örneği döndürülmesi için, aşağıdaki örnekteki gibi statik `Connect` yöntemini çağırın ve önbellek uç noktasını ve anahtarı geçirin. Parola parametresi olarak Azure Portal’da oluşturulan anahtarı kullanın.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Uyarı: Kimlik bilgilerini asla kaynak kodunda depolamayın. Bu örneği basit tutmak için bunları kaynak kodunda gösteriyorum. Kimlik bilgilerini depolamaya hakkında bilgi için, bkz. [Uygulama Dizeleri ve Bağlantı Dizeleri Nasıl Çalışır?][] 

SSL kullanmak istemiyorsanız, `ssl=false` ayarlayın ya da `ssl` parametresini atlayın.

>[AZURE.NOTE] SSL olmayan bağlantı noktasın yeni önbellekler için varsayılan olarak devre dışı bırakılmıştır. SSL olmayan bağlantı noktası etkinleştirme hakkında yönergeler için, bkz. [Erişim Bağlantı Noktaları](cache-configure.md#access-ports).

Uygulamanızda bir `ConnectionMultiplexer` örneği paylaşmaya ilişkin bir yaklaşım, aşağıdaki örneğe benzer bir bağlı örnek döndüren statik özelliğe sahip olmaktır. Bu, yalnızca tek bir bağlı `ConnectionMultiplexer` örneği başlatmak için iş parçacığı güvenli bir yol sağlar. Bu örneklerde, `abortConnect` false olarak ayarlanır; bunun anlamı Azure Redis Önbelleğine bağlantı kurulmasa bile çağrının başarılı olacağıdır. `ConnectionMultiplexer` temel özelliklerinden biri ağ sorunu ya da diğer nedenler çözümlendiğinde önbellek bağlantısını otomatik olarak geri yüklemesidir.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

Gelişmiş bağlantı yapılandırma seçenekleri hakkında daha fazla bilgi için bkz:.[StackExchange.Redis yapılandırma modeli][].

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Bağlantı kurulduktan sonra, `ConnectionMultiplexer.GetDatabase` yöntemini çağırarak redis önbelleği veritabanına bir başvuru döndürün. `GetDatabase` yönteminden döndürülen nesne küçük, geçişli bir nesnedir ve depolanması gerekmez.

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

Artık Azure Redis Önbelleği örneğine bağlanmayı ve önbellek veritabanına bir başvuru döndürmeyi bildiğinize göre, şimdi önbellekle çalışmaya göz atalım.

<a name="add-object"></a>
## Önbelleğe nesneler ekleme ve nesneleri önbellekten alma

`StringSet` ve `StringGet` yöntemleri.kullanılarak öğeleri bir önbellekte depolanabilir ve önbellekten alınabilir.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis, Redis dizeleri kadar veri depolar, ancak bu dizeler önbellekte .NET nesneleri depolarken kullanılabilecek seri hale getirilmiş ikili veriler dahil, birçok veri türünü içerebilir.

`StringGet` çağrılırken, nesne varsa, döndürülür ve nesne yoksa, `null` döndürülür. Bu durumda istenen veri kaynağından değeri alabilir ve sonra kullanmak için önbellekte saklayabilirsiniz. Bu edilgen önbellek düzeni olarak bilinir.

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Bir öğenin önbellekte sona erme tarihini belirtmek için, `StringSet` dizesine ait `TimeSpan` parametresini kullanın.

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## Önbellekte .NET nesneleriyle çalışma

Azure Redis Önbelleği temel veri türlerinin yanı sıra .NET nesnelerini de önbelleğe alabilir, ancak bir .NET nesnesini önbelleğe alabilmek için seri hale getirilmesi gerekir. Bu uygulama geliştiricisinin sorumluluğundadır ve geliştiriciye seri hale getirici tercihinde esneklik sağlar.

Nesneleri seri hale getirmenin basit bir yolu [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1)’te `JsonConvert` seri hale getirme yöntemleri kullanmak ve JSON’a ve JSON’dan seri hale getirmektir. Aşağıdaki örnekte bir `Employee` nesnesi örneği kullanılarak al ve ayarla seçeneği gösterilmiştir.


    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    
        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## Sonraki Adımlar

Artık temel bilgileri öğrendiğinize göre, Azure Redis Önbelleği hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

-   Azure Redis Önbelleği için ASP.NET sağlayıcılarına göz atın.
    -   [Azure Redis Oturum Durumu Sağlayıcısı](cache-aspnet-session-state-provider.md)
    -   [Azure Redis Önbelleği ASP.NET Çıktı Önbelleği Sağlayıcısı](cache-aspnet-output-cache-provider.md)
-   Önbelleğinizin sistem durumunu [izleyebilmeniz](cache-how-to-monitor.md) için [önbellek tanılamayı etkinleştirin](cache-how-to-monitor.md#enable-cache-diagnostics). Azure Portal’da ölçümleri görüntüleyebilir ve ayrıca istediğiniz araçları kullanarak bunları [indirebilir ve gözden geçirebilirsiniz](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring).
-   [StackExchange.Redis önbellek istemcisi belgeleri][]ne bakın.
    -   Azure Redis Önbelleği birçok Redis istemcisinden ve geliştirme dilinden erişilebilir. Daha fazla bilgi için bkz. [http://redis.io/clients][].
    -   Azure Redis Önbelleği Redsmin gibi hizmetlerle de kullanılabilir. Daha fazla bilgi için, bkz. [Azure Redis bağlantı dizesi alma ve Redsmin ile birlikte kullanma][].
-   [Redis][] belgelerine bakın ve [redis veri türleri][] hakkında bilgi edinin ve [Redis veri türlerine on beş dakikalık bir giriş][]e göz atın.



<!-- INTRA-TOPIC LINKS -->
[Sonraki Adımlar]: #next-steps
[Azure Redis Önbelleğine giriş (Video)]: #video
[Azure Redis Önbelleği nedir?]: #what-is
[Bir Azure Önbelleği oluşturma]: #create-cache
[Bana uygun önbelleğe alma türü hangisidir?]: #choosing-cache
[Visual Studio Projenizi Azure Önbelleğe Almayı Kullanmak Üzere Hazırlama]: #prepare-vs
[Uygulamanızı Önbelleğe Almayı Kullanmak Üzere Yapılandırma]: #configure-app
[Azure Redis Önbelleğini kullanmaya başlama]: #getting-started-cache-service
[Önbelleği oluşturma]: #create-cache
[Önbelleği yapılandırma]: #enable-caching
[Önbellek istemcilerini yapılandırma]: #NuGet
[Önbelleklerle Çalışma]: #working-with-caches
[Önbelleğe bağlanma]: #connect-to-cache
[Önbelleğe nesneler ekleme ve nesneleri önbellekten alma]: #add-object
[Bir nesnenin sona erme tarihini önbellekte belirtme]: #specify-expiration
[Önbellekte ASP.NET oturumu durumu depolama]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://redis.io/clients]: http://redis.io/clients
[Azure Redis Önbelleği için diğer dillerde geliştirme]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[Azure Redis bağlantı dizesi alma ve Redsmin ile birlikte kullanma]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Oturum Durumu Sağlayıcısı]: http://go.microsoft.com/fwlink/?LinkId=398249
[Nasıl yapılır: Programlama ile Önbellek İstemcisi yapılandırma]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Azure Önbelleği için Oturum Durumu Sağlayıcısı]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Oturum Durumunu Önbelleğe Alma]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Azure Önbelleği için Çıktı Önbelleği]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Paylaşılan Önbellek]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Ekip Blogu]: http://blogs.msdn.com/b/windowsazure/
[Azure Önbelleği]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[Sanal Makine Boyutlarını Yapılandırma]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Önbelleği Kapasite Planlamada Dikkate Alınması Gerekenler]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Önbelleği]: http://go.microsoft.com/fwlink/?LinkId=252658
[Nasıl yapılır: Bir ASP.NET Sayfasının Önbelleğe Alınabilirliğini Bildirimli Olarak Ayarlama]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[Nasıl yapılır: Bir Sayfanın Önbelleğe Alınabilirliğini Programlı Olarak Ayarlama]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Azure Redis Önbelleğinde önbellek yapılandırma]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis yapılandırma modeli]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Önbellekte .NET nesneleriyle çalışma]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Paket Yöneticisini Yükleme]: http://go.microsoft.com/fwlink/?LinkId=240311
[Önbellek Fiyatlandırma Ayrıntıları]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portalı]: https://portal.azure.com/

[Azure Redis Önbelleğine Genel Bakış]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Önbelleği]: http://go.microsoft.com/fwlink/?LinkId=398247

[Azure Redis Önbelleğine Geçiş]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Önbelleği Örnekleri]: http://go.microsoft.com/fwlink/?LinkId=320840
[Azure kaynaklarınızı yönetmek için Kaynak gruplarını kullanma]: http://azure.microsoft.com/documentation/articles/resource-group-overview/

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis önbellek istemcisi belgeleri]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis veri türleri]: http://redis.io/topics/data-types
[Redis veri türlerine on beş dakikalık bir giriş]: http://redis.io/topics/data-types-intro

[Uygulama Dizeleri ve Bağlantı Dizeleri Nasıl Çalışır?]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/





<!----HONumber=Jun16_HO2-->


