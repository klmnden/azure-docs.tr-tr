---
title: Sistem ve kullanıcı yapılandırmak nasıl Azure CLI kullanarak Azure VM'de kimlikleri atanan
description: Adım adım sistemi ve kullanıcı yapılandırmaya yönelik yönergeler Azure CLI kullanarak bir Azure VM kimliği atanır.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 44d1dabdb6a9e5f4b405b876f37daa9097c6e7f8
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="configure-managed-service-identity-msi-on-an-azure-vm-using-azure-cli"></a>Yönetilen hizmet kimliği (MSI) Azure CLI kullanarak bir Azure VM yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, Azure CLI kullanarak bir Azure VM aşağıdaki yönetilen hizmet kimliği işlemleri gerçekleştirmek nasıl öğrenin:
- Etkinleştirme ve Azure VM'de kimliği atanır sistem devre dışı
- Ekleme ve kaldırma kimlik Azure VM'de atanan kullanıcı

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile emin değilseniz, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [atanan sistemi ve kullanıcı kimliği atanır arasındaki farkı](overview.md#how-does-it-work)**.
- Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.
- CLI komut dosyası örnekleri çalıştırmak için üç seçeneğiniz vardır:

    - Kullanım [Azure bulut Kabuk](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
    - Her kod bloğunun sağ üst köşesinde yer alan "deneyin" düğmesini, aracılığıyla katıştırılmış Azure bulut kabuğunu kullanın.
    - [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya sonrası) yerel CLI konsol kullanmayı tercih ederseniz. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Sistem kimliği atanır

Bu bölümde, etkinleştirme ve devre dışı Azure CLI kullanarak Azure VM'de kimliği atanır sistem öğrenin.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında kimliği atanır Sistemi'ni etkinleştir

Etkin kimliği atanır sistemi ile bir Azure VM oluşturmak için:

1. Yerel bir konsolda Azure CLI kullanıyorsanız, ilk kez Azure kullanarak oturum [az oturum açma](/cli/azure/reference-index#az_login). Altında VM dağıtmak istediğiniz Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

   ```azurecli-interactive
   az login
   ```

2. Oluşturma bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md#terminology) kapsama ve VM'nizi ve kullanarak kaynaklarıyla ilgili dağıtımı için [az grubu oluşturma](/cli/azure/group/#az_group_create). Bunun yerine kullanmak istediğiniz kaynak grubu zaten varsa bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Kullanarak bir VM oluşturun [az vm oluşturma](/cli/azure/vm/#az_vm_create). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVM* tarafından istendiği gibi bir sistem atanan kimliği ile `--assign-identity` parametresi. `--admin-username` Ve `--admin-password` parametreleri sanal makine oturum açma için yönetici kullanıcı adı ve parola hesabı belirtin. Bu değerleri, ortamınız için uygun şekilde güncelleştirin: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Mevcut bir Azure VM'i kimliğini atanan Sistemi'ni etkinleştir

Mevcut bir VM'yi atanan sistem kimliğini etkinleştirmeniz gerekirse:

1. Yerel bir konsolda Azure CLI kullanıyorsanız, ilk kez Azure kullanarak oturum [az oturum açma](/cli/azure/reference-index#az_login). VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```azurecli-interactive
   az login
   ```

2. Kullanım [az vm kimliği atamak](/cli/azure/vm/identity/#az_vm_identity_assign) ile `identity assign` komutu etkinleştirmek mevcut bir VM'yi atanan sistem kimliği:

   ```azurecli-interactive
   az vm identity assign -g myResourceGroup -n myVm
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vm"></a>Bir Azure sanal makineden kimliği atanır sisteminizi devre dışı

> [!NOTE]
> Yönetilen hizmet kimliği, bir sanal makineden devre dışı bırakma şu anda desteklenmiyor. Bu arada, sistem atanan ve kullanıcı atanan kimlikler kullanma arasında geçiş yapabilirsiniz.

Artık kimliği atanır sistem gerekiyor ancak hala kimlikleri atanan kullanıcı gereken bir sanal makine varsa, aşağıdaki komutu kullanın:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```
MSI VM uzantısı kaldırmak için kullanıcı `-n ManagedIdentityExtensionForWindows` veya `-n ManagedIdentityExtensionForLinux` anahtarı (VM türünü bağlı olarak) [az vm uzantısı delete](https://docs.microsoft.com/cli/azure/vm/#assign-identity):

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

## <a name="user-assigned-identity"></a>Kullanıcı atanan kimliği

Bu bölümde, ekleme ve kaldırma kimlik Azure Azure CLI kullanarak bir VM'den, atanmış bir kullanıcı öğreneceksiniz.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında kimlik atanan kullanıcı atama

Bu bölümde, bir kullanıcı kimliği atanır atamasının olan bir VM oluşturulması açıklanmaktadır. Zaten kullanmak istediğiniz bir VM'niz varsa, bu bölüm atlayın ve sonraki devam edin.

1. Kullanmak istediğiniz bir kaynak grubu zaten varsa bu adımı atlayabilirsiniz. Oluşturma bir [kaynak grubu](~/articles/azure-resource-manager/resource-group-overview.md#terminology) kapsama ve, MSI dağıtımı için kullanarak [az grubu oluşturma](/cli/azure/group/#az_group_create). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<LOCATION>` parametre değerlerini kendi değerlere sahip. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Kimlik bilgileriniz kullanılarak atanmış bir kullanıcı oluşturmak [az kimliği oluşturma](/cli/azure/identity#az_identity_create).  `-g` Parametresi, burada kimlik atanmış kullanıcı oluşturuldu, kaynak grubu belirtir ve `-n` parametresi adını belirtir.    
    
    > [!IMPORTANT]
    > Atanan kullanıcı kimlikleri yalnızca destekler alfasayısal oluşturma ve tire (0-9 veya a-z veya A-Z veya -) karakter. Ayrıca, ad atama düzgün çalışması için VM/VMSS için 24 karakter uzunluğu sınırlı olmalıdır. Geri güncelleştirmeleri denetleyin. Daha fazla bilgi için bkz: [SSS ve bilinen sorunlar](known-issues.md)


    ```azurecli-interactive
    az identity create -g myResourceGroup -n myUserAssignedIdentity
    ```
Yanıt oluşturulan, aşağıdakine benzer atanan kullanıcı kimliğini ayrıntılarını içerir. Aşağıdaki adımda kimlik atanan kullanıcıya atanan kaynak kimliği değeri kullanılır.

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

3. Kullanarak bir VM oluşturun [az vm oluşturma](/cli/azure/vm/#az_vm_create). Aşağıdaki örnek, bir VM tarafından belirtilen yeni atanan kullanıcı kimliği ile ilişkili oluşturur `--assign-identity` parametresi. Değiştirdiğinizden emin olun `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>`, ve `<MSI ID>` parametre değerlerini kendi değerlere sahip. İçin `<MSI ID>`, kullanıcı tarafından atanan kimliğin kaynağı kullanmak `id` önceki adımda oluşturduğunuz özelliği: 

   ```azurecli-interactive 
   az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <MSI ID>
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Mevcut bir Azure VM'i kimlik atanan kullanıcı atama

1. Kimlik bilgileriniz kullanılarak atanmış bir kullanıcı oluşturmak [az kimliği oluşturma](/cli/azure/identity#az-identity-create).  `-g` Parametresi, burada kimlik atanmış kullanıcı oluşturuldu, kaynak grubu belirtir ve `-n` parametresi adını belirtir. Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<MSI NAME>` parametre değerlerini kendi değerlerinizi ile:

    > [!IMPORTANT]
    > Kullanıcı adında özel karakterler (örneğin, alt çizgi) kimliklerle atanan oluşturma şu anda desteklenmiyor. Lütfen alfasayısal karakterler kullanın. Geri güncelleştirmeleri denetleyin.  Daha fazla bilgi için bkz: [SSS ve bilinen sorunlar](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
Yanıt oluşturulan, aşağıdakine benzer MSI atanmış kullanıcı ayrıntılarını içerir. Kaynak `id` değeri atanmış kullanıcı kimliği atanır, aşağıdaki adımda kullanılır.

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

2. Atanan kullanıcı kimliğini kullanarak VM atamak [az vm kimliği atamak](/cli/azure/vm#az-vm-identity-assign). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlere sahip. `<MSI ID>` Kullanıcıya atanan kimliğin kaynak olacak `id` özelliği, önceki adımda oluşturduğunuz olarak:

    ```azurecli-interactive
    az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vm"></a>Bir Azure sanal makineden kimliği atanır kullanıcıyı kaldırma

> [!NOTE]
> Kimlik atanmış bir sistem olmadığı sürece tüm atanan kullanıcı kimlikleri bir sanal makineden kaldırma şu anda, desteklenmiyor.

VM'yi birden çok kullanıcı tarafından atanan kimlikleri varsa, tüm son bir kullanarak ancak kaldırabilir [az vm kimliğini kaldırma](/cli/azure/vm#az-vm-identity-remove). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlere sahip. `<MSI NAME>` Kullanıcıya atanan kimliğin olacaktır `name` tarafından VM kullanmanın kimlik bölümünde bulunabilir özelliği `az vm show`:

```azurecli-interactive
az vm identity remove -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
```
VM atanan sistem ve kimlikler atanan kullanıcı varsa, sadece atanan sistemini kullanacak şekilde geçerek kimlikleri atanan tüm kullanıcı kaldırabilirsiniz. Aşağıdaki komutu kullanın:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null 
```

## <a name="related-content"></a>İlgili içerik
- [Yönetilen hizmet Kimliği'ne genel bakış](overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç ipuçları, bakın: 
  - [CLI ile Windows sanal makine oluşturma](../../virtual-machines/windows/quick-create-cli.md)  
  - [CLI ile Linux sanal makine oluşturma](../../virtual-machines/linux/quick-create-cli.md) 

















