---
title: RBAC ve Azure CLI kullanarak erişimini yönetme | Microsoft Docs
description: Kullanıcılar, gruplar ve uygulamalar, rol tabanlı erişim denetimi (RBAC) ve Azure CLI kullanarak erişimi yönetmek üzere öğrenin. Bu erişim listesinde, erişim ve erişimi kaldırmak nasıl içerir.
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
ms.openlocfilehash: 16577339f1aa33fbd1a8b90f4beaef1ee4ce806c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316405"
---
# <a name="manage-access-using-rbac-and-azure-cli"></a>RBAC ve Azure CLI kullanarak erişimini yönetme

[Rol tabanlı erişim denetimi (RBAC)](overview.md) Azure kaynaklarına erişimi yönetme yoludur. Bu makalede, kullanıcılar, gruplar ve RBAC ve Azure CLI kullanarak uygulamalar için erişim yönetme açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Erişimi yönetmek için aşağıdakilerden biri gerekir:

* [Azure bulut Kabuğu'nda bash](/azure/cloud-shell/overview)
* [Azure CLI](/cli/azure)

## <a name="list-roles"></a>Liste rolleri

Tüm kullanılabilir rol tanımlarını listelemek için kullanın [az rol tanımı listesi](/cli/azure/role/definition#az-role-definition-list):

```azurecli
az role definition list
```

Aşağıdaki örnek adını ve tüm kullanılabilir rol tanımlarını açıklamasını listeler:

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.roleName, "description":.description}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs"
}

...
```

Aşağıdaki örnekte tüm yerleşik rol tanımları listelenmiştir:

```azurecli
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.roleName, "description":.description, "roleType":.roleType}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "roleType": "BuiltInRole"
}

...
```

### <a name="list-actions-of-a-role"></a>Bir rolün liste eylemleri

Rol tanımı Eylemler listelemek için kullanın [az rol tanımı listesi](/cli/azure/role/definition#az-role-definition-list):

```azurecli
az role definition list --name <role_name>
```

Aşağıdaki örnek listeleri *katkıda bulunan* rol tanımı:

```azurecli
az role definition list --name "Contributor"
```

```Output
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Aşağıdaki örnek listeleri *Eylemler* ve *notActions* , *katkıda bulunan* rol:

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.permissions[0].actions, "notActions":.permissions[0].notActions}'
```

```Output
{
  "actions": [
    "*"
  ],
  "notActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ]
}
```

Aşağıdaki örnek eylemleri listeler *sanal makine Katılımcısı* rol:

```azurecli
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .permissions[0].actions'
```

```Output
[
  "Microsoft.Authorization/*/read",
  "Microsoft.Compute/availabilitySets/*",
  "Microsoft.Compute/locations/*",
  "Microsoft.Compute/virtualMachines/*",
  "Microsoft.Compute/virtualMachineScaleSets/*",
  "Microsoft.Insights/alertRules/*",
  "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
  "Microsoft.Network/loadBalancers/backendAddressPools/join/action",

  ...

  "Microsoft.Storage/storageAccounts/listKeys/action",
  "Microsoft.Storage/storageAccounts/read"
]
```

## <a name="list-access"></a>Liste erişim

RBAC, liste erişim için rol atamalarını listeleyin.

### <a name="list-role-assignments-for-a-user"></a>Bir kullanıcı için rol atamalarını listesi

Belirli bir kullanıcı için rol atamalarını listelemek için kullanın [az rol ataması listesi](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --assignee <assignee>
```

Varsayılan olarak, yalnızca aboneliği kapsamlıdır atamaları görüntülenir. Kaynak veya grup tarafından kapsamlı atamalarını görüntülemek için kullanma `--all`.

Aşağıdaki örnek, doğrudan atanmış olan rol atamalarını listeler *patlong@contoso.com* kullanıcı:

```azurecli
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
```

### <a name="list-role-assignments-for-a-resource-group"></a>Bir kaynak grubu için liste rol atamaları

Bir kaynak grubu için mevcut rol atamalarını listelemek için kullanın [az rol ataması listesi](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --resource-group <resource_group>
```

Aşağıdaki örnek için rol atamalarını listeler *pharma satış projectforecast* kaynak grubu:

```azurecli
az role assignment list --resource-group pharma-sales-projectforecast --output json | jq '.[] | {"roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}

...
```

## <a name="grant-access"></a>Erişim verme

RBAC, erişim vermek için bir rol ataması oluşturun.

### <a name="create-a-role-assignment-for-a-user"></a>Bir kullanıcı için rol ataması oluşturma

Kaynak grubu kapsamda bir kullanıcının bir rol ataması oluşturmak üzere kullanmanız [az rol ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role> --assignee <assignee> --resource-group <resource_group>
```

Aşağıdaki örnek atar *sanal makine Katılımcısı* rolüne *patlong@contoso.com* kullanıcı *pharma satış projectforecast* Kaynak Grup kapsamı:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales-projectforecast
```

### <a name="create-a-role-assignment-for-a-group"></a>Bir grup için bir rol ataması oluşturma

Bir grup için bir rol ataması oluşturmak üzere kullanmanız [az rol ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Aşağıdaki örnek atar *okuyucu* rolüne *Ann Mack takım* kimliği 22222222-2222-2222-2222-222222222222 abonelik kapsamında grubu. Grup Kimliğini almak için kullanabileceğiniz [az ad Grup listesi](/cli/azure/ad/group#az-ad-group-list) veya [az ad Grup Göster](/cli/azure/ad/group#az-ad-group-show).

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

Aşağıdaki örnek atar *sanal makine Katılımcısı* rolüne *Ann Mack takım* kimliği 22222222-2222-2222-2222-222222222222 adlıbirsanalağiçinbirkaynakkapsamdagrubu*satış proje ağ pharma*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111/resourcegroups/pharma-sales-projectforecast/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="create-a-role-assignment-for-an-application"></a>Bir uygulama için bir rol ataması oluşturma

Bir uygulama için bir rol oluşturmak için kullanmak [az rol ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Aşağıdaki örnek atar *sanal makine Katılımcısı* nesne kimliği 44444444-4444-4444-4444-444444444444 uygulamayla rolüne *pharma satış projectforecast* kaynak grubu kapsamı. Uygulama nesnesi Kimliği almak için kullanabileceğiniz [az ad uygulama listesinde](/cli/azure/ad/app#az-ad-app-list) veya [az ad uygulama Göster](/cli/azure/ad/app#az-ad-app-show).

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales-projectforecast
```

## <a name="remove-access"></a>Erişimi kaldır

RBAC, erişimi kaldırmak için bir rol ataması kullanarak Kaldır [az rol atamasını silin](/cli/azure/role/assignment#az-role-assignment-delete):

```azurecli
az role assignment delete --assignee <assignee> --role <role> --resource-group <resource_group>
```

Aşağıdaki örnek kaldırır *sanal makine Katılımcısı* gelen rol atamasını *patlong@contoso.com* kullanıcı *pharma satış projectforecast* kaynak Grup:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales-projectforecast
```

Aşağıdaki örnek kaldırır *okuyucu* rolünden *Ann Mack takım* kimliği 22222222-2222-2222-2222-222222222222 abonelik kapsamında grubu. Grup Kimliğini almak için kullanabileceğiniz [az ad Grup listesi](/cli/azure/ad/group#az-ad-group-list) veya [az ad Grup Göster](/cli/azure/ad/group#az-ad-group-show).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure CLI kullanarak özel bir rol oluşturma](tutorial-custom-role-cli.md)
- [Azure kaynakları ve kaynak gruplarını yönetmek için Azure CLI kullanma](../azure-resource-manager/xplat-cli-azure-resource-manager.md)