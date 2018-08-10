---
title: Oluşturma, Azure SQL sunucuları ve tek veritabanlarını yönetmek | Microsoft Docs
description: Oluşturma ve mantıksal sunucuları ve tek veritabanlarını yönetme hakkında bilgi edinin.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.subservice: single-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: carlrab
ms.openlocfilehash: a94c3a4c4b8ffb22b1d75ca064bd3e48a2e50141
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005687"
---
# <a name="create-and-manage-logical-servers-and-single-databases-in-azure-sql-database"></a>Mantıksal sunucuları ve Azure SQL veritabanı'nda tek veritabanları oluşturma ve yönetme 

Oluşturun ve Azure SQL veritabanı mantıksal sunucuları ve Azure portalı, PowerShell, Azure CLI, REST API ve Transact-SQL kullanarak tek veritabanlarını yönetmek.

## <a name="azure-portal-manage-logical-servers-and-databases"></a>Azure portalı: mantıksal sunucuları ve veritabanlarını yönetme

Azure SQL veritabanı'nın kaynak grubu önceden veya sunucunun kendisini oluştururken oluşturabilirsiniz. Yeni SQL server forma yeni bir SQL server oluşturma veya yeni bir veritabanı oluşturma işleminin parçası olarak almak için birden fazla yöntem vardır. 

### <a name="create-a-blank-sql-server-logical-server"></a>Boş bir SQL server (mantıksal sunucu) oluşturma

Sunucu (olmadan, bir veritabanı) kullanarak bir Azure SQL veritabanı oluşturmak için [Azure portalında](https://portal.azure.com), boş bir SQL server (mantıksal sunucu) formunu için gidin.  

### <a name="create-a-blank-or-sample-sql-database"></a>Boş veya örnek SQL veritabanı oluşturma

Kullanarak bir Azure SQL veritabanı oluşturmak için [Azure portalında](https://portal.azure.com)için boş bir SQL veritabanı formunu gidin ve istenen bilgileri sağlayın. Azure SQL veritabanı'nın kaynak grubu ve mantıksal sunucuya önceden veya veritabanı oluşturulurken oluşturabilirsiniz. Boş bir veritabanı oluşturun veya Adventure Works LT. üzerinde bağlı örnek bir veritabanı oluşturun 

  ![create database-1](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> Veritabanınıza ait fiyatlandırma katmanını seçme hakkında daha fazla bilgi için bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

Bir yönetilen örnek oluşturmak için bkz [bir yönetilen örnek oluşturma](sql-database-managed-instance-create-tutorial-portal.md)

### <a name="manage-an-existing-sql-server"></a>Mevcut bir SQL server'ı yönetme

Mevcut bir sunucuyu yönetmek için bir dizi yöntem - gibi belirli SQL veritabanı sayfasında, uğradıysa kullanarak sunucuya gidin **SQL sunucuları** sayfasında veya **tüm kaynakları** sayfası. 

Varolan bir veritabanını yönetmek için gidin **SQL veritabanları** sayfasında ve yönetmek istediğiniz veritabanına tıklayın. Aşağıdaki ekran görüntüsünde, bir veritabanından için sunucu düzeyinde güvenlik duvarı ayarını başlamak gösterilmektedir **genel bakış** bir veritabanı için sayfa. 

   ![sunucu güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> Bir veritabanı için performans özelliklerini yapılandırmak için bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).
>

> [!TIP]
> Bir Azure portalı Hızlı Başlangıç için bkz: [Azure portalında bir Azure SQL veritabanı oluşturma](sql-database-get-started-portal.md).

## <a name="powershell-manage-logical-servers-and-databases"></a>PowerShell: mantıksal sunucuları ve veritabanlarını yönetme

Azure SQL server, veritabanları ve Azure PowerShell ile güvenlik duvarları oluşturmak ve yönetmek için aşağıdaki PowerShell cmdlet'lerini kullanın. Gerekirse yükleyin veya PowerShell yükseltmek için bkz [Azure PowerShell modülü yükleme](/powershell/azure/install-azurerm-ps). 

> [!TIP]
> PowerShell için hızlı başlangıç için bkz: [PowerShell kullanarak tek bir Azure SQL veritabanı oluşturma](sql-database-get-started-portal.md). PowerShell örnek komut dosyaları için bkz: [tek bir Azure SQL veritabanı oluşturma ve bir güvenlik duvarı kuralı yapılandırmak için PowerShell kullanma](scripts/sql-database-create-and-configure-database-powershell.md) ve [İzleyici ve ölçek tek bir SQL veritabanını PowerShell kullanarak](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

| Cmdlet | Açıklama |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Bir veritabanı oluşturur |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Bir veya daha fazla veritabanını alır|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Bir veritabanı özelliklerini ayarlar veya varolan bir veritabanını esnek havuza taşır.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Bir veritabanı kaldırır|
|[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Bir kaynak grubu oluşturur|
|[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Bir sunucu oluşturur|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Sunucuları hakkında bilgi döndürür|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlserver)|Bir sunucu özelliklerini değiştirir|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Bir sunucuyu kaldırır|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Bir sunucu düzeyinde güvenlik duvarı kuralı oluşturulmaktadır. |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Bir sunucu için güvenlik duvarı kuralları alır|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Sunucu bir güvenlik duvarı kuralı değiştirir|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Bir güvenlik duvarı kuralı, bir sunucudan siler.|
| New-AzureRmSqlServerVirtualNetworkRule | Oluşturur bir [ *sanal ağ kuralı*](sql-database-vnet-service-endpoint-rule-overview.md)bağlı olarak bir sanal ağ hizmet uç noktası olan bir alt ağ. |

## <a name="azure-cli-manage-logical-servers-and-databases"></a>Azure CLI: mantıksal sunucuları ve veritabanlarını yönetme

Azure SQL server, veritabanlarını ve güvenlik duvarlarıyla oluşturmak ve yönetmek için [Azure CLI](/cli/azure), aşağıdaki [Azure CLI SQL veritabanı](/cli/azure/sql/db) komutları. CLI’yi tarayıcınızda çalıştırmak için [Cloud Shell](/azure/cloud-shell/overview) kullanın veya macOS, Linux ya da Windows’da [yükleyin](/cli/azure/install-azure-cli). Oluşturma ve elastik havuzları yönetme için bkz: [elastik havuzlar](sql-database-elastic-pool.md).

> [!TIP]
> Azure CLI Hızlı Başlangıç için bkz. [Azure CLI kullanarak tek bir Azure SQL veritabanı oluşturma](sql-database-get-started-cli.md). Azure CLI örnek betikler için bkz: [kullanımı tek bir Azure SQL veritabanı oluşturma ve bir güvenlik duvarı kuralı yapılandırmak için CLI](scripts/sql-database-create-and-configure-database-cli.md) ve [kullanımı tek bir SQL veritabanını izleme ve ölçeklendirme için CLI](scripts/sql-database-monitor-and-scale-database-cli.md).
>

| Cmdlet | Açıklama |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#az_sql_db_create) |Bir veritabanı oluşturur|
|[az sql db listesi](/cli/azure/sql/db#az_sql_db_list)|Tüm veritabanları ve veri ambarında bir sunucu veya elastik bir havuzdaki tüm veritabanları listeler|
|[az sql db sürümleri Listele](/cli/azure/sql/db#az_sql_db_list_editions)|Kullanılabilir hizmet amaçlarını listeler ve depolama sınırları|
|[az sql db kullanımları-Listele](/cli/azure/sql/db#az_sql_db_list_usages)|Kullanımları döndürür veritabanı|
|[az sql db show](/cli/azure/sql/db#az_sql_db_show)|Bir veritabanını veya veri ambarını alır|
|[az sql db update](/cli/azure/sql/db#az_sql_db_update)|Bir veritabanını güncelleştirir|
|[az sql db delete](/cli/azure/sql/db#az_sql_db_delete)|Bir veritabanı kaldırır|
|[az group create](/cli/azure/group#az_group_create)|Bir kaynak grubu oluşturur|
|[az sql server create](/cli/azure/sql/server#az_sql_server_create)|Bir sunucu oluşturur|
|[az sql server listesi](/cli/azure/sql/server#az_sql_server_list)|Sunucuları listeler|
|[az sql server kullanımları-Listele](/cli/azure/sql/server#az_sql_server_list_usages)|Sunucu kullanımları döndürür|
|[az sql server show](/cli/azure/sql/server#az_sql_server_show)|Bir sunucu alır|
|[az sql server güncelleştirmesi](/cli/azure/sql/server#az_sql_server_update)|Bir sunucu güncelleştirir|
|[az sql server delete](/cli/azure/sql/server#az_sql_server_delete)|Bir sunucu siler|
|[az sql server güvenlik duvarı kuralı oluşturma](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Sunucu güvenlik duvarı kuralı oluşturur.|
|[az sql server güvenlik duvarı kuralı listesi](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Bir sunucudaki güvenlik duvarı kurallarını listeler|
|[az sql server güvenlik duvarı-rule show](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Bir güvenlik duvarı kuralı ayrıntılarını gösterir|
|[az sql server güvenlik duvarı kuralı güncelleştirme](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Bir güvenlik duvarı kuralını güncelleştirir|
|[az sql server güvenlik duvarı kuralını Sil](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Bir güvenlik duvarı kuralını siler|

## <a name="transact-sql-manage-logical-servers-and-databases"></a>Transact-SQL: mantıksal sunucuları ve veritabanlarını yönetme

Azure SQL server, veritabanları ve Transact-SQL ile güvenlik duvarları oluşturmak ve yönetmek için aşağıdaki T-SQL komutlarını kullanın. Azure portalını kullanarak şu komutları verebilirsiniz [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), veya bir Azure SQL Database sunucusuna bağlanma ve Transact-SQL geçirmek başka bir programı komutları. Elastik havuzlar için bkz. [elastik havuzlar](sql-database-elastic-pool.md).


> [!TIP]
> Microsoft Windows üzerinde SQL Server Management Studio'yu kullanarak bir hızlı başlangıç için bkz: [Azure SQL veritabanı: bağlanmak ve veri sorgulamak için kullanım SQL Server Management Studio](sql-database-connect-query-ssms.md). MacOS, Linux veya Windows üzerinde Visual Studio Code'u kullanarak bir hızlı başlangıç için bkz: [Azure SQL veritabanı: kullanım Visual Studio Code bağlanmak ve veri sorgulamak için](sql-database-connect-query-vscode.md).

> [!IMPORTANT]
> Oluşturamaz veya Transact-SQL kullanarak bir sunucuyu silin.
>

| Komut | Açıklama |
| --- | --- |
|[Veritabanı (Azure SQL veritabanı) oluşturma](/sql/t-sql/statements/create-database-azure-sql-database)|Yeni bir veritabanı oluşturur. Yeni bir veritabanı oluşturmak için ana veritabanına bağlı olmanız gerekir.|
| [ALTER DATABASE (Azure SQL veritabanı)](/sql/t-sql/statements/alter-database-azure-sql-database) |Azure SQL veritabanını değiştirir. |
|[Veritabanı (Azure SQL veri ambarı) değiştirme](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Bir Azure SQL veri ambarı değiştirir.|
|[Veritabanı (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Bir veritabanını siler.|
|[sys.database_service_objectives (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Edition (hizmet katmanı), hizmet hedefi (fiyatlandırma katmanı) ve elastik havuz adı varsa, Azure SQL veritabanına veya bir Azure SQL veri ambarı için döndürür. Azure SQL veritabanı sunucusu ana veritabanında oturum açtıysanız, tüm veritabanlarında bilgileri döndürür. Azure SQL veri ambarı için ana veritabanına bağlı olmanız gerekir.|
|[sys.dm_db_resource_stats (Azure SQL veritabanı)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Bir Azure SQL veritabanı için CPU, GÇ ve bellek tüketimi döndürür. Veritabanında hiç etkinlik olsa her 15 saniyede bir satır yok.|
|[sys.resource_stats (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|Azure SQL veritabanı için CPU kullanımı ve depolama verilerini döndürür. Veriler toplanır ve beş dakikalık aralıklarla içinde toplanır.|
|[sys.database_connection_stats (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|SQL veritabanı veritabanı bağlantı olayları, veritabanı bağlantı başarı ve başarısızlık genel bir bakış sağlayarak istatistiklerini içerir. |
|[sys.event_log (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Başarılı Azure SQL veritabanı, veritabanı bağlantıları, bağlantı hataları ve kilitlenmeleri döndürür. Veritabanı etkinliğinizi SQL veritabanı ile ilgili sorunları giderme veya izlemek için bu bilgileri kullanabilirsiniz.|
|[sp_set_firewall_rule (Azure SQL veritabanı)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Oluşturur veya SQL veritabanı sunucunuz için sunucu düzeyinde güvenlik duvarı ayarlarını güncelleştirir. Bu saklı yordam yalnızca ana veritabanında sunucu düzeyi asıl oturum açma için kullanılabilir. Sunucu düzeyinde güvenlik duvarı kuralı yalnızca Azure düzeyi izinlere sahip bir kullanıcı tarafından ilk sunucu düzeyinde güvenlik duvarı kural oluşturulduktan sonra Transact-SQL kullanılarak oluşturulabilir.|
|[sys.firewall_rules (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Microsoft Azure SQL veritabanı ile ilişkili sunucu düzeyinde güvenlik duvarı ayarları hakkında bilgi döndürür.|
|[sp_delete_firewall_rule (Azure SQL veritabanı)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Sunucu düzeyinde güvenlik duvarı ayarları, SQL veritabanı sunucusundan kaldırır. Bu saklı yordam yalnızca ana veritabanında sunucu düzeyi asıl oturum açma için kullanılabilir.|
|[sp_set_database_firewall_rule (Azure SQL veritabanı)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Oluşturur veya Azure SQL veritabanınız veya SQL veri ambarı için veritabanı düzeyinde güvenlik duvarı kurallarını güncelleştirir. SQL veritabanı kullanıcı veritabanlarında ve ana veritabanı için veritabanı güvenlik duvarı kuralları yapılandırılabilir. Veritabanı güvenlik duvarı kuralları kullanarak, bağımsız veritabanı kullanıcıları için yararlıdır. |
|[sys.database_firewall_rules (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Microsoft Azure SQL veritabanı ile ilişkili veritabanı düzeyinde güvenlik duvarı ayarları hakkında bilgi döndürür. |
|[sp_delete_database_firewall_rule (Azure SQL veritabanı)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Veritabanı düzeyinde güvenlik duvarı ayarı, Azure SQL veritabanı veya SQL veri ambarı kaldırır. |



## <a name="rest-api-manage-logical-servers-and-databases"></a>REST API: mantıksal sunucuları ve veritabanlarını yönetme

Azure SQL server, veritabanlarını ve güvenlik duvarları oluşturmak ve yönetmek için bu REST API istekleri'ni kullanın.

| Komut | Açıklama |
| --- | --- |
|[Sunucuları - oluştur veya güncelleştir](/rest/api/sql/servers/createorupdate)|Oluşturur veya yeni bir sunucu güncelleştirir.|
|[Sunucuları - Sil](/rest/api/sql/servers/delete)|Bir SQL server siler.|
|[Sunucuları - Get](/rest/api/sql/servers/get)|Bir sunucu alır.|
|[Sunucuları - liste](/rest/api/sql/servers/list)|Sunucularının bir listesini döndürür.|
|[Sunucuları - kaynak grubuna göre listesi](/rest/api/sql/servers/listbyresourcegroup)|Bir kaynak grubunda sunucularının bir listesini döndürür.|
|[Sunucuları - güncelleştirme](/rest/api/sql/servers/update)|Mevcut bir sunucu güncelleştirir.|
|[Veritabanları - oluştur veya güncelleştir](/rest/api/sql/databases/createorupdate)|Yeni bir veritabanı oluşturur veya mevcut bir veritabanını güncelleştirir.|
|[Veritabanları - Get](/rest/api/sql/databases/get)|Bir veritabanını alır.|
|[Veritabanları - Get ile elastik havuz](/rest/api/sql/databases/getbyelasticpool)|Bir veritabanını bir elastik havuzun içine alır.|
|[Veritabanları - önerilen elastik havuzu tarafından alınamadı](/rest/api/sql/databases/getbyrecommendedelasticpool)|Bir veritabanı içinde recommented bir elastik havuz alır.|
|[Veritabanı - elastik havuz göre listesi](/rest/api/sql/databases/listbyelasticpool)|Elastik havuzdaki veritabanlarının listesini döndürür.|
|[Veritabanları - önerilen elastik havuz göre listesi](/rest/api/sql/databases/listbyrecommendedelasticpool)|Önerilen bir elastik havuz içindeki veritabanları listesi döndürür.|
|[Veritabanı - sunucu listesi](/rest/api/sql/databases/listbyserver)|Bir sunucu veritabanlarının listesini döndürür.|
|[Veritabanları - güncelleştirme](/rest/api/sql/databases/update)|Varolan bir veritabanını güncelleştirir.|
|[Güvenlik duvarı kuralları - oluştur veya güncelleştir](/rest/api/sql/firewallrules/createorupdate)|Oluşturur veya bir güvenlik duvarı kuralını güncelleştirir.|
|[Güvenlik duvarı kuralları - Sil](/rest/api/sql/firewallrules/delete)|Bir güvenlik duvarı kuralını siler.|
|[Güvenlik duvarı kuralları - Get](/rest/api/sql/firewallrules/get)|Bir güvenlik duvarı kuralını alır.|
|[Güvenlik duvarı kuralları - sunucu listesi](/rest/api/sql/firewallrules/listbyserver)|Güvenlik duvarı kurallarının bir listesini döndürür.|

## <a name="next-steps"></a>Sonraki adımlar

- Bir SQL Server veritabanını Azure'a geçirme hakkında bilgi edinmek için [Azure SQL veritabanına geçirme](sql-database-cloud-migrate.md).
- Desteklenen özellikler hakkında bilgi edinmek için bkz. [Özellikler](sql-database-features.md).
