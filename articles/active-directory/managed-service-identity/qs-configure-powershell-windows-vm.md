---
title: PowerShell kullanarak bir Azure sanal makinesinde MSI yapılandırma
description: Azure PowerShell kullanarak sanal makinesinde, bir yönetilen hizmet kimliği (MSI) yapılandırma yönergeleri adım adım.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: daveba
ms.openlocfilehash: add61dbbdaa90ae23e200163f1fa962adc2b3b8e
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37902104"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-powershell"></a>Bir VM yönetilen hizmet kimliği (PowerShell kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

<a name="in-this-article-you-learn-how-to-perform-the-following-managed-service-identity-operations-on-an-azure-vm-using-powershell"></a>Bu makalede, PowerShell kullanarak bir Azure sanal makinesinde aşağıdaki yönetilen hizmet kimliği işlemlerini nasıl gerçekleştireceğinizi öğrenin:
- 

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Azure hesabınız yoksa, [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.
- Yükleme [Azure PowerShell'in en son sürümünü](https://www.powershellgallery.com/packages/AzureRM) henüz yapmadıysanız.

## <a name="system-assigned-identity"></a>Sistem tarafından atanan kimlik

Bu bölümde, etkinleştirin ve Azure PowerShell kullanarak sistem tarafından atanan kimliği devre dışı öğreneceksiniz.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında atanan kimliği Sistemi'ni etkinleştir

Azure VM ile sistem oluşturma etkinleştirildi kimlik atanan:

1. Aşağıdaki Azure VM yalnızca gerekli bölümlerini Tamamlanıyor hızlı Başlangıçlar, birine bakın ("Azure'da oturum açma", "kaynak grubu oluşturma", "Ağ grubu oluşturma", "VM oluşturma").
    
    "VM oluşturma" bölümüne aldığınızda, küçük bir değişiklik yapmak [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) cmdlet sözdizimi. Eklediğinizden emin olun bir `-AssignIdentity:$SystemAssigned` parametresi, örneğin etkin sistem tarafından atanan kimlikle VM sağlamak için:
      
    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [PowerShell'i kullanarak bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md)
   - [PowerShell kullanarak bir Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md)

2. (İsteğe bağlı) MSI VM uzantısı kullanılarak ekleme `-Type` parametresi [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM, türüne bağlı olarak geçirin ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Atanan kimliği mevcut bir Azure sanal makinesinde Sistemi'ni etkinleştir

Mevcut bir sanal makine üzerinde bir sistem tarafından atanan kimliği etkinleştirmeniz gerekirse:

1. Oturum açmak için Azure kullanarak `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Login-AzureRmAccount
   ```

2. İlk VM özelliklerini kullanarak alınması `Get-AzureRmVM` cmdlet'i. Bir sistem tarafından atanan kimliği etkinleştirmek için ardından kullanmak `-AssignIdentity` açın [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```

3. (İsteğe bağlı) MSI VM uzantısı kullanılarak ekleme `-Type` parametresi [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM, türüne bağlı olarak geçirin ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir. Doğru belirttiğinizden emin olun `-Location` parametre, var olan VM konumunu eşleşen:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.

## <a name="disable-the-system-assigned-identity-from-an-azure-vm"></a>Bir Azure VM'den atanan kimliği sistemi devre dışı bırak

> [!NOTE]
>  Bir sanal makineden yönetilen hizmet kimliği devre dışı bırakma şu anda desteklenmiyor. Bu arada, sistem atanan ve atanan kullanıcı kimliklerini kullanma arasında geçiş yapabilirsiniz.

Artık sistem tarafından atanan kimlik gerekiyor ancak yine de kullanıcı tarafından atanan kimliklerle gereken bir sanal makine varsa, aşağıdaki cmdlet'i kullanın:

1. Oturum açmak için Azure kullanarak `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Login-AzureRmAccount
   ```

2. Aşağıdaki cmdlet'i çalıştırın: 
    ```powershell       
    Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -IdentityType "UserAssigned"
    ```
MSI VM uzantısı'nı kaldırmak için kullanıcı adı anahtarıyla [Remove-AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) uzantı eklendiğinde kullandığınız aynı adı belirterek cmdlet'ini:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği

Bu bölümde, bir kullanıcı tarafından atanan kimliği Azure PowerShell kullanarak bir sanal makineden ekleyip öğrenin.

### <a name="assign-a-user-assigned-identity-to-a-vm-during-creation"></a>Bir kullanıcı tarafından atanan kimliği için bir VM oluşturma sırasında atama

Bir kullanıcı atamak için VM'yi oluştururken kimlik bir Azure VM'sine atanan:

1. Aşağıdaki Azure VM yalnızca gerekli bölümlerini Tamamlanıyor hızlı Başlangıçlar, birine bakın ("Azure'da oturum açma", "kaynak grubu oluşturma", "Ağ grubu oluşturma", "VM oluşturma"). 
  
    "VM oluşturma" bölümüne aldığınızda, küçük bir değişiklik yapmak [ `New-AzureRmVMConfig` ](/powershell/module/azurerm.compute/new-azurermvm) cmdlet sözdizimi. Ekleme `-IdentityType UserAssigned` ve `-IdentityID ` bir kullanıcı tarafından atanan kimlikle VM sağlamak için parametreleri.  Değiştirin `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, ve `<MSI NAME>` kendi değerlerinizle.  Örneğin:
    
    ```powershell 
    $vmConfig = New-AzureRmVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>..."
    ```
    
    - [PowerShell'i kullanarak bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md)
    - [PowerShell kullanarak bir Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md)

2. (İsteğe bağlı) MSI VM uzantısı kullanılarak ekleme `-Type` parametresi [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM, türüne bağlı olarak geçirin ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir. Doğru belirttiğinizden emin olun `-Location` parametre, var olan VM konumunu eşleşen:
      > [!NOTE]
    > Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-identity-to-an-existing-azure-vm"></a>Bir kullanıcı kimliği mevcut bir Azure VM'ye atayın

Bir kullanıcı tarafından atanan kimliği mevcut bir Azure VM'ye atamak için:

1. Oturum açmak için Azure kullanarak `Connect-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Connect-AzureRmAccount
   ```

2. Kimlik bilgileriniz kullanılarak atanan bir kullanıcı oluşturmak [yeni AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) cmdlet'i.  Not `Id` çıktıda çünkü bu bir sonraki adımda gerekecektir.

    > [!IMPORTANT]
    > Kullanıcı tarafından atanan kimlikleri yalnızca destekler alfasayısal oluşturma ve kısa çizgi (0-9 veya a-z veya A-Z veya -) karakter. Ayrıca, ad atama düzgün çalışması için VM/VMSS için 24 karakter uzunluğu sınırlı olmalıdır. Geri güncelleştirmeleri denetleyin. Daha fazla bilgi için [SSS ve bilinen sorunlar](known-issues.md)


  ```powershell
  New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
  ```
3. Kullanarak VM özelliklerini almak `Get-AzureRmVM` cmdlet'i. Ardından Azure VM'sine atanan kullanıcı kimliği atamak için kullan `-IdentityType` ve `-IdentityID` açın [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet'i.  Değeri`-IdentityId` parametresi `Id` , önceki adımda not ettiğiniz.  Değiştirin `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, ve `<USER ASSIGNED IDENTITY NAME>` kendi değerlerinizle.

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. MSI VM uzantısı kullanılarak ekleme `-Type` parametresi [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM, türüne bağlı olarak geçirin ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir. Doğru belirtin `-Location` parametre, var olan VM konumunu eşleşen.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Bir Azure VM'den yönetilen kimlik atanmış bir kullanıcıyı kaldırma

> [!NOTE]
>  Bir sistem tarafından atanan kimlik olmadığı sürece tüm kullanıcı tarafından atanan kimlikleri bir sanal makineden kaldırılması şu anda, desteklenmiyor. Geri güncelleştirmeleri denetleyin.

Sanal makinenize birden çok kullanıcı tarafından atanan kimliği varsa, aşağıdaki komutları kullanarak tüm sonuncu kaldırabilirsiniz. Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlerinizle. `<MSI NAME>` VM üzerinde kalmalıdır atanan kullanıcı kimliğin adı özelliği. Bu bilgileri tarafından VM kullanarak kimlik bölümünde bulunabilir `az vm show`:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
$vm.Identity.IdentityIds = "<MSI NAME>"
Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -VirtualMachine $vm
```

Sanal makinenize atanmış sistem ve kullanıcı tarafından atanan kimliklerle varsa, yalnızca atanan sistemi kullanmaya geçiş tarafından atanan kimliklerle tüm kullanıcı kaldırabilirsiniz. Aşağıdaki komutu kullanın:

```powershell 
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
$vm.Identity.IdentityIds = $null
Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>İlgili içerik

- [Yönetilen hizmet Kimliği'ne genel bakış](overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç kılavuzları, bkz:
  
  - [PowerShell ile bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md) 
