---
title: "Azure SQL veritabanı tek veritabanı | Microsoft Docs"
description: "Hizmet katmanı, performans düzeyi ve tek bir Azure SQL veritabanı için storagea miktarı yönetin."
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 02/12/2018
ms.author: carlrab
ms.openlocfilehash: 167a72ae55052b8ac1dfe8f032f136a9bf8bcedf
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="manage-resources-for-a-single-database-in-azure-sql-database"></a>Azure SQL Database tek bir veritabanı için kaynakları yönetme

Tek bir veritabanı ile veritabanı hizmet katmanı, performans düzeyinde ve gerektirdiği depolama miktarını iş yükünü işlemek için gereken kaynakları miktarını belirler. 

## <a name="manage-single-database-resources-using-the-azure-portal"></a>Azure Portalı'nı kullanarak tek veritabanı kaynaklarını yönetme

Ayarlayın veya hizmet katmanı, performans düzeyi ya da Azure Portalı'nı kullanarak yeni veya mevcut bir Azure SQL veritabanı için depolama miktarını değiştirmek için açın **performansını yapılandırın** tıklatarak veritabanınız için pencere **fiyatlandırma Katmanı ( Dtu'lar ölçeklendirme)** - aşağıdaki ekran görüntüsünde gösterildiği gibi. 

- Ayarlayabilir veya iş yükü hizmet katmanı seçerek Hizmet katmanını değiştirebilirsiniz. 
- Ayarlama veya performans düzeyini değiştirme (**Dtu'lar**) kullanarak bir hizmet katmanı içinde **DTU** kaydırıcı.
- Ayarlama veya değiştirme performans düzeyi kullanan bir depolama miktarına **depolama** kaydırıcı. 

![Hizmet katmanını ve performans düzeyini yapılandırın](./media/sql-database-single-database-resources/change-service-tier.png)

Tıklatın **genel bakış** izlemek ve/veya devam eden işlemi iptal edin.

![İşlemi iptal edin](./media/sql-database-single-database-resources/cancel-operation.png)

> [!IMPORTANT]
> Gözden geçirme [P11 ve P15 veritabanları ile en fazla 4 TB boyut, geçerli sınırlamalar](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb) P11 veya P15 bir hizmet katmanı seçerken.
>

## <a name="manage-single-database-resources-using-powershell"></a>PowerShell kullanarak tek veritabanı kaynaklarını yönetme

Azure SQL veritabanı hizmet katmanları, performans düzeyleri ve PowerShell kullanarak depolama miktarını ayarlama veya değiştirme için şu PowerShell cmdlet'lerini kullanın. Gerekirse yükleyin veya PowerShell yükseltme, bakın [yükleme Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps). 

| Cmdlet | Açıklama |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Bir veritabanı oluşturur |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Bir veya daha fazla veritabanı alır|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Bir veritabanı özelliklerini ayarlar veya varolan bir veritabanını bir esnek havuza taşır. Örneğin, **MaxSizeBytes** bir veritabanı boyutu üst sınırı ayarlamak için özellik.|
|[Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity)|Veritabanı işlemleri durumunu alır. |
|[Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity)|Veritabanında zaman uyumsuz güncelleştirme işlemi iptal eder.|


> [!TIP]
> Bir veritabanı performans ölçümlerini izler bir PowerShell örnek betiği daha yüksek bir performans düzeyine ölçeklendirir ve performans ölçümleri birinde bir uyarı kuralı oluşturur bkz [İzleyici ve ölçek tek bir SQL veritabanı PowerShellkullanma](scripts/sql-database-monitor-and-scale-database-powershell.md).

## <a name="manage-single-database-resources-using-the-azure-cli"></a>Tek veritabanı kaynakları Azure CLI kullanarak yönetme

Ayarlamak veya Azure SQL veritabanlarını değiştirmek için hizmet katmanları, performans düzeyleri ve depolama tutarı Azure CLI kullanarak bunları kullanın [Azure CLI SQL veritabanı](/cli/azure/sql/db) komutları. CLI’yi tarayıcınızda çalıştırmak için [Cloud Shell](/azure/cloud-shell/overview) kullanın veya macOS, Linux ya da Windows’da [yükleyin](/cli/azure/install-azure-cli). Oluşturma ve SQL esnek havuzlarını yönetmek için bkz: [esnek havuzlar](sql-database-elastic-pool.md).

| Cmdlet | Açıklama |
| --- | --- |
|[az sql server güvenlik duvarı kuralı oluşturma](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Sunucu güvenlik duvarı kuralı oluşturur|
|[az sql server güvenlik duvarı kuralı listesi](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Bir sunucudaki güvenlik duvarı kurallarını listeler|
|[az sql server güvenlik duvarı kuralı Göster](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Bir güvenlik duvarı kuralı ayrıntılarını gösterir|
|[az sql server güvenlik duvarı kuralı güncelleştirme](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Bir güvenlik duvarı kuralını güncelleştirir|
|[az sql server güvenlik duvarı kuralını Sil](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Bir güvenlik duvarı kuralını siler|
|[az sql db op listesi](/cli/azure/sql/db/op?#az_sql_db_op_list)|Veritabanı üzerinde gerçekleştirilen işlemler listesini alır.|
|[az sql db op iptal](/cli/azure/sql/db/op#az_sql_db_op_cancel)|Veritabanında zaman uyumsuz işlemi iptal eder.|

> [!TIP]
> Veritabanının boyutu bilgileri sorgulama sonra farklı performans düzeyi tek bir Azure SQL veritabanına ölçeklendirilebilen bir Azure CLI örnek komut dosyası için bkz: [kullanımı izlemek ve tek bir SQL veritabanı ölçeklendirmek için CLI](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-single-database-resources-using-transact-sql"></a>Transact-SQL kullanarak tek veritabanı kaynaklarını yönetme

Azure SQL veritabanı hizmet katmanları, performans düzeyleri ve Transact-SQL ile depolama miktarını ayarlama veya değiştirme için bu T-SQL komutlarını kullanın. Azure portalını kullanarak bu komutlar gönderebilirsiniz [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), veya bir Azure SQL veritabanı sunucusuna bağlanın ve Transact-SQL geçirmek başka bir programı komutları. 

| Komut | Açıklama |
| --- | --- |
|[Veritabanı (Azure SQL veritabanı) oluşturma](/sql/t-sql/statements/create-database-azure-sql-database)|Yeni bir veritabanı oluşturur. Yeni bir veritabanı oluşturmak için ana veritabanına bağlanması gerekir.|
| [ALTER DATABASE (Azure SQL veritabanı)](/sql/t-sql/statements/alter-database-azure-sql-database) |Bir Azure SQL veritabanı değiştirir. |
|[sys.database_service_objectives (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Edition (hizmet katmanı), hizmet hedefi (fiyatlandırma katmanı) ve esnek havuz adı, varsa Azure SQL veritabanına veya Azure SQL Data Warehouse için döndürür. Azure SQL Database sunucusu ana veritabanında oturum açtıysanız, bilgiler tüm veritabanlarını döndürür. Azure SQL Data Warehouse için ana veritabanına bağlı olmalıdır.|
|[sys.database_usage (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-usage-azure-sql-database)|Sayısı, türü ve süresi bir Azure SQL veritabanı sunucusu üzerindeki veritabanlarının listeler.|

Aşağıdaki örnek, bir veritabanı boyutu üst sınırı gösterir ALTER DATABASE komutunu kullanarak değiştiriliyor:

 ```sql
ALTER DATABASE <myDatabaseName> 
   MODIFY (MAXSIZE = 4096 GB);
```

## <a name="manage-single-database-resources-using-the-rest-api"></a>REST API kullanarak tek veritabanı kaynaklarını yönetme

Ayarlama veya değiştirme Azure SQL veritabanları için REST API istekleri hizmet katmanları ve performans düzeyleri depolama miktarına kullanın.

| Komut | Açıklama |
| --- | --- |
|[Veritabanları - oluştur veya güncelleştir](/rest/api/sql/databases/createorupdate)|Yeni bir veritabanı oluşturur veya varolan bir veritabanını güncelleştirir.|
|[Veritabanları - Al](/rest/api/sql/databases/get)|Bir veritabanı alır.|
|[Veritabanı - esnek havuz tarafından Al](/rest/api/sql/databases/getbyelasticpool)|Bir veritabanını bir esnek havuz içinde alır.|
|[Önerilen esnek havuzu tarafından veritabanları - Al](/rest/api/sql/databases/getbyrecommendedelasticpool)|Bir veritabanı içinde recommented bir esnek havuz alır.|
|[Veritabanı - esnek havuz göre listesi](/rest/api/sql/databases/listbyelasticpool)|Bir esnek havuz veritabanlarının bir listesini döndürür.|
|[Veritabanları - önerilen esnek havuz göre listesi](/rest/api/sql/databases/listbyrecommendedelasticpool)|Recommented bir esnek havuz içindeki veritabanlarının bir listesini döndürür.|
|[Veritabanları - sunucu tarafından listesi](/rest/api/sql/databases/listbyserver)|Bir sunucu veritabanlarının bir listesini döndürür.|
|[Veritabanları - güncelleştirme](/rest/api/sql/databases/update)|Varolan bir veritabanını güncelleştirir.|
|[İşlem - liste](/rest/api/sql/Operations/List)|Tüm kullanılabilir SQL Rest API işlemleri listeler.|



## <a name="next-steps"></a>Sonraki adımlar

- Hizmet katmanları ve performans düzeyleri depolama tutarlar, bkz: hakkında bilgi edinin [hizmet katmanları](sql-database-service-tiers.md).
- Esnek havuzları hakkında bilgi edinmek için bkz: [esnek havuzlar](sql-database-elastic-pool.md).
- Hakkında bilgi edinin [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md)
