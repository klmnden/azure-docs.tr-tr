---
title: Azure önbelleği için Redis için en iyi uygulamalar
description: Bu en iyi uygulamaları takip ederek etkili bir şekilde, Azure önbelleği için Redis kullanmayı öğrenin.
services: cache
documentationcenter: na
author: joncole
manager: jhubbard
editor: tysonn
ms.assetid: 3e4905e3-89e3-47f7-8cfb-12caf1c6e50e
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache
ms.workload: tbd
ms.date: 06/21/2019
ms.author: joncole
ms.openlocfilehash: bdc75033e0aa2e401a511789728feef3248d46ad
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67452468"
---
# <a name="best-practices-for-azure-cache-for-redis"></a>Azure önbelleği için Redis için en iyi uygulamalar 
Bu en iyi uygulamaları takip ederek, performans ve düşük maliyetli Azure önbelleğinizin kullanımını Redis örneği için en üst düzeye yardımcı olabilir.

## <a name="configuration-and-concepts"></a>Yapılandırma ve kavramları
 * **Standart veya Premium katmanı, üretim sistemleri için kullanın.**  Temel katmanı, veri çoğaltma ve SLA ile bir tek düğümlü sistemidir. Ayrıca, en az bir C1 önbelleği kullanır.  Paylaşılan bir CPU çekirdeği, çok az bellek vardır ve "gürültülü komşu" sorunlarını meyillidir C0 önbellekler basit geliştirme/test senaryoları için yöneliktir.

 * **Redis bir bellek içi veri deposuna olduğunu unutmayın.**  [Bu makalede](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) burada veri kaybı oluşabilir bazı senaryolar açıklanmaktadır.

 * **Sisteminizde bağlantı blips işleyebilir, geliştirme** [düzeltme eki uygulama ve yük devretme nedeniyle](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

 * **Yapılandırma, [maxmemory ayrılmış ayarı](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) sistem yanıt hızını artırmanın** bellek baskısı koşullar altında.  Bu ayar, yazma yoğunluklu iş yükleri veya (100 KB veya daha fazla) daha büyük değerler Redis depoladığınız durumunda özellikle önemlidir.  Ben % önbelleğinizin boyutu 10 ile başlamanızı öneririz sonra ağır yazma yüklerini varsa artırın. Bkz: [noktalara](cache-how-to-troubleshoot.md#considerations-for-memory-reservations) bir değer seçerken.

 * **Works en küçük değerler ile redis**, bu nedenle büyük verilerin birden çok anahtar chopping göz önünde bulundurun.  İçinde [bu Redis tartışma](https://stackoverflow.com/questions/55517224/what-is-the-ideal-value-size-range-for-redis-is-100kb-too-large/), olduğunu dikkatle dikkate almanız gereken bazı noktalar listelenir.  Okuma [bu makalede](cache-how-to-troubleshoot.md#large-requestresponse-size) büyük değerler neden bir örneğin sorun için.

 * **Aynı bölgede, önbellek örneğinizin ve uygulamanızı bulun.**  Farklı bir bölgede bir önbellek bağlanma önemli ölçüde gecikme süresini ve güvenilirlik azaltın.  Azure dışından gelen bağlanabilirsiniz buna karşın bu tavsiye *özellikle Redis önbelleği olarak kullanılırken*.  Redis yalnızca bir anahtar/değer deposu olarak kullanıyorsanız, birincil sorun gecikme olmayabilir. 

 * **Bağlantılarını yeniden** -yeni bağlantıları oluşturma pahalıdır ve gecikme süresi artar, dolayısıyla bağlantılarını mümkün olduğunca yeniden. Yeni bağlantılar oluşturmak tercih ederseniz (bile yönetilen bellek gibi dillerde .NET veya Java) onları bırakmadan önce eski bağlantıları kapatmak emin olun.

 * **İstemci Kitaplığı, yapılandırma bir *bağlantı zaman aşımı* en az 15 saniyelik**, daha yüksek CPU koşullarda bile bağlanmak için sistem zamanı.  Küçük bağlantı zaman aşımı değeri, bağlantı o zaman çerçevesi içinde kurulduktan garanti etmez.  Bir şey gelecek yanlış (yüksek istemci CPU, yüksek sunucu CPU'sunu vb.) ve ardından bir kısa bağlantı zaman aşımı değeri bağlantı girişimi başarısız olmasına neden olur. Bu davranış, genellikle hatalı durumda daha da kötüsü yapar.  Yardımcı yerine daha kısa zaman aşımları açabilir, sistemin yeniden bağlanmaya çalışan işleminin yeniden başlatılmasına gerek tarafından sorun aggravate bir *bağlanma başarısız -> Yeniden Dene ' ->* döngü. Genellikle, bağlantı zaman aşımı 15 saniye veya daha yüksek tıklatmanızı öneririz. 15 veya 20 saniye sonra hızlı bir şekilde başarısız çok daha başarılı, bağlantı denemesi izin vermek iyidir yalnızca yeniden denemek için. Böyle bir yeniden deneme döngüsüne, yalnızca sistem izin verirseniz artık başlangıçta olması daha uzun son, kesinti neden olabilir.  
     > [!NOTE]
     > Bu kılavuz özeldir *bağlantı denemesi* ve bekleyin edebileceğiniz süreyi ilişkili olmayan bir *işlemi* GET veya SET tamamlanması gibi.
 

 * **Yüksek maliyetli komutları önlemek** -bazı redis işlemleri gibi [ANAHTARLARI komutunu](https://redis.io/commands/keys), olan *çok* pahalı ve kaçınılmalıdır.  Daha fazla bilgi için [yüksek maliyetli komutları geçici olarak bazı önemli noktalar](cache-how-to-troubleshoot.md#expensive-commands)


 
## <a name="memory-management"></a>Bellek yönetimi
Bellek kullanımı düşünmek isteyebilirsiniz Redis sunucu örneğinizin içinde ilgili birkaç şey vardır.  Birkaçı şunlardır:

 * **Seçin bir [çıkarma İlkesi](https://redis.io/topics/lru-cache) uygulamanız için çalışır.**  Azure Redis için varsayılan ilke *geçici lru*, yani bir TTL sahip anahtarları kümesi değer çıkarma için uygun olacaktır.  Anahtar yok TTL değeri varsa, sistemin herhangi bir anahtar Tahliye olmaz.  Sistem, bellek baskısı altında çıkarılacak herhangi bir tuşa izin vermek istediğiniz sonra düşünmek isteyebilirsiniz *allkeys lru* ilkesi.

 * **Süre sonu değeri anahtarlarınızı üzerinde ayarlayın.**  Bu anahtarları oluncaya kadar bellek baskısı proaktif olarak beklemek yerine kaldırır.  Çıkarma bellek baskısı nedeniyle yaslanıp, sunucuda ek yük neden olabilir.  Daha fazla bilgi için belgelerine bakın [sona erme](https://redis.io/commands/expire) ve [ExpireAt](https://redis.io/commands/expireat) komutları.
 
## <a name="client-library-specific-guidance"></a>İstemci Kitaplığı özel Kılavuzu
 * [StackExchange.Redis (.NET)](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-stackexchange-redis-md)
 * [Java - istemci kullanmalıyım?](https://gist.github.com/warrenzhu25/1beb02a09b6afd41dff2c27c53918ce7#file-azure-redis-java-best-practices-md)
 * [Lettuce (Java)](https://gist.github.com/warrenzhu25/181ccac7fa70411f7eb72aff23aa8a6a#file-azure-redis-lettuce-best-practices-md)
 * [Jedis (Java)](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-java-jedis-md)
 * [Node.js](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-node-js-md)
 * [PHP](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-php-md)
 * [Asp.Net oturum durumu sağlayıcısı](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#file-redis-bestpractices-session-state-provider-md)


## <a name="when-is-it-safe-to-retry"></a>Ne zaman yeniden denemek güvenlidir?
Ne yazık ki, bir kolayca yanıt yoktur.  Her uygulama hangi işlemleri yeniden denenebilir ve hangi olamaz karar vermeniz gerekir.  Her işlemi farklı gereksinimleri ve anahtar arası bağımlılıkları vardır.  Göz önünde tutabileceğiniz bazı işlemler aşağıda verilmiştir:

 * Redis çalışmaya sorulan komutu başarıyla çalıştı olsa bile, istemci tarafı hataları alabilirsiniz.  Örneğin:
     - Zaman aşımları, bir istemci-tarafı kavramdır.  İşlem sunucusu ulaştıysanız, bile istemciye yedekleme bekletme verir sunucu komut çalışır.  
     - Yuva bağlantısı üzerinde bir hata oluştuğunda, işlem sunucusunda gerçekten çalıştırıp çalıştırmadığını öğrenmek mümkün değildir.  Örneğin, sunucu tarafından ancak yanıt, istemci tarafından alınmadan önce istek işlendikten sonra bağlantı hatası oluşabilir.
 *  Aynı işlemi iki kez yanlışlıkla çalıştırabilir, uygulamamı nasıl tepki?  Örneğin, ne miyim tamsayı iki kez yerine yalnızca bir kez Artır?  Uygulamamın aynı anahtar için birden çok yerde yazıyor?  Ne my yeniden deneme mantığı uygulamamı diğer bazı parçası ayarlanmış bir değer üzerine yazar?

Kodunuzu hata koşulları altında nasıl çalıştığını test etmek istiyorsanız, kullanmayı [yeniden özellik](cache-administration.md#reboot). Bu, bağlantı blips uygulamanızı nasıl etkilediğini görmek sağlar.

## <a name="performance-testing"></a>Performansı test etme
 * **Başlangıç kullanarak `redis-benchmark.exe`**  kendi perf yazmadan önce bir olası işleme/gecikme almak için test eder.  Redis Kıyaslama belgeleri olabilir [burada bulunan](http://redis.io/topics/benchmarks).  Bu redis Kıyaslama gerekir böylece SSL desteklemez unutmayın [Portal üzerinden SSL olmayan bağlantı noktasını etkinleştirmek](cache-configure.md#access-ports) testi çalıştırmadan önce.  [Redis benchmark.exe'nın uyumlu bir windows sürümünü buradan ulaşabilirsiniz](https://github.com/MSOpenTech/redis/releases)
 * Test etmek için kullanılan VM istemci olmalıdır **aynı bölgede** Redis cache örneğinizin olarak.
 * **Dv2 VM serisi kullanmanızı öneririz** istemciniz için daha iyi bir donanım profiliniz ve en iyi sonuçlar verir.
 * İstemci kullandığınız VM'nin olduğundan emin olun. **en az işlem ve bant genişliği* sınanan önbelleği olarak. 
 * **VRSS etkinleştirme** Windows ise istemci makine üzerinde.  [Ayrıntılar için buraya bakın](https://technet.microsoft.com/library/dn383582(v=ws.11).aspx).  Örnek powershell betiği:
     >PowerShell - ExecutionPolicy Enable-NetAdapterRSS sınırsız-adı (Get-NetAdapter). Adı 
     
 * **Premium katmanı Redis örneği kullanmayı**.  Bu önbellek boyutları, hem CPU hem de ağ daha iyi donanım üzerinde çalıştırıyorsanız olduğundan daha iyi ağ gecikme süresi ve aktarım hızı olmayacak.
 
     > [!NOTE]
     > Bizim gözlemlenen performans sonuçları [Burada yayımlanan](cache-faq.md#azure-cache-for-redis-performance) referans.   Ayrıca, SSL/TLS aktarım şifreleme kullanıyorsanız, farklı gecikme süreleri ve/veya aktarım hızı elde edebilirsiniz için bazı ek yükler ekler unutmayın.
 
### <a name="redis-benchmark-examples"></a>Redis Kıyaslama örnekleri
**Kurulum ön test**: Bu önbellek örneği aşağıda listelenen komutları test aktarım hızı ve gecikme süresi için gerekli veri hazırlık yapacaksınız.
> redis-benchmark.exe -h yourcache.redis.cache.windows.net -a yourAccesskey -t SET -n 10 -d 1024 

**Gecikme süresi test etmek için**: Bu, GET istekleri kullanarak bir 1 k yükü test eder.
> redis-benchmark.exe -h yourcache.redis.cache.windows.net -a yourAccesskey -t GET -d 1024 -P 50 -c 4

**Aktarım hızı test etmek için:** Bu, 1 k yüke sahip ardışık GET istekleri kullanır.
> redis-benchmark.exe -h yourcache.redis.cache.windows.net -a yourAccesskey -t  GET -n 1000000 -d 1024 -P 50  -c 50
