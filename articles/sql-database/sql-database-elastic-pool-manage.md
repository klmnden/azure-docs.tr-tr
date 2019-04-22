---
title: Elastik havuzlar - Azure SQL veritabanı'nı yönetme | Microsoft Docs
description: Oluşturma ve Azure SQL elastik havuzları yönetme.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: seoapril2019
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: debf91f04cff3cb9705ebc5915e2e665679230a9
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59267591"
---
# <a name="manage-elastic-pools-in-azure-sql-database"></a>Azure SQL veritabanı elastik havuzları yönetme

Bir elastik havuz ile elastik havuz, veritabanı iş yükünü işlemek için gerektirdiği kaynakların miktarını ve havuza alınmış her veritabanı için kaynakların miktarını belirler.

## <a name="azure-portal-manage-elastic-pools-and-pooled-databases"></a>Azure portalı: Elastik havuzlara ve havuza alınmış veritabanlarını yönetme

Tüm havuzu ayarları tek bir yerde bulunabilir: **havuzu Yapılandır** dikey penceresi. Buraya ulaşmak için elastik havuz portalı ve tıklatın bulma **havuzu Yapılandır** dikey penceresinin üst veya sol taraftaki kaynak menüsünde.

Buradan tüm bir toplu işlemde herhangi bir birleşimini kaydetmek ve aşağıdaki değişiklikleri yapabilir:

1. Değişiklik havuzun hizmet katmanına
2. Depolama ve performans (DTU veya sanal çekirdekler) ölçeği artırın veya azaltın
3. Veritabanları için/havuzundan ekleyip
4. (Garanti) en az ayarlayın ve havuzlardaki veritabanları için performans sınırı maks.
5. Yeni seçimlerinizi sonucu olarak, faturanıza değişiklikleri görüntülemek için maliyet özeti gözden geçirin

![Elastik havuz yapılandırma dikey penceresi](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

## <a name="powershell-manage-elastic-pools-and-pooled-databases"></a>PowerShell: Elastik havuzlara ve havuza alınmış veritabanlarını yönetme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

SQL veritabanı elastik havuzları ve Azure PowerShell ile havuza alınan veritabanları oluşturmak ve yönetmek için aşağıdaki PowerShell cmdlet'lerini kullanın. Gerekirse yükleyin veya PowerShell yükseltmek için bkz [Azure PowerShell modülü yükleme](/powershell/azure/install-az-ps). Oluşturma ve elastik havuzlar için SQL veritabanı sunucularını yönetmek için bkz: [oluşturma ve SQL veritabanı sunucularını yönetme](sql-database-servers.md). Oluşturma ve güvenlik duvarı kurallarını yönetmek için bkz: [oluşturma ve PowerShell kullanarak güvenlik duvarı kurallarını yönetme](sql-database-firewall-configure.md#manage-server-level-ip-firewall-rules-using-azure-powershell).

> [!TIP]
> PowerShell örnek komut dosyaları için bkz: [elastik havuzlar oluşturma ve PowerShell kullanarak bir havuz dışına ve havuzlar arasında veritabanlarını taşıma](scripts/sql-database-move-database-between-pools-powershell.md) ve [PowerShell kullanarak izleme veAzureSQLveritabanındabirSQLelastikhavuzunuölçeklendirme](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Cmdlet | Açıklama |
| --- | --- |
|[New-AzSqlElasticPool](/powershell/module/az.sql/new-azsqlelasticpool)|Elastik havuz oluşturur.|
|[Get-AzSqlElasticPool](/powershell/module/az.sql/get-azsqlelasticpool)|Elastik havuzlar ve özellik değerlerini alır.|
|[Set-AzSqlElasticPool](/powershell/module/az.sql/set-azsqlelasticpool)|Örneğin, kullanım elastik havuz özelliklerini değiştirir **StorageMB** özelliği en fazla depolama alanı, bir elastik havuzun değiştirilecek.|
|[Remove-AzSqlElasticPool](/powershell/module/az.sql/remove-azsqlelasticpool)|Elastik havuz siler.|
|[Get-AzSqlElasticPoolActivity](/powershell/module/az.sql/get-azsqlelasticpoolactivity)|Elastik havuz üzerinde işlem durumunu alır|
|[New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase)|Mevcut havuzlardan veya tek bir veritabanı olarak yeni bir veritabanı oluşturur. |
|[Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase)|Bir veya daha fazla veritabanını alır.|
|[Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase)|Bir veritabanı özelliklerini ayarlar veya mevcut bir veritabanı içine, dışı ya da elastik havuzlar arasında taşır.|
|[Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase)|Bir veritabanı kaldırır.|

> [!TIP]
> Çok sayıda veritabanını bir elastik havuzdaki oluşturulmasını, portal veya aynı anda yalnızca tek bir veritabanı oluşturma PowerShell cmdlet'leri kullanılarak bittiğinde zaman alabilir. Bir elastik havuzun içine oluşturmayı otomatikleştirmek için bkz: [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).

## <a name="azure-cli-manage-elastic-pools-and-pooled-databases"></a>Azure CLI: Elastik havuzlara ve havuza alınmış veritabanlarını yönetme

Oluşturma ve SQL veritabanı elastik havuzları yönetme [Azure CLI](/cli/azure), aşağıdaki [Azure CLI SQL veritabanı](/cli/azure/sql/db) komutları. CLI’yi tarayıcınızda çalıştırmak için [Cloud Shell](/azure/cloud-shell/overview) kullanın veya macOS, Linux ya da Windows’da [yükleyin](/cli/azure/install-azure-cli).

> [!TIP]
> Azure CLI örnek betikler için bkz: [kullanımı bir SQL elastik havuzunda bir Azure SQL veritabanını taşımak için CLI](scripts/sql-database-move-database-between-pools-cli.md) ve [Azure SQL veritabanında bir SQL elastik havuzunu ölçeklendirmek için Azure CLI'yı kullanmak](scripts/sql-database-scale-pool-cli.md).
>

| Cmdlet | Açıklama |
| --- | --- |
|[az sql elastic-pool oluşturma](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-create)|Elastik havuz oluşturur.|
|[az sql elastic-pool listesi](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-list)|Bir sunucu elastik havuzları listesini döndürür.|
|[az sql elastic-pool DB'leri-Listele](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-list-dbs)|Elastik havuzdaki veritabanlarının listesini döndürür.|
|[az sql elastic-pool sürümleri Listele](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-list-editions)|Ayrıca depolama sınırları, kullanılabilir havuz DTU ayarlarını içerir ve veritabanı ayarları başına. Ayrıntı düzeyi, ek depolama alanı sınırları azaltmak için ve veritabanı başına ayarları varsayılan olarak gizlidir.|
|[az sql elastic-pool update](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update)|Elastik havuz güncelleştirir.|
|[az sql elastic-pool delete](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-delete)|Esnek havuz siler.|

## <a name="transact-sql-manage-pooled-databases"></a>Transact-SQL: Havuza alınmış veritabanlarını yönetme

Oluşturma ve veritabanlarını taşıma mevcut elastik havuzlar için ya da Transact-SQL ile SQL veritabanı elastik havuz hakkında bilgi döndürmek için aşağıdaki T-SQL komutlarını kullanın. Azure portalını kullanarak şu komutları verebilirsiniz [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), veya bir Azure SQL Database sunucusuna bağlanma ve Transact-SQL geçirmek başka bir programı komutları. Oluşturma ve T-SQL'yi kullanarak güvenlik duvarı kurallarını yönetmek için bkz: [Transact-SQL kullanarak güvenlik duvarı kurallarını yönetme](sql-database-firewall-configure.md#manage-ip-firewall-rules-using-transact-sql).

> [!IMPORTANT]
> Oluşturma, güncelleştirme veya Transact-SQL kullanarak bir Azure SQL Database esnek havuzunu silme olamaz. Ekleyebilir veya veritabanlarını bir elastik havuzdan kaldırın ve mevcut elastik havuzlar hakkında bilgi döndürmek için Dmv'leri kullanın.
>

| Komut | Açıklama |
| --- | --- |
|[Veritabanı (Azure SQL veritabanı) oluşturma](/sql/t-sql/statements/create-database-azure-sql-database)|Mevcut havuzlardan veya tek bir veritabanı olarak yeni bir veritabanı oluşturur. Yeni bir veritabanı oluşturmak için ana veritabanına bağlı olmanız gerekir.|
| [ALTER DATABASE (Azure SQL veritabanı)](/sql/t-sql/statements/alter-database-azure-sql-database) |Bir veritabanı içine, dışı ya da elastik havuzlar arasında taşıyın.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Bir veritabanını siler.|
|[sys.elastic_pool_resource_stats (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Tüm elastik havuzlar için kaynak kullanım istatistikleri, SQL veritabanı sunucusu döndürür. Her bir elastik havuz için her 15 saniyede penceresi (dakika başına dört satır) raporlama için bir satır yok. Bu CPU, GÇ, günlük, depolama alanı tüketimi ve eş zamanlı istek/oturum kullanımı havuzdaki tüm veritabanları tarafından içerir.|
|[sys.database_service_objectives (Azure SQL veritabanı)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Edition (hizmet katmanı), hizmet hedefi (fiyatlandırma katmanı) ve elastik havuz adı varsa, Azure SQL veritabanına veya bir Azure SQL veri ambarı için döndürür. Azure SQL veritabanı sunucusu ana veritabanında oturum açtıysanız, tüm veritabanlarında bilgileri döndürür. Azure SQL veri ambarı için ana veritabanına bağlı olmanız gerekir.|

## <a name="rest-api-manage-elastic-pools-and-pooled-databases"></a>REST API: Elastik havuzlara ve havuza alınmış veritabanlarını yönetme

SQL veritabanı elastik havuzları ve havuza alınan veritabanları oluşturmak ve yönetmek için bu REST API istekleri'ni kullanın.

| Komut | Açıklama |
| --- | --- |
|[Elastik havuzlar - oluştur veya güncelleştir](https://docs.microsoft.com/rest/api/sql/elasticpools/createorupdate)|Yeni bir elastik havuz oluşturur veya mevcut bir elastik havuz güncelleştirir.|
|[Elastik havuzlar - Sil](https://docs.microsoft.com/rest/api/sql/elasticpools/delete)|Esnek havuz siler.|
|[Elastik havuzlar - Get](https://docs.microsoft.com/rest/api/sql/elasticpools/get)|Elastik havuz alır.|
|[Elastik havuzlar - sunucu listesi](https://docs.microsoft.com/rest/api/sql/elasticpools/listbyserver)|Bir sunucu elastik havuzları listesini döndürür.|
|[Elastik havuzlar - güncelleştirme](https://docs.microsoft.com/rest/api/sql/elasticpools/listbyserver)|Var olan bir esnek havuzun güncelleştirir.|
|[Elastik havuz etkinlikleri](https://docs.microsoft.com/rest/api/sql/elasticpoolactivities)|Elastik havuz etkinlikleri döndürür.|
|[Elastik havuz veritabanı etkinlikleri](https://docs.microsoft.com/rest/api/sql/elasticpooldatabaseactivities)|İçinde bir elastik havuzdaki veritabanları üzerinde etkinlik döndürür.|
|[Veritabanları - oluştur veya güncelleştir](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)|Yeni bir veritabanı oluşturur veya mevcut bir veritabanını güncelleştirir.|
|[Veritabanları - Get](https://docs.microsoft.com/rest/api/sql/databases/get)|Bir veritabanını alır.|
|[Veritabanı - elastik havuz göre listesi](https://docs.microsoft.com/rest/api/sql/databases/listbyelasticpool)|Elastik havuzdaki veritabanlarının listesini döndürür.|
|[Veritabanı - sunucu listesi](https://docs.microsoft.com/rest/api/sql/databases/listbyserver)|Bir sunucu veritabanlarının listesini döndürür.|
|[Veritabanları - güncelleştirme](https://docs.microsoft.com/rest/api/sql/databases/update)|Varolan bir veritabanını güncelleştirir.|

## <a name="next-steps"></a>Sonraki adımlar

* Esnek havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Database kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* Esnek havuzları kullanan bir SaaS öğretici için bkz. [Wingtip SaaS uygulamasına giriş](sql-database-wtp-overview.md).
