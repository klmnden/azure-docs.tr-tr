---
title: Azure önbelleği için Redis sorunlarını giderme | Microsoft Docs
description: Redis için Azure önbellek ile ilgili yaygın sorunları çözmeyi öğrenin.
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 928b9b9c-d64f-4252-884f-af7ba8309af6
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: azure-cache-for-redis
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: wesmc
ms.openlocfilehash: a0bf8543338043d9a1990fd2be33a65a478af721
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53019966"
---
# <a name="how-to-troubleshoot-azure-cache-for-redis"></a>Azure önbelleği için Redis sorunlarını giderme
Bu makalede, Azure Cache aşağıdaki kategorileri Redis sorunlarına yönelik sorun giderme kılavuzu sağlar.

* [İstemci tarafı sorunlarını giderme](#client-side-troubleshooting) : Bu bölüm tanımlama hakkında yönergeler sağlar ve Redis için Azure önbellek'e bağlanırken uygulama nedeniyle sorunlarını çözme.
* [Sunucu tarafı sorunlarını giderme](#server-side-troubleshooting) : Bu bölüm tanımlama hakkında yönergeler sağlar ve sorunlarını çözme neden Azure önbelleği için Redis sunucu tarafı üzerinde.
* [StackExchange.Redis zaman aşımı özel durumları](#stackexchangeredis-timeout-exceptions) -Bu bölümde, StackExchange.Redis istemcisi kullanılırken sorun giderme konuları hakkında bilgi sağlar.

> [!NOTE]
> Bazı sorun giderme adımları bu kılavuzun Redis komutları çalıştırın ve çeşitli performans ölçümleri izlemek için yönergeler içerir. Makaleler, daha fazla bilgi ve yönergeler için bkz: [ek bilgi](#additional-information) bölümü.
> 
> 

## <a name="client-side-troubleshooting"></a>İstemci tarafı sorunlarını giderme
Bu bölümde, istemci uygulamasındaki bir koşul nedeniyle meydana gelen sorun giderme konuları anlatılmaktadır.

* [İstemci üzerinde bellek baskısı](#memory-pressure-on-the-client)
* [Trafik](#burst-of-traffic)
* [Yüksek istemci CPU kullanımı](#high-client-cpu-usage)
* [İstemci tarafı bant genişliği aşıldı](#client-side-bandwidth-exceeded)
* [Büyük istek/yanıt boyutu](#large-requestresponse-size)
* [Redis verilerimi ne oldu?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>İstemci üzerinde bellek baskısı
#### <a name="problem"></a>Sorun
Bellek baskısı istemci makine üzerinde her türlü herhangi bir gecikme olmadan Redis örneği tarafından gönderilen verilerin işlenmesini geciktirebilir performans sorunlarına neden olur. Bellek baskısı ulaştığında, sistemin sanal bellek, disk üzerinde fiziksel bellekten sayfa verileri genellikle vardır. Bu *sayfasında hatalı* sistemin önemli ölçüde yavaşlamasına neden olur.

#### <a name="measurement"></a>Ölçüm
1. Kullanılabilir bellek aşmadığından emin olmak için makineye bellek kullanımı izleyin. 
2. İzleyici `Page Faults/Sec` performans sayacı. Çoğu sistemleri bile normal işlem sırasında bazı sayfa hataları sahip, karşılık gelen zaman aşımlarını bu sayfa hataları performans sayacı artış için kadar izleyin.

#### <a name="resolution"></a>Çözüm
Daha fazla belleğe sahip VM boyutu daha büyük bir istemciye istemcinizi yükseltin veya bellek tüketimini azaltmak için bellek kullanım desenleri ile inin.

### <a name="burst-of-traffic"></a>Trafik
#### <a name="problem"></a>Sorun
Trafik artışlarıyla başa düşük ile birleştirilmiş `ThreadPool` ayarları zaten Redis sunucu tarafından gönderilen, ancak henüz istemci tarafında kullanılan veri işlenmesinde gecikmelere neden.

#### <a name="measurement"></a>Ölçüm
İzleyici nasıl, `ThreadPool` istatistikleri kod kullanarak zaman içinde değiştirmek [şöyle](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Ayrıca bakabilirsiniz `TimeoutException` StackExchange.Redis ileti. Örnek aşağıda verilmiştir:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

Önceki iletide, ilgi çekici birkaç sorun vardır:

1. Dikkat `IOCP` bölümü ve `WORKER` bölüm sahip olduğunuz bir `Busy` değerinden büyük bir değer `Min` değeri. Yani bu fark, `ThreadPool` ayarlarının ayarlanması gerekir.
2. Ayrıca bkz `in: 64221`. Bu değer, 64,211 bayt sayısı çekirdek yuva katmanında alındı ancak (örneğin, StackExchange.Redis) uygulama tarafından henüz okuma henüz gösterir. Bu fark, uygulamanızı verileri ağ üzerinden sunucu için gönderiyor olabildiğince çabuk okuma değil, genellikle anlamına gelir.

#### <a name="resolution"></a>Çözüm
Yapılandırma, [iş parçacığı havuzu ayarları](https://gist.github.com/JonCole/e65411214030f0d823cb) patlama senaryoları, iş parçacığı havuzu hızlı bir şekilde altında ölçeklendirilirse emin emin olun.

### <a name="high-client-cpu-usage"></a>Yüksek istemci CPU kullanımı
#### <a name="problem"></a>Sorun
Yüksek CPU kullanımı istemcide gerçekleştirmek için istenmişti çalışmak system yetişemiyor göstergesidir. Bu durum, istemci Redis hızla yanıt gönderilmiş olsa da, redis zamanında yanıt işlemek başarısız olabileceği anlamına gelir.

#### <a name="measurement"></a>Ölçüm
Sistem genelinde CPU kullanımı ilişkili performans sayacı veya Azure Portalı aracılığıyla izleyin. İzleme dikkatli olun *işlem* CPU tek bir işlem aynı anda düşük CPU kullanımı olabilir çünkü o genel sistem zamanı CPU yüksek olabilir. Zaman aşımlarını karşılık gelen ani CPU kullanımını izleyin. Yüksek CPU sonucunda da yüksek görebilirsiniz `in: XXX` değerler `TimeoutException` hata iletileri açıklandığı [trafik](#burst-of-traffic) bölümü.

> [!NOTE]
> StackExchange.Redis 1.1.603 ve daha sonra içerir `local-cpu` içinde ölçüm `TimeoutException` hata iletileri. En son sürümünü kullandığından emin olmak [StackExchange.Redis NuGet paketi](https://www.nuget.org/packages/StackExchange.Redis/). Sürekli olarak en son sürümüne sahip olunması önemlidir daha sağlam zaman aşımına uğrayan sağlamak için kodda düzeltilmekte olan hataları vardır.
> 
> 

#### <a name="resolution"></a>Çözüm
Daha fazla CPU kapasiteli daha büyük bir VM boyutu yükseltin veya CPU'daki ani değişikliklerin neden olduğunu araştırın. 

### <a name="client-side-bandwidth-exceeded"></a>İstemci tarafı bant genişliği aşıldı
#### <a name="problem"></a>Sorun
İstemci makineler mimarisine bağlı olarak, sınırlamalar olabilir kullanılabilir olan ne kadar üzerindeki ağ bant genişliği. İstemci, ağ kapasitesinin aşırı yükleme tarafından kullanılabilir bant genişliğini aşarsa, ardından veri istemci tarafında sunucusunu göndermeye olabildiğince çabuk işlenmedi. Bu durum, zaman aşımına uğrayan neden olabilir.

#### <a name="measurement"></a>Ölçüm
Bant genişliği kullanım kod kullanarak zaman içinde nasıl değiştiğini izleme [şöyle](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Bu kod, bazı ortamlarda (örneğin, Azure web siteleri) sınırlı izinler ile başarılı bir şekilde çalışmayabilir.

#### <a name="resolution"></a>Çözüm
İstemci VM boyutunu artırın veya ağ bant genişliği tüketimini azaltabilir.

### <a name="large-requestresponse-size"></a>Büyük istek/yanıt boyutu
#### <a name="problem"></a>Sorun
Büyük bir istek/yanıt zaman aşımı neden olabilir. Örneğin, istemcide yapılandırılan, zaman aşımı değeri 1 saniyedir varsayalım. Uygulamanızı (aynı fiziksel ağ bağlantısını kullanarak) aynı anda iki anahtar (örneğin, 'A' ve 'B') ister. Her iki 'A' ve 'B' sunucusuna Kablodaki art arda yanıtlar için beklemenize gerek kalmadan gönderdiği istekleri, çoğu istemci isteklerinin "Pipelining" destekler. Sunucu yanıtları aynı sırada geri gönderir. Yanıt 'A' yeterince büyük ise, sonraki istekler için zaman aşımı çoğu yemek. 

Aşağıdaki örnek, bu senaryo gösterir. Bu senaryoda, istek 'A' ve 'B' hızlı bir şekilde gönderilir, hızlı yanıt 'A' ve 'B' gönderme sunucuyu başlatır, ancak hızlı bir şekilde sunucu yanıtı olsa bile veri aktarım süreleri nedeniyle 'B' bir istek ve zaman aşımına arkasında takılabilir.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Ölçüm
Bu istek/yanıt ölçmek için zor bir bilgisayardır. Temelde büyük istekleri ve yanıtlarını izlemek için istemci kodunuz izleme gerekir. 

#### <a name="resolution"></a>Çözüm
1. Redis, çok sayıda birkaç büyük değerler yerine küçük değerler için optimize edilmiştir. Tercih edilen verilerinizi ilgili küçük değerler bölmeniz çözümdür. Bkz: [redis için ideal değer boyut aralığı nedir? 100 KB çok büyük? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) sonrası neden daha küçük değerler önerilir etrafında Ayrıntılar için.
2. Veri Aktarım süreleri daha büyük yanıtları için azaltma, daha yüksek bant genişliği özellikleri almak için (istemci ve Azure önbelleği için Redis sunucu) için sanal makinenizin boyutunu artırın. Daha fazla bant genişliği yalnızca sunucunun veya yalnızca açık alma istemci yeterli olmayabilir. Bant genişliği kullanımını ölçmek ve şu anda kullandığınız VM'nin boyutunu özelliklerini karşılaştırın.
3. Sayısını artırmak `ConnectionMultiplexer` farklı bağlantılar üzerinden kullanın ve hepsini bir kez deneme istekleri nesneleri.

### <a name="what-happened-to-my-data-in-redis"></a>Redis verilerimi ne oldu?
#### <a name="problem"></a>Sorun
Miyim belirli Azure Önbelleğimin Redis örneği için oluşturulacak veri bekleniyordu, ancak var. olmak görünen kaydetmedi.

#### <a name="resolution"></a>Çözüm
Bkz: [Redis verilerimi ne?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) olası nedenleri ve çözümlemeleri için.

## <a name="server-side-troubleshooting"></a>Sunucu tarafı sorunlarını giderme
Bu bölümde, önbellek sunucusunda bir koşul nedeniyle meydana gelen sorun giderme konuları anlatılmaktadır.

* [Sunucudaki bellek baskısı](#memory-pressure-on-the-server)
* [Yüksek CPU kullanımı / sunucu yükü](#high-cpu-usage-server-load)
* [Sunucu tarafı bant genişliği aşıldı](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Sunucudaki bellek baskısı
#### <a name="problem"></a>Sorun
Bellek baskısı sunucu tarafında isteklerinin işlenmesini geciktirebilir performans sorunlarını her türlü yol açar. Bellek baskısı ulaştığında, sistemin sanal bellek, disk üzerinde fiziksel bellekten sayfa verileri genellikle vardır. Bu *sayfasında hatalı* sistemin önemli ölçüde yavaşlamasına neden olur. Bu bellek baskısı birkaç olası nedeni vardır: 

1. Tam kapasite önbelleğe verilerle doldurmuş olmanız. 
2. Redis büyük nesneler depolayarak nedeni genellikle en yüksek bellek parçalanması - görmeye (Redis küçük nesneleri için - bakın optimize [redis için ideal değer boyut aralığı nedir? 100 KB çok büyük? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) sonrası ayrıntılarını). 

#### <a name="measurement"></a>Ölçüm
Redis bu sorunu belirlemenize yardımcı olabilecek iki ölçüm sunar. İlk `used_memory` ve diğeri `used_memory_rss`. [Bu ölçümler](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) aracılığıyla veya Azure Portal'da kullanılabilir [Redis bilgisi](http://redis.io/commands/info) komutu.

#### <a name="resolution"></a>Çözüm
Bellek kullanımı sağlıklı tutmaya yardımcı olmak için yapabileceğiniz birkaç olası değişiklikler şunlardır:

1. [Bir bellek ilkesini yapılandırma](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) ve anahtarlarınızı zaman aşımı değeri ayarlayın. Bu yapılandırma parçalanma varsa yeterli olmayabilir.
2. [Maxmemory ayrılmış değerini yapılandırmak](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) için bellek parçalanması dengelemek için büyük olmasıdır.
3. Büyük, önbelleğe alınan nesneleri küçük ilgili nesneleri halinde bölün.
4. [Ölçek](cache-how-to-scale.md) daha büyük bir önbellek boyutu için.
5. Kullanıyorsanız bir [etkin Redis kümesi ile premium önbellek](cache-how-to-premium-clustering.md), yapabilecekleriniz [parça sayısını artırmak](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Yüksek CPU kullanımı / sunucu yükü
#### <a name="problem"></a>Sorun
Yüksek CPU kullanımı, Redis hızla yanıt gönderilmiş olsa da, redis zamanında yanıt işlemek istemci tarafı yük devredebildiğini anlamına gelebilir.

#### <a name="measurement"></a>Ölçüm
Sistem genelinde CPU kullanımı ilişkili performans sayacı veya Azure Portalı aracılığıyla izleyin. İzleme dikkatli olun *işlem* CPU tek bir işlem aynı anda düşük CPU kullanımı olabilir çünkü o genel sistem zamanı CPU yüksek olabilir. Zaman aşımlarını karşılık gelen ani CPU kullanımını izleyin.

#### <a name="resolution"></a>Çözüm
* Tüm önerileri gözden geçirin ve uyarılar bahsedilen içinde [Azure önbelleği için Redis Advisor](cache-configure.md#azure-cache-for-redis-advisor).
* Ayrıca bu konuda, diğer önerileri gözden geçirin ve [Azure Redis için en iyi](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f) önbelleğinizi ve istemci daha da iyileştirmek için tüm seçenekleri işe, görmek için. 
* Gözden geçirme [Azure önbelleği için Redis performans](cache-faq.md#azure-cache-for-redis-performance) grafikleri ve olabilir, bkz: neredeyse geçerli katman üst eşik. Gerekirse, [ölçek](cache-how-to-scale.md) daha büyük bir önbellek katmanına ile daha fazla CPU kapasitesi. Premium katmanı zaten kullanıyorsanız, isteyebilirsiniz [ölçeğini genişletme ile kümeleme](cache-how-to-premium-clustering.md)


### <a name="server-side-bandwidth-exceeded"></a>Sunucu tarafı bant genişliği aşıldı
#### <a name="problem"></a>Sorun
Önbellek örnekleri boyutuna bağlı olarak, sınırlamalar olabilir kullanılabilir olan ne kadar üzerindeki ağ bant genişliği. Ardından sunucu kullanılabilir bant genişliğini aşarsa, olabildiğince çabuk istemciye veri gönderilmez. Bu durum, zaman aşımına uğrayan neden olabilir.

#### <a name="measurement"></a>Ölçüm
İzleyeceğiniz `Cache Read` önbelleğin megabayt (MB/sn) belirtilen raporlama aralığı sırasında saniye başına veri miktarıdır ölçüm, okuma. Bu değer bu önbelleği tarafından kullanılan ağ bant genişliği karşılık gelir. Uyarılar için sunucu tarafı ağ bant genişliği sınırlarını ayarlamak istiyorsanız, bunu kullanarak bunları oluşturabilirsiniz `Cache Read` sayacı. Değerleri, okumalar karşılaştırma [bu tabloda](cache-faq.md#cache-performance) gözlemlenen bant genişliği sınırlarını çeşitli önbellek fiyatlandırma katmanları ve boyutları için.

#### <a name="resolution"></a>Çözüm
Fiyatlandırma katmanı ve önbellek boyutu için tutarlı bir şekilde gözlemlenen en yüksek bant genişliği kullanıyorsanız [ölçeklendirme](cache-how-to-scale.md) bir fiyatlandırma katmanı veya daha fazla ağ bant genişliğine sahip boyutu için değerleri kullanarak [bu tabloda](cache-faq.md#cache-performance)bir kılavuz olarak.

## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis zaman aşımı özel durumları
StackExchange.Redis yapılandırma adlandırılmış ayarı kullanan `synctimeout` zaman uyumlu işlemler için sahip olduğunuz bir varsayılan değer 1000 MS. Zaman uyumlu çağrıyı görünürlüğe sürede tamamlamazsa StackExchange.Redis istemcisi aşağıdaki örneğe benzer bir zaman aşımı hatası oluşturur:

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Bu hata iletisi sorunun nedenini ve olası çözümü noktası yardımcı olabilecek ölçümleri içerir. Aşağıdaki tablo, hata iletisi ölçümleri ilgili ayrıntıları içerir.

| Hata iletisi ölçümü | Ayrıntılar |
| --- | --- |
| yönerge |Son zaman dilimi içinde: 0 komut verildi |
| Yöneticisi |Yuva Yöneticisi gerçekleştiriyor `socket.select`, yani bir şey yapmak için; olan bir yuvayı belirtmek için işletim sistemi isteyen temel: Bunu yapmak için herhangi bir şey yoktur düşünün değil çünkü okuyucu etkin olarak ağdan okuyor değil |
| kuyruk |73 toplam devam eden işlemler vardır. |
| QU |devam eden işlemlerin 6 kuyruktaki gönderilmeyen ve giden ağ yazılmadı |
| qs |devam eden işlemlerin 67 sunucuya gönderildikten ancak yanıt henüz kullanıma sunulmamıştır. Yanıt olabilir `Not yet sent by the server` veya `sent by the server but not yet processed by the client.` |
| QC |devam eden işlemlerin 0 yanıtları gördünüz ancak henüz tamamlandı, tamamlanma döngü üzerinde bekleyen nedeniyle silinmek üzere işaretlenen değil |
| wr |(6 gönderilmemiş istekleri yok sayılan anlamına gelir) etkin yazan bayt/activewriters yoktur |
| in |Hiçbir etkin okuyucular vardır ve sıfır bayt NIC bayt/activereaders okumak kullanılabilir |

### <a name="steps-to-investigate"></a>Araştırmak için adımları
1. En iyi uygulama emin yaparken, StackExchange.Redis istemcisi kullanılırken bağlanmak için şu desene kullanıyorsunuz.

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

    Daha fazla bilgi için [StackExchange.Redis kullanarak önbelleğe bağlanma](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

1. Azure önbelleği için Redis hem de istemci uygulaması aynı Azure bölgesinde olduğundan emin olun. Örneğin, zaman aşımları, önbellek, Doğu ABD bölgesinde, ancak istemci Batı ABD'de ve içinde istek işlemini tamamlamazsa olduğunda ekleyene `synctimeout` aralığı veya alınırken zaman aşımı, yerel geliştirme makinenizde hata ayıklaması yaparken. 
   
    Yüksek oranda önbelleğe sahip önerilir ve aynı Azure bölgesinde istemci. Bölgeler arası aramalar içeren bir senaryo varsa ayarlamalısınız `synctimeout` aralığı ekleyerek varsayılan 1000 MS'den aralığından daha yüksek bir değere bir `synctimeout` bağlantı dizesi özelliği. Aşağıdaki örnek, bir StackExchange.Azure önbelleği için Redis bağlantı dizesi parçacığıyla gösterir. bir `synctimeout` 2000 MS.
   
        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...
2. En son sürümünü kullandığından emin olmak [StackExchange.Redis NuGet paketi](https://www.nuget.org/packages/StackExchange.Redis/). Sürekli olarak en son sürümüne sahip olunması önemlidir daha sağlam zaman aşımına uğrayan sağlamak için kodda düzeltilmekte olan hataları vardır.
3. Bant genişliği sınırlamaları sunucu veya istemci tarafından bağlanmış isteği yoksa, bunları tamamlayın ve böylece zaman aşımları neden daha uzun sürer. Zaman aşımı nedeniyle sunucu üzerindeki ağ bant genişliği olup olmadığını görmek için bkz: [sunucu tarafı bant genişliği aşıldı](#server-side-bandwidth-exceeded). Zaman aşımı, istemci ağ bant genişliği nedeniyle olup olmadığını görmek için bkz: [istemci-tarafı bant genişliği aşıldı](#client-side-bandwidth-exceeded).
4. CPU alma, sunucudaki veya istemcideki bağlıdır?
   
   * CPU ile içinde değil isteğin işlenmesi için neden olabilecek, istemcide bağlı olup olmadığını denetleyin `synctimeout` aralığı, bu nedenle bir zaman aşımı neden olur. Taşıma için daha büyük bir istemci boyutu ya da yük dağıtma, bu sorunu denetlemek için yardımcı olabilir. 
   * CPU alıyorsanız denetleyin izlenerek sunucuda bağlı `CPU` [performans ölçümü önbelleğe](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Redis CPU'ya bağlıdır, ancak gelen istekler, bu istekleri zaman aşımına neden olabilir. Bu durumu çözmek için yükü premium önbellekteki birden fazla parçaya dağıtmak veya bir büyük veya fiyatlandırma katmanına yükseltin. Daha fazla bilgi için [sunucu tarafı bant genişliği aşıldı](#server-side-bandwidth-exceeded).
5. Sunucu üzerinde işlem için uzun süren komutları var mı? Redis sunucu üzerinde işlem için uzun sürüp komutları uzun süre çalışan zaman aşımı neden olabilir. Uzun süre çalışan komutlar bazı örnekleri şunlardır `mget` anahtarları, çok sayıda ile `keys *` kötü, lua komut yazılamaz veya okunamaz. Redis-cli istemcisini kullanarak Redis örneği için Azure önbelleğinize bağlanma veya kullanın [Redis Konsolu](cache-configure.md#redis-console) çalıştırıp [Slowlog'u](http://redis.io/commands/slowlog) beklenenden daha uzun süren istekleri olup olmadığını görmek için komutu. Redis sunucusu ve StackExchange.Redis daha az büyük istekleri yerine çok sayıda küçük isteği için iyileştirilmiştir. Verilerinizi daha küçük öbeklere ayırma şeyleri burada artırabilir. 
   
    Azure önbelleği için redis-cli ve stunnel kullanarak Redis SSL uç bağlanma hakkında daha fazla bilgi için bkz: [Redis Önizleme sürümü için ASP.NET oturum durumu sağlayıcısı ile tanışın](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blog gönderisi. Daha fazla bilgi için [Slowlog'u](http://redis.io/commands/slowlog).
6. Yüksek Redis sunucu yükü, zaman aşımı neden olabilir. Sunucu iş yükü izleyerek izleyebilirsiniz `Redis Server Load` [performans ölçümü önbelleğe](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Bir sunucu yükü 100 (en yüksek değer), redis sunucusunun istekleri işleme boş kalma süresi ile meşgul olduğunu gösterir. Belirli isteklerini tüm sunucu özelliğine sürüyor durumunda görmek için önceki paragrafta açıklandığı gibi Slowlog'u komutu çalıştırın. Daha fazla bilgi için [yüksek CPU kullanımı / sunucu yükü](#high-cpu-usage-server-load).
7. İstemci tarafında ağ blip neden olan herhangi bir olay oluştu? (Web, çalışan rolü veya bir Iaas VM) istemcide istemci örneklerinin sayısını artırma veya azaltma gibi bir olay oluştu veya yeni bir otomatik ölçeklendirme ve istemci sürümünü dağıtmaya etkin kontrol? Test işlemlerimizi, bu otomatik ölçeklendirme veya büyütme bulduk/aşağı neden giden ağ bağlantısını için birkaç saniye kaybolabilir. StackExchange.Redis kod böyle olaylara esnektir ve yeniden bağlanır. Yeniden bağlama bu süre boyunca sırasındaki tüm istekler zaman aşımı olabilir.
8. Birçok küçük istek zaman aşımına uğradı Redis için Azure Cache için önceki büyük bir istek vardı? Parametre `qs` hata iletisi, kaç istek istemciden sunucuya gönderilen, ancak bir yanıt henüz işlenmemiş bildirir. StackExchange.Redis tek bir TCP bağlantısını kullanır ve bir yanıt aynı anda yalnızca okuyabilir çünkü bu değer büyüyen tutun. İlk işlem zaman aşımına uğradı olsa da, sunucunun içine/dışına gönderilen veri durdurmaz ve büyük isteği tamamlanmadan zaman aşımı neden diğer istekleri engellenir. Önbelleğinizi yükünüz için yeterince büyük olduğundan emin olmanın ve büyük değerler daha küçük öbeklere ayırma zaman aşımları olasılığını en aza indirmek bir çözümdür. Başka bir olası çözümü havuzu kullanmaktır `ConnectionMultiplexer` istemcinizde nesneleri ve az yüklenen seçin `ConnectionMultiplexer` yeni bir isteği gönderirken. Bu tek bir zaman aşımı da zaman aşımı giden diğer isteklerin neden olmasını engeller.
9. Kullanıyorsanız `RedisSessionStateprovider`, ayarladığınız yeniden deneme zaman aşımı doğru emin olun. `retrytimeoutInMilliseconds` daha yüksek olmalıdır `operationTimeoutinMilliseonds`, aksi takdirde, yeniden deneme yok oluşur. Aşağıdaki örnekte `retrytimeoutInMilliseconds` 3000 olarak ayarlanır. Daha fazla bilgi için [Redis için Azure Cache için ASP.NET oturum durumu sağlayıcısı](cache-aspnet-session-state-provider.md) ve [çıktı önbelleği sağlayıcısı ile oturum durumu sağlayıcısı ve yapılandırma parametrelerini kullanma](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).

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


1. Azure önbelleği için Redis sunucusu tarafından bellek kullanımını kontrol [izleme](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` ve `Used Memory`. Çıkarma İlkesi yerinde ise, Redis başlatır çıkarma, anahtarları ne zaman `Used_Memory` önbellek boyutu. İdeal olarak, `Used Memory RSS` yalnızca biraz daha yüksek olmalıdır `Used memory`. Büyük bir fark (iç veya dış) bellek parçalanması olduğu anlamına gelir. Zaman `Used Memory RSS` olduğu küçüktür `Used Memory`, önbellek parçası, işletim sistemi tarafından takas anlamına gelir. Takas bu meydana gelirse, bazı önemli bir gecikme bekleyebilirsiniz. Redis bellek sayfalarına, yüksek ayırmalarını nasıl eşleştirildiğini üzerinde denetim olmaması nedeniyle `Used Memory RSS` genellikle bellek kullanımı bir ani değişiklik sonucudur. Redis bellek serbest bırakma, bellek ayırıcısı için geri verilir ve ayırıcı olabilir veya sistemde bellek geri vermeyebilir. Arasında bir tutarsızlık olması `Used Memory` işletim sistemi tarafından bildirilen değeri ve bellek tüketimi. Hale gelmesi nedeniyle gerçek bellek olduğundan kullanılan ve Redis, ancak sistemin geri verilen değil. Bellek sorunlarının azaltılmasına yardımcı olmak için aşağıdaki adımları gerçekleştirebilirsiniz:
   
   * Önbellek sistemde bellek sınırlamaları karşılaştıklarında çalışmayan daha büyük bir boyuta yükseltin.
   * Zaman aşımı değeri tuşlar eski değerleri proaktif bir şekilde çıkarılan şekilde ayarlayın.
   * İzleyici `used_memory_rss` ölçüm önbellek. Bu değer, önbellek boyutu yaklaştığında, olası performans sorunlarını görmeye başlayacaksınız. Premium önbellek kullanmanın veya yükseltmek için daha büyük bir önbellek boyutu olan verileri birden fazla parçaya dağıtmak.
   
   Daha fazla bilgi için [sunucudaki bellek baskısı](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Ek bilgiler
* [Hangi Azure önbelleği için Redis teklifini ve boyutunu kullanmalıyım?](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)
* [Nasıl Kıyaslama ve önbelleğimin performansını test etme?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Redis komutları nasıl çalıştırabilir miyim?](cache-faq.md#how-can-i-run-redis-commands)
* [Azure önbelleği için Redis izleme](cache-how-to-monitor.md)

