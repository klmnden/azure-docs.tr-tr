---
title: Azure önbelleği için Redis sorunlarını giderme | Microsoft Docs
description: Redis için Azure önbellek ile ilgili yaygın sorunları çözmeyi öğrenin.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 928b9b9c-d64f-4252-884f-af7ba8309af6
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 03/15/2019
ms.author: yegu
ms.openlocfilehash: 66361871d365068a90a2eeab70d92adb6b246a83
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59527175"
---
# <a name="how-to-troubleshoot-azure-cache-for-redis"></a>Azure önbelleği için Redis sorunlarını giderme

Bu makale, farklı türdeki Azure önbelleği için Redis örneği bağlanırken karşılaşabileceğiniz sorunları gidermenize yardımcı olur.

- [İstemci tarafı sorunlarını giderme](#client-side-troubleshooting) tanımlamak ve önbellek'e bağlanırken uygulama içindeki sorunları çözmenize yardımcı olur.
- [Sunucu tarafı sorunlarını giderme](#server-side-troubleshooting) tanımlamak ve Azure önbelleği için Redis tarafında gerçekleşen sorunları gidermek yardımcı olur.
- [Veri kaybı sorun giderme](#data-loss-troubleshooting) tanımlamak ve anahtarları bekleniyordu ancak önbelleğinize bulunamadı burada olayları Çözümle yardımcı olur.
- [StackExchange.Redis zaman aşımı özel durumları](#stackexchangeredis-timeout-exceptions) StackExchange.Redis kitaplığı sorunlarını giderme hakkında belirli yönergeler sağlar.

> [!NOTE]
> Bazı sorun giderme adımları bu kılavuzun Redis komutları çalıştırın ve çeşitli performans ölçümleri izlemek için yönergeler içerir. Makaleler, daha fazla bilgi ve yönergeler için bkz: [ek bilgi](#additional-information) bölümü.
>
>

## <a name="client-side-troubleshooting"></a>İstemci tarafı sorunlarını giderme

Bu bölümde, istemci uygulamasındaki bir koşul nedeniyle meydana gelen sorun giderme konuları anlatılmaktadır.

- [İstemci üzerinde bellek baskısı](#memory-pressure-on-the-client)
- [Trafik](#burst-of-traffic)
- [Yüksek istemci CPU kullanımı](#high-client-cpu-usage)
- [İstemci tarafı bant genişliği aşıldı](#client-side-bandwidth-exceeded)
- [Büyük istek/yanıt boyutu](#large-requestresponse-size)

### <a name="memory-pressure-on-the-client"></a>İstemci üzerinde bellek baskısı

Bellek baskısı istemci makine üzerinde her türlü önbelleğe alınan yanıtları işlenmesini geciktirebilir performans sorunlarına neden olur. Bellek baskısı ulaştığında, sistemin verileri diske sayfası. Bu _sayfasında hatalı_ sistemin önemli ölçüde yavaşlamasına neden olur.

Bellek baskısı istemcide algılamak için:

- Kullanılabilir bellek aşmadığından emin olmak için makineye bellek kullanımı izleyin.
- İstemci izleme `Page Faults/Sec` performans sayacı. Normal işlem sırasında bazı sayfa hataları birçok sistem var. İstek zaman aşımı ile ilgili sayfa hataları artış bellek baskısı belirtebilirsiniz.

Yüksek bellek baskısı istemcideki çeşitli yolları azaltılabilir:

- İstemci üzerinde bellek tüketimini azaltmak için bellek kullanım modellerine ilişkin açıklayalım.
- Daha büyük bir boyuta daha fazla belleğe sahip VM istemcinizi yükseltin.

### <a name="burst-of-traffic"></a>Trafik

Trafik artışlarıyla başa düşük ile birleştirilmiş `ThreadPool` ayarları zaten Redis sunucu tarafından gönderilen, ancak henüz istemci tarafında kullanılan veri işlenmesinde gecikmelere neden.

İzleyici nasıl, `ThreadPool` istatistikleri değiştirme saati kullanarak üzerinden [örneği `ThreadPoolLogger` ](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Kullanabileceğiniz `TimeoutException` daha fazla araştırmak StackExchange.Redis iletilerden ister:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0,
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

Önceki özel duruma, ilgi çekici birkaç sorun vardır:

- Dikkat `IOCP` bölümü ve `WORKER` bölüm sahip olduğunuz bir `Busy` değerinden büyük bir değer `Min` değeri. Bu fark anlamına gelir, `ThreadPool` ayarlarının ayarlanması gerekir.
- Ayrıca bkz `in: 64221`. Bu değer, 64,211 bayt istemcinin çekirdek yuva katmanında alındı ancak uygulama tarafından okunur henüz gösterir. Bu fark, uygulamanız (örneğin, StackExchange.Redis) verileri ağ üzerinden sunucu için gönderiyor olabildiğince çabuk okuma değil, genellikle anlamına gelir.

Yapabilecekleriniz [yapılandırmak, `ThreadPool` ayarları](https://gist.github.com/JonCole/e65411214030f0d823cb) patlama senaryoları, iş parçacığı havuzu hızlı bir şekilde altında ölçeklendirilirse emin emin olun.

### <a name="high-client-cpu-usage"></a>Yüksek istemci CPU kullanımı

Sistem çalışma yapmak için istenmektedir yetişemiyor yüksek istemci CPU kullanımını gösterir. Önbellek hızla yanıt gönderilen olsa bile, istemci vakitli yanıta işlemek başarısız olabilir.

Azure portalında ya da performans sayaçları makinede mevcut olan ölçümler kullanarak istemcinin sistem genelinde CPU kullanımı izleyin. İzleme dikkatli olun *işlem* CPU düşük CPU kullanımı tek bir işlem olabilir ancak sistem genelinde CPU yüksek olabilir. Zaman aşımlarını karşılık gelen ani CPU kullanımını izleyin. Yüksek CPU yüksek da neden `in: XXX` değerler `TimeoutException` hata iletileri açıklandığı [trafik](#burst-of-traffic) bölümü.

> [!NOTE]
> StackExchange.Redis 1.1.603 ve daha sonra içerir `local-cpu` içinde ölçüm `TimeoutException` hata iletileri. En son sürümünü kullandığından emin olmak [StackExchange.Redis NuGet paketi](https://www.nuget.org/packages/StackExchange.Redis/). Sürekli olarak en son sürümüne sahip olunması önemlidir daha sağlam zaman aşımına uğrayan sağlamak için kodda düzeltilmekte olan hataları vardır.
>
>

Bir istemcinin yüksek CPU kullanımını azaltmak için:

- CPU'daki ani değişikliklerin neden olduğunu araştırın.
- Daha fazla CPU kapasiteli daha büyük bir VM boyutu için istemcinizi yükseltin.

### <a name="client-side-bandwidth-exceeded"></a>İstemci tarafı bant genişliği aşıldı

İstemci makineler mimarisine bağlı olarak, sınırlamalar olabilir kullanılabilir olan ne kadar üzerindeki ağ bant genişliği. İstemci, ağ kapasitesinin aşırı yükleme tarafından kullanılabilir bant genişliğini aşarsa, ardından istemci tarafında sunucusunu göndermeye olabildiğince çabuk işlenen veri değil. Bu durum, zaman aşımına uğrayan neden olabilir.

Bant genişliği kullanımını nasıl değiştiğini kullanarak zaman içinde izlemenize [örneği `BandwidthLogger` ](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Bu kod, bazı ortamlarda (örneğin, Azure web siteleri) sınırlı izinler ile başarılı bir şekilde çalışmayabilir.

Azaltmak için ağ bant genişliği tüketimini azaltabilir veya daha fazla ağ kapasitesi olan bir VM boyutu istemci artırın.

### <a name="large-requestresponse-size"></a>Büyük istek/yanıt boyutu

Büyük bir istek/yanıt zaman aşımı neden olabilir. Örneğin, istemcide yapılandırılan, zaman aşımı değeri 1 saniyedir varsayalım. Uygulamanızı (aynı fiziksel ağ bağlantısını kullanarak) aynı anda iki anahtar (örneğin, 'A' ve 'B') ister. Çoğu istemci isteği "ardışık düzen oluşturma", burada iki 'A' ve 'B' birbiri ardına bunların yanıtlarını beklemek olmadan gönderdiği istekleri destekler. Sunucu yanıtları aynı sırada geri gönderir. Yanıt 'A' büyük ise, sonraki istekleri için zaman aşımını çoğu yemek.

Aşağıdaki örnekte, 'A' ve 'B' bir istek gönderildiğinde hızla sunucuya. Sunucu, hızlı yanıt 'A' ve 'B' gönderme başlatılır. Hızlı bir şekilde sunucu yanıtı olsa da veri aktarım süreleri nedeniyle, 'B' yanıt 'A' beklemelisiniz yanıt zaman aşımına uğradı.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)

Bu istek/yanıt ölçmek için zor bir bilgisayardır. Büyük istekleri ve yanıtlarını izlemek için istemci kodunuz izleme.

Çözümleri büyük yanıt boyutu için farklılık gösterir ancak içerir:

1. Uygulamanız çok sayıda birkaç büyük değerler yerine küçük değerler için iyileştirin.
    - Tercih edilen verilerinizi ilgili küçük değerler bölmeniz çözümdür.
    - Gönderiye bakın [redis için ideal değer boyut aralığı nedir? 100 KB çok büyük? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) neden hakkında ayrıntılı bilgi için daha küçük değerler önerilir.
1. Daha yüksek bant genişliği özellikleri almak için VM boyutunu artırın
    - Daha fazla bant genişliği, istemci veya Sunucu'da VM büyük yanıtları için veri aktarım süreleri azaltabilir.
    - Geçerli ağ kullanımınızı, geçerli bir VM boyutu sınırları için her iki makinede karşılaştırın. Yalnızca sunucu veya istemci yalnızca üzerinde daha fazla bant genişliği yeterli olmayabilir.
1. Uygulamanızın kullandığı bağlantı nesne sayısını artırın.
    - Farklı bağlantı nesneleri isteğinde bulunmak için hepsini bir kez deneme yaklaşımı kullanın.

## <a name="server-side-troubleshooting"></a>Sunucu tarafı sorunlarını giderme

Bu bölümde, önbellek sunucusunda bir koşul nedeniyle meydana gelen sorun giderme konuları anlatılmaktadır.

- [Sunucudaki bellek baskısı](#memory-pressure-on-the-server)
- [Yüksek CPU kullanımı / sunucu yükü](#high-cpu-usage--server-load)
- [Sunucu tarafı bant genişliği aşıldı](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Sunucudaki bellek baskısı

Bellek baskısı sunucu tarafında isteklerinin işlenmesini geciktirebilir performans sorunlarını her türlü yol açar. Bellek baskısı ulaştığında, sistemin verileri diske sayfası. Bu _sayfasında hatalı_ sistemin önemli ölçüde yavaşlamasına neden olur. Bu bellek baskısı birkaç olası nedeni vardır:

- Önbellek, kapasite üst yakın verilerle doldurulur.
- Redis, yüksek bellek parçalanması gördüğünü. Bu parçalanma, çoğunlukla Redis küçük nesneleri için iyileştirilmiş olduğundan, büyük nesnelerin depolayarak neden olur.

Redis ile iki istatistikleri gösterir [bilgisi](https://redis.io/commands/info) yardımcı olabilecek komutu bu sorunu tanımlama: "used_memory" ve "used_memory_rss". Yapabilecekleriniz [bu ölçümleri görüntüleyin](cache-how-to-monitor.md#view-metrics-with-azure-monitor) portalı kullanarak.

Bellek kullanımı sağlıklı tutmaya yardımcı olmak için yapabileceğiniz birkaç olası değişiklikler şunlardır:

- [Bir bellek ilkesini yapılandırma](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) ve anahtarlarınızı zaman aşımı değeri ayarlayın. Bu ilke parçalanma varsa yeterli olmayabilir.
- [Maxmemory ayrılmış değerini yapılandırmak](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) için bellek parçalanması dengelemek için büyük olmasıdır. Daha fazla bilgi için bkz. ek [bellek ayırmaları için Değerlendirmeler](#considerations-for-memory-reservations) aşağıda.
- Büyük, önbelleğe alınan nesneleri küçük ilgili nesneleri halinde bölün.
- [Uyarı oluşturma](cache-how-to-monitor.md#alerts) gibi ölçümlere ilişkin bellek erken olası etkileri hakkında bildirim almak için kullanılır.
- [Ölçek](cache-how-to-scale.md) için daha fazla bellek kapasitesi ile daha büyük bir önbellek boyutu.

#### <a name="considerations-for-memory-reservations"></a>Bellek ayırmaları için dikkat edilmesi gerekenler

Bellek ayırma değerler güncelleştiriliyor, maxmemory ayrılmış ister, önbellek performansını etkileyebilir. 49 GB veri ile doldurulan bir 53 GB önbellek olduğunu varsayalım. Ayırma değeri için 8 GB değiştirilirse, sistemin en fazla kullanılabilir bellek 45 GB bırakır. Varsa _used_memory_ veya _used_memory_rss_ değerler 45 GB'den daha yüksek, sistem veri erene kadar Tahliye _used_memory_ ve _used_memory_rss_ 45 GB'ın altında olan. Çıkarma, Sunucu yükünü ve bellek Parçalanmayı arttırabilir.

### <a name="high-cpu-usage--server-load"></a>Yüksek CPU kullanımı / sunucu yükü

Yüksek sunucu iş yükü veya CPU kullanımı, sunucu zamanında istekleri işleyemiyor anlamına gelir. Sunucu yanıt yavaş ve istek hızları ile devam edilemiyor olabilir.

[İzleme ölçümlerini](cache-how-to-monitor.md#view-metrics-with-azure-monitor) gibi CPU veya sunucu yükü. Zaman aşımlarını karşılık gelen ani CPU kullanımını izleyin.

Yüksek Sunucu yükünü azaltmak için yapabileceğiniz bazı değişiklikler vardır:

- Çalıştırma gibi CPU'daki ani değişikliklerin neden olan araştırmak [yüksek maliyetli komutları](#expensive-commands) veya yüksek bellek baskısı nedeniyle hatalı sayfası.
- [Uyarı oluşturma](cache-how-to-monitor.md#alerts) erken olası etkileri hakkında bildirim almak için CPU veya sunucusu yükleme gibi ölçümlere ilişkin.
- [Ölçek](cache-how-to-scale.md) için daha fazla CPU kapasiteli daha büyük bir önbellek boyutu.

#### <a name="expensive-commands"></a>Yüksek maliyetli komutları

Tüm Redis komutları eşit oluşturulan - bazıları diğerlerinden çalıştırmak daha pahalıdır. [Redis komutları belgeleri](https://redis.io/commands) her komut zaman karmaşıklığını gösterir. Komutlarda performans üzerindeki etkisini anlamak için önbelleğinizi üzerinde çalıştırdığınız komutlar gözden geçirmeniz önerilir. Örneğin, [ANAHTARLARI](https://redis.io/commands/keys) komutu, bir o(n) olduğunu bilmeden sık sık kullanılır. ANAHTARLARI kullanarak kaçınabilirsiniz [tarama](https://redis.io/commands/scan) CPU azaltmak için ani.

Kullanarak [SLOWLOG'u](https://redis.io/commands/slowlog) komutunu, ölçmek sunucusuna karşı yürütülen yüksek maliyetli komutları.

### <a name="server-side-bandwidth-exceeded"></a>Sunucu tarafı bant genişliği aşıldı

Farklı ağ bant genişliği kapasitesi farklı bir önbellek boyutuna sahip. Sunucunun kullanılabilir bant genişliğini aşarsa, verileri hızlı olarak istemciye gönderilmez. Sunucu verilerini istemciye yeterince hızlı gönderim yapılamaz, çünkü istemci isteklerini zaman aşımına uğrayabilir.

"Önbellek Okuma" ve "Yazma önbelleği" ölçümleri, sunucu tarafı genişliği kullanıldığını görmek için kullanılabilir. Yapabilecekleriniz [bu ölçümleri görüntüleyin](cache-how-to-monitor.md#view-metrics-with-azure-monitor) portalında.

Ağ bant genişliği kullanımı en yüksek kapasite yakın olduğu durumları azaltmak için:

- Ağ talebi azaltmak için istemci çağrısı davranışını değiştirin.
- [Uyarı oluşturma](cache-how-to-monitor.md#alerts) önbellek okuma veya erken olası etkileri hakkında bildirim almak için önbellek yazması gibi ölçümlere ilişkin.
- [Ölçek](cache-how-to-scale.md) için daha fazla ağ bant genişliği kapasitesi ile daha büyük bir önbellek boyutu.

## <a name="data-loss-troubleshooting"></a>Veri kaybı sorunlarını giderme

Miyim belirli Azure Önbelleğimin Redis örneği için oluşturulacak veri bekleniyordu, ancak var. olmak görünen kaydetmedi.

Bkz: [Redis verilerimi ne?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) olası nedenleri ve çözümlemeleri için.

## <a name="stackexchangeredis-timeout-exceptions"></a>StackExchange.Redis zaman aşımı özel durumları

StackExchange.Redis yapılandırma adlandırılmış ayarı kullanan `synctimeout` 1000 MS varsayılan bir değerle zaman uyumlu işlemler için. Zaman uyumlu bir çağrı bu süre içinde tamamlanmazsa, StackExchange.Redis istemcisi aşağıdaki örneğe benzer bir zaman aşımı hatası oluşturur:

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)

Bu hata iletisi sorunun nedenini ve olası çözümü noktası yardımcı olabilecek ölçümleri içerir. Aşağıdaki tablo, hata iletisi ölçümleri ilgili ayrıntıları içerir.

| Hata iletisi ölçümü | Ayrıntılar |
| --- | --- |
| yönerge |Son zaman dilimi içinde: 0 komutları yayımlandı |
| Yöneticisi |Yuva manager yapıyor `socket.select`, işletim sistemi olan bir şey yapmak için bir yuva belirtmek için soran anlamına gelir. Bunu yapmak için herhangi bir şey yoktur düşünün değil çünkü okuyucu etkin olarak ağdan okunurken değil |
| kuyruk |73 toplam devam eden işlemler vardır. |
| QU |devam eden işlemlerin 6 kuyruktaki gönderilmeyen ve giden ağ henüz yazmadınız |
| qs |devam eden işlemlerin 67 sunucuya gönderildi ancak yanıt henüz kullanılamıyor. Yanıt olabilir `Not yet sent by the server` veya `sent by the server but not yet processed by the client.` |
| QC |devam eden işlemlerin 0 yanıtları gördünüz ancak bunlar üzerinde tamamlama döngüsü bekliyorsunuz henüz tam işaretlenmiş henüz |
| wr |(6 gönderilmemiş istekleri göz ardı olmayan anlamına gelir) etkin yazan bayt/activewriters yoktur |
| in |Hiçbir etkin okuyucular vardır ve sıfır bayt NIC bayt/activereaders okumak kullanılabilir |

### <a name="steps-to-investigate"></a>Araştırmak için adımları

1. En iyi uygulama, StackExchange.Redis istemcisi kullanılırken bağlanmak için şu desene kullandığınızdan emin olun.

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
    ```

    Daha fazla bilgi için [StackExchange.Redis kullanarak önbelleğe bağlanma](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

1. Sunucunuz ve istemci uygulama aynı Azure bölgesinde olduğundan emin olun. Örneğin, zaman aşımları, önbellek, Doğu ABD bölgesinde, ancak istemci Batı ABD'de ve içinde istek işlemini tamamlamazsa olduğunda ekleyene `synctimeout` aralığı veya alınırken zaman aşımı yerel geliştirme makinenizde hata ayıklama işlemi yaparken. 

    Yüksek oranda önbelleğe sahip önerilir ve aynı Azure bölgesinde istemci. Bölgeler arası aramalar içeren bir senaryo varsa ayarlamalısınız `synctimeout` aralığı ekleyerek varsayılan 1000 MS'den aralığından daha yüksek bir değere bir `synctimeout` bağlantı dizesi özelliği. Aşağıdaki örnek, Redis ile Azure önbelleği tarafından sağlanan StackExchange.Redis için bir kod parçacığı, bir bağlantı dizesi gösterir. bir `synctimeout` 2000 MS.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...
1. En son sürümünü kullandığından emin olmak [StackExchange.Redis NuGet paketi](https://www.nuget.org/packages/StackExchange.Redis/). Sürekli olarak en son sürümüne sahip olunması önemlidir daha sağlam zaman aşımına uğrayan sağlamak için kodda düzeltilmekte olan hataları vardır.
1. İsteklerinizi bant genişliği sınırlamaları sunucu veya istemci tarafından bağlıysa, bunların uzun sürdüğünü tamamlamak ve zaman aşımlarına neden olabilir. Zaman aşımı nedeniyle sunucu üzerindeki ağ bant genişliği olup olmadığını görmek için bkz: [sunucu tarafı bant genişliği aşıldı](#server-side-bandwidth-exceeded). Zaman aşımı, istemci ağ bant genişliği nedeniyle olup olmadığını görmek için bkz: [istemci-tarafı bant genişliği aşıldı](#client-side-bandwidth-exceeded).
1. CPU alma, sunucudaki veya istemcideki bağlıdır?

   - CPU, istemcide bağlı olmadığını kontrol edin. Yüksek CPU içinde değil isteğin işlenmesi için neden `synctimeout` aralık ve istek zaman aşımına neden. Taşıma için daha büyük bir istemci boyutu ya da yük dağıtma, bu sorunu denetlemek için yardımcı olabilir.
   - CPU alıyorsanız onay izleyerek sunucuda bağlı `CPU` [performans ölçümü önbelleğe](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Redis CPU'ya bağlıdır, ancak gelen istekler, bu istekleri zaman aşımına neden olabilir. Bu durumu çözmek için yükü premium önbellekteki birden fazla parçaya dağıtmak veya bir büyük veya fiyatlandırma katmanına yükseltin. Daha fazla bilgi için [sunucu tarafı bant genişliği aşıldı](#server-side-bandwidth-exceeded).
1. Sunucu üzerinde işlem için uzun süren komutları var mı? Redis sunucu üzerinde işlem için uzun sürüp uzun süre çalışan komutlar, zaman aşımı neden olabilir. Uzun süre çalışan komutlar hakkında daha fazla bilgi için bkz. [yüksek maliyetli komutları](#expensive-commands). Redis-cli istemci kullanarak Redis örneği için Azure önbelleği bağlanabilirsiniz veya [Redis Konsolu](cache-configure.md#redis-console). Ardından çalıştırın [SLOWLOG'u](https://redis.io/commands/slowlog) beklenenden daha yavaş istekleri olup olmadığını görmek için komutu. Redis sunucusu ve StackExchange.Redis daha az büyük istekleri yerine çok sayıda küçük isteği için iyileştirilmiştir. Verilerinizi daha küçük öbeklere ayırma şeyleri burada artırabilir.

    Redis-cli ve stunnel kullanarak, önbelleğin SSL uç noktasına bağlanma hakkında daha fazla bilgi için blog gönderisine bakın [Redis Önizleme sürümü için ASP.NET oturum durumu sağlayıcısı ile tanışın](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx).
1. Yüksek Redis sunucu yükü, zaman aşımı neden olabilir. Sunucu iş yükü izleyerek izleyebilirsiniz `Redis Server Load` [performans ölçümü önbelleğe](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Bir sunucu yükü 100 (en yüksek değer), redis sunucusunun istekleri işleme boş kalma süresi ile meşgul olduğunu gösterir. Belirli isteklerini tüm sunucu özelliğine sürüyor durumunda görmek için önceki paragrafta açıklandığı gibi Slowlog'u komutu çalıştırın. Daha fazla bilgi için bkz: yüksek CPU kullanımı / sunucu iş yükü.
1. İstemci tarafında ağ blip neden olan herhangi bir olay oluştu? Genel olaylar içerir: istemci örneklerinin sayısını artırma veya azaltma, istemci ya da otomatik ölçeklendirme etkin yeni bir sürümü dağıtma. Bizim testimizde otomatik ölçeklendirme veya ölçeklendirme yukarı/aşağı giden ağ bağlantısını için birkaç saniye kaybolmasına neden olabileceğini bulduk. StackExchange.Redis kod böyle olaylara esnektir ve yeniden bağlanır. Yeniden bağlanma sırasında herhangi bir sıra isteği zaman aşımına uğrayabilir.
1. Birçok küçük istek zaman aşımına uğradı önbelleğe önceki büyük bir istek vardı? Parametre `qs` hata iletisi, kaç istek istemciden sunucuya gönderilen, ancak bir yanıt işlenen henüz bildirir. StackExchange.Redis tek bir TCP bağlantısını kullanır ve bir yanıt aynı anda yalnızca okuyabilir çünkü bu değer büyüyen tutun. İlk işlem zaman aşımına uğradı olsa da, bunu gönderileceği veya sunucudan daha fazla veri bitmez. Büyük istek tamamlandı ve zaman aşımları oluşabilir kadar diğer istekleri engellenir. Önbelleğinizi yükünüz için yeterince büyük olduğundan emin olmanın ve büyük değerler daha küçük öbeklere ayırma zaman aşımları olasılığını en aza indirmek bir çözümdür. Başka bir olası çözümü havuzu kullanmaktır `ConnectionMultiplexer` istemcinizde nesneleri ve az yüklenen seçin `ConnectionMultiplexer` yeni bir isteği gönderirken. Birden çok bağlantı nesneleri arasında yükleme tek bir zaman aşımı diğer istekleri de zaman aşımına neden olmasını engeller.
1. Kullanıyorsanız `RedisSessionStateProvider`, ayarladığınız yeniden deneme zaman aşımı doğru emin olun. `retryTimeoutInMilliseconds` daha yüksek olmalıdır `operationTimeoutInMilliseconds`, aksi takdirde, yeniden deneme yok oluşur. Aşağıdaki örnekte `retryTimeoutInMilliseconds` 3000 olarak ayarlanır. Daha fazla bilgi için [Redis için Azure Cache için ASP.NET oturum durumu sağlayıcısı](cache-aspnet-session-state-provider.md) ve [çıktı önbelleği sağlayıcısı ile oturum durumu sağlayıcısı ve yapılandırma parametrelerini kullanma](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).

    ```xml
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
    ```

1. Azure önbelleği için Redis sunucusu tarafından bellek kullanımını kontrol [izleme](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` ve `Used Memory`. Çıkarma İlkesi yerinde ise, Redis başlatır çıkarma, anahtarları ne zaman `Used_Memory` önbellek boyutu. İdeal olarak, `Used Memory RSS` yalnızca biraz daha yüksek olmalıdır `Used memory`. Büyük bir fark (iç veya dış) bellek parçalanması olduğu anlamına gelir. Zaman `Used Memory RSS` olduğu küçüktür `Used Memory`, önbellek parçası, işletim sistemi tarafından takas anlamına gelir. Takas bu meydana gelirse, bazı önemli bir gecikme bekleyebilirsiniz. Redis bellek sayfalarına, yüksek ayırmalarını nasıl eşleştirildiğini denetim olmadığından `Used Memory RSS` genellikle bellek kullanımı bir ani değişiklik sonucudur. Redis sunucu belleği serbest bırakır, bellek ayırıcısı alır ancak bellek geri sisteme vermeyebilir veya olabilir. Arasında bir tutarsızlık olması `Used Memory` işletim sistemi tarafından bildirilen değeri ve bellek tüketimi. Bellek kullanılan ve Redis tarafından ancak sistemin geri değil verilen boşta. Bellek sorunlarının azaltılmasına yardımcı olmak için aşağıdakileri yapabilirsiniz:

   - Önbellek sistemde bellek sınırlamaları karşı çalıştırmadığınız için daha büyük bir boyutu yükseltin.
   - Zaman aşımı değeri tuşlar eski değerleri proaktif bir şekilde çıkarılan şekilde ayarlayın.
   - İzleyici `used_memory_rss` ölçüm önbellek. Bu değer, önbellek boyutu yaklaştığında, olası performans sorunlarını görmeye başlayacaksınız. Premium önbellek kullanarak veya daha büyük bir önbellek boyutu için yükseltme yapıyorsanız, verileri birden fazla parçaya dağıtmak.

   Daha fazla bilgi için [sunucudaki bellek baskısı](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Ek bilgiler

- [Hangi Azure önbelleği için Redis teklifini ve boyutunu kullanmalıyım?](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use)
- [Nasıl Kıyaslama ve önbelleğimin performansını test etme?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
- [Redis komutları nasıl çalıştırabilir miyim?](cache-faq.md#how-can-i-run-redis-commands)
- [Azure önbelleği için Redis izleme](cache-how-to-monitor.md)
