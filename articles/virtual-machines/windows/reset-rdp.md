---
title: Parola veya uzak masaüstü yapılandırması Windows VM üzerinde sıfırlama | Microsoft Docs
description: Bir hesap parolası veya Uzak Masaüstü Hizmetleri Azure portalında veya Azure PowerShell kullanarak bir Windows VM üzerinde sıfırlama öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: cynthn
ms.openlocfilehash: b61b7501c94e9682a3b324488caf119ce4aad3df
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267212"
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Uzak Masaüstü hizmetini veya bir Windows VM oturum açma parolasını sıfırlama
Bir Windows sanal makine (VM) bağlanamıyorsanız, yerel yönetici parolasını sıfırlama veya Uzak Masaüstü hizmet yapılandırmasını (Windows etki alanı denetleyicilerinde desteklenmez). Parola sıfırlama için Azure PowerShell'de Azure portalından veya VM erişim uzantısı kullanabilirsiniz. VM oturum açtıktan sonra o kullanıcı için parola sıfırlama.  
PowerShell kullanıyorsanız, bilgisayarınızda yüklü olduğundan emin olun [yüklenmiş ve yapılandırılmış en son PowerShell Modülü](/powershell/azure/overview) ve Azure aboneliğinizde oturum açtınız. Ayrıca [Klasik dağıtım modeliyle oluşturulan VM'ler için bu adımları uygulamadan](https://docs.microsoft.com/azure/virtual-machines/windows/classic/reset-rdp).

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
Bilgisayarınızda yüklü olduğundan emin olun [yüklenmiş ve yapılandırılmış en son PowerShell Modülü](/powershell/azure/overview) ve Azure aboneliğinizle oturum `Connect-AzureRmAccount` cmdlet'i.

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parolasını sıfırlama**
Yönetici parolası veya kullanıcı adı ile Sıfırla [kümesi AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet'i. Sürüm 1 kullanım dışı olduğundan typeHandlerVersion 2.0 veya daha büyük olmalıdır. 

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
> VM üzerinde geçerli yerel yönetici hesabından farklı bir ad yazın, VMAccess uzantısını bu ada sahip bir yerel yönetici hesabı ekleyin ve bu hesap için belirtilen parola atayın. VM üzerinde yerel yönetici hesabı varsa, parolayı sıfırlar ve hesap devre dışı bırakılmışsa VMAccess uzantısını etkinleştirir.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Uzak Masaüstü hizmet yapılandırmasını sıfırlama**
Uzaktan erişim, VM ile Sıfırla [kümesi AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet'i. Aşağıdaki örnek adlı erişim uzantısı sıfırlar `myVMAccess` adlı VM üzerinde `myVM` içinde `myResourceGroup` kaynak grubu:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> Herhangi bir noktada bir VM yalnızca bir VM erişim aracıyı olabilir. VM erişim Aracısı özellikleri başarılı bir şekilde ayarlamak için `-ForceRerun` seçeneği kullanılabilir. Kullanırken `-ForceRerun`, herhangi bir önceki komut kullanılan VM erişim aracısı için aynı adı kullandığınızdan emin olun.

Hala uzaktan sanal makineye bağlanamıyorsanız, daha fazla adımları deneyin bkz [Windows tabanlı bir Azure sanal makine için sorun giderme Uzak Masaüstü bağlantıları](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Windows etki alanı denetleyicisine bağlantısı kesildiğinde bir etki alanı denetleyicisi yedekten geri yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Azure VM erişim uzantısı yanıt vermiyor ve parola sıfırlamaya yoksa, şunları yapabilirsiniz [çevrimdışı yerel Windows parola sıfırlama](reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bu yöntem daha gelişmiş bir işlemdir ve başka bir VM için sanal sabit disk sorunlu VM bağlanmanızı gerektirir. Bu makalede ilk belgelenen adımları izleyin ve yalnızca son çare olarak çevrimdışı parola sıfırlama yöntemi deneyin.

[Azure VM uzantıları ve özellikleri](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[RDP veya SSH ile bir Azure sanal makineye bağlanmak](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Windows tabanlı bir Azure sanal makine için Uzak Masaüstü bağlantı sorunlarını giderme](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

