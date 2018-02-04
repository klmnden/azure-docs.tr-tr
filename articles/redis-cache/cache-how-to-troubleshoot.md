---
title: "Azure Redis önbelleği ile ilgili sorunları giderme | Microsoft Docs"
description: "Azure Redis önbelleği ile ilgili genel sorunları çözmeyi öğrenin."
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 928b9b9c-d64f-4252-884f-af7ba8309af6
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: wesmc
ms.openlocfilehash: e5f6f423697d90e889ebde2cd203891e34278b3c
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="how-to-troubleshoot-azure-redis-cache"></a>Azure Redis önbelleği ile ilgili sorunları giderme
Bu makalede aşağıdaki kategorisinden Azure Redis önbelleği sorunlarını gidermeye yönelik yönergeler sağlar.

* [İstemci-tarafı sorun giderme](#client-side-troubleshooting) - Bu bölüm, tanımlamaya yönergeleri sağlar ve sorunları gidererek neden Azure Redis önbelleği bağlanma uygulama tarafından.
* [Sunucu tarafı sorun giderme](#server-side-troubleshooting) - Bu bölüm, tanımlamaya yönergeleri sağlar ve sorunları gidererek neden Azure Redis önbelleği sunucu tarafında.
* [StackExchange.Redis zaman aşımı özel](#stackexchangeredis-timeout-exceptions) -Bu bölüm, StackExchange.Redis istemcisi kullanılırken sorunlarını giderme hakkında bilgi sağlar.

> [!NOTE]
> Bu kılavuzdaki sorun giderme adımları çeşitli Redis komutları çalıştırmak ve çeşitli performans ölçümleri izlemek için yönergeler içerir. Daha fazla bilgi ve yönergeler için bkz: makaleler [ek bilgi](#additional-information) bölümü.
> 
> 

## <a name="client-side-troubleshooting"></a>İstemci-tarafı sorunlarını giderme
Bu bölümde, istemci uygulaması bir koşul nedeniyle oluşan sorun giderme sorunlar açıklanmaktadır.

* [Bellek baskısı istemcide](#memory-pressure-on-the-client)
* [Trafik veri bloğu](#burst-of-traffic)
* [Yüksek istemci CPU kullanımı](#high-client-cpu-usage)
* [İstemci-tarafı bant genişliği aşıldı](#client-side-bandwidth-exceeded)
* [Büyük istek/yanıt boyutu](#large-requestresponse-size)
* [Redis içinde verilerimi ne oldu?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>Bellek baskısı istemcide
#### <a name="problem"></a>Sorun
İstemci makinesinde bellek baskısı her türlü herhangi bir gecikme olmadan Redis örneği tarafından gönderilen verilerin işlenmesini geciktirebilir performans sorunlarına neden olmaktadır. Bellek baskısı geldiğinde sistem sayfa verileri için diskte sanal bellek için fiziksel bellekten genellikle sahip. Bu *sayfa hatalı* sistemin önemli ölçüde yavaşlamasına neden olur.

#### <a name="measurement"></a>Ölçüm
1. Kullanılabilir bellek aşmadığından emin olmak için makinede bellek kullanımını izleyin. 
2. İzleyici `Page Faults/Sec` performans sayacı. Çoğu sistemi bile normal işlem sırasında bazı sayfa hataları vardır, bu nedenle bu sayfa hataları performans sayacı zaman aşımlarını karşılık gelen ani izleyin.

#### <a name="resolution"></a>Çözüm
Daha fazla belleğe sahip VM boyutu büyük istemciye istemcinizi yükseltin veya bellek kullanımını azaltmak için bellek kullanım desenlerini derinliklerine.

### <a name="burst-of-traffic"></a>Trafik veri bloğu
#### <a name="problem"></a>Sorun
WINS'e trafiği düşük ile birlikte `ThreadPool` ayarları zaten Redis sunucu tarafından gönderilen ancak henüz istemci tarafında tüketilen veri işliyor gecikmelere neden olur.

#### <a name="measurement"></a>Ölçüm
İzleyici nasıl, `ThreadPool` istatistikleri değiştirme kodu kullanarak zamanla [şöyle](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Ayrıca bakabilir `TimeoutException` StackExchange.Redis ileti. Örnek aşağıda verilmiştir:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

Önceki iletide ilginç bazı sorunlar vardır:

1. İçinde dikkat `IOCP` bölüm ve `WORKER` bölümüne sahip bir `Busy` değerinden daha büyük değer `Min` değeri. Bu fark anlamına gelir, `ThreadPool` ayarlarına gereksinim ayarlama.
2. Ayrıca bkz `in: 64221`. Bu değer 64,211 bayt çekirdek yuva katmanında alınan ancak henüz (örneğin, StackExchange.Redis) uygulama tarafından henüz okunmamış gösterir. Bu fark, uygulamanızın verileri ağ üzerinden sunucu size gönderiyor olabildiğince çabuk okuma değil, genellikle anlamına gelir.

#### <a name="resolution"></a>Çözüm
Yapılandırma, [iş parçacığı havuzu ayarlarını](https://gist.github.com/JonCole/e65411214030f0d823cb) , iş parçacığı havuzu hızlı bir şekilde altında ölçek artırabilir, emin olmak için senaryolar veri bloğu.

### <a name="high-client-cpu-usage"></a>Yüksek istemci CPU kullanımı
#### <a name="problem"></a>Sorun
İstemci üzerinde yüksek CPU kullanımı ile gerçekleştirmek için istenmişti çalışma sistem yetişemiyor göstergesidir. Bu durum, istemci Redis hızla yanıt gönderdi olsa bile bir Redis yanıt zamanında işlemek başarısız olabileceği anlamına gelir.

#### <a name="measurement"></a>Ölçüm
Azure Portal veya ilişkili performans sayacı aracılığıyla sistem geniş CPU kullanımını izleyin. İzleme konusunda dikkatli olun *işlem* CPU tek bir işlem aynı anda düşük CPU kullanımı sahip olduğundan, genel sistem zamanı CPU yüksek olabilir. Zaman aşımlarını karşılık ani CPU kullanımını izleyin. Yüksek CPU sonucu olarak, aynı zamanda yüksek görebileceğiniz `in: XXX` değerler `TimeoutException` hata iletileri açıklandığı gibi [veri bloğu trafik](#burst-of-traffic) bölümü.

> [!NOTE]
> StackExchange.Redis 1.1.603 ve daha sonra içerir `local-cpu` içinde ölçüm `TimeoutException` hata iletileri. En son sürümünü kullandığından emin olun [StackExchange.Redis NuGet paketi](https://www.nuget.org/packages/StackExchange.Redis/). Sürekli olarak en son sürümünü olması önemlidir daha sağlam zaman aşımına uğrayan yapmak için kodda düzeltilen vardır.
> 
> 

#### <a name="resolution"></a>Çözüm
Daha fazla CPU kapasiteli daha büyük bir VM boyutu yükseltmek veya CPU ani neden olan araştırın. 

### <a name="client-side-bandwidth-exceeded"></a>İstemci-tarafı bant genişliği aşıldı
#### <a name="problem"></a>Sorun
İstemci makineler mimarisine bağlı olarak, bunlar sınırlamaları olabilir kullanılabilir olan ne kadar üzerindeki ağ bant genişliği. İstemci, ağ kapasitesi aşırı yüklemesi tarafından kullanılabilir bant genişliğini aşarsa, ardından veri istemci tarafında sunucusuna gönderme olabildiğince çabuk işlenmedi. Bu durum, zaman aşımına uğrayan yol açabilir.

#### <a name="measurement"></a>Ölçüm
Bant genişliği kullanımını kod kullanarak zamanla nasıl değiştiğini izlemek [şöyle](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Bu kod, kısıtlı izinleri (örneğin, Azure web siteleri) olan bazı ortamlarda başarılı bir şekilde çalışmayabilir.

#### <a name="resolution"></a>Çözüm
İstemci VM boyutunu artırın veya ağ bant genişliği tüketimini azaltabilir.

### <a name="large-requestresponse-size"></a>Büyük istek/yanıt boyutu
#### <a name="problem"></a>Sorun
Büyük bir istek/yanıt zaman aşımlarına neden olabilir. Örnek olarak, istemcide yapılandırılan, zaman aşımı değeri 1 saniyedir varsayalım. Uygulamanız (aynı fiziksel ağ bağlantısını kullanarak) aynı anda iki anahtar (örneğin, 'A' ve 'B') ister. Hem istekleri 'A' ve 'B' sunucusuna bir hat üzerinde art arda için yanıt beklemeden gönderilir, çoğu istemci isteklerinin "Pipelining" destekler. Sunucu, aynı sırayla yanıt gönderir. Yanıt 'A' yeterince büyük olursa, sonraki istekleri için zaman aşımı çoğu yemek. 

Aşağıdaki örnek, bu senaryo gösterir. Bu senaryoda, istek 'A' ve 'B' hızlı bir şekilde gönderilir, sunucunun yanıtları 'A' ve 'B' hızlı bir şekilde göndermeye başlar, ancak sunucu hızlı bir şekilde yanıt verdi olsa bile veri aktarım süreleri nedeniyle 'B' bir istek ve zaman aşımına arkasındaki takılı.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Ölçüm
Bu istek/yanıt ölçmek için zor bir adrestir. Temel olarak, istemci kodu büyük isteklerini ve yanıtlarını izlemek için izleme gerekir. 

#### <a name="resolution"></a>Çözüm
1. Redis çok sayıda birkaç büyük değerler yerine küçük değerler için optimize edilmiştir. Tercih edilen verilerinizi ilgili küçük değerler bölüneceği çözümüdür. Bkz: [redis için ideal değer boyutu aralığı nedir? 100 KB çok büyük? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) sonrası küçük değerler neden önerilen geçici Ayrıntılar için.
2. Veri Aktarım süreleri daha büyük yanıtlar için azaltarak daha yüksek bant genişliği özellikleri almak için (istemci ve Redis önbelleği sunucu için), VM boyutunu artırın. Yalnızca sunucunun veya yalnızca üzerinde daha fazla bant genişliği alma istemci yeterli olmayabilir. Bant genişliği kullanımı ölçmek ve şu anda VM boyutu özelliklerini karşılaştırın.
3. Sayısını artırmak `ConnectionMultiplexer` farklı bağlantıları üzerinden kullanın ve hepsini istekleri nesneleri.

### <a name="what-happened-to-my-data-in-redis"></a>Redis içinde verilerimi ne oldu?
#### <a name="problem"></a>Sorun
T belirli my Azure Redis önbelleği örneğine veri bekleniyordu, ancak var gibi görünüyor alamadık.

#### <a name="resolution"></a>Çözüm
Bkz: [Redis içinde verilerimi ne oldu?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) olası nedenleri ve çözümlemeleri için.

## <a name="server-side-troubleshooting"></a>Sunucu tarafı sorunlarını giderme
Bu bölümde önbellek sunucusu bir koşul nedeniyle oluşan sorun giderme sorunlar açıklanmaktadır.

* [Sunucudaki bellek baskısı](#memory-pressure-on-the-server)
* [Yüksek CPU kullanımı / sunucu iş yükü](#high-cpu-usage-server-load)
* [Sunucu tarafı bant genişliği aşıldı](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Sunucudaki bellek baskısı
#### <a name="problem"></a>Sorun
Sunucu tarafında bellek baskısı türlü isteklerinin işlenmesini geciktirebilir performans sorunlarına yol açar. Bellek baskısı geldiğinde sistem sayfa verileri için diskte sanal bellek için fiziksel bellekten genellikle sahip. Bu *sayfa hatalı* sistemin önemli ölçüde yavaşlamasına neden olur. Bu bellek baskısı birkaç olası nedenleri şunlardır: 

1. Tam kapasite önbelleğine verilerle doldurduktan. 
2. Redis yüksek bellek parçalanması - çoğunlukla büyük nesneler depolayarak neden görmesini (Redis - bakın gibi küçük bir nesneler için iyileştirilmiş [redis için ideal değer boyutu aralığı nedir? 100 KB çok büyük? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) sonrası ayrıntılarını). 

#### <a name="measurement"></a>Ölçüm
Redis bu sorunu belirlemenize yardımcı olabilecek iki ölçümleri kullanıma sunar. İlk `used_memory` ve diğer `used_memory_rss`. [Bu ölçümler](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) Azure Portalı'nda veya aracılığıyla kullanılabilir [Redis bilgisi](http://redis.io/commands/info) komutu.

#### <a name="resolution"></a>Çözüm
Bellek kullanımı sağlıklı tutmaya yardımcı olmak için yapabileceğiniz birkaç olası değişiklikleri şunlardır:

1. [Bellek ilkesini yapılandırma](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) ve anahtarlarınızı üzerinde zaman aşımı değeri ayarlayın. Bu yapılandırma parçalanma varsa yeterli olmayabilir.
2. [Maxmemory ayrılmış bir değer](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) için bellek parçalanması dengelemek için büyük olmasıdır.
3. Büyük önbelleğe alınmış nesnelerinizi küçük ilgili nesneleri içine bölün.
4. [Ölçek](cache-how-to-scale.md) daha büyük bir önbellek boyutu için.
5. Kullanıyorsanız bir [premium önbelleği etkin Redis kümesi ile](cache-how-to-premium-clustering.md), yapabilecekleriniz [parça sayısını artırmak](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Yüksek CPU kullanımı / sunucu iş yükü
#### <a name="problem"></a>Sorun
Yüksek CPU kullanımı, Redis hızla yanıt gönderdi olsa bile bir Redis yanıt zamanında işlemek istemci tarafı yük devredebildiğini anlamına gelebilir.

#### <a name="measurement"></a>Ölçüm
Azure Portal veya ilişkili performans sayacı aracılığıyla sistem geniş CPU kullanımını izleyin. İzleme konusunda dikkatli olun *işlem* CPU tek bir işlem aynı anda düşük CPU kullanımı sahip olduğundan, genel sistem zamanı CPU yüksek olabilir. Zaman aşımlarını karşılık ani CPU kullanımını izleyin.

#### <a name="resolution"></a>Çözüm
* Tüm önerileri gözden geçirin ve Uyarıları bahsedilen [Redis önbelleği Danışmanı](cache-configure.md#redis-cache-advisor).
* Ayrıca bu konu, diğer önerileri gözden geçirin ve [Azure Redis için en iyi uygulamaları](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f) daha fazla önbelleğiniz ve istemci iyileştirmek için tüm seçenekleri işe varsa görmek için. 
* Gözden geçirme [Azure Redis önbelleği performans](cache-faq.md#azure-redis-cache-performance) grafikler ve olabilir, bkz: yakınında geçerli katman üst eşikleri. Gerekirse, [ölçek](cache-how-to-scale.md) daha fazla CPU kapasiteli daha büyük bir önbellek katmanına. Premium katmanı zaten kullanıyorsanız, isteyebilirsiniz [ölçek genişletme kümeleme](cache-how-to-premium-clustering.md)


### <a name="server-side-bandwidth-exceeded"></a>Sunucu tarafı bant genişliği aşıldı
#### <a name="problem"></a>Sorun
Önbelleği örnekleri boyutuna bağlı olarak, bunlar sınırlamaları olabilir kullanılabilir olan ne kadar üzerindeki ağ bant genişliği. Ardından sunucu kullanılabilir bant genişliğini aşarsa, olabildiğince çabuk istemciye veri gönderilmez. Bu durum, zaman aşımına uğrayan yol açabilir.

#### <a name="measurement"></a>Ölçüm
İzleyeceğiniz `Cache Read` veri miktarıdır ölçüm okuma belirtilen raporlama aralığı sırasında (MB/sn) saniye başına megabayt önbelleğinden. Bu değer bu önbelleği tarafından kullanılan ağ bant genişliği karşılık gelir. Sunucu tarafı ağ bant genişliği sınırları için uyarıları ayarlama istiyorsanız, bunları bu kullanarak oluşturabilirsiniz `Cache Read` sayacı. Okumalar değerleri Karşılaştır [Bu tablo](cache-faq.md#cache-performance) çeşitli önbellek fiyatlandırma katmanlarına ve boyutları gözlemlenen bant genişliği sınırları.

#### <a name="resolution"></a>Çözüm
Fiyatlandırma katmanı ve önbellek boyutu için tutarlı bir şekilde gözlemlenen en yüksek bant genişliği olması durumunda dikkate [ölçeklendirme](cache-how-to-scale.md) fiyatlandırma katmanı için bir veya daha fazla ağ bant genişliği olan boyutu, değerleri kullanarak [Bu tablo](cache-faq.md#cache-performance) bir kılavuz olarak.

## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis zaman aşımı özel durumlar
StackExchange.Redis adlı bir yapılandırma ayarı kullanır `synctimeout` zaman uyumlu işlemler için varsayılan değer 1000 MS sahip. Zaman uyumlu çağrıyı stipulated süre içinde tamamlanmazsa, StackExchange.Redis istemcisi aşağıdaki örneğe benzer bir zaman aşımı hatası oluşturur:

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Bu hata iletisi, sorunun nedenini ve olası çözümü noktası yardımcı olabilecek ölçümleri içerir. Aşağıdaki tabloda, hata iletisi ölçümleri ayrıntılarını içerir.

| Hata iletisi ölçümü | Ayrıntılar |
| --- | --- |
| inst |Son zaman dilimi içinde: 0 komutları verilen |
| mgr |Yuva Yöneticisi gerçekleştiriyor `socket.select`, yani bir şey yapmak için; olan bir yuva belirtmek için işletim sistemi isteyen temelde: Bunu yapmak için herhangi bir şey yok düşünün değil çünkü okuyucu etkin olarak ağdan okuyor değil |
| Sırası |73 toplam devam eden işlemler vardır |
| qu |devam eden işlemlerin 6 gönderilmemiş kuyruktaki ve giden ağ yazılmadı |
| qs |devam eden işlemlerin 67 ve sunucuya gönderilen, ancak bir yanıt henüz kullanılabilir değil. Yanıt olabilir `Not yet sent by the server` veya`sent by the server but not yet processed by the client.` |
| qc |devam eden işlemlerin 0 yanıtları gördünüz ancak henüz tamamlama döngü üzerinde bekleyen nedeniyle tam olarak işaretlenmedi |
| wr |Bir (6 gönderilmemiş istekleri yok sayılan anlamına gelir) etkin yazıcı bayt/activewriters yok |
| içinde |Hiçbir etkin okuyucular vardır ve sıfır bayt NIC bayt/activereaders okumak kullanılabilir |

### <a name="steps-to-investigate"></a>Araştırmak için adımları
1. En iyi uygulama emin yaptıkça StackExchange.Redis istemcisi kullanılırken bağlanmak için şu deseni kullanıyor.

    ```csharp
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ````

    Daha fazla bilgi için bkz: [StackExchange.Redis kullanarak önbelleğe bağlanma](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

1. Azure Redis önbelleği ve istemci uygulaması azure'da aynı bölgede olduğundan emin olun. Örneğin, zaman aşımı önbelleğiniz Doğu ABD olduğunu ancak istemci Batı ABD olduğunda ve istek içinde işlemini tamamlamazsa elde `synctimeout` aralığı veya alınırken zaman aşımı yerel geliştirme makinenizden ayıklarken. 
   
    Yüksek oranda önbelleği için önerilen ve aynı Azure bölgesinde istemci. Çapraz bölge çağrıları içeren bir senaryo varsa ayarlamalısınız `synctimeout` aralığı ekleyerek varsayılan 1000 ms aralık değerinden daha yüksek bir değere bir `synctimeout` bağlantı dizesindeki özelliği. Aşağıdaki örnek, bir StackExchange.Redis önbelleği bağlantı dizesi parçacığıyla gösterir bir `synctimeout` 2000 MS.
   
        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...
2. En son sürümünü kullandığından emin olun [StackExchange.Redis NuGet paketi](https://www.nuget.org/packages/StackExchange.Redis/). Sürekli olarak en son sürümünü olması önemlidir daha sağlam zaman aşımına uğrayan yapmak için kodda düzeltilen vardır.
3. Bant genişliği sınırlamaları sunucu veya istemci tarafından bağlı isteği yoksa, bunları tamamlamak ve böylece zaman aşımlarına neden daha uzun sürer. Zaman aşımı nedeniyle sunucu üzerindeki ağ bant genişliği olup olmadığını görmek için bkz: [sunucu tarafı bant genişliği aşıldı](#server-side-bandwidth-exceeded). Zaman aşımı nedeniyle istemci ağ bant genişliği olup olmadığını görmek için bkz: [istemci-tarafı bant genişliği aşıldı](#client-side-bandwidth-exceeded).
4. Sunucu veya istemci, CPU alma bağlı?
   
   * CPU tarafından içinde değil isteğin işlenmesi için neden olabilecek, istemcide bağlı durumunda denetleyin `synctimeout` aralığı, böylece bir zaman aşımı neden. Daha büyük bir istemci boyutu taşıma veya yükü dağıtma Bu sorun denetlemek için yardımcı olabilir. 
   * CPU alıyorsanız onay izleyerek sunucuda bağlı `CPU` [performans ölçüm önbelleğe](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). İstekleri Redis CPU'ya durumdayken gelen isteklere zaman aşımına neden olabilir. Bu durumu çözmek için yük premium önbelleğinde birden fazla parça genelinde dağıtır veya bir büyük veya fiyatlandırma katmanına yükseltin. Daha fazla bilgi için bkz: [sunucu tarafı bant genişliği aşıldı](#server-side-bandwidth-exceeded).
5. Sunucu üzerinde işlem için uzun süren komutları var mı? Redis sunucu üzerinde işlem için uzun zaman ayırdığınız komutları uzun süre çalışan zaman aşımlarına neden olabilir. Uzun süre çalışan komutlar bazı örnekleri şunlardır `mget` anahtarları, çok sayıda ile `keys *` veya lua betikleri'kötü yazılmış. Redis-cli istemcisini kullanarak Azure Redis önbelleği örneğine bağlanın veya kullanmak [Redis konsol](cache-configure.md#redis-console) çalıştırıp [SlowLog](http://redis.io/commands/slowlog) beklenenden uzun sürüyor isteği olup olmadığını görmek için komutu. Redis sunucu ve StackExchange.Redis daha az büyük istekleri yerine çok sayıda küçük istekleri için iyileştirilir. Verilerinizi daha küçük parçalara bölme şeyleri burada artırabilir. 
   
    Redis-cli ve stunnel kullanarak Azure Redis önbelleği SSL uç noktasına bağlanma hakkında daha fazla bilgi için bkz: [Redis Önizleme sürümü için ASP.NET oturum durumu sağlayıcısı Duyurusu](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blog postası. Daha fazla bilgi için bkz: [SlowLog](http://redis.io/commands/slowlog).
6. Yüksek Redis sunucusu yükü zaman aşımlarına neden olabilir. Sunucu iş yükü izleyerek izleyebilirsiniz `Redis Server Load` [performans ölçüm önbelleğe](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Sunucu yükü 100 (en fazla değer) redis sunucu isteklerini işleme hiçbir boşta kalma süresi ile meşgul olduğunu belirtir. Belirli isteklerini tüm sunucu özelliğine sürüyor durumunda görmek için önceki paragrafta açıklanan SlowLog komutunu çalıştırın. Daha fazla bilgi için bkz: [yüksek CPU kullanımı / sunucu iş yükü](#high-cpu-usage-server-load).
7. İstemci tarafında bir ağ blip neden olan başka bir olay oluştu? (Web, çalışan rolü veya bir Iaas VM) istemcide istemci örneklerinin sayısını yukarı veya aşağı Ölçeklendirmesi gibi bir olay oluştu veya Otomatik ölçek ve istemci yeni bir sürümü dağıtma etkin kontrol edilsin mi? Bizim sınama sırasında bu otomatik ölçeklendirme veya ölçeklendirmeyi bulduk / aşağı neden giden ağ bağlantısı için birkaç saniye kaybolabilir. StackExchange.Redis kod bu gibi olaylar için esnek ve yeniden bağlanır. Yeniden bağlama bu süre boyunca, sıranın hiçbir isteği zaman aşımına görüntüleyebilirsiniz.
8. Birçok küçük istek zaman aşımına uğradı Redis önbelleği önceki büyük bir istek oluştu? Parametre `qs` hata iletisi, kaç istek istemciden sunucuya gönderilen, ancak bir yanıt henüz işlenmedi bildirir. StackExchange.Redis tek bir TCP bağlantı kullanır ve bir yanıt aynı anda yalnızca okuyabilir çünkü bu değer büyüyor. İlk işlem zaman aşımına uğradı olsa bile, sunucunun denetleyicisinden gönderilen verilerin durdurmaz ve büyük isteği tamamlanmadan zaman aşımları neden diğer istekleri engellenir. Önbelleğinizi işleminizi iş yükü için yeterince büyük olduğundan emin olmanın ve büyük değerler daha küçük parçalara bölme zaman aşımları olasılığını en aza bir çözümdür. Başka bir olası çözüm havuzu kullanmaktır `ConnectionMultiplexer` nesneleri, istemci ve az yüklenen seçin `ConnectionMultiplexer` yeni bir istek gönderirken. Bu tek bir zaman aşımı de zaman aşımı diğer isteklerine neden olmasını engellemeye.
9. Kullanıyorsanız `RedisSessionStateprovider`, yeniden deneme zaman aşımı doğru ayarladığınızdan emin olun. `retrytimeoutInMilliseconds`daha yüksek olmalıdır `operationTimeoutinMilliseonds`, aksi halde ortaya yeniden deneme yok. Aşağıdaki örnekte `retrytimeoutInMilliseconds` 3000 olarak ayarlanır. Daha fazla bilgi için bkz: [Azure Redis önbelleği için ASP.NET oturum durumu sağlayıcısı](cache-aspnet-session-state-provider.md) ve [oturum durumu sağlayıcısı ve çıkış önbelleği sağlayıcısı yapılandırma parametrelerini kullanma](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).

    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


1. Azure Redis önbelleği sunucu tarafından bellek kullanımı denetleyin [izleme](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` ve `Used Memory`. Bir çıkarma İlkesi yerinde ise, Redis başlatır çıkarma, anahtarları ne zaman `Used_Memory` önbellek boyutu. İdeal olarak, `Used Memory RSS` yalnızca biraz daha yüksek olmalıdır `Used memory`. Büyük bir fark (iç veya dış. bellek parçalanması olduğu anlamına gelir. Zaman `Used Memory RSS` olan değerinden `Used Memory`, önbellek parçası, işletim sistemi tarafından takas anlamına gelir. Bu değişimi meydana gelirse, bazı önemli gecikmeleri bekleyebilirsiniz. Redis, ayırma için bellek sayfaları, yüksek nasıl eşlendiğini üzerinde denetime sahip olmadığından `Used Memory RSS` genellikle bellek kullanımında ani sonucudur. Redis bellek boşaltır, bellek ayırıcısı geri verilir ve ayırıcı olabilir veya bellek sisteme geri vermeyebilir. Arasında bir farklılık olması `Used Memory` işletim sistemi tarafından bildirilen değeri ve bellek tüketimi. Bu olabilir olgu nedeniyle bellek olduğundan kullanılan ve Redis, ancak sistem geri verilen değil. Bellek sorunları azaltmaya yardımcı olmak için aşağıdaki adımları gerçekleştirebilirsiniz:
   
   * Sistemde bellek sınırlamaları sunmayı çalışmayan için büyük boyutlu bir önbellek yükseltin.
   * Zaman aşımı değeri tuşlar eski değerleri proaktif olarak çıkarılacak şekilde ayarlayın.
   * İzleyici `used_memory_rss` ölçüm önbelleğe. Bu değer, önbellek boyutunu yaklaştığında, performans sorunlarını görmeye başlayacaksınız olasılığı yüksektir. Premium önbelleği kullanarak veya daha büyük bir önbellek boyutu yükseltme olan verileri birden fazla parça dağıtır.
   
   Daha fazla bilgi için bkz: [sunucuda bellek baskısı](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Ek bilgiler
* [Hangi Redis Önbelleği teklifini ve boyutunu kullanmalıyım?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
* [Nasıl Kıyaslama ve my önbellek performansını test etme?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Redis komutları nasıl çalıştırabilir miyim?](cache-faq.md#how-can-i-run-redis-commands)
* [Azure Redis önbelleğini izleme](cache-how-to-monitor.md)

