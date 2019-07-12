---
title: Verileri MariaDB için Azure veritabanı'na çoğaltın.
description: Bu makalede, veri çoğaltma için Azure veritabanı MariaDB için açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 07/11/2019
ms.openlocfilehash: c19ec06ce353d653086fa693dde975a55f51f823
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839247"
---
# <a name="replicate-data-into-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'na veri çoğaltma

Veri çoğaltma, sanal makineleri veya diğer bulut sağlayıcılarının MariaDB hizmeti için Azure veritabanı'nda barındırılan veritabanı hizmetleri, şirket içinde çalışan bir MariaDB sunucudan veri eşitlemenize olanak tanır. Veri çoğaltma (binlog) ikili günlük dosyası konumu tabanlı çoğaltma MariaDB için yerel temel alır. Binlog çoğaltma hakkında daha fazla bilgi için bkz: [binlog Çoğaltmaya genel bakış](https://mariadb.com/kb/en/library/replication-overview/).

## <a name="when-to-use-data-in-replication"></a>Veri çoğaltma kullanma zamanı
Veri çoğaltma kullanarak dikkate alınması gereken ana senaryolar şunlardır:

- **Karma veri eşitleme:** Veri çoğaltma ile verileri MariaDB için Azure veritabanı ve şirket içi sunucular arasında eşitlenmiş kalmasını sağlayabilirsiniz. Bu eşitleme, karma uygulamalar oluşturmak için kullanışlıdır. Varolan bir yerel veritabanı sunucusuna sahip, ancak bir bölgeye yakın bir konumda son kullanıcılara verileri taşımak istediğiniz zaman bu cazip bir yöntemdir.
- **Birden çok bulut eşitlemesi:** Karmaşık yapılı bulut çözümleri, veri çoğaltma MariaDB için Azure veritabanı ile sanal makineleri ve bu bulutlarında barındırılan veritabanı Hizmetleri dahil olmak üzere, farklı bulut sağlayıcıları arasında veri eşitlemek için kullanın.

## <a name="limitations-and-considerations"></a>Sınırlamalar ve önemli noktalar

### <a name="data-not-replicated"></a>Yinelenen verileri
[ *Mysql sistem veritabanı* ](https://mariadb.com/kb/en/library/the-mysql-database-tables/) ana sunucuya çoğaltılmaz. Hesapları ve izinleri ana sunucu üzerinde değişiklikler çoğaltılmaz. Ana sunucuya bir hesap oluşturun ve bu hesap çoğaltma sunucusuna erişmesi gereken el ile aynı hesabı çoğaltma sunucu tarafında oluşturun. Hangi tablolar sistem veritabanında bulunan anlamak için bkz [MariaDB belgeleri](https://mariadb.com/kb/en/library/the-mysql-database-tables/).

### <a name="requirements"></a>Gereksinimler
- Ana sunucu sürümü en az olmalıdır MariaDB 10.2 sürümü.
- Ana ve çoğaltma sunucu sürümlerinde aynı olması gerekir. Örneğin, her ikisi de MariaDB 10.2 sürümü olması gerekir.
- Her tabloda bir birincil anahtarı olmalıdır.
- Innodb altyapısı ana sunucu kullanmanız gerekir.
- Kullanıcı, ikili günlük tutmayı yapılandırma ve ana sunucuda yeni kullanıcılar oluşturmak için izinleri olmalıdır.

### <a name="other"></a>Diğer
- Veri çoğaltma, yalnızca genel amaçlı desteklenir ve fiyatlandırma katmanları bellek için iyileştirilmiş ' dir.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [verileri, çoğaltmayı ayarlama](howto-data-in-replication.md).
