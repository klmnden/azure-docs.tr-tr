---
title: RBAC ve Azure CLI kullanarak Azure kaynaklarına erişimi yönetme | Microsoft Docs
description: Kullanıcılar, gruplar ve rol tabanlı erişim denetimi (RBAC) ve Azure CLI'yı kullanarak uygulamaları için Azure kaynaklarına erişimini yönetmeyi öğrenin. Buna erişimi listeleme, erişim verme ve erişimi kaldırma işlemleri dahildir.
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
ms.date: 04/17/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: bc5deb614e2ac6e47ff3bf241943df92d97699b2
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295168"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-azure-cli"></a>RBAC ve Azure CLI kullanarak Azure kaynaklarına erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](overview.md) Azure kaynaklarına erişimi yönetme yoludur. Bu makalede, kullanıcıları, grupları ve RBAC ve Azure CLI'yı kullanan uygulamalar için erişimi nasıl yönetileceği açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Erişimi yönetmek için aşağıdakilerden biri gerekir:

* [Azure Cloud Shell'de bash](/azure/cloud-shell/overview)
* [Azure CLI](/cli/azure)

## <a name="list-roles"></a>Rolleri listeleme

Tüm kullanılabilir rol tanımları listelemek için kullanın [az role definition listesini](/cli/azure/role/definition#az-role-definition-list):

```azurecli
az role definition list
```

Aşağıdaki örnekte, ad ve Açıklama tüm kullanılabilir rol tanımları listelenmiştir:

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

## <a name="list-a-role-definition"></a>Bir rol tanımını listeleme

Rol tanımı listelemek için kullanın [az role definition listesini](/cli/azure/role/definition#az-role-definition-list):

```azurecli
az role definition list --name <role_name>
```

Aşağıdaki örnek listeler *katkıda bulunan* rol tanımı:

```azurecli
az role definition list --name "Contributor"
```

```Output
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
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

### <a name="list-actions-of-a-role"></a>Bir rolün liste eylemleri

Aşağıdaki örnek yalnızca listeler *eylemleri* ve *notActions* , *katkıda bulunan* rolü:

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

Aşağıdaki örnek yalnızca eylemleri listeler *sanal makine Katılımcısı* rolü:

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

## <a name="list-access"></a>Erişimi listeleme

Liste erişim, RBAC, rol atamalarını listeleyin.

### <a name="list-role-assignments-for-a-user"></a>Bir kullanıcının rol atamalarını listeleme

Belirli bir kullanıcı için rol atamalarını listelemek için kullanın [az rol ataması listesi](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --assignee <assignee>
```

Varsayılan olarak, yalnızca abonelik için kapsamlı doğrudan atamaları görüntülenir. Kaynak veya grup tarafından kapsamlı atamaları görüntülemek için kullanın `--all` ve devralınan asisgnments görüntülemek için kullanın `--include-inherited`.

Aşağıdaki örnek, doğrudan atanan rol atamaları listeler *patlong\@contoso.com* kullanıcı:

```azurecli
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
```

### <a name="list-role-assignments-for-a-resource-group"></a>Bir kaynak grubunun rol atamalarını listeleme

Mevcut bir kaynak grubu için rol atamalarını listelemek için kullanın [az rol ataması listesi](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --resource-group <resource_group>
```

Aşağıdaki örnek için rol atamalarını listeler *pharma satış* kaynak grubu:

```azurecli
az role assignment list --resource-group pharma-sales --output json | jq '.[] | {"roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
{
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}

...
```

## <a name="grant-access"></a>Erişim verme

RBAC'de erişim vermek için bir rol ataması oluşturmanız gerekir.

### <a name="create-a-role-assignment-for-a-user"></a>Bir kullanıcı için rol ataması oluşturma

Kaynak grubu kapsamında bir rol ataması için bir kullanıcı oluşturmak için kullanın [az rol ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --resource-group <resource_group>
```

Aşağıdaki örnek atar *sanal makine Katılımcısı* rolüne *patlong\@contoso.com* kullanıcı *pharma satış* Kaynak Grup kapsamı:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="create-a-role-assignment-using-the-unique-role-id"></a>Benzersiz rol kimliği kullanarak bir rol ataması oluştur

Birkaç kez ne zaman bir rol adı, örneğin değişebilir vardır:

- Kendi özel rol kullanmakta olduğunu ve adını değiştirmek karar verin.
- Bir önizleme rolünü kullandığınız **(Önizleme)** adı. Rol, rol yayınlandığı zaman, adlandırılır.

> [!IMPORTANT]
> Bir önizleme sürümü bir hizmet düzeyi sözleşmesi sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bir rolü yeniden adlandırılmış olsa bile, rol kimliği değiştirmez. Rol atamalarınızı oluşturmak için betikler veya Otomasyon kullanıyorsanız, rolü adı yerine benzersiz rol kimliği kullanmak için en iyi bir uygulamadır. Bir rolü yeniden adlandırılırsa, bu nedenle, betiklerinizi çalışma olasılığı daha yüksektir.

Rol adı yerine benzersiz rol kimliği kullanarak bir rol ataması oluşturmak için kullanın [az rol ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create).

```azurecli
az role assignment create --role <role_id> --assignee <assignee> --resource-group <resource_group>
```

Aşağıdaki örnek atar [sanal makine Katılımcısı](built-in-roles.md#virtual-machine-contributor) rolüne *patlong\@contoso.com* kullanıcı *pharma satış* Kaynak Grup kapsamı. Benzersiz Rol Kimliği almak için kullanabileceğiniz [az role definition listesini](/cli/azure/role/definition#az-role-definition-list) veya [Azure kaynakları için yerleşik roller](built-in-roles.md).

```azurecli
az role assignment create --role 9980e02c-c2be-4d73-94e8-173b1dc7cf3c --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="create-a-role-assignment-for-a-group"></a>Bir grup için bir rol ataması oluştur

Bir rol ataması için bir grup oluşturmak için kullanın [az rol ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Aşağıdaki örnek atar *okuyucu* rolüne *Ann Mack takım* kimliği 22222222-2222-2222-2222-222222222222 abonelik kapsamında grubu. Grubun kimliği almak için kullanabileceğiniz [az ad Grup listesi](/cli/azure/ad/group#az-ad-group-list) veya [az ad Grup show](/cli/azure/ad/group#az-ad-group-show).

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000
```

Aşağıdaki örnek atar *sanal makine Katılımcısı* rolüne *Ann Mack takım* kimliği 22222222-2222-2222-2222-222222222222 adlıbirsanalağiçinbirkaynakkapsamdagrubu*satış proje ağ pharma*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/pharma-sales/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="create-a-role-assignment-for-an-application"></a>Bir uygulama için bir rol ataması oluştur

Bir uygulama için bir rol oluşturmak için kullanmak [az rol ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Aşağıdaki örnek atar *sanal makine Katılımcısı* nesne kimliği 44444444-4444-4444-4444-444444444444 uygulamayla rolüne *pharma satış* Kaynak Grup kapsamı. Uygulamanın nesne Kimliğini almak için kullanabileceğiniz [az ad app listesi](/cli/azure/ad/app#az-ad-app-list) veya [az ad app show](/cli/azure/ad/app#az-ad-app-show).

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales
```

## <a name="remove-access"></a>Erişimi kaldırma

RBAC, erişimi kaldırmak için rol ataması kullanarak kaldırmanız [az rol atamasını Sil](/cli/azure/role/assignment#az-role-assignment-delete):

```azurecli
az role assignment delete --assignee <assignee> --role <role_name_or_id> --resource-group <resource_group>
```

Aşağıdaki örnek kaldırır *sanal makine Katılımcısı* rol atamasından *patlong\@contoso.com* kullanıcı *pharma satış* kaynak grubu:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales
```

Aşağıdaki örnek kaldırır *okuyucu* rolünden *Ann Mack takım* kimliği 22222222-2222-2222-2222-222222222222 abonelik kapsamında grubu. Grubun kimliği almak için kullanabileceğiniz [az ad Grup listesi](/cli/azure/ad/group#az-ad-group-list) veya [az ad Grup show](/cli/azure/ad/group#az-ad-group-show).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --scope /subscriptions/00000000-0000-0000-0000-000000000000
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure CLI kullanarak Azure kaynakları için özel bir rol oluşturun](tutorial-custom-role-cli.md)
- [Azure kaynaklarını ve kaynak gruplarını yönetmek için Azure CLI kullanma](../azure-resource-manager/cli-azure-resource-manager.md)
