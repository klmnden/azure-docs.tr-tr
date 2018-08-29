---
title: 'PowerShell Betiği: veri kopyalama toplu olarak Azure Data Factory kullanarak | Microsoft Docs'
description: Bu PowerShell Betiği, Azure Data Factory kaynak veri deposundan hedef veri deposuna toplu veri kopyalamak için nasıl kullanılacağını gösterir.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: jingwang
ms.openlocfilehash: dc1bf394a34c097caa68c029e11c141dbae32aab
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125148"
---
# <a name="powershell-script---copy-multiple-tables-in-bulk-by-using-azure-data-factory"></a>PowerShell Betiği - Azure Data Factory kullanarak birden çok tabloyu toplu olarak kopyalama

Bu örnek PowerShell Betiği bir Azure SQL veri ambarı'na veri birden çok tablodan Azure SQL veritabanına kopyalar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

Bkz: [öğretici: kopyalama toplu](../tutorial-bulk-copy.md#prerequisites) Bu örneği çalıştırmak için Önkoşullar.

## <a name="sample-script"></a>Örnek betik

> [!IMPORTANT]
> Bu betik Data Factory varlıklarını (bağlı hizmet, veri kümesi ve işlem hattı) tanımlayan JSON dosyalarını sabit sürücünüzdeki c:\ klasöründe oluşturur.

[!code-powershell[main](../../../powershell_scripts/data-factory/bulk-copy-from-sql-databse-to-sql-data-warehouse/bulk-copy-from-sql-database-to-sql-data-warehouse.ps1 "Bulk copy from Azure SQL Database => Azure SQL Data Warehouse")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Örnek betiği çalıştırdıktan sonra aşağıdaki komutu kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için kullanabilirsiniz:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```
Data factory kaynak grubundan kaldırmak için aşağıdaki komutu çalıştırın: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Set-AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2) | Veri fabrikası oluşturma. |
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | Bağlı hizmet, data factory'de oluşturur. Bağlı hizmet, bir veri deposu veya işlem bir veri fabrikasına bağlar. |
| [Set-AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | Bir veri kümesi data factory oluşturur. Bir veri kümesi için bir işlem hattındaki bir etkinliğin giriş/çıkış temsil eder. | 
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2pipeline) | Veri fabrikasında bir işlem hattı oluşturur. Bir işlem hattı, belirli bir işlem gerçekleştiren bir veya daha fazla etkinlik içerir. Bu işlem hattında kopyalama etkinliği veri bir konumdan Azure Blob depolama alanındaki başka bir konuma kopyalar. |
| [Invoke-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipeline) | İşlem hattının çalıştırma oluşturur. Diğer bir deyişle, işlem hattını çalışır. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | İşlem hattında (etkinlik çalıştırma) etkinlik çalıştırması ayrıntılarını alır. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Data Factory PowerShell Betiği örnekleri, içinde bulunabilir [Azure Data Factory PowerShell betikleri](../samples-powershell.md).
