---
title: Bir çalışma alanı oluşturmak için bir Azure Resource Manager şablonu kullanma
titleSuffix: Azure Machine Learning service
description: Yeni bir Azure Machine Learning hizmeti çalışma alanı oluşturmak için bir Azure Resource Manager şablonu kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 04/02/2019
ms.custom: seoapril2019
ms.openlocfilehash: 7349998325e56d5ebb78de5ca30c0127f09102aa
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58883199"
---
# <a name="use-an-azure-resource-manager-template-to-create-a-workspace-for-azure-machine-learning-service"></a>Azure Machine Learning hizmeti için bir çalışma alanı oluşturmak için bir Azure Resource Manager şablonu kullanma

Bu makalede, Azure Resource Manager şablonlarını kullanarak bir Azure Machine Learning hizmeti çalışma alanı oluşturmak için birkaç yol öğreneceksiniz. Resource Manager şablonu tek ve eşgüdümlü bir işlemle kaynaklarını oluşturmayı kolaylaştırır. Bir dağıtımı için gerekli kaynakları tanımlayan bir JSON belgesi şablonudur. Ayrıca, dağıtım parametreleri de belirtebilirsiniz. Parametreler, şablon kullanırken Giriş değerlerini sağlamak için kullanılır.

Daha fazla bilgi için [Azure Resource Manager şablonu ile uygulama dağıtma](../../azure-resource-manager/resource-group-template-deploy.md).

## <a name="prerequisites"></a>Önkoşullar

* Bir **Azure aboneliği**. Biri yoksa deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree).

* Bir şablondan bir CLI kullanmak için ya da ihtiyacınız [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.2.0) veya [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="resource-manager-template"></a>Resource Manager şablonu

Aşağıdaki Resource Manager şablonu, bir Azure Machine Learning hizmeti çalışma alanında ve ilişkili Azure kaynaklarını oluşturmak için kullanılabilir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "The name of the Azure Machine Learning service workspace."
            }
        },
        "location": {
            "type": "string",
            "allowedValues": [
                "eastus",
                "eastus2",
                "southcentralus",
                "southeastasia",
                "westcentralus",
                "westeurope",
                "westus2"
            ],
            "metadata": {
                "description": "The location where the workspace will be created."
            }
        }
    },
    "variables": {
        "storageAccount": {
            "name": "[concat('sa',uniqueString(resourceGroup().id))]",
            "type": "Standard_LRS"
        },
        "keyVault": {
            "name": "[concat('kv',uniqueString(resourceGroup().id))]",
            "tenantId": "[subscription().tenantId]"
        },
        "applicationInsightsName": "[concat('ai',uniqueString(resourceGroup().id))]",
        "containerRegistryName": "[concat('cr',uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "name": "[variables('storageAccount').name]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2018-07-01",
            "sku": {
                "name": "[variables('storageAccount').type]"
            },
            "kind": "StorageV2",
            "properties": {
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        },
                        "file": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                },
                "supportHttpsTrafficOnly": true
            }
        },
        {
            "name": "[variables('keyVault').name]",
            "type": "Microsoft.KeyVault/vaults",
            "apiVersion": "2018-02-14",
            "location": "[parameters('location')]",
            "properties": {
                "tenantId": "[variables('keyVault').tenantId]",
                "sku": {
                    "name": "standard",
                    "family": "A"
                },
                "accessPolicies": []
            }
        },
        {
            "name": "[variables('applicationInsightsName')]",
            "type": "Microsoft.Insights/components",
            "apiVersion": "2015-05-01",
            "location": "[if(or(equals(parameters('location'),'eastus2'),equals(parameters('location'),'westcentralus')),'southcentralus',parameters('location'))]",
            "kind": "web",
            "properties": {
                "Application_Type": "web"
            }
        },
        {
            "name": "[variables('containerRegistryName')]",
            "type": "Microsoft.ContainerRegistry/registries",
            "apiVersion": "2017-10-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "Standard"
            },
            "properties": {
                "adminUserEnabled": true
            }
        },
        {
            "name": "[parameters('workspaceName')]",
            "type": "Microsoft.MachineLearningServices/workspaces",
            "apiVersion": "2018-11-19",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[variables('storageAccount').name]",
                "[variables('keyVault').name]",
                "[variables('applicationInsightsName')]",
                "[variables('containerRegistryName')]"
            ],
            "identity": {
                "type": "systemAssigned"
            },
            "properties": {
                "friendlyName": "[parameters('workspaceName')]",
                "keyVault": "[resourceId('Microsoft.KeyVault/vaults',variables('keyVault').name)]",
                "applicationInsights": "[resourceId('Microsoft.Insights/components',variables('applicationInsightsName'))]",
                "containerRegistry": "[resourceId('Microsoft.ContainerRegistry/registries',variables('containerRegistryName'))]",
                "storageAccount": "[resourceId('Microsoft.Storage/storageAccounts/',variables('storageAccount').name)]"
            }
        }
    ]
}
```

Bu şablon, aşağıdaki Azure Hizmetleri oluşturur:

* Azure Kaynak Grubu
* Azure Depolama Hesabı
* Azure Key Vault
* Azure Application Insights
* Azure Container Registry
* Azure Machine Learning çalışma alanı

Kaynak grubu Hizmetleri tutan kapsayıcıdır. Çeşitli hizmetler, Azure Machine Learning çalışma alanı tarafından gereklidir.

Örnek şablon iki parametreye sahiptir:

* **Konumu** Hizmetleri ve kaynak grubunun oluşturulacağı.

    Şablon, seçtiğiniz konum için en fazla kaynak kullanır. Özel durum tüm diğer hizmetler konumlar kullanılamıyor Application Insights hizmetidir. Hizmet, nerede kullanılabilir değil bir konum seçin, Orta Güney ABD konumunda oluşturulur.

* **Çalışma alanı adı**, Azure Machine Learning çalışma alanının kolay adı.

    Diğer hizmetlerinin adları rastgele oluşturulur.

Şablonlar hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Resource Manager Şablonları yazma](../../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager şablonları ile uygulama dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)
* [Microsoft.MachineLearningServices kaynak türleri](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

1. Bağlantısındaki [özel şablondan kaynakları dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template). Adresindeki geldiğinde __şablonu Düzen__ ekranında, bu belge şablonundan yapıştırın.
1. Seçin __Kaydet__ şablonu kullanmak için. Aşağıdaki bilgileri sağlayın ve listelenen hüküm ve koşulları kabul ediyorum:

   * Abonelik: Bu kaynaklar için kullanılacak Azure aboneliğini seçin.
   * Kaynak grubu: Hizmetleri içerecek bir kaynak grubu oluşturun veya seçin.
   * Çalışma alanı adı: Oluşturulacak Azure Machine Learning çalışma alanı için kullanılacak ad. Çalışma alanı adı 3-33 karakter arasında olmalıdır. Yalnızca alfasayısal karakterler içerebilir ve '-'.
   * Konum: Kaynakları oluşturulacağı konumu seçin.

     ![Azure portalında şablon parametreleri](media/how-to-create-workspace-template/template-parameters.png)

Daha fazla bilgi için [özel şablondan kaynakları dağıtma](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Bu örnek adlı bir dosyaya kaydedilir şablonu varsayar `azuredeploy.json` geçerli dizin:

```powershell
New-AzResourceGroup -Name examplegroup -Location "East US"
new-azresourcegroupdeployment -name exampledeployment `
  -resourcegroupname examplegroup -location "East US" `
  -templatefile .\azuredeploy.json -workspaceName "exampleworkspace"
```

Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md) ve [SAS belirteci ve Azure PowerShell ile özel Resource Manager şablonu dağıtma](../../azure-resource-manager/resource-manager-powershell-sas-token.md).

## <a name="use-azure-cli"></a>Azure CLI kullanma

Bu örnek adlı bir dosyaya kaydedilir şablonu varsayar `azuredeploy.json` geçerli dizin:

```azurecli-interactive
az group create --name examplegroup --location "East US"
az group deployment create \
  --name exampledeployment \
  --resource-group examplegroup \
  --template-file azuredeploy.json \
  --parameters workspaceName=exampleworkspace
```

Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md) ve [SAS belirteci ve Azure CLI ile özel Resource Manager şablonu dağıtma](../../azure-resource-manager/resource-manager-cli-sas-token.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynakları Resource Manager şablonları ve Resource Manager REST API'si ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-rest.md).
* [Oluşturma ve Visual Studio aracılığıyla Azure kaynak grupları dağıtma](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
