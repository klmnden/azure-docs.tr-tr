---
title: "PowerShell İzleyici ölçek tek örnek Azure SQL veritabanı | Microsoft Docs"
description: "Tek bir Azure SQL veritabanı ölçekleme ve izlemek için azure PowerShell örnek betiği"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: f88f7aaf71ff35875a13b33230abd5536c021201
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-powershell-to-monitor-and-scale-a-single-sql-database"></a>İzleme ve tek bir SQL veritabanı ölçeklendirmek için PowerShell kullanma

Bu PowerShell Betiği örnek veritabanı performans ölçümleri izler, daha yüksek bir performans düzeyine ölçeklendirir ve performans ölçümleri birinde bir uyarı kuralı oluşturur. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=13-14 "Monitor and scale single SQL Database")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Komut dosyası örneği çalıştırdıktan sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanılabilir.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
 [Yeni-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [AzureRmSqlServer yeni](/powershell/module/azurerm.sql/new-azurermsqlserver) | Bir veritabanı veya esnek havuzu barındıran bir mantıksal sunucu oluşturur. |
| [Get-AzureRmMetric](/powershell/module/azurerm.insights/get-azurermmetric) | Veritabanı boyutu kullanım bilgilerini gösterir.|
| [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) | Veritabanı özellikleri güncelleştirir veya bir veritabanı içine, dışı veya esnek havuzlar arasında taşır. |
| [Ekleme AzureRMMetricAlertRule](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | Otomatik olarak Dtu'lar gelecekte izlemek için bir uyarı kuralı ayarlar. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek SQL veritabanı PowerShell Betiği örnekleri bulunabilir [Azure SQL veritabanı PowerShell komut dosyalarını](../sql-database-powershell-samples.md).
