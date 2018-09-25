---
title: MariaDB için Azure veritabanı'nda desteklenen sürümler
description: MariaDB için Azure veritabanı'nda desteklenen sürümlerini açıklar.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: aa83b9955839aaa03aa2ebf46c9e464dc75c8d3a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977511"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>MariaDB server sürümleri için desteklenen Azure veritabanı
MariaDB için Azure veritabanı, açık kaynak geliştirilen [MariaDB sunucu](https://downloads.mariadb.org/), Innodb altyapısı kullanarak. MariaDB için Azure veritabanı şu anda şu sürümü destekler:

## <a name="mariadb-version-10217"></a>MariaDB sürüm 10.2.17
Başvurmak [MariaDB belgeleri](https://downloads.mariadb.org/mariadb/10.2.17/) geliştirmeleri ve MariaDB 10.2.17 düzeltmeler hakkında daha fazla bilgi için.

> [!NOTE]
> Hizmeti, bir ağ geçidi sunucu örnekleri bağlantılarını yeniden yönlendirmek için kullanılır. Bağlantı kurulduktan sonra MySQL istemci değil, MariaDB sunucu örneğinde çalışan asıl sürümü ağ geçidi kümesindeki MariaDB sürümünü gösterir. MariaDB server örneğinin sürümü belirlemek için `SELECT VERSION();` MySQL isteminde komutu.

## <a name="managing-updates-and-upgrades"></a>Yönetme güncelleştirmeler ve yükseltmeler
Alt sürüm güncelleştirmeleri için düzeltme eki uygulama, hizmet otomatik olarak yönetir.

## <a name="next-steps"></a>Sonraki adımlar
Kotalar ve sınırlamalar temel alarak belirli bir kaynak hakkında bilgi için **hizmet katmanı**, bkz: [hizmet katmanları](./concepts-pricing-tiers.md)