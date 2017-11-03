---
title: "Şablon Azure kaynak konumda | Microsoft Docs"
description: "Bir konumun bir kaynak için bir Azure Resource Manager şablonu nasıl ayarlanacağını gösterir"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: tomfitz
ms.openlocfilehash: 73e50a593c41e841dcaf184abb895406ff5001e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="set-resource-location-in-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kaynak konumunu ayarla
Bir şablon dağıtılırken, her kaynak için bir konum sağlamanız gerekir. Bu konu, her bir kaynak türü için aboneliğinizde kullanılabilir konumları belirlemek gösterilmiştir.

## <a name="determine-supported-locations"></a>Desteklenen konumlar belirler

Her kaynak türü için desteklenen konumlardan tam bir listesi için bkz: [bölgeye göre ürünleri](https://azure.microsoft.com/regions/services/). Ancak, aboneliğinizi bu listedeki tüm konumlara yönelik erişimi olmayabilir. Aboneliğiniz için kullanılabilir konumların özelleştirilmiş bir listesini görmek için Azure PowerShell veya Azure CLI kullanın. 

Aşağıdaki örnek PowerShell konumlarını almak için kullanır. `Microsoft.Web\sites` kaynak türü:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Aşağıdaki örnek konumlarını almak için Azure CLI 2.0 kullanan `Microsoft.Web\sites` kaynak türü:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="set-location-in-template"></a>Şablondaki konum ayarlama

Kaynaklarınız için desteklenen konumlardan belirlendikten sonra bu konuma şablonunuzda ayarlamanız gerekir. Kaynak türlerini destekleyen bir konumda bir kaynak grubu oluşturun ve her konum ayarlamak bu değeri ayarlamak için en kolay yolu olan `[resourceGroup().location]`. Farklı konumlarda kaynak gruplarına şablonu yeniden ve şablon ya da parametre değerleri değiştirin değil. 

Aşağıdaki örnek, kaynak grubu olarak aynı konuma dağıtılan bir depolama hesabı gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

Şablonunuzda konumu stillerinizin gerekiyorsa, desteklenen bölgelerinden adını sağlayın. Aşağıdaki örnek, Kuzey Orta ABD için her zaman dağıtılan bir depolama hesabı gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Şablonlarının nasıl oluşturulacağı hakkında daha fazla önerileri için bkz: [en iyi uygulamalar Azure Resource Manager şablonları oluşturmak için](resource-manager-template-best-practices.md).

