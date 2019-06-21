---
title: Birden çok abonelik ve kaynak grubu için Azure kaynaklarını dağıtın | Microsoft Docs
description: Birden fazla Azure aboneliği ve kaynak grubu dağıtım sırasında hedef işlemi gösterilmektedir.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/02/2018
ms.author: tomfitz
ms.openlocfilehash: 33b0a998206b68f1807f5bfa3c3f39164798842c
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205481"
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>Birden fazla abonelik veya kaynak grubu için Azure kaynaklarını dağıtın

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Tüm kaynakları tek bir şablonunuzda dağıtmak genellikle [kaynak grubu](resource-group-overview.md). Ancak, bir kaynak kümesini birlikte dağıtmak ancak farklı kaynak gruplarında ya da abonelik yerleştirmek istediğiniz senaryolar da vardır. Örneğin, ayrı bir kaynak grubunu ve konumu için Azure Site Recovery için yedekleme sanal makineyi dağıtmak isteyebilirsiniz. Resource Manager, farklı hedef abonelikler ve kaynak gruplarını abonelik ve kaynak grubu üst şablon için kullanılan iç içe şablonlara kullanmanıza olanak sağlar.

> [!NOTE]
> Tek bir dağıtımda yalnızca beş kaynak gruplarına dağıtabilirsiniz. Genellikle, bu sınırlama, iç içe veya bağlı dağıtımlarda dört kaynak grupları ve ana şablon için belirtilen bir kaynak grubu için dağıtabileceğiniz anlamına gelir. Bununla birlikte, üst şablonunuzu yalnızca iç içe veya bağlı şablon içeren ve kendisi tüm kaynakları dağıtma yapar, sonra en fazla beş kaynak grupları iç içe veya bağlı dağıtımlarda ekleyebilirsiniz.

## <a name="specify-a-subscription-and-resource-group"></a>Bir abonelik ve kaynak grubu belirtin

Farklı bir kaynak hedeflemek için iç içe veya bağlı bir şablon kullanın. `Microsoft.Resources/deployments` Kaynak türü için parametreler sağlar `subscriptionId` ve `resourceGroup`. Bu özellikler iç içe dağıtım için farklı bir abonelik ve kaynak grubunda belirtmenize olanak verir. Tüm kaynak grupları, dağıtım çalıştırılmadan önce mevcut olması gerekir. Ya da abonelik kimliği veya kaynak grubu, abonelik ve kaynak grubu üst şablonundan belirtmezseniz kullanılır.

Şablonu dağıtmak için kullandığınız hesap, belirtilen abonelik kimliğini dağıtmak için izinleri olmalıdır Belirtilen abonelik farklı bir Azure Active Directory kiracısında varsa yapmanız gerekenler [başka bir dizinden Konuk kullanıcıları eklemek](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).

Farklı bir kaynak grubu ve abonelik belirtmek için kullanın:

```json
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "resourceGroup": "[parameters('secondResourceGroup')]",
        "subscriptionId": "[parameters('secondSubscriptionID')]",
        ...
    }
]
```

Aynı abonelikte, kaynak gruplarınız varsa, kaldırabilirsiniz **Subscriptionıd** değeri.

Aşağıdaki örnek iki depolama hesabı - bir dağıtım sırasında belirtilen kaynak grubu dağıtır ve bir kaynak grubunda belirtilen `secondResourceGroup` parametre:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        },
        "secondResourceGroup": {
            "type": "string"
        },
        "secondSubscriptionID": {
            "type": "string",
            "defaultValue": ""
        },
        "secondStorageLocation": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "firstStorageName": "[concat(parameters('storagePrefix'), uniqueString(resourceGroup().id))]",
        "secondStorageName": "[concat(parameters('storagePrefix'), uniqueString(parameters('secondSubscriptionID'), parameters('secondResourceGroup')))]"
    },
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "[parameters('secondResourceGroup')]",
            "subscriptionId": "[parameters('secondSubscriptionID')]",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[variables('secondStorageName')]",
                            "apiVersion": "2017-06-01",
                            "location": "[parameters('secondStorageLocation')]",
                            "sku":{
                                "name": "Standard_LRS"
                            },
                            "kind": "Storage",
                            "properties": {
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('firstStorageName')]",
            "apiVersion": "2017-06-01",
            "location": "[resourceGroup().location]",
            "sku":{
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {
            }
        }
    ]
}
```

Ayarlarsanız `resourceGroup` var olmayan bir kaynak grubu adına, dağıtım başarısız olur.

## <a name="use-the-resourcegroup-and-subscription-functions"></a>ResourceGroup() ve subscription() işlevlerini kullanma

Çapraz kaynak grubu dağıtımlarında [resourceGroup()](resource-group-template-functions-resource.md#resourcegroup) ve [subscription()](resource-group-template-functions-resource.md#subscription) işlevleri iç içe geçmiş şablon belirttiğiniz nasıl farklı göre çözün. 

Başka bir şablonu içindeki bir şablonu eklerseniz, iç içe geçmiş şablon işlevleri üst kaynak grubu ve abonelik çözümleyin. Katıştırılmış bir şablon aşağıdaki biçimdedir:

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() and subscription() refer to parent resource group/subscription
    }
}
```

Ayrı bir şablon bağlarsanız, bağlı şablonun işlevler iç içe geçmiş bir kaynak grubu ve abonelik çözümleyin. Bağlı bir şablona aşağıdaki biçimdedir:

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() and subscription() in linked template refer to linked resource group/subscription
    }
}
```

## <a name="example-templates"></a>Örnek şablonları

Aşağıdaki şablonlar birden çok kaynak grubu dağıtımlarında gösterir. Şablon dağıtımı betikleri tablodan sonra gösterilir.

|Şablon  |Açıklama  |
|---------|---------|
|[Çapraz abonelik şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crosssubscription.json) |Bir kaynak grubuna bir depolama hesabı ve bir depolama hesabı, ikinci bir kaynak grubuna dağıtır. İkinci kaynak grubu farklı bir abonelikte olduğunda abonelik kimliği için bir değer içerir. |
|[Çapraz kaynak grubu şablon özellikleri](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) |Gösterir nasıl `resourceGroup()` işlev giderir. Tüm kaynakları dağıtmaz. |

### <a name="powershell"></a>PowerShell

İki kaynak grubu iki depolama hesabı dağıtmak için PowerShell, **aynı abonelik**, kullanın:

```azurepowershell-interactive
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

New-AzResourceGroup -Name $firstRG -Location southcentralus
New-AzResourceGroup -Name $secondRG -Location eastus

New-AzResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus
```

İçin iki depolama hesabı dağıtmak için PowerShell, **iki abonelik**, kullanın:

```azurepowershell-interactive
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

$firstSub = "<first-subscription-id>"
$secondSub = "<second-subscription-id>"

Select-AzSubscription -Subscription $secondSub
New-AzResourceGroup -Name $secondRG -Location eastus

Select-AzSubscription -Subscription $firstSub
New-AzResourceGroup -Name $firstRG -Location southcentralus

New-AzResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus `
  -secondSubscriptionID $secondSub
```

PowerShell, test etmek için nasıl **kaynak grup nesnesi** ana şablon, satır içi şablon ve bağlantılı şablon kullanımı için çözümler:

```azurepowershell-interactive
New-AzResourceGroup -Name parentGroup -Location southcentralus
New-AzResourceGroup -Name inlineGroup -Location southcentralus
New-AzResourceGroup -Name linkedGroup -Location southcentralus

New-AzResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Yukarıdaki örnekte, her ikisi de **parentRG** ve **inlineRG** çözümlemek **parentGroup**. **linkedRG** çözümler **linkedGroup**. Önceki örnekte çıktı.

```powershell
 Name             Type                       Value
 ===============  =========================  ==========
 parentRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
                                               "name": "parentGroup",
                                               "location": "southcentralus",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
 inlineRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
                                               "name": "parentGroup",
                                               "location": "southcentralus",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
 linkedRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/linkedGroup",
                                               "name": "linkedGroup",
                                               "location": "southcentralus",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
```

### <a name="azure-cli"></a>Azure CLI

İki kaynak grubu iki depolama hesabı dağıtmak için Azure CLI için **aynı abonelik**, kullanın:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

az group create --name $firstRG --location southcentralus
az group create --name $secondRG --location eastus
az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=tfstorage secondResourceGroup=$secondRG secondStorageLocation=eastus
```

Dağıtmak için iki depolama hesabı için Azure CLI için **iki abonelik**, kullanın:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

firstSub="<first-subscription-id>"
secondSub="<second-subscription-id>"

az account set --subscription $secondSub
az group create --name $secondRG --location eastus

az account set --subscription $firstSub
az group create --name $firstRG --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=storage secondResourceGroup=$secondRG secondStorageLocation=eastus secondSubscriptionID=$secondSub
```

Test etmek için Azure CLI için nasıl **kaynak grup nesnesi** ana şablon, satır içi şablon ve bağlantılı şablon kullanımı için çözümler:

```azurecli-interactive
az group create --name parentGroup --location southcentralus
az group create --name inlineGroup --location southcentralus
az group create --name linkedGroup --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json 
```

Yukarıdaki örnekte, her ikisi de **parentRG** ve **inlineRG** çözümlemek **parentGroup**. **linkedRG** çözümler **linkedGroup**. Önceki örnekte çıktı.

```azurecli
...
"outputs": {
  "inlineRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
      "location": "southcentralus",
      "name": "parentGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  },
  "linkedRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/linkedGroup",
      "location": "southcentralus",
      "name": "linkedGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  },
  "parentRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
      "location": "southcentralus",
      "name": "parentGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  }
},
...
```

## <a name="next-steps"></a>Sonraki adımlar

* Şablonunuzda parametreleri tanımlayan anlamak için bkz. [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](resource-group-authoring-templates.md).
* Sık karşılaşılan dağıtım hataları çözümleme hakkında daha fazla ipucu için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](resource-manager-common-deployment-errors.md).
* Bir SAS belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [SAS belirteci ile özel şablonu Dağıt](resource-manager-powershell-sas-token.md).
