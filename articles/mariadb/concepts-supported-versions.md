---
title: MariaDB için Azure veritabanı'nda desteklenen sürümler
description: MariaDB için Azure veritabanı'nda desteklenen sürümlerini açıklar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 760cb39ea1f3980faba348c7aa1de68a66b20a8d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065715"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>MariaDB server sürümleri için desteklenen Azure veritabanı

MariaDB için Azure veritabanı, açık kaynak geliştirilen [MariaDB sunucu](https://downloads.mariadb.org/), Innodb altyapısı kullanarak. MariaDB için Azure veritabanı şu anda şu sürümü destekler:

## <a name="mariadb-version-102"></a>MariaDB sürüm 10.2

Başvurmak [MariaDB belgeleri](https://mariadb.com/kb/en/library/mariadb-10223-release-notes/) geliştirmeleri ve MariaDB 10.2.23 düzeltmeler hakkında daha fazla bilgi için.

## <a name="mariadb-version-103"></a>MariaDB sürüm 10.3

Başvurmak [MariaDB belgeleri](https://mariadb.com/kb/en/library/mariadb-10314-release-notes/) geliştirmeleri ve MariaDB 10.3.14 düzeltmeler hakkında daha fazla bilgi için.

> [!NOTE]
> Hizmeti, bir ağ geçidi sunucu örnekleri bağlantılarını yeniden yönlendirmek için kullanılır. Bağlantı kurulduktan sonra MySQL istemci değil, MariaDB sunucu örneğinde çalışan asıl sürümü ağ geçidi kümesindeki MariaDB sürümünü gösterir. MariaDB server örneğinin sürümü belirlemek için `SELECT VERSION();` MySQL isteminde komutu.

## <a name="managing-updates-and-upgrades"></a>Yönetme güncelleştirmeler ve yükseltmeler

Alt sürüm güncelleştirmeleri için düzeltme eki uygulama, hizmet otomatik olarak yönetir.

## <a name="next-steps"></a>Sonraki adımlar

- Kotalar ve sınırlamalar temel alarak belirli bir kaynak hakkında bilgi için **hizmet katmanı**, bkz: [hizmet katmanları](./concepts-pricing-tiers.md).
