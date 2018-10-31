---
title: Elastik veritabanı işleri Aracı kaldırma
description: PowerShell, Azure portalını kullanarak elastik veritabanı işleri bileşenleri de kaldırmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 06/14/2018
ms.openlocfilehash: 15313bf0d2d152a8295fb831e62452f8ad63bbeb
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242458"
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
1. Microsoft Azure PowerShell komut penceresini başlatın ve araçları Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x klasörü altında alt dizinine gidin: türü **cd Araçları**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > cd araçları
2. ' % S '.\UninstallElasticDatabaseJobs.ps1 PowerShell betiğini yürütün.
   
     PS: C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > engelini kaldırma dosya.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >. \ UninstallElasticDatabaseJobs.ps1

Veya yalnızca varsayılan varsayılarak aşağıdaki betiği yürütme bileşenleri yüklemesinde kullanılan burada değerleri:

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
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a>Sonraki adımlar
Elastik veritabanı işleri yeniden yüklemek için bkz [elastik veritabanı iş hizmetini yükleme](sql-database-elastic-jobs-service-installation.md)

Elastik veritabanı işleri genel bakış için bkz. [esnek veritabanı işlerine genel bakış](sql-database-elastic-jobs-overview.md).

<!--Image references-->


