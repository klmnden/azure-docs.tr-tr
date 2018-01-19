---
title: "Redis Premium Azure Redis önbelleği için kümeleri yapılandırma | Microsoft Docs"
description: "Oluşturma ve Azure Redis önbelleği örnekleri için Premium katman kümeleme Redis yönetme hakkında bilgi edinin"
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 62208eec-52ae-4713-b077-62659fd844ab
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: wesmc
ms.openlocfilehash: 16281cca4e4bc95e145317365d42382ab11fde93
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-configure-redis-clustering-for-a-premium-azure-redis-cache"></a>Redis Premium Azure Redis önbelleği için kümeleri yapılandırma
Azure Redis önbelleği, önbellek boyutunu ve özelliklerini, kümeleme, sürdürme ve sanal ağ desteği gibi Premium katmanı özellikleri dahil olmak üzere seçimi esneklik sağlayan farklı önbellek teklifleri vardır. Bu makalede, premium Azure Redis önbelleği örneği içinde kümelemeyi yapılandırmayı açıklar.

Diğer premium önbellek özellikleri hakkında daha fazla bilgi için bkz: [Azure Redis önbelleği Premium katmanına giriş](cache-premium-tier-intro.md).

## <a name="what-is-redis-cluster"></a>Redis kümesi nedir?
Azure Redis önbelleği Redis kümesi olarak sunar [Redis içinde uygulanan](http://redis.io/topics/cluster-tutorial). Redis kümesi ile aşağıdaki faydaları alın: 

* Otomatik olarak birden çok düğüm arasında veri kümenizi bölme yeteneği. 
* İşlemlerini alt düğümlerin devam etme olanağı hataları yaşıyor veya kümenin geri kalanı ile iletişim kuramıyor. 
* Daha fazla verimlilik: verimliliğini artırır doğrusal olarak parça sayısı arttıkça. 
* Daha fazla bellek boyutu: parça sayısı arttıkça doğrusal olarak artırır.  

Boyut, işleme ve premium önbelleklere sahip bant genişliği hakkında daha fazla bilgi için bkz: [hangi Redis önbelleği teklifini ve boyutunu kullanmalıyım?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

Azure'da Redis kümesi her parça çoğaltma birincil/çoğaltma çiftiyle çoğaltma Azure Redis önbelleği hizmeti tarafından yönetildiği sahip olduğu birincil/çoğaltma modeli olarak sunulur. 

## <a name="clustering"></a>Kümeleme
Kümeleme üzerinde etkindir **yeni Redis önbelleği** dikey önbellek oluşturma sırasında. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Kümeleme yapılandırılır **Redis küme** dikey.

![Kümeleme][redis-cache-clustering]

Küme içindeki en fazla 10 parça olabilir. Tıklatın **etkin** kaydırıcısını kaydırın ve 1 ile 10 arasında bir sayı yazın **parça sayısı** tıklatıp **Tamam**.

Her parça Azure tarafından yönetilen bir birincil/çoğaltma önbelleği çifti ve fiyatlandırma katmanı seçili önbellek boyutu parça sayısı çarparak önbelleğinin toplam boyutu hesaplanır. 

![Kümeleme][redis-cache-clustering-selected]

Önbellek oluşturulduktan sonra buna bağlanmak ve yalnızca kümelenmiş olmayan bir önbellek ister ve Redis kullanın veri önbelleği parça genelinde dağıtır. Tanılama ise [etkin](cache-how-to-monitor.md#enable-cache-diagnostics), ölçümleri her parça için ayrı ayrı yakalanır ve olabilir [görüntülenen](cache-how-to-monitor.md) Redis önbelleği dikey penceresinde. 

> [!NOTE]
> 
> Kümeleme yapılandırıldığında, istemci uygulamanız gereken bazı küçük farklar vardır. Daha fazla bilgi için bkz: [kümeleme kullanmak üzere istemci Uygulamam herhangi bir değişiklik yapmanız gerekiyor mu?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 

Örnek kod StackExchange.Redis istemcisi ile kümeleme ile çalışma hakkında bkz [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) kısmı [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) örnek.

<a name="cluster-size"></a>

## <a name="change-the-cluster-size-on-a-running-premium-cache"></a>Üzerinde çalışan bir küme boyutunu değiştirme premium önbelleği
Üzerinde çalışan bir küme boyutunu değiştirmek için etkin, kümeleme ile premium Önbelleği'ni tıklatın **Redis küme boyutu** gelen **kaynak menü**.

> [!NOTE]
> Azure Redis önbelleği Premium katmanına genel kullanılabilirlik yayımlanan olsa da, Redis küme boyutu özelliği şu anda önizlemede değil.
> 
> 

![Küme boyutu redis][redis-cache-redis-cluster-size]

Küme boyutunu değiştirmek için kaydırıcıyı kullanın veya 1 ile 10 arasında bir sayı yazın **parça sayısı** metin kutusu ve tıklatın **Tamam** kaydetmek için.

> [!NOTE]
> Küme ölçeklendirme çalıştıran [geçirme](https://redis.io/commands/migrate) pahalı bir komuttur, komutu, bu nedenle düşük etkili için göz önünde bulundurun yoğun olmayan saatlerde bu işlem çalıştırılmadan. Geçiş işlemi sırasında bir ani artış sunucu iş yükü görürsünüz. Bir küme ölçeklendirme uzun işlemi çalışıyor ve geçen süre miktarı anahtar sayısı ve bu anahtarla ilişkili değerler boyutunu bağlıdır.
> 
> 

## <a name="clustering-faq"></a>Kümeleme SSS
Aşağıdaki listede, Azure Redis önbelleği kümeleri hakkında sık sorulan soruların yanıtlarını içerir.

* [Herhangi bir değişiklik kümeleme kullanmak üzere istemci uygulamam gerekiyor mu?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
* [Anahtarları bir kümede nasıl dağıtıldığını?](#how-are-keys-distributed-in-a-cluster)
* [Oluşturabilmeniz için en büyük önbellek boyutu nedir?](#what-is-the-largest-cache-size-i-can-create)
* [Tüm Redis istemcileri kümeleme destekliyor musunuz?](#do-all-redis-clients-support-clustering)
* [Kümeleme etkinleştirildiğinde nasıl my önbelleğine bağlayabilirim?](#how-do-i-connect-to-my-cache-when-clustering-is-enabled)
* [Doğrudan de benim önbellek tek tek parça için bağlanabilir miyim?](#can-i-directly-connect-to-the-individual-shards-of-my-cache)
* [Önceden oluşturulmuş bir önbelleği için kümeleri yapılandırabilir miyim?](#can-i-configure-clustering-for-a-previously-created-cache)
* [Bir temel veya standart önbelleği için kümeleri yapılandırabilir miyim?](#can-i-configure-clustering-for-a-basic-or-standard-cache)
* [Redis ASP.NET oturum durumu ve çıktı önbelleği sağlayıcılarıyla kümeleme kullanabilir miyim?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)
* [StackExchange.Redis kullanırken TAŞIMA özel durumlar alıyorum ve kümeleme, ne yapmalıyım?](#i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do)

### <a name="do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering"></a>Herhangi bir değişiklik kümeleme kullanmak üzere istemci uygulamam gerekiyor mu?
* Yalnızca veritabanı 0 Kümeleme etkin olduğunda kullanılabilir. İstemci uygulamanız birden çok veritabanını kullanır ve okuma veya 0 dışında bir veritabanına yazma çalışırsa, aşağıdaki özel durum oluşur. `Unhandled Exception: StackExchange.Redis.RedisConnectionException: ProtocolFailure on GET --->` `StackExchange.Redis.RedisCommandException: Multiple databases are not supported on this server; cannot switch to database: 6`
  
  Daha fazla bilgi için bkz: [küme belirtimi Redis - uygulanan alt](http://redis.io/topics/cluster-spec#implemented-subset).
* Kullanıyorsanız [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/), 1.0.481 kullanın veya sonraki bir sürümü. Aynı kullanarak önbelleğe bağlanma [uç noktaları, bağlantı noktalarını ve anahtarları](cache-configure.md#properties) kümeleme özelliği etkinleştirilmiş sahip olmayan bir önbellek bağlanırken kullandığınız. 0 veritabanı tüm okuma ve yazma işlemleri yapılmalıdır yalnızca farktır.
  
  * Diğer istemciler farklı gereksinimlere sahip. Bkz: [yapmak, tüm Redis istemcileri desteklemek kümeleme?](#do-all-redis-clients-support-clustering)
* Uygulamanızın tek bir komut toplu birden çok anahtar işlemleri kullanıyorsa, tüm anahtarları aynı parça içinde yer almalıdır. İçinde aynı parça anahtarlarını bulmak için bkz: [nasıl anahtarları dağıtılır bir kümede mi?](#how-are-keys-distributed-in-a-cluster)
* Redis ASP.NET oturum durumu sağlayıcısı kullanıyorsanız 2.0.1 kullanmanız gerekir ya da daha yüksek. Bkz: [Redis ASP.NET oturum durumu ve çıktı önbelleği sağlayıcılarıyla kümeleme kullanabilir miyim?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)

### <a name="how-are-keys-distributed-in-a-cluster"></a>Anahtarları bir kümede nasıl dağıtıldığını?
Redis başına [anahtarları dağıtım modeli](http://redis.io/topics/cluster-spec#keys-distribution-model) belgelerine: anahtar alanı 16384 yuvaları ayrılır. Her anahtar karma ve bir küme düğümüne dağıtılmış bu yuva atanmış. Birden çok anahtar karma etiketleri kullanarak aynı parça içinde bulunan emin olmak için hangi anahtarın parçası karma yapılandırabilirsiniz.

* Anahtar, herhangi bir parçası olarak arasına alınmışsa karma etiketiyle - anahtarları `{` ve `}`, yalnızca bu anahtarın parçası anahtar karma yuvası belirlemek amacıyla karma haline getirilir. Örneğin, aşağıdaki 3 anahtarlarını aynı parça içinde bulunur: `{key}1`, `{key}2`, ve `{key}3` beri yalnızca `key` adının bir parçası karma. Anahtarları karma etiketi belirtimleri tam bir listesi için bkz: [anahtarları karma etiketleri](http://redis.io/topics/cluster-spec#keys-hash-tags).
* Anahtarları bir karma etiketi - tüm anahtar adı olmadan, karma oluşturma için kullanılır. Bu önbellek parça istatistiksel olarak bile dağıtımlarında sonuçlanır.

En iyi performans ve verimlilik için anahtarları eşit dağıtma öneririz. Anahtarları ile kullanıyorsanız, anahtarları emin olmak için uygulamanın sorumluluğundadır karma etiketi eşit olarak dağıtılmış.

Daha fazla bilgi için bkz: [anahtarları dağıtım modeli](http://redis.io/topics/cluster-spec#keys-distribution-model), [küme Redis veri parçalama](http://redis.io/topics/cluster-tutorial#redis-cluster-data-sharding), ve [anahtarları karma etiketleri](http://redis.io/topics/cluster-spec#keys-hash-tags).

Örnek kod Kümeleme ve anahtarları StackExchange.Redis istemcisi ile aynı parça bulma ile çalışma hakkında bkz [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) kısmı [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) örnek.

### <a name="what-is-the-largest-cache-size-i-can-create"></a>Oluşturabilmeniz için en büyük önbellek boyutu nedir?
En büyük premium önbellek boyutu 53 GB'dir. En fazla 10 parça 530 GB en büyük boyutunu vermiş oluşturabilirsiniz. Daha büyük bir boyutu gerekiyorsa yapabilecekleriniz [daha fazla isteği](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Daha fazla bilgi için bkz: [Azure Redis önbelleği fiyatlandırma](https://azure.microsoft.com/pricing/details/cache/).

### <a name="do-all-redis-clients-support-clustering"></a>Tüm Redis istemcileri kümeleme destekliyor musunuz?
Şu anda tüm istemcileri destek Redis kümeleme. StackExchange.Redis için destek biridir. Diğer istemciler hakkında daha fazla bilgi için bkz: [kümeyle çalma](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) bölümünü [Redis küme Öğreticisi](http://redis.io/topics/cluster-tutorial).

> [!NOTE]
> StackExchange.Redis istemcisi olarak kullanıyorsanız, en son sürümünü kullandığınızdan emin olun [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) 1.0.481 veya düzgün çalışması için kümeleme sonrası. Taşıma özel durumlar herhangi bir sorun varsa, bkz: [özel durumlar taşıma](#move-exceptions) daha fazla bilgi için.
> 
> 

### <a name="how-do-i-connect-to-my-cache-when-clustering-is-enabled"></a>Kümeleme etkinleştirildiğinde nasıl my önbelleğine bağlayabilirim?
Aynı kullanarak önbelleğiniz bağlanabilir [uç noktaları](cache-configure.md#properties), [bağlantı noktalarını](cache-configure.md#properties), ve [anahtarları](cache-configure.md#access-keys) kümeleme özelliği etkinleştirilmiş sahip olmayan bir önbellek bağlanırken kullandığınız. Redis istemcinizden yönetmek zorunda kalmamak için arka uç kümeleme yönetir.

### <a name="can-i-directly-connect-to-the-individual-shards-of-my-cache"></a>Doğrudan de benim önbellek tek tek parça için bağlanabilir miyim?
Bu resmi olarak desteklenmez. İle denirse her parça topluca bir önbellek örneği olarak bilinen bir birincil/çoğaltma önbelleği çifti oluşur. Redis-cli yardımcı programı kullanarak bu önbelleği örnekleri bağlanabileceği [kararsız](http://redis.io/download) github'da Redis deponun dalı. Bu sürüm ile başlatıldığında temel destek uygulayan `-c` geçin. Daha fazla bilgi için bkz: [kümeyle çalma](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) üzerinde [http://redis.io](http://redis.io) içinde [Redis küme Öğreticisi](http://redis.io/topics/cluster-tutorial).

Ssl olmayan için aşağıdaki komutları kullanın.

    Redis-cli.exe –h <<cachename>> -p 13000 (to connect to instance 0)
    Redis-cli.exe –h <<cachename>> -p 13001 (to connect to instance 1)
    Redis-cli.exe –h <<cachename>> -p 13002 (to connect to instance 2)
    ...
    Redis-cli.exe –h <<cachename>> -p 1300N (to connect to instance N)

SSL için değiştirme `1300N` ile `1500N`.

### <a name="can-i-configure-clustering-for-a-previously-created-cache"></a>Önceden oluşturulmuş bir önbelleği için kümeleri yapılandırabilir miyim?
Şu anda bir önbellek oluşturduğunuzda, kümeleme yalnızca etkinleştirebilirsiniz. Önbellek oluşturuldu, ancak premium önbelleği için kümeleri eklemek veya önbellek oluşturulduktan sonra bir premium önbellekten kümeleme kaldırmak sonra küme boyutunu değiştirebilirsiniz. Kümeleme özelliği etkinleştirilmiş ve yalnızca bir parça premium önbelleği premium önbelleği hiçbir Kümeleme ile aynı boyutta farklıdır.

### <a name="can-i-configure-clustering-for-a-basic-or-standard-cache"></a>Bir temel veya standart önbelleği için kümeleri yapılandırabilir miyim?
Kümeleme yalnızca premium önbellekler için kullanılabilir.

### <a name="can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers"></a>Redis ASP.NET oturum durumu ve çıktı önbelleği sağlayıcılarıyla kümeleme kullanabilir miyim?
* **Çıkış önbelleği sağlayıcısı redis** -gerekli herhangi bir değişiklik.
* **Redis oturum durumu sağlayıcısı** - kümeleme, kullanılacak kullanmalısınız [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 ya da daha yüksek veya farklı bir özel durum oluşturulur. Önemli bir değişiklik budur; Daha fazla bilgi için bkz: [v2.0.0 sonu değişiklik ayrıntıları](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

<a name="move-exceptions"></a>

### <a name="i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do"></a>StackExchange.Redis kullanırken TAŞIMA özel durumlar alıyorum ve kümeleme, ne yapmalıyım?
StackExchange.Redis kullanıyorsanız ve alma `MOVE` kümeleme, kullanırken özel durumlar emin olun, kullandığınız [StackExchange.Redis 1.1.603](https://www.nuget.org/packages/StackExchange.Redis/) veya sonraki bir sürümü. StackExchange.Redis kullanmak için .NET uygulamaları yapılandırma ile ilgili yönergeler için bkz: [önbellek istemcilerini yapılandırma](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla premium önbellek özelliklerinin nasıl kullanılacağını öğrenin.

* [Azure Redis önbelleği Premium katmanına giriş](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-clustering]: ./media/cache-how-to-premium-clustering/redis-cache-clustering.png

[redis-cache-clustering-selected]: ./media/cache-how-to-premium-clustering/redis-cache-clustering-selected.png

[redis-cache-redis-cluster-size]: ./media/cache-how-to-premium-clustering/redis-cache-redis-cluster-size.png







