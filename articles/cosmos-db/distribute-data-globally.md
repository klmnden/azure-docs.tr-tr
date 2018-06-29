---
title: Azure Cosmos DB genel verilerle dağıtma | Microsoft Docs
description: Azure Cosmos DB, genel olarak dağıtılmış, belirleyebiliriz model veritabanı hizmeti genel veritabanlarından kullanarak planet ölçekli coğrafi çoğaltma, yük devretme ve veri kurtarma hakkında bilgi edinin.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: sngun
ms.openlocfilehash: b161fad822804ed0b2a6c7ad5315eca45984b19d
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081498"
---
# <a name="how-to-distribute-data-globally-with-azure-cosmos-db"></a>Azure Cosmos DB genel verilerle dağıtmak nasıl
Azure bulunabilen - 50 + coğrafi bölgeler arasında genel ayak izini sahiptir ve sürekli genişleyen. Genel iletişim durumu ile Azure, geliştiricilere sunduğu farklı özellikleri oluşturmak, dağıtmak ve genel olarak dağıtılmış uygulamaları kolayca yönetin olanağı biridir. 

[Azure Cosmos DB](../cosmos-db/introduction.md), Microsoft'un görev açısından kritik uygulamalar için genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB sağlar anahtar teslimi genel dağıtım [üretilen iş ve depolama esnek ölçeklendirme](../cosmos-db/partition-data.md) 99, dünya çapında, tek basamaklı milisaniyelik gecikme [beş iyi tanımlanmış tutarlılık modelleri](consistency-levels.md), yüksek oranda kullanılabilirlik, tarafından yedeklenen tüm garanti [endüstri lideri kapsamlı SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [tüm verileriniz otomatik olarak dizinler](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) gerektirmeden şema veya dizin yönetimi ile ilgilidir. Birden çok model bir hizmettir ve belge, anahtar-değer, grafik ve sütun ailesi veri modelleri destekler. Bir yerel born bulut hizmetinde, Azure Cosmos DB dikkatle sıfırdan çoklu kiracı ve genel dağıtım ile yukarı geliştirilmiştir.


![Bölümlenmiş ve üç bölgeler arasında dağıtılmış azure Cosmos DB koleksiyonu](./media/distribute-data-globally/global-apps.png)

**Bölümlenmiş ve birçok Azure bölgesinde dağıtılmış bir tek bir Azure Cosmos DB kapsayıcısı**

Biz Azure Cosmos DB oluşturulurken öğrendiğiniz gibi ekleme genel dağıtım bir sonradan akla olamaz. "Cıvatalı"tek site"veritabanı sistem üzerinde açma" olamaz. Geleneksel coğrafi olağanüstü durum kurtarma (DR coğrafi) "tek siteli" veritabanı tarafından sunulan ötesinde genel olarak dağıtılmış bir veritabanı tarafından sunulan yetenekleri span. Tek site veritabanları coğrafi DR özelliği sunan Genel dağıtılmış veritabanlarının katı bir alt kümesidir. 

İle anahtar teslimi genel dağıtım Azure Cosmos veritabanı, geliştiricilerin kendi çoğaltma yapı iskelesi ya da kullanan Lambda deseni oluşturmak zorunda değildir (örneğin, [AWS DynamoDB çoğaltma](https://github.com/awslabs/dynamodb-cross-region-library/blob/master/README.md)) veritabanı günlük üzerinden ya da "çift yazma" birden çok bölgeler arasında gerçekleştiriliyor. Bunu yapmadan *değil* böyle yaklaşımlar doğruluğunu sağlamak ve ses SLA sağlamak mümkün olduğundan bu yaklaşım önerilir. 

Bu makalede, Azure Cosmos veritabanı genel dağıtım özelliklerine genel bakış sağlar. Biz de kapsamlı SLA sağlayan Azure Cosmos DB'ın benzersiz yaklaşımı açıklanmaktadır. 

## <a id="EnableGlobalDistribution"></a>Anahtar teslimi genel dağıtım etkinleştirme
Azure Cosmos DB Genel dağıtılmış uygulamaları kolayca yazmanızı etkinleştirmek için aşağıdaki özellikleri sağlar. Bu özellikler Azure Cosmos veritabanı kaynak sağlayıcı tabanlı kullanılabilir [REST API'leri](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/) yanı sıra Azure portalı.

Eylem anahtar teslimi genel dağıtım Özelliği Azure Cosmos veritabanı görmek için aşağıdaki videoyu izleyin.

> [!VIDEO https://www.youtube.com/embed/1D06yjTVxt8]
>

### <a id="RegionalPresence"></a>Her yerden bölgesel varlığı 
Azure sürekli olarak artmaktadır coğrafi varlığını getirerek [yeni bölgeler](https://azure.microsoft.com/regions/) çevrimiçi. Azure Cosmos DB sınıflandırılmış olarak bir *temel hizmet* Azure ve varsayılan olarak tüm yeni Azure bölgelerde kullanılabilir. Bu iş için yeni bölge Azure açar açmaz coğrafi bölge Azure Cosmos DB veritabanı hesabınız ile ilişkilendirmenizi sağlar.

### <a id="UnlimitedRegionsPerAccount"></a>Bölgeler sınırsız sayıda Azure Cosmos DB veritabanı hesabınızı ile ilişkilendirme
Azure Cosmos DB Azure bölgeleri herhangi bir sayıda Azure Cosmos DB veritabanı hesabınız ile ilişkilendirmenizi sağlar. Coğrafi yalıtma kısıtlamaları dışında (örneğin, Çin, Almanya), Azure Cosmos DB veritabanı hesabınızla ilişkili olabilir bölgeler sayısının bir sınırlama yoktur. Aşağıdaki şekilde 25 Azure bölgeler arasında span için yapılandırılmış bir veritabanı hesabı gösterilmiştir.  

![25 Azure bölgeleri kapsayıcı azure Cosmos DB veritabanı hesabı](./media/distribute-data-globally/spanning-regions.png)

**Bir kiracının Azure Cosmos DB veritabanı hesabı yayılan 25 Azure bölgeleri**


### <a id="PolicyBasedGeoFencing"></a>İlke tabanlı coğrafi yalıtma
Azure Cosmos DB ilke tabanlı coğrafı destekleyecek şekilde tasarlanmıştır. Coğrafi yalıtma veri denetim ve uyumu kısıtlamaları sağlamak için önemli bir bileşenidir ve belirli bir bölgede hesabınızla ilişkilendirdiğiniz engelleyebilir. Coğrafi yalıtma örnekleri dahil (ancak bunlarla sınırlı değildir) sovereign Bulutu (örneğin, Çin ve Almanya) içinde veya kamu vergi sınırında (örneğin, Avustralya) bölgelere genel dağıtım kapsamı. İlkeleri, Azure aboneliğinizin meta veriler kullanılarak denetlenir.

### <a id="DynamicallyAddRegions"></a>Dinamik olarak ekleme ve bölgeler kaldırma
Azure Cosmos DB (ilişkilendirme) eklemek veya kaldırmak verir (ilişkilendirmesini) veritabanı hesabınızı, herhangi bir noktada bölgelerden (bkz [önceki şekil](#UnlimitedRegionsPerAccount)). Paralel çoğaltma bölümleri arasında veri Azure Cosmos DB yeni bir bölge eklendiğinde, bu, 30 dakika içinde herhangi bir yere dünyada işlemleri için kullanılabilir olmasını sağlar (verilerinizi varsayılarak olan 100 TBs veya daha az). 

### <a id="FailoverPriorities"></a>Yük devretme öncelikler
Bölgesel yük devretme işlemlerini kesintiye durumlarda tam dizisini denetlemek için Azure Cosmos DB ilişkilendirmenize olanak sağlayan bir *öncelik* veritabanıyla ilişkili çeşitli bölgeleriyle (Aşağıdaki şekle bakın) hesap. Azure Cosmos DB otomatik yük devretme sırası belirttiğiniz öncelik sırasına göre oluşmamasını sağlar. Bölgesel yük devretme hakkında daha fazla bilgi için bkz: [Azure Cosmos veritabanı iş sürekliliği için otomatik bölgesel yük devretme işlemlerini](regional-failover.md).


![Yük devretme öncelikleri Azure Cosmos DB ile yapılandırma](./media/distribute-data-globally/failover-priorities.png)

**Bir kiracı Azure Cosmos DB'nin veritabanı hesabıyla ilişkilendirilmiş bölgeler için yük devretme öncelik sırasına (sağ bölme) yapılandırabilirsiniz**

### <a id="ConsistencyLevels"></a>Genel olarak dağıtılmış veritabanları için birden çok iyi tanımlanmış tutarlılık modelleri
Azure Cosmos DB destekleyen [birden çok iyi tanımlanmış, sezgisel ve pratik tutarlılık modelleri](consistency-levels.md) SLA ile yedeklenir. (Listeden kullanılabilir seçeneklerden) belirli tutarlılık modeline iş yükü/senaryoları bağlı olarak seçebilirsiniz. 

### <a id="TunableConsistency"></a>Genel olarak çoğaltılmış veritabanları için ince ayarlanabilir tutarlılık
Azure Cosmos DB programlı olarak geçersiz kılın ve çalışma zamanında bir istek başına varsayılan tutarlılık seçeneği hafifletin izin verir. 

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Dinamik olarak yapılandırılabilir okuma ve yazma bölgeleri
Azure Cosmos DB, "Okuma", "yazma" veya "okuma/yazma" bölgeleri için (veritabanıyla ilişkili) bölgeleri yapılandırmanıza olanak sağlar. 

### <a id="ElasticallyScaleThroughput"></a>Azure bölgeler arasında özellikler esnek ölçeklendirme işleme
Özellikler esnek bir Azure Cosmos DB kapsayıcısı tarafından sağlama verimlilik programlı olarak ölçekleme yapabilirsiniz. Üretilen iş Azure Cosmos DB kapsayıcı içinde dağıtılmış tüm bölgelere uygulanır.

### <a id="GeoLocalReadsAndWrites"></a>Coğrafi yerel okuma ve yazma işlemleri
Genel olarak dağıtılmış bir veritabanı, temel avantaj, düşük gecikme süresi dünyanın verilere erişimin sunar olmalıdır. Azure Cosmos DB 99 dünya çapında yazar ve düşük gecikme süresi okuma sunar. Tüm okuma en yakın (yerel) bölgesinden sunulan sağlar. Okuma isteği hizmet için çekirdek okuma verildiği bölgesine yerel kullanılır. Aynı yazma işlemleri için geçerlidir. Yalnızca çoğaltmaların çoğunluğu işlemi tamamlandıktan sonra yazma yerel olarak ancak yazmaları onaylamak için uzaktan çoğaltmaları geçişli olmadan bir yazma onaylanır. Farklı koymak için okuma ve yazma çekirdekleri her zaman isteği bir burada verilen bölge yerel varsayımına altında Azure Cosmos DB çoğaltma Protokolü çalışır.

### <a id="ManualFailover"></a>El ile yük devretme
Azure Cosmos DB doğrulamak için bir veritabanı hesabı için yük devretme tetiklemek verir *uçtan uca* kullanılabilirlik özelliklerine (veritabanı) dışındaki tüm uygulama. Güvenlik ve hata algılama ve öncü seçim liveness özelliklerini garanti olduğundan, Azure Cosmos DB garanti *sıfır veri kaybı* Kiracı tarafından başlatılan el ile yük devretme işlemi için.

### <a id="AutomaticFailover"></a>Otomatik Yük devretme
Azure Cosmos DB otomatik yük devretme sırasında bir veya daha fazla bölgesel kesintileri destekler. Bölgesel bir yük devretme sırasında Azure Cosmos DB okuma gecikmesi, çalışma süresini kullanılabilirlik, tutarlılık ve verimlilik SLA tutar. Azure Cosmos DB tamamlamak için bir otomatik yük devretme işlemi boyunca bir üst sınır sağlar. Bu olası veri kaybı sırasında bölgesel bir kesintinin penceredir.

### <a id="GranularFailover"></a>Farklı bir yük devretme ayrıntı düzeyi için tasarlanmış
Şu anda otomatik ve el ile yük devretme yetenekleri veritabanı hesabı bazda sunulur. Not, dahili olarak Azure Cosmos DB sunmak üzere tasarlanmıştır *otomatik* yük devretme işleminde daha hassas ayrıntı düzeyi bir veritabanı, bir kapsayıcı veya hatta bir bölümünü (anahtarları çeşitli sahip olan bir kapsayıcı). 

### <a id="MultiHomingAPIs"></a>Azure Cosmos DB içinde birden çok giriş
Azure Cosmos DB kullanarak veritabanıyla etkileşim olanak tanır *mantıksal* (bölge belirsiz) veya *fiziksel* (bölgeye özgü) uç noktaları. Mantıksal uç noktalarını kullanarak çok konaklı bir yük devretme durumunda, uygulama saydam olabilir sağlar. İkincisi, fiziksel bir uç nokta okuma yönlendirmek için uygulamaya ayrıntılı denetim sağlar ve belirli bölgelerine yazar.

İçin okuma tercihlerinin nasıl yapılandırılacağı hakkında bilgi bulabilirsiniz [SQL API](../cosmos-db/tutorial-global-distribution-sql-api.md), [tablo API](../cosmos-db/tutorial-global-distribution-table.md), ve [MongoDB API](../cosmos-db/tutorial-global-distribution-mongodb.md) bu makalelerde.

### <a id="TransparentSchemaMigration"></a>Saydam ve tutarlı veritabanı şeması ve dizin geçirme 
Azure Cosmos DB olan tam olarak [şema belirsiz](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Veritabanı Altyapısı'nın benzersiz tasarım Azure Cosmos DB otomatik olarak tanır ve zaman uyumlu olarak tüm verileri alımı, bağlı herhangi bir şemayı ya da ikincil dizinlerin kullanıcıdan gerek kalmadan dizin. Bu, veritabanı şeması ve dizin geçiş hakkında kaygı veya şema değişiklikleri çok aşaması piyasaya sürülmeleri Eşgüdümleme Genel dağıtılmış uygulamanızı hızlı bir şekilde yinelemek sağlar. Azure Cosmos DB açıkça sizin tarafınızdan yapılan ilkelere dizin oluşturma için değişiklikleri performans veya kullanılabilirlik düşmesine yol açmamasını güvence altına alır.  

### <a id="ComprehensiveSLAs"></a>Kapsamlı SLA (ötesinde, yüksek kullanılabilirlik)
Genel olarak dağıtılmış veritabanı hizmet olarak Azure Cosmos DB için iyi tanımlanmış ve kapsamlı SLA sunar **kullanılabilirlik**, **gecikme**, **işleme**ve **tutarlılık** veritabanıyla ilişkili bölgeler sayısından bağımsız olarak küresel ölçekli çalıştıran veritabanı için.  

## <a id="LatencyGuarantees"></a>Gecikme garanti
Verilerinize dünyanın düşük gecikmeli erişim sunmak için Azure Cosmos DB gibi genel dağıtılmış veritabanı hizmetinin temel avantaj olmalıdır. Azure Cosmos DB çeşitli veritabanı işlemleri için 99 garantili düşük gecikme süresi sunar. Azure Cosmos DB kullanan çoğaltma Protokolü (okuma ve yazma) veritabanı işlemleri, istemcinin yerel bölgede her zaman gerçekleştirilen sağlar. Azure Cosmos DB SLA gecikme garanti 99, okuma, (zaman uyumlu olarak) dizinli yazma ve sorgular için çeşitli istek ve yanıt boyutları sağlar. Yazma işlemleri için gecikme garanti yerel bölge içinde dayanıklı çoğunluğu çekirdek işlemlerini içerir.

### <a id="LatencyAndConsistency"></a>Gecikme süresi'nin ilişki tutarlılık 
Genel olarak dağıtılmış kurulumunda güçlü tutarlılık sağlamaya Genel dağıtılmış hizmet için zaman uyumlu olarak yazar çoğaltmak için veya çapraz bölge okuma zaman uyumlu olarak gerçekleştirmek için gerekir. Güçlü tutarlılık daha yüksek gecikme süreleri ve azaltılmış kullanılabilirlik veritabanı işlemlerinin neden açık ve geniş alan ağı güvenilirlik hızını belirler. Bu nedenle, sunmak için düşük gecikme süreleri 99 ve tüm tek bölge ve tüm bölgeli hesapları gevşek tutarlılık için % 99,99 kullanılabilirlik ve tüm hesaplarda bölgeli veritabanını, hizmet %99.999 kullanılabilirliğini garanti zaman uyumsuz çoğaltma uygulamanız gerekir. Hizmet ayrıca sağlaması gerekir bu içinde dönüş gerektiren [iyi tanımlanmış, gevşek tutarlılık modellere](consistency-levels.md) – daha güçlü (düşük gecikme süresi ve kullanılabilirliğini garanti sunmak için) ve "son" tutarlılık ideal güçlü zayıf (ile bir sezgisel programlama modeli).

Azure Cosmos DB okuma işlemi çoğaltmaları belirli tutarlılık düzeyi garantili teslim etmek için birden çok bölgeler arasında iletişim için gerekli olmayan sağlar. Benzer şekilde, verileri çoğaltılırken bir yazma işlemi engellediği değil sağlar tüm bölgeler arasında diğer bir deyişle, yazma zaman uyumsuz olarak bölgeler arasında çoğaltılır). Bölgeli veritabanı hesapları için birden çok gevşek tutarlılık düzeylerini yanı sıra güçlü hem de kullanılabilir. 

### <a id="LatencyAndAvailability"></a>Kullanılabilirlik ile gecikme'nın ilişkisi 
Gecikme süresi ve kullanılabilirliği aynı para iki tarafında vardır. Gecikme süresi bir işlemin kararlı durum ve hataları ve ağ bölümleri varlığında kullanılabilirliği hakkında konuşurken. Uygulama açısından bir yavaş çalışan veritabanı kullanılamıyor bir veritabanından ayırt işlemdir. 

Yüksek gecikme kullanılamazlık ayırt etmek için çeşitli veritabanı işlemleri için gecikme süresini mutlak bir üst sınır Azure Cosmos DB sağlar. Veritabanı işlemi tamamlamak için üst sınırdan daha uzun sürerse, Azure Cosmos DB bir zaman aşımı hatası döndürür. Zaman aşımları kullanılabilirlik SLA'sı karşı sayılır Azure Cosmos DB kullanılabilirlik SLA'sı sağlar. 

### <a id="LatencyAndThroughput"></a>Üretilen iş ile gecikme'nın ilişkisi
Azure Cosmos DB, gecikme ve verimlilik arasında seçim yapmaz. Her iki gecikme süresi 99 SLA geliştirir ve sağlanan verimlilik sağlar. 

## <a id="ConsistencyGuarantees"></a>Tutarlılık
Sırada [güçlü tutarlılık modeli](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) altın standart veri programlamasına, yüksek fiyat üzerinden yüksek gecikme (kararlı durumda) gelir ve kullanılabilirlik (karşısında hataları) azalır. 

Azure Cosmos DB iyi tanımlanmış bir programlama modeli, çoğaltılmış verilerinin tutarlılık hakkında nedeni sunar. Genel olarak dağıtılmış uygulamaların çok girişli özelliğine sahip kolayca oluşturmanıza olanak etkinleştirmek için Azure Cosmos DB tarafından kullanıma sunulan tutarlılık modelleri okuma ve yazma işlemleri burada yükleniyor gelen bölgesinden bölge bağımsız ve bağımsız olacak şekilde tasarlanmıştır hizmet. 

Azure Cosmos veritabanı tutarlılık SLA % 100'okuma isteklerinin (ya da veritabanı hesabı veya istek düzeyinde) sizin tarafınızdan belirtilen tutarlılık model tutarlılığı garanti karşıladığını güvence altına alır. Tutarlılık düzeyi ile ilişkili tüm tutarlılığı garanti sağlanırsa Okuma isteği SLA, tutarlılık karşıladığınızı kabul edilir. Aşağıdaki tabloda Azure Cosmos DB tarafından sunulan belirli tutarlılık modelleri karşılık tutarlılığı garanti yakalar.

<table>
    <tr>
        <td><strong>Tutarlılık modeli</strong></td>
        <td><strong>Tutarlılık özellikleri</strong></td>
        <td><strong>SLA</strong></td>
    </tr>
        <tr>
        <td>Güçlü</td>
        <td>Linearizable</td>
        <td>100%</td>
    </tr>
    <tr>
        <td rowspan="3">Sınırlanmış eskime durumu</td>
        <td>Monoton okuma (bölge içinde)</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Tutarlı öneki</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Bağlı eskime durumu &lt; K, T</td>
        <td>100%</td>
    </tr>
<tr>
        <td rowspan="3">Oturum</td>
        <td>Okuma kendi yazma</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Monoton okuma</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Tutarlı öneki</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Tutarlı öneki</td>
        <td>Tutarlı öneki</td>
        <td>100%</td>
    </tr>
</table>

**Azure Cosmos veritabanı verilen tutarlılık modeli ile ilişkili tutarlılığı garanti**


### <a id="ConsistencyAndAvailability"></a>Kullanılabilirlik ile tutarlılık'ın ilişkisi
[İmpossibility sonuç](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) , [CAP Teoremi](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) kullanılabilir durumda kalır ve linearizable tutarlılık hataları karşısında sunmak bir sistem gerçekten olanaksız olduğunu kanıtlar. Veritabanı hizmeti CP veya AP, burada CP sistemleri atlayabilirsiniz linearizable tutarlılık lehinde kullanılabilirlik AP sistemleri atlayabilirsiniz sırada olacak şekilde seçmelisiniz [linearizable tutarlılık](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) kullanılabilirlik lehinde. Azure Cosmos DB hiçbir zaman bir CP sistem resmi olarak kolaylaştırır istenen tutarlılık modeli ihlal ediyor. Bununla birlikte, pratikte tutarlılık tümü veya hiçbiri teklifinde değil; birden çok iyi tanımlanmış tutarlılık model linearizable ve nihai tutarlılık arasında tutarlılık spektrumun boyunca vardır. Azure Cosmos DB'de gerçek dünya senaryoları için uygun olan ve kullanmak için sezgisel birkaç gevşek tutarlılık modelleri tanımlar. Azure Cosmos DB sunarak tutarlılık kullanılabilirlik bileşim gider bir [birden çok rahat henüz iyi tanımlanmış tutarlılık modelleri](consistency-levels.md) ve tek bir bölge veritabanı hesapları ve %99.999 okuma ve yazma bir % 99,99 kullanılabilirlik tüm Tüm bölgeli veritabanı hesapları için kullanılabilirlik. 

### <a id="ConsistencyAndAvailability"></a>Gecikme süresi ile tutarlılık'in ilişki
CAP Teoremi daha kapsamlı bir çeşitlemesi adlı [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf), hangi ayrıca kararlı bir duruma gecikme süresi ve tutarlılık bileşim hesapları. Bu, tutarlılık ve gecikme süresi arasında bir kararlı durumda veritabanı sistem seçmeniz gerektiğini belirtir. (Zaman uyumsuz çoğaltma ve yerel okuma ve yazma çekirdekleri tarafından desteklenir) birden çok gevşek tutarlılık modelleriyle Azure Cosmos DB tüm okuma ve yazma işlemleri için okuma yerel ve bölgeler sırasıyla yazma sağlar. Bu, Azure Cosmos DB verilen tutarlılık modelleri için bölge içinde düşük gecikme süresi garanti sunmanızı sağlar.  

### <a id="ConsistencyAndThroughput"></a>Üretilen iş ile tutarlılık'ın ilişkisi
Belirli tutarlılık modeli uyarlamasını seçimine bağlı olduğundan bir [çekirdek türü](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf), üretilen iş ayrıca göre değişir tutarlılık model seçimi. Örneğin, Azure Cosmos DB'de kesinlikle tutarlı okuma RU ücret kabaca olan *çift* , sonuçta tutarlı okur. Bu durumda, aynı verimliliği elde etmek için çift RUs sağlamanız gerekir.


![Tutarlılık ve üretilen iş arasındaki ilişki](./media/distribute-data-globally/consistency-and-throughput.png)

**İlişki belirli tutarlılık modeli Azure Cosmos veritabanı için okuma kapasitesi**

## <a id="ThroughputGuarantees"></a>Üretilen iş garanti altına alır. 
Azure Cosmos DB ölçek işleme (yanı sıra, depolama), Özellikler esnek bölgeler gereksinimlerini veya isteğe bağlı olarak herhangi bir sayıda arasında sağlar. 

![Azure Cosmos DB dağıtılmış ve Koleksiyonlar bölümlenmiş](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

**Tek bir Azure Cosmos DB kapsayıcısı (arasında bir bölge içinde üç kaynak bölümler) yatay olarak bölümlenmiş ve genel olarak üç Azure bölgeler arasında dağıtılmış**

Bir Azure Cosmos DB kapsayıcısı iki boyutları (i) içinde bir bölge ve (II) bölgeler arasında dağıtılmış. Bunu yapmak için: 

* **Yerel dağıtım**: tek bir bölge içinde bir Azure Cosmos DB kapsayıcısı yatay olarak kullanıma cinsinden ölçeklenir *kaynak bölümler*. Her kaynak bölümü anahtar yönetir ve kesinlikle tutarlıdır ve yüksek oranda kullanılabilir fiziksel olarak dört çoğaltmaları tarafından temsil edilen olarak da adlandırılan bir *çoğaltma kümesi* ve yinelemeler arasında makine çoğaltma durumu. Azure Cosmos DB bir tam kaynak yönetilen bir kaynak bölümü, üretilen iş için ayrılmış sistem kaynaklarını bütçe payını sunmak için sorumlu olduğu sistemidir. Bir Azure Cosmos DB kapsayıcının ölçeklendirme kullanıcılar için saydamdır. Azure Cosmos DB kaynak bölümlerini yönetir ve böler ve bunları depolama alanı olarak gerektiğinde birleştirir ve üretilen iş gereksinimleri değiştirin. 
* **Genel dağıtım**: bölgeli veritabanıysa, her kaynak bölümlerden biri bu bölgeler arasında daha sonra dağıtılır. Anahtarları aynı kümesini çeşitli bölgeler form üzerinde sahip olan kaynak bölümler *bölüm kümesi* (bkz [önceki şekil](#ThroughputGuarantees)).  Bir bölüm kümesi içindeki kaynak bölümler veritabanıyla ilişkili birden çok bölgede durumu makine çoğaltması kullanılarak düzenlenir. Yapılandırılan tutarlılık düzeyi bağlı olarak bir bölüm kümesi içinde kaynak bölümlerden farklı topoloji (örneğin, yıldız, zincir, ağaç vb.) kullanarak dinamik olarak yapılandırılır. 

Bir hızlı yanıt veren bölüm yönetimi, Yük Dengeleme ve katı kaynak İdaresi sayesinde, Azure Cosmos DB özellikler esnek ölçek işleme için bir Azure Cosmos DB kapsayıcı veya veritabanı ile ilişkili birden çok Azure bölgeler arasında sağlar. Sağlanan işleme değiştirme bir çalışma zamanı Azure Cosmos veritabanı bir işlemdir. Benzer diğer veritabanı işlemleri için Azure Cosmos DB sağlanan işleme değiştirme isteğinizi için gecikme süresini mutlak üst sınırı garanti eder. Örneğin, aşağıdaki şekilde talebe göre özellikler esnek sağlanan işleme (1 M - 10 M İsteği/sn iki bölgeler arasında değişen) ile bir müşterinin kapsayıcı gösterir.

![Azure Cosmos DB özellikler esnek işleme sağlanan](./media/distribute-data-globally/elastic-throughput.png)

**Müşteri'nin kapsayıcı (1 M - 10 M İsteği/sn değişen) özellikler esnek sağlanan işleme ile**

### <a id="ThroughputAndConsistency"></a>Tutarlılık işleme'nin ilişki 
Aynı açıklandığı gibi mı [üretilen iş ile tutarlılık'in ilişki](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Kullanılabilirlik ile işleme'nin ilişki
Azure Cosmos DB sağlanan işleme değişiklik yapıldığında, yüksek kullanılabilirliği sürdürmek devam eder. Azure Cosmos DB kaynak bölümler şeffaf bir şekilde yönetir (ve bölme ve birleştirme kopya gerçekleştirdiği işlemleri) ve uygulama özellikler esnek artırır veya verimliliği azaltır sırada operations performans veya kullanılabilirlik, kurtulabilirsiniz değil, sağlar. 

## <a id="AvailabilityGuarantees"></a>Kullanılabilirlik garantilerinden
Azure Cosmos DB tüm tek bölge veritabanı ve tüm bölgeli hesapları için % 99,99 kullanılabilirlik SLA gevşek tutarlılık ve tüm bölgeli veritabanı hesaplarda %99.999 kullanılabilirlik sunar. Daha önce açıklandığı gibi Azure Cosmos veritabanı kullanılabilirlik garantilerinden her veri ve denetim düzlemi işlemleri için gecikme için mutlak bir üst sınır içerir. Kullanılabilirlik garantilerinden bölgeler ve coğrafi uzaklığı bölgeler arasında sayısıyla değiştirmeyin. Kullanılabilirlik garantilerinden göre hem el ile hem de otomatik yük devretme işlemlerini geçerlidir. Azure Cosmos DB uygulamanız mantıksal uç noktalarına karşı çalışabilir ve yeni bir bölgeye bir yük devretme durumunda saydam istekleri yönlendirmek olun saydam çok girişli API'ler sunar. Uygulamanızı bölgesel bir yük devretme ve kullanılabilirlik SLA her zaman korunur durumda dağıtılması gerekmez.

### <a id="AvailabilityAndConsistency"></a>Tutarlılık, gecikme ve verimlilik ile kullanılabilirlik'ın ilişkisi
Tutarlılık, gecikme ve verimlilik ile kullanılabilirlik'in ilişki bölümlerde açıklanan [kullanılabilirlik ile tutarlılık'in ilişki](#ConsistencyAndAvailability), [kullanılabilirlik ile gecikme'nin ilişki](#LatencyAndAvailability) ve [Kullanılabilirlik ile işleme'nin ilişki](#ThroughputAndAvailability). 

## <a id="CustomerFacingSLAMetrics"></a>Müşteri dönük SLA ölçümlerini
Azure Cosmos DB verimlilik, gecikme, tutarlılık ve kullanılabilirlik ölçümleri şeffaf bir şekilde kullanıma sunar. Bu ölçümleri programlamayla ve Azure Portalı aracılığıyla erişilebilir (Aşağıdaki şekle bakın). Azure Application Insights'ı kullanarak çeşitli eşikleri uyarılar ayarlayabilirsiniz.
 

![Azure Cosmos DB müşteri görünür SLA ölçümlerini](./media/distribute-data-globally/customer-slas.png)

**Tutarlılık, gecikme süresi, performans ve kullanılabilirlik ölçümleri her kiracıya saydam olarak kullanılabilir**

## <a id="Next Steps"></a>Sonraki adımlar
* Azure portalını kullanarak Azure Cosmos DB hesabınızdaki genel çoğaltma uygulamak için bkz: [Azure portalını kullanarak Azure Cosmos DB genel veritabanı çoğaltmasını gerçekleştirme](tutorial-global-distribution-sql-api.md).
* Azure Cosmos DB ile birden çok yöneticili mimarileri gerçekleştirme hakkında bilgi edinmek için [Azure Cosmos DB ile birden çok ana veritabanı mimarileri](multi-region-writers.md).
* Azure Cosmos DB'de iş otomatik ve el ile yük devretme hakkında daha fazla bilgi edinmek için [bölgesel yük devretme işlemlerini Azure Cosmos veritabanı](regional-failover.md).

## <a id="References"></a>Başvuruları
1. Eric Brewer. [Doğru sağlam dağıtılmış sistemleri](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Eric Brewer. [CAP üzeri – on iki yıllık kuralları nasıl değiştiğini](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Gilbert, Lynch. - [Brewer&#39;s Conjecture ve tutarlı, kullanılabilir uygulanabilirliğini, bölüm dayanıklı Web Hizmetleri](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. Daniel Abadi. [Veritabanı sistemleri tasarım tutarlılık bileşim Modern içinde dağıtılmış](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. Martin Kleppmann. [Lütfen CP veya AP veritabanları çağırma durdurun](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Peter Bailis et al. [Probabilistic sınırlanmış eskime durumu (PBS) pratik kısmi çekirdekleri için](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor ve Wool. [Yükü, kapasitesi ve çekirdek sistemlerinde kullanılabilirlik](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy ve ki. [Lineralizability: Eşzamanlı nesneler için doğruluk koşul](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. [Azure Cosmos DB SLA'sı](https://azure.microsoft.com/support/legal/sla/cosmos-db/)
