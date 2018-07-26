---
title: PowerShell kullanarak bir Azure VMSS üzerinde yönetilen hizmet kimliği yapılandırma
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
ms.openlocfilehash: 5d4539c05d05053ac2ea6cd1c5fadbd161b41173
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257751"
---
# <a name="configure-a-vmss-managed-service-identity-using-powershell"></a>PowerShell kullanarak VMSS yönetilen hizmet kimliği yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, PowerShell kullanarak bir Azure sanal makine ölçek kümesinde yönetilen hizmet kimliği işlemlerini nasıl gerçekleştireceğinizi öğrenin:
- Etkinleştirme ve devre dışı bir sanal makine ölçek kümesi üzerinde sistem tarafından atanan kimlik
- Bir sanal makine ölçek kümesi üzerinde bir kullanıcı tarafından atanan kimliği ekleyip

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) yönetilen bir sanal makine ölçek kümesi oluşturun ve etkinleştirin ve sistem tarafından atanan kaldırmak için ve/veya kullanıcı kimlik bir sanal makine ölçek kümesi.
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolünün bir kullanıcı tarafından atanan kimliği oluşturma.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü atamak ve bir kullanıcı tarafından atanan kimliği ve sanal makine ölçek kümesine kaldırmak için.
- Yükleme [Azure PowerShell'in en son sürümünü](https://www.powershellgallery.com/packages/AzureRM) henüz yapmadıysanız. 

## <a name="system-assigned-managed-identity"></a>Yönetilen kimlik sistemi atanan

Bu bölümde, etkinleştirmek ve Azure PowerShell kullanarak bir sistem tarafından atanan kimliği kaldırma konusunda bilgi edinin.

### <a name="enable-system-assigned-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi oluşturma sırasında sistem tarafından atanan kimlik etkinleştir

Etkin sistem tarafından atanan kimlikle bir VMSS oluşturmak için:

1. Başvurmak *örnek 1* içinde [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) bir sistem tarafından atanan kimliği ile bir VMSS oluşturma için cmdlet başvurusu makalesinde.  Parametre Ekle `-IdentityType SystemAssigned` için `New-AzureRmVmssConfig` cmdlet:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (İsteğe bağlı) Yönetilen hizmet kimliği VMSS uzantısını kullanarak Ekle `-Name` ve `-Type` parametresi [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet'i. "ManagedIdentityExtensionForWindows" geçirebilir veya "ManagedIdentityExtensionForLinux", türüne bağlı olarak sanal makine ölçek kümesi ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

    > [!NOTE]
    > Azure örnek meta veri hizmeti (IMDS) kimlik endpoint de belirteçlerini almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Sistem tarafından atanan kimliği mevcut bir Azure sanal makine ölçek kümesinde etkinleştirin

Mevcut bir Azure sanal makine ölçek kümesi üzerinde bir sistem tarafından atanan kimliği etkinleştirmeniz gerekirse:

1. Oturum açmak için Azure kullanarak `Login-AzureRmAccount`. Sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi sanal makine ölçek kümesi üzerinde yazma izinleri:

   ```powershell
   Login-AzureRmAccount
   ```

2. Sanal makine ölçek kümesi özelliklerini kullanarak ilk alma [ `Get-AzureRmVmss` ](/powershell/module/azurerm.compute/get-azurermvmss) cmdlet'i. Bir sistem tarafından atanan kimliği etkinleştirmek için ardından kullanmak `-IdentityType` açın [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) cmdlet:

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```

3. Yönetilen hizmet kimliği VMSS uzantısını kullanarak Ekle `-Name` ve `-Type` parametresi [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet'i. "ManagedIdentityExtensionForWindows" geçirebilir veya "ManagedIdentityExtensionForLinux", türüne bağlı olarak sanal makine ölçek kümesi ve kullanarak adlandırın `-Name` parametresi. `-Settings` Parametresi, belirteç edinme için OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden sistem tarafından atanan kimliği devre dışı

Artık sistem tarafından atanan kimlik gerekiyor ancak yine de kullanıcı tarafından atanan kimliklerle gereken bir sanal makine ölçek kümesi varsa, aşağıdaki cmdlet'i kullanın:

1. Oturum açmak için Azure kullanarak `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi sanal makine ölçek kümesi üzerinde yazma izinleri:

2. Aşağıdaki cmdlet'i çalıştırın:

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

Artık sistem tarafından atanan kimlik gereken bir sanal makine ölçek kümesi vardır ve hiçbir kullanıcı tarafından atanan kimliklerle varsa, aşağıdaki komutları kullanın:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği

Bu bölümde, bir kullanıcı tarafından atanan kimliği Azure PowerShell kullanarak bir sanal makine ölçek kümesinden ekleyip öğrenin.

### <a name="assign-a-user-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi oluşturma sırasında bir kullanıcıya atanan kimlik atama

Yeni bir sanal makine ölçek kümesi bir kullanıcı tarafından atanan kimliği ile oluşturma, şu anda PowerShell desteklenmiyor. Bir kullanıcı tarafından atanan kimliği için mevcut bir sanal makine ölçek kümesi eklemek sonraki bölüme bakın. Güncelleştirmeler için sonra yeniden denetleyin.

### <a name="assign-a-user-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Bir kullanıcı kimliği için mevcut bir Azure sanal makine ölçek kümesine atama

Mevcut bir Azure sanal makine ölçek kümesi için bir kullanıcı tarafından atanan kimliği atamak için:

1. Oturum açmak için Azure kullanarak `Connect-AzureRmAccount`. Sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi sanal makine ölçek kümesi üzerinde yazma izinleri:

   ```powershell
   Connect-AzureRmAccount
   ```

2. Sanal makine ölçek kümesi özelliklerini kullanarak ilk alma `Get-AzureRmVM` cmdlet'i. Sanal makine ölçek kümesi için bir kullanıcı tarafından atanan kimliği atamak için ardından kullanmak `-IdentityType` ve `-IdentityID` açın [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) cmdlet'i. Değiştirin `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` kendi değerlerinizle.

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden yönetilen kimlik atanmış bir kullanıcıyı kaldırma

Sanal makine ölçek kümeniz birden çok kullanıcı tarafından atanan kimliği varsa, aşağıdaki komutları kullanarak tüm sonuncu kaldırabilirsiniz. `<RESOURCE GROUP>` ve `<VMSS NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<MSI NAME>` Sanal makine ölçek kümesi üzerinde kalmalıdır atanan kullanıcı kimliğin adı özelliği. Bu bilgileri kullanarak sanal makine ölçek kümesi'nin kimlik bölümünde bulunabilir `az vmss show`:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<MSI NAME>"
```
Sanal makine ölçek kümesi bir sistem tarafından atanan kimlik yok ve tüm kullanıcı kimlikleri atanmış kaldırmak istediğiniz, aşağıdaki komutu kullanın:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
Sanal makine ölçek kümenize atanan sistem ve kullanıcı tarafından atanan kimliklerle varsa, yalnızca atanan sistemi kullanmaya geçiş tarafından atanan kimliklerle tüm kullanıcı kaldırabilirsiniz.

```powershell 
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>İlgili içerik

- [Yönetilen hizmet Kimliği'ne genel bakış](overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç kılavuzları, bkz:
  
  - [PowerShell ile bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md) 

















