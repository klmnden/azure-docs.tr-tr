---
title: Özelleştirilmiş denetleyicisi erişim rolünü - Avere vFXT Azure
description: Avere vFXT küme denetleyici için bir özel erişim rolünün nasıl oluşturulacağını
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: 2a0f4a628764aaa561a5567d3435a42da804a994
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58417851"
---
# <a name="customized-controller-access-role"></a>Özelleştirilmiş denetleyici erişim rolü

Azure kümesine denetleyicisi Avere vFXT, küme oluşturma ve yönetme izin vermek için bir yönetilen kimlik ve rol tabanlı erişim denetimi (RBAC) kullanır. 

Varsayılan olarak, küme denetleyicisi atanır [yerleşik sahip rolü](../role-based-access-control/built-in-roles.md#owner). Ayrıca, denetleyicinin erişim kapsamı, kaynak grubu - küme kaynak grubu dışından öğeleri değiştiremezsiniz.

Bu makalede, varsayılan ayarı kullanmak yerine küme denetleyici için kendi erişim rolünün nasıl oluşturulacağını açıklar. 

## <a name="edit-the-role-prototype"></a>Rol örneğini Düzenle

Başlangıç kullanılabilir prototip rolünden <https://github.com/Azure/Avere/blob/master/src/vfxt/src/roles/AvereContributor.txt>.

```json
{
  "AssignableScopes": [
    "/subscriptions/YOUR SUBSCRIPTION ID HERE"
  ],
  "Name": "Avere custom contributor",
  "IsCustom": true,
  "Description": "Can create and manage an Avere vFXT cluster.",
  "NotActions": [],
  "Actions": [
    "Microsoft.Authorization/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/availabilitySets/*",
    "Microsoft.Compute/virtualMachines/*",
    "Microsoft.Compute/disks/*",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Network/*/read",
    "Microsoft.Network/networkInterfaces/*",
    "Microsoft.Network/virtualNetworks/read",
    "Microsoft.Network/virtualNetworks/subnets/join/action",
    "Microsoft.Network/virtualNetworks/subnets/read",
    "Microsoft.Resources/deployments/*",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Resources/subscriptions/resourceGroups/resources/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Storage/storageAccounts/listKeys/action",
    "Microsoft.Support/*"
  ],
  "DataActions": []
}
```

Abonelik kimliği için Azure dağıtımı için Avere vFXT AssignableScopes deyiminde ekleyin. Özelleştirme adı ve ekleyebilir veya tanımları gerektiği gibi değiştirin. 

Ayrıcalıkları kısıtlamak isterseniz dikkatli olun. Denetleyici yeterli erişim yoksa, küme oluşturma başarısız olabilir. 

Bir küme oluşturmak hangi küme denetleyicisi ayrıcalıkları anlamak için yardıma ihtiyaçları için [bir destek bileti açın](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt). 

Özel rol tanımınız bir .json dosyası olarak kaydedin. 

## <a name="define-the-role"></a>Rol tanımlayın 

Özel rol tanımını aboneliğinize eklemek için aşağıdaki adımları izleyin. 

1. Azure portalında Azure Cloud Shell'i açın veya göz atın [ https://shell.azure.com ](https://shell.azure.com).

1. VFXT aboneliğinize geçiş yapmak için Azure CLI komutunu kullanın:

   ```azurecli
   az account set --subscription YOUR_SUBSCRIPTION_ID
   ```

1. Rol oluşturun:

   ```azurecli
   az role definition create --role-definition /avere-contributor-custom.json
   ```

   Dosya adı ve yolu yerine ```/avere-contributor-custom.json``` Bu örnekte. 

Rol tanımı komutunun çıkışını kaydedin - küme oluşturma şablonu için sağlamanız gereken rol tanımlayıcısını içerir. 

## <a name="find-the-role-id"></a>Rol Kimliği bulunamadı

Avere vFXT dağıtım şablonu, denetleyici özel bir rol atamak için rol genel benzersiz tanıtıcısı (GUID) gerekir. 

GUID 32 karakterli bir dizedir bu formda rolüdür: 8e3af657-a8ff-443c-a75c-2fe8c4bcb635

, Rolün GUID aramak için rol adınızı ile bu komutu kullanın ```--name``` parametresi.

```azurecli
az role definition list --query '[*].{roleName:roleName, name:name}' -o table --name 'YOUR ROLE NAME'
```
Bu dizesi girin **Avere küme oluşturma rol kimliği** Avere vFXT Azure'a dağıtırken alan.

## <a name="next-steps"></a>Sonraki adımlar

Azure'da for Avere vFXT dağıtma okuma [vFXT kümesi dağıtma](avere-vfxt-deploy.md)
