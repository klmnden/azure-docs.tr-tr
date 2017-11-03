---
title: "Esnek veritabanı araçlarını sözlüğü | Microsoft Docs"
description: "Esnek veritabanı araçları için kullanılan terimler açıklaması"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: a23a4e81-6706-452d-afc1-a550e5e47af9
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: db8ce257479888db63758e681393c0244af01ce7
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="elastic-database-tools-glossary"></a>Esnek veritabanı araçlarını sözlüğü
Aşağıdaki terimler için tanımlanan [esnek veritabanı araçlarını](sql-database-elastic-scale-introduction.md), Azure SQL veritabanı özelliğidir. Araçlar yönetmek için kullanılan [parça eşlemeleri](sql-database-elastic-scale-shard-map-management.md)ve dahil [istemci Kitaplığı](sql-database-elastic-database-client-library.md), [bölünmüş Birleştirme aracı](sql-database-elastic-scale-overview-split-and-merge.md), [esnek havuzlar](sql-database-elastic-pool.md)ve [sorguları](sql-database-elastic-query-overview.md). 

Aşağıdaki terimler kullanılır [esnek veritabanı araçlarını kullanarak bir parça ekleme](sql-database-elastic-scale-add-a-shard.md) ve [parça eşleme sorunları düzeltmek için RecoveryManager sınıfını kullanarak](sql-database-elastic-database-recovery-manager.md).

![Esnek ölçeklendirme koşulları][1]

**Veritabanı**: bir Azure SQL veritabanı. 

**Veri bağımlı yönlendirme**: belirli parçalama anahtara verilen bir parça bağlanmak bir uygulama sağlayan işlevselliği. Bkz: [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md). Karşılaştırılacak  **[çok parça sorgu](sql-database-elastic-scale-multishard-querying.md)**.

**Genel parça eşleme**: parçalama anahtarları ve bunların ilgili parça içinde arasında eşleme bir **parça kümesi**. Genel parça eşleme depolanan **parça eşleme Yöneticisi**. Karşılaştırılacak **yerel parça eşleme**.

**Liste parça eşleme**: bir parça eşleme hangi parçalama anahtarları ayrı ayrı eşlendi. Karşılaştırılacak **aralığı parça eşleme**.   

**Yerel parça eşleme**: parça üzerinde depolanan, yerel parça eşleme parça üzerinde bulunan shardlets eşlemeler içerir.

**Çok parça sorgu**: birden çok parça; bir sorgu verme özelliği sonuç kümeleri, UNION ALL semantiği (olarak da bilinen "yayma sorgu") kullanılarak döndürülür. Karşılaştırılacak **veri bağımlı yönlendirme**.

**Çok kiracılı** ve **tek Kiracı**: Bu tek Kiracı veritabanı ve çok Kiracı veritabanı gösterir:

![Tek ve çoklu kiracı veritabanları](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

Bir gösterimini işte **parçalı** tek ve çoklu kiracı veritabanları. 

![Tek ve çoklu kiracı veritabanları](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

**Aralık parça eşleme**: bir parça eşleme parça Dağıtım stratejisi birden çok bitişik değerleri aralığı üzerinde dayanır. 

**Başvuru tabloları**: parçalı değildir ancak parça arasında çoğaltılan tabloları. Örneğin, posta kodları başvuru tablosunda depolanabilir. 

**Parça**: parçalı bir veri kümesinden veri depolayan bir Azure SQL veritabanı. 

**Parça esneklik**: her ikisi de gerçekleştirme yeteneğini **yatay ölçekleme** ve **dikey ölçeklendirme**.

**Parçalı tabloları**: tabloları, parçalı, başka bir deyişle, verileri parçalama anahtar değerlerine göre parça dağıtılır. 

**Parçalama anahtar**: veri parça arasında nasıl dağıtıldığını belirler bir sütun değeri. Değer türü şunlardan biri olabilir: **int**, **bigint**, **varbinary**, veya **uniqueidentifier**. 

**Parça kümesi**: parça eşleme Yöneticisi'nde aynı parça eşlemeye öznitelikli parça koleksiyonu.  

**Shardlet**: tüm tek bir parça bir parçalama anahtar değeriyle ilişkilendirilmiş veriler. Bir shardlet en küçük olası veri taşıma parçalı tabloları dağıtırken birimidir. 

**Parça eşleme**: parçalama anahtarları ve bunların ilgili parça arasındaki eşlemeleri kümesi.

**Parça eşleme Yöneticisi**: parça harita(lar), parça konumları ve bir veya daha fazla parça kümeleri için eşlemelerini içeren bir Yönetim nesnesi ve veri deposu.

![Eşlemeleri][2]

## <a name="verbs"></a>Fiiller
**Yatay ölçekleme**: (veya) ölçeklendirme eylemi ekleyerek veya parça parça eşleme için aşağıda gösterildiği gibi kaldırarak parça koleksiyonu.

![Yatay ve dikey ölçekleme][3]

**Birleştirme**: taşıma shardlets iki parça için bir parça ve parça eşleme uygun şekilde güncelleştirme işlemi.

**Shardlet taşıma**: tek bir shardlet için farklı bir parça taşıma işlemi. 

**Parça**: bir parçalama anahtarına göre birden fazla veritabanı üzerinden yapısal verileri yatay olarak aynı bölümleme eylemi.

**Bölünmüş**: birkaç shardlets bir parça için başka bir (genellikle yeni) parça taşıma işlemi. Bir parçalama anahtar bölünmüş noktası olarak kullanıcı tarafından sağlanır.

**Dikey ölçeklendirme**: Yukarı (veya aşağı) ölçeklendirme eylemi tek tek bir parça performans düzeyi. Örneğin, bir parça (ve daha fazla bilgi işlem kaynakları sonuçlanır) Premium'a standart değiştirme. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

