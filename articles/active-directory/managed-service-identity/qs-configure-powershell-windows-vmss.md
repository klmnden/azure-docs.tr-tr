---
title: Bir Azure PowerShell kullanarak VMSS üzerinde MSI yapılandırma
description: Adım adım sistemi ve kullanıcı yapılandırmaya ilişkin yönergeler, PowerShell kullanarak bir Azure VMSS kimliği atanır.
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
ms.date: 11/27/2017
ms.author: daveba
ms.openlocfilehash: 2b3651eaf702cfe2f73320fcaf2ab469dd7c478a
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="configure-a-vmss-managed-service-identity-msi-using-powershell"></a>Bir VMSS yönetilen hizmet kimliği (PowerShell kullanarak MSI) yapılandırma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Yönetilen hizmet kimliği Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgileri, kodunuzda gerek kalmadan destekleyen herhangi bir hizmeti için kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, aşağıdaki yönetilen hizmet kimliği üzerinde bir Azure sanal makine ölçek kümesi (PowerShell kullanarak VMSS), işlemleri öğrenin:
- Etkinleştirme ve bir Azure VMSS kimliğini atanan sistem devre dışı
- Ekleme ve bir Azure VMSS kimliğini atanmış bir kullanıcı kaldırma

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile emin değilseniz, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [atanan sistemi ve kullanıcı kimliği atanır arasındaki farkı](overview.md#how-does-it-work)**.
- Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com/free/) devam etmeden önce.
- Yükleme [Azure PowerShell'in en son sürümünü](https://www.powershellgallery.com/packages/AzureRM) yapmadıysanız. 

## <a name="system-assigned-managed-identity"></a>Sistem yönetilen kimliği atanır

Bu bölümde, etkinleştirme ve Azure PowerShell kullanarak bir sistem atanan kimliğini kaldırma öğrenin.

### <a name="enable-system-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Bir Azure VMSS oluşturma sırasında kimliği atanır Sistemi'ni etkinleştir

Bir VMSS sistemiyle oluşturmak için etkin kimliği atanır:

1. Başvurmak *örnek 1* içinde [yeni AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) atanan sistem kimliği ile bir VMSS oluşturmak için cmdlet başvurusu makalesinde.  Parametre ekleme `-IdentityType SystemAssigned` için `New-AzureRmVmssConfig` cmdlet:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (İsteğe bağlı) MSI VMSS uzantısını kullanarak Ekle `-Name` ve `-Type` parametresini [Ekle AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM türüne bağlı olarak geçirmek ve kullanarak ad `-Name` parametresi. `-Settings` Parametresi için belirteç edinme OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

    > [!NOTE]
    > Azure örneği meta veri hizmeti (IMDS) kimlik endpoint de belirteçleri almak için kullanabileceğiniz gibi bu adım isteğe bağlıdır.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-identity-on-an-existing-azure-vmss"></a>Var olan bir Azure VMSS kimliğini atanan Sistemi'ni etkinleştir

Atanan sistem kimliği mevcut bir Azure VMSS üzerinde etkinleştirmeniz gerekirse:

1. Oturum Azure kullanmaya `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Login-AzureRmAccount
   ```

2. İlk kullanarak VMSS özelliklerini almak [ `Get-AzureRmVmss` ](/powershell/module/azurerm.compute/get-azurermvmss) cmdlet'i. Bir sistem atanan kimlik etkinleştirmek için kullanın `-IdentityType` anahtarının [güncelleştirme-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVM -IdentityType "SystemAssigned"
   ```

3. MSI VMSS uzantısını kullanarak Ekle `-Name` ve `-Type` parametresini [Ekle AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet'i. "ManagedIdentityExtensionForWindows" veya "ManagedIdentityExtensionForLinux" VM türüne bağlı olarak geçirmek ve kullanarak ad `-Name` parametresi. `-Settings` Parametresi için belirteç edinme OAuth belirteç uç noktası tarafından kullanılan bağlantı noktasını belirtir:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vmss"></a>Bir Azure VMSS kimliği atanır sisteminizi devre dışı

> [!NOTE]
> Yönetilen hizmet kimliğinden bir sanal makine ölçek kümesi devre dışı bırakma şu anda desteklenmiyor. Bu arada, sistem atanan ve kullanıcı atanan kimlikler kullanma arasında geçiş yapabilirsiniz.

Bir sanal makine ölçek artık kimliği atanır sistem gerekiyor ancak hala kimlikleri atanan kullanıcı gerekiyor kümesi varsa, aşağıdaki cmdlet'i kullanın:

1. Oturum Azure kullanmaya `Login-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

2. Aşağıdaki cmdlet'i çalıştırın:

    ```powershell
    Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
    ```

## <a name="user-assigned-identity"></a>Kullanıcı kimliği atanır

Bu bölümde, Azure PowerShell kullanarak bir VMSS kimlik atanmış bir kullanıcı ekleyip öğrenin.

### <a name="assign-a-user-assigned-identity-during-creation-of-an-azure-vmss"></a>Bir Azure VMSS oluşturma sırasında kimlik atanan kullanıcı atama

Yeni bir VMSS kimlik atanmış bir kullanıcıyla oluşturma PowerShell yoluyla şu anda desteklenmiyor. Atanan kullanıcı kimliği için var olan bir VMSS ekleme sonraki bölüme bakın. Geri güncelleştirmeleri denetleyin.

### <a name="assign-a-user-identity-to-an-existing-azure-vmss"></a>Bir kullanıcı kimliği mevcut bir Azure VMSS atayın

Bir kullanıcı atamak için var olan bir Azure VMSS kimliği atanır:

1. Oturum Azure kullanmaya `Connect-AzureRmAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınızın sağlayan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi VM üzerinde yazma izinleri:

   ```powershell
   Connect-AzureRmAccount
   ```

2. İlk kullanarak VM özelliklerini almak `Get-AzureRmVM` cmdlet'i. Atanan kullanıcı kimliğini Azure VMSS atamak için kullan `-IdentityType` ve `-IdentityID` anahtarının [güncelleştirme-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet'i. Değiştir `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` kendi değerlere sahip.

   > [!IMPORTANT]
   > Kullanıcı adında özel karakterler (örneğin, alt çizgi) kimliklerle atanan oluşturma şu anda desteklenmiyor. Lütfen alfasayısal karakterler kullanın. Geri güncelleştirmeleri denetleyin.  Daha fazla bilgi için bkz: [SSS ve bilinen sorunlar](known-issues.md)

   ```powershell
   $vmss = Get-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME>
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -VM $vmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vmss"></a>Bir Azure VMSS kimliği atanır kullanıcıyı kaldırma

> [!NOTE]
> Kimlik atanmış bir sistem olmadığı sürece tüm atanan kullanıcı kimlikleri bir sanal makine ölçek kümesi'den kaldırma şu anda, desteklenmiyor. Geri güncelleştirmeleri denetleyin.

Birden çok kullanıcı tarafından atanan kimlik bilgilerinizi VMSS varsa, aşağıdaki komutları kullanarak tüm sonuncu kaldırabilirsiniz. Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VMSS NAME>` parametre değerlerini kendi değerlere sahip. `<MSI NAME>` Üzerinde VMSS kalacağı kullanıcıya atanan kimliğin adı özelliği. Bu bilgiler tarafından VMSS kullanarak kimlik bölümünde bulunabilir `az vmss show`:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = "<MSI NAME>"
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachineScaleSet $vmss
```

VMSS atanan sistem ve kimlikler atanan kullanıcı varsa, sadece atanan sistemini kullanacak şekilde geçerek kimlikleri atanan tüm kullanıcı kaldırabilirsiniz. Aşağıdaki komutu kullanın:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = $null
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachine $vmss -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>İlgili içerik

- [Yönetilen hizmet Kimliği'ne genel bakış](overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç ipuçları, bakın:
  
  - [PowerShell ile Windows sanal makine oluşturma](../../virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makine oluşturma](../../virtual-machines/linux/quick-create-powershell.md) 

















