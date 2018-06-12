---
title: Azure RBAC için özel roller oluşturmanızı | Microsoft Docs
description: Azure aboneliğinizde daha kesin kimlik yönetimi için Azure rol tabanlı erişim denetimi ile özel rolleri tanımlama öğrenin.
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
ms.date: 05/12/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3baf616e448f1f6d5292161ae125502d72141940
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266603"
---
# <a name="create-custom-roles-in-azure"></a>Özel roller oluşturma

Varsa [yerleşik roller](built-in-roles.md) belirli erişim gereksinimlerinizi karşılamadığını, kendi özel roller oluşturabilirsiniz. Yalnızca yerleşik roller gibi kullanıcıları, grupları ve hizmet asıl adı abonelik, kaynak grubu ve kaynak kapsamları özel roller atayabilirsiniz. Özel roller, bir Azure Active Directory (Azure AD) Kiracı depolanır ve abonelikler arasında paylaşılabilir. Her bir kiracı en fazla 2000 özel rollere sahip olabilir. Özel roller, Azure PowerShell, Azure CLI veya REST API kullanarak oluşturulabilir.

Bu makalede PowerShell ve Azure CLI kullanarak özel roller oluşturmaya başlamak nasıl bir örneği.

## <a name="create-a-custom-role-to-open-support-requests-using-powershell"></a>PowerShell kullanarak destek istekleri açmak için özel bir rol oluşturun

Özel bir rol oluşturmak için sahip yerleşik bir rol Başlat, düzenlemek ve yeni bir rol oluşturmak. Bu örneğin, yerleşik [okuyucu](built-in-roles.md#reader) rol özelleştirilmiş "erişim düzeyi okuyucu destek biletlerini" adlı özel bir rol oluşturmak için. Kullanıcının her şeyi abonelik açık destek istekleri de görüntüleyebilir ve olanak sağlar.

> [!NOTE]
> Destek isteklerini açmasına izin yalnızca iki yerleşik roller [sahibi](built-in-roles.md#owner) ve [katkıda bulunan](built-in-roles.md#contributor). Destek istekleri açmak bir kullanıcı için tüm destek isteklerini bir Azure aboneliğine yönelik temel alınarak oluşturulur çünkü kendisine abonelik kapsamında bir rol atanmalıdır.

PowerShell'de kullanın [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) dışa aktarmak için komutu [okuyucu](built-in-roles.md#reader) JSON biçiminde rol.

```azurepowershell
Get-AzureRmRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json
```

Aşağıdakiler için JSON çıktısını gösterir [okuyucu](built-in-roles.md#reader) rol. Tipik rol üç ana bölümden oluşan `Actions`, `NotActions`, ve `AssignableScopes`. `Actions` Bölümü rolü için izin verilen tüm işlemleri listeler. İşlemlerinden dışlanacak `Actions`, bunları Ekle `NotActions`. Etkili izinleri hesaplanan çıkarılmasıyla `NotActions` işlemlerinden `Actions` işlemleri.

```json
{
    "Name":  "Reader",
    "Id":  "acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "IsCustom":  false,
    "Description":  "Lets you view everything, but not make any changes.",
    "Actions":  [
                    "*/read"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

Ardından, çıkış özel rol oluşturmak için JSON düzenleyin. Bu durumda, destek oluşturmak için anahtarları `Microsoft.Support/*` işlemi eklenmelidir. Her işlem kaynak sağlayıcısından kullanılabilir hale getirilir. Bir kaynak sağlayıcısı için işlemleri listesini almak için kullanabilirsiniz [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) komutunu veya bkz [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md).

Rolü açık abonelik kimlikleri kullanıldığı içeren zorunludur. Abonelik kimlikleri altında listelenen `AssignableScopes`, aksi takdirde, aboneliğinizi rolünü içeri aktarmak için izin verilmez.

Son olarak, ayarlamalısınız `IsCustom` özelliğine `true` bu özel bir rol olduğunu belirtmek için.

```json
{
    "Name":  "Reader support tickets access level",
    "IsCustom":  true,
    "Description":  "View everything in the subscription and also open support requests.",
    "Actions":  [
                    "*/read",
                    "Microsoft.Support/*"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes":  [
                             "/subscriptions/11111111-1111-1111-1111-111111111111"
                         ]
}
```

Yeni özel rol oluşturmak için kullandığınız [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) komut ve güncelleştirilmiş JSON rol tanımı dosyası sağlayın.

```azurepowershell
New-AzureRmRoleDefinition -InputFile "C:\rbacrole2.json"
```

Çalıştırdıktan sonra [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition), yeni özel rol Azure portalında kullanılabilir ve kullanıcılara atanabilir.

![Azure portalında içeri özel rol ekran görüntüsü](./media/custom-roles/18.png)

![kullanıcı aynı dizinde özel alınan rol atama işleminin ekran görüntüsü](./media/custom-roles/19.png)

![Özel alınan rol izinlerini ekran görüntüsü](./media/custom-roles/20.png)

Bu özel rolü olan kullanıcılar yeni destek istekleri oluşturabilirsiniz.

![Özel rol destek istekleri oluşturma ekran görüntüsü](./media/custom-roles/21.png)

Bu özel rolü olan kullanıcılar başka eylemler gerçekleştirebilir, gibi VM'ler oluşturulamıyor veya kaynak grupları oluşturun.

![ekran görüntüsü özel rol VM'ler oluşturmak mümkün değil](./media/custom-roles/22.png)

![ekran görüntüsü özel rol yeni RGs oluşturmak mümkün değil](./media/custom-roles/23.png)

## <a name="create-a-custom-role-to-open-support-requests-using-azure-cli"></a>Azure CLI kullanarak destek istekleri açmak için özel bir rol oluşturun

JSON çıktısını farklı olması dışında PowerShell kullanarak Azure CLI kullanarak özel bir rol oluşturmak için aşağıdaki adımları benzerdir.

Bu örnekte, yerleşik ile başlayabilirsiniz [okuyucu](built-in-roles.md#reader) rol. Eylemleri listelemek için [okuyucu](built-in-roles.md#reader) rolü, kullanım [az rol tanımı listesi](/cli/azure/role/definition#az_role_definition_list) komutu.

```azurecli
az role definition list --name "Reader" --output json
```

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you view everything, but not make any changes.",
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "permissions": [
      {
        "actions": [
          "*/read"
        ],
        "additionalProperties": {},
        "notActions": [],
      }
    ],
    "roleName": "Reader",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Aşağıdaki biçimde bir JSON dosyası oluşturun. `Microsoft.Support/*` İşlemi eklendi `Actions` bu kullanıcı bir okuyucunun olmasını devam ederken destek istekleri açabileceği bölümler. Burada bu rolü kullanılacak, abonelik kimliği eklemelisiniz `AssignableScopes` bölümü.

```json
{
    "Name":  "Reader support tickets access level",
    "IsCustom":  true,
    "Description":  "View everything in the subscription and also open support requests.",
    "Actions":  [
                    "*/read",
                    "Microsoft.Support/*"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes": [
                            "/subscriptions/11111111-1111-1111-1111-111111111111"
                        ]
}
```

Yeni özel rol oluşturmak için kullanmak [az rol tanımı oluşturma](/cli/azure/role/definition#az_role_definition_create) komutu.

```azurecli
az role definition create --role-definition ~/roles/rbacrole1.json
```

Yeni özel rol Azure portalında kullanılabilir ve bu rolü kullanmak için önceki PowerShell bölüm aynı işlemidir.

![CLI 1.0 kullanılarak oluşturulan özel rol Azure portal ekran görüntüsü](./media/custom-roles/26.png)


## <a name="see-also"></a>Ayrıca bkz.
- [Rol tanımları anlayın](role-definitions.md)
- [Rol tabanlı erişim denetimi AzurePowerShell ile yönetme](role-assignments-powershell.md)
- [Rol tabanlı erişim denetimini Azure CLI ile yönetme](role-assignments-cli.md)
- [Rol tabanlı erişim denetimini REST API ile yönetme](role-assignments-rest.md)
