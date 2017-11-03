---
title: "Esnek veritabanı işleri Aracı kaldırma"
description: "PowerShell Azure Portalı'nı kullanarak esnek veritabanı işleri bileşenleri de kaldırmayı öğrenin."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: bfc9d820-edbd-4fca-bfbf-1f339cfcc448
ms.service: sql-database
ms.workload: Inactive
ms.custom: scale out apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 5e665ee8cc9efacbd31111dc0458ad6096e457c0
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="uninstall-elastic-database-jobs-components"></a>Esnek veritabanı işleri bileşenlerini Kaldır
**Esnek veritabanı iş** bileşenleri Azure portal veya PowerShell kullanarak kaldırılması.

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a>Azure portalını kullanarak esnek veritabanı işleri bileşenlerini Kaldır
1. [Azure portalı](https://portal.azure.com/) açın.
2. İçeren abonelik gidin **esnek veritabanı işleri** bileşenleri, yani hangi esnek veritabanı işleri bileşeni yüklenmedi abonelik.
3. Tıklatın **Gözat** tıklatıp **kaynak grupları**.
4. "__ElasticDatabaseJob" adlı kaynak grubunu seçin.
5. Kaynak grubunu silin.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>PowerShell kullanarak esnek veritabanı işleri bileşenlerini Kaldır
1. Microsoft Azure PowerShell komut penceresini başlatın ve Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x klasörü altındaki Araçlar alt dizinine gidin: türü **cd Araçları**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > cd araçları
2. .\UninstallElasticDatabaseJobs.ps1 PowerShell betiğini yürütün.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools > engellemesini dosya.\UninstallElasticDatabaseJobs.ps1 PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools >.\UninstallElasticDatabaseJobs.ps1

Veya yalnızca varsayılan varsayılarak aşağıdaki komut dosyası yürütme bileşenleri yüklemesinde kullanılan burada değerler:

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
Esnek veritabanı işleri yeniden yüklemek için bkz: [esnek veritabanı iş hizmetini yükleme](sql-database-elastic-jobs-service-installation.md)

Esnek veritabanı işleri genel bakış için bkz: [esnek veritabanı işleri genel bakış](sql-database-elastic-jobs-overview.md).

<!--Image references-->


