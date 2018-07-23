---
title: Sistem ve kullanıcı yapılandırma tarafından atanan kimliklerle Azure CLI kullanarak bir Azure sanal makinesinde
description: Adım adım sistem ve kullanıcı yapılandırmaya yönelik yönergeler, Azure CLI kullanarak Azure VM'deki kimlikleri atanmış.
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
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 064c2dfe625435d3faddae45c413d6b91eac8346
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188038"
---
# <a name="configure-managed-service-identity-msi-on-an-azure-vm-using-azure-cli"></a>Azure CLI kullanarak bir Azure VM yönetilen hizmet kimliği (MSI) yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, Azure CLI kullanarak bir Azure sanal makinesinde aşağıdaki yönetilen hizmet kimliği işlemlerini nasıl gerçekleştireceğinizi öğrenin:
- Etkinleştirme ve sistem tarafından atanan kimlik Azure sanal makinesinde devre dışı
- Bir kullanıcı tarafından atanan kimliği Azure VM'de ekleyip

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) bir VM oluşturun ve etkinleştirin ve sistem tarafından bir Azure VM'den yönetilen kimlik atanan kaldırmak için.
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolünün bir kullanıcı tarafından atanan kimliği oluşturma.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü atamak ve bir kullanıcı tarafından atanan kimliği ve sanal makineye kaldırmak için.
- CLI betiği örnekleri çalıştırmak için üç seçeneğiniz vardır:
    - Kullanım [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
    - Katıştırılmış Azure Cloud Shell aracılığıyla her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesini kullanın.
    - [CLI 2. 0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya üzeri) yerel bir CLI konsol kullanmak istiyorsanız. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

Bu bölümde, etkinleştirme ve sistem tarafından atanan kimlik Azure CLI kullanarak bir Azure sanal makinesinde devre dışı öğrenin.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında atanan kimliği Sistemi'ni etkinleştir

Azure VM ile sistem oluşturma etkinleştirildi kimlik atanan:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az_login) kullanarak Azure'da oturum açın. VM'yi dağıtırken kullanmak istediğiniz Azure aboneliğiyle ilişkilendirilmiş bir hesap kullanın:

   ```azurecli-interactive
   az login
   ```

2. VM'nizin ve onunla ilgili kaynakların kapsaması ve dağıtımı için, [az group create](/cli/azure/group/#az_group_create) komutunu kullanarak bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md#terminology) oluşturun. Bunun yerine kullanmak istediğiniz bir kaynak grubunuz varsa, bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. [az vm create](/cli/azure/vm/#az_vm_create) kullanarak VM oluşturun. Aşağıdaki örnekte adlı bir VM oluşturur *myVM* tarafından istendiği gibi bir sistem tarafından atanan kimlikle `--assign-identity` parametresi. `--admin-username` ve `--admin-password` parametreleri, sanal makinede oturum açmak için yönetici hesabının kullanıcı adı ve parolasını belirtir. Bu değerleri ortamınıza uyacak şekilde güncelleştirin: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Atanan kimliği mevcut bir Azure sanal makinesinde Sistemi'ni etkinleştir

Sistem tarafından atanan mevcut VM'yi hizmetinizle ihtiyacınız varsa:

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az_login) kullanarak Azure'da oturum açın. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```azurecli-interactive
   az login
   ```

2. Kullanım [az vm kimliği atamak](/cli/azure/vm/identity/#az_vm_identity_assign) ile `identity assign` komutu etkinleştirmek var olan bir sanal makineye atanan sistem kimliği:

   ```azurecli-interactive
   az vm identity assign -g myResourceGroup -n myVm
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vm"></a>Bir Azure VM'den atanan kimliği sistemi devre dışı bırak

> [!NOTE]
> Bir sanal makineden yönetilen hizmet kimliği devre dışı bırakma şu anda desteklenmiyor. Bu arada, sistem atanan ve atanan kullanıcı kimliklerini kullanma arasında geçiş yapabilirsiniz.

Artık sistem tarafından atanan kimlik gerekiyor ancak yine de kullanıcı tarafından atanan kimliklerle gereken bir sanal makine varsa, aşağıdaki komutu kullanın:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```
MSI VM uzantısı'nı kaldırmak için kullanıcı `-n ManagedIdentityExtensionForWindows` veya `-n ManagedIdentityExtensionForLinux` anahtarı (sanal makine türünü bağlı olarak) [az vm uzantısı silme](https://docs.microsoft.com/cli/azure/vm/#assign-identity):

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

## <a name="user-assigned-identity"></a>Kullanıcıya atanan kimlik

Bu bölümde, bir kullanıcı tarafından atanan kimliği Azure CLI kullanarak bir Azure VM'den ekleyip öğreneceksiniz.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vm"></a>Bir kullanıcı tarafından atanan kimliği bir Azure VM oluşturma sırasında atama

Bu bölümde, bir kullanıcı tarafından atanan kimliği atama ile VM oluşturma açıklanmaktadır. Kullanmak istediğiniz bir VM'niz varsa, bu bölümü atlayın ve sonraki devam edin.

1. Kullanmak istediğiniz bir kaynak grubu zaten varsa bu adımı atlayabilirsiniz. Oluşturma bir [kaynak grubu](~/articles/azure-resource-manager/resource-group-overview.md#terminology) kapsama ve, MSI dağıtımını kullanarak [az grubu oluşturma](/cli/azure/group/#az_group_create). `<RESOURCE GROUP>` ve `<LOCATION>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Kimlik bilgileriniz kullanılarak atanan bir kullanıcı oluşturmak [az kimliği oluşturma](/cli/azure/identity#az_identity_create).  `-g` Parametresi, kullanıcı tarafından atanan kimliği oluşturulduğu, kaynak grubunu belirtir ve `-n` parametre adını belirtir.    
    
[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


```azurecli-interactive
az identity create -g myResourceGroup -n myUserAssignedIdentity
```
Yanıt, Ayrıntılar için aşağıdakine benzer şekilde oluşturulmuş kullanıcı tarafından atanan kimlik içerir. Kullanıcı tarafından atanan kimliği için atanan kaynak kimliği değeri aşağıdaki adımda kullanılır.

```json
{
    "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
    "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
    "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
    "location": "westcentralus",
    "name": "<MSI NAME>",
    "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
    "resourceGroup": "<RESOURCE GROUP>",
    "tags": {},
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
    "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
}
```

3. [az vm create](/cli/azure/vm/#az_vm_create) kullanarak VM oluşturun. Aşağıdaki örnekte belirtildiği gibi yeni atanmış kullanıcı kimliğiyle ilişkili bir VM oluşturur `--assign-identity` parametresi. `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>` ve `<MSI ID>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. İçin `<MSI ID>`, kullanıcı tarafından atanan kimliğin kaynağı `id` önceki adımda oluşturduğunuz özelliği: 

```azurecli-interactive 
az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <MSI ID>
```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Bir kullanıcı tarafından atanan kimliği mevcut bir Azure VM'ye atayın

1. Kimlik bilgileriniz kullanılarak atanan bir kullanıcı oluşturmak [az kimliği oluşturma](/cli/azure/identity#az-identity-create).  `-g` Parametresi, kullanıcı tarafından atanan kimliği oluşturulduğu, kaynak grubunu belirtir ve `-n` parametre adını belirtir. `<RESOURCE GROUP>` ve `<MSI NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

    > [!IMPORTANT]
    > Kullanıcı tarafından atanan kimliklerle adında özel karakterler (örneğin, alt çizgi) ile oluşturma şu anda desteklenmiyor. Lütfen alfasayısal karakterler kullanın. Güncelleştirmeler için sonra yeniden denetleyin.  Daha fazla bilgi için [SSS ve bilinen sorunlar](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
Yanıt, aşağıdakine benzer şekilde oluşturulmuş MSI atanmış kullanıcı ayrıntılarını içerir. Kaynak `id` kullanıcı tarafından atanan kimlik için atanan değer, aşağıdaki adımda kullanılır.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
        "location": "westcentralus",
        "name": "<MSI NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. Atanan kullanıcı kimliğini kullanarak VM atama [az vm kimliği atamak](/cli/azure/vm#az-vm-identity-assign). `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<MSI ID>` Atanan kullanıcı kimliğin kaynak olacak `id` önceki adımda oluşturulan özelliği:

    ```azurecli-interactive
    az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vm"></a>Bir kullanıcı tarafından atanan kimliği bir Azure VM'den kaldırın

> [!NOTE]
> Bir sistem tarafından atanan kimlik olmadığı sürece tüm kullanıcı tarafından atanan kimlikleri bir sanal makineden kaldırılması şu anda, desteklenmiyor.

Sanal makinenize birden çok kullanıcı tarafından atanan kimliği varsa, tüm son bir kullanarak ancak kaldırabilirsiniz [az vm kimliğini kaldırma](/cli/azure/vm#az-vm-identity-remove). `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<MSI NAME>` Atanan kullanıcı kimliğin olacaktır `name` tarafından VM kullanarak kimlik bölümünde bulunabilir özelliği `az vm show`:

```azurecli-interactive
az vm identity remove -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
```
Sanal makinenize atanmış sistem ve kullanıcı tarafından atanan kimliklerle varsa, yalnızca atanan sistemi kullanmaya geçiş tarafından atanan kimliklerle tüm kullanıcı kaldırabilirsiniz. Aşağıdaki komutu kullanın:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null 
```

## <a name="related-content"></a>İlgili içerik
- [Yönetilen hizmet Kimliği'ne genel bakış](overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç kılavuzları, bkz: 
  - [CLI ile bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-cli.md)  
  - [CLI ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-cli.md) 

















