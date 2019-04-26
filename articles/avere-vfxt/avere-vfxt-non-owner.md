---
title: Avere vFXT sahip olmayan geçici çözüm - Azure
description: Geçici çözüm, kullanıcıların ilgili daha fazla bilgi için Azure Avere vFXT dağıtmak için abonelik sahibi izniniz olmadan
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: e72e6d969649de09389ee38b94e874fad98ee08f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60409218"
---
# <a name="authorize-non-owners-to-deploy-avere-vfxt"></a>Sahip olmayanları Avere vFXT dağıtımı yapmak için yetkilendirme

Bu yönergeler, Azure sistem için bir Avere vFXT oluşturmak için sahip ayrıcalıkları aboneliği olmayan bir kullanıcı izin veren bir geçici çözüm içindir.

(VFXT sistemidir oluşturma yapmak sahip ayrıcalıklarına sahip bir kullanıcı için Avere dağıtmak için önerilen yöntem adımları açıklandığı şekilde [Avere vFXT oluşturmak için hazırlık](avere-vfxt-prereqs.md).)  

Geçici çözüm, küme yüklemek için yeterli izinlere, kullanıcılara bir ek erişim rolü oluşturmayı içerir. Rol, abonelik sahibi tarafından oluşturulması gerekir ve sahibi uygun kullanıcılara atamanız gerekir. 

Abonelik sahibi de gerekir [kullanım koşullarını kabul](avere-vfxt-prereqs.md) Avere vFXT Market görüntüsü için. 

> [!IMPORTANT] 
> Bu adımların tümü, küme için kullanılacak olan abonelik sahibi ayrıcalıklarına sahip bir kullanıcı tarafından izlenmelidir.

1. Bu satırlar kopyalayın ve bunları bir dosyaya kaydedin (örneğin, `averecreatecluster.json`). Abonelik Kimliğinizi kullanın `AssignableScopes` deyimi.

   ```json
   {
       "AssignableScopes": ["/subscriptions/<SUBSCRIPTION_ID>"],
       "Name": "avere-create-cluster",
       "IsCustom": "true"
       "Description": "Can create Avere vFXT clusters",
       "NotActions": [],
       "Actions": [
           "Microsoft.Authorization/*/read",
           "Microsoft.Authorization/roleAssignments/*",
           "Microsoft.Authorization/roleDefinitions/*",
           "Microsoft.Compute/*/read",
           "Microsoft.Compute/availabilitySets/*",
           "Microsoft.Compute/virtualMachines/*",
           "Microsoft.Network/*/read",
           "Microsoft.Network/networkInterfaces/*",
           "Microsoft.Network/routeTables/write",
           "Microsoft.Network/routeTables/delete",
           "Microsoft.Network/routeTables/routes/delete",
           "Microsoft.Network/virtualNetworks/subnets/join/action",
           "Microsoft.Network/virtualNetworks/subnets/read",
   
           "Microsoft.Resources/subscriptions/resourceGroups/read",
           "Microsoft.Resources/subscriptions/resourceGroups/resources/read",
           "Microsoft.Storage/*/read",
           "Microsoft.Storage/storageAccounts/listKeys/action"
       ],
   }
   ```

1. Rolü oluşturmak için şu komutu çalıştırın:

   `az role definition create --role-definition <PATH_TO_FILE>`

    Örnek:
    ```azurecli
    az role definition create --role-definition ./averecreatecluster.json
    ```

1. Kümeyi oluşturacak kullanıcıya bu rolü atayın:

   `az role assignment create --assignee <USERNAME> --scope /subscriptions/<SUBSCRIPTION_ID> --role 'avere-create-cluster'`

Bu yordam sonrasında bu role atanmış herhangi bir kullanıcı abonelik için şu izinler vardır: 

* Oluşturma ve ağ altyapısını yapılandırma
* Küme denetleyiciyi oluşturun
* Kümeyi oluşturmak için küme denetleyicisinden küme oluşturma betikleri çalıştırın
