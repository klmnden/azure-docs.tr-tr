---
title: Azure CLI kullanarak Azure kaynakları için özel roller oluşturma | Microsoft Docs
description: Azure CLI kullanarak Azure kaynakları için rol tabanlı erişim denetimi (RBAC) ile özel roller oluşturma konusunda bilgi edinin. Bu liste, oluşturma, güncelleştirme ve özel roller silme nasıl içerir.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b768f6e240c354369246a6d978ed3e8dd2f58f92
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2019
ms.locfileid: "56338147"
---
# <a name="create-custom-roles-for-azure-resources-using-azure-cli"></a>Azure CLI kullanarak Azure kaynakları için özel roller oluşturma

Varsa [Azure kaynakları için yerleşik roller](built-in-roles.md) kuruluşunuzun belirli gereksinimlerine uymayan, kendi özel rollerinizi oluşturabilirsiniz. Bu makalede, Azure CLI kullanarak özel roller oluşturmak ve yönetmek açıklar.

## <a name="prerequisites"></a>Önkoşullar

Özel roller oluşturmanızı da ihtiyacınız vardır:

- [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) gibi özel rol oluşturma izni
- Yerel ortama yüklenmiş [Azure CLI](/cli/azure/install-azure-cli)

## <a name="list-custom-roles"></a>Özel rolleri listeleme

Atama için kullanılabilir olan özel roller listelemek için kullanın [az role definition listesini](/cli/azure/role/definition#az-role-definition-list). Aşağıdaki örneklerde, geçerli abonelikte yer alan tüm özel rolleri listelenmektedir.

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

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Özel bir rol oluşturmak için kullanın [az rol tanımını oluşturma](/cli/azure/role/definition#az-role-definition-create). Rol tanımı JSON açıklamasını ya da bir JSON tanımı içeren dosyanın yolu olabilir.

```azurecli
az role definition create --role-definition <role_definition>
```

Aşağıdaki örnekte adlı özel bir rol oluşturur *sanal makine işleci*. Bu özel rolü tüm okuma işlemleri için erişim atar *Microsoft.Compute*, *Microsoft.Storage*, ve *Microsoft.Network* kaynak sağlayıcıları ve atadıkları kişiler erişimi başlatma, yeniden başlatın ve sanal makinelerini izlemek için. Bu özel rolü iki abonelik kullanılabilir. Bu örnekte bir JSON dosyası girdi olarak kullanılır.

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

## <a name="update-a-custom-role"></a>Özel rolü güncelleştirme

Özel bir rol güncelleştirmek için önce kullanın [az role definition listesini](/cli/azure/role/definition#az-role-definition-list) rol tanımı alınamıyor. İkinci olarak, rol tanımı için istediğiniz değişiklikleri yapın. Son olarak, [az role definition güncelleştirme](/cli/azure/role/definition#az-role-definition-update) güncelleştirilmiş rol tanımını kaydedin.

```azurecli
az role definition update --role-definition <role_definition>
```

Aşağıdaki örnek ekler *Microsoft.Insights/diagnosticSettings/* işlemi *eylemleri* , *sanal makine işleci* özel rol.

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

## <a name="delete-a-custom-role"></a>Özel rolü silme

Bir özel rolü silmek için kullanın [az role definition delete](/cli/azure/role/definition#az-role-definition-delete). Rol adı veya rol kimliği. Silinecek rol belirtmek için kullanın Rol Kimliği belirlemek için [az role definition listesini](/cli/azure/role/definition#az-role-definition-list).

```azurecli
az role definition delete --name <role_name or role_id>
```

Aşağıdaki örnek siler *sanal makine işleci* özel rol.

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure CLI kullanarak Azure kaynakları için özel bir rol oluşturun](tutorial-custom-role-cli.md)
- [Azure kaynakları için özel roller](custom-roles.md)
- [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md)