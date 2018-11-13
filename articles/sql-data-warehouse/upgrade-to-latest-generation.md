---
title: En yeni nesil Azure SQL veri ambarı için yükseltme | Microsoft Docs
description: Azure SQL veri ambarı, yeni nesil Azure donanım ve depolama mimarisi için yükseltin.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/22/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: fe1f2e026aaa4260d34b9b1cb96064053af1c3c7
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568021"
---
# <a name="optimize-performance-by-upgrading-sql-data-warehouse"></a>SQL Veri Ambarı’nı yükselterek performansı iyileştirme
Azure SQL veri ambarı, yeni nesil Azure donanım ve depolama mimarisi için yükseltin.

## <a name="why-upgrade"></a>Neden yükseltilsin mi?
Artık sorunsuz bir şekilde Azure portalında SQL veri ambarı işlem için iyileştirilmiş 2. nesil katmana yükseltebilirsiniz. Bir işlem için iyileştirilmiş Gen1 katmanı veri ambarı varsa, yükseltme önerilir. Yükselterek, depolama mimarisi Gelişmiş olmak ve son nesli olan Azure donanım kullanabilirsiniz. Daha hızlı performans, daha yüksek ölçeklenebilirlik ve sınırsız sütunlu depolamayı yararlanabilirsiniz. 

> [!VIDEO https://www.youtube.com/embed/9B2F0gLoyss]

## <a name="applies-to"></a>Uygulandığı öğe
Bu yükseltme en iyi duruma getirilmiş Gen1 işlem katmanı veri ambarları için geçerlidir.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="before-you-begin"></a>Başlamadan önce
> [!NOTE]
> Var olan işlem için iyileştirilmiş Gen1 katmanı veri ambarınızın işlem için iyileştirilmiş Gen2 katmanı kullanılabildiği bir bölgede değil ise, yapabilecekleriniz [coğrafi geri yükleme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region) desteklenen bir bölge için PowerShell aracılığıyla.
> 
>

1. Yükseltilecek en iyi duruma getirilmiş Gen1 işlem katmanı veri ambarı duraklatıldığında [veri ambarı sürdürme](pause-and-resume-compute-portal.md).
2. Birkaç dakika kapalı kalma süresi için hazırlıklı olmalıdır. 



## <a name="start-the-upgrade"></a>Yükseltmeyi başlatın

1. Git, işlem için iyileştirilmiş katmanı veri ambarı Azure Portalı'nda ve tıklayarak Gen1 **yükseltmek için 2. nesil** kartını görevleri sekmesi altındaki: ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_1.png)
    
> [!NOTE]
> Görmüyorsanız, **yükseltmek için 2. nesil** kart görevleri sekmesindeki abonelik türü geçerli bölgede sınırlıdır. [Bir destek bileti gönderin](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) , abonelik beyaz listeye almak için.

2. Varsayılan olarak, **önerilen performans düzeyi seçin** veri ambarı için temel alarak en iyi duruma getirilmiş Gen1 işlem katmanında geçerli performans düzeyiniz aşağıdaki eşlemeyi kullanarak:
    
   | İşlem için iyileştirilmiş Gen1 katmanı | İşlem için iyileştirilmiş 2. nesil katmanı |
   | :----------------------: | :-------------------: |
   |      DEĞERİ DW100 – DW600       |        DW500c         |
   |          DW1000          |        DW1000c        |
   |          DW1200          |        DW1500c        |
   |          DW1500          |        DW1500c        |
   |          DW2000          |        DW2000c        |
   |          DW3000          |        DW3000c        |
   |          DW6000          |        DW6000c        |

3. İş yükünüz çalışıyor ve sessiz modda yükseltmeden önce tamamlandı emin olun. Veri ambarınızın işlem için iyileştirilmiş Gen2 katmanı veri ambarı olarak yeniden çevrimiçi önce birkaç dakika kapalı kalma süresi yaşar. **Yükseltme**. En iyi duruma getirilmiş Gen2 işlem katmanı performans katmanı fiyatına Önizleme dönemi boyunca şu anda yarı-kapalıdır:
    
   ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_2.png)

4. **Yükseltme işlemini izleme** tarafından Azure portalında durumu denetleniyor:

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_3.png)
   
   İlk adım yükseltme işlemini burada tüm oturumları sonlandırılacak ve bağlantıları bırakılacak ölçeklendirme işlemi ("Yükseltme - çevrimdışı") gider. 
   
   Yükseltme işleminin ikinci adım, veri taşıma ("Yükseltme - Online") ' dir. Veri geçişi yavaş sütunlu veri bir yerel SSD önbellek yararlanan yeni depolama mimarisi için eski depolama mimariden taşır bir çevrimiçi akışla arka plan işlemidir. Bu süre boyunca, veri Ambarınızı sorgulamak ve yüklemek için çevrimiçi olacak. Tüm verilerinizi olup olmadığını geçirildikten bağımsız olarak sorgulamak kullanılabilir. Veri geçişi, columnstore segmentleri sayısı veri boyutu ve performans düzeyinize bağlı olarak değişen bir hızda olur. 

5. **İsteğe bağlı öneri:** veri geçiş arka plan işlemi hızlandırmak için hemen veri taşıma çalıştırarak zorlayabilirsiniz [Alter Index yeniden](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-index) , sorgulama sırasında daha büyük bir SLO tüm birincil columnstore tabloları ve kaynak sınıfı. Bu işlem **çevrimdışı** sayısını ve boyutları tablolarınızı bağlı olarak saat sürebilir akışla arka plan işlemi karşılaştırıldığında; ancak, veri geçişi burada daha sonra tam anlamıyla alabilir çok daha hızlı olacaktır Yeni depolama mimarisi tamamlandıktan sonra yüksek kaliteli satır grupları ile geliştirilmiştir. 

Aşağıdaki sorgu, veri geçiş sürecini hızlandırmak için gerekli olan Alter Index REBUILD komutları oluşturur:

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
Yükseltilen veri Ambarınızı çevrimiçidir. Gelişmiş mimari yararlanmak için bkz: [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md).
 
