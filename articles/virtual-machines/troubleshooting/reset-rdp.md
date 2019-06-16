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
ms.date: 03/25/2019
ms.author: genli
ms.openlocfilehash: 0a12cbabc28640283f5a28eb7a83c7d7717e0882
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60921227"
---
# <a name="reset-remote-desktop-services-or-its-administrator-password-in-a-windows-vm"></a>Uzak Masaüstü Hizmetleri veya bir Windows VM'de yönetici parolasını sıfırlama
Bir Windows sanal makinesi (VM) bağlanamıyorsanız, yerel Yönetici parolanızı sıfırlayın ya da (Windows etki alanı denetleyicilerinde desteklenmez) Uzak Masaüstü Hizmetleri yapılandırmasını sıfırlayın. Parolayı sıfırlamak için Azure portalı veya Azure PowerShell'deki VM Erişimi uzantısını kullanın. VM'de oturum açtıktan sonra yerel yönetici parolasını sıfırlayın.  
PowerShell kullanıyorsanız, bilgisayarınızda yüklü olduğundan emin olun [en son PowerShell modülünün yüklü ve yapılandırılmış](/powershell/azure/overview) ve Azure aboneliğinizde oturum açtınız. Ayrıca [klasik dağıtım modeliyle oluşturulmuş olan VM'lerde bu adımları da gerçekleştirebilirsiniz](https://docs.microsoft.com/azure/virtual-machines/windows/classic/reset-rdp).

Uzak Masaüstü Hizmetleri ve kimlik bilgileri aşağıdaki yollarla sıfırlayabilir:

- [Azure portalını kullanarak sıfırlayın](#reset-by-using-the-azure-portal)

- [VMAccess uzantısı ve PowerShell kullanarak sıfırlayın](#reset-by-using-the-vmaccess-extension-and-powershell)

## <a name="reset-by-using-the-azure-portal"></a>Azure portalını kullanarak sıfırlayın

İlk olarak oturum açın [Azure portalında](https://portal.azure.com) seçip **sanal makineler** sol menüsünde. 

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parola sıfırlama**

1. Windows VM'nizi seçin ve ardından **parolayı Sıfırla** altında **destek + sorun giderme**. **Parolayı Sıfırla** penceresi görüntülenir.

2. Seçin **parolayı Sıfırla**, bir kullanıcı adı ve parola girin ve ardından **güncelleştirme**. 

3. Sanal makinenizde tekrar bağlanmayı deneyin.

### <a name="reset-the-remote-desktop-services-configuration"></a>**Uzak Masaüstü Hizmetleri yapılandırmasını Sıfırla**

Bu işlem sanal makinede Uzak Masaüstü hizmetini etkinleştirmek ve varsayılan RDP bağlantı noktası 3389 için bir güvenlik duvarı kuralı oluşturun.

1. Windows VM'nizi seçin ve ardından **parolayı Sıfırla** altında **destek + sorun giderme**. **Parolayı Sıfırla** penceresi görüntülenir. 

2. Seçin **yalnızca Yapılandırmayı Sıfırla** seçip **güncelleştirme**. 

3. Sanal makinenizde tekrar bağlanmayı deneyin.

## <a name="reset-by-using-the-vmaccess-extension-and-powershell"></a>VMAccess uzantısı ve PowerShell kullanarak sıfırlayın

İlk olarak, bilgisayarınızda yüklü olduğundan emin olun [en son PowerShell modülünün yüklü ve yapılandırılmış](/powershell/azure/overview) ve Azure aboneliğinizi kullanarak oturum açtınız [Connect AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet'i.

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parola sıfırlama**

- Yönetici parolası veya kullanıcı adı ile sıfırlama [kümesi AzVMAccessExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmaccessextension) PowerShell cmdlet'i. `typeHandlerVersion` Ayarı olmalıdır 2.0 veya daha büyük olduğundan sürüm 1. 

    ```powershell
    $SubID = "<SUBSCRIPTION ID>" 
    $RgName = "<RESOURCE GROUP NAME>" 
    $VmName = "<VM NAME>" 
    $Location = "<LOCATION>" 
 
    Connect-AzAccount 
    Select-AzSubscription -SubscriptionId $SubID 
    Set-AzVMAccessExtension -ResourceGroupName $RgName -Location $Location -VMName $VmName -Credential (get-credential) -typeHandlerVersion "2.0" -Name VMAccessAgent 
    ```

    > [!NOTE] 
    > Sanal makinenizde geçerli yerel yönetici hesabı yerine farklı bir ad girin, VMAccess uzantısı bu ada sahip bir yerel yönetici hesabı ekleyin ve bu hesabı belirtilen parolanızı atayın. Sanal makinenizin üzerinde yerel yönetici hesabı varsa, VMAccess uzantısı parolayı sıfırlar. Hesap devre dışı bırakılırsa, bunu VMAccess uzantısını etkinleştirir.

### <a name="reset-the-remote-desktop-services-configuration"></a>**Uzak Masaüstü Hizmetleri yapılandırmasını Sıfırla**

1. İle sanal makinenize uzaktan erişimi sıfırlama [kümesi AzVMAccessExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmaccessextension) PowerShell cmdlet'i. Aşağıdaki örnekte adlı erişim uzantısı sıfırlar `myVMAccess` adlı VM'de `myVM` içinde `myResourceGroup` kaynak grubu:

    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
    ```

    > [!TIP]
    > Herhangi bir noktada bir sanal makine yalnızca tek bir VM erişim aracı olabilir. VM erişimi aracı özellikleri ayarlamak için kullanın `-ForceRerun` seçeneği. Kullanırken `-ForceRerun`, önceki tüm komutlarında kullanılan VM erişimi aracısı için aynı adı kullandığınızdan emin olun.

1. Yine de uzaktan sanal makinenize bağlanamıyorsanız, bkz. [Uzak Masaüstü bağlantılarında sorun giderme Windows tabanlı bir Azure sanal makine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Windows etki alanı denetleyicisine bağlantısı kesilirse, bir etki alanı denetleyicisi yedekten geri yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure VM erişimi uzantısı yanıt vermiyor ve parolanızı sıfırlayamıyoruz varsa [çevrimdışı yerel Windows parola sıfırlama](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bu yöntem, daha gelişmiş ve sorunlu sanal makinenin sanal sabit diski başka bir sanal makineye bağlanmanızı gerektirir. Bu makalede ilk belgelenen adımları izleyin ve yalnızca söz konusu adımlar işe yaramazsa, çevrimdışı parola sıfırlama yöntemi deneyin.

- [Azure VM uzantıları ve özellikleri hakkında bilgi edinin](../extensions/features-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

- [Bir Azure sanal makinesinde RDP veya SSH ile bağlanma](https://msdn.microsoft.com/library/azure/dn535788.aspx).

- [Windows tabanlı bir Azure sanal makinesine Uzak Masaüstü bağlantılarında sorun giderme](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

