---
title: Azure RBAC için özel roller oluşturmanızı | Microsoft Docs
description: Azure aboneliğinizde daha kesin kimlik yönetimi için Azure rol tabanlı erişim denetimi ile özel rolleri tanımlama öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/11/2017
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2871ff5eea8fb99040dfab2593d1640d79f51092
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2018
---
# <a name="create-custom-roles-for-azure-role-based-access-control"></a>Azure rol tabanlı erişim denetimi için özel roller oluşturma
Yerleşik roller hiçbiri belirli erişim gereksinimlerinizi karşılıyorsa özel bir rol Azure rol tabanlı erişim denetimi (RBAC) oluşturun. Özel roller kullanılarak oluşturulabilir [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure komut satırı arabirimi](role-based-access-control-manage-access-azure-cli.md) (CLI) ve [REST API](role-based-access-control-manage-access-rest.md). Yalnızca yerleşik roller gibi kullanıcılar, gruplar ve uygulamalar abonelik, kaynak grubu ve kaynak kapsamları özel roller atayabilirsiniz. Özel roller Azure AD kiracısı içinde depolanır ve abonelikler arasında paylaşılabilir.

Her bir kiracı en fazla 2000 özel roller oluşturabilirsiniz. 

Aşağıdaki örnek, izleme ve sanal makineleri yeniden başlatmayı için özel bir rol gösterir:

```json
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
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
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a>Eylemler
**Eylemler** özel bir rol özelliği rol için erişim verir Azure işlemleri belirtir. Azure kaynak sağlayıcıları güvenliği sağlanabilir işlemlerini tanımlayan işlemi dizelerin koleksiyonudur. İşlemi dizeleri izleyin biçimi `Microsoft.<ProviderName>/<ChildResourceType>/<action>`. Joker karakterler içeren işlemi dizeleri (\*) işlemi dizeyle eşleşen tüm işlemler için erişim verin. Örneğin:

* `*/read` verir tüm Azure kaynak sağlayıcıları tüm kaynak türleri için okuma erişimi.
* `Microsoft.Compute/*` tüm işlemler Microsoft.Compute kaynak sağlayıcısındaki tüm kaynak türleri için erişim verir.
* `Microsoft.Network/*/read` verir Azure Microsoft.Network kaynak Sağlayıcısı'nda tüm kaynak türleri için okuma erişimi.
* `Microsoft.Compute/virtualMachines/*` verir kaynak türleri için sanal makinelerin ve kendi alt tüm işlemleri erişin.
* `Microsoft.Web/sites/restart/Action` Web siteleri yeniden başlatmak için verir erişin.

Kullanım `Get-AzureRmProviderOperation` (PowerShell'de) veya `azure provider operations show` (içinde Azure CLI) Azure kaynak sağlayıcılarının listesi işlemleri için. Bu komutlar bir işlemi dizesi geçerli olduğunu doğrulayın ve joker işlemi dizeleri genişletmek için de kullanabilirsiniz.

```powershell
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![PowerShell ekran görüntüsü - Get-AzureRMProviderOperation](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```azurecli
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure CLI ekran görüntüsü - azure sağlayıcısı işlemleri göster "Microsoft.Compute/virtualMachines/ \* /eylem" ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
Kullanım **NotActions** izin vermek istediğiniz işlem kümesi daha kolay kısıtlı işlemleri hariç tutarak tanımlanmışsa özelliği. Özel bir rol tarafından verilen erişim çıkarılmasıyla hesaplanır **NotActions** işlemlerinden **Eylemler** işlemleri.

> [!NOTE]
> Bir kullanıcı bir işlemde dışlar bir rolü atanıp atanmadığını **NotActions**ve aynı işlem erişim bu işlemi gerçekleştirmek için kullanıcının izin veren ikinci bir rol atanır. **NotActions** reddetme değil kural – bu belirli işlemleri dışarıda gerektiğinde izin verilen işlem kümesi oluşturmak için yalnızca uygun bir yoldur.
>
>

## <a name="assignablescopes"></a>AssignableScopes
**AssignableScopes** özel rol özelliği, içinde özel rol atama için uygun kapsamları (abonelik, kaynak grupları veya kaynakları) belirtir. Özel rol ataması yalnızca abonelikleri veya gerektiren kaynak grupları kullanımına ve abonelik veya kaynak grupları geri kalanı için kullanıcı deneyimi dağıtmayı değil.

Geçerli atanabilir kapsamların örnekleri şunlardır:

* "/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624" - rolü iki Aboneliklerde atama için kullanılabilir hale getirir.
* "/ subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e" - kullanılabilir hale getirir rol ataması tek bir abonelik.
* "/ abonelikleri/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/ağ" - yalnızca ağ kaynak grubunda rol ataması için kullanılabilir hale getirir.

> [!NOTE]
> En az bir kullanmalısınız abonelik, kaynak grubu veya kaynak kimliği
>
>

## <a name="custom-roles-access-control"></a>Özel roller erişim denetimi
**AssignableScopes** özel rolünün özelliği de denetimleri kimin görüntüleyin, değiştirin ve rol silin.

* Kimin özel bir rol oluşturabilir miyim?
    Sahiplerinin (ve kullanıcı erişim Yöneticileri) Abonelikleri, kaynak grupları ve kaynakları kullanmak için özel roller bu kapsamları oluşturabilirsiniz.
    Gerçekleştirmek kullanıcı rolü oluşturma gerekir `Microsoft.Authorization/roleDefinition/write` tüm işlemi **AssignableScopes** rolünün.
* Özel bir rol değiştirebilecekleri?
    Sahiplerinin (ve kullanıcı erişim Yöneticileri) Abonelikleri, kaynak grupları ve kaynakları bu kapsamları özel rollerinde değiştirebilirsiniz. Kullanıcıların gerekir arayabilmesi `Microsoft.Authorization/roleDefinition/write` tüm işlemi **AssignableScopes** özel bir rol.
* Kimin özel roller görebilir?
    Azure RBAC yerleşik rolleri tüm rollerin atama için uygun olmayan görüntüleme izin verin. Gerçekleştirebileceğiniz kullanıcılar `Microsoft.Authorization/roleDefinition/read` bir kapsamda işlemi bu kapsamda atama için kullanılabilir olan RBAC rolü görüntüleyebilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
* [Rol tabanlı erişim denetimi](role-based-access-control-configure.md): Azure portalında RBAC ile çalışmaya başlama.
* Kullanılabilir işlemleri listesi için bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](role-based-access-control-resource-provider-operations.md).
* Erişimle yönetmeyi öğrenin:
  * [PowerShell](role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
  * [REST API](role-based-access-control-manage-access-rest.md)
* [Yerleşik roller](role-based-access-built-in-roles.md): Get RBAC standart gelen rolleri hakkında ayrıntılar.
