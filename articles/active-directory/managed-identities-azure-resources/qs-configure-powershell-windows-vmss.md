---
title: PowerShell kullanarak Azure kaynakları için yönetilen kimlikleri yapılandırma üzerinde bir sanal makine ölçek kümesi
description: PowerShell kullanarak bir sanal makine ölçek üzerinde bir sistem ve kullanıcı tarafından atanan yönetilen kimlikleri yapılandırma yönergeleri adım adım ayarlayın.
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
ms.openlocfilehash: c34e858f3dc798310778877ce282fbed2788c7ea
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028497"
---
# <a name="configure-managed-identities-for-azure-resources-on-virtual-machine-scale-sets-using-powershell"></a>PowerShell kullanarak sanal makine ölçek kümelerinde Azure kaynakları için yönetilen kimlik Yapılandır

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, PowerShell kullanarak, Azure kaynaklarını bir sanal makine ölçek kümesi işlemleri için yönetilen kimlikleri gerçekleştirmeyi öğreneceksiniz:
- Etkinleştirme ve devre dışı bir sanal makine ölçek kümesi üzerinde sistem tarafından atanan yönetilen kimlik
- Bir kullanıcı tarafından atanan yönetilen kimlik bir sanal makine ölçek kümesinde ekleyip

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan arasındaki farkı ve yönetilen kullanıcı kimliği atanır](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) kümesi bir sanal makine ölçek oluşturmak için ve yönetilen etkinleştir ve sistem tarafından atanan Kaldır ve/veya bir sanal makine ölçek kümesinden yönetilen kimliği kullanıcı tarafından atanan.
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolüne bir kullanıcı tarafından atanan oluşturmak için yönetilen kimliği.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) gelen ve sanal makine ölçek kümesi yönetilen kimliği atamak ve bir kullanıcı tarafından atanan kaldırmak için rol.
- Yükleme [Azure PowerShell'in en son sürümünü](https://www.powershellgallery.com/packages/AzureRM) henüz yapmadıysanız. 

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, etkinleştirmek ve Azure PowerShell kullanarak bir sistem tarafından atanan yönetilen kimlik kaldırma konusunda bilgi edinin.

### <a name="enable-system-assigned-managed-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi oluşturma sırasında sistem tarafından atanan yönetilen kimlik etkinleştir

Etkin sistem tarafından atanan yönetilen kimlikle bir VMSS oluşturmak için:

1. Başvurmak *örnek 1* içinde [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) sistem tarafından atanan bir yönetilen kimlikle bir VMSS oluşturma için cmdlet başvurusu makalesinde.  Parametre Ekle `-IdentityType SystemAssigned` için `New-AzureRmVmssConfig` cmdlet:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (İsteğe bağlı) Uzantısını kullanarak Azure kaynaklarınızı sanal makine ölçek kümesi yönetilen kimlikleri ekleyin `-Name` ve `-Type` parametresi [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet'i. "ManagedIdentityExtensionForWindows" geçirebilir veya "ManagedIdentityExtensionForLinux", türüne bağlı olarak sanal makine ölçek kümesi ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

    > [!NOTE]
    > Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-managed-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Sistem tarafından atanan kimliği mevcut bir Azure sanal makine ölçek kümesi üzerinde yönetilen etkinleştir

Sistem tarafından atanan yönetilen bir kimlik var olan bir Azure sanal makine ölçek kümesinde etkinleştirmeniz gerekirse:

1. Oturum açmak için Azure kullanarak `Login-AzureRmAccount`. Sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi sanal makine ölçek kümesi üzerinde yazma izinleri:

   ```powershell
   Login-AzureRmAccount
   ```

2. Sanal makine ölçek kümesi özelliklerini kullanarak ilk alma [ `Get-AzureRmVmss` ](/powershell/module/azurerm.compute/get-azurermvmss) cmdlet'i. Sistem tarafından atanan bir yönetilen kimlik etkinleştirmek için ardından kullanmak `-IdentityType` açın [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) cmdlet:

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```

3. VMSS uzantısıyla Azure kaynakları için yönetilen kimlikleri ekleyin `-Name` ve `-Type` parametresi [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet'i. "ManagedIdentityExtensionForWindows" geçirebilir veya "ManagedIdentityExtensionForLinux", türüne bağlı olarak sanal makine ölçek kümesi ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden sistem tarafından atanan yönetilen kimliği devre dışı

Artık sistem tarafından atanan bir yönetilen kimlik gerekiyor ancak yine de kullanıcı tarafından atanan yönetilen kimlikleri gereken bir sanal makine ölçek kümesi varsa, aşağıdaki cmdlet'i kullanın:

1. Oturum açmak için Azure kullanarak `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi sanal makine ölçek kümesi üzerinde yazma izinleri:

2. Aşağıdaki cmdlet'i çalıştırın:

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

Artık yönetilen kimlik sistem tarafından atanan gereken bir sanal makine ölçek kümesi vardır ve hiçbir kullanıcı tarafından atanan bir yönetilen kimlik varsa, aşağıdaki komutları kullanın:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, bir kullanıcı tarafından atanan bir yönetilen kimlik Azure PowerShell kullanarak bir sanal makine ölçek kümesinden ekleyip öğrenin.

### <a name="assign-a-user-assigned-managed-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi oluşturma sırasında kullanıcı tarafından atanan bir yönetilen kimlik atama

Yeni bir sanal makine ölçek kümesi bir kullanıcı tarafından atanan yönetilen kimliği oluşturma, şu anda PowerShell desteklenmiyor. Mevcut bir sanal makine ölçek kümesi için bir kullanıcı tarafından atanan bir yönetilen kimlik eklemek sonraki bölüme bakın. Güncelleştirmeler için sonra yeniden denetleyin.

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan bir yönetilen kimlik mevcut bir Azure sanal makine ölçek kümesine atama

Mevcut bir Azure sanal makine ölçek kümesi için bir kullanıcı tarafından atanan bir yönetilen kimlik atamak için:

1. Oturum açmak için Azure kullanarak `Connect-AzureRmAccount`. Sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi sanal makine ölçek kümesi üzerinde yazma izinleri:

   ```powershell
   Connect-AzureRmAccount
   ```

2. Sanal makine ölçek kümesi özelliklerini kullanarak ilk alma `Get-AzureRmVM` cmdlet'i. Sanal makine ölçek kümesi için bir kullanıcı tarafından atanan bir yönetilen kimlik atamak için ardından kullanmak `-IdentityType` ve `-IdentityID` açın [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) cmdlet'i. Değiştirin `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` kendi değerlerinizle.

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden bir kullanıcı tarafından atanan bir yönetilen kimlik Kaldır

Kullanıcı tarafından atanan birden çok yönetilen kimlik sanal makine ölçek kümeniz varsa, aşağıdaki komutları kullanarak tüm sonuncu kaldırabilirsiniz. `<RESOURCE GROUP>` ve `<VMSS NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<USER ASSIGNED IDENTITY NAME>` Sanal makine ölçek kümesi üzerinde kalmalıdır kullanıcı tarafından atanan yönetilen kimliğin adı özelliği. Bu bilgileri kullanarak sanal makine ölçek kümesi'nin kimlik bölümünde bulunabilir `az vmss show`:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED IDENTITY NAME>"
```
Bir sistem tarafından atanan yönetilen sanal makine ölçek kümeniz yoksa, kimlik ve istediğiniz kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırın, aşağıdaki komutu kullanın:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
Sanal makine ölçek kümesi, hem de sistem tarafından atanan ve yönetilen kimlikleri, kullanıcı tarafından atanan tüm kullanıcı tarafından atanan yönetilen kimlikleri yalnızca sistem tarafından atanan yönetilen kimliği kullanma geçerek kaldırabilirsiniz.

```powershell 
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlik](overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç kılavuzları, bkz:
  
  - [PowerShell ile bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md) 

















