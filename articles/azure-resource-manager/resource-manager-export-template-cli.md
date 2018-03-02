---
title: "Azure CLI ile Resource Manager şablonu aktarma | Microsoft Docs"
description: "Bir şablonu kaynak grubundan dışarı aktarmak için Azure Resource Manager ve Azure CLI kullanın."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/23/2018
ms.author: tomfitz
ms.openlocfilehash: 15e7e811c7cb1777e34f1bfb629fa24a60f9e5cb
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="export-azure-resource-manager-templates-with-azure-cli"></a>Azure CLI ile Azure Resource Manager şablonları dışarı aktarma

Resource Manager, aboneliğinizde var olan kaynaklardan bir Resource Manager şablonunu dışarı aktarmanızı sağlar. Bu oluşturulan şablonu şablon söz dizimi hakkında bilgi edinmek veya çözümünüzün yeniden dağıtımını gerektiği gibi otomatikleştirmek için kullanabilirsiniz.

Bir şablonu dışarı aktarmak için iki farklı yolları da olduğunu dikkate almak önemlidir:

* Dışa aktarabilirsiniz **bir dağıtım için kullanılan gerçek şablonu**. Dışarı aktarılan şablonda, tüm parametreler ve değişkenler özgün şablondaki gibidir. Bir şablon almanız gerektiğinde, bu yararlı bir yaklaşımdır.
* **Kaynak grubunun geçerli durumunu temsil eden, oluşturulmuş bir şablonu** dışarı aktarabilirsiniz. Dışarı aktarılan şablon, dağıtım için kullandığınız herhangi bir şablonu temel almaz. Bunun yerine, bir "anlık görüntüsü" veya "yedek" kaynak grubunun olan bir şablon oluşturur. Dışarı aktarılan şablon birçok sabit kodlu değer ve büyük olasılıkla normalde tanımlayacağınızdan daha az sayıda parametre içerir. Kaynaklar aynı kaynak grubuna yeniden dağıtmak için bu seçeneği kullanın. Başka bir kaynak grubu için bu şablonu kullanmak için önemli ölçüde değiştirmeniz gerekebilir.

Bu makalede her iki yaklaşımın gösterilmektedir.

## <a name="deploy-a-solution"></a>Bir çözüm dağıtma

Bir şablonu dışarı aktarmak için her iki yaklaşımın göstermek için Aboneliğinize bir çözümü başlayalım. Aboneliğinizdeki vermek istediğiniz bir kaynak grubu zaten varsa, bu çözümü dağıtmak gerekmez. Ancak, bu makalenin geri kalanında bu çözüm için şablonu ifade eder. Örnek komut dosyasını bir depolama hesabı dağıtır.

```azurecli
az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name NewStorage \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
```  

## <a name="save-template-from-deployment-history"></a>Şablonu dağıtım geçmişinden Kaydet

Kullanarak bir şablonu dağıtım geçmişinden alabilirsiniz [az grubu dağıtım verme](/cli/azure/group/deployment#az_group_deployment_export) komutu. Aşağıdaki örnek, daha önce dağıttığınız şablonu kaydeder:

```azurecli
az group deployment export --name NewStorage --resource-group ExampleGroup
```

Şablon döndürür. JSON kopyalayın ve bir dosya olarak kaydedin. Dağıtım için kullanılan tam şablonu olduğuna dikkat edin. Parametreler ve değişkenler github'dan şablon eşleşmesi. Bu şablonu yeniden dağıtabilirsiniz.


## <a name="export-resource-group-as-template"></a>Kaynak grubunu şablon olarak dışarı aktarma

Bir şablonu dağıtım geçmişinden almanın yerine kullanarak bir kaynak grubunun geçerli durumunu temsil eden bir şablonu alabilir [az grup verme](/cli/azure/group#az_group_export) komutu. Kaynak grubu için çok sayıda değişiklik yaptınız ve tüm değişiklikleri olan bir şablonunu temsil ettiğinde bu komutu kullanın. Aynı kaynak grubuna dağıtmak için kullanabileceğiniz kaynak grubunun anlık görüntü olarak tasarlanmıştır. Diğer çözümleri için dışarı aktarılan şablonu kullanmak için önemli ölçüde değiştirmeniz gerekir.

```azurecli
az group export --name ExampleGroup
```

Şablon döndürür. JSON kopyalayın ve bir dosya olarak kaydedin. GitHub şablonunda farklı olduğuna dikkat edin. Şablon farklı parametreler ve değişken vardır. Depolama SKU ve konum değerleri sabit kodlanmış. Aşağıdaki örnek, dışarı aktarılan şablon gösterir, ancak şablonunuzu biraz farklı parametre adı vardır:

```json
{
  "parameters": {
    "storageAccounts_mcyzaljiv7qncstandardsa_name": {
      "type": "String",
      "defaultValue": null
    }
  },
  "variables": {},
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "resources": [
    {
      "tags": {},
      "dependsOn": [],
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "type": "Microsoft.Storage/storageAccounts",
      "location": "centralus",
      "name": "[parameters('storageAccounts_mcyzaljiv7qncstandardsa_name')]",
      "properties": {}
    }
  ],
  "contentVersion": "1.0.0.0"
}
```

Bu şablonu yeniden dağıtabilirsiniz, ancak depolama hesabı için benzersiz bir ad tahmin gerektirir. Parametrenizin adını biraz farklıdır.

```azurecli
az group deployment create --name NewStorage --resource-group ExampleGroup \
  --template-file examplegroup.json \
  --parameters "{\"storageAccounts_mcyzaljiv7qncstandardsa_name\":{\"value\":\"tfstore0501\"}}"
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