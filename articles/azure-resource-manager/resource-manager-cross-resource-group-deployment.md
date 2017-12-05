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
ms.date: 12/01/2017
ms.author: tomfitz
ms.openlocfilehash: 763f46b9b5be7edf06ee0604bfc51a2482405b60
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>Birden fazla abonelik veya kaynak grubu için Azure kaynaklarını dağıtma

Genellikle, tüm kaynakları tek bir kaynak grubu için şablonunuzdaki dağıtın. Ancak, bir kaynak kümesi birlikte dağıtmasını ancak bunları farklı kaynak grupları ya da abonelik yerleştirmek istediğiniz senaryolar vardır. Örneğin, yedekleme sanal makineyi Azure Site Recovery için ayrı kaynak grubunu ve konumu için dağıtmak isteyebilirsiniz. Resource Manager hedef farklı Abonelikleriniz ve kaynak gruplarınız daha abonelik ve kaynak grubu üst şablon için kullanılan iç içe geçmiş şablonlarını kullanmanıza olanak sağlar.

Kaynak grubu, uygulama ve onun kaynaklar topluluğu için yaşam döngüsü kapsayıcıdır. Şablon dışında kaynak grubu oluşturun ve dağıtım sırasında hedef kaynak grubu belirtin. Kaynak grupları giriş için bkz: [Azure Resource Manager'a genel bakış](resource-group-overview.md).

## <a name="specify-a-subscription-and-resource-group"></a>Bir abonelik ve kaynak grubu belirtin

Farklı bir kaynak hedeflemek için dağıtım sırasında bir iç içe ya da bağlantılı şablonu kullanmanız gerekir. `Microsoft.Resources/deployments` Kaynak türü için parametreler sağlar `subscriptionId` ve `resourceGroup`. Bu özellikler, iç içe geçmiş dağıtım için farklı bir abonelik ve kaynak grubu belirtmenize olanak verir. Tüm kaynak grupları, dağıtım çalıştırılmadan önce mevcut olması gerekir. Ya da abonelik Kimliğine veya kaynak grubu, abonelik ve kaynak grubu üst şablondan belirtmezseniz kullanılır.

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

## <a name="deploy-the-template"></a>Şablonu dağıtma

Örnek şablonu dağıtmak için Azure PowerShell veya Azure CLI May 2017 veya sonraki bir sürümünü kullanın. Bu örnekler için [abonelik şablon çapraz](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crosssubscription.json) github'da.

### <a name="two-resource-groups-in-the-same-subscription"></a>Aynı abonelikte iki kaynak grubu

İki depolama hesabı aynı abonelikte iki kaynak grubu dağıtmak için PowerShell için kullanın:

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

İki depolama hesabı aynı abonelikte iki kaynak grubu dağıtmak için Azure CLI için şunu kullanın:

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

Dağıtım tamamlandıktan sonra iki kaynak grubu bakın. Her kaynak grubu bir depolama hesabını içerir.

### <a name="two-resource-groups-in-different-subscriptions"></a>Farklı Aboneliklerde iki kaynak grubu

İki depolama hesabı için iki abonelikleri dağıtmak için PowerShell için kullanın:

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

İki depolama hesabı için iki abonelikleri dağıtmak için Azure CLI için şunu kullanın:

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

Farklı yolları test etmek için `resourceGroup()` çözümler, dağıtma bir [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) , kaynak grubu nesne için üst şablonu, satır içi şablon ve bağlantılı şablonu döndürür. Her ikisi de aynı kaynak grubunu çözmek üst ve satır içi şablon. Bağlantılı kaynak grubuna bağlı şablon çözümler.

PowerShell için şunu kullanın:

```powershell
New-AzureRmResourceGroup -Name parentGroup -Location southcentralus
New-AzureRmResourceGroup -Name inlineGroup -Location southcentralus
New-AzureRmResourceGroup -Name linkedGroup -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Azure CLI için şunu kullanın:

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
