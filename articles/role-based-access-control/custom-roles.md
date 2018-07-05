---
title: Azure'da özel roller | Microsoft Docs
description: Özel roller içeren Azure rol tabanlı erişim denetimi (RBAC) tanımlamak için ayrıntılı erişim yönetimi, Azure kaynaklarında öğrenin.
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
ms.date: 06/12/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 446cb34f2de8d0de3ee52e23df6cd26644d31bba
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435979"
---
# <a name="custom-roles-in-azure"></a>Azure'da özel roller

[Yerleşik roller](built-in-roles.md) kuruluşunuzun ihtiyaçlarını karşılamıyorsa kendi özel rollerinizi oluşturabilirsiniz. Yerleşik roller gibi yalnızca kullanıcıları, grupları ve abonelik, kaynak grubu ve kaynak kapsamları hizmet sorumluları için özel roller atayabilirsiniz. Özel roller, bir Azure Active Directory (Azure AD) kiracısı içinde depolanır ve abonelikler arasında paylaşılabilir. Her Kiracı en fazla 2000 özel roller olabilir. Özel roller, Azure PowerShell, Azure CLI veya REST API'yi kullanarak oluşturulabilir.

## <a name="custom-role-example"></a>Özel rol örneği

Aşağıda gösterildiği Azure PowerShell kullanarak sanal makinelerin yeniden başlatılmasından ve izleme için özel bir rol gösterilmiştir:

```json
{
  "Name":  "Virtual Machine Operator",
  "Id":  "88888888-8888-8888-8888-888888888888",
  "IsCustom":  true,
  "Description":  "Can monitor and restart virtual machines.",
  "Actions":  [
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
  "NotActions":  [

                 ],
  "DataActions":  [

                  ],
  "NotDataActions":  [

                     ],
  "AssignableScopes":  [
                           "/subscriptions/{subscriptionId1}",
                           "/subscriptions/{subscriptionId2}",
                           "/subscriptions/{subscriptionId3}"
                       ]
}
```

Özel bir rol oluşturduğunuzda, Azure portalında bir kaynak Turuncu simge ile görünür.

![Özel rol simgesi](./media/custom-roles/roles-custom-role-icon.png)

## <a name="steps-to-create-a-custom-role"></a>Özel rol oluşturma adımları

1. İhtiyaç duyduğunuz izinleri belirleyin

    Özel bir rol oluşturduğunuzda, kaynak izinlerinizi tanımlamak kullanılabilen sağlayıcısı işlemleri bilmeniz gerekir. İşlemlerin listesini görüntülemek için kullanabileceğiniz [Get-AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) veya [az sağlayıcı işlemi listesi](/cli/azure/provider/operation#az-provider-operation-list) komutları.
    Özel rolünüz için izinleri belirlemek için işlemleri ekleyin. `actions` veya `notActions` özelliklerini [rol tanımı](role-definitions.md). Veri işlemleri varsa, bu ekleme `dataActions` veya `notDataActions` özellikleri.

2. Özel rol oluşturma

    Özel rol oluşturmak için Azure PowerShell veya Azure CLI'yı kullanabilirsiniz. Genellikle, mevcut bir yerleşik rolü ile başlayın ve sonra gereksinimleriniz için değiştirin. Kullanmanız [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) veya [az rol tanımını oluşturma](/cli/azure/role/definition#az-role-definition-create) özel rolü oluşturmak için komutları. Özel bir rol oluşturmak için olmalıdır `Microsoft.Authorization/roleDefinitions/write` tüm izin `assignableScopes`, gibi [sahibi](built-in-roles.md#owner) veya [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator).

3. Özel rol testi

    Özel rolünüz olduktan sonra beklendiği gibi çalıştığını doğrulamak için test etmek kullanabilirsiniz. Ayarlamaların yapılması gerekiyorsa, özel rol güncelleştirebilirsiniz.

## <a name="custom-role-properties"></a>Özel rol özellikleri

Özel bir rol, aşağıdaki özelliklere sahiptir.

| Özellik | Gerekli | Tür | Açıklama |
| --- | --- | --- | --- |
| `Name` | Evet | Dize | Özel rol görünen adı. Kiracınız için benzersiz olmalıdır. Harf, sayı, boşluk ve özel karakterler içerebilir. En fazla karakter sayısı 128'dir. |
| `Id` | Evet | Dize | Özel rol benzersiz kimliği. Yeni bir rol oluşturduğunuzda, Azure PowerShell ve Azure CLI için bu kimliği otomatik olarak oluşturulur. |
| `IsCustom` | Evet | Dize | Bu özel bir rol olup olmadığını gösterir. Kümesine `true` özel roller için. |
| `Description` | Evet | Dize | Özel rol tanımı. Harf, sayı, boşluk ve özel karakterler içerebilir. En fazla karakter sayısı 1024'tür. |
| `Actions` | Evet | String[] | Rol gerçekleştirilmesine izin veren yönetim işlemleri belirten bir dize dizisi. Daha fazla bilgi için [eylemleri](role-definitions.md#actions). |
| `NotActions` | Hayır | String[] | Hariç tutulan yönetim işlemleri belirten bir dize dizisi izin verilen gelen `actions`. Daha fazla bilgi için [notActions](role-definitions.md#notactions). |
| `DataActions` | Hayır | String[] | Bu nesnenin içinde verilerinizin gerçekleştirilecek rolü sağlar veri işlemleri belirten bir dize dizisi. Daha fazla bilgi için [dataActions (Önizleme)](role-definitions.md#dataactions-preview). |
| `NotDataActions` | Hayır | String[] | Hariç tutulan veri işlemleri belirten bir dize dizisi izin verilen gelen `dataActions`. Daha fazla bilgi için [notDataActions (Önizleme)](role-definitions.md#notdataactions-preview). |
| `AssignableScopes` | Evet | String[] | Özel rol atama için kullanılabilir olduğunu kapsamları belirten bir dize dizisi. Kök kapsam ayarlanamaz (`"/"`). Daha fazla bilgi için [assignableScopes](role-definitions.md#assignablescopes). |

## <a name="assignablescopes-for-custom-roles"></a>özel roller için assignableScopes

Yerleşik roller'olduğu gibi `assignableScopes` özellik kapsamları rol atama için kullanılabilir olduğunu belirtir. Ancak, kök kapsam kullanamazsınız? (`"/"`) kendi özel roller. Denerseniz, bir Yetkilendirme hatası alırsınız. `assignableScopes` Özelliği özel bir rol için de denetimleri kimlerin oluşturma, silme, değiştirme veya özel rolü görüntüleyin.

| Görev | İşlem | Açıklama |
| --- | --- | --- |
| Özel rol oluşturma/silme | `Microsoft.Authorization/ roleDefinition/write` | Bu işlem tüm izni verilen kullanıcıları `assignableScopes` özel rolü (Sil bu kapsamları kullanmak için özel roller ya da oluşturabilmeleri). Örneğin, [sahipleri](built-in-roles.md#owner) ve [kullanıcı erişim yöneticileri](built-in-roles.md#user-access-administrator) abonelikler, kaynak grupları ve kaynaklar. |
| Özel bir rolü Değiştir | `Microsoft.Authorization/ roleDefinition/write` | Bu işlem tüm izni verilen kullanıcıları `assignableScopes` özel rolü bu kapsamlarda özel rolleri değiştirebilirsiniz. Örneğin, [sahipleri](built-in-roles.md#owner) ve [kullanıcı erişim yöneticileri](built-in-roles.md#user-access-administrator) abonelikler, kaynak grupları ve kaynaklar. |
| Özel bir rol görüntüleyin | `Microsoft.Authorization/ roleDefinition/read` | Bu işlem bir kapsamda izni verilen kullanıcıları bu kapsamda atama için uygun olan özel roller görüntüleyebilirsiniz. Tüm yerleşik roller özel roller atama için kullanılabilir olmasını sağlar. |

## <a name="next-steps"></a>Sonraki adımlar
- [Azure PowerShell kullanarak özel roller oluşturma](custom-roles-powershell.md)
- [Azure CLI'yı kullanarak özel roller oluşturma](custom-roles-cli.md)
- [Rol tanımları anlama](role-definitions.md)
