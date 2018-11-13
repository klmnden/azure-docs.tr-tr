---
title: PowerShell kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen kimlik yapılandırma
description: Adım adım yapılandırma yönergeleri kimliklerini Azure VM'de PowerShell kullanarak Azure kaynakları için yönetilen.
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
ms.openlocfilehash: a29980da64775ca39f103b7430239f38c98a43fc
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578465"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-powershell"></a>PowerShell kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen kimlik Yapılandır

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, PowerShell kullanarak, Azure sanal makinesinde Azure kaynaklarını işlemleri için yönetilen şu kimlikler gerçekleştirmeyi öğreneceksiniz:

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Yükleme [Azure PowerShell'in en son sürümünü](https://www.powershellgallery.com/packages/AzureRM) henüz yapmadıysanız.

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, Azure PowerShell kullanarak sistem tarafından atanan yönetilen kimlik devre öğreneceksiniz.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında sistem tarafından atanan yönetilen kimlik etkinleştir

Etkin sistem tarafından atanan yönetilen kimlik ile bir Azure VM oluşturmak için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Aşağıdaki Azure VM yalnızca gerekli bölümlerini Tamamlanıyor hızlı Başlangıçlar, birine bakın ("Azure'da oturum açma", "kaynak grubu oluşturma", "Ağ grubu oluşturma", "VM oluşturma").
    
    "VM oluşturma" bölümüne aldığınızda, küçük bir değişiklik yapmak [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) cmdlet sözdizimi. Eklediğinizden emin olun bir `-AssignIdentity:$SystemAssigned` parametresi, sistem tarafından atanan kimliği etkinleştirildi, örneğin VM sağlamak için:
      
    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [PowerShell'i kullanarak bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md)
   - [PowerShell kullanarak bir Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md)

2. (İsteğe bağlı) Azure kaynaklarını VM uzantısı (Ocak 2019'da kullanımdan kaldırma planlanan) kullanmak için yönetilen kimlikleri ekleyin `-Type` parametresi [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM, türüne bağlı olarak geçirin ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır. VM uzantısı Ocak 2019'da kullanımdan kaldırma için planlanan Azure kaynakları için yönetilen kimlikleri. 

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>Yönetilen kimlik sistemi atanmış mevcut bir Azure sanal makinesinde etkinleştirin

Sistem tarafından atanan yönetilen kimlik olmadan ilk olarak sağlanan bir VM üzerinde etkinleştirmek için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Oturum açmak için Azure kullanarak `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```powershell
   Login-AzureRmAccount
   ```

2. İlk VM özelliklerini kullanarak alınması `Get-AzureRmVM` cmdlet'i. Sistem tarafından atanan bir yönetilen kimlik etkinleştirmek için ardından kullanmak `-AssignIdentity` açın [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```

3. (İsteğe bağlı) Azure kaynaklarını VM uzantısı (Ocak 2019'da kullanımdan kaldırma planlanan) kullanmak için yönetilen kimlikleri ekleyin `-Type` parametresi [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM, türüne bağlı olarak geçirin ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir. Doğru belirttiğinizden emin olun `-Location` parametre, var olan VM konumunu eşleşen:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.

## <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Yönetilen kimlik sistem tarafından atanan bir Azure VM'den devre dışı bırak

Yönetilen kimlik sistem tarafından atanan bir VM'de devre dışı bırakmak için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

Artık sistem tarafından atanan bir yönetilen kimlik gerekiyor ancak yine de kullanıcı tarafından atanan yönetilen kimlikleri gereken bir sanal makine varsa, aşağıdaki cmdlet'i kullanın:

1. Oturum açmak için Azure kullanarak `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```powershell
   Login-AzureRmAccount
   ```

2. Kullanarak VM özelliklerini almak `Get-AzureRmVM` cmdlet'i ve `-IdentityType` parametresi `UserAssigned`:

   ```powershell   
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM    
   Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType "UserAssigned"
   ```

Artık yönetilen kimlik sistem tarafından atanan gereken bir sanal makineye sahip ve kullanıcı tarafından atanan yönetilen kimlik varsa, aşağıdaki komutları kullanın:

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```

Azure kaynaklarını VM uzantısı, yönetilen kimlik bilgilerini kaldırmak için kullanıcı adı anahtarıyla [Remove-AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) uzantı eklendiğinde kullandığınız aynı adı belirterek cmdlet'ini:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, bir kullanıcı tarafından atanan bir yönetilen kimlik Azure PowerShell kullanarak bir sanal makineden ekleyip öğrenin.

### <a name="assign-a-user-assigned-managed-identity-to-a-vm-during-creation"></a>Kullanıcı tarafından atanan bir yönetilen kimlik oluşturma sırasında bir VM'ye atayın

Bir VM için bir kullanıcı tarafından atanan kimliği atamak için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamaları. Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Aşağıdaki Azure VM yalnızca gerekli bölümlerini Tamamlanıyor hızlı Başlangıçlar, birine bakın ("Azure'da oturum açma", "kaynak grubu oluşturma", "Ağ grubu oluşturma", "VM oluşturma"). 
  
    "VM oluşturma" bölümüne aldığınızda, küçük bir değişiklik yapmak [ `New-AzureRmVMConfig` ](/powershell/module/azurerm.compute/new-azurermvm) cmdlet sözdizimi. Ekleme `-IdentityType UserAssigned` ve `-IdentityID ` kullanıcı tarafından atanan bir kimlikle VM sağlamak için parametreleri.  Değiştirin `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, ve `<USER ASSIGNED IDENTITY NAME>` kendi değerlerinizle.  Örneğin:
    
    ```powershell 
    $vmConfig = New-AzureRmVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>..."
    ```
    
    - [PowerShell'i kullanarak bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md)
    - [PowerShell kullanarak bir Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md)

2. (İsteğe bağlı) VM uzantısıyla Azure kaynakları için yönetilen kimlik ekleme `-Type` parametresi [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM, türüne bağlı olarak geçirin ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir. Doğru belirttiğinizden emin olun `-Location` parametre, var olan VM konumunu eşleşen:
      > [!NOTE]
    > Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır. VM uzantısı Ocak 2019'da kullanımdan kaldırma için planlanan Azure kaynakları için yönetilen kimlikleri.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Kullanıcı tarafından atanan bir yönetilen kimlik mevcut bir Azure VM'ye atayın

Bir VM için bir kullanıcı tarafından atanan kimliği atamak için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamaları. Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Oturum açmak için Azure kullanarak `Connect-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```powershell
   Connect-AzureRmAccount
   ```

2. Bir kullanıcı tarafından atanan yönetilen kimlik kullanarak oluşturduğunuz [yeni AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) cmdlet'i.  Not `Id` çıktıda çünkü bu bir sonraki adımda gerekecektir.

   > [!IMPORTANT]
   > Kullanıcı tarafından atanan yönetilen kimlikleri yalnızca destekler alfasayısal oluşturma ve kısa çizgi (0-9 veya a-z veya A-Z veya -) karakter. Ayrıca, ad atama düzgün çalışması için VM/VMSS için 24 karakter uzunluğu sınırlı olmalıdır. Güncelleştirmeler için sonra yeniden denetleyin. Daha fazla bilgi için [SSS ve bilinen sorunlar](known-issues.md)

   ```powershell
   New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
   ```
3. Kullanarak VM özelliklerini almak `Get-AzureRmVM` cmdlet'i. Ardından, kullanıcı tarafından atanan bir yönetilen kimlik Azure VM'sine atanacak kullanın `-IdentityType` ve `-IdentityID` açın [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet'i.  Değeri`-IdentityId` parametresi `Id` , önceki adımda not ettiğiniz.  Değiştirin `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, ve `<USER ASSIGNED IDENTITY NAME>` kendi değerlerinizle.

   > [!WARNING]
   > VM'ye atanmış tüm daha önce kullanıcı tarafından atanan yönetilen kimlikleri korumak için sorgu `Identity` VM nesnesinin özelliği (örneğin, `$vm.Identity`).  Herhangi bir kullanıcı atanmış yönetilen kimlikleri döndürülür, sanal Makineye atamak istediğiniz yeni kullanıcı tarafından atanan yönetilen kimliği ile birlikte aşağıdaki komutu ekleyin.

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. Azure kaynaklarını VM uzantısı (Ocak 2019'da kullanımdan kaldırma planlanan) kullanmak için yönetilen kimlik ekleme `-Type` parametresi [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM, türüne bağlı olarak geçirin ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir. Doğru belirtin `-Location` parametre, var olan VM konumunu eşleşen.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Kullanıcı tarafından atanan bir yönetilen kimlik bir Azure VM'den kaldırın.

Bir VM için bir kullanıcı tarafından atanan kimliği kaldırmak için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.

Sanal makinenize birden çok kullanıcı tarafından atanan yönetilen kimlik varsa, aşağıdaki komutları kullanarak tüm sonuncu kaldırabilirsiniz. `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<USER ASSIGNED IDENTITY NAME>` VM üzerinde kalmalıdır kullanıcı tarafından atanan yönetilen kimliğin adı özelliği. Bu bilgiler sorgulanarak bulunabilir `Identity` VM nesnesinin özelliği.  Örneğin, `$vm.Identity`:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType UserAssigned -IdentityID <USER ASSIGNED IDENTITY NAME>
```
Sanal makinenize bir sistem tarafından atanan yönetilen yoksa kimlik ve istediğiniz kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırın, aşağıdaki komutu kullanın:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```
Sanal makinenizin sistem tarafından atanan hem de kullanıcı tarafından atanan yönetilen kimlikleri varsa, tüm kullanıcı tarafından atanan yönetilen kimlikleri yalnızca sistem tarafından atanan yönetilen kimlikleri kullanmak için geçerek kaldırabilirsiniz.

```powershell 
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlik](overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç kılavuzları, bkz:
  
  - [PowerShell ile bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md) 
