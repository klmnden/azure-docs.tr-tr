---
title: Oluşturma ve esnek havuzlar - Azure SQL veritabanını yönetme | Microsoft Docs
description: Oluşturun ve Azure SQL esnek havuzlarını yönetme.
keywords: birden çok veritabanı, veritabanı kaynakları, veritabanı performansı
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.date: 06/20/2018
ms.author: ninarn
ms.topic: conceptual
ms.reviewer: carlrab
ms.openlocfilehash: 3cdc82d71c05298aa5822b87c22edcc5d8e73385
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313328"
---
# <a name="create-and-manage-elastic-pools-in-azure-sql-database"></a>Oluşturma ve Azure SQL Database esnek havuzlarını yönetme

Sahip bir esnek havuz, esnek havuz veritabanlarını iş yükünü işlemek için gerekli kaynakları miktarını ve havuza alınmış her veritabanı için kaynak miktarını belirler. 

## <a name="azure-portal-manage-elastic-pools-and-pooled-databases"></a>Azure portal: esnek havuzlar ve havuza alınmış veritabanlarını yönetme

Tüm havuzu ayarları tek bir yerde bulunabilir: **havuzu yapılandırma** dikey. Burada almak için bir esnek havuz portal ve tıklatın Bul **havuzu yapılandırma** dikey pencerenin üst veya sol kaynak menüsünden.

Buradan tüm bir toplu işlemde herhangi bir birleşimini kaydetmek ve aşağıdaki değişiklikleri yapabilirsiniz:
1. Havuz Hizmet katmanını değiştirme
2. Performans (DTU veya vCores) ve depolama yukarı veya aşağı ölçeklendirme
3. Eklemek veya kaldırmak için/havuzundan veritabanları
4. (Garanti) min ayarlamak ve performans sınırı havuzları veritabanları için en fazla
5. Faturanızı yeni seçimlerinizi sonucunda herhangi bir değişiklik görüntülemeyi maliyet özeti gözden geçirin

![Esnek havuzu yapılandırma dikey penceresi](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

## <a name="powershell-manage-elastic-pools-and-pooled-databases"></a>PowerShell: esnek havuzlar ve havuza alınmış veritabanlarını yönetme 

SQL Database esnek havuzlar ve Azure PowerShell ile havuza alınmış veritabanları oluşturmak ve yönetmek için aşağıdaki PowerShell cmdlet'lerini kullanın. Gerekirse yükleyin veya PowerShell yükseltme, bakın [yükleme Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps). Veritabanları, sunucuları ve güvenlik duvarı kuralları oluşturmak ve yönetmek için bkz: [oluşturma ve Azure SQL veritabanı sunucularının ve PowerShell kullanarak veritabanlarını](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell).

> [!TIP]
> PowerShell örnek komut dosyaları için bkz: [esnek havuzlar oluşturmak ve PowerShell kullanarak havuz dışında havuzları arasında veritabanlarını taşımak](scripts/sql-database-move-database-between-pools-powershell.md) ve [kullanımı izlemek ve Azure SQL veritabanıSQLesnekhavuzdaölçeklendirmeiçinPowerShell](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Cmdlet | Açıklama |
| --- | --- |
|[New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Esnek veritabanı havuzu bir mantıksal SQL sunucusu üzerinde oluşturur.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Mantıksal bir SQL Server'da esnek havuzlar ve özellik değerlerini alır.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Esnek veritabanı havuzu mantıksal SQL Server'da özelliklerini değiştirir. Örneğin, **StorageMB** bir esnek havuzun en fazla depolama değiştirmek için özellik.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Esnek veritabanı havuzu mantıksal SQL Server'da siler.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Mantıksal SQL Server'da bir esnek havuz işlemlerinin durumunu alır.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Yeni bir veritabanı var olan bir havuzu veya tek bir veritabanı oluşturur. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Bir veya daha fazla veritabanını alır.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Bir veritabanı özelliklerini ayarlar ya da var olan bir veritabanı içine, dışı veya esnek havuzlar arasında taşır.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Bir veritabanı kaldırır.|


> [!TIP]
> Esnek havuzdaki birçok veritabanı oluşturulmasını portalı veya aynı anda yalnızca tek bir veritabanı oluşturabilirsiniz PowerShell cmdlet'lerini kullanarak tamamlanınca zaman alabilir. İçine bir esnek havuz oluşturmayı otomatikleştirmek için bkz: [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="azure-cli-manage-elastic-pools-and-pooled-databases"></a>Azure CLI: esnek havuzlar ve havuza alınmış veritabanlarını yönetme

Oluşturun ve SQL Database esnek havuzları ile yönetmek için [Azure CLI](/cli/azure), aşağıdaki [Azure CLI SQL veritabanı](/cli/azure/sql/db) komutları. CLI’yi tarayıcınızda çalıştırmak için [Cloud Shell](/azure/cloud-shell/overview) kullanın veya macOS, Linux ya da Windows’da [yükleyin](/cli/azure/install-azure-cli).

> [!TIP]
> Azure CLI örnek komut dosyaları için bkz: [kullanım SQL esnek havuzu içinde bir Azure SQL veritabanını taşımak için CLI](scripts/sql-database-move-database-between-pools-cli.md) ve [Azure SQL veritabanındaki bir SQL esnek havuzu ölçeklendirmek için kullanım Azure CLI](scripts/sql-database-scale-pool-cli.md).
>

| Cmdlet | Açıklama |
| --- | --- |
|[az sql esnek havuzu oluşturma](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create)|Bir esnek havuz oluşturur.|
|[az sql esnek havuzu listesi](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list)|Bir sunucu esnek havuzlar listesini döndürür.|
|[az sql esnek havuzu listesi-dbs](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_dbs)|Bir esnek havuz veritabanlarının bir listesini döndürür.|
|[az sql esnek havuzu listesi-sürümleri](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_editions)|Ayrıca, depolama sınırları, kullanılabilir havuz DTU ayarlarını bulundurur ve veritabanı ayarlarını başına. Ayrıntı, ek depolama sınırları azaltmak için ve veritabanı başına ayarlar varsayılan olarak gizlidir.|
|[az sql esnek havuzu güncelleştirme](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update)|Bir esnek havuz güncelleştirir.|
|[az sql esnek havuzu silme](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_delete)|Esnek havuz siler.|

## <a name="transact-sql-manage-pooled-databases"></a>Transact-SQL: havuza alınmış veritabanlarını yönetme

Oluşturma ve içinde var olan esnek havuzlar veritabanlarını taşımak veya Transact-SQL ile bir SQL Database esnek havuzunu hakkında bilgi döndürmek için aşağıdaki T-SQL komutlarını kullanın. Azure portalını kullanarak bu komutlar gönderebilirsiniz [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), veya bir Azure SQL veritabanı sunucusuna bağlanın ve Transact-SQL geçirmek başka bir programı komutları. Veritabanları, sunucuları ve güvenlik duvarı kuralları oluşturmak ve yönetmek için bkz: [oluşturma ve Azure SQL veritabanı sunucularının ve Transact-SQL kullanarak veritabanlarını](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-transact-sql).

> [!IMPORTANT]
> Oluşturmak, güncelleştirmek veya Transact-SQL kullanarak bir Azure SQL Database esnek havuzunu silmek olamaz. Ekleyebilir veya bir esnek havuzdan veritabanı kaldırma ve var olan esnek havuzları hakkında bilgi döndürmek için Dmv'leri kullanabilirsiniz.
>

| Komut | Açıklama |
| --- | --- |
|[Veritabanı (Azure SQL veritabanı) oluşturma](/sql/t-sql/statements/create-database-azure-sql-database)|Yeni bir veritabanı var olan bir havuzu veya tek bir veritabanı oluşturur. Yeni bir veritabanı oluşturmak için ana veritabanına bağlanması gerekir.|
| [ALTER DATABASE (Azure SQL veritabanı)](/sql/t-sql/statements/alter-database-azure-sql-database) |Bir veritabanı içine, dışı veya esnek havuzlar arasında taşıyın.|
|[VERİTABANINI (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Bir veritabanını siler.|
|[sys.elastic_pool_resource_stats (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Tüm esnek veritabanı havuzları için kaynak kullanım istatistikleri, bir mantıksal sunucu döndürür. Her esnek veritabanı havuzu için 15 penceresi (dakika başına dört satır) bildirdiği saniyede için bir satır yok. Bu CPU, IO, günlük, depolama alanı tüketimi ve eşzamanlı istek/oturum kullanımı havuzdaki tüm veritabanları tarafından içerir.|
|[sys.database_service_objectives (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Edition (hizmet katmanı), hizmet hedefi (fiyatlandırma katmanı) ve esnek havuz adı, varsa Azure SQL veritabanına veya Azure SQL Data Warehouse için döndürür. Azure SQL Database sunucusu ana veritabanında oturum açtıysanız, bilgiler tüm veritabanlarını döndürür. Azure SQL Data Warehouse için ana veritabanına bağlı olmalıdır.|

## <a name="rest-api-manage-elastic-pools-and-pooled-databases"></a>REST API: esnek havuzlar ve havuza alınmış veritabanlarını yönetme

SQL Database esnek havuzlar ve havuza alınmış veritabanları oluşturmak ve yönetmek için bu REST API istekleri kullanın.

| Komut | Açıklama |
| --- | --- |
|[Esnek havuzlar - oluştur veya güncelleştir](/rest/api/sql/elasticpools/createorupdate)|Yeni bir esnek havuz oluşturur veya mevcut bir esnek havuz güncelleştirir.|
|[Esnek havuzlar - Sil](/rest/api/sql/elasticpools/delete)|Esnek havuz siler.|
|[Esnek havuzlar - Al](/rest/api/sql/elasticpools/get)|Bir esnek havuz alır.|
|[Esnek havuzlar - sunucu tarafından listesi](/rest/api/sql/elasticpools/listbyserver)|Bir sunucu esnek havuzlar listesini döndürür.|
|[Esnek havuzlar - güncelleştirme](/rest/api/sql/elasticpools/update)|Var olan bir esnek havuzu güncelleştirir.|
|[Önerilen esnek havuzları - Al](/rest/api/sql/recommendedelasticpools/get)|Önerilen esnek havuz alır.|
|[Önerilen esnek havuzları - sunucu tarafından listesi](/rest/api/sql/recommendedelasticpools/listbyserver)|Önerilen esnek havuzları döndürür.|
|[Önerilen esnek havuzları - liste ölçümleri](/rest/api/sql/recommendedelasticpools/listmetrics)|Esnek havuz ölçümleri döndürür önerilir.|
|[Esnek havuz etkinlikleri](/rest/api/sql/elasticpoolactivities)|Esnek havuz etkinlikleri döndürür.|
|[Esnek havuz veritabanı etkinlikleri](/rest/api/sql/elasticpooldatabaseactivities)|Etkinlik bir esnek havuz içinde veritabanlarını döndürür.|
|[Veritabanları - oluştur veya güncelleştir](/rest/api/sql/databases/createorupdate)|Yeni bir veritabanı oluşturur veya varolan bir veritabanını güncelleştirir.|
|[Veritabanları - Al](/rest/api/sql/databases/get)|Bir veritabanı alır.|
|[Veritabanı - esnek havuz tarafından Al](/rest/api/sql/databases/getbyelasticpool)|Bir veritabanını bir esnek havuz içinde alır.|
|[Önerilen esnek havuzu tarafından veritabanları - Al](/rest/api/sql/databases/getbyrecommendedelasticpool)|Bir veritabanı içinde önerilen bir esnek havuz alır.|
|[Veritabanı - esnek havuz göre listesi](/rest/api/sql/databases/listbyelasticpool)|Bir esnek havuz veritabanlarının bir listesini döndürür.|
|[Veritabanları - önerilen esnek havuz göre listesi](/rest/api/sql/databases/listbyrecommendedelasticpool)|Önerilen esnek havuz içindeki veritabanlarının bir listesini döndürür.|
|[Veritabanları - sunucu tarafından listesi](/rest/api/sql/databases/listbyserver)|Bir sunucu veritabanlarının bir listesini döndürür.|
|[Veritabanları - güncelleştirme](/rest/api/sql/databases/update)|Varolan bir veritabanını güncelleştirir.|

## <a name="next-steps"></a>Sonraki adımlar

* Video için bkz: [Microsoft Virtual Academy video indirmelere Azure SQL Database esnek özellikleri hakkında](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* Esnek havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Database kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* Esnek havuzları kullanan SaaS öğretici için bkz: [Wingtip SaaS uygulamasına giriş](sql-database-wtp-overview.md).
