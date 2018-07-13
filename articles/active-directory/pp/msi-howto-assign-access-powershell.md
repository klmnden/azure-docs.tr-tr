---
title: PowerShell kullanarak bir Azure kaynağına MSI erişimi atama
description: Adım adım yönergeler bir kaynakta bir MSI atamak için PowerShell kullanarak başka bir kaynağa erişin.
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
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: ac8cca1e80defca33a879db5d4c160362314931a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610971"
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-powershell"></a>PowerShell kullanarak bir kaynak için bir yönetilen hizmet kimliği (MSI) erişim atama

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bir MSI ile bir Azure kaynağı yapılandırdıktan sonra herhangi bir güvenlik sorumlusu gibi başka bir kaynak için MSI erişimi verebilirsiniz. Bu örnek PowerShell kullanarak Azure depolama hesabınız için bir Azure sanal makinenin MSI erişimi verme gösterir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Ayrıca, yükleme [Azure PowerShell 4.3.1 sürümü](https://www.powershellgallery.com/packages/AzureRM/4.3.1) henüz yapmadıysanız.

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Başka bir kaynağa MSI erişimi atamak için RBAC kullanma

Bir Azure kaynağında MSI etkinleştirdikten sonra [Azure VM'deki gibi](msi-qs-configure-powershell-windows-vm.md):

1. Oturum açmak için Azure kullanarak `Connect-AzureRmAccount` cmdlet'i. MSI altında yapılandırdığınız Azure aboneliği ile ilişkili olan bir hesabı kullanın:

   ```powershell
   Connect-AzureRmAccount
   ```
2. Bu örnekte biz bir depolama hesabı için bir Azure VM erişimini vermiş olursunuz. Önce kullandığımız [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm) "MSI etkinleştirdik oluşturulduğu myVM", adlı VM için hizmet sorumlusu alınamıyor. Ardından, kullandığımız [New-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment) VM vermek için "Okuyucu" erişimi bir depolama hesabına "myStorageAcct" olarak adlandırılan:

    ```powershell
    $spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="troubleshooting"></a>Sorun giderme

Kaynak için MSI kullanılabilir kimlikleri listesinde görünmüyor, MSI doğru etkin olduğunu doğrulayın. Örneğimizde, biz Azure VM gidip [Azure portalında](https://portal.azure.com) ve:

- "Yapılandırma" sayfasına bakın ve MSI etkin olmak = "Yes"
- "Uzantıları" sayfasına bakın ve MSI uzantı başarıyla dağıtıldığından emin olun.

Ya da yanlışsa kaynağınızda MSI yeniden dağıtmanız veya dağıtım hatasıyla ilgili sorunları giderme gerekebilir.

## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Azure VM'deki MSI etkinleştirmek için bkz: [bir Azure VM yönetilen hizmet kimliği (PowerShell kullanarak MSI) yapılandırma](msi-qs-configure-powershell-windows-vm.md).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.

