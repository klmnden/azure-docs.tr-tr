---
title: Azure SQL veritabanı hizmetinin - vCore | Microsoft Docs
description: VCore tabanlı satın alma modeli (Önizleme) bağımsız olarak işlem ve depolama kaynaklarını ölçeklendirme, şirket içi performans eşleşen ve fiyat en iyi duruma olanak tanır.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: bfa32796b40033a13d1ced9f8431bd19492e6498
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309588"
---
# <a name="choosing-a-vcore-service-tier-compute-memory-storage-and-io-resources"></a>VCore hizmet katmanı, hesaplama, bellek, depolama ve g/ç kaynakları seçme

Hizmet katmanları, bir dizi performans düzeyleri, yüksek kullanılabilirlik tasarımı, arıza yalıtımı, depolama türlerini ve g/ç aralığı tarafından ayrılır. Müşteri yedeklemeler için gerekli depolama ve Bekletme dönemi ayrı olarak yapılandırmanız gerekir. VCore modeliyle tek veritabanları ve esnek havuzlar ile yüzde 30 tasarrufları uygun için yukarı [SQL Server için Azure karma kullanımı avantajı](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

Aşağıdaki tabloda, bu iki katmanı arasındaki farklar anlamanıza yardımcı olur:

||**Genel amaçlı**|**Kritik iş**|
|---|---|---|
|En iyi kullanım alanı:|Çoğu kurumsal iş yükleri. Teklifler yönlendirilmiş Dengeli ve ölçeklenebilir bilgi işlem ve depolama seçenekleri bütçe.|Yüksek GÇ gereksinimleri olan iş uygulamaları. Çeşitli yalıtılmış çoğaltmaları kullanarak hatalara karşı en yüksek düzeyde dayanıklılık sağlar.|
|İşlem|1 ila 80 vCore, nesil 4 ve 5 oluşturma |1 ila 80 vCore, nesil 4 ve 5 oluşturma|
|Bellek|Çekirdek başına 7 GB |Çekirdek başına 7 GB |
|Depolama|Premium uzaktaki depolama birimi, 5 GB – 4 TB|Yerel SSD depolama, 5 GB – 4 TB|
|G/ç işleme (yaklaşık)|7000 en fazla IOPS ile vCore başına 500 IOPS|Çekirdek başına 5000 IOPS'yi 200000 en fazla IOPS ile|
|Kullanılabilirlik|1 çoğaltma, herhangi bir okuma ölçek|3 çoğaltmalar, 1 [okuma ölçekli](sql-database-read-scale-out.md), yedekli HA bölge|
|Yedeklemeler|RA-GRS, 7-35 gün (varsayılan olarak 7 gün)|RA-GRS, 7-35 gün (varsayılan olarak 7 gün) *|
|Bellek içi|Yok|Desteklenen|
|||

\* Önizleme sırasında yedeklemelerin saklama dönemi yapılandırılabilir değildir ve 7 gün için sabit.

> [!IMPORTANT]
> İşlem kapasitesi değerinden bir vCore gerekirse DTU tabanlı satın alma modeli kullanın.

Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için. 

## <a name="storage-considerations"></a>Depolama hakkında dikkat edilmesi gerekenler

Aşağıdaki topluluklara bir göz atın:
- Ayrılan depolama alanı, veri dosyalarını (MDF) ve günlük dosyalarını (LDF) tarafından kullanılır.
- Her performans düzeyi varsayılan en büyük boyutu 32 GB olan bir maksimum veritabanı boyutu destekler.
- İstenen veritabanı boyutu (MDF boyutunu) yapılandırırken, ek depolama alanı % 30 LDF desteklemek için otomatik olarak eklenir
- Desteklenen en fazla 10 GB arasındaki herhangi bir veritabanı boyutu seçebilirsiniz
 - Standart depolama artırabilir ya da 10 GB'lik artışlarla boyutunu azaltın
 - Premium depolama artırın veya boyutu 250 GB artışlarla azaltın
- Genel amaçlı hizmet katmanında `tempdb` ekli bir SSD ve maliyet bu depolama vCore fiyatına dahil kullanır.
- İş kritik hizmet katmanında `tempdb` paylaşımları MDF ve LDF dosyalarını ve maliyet tempDB depolama alanı ile ekli SSD vCore fiyatına dahildir.

> [!IMPORTANT]
> MDF ve LDF için ayrılan toplam depolama alanı için sizden ücret kesilir.

MDF ve LDF geçerli toplam boyutunu izlemek için kullanın [bilgilerini sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Tek tek MDF ve LDF dosyalarını boyutunu izlemek için kullanın [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

## <a name="backups-and-storage"></a>Yedekleme ve depolama

Veritabanı Yedeklemeleri için depolama noktası SQL veritabanı zaman geri yükleme (PITR) ve uzun vadeli bekletme (LTR) yeteneklerini desteklemek için ayrılır. Bu depolama ayrı olarak her veritabanı için ayrılan ve iki ayrı veritabanı başına ücret faturalandırılır. 

- **PITR**: olan otomatik olarak tek tek veritabanı yedeklemeleri RA-GRS depolama alanına kopyalanır. Yeni yedeklemeler oluşturuldukça dinamik olarak depolama boyutunu artırır.  Depolama alanı, haftalık tam yedeklemeler, günlük fark yedekleri ve 5 dakikada bir kopyalanan işlem günlüğü yedeklemeleri tarafından kullanılır. Depolama alanı tüketimi veritabanının ve Bekletme dönemi değişiklik oranına bağlıdır. Farklı bekletme dönemi 7-35 gün arasında her veritabanı için yapılandırabilirsiniz. Veri boyutu, 1 x eşit bir en az depolama miktarını ekstra ücret ödemeden sağlanır. Çoğu veritabanları için bu miktar 7 gün yedeklemelerini depolamak için yeterlidir.
- **LTR**: SQL veritabanı uzun vadeli bekletme, tam yedekleme 10 yılı aşkın yapılandırma seçeneği sunar. LTR ilkesi etkinleştirilirse, bu yedeklemeler RA-GRS depolama alanına otomatik olarak depolanır, ancak yedeklemelerin ne sıklıkta kopyalanır kontrol edebilirsiniz. Farklı bir uyumluluk gereksinimini karşılayacak şekilde, haftalık, aylık ve/veya yıllık yedekleme için farklı bekletme sürelerinin seçebilirsiniz. Bu yapılandırma, ne kadar depolama alanı LTR yedeklemeler için kullanılacak tanımlayacaksınız. LTR depolama maliyetini tahmin etmek için LTR fiyatlandırma hesaplayıcısı kullanabilirsiniz. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

## <a name="azure-hybrid-use-benefit"></a>Azure Hibrit Kullanım Teklifi

VCore tabanlı satın alma modeli (Önizleme), SQL veritabanı kullanma indirimli fiyatlar için varolan lisanslarınızı exchange [SQL Server için Azure karma kullanımı avantajı](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Bu Azure avantajı, şirket içi SQL Server lisansları Yazılım Güvencesi ile kullanarak % 30 üzerinde Azure SQL veritabanı kaydetmek için şirket içi SQL Server lisanslarınızı kullanmanıza olanak sağlar.

![fiyatlandırma](./media/sql-database-service-tiers/pricing.png)

## <a name="migration-of-single-databases-with-geo-replication-links"></a>Coğrafi Çoğaltma bağlantılarını tek veritabanlarını taşıma

DTU tabanlı modelinden vCore tabanlı modeline geçiş yükseltme veya standart ve Premium veritabanlarını coğrafi çoğaltma ilişkiler eski sürüme düşürmeyi benzer. Coğrafi çoğaltma ancak kullanıcı sonlandırma sıralama kuralları inceleyip gerekir gerektirmez. Yükseltme yaparken, ikincil veritabanı ilk yükseltin ve ardından birincil yükseltmeniz gerekir. Eski sürüme düşürmeyi, ters sırada: birincil veritabanı ilk düşürmek ve ikincil düşürmek gerekir. 

Coğrafi çoğaltma iki esnek havuzlar arasında kullanırken, birincil ve diğer – ikincil olarak olarak bir havuz belirleyin önerilir. Bu durumda, geçirme esnek havuzlar aynı kılavuzu kullanmanız gerekir.  Ancak, teknik bir esnek havuz birincil ve ikincil veritabanlarını içeren mümkün değildir. Bu durumda, düzgün bir şekilde geçiş için "birincil" olarak daha yüksek kullanımı ile havuzunun kabul eder ve sıralama kuralları buna göre izleyin.  

Aşağıdaki tabloda özel geçiş senaryoları için yönergeler sağlar: 

|Geçerli hizmet katmanı|Hedef hizmet katmanı|Geçiş türü|Kullanıcı eylemleri|
|---|---|---|---|
|Standart|Genel amaçlı|Yanal|Herhangi bir sırada geçirme ancak bir uygun vCore boyutlandırma * emin olmanız gerekir|
|Premium|İş Açısından Kritik|Yanal|Herhangi bir sırada geçirme ancak uygun vCore boyutlandırma * emin olmanız gerekir|
|Standart|İş Açısından Kritik|Yükseltme|İlk ikincil geçirmeniz gerekir|
|İş Açısından Kritik|Standart|Eski sürümü yükle|İlk birincil geçirmeniz gerekir|
|Premium|Genel amaçlı|Eski sürümü yükle|İlk birincil geçirmeniz gerekir|
|Genel amaçlı|Premium|Yükseltme|İlk ikincil geçirmeniz gerekir|
|İş Açısından Kritik|Genel amaçlı|Eski sürümü yükle|İlk birincil geçirmeniz gerekir|
|Genel amaçlı|İş Açısından Kritik|Yükseltme|İlk ikincil geçirmeniz gerekir|
||||

\* En az 1 vCore her 100 DTU standart katmanındaki gerektirir ve en az 1 vCore Premium katmanındaki 125 her DTU gerektirir

## <a name="migration-of-failover-groups"></a>Yük devretme gruplarının geçiş 

Birden fazla veritabanı olan yük devretme gruplarının geçiş birincil ve ikincil veritabanlarının ayrı geçiş gerektirir. Bu işlem sırasında sıralama kuralları ve ilgili noktaların aynısı geçerlidir. Veritabanlarını vCore tabanlı modeline dönüştürüldükten sonra Yük devretme grubu aynı ilke ayarlarıyla yürürlükte kalır. 

## <a name="creation-of-a-geo-replication-secondary"></a>Coğrafi çoğaltma ikincil oluşturma

Yalnızca birincil olarak aynı hizmet katmanı kullanarak bir coğrafi-ikincil oluşturabilirsiniz. Yüksek günlük oluşturma hızını ile daha fazla veritabanı için ikincil birincil olarak aynı performans düzeyiyle oluşturulur önerilir. Esnek havuz tek bir birincil veritabanı için bir coğrafi ikincil oluşturuyorsanız havuzu olduğunu önerilir `maxVCore` eşleşen birincil veritabanı performans düzeyi ayarı. Esnek havuz için başka bir esnek havuzdaki birincil coğrafi ikincil oluşturuyorsanız havuzlarının aynı olması önerilir `maxVCore` ayarları

## <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Veritabanı kopyasını vCore tabanlı bir veritabanı kullanarak DTU tabanlı bir veritabanı.

DTU tabanlı performans düzeyine sahip herhangi bir veritabanı bir veritabanı vCore tabanlı performans düzeyi kısıtlamaları olmadan veya özel kaynak veritabanı en fazla veritabanı boyutu hedef performans düzeyi desteklediği sürece sıralama ile kopyalayabilirsiniz. Veritabanı kopyasını kopyalama işlemi başlangıç saatini itibariyle verilerin bir anlık görüntüsünü oluşturur ve kaynak ve hedef arasında veri eşitlemeye gerçekleştirmez olmasıdır. 

## <a name="next-steps"></a>Sonraki adımlar

- Belirli performans düzeylerini ve tek veritabanı için kullanılabilir depolama boyutu seçenekleri hakkında daha fazla bilgi için bkz: [tek veritabanları için SQL veritabanı vCore tabanlı kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels)
- Esnek havuzlar için kullanılabilir boyut seçenekleri belirli performans düzeylerini ve depolama hakkında ayrıntılar için bkz [esnek havuzlar için SQL veritabanı vCore tabanlı kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels).