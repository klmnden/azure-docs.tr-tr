---
title: PostgreSQL server sorgu istatistikleri koleksiyonu için Azure veritabanı'nda sorgu istatistikleri koleksiyonu en iyi duruma getirme
description: Bu makalede, PostgreSQL sunucusu için Azure veritabanı üzerinde sorgu istatistikleri koleksiyonu nasıl iyileştirebileceğiniz de açıklanır.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/25/2018
ms.openlocfilehash: 8b8d1a5a16a948953838f51edca091121dc14df1
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629294"
---
# <a name="optimize-query-statistics-collection-in-azure-database-for-postgresql-server"></a>PostgreSQL için Azure veritabanı'nda sorgu istatistikleri koleksiyonu en iyi duruma getirme 
Sorgu istatistikleri koleksiyonu PostgreSQL sunucusu için Azure veritabanı içinde iyileştirmek için bu makalede açıklanır.

## <a name="using-pgstatsstatements"></a>Pg_stats_statements kullanma
**Pg_stat_statements** , PostgreSQL için Azure veritabanı'nda varsayılan olarak etkin bir PostgreSQL uzantısıdır. Uzantı bir sunucu tarafından yürütülen tüm SQL deyimleri yürütme istatistikleri izlemek için bir yol sağlar. Ancak, bu modül her sorgu yürütme kancaları ve önemsiz olmayan bir performans maliyetine ile birlikte gelir. Etkinleştirme **pg_stat_statements** sorgu disk üzerindeki dosyalara metin yazma zorlar.

Uzun sorgu metni benzersiz sorgularla olan veya olmayan etkin izleme müşteriler için **pg_stat_statements**, devre dışı bırakılması önerilir **pg_stat_statements** ayarlayarakeniyiperformansiçin`pg_stat_statements.track = NONE`.

Bazı müşteri iş yüklerinin üzerinde kadar yüzde 50 performans iyileştirmesi devre dışı bırakarak gördük **pg_stat_statements**. Ancak, bir pg_stat_statements devre dışı bırakarak yapar artırabilen yükleyememesine performans sorunlarını giderme olur.

Ayarlanacak `pg_stat_statements.track = NONE`:

- Azure portalında gidin [PostgreSQL kaynak Yönetimi sayfasında ve sunucu parametreleri dikey seçin](howto-configure-server-parameters-using-portal.md).

![PostgreSQL sunucu parametresi dikey penceresi](.\media\howto-optimize-query-stats-collection\pg_stats_statements_portal.png)

- Kullanarak [Azure CLI](howto-configure-server-parameters-using-cli.md), az postgres server configuration set `--name pg_stat_statements.track --resource-group myresourcegroup --server mydemoserver --value NONE`.

## <a name="using-query-store"></a>Query Store kullanma 
[Query Store](concepts-query-store.md) PostgreSQL sorgu istatistikleri izlemek için daha fazla yüksek performanslı yöntemi sağlar ve kullanmaya alternatif olarak önerilen için Azure veritabanı'nda özellik *pg_stats_statements*. 

## <a name="next-steps"></a>Sonraki adımlar
Ayarlamayı düşünün `pg_stat_statements.track = NONE` içinde [Azure portalı](howto-configure-server-parameters-using-portal.md) veya bu adı kullanıyor [Azure CLI](howto-configure-server-parameters-using-cli.md).

Bkz: [Query Store kullanım senaryoları](concepts-query-store-scenarios.md) ve [Query Store en iyi](concepts-query-store-best-practices.md) daha fazla bilgi için. 
