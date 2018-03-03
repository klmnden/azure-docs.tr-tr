---
title: "PowerShell Betiği: veri kopyalama şirket içinden Azure'a veri fabrikası kullanarak | Microsoft Docs"
description: "Bu PowerShell Betiği veri bir şirket içi SQL Server veritabanından başka bir Azure Blob Storage kopyalar."
services: data-factory
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: jingwang
ms.openlocfilehash: bf84603c587b7bee5d0f69355ff9c1375ed7e60c
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="use-powershell-to-create-a-data-factory-pipeline-to-copy-data-from-on-premises-to-azure"></a>Şirket içinden Azure'a veri kopyalamak için bir data factory işlem hattı oluşturmak için PowerShell kullanın

Bu örnek PowerShell komut dosyasını bir Azure Blob depolama alanına bir şirket içi SQL Server veritabanından veri kopyalar Azure Data Factory işlem hattı oluşturur.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Önkoşullar

- **SQL Server**. Bir şirket içi SQL Server veritabanı olarak kullandığınız bir **kaynak** verileri depolamak Bu örnekte.
- **Azure Depolama hesabı**. Azure blob depolama alanı olarak kullandığınız bir **hedef/havuz** verileri depolamak Bu örnekte. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account) makalesine bakın.
- **Tümleştirme çalışma zamanı'kendi kendini barındıran**. MSI dosyasından karşıdan [İndirme Merkezinden](https://www.microsoft.com/download/details.aspx?id=39717) ve kendi kendini barındıran tümleştirmesi çalışma zamanı makinenize yüklemek için çalıştırın.  

### <a name="create-sample-database-in-sql-server"></a>SQL Server örnek veritabanı oluşturma
1. Şirket içi SQL Server veritabanında adlı bir tablo oluşturmak **emp** aşağıdaki SQL betiğini kullanarak: 

   ```sql   
     CREATE TABLE dbo.emp
     (
         ID int IDENTITY(1,1) NOT NULL,
         FirstName varchar(50),
         LastName varchar(50),
         CONSTRAINT PK_emp PRIMARY KEY (ID)
     )
     GO
   ```

2. Bazı örnek veriler tabloya ekleyin:

   ```sql
     INSERT INTO emp VALUES ('John', 'Doe')
     INSERT INTO emp VALUES ('Jane', 'Doe')
   ```

## <a name="sample-script"></a>Örnek betik

> [!IMPORTANT]
> Bu komut dosyası c:\ klasöründe sabit diskinizde Data Factory varlıkları (bağlı hizmet, veri kümesi ve ardışık düzeni) tanımlayan JSON dosyaları oluşturur.

[!code-powershell[main](../../../powershell_scripts/data-factory/copy-from-onprem-sql-server-to-azure-blob/copy-from-onprem-sql-server-to-azure-blob.ps1 "Copy from on-premises SQL Server -> Azure Blob Storage")]


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
| [New-AzureRmDataFactoryV2LinkedServiceEncryptCredential](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactoryv2linkedserviceencryptedcredential) | Bağlı hizmet kimlik bilgilerini şifreler ve şifrelenmiş kimlik bilgileri ile yeni bir bağlantılı hizmet tanımı oluşturur. 
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