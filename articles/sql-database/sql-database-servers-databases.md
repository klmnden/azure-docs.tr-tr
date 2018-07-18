---
title: Azure SQL mantıksal sunucuları ve tek veritabanları | Microsoft Docs
description: Azure SQL veritabanı mantıksal sunucusu ve tek veritabanı kavramlarını ve kaynakları hakkında bilgi edinin.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: carlrab
ms.openlocfilehash: c96282b8163cc48001ee3c6fe89497e2793309f6
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091805"
---
# <a name="azure-sql-database-logical-servers-and-single-databases-and-their-resources"></a>Azure SQL veritabanı mantıksal sunucuları ve tek veritabanları ve kaynakları

## <a name="what-is-an-azure-sql-logical-server"></a>Bir Azure SQL mantıksal sunucusu nedir?

Merkezi bir yönetim noktası için birden çok tek bir mantıksal sunucu görür veya [havuza alınmış](sql-database-elastic-pool.md) veritabanları [oturumları](sql-database-manage-logins.md), [güvenlik duvarı kuralları](sql-database-firewall-configure.md), [kurallarıdenetleme](sql-database-auditing.md), [tehdit algılama ilkeleri](sql-database-threat-detection.md), ve [yük devretme grupları](sql-database-geo-replication-overview.md). Bir mantıksal sunucu, kaynak grubu farklı bir bölgede olabilir. Azure SQL veritabanı oluşturmadan önce mantıksal sunucunun mevcut olması gerekir. Bir sunucudaki tüm veritabanları, mantıksal sunucusuyla aynı bölgede oluşturulur.

Bir mantıksal sunucu şirket içi dünyada alışkın olabileceğiniz bir SQL Server örneğinden farklı olan mantıksal bir yapıdır. SQL Veritabanı hizmeti, veritabanlarının mantıksal sunuculara göre konumuyla ilgili özel bir garanti vermez ve örnek düzeyinde erişim ya da özellik sunmaz. Buna karşılık, bir SQL veritabanı yönetilen örneği'nde bir sunucuya şirket içi dünyada alışkın olabileceğiniz bir SQL Server örneğine benzer.

Bir mantıksal sunucu oluşturduğunuzda, oturum açma hesabı ve parola ana veritabanında bu sunucudaki ve o sunucuda oluşturulan tüm veritabanları için yönetici haklarına sahip bir sunucu sağlayın. Bir SQL oturum açma hesabı bu ilk hesaptır. Azure SQL veritabanı, SQL kimlik doğrulaması ve Azure Active Directory kimlik doğrulaması için kimlik doğrulamasını destekler. Oturum açma bilgileri ve kimlik doğrulaması hakkında daha fazla bilgi için bkz. [yönetme veritabanları ve Azure SQL veritabanı'nda oturum açma bilgileri](sql-database-manage-logins.md). Windows Kimlik Doğrulaması desteklenmez. 

> [!TIP]
> Geçerli kaynak grubu ve sunucu adları için bkz. [adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).
>

Azure SQL Veritabanı mantıksal sunucusu:

- Bir Azure aboneliği içinde oluşturulur, ancak içerdiği kaynaklarla birlikte başka bir aboneliğe taşınabilir
- Veritabanları, elastik havuzlar ve veri ambarları için üst kaynaktır
- Veritabanları, elastik havuzlar ve veri ambarları için ad alanı sağlar.
- Bir mantıksal kapsayıcıdır güçlü kullanım ömrü semantiğine sahip - bir sunucu silme ve kapsanan veritabanları, elastik havuzlar ve veri ambarlarını siler
- Katılan [Azure rol tabanlı erişim denetimi (RBAC)](/azure/role-based-access-control/overview) -bir sunucu içindeki veri ambarları veritabanları ve elastik havuzlar sunucudan erişim haklarını devralır
- Veritabanları, elastik havuzlar ve veri ambarları için Azure kaynak kimliğini yüksek düzeyli bir öğedir (veritabanları ve havuzlar için URL şemasına bakın), yönetim amaçları mi
- Bir bölgedeki kaynakları birlikte bulundurur
- Veritabanı erişimi için bağlantı uç noktası sağlar (<serverName>.database.windows.net)
- Bir ana veritabanına bağlanarak DMV’ler aracılığıyla içerdiği kaynaklarla ilgili meta verilere erişim sağlar 
- Uygulama veritabanları - oturum açma bilgileri için güvenlik duvarı, Denetim, tehdit algılama, vb. Yönetimi ilkelerinin kapsamını sağlar. 
- Üst abonelik içinde bir kota ile sınırlandırılır (varsayılan - olarak abonelik başına altı sunucu [bkz. abonelik limitleri](../azure-subscription-service-limits.md))
- (45000 DTU gibi) içeren kaynaklar için yönelik veritabanı kotası ile DTU veya sanal çekirdek kotası kapsamını sağlar
- İçerdiği kaynaklarda etkinleştirilmiş özelliklerin sürüm kapsamıdır 
- Sunucu düzeyinde asıl kullanıcı bilgileri bir sunucudaki tüm veritabanlarını yönetebilir
- Şirketinizde sunucu üzerindeki bir veya daha fazla veritabanına erişim verilmiş SQL Server örneklerinde bulunanlara benzer kullanıcı bilgileri içerebilir ve sınırlı yönetici hakları alabilir. Daha fazla bilgi için bkz. [Kullanıcı Bilgileri](sql-database-manage-logins.md).
- Bir mantıksal sunucuda oluşturulan tüm kullanıcı veritabanları için varsayılan harmanlaması `SQL_LATIN1_GENERAL_CP1_CI_AS`burada `LATIN1_GENERAL` İngilizce (Amerika Birleşik Devletleri) `CP1` kod sayfası 1252, `CI` duyarsızdır, ve `AS` is Aksan duyarlıdır.

## <a name="logical-servers-and-databases"></a>Mantıksal sunucuları ve veritabanları

Bir mantıksal sunucuda oluşturabilirsiniz:

- Tek bir veritabanı içinde oluşturulan bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ile bir [işlem ve depolama kaynakları kümesi birleştirilmiş](sql-database-service-tiers-dtu.md) veya [bağımsız işlem ve depolama kaynaklarınıölçeğini](sql-database-service-tiers-vcore.md). Bir Azure SQL veritabanı, belirli bir Azure bölgesi içinde oluşturulan bir Azure SQL veritabanı mantıksal sunucusu ile ilişkilidir.
- Bir parçası olarak oluşturulmuş bir veritabanı bir [veritabanı havuzu](sql-database-elastic-pool.md) içinde bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ile bir [işlem ve depolama kaynakları kümesi birleştirilmiş (DTU tabanlı)](sql-database-service-tiers-dtu.md) veya bir [bağımsız işlem ve depolama kaynakları (sanal çekirdek tabanlı) ölçeğini](sql-database-service-tiers-vcore.md) tüm havuzdaki veritabanları arasında paylaşılır. Bir Azure SQL veritabanı, belirli bir Azure bölgesi içinde oluşturulan bir Azure SQL veritabanı mantıksal sunucusu ile ilişkilidir.

> [!IMPORTANT]
> SQL veritabanı yönetilen örneği şu anda genel önizlemede olan bir [bir SQL server örneğini](sql-database-managed-instance.md) (yönetilen örnek) oluşturulan bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) ile bir dizi işlem ve depolama Bu sunucu örneğindeki tüm veritabanları için kaynaklar. Yönetilen örnek, sistem ve kullanıcı veritabanlarını içerir. Yönetilen örnek, veritabanı lift-and-shift ile taşıma tam olarak yönetilen bir PaaS için uygulamayı yeniden tasarlamaya gerek kalmadan sağlamak için tasarlanmıştır. Yönetilen örnek, şirket içi SQL Server programlama modeli ile yüksek uyumluluk sağlar ve SQL Server özelliklerini ve eşlik eden araç ve hizmetlerden oluşan büyük bir çoğunluğu destekler. Daha fazla bilgi için bkz. [SQL Veritabanı Yönetilen Örneği](sql-database-managed-instance.md). Bu makalenin geri kalanında yönetilen örneği için geçerli değildir.

## <a name="tds-and-tcpip-connections"></a>TDS ve TCP/IP'yi bağlantıları

Microsoft Azure SQL veritabanı, tablo veri akışı (TDS) Protokolü istemci sürümü 7.3 veya sonraki sürümlerini destekler ve şifrelenmiş TCP/IP bağlantılarını sağlar.

## <a name="azure-sql-databases-protected-by-sql-database-firewall"></a>SQL veritabanı güvenlik duvarı tarafından korunan azure SQL veritabanları

Verilerinizi korumaya yardımcı olmak için bir [SQL veritabanı Güvenlik Duvarı](sql-database-firewall-configure.md) veya kendi veritabanlarından bağlantınızı dışında Azure abonelik bağlantısı üzerinden doğrudan sunucuya birine veritabanı sunucunuza tüm erişimi engeller. Ek bağlantısını etkinleştirmek için şunları yapmanız gerekir [bir veya daha fazla güvenlik duvarı kuralları oluşturma](sql-database-firewall-configure.md#creating-and-managing-firewall-rules). Oluşturma ve elastik havuzları yönetme için bkz: [elastik havuzlar](sql-database-elastic-pool.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-portal"></a>Azure SQL sunucuları, veritabanları ve güvenlik duvarları Azure portalını kullanarak yönetme

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
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-powershell"></a>Azure SQL sunucularını, veritabanlarını ve güvenlik duvarları PowerShell kullanarak yönetme

Azure SQL server, veritabanları ve Azure PowerShell ile güvenlik duvarları oluşturmak ve yönetmek için aşağıdaki PowerShell cmdlet'lerini kullanın. Gerekirse yükleyin veya PowerShell yükseltmek için bkz [Azure PowerShell modülü yükleme](/powershell/azure/install-azurerm-ps). Oluşturma ve elastik havuzları yönetme için bkz: [elastik havuzlar](sql-database-elastic-pool.md).

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

> [!TIP]
> PowerShell için hızlı başlangıç için bkz: [PowerShell kullanarak tek bir Azure SQL veritabanı oluşturma](sql-database-get-started-portal.md). PowerShell örnek komut dosyaları için bkz: [tek bir Azure SQL veritabanı oluşturma ve bir güvenlik duvarı kuralı yapılandırmak için PowerShell kullanma](scripts/sql-database-create-and-configure-database-powershell.md) ve [İzleyici ve ölçek tek bir SQL veritabanını PowerShell kullanarak](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-cli"></a>Azure SQL sunucularını, veritabanlarını ve güvenlik duvarlarını Azure CLI kullanarak yönetme

Azure SQL server, veritabanlarını ve güvenlik duvarlarıyla oluşturmak ve yönetmek için [Azure CLI](/cli/azure), aşağıdaki [Azure CLI SQL veritabanı](/cli/azure/sql/db) komutları. CLI’yi tarayıcınızda çalıştırmak için [Cloud Shell](/azure/cloud-shell/overview) kullanın veya macOS, Linux ya da Windows’da [yükleyin](/cli/azure/install-azure-cli). Oluşturma ve elastik havuzları yönetme için bkz: [elastik havuzlar](sql-database-elastic-pool.md).

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

> [!TIP]
> Azure CLI Hızlı Başlangıç için bkz. [Azure CLI kullanarak tek bir Azure SQL veritabanı oluşturma](sql-database-get-started-cli.md). Azure CLI örnek betikler için bkz: [kullanımı tek bir Azure SQL veritabanı oluşturma ve bir güvenlik duvarı kuralı yapılandırmak için CLI](scripts/sql-database-create-and-configure-database-cli.md) ve [kullanımı tek bir SQL veritabanını izleme ve ölçeklendirme için CLI](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-transact-sql"></a>Azure SQL sunucularını, veritabanlarını ve güvenlik duvarları Transact-SQL kullanarak yönetme

Azure SQL server, veritabanları ve Transact-SQL ile güvenlik duvarları oluşturmak ve yönetmek için aşağıdaki T-SQL komutlarını kullanın. Azure portalını kullanarak şu komutları verebilirsiniz [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), veya bir Azure SQL Database sunucusuna bağlanma ve Transact-SQL geçirmek başka bir programı komutları. Elastik havuzlar için bkz. [elastik havuzlar](sql-database-elastic-pool.md).

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


> [!TIP]
> Microsoft Windows üzerinde SQL Server Management Studio'yu kullanarak bir hızlı başlangıç için bkz: [Azure SQL veritabanı: bağlanmak ve veri sorgulamak için kullanım SQL Server Management Studio](sql-database-connect-query-ssms.md). MacOS, Linux veya Windows üzerinde Visual Studio Code'u kullanarak bir hızlı başlangıç için bkz: [Azure SQL veritabanı: kullanım Visual Studio Code bağlanmak ve veri sorgulamak için](sql-database-connect-query-vscode.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-rest-api"></a>Azure SQL sunucularını, veritabanlarını ve güvenlik duvarları REST API kullanarak yönetme

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
