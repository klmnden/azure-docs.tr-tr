---
title: "Azure CLI kullanarak bir Azure kaynağı için geçersiz bir MSI erişimi atama"
description: "Adım adım yönergeleri bir kaynakta bir MSI atamak için Azure CLI kullanarak başka bir kaynağa erişim."
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
ms.date: 09/25/2017
ms.author: daveba
ms.openlocfilehash: ca228a99eec1d376e456f5730aaaf8c569620f43
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Azure CLI kullanarak bir kaynak için bir yönetilen hizmet kimliği (MSI) erişimi atayın

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bir Azure kaynağı ile bir MSI yapılandırdıktan sonra herhangi bir güvenlik asıl gibi başka bir kaynak MSI erişim izni verebilirsiniz. Bu örnek bir Azure depolama hesabı için Azure CLI kullanarak bir Azure sanal makine veya sanal makine ölçek kümesinin MSI erişim vermek nasıl gösterir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

CLI komut dosyası örnekleri çalıştırmak için üç seçeneğiniz vardır:

- Kullanım [Azure bulut Kabuk](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
- Her kod bloğunun sağ üst köşesinde yer alan "deneyin" düğmesini, aracılığıyla katıştırılmış Azure bulut kabuğunu kullanın.
- [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya sonrası) yerel CLI konsol kullanmayı tercih ederseniz. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Başka bir kaynağa MSI erişim atamak için RBAC kullanın

Bir Azure kaynağı üzerinde gibi MSI etkinleştirdikten sonra bir [Azure sanal makinesi](qs-configure-cli-windows-vm.md) veya [Azure sanal makine ölçek kümesi](qs-configure-cli-windows-vmss.md): 

1. Yerel bir konsolda Azure CLI kullanıyorsanız, ilk kez Azure kullanarak oturum [az oturum açma](/cli/azure/reference-index#az_login). Altında VM veya sanal makine ölçek kümesini dağıtmak istediğiniz Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

   ```azurecli-interactive
   az login
   ```

2. Bu örnekte, biz bir depolama hesabı için bir Azure sanal makine erişimini vermiş olursunuz. İlk kullanırız [az kaynak listesi](/cli/azure/resource/#az_resource_list) hizmet sorumlusu "myVM" adlı sanal makinede almak için:

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```
   Bir Azure sanal makine ölçek kümesi için komut burada dışında aynı olduğundan, "DevTestVMSS" adlı sanal makine ölçek kümesi için hizmet sorumlusu alın:
   
   ```azurecli-interactive
   spID=$(az resource list -n DevTestVMSS --query [*].identity.principalId --out tsv)
   ```

3. Hizmet asıl Kimliğine sahip olduğunuzda kullanın [az rol ataması oluşturma](/cli/azure/role/assignment#az_role_assignment_create) sanal makine veya sanal makine ölçek vermek için "myStorageAcct" adlı bir depolama hesabı "Okuyucu" erişim ayarlayın:

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="troubleshooting"></a>Sorun giderme

Kaynak için MSI kullanılabilir kimlikleri listesinde görünmüyor, MSI doğru etkin olduğunu doğrulayın. Örneğimizde, biz Azure sanal makine veya sanal makine ölçek kümesinde dönebilirsiniz [Azure portal](https://portal.azure.com) ve:

- "Yapılandırma" sayfasına bakın ve etkin MSI olun = "Yes."
- "Uzantılarla" sayfasına bakın ve başarılı bir şekilde dağıtılan MSI uzantısı emin olun (**uzantıları** sayfa bir Azure sanal makine ölçek kümesi için kullanılabilir değil).

Ya da yanlışsa, kaynakta MSI yeniden dağıtmanız veya dağıtım hatası sorun giderme gerekebilir.

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](overview.md).
- Bir Azure sanal makinede MSI etkinleştirmek için bkz: [bir Azure VM yönetilen hizmet kimliği (Azure CLI kullanarak MSI) yapılandırma](qs-configure-cli-windows-vm.md).
- Bir Azure sanal makine ölçek kümesinde MSI etkinleştirmek için bkz: [bir Azure sanal makine ölçek kümesi yönetilen hizmet kimlik (Azure Portalı'nı kullanarak MSI) yapılandırma](qs-configure-portal-windows-vmss.md)

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.

