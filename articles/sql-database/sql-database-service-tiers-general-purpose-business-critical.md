---
title: Azure SQL veritabanı - genel amaçlı ve iş açısından kritik | Microsoft Docs
description: Genel amaçlı ve satın alma modeli sanal çekirdek iş kritik hizmet katmanında anlatılmaktadır.
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
ms.date: 10/04/2018
ms.openlocfilehash: 053bcd46f5b0f7e06997bb0bcd57f448617b9911
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48832327"
---
# <a name="general-purpose-and-business-critical-service-tiers"></a>Genel amaçlı ve iş açısından kritik hizmet katmanları

Bu makalede, sanal çekirdek tabanlı satın alma modeli, genel amaçlı ve iş açısından kritik hizmet katmanlarına yönelik depolama ve yedekleme konuları anlatılmaktadır. 

> [!NOTE]
> Hiper ölçekli bir hizmet katmanındaki sanal çekirdek tabanlı satın alma modeli hakkında daha fazla bilgi için bkz: [hiper ölçekli hizmet katmanı](sql-database-service-tier-hyperscale.md). Sanal çekirdek tabanlı satın alma modeli DTU tabanlı satın alma modeli ile bir karşılaştırması için bkz: [Azure SQL veritabanı'nın modelleri ve kaynakları satın alma](sql-database-service-tiers.md).

## <a name="data-and-log-storage"></a>Veri ve günlük depolaması

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

## <a name="backups-and-storage"></a>Yedekleme ve depolama

Veritabanı Yedeklemeleri için depolama noktası zaman geri yükleme (PITR içinde) desteklemek için ayrılır ve [uzun süreli saklama (LTR)](sql-database-long-term-retention.md) SQL veritabanı özellikleri. Bu depolama alanı ayrı ayrı her veritabanı için ayrılan ve iki ayrı veritabanı başına ücret üzerinden faturalandırılırsınız.

- **PITR**: tek tek veritabanı yedeklemeleri kopyalanır [RA-GRS depolama](../storage/common/storage-designing-ha-apps-with-ragrs.md) otomatik olarak. Oluşturulan yeni yedekleme depolama alanı boyutu dinamik olarak artar.  Depolama alanı, haftalık tam yedeklemeler, günlük fark yedekleri ve 5 dakikada bir kopyalanan işlem günlüğü yedeklemeleri tarafından kullanılır. Depolama alanı tüketimi, veritabanının ve saklama dönemi değişiklik oranına bağlıdır. 7-35 gün arasında her veritabanı için ayrı tutma süresine yapılandırabilirsiniz. Veri boyutu 1 x ile eşit bir en düşük depolama alanı miktarı, ek ücret alınmadan sağlanır. Çoğu veritabanı için bu miktar 7 güne kadar yedek depolamak için yeterli olacaktır.
- **LTR**: SQL veritabanı için 10 yıla kadar uzun süreli saklama yedeklerini tam yapılandırma seçeneği sunar. LTR ilkesi etkinleştirilirse, bu yedeklemeler otomatik olarak RA-GRS depolama alanında depolanır, ancak sıklıkla yedeklemeleri kopyalanır denetleyebilirsiniz. Farklı bir uyumluluk gereksinimini karşılamak için haftalık, aylık ve/veya yıllık yedeklemeler için farklı bekletme sürelerinin seçebilirsiniz. Bu yapılandırma, ne kadar depolama alanı LTR yedeklemeleri için kullanılacak tanımlayacaksınız. LTR fiyatlandırma hesaplayıcısını LTR depolama maliyetini tahmin etmek için kullanabilirsiniz. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

## <a name="next-steps"></a>Sonraki adımlar

- Özel hakkında ayrıntılı bilgi işlem boyutları ve tek veritabanı genel amaçlı ve iş kritik hizmet katmanları için kullanılabilir depolama boyutu seçenekleri için bkz: [tek veritabanları için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes)
- Özel hakkında ayrıntılı bilgi işlem boyutları ve elastik havuzlar için genel amaçlı ve iş kritik hizmet katmanlarında kullanılabilir depolama boyutu seçenekleri için bkz: [elastik havuzlar için SQL veritabanı sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
