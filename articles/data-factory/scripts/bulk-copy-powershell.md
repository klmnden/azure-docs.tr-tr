---
title: "PowerShell Betiği: veri kopyalama toplu olarak Azure Data Factory kullanarak | Microsoft Docs"
description: "Bu PowerShell betik hedef veri deposuna toplu bir kaynak veri deposundan verileri kopyalamak için Azure Data Factory kullanmayı gösterir."
services: data-factory
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: spelluru
ms.openlocfilehash: c8044d70a7c793d865884223081433e49f014f7b
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="powershell-script---copy-multiple-tables-in-bulk-by-using-azure-data-factory"></a>PowerShell komut dosyası - Azure Data Factory kullanarak birden çok tablo toplu olarak Kopyala

Bu örnek PowerShell komut dosyasını verileri bir Azure SQL veri ambarı için bir Azure SQL veritabanındaki birden çok tablodan kopyalar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

Bkz: [Öğreticisi: toplu kopyalama](../tutorial-bulk-copy.md#prerequisites) Bu örneği çalıştırmak için Önkoşullar.

## <a name="sample-script"></a>Örnek komut dosyası

> [!IMPORTANT]
> Bu komut dosyası c:\ klasöründe sabit diskinizde Data Factory varlıkları (bağlı hizmet, veri kümesi ve ardışık düzeni) tanımlayan JSON dosyaları oluşturur.

[!code-powershell[main](../../../powershell_scripts/data-factory/bulk-copy-from-sql-databse-to-sql-data-warehouse/bulk-copy-from-sql-database-to-sql-data-warehouse.ps1 "Bulk copy from Azure SQL Database => Azure SQL Data Warehouse")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Örnek betiği çalıştırma sonra aşağıdaki komutu kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için kullanabilirsiniz:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```
Veri Fabrikası kaynak grubundan kaldırmak için aşağıdaki komutu çalıştırın: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Yeni-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [Set-AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2) | Veri fabrikası oluşturma. |
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | Veri fabrikasında bağlı hizmet oluşturur. Bağlı hizmet veri deposunda veya işlem bir data factory'ye bağlar. |
| [Set-AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | Bir veri kümesi veri fabrikasında oluşturur. Bir veri kümesi bir ardışık düzeninde bir etkinlik için giriş/çıkış temsil eder. | 
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactorv2ypipeline) | Data factory işlem hattı oluşturur. Bir işlem hattı belirli bir işlemi gerçekleştiren bir veya daha fazla etkinlik içerir. Bu ardışık düzeninde kopyalama etkinliği verileri tek bir konumdan bir Azure Blob Depolama başka bir konuma kopyalar. |
| [Çağırma AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipelinerun) | Ardışık düzeni için bir farklı çalıştır oluşturur. Diğer bir deyişle, ardışık düzen çalışır. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | (Etkinlik) etkinlik çalışma ayrıntıları ardışık düzeninde alır. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](https://docs.microsoft.com/powershell/).

Ek Azure veri fabrikası PowerShell komut dosyası örnekleri bulunabilir [Azure veri fabrikası PowerShell komut dosyalarını](../samples-powershell.md).