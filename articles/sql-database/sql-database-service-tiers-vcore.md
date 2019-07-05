---
title: Sanal çekirdek - Azure SQL veritabanı hizmeti | Microsoft Docs
description: Sanal çekirdek tabanlı satın alma modeli, bağımsız olarak işlem ve depolama kaynaklarının ölçeğini, aynı şirket içi performans ve fiyat iyileştirme olanak tanır.
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
ms.date: 06/26/2019
ms.openlocfilehash: e9d1ce3bcd3bf958be0a7837e8416300af03f5a2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449743"
---
# <a name="choose-among-the-vcore-service-tiers-and-migrate-from-the-dtu-service-tiers"></a>Sanal çekirdek hizmet katmanları seçin ve DTU hizmet katmanı arasından geçirme

Sanal çekirdek (vCore)-tabanlı satın alma modeli bağımsız olarak ölçeklendirme işlem ve depolama kaynakları, aynı şirket içi performans ve fiyat iyileştirmenize olanak tanır. Ayrıca donanımın seçmenizi sağlar:

- **4. nesil**: En fazla 24 mantıksal CPU'lar Intel E5-2673 v3 (Haswell) 2,4 GHz işlemcileri, sanal çekirdek tabanlı = 1 bağlı PP (fiziksel çekirdek), çekirdek başına 7 GB SSD
- **5. nesil**: En fazla 80 mantıksal CPU'lar Intel E5-2673 v4 (Broadwell) 2.3 GHz işlemcileri, sanal çekirdek tabanlı 1 LP (hiper iş parçacığı), çekirdek, hızlı eNVM SSD başına 5.1 GB =

Donanım 4. nesil sanal çekirdek başına önemli ölçüde daha fazla bellek sunar. Ancak, 5. nesil donanım çok daha yüksek bilgi işlem kaynaklarını ölçeklendirme olanak tanır.

> [!IMPORTANT]
> Yeni 4. nesil veritabanları, AustraliaEast bölgesinde artık desteklenmemektedir.
> [!NOTE]
> DTU tabanlı hizmet katmanları hakkında daha fazla bilgi için bkz: [hizmet katmanları için DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md). DTU tabanlı hizmet katmanları ve sanal çekirdek tabanlı satın alma modeli arasındaki farklar hakkında daha fazla bilgi için bkz: [Azure SQL veritabanı'nın satın alma modeli](sql-database-purchase-models.md).

## <a name="service-tier-characteristics"></a>Hizmet katmanı özellikleri

Sanal çekirdek tabanlı satın alma modeli, üç hizmet katmanı sunar: genel amaçlı, Hiper ölçekli ve iş açısından kritik. Bu hizmet katmanları, bir dizi işlem boyutları, yüksek kullanılabilirlik tasarımı, hata Yalıtımı yöntemleri, türleri ve boyutları, depolama ve g/ç aralıkları tarafından ayrılır.

Ayrı olarak, gerekli depolama ve saklama dönemi yedeklemeleri için yapılandırmanız gerekir. Yedekleme bekletme süresini ayarlama, Azure portalını açın, sunucu (veritabanı değil) gidin ve ardından Git **yedekleri Yönet** > **ilkesini yapılandırma**  >   **Noktası içinde zaman geri yükleme Yapılandırması** > **7-35 gün**.

Aşağıdaki tabloda, üç katmanı arasındaki farklar açıklanmaktadır:

||**Genel amaçlı**|**İş açısından kritik**|**Hiper ölçekli**|
|---|---|---|---|
|En iyi kullanım alanı:|Çoğu iş yükü. Teklifler bütçeye yönelik, dengeli ve ölçeklenebilir işlem ve Depolama Seçenekleri.|Yüksek g/ç gereksinimlerine sahip iş uygulamaları. Çeşitli yalıtılmış çoğaltmaları kullanarak hatalara en yüksek esnekliği sunar.|Çoğu iş yükü ile yüksek düzeyde ölçeklenebilir depolama ve okuma ölçek gereksinimleri.|
|İşlem|**Sağlanan işlem**:<br/>4\. nesil: 1-24 sanal çekirdek<br/>5\. nesil: 80 2 sanal çekirdek<br/>**Sunucusuz bilgi işlem**:<br/>5\. nesil: 0,5 - 4 sanal çekirdek|**Sağlanan işlem**:<br/>4\. nesil: 1-24 sanal çekirdek<br/>5\. nesil: 80 2 sanal çekirdek|**Sağlanan işlem**:<br/>4\. nesil: 1-24 sanal çekirdek<br/>5\. nesil: 80 2 sanal çekirdek|
|Bellek|**Sağlanan işlem**:<br/>4\. nesil: Sanal çekirdek başına 7 GB<br/>5\. nesil: Sanal çekirdek başına 5.1 GB<br/>**Sunucusuz bilgi işlem**:<br/>5\. nesil: Sanal çekirdek başına 3 GB|**Sağlanan işlem**:<br/>4\. nesil: Sanal çekirdek başına 7 GB<br/>5\. nesil: Sanal çekirdek başına 5.1 GB |**Sağlanan işlem**:<br/>4\. nesil: Sanal çekirdek başına 7 GB<br/>5\. nesil: Sanal çekirdek başına 5.1 GB|
|Depolama|Uzak Depolama kullanır.<br/>**Tek veritabanı sağlanan işlem**:<br/>5 GB – 4 TB<br/>**Tek veritabanı sunucusuz işlem**:<br/>5 GB - 1 TB<br/>**Yönetilen örnek**: 32 GB - 8 TB |Yerel SSD depolama kullanır.<br/>**Tek veritabanı sağlanan işlem**:<br/>5 GB – 4 TB<br/>**Yönetilen örnek**:<br/>32 GB - 4 TB |Esnek otomatik olarak büyütme gerektiğinde depolama. En fazla 100 TB depolama destekler. Yerel SSD depolaması yerel arabellek havuzu önbellek ve yerel veri depolama için kullanır. Azure Uzaktan Depolama son uzun süreli veri deposu kullanır. |
|G/ç aktarım hızı (yaklaşık)|**Tek veritabanı**: Sanal çekirdek başına 500 IOPS 7000 maksimum IOPS ile.<br/>**Yönetilen örnek**: Bağımlı [dosya boyutunu](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes).|Çekirdek başına 5000 IOPS'yi 200.000 maksimum IOPS ile|Hiper ölçekli birden çok düzeyde önbelleğe alma ile çok katmanlı bir mimari var. Etkili IOPS, iş yüküne bağlıdır.|
|Kullanılabilirlik|1 çoğaltma, hiçbir okuma ölçeği çoğaltması|3 çoğaltma, 1 [okuma ölçeği çoğaltma](sql-database-read-scale-out.md),<br/>Bölgesel olarak yedekli yüksek kullanılabilirlik (HA)|1 okuma-yazma çoğaltma yanı sıra, 0-4 [okuma ölçeği çoğaltmalar](sql-database-read-scale-out.md)|
|Yedeklemeler|[Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 gün (varsayılan olarak 7 gün)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 gün (varsayılan olarak 7 gün)|Anlık görüntü tabanlı yedeklemelerin Azure uzak depolama. Geri yüklemeler bu anlık görüntüler, Hızlı Kurtarma için kullanın. Yedeklemeler anlıktır ve işlem g/ç performans etkisi yoktur. Geri yüklemeler, hızlı ve bir veri boyutu işlemi (alma dakika yerine saatler veya günler) değil.|
|Bellek içi|Desteklenmiyor|Desteklenen|Desteklenmiyor|
|||

> [!NOTE]
> Ücretsiz bir Azure hesabı ile birlikte temel bir hizmet katmanında, ücretsiz bir Azure SQL veritabanı alabilirsiniz. Daha fazla bilgi için [Azure ücretsiz hesabınızla yönetilen bir bulut veritabanı oluşturun](https://azure.microsoft.com/free/services/sql-database/).

- Sanal çekirdek kaynak sınırları hakkında daha fazla bilgi için bkz. [tek bir veritabanında sanal çekirdek kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md) ve [sanal çekirdek kaynak sınırları yönetilen örneğinde](sql-database-managed-instance.md#vcore-based-purchasing-model).
- Genel amaçlı ve iş kritik hizmet katmanları hakkında daha fazla bilgi için bkz. [genel amaçlı ve iş açısından kritik hizmet katmanları](sql-database-service-tiers-general-purpose-business-critical.md).
- Hiper ölçekli bir hizmet katmanındaki sanal çekirdek tabanlı satın alma modeli hakkında daha fazla bilgi için bkz: [hiper ölçekli hizmet katmanı](sql-database-service-tier-hyperscale.md).  

## <a name="azure-hybrid-benefit"></a>Azure Hibrit Avantajı

Sanal çekirdek tabanlı satın alma modeli sağlanan işlem katmanında indirimli Fiyatlardan SQL veritabanı için mevcut lisanslarınızı kullanarak değiştirebilir [SQL Server için Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/). Bu Azure avantajı, Yazılım Güvencesi içeren şirket içi SQL Server lisanslarınızı kullanarak Azure SQL veritabanı'nda yüzde 30 tasarruf sağlar.

![Fiyatlandırma](./media/sql-database-service-tiers/pricing.png)

Azure hibrit avantajı ile SQL veritabanı altyapısı için kendisine (temel işlem fiyatı) mevcut SQL Server lisansınızı kullanarak yalnızca temel alınan Azure altyapı için ödeme yapmayı seçebilirsiniz veya temel alınan altyapı ve SQL Server için ödeme yapabilirsiniz Lisans, (lisans dahil fiyatlandırma).

Lisanslama modelinizin, Azure portalını kullanarak veya aşağıdaki API'lerden birini kullanarak değiştirin ya da seçin:

- Ayarlamak veya PowerShell kullanarak lisans türünü güncelleştirmek için:

  - [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase)
  - [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase)
  - [Yeni AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)
  - [Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance)

- Azure CLI'yı kullanarak lisans türünü güncelleştirmek veya ayarlamak için:

  - [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create)
  - [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update)
  - [az sql mı oluşturma](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create)
  - [az sql mı güncelleştirme](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)

- Lisans türü REST API'si kullanarak güncelleştirin veya ayarlamak için:

  - [Veritabanları - oluştur veya güncelleştir](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)
  - [Veritabanları - güncelleştirme](https://docs.microsoft.com/rest/api/sql/databases/update)
  - [Yönetilen örnekler - oluştur veya güncelleştir](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)
  - [Yönetilen örnekleri - güncelleştirme](https://docs.microsoft.com/rest/api/sql/managedinstances/update)

## <a name="migrate-from-the-dtu-based-model-to-the-vcore-based-model"></a>DTU tabanlı modeli sanal çekirdek tabanlı modele geçiş

### <a name="migrate-a-database"></a>Veritabanını geçirme

Sanal çekirdek tabanlı satın alma modeli için DTU tabanlı satın alma modeli bir veritabanını geçirme, yükseltme veya indirgeme DTU tabanlı satın alma modeli, standart ve premium hizmet katmanları arasında benzerdir.

### <a name="migrate-databases-with-geo-replication-links"></a>Coğrafi Çoğaltma bağlantılarını ile veritabanlarını geçirme

DTU tabanlı modele geçiş için sanal çekirdek tabanlı satın alma modeli, yükseltme veya indirgeme standart ve premium hizmet katmanlarındaki veritabanları arasındaki coğrafi çoğaltma ilişkileri için benzerdir. Geçiş sırasında coğrafi çoğaltma durdurma gerekmez, ancak bu sıralama kuralları izlemelidir:

- Yükseltme sırasında ikincil veritabanı yükseltmeniz ve ardından birincil yükseltmeniz gerekir.
- Önceki sürüme indirirken ters sırada: birincil veritabanının ilk sürümüne düşürürseniz ve ardından ikincil düşürme gerekir.

İki elastik havuzlar arasında coğrafi çoğaltma kullanırken, birincil ve ikincil olarak başka bir havuz belirlemeniz önerilir. Bu durumda, elastik havuzlar geçirirken, aynı sıralama kılavuzu kullanmanız gerekir. Ancak, hem birincil hem de ikincil veritabanlarını içeren bir elastik havuzları varsa, daha yüksek kullanımı havuz birincil olarak kabul ve sıralama kuralları buna göre izleyin.  

Aşağıdaki tablo, belirli bir geçiş senaryoları için yönergeler sağlar:

|Geçerli hizmet katmanı|Hedef hizmet katmanı|Geçiş türü|Kullanıcı eylemleri|
|---|---|---|---|
|Standart|Genel amaçlı|Yanal|Herhangi bir sırada geçirme ancak uygun sanal çekirdek boyutlandırma * emin olmanız gerekir|
|Premium|İş açısından kritik|Yanal|Herhangi bir sırada geçirme ancak uygun sanal çekirdek boyutlandırma * emin olmanız gerekir|
|Standart|İş açısından kritik|Yükseltme|Öncelikle ikincil geçirmeniz gerekir|
|İş açısından kritik|Standart|Eski sürüme düşür|Öncelikle birincil geçirmeniz gerekir|
|Premium|Genel amaçlı|Eski sürüme düşür|Öncelikle birincil geçirmeniz gerekir|
|Genel amaçlı|Premium|Yükseltme|Öncelikle ikincil geçirmeniz gerekir|
|İş açısından kritik|Genel amaçlı|Eski sürüme düşür|Öncelikle birincil geçirmeniz gerekir|
|Genel amaçlı|İş açısından kritik|Yükseltme|Öncelikle ikincil geçirmeniz gerekir|
||||

\* Her 100 Dtu ve standart katmanda en az 1 sanal çekirdek gerektirir ve her bir 125 Dtu premium katmanda en az 1 sanal çekirdek gerektirir.

### <a name="migrate-failover-groups"></a>Yük devretme gruplarını geçirme

Yük devretme grupları ile birden çok veritabanı geçişi birincil ve ikincil veritabanlarını tek tek geçişini gerektirir. Bu işlem sırasında aynı önemli noktalar ve sıralama kuralları geçerlidir. Veritabanları için sanal çekirdek tabanlı satın alma modeli dönüştürüldükten sonra Yük devretme grubu aynı ilke ayarlarıyla yürürlükte kalır.

### <a name="create-a-geo-replication-secondary-database"></a>Coğrafi çoğaltma ikincil veritabanı oluşturma

Birincil veritabanı için kullanılan yalnızca aynı hizmet katmanını kullanarak bir coğrafi çoğaltma ikincil veritabanı (bir coğrafi-ikincil) oluşturabilirsiniz. Yüksek günlüğü oluşturmayı oranına sahip veritabanları için aynı işlem boyutu birincil olarak ile coğrafi-ikincil oluşturmanızı öneririz.

Tek bir birincil veritabanı için elastik havuzdaki bir coğrafi-ikincil oluşturuyorsanız emin `maxVCore` havuzu eşleşiyorsa birincil veritabanının işlem boyutu ayarlama. Başka bir elastik havuzda bir birincil site için bir coğrafi-ikincil oluşturuyorsanız, havuzları aynı olmasını öneririz `maxVCore` ayarları.

### <a name="use-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Sanal çekirdek tabanlı bir veritabanı için DTU tabanlı bir veritabanı dönüştürmek için veritabanı kopyasını kullanın.

DTU tabanlı işlem boyutu olan herhangi bir veritabanı sanal çekirdek tabanlı işlem boyutu kısıtlamaları veya kaynak veritabanının en büyük veritabanı boyutu hedef işlem boyutu desteklediği sürece özel sıralaması olmayan bir veritabanına kopyalayabilirsiniz. Veritabanı kopyalama, kopyalama işleminin başlangıç tarihindeki verileri anlık görüntüsünü oluşturur ve kaynak ve hedef arasında verileri eşitleyebilmeniz değil.

## <a name="next-steps"></a>Sonraki adımlar

- Belirli bir bilgi işlem boyutlarına ve tek veritabanları için kullanılabilen depolama boyutu seçenekleri için bkz: [tek veritabanları için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md).
- Belirli bir bilgi işlem boyutlarına ve elastik havuzlar için kullanılabilir depolama boyutu seçenekleri için bkz. [elastik havuzlar için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).
