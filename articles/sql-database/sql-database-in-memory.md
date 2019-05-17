---
title: Azure SQL veritabanında bellek içi teknolojiler | Microsoft Docs
description: Azure SQL veritabanında bellek içi teknolojileri analizi iş yükleri ve işlem performansını büyük ölçüde geliştirebilirsiniz.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/19/2019
ms.openlocfilehash: 5681b5aa46acc1192675da0b1cceee596dfa0105
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65799899"
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a>SQL veritabanında bellek içi teknolojileri kullanarak performansını iyileştirin

Azure SQL veritabanında bellek içi teknolojileri, uygulamanızın performansını sağlar ve potansiyel olarak veritabanı maliyetini azaltın. 

## <a name="when-to-use-in-memory-technologies"></a>Bellek içi teknolojileri kullanmayı ne zaman

Azure SQL veritabanı'nda Bellek içi teknolojileri kullanarak, çeşitli iş yükleriyle performans iyileştirmeleri elde edebilirsiniz:

- **İşlem** (çevrimiçi işlem gerçekleştirme (OLTP)) burada isteklerin çoğunu okuma veya güncelleştirme daha küçük veri (örneğin, CRUD işlemleri) kümesi.
- **Analitik** (çevrimiçi analitik işlem (OLAP)) çoğu sorgu ve raporlama için karmaşık hesaplamalar sahip olduğu amacıyla, belirli sayıda yüklemek ve verileri (Bu nedenle toplu yükleme olarak adlandırılır) mevcut tablolar eklemek ya da silme sorguları ile tablolardaki verileri. 
- **Karma** (hibrit işlem/analitik işlem (HTAP)) OLTP ve OLAP sorguları aynı veri kümesi üzerinde yürütülen burada.

Bellek içi teknolojileri sorguları native derlemesi kullanarak belleğe işlenen verileri tutarak bu iş yüklerinin performansını geliştirebilir veya gelişmiş işleme tür olarak toplu işleme ve mevcut SIMD yönergeleri temel alınan donanım. 

## <a name="overview"></a>Genel Bakış

Azure SQL veritabanı, bellek içi teknolojilerin sahiptir:
- *[Bellek içi OLTP](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization)*  saniye başına işlem sayısını artırır ve işlem için gecikme süresini azaltır. Bellek içi OLTP ' yararlanan senaryolar şunlardır: olayları veya önbelleğe alma, veri yükleme ve geçici tablo ve tablo değişkeni senaryoları IOT cihazları, ticaret ve oyun, veri alma gibi yüksek performanslı işlem.
- *Kümelenmiş columnstore dizinleri* (en fazla 10 kez) depolama altyapınızın kapladığı alanı azaltmak ve raporlama ve analiz sorguları için performansı geliştirin. Bu olgu tabloları ile veri reyonlarınızı veritabanınızda daha fazla veri uyacak şekilde genişletebilir ve performansı artırmak için kullanabilirsiniz. Ayrıca, bu birlikte çalışarak geçmiş verileri işletimsel veritabanınız arşivlemek ve 10 kata kadar daha fazla veri sorgulama yapmak için kullanabilirsiniz.
- *Kümelenmemiş columnstore dizinleri* HTAP yardımcı olmak için işletimsel veritabanını çalıştırmak pahalı ayıklama, gerek kalmadan doğrudan sorgulama aracılığıyla işinizi gerçek zamanlı Öngörüler edinmek için dönüştürme ve yükleme (ETL) işlemi ve bekleyin veri ambarı'doldurulmalıdır. Kümelenmemiş columnstore dizinleri, işlemsel iş etkisi azaltırken, OLTP veritabanı üzerinde hızlı analiz sorguları yürütülmesine izin.
- *Kümelenmiş columnstore dizinleri bellek için iyileştirilmiş* HTAP hızlı işlem yapmanıza olanak sağlar ve *eşzamanlı olarak* analiz sorguları verilere çok hızlı bir şekilde çalıştırma.

SQL Server ürününün bir parçası 2012 ve 2014'ten itibaren columnstore dizinleri hem de bellek içi OLTP sırasıyla olmuştur. Azure SQL veritabanı ve SQL Server bellek içi teknolojileri aynı uygulamasını paylaşın. SQL Server'da yayınlanmadan önce bundan sonra bu teknolojiler için yeni özellikler Azure SQL veritabanı'nda ilk olarak kullanıma sunulur.

## <a name="benefits-of-in-memory-technology"></a>Bellek içi teknolojisi avantajları

Daha verimli sorgu ve işlem nedeniyle, bellek içi teknolojileri de maliyetini azaltmak için yardımcı olur. Genellikle, performans kazanç elde etmek için veritabanı fiyatlandırma katmanını yükseltme gerekmez. Bazı durumlarda bile aktarmanızı fiyatlandırma katmanı, hala performans geliştirmeleri ile bellek içi teknolojileri görerek azaltın.

Bellek içi OLTP performansını önemli ölçüde artırmak için nasıl yardımcı oldu iki örnek aşağıda verilmiştir:

- Bellek içi OLTP kullanarak [70 oranında Dtu artırırken iş yüklerinin çift çekirdek iş çözümleri bağlanabildi](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).

  - DTU anlamına gelir *veritabanı işlem birimi*, ve bir ölçüm kaynak tüketimi içerir.
- Aşağıdaki video, bir örnek iş yükü ile kaynak tüketimi önemli bir iyileştirme göstermektedir: [Azure SQL veritabanı video bellek içi OLTP](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).
  - Daha fazla bilgi için blog gönderisine bakın: [Azure SQL veritabanı Web günlüğü gönderisinde bellek içi OLTP](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

> [!NOTE]  
> Bellek içi teknolojileri, Premium ve iş açısından kritik katmanında Azure SQL veritabanları ve Premium elastik havuzlar için kullanılabilir.

Aşağıdaki videoda, olası performans artışı ile Azure SQL veritabanında bellek içi teknolojiler açıklanmaktadır. Her zaman gördüğünüz performans kazancı iş yüküne ve veri erişimi deseni veritabanının yapısını dahil olmak üzere birçok faktöre bağlı olduğunu unutmayın ve benzeri.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

Bu makalede, Azure SQL veritabanı'na özgü bellek içi OLTP ve columnstore dizinleri yönlerini açıklar ve ayrıca örnekleri içerir:

- Depolama ve veri boyut sınırları, bu teknolojiler etkisini göreceksiniz.
- Bu teknolojiler farklı fiyatlandırma katmanları arasında kullanan veritabanlarını hareketini yönetme görürsünüz.
- Azure SQL veritabanında columnstore dizinleri yanı sıra, bellek içi OLTP kullanımını gösteren iki örnek görürsünüz.

Daha fazla bilgi için bkz.

- [Bellek içi OLTP genel bakış ve kullanım senaryoları](https://msdn.microsoft.com/library/mt774593.aspx) (müşteri örnek olay incelemeleri ve kullanmaya başlamak için bilgi başvurular içerir)
- [Bellek içi OLTP için belgeleri](https://msdn.microsoft.com/library/dn133186.aspx)
- [Columnstore dizinleri Kılavuzu](https://msdn.microsoft.com/library/gg492088.aspx)
- Karma işlemsel / (HTAP), olarak da bilinen analitik işleme [gerçek zamanlı işlem analizi](https://msdn.microsoft.com/library/dn817827.aspx)

## <a name="in-memory-oltp"></a>Bellek içi OLTP

Bellek içi OLTP teknolojisi, tüm verileri bellek içinde tutarak son derece hızlı veri erişimi işlemleri sağlar. Bu da özelleştirilmiş dizinler, sorgu ve veri erişim Mandal yerel derleme OLTP iş yükünün performansını geliştirmek için kullanır. Bellek içi OLTP olarak verilerinizi düzenlemek için iki yol vardır:

- **Bellek için iyileştirilmiş rowstore** her satır ayrı bir bellek nesne olduğu biçimi. Yüksek performanslı OLTP iş yükleri için iyileştirilmiş klasik bir bellek içi OLTP biçimi budur. Bellek için iyileştirilmiş tablolar, bellek için iyileştirilmiş rowstore biçiminde kullanılabilir iki tür vardır:
  - *Dayanıklı tabloları* (schema_and_data dayanıklılığına) burada bellekte yer satırları korunur sunucuyu yeniden başlattıktan sonra. Bu tür bir tablo, bellek içi iyileştirmeler ek avantajları ile geleneksel rowstore tablo gibi davranır.
  - *Dayanıklı olmayan tablolar* (SCHEMA_ONLY) satırları nerede değil korunur yeniden başlatıldıktan sonra. Bu tür bir tabloya geçici verileri (örneğin, geçici tabloları değiştirme) için tasarlanmıştır ya da (Bu nedenle hazırlama tabloları da denir) bazı kalıcı tabloya taşımadan önce tablolar, hızlı bir şekilde gereken veri yükleyin.
- **Bellek için iyileştirilmiş columnstore** burada veri düzenlenir sütunlu bir biçimde biçimi. Bu yapı, OLTP iş yükünüz nerede çalışıyor aynı veri yapısına analitik sorguları çalıştırmak için gereken HTAP senaryoları için tasarlanmıştır.

> [!Note]
> Bellek içi OLTP teknoloji bellekte tam olarak bulunabilir veri yapıları için tasarlanmıştır. Bellek içi verileri Boşaltılan olduğundan emin olun, disk için yeterli belleğe sahip bir veritabanı kullanıyor. Bkz: [verileri boyutu ve depolama kapasitesi için bellek içi OLTP](#data-size-and-storage-cap-for-in-memory-oltp) daha fazla ayrıntı için.

Bellek içi OLTP hakkında hızlı bilgi: [Hızlı Başlangıç 1: Daha hızlı T-SQL performansı için bellek içi OLTP teknolojileri](https://msdn.microsoft.com/library/mt694156.aspx) (yardımcı olması için başka bir makalede başlama)

Kapsamlı videoları teknolojileri hakkında:

- [Azure SQL veritabanında bellek içi OLTP](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (performans avantajlarının bu sonuçları kendiniz yeniden oluşturma adımları ve bir tanıtım içeren)
- [Bellek içi OLTP videolar: Nedir ve ne zaman/nasıl kullanmak için](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)

Belirli bir veritabanı, bellek içi OLTP destekleyip desteklemediğini anlamak için programlı bir yolu yoktur. Aşağıdaki Transact-SQL sorgusunu yürütebilirsiniz:
```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```
Sorgu döndürürse **1**, bellek içi OLTP, bu veritabanında desteklenir. Aşağıdaki sorgularda standart/temel bir veritabanı indirgenen önce kaldırılması gereken tüm nesneleri tanımlar:
```
SELECT * FROM sys.tables WHERE is_memory_optimized=1
SELECT * FROM sys.table_types WHERE is_memory_optimized=1
SELECT * FROM sys.sql_modules WHERE uses_native_compilation=1
```

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a>Bellek içi OLTP için verileri boyutu ve depolama kapasitesi

Bellek içi OLTP, kullanıcı verilerini depolamak için kullanılan bellek için iyileştirilmiş tablolar içerir. Bu tablolar belleğe sığması gerekir. SQL veritabanı hizmeti bellekte doğrudan yönetmek için kullanıcı verileri için bir kota kavramını sahibiz. Bu fikir olarak adlandırılır *bellek içi OLTP depolama alanını*.

Fiyatlandırma katmanı ve her esnek havuzun fiyatlandırma katmanı her desteklenen tek veritabanı, belirli bir miktarda bellek içi OLTP depolama alanı içerir. Bkz: [DTU tabanlı kaynak sınırları - tek veritabanı](sql-database-dtu-resource-limits-single-databases.md), [DTU tabanlı kaynak sınırları - elastik havuzlar](sql-database-dtu-resource-limits-elastic-pools.md),[sanal çekirdek tabanlı kaynak sınırları - tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [sanal çekirdek tabanlı kaynak sınırları - elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md).

Aşağıdaki öğeler, bellek içi OLTP depolama tavanınızı doğru sayısı:

- Etkin kullanıcı veri satırları bellek için iyileştirilmiş tablolarda ve Tablo değişkenleri. Eski satır sürümleri doğru uç sayılmaz unutmayın.
- Bellek için iyileştirilmiş tablolarda dizinler.
- ALTER TABLE işlemlerin işlemsel yükünü.

Sınırına ulaşırsanız, bir kota aşımı hatayla karşılaştıysanız ve artık ekleyemez veya verileri güncelleştirin. Bu hatayı gidermek için verileri silmek veya havuz veya veritabanı fiyatlandırma katmanı artırın.

Bellek içi OLTP depolama alanı kullanımı izleme ve neredeyse sınırına ulaştığınızda uyarılarını yapılandırma hakkında daha fazla ayrıntı için bkz. [bellek içi izleme depolama](sql-database-in-memory-oltp-monitoring.md).

#### <a name="about-elastic-pools"></a>Elastik havuzlar hakkında

Elastik havuzlar sayesinde, bellek içi OLTP depolama alanı, havuzdaki tüm veritabanları arasında paylaşılır. Bu nedenle, bir veritabanında kullanım büyük olasılıkla diğer veritabanları etkileyebilir. Bu iki Azaltıcı Etkenler şunlardır:

- Yapılandırma bir `Max-eDTU` veya `MaxvCore` bir bütün olarak havuz eDTU veya sanal çekirdek sayısı daha düşük maliyetlidir veritabanları için. Bu maksimum bellek içi OLTP depolama alanı kullanımı, havuzdaki eDTU sayısına karşılık gelen boyutuna herhangi bir veritabanında belirler.
- Yapılandırma bir `Min-eDTU` veya `MinvCore` 0'dan büyük. Bu minimum havuzdaki her bir veritabanına karşılık gelen yapılandırılmış kullanılabilir bellek içi OLTP depolama alanı miktarı olduğunu güvence altına alır. `Min-eDTU` veya `vCore`.

### <a name="changing-service-tiers-of-databases-that-use-in-memory-oltp-technologies"></a>Bellek içi OLTP teknolojileri kullanan veritabanlarını hizmet katmanlarını değiştirme

Her zaman veritabanı veya örnek daha yüksek bir katmana gibi iş açısından kritik (veya standart, Premium) için genel amaçlı yükseltebilirsiniz. Kullanılabilir işlevleri ve kaynakları yalnızca artırın.

Ancak, katman eski sürüme düşürme veritabanınızı olumsuz yönde etkileyebilir. Genel amaçlı (veya standart veya temel Premium) için iş kritik sürümüne düşürürseniz, etkiyi özellikle açıktır, veritabanı, bellek içi OLTP nesneleri içerdiğinde. (Bunlar görünür kalır olsa bile), bellek için iyileştirilmiş tablolar indirgeme sonra kullanılamaz. Bir esnek havuzun fiyatlandırma katmanını azaltmayı veya bellek içi teknolojileri ile bir veritabanını elastik havuzun bir standart veya temel taşıma ilgili noktaların aynısı geçerlidir.

> [!Important]
> Bellek içi OLTP genel amaçlı, standart veya temel katmanında desteklenmiyor. Bu nedenle, standart veya temel katmanına herhangi bir bellek içi OLTP nesneleri olan bir veritabanını taşımak mümkün değildir.

Standart/temel veritabanına düşürme önce tüm bellek için iyileştirilmiş tablolar ve tablo türleri yanı sıra, tüm yerel olarak derlenen T-SQL modülleri kaldırın. 

*İş açısından kritik katmanında ölçeklendirme aşağı kaynakları*: Bellek için iyileştirilmiş tablolardaki verileri veya veritabanı yönetilen örneği bir katman ile ilişkili olan bellek içi OLTP depolama alanı içinde olmalıdır ya da elastik Havuzda kullanılabilir. Katmanı ölçek azaltma deneyin veya veritabanını kullanılabilir yeterli bellek içi OLTP depolama alanı sahip olmayan bir havuza taşıma işlemi başarısız olur.

## <a name="in-memory-columnstore"></a>Bellek içi columnstore

Bellek içi columnstore teknolojisi, depolamak ve çok miktarda tablolardaki verileri sorgulamak etkileştirir. Columnstore teknolojisi, sütun tabanlı bir veri depolama biçimini kullanır ve elde etmek için sorgu toplu işleme, geleneksel satır yönelimli depolamaya 10 kata kadar sorgu performansı OLAP iş yükleri edinin. Ayrıca üzerinde sıkıştırılmamış veri boyutu 10 kez veri sıkıştırma en fazla kazanç elde edebilirsiniz.
Columnstore modellerin olarak verilerinizi düzenlemek için kullanabileceğiniz iki tür vardır:

- **Kümelenmiş columnstore** nerede tablosundaki tüm verileri düzenlenir sütunlu biçiminde. Bu modelde, tablodaki tüm satırları, yüksek oranda verileri sıkıştırır ve analitik sorguları ve raporları tablosunda yürütülecek sağlayan irdelemenizde yerleştirilir. Verilerinizi doğasına bağlı olarak, verilerin boyutunu azalan 10 olabilir x-100 x. Kümelenmiş columnstore modeli, verilerin diskte depolanan önce 100 bin satır sıkıştırılır değerinden büyük toplu beri büyük boyutlu verileri (toplu yükleme) hızlı alımı de sağlar. Bu model, Klasik veri ambarı senaryoları için iyi bir seçimdir. 
- **Olmayan kümelenmiş columnstore** burada veri geleneksel rowstore tablosunda depolanır ve analitik sorgular için kullanılan columnstore biçiminde bir dizin yok. Karma işlem analitik işleme (HTAP) Bu modeli sağlar: bir işlem yüküne üzerinde yüksek performanslı gerçek zamanlı analiz çalıştırma olanağı. OLTP sorguları taramaları ve analiz için daha iyi bir seçimdir columnstore dizininde OLAP sorgular yürütülürken küçük bir satır erişmek için optimize edilmiştir rowstore tablosunda yürütülür. Azure SQL veritabanı sorgu iyileştiricisi temelli rowstore veya columnstore biçiminde dinamik olarak seçer. Olmayan kümelenmiş columnstore dizinleri, özgün veri kümesi herhangi bir değişiklik olmadan özgün rowstore tabloda bulunduğundan verilerin boyutunu azaltmak yok. Ancak, ek bir columnstore dizini boyutu, büyüklük sırası eşdeğer B-Ağacı dizini daha küçük olmalıdır.

> [!Note]
> Bellek içi columnstore teknolojisi, diskteki belleğe sığamıyorsa veri barındırırken, bellek, işleme için gereken verileri tutar. Bu nedenle, bellek içi columnstore yapılardaki veri miktarı, kullanılabilir bellek miktarını aşabilir. 

Ayrıntılı video teknolojisi hakkında:

- [Columnstore dizini: Bellek içi analiz videolardan 2016 Ignite](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

### <a name="data-size-and-storage-for-columnstore-indexes"></a>Veri boyutu ve depolama için columnstore dizinleri

Columnstore dizinleri, belleğe sığması için gerekli değildir. Bu nedenle, yalnızca cap dizinleri boyutuna, belgelenen genel veritabanı boyutu, olan [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) makaleler.

Kümelenmiş columnstore dizinleri kullandığınızda, aynı zamanda sütunlu sıkıştırma temel tablo depolaması için kullanılır. Bu sıkıştırma veritabanında daha fazla veri sığabilen anlamına gelir, kullanıcı veri depolama ayak izini önemli ölçüde azaltabilir. Ve sıkıştırma ile daha da artırılabilir [sütunlu arşiv sıkıştırma](https://msdn.microsoft.com/library/cc280449.aspx#using-columnstore-and-columnstore-archive-compression). Verilerin niteliğine üzerinde elde edebileceğiniz sıkıştırma bağlıdır, ancak 10 kez sıkıştırma durumdur.

Örneğin, bir maksimum boyut 1 terabayt (TB) sahip bir veritabanı varsa ve 10 kez sıkıştırma columnstore dizinleri kullanarak elde etmek, toplam 10 TB kullanıcı verilerini veritabanında sığabilen.

Kümelenmemiş columnstore dizinleri kullandığınızda, temel tablo hala geleneksel rowstore biçiminde depolanır. Bu nedenle, depolama tasarrufu kümelenmiş columnstore dizinleri ile kadar büyük değil. Ancak, tek bir columnstore dizini ile geleneksel kümelenmemiş dizinler sayısı değiştiriyorsanız, genel bir tablo için depolama ayak izini tasarruf görebilirsiniz.

### <a name="changing-service-tiers-of-databases-containing-columnstore-indexes"></a>Columnstore dizinleri içeren veritabanları hizmet katmanlarını değiştirme

*Eski sürüme düşürme tek veritabanı için temel veya standart* hedef katmanınızı S3 ise mümkün olmayabilir. Columnstore dizinleri yalnızca iş kritik/Premium fiyatlandırma katmanını temel ve standart katmanda, S3 ve yukarıda ve değil, temel katmanı desteklenir. Veritabanınızı desteklenmeyen katmanı veya düzeyini düşürme, columnstore dizininiz kullanılamaz duruma gelir. Sistem, columnstore dizini korur, ancak hiç dizin yararlanır. Daha sonra desteklenen katmanı veya düzeyi geri yükseltirseniz, columnstore dizininiz tekrar havuzlamanızı hemen hazırdır.

Varsa bir **kümelenmiş** columnstore dizini, tüm tablonun kullanım dışı olur sonra düşürme. Bu nedenle tüm bırak öneririz *kümelenmiş* veritabanınızı desteklenmeyen katmanı veya düzeyini düşürme önce columnstore dizinini oluşturur.

> [!Note]
> Örnek destekleyen tüm katmanlarda ColumnStore dizinlerinde yönetilen.

<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı Başlangıç 1: T-SQL daha hızlı performans için bellek içi OLTP teknolojileri](https://msdn.microsoft.com/library/mt694156.aspx)
- [Mevcut bir Azure SQL uygulamadaki bellek içi OLTP kullanın](sql-database-in-memory-oltp-migration.md)
- [İzleyici bellek içi OLTP depolama alanını](sql-database-in-memory-oltp-monitoring.md) için bellek içi OLTP
- [Azure SQL veritabanında bellek içi özellikleri deneyin](sql-database-in-memory-sample.md)

## <a name="additional-resources"></a>Ek kaynaklar

### <a name="deeper-information"></a>Daha ayrıntılı bilgi

- [SQL veritabanında bellek içi OLTP ile 70 oranında DTU düşürürken çekirdek anahtar veritabanının iş yükü nasıl Katlar öğrenin](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)
- [Azure SQL veritabanı Web günlüğü gönderisinde bellek içi OLTP](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)
- [Bellek içi OLTP hakkında bilgi edinin](https://msdn.microsoft.com/library/dn133186.aspx)
- [Columnstore dizinleri hakkında bilgi edinin](https://msdn.microsoft.com/library/gg492088.aspx)
- [Gerçek zamanlı işlem analizi hakkında bilgi edinin](https://msdn.microsoft.com/library/dn817827.aspx)
- Bkz: [yaygın iş yükü düzenleri ve geçiş konuları](https://msdn.microsoft.com/library/dn673538.aspx) (burada bellek içi OLTP yaygın olarak sağlayan önemli ölçüde performans kazanımı iş yükü düzenleri açıklayan)

### <a name="application-design"></a>Uygulama tasarımı

- [Bellek içi OLTP (bellek içi iyileştirme)](https://msdn.microsoft.com/library/dn133186.aspx)
- [Mevcut bir Azure SQL uygulamadaki bellek içi OLTP kullanın](sql-database-in-memory-oltp-migration.md)

### <a name="tools"></a>Araçlar

- [Azure portal](https://portal.azure.com/)
- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)
- [SQL Server Veri Araçları (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
