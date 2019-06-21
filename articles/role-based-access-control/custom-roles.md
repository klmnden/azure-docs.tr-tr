---
title: Azure kaynakları için özel roller | Microsoft Docs
description: Rol tabanlı erişim denetimi (RBAC) ile özel roller oluşturma işlemini Azure kaynakları için ayrıntılı erişim yönetimi bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/07/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8b628086a67f1d76357fda4f753350b6411b8f15
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273724"
---
# <a name="custom-roles-for-azure-resources"></a>Azure kaynakları için özel roller

Varsa [Azure kaynakları için yerleşik roller](built-in-roles.md) kuruluşunuzun belirli gereksinimlerine uymayan, kendi özel rollerinizi oluşturabilirsiniz. Yerleşik roller gibi yalnızca kullanıcıları, grupları ve abonelik, kaynak grubu ve kaynak kapsamları hizmet sorumluları için özel roller atayabilirsiniz.

Özel roller, bir Azure Active Directory (Azure AD) dizinde depolanır ve abonelikler arasında paylaşılabilir. Her dizini kadar olabilir **5000** özel roller. (Azure Kamu, Azure Almanya ve Azure Çin 21Vianet gibi özel Bulutlar için 2000 özel rol sınırı vardır.) Özel roller, Azure PowerShell, Azure CLI veya REST API'yi kullanarak oluşturulabilir.

## <a name="custom-role-example"></a>Özel rol örneği

Aşağıdaki özel bir rol JSON biçiminde gösterilen gibi göründüğünü gösterir. Bu özel rolü, izleme ve sanal makinelerin yeniden başlatılmasından için kullanılabilir.

```json
{
  "Name": "Virtual Machine Operator",
  "Id": "88888888-8888-8888-8888-888888888888",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.ResourceHealth/availabilityStatuses/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/{subscriptionId1}",
    "/subscriptions/{subscriptionId2}",
    "/subscriptions/{subscriptionId3}"
  ]
}
```

Özel bir rol oluşturduğunuzda, Azure portalında bir kaynak Turuncu simge ile görünür.

![Özel rol simgesi](./media/custom-roles/roles-custom-role-icon.png)

## <a name="steps-to-create-a-custom-role"></a>Özel rol oluşturma adımları

1. Nasıl özel rolü oluşturmak istediğinize karar verin

    Kullanarak özel roller oluşturabilirsiniz [Azure PowerShell](custom-roles-powershell.md), [Azure CLI](custom-roles-cli.md), veya [REST API](custom-roles-rest.md).

1. İhtiyaç duyduğunuz izinleri belirleyin

    Özel bir rol oluşturduğunuzda, kaynak izinlerinizi tanımlamak kullanılabilen sağlayıcısı işlemleri bilmeniz gerekir. İşlemlerin listesini görüntülemek için bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md). İşlemleri ekleyeceksiniz `Actions` veya `NotActions` özelliklerini [rol tanımı](role-definitions.md). Veri işlemleri varsa, bu ekleyeceksiniz `DataActions` veya `NotDataActions` özellikleri.

1. Özel rol oluşturma

    Genellikle, mevcut bir yerleşik rolü ile başlayın ve sonra gereksinimleriniz için değiştirin. Kullanmanız [yeni AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) veya [az rol tanımını oluşturma](/cli/azure/role/definition#az-role-definition-create) özel rolü oluşturmak için komutları. Özel bir rol oluşturmak için olmalıdır `Microsoft.Authorization/roleDefinitions/write` tüm izin `AssignableScopes`, gibi [sahibi](built-in-roles.md#owner) veya [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator).

1. Özel rol testi

    Özel rolünüz olduktan sonra beklendiği gibi çalıştığını doğrulamak için test etmek kullanabilirsiniz. Daha sonra ayarlamalar yapmanız gerekiyorsa, özel rol güncelleştirebilirsiniz.

Özel rol oluşturma hakkında adım adım bir öğretici için bkz: [Öğreticisi: Azure PowerShell kullanarak özel bir rol oluşturun](tutorial-custom-role-powershell.md) veya [Öğreticisi: Azure CLI kullanarak bir özel rol oluşturma](tutorial-custom-role-cli.md).

## <a name="custom-role-properties"></a>Özel rol özellikleri

Özel bir rol, aşağıdaki özelliklere sahiptir.

| Özellik | Gerekli | Tür | Açıklama |
| --- | --- | --- | --- |
| `Name` | Evet | String | Özel rol görünen adı. Rol tanımı bir abonelik düzeyinde kaynak olsa da, bir rol tanımı aynı Azure AD dizini paylaşan birden çok abonelik içinde kullanılabilir. Bu görünen ad, Azure AD dizini kapsamında benzersiz olması gerekir. Harf, sayı, boşluk ve özel karakterler içerebilir. En fazla karakter sayısı 128'dir. |
| `Id` | Evet | String | Özel rol benzersiz kimliği. Yeni bir rol oluşturduğunuzda, Azure PowerShell ve Azure CLI için bu kimliği otomatik olarak oluşturulur. |
| `IsCustom` | Evet | String | Bu özel bir rol olup olmadığını gösterir. Kümesine `true` özel roller için. |
| `Description` | Evet | String | Özel rol tanımı. Harf, sayı, boşluk ve özel karakterler içerebilir. En fazla karakter sayısı 1024'tür. |
| `Actions` | Evet | String[] | Rol gerçekleştirilmesine izin veren yönetim işlemleri belirten bir dize dizisi. Daha fazla bilgi için [eylemleri](role-definitions.md#actions). |
| `NotActions` | Hayır | String[] | Hariç tutulan yönetim işlemleri belirten bir dize dizisi izin verilen gelen `Actions`. Daha fazla bilgi için [NotActions](role-definitions.md#notactions). |
| `DataActions` | Hayır | String[] | Bu nesnenin içinde verilerinizin gerçekleştirilecek rolü sağlar veri işlemleri belirten bir dize dizisi. Daha fazla bilgi için [DataActions](role-definitions.md#dataactions). |
| `NotDataActions` | Hayır | String[] | Hariç tutulan veri işlemleri belirten bir dize dizisi izin verilen gelen `DataActions`. Daha fazla bilgi için [NotDataActions](role-definitions.md#notdataactions). |
| `AssignableScopes` | Evet | String[] | Özel rol atama için kullanılabilir olduğunu kapsamları belirten bir dize dizisi. Özel roller için şu anda ayarlanamıyor `AssignableScopes` kök kapsam (`"/"`) veya bir yönetim grubu kapsamı. Daha fazla bilgi için [AssignableScopes](role-definitions.md#assignablescopes) ve [kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../governance/management-groups/index.md#custom-rbac-role-definition-and-assignment). |

## <a name="who-can-create-delete-update-or-view-a-custom-role"></a>Kimlerin oluşturma, silme, güncelleştirme veya özel bir rol görüntülemek

Yerleşik roller'olduğu gibi `AssignableScopes` özellik kapsamları rol atama için kullanılabilir olduğunu belirtir. `AssignableScopes` Özelliği özel bir rol için de denetimleri kimlerin oluşturma, silme, güncelleştirme veya özel rolü görüntüleyin.

| Görev | İşlem | Açıklama |
| --- | --- | --- |
| Özel rol oluşturma/silme | `Microsoft.Authorization/ roleDefinitions/write` | Bu işlem tüm izni verilen kullanıcıları `AssignableScopes` özel rolü (Sil bu kapsamları kullanmak için özel roller ya da oluşturabilmeleri). Örneğin, [sahipleri](built-in-roles.md#owner) ve [kullanıcı erişim yöneticileri](built-in-roles.md#user-access-administrator) abonelikler, kaynak grupları ve kaynaklar. |
| Özel rolü güncelleştirme | `Microsoft.Authorization/ roleDefinitions/write` | Bu işlem tüm izni verilen kullanıcıları `AssignableScopes` özel rolü bu kapsamlarda özel roller güncelleştirebilirsiniz. Örneğin, [sahipleri](built-in-roles.md#owner) ve [kullanıcı erişim yöneticileri](built-in-roles.md#user-access-administrator) abonelikler, kaynak grupları ve kaynaklar. |
| Özel bir rol görüntüleyin | `Microsoft.Authorization/ roleDefinitions/read` | Bu işlem bir kapsamda izni verilen kullanıcıları bu kapsamda atama için uygun olan özel roller görüntüleyebilirsiniz. Tüm yerleşik roller özel roller atama için kullanılabilir olmasını sağlar. |

## <a name="next-steps"></a>Sonraki adımlar
- [Azure PowerShell kullanarak Azure kaynakları için özel roller oluşturma](custom-roles-powershell.md)
- [Azure CLI kullanarak Azure kaynakları için özel roller oluşturma](custom-roles-cli.md)
- [Azure kaynakları için rol tanımları anlama](role-definitions.md)
- [RBAC, Azure kaynakları için sorun giderme](troubleshooting.md)
