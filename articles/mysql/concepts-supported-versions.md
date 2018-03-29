---
title: MySQL için Azure veritabanında desteklenen sürümleri
description: Azure veritabanı'nın desteklenen sürümleri için MySQL açıklar.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 03/22/2018
ms.openlocfilehash: cfebdbe7485f0ffaa15828803d72c2a3f97c118d
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>MySQL server sürümleri için desteklenen Azure veritabanı
Azure veritabanı için MySQL geliştirilmiş gelen [MySQL Community Edition](https://www.mysql.com/products/community/), InnoDB altyapısını kullanarak.  MySQL için Azure veritabanı şu anda aşağıdaki sürümlerini destekler:

## <a name="mysql-version-5638"></a>MySQL sürüm 5.6.38
MySQL başvuran [belgelerine](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-38.html) geliştirmeleri ve MySQL 5.6.38 düzeltmeler hakkında daha fazla bilgi edinmek için.

## <a name="mysql-version-5720"></a>MySQL sürüm 5.7.20
MySQL başvuran [belgelerine](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-20.htmll) geliştirmeleri ve düzeltmeler hakkında bilgi edinmek için 5.7.20 mysql'deki.

> [!NOTE]
> Hizmetinde bir ağ geçidi bağlantıları sunucu örnekleri yeniden yönlendirmek için kullanılır. Bağlantı kurulduktan sonra MySQL istemci MySQL server örneğinde çalışan gerçek sürüm ağ geçidi kümesindeki MySQL sürümünü görüntüler. MySQL server örneğinin sürümü belirlemek için `SELECT VERSION();` MySQL isteminde komutu. 

## <a name="managing-updates-and-upgrades"></a>Yönetme güncelleştirmeleri ve yükseltmeleri
Alt sürüm güncelleştirmeleri düzeltme eki uygulama hizmeti otomatik olarak yönetir. (Örn. ana sürüm yükseltme desteklenmez MySQL 5.6 MySQL 5.7 yükseltme).

## <a name="next-steps"></a>Sonraki adımlar

Kotalar ve sınırlamalara bağlı olarak belirli bir kaynak hakkında bilgi için **hizmet katmanı**, bkz: [hizmet katmanları](./concepts-pricing-tiers.md)
