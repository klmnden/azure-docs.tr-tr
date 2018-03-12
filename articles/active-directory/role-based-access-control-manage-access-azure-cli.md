---
title: "Rol tabanlı erişim denetimi (RBAC) Azure CLI ile yönetme | Microsoft Docs"
description: "Rolleri ve rol eylemlerin listesini içeren ve rolleri için abonelik ve uygulama kapsamları atama rol tabanlı erişim denetimi (RBAC) Azure komut satırı arabirimi ile yönetmeyi öğrenin."
services: active-directory
documentationcenter: 
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.openlocfilehash: 6c9df11e528601d94cb72a8e3ef0868dc7781e12
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Rol tabanlı erişim denetimini Azure komut satırı arabirimi ile yönetme

> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)


Rol tabanlı erişim denetimi (RBAC) ile belirli bir kapsamda rolleri atayarak erişim için kullanıcıları, grupları ve hizmet asıl adı tanımlayın. Bu makalede, Azure komut satırı arabirimi (CLI) kullanarak erişimi yönetmek üzere açıklar.

## <a name="prerequisites"></a>Önkoşullar

RBAC yönetmek için Azure CLI kullanmak için aşağıdaki önkoşullar olması gerekir:

* [Azure CLI 2.0](/cli/azure). Bunu [Azure Cloud Shell](../cloud-shell/overview.md) ile tarayıcınızda kullanabilir veya macOS, Linux ve Windows’a [yükleyerek](/cli/azure/install-azure-cli) komut satırından çalıştırabilirsiniz.

## <a name="list-roles"></a>Liste rolleri

### <a name="list-role-definitions"></a>Liste rol tanımları

Tüm kullanılabilir rol tanımlarını listelemek için kullanın [az rol tanımı listesi](/cli/azure/role/definition#az_role_definition_list):

```azurecli
az role definition list
```

Aşağıdaki örnek adını ve tüm kullanılabilir rol tanımlarını açıklamasını listeler:

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
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
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description, "type":.properties.type}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "type": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "type": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "type": "BuiltInRole"
}

...
```

### <a name="list-actions-of-a-role-definition"></a>Bir rol tanımı liste eylemleri

Rol tanımı Eylemler listelemek için kullanın [az rol tanımı listesi](/cli/azure/role/definition#az_role_definition_list):

```azurecli
az role definition list --name <role_name>
```

Aşağıdaki örnek listeleri *katkıda bulunan* rol tanımı:

```azurecli
az role definition list --name "Contributor"
```

```Output
[
  {
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "properties": {
      "additionalProperties": {
        "createdBy": null,
        "createdOn": "0001-01-01T08:00:00.0000000Z",
        "updatedBy": null,
        "updatedOn": "2016-12-14T02:04:45.1393855Z"
      },
      "assignableScopes": [
        "/"
      ],
      "description": "Lets you manage everything except access to resources.",
      "permissions": [
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
      ],
      "roleName": "Contributor",
      "type": "BuiltInRole"
    },
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Aşağıdaki örnek listeleri *Eylemler* ve *notActions* , *katkıda bulunan* rol:

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.properties.permissions[0].actions, "notActions":.properties.permissions[0].notActions}'
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
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .properties.permissions[0].actions'
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

### <a name="list-role-assignments-for-a-user"></a>Bir kullanıcı için rol atamalarını listesi

Belirli bir kullanıcı için rol atamalarını listelemek için kullanın [az rol ataması listesi](/cli/azure/role/assignment#az_role_assignment_list):

```azurecli
az role assignment list --assignee <assignee>
```

Varsayılan olarak, yalnızca aboneliği kapsamlıdır atamaları görüntülenir. Kaynak veya grup tarafından kapsamlı atamalarını görüntülemek için kullanma `--all`.

Aşağıdaki örnek, doğrudan atanmış olan rol atamalarını listeler  *patlong@contoso.com*  kullanıcı:

```azurecli
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.properties.principalName, "roleDefinitionName":.properties.roleDefinitionName, "scope":.properties.scope}'
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

Bir kaynak grubu için mevcut rol atamalarını listelemek için kullanın [az rol ataması listesi](/cli/azure/role/assignment#az_role_assignment_list):

```azurecli
az role assignment list --resource-group <resource_group>
```

Aşağıdaki örnek için rol atamalarını listeler *pharma satış projectforecast* kaynak grubu:

```azurecli
az role assignment list --resource-group pharma-sales-projectforecast --output json | jq '.[] | {"roleDefinitionName":.properties.roleDefinitionName, "scope":.properties.scope}'
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

## <a name="assign-access"></a>Erişimi atayın

### <a name="assign-a-role-to-a-user"></a>Bir kullanıcıya rol atama

Kaynak grubu kapsamındaki bir kullanıcıya rol atamak için kullandığınız [az rol ataması oluşturma](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee <assignee> --resource-group <resource_group>
```

Aşağıdaki örnek atar *sanal makine Katılımcısı* rolüne  *patlong@contoso.com*  kullanıcı *pharma satış projectforecast* Kaynak Grup kapsamı:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales-projectforecast
```

### <a name="assign-a-role-to-a-group"></a>Bir gruba bir rol atayın

Bir rol bir gruba atamak için kullandığınız [az rol ataması oluşturma](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Aşağıdaki örnek atar *okuyucu* rolüne *Ann Mack takım* kimliği 22222222-2222-2222-2222-222222222222 abonelik kapsamında grubu. Grup Kimliğini almak için kullanabileceğiniz [az ad Grup listesi](/cli/azure/ad/group#az_ad_group_list) veya [az ad Grup Göster](/cli/azure/ad/group#az_ad_group_show).

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

Aşağıdaki örnek atar *sanal makine Katılımcısı* rolüne *Ann Mack takım* kimliği 22222222-2222-2222-2222-222222222222 adlıbirsanalağiçinbirkaynakkapsamdagrubu*satış proje ağ pharma*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111/resourcegroups/pharma-sales-projectforecast/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="assign-a-role-to-an-application"></a>Uygulama için bir rol atama

Bir uygulama için bir rol atamak için kullandığınız [az rol ataması oluşturma](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

Aşağıdaki örnek atar *sanal makine Katılımcısı* nesne kimliği 44444444-4444-4444-4444-444444444444 uygulamayla rolüne *pharma satış projectforecast* kaynak grubu kapsamı. Uygulama nesnesi Kimliği almak için kullanabileceğiniz [az ad uygulama listesinde](/cli/azure/ad/app#az_ad_app_list) veya [az ad uygulama Göster](/cli/azure/ad/app#az_ad_app_show).

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales-projectforecast
```

## <a name="remove-access"></a>Erişimi kaldırma

### <a name="remove-a-role-assignment"></a>Bir rol ataması Kaldır

Bir rol atamasını kaldırmak için kullanın [az rol atamasını silin](/cli/azure/role/assignment#az_role_assignment_delete):

```azurecli
az role assignment delete --assignee <assignee> --role <role> --resource-group <resource_group>
```

Aşağıdaki örnek kaldırır *sanal makine Katılımcısı* gelen rol atamasını  *patlong@contoso.com*  kullanıcı *pharma satış projectforecast* kaynak Grup:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales-projectforecast
```

Aşağıdaki örnek kaldırır *okuyucu* rolünden *Ann Mack takım* kimliği 22222222-2222-2222-2222-222222222222 abonelik kapsamında grubu. Grup Kimliğini almak için kullanabileceğiniz [az ad Grup listesi](/cli/azure/ad/group#az_ad_group_list) veya [az ad Grup Göster](/cli/azure/ad/group#az_ad_group_show).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="custom-roles"></a>Özel roller

### <a name="list-custom-roles"></a>Liste özel roller

Bir kapsamda atama için kullanılabilen rolleri listelemek için kullanın [az rol tanımı listesi](/cli/azure/role/definition#az_role_definition_list).

Aşağıdaki örnekler her ikisi de geçerli Abonelikteki tüm özel rollere listesi:

```azurecli
az role definition list --custom-role-only true --output json | jq '.[] | {"roleName":.properties.roleName, "type":.properties.type}'
```

```azurecli
az role definition list --output json | jq '.[] | if .properties.type == "CustomRole" then {"roleName":.properties.roleName, "type":.properties.type} else empty end'
```

```Output
{
  "roleName": "My Management Contributor",
  "type": "CustomRole"
}
{
  "roleName": "My Service Operator Role",
  "type": "CustomRole"
}
{
  "roleName": "My Service Reader Role",
  "type": "CustomRole"
}

...
```

### <a name="create-a-custom-role"></a>Özel bir rol oluşturun

Özel bir rol oluşturmak üzere kullanmanız [az rol tanımı oluşturma](/cli/azure/role/definition#az_role_definition_create). Rol tanımı JSON açıklamasını veya JSON açıklamasını içeren bir dosya için bir yol olabilir.

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

### <a name="update-a-custom-role"></a>Özel bir rol güncelleştirme

Özel bir rol güncelleştirmek için önce kullanın [az rol tanımı listesi](/cli/azure/role/definition#az_role_definition_list) rol tanımı alınamadı. İkinci olarak, rol tanımı istediğiniz değişiklikleri yapın. Son olarak, [az rol tanımı güncelleştirme](/cli/azure/role/definition#az_role_definition_update) güncelleştirilmiş rol tanımı kaydetmek için.

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

### <a name="delete-a-custom-role"></a>Bir özel rolü silmeyi

Bir özel rolü silmek için kullanın [az rol tanımı silinemiyor](/cli/azure/role/definition#az_role_definition_delete). Rol adı veya rol kimliği silmek için rolü belirtmek için kullanın Rol Kimliği belirlemek için [az rol tanımı listesi](/cli/azure/role/definition#az_role_definition_list).

```azurecli
az role definition delete --name <role_name or role_id>
```

Aşağıdaki örnek siler *sanal makine işleci* özel rol:

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

