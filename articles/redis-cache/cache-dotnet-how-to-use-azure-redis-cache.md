---
title: "Azure Redis Önbelleğini Kullanma | Microsoft Belgeleri"
description: "Azure Redis Önbelleği ile Azure, uygulamalarınızın performansını artırmayı öğrenin"
services: redis-cache,app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: c502f74c-44de-4087-8303-1b1f43da12d5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 02/14/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: a3fc1a6bf552ed8c6511c432c0d74b76247ce877
ms.openlocfilehash: c08d863ef8913b9bad766c6232faaaa0a6cfa950
ms.lasthandoff: 02/17/2017


---
# <a name="how-to-use-azure-redis-cache"></a>Azure Redis Önbelleğini kullanma
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Bu kılavuz **Azure Redis Önbelleğini** kullanmaya başlamayı gösterir. Microsoft Azure Redis Önbelleği popüler açık kaynak Redis Önbelleğini temel alır. Microsoft tarafından yönetilen güvenli, ayrılmış bir Redis önbelleğine erişmenizi sağlar. Azure Redis Önbelleği kullanılarak oluşturulan bir önbelleğe Microsoft Azure’daki her uygulamadan erişilebilir.

Microsoft Azure Redis Önbelleği aşağıdaki katmanlarda kullanılabilir:

* **Temel** – Tek düğümlü. 53 GB'a kadar birden çok boyut.
* **Standart** – İki düğümlü Birincil/Çoğaltma. 53 GB'a kadar birden çok boyut. %99,9 SLA.
* **Premium** – İki düğümlü Birincil/Çoğaltma, En fazla 10 parça. 6 GB ila 530 GB birden çok boyut (daha fazlası için bizimle iletişime geçin). [Redis kümesi](cache-how-to-premium-clustering.md), [Redis kalıcılığı](cache-how-to-premium-persistence.md), and [Azure Virtual Network](cache-how-to-premium-vnet.md) dahil tüm Standart katman özellikleri ve fazlası. %99,9 SLA.

Her katman özellikler ve fiyatlandırma açısından farklıdır. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Önbellek Fiyatlandırma Ayrıntıları][Cache Pricing Details].

Bu kılavuz C\# kodu kullanarak [StackExchange.Redis][StackExchange.Redis] istemcisi kullanmayı gösterir. Ele alınan senaryolar **bir önbellek oluşturma ve yapılandırma **, **önbellek istemcilerini yapılandırma** ve **önbelleğe nesne ekleme ve nesneleri önbellekten kaldırma** konularını içerir. Azure Redis Önbelleğini kullanma hakkında daha fazla bilgi için bkz. [Sonraki Adımlar][Next Steps]. Redis Önbelleği ile ASP.NET MVC web uygulaması oluşturmaya ilişkin adım bir öğretici için, bkz. [Redis Önbelleği ile Web Uygulaması Oluşturma](cache-web-app-howto.md)

<a name="getting-started-cache-service"></a>

## <a name="get-started-with-azure-redis-cache"></a>Azure Redis Önbelleğini kullanmaya başlama
Azure Redis Önbelleğini kullanmaya başlamak kolaydır. Başlamak için, bir önbellek hazırlayın ve yapılandırın. Ardından, önbelleğe erişebilmeleri için önbellek istemcilerini yapılandırın. Önbellek istemcileri yapılandırıldıktan sonra, bunlarla çalışmaya başlayabilirsiniz.

* [Önbelleği oluşturma][Create the cache]
* [Önbellek istemcilerini yapılandırma][Configure the cache clients]

<a name="create-cache"></a>

## <a name="create-a-cache"></a>Bir önbellek oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="to-access-your-cache-after-its-created"></a>Oluşturulduktan sonra önbelleğinize erişmek için
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Önbelleğinizi yapılandırma hakkında daha fazla bilgi için, bkz. [Azure Redis Önbelleğini yapılandırma](cache-configure.md).

<a name="NuGet"></a>

## <a name="configure-the-cache-clients"></a>Önbellek istemcilerini yapılandırma
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

İstemci projeniz önbelleğe almak üzere yapılandırıldığında, önbelleğinizle çalışmak için aşağıdaki bölümlerde açıklanan teknikleri kullanabilirsiniz.

<a name="working-with-caches"></a>

## <a name="working-with-caches"></a>Önbelleklerle Çalışma
Bu bölümdeki adımlar Önbellek ile ortak görevler gerçekleştirmeyi açıklar.

* [Önbelleğe bağlanma][Connect to the cache]
* [Önbelleğe nesneler ekleme ve nesneleri önbellekten alma][Add and retrieve objects from the cache]
* [Önbellekte .NET nesneleriyle çalışma](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>

## <a name="connect-to-the-cache"></a>Önbelleğe bağlanma
Program aracılığıyla bir önbellekle çalışmak için önbelleğe başvuru gerekir. Azure Redis Önbelleğine erişmek üzere StackExchange.Redis istemcisini kullanmak istediğiniz bir dosyanın en üstüne aşağıdakileri ekleyin.

    using StackExchange.Redis;

> [!NOTE]
> StackExchange.Redis istemcisi .NET Framework 4 veya üst sürümünü gerektirir.
> 
> 

Azure Redis Önbelleği bağlantısı `ConnectionMultiplexer` sınıfı tarafından yönetilir. Bu sınıf istemci uygulamanız genelinde paylaşılmak ve yeniden kullanılmak içindir ve işlem bazında oluşturulmasına gerek yoktur. 

Bir Azure Redis Önbelleğine bağlanmak ve bağlı bir `ConnectionMultiplexer` örneği döndürülmesi için, statik `Connect` yöntemini çağırın ve önbellek uç noktasını ve anahtarı geçirin. Parola parametresi olarak Azure portalında oluşturulan anahtarı kullanın.

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

> [!IMPORTANT]
> Uyarı: Kimlik bilgilerini asla kaynak kodunda depolamayın. Bu örneği basit tutmak için bunları kaynak kodunda gösteriyorum. Kimlik bilgilerini depolamaya hakkında bilgi için, bkz. [Uygulama Dizeleri ve Bağlantı Dizeleri Nasıl Çalışır?][How Application Strings and Connection Strings Work]
> 
> 

SSL kullanmak istemiyorsanız, `ssl=false` ayarlayın ya da `ssl` parametresini atlayın.

> [!NOTE]
> SSL olmayan bağlantı noktasın yeni önbellekler için varsayılan olarak devre dışı bırakılmıştır. SSL olmayan bağlantı noktası etkinleştirme hakkında yönergeler için bkz. [Erişim Bağlantı Noktaları](cache-configure.md#access-ports).
> 
> 

Uygulamanızda bir `ConnectionMultiplexer` örneği paylaşmaya ilişkin bir yaklaşım, aşağıdaki örneğe benzer bir bağlı örnek döndüren statik özelliğe sahip olmaktır. Bu yaklaşım yalnızca tek bir bağlı `ConnectionMultiplexer` örneği başlatmak için iş parçacığı güvenli bir yol sağlar. Bu örneklerde `abortConnect` false olarak ayarlanır; bunun anlamı Azure Redis Önbelleğine bağlantı kurulmasa bile çağrının başarılı olacağıdır. `ConnectionMultiplexer` temel özelliklerinden biri ağ sorunu ya da diğer nedenler çözümlendiğinde önbellek bağlantısını otomatik olarak geri yüklemesidir.

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

Gelişmiş bağlantı yapılandırma seçenekleri hakkında daha fazla bilgi için bkz:.[StackExchange.Redis yapılandırma modeli][StackExchange.Redis configuration model].

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

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

## <a name="add-and-retrieve-objects-from-the-cache"></a>Önbelleğe nesneler ekleme ve nesneleri önbellekten alma
`StringSet` ve `StringGet` yöntemleri.kullanılarak öğeleri bir önbellekte depolanabilir ve önbellekten alınabilir.

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis, Redis dizeleri kadar veri depolar, ancak bu dizeler önbellekte .NET nesneleri depolarken kullanılabilecek seri hale getirilmiş ikili veriler dahil, birçok veri türünü içerebilir.

`StringGet` çağrılırken, nesne varsa, döndürülür ve nesne yoksa, `null` döndürülür. `null` döndürülürse istenen veri kaynağından değeri alabilir ve sonra kullanmak için önbellekte saklayabilirsiniz. Bu kullanım şekli edilgen önbellek düzeni olarak bilinir.

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

## <a name="work-with-net-objects-in-the-cache"></a>Önbellekte .NET nesneleriyle çalışma
Azure Redis Önbelleği temel veri türlerinin yanı sıra .NET nesnelerini de önbelleğe alabilir, ancak bir .NET nesnesini önbelleğe alabilmek için seri hale getirilmesi gerekir. Bu .NET nesne serileştirmesi uygulama geliştiricisinin sorumluluğundadır ve geliştiriciye seri hale getirici tercihinde esneklik sağlar.

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

## <a name="next-steps"></a>Sonraki Adımlar
Artık temel bilgileri öğrendiğinize göre, Azure Redis Önbelleği hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

* Azure Redis Önbelleği için ASP.NET sağlayıcılarına göz atın.
  * [Azure Redis Oturum Durumu Sağlayıcısı](cache-aspnet-session-state-provider.md)
  * [Azure Redis Önbelleği ASP.NET Çıktı Önbelleği Sağlayıcısı](cache-aspnet-output-cache-provider.md)
* Önbelleğinizin sistem durumunu [izleyebilmeniz](cache-how-to-monitor.md) için [önbellek tanılamayı etkinleştirin](cache-how-to-monitor.md#enable-cache-diagnostics). Azure portalında ölçümleri görüntüleyebilir ve ayrıca istediğiniz araçları kullanarak bunları [indirebilir ve gözden geçirebilirsiniz](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring).
* [StackExchange.Redis önbellek istemcisi belgelerine][StackExchange.Redis cache client documentation] bakın.
  * Azure Redis Önbelleği birçok Redis istemcisinden ve geliştirme dilinden erişilebilir. Daha fazla bilgi için bkz. [http://redis.io/clients][http://redis.io/clients].
* Azure Redis Önbelleği ayrıca Redsmin ve Redis Desktop Manager gibi üçüncü taraf hizmetler ve araçlarla birlikte kullanılabilir.
  * Redsmin hakkında daha fazla bilgi için bkz. [Azure Redis bağlantı dizesi alma ve Redsmin ile birlikte kullanma][How to retrieve an Azure Redis connection string and use it with Redsmin].
  * Azure Redis Önbelleği’ndeki verilerinize erişin ve bunları [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager) kullanan bir GUI ile inceleyin.
* [redis][redis] belgelerine bakın ve [redis veri türleri][redis data types] hakkında bilgi edinin ve [Redis veri türlerine on beş dakikalık bir giriş][a fifteen minute introduction to Redis data types] sayfasına göz atın.

<!-- INTRA-TOPIC LINKS -->
[Next Steps]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Create the cache]: #create-cache
[Configure the cache]: #enable-caching
[Configure the cache clients]: #NuGet
[Working with Caches]: #working-with-caches
[Connect to the cache]: #connect-to-cache
[Add and retrieve objects from the cache]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session


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
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[How to retrieve an Azure Redis connection string and use it with Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis configuration model]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache Pricing Details]: http://www.windowsazure.com/pricing/details/cache/
[Azure portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis cache client documentation]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis data types]: http://redis.io/topics/data-types
[a fifteen minute introduction to Redis data types]: http://redis.io/topics/data-types-intro

[How Application Strings and Connection Strings Work]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/



