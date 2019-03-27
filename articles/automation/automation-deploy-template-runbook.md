---
title: Azure Otomasyonu runbook'u bir Azure Resource Manager şablonu dağıtma
description: Azure Depolama'da depolanan bir runbook'tan bir Azure Resource Manager şablonu dağıtma
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
keywords: PowerShell, runbook, json, azure Otomasyonu
ms.openlocfilehash: 2008ba697665baa0e8cf73564ec31d6267425404
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58446962"
---
# <a name="deploy-an-azure-resource-manager-template-in-an-azure-automation-powershell-runbook"></a>Azure Otomasyonu PowerShell runbook’unda Azure Resource Manager şablonu dağıtma

Yazabileceğiniz bir [Azure Otomasyonu PowerShell runbook'unda](automation-first-runbook-textual-powershell.md) olup kullanarak bir Azure kaynağı dağıtan bir [Azure kaynak yönetimi şablonu](../azure-resource-manager/resource-manager-create-first-template.md).

Bunu yaparak, Azure kaynaklarının dağıtımını otomatik hale getirebilirsiniz. Resource Manager şablonlarınızı Azure depolama gibi Merkezi, güvenli bir konumda saklayabilirsiniz.

Bu makalede, depolanan bir Resource Manager şablonunu kullanan bir PowerShell runbook'u oluştururuz [Azure depolama](../storage/common/storage-introduction.md) yeni bir Azure depolama hesabı dağıtmak için.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz yoksa, şunları yapabilirsiniz [MSDN abone Avantajlarınızı etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/).
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-sec-configure-azure-runas-account.md).  Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* [Azure depolama hesabı](../storage/common/storage-create-storage-account.md) depolanacağı Resource Manager şablonu
* Azure Powershell, yerel bir makinede yüklü. Bkz: [yüklemek ve Azure Powershell yapılandırma](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) Azure PowerShell edinme hakkında bilgi için.

## <a name="create-the-resource-manager-template"></a>Resource Manager şablonu oluşturma

Bu örnekte, yeni bir Azure depolama hesabı dağıtan bir Resource Manager şablonu kullanırız.

Bir metin düzenleyicisinde, aşağıdaki metni kopyalayın:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2018-02-01",
      "location": "[parameters('location')]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

Dosyayı yerel olarak kaydedin `TemplateTest.json`.

## <a name="save-the-resource-manager-template-in-azure-storage"></a>Resource Manager şablonu Azure depolama alanına kaydedin

Bir Azure depolama dosya paylaşımı oluşturma ve karşıya yüklemek için kullandığımız PowerShell artık `TemplateTest.json` dosya.
Bir dosya oluşturmak yönergeler paylaşın ve Azure portalında bir dosyayı karşıya yüklemek için bkz. [Windows üzerinde Azure dosya depolama ile çalışmaya başlama](../storage/files/storage-dotnet-how-to-use-files.md).

Yerel makinenizde PowerShell'i başlatın ve bir dosya paylaşımı oluşturmak ve Resource Manager şablonu bu dosya paylaşımına karşıya yüklemek için aşağıdaki komutları çalıştırın.

```powershell
# Login to Azure
Connect-AzureRmAccount

# Get the access key for your storage account
$key = Get-AzureRmStorageAccountKey -ResourceGroupName 'MyAzureAccount' -Name 'MyStorageAccount'

# Create an Azure Storage context using the first access key
$context = New-AzureStorageContext -StorageAccountName 'MyStorageAccount' -StorageAccountKey $key[0].value

# Create a file share named 'resource-templates' in your Azure Storage account
$fileShare = New-AzureStorageShare -Name 'resource-templates' -Context $context

# Add the TemplateTest.json file to the new file share
# "TemplatePath" is the path where you saved the TemplateTest.json file
$templateFile = 'C:\TemplatePath'
Set-AzureStorageFileContent -ShareName $fileShare.Name -Context $context -Source $templateFile
```

## <a name="create-the-powershell-runbook-script"></a>PowerShell runbook betiği oluşturma

Biz alır bir PowerShell betiği oluşturmak şimdi `TemplateTest.json` dosyası Azure depolama biriminden ve yeni bir Azure depolama hesabı oluşturmak için şablon dağıtır.

Bir metin düzenleyicisinde aşağıdaki metni yapıştırın:

```powershell
param (
    [Parameter(Mandatory=$true)]
    [string]
    $ResourceGroupName,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageAccountName,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageAccountKey,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageFileName
)



# Authenticate to Azure if running from Azure Automation
$ServicePrincipalConnection = Get-AutomationConnection -Name "AzureRunAsConnection"
Connect-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint | Write-Verbose

#Set the parameter values for the Resource Manager template
$Parameters = @{
    "storageAccountType"="Standard_LRS"
    }

# Create a new context
$Context = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

Get-AzureStorageFileContent -ShareName 'resource-templates' -Context $Context -path 'TemplateTest.json' -Destination 'C:\Temp'

$TemplateFile = Join-Path -Path 'C:\Temp' -ChildPath $StorageFileName

# Deploy the storage account
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterObject $Parameters 
``` 

Dosyayı yerel olarak kaydedin `DeployTemplate.ps1`.

## <a name="import-and-publish-the-runbook-into-your-azure-automation-account"></a>İçeri aktarma ve Azure Otomasyonu hesabınızı runbook yayımlama

Şimdi biz runbook'un Azure Otomasyon hesabınızda içeri aktarmak için PowerShell kullanın ve ardından runbook'u yayımlayamadı.
İçeri aktarma ve Azure Portalı'nda bir runbook yayımlama hakkında daha fazla bilgi için bkz. [Azure Otomasyonu runbook'ları yönetme](manage-runbooks.md).

İçeri aktarmak için `DeployTemplate.ps1` Otomasyon hesabınızda PowerShell runbook'u olarak aşağıdaki PowerShell komutlarını çalıştırın:

```powershell
# MyPath is the path where you saved DeployTemplate.ps1
# MyResourceGroup is the name of the Azure ResourceGroup that contains your Azure Automation account
# MyAutomationAccount is the name of your Automation account
$importParams = @{
    Path = 'C:\MyPath\DeployTemplate.ps1'
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Type = 'PowerShell'
}
Import-AzureRmAutomationRunbook @importParams

# Publish the runbook
$publishParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
}
Publish-AzureRmAutomationRunbook @publishParams
```

## <a name="start-the-runbook"></a>Runbook’u başlatma

Şimdi biz çağırarak runbook'u başlatmak [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook) cmdlet'i.

Azure Portalı'nda bir runbook başlatma hakkında daha fazla bilgi için bkz: [Azure Automation'da bir runbook başlatma](automation-starting-a-runbook.md).

PowerShell konsolunda aşağıdaki komutları çalıştırın:

```powershell
# Set up the parameters for the runbook
$runbookParams = @{
    ResourceGroupName = 'MyResourceGroup'
    StorageAccountName = 'MyStorageAccount'
    StorageAccountKey = $key[0].Value # We got this key earlier
    StorageFileName = 'TemplateTest.json' 
}

# Set up parameters for the Start-AzureRmAutomationRunbook cmdlet
$startParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
    Parameters = $runbookParams
}

# Start the runbook
$job = Start-AzureRmAutomationRunbook @startParams
```

Runbook'u çalıştırır, ve çalıştırarak durumunu denetleyebilirsiniz `$job.Status`.

Runbook, Resource Manager şablonunu alır ve yeni bir Azure depolama hesabı dağıtmak için kullanır.
Yeni depolama hesabı aşağıdaki komutu çalıştırarak oluşturulduğunu görebilirsiniz:
```powershell
Get-AzureRmStorageAccount
```

## <a name="summary"></a>Özet

İşte bu kadar! Artık tüm Azure kaynaklarınızı dağıtmak için Azure Otomasyonu ve Azure depolama ve Resource Manager şablonlarını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Resource Manager şablonları hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md)
* Azure depolama ile çalışmaya başlamak için bkz. [Azure Storage'a giriş](../storage/common/storage-introduction.md).
* Diğer kullanışlı Azure Otomasyonu runbook'ları bulmak için bkz: [Azure Otomasyonu Runbook ve modül galerileri](automation-runbook-gallery.md).
* Diğer kullanışlı bir Resource Manager şablonları bulmak için bkz: [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/)


