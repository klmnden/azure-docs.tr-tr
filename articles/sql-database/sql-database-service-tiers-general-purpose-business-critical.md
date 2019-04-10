---
title: Azure SQL veritabanı - genel amaçlı ve iş açısından kritik | Microsoft Docs
description: Genel amaçlı ve satın alma modeli sanal çekirdek iş kritik hizmet katmanında anlatılmaktadır.
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
ms.date: 02/23/2019
ms.openlocfilehash: 067ea8eee297eb8572bd37e240b8d13afe458ca7
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358474"
---
# <a name="azure-sql-database-service-tiers"></a>Azure SQL veritabanı hizmet katmanları

Azure SQL veritabanı altyapı hataları durumda bile % 99,99 kullanılabilirlik sağlamak üzere bulut ortamı için ayarlanmış bir SQL Server veritabanı altyapısı mimarisini temel alır. Azure SQL veritabanı'nda kullanılan üç Mimari modeli vardır:

- [Genel amaçlı](sql-database-service-tier-general-purpose.md) genel iş yüklerinin çoğu için tasarlanmıştır.
- [İş açısından kritik](sql-database-service-tier-business-critical.md) bir okunabilir çoğaltma ile düşük gecikme süreli iş yükleri için tasarlanmıştır.
- [Hiper ölçekli](sql-database-service-tier-hyperscale.md) çok büyük veritabanları için tasarlanan (en fazla 100 TB) birden çok okunabilir çoğaltma ile.

Bu makalede, sanal çekirdek tabanlı satın alma modeli, genel amaçlı ve iş açısından kritik hizmet katmanlarına yönelik depolama ve yedekleme konuları anlatılmaktadır.

> [!NOTE]
> Hiper ölçekli bir hizmet katmanındaki sanal çekirdek tabanlı satın alma modeli hakkında daha fazla bilgi için bkz: [hiper ölçekli hizmet katmanı](sql-database-service-tier-hyperscale.md). Sanal çekirdek tabanlı satın alma modeli DTU tabanlı satın alma modeli ile bir karşılaştırması için bkz: [Azure SQL veritabanı'nın modelleri ve kaynakları satın alma](sql-database-purchase-models.md).

## <a name="data-and-log-storage"></a>Veri ve günlük depolaması

Aşağıdaki topluluklara bir göz atın:

- Ayrılmış depolama, veri dosyaları (MDF) ve günlük dosyalarını (LDF) tarafından kullanılır.
- Varsayılan en büyük boyutu 32 GB olan bir maksimum veritabanı boyutu her tek veritabanı işlem boyutu destekler.
- Gereken tek veritabanı boyutu (MDF boyutu) yapılandırırken, ek depolama alanı % 30'luk LDF desteklemek için otomatik olarak eklenir
- Yönetilen örnek depolama boyutu, 32 GB'ın katları şeklinde belirtilmelidir.
- Desteklenen en fazla 10 GB arasındaki herhangi bir tek veritabanı boyutu seçebilirsiniz.
  - Depolama, standart veya genel amaçlı hizmet katmanlarında sunulduğundan, artırmak veya azaltmak boyutu 10 GB'lık artışlarla
  - İş açısından kritik veya premium depolama için hizmet katmanları, artırmak veya azaltmak boyutu 250 GB'lık artışlarla
- Genel amaçlı hizmet katmanındaki `tempdb` ekli bir SSD ve bu depolama maliyeti sanal çekirdek fiyatına dahil kullanır.
- İş açısından kritik hizmet katmanındaki `tempdb` paylaşımları ekli SSD MDF ve LDF dosyaları ve tempDB depolama maliyeti sanal çekirdek fiyatı yer almaktadır.

> [!IMPORTANT]
> MDF ve LDF için ayrılan toplam depolama alanı için ücretlendirilirsiniz.

MDF ve LDF geçerli toplam boyutunu izlemek için kullanabilirsiniz [bilgilerini sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Tek tek MDF ve LDF dosyaları geçerli boyutunu izlemek için kullanabilirsiniz [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="backups-and-storage"></a>Yedekleme ve depolama

Veritabanı Yedeklemeleri için depolama noktası zaman geri yükleme (PITR içinde) desteklemek için ayrılır ve [uzun süreli saklama (LTR)](sql-database-long-term-retention.md) SQL veritabanı özellikleri. Bu depolama alanı ayrı ayrı her veritabanı için ayrılan ve iki ayrı veritabanı başına ücret üzerinden faturalandırılırsınız.

- **PITR**: Tek veritabanı yedeklemeleri kopyalanır [RA-GRS depolama](../storage/common/storage-designing-ha-apps-with-ragrs.md) otomatik olarak. Oluşturulan yeni yedekleme depolama alanı boyutu dinamik olarak artar.  Depolama alanı, haftalık tam yedeklemeler, günlük fark yedekleri ve 5 dakikada bir kopyalanan işlem günlüğü yedeklemeleri tarafından kullanılır. Depolama alanı tüketimi, veritabanının ve saklama dönemi değişiklik oranına bağlıdır. 7-35 gün arasında her veritabanı için ayrı tutma süresine yapılandırabilirsiniz. Veri boyutu 1 x ile eşit bir en düşük depolama alanı miktarı, ek ücret alınmadan sağlanır. Çoğu veritabanı için bu miktar 7 güne kadar yedek depolamak için yeterli olacaktır.
- **LTR**: SQL veritabanı için 10 yıla kadar uzun süreli saklamak üzere tam yedeklemeleri yapılandırma seçeneğini sunar. LTR ilkesi etkinleştirilirse, bu yedeklemeler otomatik olarak RA-GRS depolama alanında depolanır, ancak sıklıkla yedeklemeleri kopyalanır denetleyebilirsiniz. Farklı bir uyumluluk gereksinimini karşılamak için haftalık, aylık ve/veya yıllık yedeklemeler için farklı bekletme sürelerinin seçebilirsiniz. Bu yapılandırma, ne kadar depolama alanı LTR yedeklemeleri için kullanılacak tanımlayacaksınız. LTR fiyatlandırma hesaplayıcısını LTR depolama maliyetini tahmin etmek için kullanabilirsiniz. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

## <a name="next-steps"></a>Sonraki adımlar

- Özel hakkında ayrıntılı bilgi işlem boyutları ve tek veritabanı genel amaçlı ve iş kritik hizmet katmanları için kullanılabilir depolama boyutu seçenekleri için bkz: [tek veritabanları için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-storage-sizes-and-compute-sizes)
- Özel hakkında ayrıntılı bilgi işlem boyutları ve elastik havuzlar için genel amaçlı ve iş kritik hizmet katmanlarında kullanılabilir depolama boyutu seçenekleri için bkz: [elastik havuzlar için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).
