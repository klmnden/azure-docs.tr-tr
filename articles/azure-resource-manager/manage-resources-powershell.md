---
title: Azure PowerShell kullanarak Azure kaynaklarını yönetmek | Microsoft Docs
description: Azure PowerShell ve Azure Resource Manager kaynaklarınızı yönetmek için kullanın.
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: 51ef6f3f8ac18b71064f73f32597c1f59ffa1d18
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59525407"
---
# <a name="manage-azure-resources-by-using-azure-powershell"></a>Azure PowerShell kullanarak Azure kaynaklarını yönetme

Azure PowerShell ile kullanma hakkında bilgi edinin [Azure Resource Manager](resource-group-overview.md) Azure kaynaklarınızı yönetmek için. Kaynak grupları yönetmek için bkz: [Azure PowerShell kullanarak yönetme Azure kaynak grupları](./manage-resource-groups-powershell.md).

Kaynakları yönetme hakkında diğer makaleler:

- [Azure portalını kullanarak Azure kaynaklarını yönetme](./manage-resources-portal.md)
- [Azure CLI kullanarak Azure kaynaklarını yönetme](./manage-resources-cli.md)

## <a name="deploy-resources-to-an-existing-resource-group"></a>Kaynakları var olan bir kaynak grubuna dağıtma

Azure PowerShell kullanarak doğrudan Azure kaynaklarını dağıtın veya Azure kaynakları oluşturmak için Resource Manager şablonu dağıtın.

### <a name="deploy-a-resource"></a>Bir kaynak dağıtma

Aşağıdaki betik, bir depolama hesabı oluşturur.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

# Create the storage account.
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroupName `
  -Name $storageAccountName `
  -Location $location `
  -SkuName "Standard_LRS"

# Retrieve the context.
$ctx = $storageAccount.Context
```

### <a name="deploy-a-template"></a>Şablon dağıtma

Aşağıdaki betik bir depolama hesabı oluşturmak için Hızlı Başlangıç şablonu dağıtın. Daha fazla bilgi için [hızlı başlangıç: Visual Studio Code kullanarak Azure Resource Manager şablonları oluşturma](./resource-manager-quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell).

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -Location $location
```

Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](./resource-group-template-deploy.md).

## <a name="deploy-a-resource-group-and-resources"></a>Bir kaynak grubu ve kaynakları dağıtma

Bir kaynak grubu oluşturun ve kaynak grubuna dağıtın. Daha fazla bilgi için [kaynak grubu oluşturun ve kaynakları dağıtma](./deploy-to-subscription.md#create-resource-group-and-deploy-resources).

## <a name="deploy-resources-to-multiple-subscriptions-or-resource-groups"></a>Birden çok abonelik veya kaynak grupları, kaynakları dağıtma

Genellikle, tüm kaynakları tek bir kaynak grubu için şablonunuzdaki dağıtın. Ancak, bir kaynak kümesini birlikte dağıtmak ancak farklı kaynak gruplarında ya da abonelik yerleştirmek istediğiniz senaryolar da vardır. Daha fazla bilgi için [birden çok abonelik veya kaynak grupları dağıtma Azure kaynaklarına](./resource-manager-cross-resource-group-deployment.md).

## <a name="delete-resources"></a>Kaynakları silme

Aşağıdaki komut dosyası, bir depolama hesabını silme işlemi gösterilmektedir.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

Remove-AzStorageAccount -ResourceGroupName $resourceGroupName -AccountName $storageAccountName
```

Azure Resource Manager kaynakların silinmesini nasıl siparişler hakkında daha fazla bilgi için bkz. [Azure Resource Manager kaynak grubu silme işlemi](./resource-group-delete.md).

## <a name="move-resources"></a>Kaynakları taşıma

Aşağıdaki betik, bir depolama hesabı, başka bir kaynak grubu için bir kaynak grubundan kaldırmak gösterilmektedir.

```azurepowershell-interactive
$srcResourceGroupName = Read-Host -Prompt "Enter the source Resource Group name"
$destResourceGroupName = Read-Host -Prompt "Enter the destination Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

$storageAccount = Get-AzResource -ResourceGroupName $srcResourceGroupName -ResourceName $storageAccountName
Move-AzResource -DestinationResourceGroupName $destResourceGroupName -ResourceId $storageAccount.ResourceId
```

Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).

## <a name="lock-resources"></a>Kaynakları kilitleme

Kilitleme yanlışlıkla Azure aboneliği, kaynak grubu ya da kaynağa gibi kritik kaynakları silmesini veya kuruluşunuzdaki diğer kullanıcılar engeller. 

Hesabı tarafından silindiği için aşağıdaki betiği bir depolama hesabı kilitler.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

New-AzResourceLock -LockName LockStorage -LockLevel CanNotDelete -ResourceGroupName $resourceGroupName -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts 
```

Aşağıdaki betik, bir depolama hesabı için tüm kilitleri alır:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

Get-AzResourceLock -ResourceGroupName $resourceGroupName -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts
```

Aşağıdaki betiği bir kilit depolama hesabını siler:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

$lockId = (Get-AzResourceLock -ResourceGroupName $resourceGroupName -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts).LockId
Remove-AzResourceLock -LockId $lockId
```

Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).

## <a name="tag-resources"></a>Kaynakları etiketleme

Kaynak grubu ve kaynakları mantıksal olarak düzenlemek etiketleme yardımcı olur. Bilgi için [etiketleri kullanarak Azure kaynaklarınızı düzenleme](./resource-group-using-tags.md#powershell).

## <a name="manage-access-to-resources"></a>Kaynaklara erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Daha fazla bilgi için [RBAC ve Azure PowerShell kullanarak erişimini yönetme](../role-based-access-control/role-assignments-powershell.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Resource Manager bilgi edinmek için [Azure Resource Manager'a genel bakış](./resource-group-overview.md).
- Resource Manager şablon söz dizimi bilgi edinmek için [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](./resource-group-authoring-templates.md).
- Şablonları geliştirme hakkında bilgi edinmek için [adım adım öğreticiler](/azure/azure-resource-manager/).
- Azure Resource Manager Şablon Şemaları görüntülemek için bkz: [şablon başvurusu](/azure/templates/).
