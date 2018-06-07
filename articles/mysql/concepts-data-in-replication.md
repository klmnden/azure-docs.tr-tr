---
title: MySQL için Azure veritabanına verileri.
description: Bu makalede, verileri, Azure veritabanı için MySQL için çoğaltma açıklanmaktadır.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/18/2018
ms.openlocfilehash: 6efb034f63f65283911f46bbb251b5eb12d517ec
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655788"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>MySQL için Azure veritabanına verileri

Verileri, çoğaltma, şirket içi, sanal makine ya da MySQL hizmeti Azure veritabanına diğer bulut sağlayıcıları tarafından barındırılan veritabanı Hizmetleri çalışan bir MySQL sunucusu verileri eşitlemenize olanak tanır. Verileri, çoğaltma ikili günlük (binlog) dosya konum temelinde çoğaltma için MySQL yerel temel alır. Binlog çoğaltma hakkında daha fazla bilgi için bkz: [MySQL binlog Çoğaltmaya genel bakış](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html). 

## <a name="when-to-use-data-in-replication"></a>Verileri, çoğaltma kullanma zamanı
Verileri, çoğaltma'yı kullanarak dikkate alınması gereken temel senaryolar şunlardır:

- **Karma veri eşitleme:** çoğaltma verileri, MySQL için şirket içi sunucular ile Azure veritabanı arasında eşitlenen verileri tutabilirsiniz. Bu eşitleme, karma uygulamalar oluşturmak için kullanışlıdır. Varolan bir yerel veritabanı sunucusuna sahip, ancak bir bölgeye yakın son kullanıcılara verileri taşımak istediğiniz zaman bu çekici bir yetenektir.
- **Birden çok bulut eşitlemesi:** karmaşık bulut çözümleri, kullanım verileri, Azure veritabanı için MySQL ve farklı bulut sağlayıcıları arasında veri eşitlemeye çoğaltma dahil olmak üzere sanal makineleri ve bu bulutlara barındırılan veritabanı hizmetleri.

## <a name="limitations-and-considerations"></a>Sınırlamalar ve ilgili önemli noktalar

### <a name="data-not-replicated"></a>Yinelenen verileri
[ *Mysql sistem veritabanı* ](https://dev.mysql.com/doc/refman/5.7/en/system-database.html) birincil sunucuda çoğaltılmaz. Bu hesapları ve izinleri birincil sunucudaki değişiklikleri içerir. Çoğaltma sunucusuna erişmek bu hesabı birincil sunucuda bir hesap oluşturun ve gerekirse daha sonra el ile aynı hesabı çoğaltma sunucu tarafında oluşturun. Hangi tablolar sistem veritabanında bulunan anlamak için lütfen bkz [MySQL el ile](https://dev.mysql.com/doc/refman/5.7/en/system-database.html).

### <a name="requirements"></a>Gereksinimler
- Birincil sunucu sürümü en az olmalıdır MySQL sürüm 5.6. 
- Birincil ve çoğaltma sunucu sürümlerinin aynı olması gerekir. Örneğin, hem de MySQL sürüm 5.6 olmalıdır veya MySQL sürüm 5.7 olmalıdır.
- Her tablonun birincil anahtarı olmalıdır.
- Birincil sunucu MySQL InnoDB altyapısı kullanmanız gerekir.
- Kullanıcı ikili günlük yapılandırmak ve birincil sunucuya yeni kullanıcılar oluşturmak için izinleri olmalıdır.

### <a name="other"></a>Diğer
- Genel işlem tanımlayıcıları (GTID) desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
- Bilgi edinmek için nasıl [verileri, çoğaltmayı ayarlama](howto-data-in-replication.md)