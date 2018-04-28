---
title: Sistem ve kullanıcı yapılandırmak nasıl Azure CLI kullanarak bir Azure VMSS üzerinde kimlikleri atanan
description: Adım adım sistemi ve kullanıcı yapılandırmaya ilişkin yönergeler, Azure CLI kullanarak bir Azure VMSS kimliği atanır.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: e1084e3e318ce8bd10c80cf1e4192fff85ccc028
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>Sanal makine yapılandırma ölçek kümesi yönetilen hizmet kimliği (MSI) Azure CLI kullanma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, aşağıdaki yönetilen hizmet kimliği üzerinde bir Azure sanal makine ölçek kümesi (Azure CLI kullanarak VMSS), işlemleri öğrenin:
- Etkinleştirme ve bir Azure VMSS kimliğini atanan sistem devre dışı
- Ekleme ve bir Azure VMSS kimliğini atanmış bir kullanıcı kaldırma


## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile emin değilseniz, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [atanan sistemi ve kullanıcı kimliği atanır arasındaki farkı](overview.md#how-does-it-work)**.
- Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.

CLI komut dosyası örnekleri çalıştırmak için üç seçeneğiniz vardır:

- Kullanım [Azure bulut Kabuk](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
- Her kod bloğunun sağ üst köşesinde yer alan "deneyin" düğmesini, aracılığıyla katıştırılmış Azure bulut kabuğunu kullanın.
- [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya sonrası) yerel CLI konsol kullanmayı tercih ederseniz. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Sistem kimliği atanır

Bu bölümde, etkinleştirme ve devre dışı kimlik Azure CLI kullanarak bir Azure VMSS için atanan sistem öğrenin.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi oluşturma sırasında atanmış sistem kimliğini etkinleştir

Bir sanal makine ölçek oluşturmak için etkin kimliği atanır sistemiyle ayarlayın:

1. Yerel bir konsolda Azure CLI kullanıyorsanız, ilk kez Azure kullanarak oturum [az oturum açma](/cli/azure/reference-index#az_login). Altında sanal makine ölçek kümesini dağıtmak istediğiniz Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

   ```azurecli-interactive
   az login
   ```

2. Oluşturma bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md#terminology) kapsama ve sanal makine ölçek kümesini ve kullanarak kaynaklarıyla ilgili dağıtımı için [az grubu oluşturma](/cli/azure/group/#az_group_create). Bunun yerine kullanmak istediğiniz bir kaynak grubu zaten varsa bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Kullanılarak ayarlanan bir sanal makine ölçek oluşturmak [az vmss oluşturma](/cli/azure/vmss/#az_vmss_create) . Aşağıdaki örnek, bir sanal makine ölçek adlandırılmış kümesi oluşturur *myVMSS* tarafından istendiği gibi bir sistem atanan kimliği ile `--assign-identity` parametresi. `--admin-username` Ve `--admin-password` parametreleri sanal makine oturum açma için yönetici kullanıcı adı ve parola hesabı belirtin. Bu değerleri, ortamınız için uygun şekilde güncelleştirin: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Var olan bir Azure sanal makine ölçek kümesi üzerinde atanan sistem kimliğini etkinleştir

Varolan bir Azure sanal makine ölçek kümesini atanan sistem kimliğini etkinleştirmeniz gerekirse:

1. Yerel bir konsolda Azure CLI kullanıyorsanız, ilk kez Azure kullanarak oturum [az oturum açma](/cli/azure/reference-index#az_login). Sanal makine ölçek kümesini içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```azurecli-interactive
   az login
   ```

2. Kullanım [az vmss kimlik atamak](/cli/azure/vmss/identity/#az_vmss_identity_assign) komutu mevcut bir VM'yi atanan sistem kimliğine etkinleştirmek için:

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi atanan sistem kimlikten devre dışı bırak

> [!NOTE]
> Yönetilen hizmet kimliğinden bir sanal makine ölçek kümesi devre dışı bırakma şu anda desteklenmiyor. Bu arada, sistem atanan ve kullanıcı atanan kimlikler kullanma arasında geçiş yapabilirsiniz. Geri güncelleştirmeleri denetleyin.

Artık kimlik atanmış bir sistem gerekiyor, ancak hala kimlikleri atanan kullanıcı gereken bir sanal makine ölçek kümesi varsa, aşağıdaki komutu gerçekleştirin:

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='UserAssigned' 
```

MSI VM uzantısı kaldırmak için kullanın [az vmss kimliğini kaldırma](/cli/azure/vmss/identity/#az_vmss_remove_identity) atanan sistem kimliği VMSS kaldırmak için komutu:

   ```azurecli-interactive
   az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
   ```

## <a name="user-assigned-identity"></a>Kullanıcı kimliği atanır

Bu bölümde, etkinleştirme ve Azure CLI kullanarak bir atanan kullanıcı kimliğini kaldırma öğrenin.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Bir Azure VMSS oluşturma sırasında kimlik atanan kullanıcı atama

Bu bölümde bir VMSS oluşturulmasını ve atama kimlik VMSS atanmış bir kullanıcı, size yol göstermektedir. Kullanmak istediğiniz bir VMSS zaten varsa bu bölüm atlayın ve sonraki devam edin.

1. Kullanmak istediğiniz bir kaynak grubu zaten varsa bu adımı atlayabilirsiniz. Oluşturma bir [kaynak grubu](~/articles/azure-resource-manager/resource-group-overview.md#terminology) kapsama ve atanan kullanıcı kimliğinizi dağıtımını kullanarak [az grubu oluşturma](/cli/azure/group/#az_group_create). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<LOCATION>` parametre değerlerini kendi değerlere sahip. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Kimlik bilgileriniz kullanılarak atanmış bir kullanıcı oluşturmak [az kimliği oluşturma](/cli/azure/identity#az-identity-create).  `-g` Parametresi, burada kimlik atanmış kullanıcı oluşturuldu, kaynak grubu belirtir ve `-n` parametresi adını belirtir. Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizi ile:

    > [!IMPORTANT]
    > Kullanıcı adında özel karakterler (örneğin, alt çizgi) kimliklerle atanan oluşturma şu anda desteklenmiyor. Lütfen alfasayısal karakterler kullanın. Geri güncelleştirmeleri denetleyin.  Daha fazla bilgi için bkz: [SSS ve bilinen sorunlar](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
Yanıt oluşturulan, aşağıdakine benzer atanan kullanıcı kimliğini ayrıntılarını içerir. Kaynak `id` değeri atanmış kullanıcı kimliği atanır, aşağıdaki adımda kullanılır.

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

3. VMSS kullanarak oluşturduğunuz [az vmss oluşturma](/cli/azure/vmss/#az-vmss-create). Aşağıdaki örnek tarafından belirtilen yeni atanan kullanıcı kimliği ile ilişkili bir VMSS oluşturur `--assign-identity` parametresi. Değiştirdiğinizden emin olun `<RESOURCE GROUP>`, `<VMSS NAME>`, `<USER NAME>`, `<PASSWORD>`, ve `<USER ASSIGNED IDENTITY ID>` parametre değerlerini kendi değerlere sahip. İçin `<USER ASSIGNED IDENTITY ID>`, kullanıcı tarafından atanan kimliğin kaynağı kullanmak `id` önceki adımda oluşturduğunuz özelliği: 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY ID>
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Mevcut bir Azure VM'i kimlik atanan kullanıcı atama

1. Kimlik bilgileriniz kullanılarak atanmış bir kullanıcı oluşturmak [az kimliği oluşturma](/cli/azure/identity#az-identity-create).  `-g` Parametresi, burada kimlik atanmış kullanıcı oluşturuldu, kaynak grubu belirtir ve `-n` parametresi adını belirtir. Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<USER ASSIGNED IDENTITY NAME>` parametre değerlerini kendi değerlerinizi ile:

    > [!IMPORTANT]
    > Kullanıcı adında özel karakterler (örneğin, alt çizgi) kimliklerle atanan oluşturma şu anda desteklenmiyor. Lütfen alfasayısal karakterler kullanın. Geri güncelleştirmeleri denetleyin.  Daha fazla bilgi için bkz: [SSS ve bilinen sorunlar](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
Yanıt oluşturulan, aşağıdakine benzer atanan kullanıcı kimliğini ayrıntılarını içerir. Kaynak `id` değeri atanmış kullanıcı kimliği atanır, aşağıdaki adımda kullanılır.

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

2. Atanan kullanıcı kimliğini kullanarak VMSS Ata [az vmss kimlik atamak](/cli/azure/vmss/identity#az_vm_assign_identity). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlere sahip. `<USER ASSIGNED IDENTITY ID>` Kullanıcıya atanan kimliğin kaynak olacak `id` özelliği, önceki adımda oluşturduğunuz olarak:

    ```azurecli-interactive
    az vmss assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities <USER ASSIGNED IDENTITY ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vmss"></a>Bir Azure VMSS kimliği atanır kullanıcıyı kaldırma

> [!NOTE]
>  Kimlik atanmış bir sistem olmadığı sürece tüm atanan kullanıcı kimlikleri bir sanal makine ölçek kümesi'den kaldırma şu anda, desteklenmiyor. 

Birden çok kullanıcı tarafından atanan kimlik bilgilerinizi VMSS varsa, tüm son bir kullanarak ancak kaldırabilirsiniz [az vmss kimliğini kaldırma](/cli/azure/vmss/identity#az-vmss-identity-remove). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlere sahip. `<MSI NAME>` Tarafından VM kullanmanın kimlik bölümünde bulunabilir kullanıcıya atanan kimliğin adı özelliği `az vm show`:

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
```
VMSS atanan sistem ve kimlikler atanan kullanıcı varsa, sadece atanan sistemini kullanacak şekilde geçerek kimlikleri atanan tüm kullanıcı kaldırabilirsiniz. Aşağıdaki komutu kullanın: 

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null
```

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen hizmet Kimliği'ne genel bakış](overview.md)
- Tam Azure için sanal makine ölçek oluşturma Hızlı Başlangıç kümesi, bakın: 

  - [CLI ile bir sanal makine ölçek kümesi oluşturma](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

















