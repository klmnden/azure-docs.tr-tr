---
title: "Parola veya uzak masaüstü yapılandırması Windows VM üzerinde sıfırlama | Microsoft Docs"
description: "Bir hesap parolası veya Uzak Masaüstü Hizmetleri Azure portalında veya Azure PowerShell kullanarak bir Windows VM üzerinde sıfırlama öğrenin."
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 105dc8a17d0bf8862b772ad241f4522e4c658095
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Uzak Masaüstü hizmetini veya bir Windows VM oturum açma parolasını sıfırlama
Bir Windows sanal makine (VM) bağlanamıyorsanız, yerel yönetici parolasını sıfırlama veya Uzak Masaüstü hizmet yapılandırmasını sıfırlayın. Parola sıfırlama için Azure PowerShell'de Azure portalından veya VM erişim uzantısı kullanabilirsiniz. PowerShell kullanıyorsanız, bilgisayarınızda yüklü olduğundan emin olun [yüklenmiş ve yapılandırılmış en son PowerShell Modülü](/powershell/azure/overview) ve Azure aboneliğinizde oturum açtınız. Ayrıca [Klasik dağıtım modeliyle oluşturulan VM'ler için bu adımları uygulamadan](reset-rdp.md).

## <a name="ways-to-reset-configuration-or-credentials"></a>Yapılandırma veya kimlik bilgilerini sıfırlama yolları
Gereksinimlerinize bağlı olarak birkaç farklı şekilde, Uzak Masaüstü Hizmetleri ve kimlik bilgilerini sıfırlayabilirsiniz:

- [Azure portalını kullanarak Sıfırla](#azure-portal)
- [Azure PowerShell kullanarak Sıfırla](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure portalına
Portal menü genişletmek için sol üst köşedeki üç çubuklarında'ı tıklatın ve ardından **sanal makineleri**:

![Azure VM için Gözat](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parolasını sıfırlama**

Windows sanal makinenizi seçip'i tıklatın **destek + sorun giderme** > **parola sıfırlama**. Parola sıfırlama dikey penceresinde görüntülenir:

![Parola sıfırlama sayfası](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

Kullanıcı adı ve yeni bir parola girin ve ardından **güncelleştirme**. VM'nize yeniden bağlanmayı deneyin.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Uzak Masaüstü hizmet yapılandırmasını sıfırlama**

Windows sanal makinenizi seçip'i tıklatın **destek + sorun giderme** > **parola sıfırlama**. Parola sıfırlama dikey penceresinde görüntülenir. 

![RDP yapılandırması sıfırlandı](./media/reset-rdp/Portal-RM-RDP-Reset.png)

Seçin **yalnızca sıfırlama yapılandırma** aşağı açılır menüden, ardından **güncelleştirme**. VM'nize yeniden bağlanmayı deneyin.


## <a name="vmaccess-extension-and-powershell"></a>VMAccess uzantısını ve PowerShell
Bilgisayarınızda yüklü olduğundan emin olun [yüklenmiş ve yapılandırılmış en son PowerShell Modülü](/powershell/azure/overview) ve Azure aboneliğinizle oturum `Login-AzureRmAccount` cmdlet'i.

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parolasını sıfırlama**
Yönetici parolası veya kullanıcı adı ile Sıfırla [kümesi AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet'i. Hesap kimlik bilgilerinizi gibi oluşturun:

```powershell
$cred=Get-Credential
```

> [!NOTE] 
> VM üzerinde geçerli yerel yönetici hesabından farklı bir ad yazın, VMAccess uzantısını yerel yönetici hesabı yeniden adlandırır, bu hesap için belirtilen parolanızı atar ve bir Uzak Masaüstü oturumu kapatma olayı verir. VMAccess uzantısını VM üzerinde yerel yönetici hesabı devre dışı bırakılırsa sağlar.

Aşağıdaki örnek adlı VM güncelleştirmeleri `myVM` kaynak grubunda adlı `myResourceGroup` belirtilen kimlik bilgileri.

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

### <a name="reset-the-remote-desktop-service-configuration"></a>**Uzak Masaüstü hizmet yapılandırmasını sıfırlama**
Uzaktan erişim, VM ile Sıfırla [kümesi AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet'i. Aşağıdaki örnek adlı erişim uzantısı sıfırlar `myVMAccess` adlı VM üzerinde `myVM` içinde `myResourceGroup` kaynak grubu:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" `
    -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> Herhangi bir noktada bir VM yalnızca bir VM erişim aracıyı olabilir. VM erişim Aracısı özellikleri başarılı bir şekilde ayarlamak için `-ForceRerun` seçeneği kullanılabilir. Kullanırken `-ForceRerun`, herhangi bir önceki komut kullanılan VM erişim aracısı için aynı adı kullandığınızdan emin olun.

Hala uzaktan sanal makineye bağlanamıyorsanız, daha fazla adımları deneyin bkz [Windows tabanlı bir Azure sanal makine için sorun giderme Uzak Masaüstü bağlantıları](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


## <a name="next-steps"></a>Sonraki adımlar
Azure VM erişim uzantısı yanıt vermiyor ve parola sıfırlamaya yoksa, şunları yapabilirsiniz [çevrimdışı yerel Windows parola sıfırlama](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bu yöntem daha gelişmiş bir işlemdir ve başka bir VM için sanal sabit disk sorunlu VM bağlanmanızı gerektirir. Bu makalede ilk belgelenen adımları izleyin ve yalnızca son çare olarak çevrimdışı parola sıfırlama yöntemi deneyin.

[Azure VM uzantıları ve özellikleri](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[RDP veya SSH ile bir Azure sanal makineye bağlanmak](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Windows tabanlı bir Azure sanal makine için Uzak Masaüstü bağlantı sorunlarını giderme](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

