---
title: Uzak Masaüstü Hizmetleri veya bir Windows VM'de yönetici parolası sıfırlama | Microsoft Docs
description: Azure portal veya Azure PowerShell kullanarak bir hesap parolası veya Uzak Masaüstü Hizmetleri'yle azure'da bir Windows VM sıfırlama öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 99b915f14aaa7d306d1bceb5bd4f6bb23abdb929
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51245384"
---
# <a name="reset-remote-desktop-services-or-its-administrator-password-in-a-windows-vm"></a>Uzak Masaüstü Hizmetleri veya bir Windows VM'de yönetici parolasını sıfırlama
Bir Windows sanal makinesi (VM) bağlanamıyorsanız, yerel Yönetici parolanızı sıfırlayın ya da (Windows etki alanı denetleyicilerinde desteklenmez) Uzak Masaüstü Hizmetleri yapılandırmasını sıfırlayın. Parola sıfırlama için Azure PowerShell'de Azure portal'ı veya VM erişimi uzantısı kullanın. VM'ye açtıktan sonra yerel yönetici için parolayı sıfırlayın.  
PowerShell kullanıyorsanız, bilgisayarınızda yüklü olduğundan emin olun [en son PowerShell modülünün yüklü ve yapılandırılmış](/powershell/azure/overview) ve Azure aboneliğinizde oturum açtınız. Ayrıca [Klasik dağıtım modeliyle oluşturulan sanal makineler için bu adımları uygulamadan](https://docs.microsoft.com/azure/virtual-machines/windows/classic/reset-rdp).

Uzak Masaüstü Hizmetleri ve kimlik bilgileri aşağıdaki yollarla sıfırlayabilir:

- [Azure portalını kullanarak sıfırlayın](#reset-by-using-the-azure-portal)

- [VMAccess uzantısı ve PowerShell kullanarak sıfırlayın](#reset-by-using-the-vmaccess-extension-and-powershell)

## <a name="reset-by-using-the-azure-portal"></a>Azure portalını kullanarak sıfırlayın

İlk olarak oturum açın [Azure portalında](https://portal.azure.com) seçip **sanal makineler** sol menüsünde. 

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parola sıfırlama**

1. Windows VM'nizi seçin ve ardından **parolayı Sıfırla** altında **destek + sorun giderme**. **Parolayı Sıfırla** penceresi görüntülenir.

1. Seçin **parolayı Sıfırla**, bir kullanıcı adı ve parola girin ve ardından **güncelleştirme**. 

1. Sanal makinenizde tekrar bağlanmayı deneyin.

### <a name="reset-the-remote-desktop-services-configuration"></a>**Uzak Masaüstü Hizmetleri yapılandırmasını Sıfırla**

1. Windows VM'nizi seçin ve ardından **parolayı Sıfırla** altında **destek + sorun giderme**. **Parolayı Sıfırla** penceresi görüntülenir. 

1. Seçin **yalnızca Yapılandırmayı Sıfırla** seçip **güncelleştirme**. 

1. Sanal makinenizde tekrar bağlanmayı deneyin.


## <a name="reset-by-using-the-vmaccess-extension-and-powershell"></a>VMAccess uzantısı ve PowerShell kullanarak sıfırlayın

İlk olarak, bilgisayarınızda yüklü olduğundan emin olun [en son PowerShell modülünün yüklü ve yapılandırılmış](/powershell/azure/overview) ve Azure aboneliğinizi kullanarak oturum açtınız [Connect-AzureRmAccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet'i.

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parola sıfırlama**

- Yönetici parolası veya kullanıcı adı ile sıfırlama [kümesi AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet'i. `typeHandlerVersion` Ayarı olmalıdır 2.0 veya daha büyük olduğundan sürüm 1. 

    ```powershell
    $SubID = "<SUBSCRIPTION ID>" 
    $RgName = "<RESOURCE GROUP NAME>" 
    $VmName = "<VM NAME>" 
    $Location = "<LOCATION>" 
 
    Connect-AzureRmAccount 
    Select-AzureRMSubscription -SubscriptionId $SubID 
    Set-AzureRmVMAccessExtension -ResourceGroupName $RgName -Location $Location -VMName $VmName -Credential (get-credential) -typeHandlerVersion "2.0" -Name VMAccessAgent 
    ```

    > [!NOTE] 
    > Sanal makinenizde geçerli yerel yönetici hesabı yerine farklı bir ad girin, VMAccess uzantısı bu ada sahip bir yerel yönetici hesabı ekleyin ve bu hesabı belirtilen parolanızı atayın. Sanal makinenizin üzerinde yerel yönetici hesabı varsa, VMAccess uzantısı parolayı sıfırlar. Hesap devre dışı bırakılırsa, bunu VMAccess uzantısını etkinleştirir.

### <a name="reset-the-remote-desktop-services-configuration"></a>**Uzak Masaüstü Hizmetleri yapılandırmasını Sıfırla**

1. İle sanal makinenize uzaktan erişimi sıfırlama [kümesi AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet'i. Aşağıdaki örnekte adlı erişim uzantısı sıfırlar `myVMAccess` adlı VM'de `myVM` içinde `myResourceGroup` kaynak grubu:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
    ```

    > [!TIP]
    > Herhangi bir noktada bir sanal makine yalnızca tek bir VM erişim aracı olabilir. VM erişimi aracı özellikleri ayarlamak için kullanın `-ForceRerun` seçeneği. Kullanırken `-ForceRerun`, önceki tüm komutlarında kullanılan VM erişimi aracısı için aynı adı kullandığınızdan emin olun.

1. Yine de uzaktan sanal makinenize bağlanamıyorsanız, bkz. [Uzak Masaüstü bağlantılarında sorun giderme Windows tabanlı bir Azure sanal makine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Windows etki alanı denetleyicisine bağlantısı kesilirse, bir etki alanı denetleyicisi yedekten geri yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure VM erişimi uzantısı yanıt vermiyor ve parolanızı sıfırlayamıyoruz varsa [çevrimdışı yerel Windows parola sıfırlama](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bu yöntem, daha gelişmiş ve sorunlu sanal makinenin sanal sabit diski başka bir sanal makineye bağlanmanızı gerektirir. Bu makalede ilk belgelenen adımları izleyin ve yalnızca söz konusu adımlar işe yaramazsa, çevrimdışı parola sıfırlama yöntemi deneyin.

- [Azure VM uzantıları ve özellikleri hakkında bilgi edinin](../extensions/features-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

- [Bir Azure sanal makinesinde RDP veya SSH ile bağlanma](https://msdn.microsoft.com/library/azure/dn535788.aspx).

- [Windows tabanlı bir Azure sanal makinesine Uzak Masaüstü bağlantılarında sorun giderme](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

