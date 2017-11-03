---
title: "Depolama hesapları için Azure kaynak ilkeleri | Microsoft Docs"
description: "Depolama hesapları dağıtımını yönetmek için Azure Resource Manager ilkelerini açıklar."
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
ms.date: 07/05/2017
ms.author: tomfitz
ms.openlocfilehash: 6612ee61f5c50e743241b92030660cea7ae7094d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="apply-resource-policies-to-storage-accounts"></a>Depolama hesapları için kaynak ilkelerini uygula
Bu konu çeşitli gösterir [kaynak ilkeleri](resource-manager-policy.md) Azure depolama hesapları için geçerli olabilir. Bu ilkeler kuruluşunuza dağıtmış depolama hesapları için tutarlılık emin olun. 

## <a name="define-permitted-storage-account-types"></a>İzin verilen depolama hesabı türlerini tanımlayın

Aşağıdaki ilke hangi kısıtlayan [depolama hesabı türlerini](../storage/common/storage-redundancy.md) dağıtılabilir:

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/sku.name",
          "in": [
            "Standard_LRS",
            "Standard_GRS"
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

İzin verilen SKU'lar kabul etmek için bir parametre ile benzer bir ilke kuralı, bir yerleşik ilke tanımı kullanılabilir. Kaynak Kimliği yerleşik ilkesine sahip `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`. 

## <a name="define-permitted-access-tier"></a>İzin verilen erişim katmanı tanımlayın

Aşağıdaki ilke türünü belirtir. [erişim katmanı](../storage/blobs/storage-blob-storage-tiers.md) depolama hesapları için belirtilebilir:

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="ensure-encryption-is-enabled"></a>Şifreleme etkin olduğundan emin olun

Tüm depolama hesaplarını etkinleştirmek için aşağıdaki ilke gerektiriyorsa [depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md):

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/enableBlobEncryption",
          "equals": "true"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Bu ilke kuralı olarak da kaynak kimliği yerleşik ilke tanımı kullanılabilir `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.

## <a name="next-steps"></a>Sonraki adımlar
* (Yukarıdaki örneklerde gösterildiği gibi) bir ilke kuralı tanımladıktan sonra ilke tanımı oluşturun ve bir kapsama atamanız gerekir. Kapsamı bir abonelik, kaynak grubu veya kaynak olabilir. Portal üzerinden ilkeler atamak için bkz: [atamak ve kaynak ilkelerini yönetmek için kullanım Azure portal](resource-manager-policy-portal.md). REST API'si, PowerShell veya Azure CLI aracılığıyla ilkeleri atamak için bkz: [atayın ve komut dosyası aracılığıyla ilkelerini yönetme](resource-manager-policy-create-assign.md). 
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).

