---
title: PostgreSQL için Azure veritabanı'nda desteklenen sürümler
description: PostgreSQL için Azure veritabanı'nda desteklenen sürümlerini açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/12/2018
ms.openlocfilehash: 5b06128979bf448a0b85084d5178d9291beb7691
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60422074"
---
# <a name="supported-postgresql-database-versions"></a>Desteklenen PostgreSQL veritabanı sürümleri
Microsoft, hizmet PostgreSQL için Azure veritabanı'nda PostgreSQL altyapısının n-2 sürümlerini desteklemek üzere amaçlar. Sürümler, geçerli ana sürüme (n) azure'da ve iki önceki ana sürümü (-2) olacaktır.

PostgreSQL için Azure veritabanı şu anda aşağıdaki sürümlerini destekler:

## <a name="postgresql-version-105"></a>PostgreSQL sürümü 10.5
Başvurmak [PostgreSQL belgeleri](https://www.postgresql.org/docs/10/static/release-10-5.html) iyileştirmeler ve düzeltmeler alt bu sürüm hakkında daha fazla bilgi için.

## <a name="postgresql-version-9610"></a>PostgreSQL sürümü 9.6.10
Başvurmak [PostgreSQL belgeleri](https://www.postgresql.org/docs/9.6/static/release-9-6-10.html) iyileştirmeler ve düzeltmeler alt bu sürüm hakkında daha fazla bilgi için.

## <a name="postgresql-version-9514"></a>PostgreSQL sürümü 9.5.14
Başvurmak [PostgreSQL belgeleri](https://www.postgresql.org/docs/9.5/static/release-9-5-14.html) iyileştirmeler ve düzeltmeler hakkında bilgi edinmek için bu ikincil sürüm.

## <a name="managing-updates-and-upgrades"></a>Yönetme güncelleştirmeler ve yükseltmeler
PostgreSQL için Azure veritabanı, ikincil sürüm düzeltme eklerini otomatik olarak yönetir. Şu anda ana sürüm yükseltme desteklenmez. Örneğin, PostgreSQL 9.6 için PostgreSQL 9.5 ' yükseltme desteklenmiyor. Bir sonraki ana sürümüne yükseltmek istiyorsanız, bir veritabanı oluşturma [döküm ve geri yükleme](./howto-migrate-using-dump-and-restore.md) yeni altyapısı sürümü ile oluşturulmuş bir sunucu için.

## <a name="next-steps"></a>Sonraki adımlar
PostgreSQL Uzantıları desteği hakkında daha fazla bilgi için bkz. [PostgreSQL uzantıları](concepts-extensions.md).
