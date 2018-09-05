---
title: Verileri, MySQL için Azure veritabanı'na çoğaltın.
description: Bu makalede, veri çoğaltma için Azure veritabanı için MySQL açıklanır.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 08/31/2018
ms.openlocfilehash: 6135e4a0182f3af7db54eab974e4c307402185ab
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666085"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>MySQL için Azure veritabanı'na veri çoğaltma

Veri çoğaltma, verileri bir MySQL sunucusu şirket içi sanal makineleri veya diğer bulut sağlayıcılarının MySQL hizmeti için Azure veritabanı'nda barındırılan veritabanı Hizmetleri çalıştıran eşitlemenize olanak tanır. Veri çoğaltma (binlog) ikili günlük dosyası konumu tabanlı çoğaltma Mysql'e yerel temel alır. Binlog çoğaltma hakkında daha fazla bilgi için bkz: [MySQL binlog Çoğaltmaya genel bakış](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html). 

## <a name="when-to-use-data-in-replication"></a>Veri çoğaltma kullanma zamanı
Veri çoğaltma kullanarak dikkate alınması gereken ana senaryolar şunlardır:

- **Karma veri eşitleme:** verileri, daha fazla çoğaltma ile verileri MySQL için Azure veritabanı ve şirket içi sunucular arasında eşitlenmiş kalmasını sağlayabilirsiniz. Bu eşitleme, karma uygulamalar oluşturmak için kullanışlıdır. Varolan bir yerel veritabanı sunucusuna sahip, ancak bir bölgeye yakın bir konumda son kullanıcılara verileri taşımak istediğiniz zaman bu cazip bir yöntemdir.
- **Birden çok bulut eşitlemesi:** karmaşık bulut çözümleri, kullanım verileri, MySQL için Azure veritabanı ile farklı bulut sağlayıcıları arasında verileri eşitleyebilmeniz için çoğaltma sanal makineleri ve bu bulutlarında barındırılan veritabanı Hizmetleri dahil.

## <a name="limitations-and-considerations"></a>Sınırlamalar ve önemli noktalar

### <a name="data-not-replicated"></a>Yinelenen verileri
[ *Mysql sistem veritabanı* ](https://dev.mysql.com/doc/refman/5.7/en/system-database.html) ana sunucuya çoğaltılmaz. Hesapları ve izinleri ana sunucu üzerinde değişiklikler çoğaltılmaz. Ana sunucuya bir hesap oluşturun ve bu hesap çoğaltma sunucusuna erişmesi gereken el ile aynı hesabı çoğaltma sunucu tarafında oluşturun. Hangi tablolar sistem veritabanında bulunan anlamak için bkz [MySQL el ile](https://dev.mysql.com/doc/refman/5.7/en/system-database.html).

### <a name="requirements"></a>Gereksinimler
- Ana sunucu sürümü en az olmalıdır MySQL 5.6 sürümü. 
- Ana ve çoğaltma sunucu sürümlerinde aynı olması gerekir. Örneğin, hem de MySQL 5.6 sürümü olmalıdır veya her ikisi de MySQL 5.7 sürüm olmalıdır.
- Her tabloda bir birincil anahtarı olmalıdır.
- Ana sunucu MySQL Innodb altyapısı kullanmanız gerekir.
- Kullanıcı, ikili günlük tutmayı yapılandırma ve ana sunucuda yeni kullanıcılar oluşturmak için izinleri olmalıdır.

### <a name="other"></a>Diğer
- Veri çoğaltma yalnızca genel amaçlı desteklenir ve fiyatlandırma katmanları bellek için iyileştirilmiş olan
- Genel işlem tanımlayıcıları (GTID) desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [verileri, çoğaltmayı ayarlama](howto-data-in-replication.md)
