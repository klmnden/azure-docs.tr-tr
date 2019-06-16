---
title: Azure SQL veritabanı - genel amaçlı ve iş açısından kritik | Microsoft Docs
description: Genel amaçlı ve iş kritik hizmet katmanlarında sanal çekirdek tabanlı satın alma modeli anlatılmaktadır.
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
ms.openlocfilehash: e5426bb7c8eba9d58dbf0472360c6ce0b19c9bc4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66431335"
---
# <a name="azure-sql-database-service-tiers"></a>Azure SQL veritabanı hizmet katmanları

Azure SQL veritabanı, bir altyapı hatası olsa bile, yüzde 99,99 kullanılabilirlik sağlamak bulut ortamı için ayarlanan SQL Server veritabanı altyapısı mimarisine temel alır. Üç hizmet katmanı, Azure SQL veritabanı, her biri farklı bir mimari modeli kullanılır. Bu hizmet katmanlarıdır:

- [Genel amaçlı](sql-database-service-tier-general-purpose.md), en genel iş yükleri için tasarlanmıştır.
- [İş açısından kritik](sql-database-service-tier-business-critical.md), bir okunabilir çoğaltma ile düşük gecikme süreli iş yükleri için tasarlanmıştır.
- [Hiper ölçekli](sql-database-service-tier-hyperscale.md), çok büyük veritabanları için tasarlanan (en fazla 100 TB) birden çok okunabilir çoğaltma ile.

Bu makalede ve genel amaçlı sanal çekirdek tabanlı satın alma modeli, iş kritik hizmet katmanları için depolama ve yedekleme konuları anlatılmaktadır.

> [!NOTE]
> Hiper ölçekli bir hizmet katmanındaki sanal çekirdek tabanlı satın alma modeli hakkında daha fazla bilgi için bkz. [hiper ölçekli hizmet katmanı](sql-database-service-tier-hyperscale.md). Sanal çekirdek tabanlı satın alma modeli DTU tabanlı satın alma modeli ile bir karşılaştırması için bkz: [Azure SQL veritabanı'nın modelleri ve kaynakları satın alma](sql-database-purchase-models.md).

## <a name="data-and-log-storage"></a>Veri ve günlük depolaması

Veri ve günlük dosyaları için kullanılan depolama miktarı aşağıdaki etmenlere etkiler:

- Ayrılmış depolama, veri dosyaları (MDF) ve günlük dosyaları (LDF) tarafından kullanılır.
- En fazla veritabanı boyutu, 32 GB maksimum boyuta sahip her tek veritabanı işlem boyutu destekler.
- Gereken tek veritabanı boyutu (bir MDF dosyası boyutunu) yapılandırdığınızda, yüzde 30 daha fazla ek depolama alanı LDF dosyalarını desteklemek için otomatik olarak eklenir.
- SQL veritabanı yönetilen örneği için depolama alanı boyutu, 32 GB'ın katları şeklinde belirtilmelidir.
- Desteklenen en fazla 10 GB arasındaki herhangi bir tek veritabanı boyutu seçebilirsiniz.
  - Depolama, standart veya genel amaçlı hizmet katmanlarında artırabilir veya boyutu 10 GB'lık artışlarla azaltabilirsiniz.
  - Premium veya iş kritik hizmet katmanları, artış veya düşüş boyutu 250 GB'lık artışlarla depolama.
- Genel amaçlı hizmet katmanındaki `tempdb` ekli bir SSD ve bu depolama maliyeti sanal çekirdek fiyatına dahil kullanır.
- İş kritik hizmet katmanındaki `tempdb` ekli SSD MDF ve LDF dosyaları ile paylaşır ve `tempdb` depolama maliyeti sanal çekirdek fiyatı yer almaktadır.

> [!IMPORTANT]
> MDF ve LDF dosyaları için ayrılan toplam depolama alanı için ücretlendirilirsiniz.

Geçerli toplam boyutu MDF ve LDF dosyaları izlemek için kullanabilirsiniz [bilgilerini sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Tek tek MDF ve LDF dosyaları geçerli boyutunu izlemek için kullanabilirsiniz [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="backups-and-storage"></a>Yedekleme ve depolama

Veritabanı Yedeklemeleri için depolama, zaman içinde nokta geri yükleme (PITR) desteklemek için ayrılır ve [uzun süreli saklama (LTR)](sql-database-long-term-retention.md) SQL veritabanı özellikleri. Bu depolama alanı ayrı ayrı her veritabanı için ayrılan ve iki ayrı veritabanı başına ücret üzerinden faturalandırılırsınız.

- **PITR**: Tek veritabanı yedeklemeleri kopyalanır [okuma erişimli coğrafi olarak yedekli (RA-GRS) depolama](../storage/common/storage-designing-ha-apps-with-ragrs.md) otomatik olarak. Oluşturulan yeni yedekleme depolama alanı boyutu dinamik olarak artar. Depolama, haftalık tam yedeklemeler, günlük fark yedekleri ve 5 dakikada bir kopyalanan işlem günlüğü yedeklemeleri tarafından kullanılır. Depolama tüketimini yedekleri saklama süresini ve veritabanı değişiklik oranına bağlıdır. 7-35 gün arasında her veritabanı için ayrı tutma süresine yapılandırabilirsiniz. Veritabanı boyutunun yüzde 100'e (1 x) eşit bir en düşük depolama alanı miktarı, ek ücret alınmadan sağlanır. Çoğu veritabanı için bu miktar 7 güne kadar yedek depolamak için yeterli olacaktır.
- **LTR**: SQL veritabanı için 10 yıla kadar uzun süreli saklamak üzere tam yedeklemeleri yapılandırma seçeneğini sunar. Bir LTR ilkesi ayarlayın, bu yedeklemeler otomatik olarak RA-GRS depolama alanında depolanır, ancak sıklıkla yedeklemeleri kopyalanır denetleyebilirsiniz. Farklı uyumluluk gereksinimlerini karşılamak için haftalık, aylık ve yıllık yedeklemeler için farklı bekletme sürelerinin seçebilirsiniz. Seçtiğiniz yapılandırma ne kadar depolama alanı LTR yedeklemeler için kullanılacak belirler. LTR depolama maliyetini tahmin etmek için LTR fiyatlandırma hesaplayıcısı kullanabilirsiniz. Daha fazla bilgi için [SQL veritabanı uzun süreli saklama](sql-database-long-term-retention.md).

## <a name="next-steps"></a>Sonraki adımlar

- Belirli ayrıntılarını işlem boyutları ve tek bir veritabanı genel amaçlı ve iş kritik hizmet katmanları için kullanılabilir depolama alanı boyutları için bkz: [tek veritabanları için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md).
- Belirli ayrıntılarını işlem boyutları ve elastik havuzlar genel amaçlı ve iş kritik hizmet katmanları için kullanılabilir depolama alanı boyutları için bkz: [elastik havuzlar için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md).
