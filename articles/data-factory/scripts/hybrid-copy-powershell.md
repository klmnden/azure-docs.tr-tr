---
title: "PowerShell Betiği: Data Factory kullanarak verileri şirket içinden Azure'a kopyalama | Microsoft Docs"
description: Bu PowerShell Betiği veri bir şirket içi SQL Server veritabanındaki verileri başka bir Azure Blob Depolama kopyalar.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2017
ms.author: jingwang
ms.openlocfilehash: 7027812a61f9a2577f7cb2c778e574a3a7aaa55b
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54023983"
---
# <a name="use-powershell-to-create-a-data-factory-pipeline-to-copy-data-from-on-premises-to-azure"></a>Verileri şirket içinden Azure'a kopyalamak için bir veri fabrikası işlem hattı oluşturmak için PowerShell kullanma

Bu örnek PowerShell Betiği, Azure Data factory'de bir şirket içi SQL Server veritabanından Azure Blob Depolama'ya veri kopyalayan bir işlem hattı oluşturur.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Önkoşullar

- **SQL Server**. Bir şirket içi SQL Server veritabanı olarak kullandığınız bir **kaynak** Bu örnekte veri deposu.
- **Azure Depolama hesabı**. Azure blob depolama alanı olarak kullandığınız bir **hedef/havuz** Bu örnekte veri deposu. Azure depolama hesabınız yoksa, oluşturma adımları için [Depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md) makalesine bakın.
- **Barındırılan tümleştirme çalışma zamanını**. MSI dosyasını indirme [İndirme Merkezinden](https://www.microsoft.com/download/details.aspx?id=39717) ve şirket içinde barındırılan tümleştirme çalışma zamanını makinenize yüklemek için çalıştırın.  

### <a name="create-sample-database-in-sql-server"></a>SQL Server örnek veritabanını oluşturma
1. Şirket içi SQL Server veritabanında adlı bir tablo oluşturun **emp** aşağıdaki SQL betiğini kullanarak: 

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

2. Tabloya bazı örnek veriler ekleyin:

   ```sql
     INSERT INTO emp VALUES ('John', 'Doe')
     INSERT INTO emp VALUES ('Jane', 'Doe')
   ```

## <a name="sample-script"></a>Örnek betik

> [!IMPORTANT]
> Bu betik Data Factory varlıklarını (bağlı hizmet, veri kümesi ve işlem hattı) tanımlayan JSON dosyalarını sabit sürücünüzdeki c:\ klasöründe oluşturur.

[!code-powershell[main](../../../powershell_scripts/data-factory/copy-from-onprem-sql-server-to-azure-blob/copy-from-onprem-sql-server-to-azure-blob.ps1 "Copy from on-premises SQL Server -> Azure Blob Storage")]


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
| [Yeni-AzureRmDataFactoryV2LinkedServiceEncryptCredential](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactoryv2linkedserviceencryptedcredential) | Bağlı hizmet kimlik bilgilerini şifreler ve şifrelenmiş kimlik bilgileri ile yeni bir bağlı hizmet tanımı oluşturur. 
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | Bağlı hizmet, data factory'de oluşturur. Bağlı hizmet, bir veri deposu veya işlem bir veri fabrikasına bağlar. |
| [Set-AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | Bir veri kümesi data factory oluşturur. Bir veri kümesi için bir işlem hattındaki bir etkinliğin giriş/çıkış temsil eder. | 
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2pipeline) | Veri fabrikasında bir işlem hattı oluşturur. Bir işlem hattı, belirli bir işlem gerçekleştiren bir veya daha fazla etkinlik içerir. Bu işlem hattında kopyalama etkinliği veri bir konumdan Azure Blob depolama alanındaki başka bir konuma kopyalar. |
| [Invoke-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipeline) | İşlem hattının çalıştırma oluşturur. Diğer bir deyişle, işlem hattını çalışır. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | İşlem hattında (etkinlik çalıştırma) etkinlik çalıştırması ayrıntılarını alır. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Data Factory PowerShell Betiği örnekleri, içinde bulunabilir [Azure Data Factory PowerShell örnekleri](../samples-powershell.md).
