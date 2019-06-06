---
title: PowerShell örneği - Azure SQL veritabanı'nda yönetilen örnek oluşturma | Microsoft Docs
description: Azure PowerShell örnek betiği Azure SQL veritabanı yönetilen örnek oluşturma
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop-msft
ms.reviewer: sstein
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 2d5ecb3035aad657485916a4c472f6f4dc9eb530
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729146"
---
# <a name="use-powershell-with-azure-resource-manager-template-to-create-a-managed-instance-in-azure-sql-database"></a>Azure SQL veritabanı'nda bir yönetilen örnek oluşturmak için Azure Resource Manager şablonu ile PowerShell kullanma

Azure SQL veritabanı yönetilen örneği, Azure PowerShell kitaplığını ve Azure Resource Manager şablonları kullanılarak oluşturulabilir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici AZ PowerShell 1.4.0 gerektirir veya üzeri. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

Azure PowerShell komutları, önceden tanımlanmış Azure Resource Manager şablonu kullanarak dağıtım başlayabilirsiniz. Şablonda aşağıdaki özellikler belirtilebilir:

- Örnek adı
- SQL yönetici kullanıcı adı ve parola.
- (Çekirdek sayısını ve en yüksek depolama boyutu) örnek boyutu.
- VNet ve alt ağ örneği nereye yerleştirileceğini.
- Sunucu düzeyinde harmanlama örneği (Önizleme).

Örnek adı, SQL yönetici kullanıcı adı, VNet/alt ağ ve harmanlama daha sonra değiştirilemez. Diğer örnek özellikleri değiştirilebilir.

## <a name="prerequisites"></a>Önkoşullar

Bu örnek, sahibi olduğunuzu varsayar [geçerli ağ ortamını oluşturan](../sql-database-managed-instance-create-vnet-subnet.md) veya [mevcut bir VNet değiştiren](../sql-database-managed-instance-configure-vnet-subnet.md) yönetilen Örneğiniz için. Örnek cmdlet'ler kullanır [yeni AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment) ve [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork) bu nedenle, aşağıdaki PowerShell modüllerine yüklendiğinden emin olun:

```powershell
Install-Module Az.Network
Install-Module Az.Resources
```

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Aşağıdaki içeriğe örneği oluşturmak için kullanılacak şablonu temsil eder bir dosyada yerleştirilmelidir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
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

Düzgün bir şekilde yapılandırılmış alt ağ ile Azure sanal ağı zaten varsayılır. Düzgün bir şekilde yapılandırılmış bir alt ağ yoksa ayrı kullanarak ağ ortamını hazırlama [Azure yönetilen kaynak şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment) , bağımsız olarak yürütülen veya bu şablona dahil.

Bu dosyanın içeriği .json dosyası olarak kaydedin, aşağıdaki PowerShell Betiği dosya yoluna koyun ve betiğinde nesnelerin adlarını değiştirin:

```powershell
$subscriptionId = "ed827499-xxxx-xxxx-xxxx-xxxxxxxxxx"
Select-AzSubscription -SubscriptionId $subscriptionId

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
$vNet = Get-AzVirtualNetwork -Name $vNetName -ResourceGroupName $vNetResourceGroup
$subnet = Get-AzVirtualNetworkSubnetConfig -Name $SubnetName -VirtualNetwork $vNet
$subnetId = $subnet.Id

# Deploy Instance using Azure Resource Manager template:
New-AzResourceGroupDeployment  -Name MyDeployment -ResourceGroupName $resourceGroup  `
                                    -TemplateFile 'C:\...\create-managed-instance.json' `
                                    -instance $name -user $user -pwd $secpasswd -subnetId $subnetId
```

Betik başarılı şekilde çalıştırıldıktan sonra, SQL Veritabanı tüm Azure hizmetlerinden ve yapılandırılmış IP adresinden erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
