---
title: Esnek veritabanı araçları sözlüğü | Microsoft Docs
description: Esnek veritabanı araçları için kullanılan terimler açıklaması
services: sql-database
ms.service: sql-database
subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 354d972e78a7fb7270b1b09f4af5aa95709fcd06
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162585"
---
# <a name="elastic-database-tools-glossary"></a>Esnek veritabanı araçları sözlüğü
Aşağıdaki terimler için tanımlanan [esnek veritabanı araçlarını](sql-database-elastic-scale-introduction.md), Azure SQL veritabanı'nın bir özelliği. Yönetmek için kullanılan araçları [parça eşlemeleri](sql-database-elastic-scale-shard-map-management.md)ve [istemci Kitaplığı](sql-database-elastic-database-client-library.md), [bölme-birleştirme aracını](sql-database-elastic-scale-overview-split-and-merge.md), [elastik havuzlar](sql-database-elastic-pool.md)ve [sorguları](sql-database-elastic-query-overview.md). 

Bu terimler kullanılır [esnek veritabanı araçlarını kullanarak bir parça ekleme](sql-database-elastic-scale-add-a-shard.md) ve [parça eşleme sorunlarını düzeltme RecoveryManager sınıfı kullanarak](sql-database-elastic-database-recovery-manager.md).

![Esnek ölçek koşulları][1]

**Veritabanı**: bir Azure SQL veritabanı. 

**Verilere bağımlı yönlendirme**: belirli bir parçalama anahtarı verilen bir parçaya bağlanmak bir uygulama sağlayan işlevselliği. Bkz: [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md). Karşılaştırılacak  **[çok parçalı sorgu](sql-database-elastic-scale-multishard-querying.md)**.

**Genel parça eşleme**: parçalama anahtarları ve içinde ilgili, parçalar arasında eşleme bir **parça kümesi**. Genel parça eşleme depolanan **parça eşleme Yöneticisi**. Karşılaştırılacak **yerel parça eşlemesi**.

**Liste parça eşlemesi**: bir parça eşlemesi içinde hangi parçalama anahtarları ayrı olarak eşleştirilir. Karşılaştırılacak **aralık parça eşlemesi**.   

**Yerel parça eşlemesi**: bir parça üzerinde depolanan, yerel parça eşlemesi için parça üzerinde bulunan parçacıklara eşlemeleri içerir.

**Çok parçalı sorgu**: birden çok parça; sorgu oluşturabilme olanağı sonuç kümeleri (diğer adıyla "yaygın sorgu") UNION ALL semantiği kullanarak döndürülür. Karşılaştırılacak **verilere bağımlı yönlendirme**.

**Çok kiracılı** ve **tek kiracılı**: Bu tek kiracılı veritabanı ile çok kiracılı veritabanı gösterir:

![Tek ve birden çok Kiracı veritabanları](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

İşte bir temsilini **parçalı** tek ve çok kiracılı veritabanları. 

![Tek ve birden çok Kiracı veritabanları](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

**Aralık parça eşlemesi**: bir parça eşlemesi bitişik değerleri üzerinde birden çok aralık parça Dağıtım stratejisi dayanır. 

**Başvuru tabloları**: tabloları, parçalı değildir ancak parçalar arasında çoğaltılır. Örneğin, posta kodları başvuru tablosunda depolanabilir. 

**Parça**: parçalı bir veri kümesinden veri depolayan bir Azure SQL veritabanı. 

**Parça esnekliği**: her ikisi de gerçekleştirme becerisi **yatay ölçeklendirme** ve **dikey ölçeklendirme**.

**Parçalı tablo**: tabloları, parçalı, yani, verisini parçalama anahtar değerlerine göre parçalar arasında dağıtılır. 

**Parçalama anahtarı**: bir sütun değeri verileri parçalar arasında nasıl dağıtıldığını belirler. Değer türü aşağıdakilerden biri olabilir: **int**, **bigint**, **varbinary**, veya **uniqueidentifier**. 

**Parça kümesi**: aynı parça eşlemesine parça eşleme Yöneticisi'nde öznitelikli parçalar koleksiyonu.  

**Parçacık**: tüm bir parçalama anahtarı bir parça üzerinde tek bir değer ile ilişkili veriler. Parçacık en küçük olası veri taşıma parçalı tablo yeniden dağıtırken birimidir. 

**Parça eşlemesi**: belirlenen parçalama anahtarları ve bunların ilgili parçalar arasında eşleme.

**Parça eşleme Yöneticisi**: parça harita(lar), parça konumlarını ve bir veya daha fazla parça kümeleri eşlemelerini içeren bir Yönetim nesnesi ve veri deposu.

![Eşlemeler][2]

## <a name="verbs"></a>Fiiller
**Yatay ölçeklendirme**: (veya) ölçeklendirme eylemi ekleyerek veya bir parça eşlemesine parçalar aşağıda gösterildiği gibi kaldırarak parçalar koleksiyonu.

![Yatay ve dikey ölçeklendirme][3]

**Birleştirme**: taşıma parçacıklara iki parçadan bir parçaya ve parça eşlemesi uygun şekilde güncelleştirme işlemi.

**Parçacık taşıma**: tek parçacık farklı bir parçaya taşıma işlemi. 

**Parça**: yapısal verileri parçalama anahtarını temel alan birden çok veritabanı genelinde aynı yatay olarak bölümleme işlemi.

**Bölünmüş**: bazı parçacıklara (genellikle yeni) başka bir parçaya bir parçadan veri taşıma işlemi. Bir parçalama anahtarı bölünmüş noktası olarak kullanıcı tarafından sağlanır.

**Dikey ölçeklendirme**: ölçeği artırılabilen (veya azaltılabilen) işlemi tek bir parçanın işlem boyutu. Örneğin, bir parça (Bu, daha fazla bilgi işlem kaynaklarına sonuçları) Premium standart değiştiriliyor. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

