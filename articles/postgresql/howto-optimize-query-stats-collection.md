---
title: Sorgu istatistikleri koleksiyonu PostgreSQL - tek bir sunucu için Azure veritabanı üzerinde en iyi duruma getirme
description: Bu makalede, PostgreSQL - tek bir sunucu için Azure veritabanı üzerinde sorgu istatistikleri koleksiyonu nasıl iyileştirebileceğiniz de açıklanır
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 7425ee7916fd71625f336a7af35f6481d1ed2474
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65068954"
---
# <a name="optimize-query-statistics-collection-on-an-azure-database-for-postgresql---single-server"></a>Sorgu istatistikleri koleksiyonu PostgreSQL - tek bir sunucu için Azure veritabanı üzerinde en iyi duruma getirme
Bu makalede, PostgreSQL sunucusu için Azure veritabanı üzerinde sorgu istatistikleri koleksiyonu en iyi duruma getirme açıklanır.

## <a name="use-pgstatsstatements"></a>Pg_stats_statements kullanın
**Pg_stat_statements** , PostgreSQL için Azure veritabanı'nda varsayılan olarak etkin bir PostgreSQL uzantısıdır. Uzantı için bir sunucu tarafından yürütülen tüm SQL deyimleri yürütme istatistikleri izlemek için bir yol sağlar. Bu modül, her sorgu yürütme kancaları ve önemsiz olmayan bir performans maliyetine ile birlikte gelir. Etkinleştirme **pg_stat_statements** sorgu disk üzerindeki dosyalara metin yazma zorlar.

Benzersiz sorgu uzun sorgu metni ile sahip olduğunuz veya etkin izleme yok **pg_stat_statements**, devre dışı **pg_stat_statements** en iyi performans için. Bunu yapmak için ayarı değiştirmek `pg_stat_statements.track = NONE`.

Bazı müşteri iş yüklerinin kadar yüzde 50 performans iyileştirmesi gördünüz olduğunda **pg_stat_statements** devre dışı bırakıldı. Pg_stat_statements devre dışı bıraktığınızda yaptığınız artırabilen performans sorunlarını giderme yeteneğinin ' dir.

Ayarlanacak `pg_stat_statements.track = NONE`:

- Azure portalında Git [PostgreSQL kaynak Yönetimi sayfasında ve sunucu parametreleri dikey seçin](howto-configure-server-parameters-using-portal.md).

  ![PostgreSQL sunucu parametresi dikey penceresi](./media/howto-optimize-query-stats-collection/pg_stats_statements_portal.png)

- Kullanım [Azure CLI](howto-configure-server-parameters-using-cli.md) kümesine az postgres server configuration `--name pg_stat_statements.track --resource-group myresourcegroup --server mydemoserver --value NONE`.

## <a name="use-the-query-store"></a>Query Store kullanın 
[Query Store](concepts-query-store.md) Özelliği Azure veritabanı'nda PostgreSQL sorgu istatistikleri izlemek için daha etkili bir yöntem sağlar. Bu özelliği kullanmaya alternatif olarak önerilir *pg_stats_statements*. 

## <a name="next-steps"></a>Sonraki adımlar
Ayarlamayı düşünün `pg_stat_statements.track = NONE` içinde [Azure portalında](howto-configure-server-parameters-using-portal.md) kullanarak veya [Azure CLI](howto-configure-server-parameters-using-cli.md).

Daha fazla bilgi için bkz. 
- [Sorgu Deposu kullanım senaryoları](concepts-query-store-scenarios.md) 
- [Query Store en iyi uygulamalar](concepts-query-store-best-practices.md) 
