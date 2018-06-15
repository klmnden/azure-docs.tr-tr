---
title: Azure Resource Manager şablonu çıkarır | Microsoft Docs
description: Bildirim temelli JSON sözdizimini kullanarak bir Azure Resource Manager şablonları çıktıların tanımlamak açıklar.
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
ms.date: 12/14/2017
ms.author: tomfitz
ms.openlocfilehash: e3c5a581b02f1dd7b7415ebd93de0e425ac2f8ae
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358374"
---
# <a name="outputs-section-in-azure-resource-manager-templates"></a>Azure Resource Manager şablonları bölümünde çıkışları
Çıkış bölümünde dağıtımından döndürülen değerlerini belirtin. Örneğin, dağıtılan bir kaynağa erişmek için URI döndürebilirsiniz.

## <a name="define-and-use-output-values"></a>Tanımlama ve çıktı değerleri kullanma

Aşağıdaki örnek, bir ortak IP adresi kaynak Kimliğini döndürme gösterilmektedir:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Dağıtım sonrası betik değerle alabilir. PowerShell için şunu kullanın:

```powershell
(Get-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -Name <deployment-name>).Outputs.resourceID.value
```

Azure CLI için şunu kullanın:

```azurecli-interactive
az group deployment show -g <resource-group-name> -n <deployment-name> --query properties.outputs.resourceID.value
```

Çıkış değerini kullanarak bağlantılı bir şablondan alabilirsiniz [başvuru](resource-group-template-functions-resource.md#reference) işlevi. Bağlantılı bir şablondan bir çıkış değerini almak için özellik değeri gibi bir sözdizimiyle almak: `"[reference('<name-of-deployment>').outputs.<property-name>.value]"`.

Örneğin, bağlantılı bir şablondan bir değer alarak IP adresi yük dengeleyicide ayarlayabilirsiniz.

```json
"publicIPAddress": {
    "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

Kullanamazsınız `reference` işlevi çıkışları bölümünde bir [iç içe geçmiş şablon](resource-group-linked-templates.md#link-or-nest-a-template). Bir iç içe geçmiş şablonunda dağıtılmış bir kaynak için değer döndürmek için iç içe geçmiş şablonunuzu bağlantılı şablona dönüştürebilirsiniz.

## <a name="available-properties"></a>Kullanılabilir özellikler

Aşağıdaki örnek bir çıktı tanımının yapısını gösterir:

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| outputName |Evet |Çıkış değeri adı. Geçerli bir JavaScript tanımlayıcı olması gerekir. |
| type |Evet |Çıktı değerin türü. Şablon giriş parametreleri aynı türlerine çıkış değerlerini destekler. |
| değer |Evet |Hesaplanan ve çıktı değeri olarak döndürülen şablon dili ifadesi. |

## <a name="recommendations"></a>Öneriler

Genel IP adresleri oluşturmak için bir şablon kullanıyorsanız, IP adresi ve tam etki alanı adı (FQDN) ayrıntılarını döndürür bir çıkış bölümüne ekler. Çıkış değerleri kolayca dağıtımdan sonra ortak IP adresleri ve FQDN'ler hakkındaki ayrıntıları almak için kullanabilirsiniz.

```json
"outputs": {
    "fqdn": {
        "value": "[reference(parameters('publicIPAddresses_name')).dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(parameters('publicIPAddresses_name')).ipAddress]",
        "type": "string"
    }
}
```

## <a name="example-templates"></a>Örnek şablonları


|Şablon  |Açıklama  |
|---------|---------|
|[Kopyalama değişkenleri](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Karmaşık değişkenleri oluşturur ve bu değerleri çıkarır. Herhangi bir kaynağa dağıtmaz. |
|[Genel IP adresi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Bir ortak IP adresi oluşturur ve kaynak kimliğine çıkarır |
|[Yük Dengeleyici](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Yukarıdaki şablonu bağlantılar. Kaynak Kimliği, yük dengeleyici oluştururken çıktıda kullanır. |


## <a name="next-steps"></a>Sonraki adımlar
* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Kullanabileceğiniz gelen bir şablonda işlevleri hakkında daha fazla ayrıntı için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Birden fazla şablon dağıtımı sırasında birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Farklı bir kaynak grubu içinde mevcut kaynakları kullanmanız gerekebilir. Bu senaryo, depolama hesapları veya birden çok kaynak grupları arasında paylaşılan sanal ağlar ile çalışırken yaygındır. Daha fazla bilgi için bkz: [ResourceId işlevi](resource-group-template-functions-resource.md#resourceid).