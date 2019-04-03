---
title: En yeni nesil Azure SQL veri ambarı için yükseltme | Microsoft Docs
description: Azure SQL veri ambarı, yeni nesil Azure donanım ve depolama mimarisi için yükseltin.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/19/2019
ms.author: martinle
ms.reviewer: jrasnick
ms.openlocfilehash: a8bd260db7a141ce845ce7fb5b7e10f642907b82
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851082"
---
# <a name="optimize-performance-by-upgrading-sql-data-warehouse"></a>SQL Veri Ambarı’nı yükselterek performansı iyileştirme

Azure SQL veri ambarı, yeni nesil Azure donanım ve depolama mimarisi için yükseltin.

## <a name="why-upgrade"></a>Neden yükseltilsin mi?

Artık sorunsuz bir şekilde Azure portalında SQL veri ambarı işlem için iyileştirilmiş 2. nesil katmana yükseltebilirsiniz [desteklenen bölgeler](gen2-migration-schedule.md#automated-schedule-and-region-availability-table). Bölgenizde, kendi kendine yükseltme desteklemiyorsa yükseltme için desteklenen bir bölge ya da kendi Bölgenizde kullanılabilir olması için yükseltme bekleyin. Artık en yeni nesil Azure donanım ve daha hızlı performans, daha yüksek ölçeklenebilirlik ve sınırsız sütunlu depolama gibi gelişmiş depolama mimarisi yararlanmak için yükseltin. 

> [!VIDEO https://www.youtube.com/embed/9B2F0gLoyss]

## <a name="applies-to"></a>Uygulandığı öğe

İşlem için iyileştirilmiş Gen1 katmanı veri ambarında bu yükseltmenin uygulanabileceği [desteklenen bölgeler](gen2-migration-schedule.md#automated-schedule-and-region-availability-table).

## <a name="before-you-begin"></a>Başlamadan önce

1. Kontrol edin, [bölge](gen2-migration-schedule.md#automated-schedule-and-region-availability-table) GEN1 için Gen2'ye geçiş için desteklenir. Otomatik geçiş tarihleri unutmayın. Otomatik işlemine çakışmalarını önlemek için otomatik işlem başlangıç tarihinden önce el ile geçişinizi planlayın.
2. Henüz desteklenmeyen bir bölgede ise, eklenecek bölgeniz için denetlemeye devam veya [geri yükleme kullanarak yükseltme](#upgrade-from-an-azure-geographical-region-using-restore-through-the-azure-portal) desteklenen bir bölge.
3. Bölgeniz destekleniyorsa [Azure Portalı aracılığıyla yükseltme](#upgrade-in-a-supported-region-using-the-azure-portal)
4. **Önerilen performans düzeyi seçin** veri ambarı için temel alarak en iyi duruma getirilmiş Gen1 işlem katmanında geçerli performans düzeyiniz aşağıdaki eşlemeyi kullanarak:

   | İşlem için iyileştirilmiş Gen1 katmanı | İşlem için iyileştirilmiş 2. nesil katmanı |
   | :-------------------------: | :-------------------------: |
   |            DW100            |           DW100c            |
   |            DW200            |           DW200c            |
   |            DW300            |           DW300c            |
   |            DW400            |           DW400c            |
   |            DW500            |           DW500c            |
   |            DW600            |           DW500c            |
   |           DW1000            |           DW1000c           |
   |           DW1200            |           DW1000c           |
   |           DW1500            |           DW1500c           |
   |           DW2000            |           DW2000c           |
   |           DW3000            |           DW3000c           |
   |           DW6000            |           DW6000c           |

> [!Note]
> Önerilen performans düzeyleri doğrudan dönüştürme değildir. Örneğin, DW500c için DW600 geçmesini öneririz.

## <a name="upgrade-in-a-supported-region-using-the-azure-portal"></a>Desteklenen bir bölgede Azure portalını kullanarak yükseltme

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!NOTE]
> Gen1 Azure portalı üzerinden geçiş Gen2'ye kalıcıdır. İçin Gen1 döndürmek için bir işlem yoktur.  

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

1. Yükseltilecek en iyi duruma getirilmiş Gen1 işlem katmanı veri ambarı duraklatıldığında [veri ambarı sürdürme](pause-and-resume-compute-portal.md).

   > [!NOTE]
   > Azure SQL veri ambarı Gen2'ye için geçirmek için çalıştırılması gerekir.

2. Birkaç dakika kapalı kalma süresi için hazırlıklı olmalıdır. 

3. Tüm kod başvurularını işlem için iyileştirilmiş Gen1 performans düzeylerini tanımlayın ve bunları eşdeğer işlem için iyileştirilmiş Gen2 performans düzeylerini değiştirin. Aşağıdaki iki örnek verilmiştir, yükseltmeden önce kod başvurularını nerede güncelleştirmeniz gerekir:

   Özgün Gen1 PowerShell komutunu:

   ```powershell
   Set-AzSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300"
   ```

   Değiştirilmiş için:

   ```powershell
   Set-AzSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300c"
   ```

   > [!NOTE] 
   > -RequestedServiceObjectiveName "DW300"-RequestedServiceObjectiveName değiştirilir "DW300**c**"
   >

   Özgün Gen1 T-SQL komutu:

   ```SQL
   ALTER DATABASE mySampleDataWarehouse MODIFY (SERVICE_OBJECTIVE = 'DW300') ;
   ```

   Değiştirilmiş için:

   ```sql
   ALTER DATABASE mySampleDataWarehouse MODIFY (SERVICE_OBJECTIVE = 'DW300c') ; 
   ```
   > [!NOTE] 
   > Servıce_objectıve = 'DW300' servıce_objectıve için değiştirilen = ' DW300**c**'

## <a name="start-the-upgrade"></a>Yükseltmeyi başlatın

1. Azure portalında en iyi duruma getirilmiş Gen1 işlem katmanı veri ambarınıza gidin. Yükseltilecek en iyi duruma getirilmiş Gen1 işlem katmanı veri ambarı duraklatıldığında [veri ambarı sürdürme](pause-and-resume-compute-portal.md). 
2. Seçin **yükseltmek için 2. nesil** kartını görevleri sekmesi altında:  ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_1.png)
    
    > [!NOTE]
    > Görmüyorsanız, **yükseltmek için 2. nesil** kart görevleri sekmesindeki abonelik türü geçerli bölgede sınırlıdır.
    > [Bir destek bileti gönderin](sql-data-warehouse-get-started-create-support-ticket.md) , abonelik beyaz listeye almak için.

3. İş yükünüz çalışıyor ve sessiz modda yükseltmeden önce tamamlandı emin olun. Veri ambarınızın işlem için iyileştirilmiş Gen2 katmanı veri ambarı olarak yeniden çevrimiçi önce birkaç dakika kapalı kalma süresi deneyeceksiniz. **Yükseltme seçin**:

   ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_2.png)

4. **Yükseltme işlemini izleme** tarafından Azure portalında durumu denetleniyor:

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_3.png)

   İlk adım yükseltme işlemini burada tüm oturumları sonlandırılacak ve bağlantıları bırakılacak ölçeklendirme işlemi ("Yükseltme - çevrimdışı") gider. 

   Yükseltme işleminin ikinci adım, veri taşıma ("Yükseltme - Online") ' dir. Veri geçişi, bir çevrimiçi akışla arka plan işlemidir. Bu işlem, yerel SSD önbellek kullanmanın yeni depolama mimarisi için eski depolama mimariden sütunlu veri yavaş taşır. Bu süre boyunca, veri Ambarınızı sorgulamak ve yüklemek için çevrimiçi olacak. Verilerinizi olup olmadığını geçirildikten bağımsız olarak sorgulamak kullanılabilir. Veri geçişi, columnstore segmentleri sayısı veri boyutu ve performans düzeyinize bağlı olarak değişen hızlarında gerçekleşir. 

5. **İsteğe bağlı öneri:** Ölçeklendirme işlemi tamamlandıktan sonra veri geçiş arka plan işlemini hızlandırabilirsiniz. Veri taşıma çalıştırarak zorlayabilirsiniz [Alter Index yeniden](sql-data-warehouse-tables-index.md) tüm birincil columnstore tabloları, daha büyük bir SLO ve kaynak sınıfı sorgulama. Bu işlem **çevrimdışı** sayısını ve boyutları tablolarınızı bağlı olarak saat sürebilir akışla arka plan işlemi, karşılaştırılan. Ancak, işlem tamamlandıktan sonra veri geçişi yüksek kaliteli satır grupları ile yeni gelişmiş depolama mimarisi nedeniyle daha hızlı olacaktır.
 
> [!NOTE]
> ALTER INDEX yeniden çevrimdışı bir işlemdir ve tabloları yeniden tamamlanana kadar kullanılamaz.

Aşağıdaki sorgu, veri geçişi hızlandırmak için gerekli olan Alter Index REBUILD komutları oluşturur:

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

## <a name="upgrade-from-an-azure-geographical-region-using-restore-through-the-azure-portal"></a>Azure Portalı aracılığıyla geri yükleme kullanarak bir Azure coğrafi bölgesinde sürümünden yükseltme

## <a name="create-a-user-defined-restore-point-using-the-azure-portal"></a>Azure portalını kullanarak bir kullanıcı tanımlı bir geri yükleme noktası oluştur

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. İçin bir geri yükleme noktası oluşturmak istediğiniz SQL veri ambarı gidin.

3. Genel Bakış bölümünde üstüne seçin **+ yeni geri yükleme noktası**.

    ![Yeni Geri Yükleme Noktası](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_0.png)

4. Geri yükleme noktası için bir ad belirtin.

    ![Geri yükleme noktası adı](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_1.png)

## <a name="restore-an-active-or-paused-database-using-the-azure-portal"></a>Azure portalını kullanarak etkin ya da duraklatılmış bir veritabanı geri yükleme

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Öğesinden geri yüklemek istediğiniz SQL veri ambarı gidin.
3. Genel Bakış bölümünde üstüne seçin **geri**.

    ![ Geri Yüklemeye Genel Bakış](./media/sql-data-warehouse-restore-database-portal/restoring_0.png)

4. Şunlardan birini seçin **otomatik geri yükleme noktaları** veya **kullanıcı tanımlı bir geri yükleme noktaları**.

    ![Otomatik Geri Yükleme Noktaları](./media/sql-data-warehouse-restore-database-portal/restoring_1.png)

5. Kullanıcı tanımlı noktaları, geri yüklemek için **geri yükleme noktası seçin** veya **yeni bir kullanıcı tanımlı bir geri yükleme noktası oluşturma**. Bir Gen2 Server'da desteklenen coğrafi bölgeyi seçin. 

    ![Kullanıcı tanımlı bir geri yükleme noktaları](./media/sql-data-warehouse-restore-database-portal/restoring_2_udrp.png)

## <a name="restore-from-an-azure-geographical-region-using-powershell"></a>PowerShell kullanarak bir Azure coğrafi bölgesinden geri yükleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir veritabanını kurtarmak için kullanmak [geri yükleme-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) cmdlet'i.

> [!NOTE]
> Gen2'ye coğrafi geri yükleme bir gerçekleştirebileceğiniz! Bunu yapmak için bir Gen2 ServiceObjectiveName belirtin (örneğin DW1000**c**) isteğe bağlı bir parametre olarak.

1. Windows PowerShell'i açın.
2. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeleyin.
3. Geri yüklenecek veritabanını içeren aboneliği seçin.
4. Kurtarmak istediğiniz veritabanını alır.
5. Bir Gen2 ServiceObjectiveName belirterek bu veritabanı için kurtarma isteği oluşturun.
6. Coğrafi geri veritabanının durumunu doğrulayın.

```Powershell
Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName "<YourTargetServiceLevel>" -RequestedServiceObjectiveName "DW300c"

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> Geri yükleme tamamlandıktan sonra veritabanını yapılandırmak için bkz: [kurtarma işleminden sonra veritabanını yapılandırma](../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery).

Kaynak veritabanı TDE etkinse kurtarılmış veritabanını TDE etkin olacaktır.


Veri Ambarınızda sorunla karşılaşırsanız, oluşturun bir [destek isteği](sql-data-warehouse-get-started-create-support-ticket.md) ve "Gen2'ye yükseltme" olası nedeni olarak başvuru.

## <a name="next-steps"></a>Sonraki adımlar

Yükseltilen veri Ambarınızı çevrimiçidir. Gelişmiş mimari yararlanmak için bkz: [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md).
