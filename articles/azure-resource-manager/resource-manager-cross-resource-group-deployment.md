---
title: "Birden çok aboneliğe ve kaynak gruplarına Azure kaynaklarını dağıtma | Microsoft Docs"
description: "Birden fazla Azure abonelik ve kaynak grubu dağıtımı sırasında hedef gösterilmektedir."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/18/2017
ms.author: tomfitz
ms.openlocfilehash: 48ba938db992ce192d8afb51365d87fba4422590
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>Birden fazla abonelik veya kaynak grubu için Azure kaynaklarını dağıtma

Genellikle, tüm kaynakları tek şablonunuzdaki dağıttığınız [kaynak grubu](resource-group-overview.md). Ancak, bir kaynak kümesi birlikte dağıtmasını ancak bunları farklı kaynak grupları ya da abonelik yerleştirmek istediğiniz senaryolar vardır. Örneğin, yedekleme sanal makineyi Azure Site Recovery için ayrı kaynak grubunu ve konumu için dağıtmak isteyebilirsiniz. Resource Manager hedef farklı Abonelikleriniz ve kaynak gruplarınız daha abonelik ve kaynak grubu üst şablon için kullanılan iç içe geçmiş şablonlarını kullanmanıza olanak sağlar.

## <a name="specify-a-subscription-and-resource-group"></a>Bir abonelik ve kaynak grubu belirtin

Farklı bir kaynak hedeflemek için iç içe ya da bağlantılı bir şablon kullanın. `Microsoft.Resources/deployments` Kaynak türü için parametreler sağlar `subscriptionId` ve `resourceGroup`. Bu özellikler, iç içe geçmiş dağıtım için farklı bir abonelik ve kaynak grubu belirtmenize olanak verir. Tüm kaynak grupları, dağıtım çalıştırılmadan önce mevcut olması gerekir. Ya da abonelik Kimliğine veya kaynak grubu, abonelik ve kaynak grubu üst şablondan belirtmezseniz kullanılır.

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

Kaynak gruplarınızı aynı abonelikte olması durumunda, kaldırabilirsiniz **Subscriptionıd** değeri.

Aşağıdaki örnekte iki depolama hesabı - bir dağıtım sırasında belirtilen kaynak grubunda dağıtır ve bir kaynak grubunda belirtilen `secondResourceGroup` parametresi:

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

Ayarlarsanız `resourceGroup` var olmayan bir kaynak grubu adı için dağıtım başarısız olur.

Örnek şablonu dağıtmak için Azure PowerShell 4.0.0 veya üstü ya da Azure CLI 2.0.0 kullanın veya sonraki bir sürümü.

## <a name="use-the-resourcegroup-function"></a>Kullanım resourceGroup() işlevi

Kaynak grubu dağıtımı arası için [resourceGroup() işlevi](resource-group-template-functions-resource.md#resourcegroup) çözümler farklı dayalı iç içe geçmiş şablonu nasıl belirtin. 

Başka bir şablonu içindeki bir şablon ekleme, iç içe geçmiş şablonunda resourceGroup() üst kaynak grubuna çözümler. Katıştırılmış bir şablonu aşağıdaki biçimi kullanır:

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers to parent resource group
    }
}
```

Ayrı bir şablon bağlantı varsa, bağlantılı şablonunda resourceGroup() iç içe kaynak grubuna çözümler. Bağlantılı bir şablon aşağıdaki biçimi kullanır:

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers to linked resource group
    }
}
```

## <a name="example-templates"></a>Örnek şablonları

Aşağıdaki şablonlardan birden çok kaynak grubu dağıtımı göstermektedir. Şablonları dağıtmak üzere komut dosyaları tablodan sonra gösterilir.

|Şablon  |Açıklama  |
|---------|---------|
|[Abonelik şablon arası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crosssubscription.json) |Bir kaynak grubu için bir depolama hesabı ve bir depolama hesabı için ikinci bir kaynak grubu dağıtır. İkinci kaynak grubu farklı bir abonelikte olduğunda abonelik kimliği için bir değer içerir. |
|[Kaynak grubu özellikleri şablonu arası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) |Gösteren nasıl `resourceGroup()` işlev giderir. Herhangi bir kaynağa dağıtmaz. |

### <a name="powershell"></a>PowerShell

İki depolama hesabı iki kaynak gruplarını dağıtmak için PowerShell **aynı abonelik**, kullanın:

```powershell
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

New-AzureRmResourceGroup -Name $firstRG -Location southcentralus
New-AzureRmResourceGroup -Name $secondRG -Location eastus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus
```

İki depolama hesaplarına dağıtmak için PowerShell **iki abonelikleri**, kullanın:

```powershell
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

$firstSub = "<first-subscription-id>"
$secondSub = "<second-subscription-id>"

Select-AzureRmSubscription -Subscription $secondSub
New-AzureRmResourceGroup -Name $secondRG -Location eastus

Select-AzureRmSubscription -Subscription $firstSub
New-AzureRmResourceGroup -Name $firstRG -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus `
  -secondSubscriptionID $secondSub
```

Test etmek için PowerShell nasıl **kaynak grup nesnesi** üst şablonu, satır içi şablon ve bağlantılı şablonu kullanımı için çözümler:

```powershell
New-AzureRmResourceGroup -Name parentGroup -Location southcentralus
New-AzureRmResourceGroup -Name inlineGroup -Location southcentralus
New-AzureRmResourceGroup -Name linkedGroup -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

### <a name="azure-cli"></a>Azure CLI

İki depolama hesabı iki kaynak gruplarını dağıtmak için Azure CLI için **aynı abonelik**, kullanın:

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

İki depolama hesaplarına dağıtmak için Azure CLI için **iki abonelikleri**, kullanın:

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

Test etmek için Azure CLI için nasıl **kaynak grup nesnesi** üst şablonu, satır içi şablon ve bağlantılı şablonu kullanımı için çözümler:

```azurecli-interactive
az group create --name parentGroup --location southcentralus
az group create --name inlineGroup --location southcentralus
az group create --name linkedGroup --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json 
```

## <a name="next-steps"></a>Sonraki adımlar

* Şablonunuzda parametrelerini tanımlamak nasıl anlamak için bkz: [yapısı ve Azure Resource Manager şablonları sözdizimini anlamanız](resource-group-authoring-templates.md).
* Genel dağıtım hatalarını giderme ipuçları için bkz: [ortak Azure dağıtım hataları Azure Resource Manager ile ilgili sorunları giderme](resource-manager-common-deployment-errors.md).
* Bir SAS belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [dağıtma özel şablonu SAS belirteci ile](resource-manager-powershell-sas-token.md).
