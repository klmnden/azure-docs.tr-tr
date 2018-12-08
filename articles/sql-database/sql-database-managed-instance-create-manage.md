---
title: Oluşturma, Azure SQL yönetilen örneği yönetme | Microsoft Docs
description: Oluşturma ve Azure SQL veritabanı yönetilen örnekleri yönetme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 12/03/2018
ms.openlocfilehash: 3e714df775d316ceaafe1a0ce9b55c4e986804cd
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856871"
---
# <a name="create-and-manage-azure-sql-database-managed-instances"></a>Azure SQL veritabanı yönetilen örnekleri oluşturma ve yönetme

Oluşturun ve Azure SQL veritabanı yönetilen Azure portalı, PowerShell, Azure CLI, REST API ve Transact-SQL kullanarak örnekleri yönetin.

## <a name="azure-portal-create-a-managed-instance"></a>Azure portalı: bir yönetilen örnek oluşturma

Bir Azure SQL veritabanı yönetilen örneği oluşturmayı gösteren Hızlı Başlangıç için bkz: [hızlı başlangıç: Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-get-started.md).

## <a name="powershell-create-and-manage-a-managed-instance"></a>PowerShell: Oluşturma ve bir yönetilen örnek'ı yönetme

Azure SQL server, veritabanları ve Azure PowerShell ile güvenlik duvarları oluşturmak ve yönetmek için aşağıdaki PowerShell cmdlet'lerini kullanın. Gerekirse yükleyin veya PowerShell yükseltmek için bkz [Azure PowerShell modülü yükleme](/powershell/azure/install-azurerm-ps).

> [!TIP]
> PowerShell örnek komut dosyaları için bkz. [betik hızlı başlangıç: oluşturma kullanarak Azure SQL yönetilen PowerShell kitaplık örneği](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/).

| Cmdlet | Açıklama |
| --- | --- |
|[Yeni AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlinstance)|Bir Azure SQL veritabanı yönetilen örneği oluşturur. |
|[Get-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/Get-AzureRmSqlInstance)|Azure SQL yönetilen örneği hakkında bilgi döndürür|
|[Set-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/Set-AzureRmSqlInstance)|Azure SQL veritabanı yönetilen örneği için özellikleri ayarlar|
|[Remove-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/Remove-AzureRmSqlInstance)|Bir Azure SQL veritabanı yönetilen örneği kaldırır|

## <a name="azure-cli-manage-logical-servers-and-databases"></a>Azure CLI: mantıksal sunucuları ve veritabanlarını yönetme

Azure SQL server, veritabanlarını ve güvenlik duvarlarıyla oluşturmak ve yönetmek için [Azure CLI](/cli/azure), aşağıdaki [Azure CLI SQL yönetilen örneği](/cli/azure/sql/mi) komutları. CLI’yi tarayıcınızda çalıştırmak için [Cloud Shell](/azure/cloud-shell/overview) kullanın veya macOS, Linux ya da Windows’da [yükleyin](/cli/azure/install-azure-cli).

> [!TIP]
> Azure CLI Hızlı Başlangıç için bkz. [SQL yönetilen örneği kullanarak Azure CLI ile çalışma](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44).
>

| Cmdlet | Açıklama |
| --- | --- |
|[az sql mı oluşturma](https://docs.microsoft.com/cli/azure/sql/db#az-sql-mi-create) |Yönetilen bir örneğini oluşturur|
|[az sql mı listesi](https://docs.microsoft.com/cli/azure/sql/db#az-sql-mi-list)|Listeleri kullanılabilir yönetilen örnekler|
|[az sql mı Göster](/cli/azure/sql/db#az-sql-mi-show)|Ayrıntılı bilgi almak için bir yönetilen örnek|
|[az sql mı güncelleştirme](/cli/azure/sql/db#az-sql-mi-update)|Yönetilen örnek güncelleştirir|
|[az sql mı Sil](/cli/azure/sql/db#az-sql-mi-delete)|Yönetilen örnek kaldırır|

## <a name="transact-sql-manage-logical-servers-and-databases"></a>Transact-SQL: mantıksal sunucuları ve veritabanlarını yönetme

Oluşturma ve Azure SQL veritabanı yönetilen örneği veritabanı yönetilen örneği oluşturulduktan sonra yönetmek için aşağıdaki T-SQL komutlarını kullanın. Azure portalını kullanarak şu komutları verebilirsiniz [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Azure veri Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is). [Visual Studio Code](https://code.visualstudio.com/docs), veya bir Azure SQL Database sunucusuna bağlanma ve Transact-SQL komutlarını geçirmek başka bir program.

> [!TIP]
> İçin yapılandırmak ve Microsoft Windows üzerinde SQL Server Management Studio kullanarak yönetilen örneğe bağlanmak zorunda gösteren hızlı başlangıçlara bakın [hızlı başlangıç: Azure SQL veritabanı yönetilen örneğine bağlanmak için Azure VM yapılandırma](sql-database-managed-instance-configure-vm.md) ve [ Hızlı Başlangıç: Azure SQL veritabanı yönetilen örneği için noktadan siteye bağlantı, şirket içinden yapılandırın](sql-database-managed-instance-configure-p2s.md).
> [!IMPORTANT]
> Oluşturamaz veya Transact-SQL kullanarak bir yönetilen örneğini silin.

| Komut | Açıklama |
| --- | --- |
|[CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current)|Yeni bir yönetilen örnek veritabanı oluşturur. Yeni bir veritabanı oluşturmak için ana veritabanına bağlı olmanız gerekir.|
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) |Bir Azure SQL yönetilen örnek veritabanını değiştirir.|

## <a name="rest-api-manage-logical-servers-and-databases"></a>REST API: mantıksal sunucuları ve veritabanlarını yönetme

Oluşturma ve Azure SQL veritabanı yönetilen örneği yönetmek için bu REST API istekleri'ni kullanın.

| Komut | Açıklama |
| --- | --- |
|[Yönetilen örnekler - oluştur veya güncelleştir](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)|Oluşturur veya yönetilen bir örneğini güncelleştirir.|
|[Yönetilen örnekleri - sil](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)|Yönetilen örnek siler.|
|[Yönetilen örnekleri - Get](https://docs.microsoft.com/rest/api/sql/managedinstances/get)|Yönetilen örnek alır.|
|[Liste Örnekleri - yönetilen](https://docs.microsoft.com/rest/api/sql/managedinstances/list)|Bir abonelikte yönetilen örneklerinin bir listesini döndürür.|
|[Yönetilen örnekler - kaynak grubuna göre listesi](https://docs.microsoft.com/rest/api/sql/managedinstances/listbyresourcegroup)|Bir kaynak grubunda yönetilen örneklerinin bir listesini döndürür.|
|[Yönetilen örnekleri - güncelleştirme](https://docs.microsoft.com/rest/api/sql/managedinstances/update)|Yönetilen örnek güncelleştirir.|

## <a name="next-steps"></a>Sonraki adımlar

- Bir SQL Server veritabanını Azure'a geçirme hakkında bilgi edinmek için [Azure SQL veritabanına geçirme](sql-database-cloud-migrate.md).
- Desteklenen özellikler hakkında bilgi edinmek için bkz. [Özellikler](sql-database-features.md).