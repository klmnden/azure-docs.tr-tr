---
title: "Azure Cosmos DB genel verilerle dağıtma | Microsoft Docs"
description: "Azure Cosmos DB, genel olarak dağıtılmış, belirleyebiliriz model veritabanı hizmeti genel veritabanlarından kullanarak planet ölçekli coğrafi çoğaltma, yük devretme ve veri kurtarma hakkında bilgi edinin."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: ba5ad0cc-aa1f-4f40-aee9-3364af070725
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: arramac
ms.openlocfilehash: 4427e65930aaeac6335e31dcfe3479baa6fdb6cd
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-distribute-data-globally-with-azure-cosmos-db"></a>Azure Cosmos DB genel verilerle dağıtmak nasıl
Azure bulunabilen - 30 + coğrafi bölgeler arasında genel ayak izini sahiptir ve sürekli genişleyen. Dünya çapında iletişim durumu ile Azure, geliştiricilere sunduğu farklı özellikleri oluşturmak, dağıtmak ve genel olarak dağıtılmış uygulamaları kolayca yönetin olanağı biridir. 

[Azure Cosmos DB](../cosmos-db/introduction.md), Microsoft'un görev açısından kritik uygulamalar için genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB sağlar anahtar teslimi genel dağıtım [üretilen iş ve depolama esnek ölçeklendirme](../cosmos-db/partition-data.md) 99, dünya çapında, tek basamaklı milisaniyelik gecikme [beş iyi tanımlanmış tutarlılık düzeylerini](consistency-levels.md), yüksek oranda kullanılabilirlik, tarafından yedeklenen tüm garanti [endüstri lideri SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB, şema ve dizin yönetimiyle ilgilenmenize gerek kalmadan [otomatik olarak verilerin dizinini oluşturur](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Çok modelli olan bu hizmet belge, anahtar-değer, grafik ve sütunlu veri modellerini destekler. Cloud doğacak bir hizmet olarak Azure Cosmos DB dikkatle sıfırdan çoklu kiracı ve genel dağıtım ile yukarı geliştirilmiştir.

**Tek bir Azure Cosmos DB koleksiyon bölümlenmiş ve birçok Azure bölgesinde dağıtılmış**

![Bölümlenmiş ve üç bölgeler arasında dağıtılmış azure Cosmos DB koleksiyonu](./media/distribute-data-globally/global-apps.png)

Biz Azure Cosmos DB oluşturulurken öğrendiğiniz gibi genel dağıtım ekleme bir sonradan akla olamaz - "cıvatalı"tek site"veritabanı sistem üzerinde açma" olamaz. Geleneksel coğrafi olağanüstü durum kurtarma (DR coğrafi) "tek siteli" veritabanı tarafından sunulan ötesinde genel olarak dağıtılmış bir veritabanı tarafından sunulan yetenekleri span. Tek site veritabanları coğrafi DR özelliği sunan Genel dağıtılmış veritabanlarının katı bir alt kümesidir. 

İle anahtar teslimi genel dağıtım Azure Cosmos veritabanı, geliştiricilerin kendi çoğaltma yapı iskelesi ya da kullanan Lambda deseni oluşturmak zorunda değildir (örneğin, [AWS DynamoDB çoğaltma](https://github.com/awslabs/dynamodb-cross-region-library/blob/master/README.md)) veritabanı günlük üzerinden veya birden çok bölgeler arasında "çift yazma" yaparak. Böyle yaklaşımlar doğruluğunu sağlamak ve ses SLA sağlamak mümkün olduğundan bu yaklaşımlar önermiyoruz. 

Bu makalede, Azure Cosmos veritabanı genel dağıtım özelliklerine genel bakış sağlar. Biz de kapsamlı SLA sağlayan Azure Cosmos DB'ın benzersiz yaklaşımı açıklanmaktadır. 

## <a id="EnableGlobalDistribution"></a>Anahtar teslimi genel dağıtım etkinleştirme
Azure Cosmos DB planet ölçek uygulamaları kolayca yazmanızı etkinleştirmek için aşağıdaki özellikleri sağlar. Bu özellikler Azure Cosmos veritabanı kaynak sağlayıcı tabanlı kullanılabilir [REST API'leri](https://docs.microsoft.com/rest/api/documentdbresourceprovider/) yanı sıra Azure portalı.

### <a id="RegionalPresence"></a>Her yerden bölgesel varlığı 
Azure sürekli olarak artmaktadır coğrafi varlığını getirerek [yeni bölgeler](https://azure.microsoft.com/regions/) çevrimiçi. Azure Cosmos DB tüm yeni Azure bölgeleri varsayılan olarak kullanılabilir. Bu iş için yeni bölge Azure açar açmaz coğrafi bölge Azure Cosmos DB veritabanı hesabınız ile ilişkilendirmenizi sağlar.

### <a id="UnlimitedRegionsPerAccount"></a>Bölgeler sınırsız sayıda Azure Cosmos DB veritabanı hesabınızı ile ilişkilendirme
Azure Cosmos DB Azure bölgeleri herhangi bir sayıda Azure Cosmos DB veritabanı hesabınız ile ilişkilendirmenizi sağlar. Coğrafi yalıtma kısıtlamaları dışında (örneğin, Çin, Almanya), Azure Cosmos DB veritabanı hesabınızla ilişkili olabilir bölgeler sayısının bir sınırlama yoktur. Aşağıdaki şekilde 25 Azure bölgeler arasında span için yapılandırılmış bir veritabanı hesabı gösterilmiştir.  

**Bir kiracının Azure Cosmos DB veritabanı hesabı yayılan 25 Azure bölgeleri**

![25 Azure bölgeleri kapsayıcı azure Cosmos DB veritabanı hesabı](./media/distribute-data-globally/spanning-regions.png)

### <a id="PolicyBasedGeoFencing"></a>İlke tabanlı coğrafi yalıtma
Azure Cosmos DB ilke tabanlı coğrafı yetenekleri sağlamak için tasarlanmıştır. Coğrafi yalıtma veri denetim ve uyumu kısıtlamaları sağlamak için önemli bir bileşenidir ve belirli bir bölgede hesabınızla ilişkilendirdiğiniz engelleyebilir. Coğrafi yalıtma örnekleri arasında (ancak sınırlı değildir), genel dağıtım sovereign Bulutu (örneğin, Çin ve Almanya) içinde veya kamu vergi sınırında (örneğin, Avustralya) bölgelere kapsamı. İlkeleri, Azure aboneliğinizin meta veriler kullanılarak denetlenir.

### <a id="DynamicallyAddRegions"></a>Dinamik olarak ekleme ve bölgeler kaldırma
Azure Cosmos DB (ilişkilendirme) eklemek veya kaldırmak verir (ilişkilendirmesini) veritabanı hesabınızı, herhangi bir noktada bölgelere (bkz [önceki şekil](#UnlimitedRegionsPerAccount)). Paralel bölümleri arasında veri çoğaltmak Azure Cosmos DB yeni bir bölge çevrimiçi olduğunda, Azure Cosmos DB 30 dakika içinde herhangi bir yere dünyanın en fazla 100 TBs için kullanılabilir olmasını sağlar. 

### <a id="FailoverPriorities"></a>Yük devretme öncelikler
Azure Cosmos DB etkinleştirir çok bölgesel bir kesintinin bölgesel yük devretme işlemlerini tam dizisini denetlemek için çok çeşitli bölgelere veritabanıyla ilişkili öncelik ilişkilendirmek için (Aşağıdaki şekil bakın) hesap. Azure Cosmos DB otomatik yük devretme sırası belirttiğiniz öncelik sırasına göre oluşmamasını sağlar. Bölgesel yük devretme hakkında daha fazla bilgi için bkz: [Azure Cosmos veritabanı iş sürekliliği için otomatik bölgesel yük devretme işlemlerini](regional-failover.md).

**Bir kiracı Azure Cosmos DB'nin veritabanı hesabıyla ilişkilendirilmiş bölgeler için yük devretme öncelik sırasına (sağ bölme) yapılandırabilirsiniz**

![Yük devretme öncelikleri Azure Cosmos DB ile yapılandırma](./media/distribute-data-globally/failover-priorities.png)

### <a id="ConsistencyLevels"></a>Genel olarak çoğaltılmış veritabanları için birden çok iyi tanımlanmış tutarlılık modelleri
Azure Cosmos DB sunan [birden çok iyi tanımlanmış tutarlılık düzeylerini](consistency-levels.md) SLA ile yedeklenir. (Listeden kullanılabilir seçeneklerden) belirli tutarlılık modeline iş yükü/senaryoları bağlı olarak seçebilirsiniz. 

### <a id="TunableConsistency"></a>Genel olarak çoğaltılmış veritabanları için ince ayarlanabilir tutarlılık
Azure Cosmos DB programlı olarak geçersiz kılın ve çalışma zamanında bir istek başına varsayılan tutarlılık seçeneği hafifletin izin verir. 

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Dinamik olarak yapılandırılabilir okuma ve yazma bölgeleri
Azure Cosmos DB, "Okuma", "yazma" veya "okuma/yazma" bölgeleri için (veritabanıyla ilişkili) bölgeleri yapılandırmanıza olanak sağlar. 

### <a id="ElasticallyScaleThroughput"></a>Azure bölgeler arasında özellikler esnek ölçeklendirme işleme
Özellikler esnek bir Azure Cosmos DB koleksiyonu tarafından sağlama verimlilik programlı olarak ölçeklendirebilirsiniz. Üretilen iş koleksiyonu içinde dağıtılmış tüm bölgelere uygulanır.

### <a id="GeoLocalReadsAndWrites"></a>Coğrafi yerel okuma ve yazma işlemleri
Düşük gecikme süresi verilere erişim olanağı dünyanın sunmak için temel avantaj genel olarak dağıtılmış bir veritabanı olmalıdır. Azure Cosmos DB çeşitli veritabanı işlemleri için P99 en düşük gecikme süresi garanti sunar. Tüm okuma en yakın yerel okuma bölgesine yönlendirilir sağlar. Okuma isteği hizmet için çekirdek okuma verildiği bölgesine yerel kullanılır; aynı yazma işlemleri için geçerlidir. Yalnızca çoğaltmaların çoğunluğu işlemi yazma yerel olarak ancak yazmaları onaylamak için uzaktan çoğaltmaları geçişli olmadan tamamlandığı sonra yazma onaylanır. Farklı put, Azure Cosmos DB çoğaltma protokolü isteği verildiği okuma ve yazma çekirdekleri her zaman için okuma yerel ve bölgeler, sırasıyla yazma varsayımına altında çalışır.

### <a id="ManualFailover"></a>Bölgesel yük devretme el ile başlatın
Azure Cosmos DB veritabanı hesabı doğrulamak için yük devretme tetiklemek verir *uçtan uca* kullanılabilirlik özelliklerine (veritabanı) dışındaki tüm uygulama. Hata algılama ve öncü seçim hem güvenlik hem de liveness özelliklerini garanti edildiğinden, Azure Cosmos DB garanti *sıfır veri kaybı* Kiracı tarafından başlatılan el ile yük devretme işlemi için.

### <a id="AutomaticFailover"></a>Otomatik Yük devretme
Azure Cosmos DB bir veya daha fazla bölgesel kesintiler durumunda otomatik yük devretme destekler. Bölgesel bir yük devretme sırasında Azure Cosmos DB okuma gecikmesi, çalışma süresini kullanılabilirlik, tutarlılık ve verimlilik SLA tutar. Azure Cosmos DB tamamlamak için bir otomatik yük devretme işlemi süresince bir üst sınır sağlar. Bu olası veri kaybı sırasında bölgesel kesinti penceredir.

### <a id="GranularFailover"></a>Farklı bir yük devretme ayrıntı düzeyi için tasarlanmış
Şu anda otomatik ve el ile yük devretme yetenekleri veritabanı hesabı bazda sunulur. Not, dahili olarak Azure Cosmos DB sunmak üzere tasarlanmıştır *otomatik* yük devretme işleminde daha hassas ayrıntı düzeyi bir veritabanı, koleksiyon veya hatta bir bölümünü (anahtarları çeşitli sahip olan bir koleksiyon). 

### <a id="MultiHomingAPIs"></a>Azure Cosmos DB çok girişli API'leri
Azure Cosmos DB kullanarak veritabanıyla etkileşim olanak tanır mantıksal (bölge belirsiz) ya da fiziksel (bölgeye özgü) uç noktaları. Mantıksal uç noktalarını kullanarak çok konaklı yük devretme durumunda, uygulama saydam olabilir sağlar. İkincisi, fiziksel uç noktaları yeniden yönlendirmek için uygulamaya ayrıntılı denetim okur ve belirli bölgelerine Yazar sağlar.

İçin okuma tercihlerinin nasıl yapılandırılacağı hakkında bilgi bulabilirsiniz [SQL API](../cosmos-db/tutorial-global-distribution-documentdb.md), [grafik API'si](../cosmos-db/tutorial-global-distribution-graph.md), [tablo API](../cosmos-db/tutorial-global-distribution-table.md), ve [MongoDB API](../cosmos-db/tutorial-global-distribution-mongodb.md) içinde kendi ilgili bağlantılı makaleler.

### <a id="TransparentSchemaMigration"></a>Saydam ve tutarlı veritabanı şeması ve dizin geçirme 
Azure Cosmos DB olan tam olarak [şema belirsiz](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Veritabanı Altyapısı'nın benzersiz tasarım otomatik olarak izin verir ve zaman uyumlu olarak herhangi bir şemayı ya da ikincil dizinlerin sizden gerek kalmadan alır verilerin tümünü dizin. Bu, veritabanı şeması ve dizin geçiş hakkında kaygı veya şema değişiklikleri çok aşaması piyasaya sürülmeleri Eşgüdümleme Genel dağıtılmış uygulamanızı hızlı bir şekilde yinelemek sağlar. Azure Cosmos DB açıkça sizin tarafınızdan yapılan ilkelere dizin yapılan herhangi bir değişiklik neden olmaz, performans veya kullanılabilirlik düşüşü güvence altına alır.  

### <a id="ComprehensiveSLAs"></a>Kapsamlı SLA (ötesinde yalnızca yüksek oranda kullanılabilir)
Genel olarak dağıtılmış veritabanı hizmet olarak Azure Cosmos DB iyi tanımlanmış SLA sunar **veri kaybı**, **kullanılabilirlik**, **P99 gecikme süresi**, **işleme** ve **tutarlılık** veritabanıyla ilişkili bölgeler sayısından bağımsız olarak bir bütün olarak veritabanı için.  

## <a id="LatencyGuarantees"></a>Gecikme garanti
Verilerinize dünyanın düşük gecikmeli erişim sunmak için Azure Cosmos DB gibi genel dağıtılmış veritabanı hizmetinin temel avantaj olmalıdır. Azure Cosmos DB garantili düşük gecikme süresi P99 çeşitli veritabanı işlemleri sunar. Azure Cosmos DB kullanan çoğaltma Protokolü veritabanı işlemleri sağlar (İdeal olarak, hem okuyan ve yazan) her zaman, istemcinin yerel bölgede gerçekleştirilir. Azure Cosmos DB SLA gecikme P99 okuma, (zaman uyumlu olarak) dizinli yazma ve çeşitli istek ve yanıt boyutları için sorgular içerir. Yazma işlemleri için gecikme garanti dayanıklı çoğunluğu çekirdek yürütme yerel veri merkezinde bulunan içerir.

### <a id="LatencyAndConsistency"></a>Gecikme süresi'nin ilişki tutarlılık 
Zaman uyumlu olarak yazar çoğaltmak gereken genel dağıtılmış kurulumunda güçlü tutarlılık sağlamaya Genel dağıtılmış hizmet için ya da zaman uyumlu açık hızına ve geniş alan ağı güvenilirlik bu strong dikte çapraz bölge okuma – gerçekleştirin Tutarlılık daha yüksek gecikme süreleri ve azaltılmış kullanılabilirlik veritabanı işlemi ile sonuçlanır. Bu nedenle, sunmak için P99 en düşük gecikme ve tüm tek bölge ve tüm bölgeli hesapları için % 99,99 kullanılabilirlik gevşek tutarlılığı garanti ve kullanılabilirlik tüm bölgeli veritabanı hesaplarda %99.999 okuma, hizmet uygulamadığınız gerekir zaman uyumsuz çoğaltma. Hizmet ayrıca sağlaması gerekir bu içinde dönüş gerektiren [iyi tanımlanmış, gevşek tutarlılık choice(s)](consistency-levels.md) – daha güçlü (düşük gecikme süresi ve kullanılabilirliğini garanti sunmak için) ve (sezgisel bir programlama modeli sunmak için) "son" tutarlılık ideal güçlü zayıf.

Azure Cosmos DB okuma işlemi çoğaltmaları belirli tutarlılık düzeyi garantisi sunmak için birden çok bölgeler arasında iletişim için gerekli olmayan sağlar. Benzer şekilde, (yani yazma zaman uyumsuz olarak bölgeler arasında çoğaltılır) tüm bölgeler arasında veri çoğaltılırken bir yazma işlemi engellediği değil sağlar. Birden çok gevşek tutarlılık düzeylerini bölgeli veritabanı hesapları için kullanılabilir. 

### <a id="LatencyAndAvailability"></a>Kullanılabilirlik ile gecikme'nın ilişkisi 
Gecikme süresi ve kullanılabilirliği aynı para iki tarafında vardır. Gecikme kararlı durum ve hataları karşısında kullanılabilirlik işleminin hakkında konuşun. Uygulama açısından bir yavaş çalışan veritabanı kullanılamıyor bir veritabanından ayırt işlemdir. 

Yüksek gecikme kullanılamazlık ayırt etmek için çeşitli veritabanı işlemlerini gecikme üzerinde mutlak bir üst sınır Azure Cosmos DB sağlar. Veritabanı işlemi tamamlamak için üst sınırdan daha uzun sürerse, Azure Cosmos DB bir zaman aşımı hatası döndürür. Zaman aşımları kullanılabilirlik SLA'sı karşı sayılır Azure Cosmos DB kullanılabilirlik SLA'sı sağlar. 

### <a id="LatencyAndThroughput"></a>Üretilen iş ile gecikme'nın ilişkisi
Azure Cosmos DB, gecikme ve verimlilik arasında seçim yapmaz. Her iki gecikme P99 ve teslim sağlanan verimlilik SLA geliştirir. 

## <a id="ConsistencyGuarantees"></a>Tutarlılık
Sırada [güçlü tutarlılık modeli](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) altın standart programlama, onu (kararlı durumda) daha yüksek gecikme hızı yüksek fiyatına gelir ve kullanılabilirlik (karşısında hataları) azalır. 

Azure Cosmos DB iyi tanımlanmış bir programlama modeli, çoğaltılmış verilerinin tutarlılık hakkında nedeni sunar. Çok konaklı uygulamalar oluşturmanıza olanak etkinleştirmek için Azure Cosmos DB tarafından kullanıma sunulan tutarlılık modelleri bölge belirsiz ve okuma ve yazma işlemleri Burada sunulan gelen bölge bağlı olmayan şekilde tasarlanmıştır. 

Azure Cosmos veritabanı tutarlılık SLA % 100'okuma isteklerinin tutarlılığı garanti size (varsayılan tutarlılık düzeyi veritabanı hesabındaki) veya istek üzerine geçersiz kılınan değeri tarafından istenen tutarlılık düzeyi karşıladığını güvence altına alır. Okuma isteği tutarlılık düzeyi ile ilişkili tüm tutarlılığı garanti sağlanırsa tutarlılık SLA karşıladığınızı kabul edilir. Aşağıdaki tabloda Azure Cosmos DB tarafından sunulan belirli tutarlılık düzeylerini karşılık tutarlılığı garanti yakalar.

**Verilen tutarlılık düzeyi Azure Cosmos veritabanı ile ilişkili tutarlılığı garanti**

<table>
    <tr>
        <td><strong>Tutarlılık düzeyi</strong></td>
        <td><strong>Tutarlılık özellikleri</strong></td>
        <td><strong>SLA</strong></td>
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
        <td>Tutarlı öneki</td>
        <td>Tutarlı öneki</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Güçlü</td>
        <td>Linearizable</td>
        <td>100%</td>
    </tr>
</table>

### <a id="ConsistencyAndAvailability"></a>Kullanılabilirlik ile tutarlılık'ın ilişkisi
[İmpossibility sonuç](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) , [CAP Teoremi](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) kullanılabilir durumda kalır ve linearizable tutarlılık hataları karşısında sunmak sistem gerçekten olanaksız olduğunu kanıtlar. Veritabanı hizmeti CP veya AP seçmelisiniz - TP sistemleri atlayabilirsiniz linearizable tutarlılık lehinde kullanılabilirlik AP sistemleri atlayabilirsiniz sırada [linearizable tutarlılık](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) kullanılabilirlik lehinde. Azure Cosmos DB hiçbir zaman bir CP sistem resmi olarak kolaylaştırır istenen tutarlılık düzeyi ihlal ediyor. Bununla birlikte, pratikte tutarlılık tümü veya hiçbiri teklifinde değil – linearizable ve nihai tutarlılık arasında tutarlılık spektrumun boyunca birden çok iyi tanımlanmış tutarlılık model vardır. Azure Cosmos DB'de biz birkaç gevşek tutarlılık modellerin gerçek dünya Uygulanabilirlik ve sezgisel bir programlama modeli tanımlamak çalıştınız. Azure Cosmos DB sunarak tutarlılık kullanılabilirlik bileşim gider bir [birden çok rahat henüz iyi tanımlanmış tutarlılık düzeylerini](consistency-levels.md) ve tüm tek bölge ve tüm bölgeli hesapları ile için % 99,99 kullanılabilirlik Tutarlılık rahat ve kullanılabilirlik tüm bölgeli veritabanı hesaplarda %99.999 okuyun. 

### <a id="ConsistencyAndAvailability"></a>Gecikme süresi ile tutarlılık'in ilişki
Daha kapsamlı bir değişim ucunun Prof. Daniel Abadi tarafından önerilen ve çağrılır [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf), hangi de gecikme süresi ve tutarlılık bileşim kararlı durumda hesapları. Bu, tutarlılık ve gecikme süresi arasında kararlı durumda veritabanı sistem seçmeniz gerektiğini belirtir. (Zaman uyumsuz çoğaltma ve yerel okuma, yazma çekirdekleri tarafından desteklenir) birden çok gevşek tutarlılık modelleriyle Azure Cosmos DB tüm okuma ve yazma işlemleri için okuma yerel ve bölgeler sırasıyla yazma sağlar.  Bu, Azure Cosmos DB tutarlılık düzeyleri için bölge içinde düşük gecikme süresi garanti sunmanızı sağlar.  

### <a id="ConsistencyAndThroughput"></a>Üretilen iş ile tutarlılık'ın ilişkisi
Belirli tutarlılık modeli uyarlamasını seçimine bağlı olduğundan bir [çekirdek türü](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf), üretilen işi de tutarlılık seçimine göre değişir. Örneğin, Azure Cosmos DB'de kesinlikle tutarlı okuma RU ücret, sonuçta tutarlı okuduğu kabaca çift olabilir. Bu durumda aynı verimliliği elde etmek için çift RUs koleksiyonunuzu üzerinde sağlamanız gerekir.
 
**İlişki belirli tutarlılık düzeyi Azure Cosmos veritabanı için okuma kapasitesi**

![Tutarlılık ve üretilen iş arasındaki ilişki](./media/distribute-data-globally/consistency-and-throughput.png)

## <a id="ThroughputGuarantees"></a>Üretilen iş garanti altına alır. 
Azure Cosmos DB ölçek işleme (yanı sıra, depolama), Özellikler esnek isteğe bağlı olarak farklı bölgelere sağlar. 

**(Üç parça) bölümlenmiş ve üç Azure bölgeler arasında dağıtılan bir tek bir Azure Cosmos DB koleksiyonu**

![Azure Cosmos DB dağıtılmış ve Koleksiyonlar bölümlenmiş](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

Bir Azure Cosmos DB koleksiyonu bir bölge içinde ve ardından bölgeler arasında iki boyutu – kullanarak dağıtılmış. Bunu yapmak için: 

* Tek bir bölge içinde kaynak bölümler bakımından çıkışı bir Azure Cosmos DB koleksiyonu ölçeklendirilir. Her kaynak bölümü anahtar yönetir ve kesinlikle tutarlı ve çoğaltmaları kümesi arasında durumu makine çoğaltma, yüksek oranda kullanılabilir. Azure Cosmos DB bir tam olarak yönetilen kaynak bir kaynak bölümü, üretilen iş için ayrılmış sistem kaynaklarını bütçe payını teslim etmek için sorumlu olduğu sistemidir. Bir Azure Cosmos DB koleksiyonunu ölçeklendirme tamamen saydamdır – Azure Cosmos DB kaynak bölümlerini yönetir ve böler ve gerektiğinde birleştirir. 
* Her kaynak bölümlerinin daha sonra birçok bölgeye dağıtılır. Anahtarları aynı kümesini çeşitli bölgelerdeki sahip olan kaynak bölümler form bölüm kümesi (bkz [önceki şekil](#ThroughputGuarantees)).  Kaynak bölümler bir bölüm kümesi içinde birden çok bölgeler arasında durumu makine çoğaltması kullanılarak düzenlenir. Yapılandırılan tutarlılık düzeyi bağlı olarak bir bölüm kümesi içinde kaynak bölümlerden farklı topoloji (örneğin, yıldız, zincir, ağaç vb.) kullanarak dinamik olarak yapılandırılır. 

Bir hızlı yanıt veren bölüm yönetimi, Yük Dengeleme ve katı kaynak İdaresi sayesinde, Azure Cosmos DB özellikler esnek ölçek işleme için bir Azure Cosmos DB koleksiyonunda birden çok Azure bölgeler arasında sağlar. Diğer veritabanı işlemleriyle Azure Cosmos DB işleme değiştirme isteğinizi için gecikme süresini mutlak üst sınırı garanti gibi bir koleksiyon üzerinde değişen üretilen işi Azure Cosmos veritabanı - çalışma zamanı işlemdir. Örnek olarak, aşağıdaki şekilde talebe göre özellikler esnek sağlanan işleme (1 M - 10 M İsteği/sn iki bölgeler arasında değişen) ile bir müşterinin koleksiyonda gösterir.
 
**Müşteri'nin koleksiyonuyla özellikler esnek sağlanan işleme (1M - 10M İsteği/sn)**

![Azure Cosmos DB özellikler esnek işleme sağlanan](./media/distribute-data-globally/elastic-throughput.png)

### <a id="ThroughputAndConsistency"></a>Tutarlılık işleme'nin ilişki 
Aynı [üretilen iş ile tutarlılık'in ilişki](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Kullanılabilirlik ile işleme'nin ilişki
Azure Cosmos DB işleme değişiklik yapıldığında, kullanılabilirliği sürdürmek devam eder. Azure Cosmos DB bölümleri (örneğin, bölme, birleştirme, kopyalama işlemleri) şeffaf bir şekilde yönetir ve uygulama özellikler esnek artırır veya verimliliği azaltır sırada operations performans veya kullanılabilirlik, kurtulabilirsiniz değil, sağlar. 

## <a id="AvailabilityGuarantees"></a>Kullanılabilirlik garantilerinden
Azure Cosmos DB tüm tek bölge ve tüm bölgeli hesapları için % 99,99 kullanılabilirlik SLA'sı ile gevşek tutarlılık sunar ve tüm bölgeli veritabanı hesaplarda kullanılabilirlik %99.999 okuyun. Daha önce açıklandığı gibi Azure Cosmos veritabanı kullanılabilirlik garantilerinden her veri ve denetim düzlemi işlemleri için gecikme süresini mutlak bir üst sınır içerir. Kullanılabilirlik garantilerinden steadfast ve bölgeler ve coğrafi uzaklığı bölgeler arasında sayısıyla değiştirmeyin. Kullanılabilirlik garantilerinden hem el ile yanı sıra, otomatik yük devretme kümelemesiyle uygulayın. Azure Cosmos DB uygulamanızı mantıksal uç noktalarına karşı çalışabilir saydam yük devretme durumunda yeni bölge isteklerini yönlendirebilirsiniz emin olun ve saydam çok girişli API'ler sunar. Farklı put, uygulamanızın üzerinde bölgesel yük devretme dağıtılması gerekmez ve kullanılabilirlik SLA'ları korunur.

### <a id="AvailabilityAndConsistency"></a>Tutarlılık, gecikme ve verimlilik ile kullanılabilirlik'ın ilişkisi
Kullanılabilirlik'in ilişki tutarlılık, gecikme ve verimlilik ile açıklanan [kullanılabilirlik ile tutarlılık'in ilişki](#ConsistencyAndAvailability), [kullanılabilirlik ile gecikme'nin ilişki](#LatencyAndAvailability) ve [kullanılabilirlik ile işleme'nin ilişki](#ThroughputAndAvailability). 

## <a id="GuaranteesAgainstDataLoss"></a>Garanti ve "veri kaybı" Sistem davranışı
Azure Cosmos DB'de (koleksiyonunun) her bölüm en az 10-20 hata etki alanlarında yayılır çoğaltmaları sayısına göre yüksek oranda kullanılabilir hale getirilir. Bunlar istemci için Onaylandı önce tüm yazma işlemlerini çoğaltmaların çoğunluğu çekirdek tarafından eş zamanlı olarak ve işlemi çalışıyoruz. Zaman uyumsuz çoğaltma ile birlikte, birçok bölgeye yayılan bölümleri arasında uygulanır. Azure Cosmos DB bir kiracı tarafından başlatılan el ile yük devretme için hiçbir veri kaybı olduğunu güvence altına alır. Otomatik Yük devretme sırasında Azure Cosmos DB ve veri kaybı penceresini SLA'sını bir parçası olarak yapılandırılmış sınırlanmış eskime durumu aralığının bir üst sınır güvence altına alır.

## <a id="CustomerFacingSLAMetrics"></a>Müşteri dönük SLA ölçümlerini
Azure Cosmos DB verimlilik, gecikme, tutarlılık ve kullanılabilirlik ölçümleri şeffaf bir şekilde kullanıma sunar. Bu ölçümleri programlamayla ve Azure portalı (aşağıdaki şekilde bakın) üzerinden erişilebilir. Azure Application Insights'ı kullanarak çeşitli eşikleri uyarılar ayarlayabilirsiniz.
 
**Tutarlılık, gecikme süresi, performans ve kullanılabilirlik ölçümleri her kiracıya saydam olarak kullanılabilir**

![Azure Cosmos DB müşteri görünür SLA ölçümlerini](./media/distribute-data-globally/customer-slas.png)

## <a id="Next Steps"></a>Sonraki adımlar
* Azure portalını kullanarak Azure Cosmos DB hesabınızdaki genel çoğaltma uygulamak için bkz: [Azure portalını kullanarak Azure Cosmos DB genel veritabanı çoğaltmasını gerçekleştirme](tutorial-global-distribution-documentdb.md).
* Azure Cosmos DB ile birden çok yöneticili mimarileri gerçekleştirme hakkında bilgi edinmek için [Azure Cosmos DB ile birden çok ana veritabanı mimarileri](multi-region-writers.md).
* Azure Cosmos DB'de iş otomatik ve el ile yük devretme hakkında daha fazla bilgi edinmek için [bölgesel yük devretme işlemlerini Azure Cosmos veritabanı](regional-failover.md).

## <a id="References"></a>Başvuruları
1. Eric Brewer. [Doğru sağlam dağıtılmış sistemleri](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Eric Brewer. [CAP üzeri – on iki yıllık kuralları nasıl değiştiğini](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Gilbert, Lynch. - [Brewer &#39; s Conjecture ve tutarlı, kullanılabilir uygulanabilirliğini, bölüm dayanıklı Web Hizmetleri](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. Daniel Abadi. [Veritabanı sistemleri tasarım tutarlılık bileşim Modern içinde dağıtılmış](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. Martin Kleppmann. [Lütfen CP veya AP veritabanları çağırma durdurun](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Peter Bailis et al. [Probabilistic sınırlanmış eskime durumu (PBS) pratik kısmi çekirdekleri için](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor ve Wool. [Yükü, kapasitesi ve çekirdek sistemlerinde kullanılabilirlik](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy ve ki. [Lineralizability: Eşzamanlı nesneler için doğruluk koşul](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. [Azure Cosmos DB SLA'sı](https://azure.microsoft.com/support/legal/sla/cosmos-db/)
