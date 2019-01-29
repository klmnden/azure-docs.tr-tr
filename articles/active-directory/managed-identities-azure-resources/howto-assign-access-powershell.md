---
title: PowerShell kullanarak bir Azure kaynağı için bir yönetilen kimlik erişim atama
description: Adım adım yönergeler bir kaynakta bir yönetilen kimlik atamak için PowerShell kullanarak başka bir kaynağa erişin.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2018
ms.author: priyamo
ms.openlocfilehash: 765276ce179c0d9858a39a62adc5ea0e96ae79ea
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188788"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-powershell"></a>PowerShell kullanarak bir kaynak için bir yönetilen kimlik erişim atama

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen bir kimlik ile bir Azure kaynağı yapılandırdıktan sonra herhangi bir güvenlik sorumlusu gibi başka bir kaynak yönetilen kimlik erişim izni verebilirsiniz. Bu örnek PowerShell kullanarak Azure depolama hesabınız için bir Azure sanal makinenin yönetilen kimlik erişim vermek nasıl gösterir.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Yükleme [Azure PowerShell'in en son sürümünü](/powershell/azure/install-az-ps) henüz yapmadıysanız.

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Başka bir kaynak için bir yönetilen kimlik erişimi atamak için RBAC kullanma

Yönetilen bir Azure kaynak kimliğini etkinleştirdikten sonra [Azure VM'deki gibi](qs-configure-powershell-windows-vm.md):

1. Oturum açmak için Azure kullanarak `Connect-AzAccount` cmdlet'i. Yönetilen kimlik altında yapılandırdığınız Azure aboneliği ile ilişkili olan bir hesabı kullanın:

   ```powershell
   Connect-AzAccount
   ```
2. Bu örnekte biz bir depolama hesabı için bir Azure VM erişimini vermiş olursunuz. Önce kullandığımız [Get-AzVM](/powershell/module/az.compute/get-azvm) adlı VM için hizmet sorumlusu almak için `myVM`, biz etkinleştirildiğinde oluşturulduğu yönetilen kimliği. Ardından, [yeni AzRoleAssignment](/powershell/module/Az.Resources/New-AzRoleAssignment) VM vermek **okuyucu** adlı bir depolama hesabı erişim `myStorageAcct`:

    ```powershell
    $spID = (Get-Az -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen kimlik Azure kaynaklarına genel bakış](overview.md)
- Azure VM'de yönetilen kimlik etkinleştirmek için bkz: [yapılandırma kimliklerini Azure VM'de PowerShell kullanarak Azure kaynakları için yönetilen](qs-configure-powershell-windows-vm.md).
