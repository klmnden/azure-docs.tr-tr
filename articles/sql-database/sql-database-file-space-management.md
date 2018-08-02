---
title: Azure SQL veritabanı dosya alanı yönetimi | Microsoft Docs
description: Bu sayfada Azure SQL veritabanı ile dosya alanı yönetme işlemi açıklanır ve nasıl gerçekleştirmek için bir veritabanı küçültme işlemi nasıl de bir veritabanı küçültme gerekip gerekmediğini belirlemek için kod örneği sağlanmıştır.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: how-to
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: carlrab
ms.openlocfilehash: 1ecc0ce08ef42f5f5935bca29e8269be2ea142f0
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39415151"
---
# <a name="manage-file-space-in-azure-sql-database"></a>Azure SQL veritabanı'nda dosya alanı yönetme

Bu makalede, Azure SQL veritabanı ve dosya alanı veritabanları için ayrılan ve elastik havuzlar müşteri tarafından yönetilmesi gerekiyor, gerçekleştirilen adımlar, depolama alanının farklı türleri açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Azure SQL veritabanı'nda, Azure portalı ve aşağıdaki API'leri gösterilen depolama boyutu ölçümleri, veritabanları ve elastik havuzlar için kullanılan veri sayfaların sayısını ölçmek:
- Azure Resource Manager tabanlı ölçümleri API'leri PowerShell dahil olmak üzere [get-metrics](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermmetric)
- T-SQL: [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)
- T-SQL: [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL: [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

İş yükü düzenleri alan ayırmak, temel alınan veri dosyalarında veritabanları için kullanılan veri sayfalarında veri dosyalarının sayısından daha büyük hale vardır. Bu senaryoda kullanılan alanı artar ve ardından veri sonradan silindiğinde ortaya çıkabilir. Veriler silindiğinde, ayrılmış dosya alanı verileri silindiğinde otomatik olarak alınmaz. Böyle senaryolarda ayrılan alanı bir veritabanı havuzu desteklenen en üst sınırlara kümesi aşabilir (veya desteklenen) veritabanı için ve sonuç olarak, veri büyümesini engellemek veya gerçekten kullanılan veritabanı boş alanı en büyük değerinden küçük olsa da performans katmanı değişiklikleri, engelleme alanı sınırı. Azaltmak için veritabanında ayrılmış ancak kullanılmayan alanı azaltmak için veritabanı daraltma gerekebilir.

SQL veritabanı hizmeti otomatik olarak kullanılmayan bir ayrılmış alanı veritabanı performansı potansiyel etkisi nedeniyle geri kazanmak için veritabanı dosyalarını Küçült değil. Ancak bir zaman içinde açıklanan adımları takip ederek, seçtiğiniz bir veritabanı dosyasında küçültebilirsiniz [geri kullanılmayan ayrılmış alanı](#reclaim-unused-allocated-space). 

> [!NOTE]
> Olduğundan bu işlem veritabanı performansını etkilemez veri dosyalarının aksine, SQL veritabanı hizmeti günlük dosyalarını otomatik olarak küçülür.

## <a name="understanding-the-types-of-storage-space-for-a-database"></a>Bir veritabanı için depolama alanı türlerini anlama

Dosya alanı yönetmek için hem tek veritabanı ve elastik havuzlar için veritabanı depolama alanı ile ilgili aşağıdaki terimler anlamanız gerekir.

|Depolama alanı terimi|Tanım|Yorumlar|
|---|---|---|
|**Kullanılan veri alanı**|8 KB'lık sayfalarında veritabanı verilerini depolamak için kullanılan alanı miktarı.|Genellikle, bu alanı ekler (siler) üzerinde artar (azaldıkça) kullanılır. Bazı durumlarda, kullanılan alanı üzerinde ekler değiştirmez veya tutar ve veri işleme ve her türlü parçalanma desenini bağlı olarak siler. Örneğin, her veri sayfasından bir satırın silinmesi mutlaka kullanılan alanı azalmaz.|
|**Ayrılan alanı**|Veritabanı verilerini depolamak için kullanılabilir hale dosya alanı miktarını biçimlendirilmiş|Ayrılan alanı otomatik olarak artar, ancak hiçbir zaman siler sonra azaltır. Bu davranış, gelecekteki ekler alanı yeniden biçimlendirildi gerekmez. bu yana daha hızlı olmasını sağlar.|
|**Ancak kullanılmamış ayrılmış alanı**|Kullanılmayan verileri dosya alanı veritabanı için ayrılan miktarı.|Bu miktar, ayrılan alan miktarını ve kullanılan alanı arasındaki fark ve en fazla veritabanı dosyalarını küçülterek kazanılabilir alanını temsil eder.|
|**En büyük boyutu**|En fazla veritabanı tarafından kullanılan veri alanı miktarı.|Ayrılan veri alanı dışında veri boyutu kuramaz.|
||||

Aşağıdaki diyagramda, depolama alanı türleri arasındaki ilişkiyi gösterir.

![depolama alanı türleri ve ilişkiler](./media/sql-database-file-space-management/storage-types.png)

## <a name="query-a-database-for-storage-space-information"></a>Depolama alanı bilgisi için bir veritabanını sorgulama

Ancak kullanılmayan ayırmış olmanız durumunda belirlemek için tek bir veritabanı veri alanı geri kazanmak isteyebilir kullanın Aşağıdaki sorgular:

### <a name="database-data-space-used"></a>Kullanılan veritabanı veri alanı
MB cinsinden kullanılan veritabanı veri alan miktarı aşağıdaki sorguyu değiştirin.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC
```

### <a name="database-data-allocated-and-allocated-space-unused"></a>Veritabanı verilerinin ayrılan ve ayrılmış alanı kullanılmayan
Veritabanı veri miktarı ayrılan ve ayrılmış kullanılmayan alanı geri dönmek için aşağıdaki sorguyu değiştirin.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, 
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB 
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS'
```
 
### <a name="database-max-size"></a>Veritabanı boyutu üst sınırını
Veritabanı boyutu üst sınırını bayt cinsinden döndürmek için aşağıdaki sorguyu değiştirin.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes
```

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Elastik havuz depolama alanı bilgisi için sorgulama

Ancak kullanılmayan ayırmış olmanız durumunda belirlemek için elastik havuzlara ve havuza alınmış her veritabanı için veri alanı geri kazanmak isteyebilir kullanın Aşağıdaki sorgular:

### <a name="elastic-pool-data-space-used"></a>Kullanılan elastik havuzu veri alanı
MB cinsinden kullanılan elastik havuzu veri alanı miktarı aşağıdaki sorguyu değiştirin.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

### <a name="elastic-pool-data-allocated-and-allocated-space-unused"></a>Elastik havuz veri ayrılan ve ayrılmış alanı kullanılmayan

Ayrılan toplam alan ve kullanılmayan bir elastik havuzdaki her veritabanı için ayrılan listeleyen bir tablo döndürmek için aşağıdaki PowerShell betiğini değiştirin. Tabloyu sıralar ayrılan alanı miktarı en yüksek olan veritabanlarından kullanılmayan ayrılmış alanı en az miktardaki kullanılmayan.  

Havuzdaki her veritabanı için ayrılan alanı belirlemek için sorgu sonuçları birlikte ayrılmış elastik havuz alanı belirleme eklenebilir. Elastik havuz en büyük boyutu ayrılan esnek havuz alanı aşmamalıdır.  

```powershell
# Resource group name
$resourceGroupName = "rg1" 
# Server name
$serverName = "ls2" 
# Elastic pool name
$poolName = "ep1"
# User name for server
$userName = "name"
# Password for server
$password = "password"

# Get list of databases in elastic pool
$databasesInPool = Get-AzureRmSqlElasticPoolDatabase `
    -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -ElasticPoolName $poolName
$databaseStorageMetrics = @()

# For each database in the elastic pool,
# get its space allocated in MB and space allocated unused in MB.
# Requires SQL Server PowerShell module – see here to install.  
foreach ($database in $databasesInPool)
{
    $sqlCommand = "SELECT DB_NAME() as DatabaseName, `
    SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, `
    SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB `
    FROM sys.database_files `
    GROUP BY type_desc `
    HAVING type_desc = 'ROWS'"
    $serverFqdn = "tcp:" + $serverName + ".database.windows.net,1433"
    $databaseStorageMetrics = $databaseStorageMetrics + 
        (Invoke-Sqlcmd -ServerInstance $serverFqdn `
        -Database $database.DatabaseName `
        -Username $userName `
        -Password $password `
        -Query $sqlCommand)
}
# Display databases in descending order of space allocated unused
Write-Output "`n" "ElasticPoolName: $poolName"
Write-Output $databaseStorageMetrics | Sort `
    -Property DatabaseDataSpaceAllocatedUnusedInMB `
    -Descending | Format-Table
```

Aşağıdaki ekran görüntüsünde, komut çıktısı örneğidir:

![Elastik havuz ayrılan alanı ve kullanılmayan ayrılmış örnek](./media/sql-database-file-space-management/elastic-pool-allocated-unused.png)

### <a name="elastic-pool-max-size"></a>Elastik havuz en büyük boyutu

Elastik veritabanı boyutu, MB olarak döndürmek için aşağıdaki T-SQL sorgusunu kullanın.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

## <a name="reclaim-unused-allocated-space"></a>Kullanılmayan ayrılmış alanı geri kazanmalıdır

Sahip olduğunuz belirledikten sonra geri kazanmak için istediğiniz kullanılmayan ayrılmış alanı ayrılan veritabanı alanı daraltmak için aşağıdaki komutu kullanın. 

> [!IMPORTANT]
> En çok ayrılan alanı ile bir elastik havuzdaki veritabanları için veritabanı kullanılmayan dosya alanı en hızlı bir şekilde geri kazanmak için önce TempDB'de.  

Belirtilen veritabanında veri dosyalarının tümünü daraltmak için aşağıdaki komutu kullanın:

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'<database_name>')
```

Bu komut hakkında daha fazla bilgi için bkz. [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql).

> [!IMPORTANT] 
> Veritabanı veri dosyaları sonra yeniden oluşturma veritabanı dizinleri küçültülebilir, dizinleri parçalanmış ve performans iyileştirme verimliliğine kaybetmek göz önünde bulundurun. Bu meydana gelirse, dizinleri yeniden. Parçalanma ve dizinlerini yeniden oluşturma hakkında daha fazla bilgi için bkz. [Reorganıze ve dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).

## <a name="next-steps"></a>Sonraki adımlar

- En büyük veritabanı boyutları hakkında daha fazla bilgi için bkz:
  - [Azure SQL veritabanı sanal çekirdek tabanlı model sınırları tek bir veritabanı için satın alma](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases)
  - [DTU tabanlı satın alma modeli kullanarak tek veritabanı kaynak sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-single-databases)
  - [Azure SQL veritabanı sanal çekirdek tabanlı model sınırları elastik havuzlar için satın alma](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)
  - [DTU tabanlı satın alma modeli kullanarak elastik havuzlar için kaynak sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools)
- Hakkında daha fazla bilgi için `SHRINKDATABASE` komutu, bkz: [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql). 
- Parçalanma ve dizinlerini yeniden oluşturma hakkında daha fazla bilgi için bkz. [Reorganıze ve dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).