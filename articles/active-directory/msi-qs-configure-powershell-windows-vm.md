---
title: "PowerShell kullanarak bir Azure VM'deki MSI yapılandırma"
description: "Azure PowerShell kullanarak bir VM, bir yönetilen hizmet Kimliği'ni (MSI) yapılandırma için adım yönergeler tarafından adım."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: bryanla
ms.openlocfilehash: c166d5d4e0ae054e89eb3a5728f1d86f4e008c12
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="configure-a-vm-managed-service-identity-msi-using-powershell"></a>Bir VM yönetilen hizmet kimliği (PowerShell kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, etkinleştirme ve Azure PowerShell kullanarak VM için MSI kaldırma öğrenin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Ayrıca, yükleme [Azure PowerShell sürüm 4.3.1](https://www.powershellgallery.com/packages/AzureRM/4.3.1) yapmadıysanız.

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında MSI etkinleştir

MSI etkin bir VM oluşturmak için:

1. Aşağıdaki Azure VM yalnızca gerekli bölümlerini Tamamlanıyor hızlı Başlangıçların birine bakın ("Oturum Azure'a", "kaynak grubu oluşturma", "Ağ Grup oluşturma", "VM oluşturma"). 

   > [!IMPORTANT] 
   > "VM oluşturma" bölümüne aldığınızda, küçük bir değişiklik yapmak [yeni AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) cmdlet sözdizimi. Eklediğinizden emin olun bir `-IdentityType "SystemAssigned"` bir MSI VM'ye örneğin sağlamak için parametre:
   >  
   > `$vmConfig = New-AzureRmVMConfig -VMName myVM -IdentityType "SystemAssigned" ...`

   - [PowerShell kullanarak bir Windows sanal makine oluşturun](../virtual-machines/windows/quick-create-powershell.md)
   - [PowerShell kullanarak bir Linux sanal makine oluşturun](../virtual-machines/linux/quick-create-powershell.md)



2. MSI VM uzantısı kullanılarak eklemek `-Type` parametresini [kümesi AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM türüne bağlı olarak geçirmek ve kullanarak ad `-Name` parametresi. `-Settings` Parametresi için belirteç edinme OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>Var olan Azure VM'de MSI etkinleştir

Var olan bir sanal makine üzerinde MSI etkinleştirmeniz gerekirse:

1. Oturum Azure kullanmaya `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Login-AzureRmAccount
   ```

2. İlk kullanarak VM özelliklerini almak `Get-AzureRmVM` cmdlet'i. MSI etkinleştirmek için kullanın `-IdentityType` anahtarının [güncelleştirme-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -IdentityType "SystemAssigned"
   ```

3. MSI VM uzantısı kullanılarak eklemek `-Type` parametresini [kümesi AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM türüne bağlı olarak geçirmek ve kullanarak ad `-Name` parametresi. `-Settings` Parametresi için belirteç edinme OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir. Doğru belirttiğinizden emin olun `-Location` parametresi, var olan VM konumunu eşleşen:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="remove-msi-from-an-azure-vm"></a>MSI bir Azure sanal makineden kaldırın

Artık bir MSI gereken bir sanal makine varsa, kullanabileceğiniz `RemoveAzureRmVMExtension` cmdlet MSI sanal makineden kaldırmak için:

1. Oturum Azure kullanmaya `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Login-AzureRmAccount
   ```

2. Kullanım `-Name` anahtarı ile [Kaldır AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) uzantısı eklendiğinde kullandığınız aynı adı belirterek cmdlet:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="related-content"></a>İlgili içerik

- [Yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç ipuçları, bakın:
  
  - [PowerShell ile Windows sanal makine oluşturma](../virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makine oluşturma](../virtual-machines/linux/quick-create-powershell.md) 

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
















