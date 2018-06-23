---
title: Azure özel roller | Microsoft Docs
description: Azure rol tabanlı erişim denetimi (RBAC) ile özel roller tanımlamak için Azure kaynaklarında ayrıntılı erişim yönetimi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/12/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 074c305cb15bc1fb25dfa5cfc52dcce53b661a7e
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321181"
---
# <a name="custom-roles-in-azure"></a>Azure özel roller

Varsa [yerleşik roller](built-in-roles.md) kuruluşunuzun belirli gereksinimlerine uymayan, kendi özel roller oluşturabilirsiniz. Yalnızca yerleşik roller gibi kullanıcıları, grupları ve hizmet asıl adı abonelik, kaynak grubu ve kaynak kapsamları özel roller atayabilirsiniz. Özel roller, bir Azure Active Directory (Azure AD) Kiracı depolanır ve abonelikler arasında paylaşılabilir. Her bir kiracı en fazla 2000 özel rollere sahip olabilir. Özel roller, Azure PowerShell, Azure CLI veya REST API kullanarak oluşturulabilir.

## <a name="custom-role-example"></a>Özel rol örneği

Aşağıdaki izleme ve Azure PowerShell kullanarak görüntülendiği gibi sanal makine yeniden başlatma için özel bir rol gösterir:

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

Özel bir rol oluşturduktan sonra kaynak Turuncu simge ile Azure portalında görünür.

![Özel rol simgesi](./media/custom-roles/roles-custom-role-icon.png)

## <a name="steps-to-create-a-custom-role"></a>Özel bir rol oluşturma adımları

1. Gereksinim duyduğunuz izinleri belirler

    Özel bir rol oluşturduğunuzda, kaynak izinlerinizi tanımlamak kullanılabilen sağlayıcısı işlemleri bilmeniz gerekir. İşlemlerin listesini görüntülemek için kullanabileceğiniz [Get-AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) veya [az sağlayıcı işlemi listesi](/cli/azure/provider/operation#az-provider-operation-list) komutları.
    Özel rol için izinleri belirtmek için işlemlerini ekleyin `actions` veya `notActions` özelliklerini [rol tanımı](role-definitions.md). Veri işlemi varsa, bu ekleyin `dataActions` veya `notDataActions` özellikleri.

2. Özel rol oluşturun

    Özel rol oluşturmak için Azure PowerShell veya Azure CLI kullanın. Genellikle, varolan bir yerleşik rolüyle başlatın ve gereksinimlerinize göre değiştirin. Kullandığınız sonra [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) veya [az rol tanımı oluşturma](/cli/azure/role/definition#az-role-definition-create) özel rol oluşturmak için komutları. Özel bir rol oluşturmak için ihtiyacınız `Microsoft.Authorization/roleDefinitions/write` izni tüm `assignableScopes`, gibi [sahibi](built-in-roles.md#owner) veya [kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator).

3. Özel rol testi

    Özel rol olduktan sonra beklendiği gibi çalıştığını doğrulamak için test etmek kullanabilirsiniz. Değişiklikler yapılması gerekiyorsa, özel rol güncelleştirebilirsiniz.

## <a name="custom-role-properties"></a>Özel rol özellikleri

Özel bir rol, aşağıdaki özelliklere sahiptir.

| Özellik | Gerekli | Tür | Açıklama |
| --- | --- | --- | --- |
| `Name` | Evet | Dize | Özel rol görünen adı. Kiracınız için benzersiz olmalıdır. Harf, rakam, boşluk ve özel karakterler içerebilir. En fazla karakter sayısı 128'dir. |
| `Id` | Evet | Dize | Özel rol benzersiz kimliği. Yeni bir rol oluşturduğunuzda, Azure PowerShell ve Azure CLI için bu kodu otomatik olarak oluşturulur. |
| `IsCustom` | Evet | Dize | Bu özel bir rol olup olmadığını gösterir. Kümesine `true` özel roller için. |
| `Description` | Evet | Dize | Özel rol tanımı. Harf, rakam, boşluk ve özel karakterler içerebilir. En fazla karakter sayısını 1024'tür. |
| `Actions` | Evet | String[] | Rol gerçekleştirilmesine izin veren yönetim işlemlerini belirten bir dizeler dizisi. Daha fazla bilgi için bkz: [Eylemler](role-definitions.md#actions). |
| `NotActions` | Hayır | String[] | Hariç tutulan yönetim işlemlerini belirten bir dizeler dizisi izin verilen gelen `actions`. Daha fazla bilgi için bkz: [notActions](role-definitions.md#notactions). |
| `DataActions` | Hayır | String[] | Bu nesne içinde verilerinize gerçekleştirilecek rolü sağlar veri işlemleri belirten bir dizeler dizisi. Daha fazla bilgi için bkz: [dataActions (Önizleme)](role-definitions.md#dataactions-preview). |
| `NotDataActions` | Hayır | String[] | Hariç tutulan veri işlemleri belirten bir dizeler dizisi izin verilen gelen `dataActions`. Daha fazla bilgi için bkz: [notDataActions (Önizleme)](role-definitions.md#notdataactions-preview). |
| `AssignableScopes` | Evet | String[] | Özel rol atama için uygun olduğunu kapsamları belirten bir dizeler dizisi. Kök kapsamına ayarlanamaz (`"/"`). Daha fazla bilgi için bkz: [assignableScopes](role-definitions.md#assignablescopes). |

## <a name="assignablescopes-for-custom-roles"></a>özel roller için assignableScopes

Yerleşik roller'olduğu gibi `assignableScopes` özelliği, kapsamları rol atama için uygun olduğunu belirtir. Ancak, kök kapsamı kullanılamaz (`"/"`), kendi özel rolleri. Denerseniz, bir Yetkilendirme hatası alırsınız. `assignableScopes` Özelliği özel bir rol için ayrıca denetler kimin oluşturmak, silebilir, değiştirme veya özel rol görüntüleyin.

| Görev | İşlem | Açıklama |
| --- | --- | --- |
| Özel bir rol oluşturma/silme | `Microsoft.Authorization/ roleDefinition/write` | Bu işlem tüm izni verilen kullanıcıları `assignableScopes` özel rolü oluşturun (silmek bu kapsamları kullanmak için özel roller veya). Örneğin, [sahipleri](built-in-roles.md#owner) ve [kullanıcı erişim yöneticileri](built-in-roles.md#user-access-administrator) Abonelikleri, kaynak grupları ve kaynakları. |
| Özel bir rol değiştirme | `Microsoft.Authorization/ roleDefinition/write` | Bu işlem tüm izni verilen kullanıcıları `assignableScopes` özel rolü bu kapsamları özel rollerinde değiştirebilirsiniz. Örneğin, [sahipleri](built-in-roles.md#owner) ve [kullanıcı erişim yöneticileri](built-in-roles.md#user-access-administrator) Abonelikleri, kaynak grupları ve kaynakları. |
| Özel bir rol görüntüleyin | `Microsoft.Authorization/ roleDefinition/read` | Bu işlem bir kapsamda izni verilen kullanıcıları bu kapsamda atama için uygun olan özel roller görüntüleyebilirsiniz. Tüm yerleşik roller atama için uygun olması özel rollere izin verin. |

## <a name="next-steps"></a>Sonraki adımlar
- [Azure PowerShell kullanarak özel roller oluşturma](custom-roles-powershell.md)
- [Azure CLI kullanarak özel roller oluşturma](custom-roles-cli.md)
- [Rol tanımları anlayın](role-definitions.md)
