---
title: Azure CLI kullanarak Azure kaynaklarını yönetmek | Microsoft Docs
description: Azure CLI ve Azure Resource Manager kaynaklarınızı yönetmek için kullanın.
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
ms.openlocfilehash: 076c57f5415a4f6f19252fb5a3546e5e9a8a23f4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60550258"
---
# <a name="manage-azure-resources-by-using-azure-cli"></a>Azure CLI kullanarak Azure kaynaklarını yönetme

Azure CLI ile kullanmayı öğrenin [Azure Resource Manager](resource-group-overview.md) Azure kaynaklarınızı yönetmek için. Kaynak grupları yönetmek için bkz: [Azure CLI kullanarak yönetme Azure kaynak grupları](./manage-resource-groups-cli.md).

Kaynakları yönetme hakkında diğer makaleler:

- [Azure portalını kullanarak Azure kaynaklarını yönetme](./manage-resources-portal.md)
- [Azure PowerShell kullanarak Azure kaynaklarını yönetme](./manage-resources-powershell.md)

## <a name="deploy-resources-to-an-existing-resource-group"></a>Kaynakları var olan bir kaynak grubuna dağıtma

Azure PowerShell kullanarak doğrudan Azure kaynaklarını dağıtın veya Azure kaynakları oluşturmak için Resource Manager şablonu dağıtın.

### <a name="deploy-a-resource"></a>Bir kaynak dağıtma

Aşağıdaki betik, bir depolama hesabı oluşturur.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az storage account create --resource-group $resourceGroupName --name $storageAccountName --location $location --sku Standard_LRS --kind StorageV2 &&
az storage account show --resource-group $resourceGroupName --name $storageAccountName 
```

### <a name="deploy-a-template"></a>Şablon dağıtma

Aşağıdaki betik bir depolama hesabı oluşturmak için Hızlı Başlangıç şablonu dağıtın. Daha fazla bilgi için [hızlı başlangıç: Visual Studio Code kullanarak Azure Resource Manager şablonları oluşturma](./resource-manager-quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell).

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group deployment create --resource-group $resourceGroupName --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](./resource-group-template-deploy-cli.md).

## <a name="deploy-a-resource-group-and-resources"></a>Bir kaynak grubu ve kaynakları dağıtma

Bir kaynak grubu oluşturun ve kaynak grubuna dağıtın. Daha fazla bilgi için [kaynak grubu oluşturun ve kaynakları dağıtma](./deploy-to-subscription.md#create-resource-group-and-deploy-resources).

## <a name="deploy-resources-to-multiple-subscriptions-or-resource-groups"></a>Birden çok abonelik veya kaynak grupları, kaynakları dağıtma

Genellikle, tüm kaynakları tek bir kaynak grubu için şablonunuzdaki dağıtın. Ancak, bir kaynak kümesini birlikte dağıtmak ancak farklı kaynak gruplarında ya da abonelik yerleştirmek istediğiniz senaryolar da vardır. Daha fazla bilgi için [birden çok abonelik veya kaynak grupları dağıtma Azure kaynaklarına](./resource-manager-cross-resource-group-deployment.md).

## <a name="delete-resources"></a>Kaynakları silme

Aşağıdaki komut dosyası, bir depolama hesabını silme işlemi gösterilmektedir.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az storage account delete --resource-group $resourceGroupName --name $storageAccountName 
```

Azure Resource Manager kaynakların silinmesini nasıl siparişler hakkında daha fazla bilgi için bkz. [Azure Resource Manager kaynak grubu silme işlemi](./resource-group-delete.md).

## <a name="move-resources"></a>Kaynakları taşıma

Aşağıdaki betik, bir depolama hesabı, başka bir kaynak grubu için bir kaynak grubundan kaldırmak gösterilmektedir.

```azurecli-interactive
echo "Enter the source Resource Group name:" &&
read srcResourceGroupName &&
echo "Enter the destination Resource Group name:" &&
read destResourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
storageAccount=$(az resource show --resource-group $srcResourceGroupName --name $storageAccountName --resource-type Microsoft.Storage/storageAccounts --query id --output tsv) &&
az resource move --destination-group $destResourceGroupName --ids $storageAccount
```

Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](resource-group-move-resources.md).

## <a name="lock-resources"></a>Kaynakları kilitleme

Kilitleme yanlışlıkla Azure aboneliği, kaynak grubu ya da kaynağa gibi kritik kaynakları silmesini veya kuruluşunuzdaki diğer kullanıcılar engeller. 

Hesabı tarafından silindiği için aşağıdaki betiği bir depolama hesabı kilitler.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az lock create --name LockSite --lock-type CanNotDelete --resource-group $resourceGroupName --resource-name $storageAccountName --resource-type Microsoft.Storage/storageAccounts 
```

Aşağıdaki betik, bir depolama hesabı için tüm kilitleri alır:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az lock list --resource-group $resourceGroupName --resource-name $storageAccountName --resource-type Microsoft.Storage/storageAccounts --parent ""
```

Aşağıdaki betiği bir kilit depolama hesabını siler:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
lockId=$(az lock show --name LockSite --resource-group $resourceGroupName --resource-type Microsoft.Storage/storageAccounts --resource-name $storageAccountName --output tsv --query id)&&
az lock delete --ids $lockId
```

Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).

## <a name="tag-resources"></a>Kaynakları etiketleme

Kaynak grubu ve kaynakları mantıksal olarak düzenlemek etiketleme yardımcı olur. Bilgi için [etiketleri kullanarak Azure kaynaklarınızı düzenleme](./resource-group-using-tags.md#azure-cli).

## <a name="manage-access-to-resources"></a>Kaynaklara erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md), Azure'daki kaynaklara erişimi yönetmek için kullanılan sistemdir. Daha fazla bilgi için [RBAC ve Azure CLI kullanarak erişimini yönetme](../role-based-access-control/role-assignments-cli.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Resource Manager bilgi edinmek için [Azure Resource Manager'a genel bakış](./resource-group-overview.md).
- Resource Manager şablon söz dizimi bilgi edinmek için [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](./resource-group-authoring-templates.md).
- Şablonları geliştirme hakkında bilgi edinmek için [adım adım öğreticiler](/azure/azure-resource-manager/).
- Azure Resource Manager Şablon Şemaları görüntülemek için bkz: [şablon başvurusu](/azure/templates/).
