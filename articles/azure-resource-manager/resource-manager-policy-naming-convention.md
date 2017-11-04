---
title: "Adlandırma kuralları için Azure kaynak ilkeleri | Microsoft Docs"
description: "Kaynak adlandırma kuralları için Azure Resource Manager ilkelerini açıklar."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: tomfitz
ms.openlocfilehash: 955b04517c3ccdbe530eb982e6aa1255b69e1edd
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="apply-resource-policies-for-names-and-text"></a>Kaynak adları ve metin için geçerlidir
Bu makalede birkaç gösterilmektedir [kaynak ilkeleri](resource-manager-policy.md) adlandırma ve metin kuralları oluşturmak için geçerli olabilir. Bu ilkeler kaynak adları ve etiket değerleri için tutarlılık emin olun. 

## <a name="set-naming-convention-with-wildcard"></a>Joker karakter ile adlandırma kuralı ayarlayın
Aşağıdaki örnek tarafından desteklenen joker karakter kullanımı gösterilmiştir **gibi** koşulu. Adı belirtilen desen eşleşirse bildiren koşul (namePrefix\*nameSuffix) isteği reddeder:

```json
{
  "if": {
    "not": {
      "field": "name",
      "like": "namePrefix*nameSuffix"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-naming-convention-with-pattern"></a>Desen ile adlandırma kuralı ayarlayın

Kaynak adları bir desenle eşleşen belirtmek için eşleşme koşul kullanın. Aşağıdaki örnek adları başlamak gerektirir `contoso` ve altı ek harf içermelidir:

```json
{
  "if": {
    "not": {
      "field": "name",
      "match": "contoso??????"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-date-pattern-for-tag-value"></a>Etiket değeri için tarih deseni ayarlayın

İki basamaklı, tire, üç harf, tire ve dört basamak kullanım tarihi düzeni istemek için:

```json
{
  "if": {
    "field": "tags.date",
    "match": "##-???-####"
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-multiple-naming-patterns"></a>Birden çok adlandırma modelleri ayarlayın

Birden fazla adlandırma kuralı izin belirtmek için kullanın **tümü** ve **değil** işleçler. Sağlanan ad ya da Desen eşleşmiyorsa, aşağıdaki örnekte, reddedildi.

```json
{
    "if": {
        "allOf": [
            {
                "not": {
                    "field": "name",
                    "match": "contoso??????"
                }
            },
            {
                "not": {
                    "field": "name",
                    "match": "contoso-???-##"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* (Yukarıdaki örneklerde gösterildiği gibi) bir ilke kuralı tanımladıktan sonra ilke tanımı oluşturun ve bir kapsama atamanız gerekir. Kapsamı bir abonelik, kaynak grubu veya kaynak olabilir. Portal üzerinden ilkeler atamak için bkz: [atamak ve kaynak ilkelerini yönetmek için kullanım Azure portal](resource-manager-policy-portal.md). REST API'si, PowerShell veya Azure CLI aracılığıyla ilkeleri atamak için bkz: [atayın ve komut dosyası aracılığıyla ilkelerini yönetme](resource-manager-policy-create-assign.md). 
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).

