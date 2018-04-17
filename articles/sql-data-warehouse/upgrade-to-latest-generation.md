---
title: Azure SQL Data Warehouse yeni nesil yükseltin | Microsoft Docs
description: Yeni nesil Azure donanım ve depolama mimarisi için Azure SQL veri ambarını yükseltmek için adımlar.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.services: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/02/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 724f027f3f43cd0ad846210b511c8fc1af27153f
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="optimize-performance-by-upgrading-sql-data-warehouse"></a>SQL Veri Ambarı’nı yükselterek performansı iyileştirme

Şimdi sorunsuz bir şekilde Azure portalında işlem performans katmanı için iyileştirilmiş yükseltebilirsiniz. Esneklik veri ambarı için bir iyileştirilmiş varsa, Azure donanım ve geliştirilmiş depolama mimarisi için en yeni nesil yükseltmeniz önerilir. Daha hızlı performans, daha yüksek ölçeklenebilirlik ve sınırsız sütunlu depolama yararlanmak kuramaz. 

## <a name="applies-to"></a>Uygulandığı öğe:
Bu yükseltme esneklik performans katmanı için iyileştirilmiş veri ambarlarında uygular.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce

> [!NOTE]
> 3/30'dan sonra bilmeniz gereken [sunucu düzeyi denetim](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing#subheading-8) yükseltme işlemine başlamadan önce devre dışı.
> 
>

> [!NOTE]
> Var olan veri ambarı değil esneklik için en iyi hale getirilmiş bir bölgede için en iyi duruma getirilmiş burada işlem kullanılabilir, yapabilecekleriniz [coğrafi geri yükleme işlem için iyileştirilmiş için](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region) desteklenen bir bölge için PowerShell aracılığıyla.
> 
>

1. Yükseltilecek esneklik veri ambarı için iyileştirilmiş duraklatıldığında [veri ambarı sürdürmek](pause-and-resume-compute-portal.md).
2. Kapalı kalma süresi birkaç dakika için hazır olun. 



## <a name="start-the-upgrade"></a>Yükseltme işlemini başlatmak

1. Veri ambarı Azure portalında ve tıklayın esneklik için iyileştirilmiş gidin **'e yükseltmek için iyileştirilmiş işlem**: ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_1.png)

2. Varsayılan olarak, **önerilen performans düzeyi seçin** veri ambarı için temel alarak, geçerli esneklik için iyileştirilmiş performans düzeyini eşleme kullanarak:
    
| Elastiklik için İyileştirilmiş | İşlem için İyileştirilmiş |
| :----------------------: | :-------------------: |
|      DW100 – DW1000      |        DW1000c        |
|          DW1200          |        DW1500c        |
|          DW1500          |        DW1500c        |
|          DW2000          |        DW2000c        |
|          DW3000          |        DW3000c        |
|          DW6000          |        DW6000c        |


3. İş yükünüzün çalıştığından ve sessiz modda yükseltmeden önce tamamlandı emin olun. Veri ambarınız işlem veri ambarı için bir iyileştirilmiş olarak yeniden çevrimiçi olduktan birkaç dakika için kapalı kalma yaşayacaktır. **Yükselt'i tıklatın**. İşlem performans katmanı için iyileştirilmiş bedelinin Önizleme dönemi boyunca şu anda yarı-kapalıdır:
    
    ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_2.png)

4. **Yükseltme izlemek** Azure portalında durumunu denetleyerek:

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_3.png)
   
   Yükseltme işleminin ilk adımı, burada tüm oturumları sonlandırılacak ve bağlantıları bırakılacak ölçeklendirme işlemi ("Yükseltme - çevrimdışı") gider. 
   
   İkinci adım yükseltme işlemini veri geçiş ("Yükseltme - çevrimiçi") olur. Veri geçişi yavaş sütunlu verileri yerel bir SSD önbellek yararlanarak yeni depolama mimarisi için eski depolama mimarisinden taşır bir çevrimiçi akışla arka plan işlemidir. Bu süre boyunca, veri Ambarınızı sorgulama ve yükleme için çevrimiçi olacaktır. Tüm verilerinizi olup olmadığını geçirildikten bağımsız olarak sorgulamak kullanılabilir. Veri boyutu, performans düzeyi ve, columnstore Segment sayısına bağlı olarak değişen bir hızda veri geçişi yapılır. 

5. **İsteğe bağlı öneri:** veri geçiş arka plan işlemi hızlandırmak için hemen çalıştırarak veri taşıma zorlamak için önerilir [Alter Index yeniden](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-tables-index) daha büyük bir SLO ve kaynak tüm columnstore tabloları sınıf. Bu akışla arka plan işleme karşılaştırıldığında çevrimdışı bir işlemdir; Ancak, veri geçişi burada daha sonra yeni gelişmiş depolama mimarisi ile yüksek kaliteli rowgroups kez tamamlandı tam anlamıyla alabilir çok daha hızlı olacaktır. 

Bu aşağıdaki sorguyu veri geçiş işlemi hızlandırmak için gerekli olan Alter Index REBUILD komutları oluşturur:

```sql
SELECT 'ALTER INDEX [' + idx.NAME + '] ON [' 
       + Schema_name(tbl.schema_id) + '].[' 
       + Object_name(idx.object_id) + '] REBUILD ' + ( CASE 
                                                         WHEN ( 
                                                     (SELECT Count(*) 
                                                      FROM   sys.partitions 
                                                             part2 
                                                      WHERE  part2.index_id 
                                                             = idx.index_id 
                                                             AND 
                                                     idx.object_id = 
                                                     part2.object_id) 
                                                     > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              ELSE '' 
                                                       END ) + '; SELECT ''[' + 
              idx.NAME + '] ON [' + Schema_name(tbl.schema_id) + '].[' + 
              Object_name(idx.object_id) + '] ' + ( 
              CASE 
                WHEN ( (SELECT Count(*) 
                        FROM   sys.partitions 
                               part2 
                        WHERE 
                     part2.index_id = 
                     idx.index_id 
                     AND idx.object_id 
                         = part2.object_id) > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              + ' completed'';' 
              ELSE ' completed'';' 
                                                    END ) 
FROM   sys.indexes idx 
       INNER JOIN sys.tables tbl 
               ON idx.object_id = tbl.object_id 
       LEFT OUTER JOIN sys.partitions part 
                    ON idx.index_id = part.index_id 
                       AND idx.object_id = part.object_id 
WHERE  idx.type_desc = 'CLUSTERED COLUMNSTORE'; 
```



## <a name="next-steps"></a>Sonraki adımlar
Yükseltilen veri ambarınız çevrimiçidir. Gelişmiş mimari yararlanmak için bkz: [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md).
 
