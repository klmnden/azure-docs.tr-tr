---
title: Azure Cosmos DB anahtar teslim küresel dağıtım nasıl sağladığını | Microsoft Docs
description: Bir Global olarak dağıtılmış çok modelli veritabanı hizmeti olan Azure Cosmos DB genel veritabanlarından kullanarak çok büyük ölçekli coğrafi çoğaltma, çok yöneticili, yük devretme ve veri kurtarma hakkında bilgi edinin.
services: cosmos-db
author: markjbrown
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.openlocfilehash: 1aa0fa3d4d9e1821a6980d9334456a78eba7bfbc
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956923"
---
# <a name="how-azure-cosmos-db-enables-turnkey-global-distribution"></a>Azure Cosmos DB anahtar teslim küresel dağıtım nasıl sağlar?
Azure Cosmos DB, kolayca dünya çapında dağıtılan uygulamalar yazma olanağı sağlamak için aşağıdaki özellikleri sağlar. Bu özellikler Azure Cosmos DB kaynağı aracılığıyla sağlayıcı tabanlı kullanılabilir [REST API'leri](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/) Azure portalında yanı sıra.

## <a id="GlobalDistribution"></a>Genel dağıtım

### <a id="RegionalPresence"></a>Her yerde bulunan bölgesel varlığı 
Azure sürekli giderek kendi coğrafi durum taşıyarak [yeni bölgeler](https://azure.microsoft.com/regions/) çevrimiçi. Azure Cosmos DB olarak sınıflandırılmış bir *temel hizmet* azure'da ve varsayılan olarak tüm yeni Azure bölgelerinde kullanılabilir. Bu iş için yeni bölge Azure açar açmaz coğrafi bölge Azure Cosmos DB veritabanı hesabınız ile ilişkilendirmenizi sağlar.

### <a id="MultiMasterSupport"></a>Çok yöneticili desteği
Azure Cosmos DB, eşit bir herhangi bir yazma modeli içinde yer alan birden çok ana bölgeleri için yerel, sunucu tarafı desteği sağlar. Bu destek sağlar < 10 ms yazma gecikme süresi ve % 99,999 kullanılabilirlik tarafından yazma [SLA'mali olarak desteklenen](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Çok yöneticili, tüm API dahil olmak üzere için kullanılabilir [SQL](sql-api-introduction.md), [MongoDB](mongodb-introduction.md), [Cassandra](cassandra-introduction.md), [grafik](graph-introduction.md), ve [tablo](table-introduction.md) ve Cosmos DB için tüm SDK diller. Azure Cosmos DB, 4 farklı tutarlılık düzeylerini destekler (sınırlanmış eskime durumu, oturum, tutarlı ön ek ve nihai) birden çok yönetim özelliğine sahip hesaplar için.

### <a id="UnlimitedRegionsPerAccount"></a>Sınırsız sayıda bölgede Azure Cosmos DB veritabanı hesabınız ile ilişkilendirme
Azure Cosmos DB, dilediğiniz sayıda Azure bölgesinde Azure Cosmos DB veritabanı hesabınız ile ilişkilendirmenizi sağlar. Şirketin coğrafı kısıtlamalarının dışında (örneğin, Çin, Almanya), Azure Cosmos DB veritabanı hesabıyla ilişkili bölge sayısı ile ilgili bir sınırlama vardır. 25 Azure bölgeleri arasında yaymasına izin yapılandırılmış bir veritabanı hesabı aşağıdaki şekilde gösterilmiştir:

![25 Azure bölgesini kapsayan, azure Cosmos DB veritabanı hesabı](./media/distribute-data-globally-turnkey/spanning-regions.png)


### <a id="PolicyBasedGeoFencing"></a>İlke tabanlı şirketin coğrafı
Azure Cosmos DB, ilke tabanlı şirketin coğrafı desteklemek için tasarlanmıştır. Şirketin coğrafı veri yönetimini ve uyumluluğunu kısıtlamaları sağlamak için önemli bir bileşendir ve belirli bir bölgeye hesabınızla ilişkilendirmek engelliyor olabilir. Şirketin coğrafı örnekleri dahil (ancak bunlarla sınırlı olmayan) bir kamu vergilendirme sınırı (örneğin, Avustralya) veya bağımsız bir bulut (örneğin, Çin ve Almanya) içinde bölgelere genel dağıtım kapsamı. İlkeler, Azure aboneliğinizin meta veriler kullanılarak denetlenir.

### <a id="DynamicallyAddRegions"></a>Bölgeleri dinamik olarak ekleyip
Azure Cosmos DB (ilişkilendirme) ekleme veya kaldırma olanak tanır (ilişkisini) veritabanı hesabınız, herhangi bir noktada bölgelerden (bkz [önceki şekilde](#UnlimitedRegionsPerAccount)). Verileri bölümler arasında paralel çoğaltılmasını sayesinde, Azure Cosmos DB yeni bir bölge eklediğinizde, bu, 30 dakika içinde herhangi bir dünyada işlemleri için uygun olmasını sağlar (100 TB'a olduğu varsayılarak verilerinizi veya daha az). 

### <a id="FailoverPriorities"></a>Yük devretme önceliklerini
Çok yöneticili kullanılmadığında, müşterilerinin kesinti durumlarda bölgesel yük devretme tam bir dizi kontrol, Azure Cosmos DB ilişkilendirmenizi sağlar bir *öncelik* veritabanı hesabıyla ilişkili çeşitli bölge ile (bkz: Aşağıdaki şekilde). Azure Cosmos DB, otomatik yük devretme sırasını belirttiğiniz öncelik sırasına göre gerçekleştirilmesini sağlar. Bölgesel yük devretme hakkında daha fazla bilgi için bkz: [iş sürekliliği için Azure Cosmos DB, otomatik bölgesel yük devretme](regional-failover.md). Aşağıdaki resimde, bir veritabanı hesabıyla ilişkili bölge için bir kiracıdaki Azure Cosmos DB yük devretme öncelik sırası (sağ bölme) nasıl yapılandıracağınız gösterilmektedir:

![Azure Cosmos DB ile yük devretme önceliklerini yapılandırma](./media/distribute-data-globally-turnkey/failover-priorities.png)

### <a id="ConsistencyLevels"></a>Global olarak dağıtılmış veritabanları için birden çok, iyi tanımlanmış tutarlılık modeli
Azure Cosmos DB destekler [birden çok sayıda iyi tanımlanmış, sezgisel ve pratik tutarlılık modeli](consistency-levels.md) SLA'lar ile desteklenen. İş yükü/senaryoları bağlı olarak bir belirli bir tutarlılık modelini (listeden kullanılabilir seçeneklerden) seçebilirsiniz.

### <a id="TunableConsistency"></a>Genel olarak çoğaltılan veritabanları için ayarlanabilir tutarlılık
Azure Cosmos DB, programlı olarak geçersiz kılın ve çalışma zamanında istek başına varsayılan tutarlılık seçeneği hafifletin sağlar.

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Dinamik olarak yapılandırılabilir okuma ve yazma bölgeleri
Azure Cosmos DB, "Okuma", "yazma" veya "okuma/yazma" bölgeleri için (veritabanıyla ilişkili) bölgeleri yapılandırmanıza olanak sağlar.

### <a id="ElasticallyScaleThroughput"></a>Esnek bir şekilde Azure bölgeleri arasında aktarım hızını ölçeklendirme
Esnek bir Azure Cosmos DB kapsayıcısı tarafından sağlama aktarım hızı programlı olarak ölçeklendirebilirsiniz. Aktarım hızı, Azure Cosmos DB kapsayıcısı içinde dağıtılmış tüm bölgeler için uygulanır.

### <a id="GeoLocalReadsAndWrites"></a>Coğrafi yerel okuma ve yazma işlemleri
Global olarak dağıtılmış bir veritabanının en önemli avantajı, verilere dünyanın her yerinden düşük gecikme süreli erişim sunar ' dir. Azure Cosmos DB, düşük gecikme süresi sağlar (< 10 milisaniye) okur ve 99. yüzdebirlik dilimde dünya çapında yazar. Tüm okuma (yerel) en yakın bölgeden sunulur sağlar. Okuma isteği hizmet için çekirdek okuma verildiği bölgesine yerel kullanılır. Aynı yazma işlemleri için geçerlidir. Bir yazma dizinlendiğini her yalnızca çoğaltmalar çoğunu sonra yazma yerel olarak erişilebiliyor ancak yazma onaylamak için Uzak çoğaltmalarda Geçitli olmadan onaylanır. Farklı bir şekilde geçirmek için Azure Cosmos DB çoğaltma Protokolü okuma ve yazma çekirdekleri her zaman burada isteği verildi bölgesine yerel varsayım altında çalışır.

### <a id="ManualFailover"></a>El ile yük devretme
Azure Cosmos DB veritabanı hesabının doğrulamak için yük devretmeyi tetiklemek verir *uçtan uca* kullanılabilirlik özelliklerini (dışında veritabanı) tüm uygulama. Azure Cosmos DB hem güvenliği hem de hata algılama ve öncü seçimi canlılık özelliklerini garanti edildiğinden garanti *sıfır veri kaybı* Kiracı tarafından başlatılan el ile yük devretme işlemi için.

### <a id="AutomaticFailover"></a>Otomatik Yük devretme
Azure Cosmos DB, bir veya daha fazla bölgesel kesintiler sırasında otomatik yük devretmeyi destekler. Bölgesel yük devretme sırasında Azure Cosmos DB, okuma gecikme süresi, çalışma süresi kullanılabilirlik, tutarlılık ve aktarım hızı SLA'lar tutar. Azure Cosmos DB, bir üst sınır tamamlamak için bir otomatik yük devretme işlemi süresince sağlar. Bu olası veri kaybı penceresini bölgesel bir kesinti sırasında dir.

### <a id="GranularFailover"></a>Farklı yük devretme ayrıntı düzeyi için tasarlanmış
Şu anda otomatik ve el ile yük devretme özellikleri, veritabanı hesabının ayrıntı düzeyinde kullanıma sunulur. Not, dahili olarak Azure Cosmos DB sunmak üzere tasarlanan *otomatik* yük devretme daha iyi tanecikli bir veritabanı, bir kapsayıcı veya hatta bir bölümünü (anahtarları bir dizi sahip olan bir kapsayıcı). 

### <a id="MultiHomingAPIs"></a>Azure cosmos DB'de birden çok giriş

Burada bir yazma bölgesine sahip ve birden çok okuma-çoğaltma, Azure Cosmos DB bir veritabanıyla etkileşim kurmanıza imkan tanır tek yöneticili senaryosunda mantıksal (bölge geçişte sorun yaşamaz) ya da fiziksel (bölgeye özel) uç noktaları kullanma. Mantıksal uç noktalarını kullanarak uygulama birden çok girişli bir yük devretme durumunda şeffaf bir şekilde olabileceğini sağlar. İkincisi, fiziksel bir uç nokta okumaları için uygulamaya ayrıntılı denetim sağlar ve belirli bölgelere yazar. Yazma etkinleştirilmiş birden çok bölgeli senaryolarda, kullanıcılar, kullanım ve yeniden yönlendirmeleri öncelik sırasına eklenen bölge ile bölgeye özgü uç noktaları belirtmeniz gerekir.

Salt okunur tercihleri yapılandırma hakkında daha fazla bilgi bulabilirsiniz [SQL API](../cosmos-db/tutorial-global-distribution-sql-api.md), [tablo API'si](../cosmos-db/tutorial-global-distribution-table.md), ve [MongoDB API'si](../cosmos-db/tutorial-global-distribution-mongodb.md) bu makalelere göz atın.

### <a id="TransparentSchemaMigration"></a>Saydam ve tutarlı veritabanı şema ve Dizin geçişi 
Azure Cosmos DB, tam olarak [şemadan](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Veritabanı Altyapısı'nın benzersiz tasarım, Azure Cosmos DB için otomatik olarak sağlar ve zaman uyumlu olarak herhangi bir şemayı ya da ikincil dizinlerin kullanıcıdan gerek kalmadan tüm verileri alma, bağlı dizin. Bu, veritabanı şema ve dizin geçiş hakkında endişelenmeden veya birden çok aşamalı olarak piyasaya sürülmeleri şema değişiklikleri koordine Global olarak dağıtılmış uygulamanızı hızla yineleme yapmasına sağlar. Azure Cosmos DB dizinleme ilkeleri açıkça yaptığınız herhangi bir değişiklik performans veya kullanılabilirlik düşmesine yol yok garanti eder.  

### <a id="ComprehensiveSLAs"></a>Kapsamlı SLA'lar (ötesinde, yüksek kullanılabilirlik)
Bir Global olarak dağıtılmış veritabanı hizmeti olarak Azure Cosmos DB için iyi tanımlanmış ve kapsamlı SLA'lar sunar **kullanılabilirlik**, **gecikme**, **aktarım hızı**ve **tutarlılık** veritabanıyla ilişkili bölge sayısı ne olursa olsun, küresel ölçekte çalışan veritabanı için.  

## <a id="LatencyGuarantees"></a>Gecikme süresi garantileri
Verilerinize dünyanın her yerinden düşük gecikme süreli erişim sunmak için en önemli avantajı, Azure Cosmos DB gibi bir Global olarak dağıtılmış veritabanı hizmeti olan. Azure Cosmos DB, çeşitli veritabanı işlemlerini için 99. yüzdebirlik dilimde garantili düşük gecikme süresi sunar. Azure Cosmos DB kullanan çoğaltma protokolü, veritabanı işlemleri (okuma ve yazma), istemcinin yerel bölgede her zaman gerçekleştirilir sağlar. Azure Cosmos DB SLA gecikme süresi garantileri 99. yüzdebirlik okuma, (eşzamanlı olarak) dizinli yazma ve sorgular için çeşitli istek ve yanıt boyutları için sağlar. Yazma işlemleri için gecikme süresi garantileri yerel bölge içinde dayanıklı çoğunluğu çekirdek işlemeler içerir.

### <a id="LatencyAndConsistency"></a>Gecikme süresi'nin ilişki tutarlılık 
Küresel olarak dağıtılan bir kurulumda güçlü tutarlılık sağlamaya Global olarak dağıtılmış hizmet için zaman uyumlu olarak yazar çoğaltmak için veya bölgeler arası okuma zaman uyumlu olarak gerçekleştirmek için gerekir. Güçlü tutarlılık daha yüksek gecikme süreleri ve azaltılmış kullanılabilirlik veritabanı işlemleri neden açık ve geniş alan ağı güvenilirlik hızını belirler. Bu nedenle, sunmak için düşük gecikme süreleriyle 99. yüzdebirlik dilimde ve tek bölgeli tüm hesaplar ve tutarlılıkla çok bölgeli tüm hesaplar için % 99,99 kullanılabilirlik ve çok bölgeli tüm veritabanı hesaplarında, bir hizmet üzerinde % 99,999 kullanılabilirlik garantisi. zaman uyumsuz çoğaltma uygulamanız gerekir. Bu, dönüş hizmet ayrıca sunmalı gerektirir [iyi tanımlanmış, gevşek tutarlılık modellere](consistency-levels.md) : güçlü (düşük gecikme süreli ve kullanılabilirliği garantisi sunmak için) ve "son" tutarlılık processContents'i ideal olarak daha zayıf (ile bir sezgisel programlama modeli).

Azure Cosmos DB, okuma işlemi belirli tutarlılık düzeyi garantisi sunmak için birden çok bölgede çoğaltma başvurmak için gerekli değildir sağlar. Benzer şekilde, verileri çoğaltılırken bir yazma işlemi engellenmiş değil, sağlar tüm bölgelerde diğer bir deyişle, yazma işlemlerini zaman uyumsuz olarak bölgeler arasında çoğaltılır). Çoklu bölge veritabanı hesapları için güçlü tutarlılıkla düzeylerinde yanı sıra hem de kullanılabilir. 

### <a id="LatencyAndAvailability"></a>Kullanılabilirlik ile gecikme süresi'nin ilişki 
Gecikme süresi ve kullanılabilirlik aynı para iki tarafının var. Bir işlem kararlı bir duruma ve varsa hataları ve ağ bölümlerinden kullanılabilirlik, gecikme süresi hakkında Bahsediyor. Uygulama açısından yavaş çalışan bir veritabanı işlemi kullanılamayan bir veritabanından ayırt. 

Azure Cosmos DB kullanılamazlık yüksek gecikme ayırt etmek için çeşitli veritabanı işlemlerini gecikme süresi için mutlak bir üst sınır sağlar. Azure Cosmos DB, veritabanı işlemi tamamlamak için üst sınırdan daha uzun sürerse, zaman aşımı hatası döndürür. Azure Cosmos DB kullanılabilirlik SLA'sı, zaman aşımları kullanılabilirlik SLA'sı karşı sayıldığını sağlar. 

### <a id="LatencyAndThroughput"></a>Aktarım hızı ile gecikme süresi'nin ilişki
Azure Cosmos DB, aktarım hızı ve gecikme süresi arasında seçim yapmaz. Bu SLA'sı hem düşük gecikme süresi 99. yüzdebirlik dilimde geliştirir ve, sağladığınız aktarım hızı sunar. 

## <a id="ConsistencyGuarantees"></a>Tutarlılık garantisi
Sırada [güçlü tutarlılık modeli](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) altın standart veri programlama, daha yüksek gecikme süresine (durağan), yüksek bir fiyatla sunulur ve azaltılmış kullanılabilirlik (karşılaşıldığında hata sayısı). 

Azure Cosmos DB iyi tanımlanmış bir programlama modeli, nedeni hakkında çoğaltılan verilerin tutarlılık sunar. Global olarak dağıtılmış uygulamaları çok girişli özelliğiyle kolayca oluşturmanızı etkinleştirmek için Azure Cosmos DB tarafından sunulan tutarlılık modeli okuma ve yazma işlemleri burada olmasını bölgeden bölgeye geçişte sorun yaşamaz ve bağımsız olacak şekilde tasarlanmıştır hizmet. 

Azure Cosmos DB tutarlılık SLA'sı garanti eder okuma isteklerinin % 100 (ya da veritabanı hesabı veya talep düzeyinde), belirtilen bir tutarlılık modelini için tutarlılık garantisini karşılar. Tutarlılık düzeyi ile ilişkili tüm tutarlılık garantileri sağlanırsa Okuma isteği tutarlılık SLA'sı karşıladığınızdan kabul edilir. Aşağıdaki tabloda tutarlılık modeli ve tutarlılık gurantees yakalar. Her biri bu tutarlılık modeli, %100 SLA'sı garanti eder. 

|Tutarlılık modeli  | Özellikler  |
|---------|---------|
|Güçlü |  Linearizable  |
|Sınırlanmış eskime durumu  |  Monoton okuma (bir bölgede), tutarlı ön ek, bağımlı eskime &lt; K, T   |
|Oturum  |  Kendi yazma, Monotonic okuma, tutarlı ön ek    |
|Tutarlı ön ek  | Tutarlı ön ek  |

### <a id="ConsistencyAndAvailability"></a>Kullanılabilirlik ile tutarlılık'ın ilişkisi
[İmpossibility sonucu](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) , [CAP Teoremi](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) kullanılabilir durumda kalır ve linearizable tutarlılık hataları karşılaşıldığında sunmak bir sistem gerçekten de mümkün olduğunu kanıtlar. CP ya da AP, burada CP sistemleri bırakmayı linearizable tutarlılık yerine kullanılabilirlik AP sistemleri bırakmayı sırada olması veritabanı hizmeti seçmelisiniz [linearizable tutarlılık](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) kullanılabilirlik ile değiştiriliyor. Azure Cosmos DB, hiçbir zaman bir CP sistem resmi kolaylaştırır istenen tutarlılık modeli ihlal ediyor. Ancak, uygulamada tutarlılık tüm ya da herhangi bir teklifi değildir; linearizable ve nihai tutarlılık arasında tutarlılık spektrumu boyunca birden çok sayıda iyi tanımlanmış tutarlılık modeli vardır. Azure Cosmos DB'de gerçek dünya senaryoları için uygun olan ve kullanmak için sezgisel çeşitli tutarlılıkla yöntemleri tanımlar. Azure Cosmos DB tutarlılık kullanılabilirlik seçenekleri sunarak gider bir [birden fazla esnek henüz iyi tanımlanmış tutarlılık modeli](consistency-levels.md) ve % 99,99 kullanılabilirlik tüm çoklu bölge veritabanı hesapları ve % 99,999 okuma ve yazma tüm çoklu bölge veritabanı hesapları için kullanılabilir. 

### <a id="ConsistencyAndAvailability"></a>Gecikme süresi ile tutarlılık'ın ilişkisi
CAP Teoremi daha kapsamlı bir çeşitlemesi adlı [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf), hangi ayrıca hesapları için gecikme süresi ve tutarlılık dengelerin bir kararlı durumda. Bu, tutarlılık ve gecikme süresi arasında bir kararlı durumda bir veritabanı sistemi seçmeniz gerektiğini belirtir. Birden fazla gevşek tutarlılık modeli (zaman uyumsuz çoğaltma ve yerel okuma ve yazma çekirdekleri desteklenen) ile Azure Cosmos DB tüm okuma ve yazma işlemleri için salt okunur yerel ve bölgeleri sırasıyla yazma sağlar. Bu, verilen tutarlılık modeli için bölge içinde düşük gecikme süresi garanti eder sunmak Azure Cosmos DB sağlar.  

### <a id="ConsistencyAndThroughput"></a>Aktarım hızı ile tutarlılık'ın ilişkisi
Belirli tutarlılık modelini uygulanması seçiminize bağlı olduğundan bir [çekirdek türü](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf), aktarım hızı da bağlı olarak değişiklik bir tutarlılık modelini seçimi. Örneğin, Azure Cosmos DB'de RU kesinlikle tutarlı okumaların için kabaca ücrettir *çift* , sonuçta tutarlı okur. Bu durumda, aynı aktarım hızı elde etmek için RUs çift sağlamanız gerekir. Aşağıdaki görüntüde Azure Cosmos DB'de belirli tutarlılık modeli için salt okunur kapasite arasındaki ilişki gösterilmektedir:

![Aktarım hızı ve tutarlılık arasındaki ilişki](./media/distribute-data-globally-turnkey/consistency-and-throughput.png)

## <a id="ThroughputGuarantees"></a>Aktarım hızı garanti eder 
Azure Cosmos DB, aktarım hızını ölçeklendirme (yanı sıra, depolama), dünyanın herhangi bir sayıda bölgede gereksinimlerimi ve isteğe bağlı olarak sağlar. Aşağıdaki görüntüde (arasında bir bölgede üç kaynak bölümler) yatay olarak bölümlenir tek bir Azure Cosmos DB kapsayıcısı gösterir ve üç Azure bölgeleri arasında genel olarak dağıtılmış:

![Azure Cosmos DB, dağıtılmış ve bölümlenmiş kapsayıcıları](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

Bir Azure Cosmos DB kapsayıcısı (i) bir bölge içinde ve (ii) farklı bölgelerdeki iki boyutta dağıtılır. Bunu yapmak için: 

* **Yerel dağıtım**: tek bir bölgede bir Azure Cosmos DB kapsayıcısı yatay olarak kullanıma açısından ölçeklenir *kaynak bölümler*. Her kaynak bölümü, bir anahtar kümesini yönetir ve kesinlikle tutarlı ve yüksek oranda kullanılabilir fiziksel olarak dört çoğaltmaları tarafından temsil edilen da bilinen bir *çoğaltma kümesine* ve yinelemeler arasında makine çoğaltma durumu. Azure Cosmos DB bir tam kaynak yönetilen kaynak bölümü aktarım hızı için ayrılan sistem kaynaklarının bütçenin payını sunmak için sorumlu olduğu sistemidir. Bir Azure Cosmos DB kapsayıcısı ölçeklendirme, kullanıcılar için saydamdır. Azure Cosmos DB kaynak bölümlerini yönetir ve böler ve depolama alanı olarak gerektiğinde birleştirir ve aktarım hızı gereksinimleri değiştirin. 
* **Genel dağıtım**: çoklu bölge veritabanı ise, her kaynak bölümlerden bu bölgede ardından dağıtılır. Çeşitli bölgeleri formun tamamı boyunca anahtarları kümesinin aynısına sahip olan kaynak bölümler *bölüm kümesi* (bkz [önceki şekilde](#ThroughputGuarantees)).  Veritabanıyla ilişkilendirilmiş birden çok bölge arasında durum makine çoğaltmayı kullanarak kaynak bölümler bölüm kümesinde düzenlenir. Yapılandırılmış, tutarlılık düzeyine bağlı olarak, farklı topoloji (örneğin, yıldız, zincir, ağaç vb.) kullanarak dinamik olarak kaynak bölümler bölüm kümesi içinde yapılandırılır. 

Bir yüksek derecede yanıt veren bölüm yönetimi, Yük Dengeleme ve katı kaynak İdaresi sayesinde, Azure Cosmos DB, esnek bir biçimde aktarım hızını ölçeklendirme için bir Azure Cosmos DB kapsayıcısı veya veritabanı ile ilişkili birden çok Azure bölgeleri arasında sağlar. Bir çalışma zamanı işlemi Azure Cosmos DB'de sağlanan aktarım hızı değiştiriyor. Benzer şekilde diğer veritabanı işlemleri, Azure Cosmos DB mutlak üst sınırı için sağlanan aktarım hızı değiştirmek, istek gecikme süresi garanti eder. Örneğin, aşağıdaki şekilde, isteğe bağlı (1 M - 10 milyon istek/sn iki bölgede arasında) esnek bir şekilde sağlanan aktarım hızı ile müşterinin kapsayıcı gösterilmektedir.

![Azure Cosmos DB, esnek bir biçimde aktarım hızı sağlanmış](./media/distribute-data-globally-turnkey/elastic-throughput.png)

### <a id="ThroughputAndConsistency"></a>Aktarım hızı'nın ilişki tutarlılık 
Açıklanan aynı mıdır [aktarım hızı ile tutarlılık'ın ilişki](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Kullanılabilirlik ile Aktarım'ın ilişkisi
Azure Cosmos DB, sağlanan aktarım hızı için değişiklik yapıldığında, yüksek kullanılabilirliği sürdürmek devam eder. Azure Cosmos DB kaynak bölümlerini şeffaf bir şekilde yönetir (ve bölme ve birleştirme kopya gerçekleştirir işlem) ve uygulama esnek bir şekilde artırır veya azaltır aktarım hızı işlemleri performans veya kullanılabilirlik düşürmeyecektir olmasını sağlar. 

## <a id="AvailabilityGuarantees"></a>Kullanılabilirlik garantileri
Azure Cosmos DB tüm çoklu bölge veritabanı hesapları ve çok bölgeli tüm hesaplar için % 99,99 kullanılabilirlik SLA'sı tutarlılıkla ve çok bölgeli tüm veritabanı hesaplarında % 99,999 kullanılabilirlik sağlar. Daha önce açıklandığı gibi Azure Cosmos DB'nin kullanılabilirlik garantilerinden her veri ve denetim düzlemi işlemleri için gecikme süresi için mutlak bir üst sınır içerir. Kullanılabilirlik garantileri bölgeleri veya bölgeler arasındaki coğrafi uzaklıktan sayısı ile değiştirmeyin. Kullanılabilirlik garantileri göre hem el ile hem de otomatik yük devretme işlemleri geçerlidir. Azure Cosmos DB, uygulamanızın mantıksal uç noktalarına karşı çalışabilir ve istekleri yeni bir bölgeye yük devretme durumunda şeffaf bir şekilde yönlendirebilir belirli bölgesel uç noktaları için öncelik sırasına göre yapabilecekleriniz olduğundan emin olun veya saydam çok girişli API'ler sunar. Kullanım. Uygulamanızı bir bölgesel yük devretme ve kullanılabilirlik SLA'ları her zaman korunur durumunda dağıtılması gerekmez.

### <a id="AvailabilityAndConsistency"></a>Tutarlılık, gecikme süresi ve aktarım hızı ile kullanılabilirlik'ın ilişkisi
Tutarlılık, gecikme süresi ve aktarım hızı ile kullanılabilirlik'ın ilişki bölümlerinde açıklanan [kullanılabilirlik ile tutarlılık'ın ilişki](#ConsistencyAndAvailability), [gecikme süresi'nin ilişki kullanılabilirlik ile](#LatencyAndAvailability) ve [Kullanılabilirlik ile Aktarım'ın ilişki](#ThroughputAndAvailability). 

## <a id="CustomerFacingSLAMetrics"></a>Müşterilere yönelik SLA ölçümleri
Azure Cosmos DB, şeffaf bir şekilde aktarım hızı, gecikme süresi, tutarlılık ve kullanılabilirlik ölçümlerini gösterir. Bu ölçümler program aracılığıyla ve Azure portalından erişilebilir (aşağıdaki şekilde bakın). Azure Application ınsights'ı kullanarak çeşitli eşikleri uyarılar ayarlayabilirsiniz. Aşağıdaki görüntüde, tutarlılık, gecikme süresi, aktarım hızı ve şeffaf bir şekilde her bir kiracı kullanılabilir kullanılabilirlik ölçümlerini gösterir:
 
![Azure Cosmos DB müşteri görünür SLA ölçümleri](./media/distribute-data-globally-turnkey/customer-slas.png)

## <a id="Next Steps"></a>Sonraki adımlar

* [Azure Cosmos DB genel dağıtımını önemli avantajları](distribute-data-globally-benefits.md)

* [Azure Cosmos DB genel veritabanı çoğaltması yapılandırma](tutorial-global-distribution-sql-api.md)

* [Azure Cosmos DB hesapları için çok yöneticili etkinleştirme](enable-multi-master.md)

* [Otomatik ve el ile yük devretme, Azure Cosmos DB'de nasıl çalışır?](regional-failover.md)

* [Azure Cosmos DB'de çakışma çözümü anlama](multi-master-conflict-resolution.md)

* [çok yöneticili açık kaynak NoSQL veritabanları ile kullanma](multi-master-oss-nosql.md)


## <a id="References"></a>Başvuruları
1. Eric Brewer. [Doğru güçlü bir dağıtılmış sistemler](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Eric Brewer. [CAP daha sonra – on yıl nasıl kurallar değişti](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Gilbert, Lynch'in. - [Brewer&#39;s Conjecture ve tutarlı, yüksek uygulanabilirliğini, bölüm dayanıklı Web Hizmetleri](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. Daniel Abadi. [Tutarlılık Ödünler Modern, dağıtılmış veritabanı sistem tasarımı](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. Martin Kleppmann. [Lütfen veritabanları CP veya AP çağırma durdurun.](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Peter Bailis v.d. [Olasılığa dayalı sınırlanmış eskime durumu (PBS) için pratik kısmi çekirdekleri](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor ve Wool. [Yük, kapasite ve çekirdek sistemlerde kullanılabilirlik](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy ve Kelebek. [Lineralizability: Eş zamanlı nesnelerin doğruluk koşul](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. [Azure Cosmos DB SLA'sı](https://azure.microsoft.com/support/legal/sla/cosmos-db/)
