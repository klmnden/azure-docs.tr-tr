---
title: Azure önbelleği için Redis SSS | Microsoft Docs
description: Sık sorulan sorular, desenleri ve en iyi yanıtları Azure önbelleği için Redis için öğrenin.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: c2c52b7d-b2d1-433a-b635-c20180e5cab2
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: yegu
ms.openlocfilehash: 65e8553969aa92848b1c4496724a7b7754b5d659
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895605"
---
# <a name="azure-cache-for-redis-faq"></a>Redis için Azure Önbelleği SSS
Sık sorulan sorular, desenleri ve en iyi yanıtları Azure önbelleği için Redis için öğrenin.

## <a name="what-if-my-question-isnt-answered-here"></a>Peki sorumun cevabı burada bulamadığınız?
Sorunuzu burada listelenmiyorsa, bize bildirin ve yanıt bulmanıza yardımcı olacağız.

* Bu SSS, sonunda açıklamalarda bir soru gönderin ve Azure cache'i takım ve diğer topluluk üyelerinin bu makaleyle ilgili etkileşim kurun.
* Geniş bir kitleye ulaşmak için soru üzerindeki gönderebildiği [Azure Cache MSDN Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=azurecache) ve Azure cache'i takım ve diğer topluluk üyelerinin ile etkileşim kurun.
* Özellik isteğinde bulunmak istiyorsanız, için fikir ve istek gönderebilirsiniz [Azure önbelleği için Redis User Voice](https://feedback.azure.com/forums/169382-cache).
* Adresinden bize e-posta da gönderebilirsiniz [Azure önbellek dış geri bildirim](mailto:azurecache@microsoft.com).

## <a name="azure-cache-for-redis-basics"></a>Azure önbelleği için Redis temelleri
Bu bölümdeki SSS'leri bazı temel işlemleri Azure önbelleği için Redis kapsar.

* [Azure önbelleği için Redis nedir?](#what-is-azure-cache-for-redis)
* [Nasıl Azure önbelleği için Redis kullanmaya başlayabilir miyim?](#how-can-i-get-started-with-azure-cache-for-redis)

Aşağıdaki sık sorulan sorular, Redis için Azure önbelleği hakkında soru ve temel kavramları kapsar ve diğer SSS bölümlerden birine yanıtlanır.

* [Hangi Azure önbelleği için Redis teklifini ve boyutunu kullanmalıyım?](#what-azure-cache-for-redis-offering-and-size-should-i-use)
* [Hangi Azure önbelleği için Redis istemcileri kullanabilirim?](#what-azure-cache-for-redis-clients-can-i-use)
* [Yerel bir öykünücü Azure önbelleği için Redis mı?](#is-there-a-local-emulator-for-azure-cache-for-redis)
* [Sistem durumunu ve performansını önbelleğimin nasıl izleyebilirim?](#how-do-i-monitor-the-health-and-performance-of-my-cache)

## <a name="planning-faqs"></a>Planlama ile ilgili SSS
* [Hangi Azure önbelleği için Redis teklifini ve boyutunu kullanmalıyım?](#what-azure-cache-for-redis-offering-and-size-should-i-use)
* [Azure önbelleği için Redis performans](#azure-cache-for-redis-performance)
* [Önbelleğimin hangi bölgede bulabilirim?](#in-what-region-should-i-locate-my-cache)
* [Azure önbelleği için Redis için nasıl faturalandırılırım?](#how-am-i-billed-for-azure-cache-for-redis)
* [Azure önbelleği için Redis Azure kamu Bulutu, Azure Çin Bulutu veya Microsoft Azure Almanya ile kullanabilir?](#can-i-use-azure-cache-for-redis-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany)

## <a name="development-faqs"></a>Geliştirme ile ilgili SSS
* [StackExchange.Redis yapılandırma seçeneklerini ne?](#what-do-the-stackexchangeredis-configuration-options-do)
* [Hangi Azure önbelleği için Redis istemcileri kullanabilirim?](#what-azure-cache-for-redis-clients-can-i-use)
* [Yerel bir öykünücü Azure önbelleği için Redis mı?](#is-there-a-local-emulator-for-azure-cache-for-redis)
* [Redis komutları nasıl çalıştırabilir miyim?](#how-can-i-run-redis-commands)
* [Neden Azure önbelleği için Redis bazı diğer Azure Hizmetleri gibi bir MSDN sınıf kitaplığı başvurusu yok?](#why-doesnt-azure-cache-for-redis-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services)
* [Azure önbelleği için Redis PHP oturum önbelleği olarak kullanabilir miyim?](#can-i-use-azure-cache-for-redis-as-a-php-session-cache)
* [Redis veritabanı nedir?](#what-are-redis-databases)

## <a name="security-faqs"></a>Güvenlik hakkında SSS
* [Redis'e bağlanmak için SSL olmayan bağlantı noktası olduğunda etkinleştirmeli miyim?](#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis)

## <a name="production-faqs"></a>Üretim SSS
* [Bazı üretim en iyi uygulamalar nelerdir?](#what-are-some-production-best-practices)
* [Bazı önemli noktalar ortak Redis komutları kullanılırken nelerdir?](#what-are-some-of-the-considerations-when-using-common-redis-commands)
* [Nasıl Kıyaslama ve önbelleğimin performansını test etme?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [İş parçacığı havuzu büyüme hakkında önemli ayrıntıları](#important-details-about-threadpool-growth)
* [StackExchange.Redis kullanırken istemcide daha fazla iş üretmek sunucu GC etkinleştir](#enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis)
* [Bağlantıları etrafında performansla ilgili önemli noktalar](#performance-considerations-around-connections)

## <a name="monitoring-and-troubleshooting-faqs"></a>İzleme ve sorun giderme SSS
Bu bölümdeki SSS'leri ortak izleme ve sorun giderme soruları kapsar. İzleme ve sorun giderme, Azure önbelleği için Redis örneği hakkında daha fazla bilgi için bkz. [Azure önbelleği için Redis izleme](cache-how-to-monitor.md) ve [Azure önbelleği için Redis sorunlarını giderme](cache-how-to-troubleshoot.md).

* [Sistem durumunu ve performansını önbelleğimin nasıl izleyebilirim?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
* [Zaman aşımı neden görüyorum?](#why-am-i-seeing-timeouts)
* [Neden istemcim önbellekten kesildi?](#why-was-my-client-disconnected-from-the-cache)

## <a name="prior-cache-offering-faqs"></a>Önceki önbellek teklifi hakkında SSS
* [Hangi Azure Önbellek teklifi bana uygundur?](#which-azure-cache-offering-is-right-for-me)

### <a name="what-is-azure-cache-for-redis"></a>Azure önbelleği için Redis nedir?
Azure önbelleği için Redis popüler açık kaynak yazılım tabanlı [Redis](https://redis.io/). Güvenli ve adanmış bir Azure önbelleği için Redis, Microsoft tarafından yönetilir ve azure'daki herhangi bir uygulamadan erişilebilir için erişmenizi sağlar. Daha ayrıntılı bir genel bakış için bkz. [Azure önbelleği için Redis](https://azure.microsoft.com/services/cache/) Azure.com'daki ürün sayfası.

### <a name="how-can-i-get-started-with-azure-cache-for-redis"></a>Nasıl Azure önbelleği için Redis kullanmaya başlayabilir miyim?
İle Azure önbelleği için Redis oluşturabileceğinize dair birkaç yolu vardır.

* Çıkış için kullanılabilen öğreticilerimizden birini denetleyebilirsiniz [.NET](cache-dotnet-how-to-use-azure-redis-cache.md), [ASP.NET](cache-web-app-howto.md), [Java](cache-java-get-started.md), [Node.js](cache-nodejs-get-started.md), ve [Python](cache-python-get-started.md).
* İzleyebilir [derleme yüksek performanslı uygulamaları kullanarak Microsoft Azure önbelleği için Redis nasıl](https://azure.microsoft.com/documentation/videos/how-to-build-high-performance-apps-using-microsoft-azure-cache/).
* Redis kullanma hakkında bilgi için projenizin bir geliştirme dilini eşleşen istemciler için istemci belgeleri kullanıma alabilirsiniz. İle Azure Cache, Redis için kullanılabilecek birçok Redis istemcilerinin vardır. Redis istemcilerinin listesi için bkz. [ https://redis.io/clients ](https://redis.io/clients).

Bir Azure hesabınız yoksa, şunları yapabilirsiniz:

* [Ücretsiz bir Azure hesabı açın](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). Ücretli Azure hizmetlerini denemek için kullanabileceğiniz krediler alırsınız. Krediler bitmiş olsa bile hesabı sürdürebilir ve ücretsiz Azure hizmet ve özelliklerinden faydalanabilirsiniz.
* [Visual Studio abone avantajları etkinleştirin](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). MSDN aboneliğiniz, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay size kredi verir.

<a name="cache-size"></a>

### <a name="what-azure-cache-for-redis-offering-and-size-should-i-use"></a>Hangi Azure önbelleği için Redis teklifini ve boyutunu kullanmalıyım?
Her Azure önbelleği için Redis teklifi farklı düzeylerde sağlar **boyutu**, **bant genişliği**, **yüksek kullanılabilirlik**, ve **SLA** seçenekleri.

Bir önbellek teklifi seçmek için dikkat edilecek noktalar aşağıda verilmiştir.

* **Bellek**: Temel ve standart katmanları, 250 MB – 53 GB sunar. Premium katmanı, 530 GB'a kadar sunar. Daha fazla bilgi için [Azure önbelleği için Redis fiyatlandırma](https://azure.microsoft.com/pricing/details/cache/).
* **Ağ performansı**: Premium katmanı, yüksek aktarım hızı gerektiren bir iş yükü varsa, standart ya da temel sürümüne kıyasla daha fazla bant genişliği sunar. Ayrıca her bir katman içinde önbellek barındıran ve temel alınan VM nedeniyle daha fazla bant genişliği boyutu daha büyük önbellekleri vardır. Bkz: [aşağıdaki tabloda](#cache-performance) daha fazla bilgi için.
* **Aktarım hızı**: Premium katmanı, en yüksek kullanılabilir aktarım sunar. Önbellek sunucu veya istemci bant genişliği sınırlarını ulaşırsa, istemci tarafındaki zaman aşımı alabilirsiniz. Daha fazla bilgi için aşağıdaki tabloya bakın.
* **Yüksek kullanılabilirlik SLA**: Azure önbelleği için Redis standart/Premium önbelleğin en az % 99,9 oranında kullanılabilir olduğunu garanti eder. Sunduğumuz SLA hakkında daha fazla bilgi için bkz: [Azure önbelleği için Redis fiyatlandırma](https://azure.microsoft.com/support/legal/sla/cache/v1_0/). SLA yalnızca önbellek uç noktalarına bağlantıyı kapsar. SLA, veri kaybından korumayı kapsamaz. Veri kaybına karşı dayanıklılığı artırmak için Premium katmandaki Redis veri dayanıklılığı özelliğinin kullanılmasını öneririz.
* **Redis veri kalıcılığı**: Premium katman, kalıcı önbellek verileri bir Azure depolama hesabı olanak tanır. Temel/standart önbellekte, tüm verileri yalnızca bellekte depolanır. Temel alınan altyapı sorunları, olması durumunda olası veri kaybı olabilir. Veri kaybına karşı dayanıklılığı artırmak için Premium katmandaki Redis veri dayanıklılığı özelliğinin kullanılmasını öneririz. Azure önbelleği için Redis RDB ve (çok yakında) AOF Redis kalıcılığı seçenekleri sunar. Daha fazla bilgi için [Redis için bir Premium Azure Cache için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md).
* **Redis kümesi**: Birden çok Redis düğümü arasında verileri veya 53 GB'den daha büyük önbellekler oluşturmak için Premium katmanda kullanılabilir Redis kümeleme, kullanabilirsiniz. Her düğüm, yüksek kullanılabilirlik için bir birincil/çoğaltma önbellek çiftinin oluşur. Daha fazla bilgi için [Redis için Premium Azure Cache için kümeleri yapılandırma](cache-how-to-premium-clustering.md).
* **Gelişmiş Güvenlik ve ağ yalıtımı**: Geliştirilmiş güvenlik ve yalıtım, Azure önbelleği için Redis yanı sıra alt ağlar, erişim denetim ilkeleri için Azure sanal ağ (VNET) dağıtımı sağlar ve diğer özellikleri daha da fazla erişimi kısıtlayın. Daha fazla bilgi için [Redis için bir Premium Azure Cache için sanal ağ desteğini yapılandırma](cache-how-to-premium-vnet.md).
* **Redis'i yapılandırma**: Standart ve Premium katmanlarda, Redis için anahtar alanı bildirimleri yapılandırabilirsiniz.
* **İstemci bağlantılarının maksimum sayısı**: Premium katmanı, Redis, bağlantıların daha büyük boyutlu önbellekler için daha yüksek bir sayı ile bağlanabilir istemcilerin sayısı sunar. Küme, kümelenmiş bir önbellek için kullanılabilir bağlantı sayısını artırmaz. Daha fazla bilgi için [Azure Cache, Redis fiyatlandırma](https://azure.microsoft.com/pricing/details/cache/).
* **Redis sunucusu için çekirdek ayrılmış**: Premium katmanında, Redis için adanmış çekirdek tüm önbellek boyutuna sahip. C1 boyutu temel/standart katmanları ve sonraki sürümlerde Redis sunucusu için ayrılmış bir çekirdek vardır.
* **Redis, tek iş parçacıklı** ikiden fazla çekirdeğe sahip sağlamaz ek bir avantaja sahip iki çekirdek üzerinde ancak daha büyük VM boyutları, genellikle daha fazla bant genişliği daha küçük boyutlara sahip. Bant genişliği sınırlarını ulaşırsa, önbellek sunucu veya istemci, istemci tarafındaki zaman aşımı alırsınız.
* **Performans iyileştirmeleri**: Premium katmanda önbellekler, temel veya standart katmana kıyasla daha iyi performans sunar, daha hızlı işlemcilere sahip donanımda dağıtılır. Premium katman Önbelleklerinden daha yüksek aktarım hızı ve düşük gecikme sürelerine sahip.

<a name="cache-performance"></a>

### <a name="azure-cache-for-redis-performance"></a>Azure önbelleği için Redis performans
Aşağıdaki tablo standart çeşitli boyutlarda test ederken gözlemlenen en yüksek bant genişliği değerlerini gösterir ve Premium önbellekleri kullanarak `redis-benchmark.exe` Azure önbelleği için Redis uç nokta karşı bir Iaas VM'den. SSL Aktarım için redis Kıyaslama stunnel ile Azure önbelleği için Redis uç nokta bağlanmak için kullanılır.

>[!NOTE] 
>Bu değerleri garantili değildir ve bu SLA, sayılar, ancak tipik olmalıdır yoktur. Yük, uygulamanız için doğru önbellek boyutunu belirlemek için kendi uygulamanızı test edin.
>Bu sayı, biz yeni sonuçları düzenli aralıklarla gönderi gibi değişebilir.
>

Bu tabloda, size aşağıdaki sonuçları çizebilirsiniz:

* Aktarım hızı aynı boyutta önbellekler için standart katmana göre Premium katmanındaki daha yüksektir. Örneğin, bir 6 GB önbellek ile karşılaştırıldığında C3 için 100.000 180.000 RPS P1'in aktarım hızıdır.
* Kümedeki parça (düğümler) sayısı arttıkça, Redis Kümeleme ile üretilen işi doğrusal olarak artırır. 10 parça P4 kümesi oluşturursanız, örneğin, kullanılabilir aktarım hızı 400.000 ise * 10 = 4 milyon RPS.
* Aktarım hızı büyük anahtar boyutları için standart katmana göre Premium katmanındaki daha yüksektir.

| Fiyatlandırma katmanı | Boyut | CPU çekirdekleri | Kullanılabilir bant genişliği | 1 KB değeri boyutu | 1 KB değeri boyutu |
| --- | --- | --- | --- | --- | --- |
| **Standart önbellek boyutu** | | |**Megabit / sn (Mb/sn) / megabayt sayısı / sn (MB/sn)** |**İkinci (RP'ler) SSL olmayan başına istek sayısı** |**İkinci (RP'ler) SSL başına istek sayısı** |
| C0 |250 MB |Paylaşılan |100 / 12.5 |15.000 |7.500 |
| C1 |1 GB |1 |500 / 62.5 |38,000 |20,720 |
| C2 |2,5 GB |2 |500 / 62.5 |41,000 |37,000 |
| C3 |6 GB |4 |1000 / 125 |100.000 |90,000 |
| C4 |13 GB |2 |500 / 62.5 |60,000 |55,000 |
| C5 |26 GB |4 |1,000 / 125 |102,000 |93,000 |
| C6 |53 GB |8 |2,000 / 250 |126,000 |120,000 |
| **Premium önbellek boyutu** | |**Parça başına CPU çekirdekleri** | **Megabit / sn (Mb/sn) / megabayt sayısı / sn (MB/sn)** |**İkinci (RP'ler) SSL olmayan, parça başına başına istek sayısı** |**Parça başına ikinci (RP'ler) SSL başına istek sayısı** |
| P1 |6 GB |2 |1,500 / 187.5 |180,000 |172,000 |
| P2 |13 GB |4 |3,000 / 375 |350,000 |341,000 |
| P3 |26 GB |4 |3,000 / 375 |350,000 |341,000 |
| P4 |53 GB |8 |6,000 / 750 |400,000 |373,000 |

Stunnel ' ayarlama veya Redis araçları gibi yükleme yönergeleri için `redis-benchmark.exe`, bkz: [Redis komutları nasıl çalıştırırım?](#cache-commands) bölümü.

<a name="cache-region"></a>

### <a name="in-what-region-should-i-locate-my-cache"></a>Önbelleğimin hangi bölgede bulabilirim?
En iyi performans ve düşük gecikme süresi için Azure önbelleği için Redis önbelleği istemci uygulamanızı aynı bölgede bulun.

<a name="cache-billing"></a>

### <a name="how-am-i-billed-for-azure-cache-for-redis"></a>Azure önbelleği için Redis için nasıl faturalandırılırım?
Azure önbelleği için Redis fiyatlandırma [burada](https://azure.microsoft.com/pricing/details/cache/). Fiyatlandırma sayfasına bir saatlik tarife listeler. Önbellekler, önbelleği önbellek silinen zamana kadar oluşturulduğu zamandan dakika başına temelinde faturalandırılır. Durdurmak veya bir önbellek Faturalaması duraklatmak için seçeneği yoktur.

### <a name="can-i-use-azure-cache-for-redis-with-azure-government-cloud-azure-china-cloud-or-microsoft-azure-germany"></a>Azure önbelleği için Redis Azure kamu Bulutu, Azure Çin Bulutu veya Microsoft Azure Almanya ile kullanabilir?
Evet, Azure kamu Bulutu, Azure Çin Bulutu ve Microsoft Azure Almanya'yı Azure önbelleği için Redis kullanılabilir. Erişmek ve Azure Cache, Redis için farklı Azure genel bulut ile karşılaştırıldığında bu bulutlarındaki yönetmek için URL. 

| Bulut   | Redis için DNS son eki            |
|---------|---------------------------------|
| Genel  | *.redis.cache.windows.net       |
| US Gov  | *.redis.cache.usgovcloudapi.net |
| Almanya | *.redis.cache.cloudapi.de       |
| Çin   | *.redis.cache.chinacloudapi.cn  |

Azure önbelleği için Redis ile diğer bulutlarda kullanmayla ilgili konular hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın.

- [Azure kamu veritabanları - Azure önbelleği için Redis](../azure-government/documentation-government-services-database.md#azure-cache-for-redis)
- [Azure Çin Bulutu - Azure önbelleği için Redis](https://www.azure.cn/home/features/redis-cache/)
- [Microsoft Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)

Azure Cache için Redis Azure kamu Bulutu, Azure Çin Bulutu ve Microsoft Azure Almanya'yı PowerShell ile kullanma hakkında daha fazla bilgi için bkz: [diğer bulutlarda - Azure önbelleği için Redis PowerShell bağlanma](cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-other-clouds).

<a name="cache-configuration"></a>

### <a name="what-do-the-stackexchangeredis-configuration-options-do"></a>StackExchange.Redis yapılandırma seçeneklerini ne?
StackExchange.Redis pek çok seçenek vardır. Bu bölümde bazı ortak ayarlar hakkında konuşuyor. StackExchange.Redis seçenekleri hakkında daha ayrıntılı bilgi için bkz: [StackExchange.Redis yapılandırma](https://stackexchange.github.io/StackExchange.Redis/Configuration).

| ConfigurationOptions | Açıklama | Öneri |
| --- | --- | --- |
| AbortOnConnectFail |Ne zaman bir ağ hatasından sonra bağlantı true olarak yeniden bağlanmaz. |False olarak ayarlayın ve otomatik olarak yeniden StackExchange.Redis olanak tanır. |
| ConnectRetry |İlk kurulum sırasında bağlantı girişimlerini yineleme sayısı bağlanın. |Yönergeler için aşağıdaki notlara bakın. |
| ConnectTimeout |Connect işlemleri için ms cinsinden zaman aşımı. |Yönergeler için aşağıdaki notlara bakın. |

Genellikle istemcinin varsayılan değerler yeterlidir. Seçenekler, iş yüküne göre ince ayar yapabilirsiniz.

* **Yeniden deneme sayısı**
  * ConnectRetry ve ConnectTimeout için genel kılavuz hızla başarısız kılınacak ve yeniden deneyin sağlamaktır. Bu kılavuz iş yükünüzü ve ne kadar süre üzerinde ortalama, bir Redis komutu yürütün ve bir yanıt almak, istemci tarafından gerçekleştirilen işlemlerin temel alır.
  * Bağlantı durumunu denetleme ve kendiniz yeniden bağlanmayı yerine otomatik olarak yeniden StackExchange.Redis olanak tanır. **ConnectionMultiplexer.IsConnected özelliğini kullanmaktan kaçının**.
  * Snowballing - bazen Burada, yeniden deneniyor ve yeniden deneme Snowball'un bir sorun ve hiçbir zaman kurtarır çalışabilir. Snowballing ortaya çıkarsa, açıklandığı bir üstel geri alma yeniden deneme algoritmasını kullanmayı düşünmelisiniz [yeniden deneme genel Kılavuzu](../best-practices-retry-general.md) Microsoft Patterns & Uygulama grubu tarafından yayımlanmış.
* **Zaman aşımı değerleri**
  * İş yükünüz göz önünde bulundurun ve değerleri uygun şekilde ayarlayın. Büyük değerler depolanıyorsa, zaman aşımı daha yüksek bir değere ayarlayın.
  * Ayarlama `AbortOnConnectFail` için false ve let StackExchange.Redis sizin için yeniden bağlayın.
  * Tek bir ConnectionMultiplexer örnek uygulama için kullanın. Bir LazyConnection gösterildiği gibi bir bağlantı özelliği tarafından döndürülen tek bir örneğini oluşturmak için kullanabileceğiniz [ConnectionMultiplexer sınıfını kullanarak önbelleğe bağlanma](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).
  * Ayarlama `ConnectionMultiplexer.ClientName` özelliğini tanılama amacıyla bir uygulama örneği benzersiz adı.
  * Birden çok kullanın `ConnectionMultiplexer` örnekleri özel iş yükleri için.
      * Uygulamanızda değişen yükü varsa, bu modeli takip edebilirsiniz. Örneğin:
      * Çoğaltıcı büyük anahtarları başa çıkmak için bir tane olabilir.
      * Çoğaltıcı küçük anahtarları başa çıkmak için bir tane olabilir.
      * Bağlantı zaman aşımları için farklı değerler ayarlayın ve kullandığınız her ConnectionMultiplexer için yeniden deneme mantığı.
      * Ayarlama `ClientName` her özellik çoğaltıcı tanılamalarını sağlayacak.
      * Bu kılavuz için daha fazla yol açabilir rahat bir gecikme süresi başına `ConnectionMultiplexer`.

### <a name="what-azure-cache-for-redis-clients-can-i-use"></a>Hangi Azure önbelleği için Redis istemcileri kullanabilirim?
Redis hakkındaki harika şeylerden biri çok sayıda farklı geliştirme dili destekleyen birden çok istemci yoktur. İstemcileri güncel bir listesi için bkz. [Redis istemcileri](https://redis.io/clients). Birkaç farklı dilleri ve istemcilerin kapsayan öğreticiler için bkz. [Azure önbelleği için Redis kullanma](cache-dotnet-how-to-use-azure-redis-cache.md) ve bu tablodaki eşdüzey makaleler İçindekiler.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<a name="cache-emulator"></a>

### <a name="is-there-a-local-emulator-for-azure-cache-for-redis"></a>Yerel bir öykünücü Azure önbelleği için Redis mı?
Azure önbelleği için Redis için hiçbir yerel öykünücü yoktur, ancak redis server.exe MSOpenTech sürümünü çalıştırabileceğiniz [Redis komut satırı araçları](https://github.com/MSOpenTech/redis/releases/) yerel makine ve benzer bir deneyim yerel önbelleğe almak için bağlanın öykünücü, aşağıdaki örnekte gösterildiği gibi:

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


İsteğe bağlı olarak yapılandırabileceğiniz bir [redis.conf](https://redis.io/topics/config) daha yakından eşleşecek şekilde dosya [varsayılan önbellek ayarları](cache-configure.md#default-redis-server-configuration) isterseniz Redis için çevrimiçi, Azure önbelleği için.

<a name="cache-commands"></a>

### <a name="how-can-i-run-redis-commands"></a>Redis komutları nasıl çalıştırabilir miyim?
Konusunda listelenen komutlardan herhangi birini kullanabilirsiniz [Redis komutları](https://redis.io/commands#) konusunda listelenen komutları dışında [Redis komutları Azure önbelleği için Redis desteklenmeyen](cache-configure.md#redis-commands-not-supported-in-azure-cache-for-redis). Redis komutları çalıştırmak için birkaç seçeneğiniz vardır.

* Bir standart veya Premium önbellek varsa kullanarak Redis komutları çalıştırabilirsiniz [Redis Konsolu](cache-configure.md#redis-console). Redis konsolu, Azure portalında Redis komutları çalıştırmak için güvenli bir yol sağlar.
* Ayrıca, Redis komut satırı araçları da kullanabilirsiniz. Bunları kullanmak için aşağıdaki adımları gerçekleştirin:
* İndirme [Redis komut satırı araçları](https://github.com/MSOpenTech/redis/releases/).
* Önbellek kullanarak bağlanmak `redis-cli.exe`. -H geçiş kullanarak önbellek uç noktası aşağıdaki örnekte gösterildiği gibi kullanarak ve anahtarı geçirin:
* `redis-cli -h <Azure Cache for Redis name>.redis.cache.windows.net -a <key>`

> [!NOTE]
> Redis komut satırı araçlarını SSL bağlantı noktası ile çalışmaz, ancak gibi bir yardımcı programını kullanabilirsiniz `stunnel` güvenli bir şekilde Araçları'ndaki yönergeleri takip ederek SSL bağlantı noktasına bağlanmak için [Redis Önizleme için ASP.NET oturum durumu sağlayıcısı ile tanışın Yayın](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blog gönderisi.
>
>

<a name="cache-reference"></a>

### <a name="why-doesnt-azure-cache-for-redis-have-an-msdn-class-library-reference-like-some-of-the-other-azure-services"></a>Neden Azure önbelleği için Redis bazı diğer Azure Hizmetleri gibi bir MSDN sınıf kitaplığı başvurusu yok?
Microsoft Azure Cache Redis popüler Aç temel alınır, Azure önbelleği için Redis kaynak ve çok çeşitli tarafından erişilebilir [Redis istemcileri](https://redis.io/clients) birçok programlama dili için. Azure Cache, Redis örneğinin kullanılması için çağrılar kendi API her bir istemciye sahip [Redis komutları](https://redis.io/commands).

Her bir istemciye farklı olduğu için MSDN'de değil bir merkezi sınıf başvurusu yok ve kendi başvuru belgeleri her istemci korur. Başvuru belgeleri yanı sıra farklı dilleri kullanarak Redis için Azure Cache kullanmaya başlama ve istemciler önbelleğe nasıl yapıldığını gösteren çeşitli öğreticiler vardır. Bu öğreticiler erişmek için bkz: [Azure önbelleği için Redis kullanma](cache-dotnet-how-to-use-azure-redis-cache.md) ve bu tablodaki eşdüzey makaleler İçindekiler.

### <a name="can-i-use-azure-cache-for-redis-as-a-php-session-cache"></a>Azure önbelleği için Redis PHP oturum önbelleği olarak kullanabilir miyim?
Evet, Azure önbelleği için Redis PHP oturum önbelleği olarak kullanmak için Azure önbelleği için Redis örneği için bağlantı dizesini belirtin. `session.save_path`.

> [!IMPORTANT]
> Azure Cache, Redis için PHP oturum önbelleği olarak kullanırken, URL gerekir aşağıdaki örnekte gösterildiği gibi önbelleğe bağlanmak için kullanılan güvenlik anahtarı kodlama:
>
> `session.save_path = "tcp://mycache.redis.cache.windows.net:6379?auth=<url encoded primary or secondary key here>";`
>
> Anahtar URL kodlanmış değilse bir özel durumla gibi bir ileti alabilirsiniz: `Failed to parse session.save_path`
>
>

Azure Cache PhpRedis istemcisi ile PHP oturum önbelleği olarak Redis kullanma hakkında daha fazla bilgi için bkz. [PHP oturumu işleyici](https://github.com/phpredis/phpredis#php-session-handler).

### <a name="what-are-redis-databases"></a>Redis veritabanı nedir?

Redis, yalnızca bir mantıksal ayrılığı aynı Redis örneği içinde veri veritabanlarıdır. Önbellek, tüm gerçek bellek tüketimi, belirli bir veritabanının bu veritabanında depolanan anahtarları/değerleri bağlıdır ve veritabanları arasında paylaşılır. Örneğin C6 önbellek, 53 GB bellek bulunur. Veya, bunu birden çok veritabanı arasında bölmek tüm 53 GB bir veritabanı yerleştirmek seçebilirsiniz. 

> [!NOTE]
> Premium Azure önbelleği için Redis kümeleme özellikli kullanırken, yalnızca veritabanı 0 kullanılabilir. Bu sınırlama, iç bir Redis sınırlamasıdır ve Azure önbelleği için Redis özel değildir. Daha fazla bilgi için [kümeleme kullanmak üzere istemci uygulamamın herhangi bir değişiklik yapmanız gerekiyor mu?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 


<a name="cache-ssl"></a>

### <a name="when-should-i-enable-the-non-ssl-port-for-connecting-to-redis"></a>Redis'e bağlanmak için SSL olmayan bağlantı noktası olduğunda etkinleştirmeli miyim?
Redis sunucu SSL yerel olarak desteklemez, ancak Azure önbelleği için Redis yapar. Azure önbelleği için Redis için bağlama ve SSL, StackExchange.Redis gibi istemcinizi destekliyorsa SSL kullanmanız gerekir.

>[!NOTE]
>SSL olmayan bağlantı noktası yeni Azure önbelleği için Redis örneği için varsayılan olarak devre dışıdır. İstemci SSL desteklemeyen sonra SSL olmayan bağlantı noktası'ndaki yönergeleri takip ederek etkinleştirmelisiniz [erişim bağlantı noktaları](cache-configure.md#access-ports) bölümünü [önbelleği için Redis Azure önbelleğinde önbellek yapılandırma](cache-configure.md) makalesi.
>
>

Araçları gibi redis `redis-cli` SSL bağlantı noktası ile çalışmaz, ancak gibi bir yardımcı programını kullanabilirsiniz `stunnel` güvenli bir şekilde Araçları'ndaki yönergeleri takip ederek SSL bağlantı noktasına bağlanmak için [Redis için ASP.NET oturum durumu sağlayıcısı ile tanışın Önizleme sürümü](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) blog gönderisi.

Redis Araçları'nı yükleme hakkında yönergeler için bkz. [Redis komutları nasıl çalıştırırım?](#cache-commands) bölümü.

### <a name="what-are-some-production-best-practices"></a>Bazı üretim en iyi uygulamalar nelerdir?
* [StackExchange.Redis en iyi uygulamalar](#stackexchangeredis-best-practices)
* [Yapılandırma ve kavramları](#configuration-and-concepts)
* [Performansı test etme](#performance-testing)

#### <a name="stackexchangeredis-best-practices"></a>StackExchange.Redis en iyi uygulamalar
* Ayarlama `AbortConnect` false olarak ardından otomatik olarak yeniden ConnectionMultiplexer sağlar. [Ayrıntılar için buraya bakın](https://gist.github.com/JonCole/36ba6f60c274e89014dd#file-se-redis-setabortconnecttofalse-md).
* ConnectionMultiplexer yeniden - her istek için yeni bir tane oluşturmayın. `Lazy<ConnectionMultiplexer>` Deseni [burada gösterilen](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache) önerilir.
* Works en küçük değerler ile redis, bu nedenle büyük verilerin birden çok anahtar chopping göz önünde bulundurun. İçinde [bu Redis tartışma](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ), 100 kb büyük değerlendirilir. Okuma [bu makalede](https://gist.github.com/JonCole/db0e90bedeb3fc4823c2#large-requestresponse-size) büyük değerler neden bir örneğin sorun için.
* Yapılandırma, [iş parçacığı havuzu ayarları](#important-details-about-threadpool-growth) zaman aşımları önlemek için.
* En az 5 saniyelik varsayılan connectTimeout kullanın. Bu aralık, ağ blip durumunda bağlantıyı yeniden kurmak için yeterli zaman StackExchange.Redis verirsiniz.
* Kullanmakta olduğunuz farklı işlemleriyle ilişkili performans maliyetleri farkında olun. Örneğin, `KEYS` komut bir o(n) ve kaçınılmalıdır. [Redis.io site](https://redis.io/commands/) desteklediği her bir işlem için zaman karmaşıklığı ayrıntılarla sahiptir. Her komut, her işlem için karmaşıklık görmek için tıklayın.

#### <a name="configuration-and-concepts"></a>Yapılandırma ve kavramları
* Standart veya Premium katmanı, üretim sistemleri için kullanın. Temel katmanı, veri çoğaltma ve SLA ile bir tek düğümlü sistemidir. Ayrıca, en az bir C1 önbelleği kullanır. C0 önbellekler genellikle basit geliştirme/test senaryoları için kullanılır.
* Redis olduğunu unutmayın bir **bellek içi** veri deposu. Okuma [bu makalede](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) , burada veri kaybı oluşabilir senaryoları uyumlu olmasını sağlamak.
* Sisteminizde bağlantı blips işleyebilir, geliştirme [düzeltme eki uygulama ve yük devretme nedeniyle](https://gist.github.com/JonCole/317fe03805d5802e31cfa37e646e419d#file-azureredis-patchingexplained-md).

#### <a name="performance-testing"></a>Performansı test etme
* Başlangıç kullanarak `redis-benchmark.exe` kendi perf yazmadan önce olası aktarım hızı için bir genel görünüm almak için test eder. Çünkü `redis-benchmark` SSL desteklemez gerekir [Azure portalı üzerinden SSL olmayan bağlantı noktasını etkinleştirmek](cache-configure.md#access-ports) testi çalıştırmadan önce. Örnekler için bkz [nasıl Kıyaslama ve önbelleğimin performansını test etme?](#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* İstemci test etmek için kullanılan VM, Azure önbelleği için Redis örneği ile aynı bölgede olması gerekir.
* Daha iyi bir donanım profiliniz ve en iyi sonucu vermelidir Dv2 VM serisi için istemcinizi kullanmanızı öneririz.
* İstemcinizi seçtiğiniz VM en az bir bilgisayar vardır ve bant genişliği özelliği önbelleği olarak test ettiğiniz emin olun.
* Windows üzerinde ise VRSS istemci makinesinde etkinleştirin. [Ayrıntılar için buraya bakın](https://technet.microsoft.com/library/dn383582.aspx).
* Hem CPU hem de ağ daha iyi donanım üzerinde çalıştırdığınız için premium katmanı Redis örneği daha iyi gecikme süresi ve aktarım hızı ağ.

<a name="cache-redis-commands"></a>

### <a name="what-are-some-of-the-considerations-when-using-common-redis-commands"></a>Bazı önemli noktalar ortak Redis komutları kullanılırken nelerdir?
* Bu komutlar etkisini anlama olmadan tamamlanması uzun süren belirli Redis komutları çalıştırmamanız gerekir.
  * Örneğin, çalışmaz [ANAHTARLARI](https://redis.io/commands/keys) anahtarları sayısına bağlı olarak döndürülecek uzun sürebilir olarak üretim ortamında komutu. Redis bir tek iş parçacıklı bir sunucudur ve komutları teker teker işler. Diğer komutlar sonra ANAHTARLARI varsa, Redis ANAHTARLARI komut işlem kadar bunlar işlenmeyecek. [Redis.io site](https://redis.io/commands/) desteklediği her bir işlem için zaman karmaşıklığı ayrıntılarla sahiptir. Her komut, her işlem için karmaşıklık görmek için tıklayın.
* Küçük bir anahtar/değer ya da büyük bir anahtar/değer anahtar boyutları - kullanmalıyım? Genel olarak, senaryoya bağlıdır. Büyük anahtarlar senaryonuz gerektiriyorsa, ConnectionTimeout ayarlayın ve yeniden deneme değerleri ve yeniden deneme mantığınız ayarlayın. Redis sunucu açısından bakıldığında, daha iyi performans sağlamak için daha küçük değerler gözlenmiştir.
* Bu konuları daha büyük değerler Redis depolanamıyor olduğu anlamına gelmez; Aşağıdaki hususlara farkında olmanız gerekir. Gecikme, daha yüksek olacaktır. Bir büyük veri kümesi ve daha küçük olanı varsa, birden çok ConnectionMultiplexer örneği kullanabilirsiniz, her önceki açıklandığı gibi farklı bir zaman aşımı ve yeniden deneme değerlerini kümesi ile yapılandırılmış [StackExchange.Redis ne yapılandırma seçenekleri yapın](#cache-configuration) bölümü.

<a name="cache-benchmarking"></a>

### <a name="how-can-i-benchmark-and-test-the-performance-of-my-cache"></a>Nasıl Kıyaslama ve önbelleğimin performansını test etme?
* Önbelleğinizin sistem durumunu [izleyebilmeniz](cache-how-to-monitor.md) için [önbellek tanılamayı etkinleştirin](cache-how-to-monitor.md#enable-cache-diagnostics). Azure portalında ölçümleri görüntüleyebilir ve ayrıca istediğiniz araçları kullanarak bunları [indirebilir ve gözden geçirebilirsiniz](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring).
* Redis sunucunuza yük testi için redis benchmark.exe kullanabilirsiniz.
* İstemci ve Azure önbelleği için Redis sınama yük aynı bölgede olduğundan emin olun.
* Redis-cli.exe kullanın ve INFO komutunu kullanarak önbelleğe izleyin.
* Yüksek bellek parçalanması yük neden oluyorsa, daha büyük bir önbellek boyutu için ölçeğinizi.
* Redis Araçları'nı yükleme hakkında yönergeler için bkz. [Redis komutları nasıl çalıştırırım?](#cache-commands) bölümü.

Aşağıdaki komutları kullanarak redis benchmark.exe bir örnek sağlar. Doğru sonuçlar için önbellek ile aynı bölgede bir VM'den şu komutları çalıştırın.

* Bir 1 k yükü kullanarak test ardışık SET istekleri

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t SET -n 1000000 -d 1024 -P 50`
* Test ardışık alın, 1 k yük kullanarak ister.
  NOT: Önbellek doldurmak için ilk gösterilen KÜMESİ testi çalıştırın

  `redis-benchmark.exe -h **yourcache**.redis.cache.windows.net -a **yourAccesskey** -t GET -n 1000000 -d 1024 -P 50`

<a name="threadpool"></a>

### <a name="important-details-about-threadpool-growth"></a>İş parçacığı havuzu büyüme hakkında önemli ayrıntıları
CLR iş parçacığı havuzu iş parçacıkları - "Alt" ve "G/ç tamamlama bağlantı" (diğer adıyla görev:%TG/Ç) iki tür olan iş parçacıkları.

* Çalışan iş parçacığı işleme gibi şeyler için kullanılan `Task.Run(…)`, veya `ThreadPool.QueueUserWorkItem(…)` yöntemleri. İş bir arka plan iş parçacığında gerçekleştirilmesi gerektiğinde bu iş parçacıkları CLR içinde çeşitli bileşenleri tarafından da kullanılır.
* Görev:%TG/Ç iş parçacıkları, zaman uyumsuz g/ç (örneğin ağdan okunurken) olduğunda kullanılır.

İş parçacığı her tür için "Düşük" ayarı ulaşana kadar iş parçacığı havuzu yeni çalışan iş parçacıkları veya g/ç Tamamlama iş parçacıkları isteğe bağlı (bir kısıtlama olmadan) sağlar. Varsayılan olarak, bir sistemde işlemci sayısı için en az iş parçacığı sayısını ayarlanır.

Var olan (meşgul) iş parçacığı sayısı "minimum" iş parçacığı sayısını İsabetleri sonra işten başlangıçtan aralığını 500 milisaniyenin başına tek bir iş parçacığı için yeni iş parçacığı eklediği oranı kısıtlama. Sisteminizde bir görev:%TG/Ç iş parçacığı ihtiyaç duyan iş çok alırsa, genellikle, bu iş çok hızlı bir şekilde işler. Ancak, çok sayıda iş birden çok yapılandırılmış "Düşük" ayarı ise, olacaktır bazı gecikme işten olmasını ikisinden biri için bekleyeceği gibi işinin bir kısmını işleme.

1. Mevcut bir iş parçacığı işleri işlemek boş olur.
2. Yeni bir iş parçacığı oluşmasını mevcut iş parçacığının 500ms için boş olur.

Temelde, meşgul iş parçacığı sayısı en az iş parçacığı büyük olduğunda, ağ trafiğini uygulama tarafından işlenmeden önce büyük olasılıkla bir 500ms gecikme ödeme yaparsınız olduğunu anlamına gelir. Ayrıca, mevcut bir iş parçacığı (ben unutmayın üzerinde göre) 15 saniyeden daha uzun süre boşta kalır, temizlenen ve bu döngü büyüme ve azalma yineleyebilirsiniz dikkat etmeniz önemlidir.

Örnek hata iletisi StackExchange.Redis baktığımızda, (1.0.450 derleme veya üzeri), bunu şimdi (aşağıdaki ayrıntılara görev:%TG/Ç ve çalışan bakın) iş parçacığı havuzu istatistikleri yazdırır görürsünüz.

    System.TimeoutException: Timeout performing GET MyKey, inst: 2, mgr: Inactive,
    queue: 6, qu: 0, qs: 6, qc: 0, wr: 0, wq: 0, in: 0, ar: 0,
    IOCP: (Busy=6,Free=994,Min=4,Max=1000),
    WORKER: (Busy=3,Free=997,Min=4,Max=1000)

Önceki örnekte görev:%TG/Ç iş parçacığı için 6 meşgul iş parçacığı vardır ve sistem 4 en az iş parçacığı izin verecek şekilde yapılandırıldığını görebilirsiniz. Bu durumda, istemci büyük olasılıkla iki 500 ms gecikme nedeniyle gördünüz mü 6 > 4.

StackExchange.Redis görev:%TG/Ç veya çalışan iş parçacıkları büyütülmesini kısıtlanan, zaman aşımları ulaşmasını unutmayın.

### <a name="recommendation"></a>Öneri
Bu bilgiler verildiğinde, müşteriler varsayılan değerinden daha büyük bir şey görev:%TG/Ç ve çalışan iş parçacıkları için en düşük yapılandırma değeri ayarlanmış kesinlikle öneririz. Çok yüksek/düşük başka bir uygulama için bir uygulama için doğru değeri olacağından bu değer neler olması gerektiğini BT'ye Kılavuzu sunuyoruz olamaz. Her müşteriye kendi ihtiyaçlarınıza göre istediğiniz bu ayar ince ayar yapmak gereken şekilde bu ayar de karmaşık uygulamalar, diğer bölümlerini performansını etkileyebilir. İyi bir başlangıç noktası 200 300 olup, ardından test ve gerektiği gibi ince ayar.

Bu ayarı yapılandırmak nasıl:

* ASP.NET'te, kullanın ["minIoThreads" veya "minWorkerThreads" yapılandırma ayarı] [ "minIoThreads" configuration setting] altında `<processModel>` web.config dosyasındaki yapılandırma öğesi. Azure Web siteleri içinde çalıştırıyorsanız, bu ayarı yapılandırma seçenekleri gösterilmez. Ancak, yine de bu program aracılığıyla ayarını yapılandırmak erişebileceğinizi (aşağıda uygulama_başlatma yönteminizi global.asax.cs gelen bakın).

  > [!NOTE] 
  > Bu yapılandırma öğesinde belirtilen değer bir *çekirdek başına* ayarı. 4 çekirdekli makine olması ve minIOThreads ayarınızı zamanında 200 olmasını istiyorsanız, örneğin, kullanacağınız `<processModel minIoThreads="50"/>`.
  >

* ASP.NET dışında ve kullanmak, Azure Web siteleri global.asax [ThreadPool.SetMinThreads (...)](/dotnet/api/system.threading.threadpool.setminthreads#System_Threading_ThreadPool_SetMinThreads_System_Int32_System_Int32_) API.

  > [!NOTE]
  > Bu API tarafından belirtilen değere tam AppDomain etkileyen genel bir ayardır. 4 çekirdekli makine olması ve CPU başına 50 minWorkerThreads ve minIOThreads çalışma zamanı sırasında ayarlamak istediğiniz ThreadPool.SetMinThreads (200, 200) kullanmanız gerekir.

<a name="server-gc"></a>

### <a name="enable-server-gc-to-get-more-throughput-on-the-client-when-using-stackexchangeredis"></a>StackExchange.Redis kullanırken istemcide daha fazla iş üretmek sunucu GC etkinleştir
Etkinleştirme sunucusu GC istemci en iyi duruma getirmek ve StackExchange.Redis kullanırken daha iyi performans ve aktarım hızı sağlar. Sunucusu GC ve nasıl etkinleştirileceği konusunda daha fazla bilgi için aşağıdaki makalelere bakın:

* [Sunucu GC etkinleştirmek için](/dotnet/framework/configure-apps/file-schema/runtime/gcserver-element)
* [Çöp toplamanın temelleri](/dotnet/standard/garbage-collection/fundamentals)
* [Çöp toplama ve performans](/dotnet/standard/garbage-collection/performance)


### <a name="performance-considerations-around-connections"></a>Bağlantıları etrafında performansla ilgili önemli noktalar

Her fiyatlandırma katmanının istemci bağlantıları, bellek ve bant genişliği için farklı sınırlara sahiptir. Her önbellek boyutu sağlar, ancak *kadar* belirli sayıda bağlantıları, her bağlantı için Redis ek yükü ile ilişkili. TLS/SSL şifreleme sonucunda CPU ve bellek kullanımı gibi ek yükü örneği olacaktır. Belirtilen önbellek boyutu için en yüksek bağlantı sınırı hafifçe yüklü bir önbellek varsayar. Yük bağlantı ek yükünden *artı* istemci işlemleri yükü sistem kapasiteyi aşıyor, geçerli önbellek boyutu için bağlantı sınırını aştınız değil olsa bile önbelleğin kapasite sorunları oluşabilir.

Her katman farklı bağlantı sınırları hakkında daha fazla bilgi için bkz. [Azure Cache, Redis fiyatlandırma](https://azure.microsoft.com/pricing/details/cache/). Bağlantılar ve diğer varsayılan yapılandırmalar hakkında daha fazla bilgi için bkz. [varsayılan Redis sunucu yapılandırması](cache-configure.md#default-redis-server-configuration).

<a name="cache-monitor"></a>

### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Sistem durumunu ve performansını önbelleğimin nasıl izleyebilirim?
Microsoft Azure Cache, Redis örneği için de izlenebilir [Azure portalında](https://portal.azure.com). Ölçümleri görüntüleyin, ölçüm grafikleri başlangıç panosuna sabitlemek, tarih ve saat aralığı grafikleri izleme özelleştirme, ekleyin ve ölçümleri grafikten kaldırabileceğiniz ve belirli koşullar karşılandığında uyarılar ayarlayın. Daha fazla bilgi için [İzleyici Azure önbelleği için Redis](cache-how-to-monitor.md).

Azure önbelleği için Redis **kaynak menüsünde** de izleme ve sorun giderme önbelleklerinizi için çeşitli araçlar içerir.

* **Sorunları tanılama ve çözme** çözümlemek için genel sorunlar ve stratejileri hakkında bilgi sağlar.
* **Kaynak durumu** , kaynağınızı izler ve beklendiği gibi çalışıp çalışmadığını bildirir. Azure kaynak sistem durumu hizmeti hakkında daha fazla bilgi için bkz: [Azure kaynak durumu genel bakış](../resource-health/resource-health-overview.md).
* **Yeni destek isteği** önbellek hesabınız için bir destek isteği açmak için seçenekler sağlar.

Bu araçlar, Azure önbelleği için Redis örneği ve önbelleğe alma uygulamalarınızı yönetmenize yardımcı durumunu izlemenize olanak tanır. Daha fazla bilgi için "Destek ve sorun giderme ayarları" bölümüne bakın. [Azure önbelleği için Redis yapılandırma](cache-configure.md).

<a name="cache-timeouts"></a>

### <a name="why-am-i-seeing-timeouts"></a>Zaman aşımı neden görüyorum?
Redis'e konuşmak için kullandığınız istemci zaman aşımları gerçekleşir. Redis sunucusuna komut gönderildiğinde, komut kuyruğa alınır ve Redis sunucusuna sonunda komutu seçer ve yürütür. Ancak istemci, bu işlem sırasında zaman aşımı ve bir özel durum mu olabilir, çağıran tarafında oluşturulur. Zaman aşımı sorunlarını giderme hakkında daha fazla bilgi için bkz. [istemci tarafı sorunlarını giderme](cache-how-to-troubleshoot.md#client-side-troubleshooting) ve [StackExchange.Redis zaman aşımı özel durumları](cache-how-to-troubleshoot.md#stackexchangeredis-timeout-exceptions).

<a name="cache-disconnect"></a>

### <a name="why-was-my-client-disconnected-from-the-cache"></a>Neden istemcim önbellekten kesildi?
Bir önbellek bağlantıyı kes bazı yaygın nedeni aşağıda verilmiştir.

* İstemci tarafı neden olur
  * İstemci uygulaması yeniden dağıtıldı.
  * İstemci uygulama bir ölçeklendirme işlemi gerçekleştirildi.
    * Bulut hizmeti veya Web uygulaması söz konusu olduğunda, bu otomatik ölçeklendirme nedeniyle olabilir.
  * İstemci tarafında ağ katmanı değiştirildi.
  * Geçici bir hata oluştu. istemci veya sunucu ile istemci arasındaki ağ düğümleri.
  * Bant genişliği eşiği sınırları ulaşılan.
  * CPU bağımlı işlemlerinin tamamlanması çok uzun sürdü.
* Sunucu tarafı neden olur
  * Standart önbellek teklifi üzerinde bir yük devretme birincil düğümden ikincil düğüme Azure önbelleği için Redis hizmeti başlatıldı.
  * Azure önbellek dağıtıldığı örneği düzeltme eki
    * Bu Redis sunucu güncelleştirmeleri veya genel sanal makine bakım için olabilir.

### <a name="which-azure-cache-offering-is-right-for-me"></a>Hangi Azure Önbellek teklifi bana uygundur?
> [!IMPORTANT]
> Önceki yılın göre [duyuru](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/), Azure yönetilen önbellek hizmeti ve Azure rol içi önbellek hizmeti **devre dışı bırakılmış** 30 Kasım 2016. Önerimizde kullanmaktır [Azure önbelleği için Redis](https://azure.microsoft.com/services/cache/). Geçiş hakkında daha fazla bilgi için bkz: [Azure önbelleği için Redis yönetilen önbellek Hizmeti'nden geçiş](cache-migrate-to-redis.md).
>
>

### <a name="azure-cache-for-redis"></a>Redis için Azure Önbelleği
Azure önbelleği için Redis sunulmuştur 53 GB'ye varan boyutlarda ve kullanılabilirlik SLA'sı % 99,9 olan ' dir. Yeni [premium katmanı](cache-premium-tier-intro.md) boyutu en fazla sunar 530 GB'a kadar ve kümeleme, sanal ağ ve % 99,9 SLA ile Kalıcılık için destek.

Azure önbelleği için Redis müşteriler için Redis, Microsoft tarafından yönetilen güvenli ve adanmış bir Azure önbelleği kullanma olanağı sunar. Bu teklif ile Redis, güvenilir barındırma ve izleme Microsoft tarafından sağlanan bir ekosistem ve zengin özelliklerle yararlanarak alın.

Yalnızca anahtar-değer çiftleri ile uğraşmak geleneksel önbellekler, Redis, yüksek performans sağlamasının yanı sıra veri türleri için yaygın olarak kullanılır. Ayrıca, bir dizeye ekleme gibi bu türlerinde atomik işlemler çalıştırmayı destekler redis; bir karma değer artan; bir listeye gönderme; bilgi işlem küme kesişimi, union ve fark; veya üye bir sıralanmış küme, en yüksek derecelendirme ile alınıyor. Diğer özellikler, işlemler, yayımlama/abonelik, Lua komut dosyası, anahtarlar zaman yaşam ve daha geleneksel bir önbellek gibi davranmasını Redis davranmasını sağlayan yapılandırma ayarları için destek içerir.

Başka bir anahtar Redis başarılı etrafına yerleşik sağlıklı, canlı bir açık kaynak ekosistemine yönüdür. Bu Redis istemcilerinin kullanılabilir farklı kümesinde birden çok dil arasında yansıtılır. Bu ekosistem ve çeşitli istemciler Azure önbelleği için Redis, Azure'da oluşturabileceğiniz her türlü iş yükü tarafından kullanılmak üzere izin verir.

Redis için Azure önbelleği ile çalışmaya başlama hakkında daha fazla bilgi için bkz. [kullanımı Azure Cache, Redis için nasıl](cache-dotnet-how-to-use-azure-redis-cache.md) ve [Azure önbelleği için Redis belgeleri](index.md).

### <a name="managed-cache-service"></a>Yönetilen önbellek hizmeti
[Önbellek hizmeti 30 Kasım 2016 devre dışı bırakılan yönetilen.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

Arşivlenmiş belgeler görüntülemek için bkz: [arşivlenmiş yönetilen önbellek hizmeti belgeleri](/previous-versions/azure/azure-services/dn386094(v=azure.100)).

### <a name="in-role-cache"></a>Rol İçi Önbellek
[Rol içi önbellek 30 Kasım 2016 devre dışı bırakılan.](https://azure.microsoft.com/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/)

Arşivlenmiş belgeler görüntülemek için bkz: [arşivlenmiş rol içi önbellek belgeleri](/previous-versions/azure/azure-services/dn386103(v=azure.100)).

["minIoThreads" configuration setting]: https://msdn.microsoft.com/library/vstudio/7w2sway1(v=vs.100).aspx
