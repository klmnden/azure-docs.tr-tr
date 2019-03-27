---
title: PowerShell kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen kimlik yapılandırma
description: Adım adım yapılandırma yönergeleri kimliklerini Azure VM'de PowerShell kullanarak Azure kaynakları için yönetilen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: eb0eacd90c3b748920e5f43bf669a36df7a3f17c
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58446994"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-powershell"></a>PowerShell kullanarak bir Azure sanal makinesinde Azure kaynakları için yönetilen kimlik Yapılandır

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, PowerShell kullanarak, Azure sanal makinesinde Azure kaynaklarını işlemleri için yönetilen şu kimlikler gerçekleştirmeyi öğrenin.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Yükleme [Azure PowerShell'in en son sürümünü](/powershell/azure/install-az-ps) henüz yapmadıysanız.

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, Azure PowerShell kullanarak sistem tarafından atanan yönetilen kimlik devre öğreneceksiniz.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Bir Azure VM oluşturma sırasında sistem tarafından atanan yönetilen kimlik etkinleştir

Etkin sistem tarafından atanan yönetilen kimlik ile bir Azure VM oluşturmak için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Aşağıdaki Azure VM yalnızca gerekli bölümlerini ("" Oluşturma kaynak grubu","Ağ Grup Oluştur","oluşturma"VM Azure'da oturum",) tamamlama hızlı Başlangıçlar, birine bakın.
    
    "VM oluşturma" bölümüne aldığınızda, küçük bir değişiklik yapmak [yeni AzVMConfig](/powershell/module/az.compute/new-azvm) cmdlet sözdizimi. Eklediğinizden emin olun bir `-AssignIdentity:$SystemAssigned` parametresi, sistem tarafından atanan kimliği etkinleştirildi, örneğin VM sağlamak için:
      
    ```powershell
    $vmConfig = New-AzVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [PowerShell'i kullanarak bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md)
   - [PowerShell kullanarak bir Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md)

> [!NOTE]
> İsteğe bağlı olarak, VM uzantısını Azure kaynakları için yönetilen kimlik sağlama, ancak yakında kullanımdan kaldırılacak. Azure örnek meta veri kimlik uç nokta kimlik doğrulaması için kullanmanızı öneririz. Daha fazla bilgi için [Azure IMDS uç nokta kimlik doğrulaması için VM uzantısı'ten geçiş](howto-migrate-vm-extension.md).

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>Yönetilen kimlik sistemi atanmış mevcut bir Azure sanal makinesinde etkinleştirin

Sistem tarafından atanan yönetilen kimlik olmadan ilk olarak sağlanan bir VM üzerinde etkinleştirmek için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Oturum açmak için Azure kullanarak `Connect-AzAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```powershell
   Connect-AzAccount
   ```

2. İlk VM özelliklerini kullanarak alınması `Get-AzVM` cmdlet'i. Sistem tarafından atanan bir yönetilen kimlik etkinleştirmek için ardından kullanmak `-AssignIdentity` açın [güncelleştirme-AzVM](/powershell/module/az.compute/update-azvm) cmdlet:

   ```powershell
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```

> [!NOTE]
> İsteğe bağlı olarak, VM uzantısını Azure kaynakları için yönetilen kimlik sağlama, ancak yakında kullanımdan kaldırılacak. Azure örnek meta veri kimlik uç nokta kimlik doğrulaması için kullanmanızı öneririz. Daha fazla bilgi için [Azure IMDS uç nokta kimlik doğrulaması için VM uzantısı'ten geçiş](howto-migrate-vm-extension.md).

### <a name="add-vm-system-assigned-identity-to-a-group"></a>VM sistem tarafından atanan kimliği gruba ekleme

Sistem tarafından atanan kimlik bir VM'de etkinleştirdikten sonra gruba ekleyebilirsiniz.  Aşağıdaki yordamda, bir sanal makinenin sistem tarafından atanan kimlik bir gruba ekler.

1. Oturum açmak için Azure kullanarak `Connect-AzAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```powershell
   Connect-AzAccount
   ```

2. Alın ve Not `ObjectID` (belirtilmiş `Id` döndürülen değerleri alan) sanal makinenin hizmet sorumlusunun:

   ```powerhshell
   Get-AzADServicePrincipal -displayname "myVM"
   ```

3. Alın ve Not `ObjectID` (belirtilmiş `Id` döndürülen değerleri alan) grubunun:

   ```powershell
   Get-AzADGroup -searchstring "myGroup"
   ```

4. Sanal makinenin hizmet sorumlusu gruba ekleyin:

   ```powershell
   Add-AzureADGroupMember -ObjectId "<objectID of group>" -RefObjectId "<object id of VM service principal>"
   ```

## <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Yönetilen kimlik sistem tarafından atanan bir Azure VM'den devre dışı bırak

Yönetilen kimlik sistem tarafından atanan bir VM'de devre dışı bırakmak için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.  Hiçbir ek Azure AD dizini rol atamaları gereklidir.

Artık sistem tarafından atanan bir yönetilen kimlik gerekiyor ancak yine de kullanıcı tarafından atanan yönetilen kimlikleri gereken bir sanal makine varsa, aşağıdaki cmdlet'i kullanın:

1. Oturum açmak için Azure kullanarak `Connect-AzAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```powershell
   Connect-AzAccount
   ```

2. Kullanarak VM özelliklerini almak `Get-AzVM` cmdlet'i ve `-IdentityType` parametresi `UserAssigned`:

   ```powershell   
   $vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 
   Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType "UserAssigned"
   ```

Artık yönetilen kimlik sistem tarafından atanan gereken bir sanal makineye sahip ve kullanıcı tarafından atanan yönetilen kimlik varsa, aşağıdaki komutları kullanın:

```powershell
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```

> [!NOTE]
> (Kullanım dışı için) VM uzantısını Azure kaynakları için yönetilen kimliği sağladıysa kullanarak kaldırmanız gereken [Remove-AzVMExtension](/powershell/module/az.compute/remove-azvmextension). Daha fazla bilgi için [Azure IMDS kimlik doğrulaması için VM uzantısı'ten geçiş](howto-migrate-vm-extension.md).

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, bir kullanıcı tarafından atanan bir yönetilen kimlik Azure PowerShell kullanarak bir sanal makineden ekleyip öğrenin.

### <a name="assign-a-user-assigned-managed-identity-to-a-vm-during-creation"></a>Kullanıcı tarafından atanan bir yönetilen kimlik oluşturma sırasında bir VM'ye atayın

Bir VM için bir kullanıcı tarafından atanan kimliği atamak için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamaları. Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Aşağıdaki Azure VM yalnızca gerekli bölümlerini ("" Oluşturma kaynak grubu","Ağ Grup Oluştur","oluşturma"VM Azure'da oturum",) tamamlama hızlı Başlangıçlar, birine bakın. 
  
    "VM oluşturma" bölümüne aldığınızda, küçük bir değişiklik yapmak [ `New-AzVMConfig` ](/powershell/module/az.compute/new-azvm) cmdlet sözdizimi. Ekleme `-IdentityType UserAssigned` ve `-IdentityID ` kullanıcı tarafından atanan bir kimlikle VM sağlamak için parametreleri.  Değiştirin `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, ve `<USER ASSIGNED IDENTITY NAME>` kendi değerlerinizle.  Örneğin:
    
    ```powershell 
    $vmConfig = New-AzVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>..."
    ```
    
    - [PowerShell'i kullanarak bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md)
    - [PowerShell kullanarak bir Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md)

> [!NOTE]
> İsteğe bağlı olarak, VM uzantısını Azure kaynakları için yönetilen kimlik sağlama, ancak yakında kullanımdan kaldırılacak. Azure örnek meta veri kimlik uç nokta kimlik doğrulaması için kullanmanızı öneririz. Daha fazla bilgi için [Azure IMDS uç nokta kimlik doğrulaması için VM uzantısı'ten geçiş](howto-migrate-vm-extension.md).

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Kullanıcı tarafından atanan bir yönetilen kimlik mevcut bir Azure VM'ye atayın

Bir VM için bir kullanıcı tarafından atanan kimliği atamak için hesabınızın gerekir [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamaları. Hiçbir ek Azure AD dizini rol atamaları gereklidir.

1. Oturum açmak için Azure kullanarak `Connect-AzAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın.

   ```powershell
   Connect-AzAccount
   ```

2. Bir kullanıcı tarafından atanan yönetilen kimlik kullanarak oluşturduğunuz [yeni AzUserAssignedIdentity](/powershell/module/az.managedserviceidentity/new-azuserassignedidentity) cmdlet'i.  Not `Id` çıktıda çünkü bu bir sonraki adımda gerekecektir.

   > [!IMPORTANT]
   > Kullanıcı tarafından atanan yönetilen kimlikleri yalnızca destekler alfasayısal oluşturma, alt çizgi ve tire (0-9 veya a-z veya A-Z, \_ veya -) karakter. Ayrıca, ad 3 ila 128 karakter uzunluğu atama düzgün çalışması için VM/VMSS için sınırlı olmalıdır. Daha fazla bilgi için [SSS ve bilinen sorunlar](known-issues.md)

   ```powershell
   New-AzUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
   ```
3. Kullanarak VM özelliklerini almak `Get-AzVM` cmdlet'i. Ardından, kullanıcı tarafından atanan bir yönetilen kimlik Azure VM'sine atanacak kullanın `-IdentityType` ve `-IdentityID` açın [güncelleştirme-AzVM](/powershell/module/az.compute/update-azvm) cmdlet'i.  Değeri`-IdentityId` parametresi `Id` , önceki adımda not ettiğiniz.  Değiştirin `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, ve `<USER ASSIGNED IDENTITY NAME>` kendi değerlerinizle.

   > [!WARNING]
   > VM'ye atanmış tüm daha önce kullanıcı tarafından atanan yönetilen kimlikleri korumak için sorgu `Identity` VM nesnesinin özelliği (örneğin, `$vm.Identity`).  Herhangi bir kullanıcı atanmış yönetilen kimlikleri döndürülür, sanal Makineye atamak istediğiniz yeni kullanıcı tarafından atanan yönetilen kimliği ile birlikte aşağıdaki komutu ekleyin.

   ```powershell
   $vm = Get-AzVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

> [!NOTE]
> İsteğe bağlı olarak, VM uzantısını Azure kaynakları için yönetilen kimlik sağlama, ancak yakında kullanımdan kaldırılacak. Azure örnek meta veri kimlik uç nokta kimlik doğrulaması için kullanmanızı öneririz. Daha fazla bilgi için [Azure IMDS uç nokta kimlik doğrulaması için VM uzantısı'ten geçiş](howto-migrate-vm-extension.md).

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Kullanıcı tarafından atanan bir yönetilen kimlik bir Azure VM'den kaldırın.

Bir VM için bir kullanıcı tarafından atanan kimliği kaldırmak için hesabınızın gerekli [sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rol ataması.

Sanal makinenize birden çok kullanıcı tarafından atanan yönetilen kimlik varsa, aşağıdaki komutları kullanarak tüm sonuncu kaldırabilirsiniz. `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<USER ASSIGNED IDENTITY NAME>` VM üzerinde kalmalıdır kullanıcı tarafından atanan yönetilen kimliğin adı özelliği. Bu bilgiler sorgulanarak bulunabilir `Identity` VM nesnesinin özelliği.  Örneğin, `$vm.Identity`:

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType UserAssigned -IdentityID <USER ASSIGNED IDENTITY NAME>
```
Sanal makinenize bir sistem tarafından atanan yönetilen yoksa kimlik ve istediğiniz kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırın, aşağıdaki komutu kullanın:

```powershell
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```
Sanal makinenizin sistem tarafından atanan hem de kullanıcı tarafından atanan yönetilen kimlikleri varsa, tüm kullanıcı tarafından atanan yönetilen kimlikleri yalnızca sistem tarafından atanan yönetilen kimlikleri kullanmak için geçerek kaldırabilirsiniz.

```powershell 
$vm = Get-AzVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlik](overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç kılavuzları, bkz:
  
  - [PowerShell ile bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md) 
