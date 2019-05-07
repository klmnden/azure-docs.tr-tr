---
title: PostgreSQL – hiper ölçekli (Citus) (Önizleme) için Azure veritabanı'nda dağıtılmış veriler
description: Tabloları ve parçalar sunucu grubunda dağıtılmış.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 9020ee690d93a1b477471fac4a482a909fca5935
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65077343"
---
# <a name="distributed-data-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>PostgreSQL – hiper ölçekli (Citus) (Önizleme) için Azure veritabanı'nda dağıtılmış veriler

Bu makalede, Hiper ölçekli (Citus) olarak üç tablo türleri özetlenmektedir.
Nasıl dağıtılmış tablolar gösteren parçaları ve parçalar düğümlere yerleştirilir yol olarak depolanır.

## <a name="table-types"></a>Tablo türleri

Üç tür hiper ölçekli sunucu grubundaki tablolar, her farklı amaçlar için kullanılır.

### <a name="type-1-distributed-tables"></a>1. tür: Dağıtılmış tablolar

İlk tür ve en yaygın *dağıtılmış* tablolar. SQL deyimleri için normal tablolar gibi görünüyor ancak yatay olan *bölümlenmiş* çalışan düğümleri arasında. Ne bu tablonun satırlarını adlı parça tablolarda, farklı düğümlerde depolandığı anlamına gelir *parçalar*.

Yalnızca SQL hiper ölçekli çalışır, ancak dağıtılmış bir tablonun şeması şekilde değiştirme, bir küme genelinde DDL deyimleri basamaklı tablonun tüm parçalar arasında çalışanları güncelleştirilecek.

#### <a name="distribution-column"></a>Dağıtım sütunu

Hiper ölçekli, algoritmik parçalama satırlar için parça atamak için kullanır. Atama adlı bir tablo sütununun değerini temel belirleyici tabanlı yaptığı *dağıtım sütunu.* Küme Yöneticisi, bir tablo dağıtırken bu sütun tanımlamalısınız.
Doğru seçim yapmak, performansı ve işlevselliği için önemlidir.

### <a name="type-2-reference-tables"></a>2. tür: başvuru tabloları

Başvuru tablosu tüm içerikleri tek bir parçanın yoğunlaşmıştır dağıtılmış tablo türüdür. Tüm alt sorgularda başvuru bilgileri yerel olarak başka bir düğümden satırları isteyen ağ ek yükü olmadan erişebilmesi için her bir çalışan üzerinde parça çoğaltılır. Her satır ayrı parçalardaki ayırt etmenize gerek olmadığından başvuru tabloları hiç dağıtım sütunu var.

Başvuru tabloları genelde küçüktür ve herhangi bir alt düğüm üzerinde çalışmakta olan sorgulara ile ilgili verileri depolamak için kullanılır. Örneğin, sipariş durumları veya ürün kategorileri gibi değerler numaralandırılır.

### <a name="type-3-local-tables"></a>3. tür: yerel tablolar

Hiper ölçekli kullandığınızda, bağlandığınız Düzenleyici düğüm normal bir PostgreSQL veritabanı hizmetidir. Düzenleyici normal tablolar oluşturabilir ve bunları parçalara ayırmak seçin.

Yerel tablolar için iyi bir aday JOIN sorguları katılan yoksa küçük yönetim tablolar olacaktır. Örneğin, uygulama oturum açma ve kimlik doğrulaması için kullanıcıların tablo.

## <a name="shards"></a>Parçalar

Önceki bölümde açıklandığı gibi nasıl dağıtılmış tablolar parçalar çalışan düğümlerinde depolanır. Bu bölümde, teknik ayrıntılara daha fazla alır.

`pg_dist_shard` Düzenleyici meta veri tablosunda sistemde dağıtılmış her tablonun her parça için bir satır içerir. Satır bir parça kimliği (shardminvalue, shardmaxvalue) bir karma alanında tamsayılar aralığı ile eşleşiyor:

```sql
SELECT * from pg_dist_shard;
 logicalrelid  | shardid | shardstorage | shardminvalue | shardmaxvalue 
---------------+---------+--------------+---------------+---------------
 github_events |  102026 | t            | 268435456     | 402653183
 github_events |  102027 | t            | 402653184     | 536870911
 github_events |  102028 | t            | 536870912     | 671088639
 github_events |  102029 | t            | 671088640     | 805306367
 (4 rows)
```

Düzenleyici düğüm bir satır, hangi parçanın tutan belirlemek isteyip istemediğini `github_events`, sıradaki dağıtım sütununun değerini karma hale getirir ve hangi parçanın denetler\'s aralığı, karma değer içeriyor. (Karma işlevi görüntüsü, ayrık bir birleşimdir böylece aralıkları tanımlanır.)

### <a name="shard-placements"></a>Parça yerleşimi

Bu parçadaki 102027 söz konusu satır ile ilişkili olduğunu varsayın. Satır okuma veya adlı tabloda yazılan `github_events_102027` çalışanları birinde. Hangi çalışan? Tamamen meta veri tablolarını tarafından belirlenir ve parça bilinen bir çalışan için parça eşleme *yerleştirme*.

Düzenleyici düğüm gibi belirli tablolar başvuran bir parça halinde yeniden sorguları Yazar `github_events_102027`, ve bu parçaları uygun çalışanları üzerinde çalışır. Bir sorgu çalıştırın parça kimliği 102027 tutan bir düğümü bulunamadı arka planda örneği aşağıda verilmiştir.

```sql
SELECT
    shardid,
    node.nodename,
    node.nodeport
FROM pg_dist_placement placement
JOIN pg_dist_node node
  ON placement.groupid = node.groupid
 AND node.noderole = 'primary'::noderole
WHERE shardid = 102027;
```

    ┌─────────┬───────────┬──────────┐
    │ shardid │ nodename  │ nodeport │
    ├─────────┼───────────┼──────────┤
    │  102027 │ localhost │     5433 │
    └─────────┴───────────┴──────────┘

## <a name="next-steps"></a>Sonraki adımlar
- Öğrenme [dağıtım sütunu seçin](concepts-hyperscale-choose-distribution-column.md) için Dağıtılmış tablolar
