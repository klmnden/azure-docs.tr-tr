---
title: Azure SQL veritabanı yönetilen örneği için yönetim API Başvurusu | Microsoft Docs
description: Oluşturma ve Azure SQL veritabanı yönetilen örnekleri yönetme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 6362084c11ce7aa9078823758700239694162765
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59359144"
---
# <a name="managed-api-reference-for-azure-sql-database-managed-instances"></a>Azure SQL veritabanı yönetilen örnekleri için yönetilen API Başvurusu

Oluşturun ve Azure SQL veritabanı yönetilen Azure portalı, PowerShell, Azure CLI, REST API ve Transact-SQL kullanarak örnekleri yönetin. Bu makalede, İşlevler ve oluşturmak ve yönetilen örneği yapılandırmak için kullanabileceğiniz bir API genel bakış bulabilirsiniz.

## <a name="azure-portal-create-a-managed-instance"></a>Azure portalı: Yönetilen örnek oluşturma

Bir Azure SQL veritabanı yönetilen örneği oluşturmayı gösteren Hızlı Başlangıç için bkz: [hızlı başlangıç: Bir Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-get-started.md).

## <a name="powershell-create-and-manage-managed-instances"></a>PowerShell: Yönetilen örnekleri oluşturma ve yönetme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

Azure PowerShell ile yönetilen örnekleri oluşturma ve yönetme hakkında bilgi için aşağıdaki PowerShell cmdlet'lerini kullanın. Gerekirse yükleyin veya PowerShell yükseltmek için bkz [Azure PowerShell modülü yükleme](/powershell/azure/install-az-ps).

> [!TIP]
> PowerShell örnek komut dosyaları için bkz: [hızlı başlangıç betiği: Azure SQL yönetilen örneği PowerShell kitaplığını kullanarak oluşturma](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../quick-start-script-create-azure-sql-managed-instance-using-powershell/).

| Cmdlet | Açıklama |
| --- | --- |
|[Yeni AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)|Bir Azure SQL veritabanı yönetilen örneği oluşturur. |
|[Get-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstance)|Azure SQL yönetilen örneği hakkında bilgi döndürür|
|[Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance)|Azure SQL veritabanı yönetilen örneği için özellikleri ayarlar|
|[Remove-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstance)|Bir Azure SQL veritabanı yönetilen örneği kaldırır|
|[New-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstancedatabase)|Bir Azure SQL veritabanı yönetilen örneği veritabanı oluşturur|
|[Get-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabase)|Azure SQL yönetilen örnek veritabanı hakkındaki bilgileri döndürür|
|[Remove-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstancedatabase)|Bir Azure SQL veritabanı örneği yönetilen veritabanı kaldırır|
|[Geri yükleme-AzSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase)|Bir Azure SQL veritabanı örneği yönetilen veritabanı geri yükler|

## <a name="azure-cli-create-and-manage-managed-instances"></a>Azure CLI: Yönetilen örnekleri oluşturma ve yönetme

İle yönetilen örnekleri oluşturma ve yönetme için [Azure CLI](/cli/azure), aşağıdaki [Azure CLI SQL yönetilen örneği](/cli/azure/sql/mi) komutları. CLI’yi tarayıcınızda çalıştırmak için [Cloud Shell](/azure/cloud-shell/overview) kullanın veya macOS, Linux ya da Windows’da [yükleyin](/cli/azure/install-azure-cli).

> [!TIP]
> Azure CLI Hızlı Başlangıç için bkz. [SQL yönetilen örneği kullanarak Azure CLI ile çalışma](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44).

| Cmdlet | Açıklama |
| --- | --- |
|[az sql mı oluşturma](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) |Yönetilen bir örneğini oluşturur|
|[az sql mı listesi](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-list)|Listeleri kullanılabilir yönetilen örnekler|
|[az sql mı Göster](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-show)|Ayrıntılı bilgi almak için bir yönetilen örnek|
|[az sql mı güncelleştirme](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)|Yönetilen örnek güncelleştirir|
|[az sql mı Sil](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-delete)|Yönetilen örnek kaldırır|
|[az sql ORTAB oluşturma](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-create) |Yönetilen bir veritabanı oluşturur.|
|[az sql ORTAB listesi](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-list)|Listeleri kullanılabilir yönetilen veritabanları|
|[az sql ORTAB geri yükleme](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-restore)|Bir yönetilen veritabanı geri yükleme|
|[az sql ORTAB Sil](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-delete)|Yönetilen bir veritabanı kaldırır|

## <a name="transact-sql-create-and-manage-instance-databases"></a>Transact-SQL: Örnek veritabanları oluşturma ve yönetme

Oluşturup örnek veritabanı yönetilen örneği oluşturulduktan sonra yönetmek için aşağıdaki T-SQL komutlarını kullanın. Azure portalını kullanarak şu komutları verebilirsiniz [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Azure veri Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is). [Visual Studio Code](https://code.visualstudio.com/docs), veya bir Azure SQL Database sunucusuna bağlanma ve Transact-SQL komutlarını geçirmek başka bir program.

> [!TIP]
> İçin yapılandırmak ve Microsoft Windows üzerinde SQL Server Management Studio kullanarak yönetilen örneğe bağlanmak zorunda gösteren hızlı başlangıçlara bakın [hızlı başlangıç: Bir Azure SQL veritabanı yönetilen örneğine bağlanmak için Azure VM yapılandırma](sql-database-managed-instance-configure-vm.md) ve [hızlı başlangıç: Noktadan siteye bağlantı, şirket içinden Azure SQL veritabanı yönetilen örneği için yapılandırma](sql-database-managed-instance-configure-p2s.md).
> [!IMPORTANT]
> Oluşturamaz veya Transact-SQL kullanarak bir yönetilen örneğini silin.

| Komut | Açıklama |
| --- | --- |
|[VERİTABANI OLUŞTUR](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current)|Yeni bir yönetilen örnek veritabanı oluşturur. Yeni bir veritabanı oluşturmak için ana veritabanına bağlı olmanız gerekir.|
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) |Bir Azure SQL yönetilen örnek veritabanını değiştirir.|

## <a name="rest-api-create-and-manage-managed-instances"></a>REST API: Yönetilen örnekleri oluşturma ve yönetme

Yönetilen örnekleri oluşturma ve yönetme için bu REST API istekleri'ni kullanın.

| Komut | Açıklama |
| --- | --- |
|[Yönetilen örnekler - oluştur veya güncelleştir](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)|Oluşturur veya yönetilen bir örneğini güncelleştirir.|
|[Yönetilen örnekleri - sil](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)|Yönetilen örnek siler.|
|[Yönetilen örnekleri - Get](https://docs.microsoft.com/rest/api/sql/managedinstances/get)|Yönetilen örnek alır.|
|[Liste Örnekleri - yönetilen](https://docs.microsoft.com/rest/api/sql/managedinstances/list)|Bir abonelikte yönetilen örneklerinin bir listesini döndürür.|
|[Yönetilen örnekler - kaynak grubuna göre listesi](https://docs.microsoft.com/rest/api/sql/managedinstances/listbyresourcegroup)|Bir kaynak grubunda yönetilen örneklerinin bir listesini döndürür.|
|[Yönetilen örnekleri - güncelleştirme](https://docs.microsoft.com/rest/api/sql/managedinstances/update)|Yönetilen örnek güncelleştirir.|

## <a name="next-steps"></a>Sonraki adımlar

- Bir SQL Server veritabanını Azure'a geçirme hakkında bilgi edinmek için [Azure SQL veritabanına geçirme](sql-database-single-database-migrate.md).
- Desteklenen özellikler hakkında bilgi edinmek için bkz. [Özellikler](sql-database-features.md).
