---
title: "Azure kaynakları için birden çok kaynak gruplarını dağıtmak | Microsoft Docs"
description: "Birden fazla Azure kaynak grubu dağıtımı sırasında hedef gösterilmektedir."
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
ms.date: 06/15/2017
ms.author: tomfitz
ms.openlocfilehash: d8b041213b269775175a810e585103d3c538557f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-azure-resources-to-more-than-one-resource-group"></a>Azure kaynakları birden fazla kaynak grubuna Dağıt

Genellikle, tüm kaynakları tek bir kaynak grubu için şablonunuzdaki dağıtın. Ancak, bir kaynak kümesi birlikte dağıtmasını ancak bunları farklı kaynak gruplarında yerleştirmek istediğiniz senaryolar vardır. Örneğin, yedekleme sanal makineyi Azure Site Recovery için ayrı kaynak grubunu ve konumu için dağıtmak isteyebilirsiniz. Resource Manager üst şablonu için kullanılan kaynak grubundan farklı kaynak grupları hedeflemek için iç içe geçmiş şablonları kullanmanıza olanak sağlar.

Kaynak grubu, uygulama ve onun kaynaklar topluluğu için yaşam döngüsü kapsayıcıdır. Şablon dışında kaynak grubu oluşturun ve dağıtım sırasında hedef kaynak grubu belirtin. Kaynak grupları giriş için bkz: [Azure Resource Manager'a genel bakış](resource-group-overview.md).

## <a name="example-template"></a>Örnek şablon

Farklı bir kaynak hedeflemek için dağıtım sırasında bir iç içe ya da bağlantılı şablonu kullanmanız gerekir. `Microsoft.Resources/deployments` Kaynak türü sağlayan bir `resourceGroup` farklı bir kaynak grubu için iç içe geçmiş dağıtım belirtmenize olanak tanıyan parametresi. Tüm kaynak grupları, dağıtım çalıştırılmadan önce mevcut olması gerekir. Aşağıdaki örnekte iki depolama hesabı - bir dağıtım sırasında belirtilen kaynak grubunda dağıtır ve diğeri adlı bir kaynak grubunda `crossResourceGroupDeployment`:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "StorageAccountName1": {
            "type": "string"
        },
        "StorageAccountName2": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "crossResourceGroupDeployment",
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
                            "name": "[parameters('StorageAccountName2')]",
                            "apiVersion": "2015-06-15",
                            "location": "West US",
                            "properties": {
                                "accountType": "Standard_LRS"
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('StorageAccountName1')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
                "accountType": "Standard_LRS"
            }
        }
    ]
}
```

Ayarlarsanız `resourceGroup` var olmayan bir kaynak grubu adı için dağıtım başarısız olur. İçin bir değer belirtmezseniz, `resourceGroup`, Resource Manager üst kaynak grubu kullanır.  

## <a name="deploy-the-template"></a>Şablonu dağıtma

Örnek şablonu dağıtmak için portal, Azure PowerShell veya Azure CLI kullanabilirsiniz. Azure PowerShell veya Azure CLI için May 2017 veya sonraki bir sürümü kullanmanız gerekir. Örnekler, kaydettiğiniz şablon yerel olarak adlı bir dosya varsayar **crossrgdeployment.json**.

PowerShell için:

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

Azure CLI için:

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

Dağıtım tamamlandıktan sonra iki kaynak grubu bakın. Her kaynak grubu bir depolama hesabını içerir.

## <a name="use-resourcegroup-function"></a>Kullanım resourceGroup() işlevi

Kaynak grubu dağıtımı arası için [resouceGroup() işlevi](resource-group-template-functions-resource.md#resourcegroup) çözümler farklı dayalı iç içe geçmiş şablonu nasıl belirtin. 

Başka bir şablonu içindeki bir şablon ekleme, iç içe geçmiş şablonunda resouceGroup() üst kaynak grubuna çözümler. Katıştırılmış bir şablonu aşağıdaki biçimi kullanır:

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

Ayrı bir şablon bağlantı varsa, bağlantılı şablonunda resouceGroup() iç içe kaynak grubuna çözümler. Bağlantılı bir şablon aşağıdaki biçimi kullanır:

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

## <a name="next-steps"></a>Sonraki adımlar

* Şablonunuzda parametrelerini tanımlamak nasıl anlamak için bkz: [yapısı ve Azure Resource Manager şablonları sözdizimini anlamanız](resource-group-authoring-templates.md).
* Genel dağıtım hatalarını giderme ipuçları için bkz: [ortak Azure dağıtım hataları Azure Resource Manager ile ilgili sorunları giderme](resource-manager-common-deployment-errors.md).
* Bir SAS belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [dağıtma özel şablonu SAS belirteci ile](resource-manager-powershell-sas-token.md).
