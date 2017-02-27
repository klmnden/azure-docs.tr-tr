---
title: "Azure’da kaynak dağıtma | Microsoft Belgeleri"
description: "Azure PowerShell veya Azure CLI kullanarak Azure&quot;da kaynak dağıtın. Kaynaklar, bir Resource Manager şablonunda tanımlanır."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 41d9476d4ce6b7afc2ba9c757e96db7e8e72d5ae
ms.openlocfilehash: a77354d0719240e5916b5ec945dad75e534802d3


---
# <a name="deploy-resources-to-azure"></a>Azure’da kaynak dağıtma

Bu konu başlığında, Azure aboneliğinize nasıl kaynak dağıtılacağı gösterilmektedir. Azure PowerShell veya Azure CLI kullanarak çözümünüze ilişkin altyapıyı tanımlayan bir Resource Manager şablonu dağıtabilirsiniz.

Resource Manager kavramlarına giriş için bkz. [Azure Resource Manager'a genel bakış](resource-group-overview.md).

## <a name="steps-for-deployment"></a>Dağıtım adımları

Bu konu başlığında, aşağıdaki [örnek depolama şablonunu](#example-storage-template) dağıttığınız varsayılmaktadır. Farklı bir şablon kullanabilirsiniz, ancak geçirdiğiniz parametreler bu konu başlığında gösterilen değerlerden farklı olacaktır.

Şablon oluşturulduktan sonra şablonu dağıtmaya yönelik genel adımlar şunlardır:

1. Hesabınızda oturum açma
2. Kullanılacak aboneliği seçin (Yalnızca birden çok aboneliğiniz varsa ve varsayılan abonelik dışında birini kullanmak istiyorsanız gerekir)
3. Kaynak grubu oluşturma
4. Şablonu dağıtma
5. Dağıtım durumunuzu denetleme

Aşağıdaki bölümlerde, bu adımların [PowerShell](#powershell) veya [Azure CLI](#azure-cli) ile nasıl gerçekleştirileceği gösterilmektedir.

## <a name="powershell"></a>PowerShell

1. Azure PowerShell'i yüklemek için bkz. [Azure PowerShell cmdlet'lerini kullanmaya başlama](/powershell/azureps-cmdlets-docs).

2. Dağıtımı hızla gerçekleştirmek için aşağıdaki cmdlet'leri kullanın:

  ```powershell
  Login-AzureRmAccount
  Set-AzureRmContext -SubscriptionID {subscription-id}

  New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json 
  ```

  `Set-AzureRmContext` cmdlet’i, yalnızca varsayılan aboneliğinizden farklı bir abonelik kullanmak istiyorsanız gerekir. Tüm aboneliklerinizi ve kimliklerini görmek için şunu kullanın:

  ```powershell
  Get-AzureRmSubscription
  ```

3. Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında aşağıdakine benzer bir ileti görürsünüz:

  ```powershell
  DeploymentName          : ExampleDeployment
  CorrelationId           : 07b3b233-8367-4a0b-b9bc-7aa189065665
  ResourceGroupName       : ExampleGroup
  ProvisioningState       : Succeeded
  ...
  ```

4. Kaynak grubunuz ve depolama hesabınızın aboneliğinize dağıtıldığını denetlemek için aşağıdakini kullanın:

  ```powershell
  Get-AzureRmResourceGroup -Name ExampleGroup
  Find-AzureRmResource -ResourceGroupNameEquals ExampleGroup
  ```

5. Şablon dağıtırken, şablon parametrelerini PowerShell parametreleri olarak belirtebilirsiniz. Önceki örnekte hiçbir şablon parametresi olmadığından şablondaki varsayılan değerler kullanıldı. Başka bir depolama hesabı dağıtıp depolama adı ön eki ve depolama hesabı SKU’su için parametre değerleri sağlamak üzere aşağıdakini kullanın:

  ```powershell
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment2 -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json -storageNamePrefix "contoso" -storageSKU "Standard_GRS"
  ```

  Artık kaynak grubunuzda iki depolama hesabı bulunmaktadır. 

## <a name="azure-cli"></a>Azure CLI

1. Azure CLI’yı yüklemek için bkz. [Azure CLI 2.0’ı yükleme](/cli/azure/install-az-cli2).

2. Dağıtımı hızla gerçekleştirmek için aşağıdaki komutları kullanın:

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create --name ExampleDeployment --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json
  ```

  `az account set` komutu, yalnızca varsayılan aboneliğinizden farklı bir abonelik kullanmak istiyorsanız gerekir. Tüm aboneliklerinizi ve kimliklerini görmek için şunu kullanın:

  ```azurecli
  az account list
  ```

3. Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında aşağıdakine benzer bir ileti görürsünüz:

  ```azurecli
  "provisioningState": "Succeeded",
  ```

4. Kaynak grubunuz ve depolama hesabınızın aboneliğinize dağıtıldığını denetlemek için aşağıdakini kullanın:

  ```azurecli
  az group show --name ExampleGroup
  az resource list --resource-group ExampleGroup
  ```

5. Şablon dağıtırken, şablon parametrelerini PowerShell parametreleri olarak belirtebilirsiniz. Önceki örnekte hiçbir şablon parametresi olmadığından şablondaki varsayılan değerler kullanıldı. Başka bir depolama hesabı dağıtıp depolama adı ön eki ve depolama hesabı SKU’su için parametre değerleri sağlamak üzere aşağıdakini kullanın:

  ```azurecli
  az group deployment create --name ExampleDeployment2 --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  Artık kaynak grubunuzda iki depolama hesabı bulunmaktadır. 

## <a name="example-storage-template"></a>Örnek depolama şablonu

Aboneliğinize bir depolama hesabı dağıtmak için aşağıdaki örnek şablonu kullanın:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    },
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "The type of replication to use for the storage account."
      }
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell ile şablon dağıtımı hakkında ayrıntılı bilgi edinmek için bkz. [Resource Manager şablonları ve Azure PowerShell ile kaynak dağıtma](/azure/azure-resource-manager/resource-group-template-deploy).
* Azure CLI ile şablon dağıtımı hakkında ayrıntılı bilgi edinmek için bkz. [Resource Manager şablonları ve Azure CLI ile kaynak dağıtma](/azure/azure-resource-manager/resource-group-template-deploy-cli).






<!--HONumber=Feb17_HO3-->


