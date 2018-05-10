---
title: PowerShell kullanarak bir Azure VM'deki MSI yapılandırma
description: Azure PowerShell kullanarak bir VM, bir yönetilen hizmet Kimliği'ni (MSI) yapılandırma için adım yönergeler tarafından adım.
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
ms.date: 11/27/2017
ms.author: daveba
ms.openlocfilehash: 6981c0f917fb7175f444ceca8c55c0df186774db
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="configure-a-vm-managed-service-identity-msi-using-powershell"></a>Bir VM yönetilen hizmet kimliği (PowerShell kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

<a name="in-this-article-you-learn-how-to-perform-the-following-managed-service-identity-operations-on-an-azure-vm-using-powershell"></a>Bu makalede, aşağıdaki PowerShell kullanarak bir Azure VM yönetilen hizmet kimliği işlemleri gerçekleştirmek nasıl öğrenin:
- 

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile emin değilseniz, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [atanan sistemi ve kullanıcı kimliği atanır arasındaki farkı](overview.md#how-does-it-work)**.
- Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.
- Yükleme [Azure PowerShell'in en son sürümünü](https://www.powershellgallery.com/packages/AzureRM) yapmadıysanız.

## <a name="system-assigned-identity"></a>Sistem kimliği atanır

Bu bölümde, etkinleştirme ve devre dışı Azure PowerShell kullanarak sistem atanan kimlik öğreneceksiniz.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında kimliği atanır Sistemi'ni etkinleştir

Etkin kimliği atanır sistemi ile bir Azure VM oluşturmak için:

1. Aşağıdaki Azure VM yalnızca gerekli bölümlerini Tamamlanıyor hızlı Başlangıçların birine bakın ("Oturum Azure'a", "kaynak grubu oluşturma", "Ağ Grup oluşturma", "VM oluşturma").
    
    "VM oluşturma" bölümüne aldığınızda, küçük bir değişiklik yapmak [yeni AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) cmdlet sözdizimi. Eklediğinizden emin olun bir `-AssignIdentity "SystemAssigned"` VM etkinleştirilirse, örneğin atanan sistem kimlikle sağlamak için parametre:
      
    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName myVM -AssignIdentity "SystemAssigned" ...
    ```

   - [PowerShell kullanarak bir Windows sanal makine oluşturun](../../virtual-machines/windows/quick-create-powershell.md)
   - [PowerShell kullanarak bir Linux sanal makine oluşturun](../../virtual-machines/linux/quick-create-powershell.md)

2. (İsteğe bağlı) MSI VM uzantısı kullanılarak eklemek `-Type` parametresini [kümesi AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM türüne bağlı olarak geçirmek ve kullanarak ad `-Name` parametresi. `-Settings` Parametresi için belirteç edinme OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Azure örneği meta veri hizmeti (IMDS) kimlik endpoint de belirteçleri almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Mevcut bir Azure VM'i kimliğini atanan Sistemi'ni etkinleştir

Varolan bir sanal makineye atanan sistem kimliğini etkinleştirmeniz gerekirse:

1. Oturum Azure kullanmaya `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Login-AzureRmAccount
   ```

2. İlk kullanarak VM özelliklerini almak `Get-AzureRmVM` cmdlet'i. Bir sistem atanan kimlik etkinleştirmek için kullanın `-AssignIdentity` anahtarının [güncelleştirme-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity "SystemAssigned"
   ```

3. (İsteğe bağlı) MSI VM uzantısı kullanılarak eklemek `-Type` parametresini [kümesi AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM türüne bağlı olarak geçirmek ve kullanarak ad `-Name` parametresi. `-Settings` Parametresi için belirteç edinme OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir. Doğru belirttiğinizden emin olun `-Location` parametresi, var olan VM konumunu eşleşen:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Azure örneği meta veri hizmeti (IMDS) kimlik endpoint de belirteçleri almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.

## <a name="disable-the-system-assigned-identity-from-an-azure-vm"></a>Bir Azure sanal makineden kimliği atanır sisteminizi devre dışı

> [!NOTE]
>  Yönetilen hizmet kimliği, bir sanal makineden devre dışı bırakma şu anda desteklenmiyor. Bu arada, sistem atanan ve kullanıcı atanan kimlikler kullanma arasında geçiş yapabilirsiniz.

Artık kimliği atanır sistem gerekiyor ancak hala kimlikleri atanan kullanıcı gereken bir sanal makine varsa, aşağıdaki cmdlet'i kullanın:

1. Oturum Azure kullanmaya `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Login-AzureRmAccount
   ```

2. Aşağıdaki cmdlet'i çalıştırın: 
    ```powershell       
    Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -IdentityType "UserAssigned"
    ```
MSI VM uzantısı kaldırmak için kullanıcı adı anahtarıyla [Kaldır AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) uzantısı eklendiğinde kullandığınız aynı adı belirterek cmdlet:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-identity"></a>Kullanıcı kimliği atanır

Bu bölümde, Azure PowerShell kullanarak bir sanal makineden kimlik atanmış bir kullanıcı ekleyip öğrenin.

### <a name="assign-a-user-assigned-identity-to-a-vm-during-creation"></a>Kimlik için bir VM oluşturma sırasında atanmış kullanıcı atama

VM oluşturulurken bir Azure VM'ye atanan kullanıcı kimliğini atamak için:

1. Aşağıdaki Azure VM yalnızca gerekli bölümlerini Tamamlanıyor hızlı Başlangıçların birine bakın ("Oturum Azure'a", "kaynak grubu oluşturma", "Ağ Grup oluşturma", "VM oluşturma"). 
  
    "VM oluşturma" bölümüne aldığınızda, küçük bir değişiklik yapmak [ `New-AzureRmVMConfig` ](/powershell/module/azurerm.compute/new-azurermvm) cmdlet sözdizimi. Ekleme `-IdentityType UserAssigned` ve `-IdentityID ` VM atanan kullanıcı kimliği ile sağlamak için parametreler.  Değiştir `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, ve `<MSI NAME>` kendi değerlere sahip.  Örneğin:
    
    ```powershell 
    $vmConfig = New-AzureRmVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>..."
    ```
    
    - [PowerShell kullanarak bir Windows sanal makine oluşturun](../../virtual-machines/windows/quick-create-powershell.md)
    - [PowerShell kullanarak bir Linux sanal makine oluşturun](../../virtual-machines/linux/quick-create-powershell.md)

2. (İsteğe bağlı) MSI VM uzantısı kullanılarak eklemek `-Type` parametresini [kümesi AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM türüne bağlı olarak geçirmek ve kullanarak ad `-Name` parametresi. `-Settings` Parametresi için belirteç edinme OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir. Doğru belirttiğinizden emin olun `-Location` parametresi, var olan VM konumunu eşleşen:
      > [!NOTE]
    > Azure örneği meta veri hizmeti (IMDS) kimlik endpoint de belirteçleri almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-identity-to-an-existing-azure-vm"></a>Bir kullanıcı kimliği mevcut bir Azure VM'i atayın

Bir kullanıcı atamak için mevcut bir Azure VM'i kimliği atanır:

1. Oturum Azure kullanmaya `Connect-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Connect-AzureRmAccount
   ```

2. Kimlik bilgileriniz kullanılarak atanmış bir kullanıcı oluşturmak [yeni AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) cmdlet'i.  Not `Id` çıktıda çünkü bu sonraki adımda gerekecektir.

    > [!IMPORTANT]
    > Atanan kullanıcı kimlikleri yalnızca destekler alfasayısal oluşturma ve tire (0-9 veya a-z veya A-Z veya -) karakter. Ayrıca, ad atama düzgün çalışması için VM/VMSS için 24 karakter uzunluğu sınırlı olmalıdır. Geri güncelleştirmeleri denetleyin. Daha fazla bilgi için bkz: [SSS ve bilinen sorunlar](known-issues.md)


  ```powershell
  New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
  ```
3. Kullanarak VM özelliklerini almak `Get-AzureRmVM` cmdlet'i. Ardından Azure VM'ye atanan kullanıcı kimliğini atamak için kullandığınız `-IdentityType` ve `-IdentityID` anahtarının [güncelleştirme-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet'i.  Değeri`-IdentityId` parametresi `Id` , önceki adımda not ettiğiniz.  Değiştir `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, ve `<USER ASSIGNED IDENTITY NAME>` kendi değerlere sahip.

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. MSI VM uzantısı kullanılarak eklemek `-Type` parametresini [kümesi AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM türüne bağlı olarak geçirmek ve kullanarak ad `-Name` parametresi. `-Settings` Parametresi için belirteç edinme OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir. Doğru belirtin `-Location` var olan VM konumunu eşleşen parametresi.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Bir Azure sanal makineden yönetilen kimliği atanır kullanıcıyı kaldırma

> [!NOTE]
>  Kimlik atanmış bir sistem olmadığı sürece tüm atanan kullanıcı kimlikleri bir sanal makineden kaldırma şu anda, desteklenmiyor. Geri güncelleştirmeleri denetleyin.

VM'yi birden çok kullanıcı tarafından atanan kimlikleri varsa, aşağıdaki komutları kullanarak tüm sonuncu kaldırabilirsiniz. Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlere sahip. `<MSI NAME>` VM kalacağı kullanıcıya atanan kimliğin adı özelliği. Bu bilgiler tarafından VM kullanmanın kimlik bölümünde bulunabilir `az vm show`:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
$vm.Identity.IdentityIds = "<MSI NAME>"
Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -VirtualMachine $vm
```

VM atanan sistem ve kimlikler atanan kullanıcı varsa, sadece atanan sistemini kullanacak şekilde geçerek kimlikleri atanan tüm kullanıcı kaldırabilirsiniz. Aşağıdaki komutu kullanın:

```powershell 
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
$vm.Identity.IdentityIds = $null
Update-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>İlgili içerik

- [Yönetilen hizmet Kimliği'ne genel bakış](overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç ipuçları, bakın:
  
  - [PowerShell ile Windows sanal makine oluşturma](../../virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makine oluşturma](../../virtual-machines/linux/quick-create-powershell.md) 
