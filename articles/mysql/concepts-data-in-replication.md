---
title: Verileri, MySQL için Azure veritabanı'na çoğaltın.
description: Bu makalede, veri çoğaltma için Azure veritabanı için MySQL açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: f91a6da9a305c6620e4e01ab7aa3c554374cb5d7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60996833"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>MySQL için Azure veritabanı'na veri çoğaltma

Veri çoğaltma hizmeti MySQL için Azure veritabanı'na harici bir MySQL sunucusu verilerden eşitlemenizi sağlar. Dış sunucunun şirket içi, sanal makineleri veya diğer bulut sağlayıcıları tarafından barındırılan bir veritabanı hizmeti olabilir. Veri çoğaltma (binlog) ikili günlük dosyası konumu tabanlı çoğaltma Mysql'e yerel temel alır. Binlog çoğaltma hakkında daha fazla bilgi için bkz: [MySQL binlog Çoğaltmaya genel bakış](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html). 

## <a name="when-to-use-data-in-replication"></a>Veri çoğaltma kullanma zamanı
Veri çoğaltma kullanarak dikkate alınması gereken ana senaryolar şunlardır:

- **Karma veri eşitleme:** Veri çoğaltma ile MySQL için Azure veritabanı ve şirket içi sunucular arasında eşitlenen verileri tutabilirsiniz. Bu eşitleme, karma uygulamalar oluşturmak için kullanışlıdır. Varolan bir yerel veritabanı sunucusuna sahip, ancak bir bölgeye yakın bir konumda son kullanıcılara verileri taşımak istediğiniz zaman bu cazip bir yöntemdir.
- **Birden çok bulut eşitlemesi:** Karmaşık yapılı bulut çözümleri, veri çoğaltma MySQL için Azure veritabanı ile sanal makineleri ve bu bulutlarında barındırılan veritabanı Hizmetleri dahil olmak üzere, farklı bulut sağlayıcıları arasında veri eşitlemek için kullanın.

## <a name="limitations-and-considerations"></a>Sınırlamalar ve önemli noktalar

### <a name="data-not-replicated"></a>Yinelenen verileri
[ *Mysql sistem veritabanı* ](https://dev.mysql.com/doc/refman/5.7/en/system-database.html) ana sunucuya çoğaltılamaz. Hesapları ve izinleri ana sunucu üzerinde değişiklikler çoğaltılmadığından. El ile çoğaltma sunucusuna erişmek bu hesabı ana sunucuya bir hesap oluşturun ve gerekiyorsa çoğaltma sunucu tarafında aynı hesabı oluşturun. Hangi tablolar sistem veritabanında bulunan anlamak için bkz [MySQL el ile](https://dev.mysql.com/doc/refman/5.7/en/system-database.html).

### <a name="requirements"></a>Gereksinimler
- Ana sunucu sürümü en az olmalıdır MySQL 5.6 sürümü. 
- Ana ve çoğaltma sunucu sürümlerinde aynı olması gerekir. Örneğin, hem de MySQL 5.6 sürümü olmalıdır veya her ikisi de MySQL 5.7 sürüm olmalıdır.
- Her tabloda bir birincil anahtarı olmalıdır.
- Ana sunucu MySQL Innodb altyapısı kullanmanız gerekir.
- Kullanıcı, ikili günlük tutmayı yapılandırma ve ana sunucuda yeni kullanıcılar oluşturmak için izinleri olmalıdır.

### <a name="other"></a>Diğer
- Veri çoğaltma, yalnızca genel amaçlı desteklenir ve fiyatlandırma katmanları bellek için iyileştirilmiş ' dir.
- Genel işlem tanımlayıcıları (GTID) desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [verileri, çoğaltmayı ayarlama](howto-data-in-replication.md)
- Hakkında bilgi edinin [ile azure'da çoğaltma çoğaltmaları okuyun](concepts-read-replicas.md)
