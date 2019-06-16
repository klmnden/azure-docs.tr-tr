---
title: Esnek veritabanı araçları sözlüğü | Microsoft Docs
description: Esnek veritabanı araçları için kullanılan terimler açıklaması
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 446203b45744a95c32cd41d9ded26fd960ac8a22
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60585626"
---
# <a name="elastic-database-tools-glossary"></a>Esnek veritabanı araçları sözlüğü

Aşağıdaki terimler için tanımlanan [esnek veritabanı araçlarını](sql-database-elastic-scale-introduction.md), Azure SQL veritabanı'nın bir özelliği. Yönetmek için kullanılan araçları [parça eşlemeleri](sql-database-elastic-scale-shard-map-management.md)ve [istemci Kitaplığı](sql-database-elastic-database-client-library.md), [bölme-birleştirme aracını](sql-database-elastic-scale-overview-split-and-merge.md), [elastik havuzlar](sql-database-elastic-pool.md)ve [sorguları](sql-database-elastic-query-overview.md). 

Bu terimler kullanılır [esnek veritabanı araçlarını kullanarak bir parça ekleme](sql-database-elastic-scale-add-a-shard.md) ve [parça eşleme sorunlarını düzeltme RecoveryManager sınıfı kullanarak](sql-database-elastic-database-recovery-manager.md).

![Esnek ölçek koşulları][1]

**Veritabanı**: Bir Azure SQL veritabanı. 

**Verilere bağımlı yönlendirme**: Bir uygulamanın belirli bir parçalama anahtarı verilen bir parçaya bağlanma sağlayan işlevselliği. Bkz: [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md). Karşılaştırılacak  **[çok parçalı sorgu](sql-database-elastic-scale-multishard-querying.md)** .

**Genel parça eşleme**: Parçalama anahtarları ve içinde ilgili, parçalar arasında eşleme bir **parça kümesi**. Genel parça eşleme depolanan **parça eşleme Yöneticisi**. Karşılaştırılacak **yerel parça eşlemesi**.

**Liste parça eşlemesi**: Bir parça eşlemesi içinde hangi parçalama anahtarları ayrı ayrı eşlenir. Karşılaştırılacak **aralık parça eşlemesi**.   

**Yerel parça eşlemesi**: Bir parça üzerinde depolanan, parça üzerinde bulunan parçacıklara eşlemeler yerel parça eşlemesini içerir.

**Çok parçalı sorgu**: Birden çok parça yönelik bir sorgu oluşturabilme olanağı; sonuç kümeleri (diğer adıyla "yaygın sorgu") tüm birleşim anlamsallarını kullanarak döndürülür. Karşılaştırılacak **verilere bağımlı yönlendirme**.

**Çok kiracılı** ve **tek kiracılı**: Bu, tek kiracılı veritabanı ile çok kiracılı veritabanı gösterir:

![Tek ve birden çok Kiracı veritabanları](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

İşte bir temsilini **parçalı** tek ve çok kiracılı veritabanları. 

![Tek ve birden çok Kiracı veritabanları](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

**Aralık parça eşlemesi**: Bir parça eşlemesi parça Dağıtım stratejisi birden çok bitişik değer aralıklarına üzerinde temel alır. 

**Başvuru tabloları**: Parçalı değildir ancak parçalar arasında çoğaltılan tablolar. Örneğin, posta kodları başvuru tablosunda depolanabilir. 

**Parça**: Parçalı bir veri kümesinden veri depolayan bir Azure SQL veritabanı. 

**Parça esnekliği**: Her ikisi de gerçekleştirme becerisi **yatay ölçeklendirme** ve **dikey ölçeklendirme**.

**Parçalı tablo**: Tablolar, yani parçalı,, verisini parçalama anahtar değerlerine göre parçalar arasında dağıtılır. 

**Parçalama anahtarı**: Verileri parçalar arasında nasıl dağıtıldığını belirler sütun değeri. Değer türü aşağıdakilerden biri olabilir: **int**, **bigint**, **varbinary**, veya **uniqueidentifier**. 

**Parça kümesi**: Parça eşleme Yöneticisi'nde aynı parça eşlemesine öznitelikli parçalar koleksiyonu.  

**Parçacık**: Tüm verileri bir parçalama anahtarı bir parça üzerinde tek bir değer ile ilişkili. Parçacık en küçük olası veri taşıma parçalı tablo yeniden dağıtırken birimidir. 

**Parça eşlemesi**: Parçalama anahtarları ve bunların ilgili parçalar arasındaki eşlemeler kümesidir.

**Parça eşleme Yöneticisi**: Parça harita(lar), parça konumlarını ve bir veya daha fazla parça kümeleri eşlemelerini içeren bir Yönetim nesnesi ve veri deposu.

![Eşlemeler][2]

## <a name="verbs"></a>Verbs
**Yatay ölçeklendirme**: (Veya) ölçeklendirme eylemi ekleyerek veya bir parça eşlemesine parçalar aşağıda gösterildiği gibi kaldırarak parçalar koleksiyonu.

![Yatay ve dikey ölçeklendirme][3]

**Birleştirme**: Parçacıklara iki parçadan bir parçaya taşıma ve parça eşlemesi uygun şekilde güncelleştirme işlemi.

**Parçacık taşıma**: Tek parçacık farklı bir parçaya taşıma işlemi. 

**Parça**: Aynı şekilde yapılandırılmış verileri arasında birden çok veritabanı parçalama anahtarına göre yatay olarak bölümleme işlemi.

**Bölünmüş**: Bazı parçacıklara (genellikle yeni) başka bir parçaya bir parçadan veri taşıma işlemi. Bir parçalama anahtarı bölünmüş noktası olarak kullanıcı tarafından sağlanır.

**Dikey ölçeklendirme**: Ölçeği artırılabilen (veya azaltılabilen) işlemi tek bir parçanın işlem boyutu. Örneğin, bir parça (Bu, daha fazla bilgi işlem kaynaklarına sonuçları) Premium standart değiştiriliyor. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

