---
title: PowerShell kullanarak bir Azure kaynağına MSI erişimi atama
description: Adım adım yönergeler bir kaynakta bir MSI atamak için PowerShell kullanarak başka bir kaynağa erişin.
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
ms.openlocfilehash: 936d549469df2cf4c303f0c3fd185f07281bb69b
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47107090"
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-powershell"></a>PowerShell kullanarak bir kaynak için bir yönetilen hizmet kimliği (MSI) erişim atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bir MSI ile bir Azure kaynağı yapılandırdıktan sonra herhangi bir güvenlik sorumlusu gibi başka bir kaynak için MSI erişimi verebilirsiniz. Bu örnek PowerShell kullanarak Azure depolama hesabınız için bir Azure sanal makinenin MSI erişimi verme gösterir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Ayrıca, yükleme [Azure PowerShell 4.3.1 sürümü](https://www.powershellgallery.com/packages/AzureRM/4.3.1) henüz yapmadıysanız.

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Başka bir kaynağa MSI erişimi atamak için RBAC kullanma

Bir Azure kaynağında MSI etkinleştirdikten sonra [Azure VM'deki gibi](qs-configure-powershell-windows-vm.md):

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

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](overview.md).
- Azure VM'deki MSI etkinleştirmek için bkz: [bir Azure VM yönetilen hizmet kimliği (PowerShell kullanarak MSI) yapılandırma](qs-configure-powershell-windows-vm.md).

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.

