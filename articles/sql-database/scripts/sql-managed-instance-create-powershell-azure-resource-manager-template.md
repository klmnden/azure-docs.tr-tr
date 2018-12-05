---
title: PowerShell örneği - bir Azure SQL veritabanı yönetilen örneği oluşturma | Microsoft Docs
description: Azure SQL veritabanı Managd örneği oluşturmak için azure PowerShell örnek betiği
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop-msft
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: e2075f8a8e54a091dbb82f9ed6d1c8ddaa9da4d0
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52869855"
---
# <a name="use-powershell-with-azure-resource-manager-template-to-create-an-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği oluşturmak için Azure Resource Manager şablonu ile PowerShell kullanma

Azure SQL veritabanı yönetilen örneği, Azure PowerShell kitaplığını ve Azure Resource Manager şablonları kullanılarak oluşturulabilir. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

Azure PowerShell komutları, önceden tanımlanmış Azure Resource Manager şablonu kullanarak dağıtım başlayabilirsiniz. Şablonda aşağıdaki özellikler belirtilebilir:
- Örnek adı
- SQL yönetici kullanıcı adı ve parola. 
- (Çekirdek sayısını ve en yüksek depolama boyutu) örnek boyutu.
- VNet ve alt ağ örneği nereye yerleştirileceğini.
- Sunucu düzeyinde harmanlama örneği (Önizleme).

Örnek adı, SQL yönetici kullanıcı adı, VNet/alt ağ ve harmanlama daha sonra değiştirilemez. Diğer örnek özellikleri değiştirilebilir.

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Aşağıdaki içeriğe örneği oluşturmak için kullanılacak şablonu temsil eder bir dosyada yerleştirilmelidir:
```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "instance": {
            "type": "string"
        },
        "user": {
            "type": "string"
        },
        "pwd": {
            "type": "securestring"
        },
        "subnetId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('instance')]",
            "location": "West Central US",
            "tags": {
                "Description":"GP Instance with custom instance collation - Serbian_Cyrillic_100_CS_AS"
            },          
            "sku": {
                "name": "GP_Gen4",
                "tier": "GeneralPurpose"
            },
            "properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen4", 
                "collation": "Serbian_Cyrillic_100_CS_AS"
            },
            "type": "Microsoft.Sql/managedInstances",
            "identity": {
                "type": "SystemAssigned"
            },
            "apiVersion": "2015-05-01-preview"
        }
    ]
}
```
Düzgün bir şekilde yapılandırılmış alt ağ ile Azure sanal ağı zaten varsayılır. Bu, bir durum değilse, ayrı kullanarak ağ ortamını hazırlama [şablonu kaynak Azure yönetilen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment) , bağımsız olarak yürütülen veya bu şablona dahil.

Bu dosyanın içeriği .json dosyası olarak kaydedin, aşağıdaki PowerShell Betiği dosya yoluna koyun ve betiğinde nesnelerin adlarını değiştirin: 

```powershell
$subscriptionId = "ed827499-xxxx-xxxx-xxxx-xxxxxxxxxx"
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Managed Instance properties
$resourceGroup = "rg_mi"
$location = "West Central US"
$name = "managed-instance-name"
$user = "miSqlAdmin"
$secpasswd = ConvertTo-SecureString "<Put some strong password here>" -AsPlainText -Force

# Network configuration
$vNetName = "my_vnet"
$vNetResourceGroup = "rg_mi_vnet"
$subnetName = "ManagedInstances"
$vNet = Get-AzureRmVirtualNetwork -Name $vNetName -ResourceGroupName $vNetResourceGroup
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -VirtualNetwork $vNet
$subnetId = $subnet.Id

# Deploy Instance using Azure Resource Manager template:
New-AzureRmResourceGroupDeployment  -Name MyDeployment -ResourceGroupName $resourceGroup  `
                                    -TemplateFile 'C:\...\create-managed-instance.json' `
                                    -instance $name -user $user -pwd $secpasswd -subnetId $subnetId
```
Betik başarılı şekilde çalıştırıldıktan sonra, SQL Veritabanı tüm Azure hizmetlerinden ve yapılandırılmış IP adresinden erişilebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.



