---
title: PowerShell kullanarak bir Azure VMSS üzerinde MSI yapılandırma
description: Adım adım sistem ve kullanıcı yapılandırmaya yönelik yönergeler, PowerShell kullanarak bir Azure vmss'de kimlikleri atanır.
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
ms.openlocfilehash: d5071a55c49a0749d91ec9617558ced76ebb007e
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188106"
---
# <a name="configure-a-vmss-managed-service-identity-msi-using-powershell"></a>Bir VMSS yönetilen hizmet kimliği (PowerShell kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, üzerinde bir sanal makine ölçek kümesi (PowerShell kullanarak VMSS), yönetilen hizmet kimliği işlemlerini nasıl gerçekleştireceğinizi öğrenin:
- Enable ve disable sistem tarafından atanan bir Azure VMSS kimliği
- Bir kullanıcı tarafından atanan kimliği bir Azure vmss'de ekleyip

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) bir sanal makine ölçek kümesi oluşturun ve etkinleştirin ve sistem tarafından yönetilen kimlik atanan bir sanal makine ölçek kümesinden kaldırmak için.
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolünün bir kullanıcı tarafından atanan kimliği oluşturma.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü atamak ve bir kullanıcı tarafından atanan kimliği ve sanal makine ölçek kümesine kaldırmak için.
- Yükleme [Azure PowerShell'in en son sürümünü](https://www.powershellgallery.com/packages/AzureRM) henüz yapmadıysanız. 

## <a name="system-assigned-managed-identity"></a>Yönetilen kimlik sistemi atanan

Bu bölümde, etkinleştirmek ve Azure PowerShell kullanarak bir sistem tarafından atanan kimliği kaldırma konusunda bilgi edinin.

### <a name="enable-system-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Azure VMSS oluşturma sırasında atanan kimliği Sistemi'ni etkinleştir

Etkin sistem tarafından atanan kimlikle bir VMSS oluşturmak için:

1. Başvurmak *örnek 1* içinde [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) bir sistem tarafından atanan kimliği ile bir VMSS oluşturma için cmdlet başvurusu makalesinde.  Parametre Ekle `-IdentityType SystemAssigned` için `New-AzureRmVmssConfig` cmdlet:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (İsteğe bağlı) MSI VMSS uzantısını kullanarak Ekle `-Name` ve `-Type` parametresi [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM, türüne bağlı olarak geçirin ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

    > [!NOTE]
    > Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-identity-on-an-existing-azure-vmss"></a>Mevcut bir Azure VMSS kimliği atanan Sistemi'ni etkinleştir

Mevcut bir Azure VMSS üzerinde bir sistem tarafından atanan kimliği etkinleştirmeniz gerekirse:

1. Oturum açmak için Azure kullanarak `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Login-AzureRmAccount
   ```

2. İlk kullanarak VMSS özelliklerini alınması [ `Get-AzureRmVmss` ](/powershell/module/azurerm.compute/get-azurermvmss) cmdlet'i. Bir sistem tarafından atanan kimliği etkinleştirmek için ardından kullanmak `-IdentityType` açın [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVM -IdentityType "SystemAssigned"
   ```

3. MSI VMSS uzantısını kullanarak Ekle `-Name` ve `-Type` parametresi [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM, türüne bağlı olarak geçirin ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vmss"></a>Sistem tarafından bir Azure VMSS kimlik atanan devre dışı bırak

> [!NOTE]
> Bir sanal makine ölçek kümesi'nden yönetilen hizmet kimliği devre dışı bırakma şu anda desteklenmiyor. Bu arada, sistem atanan ve atanan kullanıcı kimliklerini kullanma arasında geçiş yapabilirsiniz.

Bir sanal makine ölçek artık sistem tarafından atanan kimlik gerekiyor, ancak yine de kullanıcı tarafından atanan kimliklerle gereken kümesi varsa, aşağıdaki cmdlet'i kullanın:

1. Oturum açmak için Azure kullanarak `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

2. Aşağıdaki cmdlet'i çalıştırın:

    ```powershell
    Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
    ```

## <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği

Bu bölümde, bir kullanıcı tarafından atanan kimliği Azure PowerShell kullanarak bir VMSS ekleyip öğrenin.

### <a name="assign-a-user-assigned-identity-during-creation-of-an-azure-vmss"></a>Bir kullanıcı tarafından atanan kimliği oluşturma sırasında Azure VMSS atayın

Bir kullanıcı tarafından atanan kimliği ile yeni bir VMSS oluşturma, şu anda PowerShell desteklenmiyor. Bir kullanıcı tarafından atanan kimliği için mevcut bir VMSS eklemeye yönelik sonraki bölüme bakın. Güncelleştirmeler için sonra yeniden denetleyin.

### <a name="assign-a-user-identity-to-an-existing-azure-vmss"></a>Bir kullanıcı kimliği için mevcut bir Azure VMSS atayın

Bir kullanıcı tarafından atanan kimliği için mevcut bir Azure VMSS atamak için:

1. Oturum açmak için Azure kullanarak `Connect-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Connect-AzureRmAccount
   ```

2. İlk VM özelliklerini kullanarak alınması `Get-AzureRmVM` cmdlet'i. Azure VMSS için bir kullanıcı tarafından atanan kimliği atamak için ardından kullanmak `-IdentityType` ve `-IdentityID` açın [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet'i. Değiştirin `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` kendi değerlerinizle.

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


   ```powershell
   $vmss = Get-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME>
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -VM $vmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vmss"></a>Bir kullanıcı tarafından atanan kimliği bir Azure VMSS kaldırın

> [!NOTE]
> Bir sistem tarafından atanan kimlik olmadığı sürece tüm kullanıcı tarafından atanan kimlikleri bir sanal makine ölçek kümesinden kaldırılması şu anda, desteklenmiyor. Güncelleştirmeler için sonra yeniden denetleyin.

Birden çok kullanıcı tarafından atanan kimlikleri, VMSS varsa, aşağıdaki komutları kullanarak tüm sonuncu kaldırabilirsiniz. `<RESOURCE GROUP>` ve `<VMSS NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<MSI NAME>` VMSS üzerinde kalmalıdır atanan kullanıcı kimliğin adı özelliği. Bu bilgileri tarafından VMSS kullanarak kimlik bölümünde bulunabilir `az vmss show`:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = "<MSI NAME>"
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachineScaleSet $vmss
```

VMSS atanan sistem ve kullanıcı tarafından atanan kimliklerle varsa, yalnızca atanan sistemi kullanmaya geçiş tarafından atanan kimliklerle tüm kullanıcı kaldırabilirsiniz. Aşağıdaki komutu kullanın:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = $null
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachine $vmss -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>İlgili içerik

- [Yönetilen hizmet Kimliği'ne genel bakış](overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç kılavuzları, bkz:
  
  - [PowerShell ile bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md) 

















