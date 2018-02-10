---
title: "Rol tabanlı erişim denetimini REST - Azure AD ile | Microsoft Docs"
description: "Rol tabanlı erişim denetimini REST API ile yönetme"
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: 
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: active-directory
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: rolyon
ms.openlocfilehash: d449b53d348471275cea3c7129245569e2151864
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="manage-role-based-access-control-with-the-rest-api"></a>Rol tabanlı erişim denetimini REST API ile yönetme
> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
> * [REST API](role-based-access-control-manage-access-rest.md)

Rol tabanlı erişim denetimi (RBAC) Azure portalında ve Azure Resource Manager API abonelik ve ayrıntılı bir düzeyde kaynaklara erişimi yönetmenize yardımcı olur. Bu özellik ile bazı roller belirli bir kapsamda atayarak Active Directory Kullanıcıları, grupları veya hizmet asıl adı için erişim izni verebilir.

## <a name="list-all-role-assignments"></a>Tüm rol atamalarını listesi
Belirtilen kapsam ve subscopes tüm rol atamalarını listeler.

Liste rol atamaları için erişimi olmalıdır `Microsoft.Authorization/roleAssignments/read` kapsamındaki işlemi. Yerleşik roller, bu işlem için erişim verilir. Rol atamaları ve Azure kaynakları için erişimi yönetme hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi](role-based-access-control-configure.md).

### <a name="request"></a>İstek
Kullanım **almak** yöntemi aşağıdaki URI ile:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

URI içinde isteğiniz özelleştirmek için aşağıdaki alternatifleri olun:

1. Değiştir *{kapsamı}* rol atamalarını listelemek istediğiniz kapsama sahip. Aşağıdaki örnekler, farklı düzeylerde kapsamını belirleme konusunda gösterir:

   * Abonelik: /subscriptions/ {subscrıptıon-ID}  
   * Kaynak grubu: /subscriptions/ {subscrıptıon-ID} / resourceGroups/myresourcegroup1  
   * Kaynak: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Değiştir *{api-version}* 2015-07-01 ile.
3. Değiştir *{filtre}* rol ataması listesini filtrelemek için uygulamak istediğiniz koşulu ile:

   * Rol atamaları subscopes olarak dahil değil yalnızca belirtilen kapsam için rol atamalarını listesi:`atScope()`    
   * Belirli bir kullanıcı, Grup veya uygulama için rol atamalarını listesi:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
   * Liste olanları gruplarından devralınan dahil olmak üzere belirli bir kullanıcı için rol atamalarını |`assignedTo('{objectId of user}')`

### <a name="response"></a>Yanıt
Durum kodu: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Bir rol ataması hakkında bilgi alın
Rol ataması tanımlayıcısı tarafından belirtilen bir tek bir rol ataması hakkında bilgi alır.

Bir rol ataması hakkında bilgi almak için erişimi olmalıdır `Microsoft.Authorization/roleAssignments/read` işlemi. Yerleşik roller, bu işlem için erişim verilir. Rol atamaları ve Azure kaynakları için erişimi yönetme hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi](role-based-access-control-configure.md).

### <a name="request"></a>İstek
Kullanım **almak** yöntemi aşağıdaki URI ile:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

URI içinde isteğiniz özelleştirmek için aşağıdaki alternatifleri olun:

1. Değiştir *{kapsamı}* rol atamalarını listelemek istediğiniz kapsama sahip. Aşağıdaki örnekler, farklı düzeylerde kapsamını belirleme konusunda gösterir:

   * Abonelik: /subscriptions/ {subscrıptıon-ID}  
   * Kaynak grubu: /subscriptions/ {subscrıptıon-ID} / resourceGroups/myresourcegroup1  
   * Kaynak: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Değiştir *{rol atama kimliği}* , rol atamasının GUID tanımlayıcısı.
3. Değiştir *{api-version}* 2015-07-01 ile.

### <a name="response"></a>Yanıt
Durum kodu: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Bir rol ataması oluşturma
Belirtilen rol verme belirtilen asıl için belirtilen kapsamda bir rol ataması oluşturun.

Bir rol ataması oluşturmak için erişimi olmalıdır `Microsoft.Authorization/roleAssignments/write` işlemi. Yerleşik roller, yalnızca *sahibi* ve *kullanıcı erişimi Yöneticisi* bu işlem için erişim verilir. Rol atamaları ve Azure kaynakları için erişimi yönetme hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi](role-based-access-control-configure.md).

### <a name="request"></a>İstek
Kullanım **PUT** yöntemi aşağıdaki URI ile:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

URI içinde isteğiniz özelleştirmek için aşağıdaki alternatifleri olun:

1. Değiştir *{kapsamı}* rol atamalarını oluşturmak istediğiniz kapsama sahip. Bir üst kapsamda bir rol ataması oluşturduğunuzda, tüm alt kapsamlar aynı rol ataması devralır. Aşağıdaki örnekler, farklı düzeylerde kapsamını belirleme konusunda gösterir:

   * Abonelik: /subscriptions/ {subscrıptıon-ID}  
   * Kaynak grubu: /subscriptions/ {subscrıptıon-ID} / resourceGroups/myresourcegroup1   
   * Kaynak: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Değiştir *{rol atama kimliği}* yeni bir GUID ile haline geldikten yeni rol ataması GUID tanımlayıcısı.
3. Değiştir *{api-version}* 2015-07-01 ile.

İstek gövdesi için değerleri aşağıdaki biçimde girin:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Öğe adı | Gerekli | Tür | Açıklama |
| --- | --- | --- | --- |
| roleDefinitionId |Evet |Dize |Rol tanımlayıcısı. Tanımlayıcı biçimdedir:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId |Evet |Dize |objectID rolü atanmış Azure AD asıl (kullanıcı, Grup veya hizmet sorumlusu). |

### <a name="response"></a>Yanıt
Durum kodu: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Bir rol atamasını silin
Belirtilen kapsamdaki rol atamasını silin.

Bir rol atamasını silmek için erişimi olmalıdır `Microsoft.Authorization/roleAssignments/delete` işlemi. Yerleşik roller, yalnızca *sahibi* ve *kullanıcı erişimi Yöneticisi* bu işlem için erişim verilir. Rol atamaları ve Azure kaynakları için erişimi yönetme hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi](role-based-access-control-configure.md).

### <a name="request"></a>İstek
Kullanım **silmek** yöntemi aşağıdaki URI ile:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

URI içinde isteğiniz özelleştirmek için aşağıdaki alternatifleri olun:

1. Değiştir *{kapsamı}* rol atamalarını oluşturmak istediğiniz kapsama sahip. Aşağıdaki örnekler, farklı düzeylerde kapsamını belirleme konusunda gösterir:

   * Abonelik: /subscriptions/ {subscrıptıon-ID}  
   * Kaynak grubu: /subscriptions/ {subscrıptıon-ID} / resourceGroups/myresourcegroup1  
   * Kaynak: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Değiştir *{rol atama kimliği}* rol ataması kimliği GUID.
3. Değiştir *{api-version}* 2015-07-01 ile.

### <a name="response"></a>Yanıt
Durum kodu: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Tüm rolleri listeler
Belirtilen kapsamda atama için kullanılabilen tüm rolleri listeler.

Liste rollere erişiminiz olmalıdır `Microsoft.Authorization/roleDefinitions/read` kapsamındaki işlemi. Yerleşik roller, bu işlem için erişim verilir. Rol atamaları ve Azure kaynakları için erişimi yönetme hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi](role-based-access-control-configure.md).

### <a name="request"></a>İstek
Kullanım **almak** yöntemi aşağıdaki URI ile:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

URI içinde isteğiniz özelleştirmek için aşağıdaki alternatifleri olun:

1. Değiştir *{kapsamı}* rollerini listelemek istediğiniz kapsama sahip. Aşağıdaki örnekler, farklı düzeylerde kapsamını belirleme konusunda gösterir:

   * Abonelik: /subscriptions/ {subscrıptıon-ID}  
   * Kaynak grubu: /subscriptions/ {subscrıptıon-ID} / resourceGroups/myresourcegroup1  
   * Kaynak /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Değiştir *{api-version}* 2015-07-01 ile.
3. Değiştir *{filtre}* rollerin listesini filtrelemek için uygulamak istediğiniz koşulu ile:

   * Liste roller kendi alt kapsamını atama için belirtilen kapsamda ve diğer kullanılabilir:`atScopeAndBelow()`
   * Tam ekran adını kullanarak bir rolü arayın: `roleName%20eq%20'{role-display-name}'`. Rolün tam görünen adı, URL kodlanmış form kullanın. Örneğin,`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Yanıt
Durum kodu: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Bir rolü hakkında bilgi edinin
Rol tanımı tanımlayıcısı tarafından belirtilen tek bir rol bilgilerini alır. Görünen adını kullanarak tek bir rol hakkında bilgi almak için bkz: [tüm rolleri listesinde](role-based-access-control-manage-access-rest.md#list-all-roles).

Bir rolü hakkında bilgi almak için erişimi olmalıdır `Microsoft.Authorization/roleDefinitions/read` işlemi. Yerleşik roller, bu işlem için erişim verilir. Rol atamaları ve Azure kaynakları için erişimi yönetme hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi](role-based-access-control-configure.md).

### <a name="request"></a>İstek
Kullanım **almak** yöntemi aşağıdaki URI ile:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

URI içinde isteğiniz özelleştirmek için aşağıdaki alternatifleri olun:

1. Değiştir *{kapsamı}* rol atamalarını listelemek istediğiniz kapsama sahip. Aşağıdaki örnekler, farklı düzeylerde kapsamını belirleme konusunda gösterir:

   * Abonelik: /subscriptions/ {subscrıptıon-ID}  
   * Kaynak grubu: /subscriptions/ {subscrıptıon-ID} / resourceGroups/myresourcegroup1  
   * Kaynak: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Değiştir *{rol tanımı kimliği}* , rol tanımı GUID tanımlayıcısı.
3. Değiştir *{api-version}* 2015-07-01 ile.

### <a name="response"></a>Yanıt
Durum kodu: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Özel bir rol oluşturun
Özel bir rol oluşturun.

Özel bir rol oluşturmak için erişimi olmalıdır `Microsoft.Authorization/roleDefinitions/write` tüm işlemi `AssignableScopes`. Yerleşik roller, yalnızca *sahibi* ve *kullanıcı erişimi Yöneticisi* bu işlem için erişim verilir. Rol atamaları ve Azure kaynakları için erişimi yönetme hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi](role-based-access-control-configure.md).

### <a name="request"></a>İstek
Kullanım **PUT** yöntemi aşağıdaki URI ile:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

URI içinde isteğiniz özelleştirmek için aşağıdaki alternatifleri olun:

1. Değiştir *{kapsamı}* ilk ile *AssignableScope* özel rolü. Aşağıdaki örnekler, farklı düzeylerde için kapsamı belirtin gösterilmektedir.

   * Abonelik: /subscriptions/ {subscrıptıon-ID}  
   * Kaynak grubu: /subscriptions/ {subscrıptıon-ID} / resourceGroups/myresourcegroup1  
   * Kaynak: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Değiştir *{rol tanımı kimliği}* yeni bir GUID ile haline geldikten yeni özel rol GUID tanımlayıcısı.
3. Değiştir *{api-version}* 2015-07-01 ile.

İstek gövdesi için değerleri aşağıdaki biçimde girin:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Öğe adı | Gerekli | Tür | Açıklama |
| --- | --- | --- | --- |
| ad |Evet |Dize |Özel rol GUID tanımlayıcısı. |
| properties.roleName |Evet |Dize |Özel rol adını görüntüler. En büyük boyutu 128 karakter. |
| properties.description |Hayır |Dize |Özel rol tanımı. En büyük boyutu 1024 karakter. |
| properties.type |Evet |Dize |"CustomRole" ayarlayın |
| properties.permissions.actions |Evet |String[] |Özel rol tarafından verilen operations belirten bir eylem dizeler dizisi. |
| properties.permissions.notActions |Hayır |String[] |Özel rol tarafından verilen Operations dışarıda bırakılacak işlemler belirten bir eylem dizeler dizisi. |
| properties.assignableScopes |Evet |String[] |Özel rol kullanılabilir kapsamları dizisi. |

### <a name="response"></a>Yanıt
Durum kodu: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Özel bir rol güncelleştirme
Özel bir rol değiştirin.

Özel bir rol değiştirmek için erişimi olmalıdır `Microsoft.Authorization/roleDefinitions/write` tüm işlemi `AssignableScopes`. Yerleşik roller, yalnızca *sahibi* ve *kullanıcı erişimi Yöneticisi* bu işlem için erişim verilir. Rol atamaları ve Azure kaynakları için erişimi yönetme hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi](role-based-access-control-configure.md).

### <a name="request"></a>İstek
Kullanım **PUT** yöntemi aşağıdaki URI ile:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

URI içinde isteğiniz özelleştirmek için aşağıdaki alternatifleri olun:

1. Değiştir *{kapsamı}* ilk ile *AssignableScope* özel rolü. Aşağıdaki örnekler, farklı düzeylerde kapsamını belirleme konusunda gösterir:

   * Abonelik: /subscriptions/ {subscrıptıon-ID}  
   * Kaynak grubu: /subscriptions/ {subscrıptıon-ID} / resourceGroups/myresourcegroup1  
   * Kaynak: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Değiştir *{rol tanımı kimliği}* , özel rolünün bir GUID tanımlayıcısı.
3. Değiştir *{api-version}* 2015-07-01 ile.

İstek gövdesi için değerleri aşağıdaki biçimde girin:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Öğe adı | Gerekli | Tür | Açıklama |
| --- | --- | --- | --- |
| ad |Evet |Dize |Özel rol GUID tanımlayıcısı. |
| properties.roleName |Evet |Dize |Güncelleştirilmiş özel rol adını görüntüler. |
| properties.description |Hayır |Dize |Güncelleştirilmiş özel rol tanımı. |
| properties.type |Evet |Dize |"CustomRole" ayarlayın |
| properties.permissions.actions |Evet |String[] |Güncelleştirilmiş özel rol erişim verdiği operations belirten bir eylem dizeler dizisi. |
| properties.permissions.notActions |Hayır |String[] |Güncelleştirilmiş özel rol verir işlemlerinden hariç tutulacak işlemler belirten bir eylem dizeler dizisi. |
| properties.assignableScopes |Evet |String[] |Güncelleştirilmiş özel rol kullanılabilir kapsamları dizisi. |

### <a name="response"></a>Yanıt
Durum kodu: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Bir özel rolü silmeyi
Özel bir rol silin.

Bir özel rolü silmek için erişimi olmalıdır `Microsoft.Authorization/roleDefinitions/delete` tüm işlemi `AssignableScopes`. Yerleşik roller, yalnızca *sahibi* ve *kullanıcı erişimi Yöneticisi* bu işlem için erişim verilir. Rol atamaları ve Azure kaynakları için erişimi yönetme hakkında daha fazla bilgi için bkz: [Azure rol tabanlı erişim denetimi](role-based-access-control-configure.md).

### <a name="request"></a>İstek
Kullanım **silmek** yöntemi aşağıdaki URI ile:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

URI içinde isteğiniz özelleştirmek için aşağıdaki alternatifleri olun:

1. Değiştir *{kapsamı}* rol tanımını silmek istediğiniz kapsama sahip. Aşağıdaki örnekler, farklı düzeylerde kapsamını belirleme konusunda gösterir:

   * Abonelik: /subscriptions/ {subscrıptıon-ID}  
   * Kaynak grubu: /subscriptions/ {subscrıptıon-ID} / resourceGroups/myresourcegroup1  
   * Kaynak: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  
2. Değiştir *{rol tanımı kimliği}* özel rol GUID rol tanımı kimliği.
3. Değiştir *{api-version}* 2015-07-01 ile.

### <a name="response"></a>Yanıt
Durum kodu: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
