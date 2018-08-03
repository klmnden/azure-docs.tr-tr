---
title: Azure CLI kullanarak bir Azure kaynağına MSI erişimi atama
description: Adım adım yönergeler bir kaynakta bir MSI atamak için Azure CLI kullanarak başka bir kaynağa erişin.
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
ms.date: 09/25/2017
ms.author: daveba
ms.openlocfilehash: a5da06eac7f4680282aad305f57cb9ca1c9d5730
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39424441"
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Azure CLI kullanarak bir kaynak için bir yönetilen hizmet kimliği (MSI) erişim atama

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bir MSI ile bir Azure kaynağı yapılandırdıktan sonra herhangi bir güvenlik sorumlusu gibi başka bir kaynak için MSI erişimi verebilirsiniz. Bu örnek Azure CLI kullanarak Azure depolama hesabınız için bir Azure sanal makine veya sanal makine ölçek kümesi'nin MSI erişimi vermek nasıl gösterir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

CLI betiği örnekleri çalıştırmak için üç seçeneğiniz vardır:

- Kullanım [Azure Cloud Shell](../../cloud-shell/overview.md) Azure portalından (sonraki bölüme bakın).
- Katıştırılmış Azure Cloud Shell aracılığıyla her kod bloğunun sağ üst köşesinde bulunan "Try It" düğmesini kullanın.
- [CLI 2. 0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 veya üzeri) yerel bir CLI konsol kullanmak istiyorsanız. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Başka bir kaynağa MSI erişimi atamak için RBAC kullanma

MSI gibi bir Azure kaynağında etkinleştirdikten sonra bir [Azure sanal makine](qs-configure-cli-windows-vm.md) veya [Azure sanal makine ölçek kümesi](qs-configure-cli-windows-vmss.md): 

1. Azure CLI'yi yerel bir konsolda kullanıyorsanız, önce [az login](/cli/azure/reference-index#az-login) kullanarak Azure'da oturum açın. Sanal makine veya sanal makine ölçek kümesini dağıtmak altında istediğiniz Azure aboneliği ile ilişkili olan bir hesabı kullanın:

   ```azurecli-interactive
   az login
   ```

2. Bu örnekte biz bir depolama hesabı için bir Azure sanal makine erişimini vermiş olursunuz. Önce kullandığımız [az kaynak listesi](/cli/azure/resource/#az-resource-list) "myVM" adlı sanal makine için hizmet sorumlusu almak için:

   ```azurecli-interactive
   spID=$(az resource list -n myVM --query [*].identity.principalId --out tsv)
   ```
   Bir Azure sanal makine ölçek kümesi için komutu burada dışında aynıdır, "DevTestVMSS" adlı sanal makine ölçek kümesi için hizmet sorumlusu Al:
   
   ```azurecli-interactive
   spID=$(az resource list -n DevTestVMSS --query [*].identity.principalId --out tsv)
   ```

3. Hizmet sorumlusu Kimliğini oluşturduktan sonra kullanma [az rol ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create) sanal makine veya sanal makine ölçek vermek için "Okuyucu" erişim "myStorageAcct" adlı bir depolama hesabı ayarlayın:

   ```azurecli-interactive
   az role assignment create --assignee $spID --role 'Reader' --scope /subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

## <a name="troubleshooting"></a>Sorun giderme

Kaynak için MSI kullanılabilir kimlikleri listesinde görünmüyor, MSI doğru etkin olduğunu doğrulayın. Örneğimizde, biz Azure sanal makine veya sanal makine ölçek kümesi gidip [Azure portalında](https://portal.azure.com) ve:

- "Yapılandırma" sayfasına bakın ve MSI etkin olmak = "Yes"
- "Uzantıları" sayfasına bakın ve MSI uzantı başarıyla dağıtıldığından emin olun (**uzantıları** sayfa bir Azure sanal makine ölçek kümesi için kullanılabilir değil).

Ya da yanlışsa kaynağınızda MSI yeniden dağıtmanız veya dağıtım hatasıyla ilgili sorunları giderme gerekebilir.

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](overview.md).
- Azure sanal makinesinde MSI etkinleştirmek için bkz: [bir Azure VM yönetilen hizmet kimliği (Azure CLI kullanarak MSI) yapılandırma](qs-configure-cli-windows-vm.md).
- Bir Azure sanal makine ölçek kümesinde MSI etkinleştirmek için bkz: [bir Azure sanal makine ölçek kümesi yönetilen hizmet kimliği (Azure portalını kullanarak MSI) yapılandırma](qs-configure-portal-windows-vmss.md)

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.

