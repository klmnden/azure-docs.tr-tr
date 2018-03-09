---
title: "Azure CLI kullanarak bir Azure VM'deki MSI yapılandırma"
description: "Tarafından yönetilen hizmet kimliği (MSI) Azure Azure CLI kullanarak VM üzerinde yapılandırma için adım yönergeler adım."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: d5fc509efa0437af93326d716c37514be82e8af1
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2018
---
# <a name="configure-a-vm-managed-service-identity-msi-using-azure-cli"></a>Bir VM yönetilen hizmet kimliği (Azure CLI kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, Azure Azure CLI kullanarak VM için MSI kaldırma etkinleştirip öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

CLI komut dosyası örnekleri çalıştırmak için üç seçeneğiniz vardır:

- Kullanım [Azure bulut Kabuk](../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
- Her kod bloğunun sağ üst köşesinde yer alan "deneyin" düğmesini, aracılığıyla katıştırılmış Azure bulut kabuğunu kullanın.
- [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya sonrası) yerel CLI konsol kullanmayı tercih ederseniz. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında MSI etkinleştir

MSI etkin bir VM oluşturmak için:

1. Yerel bir konsolda Azure CLI kullanıyorsanız, ilk kez Azure kullanarak oturum [az oturum açma](/cli/azure/#az_login). Altında VM dağıtmak istediğiniz Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

   ```azurecli-interactive
   az login
   ```

2. Oluşturma bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md#terminology) kapsama ve VM'nizi ve kullanarak kaynaklarıyla ilgili dağıtımı için [az grubu oluşturma](/cli/azure/group/#az_group_create). Bunun yerine kullanmak istediğiniz kaynak grubu zaten varsa bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Kullanarak bir VM oluşturun [az vm oluşturma](/cli/azure/vm/#az_vm_create). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVM* tarafından istendiği gibi bir MSI ile `--assign-identity` parametresi. `--admin-username` Ve `--admin-password` parametreleri sanal makine oturum açma için yönetici kullanıcı adı ve parola hesabı belirtin. Bu değerleri, ortamınız için uygun şekilde güncelleştirin: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>Var olan Azure VM'de MSI etkinleştir

Var olan bir sanal makine üzerinde MSI etkinleştirmeniz gerekirse:

1. Yerel bir konsolda Azure CLI kullanıyorsanız, ilk kez Azure kullanarak oturum [az oturum açma](/cli/azure/#az_login). VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```azurecli-interactive
   az login
   ```

2. Kullanım [az vm Ata-identity](/cli/azure/vm/#az_vm_assign_identity) ile `--assign-identity` parametresi bir MSI için mevcut bir VM'yi eklemek için:

   ```azurecli-interactive
   az vm assign-identity -g myResourceGroup -n myVm
   ```

## <a name="remove-msi-from-an-azure-vm"></a>MSI bir Azure sanal makineden kaldırın

Bir sanal makine varsa, artık bir MSI gerekir:

1. Yerel bir konsolda Azure CLI kullanıyorsanız, ilk kez Azure kullanarak oturum [az oturum açma](/cli/azure/#az_login). VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```azurecli-interactive
   az login
   ```

2. Kullanım `-n ManagedIdentityExtensionForWindows` veya `-n ManagedIdentityExtensionForLinux` anahtarı (VM türünü bağlı olarak) [az vm uzantısı delete](https://docs.microsoft.com/cli/azure/vm/#assign-identity) MSI kaldırmak için:

   ```azurecli-interactive
   az vm extension delete --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
   ```

## <a name="related-content"></a>İlgili içerik

- [Yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç ipuçları, bakın: 

  - [CLI ile Windows sanal makine oluşturma](../virtual-machines/windows/quick-create-cli.md)  
  - [CLI ile Linux sanal makine oluşturma](../virtual-machines/linux/quick-create-cli.md) 

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
















