---
title: Sistem ve kullanıcı yapılandırma tarafından atanan kimliklerle üzerinde Azure CLI kullanarak bir Azure VMSS
description: Adım adım sistem ve kullanıcı yapılandırmaya yönelik yönergeler, Azure CLI kullanarak bir Azure vmss'de kimlikleri atanır.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: 36df9d00d41f3c092320fa88772b41c9a41c6d8e
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237290"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>Sanal Makine Yapılandırma Yönetilen hizmet kimliği (MSI) Azure CLI kullanarak ölçek kümesi

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, aşağıdaki üzerinde bir Azure sanal makine ölçek kümesi (Azure CLI kullanarak VMSS), yönetilen hizmet kimliği işlemlerini nasıl gerçekleştireceğinizi öğrenin:
- Enable ve disable sistem tarafından atanan bir Azure VMSS kimliği
- Bir kullanıcı tarafından atanan kimliği bir Azure vmss'de ekleyip


## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) oluşturma bir sanal makine ölçek kümesi ve etkinleştir ve Kaldır sistem ve/veya kullanıcı bir sanal makine ölçek kümesinden yönetilen kimlik atanır.
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolünün bir kullanıcı tarafından atanan kimliği oluşturma.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü atamak ve bir kullanıcı tarafından atanan kimliği ve sanal makine ölçek kümesine kaldırmak için.
- CLI betiği örnekleri çalıştırmak için üç seçeneğiniz vardır:
    - Kullanım [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
    - Katıştırılmış Azure Cloud Shell aracılığıyla her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesini kullanın.
    - [CLI 2. 0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya üzeri) yerel bir CLI konsol kullanmak istiyorsanız. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

Bu bölümde, Azure CLI kullanarak bir Azure VMSS için kimlik atanan sistemi devre öğrenin.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi oluşturma sırasında sistem tarafından atanan kimlik etkinleştir

Sanal makine ölçek kümesi atanan kimliği etkin sistemiyle oluşturmak için:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az_login) kullanarak Azure'da oturum açın. Sanal makine ölçek kümesini dağıtmak altında istediğiniz Azure aboneliği ile ilişkili olan bir hesabı kullanın:

   ```azurecli-interactive
   az login
   ```

2. Oluşturma bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md#terminology) kapsama ve sanal makine ölçek kümenizi ve ilgili kaynaklarını kullanarak, dağıtım için [az grubu oluşturma](/cli/azure/group/#az_group_create). Bunun yerine kullanmak istediğiniz bir kaynak grubu zaten varsa bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Kullanarak bir sanal makine ölçek kümesi oluşturma [az vmss oluşturma](/cli/azure/vmss/#az_vmss_create) . Aşağıdaki örnekte adlı bir sanal makine ölçek kümesi oluşturur *myVMSS* tarafından istendiği gibi bir sistem tarafından atanan kimlikle `--assign-identity` parametresi. `--admin-username` ve `--admin-password` parametreleri, sanal makinede oturum açmak için yönetici hesabının kullanıcı adı ve parolasını belirtir. Bu değerleri ortamınıza uyacak şekilde güncelleştirin: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Sistem tarafından atanan kimliği mevcut bir Azure sanal makine ölçek kümesinde etkinleştirin

Sistem tarafından atanan kimliği mevcut bir Azure sanal makine ölçek kümesi üzerinde etkinleştirmek gerekiyorsa:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az_login) kullanarak Azure'da oturum açın. Sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```azurecli-interactive
   az login
   ```

2. Kullanım [az vmss kimliği atamak](/cli/azure/vmss/identity/#az_vmss_identity_assign) var olan bir sanal makineye bir sistem tarafından atanan kimliği etkinleştirmek için komutu:

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden sistem tarafından atanan kimliği devre dışı

Artık sistem tarafından atanan kimlik gerekiyor, ancak yine de kullanıcı tarafından atanan kimliklerle gereken bir sanal makine ölçek kümesi varsa, aşağıdaki komutu kullanın:

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```

Artık sistem tarafından atanan kimlik gereken bir sanal makineye sahip ve hiçbir kullanıcı tarafından atanan kimliklerle varsa, aşağıdaki komutu kullanın:

> [!NOTE]
> Değer `none` büyük/küçük harfe duyarlıdır. Küçük harf olması gerekir. 

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type="none"
```

MSI VM uzantısı'nı kaldırmak için [az vmss kimliğini kaldırma](/cli/azure/vmss/identity/#az_vmss_remove_identity) sistem tarafından atanan kimliği bir VMSS kaldırmak için komutu:

```azurecli-interactive
az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
```

## <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği

Bu bölümde, etkinleştirmek ve Azure CLI kullanarak bir kullanıcı tarafından atanan kimliği kaldırma konusunda bilgi edinin.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Bir kullanıcı tarafından atanan kimliği Azure VMSS oluşturma sırasında atama

Bu bölümde bir VMSS oluşturulmasını ve bir kullanıcı tarafından atanan kimliği VMSS'ye atamasının gösterilmektedir. Kullanmak istediğiniz bir VMSS zaten varsa, bu bölümü atlayın ve sonraki devam edin.

1. Kullanmak istediğiniz bir kaynak grubu zaten varsa bu adımı atlayabilirsiniz. Oluşturma bir [kaynak grubu](~/articles/azure-resource-manager/resource-group-overview.md#terminology) kapsama ve atanan kullanıcı kimliğinizi dağıtımını kullanarak [az grubu oluşturma](/cli/azure/group/#az_group_create). `<RESOURCE GROUP>` ve `<LOCATION>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Kimlik bilgileriniz kullanılarak atanan bir kullanıcı oluşturmak [az kimliği oluşturma](/cli/azure/identity#az-identity-create).  `-g` Parametresi, kullanıcı tarafından atanan kimliği oluşturulduğu, kaynak grubunu belirtir ve `-n` parametre adını belirtir. `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```azurecli-interactive
   az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
   ```
   Yanıt, Ayrıntılar için aşağıdakine benzer şekilde oluşturulmuş kullanıcı tarafından atanan kimlik içerir. Kaynak `id` kullanıcı tarafından atanan kimlik için atanan değer, aşağıdaki adımda kullanılır.

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

3. Kullanarak bir VMSS oluşturma [az vmss oluşturma](/cli/azure/vmss/#az-vmss-create). Aşağıdaki örnekte belirtildiği gibi yeni atanmış kullanıcı kimliğiyle ilişkili bir VMSS oluşturur `--assign-identity` parametresi. `<RESOURCE GROUP>`, `<VMSS NAME>`, `<USER NAME>`, `<PASSWORD>` ve `<USER ASSIGNED IDENTITY ID>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. İçin `<USER ASSIGNED IDENTITY ID>`, kullanıcı tarafından atanan kimliğin kaynağı `id` önceki adımda oluşturduğunuz özelliği: 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY ID>
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Bir kullanıcı tarafından atanan kimliği mevcut bir Azure VM'ye atayın

1. Kimlik bilgileriniz kullanılarak atanan bir kullanıcı oluşturmak [az kimliği oluşturma](/cli/azure/identity#az-identity-create).  `-g` Parametresi, kullanıcı tarafından atanan kimliği oluşturulduğu, kaynak grubunu belirtir ve `-n` parametre adını belirtir. `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

    > [!IMPORTANT]
    > Kullanıcı tarafından atanan kimliklerle adında özel karakterler (örneğin, alt çizgi) ile oluşturma şu anda desteklenmiyor. Lütfen alfasayısal karakterler kullanın. Güncelleştirmeler için sonra yeniden denetleyin.  Daha fazla bilgi için [SSS ve bilinen sorunlar](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
Yanıt, Ayrıntılar için aşağıdakine benzer şekilde oluşturulmuş kullanıcı tarafından atanan kimlik içerir. Kaynak `id` kullanıcı tarafından atanan kimlik için atanan değer, aşağıdaki adımda kullanılır.

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

2. Atanan kullanıcı kimliğini kullanarak VMSS atama [az vmss kimliği atamak](/cli/azure/vmss/identity#az_vm_assign_identity). `<RESOURCE GROUP>` ve `<VMSS NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<USER ASSIGNED IDENTITY ID>` Atanan kullanıcı kimliğin kaynak olacak `id` önceki adımda oluşturulan özelliği:

    ```azurecli-interactive
    az vmss identity assign -g <RESOURCE GROUP> -n <VMSS NAME> --identities <USER ASSIGNED IDENTITY ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Bir kullanıcı tarafından atanan kimliği bir Azure sanal makine ölçek kümesinden Kaldır

Bir kullanıcı tarafından atanan kimliği bir sanal makine ölçek kümesi kullanımdan kaldırılacağı [az vmss kimliğini kaldırma](/cli/azure/vmss/identity#az-vmss-identity-remove). `<RESOURCE GROUP>` ve `<VMSS NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<MSI NAME>` Atanan kullanıcı kimliğin olacaktır `name` tarafından VM kullanarak kimlik bölümünde bulunabilir özelliği `az vmss identity show`:

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VMSS NAME> --identities <MSI NAME>
```

Sanal makine ölçek kümesi bir sistem tarafından atanan kimlik yok ve tüm kullanıcı kimlikleri atanmış kaldırmak istediğiniz, aşağıdaki komutu kullanın:

> [!NOTE]
> Değer `none` büyük/küçük harfe duyarlıdır. Küçük harf olması gerekir.

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type="none" identity.identityIds=null
```

Sanal makine ölçek kümenize atanan sistem ve kullanıcı tarafından atanan kimliklerle varsa, yalnızca atanan sistemi kullanmaya geçiş tarafından atanan kimliklerle tüm kullanıcı kaldırabilirsiniz. Aşağıdaki komutu kullanın:

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null 
```

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen hizmet Kimliği'ne genel bakış](overview.md)
- İçin tam Azure oluşturma Hızlı sanal makine ölçek kümesi, bakın: 

  - [CLI ile bir sanal makine ölçek kümesi oluşturma](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

















