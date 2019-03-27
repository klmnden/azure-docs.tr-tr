---
title: PowerShell kullanarak Azure kaynakları için yönetilen kimlikleri yapılandırma üzerinde bir sanal makine ölçek kümesi
description: PowerShell kullanarak bir sanal makine ölçek üzerinde bir sistem ve kullanıcı tarafından atanan yönetilen kimlikleri yapılandırma yönergeleri adım adım ayarlayın.
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
ms.openlocfilehash: 4917720af2396b68ccd36cc0410c9acbbba2d9b2
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448594"
---
# <a name="configure-managed-identities-for-azure-resources-on-virtual-machine-scale-sets-using-powershell"></a>PowerShell kullanarak sanal makine ölçek kümelerinde Azure kaynakları için yönetilen kimlik Yapılandır

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'de otomatik olarak yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, Azure AD kimlik doğrulaması, kimlik bilgilerini kodunuzda zorunda kalmadan destekleyen herhangi bir hizmeti kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, PowerShell kullanarak, Azure kaynaklarını bir sanal makine ölçek kümesi işlemleri için yönetilen kimlikleri gerçekleştirmeyi öğreneceksiniz:
- Etkinleştirme ve devre dışı bir sanal makine ölçek kümesi üzerinde sistem tarafından atanan yönetilen kimlik
- Bir kullanıcı tarafından atanan yönetilen kimlik bir sanal makine ölçek kümesinde ekleyip

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan arasındaki farkı ve yönetilen kullanıcı kimliği atanır](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki Azure rol tabanlı erişim denetimi atamalarını hesabınızın gerekir:

    > [!NOTE]
    > Hiçbir ek Azure AD dizini rol atamaları gerekli.

    - [Sanal makine Katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) kümesi bir sanal makine ölçek oluşturmak için ve yönetilen etkinleştir ve sistem tarafından atanan Kaldır ve/veya bir sanal makine ölçek kümesinden yönetilen kimliği kullanıcı tarafından atanan.
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolüne bir kullanıcı tarafından atanan oluşturmak için yönetilen kimliği.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) gelen ve sanal makine ölçek kümesi yönetilen kimliği atamak ve bir kullanıcı tarafından atanan kaldırmak için rol.
- Yükleme [Azure PowerShell'in en son sürümünü](/powershell/azure/install-az-ps) henüz yapmadıysanız. 

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, etkinleştirmek ve Azure PowerShell kullanarak bir sistem tarafından atanan yönetilen kimlik kaldırma konusunda bilgi edinin.

### <a name="enable-system-assigned-managed-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi oluşturma sırasında sistem tarafından atanan yönetilen kimlik etkinleştir

Sanal makine ölçek kümesi etkin sistem tarafından atanan yönetilen kimlikle oluşturmak için:

1. Başvurmak *örnek 1* içinde [yeni AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) bir sanal makine ölçek oluşturmak için cmdlet başvurusu makalesinde, sistem tarafından atanan bir yönetilen kimlikle ayarlayın.  Parametre Ekle `-IdentityType SystemAssigned` için `New-AzVmssConfig` cmdlet:

    ```powershell
    $VMSS = New-AzVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```
> [!NOTE]
> İsteğe bağlı olarak, Azure kaynaklarını sanal makine ölçek kümesi uzantısını ancak yakında kullanımdan kaldırılacak yönetilen kimlikleri sağlamak. Azure örnek meta veri kimlik uç nokta kimlik doğrulaması için kullanmanızı öneririz. Daha fazla bilgi için [VM uzantısını kullanmayı bırakmak ve Azure IMDS uç nokta kimlik doğrulaması için kullanmaya başlama](howto-migrate-vm-extension.md).


## <a name="enable-system-assigned-managed-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Sistem tarafından atanan kimliği mevcut bir Azure sanal makine ölçek kümesi üzerinde yönetilen etkinleştir

Sistem tarafından atanan yönetilen bir kimlik var olan bir Azure sanal makine ölçek kümesinde etkinleştirmeniz gerekirse:

1. Oturum açmak için Azure kullanarak `Connect-AzAccount`. Sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi sanal makine ölçek kümesi üzerinde yazma izinleri:

   ```powershell
   Connect-AzAccount
   ```

2. Sanal makine ölçek kümesi özelliklerini kullanarak ilk alma [ `Get-AzVmss` ](/powershell/module/az.compute/get-azvmss) cmdlet'i. Sistem tarafından atanan bir yönetilen kimlik etkinleştirmek için ardından kullanmak `-IdentityType` açın [güncelleştirme AzVmss](/powershell/module/az.compute/update-azvmss) cmdlet:

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```

> [!NOTE]
> İsteğe bağlı olarak, Azure kaynaklarını sanal makine ölçek kümesi uzantısını ancak yakında kullanımdan kaldırılacak yönetilen kimlikleri sağlamak. Azure örnek meta veri kimlik uç nokta kimlik doğrulaması için kullanmanızı öneririz. Daha fazla bilgi için [Azure IMDS uç nokta kimlik doğrulaması için VM uzantısı'ten geçiş](howto-migrate-vm-extension.md).

### <a name="disable-the-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden sistem tarafından atanan yönetilen kimliği devre dışı

Artık sistem tarafından atanan bir yönetilen kimlik gerekiyor ancak yine de kullanıcı tarafından atanan yönetilen kimlikleri gereken bir sanal makine ölçek kümesi varsa, aşağıdaki cmdlet'i kullanın:

1. Oturum açmak için Azure kullanarak `Connect-AzAccount`. VM içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi sanal makine ölçek kümesi üzerinde yazma izinleri:

2. Aşağıdaki cmdlet'i çalıştırın:

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

Artık yönetilen kimlik sistem tarafından atanan gereken bir sanal makine ölçek kümesi vardır ve hiçbir kullanıcı tarafından atanan bir yönetilen kimlik varsa, aşağıdaki komutları kullanın:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, bir kullanıcı tarafından atanan bir yönetilen kimlik Azure PowerShell kullanarak bir sanal makine ölçek kümesinden ekleyip öğrenin.

### <a name="assign-a-user-assigned-managed-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesi oluşturma sırasında kullanıcı tarafından atanan bir yönetilen kimlik atama

Yeni bir sanal makine ölçek kümesi bir kullanıcı tarafından atanan yönetilen kimliği oluşturma, şu anda PowerShell desteklenmiyor. Mevcut bir sanal makine ölçek kümesi için bir kullanıcı tarafından atanan bir yönetilen kimlik eklemek sonraki bölüme bakın. Güncelleştirmeler için sonra yeniden denetleyin.

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan bir yönetilen kimlik mevcut bir Azure sanal makine ölçek kümesine atama

Mevcut bir Azure sanal makine ölçek kümesi için bir kullanıcı tarafından atanan bir yönetilen kimlik atamak için:

1. Oturum açmak için Azure kullanarak `Connect-AzAccount`. Sanal makine ölçek kümesi içeren Azure aboneliği ile ilişkili olan bir hesap kullanın. Ayrıca hesabınız sunan bir role ait olduğundan emin olun "Sanal makine Katılımcısı" gibi sanal makine ölçek kümesi üzerinde yazma izinleri:

   ```powershell
   Connect-AzAccount
   ```

2. Sanal makine ölçek kümesi özelliklerini kullanarak ilk alma `Get-AzVM` cmdlet'i. Sanal makine ölçek kümesi için bir kullanıcı tarafından atanan bir yönetilen kimlik atamak için ardından kullanmak `-IdentityType` ve `-IdentityID` açın [güncelleştirme AzVmss](/powershell/module/az.compute/update-azvmss) cmdlet'i. Değiştirin `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` kendi değerlerinizle.

   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Bir Azure sanal makine ölçek kümesinden bir kullanıcı tarafından atanan bir yönetilen kimlik Kaldır

Kullanıcı tarafından atanan birden çok yönetilen kimlik sanal makine ölçek kümeniz varsa, aşağıdaki komutları kullanarak tüm sonuncu kaldırabilirsiniz. `<RESOURCE GROUP>` ve `<VIRTUAL MACHINE SCALE SET NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<USER ASSIGNED IDENTITY NAME>` Sanal makine ölçek kümesi üzerinde kalmalıdır kullanıcı tarafından atanan yönetilen kimliğin adı özelliği. Bu bilgileri kullanarak sanal makine ölçek kümesi'nin kimlik bölümünde bulunabilir `az vmss show`:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED IDENTITY NAME>"
```
Bir sistem tarafından atanan yönetilen sanal makine ölçek kümeniz yoksa, kimlik ve istediğiniz kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırın, aşağıdaki komutu kullanın:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
Sanal makine ölçek kümesi, hem de sistem tarafından atanan ve yönetilen kimlikleri, kullanıcı tarafından atanan tüm kullanıcı tarafından atanan yönetilen kimlikleri yalnızca sistem tarafından atanan yönetilen kimliği kullanma geçerek kaldırabilirsiniz.

```powershell 
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynaklarına genel bakış için yönetilen kimlik](overview.md)
- Tam Azure VM oluşturmak için hızlı başlangıç kılavuzları, bkz:
  
  - [PowerShell ile bir Windows sanal makinesi oluşturma](../../virtual-machines/windows/quick-create-powershell.md) 
  - [PowerShell ile Linux sanal makinesi oluşturma](../../virtual-machines/linux/quick-create-powershell.md) 

















