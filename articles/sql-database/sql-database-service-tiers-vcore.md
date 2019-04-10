---
title: Sanal çekirdek - Azure SQL veritabanı hizmeti | Microsoft Docs
description: Sanal çekirdek tabanlı satın alma modeli, bağımsız olarak işlem ve depolama kaynaklarının ölçeğini, aynı şirket içi performans ve fiyat iyileştirme sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: edba858f9be3350034ff48ea16d3c9137254bb97
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59357939"
---
# <a name="vcore-service-tiers-azure-hybrid-benefit-and-migration"></a>Sanal çekirdek hizmet katmanları, Azure hibrit avantajı ve geçiş

Sanal çekirdek tabanlı satın alma modeli, bağımsız olarak işlem ve depolama kaynaklarının ölçeğini, aynı şirket içi performans ve fiyat iyileştirme sağlar. Ayrıca, donanımın seçmenize olanak sağlar:

- 4-24 mantıksal CPU'lar Intel E5-2673 v3 temel kadar. nesil (Haswell) 2,4 GHz işlemcileri, sanal çekirdek = 1 bağlı PP (fiziksel çekirdek), çekirdek başına 7 GB SSD
- 5-kadar 80 mantıksal CPU'lar Intel E5-2673 v4 temel alır. nesil (Broadwell) 2,3 GHz işlemcileri, sanal çekirdek = 1 LP (hiper iş parçacığı), çekirdek, hızlı eNVM SSD başına 5.1 GB

Donanım 4. nesil sanal çekirdek başına önemli ölçüde daha fazla bellek sunar. Ancak, 5. nesil donanım çok daha yüksek bilgi işlem kaynaklarını ölçeklendirme olanak tanır.

> [!NOTE]
> DTU tabanlı hizmet katmanları hakkında daha fazla bilgi için bkz: [DTU tabanlı hizmet katmanları](sql-database-service-tiers-dtu.md). DTU tabanlı hizmet katmanları ve sanal çekirdek tabanlı hizmet katmanları ayrım yapma hakkında daha fazla bilgi için bkz: [Azure SQL veritabanı'nın satın alma modeli](sql-database-purchase-models.md).

## <a name="service-tier-characteristics"></a>Hizmet katmanı özellikleri

Üç hizmet katmanı genel amaçlı, Hiper ölçekli ve iş açısından kritik vCore modeli sağlar. Hizmet katmanları, bir dizi işlem boyutları, yüksek kullanılabilirlik tasarımı, hata yalıtımı, türler ve depolama alanının boyutunu ve g/ç aralığı tarafından ayrılır. Ayrı olarak, gerekli depolama ve saklama dönemi yedeklemeleri için yapılandırmanız gerekir. Azure portalında (veritabanı değil) sunucusuna gidin > Yönetilen yedekleme > ilkesini yapılandırma > noktası içinde zaman geri yükleme yapılandırması > 7-35 gün.

Aşağıdaki tabloda, üç katmanı arasındaki farkları yardımcı olur:

||**Genel Amaçlı**|**İş Açısından Kritik**|**Hiper ölçekli (Önizleme)**|
|---|---|---|---|
|En uygun olduğu durum|Çoğu iş yükü. Teklifler yönlendirilmiş Dengeli ve ölçeklenebilir işlem ve depolama seçenekleri bütçe.|Yüksek GÇ gereksinimleri olan iş uygulamaları. Çeşitli yalıtılmış çoğaltmaları kullanarak hatalara karşı en yüksek düzeyde dayanıklılık sağlar.|Çoğu iş yükü ile yüksek düzeyde ölçeklenebilir depolama ve okuma ölçek gereksinimleri|
|İşlem|4. nesil: 1-24 sanal çekirdek<br/>5. nesil: 1 ila 80 sanal çekirdek|4. nesil: 1-24 sanal çekirdek<br/>5. nesil: 1 ila 80 sanal çekirdek|4. nesil: 1-24 sanal çekirdek<br/>5. nesil: 1 ila 80 sanal çekirdek|
|Bellek|4. nesil: Çekirdek başına 7 GB<br>5. nesil: Çekirdek başına 5.1 GB | 4. nesil: Çekirdek başına 7 GB<br>5. nesil: Çekirdek başına 5.1 GB |4. nesil: Çekirdek başına 7 GB<br>5. nesil: Çekirdek başına 5.1 GB|
|Depolama alanı|Uzaktan Depolama kullanıyor:<br/>Tek veritabanı: 5 GB – 4 TB<br/>Yönetilen örnek: 32 GB - 8 TB |Yerel SSD depolama kullanır:<br/>Tek veritabanı: 5 GB – 4 TB<br/>Yönetilen örnek: 32 GB - 4 TB |Esnek, otomatik olarak büyütme gerektiğinde depolama. 100 TB depolama alanı kadar ve ötesinde destekler. Yerel SSD depolaması yerel arabellek havuzu önbellek ve yerel veri depolama. Son uzun süreli veri deposu olarak Azure uzak depolama. |
|GÇ verimliliği (yaklaşık)|Tek veritabanı: 7000 maksimum IOPS ile sanal çekirdek başına 500 IOPS</br>Yönetilen örnek: Bağımlı [dosya boyutu](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes)|Çekirdek başına 5000 IOPS'yi 200.000 maksimum IOPS ile|TBD|
|Kullanılabilirlik|1 çoğaltma, herhangi bir okuma ölçek|3 çoğaltma, 1 [okuma ölçeği çoğaltma](sql-database-read-scale-out.md),<br/>Bölge yedekli HA|?|
|Yedeklemeler|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 gün (varsayılan olarak 7 gün)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 gün (varsayılan olarak 7 gün)|Anlık görüntü tabanlı Yedekleme Azure uzak depolama ve geri yükler, bu anlık görüntüler Hızlı Kurtarma için kullanın. Yedeklemeler anlıktır ve işlem GÇ performansını etkilemez. Geri yüklemeler çok hızlıdır ve bir veri işlemi (dakika yerine saatler veya günler katılarak) boyutunu değildir.|
|Bellek içi|Desteklenmiyor|Desteklenen|Desteklenmiyor|
|||

> [!NOTE]
> Azure keşfetmek için ücretsiz bir Azure hesabı ile birlikte temel bir hizmet katmanında, ücretsiz bir Azure SQL veritabanı alabilirsiniz. Bilgi için [Azure ücretsiz hesabınızla yönetilen bir bulut veritabanı oluşturun](https://azure.microsoft.com/free/services/sql-database/).

- Daha fazla bilgi için [tek veritabanı sanal çekirdek kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md) ve [yönetilen örneği'nde sanal çekirdek kaynak sınırları](sql-database-managed-instance.md#vcore-based-purchasing-model).
- Genel amaçlı ve iş açısından kritik hizmet katmanları hakkında daha fazla bilgi için bkz. [genel amaçlı ve iş açısından kritik hizmet katmanlarına](sql-database-service-tiers-general-purpose-business-critical.md).
- Hiper ölçekli bir hizmet katmanındaki sanal çekirdek tabanlı satın alma modeli hakkında daha fazla bilgi için bkz: [hiper ölçekli hizmet katmanı](sql-database-service-tier-hyperscale.md).  

> [!IMPORTANT]
> DTU tabanlı satın alma modeli, bilgi işlem kapasitesine saatten daha az sanal çekirdek gerekiyorsa kullanın.

## <a name="azure-hybrid-benefit"></a>Azure Hibrit Avantajı

Sanal çekirdek tabanlı satın alma modeli, mevcut lisanslarınızı kullanarak SQL veritabanı üzerinde indirimli fiyatlar için exchange [SQL Server için Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/). Bu Azure avantajını Yazılım Güvencesi içeren şirket içi SQL Server lisanslarınızı kullanarak Azure SQL veritabanı'nda % 30 kaydetmek için şirket içi SQL Server lisanslarınızı kullanmanıza olanak tanır.

![fiyatlandırma](./media/sql-database-service-tiers/pricing.png)

Azure karma avantajı ile SQL veritabanı altyapısı için kendi mevcut SQL Server lisansınızı kullanarak temel Azure altyapısını karşılığında Ödeme yapmalarını sağlayan seçebilirsiniz (**BasePrice**) veya her iki temel alınan altyapı için ödeme ve SQL Server Lisansı (**LicenseIncluded**). Azure portalını kullanarak ya da aşağıdaki API'leri kullanarak, lisans modeli değiştirmek ya da seçin.

- Ayarlamak veya PowerShell kullanarak lisans türünü güncelleştirmek için:

  - [Yeni AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase):
  - [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql)
  - [Yeni AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)
  - [Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql)

- Azure CLI'yı kullanarak lisans türünü güncelleştirmek veya ayarlamak için:

  - [az sql db oluşturun](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create)
  - [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update)
  - [az sql mı oluşturma](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create)
  - [az sql mı güncelleştirme](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)

- Ayarlamak veya REST API'yi kullanarak lisans türünü güncelleştirmek için:

  - [Veritabanları - oluştur veya güncelleştir](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)
  - [Veritabanları - güncelleştirme](https://docs.microsoft.com/rest/api/sql/databases/update)
  - [Yönetilen örnekler - oluştur veya güncelleştir](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)
  - [Yönetilen örnekleri - güncelleştirme](https://docs.microsoft.com/rest/api/sql/managedinstances/update)

## <a name="migration-from-dtu-model-to-vcore-model"></a>VCore modeli DTU modeline geçiş

### <a name="migration-of-a-database"></a>Bir veritabanı geçişi

Sanal çekirdek tabanlı satın alma modeli için DTU tabanlı satın alma modeli bir veritabanını geçirme, yükseltme veya indirgeme DTU tabanlı satın alma modeli, standart ve Premium veritabanları arasında benzerdir.

### <a name="migration-of-databases-with-geo-replication-links"></a>Coğrafi Çoğaltma bağlantılarını içeren bir veritabanı geçişi

DTU tabanlı modeli sanal çekirdek tabanlı modele geçiş, yükseltme veya indirgeme standart ve Premium veritabanları arasındaki coğrafi çoğaltma ilişkileri için benzerdir. Coğrafi çoğaltma, ancak kullanıcı sonlandırma sıralama kurallara uymanız gerekir gerektirmez. Yükseltme sırasında ikincil veritabanı yükseltmeniz ve ardından birincil yükseltmeniz gerekir. Önceki sürüme indirirken ters sırada: birincil veritabanının ilk sürümüne düşürürseniz ve ardından ikincil düşürme gerekir.

İki elastik havuzlar arasında coğrafi çoğaltmayı kullanarak, bir havuz olarak birincil ve diğer – ikincil olarak belirlemeniz önerilir. Bu durumda, geçirme elastik havuzlar, aynı kılavuzu kullanmanız gerekir.  Ancak, teknik olarak birincil ve ikincil veritabanları elastik havuz içerdiğini mümkün değildir. Bu durumda, düzgün bir şekilde geçiş için "birincil" olarak daha yüksek kullanımı havuz işle ve sıralama kuralları buna göre izleyin.  

Aşağıdaki tablo, belirli bir geçiş senaryoları için yönergeler sağlar:

|Geçerli hizmet katmanı|Hedef hizmet katmanı|Geçiş türü|Kullanıcı eylemleri|
|---|---|---|---|
|Standart|Genel amaçlı|Yanal|Herhangi bir sırada geçirme ancak bir uygun sanal çekirdek boyutlandırma * emin olmanız gerekir|
|Premium|İş Açısından Kritik|Yanal|Herhangi bir sırada geçirme ancak uygun sanal çekirdek boyutlandırma * emin olmanız gerekir|
|Standart|İş Açısından Kritik|Yükselt|Öncelikle ikincil geçirmeniz gerekir|
|İş Açısından Kritik|Standart|Eski sürümü yükle|Öncelikle birincil geçirmeniz gerekir|
|Premium|Genel amaçlı|Eski sürümü yükle|Öncelikle birincil geçirmeniz gerekir|
|Genel amaçlı|Premium|Yükselt|Öncelikle ikincil geçirmeniz gerekir|
|İş Açısından Kritik|Genel amaçlı|Eski sürümü yükle|Öncelikle birincil geçirmeniz gerekir|
|Genel amaçlı|İş Açısından Kritik|Yükselt|Öncelikle ikincil geçirmeniz gerekir|
||||

\* Her 100 DTU standart katmanda en az 1 sanal çekirdek gerektirir ve en az 1 sanal çekirdek her Premium katmanda 125 DTU gerektirir

### <a name="migration-of-failover-groups"></a>Yük devretme grupları geçişi

Yük devretme grupları ile birden çok veritabanı geçişi birincil ve ikincil veritabanlarını tek tek geçişini gerektirir. Bu işlem sırasında aynı önemli noktalar ve sıralama kuralları geçerlidir. Veritabanları, sanal çekirdek tabanlı modele dönüştürüldükten sonra Yük devretme grubu aynı ilke ayarlarıyla yürürlükte kalır.

### <a name="creation-of-a-geo-replication-secondary"></a>Coğrafi çoğaltma ikincil oluşturma

Yalnızca birincil olarak aynı hizmet katmanını kullanarak bir coğrafi-ikincil oluşturabilirsiniz. İçin bir veritabanı yüksek günlük oluşturma hızı ile aynı işlem boyutu birincil olarak ikincil yaratılırken önerilir. Tek bir birincil veritabanı için elastik havuzdaki bir coğrafi-ikincil oluşturuyorsanız, havuz olduğunu önerilir `maxVCore` ayarı, birincil veritabanı işlem boyutu eşleşir. Esnek havuz başka bir elastik havuzdaki bir birincil site için bir coğrafi-ikincil oluşturuyorsanız havuzlarının aynı olduğunu önerilir `maxVCore` ayarları

### <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Sanal çekirdek tabanlı bir veritabanı için DTU tabanlı bir veritabanı dönüştürmek için veritabanı kopyalama kullanma

DTU tabanlı işlem boyutu olan herhangi bir veritabanı sanal çekirdek tabanlı işlem boyutu kısıtlamaları veya kaynak veritabanının en büyük veritabanı boyutu hedef işlem boyutu desteklediği sürece özel sıralaması olmayan bir veritabanına kopyalayabilirsiniz. Veritabanı kopyalama, kopyalama işleminin başlangıç tarihindeki verileri anlık görüntüsünü oluşturur ve kaynak ve hedef arasında veri eşitlemeye gerçekleştirmez.

## <a name="next-steps"></a>Sonraki adımlar

- Özel hakkında ayrıntılı bilgi işlem boyutları ve tek veritabanı için kullanılabilir depolama boyutu seçenekleri için bkz: [tek veritabanları için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-storage-sizes-and-compute-sizes)
- Özel hakkında ayrıntılı bilgi işlem boyutları ve elastik havuzlar için kullanılabilir depolama boyutu seçenekleri için bkz: [elastik havuzlar için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).
