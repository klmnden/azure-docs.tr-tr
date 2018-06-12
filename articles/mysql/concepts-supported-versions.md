---
title: MySQL için Azure veritabanında desteklenen sürümleri
description: Azure veritabanı'nın desteklenen sürümleri için MySQL açıklar.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 05/23/2018
ms.openlocfilehash: c9a533ed9b9eb9ac53a02439b98a78954c7aaa11
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265257"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>MySQL server sürümleri için desteklenen Azure veritabanı
Azure veritabanı için MySQL geliştirilmiş gelen [MySQL Community Edition](https://www.mysql.com/products/community/), InnoDB altyapısını kullanarak.  MySQL için Azure veritabanı şu anda aşağıdaki sürümlerini destekler:

## <a name="mysql-version-5639"></a>MySQL sürüm 5.6.39
MySQL başvuran [belgelerine](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-39.html) geliştirmeleri ve MySQL 5.6.39 düzeltmeler hakkında daha fazla bilgi edinmek için.

## <a name="mysql-version-5721"></a>MySQL sürüm 5.7.21
MySQL başvuran [belgelerine](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-21.html) geliştirmeleri ve düzeltmeler hakkında bilgi edinmek için 5.7.21 mysql'deki.

> [!NOTE]
> Hizmetinde bir ağ geçidi bağlantıları sunucu örnekleri yeniden yönlendirmek için kullanılır. Bağlantı kurulduktan sonra MySQL istemci MySQL server örneğinde çalışan gerçek sürüm ağ geçidi kümesindeki MySQL sürümünü görüntüler. MySQL server örneğinin sürümü belirlemek için `SELECT VERSION();` MySQL isteminde komutu. 

## <a name="managing-updates-and-upgrades"></a>Yönetme güncelleştirmeleri ve yükseltmeleri
Alt sürüm güncelleştirmeleri düzeltme eki uygulama hizmeti otomatik olarak yönetir. (Örn. ana sürüm yükseltme desteklenmez MySQL 5.6 MySQL 5.7 yükseltme).

## <a name="next-steps"></a>Sonraki adımlar

Kotalar ve sınırlamalara bağlı olarak belirli bir kaynak hakkında bilgi için **hizmet katmanı**, bkz: [hizmet katmanları](./concepts-pricing-tiers.md)
