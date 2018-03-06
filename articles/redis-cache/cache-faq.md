---
title: "Azure Redis önbelleği SSS | Microsoft Docs"
description: "Azure Redis önbelleği için sık sorulan sorular, desenleri ve en iyi yöntemler yanıtlarını öğrenin"
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: c2c52b7d-b2d1-433a-b635-c20180e5cab2
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: wesmc
ms.openlocfilehash: 82c01419d65e00ddf27dfeb8fd444d5d3d81803c
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
---
# <a name="azure-redis-cache-faq"></a>Azure Redis Cache SSS
Azure Redis önbelleği için sık sorulan sorular, desenleri ve en iyi yöntemler yanıtlarını öğrenin.

## <a name="what-if-my-question-isnt-answered-here"></a>Ne sorumun cevabı burada cevaplanıp değil mi?
Sorunuzun yanıtını burada listelenmiyorsa, bize bildirin ve yanıt bulmanıza yardımcı olacağız.

* Bu SSS, sonunda açıklamalarında bir soru gönderin ve Azure önbelleği ekip ve diğer topluluk üyeleri bu makale hakkında göster.
* Daha geniş bir kitleye ulaşmak için bir soru üzerinde nakledebilirsiniz [Azure önbelleği MSDN Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) ve Azure önbelleği ekip ve diğer topluluk üyeleri göster.
* Özellik isteği yapmak istiyorsanız, istekleri ve fikir için gönderebilirsiniz [Azure Redis önbelleği kullanıcı sesi](https://feedback.azure.com/forums/169382-cache).
* Bizimle için bir e-posta gönderebilirsiniz [Azure önbelleği dış geri bildirim](mailto:azurecache@microsoft.com).

## <a name="azure-redis-cache-basics"></a>Azure Redis önbelleği temel kavramları
Bu bölümdeki SSS bazı Azure Redis önbelleği temelleri kapsar.

* [Azure Redis Önbelleği nedir?](#what-is-azure-redis-cache)
* [Azure Redis önbelleği ile çalışmaya nasıl?](#how-can-i-get-started-with-azure-redis-cache)

Aşağıdaki sık sorulan sorular temel kavramları ve Azure Redis önbelleği hakkında sorular kapsar ve diğer sık sorulan sorular bölümlerden birine yanıtlanır.

* [Hangi Redis Önbelleği teklifini ve boyutunu kullanmalıyım?](#what-redis-cache-offering-and-size-should-i-use)
* [Hangi Redis önbelleği istemcileri kullanabilir miyim?](#what-redis-cache-clients-can-i-use)
* [Azure Redis önbelleği için yerel bir öykünücü var mı?](#is-there-a-local-emulator-for-azure-redis-cache)
* [Sistem durumunu ve performansını my önbelleğinin nasıl izlerim?](#how-do-i-monitor-the-health-and-performance-of-my-cache)

## <a name="planning-faqs"></a>Sık sorulan sorular planlama
* [Hangi Redis Önbelleği teklifini ve boyutunu kullanmalıyım?](#what-redis-cache-offering-and-size-should-i-use)
* [Azure Redis önbelleği performansı](#azure-redis-cache-performance)
* [Hangi bölgede ı my önbellek bulun?](#in-what-region-should-i-locate-my-cache)
* [Azure Redis önbelleği için nasıl faturalandırılır?](#how-am-i-billed-for-azure-redis-cache)
* [Azure Redis önbelleği Azure Bulutu, Azure Çin Bulut veya Microsoft Azure Almanya kullanabilir miyim?](#can-i-use-azure-redis-cache-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany)

## <a name="development-faqs"></a>Geliştirme ile ilgili SSS
* [StackExchange.Redis yapılandırma seçeneklerini ne yapacaksınız?](#what-do-the-stackexchangeredis-configuration-options-do)
* [Hangi Redis önbelleği istemcileri kullanabilir miyim?](#what-redis-cache-clients-can-i-use)
* [Azure Redis önbelleği için yerel bir öykünücü var mı?](#is-there-a-local-emulator-for-azure-redis-cache)
* [Redis komutları nasıl çalıştırabilir miyim?](#how-can-i-run-redis-commands)
* [Neden Azure Redis önbelleği bazı diğer Azure Hizmetleri gibi bir MSDN sınıf kitaplığı başvurusu yok?](#why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
* [Azure Redis önbelleği PHP oturum önbelleği olarak kullanabilir miyim?](#can-i-use-azure-redis-cache-as-a-php-session-cache)
* [Redis veritabanları nelerdir?](#what-are-redis-databases)

## <a name="security-faqs"></a>Güvenlik ile ilgili SSS
* [Redis için bağlanmak için SSL olmayan bağlantı noktası etkinleştirdiğinizde?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)

## <a name="production-faqs"></a>Üretim SSS
* [Bazı üretim en iyi uygulamalar nelerdir?](#what-are-some-production-best-practices)
* [Bazı konuları ortak Redis komutlarını kullanırken nelerdir?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
* [Nasıl Kıyaslama ve my önbellek performansını test etme?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [İş parçacığı havuzu büyüme hakkında önemli ayrıntıları](#important-details-about-threadpool-growth)
* [Daha fazla verimlilik StackExchange.Redis kullanırken almak sunucu GC etkinleştir](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)
* [Bağlantıları geçici performans etkenleri](#performance-considerations-around-connections)

## <a name="monitoring-and-troubleshooting-faqs"></a>İzleme ve sık sorulan sorular sorun giderme
Bu bölümdeki SSS ortak izleme ve sorun giderme soruları kapsar. İzleme ve Azure Redis önbelleği örnekleri sorun giderme hakkında daha fazla bilgi için bkz: [Azure Redis önbelleği izleme](cache-how-to-monitor.md) ve [Azure Redis önbelleği ile ilgili sorunları giderme](cache-how-to-troubleshoot.md).

* [Sistem durumunu ve performansını my önbelleğinin nasıl izlerim?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
* [Zaman aşımları neden görüyorum?](#why-am-i-seeing-timeouts)
* [Neden my istemci önbelleğinden kesildi?](#why-was-my-client-disconnected-from-the-cache)

## <a name="prior-cache-offering-faqs"></a>Önceki önbelleği teklifini SSS
* [Hangi Azure önbelleği teklifini benim için en uygun mi?](#which-azure-cache-offering-is-right-for-me)

### <a name="what-is-azure-redis-cache"></a>Azure Redis Önbelleği nedir?
Azure Redis önbelleği popüler açık kaynak üzerinde temel [Redis önbelleği](http://redis.io). Erişim, Azure içinde herhangi bir uygulamadan bir güvenli, ayrılmış bir Redis önbelleğine için Microsoft tarafından yönetilen ve erişilebilir sağlar. Daha ayrıntılı bir bakış için bkz: [Azure Redis önbelleği](https://azure.microsoft.com/services/cache/) Azure.com üzerindeki ürün sayfası.

### <a name="how-can-i-get-started-with-azure-redis-cache"></a>Azure Redis önbelleği ile çalışmaya nasıl?
Azure Redis önbelleği kullanmaya başlamak birkaç yolu vardır.

* Kullanılabilir öğreticilerimizden birini çıkışı denetleyebilirsiniz [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md), ve [Python](cache-python-get-started.md).
* İzleyebilir [yapı yüksek performanslı uygulamalar kullanarak Microsoft Azure Redis önbelleği nasıl](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/).
* Redis kullanılması hakkında bilgi için projenizin geliştirme dilini eşleşen istemciler için istemci belgelere kontrol edebilirsiniz. Azure Redis önbelleği ile kullanılan birçok Redis istemcileri vardır. Redis istemcileri listesi için bkz: [http://redis.io/clients](http://redis.io/clients).

Zaten bir Azure hesabınız yoksa, şunları yapabilirsiniz:

* [Ücretsiz bir Azure hesabı açın](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). Ücretli Azure hizmetlerini denemek için kullanabileceğiniz krediler alırsınız. Krediler bitmiş olsa bile hesabı sürdürebilir ve ücretsiz Azure hizmet ve özelliklerinden faydalanabilirsiniz.
* [Visual Studio abone avantajları etkinleştirin](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). MSDN aboneliğiniz, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay size kredi verir.

<a name="cache-size"></a>

### <a name="what-redis-cache-offering-and-size-should-i-use"></a>Hangi Redis Önbelleği teklifini ve boyutunu kullanmalıyım?
Her Azure Redis önbelleği teklifini farklı düzeylerde sağlar **boyutu**, **bant genişliği**, **yüksek kullanılabilirlik**, ve **SLA** seçenekleri.

Önbelleği teklifini seçme hakkında önemli noktalar şunlardır:

* **Bellek**: temel ve standart katmanları, 250 MB – 53 GB'a sunar. Premium katmanı 530 GB sunar. Daha fazla bilgi için bkz: [Azure Redis önbelleği fiyatlandırma](https://azure.microsoft.com/pricing/details/cache/).
* **Ağ performansı**: yüksek işleme gerektiren bir iş yükü varsa, Premium katmanı için standart veya Basic karşılaştırıldığında daha fazla bant genişliği sunar. Ayrıca her katman içinde önbellek barındıran temeldeki VM nedeniyle daha fazla bant genişliği daha büyük boyutu önbellekleri vardır. Bkz: [tablo aşağıdaki](#cache-performance) daha fazla bilgi için.
* **Üretilen iş**: Premium katmanı en yüksek kullanılabilir üretimi sunar. Önbellek sunucu veya istemci bant genişliği sınırlarını ulaşırsa, istemci tarafındaki zaman aşımı alabilirsiniz. Daha fazla bilgi için aşağıdaki tabloya bakın.
* **Yüksek kullanılabilirlik SLA**: Azure Redis önbelleği standart/Premium önbelleği en az % 99,9 zaman kullanılabilir olduğunu güvence altına alır. Bizim SLA hakkında daha fazla bilgi için bkz: [Azure Redis önbelleği fiyatlandırma](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). SLA yalnızca önbellek uç noktalarına bağlantı kapsar. SLA, veri kaybından korumayı kapsamaz. Redis veri kalıcılığını özelliği, veri kaybına karşı dayanıklılığı artırmak için Premium katmanındaki kullanmanızı öneririz.
* **Redis veri kalıcılığını**: Premium katmanı, bir Azure depolama hesabındaki önbellek verilerini sürdürülmesi olanak sağlar. Basic/standart önbelleğinde tüm verileri yalnızca bellekte depolanır. Temel alınan altyapı sorunları varsa olası veri kaybı olabilir. Redis veri kalıcılığını özelliği, veri kaybına karşı dayanıklılığı artırmak için Premium katmanındaki kullanmanızı öneririz. Azure Redis önbelleği RDB ve (yakında) AOF Redis kalıcılığı seçenekleri sunar. Daha fazla bilgi için bkz: [Premium Azure Redis önbelleği için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md).
* **Redis kümesi**: oluşturmak için 53 GB'den büyük önbelleğe alır ya da birden çok Redis düğümler arasında verileri için Redis kümeleme, Premium katmanında kullanılabilir olduğu kullanabilirsiniz. Her düğüm, yüksek kullanılabilirlik için birincil/çoğaltma önbelleği çiftinin oluşur. Daha fazla bilgi için bkz. [Premium Azure Redis Cache için kümeleri yapılandırma](cache-how-to-premium-clustering.md).
* **Gelişmiş Güvenlik ve ağ yalıtımı**: Gelişmiş Güvenlik ve yalıtım, Azure Redis önbelleği yanı sıra alt ağlar, erişim denetimi ilkeleri için Azure sanal ağ (VNET) dağıtım sağlar ve diğer özellikleri daha da fazla erişimi kısıtlayabilirsiniz. Daha fazla bilgi için bkz. [Premium Azure Redis Cache için Sanal Ağ desteğini yapılandırma](cache-how-to-premium-vnet.md).
* **Redis yapılandırma**: standart ve Premium katmanlarda, Redis için Keyspace bildirimleri yapılandırabilirsiniz.
* **İstemci bağlantısı sayısı**: Premium katmanı en fazla bağlantı büyük boyutlu önbellekler için daha yüksek bir sayı ile Redis bağlanabilir istemciyi sunar. Daha fazla bilgi için bkz: [Azure Redis önbelleği fiyatlandırma](https://azure.microsoft.com/pricing/details/cache/).
* **Redis sunucu için çekirdek ayrılmış**: Premium katmanındaki tüm önbellek boyutlarını adanmış bir çekirdek için Redis sahip. Basic/standart katmanları, C1 boyutu ve üzeri Redis sunucusu için ayrılmış bir çekirdek vardır.
* **Redis tek iş parçacıklı** ikiden fazla çekirdeğe sahip sağlamaz ek avantajı yalnızca iki çekirdeğe sahip üzerinden ancak büyük VM boyutları genellikle daha fazla bant genişliği daha küçük boyutlara sahiptir. Önbellek sunucu veya istemci bant genişliği sınırlarını ulaşırsa, istemci tarafındaki zaman aşımı alırsınız.
* **Performans iyileştirmeleri**: Premium katmanındaki önbellekleri, temel veya standart katmanına göre daha iyi performans vermiş daha hızlı işlemcilere sahip donanımda dağıtılır. Premium katmanı önbellekleri daha yüksek verimlilik ve düşük gecikme vardır.

<a name="cache-performance"></a>

### <a name="azure-redis-cache-performance"></a>Azure Redis önbelleği performansı
Aşağıdaki tablo standart çeşitli boyutlarda test ederken görülen en yüksek bant genişliği değerleri gösterir ve Premium önbellekleri kullanarak `redis-benchmark.exe` bir Iaas VM'den Azure Redis önbelleği uç noktası. SSL işleme için redis Kıyaslama stunnel ile Azure Redis önbelleği bitiş noktasına bağlanmak için kullanılır.

>[!NOTE] 
>Bu değerleri yıkıcıları ve bunlar için hiçbir SLA numaraları ancak tipik olması gerekir. Yüklemesi gerektiğini uygulamanız için doğru önbellek boyutunu belirlemek için kendi uygulamanızı test edin.
>Biz düzenli aralıklarla yeni sonuçlara sonrası gibi bu numaralarını değiştirebilirsiniz.
>

Bu tablodan biz aşağıdaki sonuçları çizebilirsiniz:

* Üretilen iş aynı boyuttadır önbellekler için standart katmana karşılaştırıldığında Premium katmanındaki yüksektir. Örneğin, bir 6 GB ile P1 verimini 180.000 RPS C3 100.000 karşılaştırıldığında önbelleğidir.
* Kümedeki parça (düğümlerin) sayısını artırmak kümeleme Redis ile üretilen işi doğrusal olarak artar. 10 parça P4 kümesi oluşturursanız, örneğin, kullanılabilir verimlilik 400.000 ise * 10 = 4 milyon RPS.
* Üretilen iş büyük anahtar boyutları için standart katmana karşılaştırıldığında Premium katmanındaki yüksektir.

| Fiyatlandırma katmanı | Boyut | CPU çekirdekleri | Kullanılabilir bant genişliği | 1 KB değer boyutu | 1 KB değer boyutu |
| --- | --- | --- | --- | --- | --- |
| **Standart önbellek boyutu** | | |**Megabit / sn (Mb/sn) / megabayt sayısı / sn (MB/sn)** |**İkinci (RPS) SSL olmayan başına istek sayısı** |**İkinci (RPS) SSL başına istek sayısı** |
| C0 |250 MB |Paylaşılan |100 / 12.5 |15,000 |7.500 |
| C1 |1 GB |1 |500 / 62.5 |38,000 |20,720 |
| C2 |2,5 GB |2 |500 / 62.5 |41,000 |37,000 |
| C3 |6 GB |4 |1000 / 125 |100,000 |90,000 |
| C4 |13 GB |2 |500 / 62.5 |60,000 |55,000 |
| C5 |26 GB |4 |1,000 / 125 |102,000 |93,000 |
| C6 |53 GB |8 |2,000 / 250 |126,000 |120,000 |
| **Premium önbellek boyutu** | |**Parça başına CPU çekirdekleri** | **Megabit / sn (Mb/sn) / megabayt sayısı / sn (MB/sn)** |**İkinci (RPS) SSL olmayan, parça başına başına istek sayısı** |**Parça başına ikinci (RPS) SSL başına istek sayısı** |
| P1 |6 GB |2 |1,500 / 187.5 |180,000 |172,000 |
| P2 |13 GB |4 |3,000 / 375 |350,000 |341,000 |
| P3 |26 GB |4 |3,000 / 375 |350,000 |341,000 |
| P4 |53 GB |8 |6,000 / 750 |400,000 |373,000 |

Stunnel ayarlama veya Redis araçları gibi yükleme yönergeleri için `redis-benchmark.exe`, bkz: [nasıl ı çalıştırabilirsiniz Redis komutları?](#cache-commands) bölümü.

<a name="cache-region"></a>

### <a name="in-what-region-should-i-locate-my-cache"></a>Hangi bölgede ı my önbellek bulun?
En iyi performans ve düşük gecikme, Azure Redis önbelleği önbellek istemci uygulamanız ile aynı bölgede bulun.

<a name="cache-billing"></a>

### <a name="how-am-i-billed-for-azure-redis-cache"></a>Azure Redis önbelleği için nasıl faturalandırılır?
Azure Redis önbelleği fiyatlandırma olan [burada](https://azure.microsoft.com/pricing/details/cache/). Fiyatlandırma sayfasını saat ücretine fiyatlandırma listeler. Önbellekleri önbelleğe bir önbellek silinir ve saate kadar oluşturulan zamandan itibaren dakika başına temelinde faturalandırılır. Durdurma veya duraklatma bir önbellek faturalama için bir seçenek yoktur.

### <a name="can-i-use-azure-redis-cache-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany"></a>Azure Redis önbelleği Azure Bulutu, Azure Çin Bulut veya Microsoft Azure Almanya kullanabilir miyim?
Evet, Azure Redis önbelleği Azure Bulutu, Azure Çin Bulut ve Microsoft Azure Almanya kullanılabilir. URL'leri erişme ve Azure Redis önbelleği yönetmek için Azure genel bulut ile karşılaştırıldığında bu bulutların farklıdır. 

| Bulut   | Redis için DNS son eki            |
|---------|---------------------------------|
| Genel  | *.redis.cache.windows.net       |
| ABD Devleti  | *.redis.cache.usgovcloudapi.net |
| Almanya | *.redis.cache.cloudapi.de       |
| Çin   | *.redis.cache.chinacloudapi.cn  |

Azure Redis önbelleği ile diğer Bulutları kullanmayla ilgili konular hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın.

- [Azure kamu veritabanları - Azure Redis önbelleği](../azure-government/documentation-government-services-database.md#azure-redis-cache)
- [Azure Çin bulut - Azure Redis önbelleği](https://www.azure.cn/documentation/services/redis-cache/)
- [Microsoft Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)

Azure Bulutu, Azure Çin Bulut ve Microsoft Azure Almanya de PowerShell ile Azure Redis önbelleği kullanma hakkında daha fazla bilgi için bkz: [diğer Bulutları - Azure Redis önbelleği PowerShell bağlanma](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-other-clouds).

<a name="cache-configuration"></a>

### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>StackExchange.Redis yapılandırma seçeneklerini ne yapacaksınız?
StackExchange.Redis pek çok seçenek vardır. Bu bölümde bazı ortak ayarları hakkında alınmaktadır. StackExchange.Redis seçenekleri hakkında daha ayrıntılı bilgi için bkz: [StackExchange.Redis yapılandırma](https://stackexchange.github.io/StackExchange.Redis/Configuration).

| ConfigurationOptions | Açıklama | Öneri |
| --- | --- | --- |
| AbortOnConnectFail |Ne zaman true olarak bağlantı bir ağ hatasından sonra yeniden bağlanmaz. |False olarak ayarlayın ve otomatik olarak yeniden StackExchange.Redis olanak tanır. |
| ConnectRetry |Bağlantı denemesi sırasında ilk yineleme sayısı bağlayın. |Yönergeler için aşağıdaki notlara bakın. |
| ConnectTimeout |Bağlantı işlemleri için ms cinsinden zaman aşımı. |Yönergeler için aşağıdaki notlara bakın. |

Genellikle, istemci varsayılan değerleri yeterlidir. İş yüküne göre seçenekleri ince ayar yapabilirsiniz.

* **Yeniden deneme sayısı**
  * ConnectRetry ve ConnectTimeout için hızlı başarısız ve yeniden deneyin genel yönergeler verilmiştir. Bu kılavuz, iş yükü ve ne kadar süre üzerinde ortalama, Redis komutu yürütün ve yanıt almak istemci alır temel alır.
  * Bağlantı durumunu denetleme ve kendiniz yeniden bağlanmayı yerine otomatik olarak yeniden StackExchange.Redis olanak tanır. **ConnectionMultiplexer.IsConnected özelliği kullanmaktan kaçının**.
  * Snowballing - bazen burada yeniden deneniyor ve yeniden deneme snowball bir sorun ve hiçbir zaman da kurtarır çalışabilir. Snowballing meydana gelirse, açıklandığı gibi bir üstel geri alma yeniden deneme algoritması kullanmayı düşünmelisiniz [yeniden genel rehberlik](../best-practices-retry-general.md) Microsoft Patterns & yöntemler grubu tarafından yayımlanan.
* **Zaman aşımı değerleri**
  * İş yükünüzün göz önünde bulundurun ve değerlerini uygun şekilde ayarlayın. Büyük değerler depolanıyorsa, zaman aşımı daha yüksek bir değere ayarlayın.
  * Ayarlama `AbortOnConnectFail` false ve let StackExchange.Redis sizin için yeniden bağlanın.
  * Tek bir ConnectionMultiplexer örnek uygulama için kullanın. Bir LazyConnection gösterildiği gibi bir bağlantı özellik tarafından döndürülen tek bir örneğini oluşturmak için kullanabileceğiniz [ConnectionMultiplexer sınıfını kullanarak önbelleğe bağlanma](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
  * Ayarlama `ConnectionMultiplexer.ClientName` uygulama bir örnek benzersiz adı tanılama özelliği.
  * Birden çok kullanmak `ConnectionMultiplexer` özel iş yükleri için örnek.
      * Uygulamanızda değişen yük varsa, bu model izleyebilirsiniz. Örneğin:
      * Çoğaltıcı büyük anahtarların ilgilenmek için bir bulunabilir.
      * Çoğaltıcı küçük anahtarlar ilgilenmek için bir bulunabilir.
      * Bağlantı zaman aşımı için farklı değerler ayarlayın ve kullandığınız her ConnectionMultiplexer için mantığı yeniden deneyin.
      * Ayarlama `ClientName` her özellik ile tanılama yardımcı olmak için çoğaltıcı.
      * Bu kılavuz için daha fazla yol açabilir hızlandırıldı gecikme başına `ConnectionMultiplexer`.

### <a name="what-redis-cache-clients-can-i-use"></a>Hangi Redis önbelleği istemcileri kullanabilir miyim?
Redis hakkındaki harika şeylerden olan birçok farklı geliştirme dili destekleyen birçok istemciler biridir. İstemcilerin geçerli bir listesi için bkz [Redis istemcileri](http://redis.io/clients). Birkaç farklı diller ve istemcilerin kapak öğreticileri için bkz: [Azure Redis önbelleği kullanmayı](cache-dotnet-how-to-use-azure-redis-cache.md) ve istenen dil değiştirici makalenin üst dilden'ı tıklatın.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>

### <a name="is-there-a-local-emulator-for-azure-redis-cache"></a>Azure Redis önbelleği için yerel bir öykünücü var mı?
Azure Redis önbelleği için hiçbir yerel öykünücüsü yoktur, ancak redis-server.exe MSOpenTech sürümünü çalıştırabilirsiniz [Redis komut satırı araçları](https://github.com/MSOpenTech/redis/releases/) yerel makine ve yerel önbellek öykünücüsünü benzer bir deneyim almak için bağlanın Aşağıdaki örnekte gösterildiği gibi kullanın:

    private static Lazy<ConnectionMultiplexer>
          lazyConnection = new Lazy<ConnectionMultiplexer>
        (() =>
        {
            // Connect to a locally running instance of Redis to simulate a local cache emulator experience.
            return ConnectionMultiplexer.Connect("127.0.0.1:6379");
        });

        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


İsteğe bağlı olarak yapılandırabileceğiniz bir [redis.conf](http://redis.io/topics/config) daha yakından eşleşecek şekilde dosya [varsayılan önbellek ayarları](cache-configure.md#default-redis-server-configuration) çevrimiçi Azure Redis isterseniz önbelleğiniz için.

<a name="cache-commands"></a>

### <a name="how-can-i-run-redis-commands"></a>Redis komutları nasıl çalıştırabilir miyim?
Adresinde listelenmiş komutlardan herhangi birini kullanabilirsiniz [Redis komutları](http://redis.io/commands#) adresinde listelenmiş komutları dışında [Redis Azure Redis Önbelleği'nde desteklenmeyen komutlar](cache-configure.md#redis-commands-not-supported-in-azure-redis-cache). Redis komutları çalıştırmak için birkaç seçeneğiniz vardır.

* Standart veya Premium önbellek varsa, Redis komutları kullanarak çalıştırabilirsiniz [Redis konsol](cache-configure.md#redis-console). Redis Konsolu'na Azure portalında Redis komutları çalıştırmak için güvenli bir yol sağlar.
* Redis komut satırı araçları da kullanabilirsiniz. Bunları kullanmak için aşağıdaki adımları gerçekleştirin:
* Karşıdan [Redis komut satırı araçları](https://github.com/MSOpenTech/redis/releases/).
* Önbellek bağlamak için `redis-cli.exe`. -H geçiş kullanarak önbellek son nokta aşağıdaki örnekte gösterildiği gibi kullanarak ve anahtarı geçirin:
* `redis-cli -h <redis cache name>.redis.cache.windows.net -a <key>`

> [!NOTE]
> Redis komut satırı araçlarını SSL bağlantı noktası ile çalışmaz, ancak bir yardımcı programı gibi kullanabilir `stunnel` güvenli Araçları'ndaki yönergeleri izleyerek SSL bağlantı noktasına bağlanmak için [Redis Önizleme için ASP.NET oturum durumu sağlayıcısı Duyurusu Yayın](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blog postası.
>
>

<a name="cache-reference"></a>

### <a name="why-doesnt-azure-redis-cache-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Neden Azure Redis önbelleği bazı diğer Azure Hizmetleri gibi bir MSDN sınıf kitaplığı başvurusu yok?
Microsoft Azure Redis önbelleği popüler açık kaynak Redis önbelleğini temel alır ve çok çeşitli tarafından erişilebilen [Redis istemcileri](http://redis.io/clients) pek çok programlama dili için. Her istemci Redis önbelleği örneği kullanarak çağrılar kendi API sahip [Redis komutları](http://redis.io/commands).

Her istemci farklı olduğu için MSDN'de olmayan bir merkezi sınıf başvurusu olan ve her bir istemci kendi başvuru belgeleri korur. Başvuru belgeleri yanı sıra Azure farklı diller ve önbellek istemcileri kullanarak Redis önbelleği kullanmaya başlamak nasıl gösteren birkaç öğreticileri vardır. Bu öğreticiler erişmek için bkz: [Azure Redis önbelleği kullanmayı](cache-dotnet-how-to-use-azure-redis-cache.md) ve istenen dil değiştirici makalenin üst dilden'ı tıklatın.

### <a name="can-i-use-azure-redis-cache-as-a-php-session-cache"></a>Azure Redis önbelleği PHP oturum önbelleği olarak kullanabilir miyim?
Evet, Azure Redis önbelleği PHP oturum önbelleği olarak kullanmak için Azure Redis önbelleği Örneğiniz için bağlantı dizesini belirtin `session.save_path`.

> [!IMPORTANT]
> Azure Redis önbelleği PHP oturum önbelleği olarak kullanırken, URL gerekir aşağıdaki örnekte gösterildiği gibi önbelleğine bağlanmak için kullanılan güvenlik anahtarı kodlamak:
>
> `session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
> Anahtar URL kodlanmış değilse, bir özel durumla aşağıdakine benzer bir ileti alabilirsiniz: `Failed to parse session.save_path`
>
>

PHP oturum önbelleğini PhpRedis istemcisiyle olarak Redis önbelleği kullanma hakkında daha fazla bilgi için bkz: [PHP oturum işleyici](https://github.com/phpredis/phpredis#php-session-handler).

### <a name="what-are-redis-databases"></a>Redis veritabanları nelerdir?

Redis, yalnızca bir mantıksal ayırma aynı Redis örneğini içindeki verilerin veritabanlarıdır. Önbellek tüm veritabanlarını ve bu veritabanında depolanan anahtarları/değerleri verilen bir veritabanı kullanımına bağlı gerçek bellek arasında paylaşılır. Örneğin bir C6 önbellek 53 GB'a kadar bellek sahiptir. Bir veritabanına tüm 53 GB'a yerleştirilecek seçebilir veya, onu birden çok veritabanları arasında bölmek. 

> [!NOTE]
> Premium Azure Redis önbelleği kümeleme özelliği etkinleştirilmiş kullanırken, yalnızca veritabanı 0 kullanılabilir. Bu sınırlama bir iç Redis sınırlaması ve Azure Redis önbelleği için belirli değil. Daha fazla bilgi için bkz: [kümeleme kullanmak üzere istemci Uygulamam herhangi bir değişiklik yapmanız gerekiyor mu?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 


<a name="cache-ssl"></a>

### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Redis için bağlanmak için SSL olmayan bağlantı noktası etkinleştirdiğinizde?
Redis sunucu SSL yerel olarak desteklemez ancak Azure Redis önbelleği desteklemez. Azure Redis önbelleği bağlanan ve istemci StackExchange.Redis gibi SSL destekliyorsa SSL kullanmanız gerekir.

>[!NOTE]
>SSL olmayan bağlantı noktası yeni Azure Redis önbelleği örnekleri için varsayılan olarak devre dışıdır. İstemci SSL desteklemeyen sonra'ndaki yönergeleri izleyerek SSL olmayan bağlantı noktası etkinleştirmelisiniz [erişim bağlantı noktaları](cache-configure.md#access-ports) bölümünü [Azure Redis Önbelleği'nde bir önbellek yapılandırma](cache-configure.md) makalesi.
>
>

Araçları gibi redis `redis-cli` SSL bağlantı noktası ile çalışmaz, ancak bir yardımcı programı gibi kullanabilir `stunnel` güvenli Araçları'ndaki yönergeleri izleyerek SSL bağlantı noktasına bağlanmak için [Redis için ASP.NET oturum durumu sağlayıcısı Duyurusu Önizleme sürümü](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blog postası.

Redis araçları yükleme ile ilgili yönergeler için bkz: [nasıl ı çalıştırabilirsiniz Redis komutları?](#cache-commands) bölümü.

### <a name="what-are-some-production-best-practices"></a>Bazı üretim en iyi uygulamalar nelerdir?
* [StackExchange.Redis en iyi uygulamalar](#stackexchangeredis-best-practices)
* [Yapılandırma ve kavramları](#configuration-and-concepts)
* [Performansı test etme](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>StackExchange.Redis en iyi uygulamalar
* Ayarlama `AbortConnect` false olarak sonra otomatik olarak yeniden ConnectionMultiplexer sağlayabilirsiniz. [Ayrıntılar için burada gördüğünüz](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
* ConnectionMultiplexer yeniden - her istek için yeni bir tane oluşturmayın. `Lazy<ConnectionMultiplexer>` Düzeni [burada gösterilen](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) önerilir.
* Works en küçük değerleri ile redis, böylece birden çok anahtar daha büyük veri chopping göz önünde bulundurun. İçinde [bu Redis tartışma](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ), 100 kb büyük değerlendirilir. Okuma [bu makalede](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) büyük değerler neden bir örnek sorun için.
* Yapılandırma, [iş parçacığı havuzu ayarlarını](#important-details-about-threadpool-growth) zaman aşımları önlemek için.
* En az 5 saniyelik varsayılan connectTimeout kullanın. Bu aralık, bir ağ blip durumunda bağlantıyı yeniden kurmak için yeterli zaman StackExchange.Redis verirsiniz.
* Kullanmakta olduğunuz farklı işlemlerle ilişkili performans maliyetleri farkında olun. Örneğin, `KEYS` komut bir o(n) ve kaçınılmalıdır. [Redis.io site](http://redis.io/commands/) desteklediği her bir işlemin zaman karmaşıklık ayrıntılarla sahiptir. Her komutu her bir işlemin karmaşıklık görmek için tıklatın.

#### <a name="configuration-and-concepts"></a>Yapılandırma ve kavramları
* Standart veya Premium katmanı üretim sistemleri için kullanın. Temel katman, hiçbir veri çoğaltma ve hiçbir SLA ile bir tek düğümlü sistemidir. Ayrıca, en az bir C1 önbellek kullanır. C0 önbellekleri genellikle basit geliştirme ve test senaryoları için kullanılır.
* Redis olduğunu unutmayın bir **bellek içi** veri deposu. Okuma [bu makalede](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) burada veri kaybının olabileceği senaryoları kullanan; böylece.
* Bağlantı blips işleyebilir gibi sisteminizi geliştirmek [düzeltme eki uygulama ve yük devretme nedeniyle](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>Performansı test etme
* Başlangıç kullanarak `redis-benchmark.exe` kendi perf yazmadan önce olası üretilen iş için bir fikir almak için sınar. Çünkü `redis-benchmark` SSL, desteklemeyen gerekir [Azure portalı üzerinden SSL olmayan bağlantı noktasını etkinleştirmek](cache-configure.md#access-ports) testini çalıştırmadan önce. Örnekler için bkz: [nasıl Kıyaslama ve my önbellek performansını test etme?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* VM test etmek için kullanılan istemci Redis önbelleği örneği ile aynı bölgede olması gerekir.
* Daha iyi donanım olması ve en iyi sonuçlar vermelidir Dv2 VM dizisi için istemcinizi kullanmanızı öneririz.
* En az kadar bilgi işlem seçtiğiniz VM istemciniz varsa ve test bant genişliği kapasitesi önbelleği olarak emin olun.
* Windows'da ise VRSS istemci makinesinde etkinleştirin. [Ayrıntılar için burada gördüğünüz](https://technet.microsoft.com/library/dn383582.aspx).
* CPU ve ağ için daha iyi donanım üzerinde çalıştırdığınız için daha iyi gecikme süresi ve verimlilik premium katmanı Redis örnekleri ağ.

<a name="cache-redis-commands"></a>

### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Bazı konuları ortak Redis komutlarını kullanırken nelerdir?
* Bu komutları etkisini anlamak olmadan tamamlanması uzun zaman bazı Redis komutlar çalıştırmamanız gerekir.
  * Örneğin, çalıştırmayan [ANAHTARLARI](http://redis.io/commands/keys) anahtarları sayısına bağlı olarak döndürmek için uzun bir zaman alabilir gibi üretimde komutu. Redis bir tek iş parçacıklı bir sunucudur ve aynı anda komutları işler. ANAHTARLARI sonra diğer komutlara varsa, ANAHTARLARI komutu Redis işleyene kadar bunlar işlenmeyecek. [Redis.io site](http://redis.io/commands/) desteklediği her bir işlemin zaman karmaşıklık ayrıntılarla sahiptir. Her komutu her bir işlemin karmaşıklık görmek için tıklatın.
* Küçük anahtar/değer veya büyük anahtar/değer anahtar boyutları - kullanmalıyım? Genel olarak, senaryoya bağlıdır. Büyük anahtarlar senaryonuz gerektiriyorsa, ConnectionTimeout ayarlamak ve değerleri yeniden deneyin ve yeniden deneme mantığı ayarlayın. Redis sunucu açısından bakıldığında, daha iyi performans sağlamak için küçük değerler gözetilir.
* Bu noktalar Redis büyük değerleri depolayamaz anlamına yoktur; Aşağıdaki hususlara farkında olmanız gerekir. Gecikme süreleri daha yüksek olur. Bir büyük veri kümesi ve daha küçük olan bir varsa, birden çok ConnectionMultiplexer örnekleri kullanabilirsiniz, her önceki açıklandığı gibi farklı bir zaman aşımı ve yeniden deneme değerlerini kümesi ile yapılandırılmış [StackExchange.Redis ne yapılandırma seçenekleri yapmak](#cache-configuration) bölümü.

<a name="cache-benchmarking"></a>

### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Nasıl Kıyaslama ve my önbellek performansını test etme?
* Önbelleğinizin sistem durumunu [izleyebilmeniz](cache-how-to-monitor.md) için [önbellek tanılamayı etkinleştirin](cache-how-to-monitor.md#enable-cache-diagnostics). Azure portalında ölçümleri görüntüleyebilir ve ayrıca istediğiniz araçları kullanarak bunları [indirebilir ve gözden geçirebilirsiniz](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring).
* Redis sunucunuza yük testi için redis benchmark.exe kullanabilirsiniz.
* İstemci ve Redis önbelleği sınama yük aynı bölgede olduğundan emin olun.
* Redis-cli.exe kullanın ve bilgileri komutunu kullanarak önbelleğini izleyebilirsiniz.
* Yük yüksek bellek parçalanması neden olup olmadığını, daha büyük bir önbellek boyutu ölçeği.
* Redis araçları yükleme ile ilgili yönergeler için bkz: [nasıl ı çalıştırabilirsiniz Redis komutları?](#cache-commands) bölümü.

Aşağıdaki komutları redis benchmark.exe kullanmaya ilişkin bir örnek sağlar. Doğru sonuçlar için bir VM'den önbelleğiniz ile aynı bölgede bu komutları çalıştırın.

* 1 k yükü kullanarak test ardışık SET istekleri

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t SET -n 1000000 -d 1024 -P 50`
* Ardışık test alma 1 k yükü kullanarak ister.
  Not: Sınama KÜMESİ ilk önbelleğini doldurmak için yukarıda gösterilen

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t GET -n 1000000 -d 1024 -P 50`

<a name="threadpool"></a>

### <a name="important-details-about-threadpool-growth"></a>İş parçacığı havuzu büyüme hakkında önemli ayrıntıları
CLR iş parçacığı havuzu iş parçacıkları - "Alt" ve "G/ç tamamlama bağlantı noktasını" (diğer adıyla ıocp Açılırken) iki tür vardır iş parçacığı sayısı.

* Çalışan iş parçacığı işleme gibi şeyleri zaman için kullanılan `Task.Run(…)` veya `ThreadPool.QueueUserWorkItem(…)` yöntemleri. İş arka plan iş parçacığında yapılması gerektiğinde bu iş parçacıkları CLR çeşitli bileşenleri tarafından da kullanılır.
* Zaman uyumsuz g/ç (örneğin ağdan okunurken) olduğunda ıocp Açılırken iş parçacığı kullanılır.

İş parçacığı her tür için "Minimum" ayarı ulaşana kadar iş parçacığı havuzu yeni çalışan iş parçacığı veya g/ç Tamamlama iş parçacığı isteğe bağlı (bir kısıtlama olmadan) sağlar. Varsayılan olarak, en az iş parçacığı sayısını bir sistemde işlemci sayısını ayarlanır.

Varolan (meşgul) iş parçacığı sayısı "minimum" iş parçacığı sayısını isabetler sonra iş parçacığı havuzu en yeni iş parçacıkları 500 milisaniye başına tek bir iş parçacığı için yerleştirir hızı azaltma. Sisteminizin bir veri bloğu ıocp Açılırken iş parçacığı ihtiyaç duyan iş alırsa, genellikle, bu çalışan çok hızlı bir şekilde işler. Ancak, çok sayıda iş birden çok yapılandırılmış "Minimum" ayarı ise, olacaktır bazı gecikme iş parçacığı havuzu olmasını ikisinden birini bekler gibi bazı iş işleme.

1. Var olan bir iş parçacığı iş işlemek boş olur.
2. Yeni bir iş parçacığı oluşmasını mevcut hiçbir iş parçacığı 500ms için boş olur.

Temel olarak meşgul iş parçacığı sayısı Min iş parçacıkları büyük olduğunda, ağ trafiğini uygulama tarafından işlenmeden önce büyük olasılıkla bir 500ms gecikme ödeme yapıyorsanız, anlamına gelir. Ayrıca, mevcut bir iş parçacığı (t unutmayın üzerinde göre) 15 saniye daha uzun süre boşta kalır, temizlenen ve bu büyüme ve azalması döngüsünü yineleyebilirsiniz unutmamak önemlidir.

StackExchange.Redis biz bir örnek hata ileti baktığınızda (1.0.450 yapı veya sonrası), bunu şimdi (bkz. ıocp Açılırken ve çalışan ayrıntılarına bakın) iş parçacığı havuzu istatistikleri yazdırır görürsünüz.

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive,
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0,
    IOCP: (Busy=6,Free=994,Min=4,Max=1000),
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

Önceki örnekte için ıocp Açılırken iş parçacığı 6 meşgul iş parçacığı vardır ve sistem 4 en az iş parçacığı izin verecek şekilde yapılandırılmış görebilirsiniz. Bu durumda, istemci büyük olasılıkla iki 500 ms gecikmeler nedeniyle gördünüz 6 > 4.

Büyüme ıocp Açılırken ya da çalışan iş parçacığı kısıtlanan, StackExchange.Redis zaman aşımları isabet olduğunu unutmayın.

### <a name="recommendation"></a>Öneri
Bu bilgiler verildiğinde, müşteriler varsayılan değerinden daha büyük bir şey için ıocp Açılırken ve çalışan iş parçacıkları için en düşük yapılandırma değeri ayarlayın kesinlikle öneririz. Biz, çok yüksek/düşük başka bir uygulama için bir uygulama için doğru değer olacağından bu değerin olması BT'ye yönergeler veremez. Her müşterinin gereksinimlerine için bu ayarı ince ayar yapmak gereken şekilde bu ayar ayrıca diğer bölümleri karmaşık uygulamaların performansını etkileyebilir. İyi bir başlangıç noktası 200 veya 300, ardından test ve gerektiği gibi ince ayar.

Bu ayarı yapılandırmak nasıl:

* ASP.NET, kullanın ["minIoThreads" yapılandırma ayarı] [ "minIoThreads" configuration setting] altında `<processModel>` web.config dosyasındaki yapılandırma öğesi. Azure Web siteleri içinde çalıştırıyorsanız, bu ayar yapılandırma seçenekleri gösterilmez. Ancak, siz hala Bu program aracılığıyla ayarını yapılandırmak gerekir (aşağıda Application_Start yönteminizi global.asax.cs gelen bakın).

  > [!NOTE] 
  > Bu yapılandırma öğesi için belirtilen değer bir *çekirdek başına* ayarı. Örneğin, 4 çekirdekli makine olması ve çalışma zamanında 200 olması minIOThreads ayarınız istiyorsanız kullanacağınız `<processModel minIoThreads="50"/>`.
  >

* ASP.NET dışında kullanan [ThreadPool.SetMinThreads(...) ](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads.aspx) API.

<a name="server-gc"></a>

### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>Daha fazla verimlilik StackExchange.Redis kullanırken almak sunucu GC etkinleştir
Sunucu GC etkinleştirildiğinde, istemcinin en iyi duruma getirme ve StackExchange.Redis kullanırken daha iyi performans ve verimlilik sağlar. Sunucuda GC ve nasıl etkinleştirileceği konusunda daha fazla bilgi için aşağıdaki makalelere bakın:

* [Sunucu GC etkinleştirmek için](https://msdn.microsoft.com/library/ms229357.aspx)
* [Çöp toplamanın temelleri](https://msdn.microsoft.com/library/ee787088.aspx)
* [Çöp toplama ve performans](https://msdn.microsoft.com/library/ee851764.aspx)


### <a name="performance-considerations-around-connections"></a>Bağlantıları geçici performans etkenleri

Her fiyatlandırma katmanının istemci bağlantıları, bellek ve bant genişliği için farklı sınırları vardır. Her önbellek boyutunu sağlar, ancak *kadar* belirli bir sayıda bağlantıları, her bağlantı için Redis ek yükü ile ilişkili. TLS/SSL şifreleme sonucunda CPU ve bellek kullanımı gibi ek yükü örneği olacaktır. Belirtilen önbellek boyutu için en fazla bağlantı üst sınırına hafifçe yüklü bir önbellek varsayar. Varsa yük bağlantı yükünü *artı* istemci işlemleri yükü sistem kapasitesini aşıyor, geçerli önbellek boyutu bağlantı sınırını aştınız değil olsa bile önbelleği kapasite sorunlar yaşayabilirsiniz.

Her katman için farklı bağlantı sınırları hakkında daha fazla bilgi için bkz: [Azure Redis önbelleği fiyatlandırma](https://azure.microsoft.com/pricing/details/cache/). Bağlantıları ve diğer varsayılan yapılandırmaları hakkında daha fazla bilgi için bkz: [varsayılan Redis sunucu yapılandırması](cache-configure.md#default-redis-server-configuration).

<a name="cache-monitor"></a>

### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Sistem durumunu ve performansını my önbelleğinin nasıl izlerim?
Microsoft Azure Redis önbelleği örnekleri izlenebilir [Azure portal](https://portal.azure.com). Ölçümleri görüntülemek, ölçümler grafiklerde panosuna Sabitle PIN, izleme grafikleri, tarih ve saat aralığı özelleştirme, eklemek ve ölçümleri grafikten kaldırabileceğiniz ve belirli koşullar karşılandığında Uyarıları ayarlayın. Daha fazla bilgi için bkz: [İzleyici Azure Redis önbelleği](cache-how-to-monitor.md).

Redis önbelleği **kaynak menü** de izleme ve önbellekleri sorun giderme için çeşitli araçlar içerir.

* **Sorunları tanılamak ve** çözümlemek için ortak sorunlar ve stratejileri hakkında bilgi sağlar.
* **Kaynak durumu** kaynağınız izler ve beklendiği gibi çalışıp çalışmadığını belirtir. Azure kaynak sistem durumu hizmeti hakkında daha fazla bilgi için bkz: [Azure kaynak sistem durumu genel bakış](../resource-health/resource-health-overview.md).
* **Yeni destek isteği** önbellek hesabınız için bir destek isteği açmak için seçenekler sağlar.

Bu araçlar, Azure Redis önbelleği örnekleri durumunu izlemenize ve önbelleğe alma uygulamaları yönetmenize yardımcı olmak etkinleştirin. Daha fazla bilgi için "Destek ve sorun giderme ayarları" bölümüne bakın [Azure Redis önbelleği yapılandırma](cache-configure.md).

<a name="cache-timeouts"></a>

### <a name="why-am-i-seeing-timeouts"></a>Zaman aşımları neden görüyorum?
Redis için anlaşmak için kullandığınız istemci zaman aşımları gerçekleşir. Bir komut Redis sunucuya gönderildiğinde, komut sıraya ve Redis sunucu sonunda komutu seçer ve yürütür. Ancak istemci için bu işlem sırasında zaman aşımı ve bir özel durum uyuyorsa arama tarafında tetiklenir. Zaman aşımı sorunlarının giderilmesiyle ilgili daha fazla bilgi için bkz: [istemci-tarafı sorun giderme](cache-how-to-troubleshoot.md#client-side-troubleshooting) ve [StackExchange.Redis zaman aşımı özel](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).

<a name="cache-disconnect"></a>

### <a name="why-was-my-client-disconnected-from-the-cache"></a>Neden my istemci önbelleğinden kesildi?
Önbellek bağlantıyı kes bazı yaygın nedeni şunlardır:

* İstemci-tarafı neden olur.
  * İstemci uygulaması yeniden dağıtıldı.
  * İstemci uygulaması bir ölçeklendirme işlemi gerçekleştirilir.
    * Bulut Hizmetleri veya Web Apps söz konusu olduğunda, bu nedeniyle otomatik olarak ölçeklendirme olabilir.
  * İstemci tarafında ağ katmanı değiştirildi.
  * İstemci veya sunucu ile istemci arasındaki ağ düğümleri geçici hataları oluştu.
  * Bant genişliği eşik sınırını ulaşılan.
  * CPU bağımlı işlemlerinin tamamlanması çok uzun sürdü.
* Sunucu tarafı neden olur.
  * Azure Redis önbelleği hizmeti sunan standart önbellekte, yük devretme birincil düğümünden ikincil düğüme başlattı.
  * Azure önbelleği dağıtıldığı örneği düzeltme
    * Bu, Redis sunucu güncelleştirmeleri veya genel VM bakım için olabilir.

### <a name="which-azure-cache-offering-is-right-for-me"></a>Hangi Azure Önbellek teklifi bana uygundur?
> [!IMPORTANT]
> Geçen yıla göre [duyuru](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/), Azure yönetilen önbellek hizmeti ve Azure rol içi önbelleği hizmeti **devre dışı bırakılmış** 30 Kasım 2016. Bizim önerimiz kullanmaktır [Azure Redis önbelleği](https://azure.microsoft.com/services/cache/). Geçiş hakkında daha fazla bilgi için bkz: [Azure Redis önbelleği yönetilen önbellek hizmeti geçiş](cache-migrate-to-redis.md).
>
>

### <a name="azure-redis-cache"></a>Azure Redis Cache
Azure Redis önbelleği genellikle kullanılabilir 53 GB'a kadar boyutları ve % 99,9 SLA kullanılabilirlik sahip ' dir. Yeni [premium katmanı](cache-premium-tier-intro.md) boyutları kadar sunar 530 GB ve kümeleme, sanal ağ ve % 99,9 SLA ile kalıcılığı için destek.

Azure Redis önbelleği müşteriler Microsoft tarafından yönetilen bir güvenli, ayrılmış bir Redis önbelleğine kullanma yeteneği sağlar. Bu teklif ile zengin özellik kümesi ve Redis, güvenilir barındırma ve izleme Microsoft tarafından sağlanan ekosistemi yararlanan alın.

Yalnızca anahtar-değer çiftleri ile ilgilenir geleneksel önbellekleri Redis, yüksek oranda kullanıcı veri türleri için yaygın olarak kullanılır. Ayrıca bir dizeye ekleme gibi bu türlerinde atomik işlemleri çalışmayı destekliyor redis; artan bir karma değeri; bir listeye Ftp'den; bilgi işlem kümesi, union ve kesişimi fark; veya üye bir sıralanmış küme içinde yüksek sıralamayla alınıyor. Diğer özellikler için destek işlemleri, pub/alt, komut dosyası, sınırlı bir anahtarlarla time-to-live, Lua ve daha geleneksel bir önbellek gibi davranır Redis yapmak için yapılandırma ayarlarını içerir.

Redis başarılı başka bir önemli nokta çevresinde yerleşik sağlıklı, Canlı açık kaynak ekosistemi ' dir. Bu, mevcut Redis istemcileri farklı kümesinde birden çok dil arasında yansıtılır. Bu ekosistem ve çeşitli istemciler Azure içinde oluşturmayı tercih neredeyse tüm iş yükü tarafından kullanılacak Azure Redis önbelleği izin verir.

Azure Redis önbelleği kullanmaya başlama hakkında daha fazla bilgi için bkz: [kullanım Azure Redis önbelleği nasıl](cache-dotnet-how-to-use-azure-redis-cache.md) ve [Azure Redis önbelleği belgelerine](index.md).

### <a name="managed-cache-service"></a>Yönetilen önbellek hizmeti
[Önbellek hizmeti 30 Kasım 2016 devre dışı bırakılan yönetilen.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

Arşivlenen belgeleri görüntülemek için bkz: [arşivlenmiş yönetilen önbellek hizmeti belgeleri](https://msdn.microsoft.com/library/azure/dn386094.aspx).

### <a name="in-role-cache"></a>Rol İçi Önbellek
[Rol içi önbelleği 30 Kasım 2016 devre dışı bırakılan.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

Arşivlenen belgeleri görüntülemek için bkz: [arşivlenmiş rol içi önbellek belgeleri](https://msdn.microsoft.com/library/azure/dn386103.aspx).

["minIoThreads" configuration setting]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
