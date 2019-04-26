---
title: Azure CLI kullanarak bir Azure sanal makine ölçek üzerinde sistem ve kullanıcı tarafından atanan yönetilen kimlikleri yapılandırma kümesi
description: Adım adım bir Azure sanal makine ölçek üzerinde sistem ve kullanıcı tarafından atanan yönetilen kimlikleri yapılandırmaya yönelik yönergeler, Azure CLI kullanarak ayarlayın.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: MarkusVi
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 04a3c9eba1f6498796a7e617b400649963c996d1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60290968"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-virtual-machine-scale-set-using-azure-cli"></a>Azure CLI kullanarak sanal makine ölçek kümesi üzerinde Azure kaynakları için yönetilen kimlik Yapılandır

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure CLI kullanarak bir Azure sanal makine ölçek kümesinde Azure kaynaklarını işlemleri için yönetilen şu kimlikler gerçekleştirmeyi öğrenin:
- Etkinleştirme ve devre dışı bir Azure sanal makine ölçek kümesinde sistem tarafından atanan yönetilen kimlik
- Bir kullanıcı tarafından atanan yönetilen kimlik bir Azure sanal makine ölçek kümesinde ekleyip


## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki Azure rol tabanlı erişim denetimi atamalarını hesabınızın gerekir:

    > [!NOTE]
    > Hiçbir ek Azure AD dizini rol atamaları gerekli.

    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) bir sanal makine ölçek kümesi oluşturun ve etkinleştirin ve sistem ve/veya yönetilen kimlik kullanıcı tarafından atanan bir sanal makine ölçek kümesinden kaldırmak için.
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolüne bir kullanıcı tarafından atanan oluşturmak için yönetilen kimliği.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) gelen ve sanal makine ölçek kümesi yönetilen kimliği atamak ve bir kullanıcı tarafından atanan kaldırmak için rol.
- CLI betiği örnekleri çalıştırmak için üç seçeneğiniz vardır:
    - Kullanım [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
    - Katıştırılmış Azure Cloud Shell aracılığıyla her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesini kullanın.
    - [Azure CLI'ın en son sürümü yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya üzeri) yerel bir CLI konsol kullanmak istiyorsanız. 
      
      > [!NOTE]
      > Komutlar, en son sürümünü yansıtacak şekilde güncelleştirildi [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, Azure CLI kullanarak bir Azure sanal makine ölçek kümesi için sistem tarafından atanan yönetilen kimlik devre öğrenin.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi oluşturma sırasında sistem tarafından atanan yönetilen kimlik etkinleştir

Sanal makine ölçek kümesi etkin sistem tarafından atanan yönetilen kimlikle oluşturmak için:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. Sanal makine ölçek kümesini dağıtmak altında istediğiniz Azure aboneliği ile ilişkili olan bir hesabı kullanın:

   ```azurecli-interactive
   az login
   ```

2. Oluşturma bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md#terminology) kapsama ve sanal makine ölçek kümenizi ve ilgili kaynaklarını kullanarak, dağıtım için [az grubu oluşturma](/cli/azure/group/#az-group-create). Bunun yerine kullanmak istediğiniz bir kaynak grubu zaten varsa bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Kullanarak bir sanal makine ölçek kümesi oluşturma [az vmss oluşturma](/cli/azure/vmss/#az-vmss-create) . Aşağıdaki örnekte adlı bir sanal makine ölçek kümesi oluşturur *myVMSS* bir sistem tarafından atanan ile kimliği tarafından istendiği yönetilen `--assign-identity` parametresi. `--admin-username` ve `--admin-password` parametreleri, sanal makinede oturum açmak için yönetici hesabının kullanıcı adı ve parolasını belirtir. Bu değerleri ortamınıza uyacak şekilde güncelleştirin: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Sistem tarafından atanan kimliği mevcut bir Azure sanal makine ölçek kümesi üzerinde yönetilen etkinleştir

Mevcut bir Azure sanal makine ölçek kümesinde sistem tarafından atanan yönetilen kimlik etkinleştirmeniz gerekirse:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. Sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```azurecli-interactive
   az login
   ```

2. Kullanım [az vmss kimliği atamak](/cli/azure/vmss/identity/#az-vmss-identity-assign) sistem tarafından atanan etkinleştirmek için komutu yönetilen mevcut bir VM kimliği:

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### <a name="disable-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi yönetilen kimlik sistem tarafından atanan devre dışı bırak

Artık sistem tarafından atanan bir yönetilen kimlik gerekiyor, ancak yine de kullanıcı tarafından atanan yönetilen kimlikleri gereken bir sanal makine ölçek kümesi varsa, aşağıdaki komutu kullanın:

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```

Artık yönetilen kimlik sistem tarafından atanan gereken bir sanal makineye sahip ve kullanıcı tarafından atanan yönetilen kimlik varsa, aşağıdaki komutu kullanın:

> [!NOTE]
> Değer `none` büyük/küçük harfe duyarlıdır. Küçük harf olması gerekir. 

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type="none"
```

> [!NOTE]
> (Kullanım dışı için) VM uzantısını Azure kaynakları için yönetilen kimliği sağladıysa kullanarak kaldırmanız gereken [az vmss uzantısı silme](https://docs.microsoft.com/cli/azure/vm/). Daha fazla bilgi için [Azure IMDS kimlik doğrulaması için VM uzantısı'ten geçiş](howto-migrate-vm-extension.md).

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, etkinleştirmek ve Azure CLI kullanarak bir kullanıcı tarafından atanan yönetilen kimlik kaldırma konusunda bilgi edinin.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi oluşturma sırasında kullanıcı tarafından atanan bir yönetilen kimlik atama

Bu bölümde, bir sanal makine ölçek kümesi oluşturma ve kullanıcı tarafından atanan bir yönetilen kimlik atama aracılığıyla sanal makine ölçek kümesine açıklanmaktadır. Kullanmak istediğiniz bir sanal makine ölçek kümesi zaten varsa, bu bölümü atlayın ve sonraki devam edin.

1. Kullanmak istediğiniz bir kaynak grubu zaten varsa bu adımı atlayabilirsiniz. Oluşturma bir [kaynak grubu](~/articles/azure-resource-manager/resource-group-overview.md#terminology) kapsama ve kullanıcı tarafından atanan yönetilen kimliğinizi dağıtımını kullanarak [az grubu oluşturma](/cli/azure/group/#az-group-create). `<RESOURCE GROUP>` ve `<LOCATION>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Kullanıcı tarafından atanan yönetilen kimliği oluşturmak için [az identity create](/cli/azure/identity#az-identity-create) kullanın.  `-g` parametresi kullanıcı tarafından atanan yönetilen kimliğin oluşturulduğu kaynak grubunu belirtirken, `-n` parametresi de bunun adını belirtir. `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```azurecli-interactive
   az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
   ```
   Yanıt, aşağıdakine benzer şekilde oluşturulmuş kullanıcı tarafından atanan yönetilen kimlik ayrıntılarını içerir. Kaynak `id` kullanıcı tarafından atanan yönetilen kimliğe atanır değeri aşağıdaki adımda kullanılır.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. Kullanarak bir sanal makine ölçek kümesi oluşturma [az vmss oluşturma](/cli/azure/vmss/#az-vmss-create). Aşağıdaki örnek, yeni kullanıcı tarafından atanan yönetilen kimliği tarafından belirtilen ile ilişkili bir sanal makine ölçek kümesi oluşturur. `--assign-identity` parametresi. `<RESOURCE GROUP>`, `<VMSS NAME>`, `<USER NAME>`, `<PASSWORD>` ve `<USER ASSIGNED IDENTITY>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY>
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan bir yönetilen kimlik var olan bir sanal makine ölçek kümesine atama

1. Kullanıcı tarafından atanan yönetilen kimliği oluşturmak için [az identity create](/cli/azure/identity#az-identity-create) kullanın.  `-g` parametresi kullanıcı tarafından atanan yönetilen kimliğin oluşturulduğu kaynak grubunu belirtirken, `-n` parametresi de bunun adını belirtir. `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
   Yanıt, aşağıdakine benzer şekilde oluşturulmuş kullanıcı tarafından atanan yönetilen kimlik ayrıntılarını içerir.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY >/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. Kullanıcı tarafından atanan yönetilen kimlik, sanal makine ölçek kümesi kullanarak Ata [az vmss kimliği atamak](/cli/azure/vmss/identity). `<RESOURCE GROUP>` ve `<VIRTUAL MACHINE SCALE SET NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<USER ASSIGNED IDENTITY>` Kullanıcı tarafından atanan kimliğin kaynağı `name` önceki adımda oluşturulan özelliği:

    ```azurecli-interactive
    az vmss identity assign -g <RESOURCE GROUP> -n <VIRTUAL MACHINE SCALE SET NAME> --identities <USER ASSIGNED IDENTITY>
    ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden bir kullanıcı tarafından atanan bir yönetilen kimlik Kaldır

Kullanıcı tarafından atanan bir yönetilen kimlik bir sanal makine ölçek kümesi kullanımdan kaldırılacağı [az vmss kimliğini kaldırma](/cli/azure/vmss/identity#az-vmss-identity-remove). Bu tek ise kullanıcı tarafından atanan sanal makine ölçek kümesine atanan kimlik yönetilen `UserAssigned` kimlik türü değerinden kaldırılacak.  `<RESOURCE GROUP>` ve `<VIRTUAL MACHINE SCALE SET NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<USER ASSIGNED IDENTITY>` Kullanıcı tarafından atanan yönetilen kimliğin olacaktır `name` özelliğini kullanarak sanal makine ölçek kümesi'nin kimlik bölümünde bulunabilir `az vmss identity show`:

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VIRTUAL MACHINE SCALE SET NAME> --identities <USER ASSIGNED IDENTITY>
```

Bir sistem tarafından atanan yönetilen sanal makine ölçek kümeniz yoksa, kimlik ve istediğiniz kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırın, aşağıdaki komutu kullanın:

> [!NOTE]
> Değer `none` büyük/küçük harfe duyarlıdır. Küçük harf olması gerekir.

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type="none" identity.userAssignedIdentities=null
```

Sanal makine ölçek kümesi, hem de sistem tarafından atanan ve yönetilen kimlikleri, kullanıcı tarafından atanan tüm kullanıcı tarafından atanan kimlikleri yalnızca sistem tarafından atanan yönetilen kimliği kullanma geçerek kaldırabilirsiniz. Aşağıdaki komutu kullanın:

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='SystemAssigned' identity.userAssignedIdentities=null 
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlik](overview.md)
- İçin tam Azure oluşturma Hızlı sanal makine ölçek kümesi, bakın: 

  - [CLI ile bir sanal makine ölçek kümesi oluşturma](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

















