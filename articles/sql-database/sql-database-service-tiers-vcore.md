---
title: Sanal çekirdek - Azure SQL veritabanı hizmeti | Microsoft Docs
description: Sanal çekirdek tabanlı satın alma modeli, bağımsız olarak işlem ve depolama kaynaklarının ölçeğini, aynı şirket içi performans ve fiyat iyileştirme sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan, moslake
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 8947a34f43f09281712c0e211c3dc6b8db9da6b3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47160681"
---
# <a name="choosing-a-vcore-service-tier-compute-memory-storage-and-io-resources"></a>Sanal çekirdek hizmet katmanı seçme, bilgi işlem, bellek, depolama ve GÇ kaynakları

Sanal çekirdek tabanlı satın alma modeli, bağımsız olarak işlem ve depolama kaynaklarının ölçeğini, aynı şirket içi performans ve fiyat iyileştirme sağlar. Ayrıca, donanımın seçmenize olanak sağlar:
- Gen 4 - 24 mantıksal CPU en fazla alan Intel E5-2673 v3 (Haswell) 2,4 GHz işlemcileri, sanal çekirdek = 1 PP (fiziksel çekirdek), çekirdek başına 7 GB SSD bağlı
- 80 mantıksal CPU en fazla 5 - Gen tabanlı Intel E5-2673 v4 (Broadwell) 2,3 GHz işlemcileri, sanal çekirdek = 1 LP (hiper iş parçacığı), 5.5. Çekirdek, hızlı eNVM SSD başına GB

vCore modeli de kullanmanıza olanak verir [SQL Server için Azure hibrit kullanım teklifi](../virtual-machines/windows/hybrid-use-benefit-licensing.md) maliyet tasarrufu elde etmek için.

## <a name="service-tier-characteristics"></a>Hizmet katmanı özellikleri

İki hizmet katmanıyla genel amaçlı ve iş açısından kritik vCore modeli sağlar. Hizmet katmanları, bir dizi işlem boyutları, yüksek kullanılabilirlik tasarımı, hata yalıtımı, depolama türleri ve g/ç aralığı tarafından ayrılır. Müşteri, gerekli depolama ve saklama dönemi yedeklemeler için ayrı olarak yapılandırmanız gerekir.

Aşağıdaki tabloda, bu iki katmanı arasındaki farklar anlamanıza yardımcı olur:

||**Genel amaçlı**|**İş açısından kritik**|**Hiper ölçekli (Önizleme)**|
|---|---|---|---|
|En iyi kullanım alanı:|Çoğu iş yükü. Teklifler yönlendirilmiş Dengeli ve ölçeklenebilir işlem ve depolama seçenekleri bütçe.|Yüksek GÇ gereksinimleri olan iş uygulamaları. Çeşitli yalıtılmış çoğaltmaları kullanarak hatalara karşı en yüksek düzeyde dayanıklılık sağlar.|Çoğu iş yükü ile yüksek düzeyde ölçeklenebilir depolama ve okuma ölçek gereksinimleri|
|İşlem|4. nesil: 1-24 sanal çekirdek<br/>5. nesil: 80 1 sanal çekirdek|4. nesil: 1-24 sanal çekirdek<br/>5. nesil: 80 1 sanal çekirdek|4. nesil: 1-24 sanal çekirdek<br/>5. nesil: 80 1 sanal çekirdek|
|Bellek|4. nesil: çekirdek başına 7 GB<br>5. nesil: çekirdek başına 5.5 GB | 4. nesil: çekirdek başına 7 GB<br>5. nesil: çekirdek başına 5.5 GB |4. nesil: çekirdek başına 7 GB<br>5. nesil: çekirdek başına 5.5 GB|
|Depolama|[Premium uzak depolama](../virtual-machines/windows/premium-storage.md),<br/>Tek veritabanı: 5 GB – 4 TB<br/>Yönetilen örnek: 32 GB - 8 TB |Yerel SSD depolama<br/>Tek veritabanı: 5 GB – 4 TB<br/>Yönetilen örnek: 32 GB - 4 TB |Esnek, gerektiğinde depolama otomatik-büyütün. 100 TB depolama alanı kadar ve ötesinde destekler. Yerel SSD depolaması yerel arabellek havuzu önbellek ve yerel veri depolama. Son uzun süreli veri deposu olarak Azure uzak depolama. |
|GÇ verimliliği (yaklaşık)|Tek veritabanı: 7000 maksimum IOPS ile sanal çekirdek başına 500 IOPS</br>Yönetilen örnek: bağımlı [dosya boyutu](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes)|Çekirdek başına 5000 IOPS'yi 200.000 maksimum IOPS ile|TBD|
|Kullanılabilirlik|1 çoğaltma, herhangi bir okuma ölçek|3 çoğaltma, 1 [okuma ölçeği çoğaltma](sql-database-read-scale-out.md),<br/>Bölge yedekli HA|?|
|Yedeklemeler|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 gün (varsayılan olarak 7 gün)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 gün (varsayılan olarak 7 gün)|Anlık görüntü tabanlı Yedekleme Azure uzak depolama ve geri yükler, bu anlık görüntüler Hızlı Kurtarma için kullanın. Yedeklemeler anlıktır ve işlem GÇ performansını etkilemez. Geri yüklemeler çok hızlı ve değil veri işlemleri boyutunu (dakika cinsinden saat/gün).|
|Bellek içi|Desteklenmiyor|Desteklenen|Desteklenmiyor|
|||

Daha fazla bilgi için [tek veritabanı sanal çekirdek kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md) ve [yönetilen örneği'nde sanal çekirdek kaynak sınırları](sql-database-managed-instance.md#vcore-based-purchasing-model). 

> [!IMPORTANT]
> DTU tabanlı satın alma modeli, bilgi işlem kapasitesine saatten daha az sanal çekirdek gerekiyorsa kullanın.

Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için. 

## <a name="storage-considerations"></a>Depolama hakkında dikkat edilmesi gerekenler

### <a name="general-purpose-and-business-critical-service-tiers"></a>Genel amaçlı ve iş açısından kritik hizmet katmanları
Aşağıdaki topluluklara bir göz atın:
- Ayrılmış depolama, veri dosyaları (MDF) ve günlük dosyalarını (LDF) tarafından kullanılır.
- Varsayılan en büyük boyutu 32 GB olan bir maksimum veritabanı boyutu her tek veritabanı işlem boyutu destekler.
- Gereken tek veritabanı boyutu (MDF boyutu) yapılandırırken, ek depolama alanı % 30'luk LDF desteklemek için otomatik olarak eklenir
- Yönetilen örnek depolama boyutu, 32 GB'ın katları şeklinde belirtilmelidir.
- Desteklenen en fazla 10 GB arasındaki herhangi bir tek veritabanı boyutu seçebilirsiniz.
 - Standart depolama için artırmak veya azaltmak boyutu 10 GB'lık artışlarla
 - Premium depolama için artırmak veya azaltmak boyutu 250 GB'lık artışlarla
- Genel amaçlı hizmet katmanındaki `tempdb` ekli bir SSD ve bu depolama maliyeti sanal çekirdek fiyatına dahil kullanır.
- İş açısından kritik hizmet katmanındaki `tempdb` paylaşımları ekli SSD MDF ve LDF dosyaları ve tempDB depolama maliyeti sanal çekirdek fiyatı yer almaktadır.

> [!IMPORTANT]
> MDF ve LDF için ayrılan toplam depolama alanı için ücretlendirilirsiniz.

MDF ve LDF geçerli toplam boyutunu izlemek için kullanabilirsiniz [bilgilerini sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Tek tek MDF ve LDF dosyaları geçerli boyutunu izlemek için kullanabilirsiniz [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

### <a name="hyperscale-service-tier-preview"></a>Hiper ölçekli hizmet Katmanı (Önizleme)

İçin hiper ölçekli veritabanı otomatik olarak yönetilen bir depolama alanıdır. Depolama, gerektiği şekilde genişler. Gerekli hiç sık kullanılan günlük kesme ile hızlı Azure premium depolama SSD depolama "Sonsuz günlüğü".

## <a name="backups-and-storage"></a>Yedekleme ve depolama

### <a name="general-purpose-and-business-critical-service-tiers"></a>Genel amaçlı ve iş açısından kritik hizmet katmanları

Veritabanı Yedeklemeleri için depolama noktası zaman geri yükleme (PITR içinde) desteklemek için ayrılır ve [uzun süreli saklama (LTR)](sql-database-long-term-retention.md) SQL veritabanı özellikleri. Bu depolama alanı ayrı ayrı her veritabanı için ayrılan ve iki ayrı veritabanı başına ücret üzerinden faturalandırılırsınız. 

- **PITR**: tek tek veritabanı yedeklemeleri kopyalanır [RA-GRS depolama](../storage/common/storage-designing-ha-apps-with-ragrs.md) otomatik olarak. Oluşturulan yeni yedekleme depolama alanı boyutu dinamik olarak artar.  Depolama alanı, haftalık tam yedeklemeler, günlük fark yedekleri ve 5 dakikada bir kopyalanan işlem günlüğü yedeklemeleri tarafından kullanılır. Depolama alanı tüketimi, veritabanının ve saklama dönemi değişiklik oranına bağlıdır. 7-35 gün arasında her veritabanı için ayrı tutma süresine yapılandırabilirsiniz. Veri boyutu 1 x ile eşit bir en düşük depolama alanı miktarı, ek ücret alınmadan sağlanır. Çoğu veritabanı için bu miktar 7 güne kadar yedek depolamak için yeterli olacaktır.
- **LTR**: SQL veritabanı için 10 yıla kadar uzun süreli saklama yedeklerini tam yapılandırma seçeneği sunar. LTR ilkesi etkinleştirilirse, bu yedeklemeler otomatik olarak RA-GRS depolama alanında depolanır, ancak sıklıkla yedeklemeleri kopyalanır denetleyebilirsiniz. Farklı bir uyumluluk gereksinimini karşılamak için haftalık, aylık ve/veya yıllık yedeklemeler için farklı bekletme sürelerinin seçebilirsiniz. Bu yapılandırma, ne kadar depolama alanı LTR yedeklemeleri için kullanılacak tanımlayacaksınız. LTR fiyatlandırma hesaplayıcısını LTR depolama maliyetini tahmin etmek için kullanabilirsiniz. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

### <a name="hyperscale-service-tier-preview"></a>Hiper ölçekli hizmet Katmanı (Önizleme)

Anlık görüntü tabanlı Yedekleme Azure uzak depolama ve geri yükler, bu anlık görüntüler Hızlı Kurtarma için kullanın. Yedeklemeler anlıktır ve işlem GÇ performansını etkilemez. Geri yüklemeler çok hızlı ve değil veri işlemleri boyutunu (dakika cinsinden saat/gün).

## <a name="azure-hybrid-use-benefit"></a>Azure Hibrit Kullanım Teklifi

Sanal çekirdek tabanlı satın alma modeli, mevcut lisanslarınızı kullanarak SQL veritabanı üzerinde indirimli fiyatlar için exchange [SQL Server için Azure hibrit kullanım teklifi](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Bu Azure avantajını Yazılım Güvencesi içeren şirket içi SQL Server lisanslarınızı kullanarak Azure SQL veritabanı'nda % 30 kaydetmek için şirket içi SQL Server lisanslarınızı kullanmanıza olanak tanır.

![fiyatlandırma](./media/sql-database-service-tiers/pricing.png)

## <a name="migration-from-dtu-model-to-vcore-model"></a>VCore modeli DTU modeline geçiş

### <a name="migration-of-single-databases-with-geo-replication-links"></a>Coğrafi Çoğaltma bağlantılarını içeren tek bir veritabanı geçişi

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

### <a name="migration-of-failover-groups"></a>Yük devretme grupları geçişi 

Yük devretme grupları ile birden çok veritabanı geçişi birincil ve ikincil veritabanlarını tek tek geçişini gerektirir. Bu işlem sırasında aynı önemli noktalar ve sıralama kuralları geçerlidir. Veritabanları, sanal çekirdek tabanlı modele dönüştürüldükten sonra Yük devretme grubu aynı ilke ayarlarıyla yürürlükte kalır. 

### <a name="creation-of-a-geo-replication-secondary"></a>Coğrafi çoğaltma ikincil oluşturma

Yalnızca birincil olarak aynı hizmet katmanını kullanarak bir coğrafi-ikincil oluşturabilirsiniz. İçin bir veritabanı yüksek günlük oluşturma hızı ile aynı işlem boyutu birincil olarak ikincil yaratılırken önerilir. Tek bir birincil veritabanı için elastik havuzdaki bir coğrafi-ikincil oluşturuyorsanız, havuz olduğunu önerilir `maxVCore` ayarı, birincil veritabanı işlem boyutu eşleşir. Esnek havuz başka bir elastik havuzdaki bir birincil site için bir coğrafi-ikincil oluşturuyorsanız havuzlarının aynı olduğunu önerilir `maxVCore` ayarları

### <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Veritabanı kopyası kullanarak sanal çekirdek tabanlı bir veritabanı için DTU tabanlı bir veritabanı dönüştürmek.

DTU tabanlı işlem boyutu olan herhangi bir veritabanı sanal çekirdek tabanlı işlem boyutu kısıtlamaları veya kaynak veritabanının en büyük veritabanı boyutu hedef işlem boyutu desteklediği sürece özel sıralaması olmayan bir veritabanına kopyalayabilirsiniz. Veritabanı kopyalama kopyalama işleminin başlangıç tarihindeki verileri anlık görüntüsünü oluşturur ve kaynak ve hedef arasında veri eşitlemeye gerçekleştirmez olmasıdır. 

## <a name="next-steps"></a>Sonraki adımlar

- Özel hakkında ayrıntılı bilgi işlem boyutları ve tek veritabanı için kullanılabilir depolama boyutu seçenekleri için bkz: [tek veritabanları için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes)
- Özel hakkında ayrıntılı bilgi işlem boyutlarına ve elastik havuzlar için kullanılabilir depolama boyutu seçenekleri görmek için [elastik havuzlar için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
