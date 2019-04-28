---
title: Redis için bir Premium Azure önbelleği için Redis kümeleme yapılandırma | Microsoft Docs
description: Oluşturma ve Redis örneği için Premium katman Azure önbelleği için Redis kümeleme yönetme hakkında bilgi edinin
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 62208eec-52ae-4713-b077-62659fd844ab
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 06/13/2018
ms.author: yegu
ms.openlocfilehash: 602d77f3d4e8ed10c2c964462bc2dc21240cef5c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60541395"
---
# <a name="how-to-configure-redis-clustering-for-a-premium-azure-cache-for-redis"></a>Redis Redis için Premium Azure Cache için kümeleri yapılandırma
Azure önbelleği için Redis önbellek boyutunu ve özelliklerini, kümeleme, Kalıcılık ve sanal ağ desteği gibi Premium katman özellikleri dahil olmak üzere tercih ettiğiniz esneklik sağlayan farklı bir önbellek teklifleri sahiptir. Bu makalede, Redis örneği için bir premium Azure Cache içinde kümelemeyi yapılandırmayı açıklar.

Diğer premium önbellek özelliklerini hakkında daha fazla bilgi için bkz: [Azure önbelleği için Redis Premium katmanına giriş](cache-premium-tier-intro.md).

## <a name="what-is-redis-cluster"></a>Redis kümesi nedir?
Azure önbelleği için Redis sunan Redis kümesi olarak [Redis içinde uygulanan](https://redis.io/topics/cluster-tutorial). Redis kümesiyle aşağıdaki avantajlardan yararlanabilirsiniz: 

* Otomatik olarak veri kümeniz birden çok düğüm arasında bölme olanağı. 
* İşlem düğümlerinin bir alt kümesi olduğunda devam etme olanağı hataları yaşıyor veya kümenin geri kalanı ile iletişim kurulamıyor. 
* Daha fazla aktarım hızı: Parça sayısı arttıkça aktarım hızı doğrusal olarak artar. 
* Daha fazla bellek boyutu: Parça sayısı arttıkça doğrusal olarak artar.  

Küme, kümelenmiş bir önbellek için kullanılabilir bağlantı sayısını artırmaz. Boyut, işleme ve premium önbelleklere sahip bant genişliği hakkında daha fazla bilgi için bkz: [hangi Azure önbelleği için Redis teklifini ve boyutunu kullanmalıyım?](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)

Azure'da Redis kümesi, her parça bir birincil/çoğaltma çifti çoğaltma ile çoğaltma tarafından Azure Cache için Redis hizmeti yönetildiği sahip olduğu bir birincil/çoğaltma model olarak sunulur. 

## <a name="clustering"></a>Kümeleme
Kümeleme etkinleştirilir **yeni Azure önbelleği için Redis** önbellek oluşturma sırasında dikey. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Küme üzerinde yapılandırılır **Redis küme** dikey penceresi.

![Kümeleme][redis-cache-clustering]

En fazla 10 parça kümedeki olabilir. Tıklayın **etkin** kaydırıcısını kaydırın ve 1 ile 10 arasında bir sayı yazın **parça sayısı** tıklatıp **Tamam**.

Her parça Azure tarafından yönetilen bir birincil/çoğaltma önbellek çiftidir ve toplam önbellek boyutunu fiyatlandırma katmanında seçili önbelleğin parça sayısı çarpılmasıyla hesaplanır. 

![Kümeleme][redis-cache-clustering-selected]

Önbellek oluşturulduktan sonra bu sunucuya bağlanın ve kullanımı yalnızca kümelenmemiş bir önbellek ister ve Redis önbelleği parça genelinde verileri dağıtır. Tanılama ise [etkin](cache-how-to-monitor.md#enable-cache-diagnostics), ölçümleri, her parça için ayrı olarak yakalanır ve olabilir [görüntülenen](cache-how-to-monitor.md) Azure önbelleği için Redis dikey penceresi içinde. 

> [!NOTE]
> 
> Kümeleme yapılandırıldığında istemci uygulamanız gereken bazı küçük farklar vardır. Daha fazla bilgi için [kümeleme kullanmak üzere istemci uygulamamın herhangi bir değişiklik yapmanız gerekiyor mu?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 

StackExchange.Redis istemcisi ile kümeleme ile çalışmak için örnek kod, bkz: [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) kısmı [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) örnek.

<a name="cluster-size"></a>

## <a name="change-the-cluster-size-on-a-running-premium-cache"></a>Çalışan bir küme boyutu değiştirme premium önbellek
Üzerinde çalışan bir küme boyutunu değiştirmek için kümeleme özelliği etkinleştirilmiş olan premium önbellek tıklayın **Redis küme boyutu** gelen **kaynak menüsünde**.

> [!NOTE]
> Azure önbelleği için Redis Premium katmanı genel kullanıma sunulmuş olsa da, Redis küme boyutu özelliği şu anda Önizleme aşamasındadır.
> 
> 

![Redis küme boyutu][redis-cache-redis-cluster-size]

Küme boyutunu değiştirmek için kaydırıcıyı kullanın veya 1 ile 10 arasında bir sayı yazın **parça sayısı** metin kutusu ve tıklatın **Tamam** kaydetmek için.

Küme boyutu artırıldığında en fazla aktarım hızı ve önbellek boyutu da artar. Küme boyutu artırmayı max artırmaz. bağlantılar istemciler için kullanılabilir.

> [!NOTE]
> Küme ölçeklendirme çalıştıran [geçirme](https://redis.io/commands/migrate) pahalı bir komuttur, komut, bu nedenle en az etki için göz önünde yoğun olmayan saatlerde bu işlem çalıştırılmadan. Geçiş işlemi sırasında Sunucu yükünü bir ani değişiklik görürsünüz. Küme ölçeklendirme, uzun bir işlem çalışıyor ve geçen süreyi anahtar sayısı ve bu anahtarla ilişkili değerler boyutuna bağlıdır.
> 
> 

## <a name="clustering-faq"></a>Kümeleme sık sorulan sorular
Aşağıdaki listede olan Azure önbelleği için Redis kümeleme sık sorulan soruların yanıtlarını içerir.

* [Herhangi bir değişiklik kümesi kullanmak için istemci uygulamamın gerekiyor mu?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
* [Anahtarları, bir kümede nasıl dağıtılır?](#how-are-keys-distributed-in-a-cluster)
* [Oluşturabilmeniz için en büyük önbellek boyutu nedir?](#what-is-the-largest-cache-size-i-can-create)
* [Tüm Redis istemcilerinin, kümeleme destekliyor musunuz?](#do-all-redis-clients-support-clustering)
* [Kümeleme etkinleştirildiğinde önbelleğimin için nasıl bağlanabilirim?](#how-do-i-connect-to-my-cache-when-clustering-is-enabled)
* [Doğrudan de önbelleğimin bireysel parçalar için bağlanabilir miyim?](#can-i-directly-connect-to-the-individual-shards-of-my-cache)
* [Önceden oluşturulmuş bir önbelleği için kümelemeyi yapılandırabilir miyim?](#can-i-configure-clustering-for-a-previously-created-cache)
* [Temel veya standart önbelleği için kümelemeyi yapılandırabilir miyim?](#can-i-configure-clustering-for-a-basic-or-standard-cache)
* [Redis ASP.NET oturum durumu ve çıktı önbelleği sağlayıcılarıyla kümeleme kullanabilir miyim?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)
* [StackExchange.Redis kullanırken TAŞIMA özel durumları alıyorum ve kümeleme, ne yapmam gerekir?](#i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do)

### <a name="do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering"></a>Herhangi bir değişiklik kümesi kullanmak için istemci uygulamamın gerekiyor mu?
* Yalnızca 0 veritabanı Kümelemesi etkin olduğunda kullanılabilir. İstemci uygulamanızı birden çok veritabanı kullanıyorsa ve okuma veya 0'dan farklı bir veritabanına yazma çalışır, şu özel durum oluşturulur. `Unhandled Exception: StackExchange.Redis.RedisConnectionException: ProtocolFailure on GET --->` `StackExchange.Redis.RedisCommandException: Multiple databases are not supported on this server; cannot switch to database: 6`
  
  Daha fazla bilgi için [Redis kümesi belirtimi - uygulanan alt](https://redis.io/topics/cluster-spec#implemented-subset).
* Kullanıyorsanız [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/), 1.0.481 kullanmalısınız veya üzeri. Aynı kullanarak önbelleğe bağlanma [uç noktaları, bağlantı noktalarını ve anahtarları](cache-configure.md#properties) kümeleme özellikli olmayan bir önbellek bağlanırken kullandığınız. Tek fark, tüm okuma ve yazma işlemleri veritabanına 0 yapılması gerekir.
  
  * Diğer istemciler farklı gereksinimleri olabilir. Bkz: [yapmak, tüm Redis istemcileri desteklemek kümeleme?](#do-all-redis-clients-support-clustering)
* Uygulamanız birden çok anahtar işlemleri tek bir komutta toplu kullanıyorsa, tüm anahtarlar aynı parçada bulunması gerekir. Anahtarları aynı parçada bulmak için bkz. [nasıl anahtarları dağıtılmış bir kümede mi?](#how-are-keys-distributed-in-a-cluster)
* Redis ASP.NET oturum durumu sağlayıcısı kullanıyorsanız 2.0.1 kullanmanız gerekir ya da daha yüksek. Bkz: [Redis ASP.NET oturum durumu ve çıktı önbelleği sağlayıcılarıyla kümeleme kullanabilir miyim?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)

### <a name="how-are-keys-distributed-in-a-cluster"></a>Anahtarları, bir kümede nasıl dağıtılır?
Redis başına [anahtarları dağıtım modeli](https://redis.io/topics/cluster-spec#keys-distribution-model) belgeleri: Anahtar alanı 16384'tür yuvaları ayrılır. Her anahtarı karma ve tüm küme düğümlerine dağıtılmış bu yuvaları birine atanır. Birden çok anahtar karması etiketleri kullanarak aynı parçada bulunduğundan emin olmak için hangi anahtarın parçası karma yapılandırabilirsiniz.

* Anahtar herhangi bir bölümünü içerisindeyse karma etiketiyle - anahtarları `{` ve `}`, yalnızca bu anahtarın parçası anahtar karması yuvası belirleme amacıyla karma uygulanır. Örneğin, aşağıdaki 3 anahtarlarını aynı parçada bulunur: `{key}1`, `{key}2`, ve `{key}3` olduğundan yalnızca `key` adının bir parçası karma. Anahtarları karması etiketi belirtimleri tam bir listesi için bkz. [anahtarlarını karma etiketleri](https://redis.io/topics/cluster-spec#keys-hash-tags).
* Anahtarları karması etiketi - tüm anahtar adı olmadan, karma işleme tabi tutulması için kullanılır. Bu, önbelleğin parçalar arasında istatistiksel olarak eşit dağıtım sonuçlanır.

En iyi performans ve aktarım hızı için anahtarların eşit olarak dağıtma öneririz. Anahtarları kullanıyorsanız, anahtarları emin olmak için uygulamanın sorumluluğudur bir karma etiketi eşit olarak dağıtılmış.

Daha fazla bilgi için [anahtarları dağıtım modeli](https://redis.io/topics/cluster-spec#keys-distribution-model), [Redis küme veri parçalama](https://redis.io/topics/cluster-tutorial#redis-cluster-data-sharding), ve [anahtarlarını karma etiketleri](https://redis.io/topics/cluster-spec#keys-hash-tags).

Kümeleme ve anahtarları, StackExchange.Redis istemci ile aynı parçada bulma çalışmak için örnek kod, bkz: [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) kısmı [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) örnek.

### <a name="what-is-the-largest-cache-size-i-can-create"></a>Oluşturabilmeniz için en büyük önbellek boyutu nedir?
En büyük premium önbellek boyutu 53 GB'dir. En fazla 10 parça 530 GB'lık boyut sınırı vererek oluşturabilirsiniz. Daha büyük bir boyutu gerekiyorsa [daha fazla istek](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Daha fazla bilgi için [Azure önbelleği için Redis fiyatlandırma](https://azure.microsoft.com/pricing/details/cache/).

### <a name="do-all-redis-clients-support-clustering"></a>Tüm Redis istemcilerinin, kümeleme destekliyor musunuz?
Tüm istemciler desteği şu anda Redis kümeleme. StackExchange.Redis için destek biridir. Diğer istemciler hakkında daha fazla bilgi için bkz. [kümeyle yürütmeyi](https://redis.io/topics/cluster-tutorial#playing-with-the-cluster) bölümünü [Redis kümesi Öğreticisi](https://redis.io/topics/cluster-tutorial). 

Redis küme protokolü, her parça kümeleme modunda doğrudan bağlanmak her bir istemciye gerektirir. Kümeleme neden olma olasılığı çok sayıda desteklemeyen istemci kullanma girişiminde [TAŞINDI yeniden yönlendirme özel durumları](https://redis.io/topics/cluster-spec#moved-redirection).

> [!NOTE]
> StackExchange.Redis istemci kullanıyorsanız, en son sürümünü kullandığınızdan emin olun [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) 1.0.481 veya düzgün çalışması için kümeleme sonrası. Taşıma özel durumlar herhangi bir sorun varsa, bkz: [özel durumları taşıma](#move-exceptions) daha fazla bilgi için.
> 
> 

### <a name="how-do-i-connect-to-my-cache-when-clustering-is-enabled"></a>Kümeleme etkinleştirildiğinde önbelleğimin için nasıl bağlanabilirim?
Aynı kullanarak önbelleğinizi bağlanabilir [uç noktaları](cache-configure.md#properties), [bağlantı noktaları](cache-configure.md#properties), ve [anahtarları](cache-configure.md#access-keys) kümeleme özellikli olmayan bir önbellek bağlanırken kullandığınız. Redis istemcinizden yönetmek zorunda kalmamak için arka uçta kümeleme yönetir.

### <a name="can-i-directly-connect-to-the-individual-shards-of-my-cache"></a>Doğrudan de önbelleğimin bireysel parçalar için bağlanabilir miyim?
Kümeleme protokolü, istemcinin doğru parça bağlantılarını yapması gerekir. Bu nedenle istemci doğru sizin için bunu yapmalısınız. Bu kullanıcıyla Bununla birlikte, her parça topluca bir önbellek örneği bilinen bir birincil/çoğaltma önbellek çifti oluşur. Redis CLI yardımcı programı kullanarak bu önbellek örnekleri bağlanabilirsiniz [kararsız](https://redis.io/download) GitHub Redis deponun dalı. Bu sürüm ile başlatıldığında temel desteği uygulayan `-c` geçin. Daha fazla bilgi için [kümeyle yürütmeyi](https://redis.io/topics/cluster-tutorial#playing-with-the-cluster) üzerinde [ https://redis.io ](https://redis.io) içinde [Redis kümesi Öğreticisi](https://redis.io/topics/cluster-tutorial).

Ssl olmayan için aşağıdaki komutları kullanın.

    Redis-cli.exe –h <<cachename>> -p 13000 (to connect to instance 0)
    Redis-cli.exe –h <<cachename>> -p 13001 (to connect to instance 1)
    Redis-cli.exe –h <<cachename>> -p 13002 (to connect to instance 2)
    ...
    Redis-cli.exe –h <<cachename>> -p 1300N (to connect to instance N)

SSL için değiştirin `1300N` ile `1500N`.

### <a name="can-i-configure-clustering-for-a-previously-created-cache"></a>Önceden oluşturulmuş bir önbelleği için kümelemeyi yapılandırabilir miyim?
Şu anda bir önbellek oluşturduğunuzda kümeleme yalnızca etkinleştirebilirsiniz. Önbellek oluşturuldu, ancak bir premium önbelleği için kümelemeyi ekleyin veya bir premium önbellekten önbellek oluşturulduktan sonra küme kaldırma sonra kümenizin boyutunu değiştirebilirsiniz. Bir premium önbellek kümeleme özelliği etkinleştirilmiş ve tek parça ile hiçbir Kümeleme ile aynı boyutta bir premium önbellek farklıdır.

### <a name="can-i-configure-clustering-for-a-basic-or-standard-cache"></a>Temel veya standart önbelleği için kümelemeyi yapılandırabilir miyim?
Kümeleme yalnızca premium önbellekler için kullanılabilir.

### <a name="can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers"></a>Redis ASP.NET oturum durumu ve çıktı önbelleği sağlayıcılarıyla kümeleme kullanabilir miyim?
* **Çıkış önbelleği sağlayıcısı redis** -değişiklik gerekli değil.
* **Redis oturum durumu sağlayıcısı** - kümeleme, kullanılacak kullanmalısınız [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 veya daha yüksek veya farklı bir özel durum oluşturulur. Bu, bir değişiklik, Daha fazla bilgi için [v2.0.0 bozucu değişiklik ayrıntıları](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

<a name="move-exceptions"></a>

### <a name="i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do"></a>StackExchange.Redis kullanırken TAŞIMA özel durumları alıyorum ve kümeleme, ne yapmam gerekir?
StackExchange.Redis kullanıyorsanız ve alma `MOVE` kümeleme, kullanılırken özel durumları emin olmak için kullandığınız [StackExchange.Redis 1.1.603](https://www.nuget.org/packages/StackExchange.Redis/) veya üzeri. StackExchange.Redis kullanmak için .NET uygulamaları için yapılandırma ile ilgili yönergeler için bkz: [önbellek istemcilerini yapılandırma](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla premium önbellek özelliklerini kullanmayı öğrenin.

* [Azure önbelleği için Redis Premium katmanına giriş](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-clustering]: ./media/cache-how-to-premium-clustering/redis-cache-clustering.png

[redis-cache-clustering-selected]: ./media/cache-how-to-premium-clustering/redis-cache-clustering-selected.png

[redis-cache-redis-cluster-size]: ./media/cache-how-to-premium-clustering/redis-cache-redis-cluster-size.png







