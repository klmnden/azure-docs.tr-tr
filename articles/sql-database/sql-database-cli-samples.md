---
title: SQL Veritabanı için Azure CLI betik örnekleri | Microsoft Docs
description: Azure SQL Veritabanı sunucuları, elastik havuzlar, veritabanları ve güvenlik duvarları oluşturmak ve yönetmek için Azure CLI betik örnekleri.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples, mvc
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 02/03/2019
ms.openlocfilehash: 7a1132b5857cf6c54d0566ca29bb76ce1ef88513
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66150233"
---
# <a name="azure-cli-samples-for-azure-sql-database"></a>Azure SQL Veritabanı için Azure CLI örnekleri

Azure SQL veritabanı kullanarak yapılandırılabilir <a href="/cli/azure">Azure CLI</a>.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="single-database--elastic-pools"></a>Tek veritabanı ve elastik havuzlar

Aşağıdaki tablo, Azure SQL Veritabanı için Azure CLI betik örneklerinin bağlantılarını içerir.

| |  |
|---|---|
|**Tek bir veritabanı ve elastik havuz oluşturma**||
| [Tek bir veritabanı oluşturma ve güvenlik duvarı kuralını yapılandırma](scripts/sql-database-create-and-configure-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Bu CLI betik örneği, tek bir Azure SQL veritabanı oluşturur ve sunucu düzeyinde güvenlik duvarı kuralı yapılandırır. |
| [Elastik havuzlar oluşturma ve havuza alınan veritabanlarını taşıma](scripts/sql-database-move-database-between-pools-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Bu CLI betik örneği, SQL elastik havuzları oluşturur, havuza alınmış Azure SQL veritabanlarını taşır ve işlem boyutlarını değiştirir.|
|**Tek bir veritabanını ve elastik havuzu ölçekleme**||
| [Tek bir veritabanını ölçekleme](scripts/sql-database-monitor-and-scale-database-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Bu CLI betiği örneği, veritabanının boyut bilgilerini sorguladıktan sonra tek bir Azure SQL veritabanı örneğini farklı bir işlem boyutuna ölçeklendirir. |
| [Elastik havuzu ölçekleme](scripts/sql-database-scale-pool-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Bu CLI betik örneği, SQL elastik havuzunu farklı bir işlem boyutuna ölçekler.  |
|||

Daha fazla bilgi edinin [tek veritabanı Azure CLI API](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases).

## <a name="managed-instance"></a>Yönetilen Örnek

Aşağıdaki tabloda, Azure SQL veritabanı - yönetilen örnek için Azure CLI betik örneklerinin bağlantılarını içerir.

| |  |
|---|---|
| [Yönetilen Örnek oluşturma](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../create-azure-sql-managed-instance-using-azure-cli/) | Bu CLI betiği, bir yönetilen örneğin nasıl oluşturulacağını gösterir. |
| [Yönetilen örnek güncelleştirme](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../modify-azure-sql-database-managed-instance-using-azure-cli/) | Bu CLI betik, yönetilen bir örneği gösterilmektedir. |
| [Bir veritabanını başka bir yönetilen örneğine taşıyabilirsiniz](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/) | Bu CLI betiği, bir örnekten bir veritabanının bir yedeğini geri yükleme gösterir. |
|||

Daha fazla bilgi edinin [yönetilen örnek Azure CLI API](sql-database-managed-instance-create-manage.md#azure-cli-create-and-manage-managed-instances) ve bulma [ek örnekler](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44).
