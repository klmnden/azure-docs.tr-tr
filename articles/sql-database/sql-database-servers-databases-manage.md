---
title: Oluşturma, Azure SQL sunucuları ve tek veritabanlarını yönetme | Microsoft Docs
description: Oluşturma ve mantıksal sunucu ve tek veritabanlarını yönetme hakkında bilgi edinin.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 918cfd0257c82e84451e07ef904dbda331f47b95
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313314"
---
# <a name="create-and-manage-logical-servers-and-single-databases-in-azure-sql-database"></a>Mantıksal sunucu ve tek veritabanlarında Azure SQL veritabanı oluşturma ve yönetme 

Oluşturun ve Azure SQL veritabanları mantıksal sunucu ve tek veritabanlarını Azure portal, PowerShell, Azure CLI, REST API ve Transact-SQL kullanarak yönetin.

## <a name="azure-portal-manage-logical-servers-and-databases"></a>Azure portal: mantıksal sunucular ve veritabanları yönetme

Azure SQL Database'in kaynak grubu vaktinden veya sunucu oluştururken oluşturabilirsiniz. İçin yeni bir SQL server form, yeni bir SQL server oluşturarak veya yeni bir veritabanı oluşturmak bir parçası olarak almak için birden çok yöntem bulunmaktadır. 

### <a name="create-a-blank-sql-server-logical-server"></a>Boş bir SQL server (mantıksal sunucu) oluşturun

Bir Azure SQL veritabanı sunucusu (olmadan bir veritabanı) kullanarak oluşturmak için [Azure portal](https://portal.azure.com), boş bir SQL server (mantıksal sunucu) form gidin.  

### <a name="create-a-blank-or-sample-sql-database"></a>Boş veya örnek bir SQL veritabanı oluşturma

Kullanarak bir Azure SQL veritabanı oluşturmak için [Azure portal](https://portal.azure.com), boş bir SQL veritabanı formu gidin ve istenen bilgileri sağlayın. Azure SQL Database'in kaynak grubu ve mantıksal sunucu vaktinden veya veritabanı oluşturulurken oluşturabilirsiniz. Boş bir veritabanı oluşturmayı veya Adventure Works LT. üzerinde temel bir örnek veritabanı oluşturma 

  ![create database-1](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> Veritabanınız için fiyatlandırma katmanı seçme konusunda daha fazla bilgi için bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [vCore tabanlı satın alma modeli (Önizleme)](sql-database-service-tiers-vcore.md).

Yönetilen bir örneğini oluşturmak için bkz: [yönetilen örneği oluşturma](sql-database-managed-instance-create-tutorial-portal.md)

### <a name="manage-an-existing-sql-server"></a>Mevcut bir SQL server'ı yönetme

Var olan bir sunucuyu yönetmek için çeşitli yöntemler - gibi belirli SQL veritabanı sayfası uğradıysa kullanarak sunucuya gidin **SQL sunucuları** sayfasında veya **tüm kaynakları** sayfası. 

Varolan bir veritabanını yönetmek için gidin **SQL veritabanları** sayfasında ve yönetmek istediğiniz veritabanını tıklayın. Aşağıdaki ekran görüntüsü, bir veritabanından için sunucu düzeyinde güvenlik duvarı ayarı başlamak gösterilmiştir **genel bakış** bir veritabanı için sayfası. 

   ![sunucu güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> Bir veritabanı performans özelliklerini yapılandırmak için bkz: [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [vCore tabanlı satın alma modeli (Önizleme)](sql-database-service-tiers-vcore.md).
>

> [!TIP]
> Bir Azure portalı Hızlı Başlangıç için bkz: [Azure portalında bir Azure SQL veritabanı oluşturma](sql-database-get-started-portal.md).

## <a name="powershell-manage-logical-servers-and-databases"></a>PowerShell: mantıksal sunucular ve veritabanları yönetme

Azure SQL server, veritabanları ve güvenlik duvarları Azure PowerShell ile oluşturmak ve yönetmek için aşağıdaki PowerShell cmdlet'lerini kullanın. Gerekirse yükleyin veya PowerShell yükseltme, bakın [yükleme Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps). 

> [!TIP]
> PowerShell Hızlı Başlangıç için bkz: [PowerShell kullanarak tek bir Azure SQL veritabanı oluşturma](sql-database-get-started-portal.md). PowerShell örnek komut dosyaları için bkz: [kullanım tek bir Azure SQL veritabanı oluşturmak ve bir güvenlik duvarı kuralı yapılandırmak için PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) ve [İzleyici ve ölçek tek bir SQL veritabanı PowerShell kullanarak](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

| Cmdlet | Açıklama |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Bir veritabanı oluşturur |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Bir veya daha fazla veritabanı alır|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Bir veritabanı özelliklerini ayarlar ya da varolan bir veritabanını bir esnek havuza taşır|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Bir veritabanı kaldırır|
|[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Bir kaynak grubu oluşturur|
|[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Sunucu oluşturur|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Sunucuları hakkında bilgi döndürür|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlserver)|Bir sunucu özelliklerini değiştirir|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Bir sunucuyu kaldırır|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Bir sunucu düzeyinde güvenlik duvarı kuralı oluşturur |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Bir sunucu için güvenlik duvarı kurallarını alır|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Bir sunucu güvenlik duvarı kuralında değiştirir|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Bir güvenlik duvarı kuralı bir sunucudan siler.|
| New-AzureRmSqlServerVirtualNetworkRule | Oluşturur bir [ *sanal ağ kuralı*](sql-database-vnet-service-endpoint-rule-overview.md)bağlı bir sanal ağ hizmeti uç noktası olan bir alt ağ olarak. |

## <a name="azure-cli-manage-logical-servers-and-databases"></a>Azure CLI: mantıksal sunucular ve veritabanları yönetme

Azure SQL server, veritabanları ve güvenlik duvarları ile oluşturmak ve yönetmek için [Azure CLI](/cli/azure), aşağıdaki [Azure CLI SQL veritabanı](/cli/azure/sql/db) komutları. CLI’yi tarayıcınızda çalıştırmak için [Cloud Shell](/azure/cloud-shell/overview) kullanın veya macOS, Linux ya da Windows’da [yükleyin](/cli/azure/install-azure-cli). Oluşturma ve esnek havuzlarını yönetmek için bkz: [esnek havuzlar](sql-database-elastic-pool.md).

> [!TIP]
> Azure CLI Hızlı Başlangıç için bkz: [Azure CLI kullanarak tek bir Azure SQL veritabanı oluşturma](sql-database-get-started-cli.md). Azure CLI örnek komut dosyaları için bkz: [kullanım tek bir Azure SQL veritabanı oluşturmak ve bir güvenlik duvarı kuralı yapılandırmak için CLI](scripts/sql-database-create-and-configure-database-cli.md) ve [kullanımı izlemek ve tek bir SQL veritabanı ölçeklendirmek için CLI](scripts/sql-database-monitor-and-scale-database-cli.md).
>

| Cmdlet | Açıklama |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#az_sql_db_create) |Bir veritabanı oluşturur|
|[az sql db listesi](/cli/azure/sql/db#az_sql_db_list)|Veritabanları ve tüm veri ambarlarında bir sunucu veya esnek havuzdaki tüm veritabanları listeler.|
|[az sql db listesi-sürümleri](/cli/azure/sql/db#az_sql_db_list_editions)|Listeleri kullanılabilir hizmet hedefleri ve depolama sınırları|
|[az sql db listesi-kullanımları](/cli/azure/sql/db#az_sql_db_list_usages)|Kullanımları döndürür veritabanı|
|[az sql db Göster](/cli/azure/sql/db#az_sql_db_show)|Bir veritabanı veya veri ambarı alır|
|[az sql db update](/cli/azure/sql/db#az_sql_db_update)|Bir veritabanını güncelleştirir|
|[az sql db Sil](/cli/azure/sql/db#az_sql_db_delete)|Bir veritabanı kaldırır|
|[az group create](/cli/azure/group#az_group_create)|Bir kaynak grubu oluşturur|
|[az sql server create](/cli/azure/sql/server#az_sql_server_create)|Sunucu oluşturur|
|[az sql sunucu listesi](/cli/azure/sql/server#az_sql_server_list)|Sunucuları listeler|
|[az sql server listesi-kullanımları](/cli/azure/sql/server#az_sql_server_list_usages)|Sunucu kullanımları döndürür|
|[az sql server Göster](/cli/azure/sql/server#az_sql_server_show)|Bir sunucu alır|
|[az sql server güncelleştirmesi](/cli/azure/sql/server#az_sql_server_update)|Bir sunucu güncelleştirir|
|[az sql server silme](/cli/azure/sql/server#az_sql_server_delete)|Bir sunucu siler|
|[az sql server güvenlik duvarı kuralı oluşturma](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Sunucu güvenlik duvarı kuralı oluşturur|
|[az sql server güvenlik duvarı kuralı listesi](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Bir sunucudaki güvenlik duvarı kurallarını listeler|
|[az sql server güvenlik duvarı kuralı Göster](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Bir güvenlik duvarı kuralı ayrıntılarını gösterir|
|[az sql server güvenlik duvarı kuralı güncelleştirme](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Bir güvenlik duvarı kuralını güncelleştirir|
|[az sql server güvenlik duvarı kuralını Sil](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Bir güvenlik duvarı kuralını siler|

## <a name="transact-sql-manage-logical-servers-and-databases"></a>Transact-SQL: mantıksal sunucular ve veritabanları yönetme

Azure SQL server, veritabanları ve güvenlik duvarları Transact-SQL ile oluşturmak ve yönetmek için aşağıdaki T-SQL komutlarını kullanın. Azure portalını kullanarak bu komutlar gönderebilirsiniz [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), veya bir Azure SQL veritabanı sunucusuna bağlanın ve Transact-SQL geçirmek başka bir programı komutları. Esnek havuzlarını yönetmek için bkz: [esnek havuzlar](sql-database-elastic-pool.md).


> [!TIP]
> Microsoft Windows üzerinde SQL Server Management Studio'yu kullanarak bir hızlı başlangıç için bkz: [Azure SQL Database: bağlanmak ve verileri sorgulamak için kullanım SQL Server Management Studio](sql-database-connect-query-ssms.md). Visual Studio Code macOS, Linux veya Windows kullanarak bir hızlı başlangıç için bkz: [Azure SQL Database: kullanım Visual Studio Code bağlanmak ve verileri sorgulamak için](sql-database-connect-query-vscode.md).

> [!IMPORTANT]
> Oluşturamaz veya Transact-SQL kullanarak bir sunucu silme.
>

| Komut | Açıklama |
| --- | --- |
|[Veritabanı (Azure SQL veritabanı) oluşturma](/sql/t-sql/statements/create-database-azure-sql-database)|Yeni bir veritabanı oluşturur. Yeni bir veritabanı oluşturmak için ana veritabanına bağlanması gerekir.|
| [ALTER DATABASE (Azure SQL veritabanı)](/sql/t-sql/statements/alter-database-azure-sql-database) |Bir Azure SQL veritabanı değiştirir. |
|[ALTER DATABASE (Azure SQL veri ambarı)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Bir Azure SQL veri ambarı değiştirir.|
|[VERİTABANINI (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Bir veritabanını siler.|
|[sys.database_service_objectives (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Edition (hizmet katmanı), hizmet hedefi (fiyatlandırma katmanı) ve esnek havuz adı, varsa Azure SQL veritabanına veya Azure SQL Data Warehouse için döndürür. Azure SQL Database sunucusu ana veritabanında oturum açtıysanız, bilgiler tüm veritabanlarını döndürür. Azure SQL Data Warehouse için ana veritabanına bağlı olmalıdır.|
|[sys.dm_db_resource_stats (Azure SQL veritabanı)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Bir Azure SQL Database veritabanı için CPU, g/ç ve bellek tüketimini döndürür. Veritabanında hiçbir etkinlik olsa 15 dakikada için bir satır yok.|
|[sys.resource_stats (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|CPU kullanımı ve depolama verileri için bir Azure SQL veritabanına döndürür. Veriler toplanır ve beş dakikalık aralıklarla içinde toplanır.|
|[sys.database_connection_stats (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|SQL veritabanı veritabanı bağlantı olayları, bir veritabanı bağlantısı başarı ve başarısızlık durumlarının özetini istatistiklerini içerir. |
|[sys.event_log (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Başarılı Azure SQL Database veritabanı bağlantıları, bağlantı hataları ve kilitlenmeleri döndürür. Veritabanı etkinliklerinizi SQL veritabanı ile ilgili sorunları giderme veya izlemek için bu bilgileri kullanın.|
|[sp_set_firewall_rule (Azure SQL veritabanı)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Oluşturur veya SQL veritabanı sunucunuzun sunucu düzeyinde güvenlik duvarı ayarlarını güncelleştirir. Bu saklı yordam yalnızca ana veritabanında sunucu düzeyinde asıl oturum açmak için kullanılabilir. Sunucu düzeyinde güvenlik duvarı kuralı yalnızca Azure düzeyi izinleri olan bir kullanıcı tarafından ilk sunucu düzeyinde güvenlik duvarı kural oluşturulduktan sonra Transact-SQL kullanılarak oluşturulabilir|
|[sys.firewall_rules (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Microsoft Azure SQL veritabanı ile ilişkili sunucu düzeyinde güvenlik duvarı ayarları hakkında bilgi verir.|
|[sp_delete_firewall_rule (Azure SQL veritabanı)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Sunucu düzeyinde güvenlik duvarı ayarları, SQL veritabanı sunucusundan kaldırır. Bu saklı yordam yalnızca ana veritabanında sunucu düzeyinde asıl oturum açmak için kullanılabilir.|
|[sp_set_database_firewall_rule (Azure SQL veritabanı)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Oluşturur veya Azure SQL Database veya SQL Data Warehouse için veritabanı düzeyinde güvenlik duvarı kuralları güncelleştirir. Veritabanı güvenlik duvarı kuralları SQL veritabanı kullanıcı veritabanlarında ve ana veritabanı için yapılandırılabilir. Veritabanı güvenlik duvarı kurallarını kullanarak, veritabanı kullanıcıları bağımsız faydalıdır. |
|[sys.database_firewall_rules (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Microsoft Azure SQL veritabanı ile ilişkili veritabanı düzeyinde güvenlik duvarı ayarları hakkında bilgi verir. |
|[sp_delete_database_firewall_rule (Azure SQL veritabanı)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Veritabanı düzeyi güvenlik duvarı ayarı, Azure SQL Database veya SQL Data Warehouse kaldırır. |



## <a name="rest-api-manage-logical-servers-and-databases"></a>REST API: mantıksal sunucular ve veritabanları yönetme

Azure SQL server, veritabanları ve güvenlik duvarları oluşturmak ve yönetmek için bu REST API istekleri kullanın.

| Komut | Açıklama |
| --- | --- |
|[Sunucuları - oluştur veya güncelleştir](/rest/api/sql/servers/createorupdate)|Oluşturur veya yeni bir sunucu güncelleştirir.|
|[Sunucuları - Sil](/rest/api/sql/servers/delete)|Bir SQL server siler.|
|[Sunucuları - Al](/rest/api/sql/servers/get)|Bir sunucu alır.|
|[Sunucuları - liste](/rest/api/sql/servers/list)|Sunucularının bir listesini döndürür.|
|[Sunucuları - kaynak grubuna göre listesi](/rest/api/sql/servers/listbyresourcegroup)|Bir kaynak grubunda sunucularının bir listesini döndürür.|
|[Sunucuları - güncelleştirme](/rest/api/sql/servers/update)|Var olan bir sunucu güncelleştirir.|
|[Veritabanları - oluştur veya güncelleştir](/rest/api/sql/databases/createorupdate)|Yeni bir veritabanı oluşturur veya varolan bir veritabanını güncelleştirir.|
|[Veritabanları - Al](/rest/api/sql/databases/get)|Bir veritabanı alır.|
|[Veritabanı - esnek havuz tarafından Al](/rest/api/sql/databases/getbyelasticpool)|Bir veritabanını bir esnek havuz içinde alır.|
|[Önerilen esnek havuzu tarafından veritabanları - Al](/rest/api/sql/databases/getbyrecommendedelasticpool)|Bir veritabanı içinde recommented bir esnek havuz alır.|
|[Veritabanı - esnek havuz göre listesi](/rest/api/sql/databases/listbyelasticpool)|Bir esnek havuz veritabanlarının bir listesini döndürür.|
|[Veritabanları - önerilen esnek havuz göre listesi](/rest/api/sql/databases/listbyrecommendedelasticpool)|Önerilen esnek havuz içindeki veritabanlarının bir listesini döndürür.|
|[Veritabanları - sunucu tarafından listesi](/rest/api/sql/databases/listbyserver)|Bir sunucu veritabanlarının bir listesini döndürür.|
|[Veritabanları - güncelleştirme](/rest/api/sql/databases/update)|Varolan bir veritabanını güncelleştirir.|
|[Güvenlik duvarı kuralları - oluştur veya güncelleştir](/rest/api/sql/firewallrules/createorupdate)|Oluşturur veya bir güvenlik duvarı kuralı güncelleştirir.|
|[Güvenlik duvarı kuralları - Sil](/rest/api/sql/firewallrules/delete)|Bir güvenlik duvarı kuralını siler.|
|[Güvenlik duvarı kuralları - Al](/rest/api/sql/firewallrules/get)|Bir güvenlik duvarı kuralı alır.|
|[Güvenlik duvarı kuralları - sunucu tarafından listesi](/rest/api/sql/firewallrules/listbyserver)|Güvenlik duvarı kurallarının bir listesini döndürür.|

## <a name="next-steps"></a>Sonraki adımlar

- Azure için SQL Server veritabanını geçirme hakkında bilgi edinmek için [Azure SQL veritabanına geçirme](sql-database-cloud-migrate.md).
- Desteklenen özellikler hakkında bilgi edinmek için bkz. [Özellikler](sql-database-features.md).
