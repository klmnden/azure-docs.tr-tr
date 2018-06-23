---
title: Azure CLI kullanarak özel roller oluşturmanızı | Microsoft Docs
description: Azure CLI kullanarak rol tabanlı erişim denetimi (RBAC) için özel roller oluşturmayı öğrenin. Bu liste, oluşturma, güncelleştirme ve özel rol silmek içerir.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b1df50c73497e3f5ad64a7ba7210079871b155e0
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321160"
---
# <a name="create-custom-roles-using-azure-cli"></a>Azure CLI kullanarak özel roller oluşturma

Varsa [yerleşik roller](built-in-roles.md) kuruluşunuzun belirli gereksinimlerine uymayan, kendi özel roller oluşturabilirsiniz. Bu makalede, Azure CLI kullanarak özel rollerini oluşturmak ve yönetmek açıklar.

## <a name="prerequisites"></a>Önkoşullar

Özel roller oluşturmak için ihtiyacınız:

- Özel roller gibi oluşturma izni [sahibi](built-in-roles.md#owner) veya [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator)
- [Azure CLI](/cli/azure/install-azure-cli) yerel olarak yüklü

## <a name="list-custom-roles"></a>Liste özel roller

Atama için uygun özel roller listelemek için kullanın [az rol tanımı listesi](/cli/azure/role/definition#az-role-definition-list). Aşağıdaki örneklerde geçerli Abonelikteki tüm özel rollere listelenmektedir.

```azurecli
az role definition list --custom-role-only true --output json | jq '.[] | {"roleName":.roleName, "roleType":.roleType}'
```

```azurecli
az role definition list --output json | jq '.[] | if .roleType == "CustomRole" then {"roleName":.roleName, "roleType":.roleType} else empty end'
```

```Output
{
  "roleName": "My Management Contributor",
  "type": "CustomRole"
}
{
  "roleName": "My Service Reader Role",
  "type": "CustomRole"
}
{
  "roleName": "Virtual Machine Operator",
  "type": "CustomRole"
}

...
```

## <a name="create-a-custom-role"></a>Özel bir rol oluşturun

Özel bir rol oluşturmak üzere kullanmanız [az rol tanımı oluşturma](/cli/azure/role/definition#az-role-definition-create). Rol tanımı JSON açıklamasını veya JSON açıklamasını içeren bir dosya için bir yol olabilir.

```azurecli
az role definition create --role-definition <role_definition>
```

Aşağıdaki örnek adlı özel bir rol oluşturur *sanal makine işleci*. Bu özel rolü tüm okuma işlemleri için erişim atar *Microsoft.Compute*, *Microsoft.Storage*, ve *Microsoft.Network* kaynak sağlayıcıları ve atar erişimi başlatmak için yeniden başlatın ve sanal makineleri izleyin. Bu özel rolü iki Aboneliklerde kullanılabilir. Bu örnek bir JSON dosyası bir giriş olarak kullanır.

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333"
  ]
}
```

```azurecli
az role definition create --role-definition ~/roles/vmoperator.json
```

## <a name="update-a-custom-role"></a>Özel bir rol güncelleştirme

Özel bir rol güncelleştirmek için önce kullanın [az rol tanımı listesi](/cli/azure/role/definition#az-role-definition-list) rol tanımı alınamadı. İkinci olarak, rol tanımı istediğiniz değişiklikleri yapın. Son olarak, [az rol tanımı güncelleştirme](/cli/azure/role/definition#az-role-definition-update) güncelleştirilmiş rol tanımı kaydetmek için.

```azurecli
az role definition update --role-definition <role_definition>
```

Aşağıdaki örnek, *Microsoft.Insights/diagnosticSettings/* işlemi için *Eylemler* , *sanal makine işleci* özel rol.

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333"
  ]
}
```

```azurecli
az role definition update --role-definition ~/roles/vmoperator.json
```

## <a name="delete-a-custom-role"></a>Bir özel rolü silmeyi

Bir özel rolü silmek için kullanın [az rol tanımı silinemiyor](/cli/azure/role/definition#az-role-definition-delete). Rol adı veya rol kimliği silmek için rolü belirtmek için kullanın Rol Kimliği belirlemek için [az rol tanımı listesi](/cli/azure/role/definition#az-role-definition-list).

```azurecli
az role definition delete --name <role_name or role_id>
```

Aşağıdaki örnek siler *sanal makine işleci* özel rol.

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure CLI kullanarak özel bir rol oluşturma](tutorial-custom-role-cli.md)
- [Azure özel roller](custom-roles.md)
- [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md)