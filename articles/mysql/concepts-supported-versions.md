---
title: MySQL için Azure veritabanı'nda desteklenen sürümler
description: MySQL için Azure veritabanı'nda desteklenen sürümlerini açıklar.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 10/10/2018
ms.openlocfilehash: f2a9348e267ad6a5929fda64cc08dcd837243e71
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093349"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>MySQL server sürümleri için desteklenen Azure veritabanı
MySQL için Azure veritabanı geliştirilen gelen [MySQL Community Edition](https://www.mysql.com/products/community/), Innodb altyapısı kullanarak. MySQL için Azure veritabanı şu anda aşağıdaki sürümlerini destekler:

## <a name="mysql-version-5639"></a>MySQL sürümü 5.6.39
Mysql'e başvuran [belgeleri](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-39.html) geliştirmeleri ve MySQL 5.6.39 düzeltmeler hakkında daha fazla bilgi için.

## <a name="mysql-version-5721"></a>MySQL sürümü 5.7.21
Mysql'e başvuran [belgeleri](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-21.html) iyileştirmeler ve düzeltmeler hakkında bilgi için MySQL 5.7.21 içinde.

> [!NOTE]
> Hizmeti, bir ağ geçidi sunucu örnekleri bağlantılarını yeniden yönlendirmek için kullanılır. Bağlantı kurulduktan sonra MySQL istemci değil, MySQL server örneğinde çalışan asıl sürümü ağ geçidi kümesindeki MySQL sürümünü gösterir. MySQL server örneğinin sürümü belirlemek için `SELECT VERSION();` MySQL isteminde komutu.

## <a name="managing-updates-and-upgrades"></a>Yönetme güncelleştirmeler ve yükseltmeler
Hata düzeltmesi sürüm güncelleştirmeleri için düzeltme eki uygulama, hizmet otomatik olarak yönetir. Şu anda, alt sürüm yükseltme desteklenmez. Örneğin, MySQL 5.6 ' MySQL 5.7 için yükseltme desteklenmiyor. Sonraki alt sürüme yükseltmek istiyorsanız, olması bir [döküm ve geri yükleme](./concepts-migrate-dump-restore.md) yeni altyapısı sürümü ile oluşturulmuş bir sunucu için.

## <a name="next-steps"></a>Sonraki adımlar

Kotalar ve sınırlamalar temel alarak belirli bir kaynak hakkında bilgi için **hizmet katmanı**, bkz: [hizmet katmanları](./concepts-pricing-tiers.md)
