---
title: Elastik veritabanı işleri Aracı kaldırma
description: PowerShell, Azure portalını kullanarak elastik veritabanı işleri bileşenleri de kaldırmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 2131aa63f0f5cdbbc9e0b4eb7c1f57604fe75cdb
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55754963"
---
# <a name="uninstall-elastic-database-jobs-components"></a>Elastik veritabanı işleri bileşenlerini Kaldır


[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]


**Elastik veritabanı işleri** bileşenleri Azure portal veya PowerShell kullanarak kaldırılması.

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a>Azure portalını kullanarak elastik veritabanı işleri bileşenlerini Kaldır
1. [Azure portalı](https://portal.azure.com/) açın.
2. İçeren aboneliği gidin **elastik veritabanı işleri** bileşenleri, yani hangi elastik veritabanı işleri bileşeni yüklenmedi abonelik.
3. Tıklayın **Gözat** tıklatıp **kaynak grupları**.
4. "__ElasticDatabaseJob" adlı kaynak grubunu seçin.
5. Kaynak grubunu silin.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>PowerShell kullanarak elastik veritabanı işleri bileşenlerini Kaldır
1. Microsoft Azure PowerShell komut penceresini başlatın ve araçları Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x klasörü altında alt dizinine gidin: Tür **cd Araçları**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > cd araçları
2. ' % S '.\UninstallElasticDatabaseJobs.ps1 PowerShell betiğini yürütün.
   
     PS: C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > engelini kaldırma dosya.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >. \ UninstallElasticDatabaseJobs.ps1

Veya yalnızca varsayılan varsayılarak aşağıdaki betiği yürütme bileşenleri yüklemesinde kullanılan burada değerleri:

```powershell
$ResourceGroupName = "__ElasticDatabaseJob"
Switch-AzureMode AzureResourceManager

$resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
if(!$resourceGroup)
{
     Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
     return
}

Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
Remove-AzureResourceGroup -Name $ResourceGroupName -Force
Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job components are now uninstalled."
```

## <a name="next-steps"></a>Sonraki adımlar
Elastik veritabanı işleri yeniden yüklemek için bkz [elastik veritabanı iş hizmetini yükleme](sql-database-elastic-jobs-service-installation.md)

Elastik veritabanı işleri genel bakış için bkz. [esnek veritabanı işlerine genel bakış](sql-database-elastic-jobs-overview.md).
