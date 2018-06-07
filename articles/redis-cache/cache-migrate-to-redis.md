---
title: Redis - Azure yönetilen önbellek hizmeti uygulamaları geçirme | Microsoft Docs
description: Azure Redis önbelleği için yönetilen önbellek hizmeti ve rol içi önbelleği uygulamalarını geçirmeyi öğrenme
services: redis-cache
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: tysonn
ms.assetid: 041f077b-8c8e-4d7c-a3fc-89d334ed70d6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/30/2017
ms.author: wesmc
ms.openlocfilehash: f499925ecea8ca127c90691f7d92e74e8df68cf9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34639495"
---
# <a name="migrate-from-managed-cache-service-to-azure-redis-cache"></a>Yönetilen önbellek hizmetinden Azure Redis önbelleğine geçiş
Azure Redis önbelleği için Azure yönetilen önbellek hizmeti kullanan, uygulamaları geçirme önbelleğe alma, uygulamanız tarafından kullanılan yönetilen önbellek hizmeti özelliklere bağlı olarak, uygulamanızın küçük değişiklikler ile gerçekleştirilebilir. API'ler tam olarak aynı durumdayken benzer ve küçük değişiklikler ile yönetilen önbellek hizmeti önbelleği erişmek için kullandığı mevcut kodunuzu çoğunu yeniden kullanılabilir. Bu makalede gerekli yapılandırma ve Azure Redis önbelleği kullanma, yönetilen önbellek hizmeti uygulamalarınızı geçirme için uygulama değişiklikleri yapma gösterir ve nasıl bazı Azure Redis önbelleği özellikleri işlevselliğini uygulamak için kullanılabileceğini gösterir bir Yönetilen önbellek hizmeti önbelleği.

>[!NOTE]
>Yönetilen önbellek hizmeti ve rol içi önbelleği [Çekildi](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/) 30 Kasım 2016. Azure Redis önbelleği için geçirmek istediğiniz tüm rol içi önbelleği dağıtımları varsa, bu makaledeki adımları izleyebilirsiniz.

## <a name="migration-steps"></a>Geçiş adımları
Aşağıdaki adımlar, Azure Redis önbelleği kullanma yönetilen önbellek hizmeti uygulamayı geçirmek için gereklidir.

* Azure Redis önbelleği için yönetilen önbellek hizmeti özellikleri eşleyin
* Önbelleği teklifini seçin
* Bir önbellek oluşturma
* Önbellek istemcilerini yapılandırın
  * Yönetilen önbellek hizmeti yapılandırmasını Kaldır
  * StackExchange.Redis NuGet paketi kullanarak bir önbellek istemcisini yapılandırma
* Yönetilen önbellek hizmeti kodunu taşıma
  * ConnectionMultiplexer sınıfını kullanarak önbelleğe bağlanma
  * Önbelleğinde erişim temel veri türleri
  * Önbellekte .NET nesneleriyle çalışma
* ASP.NET oturum durumu ve Azure Redis önbelleği için çıktı önbelleği geçirme 

## <a name="map-managed-cache-service-features-to-azure-redis-cache"></a>Azure Redis önbelleği için yönetilen önbellek hizmeti özellikleri eşleyin
Azure yönetilen önbellek hizmeti ve Azure Redis önbelleği benzer, ancak bazı özelliklerine farklı şekillerde uygulamak. Bu bölümde bazı farklar açıklanmaktadır ve Azure Redis Önbelleği'nde yönetilen önbellek hizmeti özelliklerini uygulanmasına yönelik rehberlik sağlar.

| Yönetilen önbellek hizmeti özelliği | Yönetilen önbellek hizmeti desteği | Azure Redis önbelleği desteği |
| --- | --- | --- |
| Adlandırılmış önbellekler |Varsayılan önbellek yapılandırılır ve standart ve Premium önbellekte önbellekleri adlı ek bir dokuz kadar teklifleri isterseniz yapılandırılabilir. |Azure Redis önbellekleri yapılandırılabilir adlandırılmış önbellekler için benzer bir işlevsellik uygulamak için kullanılan veritabanlarının (varsayılan 16) sahiptir. Daha fazla bilgi için bkz. [Redis veritabanı nedir?](cache-faq.md#what-are-redis-databases) ve [Varsayılan Redis sunucu yapılandırması](cache-configure.md#default-redis-server-configuration). |
| Yüksek Kullanılabilirlik |Standart ve Premium önbellek teklifleri önbelleğinde öğeleri için yüksek kullanılabilirlik sağlar. Öğeleri bir hata nedeniyle kaybolursa önbelleğindeki öğeler yedek kopyalarını yine kullanılabilir durumdadır. İkincil Önbelleğe yazma eş zamanlı olarak yapılır. |Yüksek kullanılabilirlik (her parça Premium önbelleğinde birincil/çoğaltma çifti sahiptir) bir iki düğümlü birincil/çoğaltma yapılandırması olan standart ve Premium önbellek tekliflerini, kullanılabilir. Çoğaltma yazma işlemlerini zaman uyumsuz olarak yapılır. Daha fazla bilgi için bkz: [Azure Redis önbelleği fiyatlandırma](https://azure.microsoft.com/pricing/details/cache/). |
| Bildirimler |Önbellek işlemleri çeşitli ortaya çıktığında bir adlandırılmış önbelleği zaman uyumsuz bildirimleri almak istemcilerin sağlar. |İstemci uygulamaları, Redis pub/alt kullanabilir veya [Keyspace bildirimleri](cache-configure.md#keyspace-notifications-advanced-settings) benzer bir işlev bildirimleri elde etmek için. |
| Yerel önbellek |Önbelleğe alınan nesnelerin bir kopyasını çok hızlı erişim için istemci üzerinde yerel olarak depolar. |İstemci uygulamaları bir sözlük veya benzer veri yapısı kullanarak bu işlevselliği uygulamanız gerekir. |
| Çıkarma İlkesi |None veya LRU. Varsayılan ilke LRU olur. |Azure Redis önbelleği aşağıdaki çıkarma ilkeleri destekler: geçici lru allkeys lru, geçici rastgele, allkeys rastgele, geçici ttl, noeviction. Varsayılan, geçici lru ilkesidir. Daha fazla bilgi için bkz: [varsayılan Redis sunucu yapılandırması](cache-configure.md#default-redis-server-configuration). |
| Süre sonu ilkesi |Varsayılan süre sonu ilkesi mutlak ve varsayılan sona erme aralığı 10 dakikadır. Kayan ve hiçbir zaman ilkeler de kullanılabilir. |Varsayılan olarak önbellekteki öğeleri son kullanma tarihi, ancak bir sona erme önbellek kümesi aşırı kullanarak bir yazma başına temelinde yapılandırılabilir. |
| Bölgeler ve etiketleme |Bölgeler önbelleğe alınmış öğeleri için alt grupları var. Bölgeler, önbelleğe alınmış öğelerin ek açıklama etiketleri adlı ek açıklama dizelerle de destekler. Bölgeler arama etiketli öğeler bu bölgede işlemleri becerisini de destekler. Bir bölge içindeki tüm öğeler tek bir düğüm önbellek küme içinde yer alır. |(Redis kümesi etkinleştirilmediği sürece) yönetilen önbellek hizmeti bölgeler kavramı uygulanamaz Redis önbelleği tek bir düğüm oluşur. Aramayı destekler ve joker işlemleri açıklayıcı etiketleri içindeki anahtar adlarını katıştırılmış ve öğeleri daha sonra almak için kullanılan anahtarları alınırken redis. Redis kullanarak etiketleme bir çözüm uygulama örneği için bkz: [Redis ile etiketleme önbellek uygulama](http://stackify.com/implementing-cache-tagging-redis/). |
| Seri hale getirme |Yönetilen önbellek NetDataContractSerializer, BinaryFormatter ve özel serileştiricileri kullanımını destekler. NetDataContractSerializer varsayılandır. |Bunları kadar istemci uygulama geliştiricisinin serileştirici seçimi ile önbelleğine yerleştirmeden önce .NET nesnelerini seri hale getirmek için istemci uygulamanın sorumluluğundadır. Daha fazla bilgi ve örnek kod için bkz: [önbellekte .NET nesneleriyle çalışma](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache). |
| Önbellek öykünücüsü |Yönetilen önbellek yerel önbelleği öykünücü sağlar. |Azure Redis önbelleği bir öykünücü sahip değildir, ancak yapabilecekleriniz [redis server.exe MSOpenTech yapısını yerel olarak çalıştırma](cache-faq.md#cache-emulator) bir öykünücü deneyimi sağlamak için. |

## <a name="choose-a-cache-offering"></a>Önbelleği teklifini seçin
Microsoft Azure Redis Cache aşağıdaki katmanlarda kullanılabilir:

* **Temel** – Tek düğümlü. 53 GB'a kadar birden çok boyut.
* **Standart** – İki düğümlü Birincil/Çoğaltma. 53 GB'a kadar birden çok boyut. %99,9 SLA.
* **Premium** – İki düğümlü Birincil/Çoğaltma, En fazla 10 parça. 6 GB'tan 530 GB'a kadar birden çok boyut. [Redis kümesi](cache-how-to-premium-clustering.md), [Redis kalıcılığı](cache-how-to-premium-persistence.md), and [Azure Virtual Network](cache-how-to-premium-vnet.md) dahil tüm Standart katman özellikleri ve fazlası. %99,9 SLA.

Her katman özellikler ve fiyatlandırma açısından farklıdır. Özellikler, bu kılavuzda daha sonra ve fiyatlandırma hakkında daha fazla bilgi için ele alınmıştır, bkz: [önbellek fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cache/).

Geçiş için bir başlangıç noktası, önceki yönetilen önbellek hizmeti önbelleği boyutunu eşleşen boyutu seçin ve ardından yukarı veya aşağı uygulamanızın gereksinimlerine bağlı olarak ölçeği olmaktır. Sağ Azure Redis önbelleği teklifini seçme hakkında daha fazla bilgi için bkz: [hangi Redis önbelleği teklifini ve boyutunu kullanmalıyım](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="create-a-cache"></a>Bir önbellek oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="configure-the-cache-clients"></a>Önbellek istemcilerini yapılandırın
Önbellek oluşturuldu ve yapılandırıldıktan sonra sonraki adıma yönetilen önbellek hizmeti yapılandırmasını kaldırın ve Azure Redis önbelleği yapılandırması eklemek için ve önbellek istemcilerinin önbelleğe erişebilmeleri başvurur.

* Yönetilen önbellek hizmeti yapılandırmasını Kaldır
* StackExchange.Redis NuGet paketi kullanarak bir önbellek istemcisini yapılandırma

### <a name="remove-the-managed-cache-service-configuration"></a>Yönetilen önbellek hizmeti yapılandırmasını Kaldır
Önce istemci uygulamalar Azure Redis önbelleği için var olan yönetilen önbellek hizmeti yapılandırmasını yapılandırılabilir ve yönetilen önbellek hizmeti NuGet paketi kaldırarak derleme başvurularını kaldırılması gerekir.

Yönetilen önbellek hizmeti NuGet paketini kaldırmak için istemci projeye sağ **Çözüm Gezgini** ve **NuGet paketlerini Yönet**. Seçin **yüklü paketleri** düğümü ve türü W**indowsAzure.Caching** arama yüklü paketleri kutusuna. Seçin **Windows** **Azure önbelleği** (veya **Windows** **Azure önbelleğe alma** NuGet paketi sürümüne bağlı olarak), tıklatın**Kaldırma**ve ardından **Kapat**.

![Azure yönetilen önbellek hizmeti NuGet Paketi'ni Kaldır](./media/cache-migrate-to-redis/IC757666.jpg)

Yönetilen önbellek hizmeti NuGet paketi kaldırma yönetilen önbellek hizmeti derlemeler ve yönetilen önbellek hizmeti girdileri app.config veya web.config istemci uygulamasının kaldırır. Bazı özelleştirilmiş ayarları NuGet paketi kaldırırken kaldırılmamış olabilir çünkü web.config veya app.config açın ve aşağıdaki öğeleri kaldırıldığından emin olun.

Emin `dataCacheClients` giriş kaldırılır `configSections` öğesi. Tüm kaldırmayın `configSections` öğesi; yalnızca Kaldır `dataCacheClients` varsa, girdi.

```xml
<configSections>
  <!-- Existing sections omitted for clarity. -->
  <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
</configSections>
```

Emin `dataCacheClients` bölüm kaldırılır. `dataCacheClients` Bölümü aşağıdaki örneğe benzer olacaktır.

```xml
<dataCacheClients>
  <dataCacheClientname="default">
    <!--To use the in-role flavor of Azure Cache, set identifier to be the cache cluster role name -->
    <!--To use the Azure Managed Cache Service, set identifier to be the endpoint of the cache cluster -->
    <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>

    <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
    <!--Use this section to specify security settings for connecting to your cache. This section is not required if your cache is hosted on a role that is a part of your cloud service. -->
    <!--<securityProperties mode="Message" sslEnabled="true">
      <messageSecurity authorizationInfo="[Authentication Key]" />
    </securityProperties>-->
  </dataCacheClient>
</dataCacheClients>
```

Yönetilen önbellek hizmeti yapılandırma kaldırıldıktan sonra aşağıdaki bölümde açıklandığı gibi önbellek istemcisi yapılandırabilirsiniz.

### <a name="configure-a-cache-client-using-the-stackexchangeredis-nuget-package"></a>StackExchange.Redis NuGet paketi kullanarak bir önbellek istemcisini yapılandırma
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

## <a name="migrate-managed-cache-service-code"></a>Yönetilen önbellek hizmeti kodunu taşıma
StackExchange.Redis önbellek İstemcisi API yönetilen önbellek hizmeti için benzer. Bu bölümde farklar genel bir bakış sağlar.

### <a name="connect-to-the-cache-using-the-connectionmultiplexer-class"></a>ConnectionMultiplexer sınıfını kullanarak önbelleğe bağlanma
Yönetilen önbellek hizmeti önbelleği bağlantılar tarafından işlenen `DataCacheFactory` ve `DataCache` sınıfları. Azure Redis Önbelleği'nde bu bağlantılar tarafından yönetilen `ConnectionMultiplexer` sınıfı.

Aşağıdaki using deyimi, dosyanın önbellek erişmek istediğiniz dön.

```csharp
using StackExchange.Redis
```

Bu ad alanı sorunu çözmezse, açıklandığı gibi StackExchange.Redis NuGet paketi eklediğinizden emin olun [hızlı başlangıç: bir .NET uygulama ile kullanım Azure Redis önbelleği](cache-dotnet-how-to-use-azure-redis-cache.md).

> [!NOTE]
> StackExchange.Redis istemcisi .NET Framework 4 veya üstünü gerektirir.
> 
> 

Azure Redis önbelleği örneğine bağlanmak için statik çağrı `ConnectionMultiplexer.Connect` yöntemi ve uç noktasını ve anahtarı geçirin. Uygulamanızda bir `ConnectionMultiplexer` örneği paylaşmaya ilişkin bir yaklaşım, aşağıdaki örneğe benzer bir bağlı örnek döndüren statik özelliğe sahip olmaktır. Bu yaklaşım bağlı tek bir başlatmak için bir iş parçacığı açısından güvenli yoldur `ConnectionMultiplexer` örneği. Bu örnekte `abortConnect` önbelleğine bağlantı kurulmasa bile çağrının başarılı olacağıdır anlamına gelir false olarak ayarlanmış. `ConnectionMultiplexer` temel özelliklerinden biri ağ sorunu ya da diğer nedenler çözümlendiğinde önbellek bağlantısını otomatik olarak geri yüklemesidir.

```csharp
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
```

Önbellek uç noktasını, anahtarlar ve bağlantı noktaları öğesinden elde edilebilir **Redis önbelleği** dikey penceresinde, önbellek örneğinizin. Daha fazla bilgi için bkz: [Redis önbelleği özellikleri](cache-configure.md#properties).

Bağlantı kurulduktan sonra çağırarak Redis önbelleği veritabanına bir başvuru döndürmek `ConnectionMultiplexer.GetDatabase` yöntemi. `GetDatabase` yönteminden döndürülen nesne küçük, geçişli bir nesnedir ve depolanması gerekmez.

```csharp
IDatabase cache = Connection.GetDatabase();

// Perform cache operations using the cache object...
// Simple put of integral data types into the cache
cache.StringSet("key1", "value");
cache.StringSet("key2", 25);

// Simple get of data types from the cache
string key1 = cache.StringGet("key1");
int key2 = (int)cache.StringGet("key2");
```

StackExchange.Redis istemcisi kullanır `RedisKey` ve `RedisValue` erişme ve öğeleri önbellekte depolamak için türleri. Bu tür dizesi dahil olmak üzere en basit dil türler harita ve genellikle doğrudan kullanılmaz. Redis dizeleri Redis değeri en temel tür ve pek çok seri hale getirilmiş ikili akışlar dahil olmak üzere veri türünü içerebilir ve türü doğrudan kullanamazsınız sırada içeren yöntemleri kullanacaksınız `String` adı. En basit veri türleri için depolama ve önbellek kullanımından öğeleri alma `StringSet` ve `StringGet` yöntemleri, koleksiyonları veya diğer Redis veri türlerine önbellekte depolamak sürece. 

`StringSet` ve `StringGet` yönetilen önbellek hizmeti için benzer `Put` ve `Get` yöntemleriyle, önemli bir fark olmasına, ayarlayın ve bir .NET nesnesini önbelleğe almak için önce onu önce seri gerekir. 

Çağrılırken `StringGet`, nesne varsa, döndürülür ve mevcut değilse null değeri döndürülür. Bu durumda, istenen veri kaynağından değerini almak ve sonra kullanmak için önbellekte saklayın. Bu deseni edilgen önbellek düzeni bilinir.

Bir öğenin önbellekte sona erme tarihini belirtmek için, `StringSet` dizesine ait `TimeSpan` parametresini kullanın.

```csharp
cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));
```

Azure Redis önbelleği temel veri türlerinin yanı sıra .NET nesnelerini ile çalışabilir, ancak bir .NET nesnesini önbelleğe alabilmek için seri hale getirilmesi gerekir. Bu seri hale getirme uygulama geliştiricisinin sorumluluğundadır ve seri hale getirici seçenek Geliştirici esnekliği sağlar. Daha fazla bilgi ve örnek kod için bkz: [önbellekte .NET nesneleriyle çalışma](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

## <a name="migrate-aspnet-session-state-and-output-caching-to-azure-redis-cache"></a>ASP.NET oturum durumu ve Azure Redis önbelleği için çıktı önbelleği geçirme
Azure Redis önbelleği ASP.NET oturum durumu ve sayfa çıktı önbelleği sağlayıcıları vardır. Bu sağlayıcılar yönetilen önbellek hizmeti sürümleri kullanır, uygulamanızı geçirmek için önce varolan bölümleri web.config dosyasından kaldırın ve sağlayıcıları Azure Redis önbelleği sürümleri yapılandırın. Azure Redis önbelleği ASP.NET sağlayıcıları kullanma ile ilgili yönergeler için bkz: [Azure Redis önbelleği için ASP.NET oturum durumu sağlayıcısı](cache-aspnet-session-state-provider.md) ve [Azure Redis önbelleği için ASP.NET çıktı önbelleği sağlayıcısı](cache-aspnet-output-cache-provider.md).

## <a name="next-steps"></a>Sonraki adımlar
Araştır [Azure Redis önbelleği belgelerine](https://azure.microsoft.com/documentation/services/cache/) öğreticiler, örnekler, videolar ve daha fazla bilgi için.

