---
title: Azure PowerShell ile Resource Manager şablonu aktarma | Microsoft Docs
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
ms.openlocfilehash: c69bab9d2956568473dd6def86ecbd9bbb6577cf
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="export-azure-resource-manager-templates-with-powershell"></a>PowerShell ile Azure Resource Manager şablonları dışarı aktarma

Resource Manager, aboneliğinizde var olan kaynaklardan bir Resource Manager şablonunu dışarı aktarmanızı sağlar. Bu oluşturulan şablonu şablon söz dizimi hakkında bilgi edinmek veya çözümünüzün yeniden dağıtımını gerektiği gibi otomatikleştirmek için kullanabilirsiniz.

Bir şablonu dışarı aktarmak için iki farklı yolları da olduğunu dikkate almak önemlidir:

* Dışa aktarabilirsiniz **bir dağıtım için kullanılan gerçek şablonu**. Dışarı aktarılan şablonda, tüm parametreler ve değişkenler özgün şablondaki gibidir. Bir şablon almanız gerektiğinde, bu yararlı bir yaklaşımdır.
* **Kaynak grubunun geçerli durumunu temsil eden, oluşturulmuş bir şablonu** dışarı aktarabilirsiniz. Dışarı aktarılan şablon, dağıtım için kullandığınız herhangi bir şablonu temel almaz. Bunun yerine, bir "anlık görüntüsü" veya "yedek" kaynak grubunun olan bir şablon oluşturur. Dışarı aktarılan şablon birçok sabit kodlu değer ve büyük olasılıkla normalde tanımlayacağınızdan daha az sayıda parametre içerir. Kaynaklar aynı kaynak grubuna yeniden dağıtmak için bu seçeneği kullanın. Başka bir kaynak grubu için bu şablonu kullanmak için önemli ölçüde değiştirmeniz gerekebilir.

Bu makalede her iki yaklaşımın gösterilmektedir.

## <a name="deploy-a-solution"></a>Bir çözüm dağıtma

Bir şablonu dışarı aktarmak için her iki yaklaşımın göstermek için Aboneliğinize bir çözümü başlayalım. Aboneliğinizdeki vermek istediğiniz bir kaynak grubu zaten varsa, bu çözümü dağıtmak gerekmez. Ancak, bu makalenin geri kalanında bu çözüm için şablonu ifade eder. Örnek komut dosyasını bir depolama hesabı dağıtır.

```powershell
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -DeploymentName NewStorage
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```  

## <a name="save-template-from-deployment-history"></a>Şablonu dağıtım geçmişinden Kaydet

Kullanarak bir şablonu dağıtım geçmişinden alabilirsiniz [Kaydet AzureRmResourceGroupDeploymentTemplate](/powershell/module/azurerm.resources/save-azurermresourcegroupdeploymenttemplate) komutu. Aşağıdaki örnek, daha önce dağıttığınız şablonu kaydeder:

```powershell
Save-AzureRmResourceGroupDeploymentTemplate -ResourceGroupName ExampleGroup -DeploymentName NewStorage
```

Şablon konumunu döndürür.

```powershell
Path
----
C:\Users\exampleuser\NewStorage.json
```

Dosyasını açın ve dağıtımı için kullanılan tam şablon olduğuna dikkat edin. Parametreler ve değişkenler github'dan şablon eşleşmesi. Bu şablonu yeniden dağıtabilirsiniz.

## <a name="export-resource-group-as-template"></a>Kaynak grubunu şablon olarak dışarı aktarma

Bir şablonu dağıtım geçmişinden almanın yerine kullanarak bir kaynak grubunun geçerli durumunu temsil eden bir şablonu alabilir [Export-AzureRmResourceGroup](/powershell/module/azurerm.resources/export-azurermresourcegroup) komutu. Kaynak grubu için çok sayıda değişiklik yaptınız ve tüm değişiklikleri olan bir şablonunu temsil ettiğinde bu komutu kullanın. Aynı kaynak grubuna dağıtmak için kullanabileceğiniz kaynak grubunun anlık görüntü olarak tasarlanmıştır. Diğer çözümleri için dışarı aktarılan şablonu kullanmak için önemli ölçüde değiştirmeniz gerekir.

```powershell
Export-AzureRmResourceGroup -ResourceGroupName ExampleGroup
```

Şablon konumunu döndürür.

```powershell
Path
----
C:\Users\exampleuser\ExampleGroup.json
```

Dosyasını açın ve şablonu github farklı olduğuna dikkat edin. Farklı parametreler ve değişken vardır. Depolama SKU ve konum değerleri sabit kodlanmış. Aşağıdaki örnek, dışarı aktarılan şablon gösterir, ancak şablonunuzu biraz farklı parametre adı vardır:

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

Bu şablonu yeniden dağıtabilirsiniz, ancak depolama hesabı için benzersiz bir ad tahmin gerektirir. Parametrenizin adını biraz farklıdır.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -TemplateFile C:\Users\exampleuser\ExampleGroup.json `
  -storageAccounts_nf3mvst4nqb36standardsa_name tfnewstorage0501
```

## <a name="customize-exported-template"></a>Dışarı aktarılan şablonu özelleştirme

Bu şablonu kullanmayı daha kolay ve daha esnek hale getirmek için değiştirebilirsiniz. Daha fazla konumları için izin vermek için aynı konumu kaynak grubu olarak kullanmak üzere konum özelliği değiştirin:

```json
"location": "[resourceGroup().location]",
```

Depolama hesabı için bir uniques ad tahmin yapmak zorunda kalmamak için depolama hesabı adı için parametresini kaldırın. Bir depolama alanı adı soneki ve depolama SKU için bir parametre ekleyin:

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

Depolama hesabı adı uniqueString işlevi ile oluşturur bir değişkeni ekleyin:

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

Depolama hesabı adı değişkeni ayarlayın:

```json
"name": "[variables('storageAccountName')]",
```

SKU parametreyi ayarlayın:

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

Değiştirilen şablon yeniden dağıtın.

## <a name="next-steps"></a>Sonraki adımlar
* Bir şablonu dışarı aktarmak için portalı kullanma hakkında daha fazla bilgi için bkz: [mevcut kaynaklardan Azure Resource Manager şablonunu dışarı aktarma](resource-manager-export-template.md).
* Şablonda parametreleri tanımlamak için bkz: [şablonları yazma](resource-group-authoring-templates.md#parameters).
* Genel dağıtım hatalarını giderme ipuçları için bkz: [ortak Azure dağıtım hataları Azure Resource Manager ile ilgili sorunları giderme](resource-manager-common-deployment-errors.md).
