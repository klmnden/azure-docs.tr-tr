---
title: Azure Resource Manager şablonu çıkarır | Microsoft Docs
description: Bildirim temelli JSON söz dizimini kullanarak bir Azure Resource Manager şablonlarını çıktıların tanımlamayı açıklar.
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
ms.date: 12/18/2018
ms.author: tomfitz
ms.openlocfilehash: 29181b19498b6735651869b6499c4a1cda5a4c3a
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55488863"
---
# <a name="outputs-section-in-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarında çıkış bölümü

Çıkış bölümünde dağıtımdan döndürülen değerlerini belirtin. Örneğin, dağıtılmış bir kaynağa erişmek için URI döndürebilir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="define-and-use-output-values"></a>Tanımlama ve çıkış değerlerini kullanma

Aşağıdaki örnek, kaynak kimliği için bir genel IP adresi döndürülecek gösterilmektedir:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Dağıtımdan sonra değeri betiğiyle alabilirsiniz. PowerShell için şunu kullanın:

```powershell
(Get-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -Name <deployment-name>).Outputs.resourceID.value
```

Azure CLI için şunu kullanın:

```azurecli-interactive
az group deployment show -g <resource-group-name> -n <deployment-name> --query properties.outputs.resourceID.value
```

Çıkış değeri kullanarak bağlantılı bir şablondan alabilirsiniz [başvuru](resource-group-template-functions-resource.md#reference) işlevi. Bağlantılı bir şablondan bir çıkış değeri almak için özellik değeri gibi bir söz dizimi ile Al: `"[reference('deploymentName').outputs.propertyName.value]"`.

Bir çıkış özelliği bağlı şablonundan alınırken, özellik adı bir tire içeremez.

Örneğin, bağlı bir şablondan bir değer alarak IP adresini bir yük dengeleyicideki ayarlayabilirsiniz.

```json
"publicIPAddress": {
    "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

Kullanamazsınız `reference` çıktılar bölümünü işlevinde bir [iç içe geçmiş şablon](resource-group-linked-templates.md#link-or-nest-a-template). Dönüş değerleri dağıtılan kaynağın içinde iç içe geçmiş bir şablon için iç içe geçmiş şablon bağlantılı şablona dönüştürebilirsiniz.

## <a name="available-properties"></a>Kullanılabilir özellikler

Aşağıdaki örnek, bir çıkış tanımı yapısını gösterir:

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
| outputName |Evet |Çıkış değeri adı. Geçerli bir JavaScript tanımlayıcı olmalıdır. |
| type |Evet |Çıkış değeri türü. Çıkış değerleri şablon giriş parametreleri aynı türlerini destekler. |
| değer |Evet |Değerlendirilen ve çıkış değeri döndürülen şablon dili ifadesi. |


## <a name="example-templates"></a>Örnek şablonları

|Şablon  |Açıklama  |
|---------|---------|
|[Değişkenleri kopyalayın](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Karmaşık değişkenler oluşturur ve bu değerleri çıkarır. Tüm kaynakları dağıtmaz. |
|[Genel IP adresi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Genel bir IP adresi oluşturur ve kaynak kimliği çıkarır |
|[Yük Dengeleyici](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Yukarıdaki şablonu bağlar. Kaynak Kimliği, yük dengeleyici oluştururken çıktısında kullanır. |


## <a name="next-steps"></a>Sonraki adımlar
* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Kullanabileceğiniz gelen içinde şablon işlevleri hakkında daha fazla ayrıntı için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Şablonları oluşturma hakkında daha fazla öneri için bkz. [Azure Resource Manager şablonu iyi](template-best-practices.md).
