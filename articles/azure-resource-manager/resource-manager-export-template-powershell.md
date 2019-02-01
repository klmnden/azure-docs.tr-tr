---
title: Azure PowerShell ile Resource Manager şablonunu dışarı aktarma | Microsoft Docs
description: Bir şablonu kaynak grubundan dışarı aktarmak için Azure Resource Manager ve Azure PowerShell kullanın.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/23/2018
ms.author: tomfitz
ms.openlocfilehash: 0313266c9e9bf7814d4581dc04d70cf80e1f8172
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55494723"
---
# <a name="export-azure-resource-manager-templates-with-powershell"></a>PowerShell ile Azure Resource Manager şablonlarını dışarı aktarma

Resource Manager, aboneliğinizde var olan kaynaklardan bir Resource Manager şablonunu dışarı aktarmanızı sağlar. Bu oluşturulan şablonu şablon söz dizimi hakkında bilgi edinmek veya çözümünüzün yeniden dağıtımını gerektiği gibi otomatikleştirmek için kullanabilirsiniz.

Bir şablonu dışarı aktarmak için iki farklı şekilde olduğuna dikkat edin önemlidir:

* Dışarı aktarabilirsiniz **bir dağıtım için kullanılan gerçek şablonu**. Dışarı aktarılan şablonda, tüm parametreler ve değişkenler özgün şablondaki gibidir. Bu yaklaşım, bir şablon almak, ihtiyacınız olduğunda yararlıdır.
* **Kaynak grubunun geçerli durumunu temsil eden, oluşturulmuş bir şablonu** dışarı aktarabilirsiniz. Dışarı aktarılan şablon, dağıtım için kullandığınız herhangi bir şablonu temel almaz. Bunun yerine "snapshot" veya "yedek" kaynak grubu olan bir şablon oluşturur. Dışarı aktarılan şablon birçok sabit kodlu değer ve büyük olasılıkla normalde tanımlayacağınızdan daha az sayıda parametre içerir. Kaynaklar aynı kaynak grubuna yeniden dağıtmak için bu seçeneği kullanın. Başka bir kaynak grubu için bu şablonu kullanmak için önemli ölçüde değiştirmeniz gerekebilir.

Bu makalede her iki yaklaşım gösterilmektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deploy-a-solution"></a>Bir çözüm dağıtma

Bir şablonu dışarı aktarma için her iki yaklaşım göstermek için Aboneliğinize bir çözüm dağıtarak başlayalım. Dışarı aktarmak istediğiniz aboneliğinizde bir kaynak grubu zaten varsa, bu çözümü dağıtmak gerekmez. Ancak, bu makalenin geri kalanında bu çözüm için şablonu ifade eder. Örnek betik, bir depolama hesabı dağıtır.

```powershell
New-AzResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -DeploymentName NewStorage
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```  

## <a name="save-template-from-deployment-history"></a>Dağıtım geçmişinden şablonu kaydetme

Dağıtım geçmişinden bir şablonu kullanarak alabilirsiniz [Kaydet AzureRmResourceGroupDeploymentTemplate](/powershell/module/az.resources/save-azresourcegroupdeploymenttemplate) komutu. Aşağıdaki örnek, daha önce dağıttığınız şablon kaydeder:

```powershell
Save-AzResourceGroupDeploymentTemplate -ResourceGroupName ExampleGroup -DeploymentName NewStorage
```

Şablon konumunu döndürür.

```powershell
Path
----
C:\Users\exampleuser\NewStorage.json
```

Dosyasını açın ve dağıtım için kullanılan tam şablon olduğuna dikkat edin. Parametreler ve değişkenler, github'dan şablon eşleştirin. Bu şablonu yeniden dağıtabilirsiniz.

## <a name="export-resource-group-as-template"></a>Kaynak grubunu şablon olarak dışarı aktarma

Dağıtım geçmişinden bir şablonu almak yerine kullanarak bir kaynak grubunun geçerli durumunu temsil eden bir şablon elde edebilirsiniz [dışarı aktarma-AzureRmResourceGroup](/powershell/module/az.resources/export-azresourcegroup) komutu. Kaynak grubunuzun birçok değişiklik yaptığınızda ve varolan bir şablonu tüm değişiklikleri temsil eder. Bu komutu kullanın. Aynı kaynak grubuna yeniden dağıtmak için kullanabileceğiniz kaynak grubunun bir anlık görüntü olarak tasarlanmıştır. Diğer çözümler için dışarı aktarılan şablon kullanmak için önemli ölçüde değiştirmeniz gerekir.

```powershell
Export-AzResourceGroup -ResourceGroupName ExampleGroup
```

Şablon konumunu döndürür.

```powershell
Path
----
C:\Users\exampleuser\ExampleGroup.json
```

Dosyasını açın ve GitHub şablonda farklı olduğuna dikkat edin. Bu, farklı parametreler ve değişken vardır. Konum ve depolama SKU'su değerlere sabit kodlanmış. Dışarı aktarılan şablon aşağıdaki örnekte, ancak biraz farklı parametre adı, şablonunuzu vardır:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccounts_nf3mvst4nqb36standardsa_name": {
      "defaultValue": null,
      "type": "String"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[parameters('storageAccounts_nf3mvst4nqb36standardsa_name')]",
      "apiVersion": "2016-01-01",
      "location": "southcentralus",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

Bu şablonu yeniden dağıtabilirsiniz, ancak tahmin depolama hesabı için benzersiz bir ad gerektirir. Parametre adını biraz farklıdır.

```powershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -TemplateFile C:\Users\exampleuser\ExampleGroup.json `
  -storageAccounts_nf3mvst4nqb36standardsa_name tfnewstorage0501
```

## <a name="customize-exported-template"></a>Dışarı aktarılan şablonu özelleştirme

Bu şablon kullanmayı daha kolay ve daha esnek hale getirmek için değiştirebilirsiniz. Daha fazla konumları için konum özelliği kaynak grubu olarak aynı konumu kullanmak üzere değiştirin:

```json
"location": "[resourceGroup().location]",
```

Depolama hesabı için bir uniques ad düşünmeniz zorunda kalmamak için depolama hesabı adı parametresi kaldırın. Depolama adı son eki ve depolama SKU'su için bir parametre ekleyin:

```json
"parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
},
```

Depolama hesabı adı ile uniqueString işlevi oluşturan bir değişken ekleyin:

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

Depolama hesabının adını bu değişkene ayarlayın:

```json
"name": "[variables('storageAccountName')]",
```

Parametre kümesi SKU'su:

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

Şablonunuz şimdi şuna benzer görünmelidir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), parameters('storageSuffix'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "[parameters('storageAccountType')]",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

Değiştirilen şablonu yeniden dağıtın.

## <a name="next-steps"></a>Sonraki adımlar
* Bir şablonu dışarı aktarmak için portalı kullanma hakkında daha fazla bilgi için bkz: [mevcut kaynaklardan Azure Resource Manager şablonunu dışarı aktarma](resource-manager-export-template.md).
* Şablonda parametreleri tanımlamak için bkz: [şablonları yazma](resource-group-authoring-templates.md#parameters).
* Sık karşılaşılan dağıtım hataları çözümleme hakkında daha fazla ipucu için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](resource-manager-common-deployment-errors.md).
