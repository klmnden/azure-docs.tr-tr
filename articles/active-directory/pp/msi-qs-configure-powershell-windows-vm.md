---
title: PowerShell kullanarak bir Azure sanal makinesinde MSI yapılandırma
description: Azure PowerShell kullanarak sanal makinesinde, bir yönetilen hizmet kimliği (MSI) yapılandırma yönergeleri adım adım.
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
ms.openlocfilehash: 9a466a5c695277a7b5833f997e2ad7281c962f3f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611008"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-powershell"></a>Bir VM yönetilen hizmet kimliği (PowerShell kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, etkinleştirmek ve PowerShell kullanarak Azure VM için MSI kaldırma öğrenin.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Ayrıca, yükleme [Azure PowerShell'in en son sürümünü](https://www.powershellgallery.com/packages/AzureRM) (sürüm 4.3.1 veya sonraki) henüz yapmadıysanız.

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında MSI etkinleştir

MSI etkin bir VM oluşturmak için:

1. Aşağıdaki Azure VM yalnızca gerekli bölümlerini Tamamlanıyor hızlı Başlangıçlar, birine bakın ("Azure'da oturum açma", "kaynak grubu oluşturma", "Ağ grubu oluşturma", "VM oluşturma"). 

   > [!IMPORTANT] 
   > "VM oluşturma" bölümüne aldığınızda, küçük bir değişiklik yapmak [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) cmdlet sözdizimi. Eklediğinizden emin olun bir `-IdentityType "SystemAssigned"` parametresi bir MSI ile VM'yi örneğin sağlamak için:
   >  
   > `$vmConfig = New-AzureRmVMConfig -VMName myVM -IdentityType "SystemAssigned" ...`

   - [PowerShell'i kullanarak bir Windows sanal makinesi oluşturma](~/articles/virtual-machines/windows/quick-create-powershell.md)
   - [PowerShell kullanarak bir Linux sanal makinesi oluşturma](~/articles/virtual-machines/linux/quick-create-powershell.md)



2. MSI VM uzantısı kullanılarak ekleme `-Type` parametresi [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM, türüne bağlı olarak geçirin ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>Mevcut Azure VM'deki MSI etkinleştir

Var olan bir sanal makineye MSI etkinleştirmeniz gerekirse:

1. Oturum açmak için Azure kullanarak `Connect-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Connect-AzureRmAccount
   ```

2. İlk VM özelliklerini kullanarak alınması `Get-AzureRmVM` cmdlet'i. MSI etkinleştirmek için ardından kullanmak `-IdentityType` açın [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -IdentityType "SystemAssigned"
   ```

3. MSI VM uzantısı kullanılarak ekleme `-Type` parametresi [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM, türüne bağlı olarak geçirin ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir. Doğru belirttiğinizden emin olun `-Location` parametre, var olan VM konumunu eşleşen:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="remove-msi-from-an-azure-vm"></a>Bir Azure VM'den MSI Kaldır

Artık bir MSI gereken bir sanal makineye varsa, kullanabileceğiniz `RemoveAzureRmVMExtension` MSI sanal makineden kaldırmak için cmdlet:

1. Oturum açmak için Azure kullanarak `Connect-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Connect-AzureRmAccount
   ```

2. Kullanım `-Name` anahtarı ile [Remove-AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) uzantı eklendiğinde kullandığınız aynı adı belirterek cmdlet'ini:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="related-content"></a>İlgili içerik

- [Yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç kılavuzları, bkz:
  
  - [PowerShell ile bir Windows sanal makinesi oluşturma](~/articles/virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makinesi oluşturma](~/articles/virtual-machines/linux/quick-create-powershell.md) 

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.
















