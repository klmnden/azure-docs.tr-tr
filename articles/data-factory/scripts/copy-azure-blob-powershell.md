---
title: "PowerShell Betiği: veri kopyalama bulutta Azure Data Factory kullanarak | Microsoft Docs"
description: "Bu PowerShell Betiği verileri tek bir konumda bir Azure Blob Depolama aynı Blob depolama alanındaki başka bir konuma kopyalar."
services: data-factory
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: jingwang
ms.openlocfilehash: cb452735b2f8f855a43d12c0fd50973baa3150fd
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="use-powershell-to-create-a-data-factory-pipeline-to-copy-data-in-the-cloud"></a>Bulutta verileri kopyalamak için bir data factory işlem hattı oluşturmak için PowerShell kullanın

Bu örnek PowerShell komut dosyasını bir konumdan verileri Azure Blob Depolama başka bir konuma kopyalar Azure Data Factory işlem hattı oluşturur.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Azure Depolama hesabı**. Blob depolama alanını hem **kaynak** hem de **havuz** veri deposu olarak kullanabilirsiniz. Azure depolama hesabınız yoksa oluşturma bilgileri için bkz. [Depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account). 
* Blob Depolama içinde bir **blob kapsayıcısı** oluşturun, kapsayıcıda bir giriş **klasörü** oluşturun ve bazı dosyaları klasöre yükleyin. [Azure Depolama gezgini](https://azure.microsoft.com/features/storage-explorer/) gibi araçları kullanarak Azure Blob depolama hesabına bağlanabilir, bir blob kapsayıcısı oluşturabilir, giriş dosyasını karşıya yükleyebilir ve çıktı dosyasını doğrulayabilirsiniz.

## <a name="sample-script"></a>Örnek betik

> [!IMPORTANT]
> Bu komut dosyası c:\ klasöründe sabit diskinizde Data Factory varlıkları (bağlı hizmet, veri kümesi ve ardışık düzeni) tanımlayan JSON dosyaları oluşturur.

[!code-powershell[main](../../../powershell_scripts/data-factory/copy-from-azure-blob-to-blob/copy-from-azure-blob-to-blob.ps1 "Copy from Blob Storage -> Blob Storage")]


## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Örnek betiği çalıştırma sonra aşağıdaki komutu kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için kullanabilirsiniz:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```
Veri Fabrikası kaynak grubundan kaldırmak için aşağıdaki komutu çalıştırın: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Set-AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2) | Veri fabrikası oluşturma. |
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | Veri fabrikasında bağlı hizmet oluşturur. Bağlı hizmet veri deposunda veya işlem bir data factory'ye bağlar. |
| [Set-AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | Bir veri kümesi veri fabrikasında oluşturur. Bir veri kümesi bir ardışık düzeninde bir etkinlik için giriş/çıkış temsil eder. | 
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactorv2ypipeline) | Data factory işlem hattı oluşturur. Bir işlem hattı belirli bir işlemi gerçekleştiren bir veya daha fazla etkinlik içerir. Bu ardışık düzeninde kopyalama etkinliği verileri tek bir konumdan bir Azure Blob Depolama başka bir konuma kopyalar. |
| [Invoke-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipelinerun) | Ardışık düzeni için bir farklı çalıştır oluşturur. Diğer bir deyişle, ardışık düzen çalışır. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | (Etkinlik) etkinlik çalışma ayrıntıları ardışık düzeninde alır. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure veri fabrikası PowerShell komut dosyası örnekleri bulunabilir [Azure veri fabrikası PowerShell örnekleri](../samples-powershell.md).