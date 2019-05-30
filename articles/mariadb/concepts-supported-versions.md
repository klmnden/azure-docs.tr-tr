---
title: MariaDB için Azure veritabanı'nda desteklenen sürümler
description: MariaDB için Azure veritabanı'nda desteklenen sürümlerini açıklar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 69330e9d5a05fbcc892889f70a04f5eb4a4a2fb9
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "60935560"
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
