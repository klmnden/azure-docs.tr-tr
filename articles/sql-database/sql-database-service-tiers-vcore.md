---
title: Sanal çekirdek - Azure SQL veritabanı hizmeti | Microsoft Docs
description: Sanal çekirdek tabanlı satın alma modeli, bağımsız olarak işlem ve depolama kaynaklarının ölçeğini, aynı şirket içi performans ve fiyat iyileştirme sağlar.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 07/16/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: d18076486704d5f03acd2253650762c3bd24b0af
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091501"
---
# <a name="choosing-a-vcore-service-tier-compute-memory-storage-and-io-resources"></a>Sanal çekirdek hizmet katmanı seçme, bilgi işlem, bellek, depolama ve GÇ kaynakları

Hizmet katmanları, bir dizi performans düzeyleri, yüksek kullanılabilirlik tasarımı, hata yalıtımı, depolama türleri ve g/ç aralığı tarafından ayrılır. Müşteri, gerekli depolama ve saklama dönemi yedeklemeler için ayrı olarak yapılandırmanız gerekir. Sanal çekirdek modeli ile tek veritabanları ve elastik havuzlar için yüzde 30 kazanım için uygun yedekleme [SQL Server için Azure hibrit kullanım teklifi](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

Aşağıdaki tabloda, bu iki katmanı arasındaki farklar anlamanıza yardımcı olur:

||**Genel amaçlı**|**İş açısından kritik**|
|---|---|---|
|En iyi kullanım alanı:|Çoğu iş yükü. Teklifler yönlendirilmiş Dengeli ve ölçeklenebilir işlem ve depolama seçenekleri bütçe.|Yüksek GÇ gereksinimleri olan iş uygulamaları. Çeşitli yalıtılmış çoğaltmaları kullanarak hatalara karşı en yüksek düzeyde dayanıklılık sağlar.|
|İşlem|1 ila 80 sanal çekirdek, 4. nesil ve 5. nesil |1 ila 80 sanal çekirdek, 4. nesil ve 5. nesil|
|Bellek|4. nesil: çekirdek başına 7 GB<br>5. nesil: çekirdek başına 5.5 GB | 4. nesil: çekirdek başına 7 GB<br>5. nesil: çekirdek başına 5.5 GB |
|Depolama|Uzak Premium depolama, 5 GB – 4 TB|5 GB – 4 TB'a kadar yerel SSD depolama|
|GÇ verimliliği (yaklaşık)|7000 maksimum IOPS ile sanal çekirdek başına 500 IOPS|Çekirdek başına 5000 IOPS'yi 200000 maksimum IOPS ile|
|Kullanılabilirlik|1 çoğaltma, herhangi bir okuma ölçek|3 çoğaltma, 1 [okuma ölçeği](sql-database-read-scale-out.md), yedekli HA bölge|
|Yedeklemeler|RA-GRS, 7-35 gün (varsayılan olarak 7 gün)|RA-GRS, 7-35 gün (varsayılan olarak 7 gün)|
|Bellek içi|Yok|Desteklenen|
|||

> [!IMPORTANT]
> DTU tabanlı satın alma modeli, bilgi işlem kapasitesine saatten daha az sanal çekirdek gerekiyorsa kullanın.

Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için. 

## <a name="storage-considerations"></a>Depolama hakkında dikkat edilmesi gerekenler

Aşağıdaki topluluklara bir göz atın:
- Ayrılmış depolama, veri dosyaları (MDF) ve günlük dosyalarını (LDF) tarafından kullanılır.
- Her performans düzeyi varsayılan en büyük boyutu 32 GB olan bir maksimum veritabanı boyutu destekler.
- İstenen veritabanı boyutu (MDF boyutu) yapılandırırken, ek depolama alanı % 30'luk LDF desteklemek için otomatik olarak eklenir
- Herhangi bir veritabanı boyutu 10 GB ve desteklenen en yüksek arasında seçebilirsiniz.
 - Standart depolama için artırmak veya azaltmak boyutu 10 GB'lık artışlarla
 - Premium depolama için artırmak veya azaltmak boyutu 250 GB'lık artışlarla
- Genel amaçlı hizmet katmanındaki `tempdb` ekli bir SSD ve bu depolama maliyeti sanal çekirdek fiyatına dahil kullanır.
- İş açısından kritik hizmet katmanındaki `tempdb` paylaşımları ekli SSD MDF ve LDF dosyaları ve tempDB depolama maliyeti sanal çekirdek fiyatı yer almaktadır.

> [!IMPORTANT]
> MDF ve LDF için ayrılan toplam depolama alanı için ücretlendirilirsiniz.

MDF ve LDF geçerli toplam boyutunu izlemek için kullanabilirsiniz [bilgilerini sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Tek tek MDF ve LDF dosyaları geçerli boyutunu izlemek için kullanabilirsiniz [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

## <a name="backups-and-storage"></a>Yedekleme ve depolama

Veritabanı Yedeklemeleri için depolama, SQL veritabanı'nın zaman geri yükleme (PITR) ve uzun süreli saklama (LTR) özelliklerinde noktası desteklemek için ayrılır. Bu depolama alanı ayrı ayrı her veritabanı için ayrılan ve iki ayrı veritabanı başına ücret üzerinden faturalandırılırsınız. 

- **PITR**: olan otomatik olarak tek tek veritabanı yedeklemeleri RA-GRS depolama alanına kopyalanır. Oluşturulan yeni yedekleme depolama alanı boyutu dinamik olarak artar.  Depolama alanı, haftalık tam yedeklemeler, günlük fark yedekleri ve 5 dakikada bir kopyalanan işlem günlüğü yedeklemeleri tarafından kullanılır. Depolama alanı tüketimi, veritabanının ve saklama dönemi değişiklik oranına bağlıdır. 7-35 gün arasında her veritabanı için ayrı tutma süresine yapılandırabilirsiniz. Veri boyutu 1 x ile eşit bir en düşük depolama alanı miktarı, ek ücret alınmadan sağlanır. Çoğu veritabanı için bu miktar 7 güne kadar yedek depolamak için yeterli olacaktır.
- **LTR**: SQL veritabanı için 10 yıla kadar uzun süreli saklama yedeklerini tam yapılandırma seçeneği sunar. LTR ilkesi etkinleştirilirse, bu yedeklemeler otomatik olarak RA-GRS depolama alanında depolanır, ancak sıklıkla yedeklemeleri kopyalanır denetleyebilirsiniz. Farklı bir uyumluluk gereksinimini karşılamak için haftalık, aylık ve/veya yıllık yedeklemeler için farklı bekletme sürelerinin seçebilirsiniz. Bu yapılandırma, ne kadar depolama alanı LTR yedeklemeleri için kullanılacak tanımlayacaksınız. LTR fiyatlandırma hesaplayıcısını LTR depolama maliyetini tahmin etmek için kullanabilirsiniz. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

## <a name="azure-hybrid-use-benefit"></a>Azure Hibrit Kullanım Teklifi

Sanal çekirdek tabanlı satın alma modeli, mevcut lisanslarınızı kullanarak SQL veritabanı üzerinde indirimli fiyatlar için exchange [SQL Server için Azure hibrit kullanım teklifi](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Bu Azure avantajını Yazılım Güvencesi içeren şirket içi SQL Server lisanslarınızı kullanarak Azure SQL veritabanı'nda % 30 kaydetmek için şirket içi SQL Server lisanslarınızı kullanmanıza olanak tanır.

![fiyatlandırma](./media/sql-database-service-tiers/pricing.png)

## <a name="migration-of-single-databases-with-geo-replication-links"></a>Coğrafi Çoğaltma bağlantılarını içeren tek bir veritabanı geçişi

İçin DTU tabanlı modeli sanal çekirdek tabanlı modele geçiş, yükseltme veya indirgeme standart ve Premium veritabanları arasındaki coğrafi çoğaltma ilişkileri için benzerdir. Coğrafi çoğaltma, ancak kullanıcı sonlandırma sıralama kurallara uymanız gerekir gerektirmez. Yükseltme sırasında ikincil veritabanı yükseltmeniz ve ardından birincil yükseltmeniz gerekir. Önceki sürüme indirirken ters sırada: birincil veritabanının ilk sürümüne düşürürseniz ve ardından ikincil düşürme gerekir. 

İki elastik havuzlar arasında coğrafi çoğaltmayı kullanarak, bir havuz olarak birincil ve diğer – ikincil olarak belirlemeniz önerilir. Bu durumda, geçirme elastik havuzlar, aynı kılavuzu kullanmanız gerekir.  Ancak, teknik olarak birincil ve ikincil veritabanları elastik havuz içerdiğini mümkün değildir. Bu durumda, düzgün bir şekilde geçiş için "birincil" olarak daha yüksek kullanımı havuz işle ve sıralama kuralları buna göre izleyin.  

Aşağıdaki tablo, belirli bir geçiş senaryoları için yönergeler sağlar: 

|Geçerli hizmet katmanı|Hedef hizmet katmanı|Geçiş türü|Kullanıcı eylemleri|
|---|---|---|---|
|Standart|Genel amaçlı|Yanal|Herhangi bir sırada geçirme ancak bir uygun sanal çekirdek boyutlandırma * emin olmanız gerekir|
|Premium|İş Açısından Kritik|Yanal|Herhangi bir sırada geçirme ancak uygun sanal çekirdek boyutlandırma * emin olmanız gerekir|
|Standart|İş Açısından Kritik|Yükseltme|Öncelikle ikincil geçirmeniz gerekir|
|İş Açısından Kritik|Standart|Eski sürümü yükle|Öncelikle birincil geçirmeniz gerekir|
|Premium|Genel amaçlı|Eski sürümü yükle|Öncelikle birincil geçirmeniz gerekir|
|Genel amaçlı|Premium|Yükseltme|Öncelikle ikincil geçirmeniz gerekir|
|İş Açısından Kritik|Genel amaçlı|Eski sürümü yükle|Öncelikle birincil geçirmeniz gerekir|
|Genel amaçlı|İş Açısından Kritik|Yükseltme|Öncelikle ikincil geçirmeniz gerekir|
||||

\* Her 100 DTU standart katmanda en az 1 sanal çekirdek gerektirir ve en az 1 sanal çekirdek her Premium katmanda 125 DTU gerektirir

## <a name="migration-of-failover-groups"></a>Yük devretme grupları geçişi 

Yük devretme grupları ile birden çok veritabanı geçişi birincil ve ikincil veritabanlarını tek tek geçişini gerektirir. Bu işlem sırasında aynı önemli noktalar ve sıralama kuralları geçerlidir. Veritabanları, sanal çekirdek tabanlı modele dönüştürüldükten sonra Yük devretme grubu aynı ilke ayarlarıyla yürürlükte kalır. 

## <a name="creation-of-a-geo-replication-secondary"></a>Coğrafi çoğaltma ikincil oluşturma

Yalnızca birincil olarak aynı hizmet katmanını kullanarak bir coğrafi-ikincil oluşturabilirsiniz. İçin bir veritabanı yüksek günlük oluşturma hızı ile aynı performans düzeyinde birincil ile ikincil oluşturulduğunu önerilir. Tek bir birincil veritabanı için elastik havuzdaki bir coğrafi-ikincil oluşturuyorsanız, havuz olduğunu önerilir `maxVCore` ayarı birincil veritabanının performans düzeyini eşleşir. Esnek havuz başka bir elastik havuzdaki bir birincil site için bir coğrafi-ikincil oluşturuyorsanız havuzlarının aynı olduğunu önerilir `maxVCore` ayarları

## <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Veritabanı kopyası kullanarak sanal çekirdek tabanlı bir veritabanı için DTU tabanlı bir veritabanı dönüştürmek.

DTU tabanlı performans düzeyine sahip herhangi bir veritabanı sanal çekirdek tabanlı performans düzeyi kısıtlama olmadan veya özel kaynak veritabanının en büyük veritabanı boyutu hedef performans düzeyi desteklediği sürece sıralaması veritabanına kopyalayabilirsiniz. Veritabanı kopyalama kopyalama işleminin başlangıç tarihindeki verileri anlık görüntüsünü oluşturur ve kaynak ve hedef arasında veri eşitlemeye gerçekleştirmez olmasıdır. 

## <a name="next-steps"></a>Sonraki adımlar

- Belirli bir performans düzeyleri ve depolama boyutu tek veritabanı için kullanılabilir seçenekler hakkında daha fazla bilgi için bkz: [tek veritabanları için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels)
- Elastik havuzlar için boyut seçenekleri belirli bir performans düzeyleri ve depolama hakkında ayrıntılı bilgi için bkz [elastik havuzlar için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels).
