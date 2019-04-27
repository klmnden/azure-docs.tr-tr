---
title: MySQL için Azure veritabanı'nda desteklenen sürümler
description: MySQL için Azure veritabanı'nda desteklenen sürümlerini açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/13/2019
ms.openlocfilehash: e7e81632b2be135fb74d375ab8a11f1b4b3ef39d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60525927"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>MySQL server sürümleri için desteklenen Azure veritabanı

MySQL için Azure veritabanı geliştirilen gelen [MySQL Community Edition](https://www.mysql.com/products/community/), Innodb altyapısı kullanarak.

MySQL X.Y.Z adlandırma şeması kullanır. X sürümle Y ikincil sürüm ve Z hata düzeltmesi sürümüdür. Düzen hakkında daha fazla bilgi için bkz: [MySQL belgeleri](https://dev.mysql.com/doc/refman/5.7/en/which-version.html).

MySQL için Azure veritabanı şu anda aşağıdaki sürümlerini destekler:

## <a name="mysql-version-56"></a>MySQL 5.6 sürümü

Yayın hata düzeltmesi: 5.6.42

Mysql'e başvuran [sürüm notları](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-42.html) geliştirmeleri ve MySQL 5.6.42 düzeltmeler hakkında daha fazla bilgi için.

## <a name="mysql-version-57"></a>MySQL sürümü 5.7

Yayın hata düzeltmesi: 5.7.24

Mysql'e başvuran [sürüm notları](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-24.html) iyileştirmeler ve düzeltmeler hakkında bilgi için MySQL 5.7.24 içinde.

> [!NOTE]
> Hizmeti, bir ağ geçidi sunucu örnekleri bağlantılarını yeniden yönlendirmek için kullanılır. Bağlantı kurulduktan sonra MySQL istemci değil, MySQL server örneğinde çalışan asıl sürümü ağ geçidi kümesindeki MySQL sürümünü gösterir. MySQL server örneğinin sürümü belirlemek için `SELECT VERSION();` MySQL isteminde komutu.

## <a name="managing-updates-and-upgrades"></a>Yönetme güncelleştirmeler ve yükseltmeler
Hata düzeltmesi sürüm güncelleştirmeleri için düzeltme eki uygulama, hizmet otomatik olarak yönetir. Örneğin, 5.7.20 için 5.7.21.  

Küçük ve büyük sürüm yükseltmeleri şu anda desteklenmiyor. Örneğin, MySQL 5.6 ' MySQL 5.7 için yükseltme desteklenmez. 5.7 için 5.6 yükseltmek isterseniz, olması bir [döküm ve geri yükleme](./concepts-migrate-dump-restore.md) yeni altyapısı sürümü ile oluşturulmuş bir sunucu için.

## <a name="next-steps"></a>Sonraki adımlar

Kotalar ve sınırlamalar temel alarak belirli bir kaynak hakkında bilgi için **hizmet katmanı**, bkz: [hizmet katmanları](./concepts-pricing-tiers.md)
