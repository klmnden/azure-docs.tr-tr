---
title: "Azure CLI kullanarak bir Azure sanal makine ölçek üzerinde MSI yapılandırın"
description: "Bir Azure sanal makine ölçek Azure CLI kullanarak kümesinde, bir yönetilen hizmet Kimliği'ni (MSI) yapılandırma için adım yönergeler tarafından adım."
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
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: c58ad9cc8bfa16b11201735bcc513e6dd692a2a9
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2018
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>Sanal makine yapılandırma ölçek kümesi yönetilen hizmet kimliği (MSI) Azure CLI kullanma

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, etkinleştirmek ve Azure CLI kullanarak ayarlamak için bir Azure sanal makine ölçek MSI kaldırma öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

CLI komut dosyası örnekleri çalıştırmak için üç seçeneğiniz vardır:

- Kullanım [Azure bulut Kabuk](../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
- Her kod bloğunun sağ üst köşesinde yer alan "deneyin" düğmesini, aracılığıyla katıştırılmış Azure bulut kabuğunu kullanın.
- [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya sonrası) yerel CLI konsol kullanmayı tercih ederseniz. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="enable-msi-during-creation-of-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi oluşturma sırasında MSI etkinleştir

Bir MSI özellikli sanal makine ölçek oluşturmak için ayarlayın:

1. Yerel bir konsolda Azure CLI kullanıyorsanız, ilk kez Azure kullanarak oturum [az oturum açma](/cli/azure/#az_login). Altında sanal makine ölçek kümesini dağıtmak istediğiniz Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

   ```azurecli-interactive
   az login
   ```

2. Oluşturma bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md#terminology) kapsama ve sanal makine ölçek kümesini ve kullanarak kaynaklarıyla ilgili dağıtımı için [az grubu oluşturma](/cli/azure/group/#az_group_create). Bunun yerine kullanmak istediğiniz kaynak grubu zaten varsa bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Kullanılarak ayarlanan bir sanal makine ölçek oluşturmak [az vmss oluşturma](/cli/azure/vmss/#az_vmss_create) . Aşağıdaki örnek, bir sanal makine ölçek adlandırılmış kümesi oluşturur *myVMSS* tarafından istendiği gibi bir MSI ile `--assign-identity` parametresi. `--admin-username` Ve `--admin-password` parametreleri sanal makine oturum açma için yönetici kullanıcı adı ve parola hesabı belirtin. Bu değerleri, ortamınız için uygun şekilde güncelleştirin: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

## <a name="enable-msi-on-an-existing-azure-virtual-machine-scale-set"></a>Var olan bir Azure sanal makine ölçek kümesinde MSI etkinleştir

Var olan bir Azure sanal makine ölçek kümesinde MSI etkinleştirmeniz gerekirse:

1. Yerel bir konsolda Azure CLI kullanıyorsanız, ilk kez Azure kullanarak oturum [az oturum açma](/cli/azure/#az_login). Sanal makine ölçek kümesini içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```azurecli-interactive
   az login
   ```

2. Kullanım [az vmss Ata-identity](/cli/azure/vm/#az_vmss_assign_identity) ile `--assign-identity` parametresi bir MSI için mevcut bir VM'yi eklemek için:

   ```azurecli-interactive
   az vmss assign-identity -g myResourceGroup -n myVMSS
   ```

## <a name="remove-msi-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden MSI kaldırma

Artık bir MSI gereken bir sanal makine ölçek kümesi varsa:

1. Yerel bir konsolda Azure CLI kullanıyorsanız, ilk kez Azure kullanarak oturum [az oturum açma](/cli/azure/#az_login). Sanal makine ölçek kümesini içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```azurecli-interactive
   az login
   ```

2. Kullanım `--identities` anahtarı ile [az vmss Kaldır-identity](/cli/azure/vmss/#az_vmss_remove_identity) MSI kaldırmak için:

   ```azurecli-interactive
   az vmss remove-identity -g myResourceGroup -n myVMSS --identities readerID writerID
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md)
- Tam Azure için sanal makine ölçek oluşturma Hızlı Başlangıç kümesi, bakın: 

  - [CLI ile bir sanal makine ölçek kümesi oluşturma](../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
















