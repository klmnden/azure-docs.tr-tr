---
title: Azure SQL veritabanı tek ve havuza veritabanları dosya alanı yönetimi | Microsoft Docs
description: Bu sayfada Azure SQL veritabanı'nda tek ve havuza alınmış veritabanlarıyla dosya alanı yönetme işlemi açıklanır ve nasıl gerçekleştirmek için bir veritabanı küçültme işlemi nasıl tek veya havuza alınmış bir veritabanı da küçültme gerekip gerekmediğini belirlemek için kod örneği sağlanmıştır.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: jrasnick, carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 043ceb6c46155ed169c080d08f37688b47e3e4b9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66123324"
---
# <a name="manage-file-space-for-single-and-pooled-databases-in-azure-sql-database"></a>Azure SQL veritabanı'nda tek ve havuza alınmış veritabanları için dosya alanı yönetme

Bu makalede Azure SQL veritabanı ve dosya alanı veritabanları için ayrılan zaman atılabilecek adımlar tek ve havuza alınmış veritabanları için depolama alanı farklı türde ve elastik havuzlar açıkça yönetilmesi gerekiyor.

> [!NOTE]
> Bu makalede, Azure SQL veritabanı yönetilen örnek dağıtım seçeneği için geçerli değildir.

## <a name="overview"></a>Genel Bakış

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

Azure SQL veritabanı'nda tek ve havuza alınmış veritabanları ile vardır iş yükü düzenleri ayırma veritabanları için temel alınan veri dosyaları burada kullanılan veri sayfaları tutardan daha büyük olabilir. Bu durum kullanılan alanın artması ve sonrasında verilerin silinmesi durumunda ortaya çıkabilir. Bunun nedeni, veri silindiğinde ayrılmış dosya alanı otomatik olarak alınmaz olmasıdır.

Aşağıdaki senaryolarda dosya alanı kullanımının izlenmesi ve veri dosyalarının küçültülmesi gerekli olabilir:

- Bir elastik havuzun veritabanları için ayrılan dosya alanının maksimum havuz boyutuna erişmesi durumunda veri artışına izin verilmesi.
- Tek bir veritabanının veya elastik havuzun maksimum boyutunun küçülmesine izin verilmesi.
- Tek bir veritabanının veya elastik havuzun daha düşük maksimum boyuta sahip farklı bir hizmet katmanına veya performans katmanına geçmesine izin verilmesi.

### <a name="monitoring-file-space-usage"></a>Dosya alanı kullanımı izleme

Azure portalı ve aşağıdaki API'leri gösterilen Çoğu depolama alanı ölçümleri yalnızca kullanılan veri sayfaların boyutu ölçü:

- Azure Resource Manager tabanlı ölçümleri API'leri PowerShell dahil olmak üzere [get-metrics](https://docs.microsoft.com/powershell/module/az.monitor/get-azmetric)
- T-SQL: [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)

Ancak aşağıdaki API'leri veritabanları ve elastik için ayrılan alanı boyutu da ölçüm havuzları:

- T-SQL: [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL: [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

### <a name="shrinking-data-files"></a>Veri dosyalarını daraltma

SQL veritabanı hizmeti, veritabanı performans için potansiyel etkisi nedeniyle kullanılmayan ayrılmış alan kazanmak için veri dosyalarını otomatik olarak küçülmez.  Müşterilerin kendi seçme içinde açıklanan adımları izleyerek bir zaman Self Servis aracılığıyla veri dosyalarını ancak küçültülebilir [ayrılmış alanı geri kullanılmayan](#reclaim-unused-allocated-space).

> [!NOTE]
> Olduğundan bu işlem veritabanı performansını etkilemez veri dosyalarının aksine, SQL veritabanı hizmeti günlük dosyalarını otomatik olarak küçülür. 

## <a name="understanding-types-of-storage-space-for-a-database"></a>Bir veritabanı için depolama alanı türlerini anlama

Aşağıdaki depolama alanı miktarları anlama bir veritabanının dosya alanı yönetmek için önemli.

|Veritabanı miktar|Tanım|Açıklamalar|
|---|---|---|
|**Kullanılan veri alanı**|8 KB'lık sayfalarında veritabanı verilerini depolamak için kullanılan alanı miktarı.|Genellikle, artar (azaldıkça) ekler (siler) üzerinde kullanılan alan. Bazı durumlarda, kullanılan alanı üzerinde ekler değiştirmez veya tutar ve veri işleme ve her türlü parçalanma desenini bağlı olarak siler. Örneğin, her veri sayfasından bir satırın silinmesi mutlaka kullanılan alanı azalmaz.|
|**Ayrılmış veri alanı**|Veritabanı verilerini depolamak için kullanılabilir hale dosya alanı miktarını biçimlendirilmiş.|Ayrılan alan miktarını otomatik olarak artar, ancak hiçbir zaman siler sonra azaltır. Bu davranış, gelecekteki ekler alanı yeniden biçimlendirildi gerekmez. bu yana daha hızlı olmasını sağlar.|
|**Ancak kullanılmamış ayrılmış veri alanı**|Ayrılmış veri alanı miktarına ve kullanılan veri alanı arasındaki fark.|Bu miktar, en fazla veritabanı veri dosyalarını küçülterek kazanılabilir boş alan miktarını temsil eder.|
|**Veri en büyük boyutu**|Veritabanı verilerini depolamak için kullanılan alanı maksimum miktarı.|Veri alanı ayrılan miktarı veri boyutu kuramaz.|
||||

Aşağıdaki diyagramda, farklı bir veritabanı için depolama alanı türleri arasındaki ilişkiyi gösterir.

![depolama alanı türleri ve ilişkiler](./media/sql-database-file-space-management/storage-types.png)

## <a name="query-a-single-database-for-storage-space-information"></a>Depolama alanı bilgisi için tek bir veritabanını sorgulama

Aşağıdaki sorgularda, tek bir veritabanı için depolama alanı miktarları belirlemek için kullanılabilir.  

### <a name="database-data-space-used"></a>Kullanılan veritabanı veri alanı

Kullanılan veritabanı veri alanı miktarı aşağıdaki sorguyu değiştirin.  MB cinsinden sorgu sonucu birimleridir.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC
```

### <a name="database-data-space-allocated-and-unused-allocated-space"></a>Ayrılan veritabanı veri alanı ve kullanılmayan bir ayrılmış alanı

Veritabanı veri alan ayrılan miktarı ve ayrılan kullanılmayan alanı miktarını döndürmek için aşağıdaki sorguyu kullanın.  MB cinsinden sorgu sonucu birimleridir.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, 
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB 
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS'
```
 
### <a name="database-data-max-size"></a>Veritabanı veri en büyük boyutu

Veritabanı veri en büyük boyutu döndürmek için aşağıdaki sorguyu değiştirin.  Bayt cinsinden sorgu sonucu birimleridir.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes
```

## <a name="understanding-types-of-storage-space-for-an-elastic-pool"></a>Elastik havuz için depolama alanı türlerini anlama

Aşağıdaki depolama alanı miktarları anlama dosya alanı, bir elastik havuzun yönetmek için önemli.

|Elastik havuz miktar|Tanım|Açıklamalar|
|---|---|---|
|**Kullanılan veri alanı**|Esnek havuzdaki tüm veritabanları tarafından kullanılan veri alanı toplamı.||
|**Ayrılmış veri alanı**|Esnek havuzdaki tüm veritabanları tarafından ayrılan veri alanı toplamı.||
|**Ancak kullanılmamış ayrılmış veri alanı**|Ayrılan veri miktarını ve esnek havuzdaki tüm veritabanları tarafından kullanılan veri alanı arasındaki fark.|Bu miktar, en fazla veritabanı veri dosyalarını küçülterek kazanılabilir esnek havuzu için ayrılan alan miktarını temsil eder.|
|**Veri en büyük boyutu**|Tüm veritabanları için elastik havuz tarafından kullanılabilecek veri alanı maksimum miktarı.|Elastik havuz boyutu elastik havuz için ayrılan alanı aşmamalıdır.  Bu durum ortaya çıkarsa, kullanılmayan bir ayrılmış alanı veritabanı veri dosyalarını küçülterek kazanılabilir.|
||||

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Elastik havuz depolama alanı bilgisi için sorgulama

Aşağıdaki sorgularda, bir elastik havuz için depolama alanı miktarları belirlemek için kullanılabilir.  

### <a name="elastic-pool-data-space-used"></a>Kullanılan elastik havuzu veri alanı

Kullanılan elastik havuzu veri alanı miktarı aşağıdaki sorguyu değiştirin.  MB cinsinden sorgu sonucu birimleridir.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent / 100.0 * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

### <a name="elastic-pool-data-space-allocated-and-unused-allocated-space"></a>Elastik havuz veri alanı ayrılan ve kullanılmayan bir ayrılmış alanı

Ayrılan alanı listeleyen bir tablo döndürmek için aşağıdaki PowerShell betiğini ve elastik bir havuzdaki her veritabanı için kullanılmayan bir ayrılmış alanı değiştirin. Tablo, ayrılmış alanı kullanılmayan en az miktarda için ayrılan alan bu miktarı en yüksek kullanılmayan veritabanlarıyla veritabanlarından sıralar.  MB cinsinden sorgu sonucu birimleridir.  

Havuzdaki her veritabanı için ayrılan alanı birlikte elastik havuz için ayrılan toplam alan belirlemek eklenebilir belirlemek için sorgu sonuçları. Elastik havuz en büyük boyutu ayrılan esnek havuz alanı aşmamalıdır.  

SQL Server PowerShell modülü, PowerShell betiğini gerektirir; bkz [indirme PowerShell Modülü](https://docs.microsoft.com/sql/powershell/download-sql-server-ps-module) yüklemek için.

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
$databasesInPool = Get-AzSqlElasticPoolDatabase `
    -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -ElasticPoolName $poolName
$databaseStorageMetrics = @()

# For each database in the elastic pool,
# get its space allocated in MB and space allocated unused in MB.
  
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

### <a name="elastic-pool-data-max-size"></a>Elastik havuz veri en büyük boyutu

Elastik havuz veri en büyük boyutu döndürmek için aşağıdaki T-SQL sorgusunu değiştirin.  MB cinsinden sorgu sonucu birimleridir.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

## <a name="reclaim-unused-allocated-space"></a>Kullanılmayan ayrılmış alanı geri kazanmalıdır

### <a name="dbcc-shrink"></a>DBCC küçültme

Veritabanları yeniden kazanmaktan kullanılmayan ayrılmış alanı belirledikten sonra her veritabanı için veri dosyalarının daraltmak için aşağıdaki komutta veritabanı adını değiştirin.

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'db1')
```

Bu komut, çalışıyor ve mümkünse düşük kullanım dönemleri sırasında çalıştırılmalıdır veritabanı performansı etkileyebilir.  

Bu komut hakkında daha fazla bilgi için bkz. [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql). 

### <a name="auto-shrink"></a>Otomatik Daralt

Alternatif olarak, otomatik küçültme bir veritabanı için etkin hale getirilebilir.  Otomatik küçültme dosya yönetim karmaşıklığı azaltır ve veritabanı performansını daha az etkili `SHRINKDATABASE` veya `SHRINKFILE`.  Otomatik küçültme birçok veritabanları ile elastik havuzları yönetme için özellikle yararlı olabilir.  Ancak, otomatik küçültme dosya alanı tekrar kullanılabilir hale getirme daha az etkili olabilir `SHRINKDATABASE` ve `SHRINKFILE`.
Otomatik küçültme etkinleştirmek için aşağıdaki komutta veritabanı adını değiştirin.


```sql
-- Enable auto-shrink for the database.
ALTER DATABASE [db1] SET AUTO_SHRINK ON
```

Bu komut hakkında daha fazla bilgi için bkz. [veritabanı](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current) seçenekleri. 

### <a name="rebuild-indexes"></a>Dizinleri yeniden oluştur

Veritabanı veri dosyaları küçültülebilir sonra dizinleri parçalanmış ve performans iyileştirme verimliliğine kaybedersiniz. Performans düşüşü ortaya çıkarsa, veritabanı dizinlerini yeniden oluşturma düşünün. Parçalanma ve dizinlerini yeniden oluşturma hakkında daha fazla bilgi için bkz. [Reorganıze ve dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).

## <a name="next-steps"></a>Sonraki adımlar

- En büyük veritabanı boyutları hakkında daha fazla bilgi için bkz:
  - [Azure SQL veritabanı sanal çekirdek tabanlı model sınırları tek bir veritabanı için satın alma](sql-database-vcore-resource-limits-single-databases.md)
  - [DTU tabanlı satın alma modeli kullanarak tek veritabanı kaynak sınırları](sql-database-dtu-resource-limits-single-databases.md)
  - [Azure SQL veritabanı sanal çekirdek tabanlı model sınırları elastik havuzlar için satın alma](sql-database-vcore-resource-limits-elastic-pools.md)
  - [DTU tabanlı satın alma modeli kullanarak elastik havuzlar için kaynak sınırları](sql-database-dtu-resource-limits-elastic-pools.md)
- Hakkında daha fazla bilgi için `SHRINKDATABASE` komutu, bkz: [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql). 
- Parçalanma ve dizinlerini yeniden oluşturma hakkında daha fazla bilgi için bkz. [Reorganıze ve dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).
