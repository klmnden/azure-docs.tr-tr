---
title: Parolayı veya uzak masaüstü yapılandırmasının bir Windows VM'de sıfırlama | Microsoft Docs
description: Bir hesap parolası veya Uzak Masaüstü Hizmetleri Azure portal veya Azure PowerShell kullanarak bir Windows VM'de sıfırlama öğrenin.
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
ms.date: 03/23/2018
ms.author: genli
ms.openlocfilehash: 08461811203232d5db1ae9c8f34f4ac180b6b0ce
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268317"
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Uzak Masaüstü hizmetini veya bir Windows VM'de oturum açma parolasını sıfırlama
Bir Windows sanal makinesi (VM) bağlanamıyorsanız, yerel yönetici parolasını sıfırlayabilir veya Uzak Masaüstü hizmet yapılandırmasını (Windows etki alanı denetleyicilerinde desteklenmez). Parola sıfırlama için Azure PowerShell'de Azure portal'ı veya VM erişimi uzantısı kullanabilirsiniz. VM'de oturum açtıktan sonra bu kullanıcının parolasını sıfırlamalısınız.  
PowerShell kullanıyorsanız, bilgisayarınızda yüklü olduğundan emin olun [en son PowerShell modülünün yüklü ve yapılandırılmış](/powershell/azure/overview) ve Azure aboneliğinizde oturum açtınız. Ayrıca [Klasik dağıtım modeliyle oluşturulan sanal makineler için bu adımları uygulamadan](https://docs.microsoft.com/azure/virtual-machines/windows/classic/reset-rdp).

## <a name="ways-to-reset-configuration-or-credentials"></a>Yapılandırma veya kimlik bilgilerini sıfırlama yolları
Uzak Masaüstü Hizmetleri ve kimlik bilgilerini, gereksinimlerinize bağlı olarak birkaç farklı yolla sıfırlayabilir:

- [Azure portalını kullanarak Sıfırla](#azure-portal)
- [Azure PowerShell kullanarak Sıfırla](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure portal
Portal menü genişletmek için sol üst köşedeki üç çubuğa tıklayın ve ardından **sanal makineler**:

![Azure sanal Makineniz için Gözat](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parola sıfırlama**

Windows sanal makinenizi seçin sonra tıklatın **destek + sorun giderme** > **parolayı Sıfırla**. Parola sıfırlama dikey penceresi görüntülenir:

![Parola sıfırlama sayfası](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

Kullanıcı adı ve yeni bir parola girin ve ardından tıklayın **güncelleştirme**. Sanal makinenizde tekrar bağlanmayı deneyin.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Uzak Masaüstü hizmet yapılandırmasını Sıfırla**

Windows sanal makinenizi seçin sonra tıklatın **destek + sorun giderme** > **parolayı Sıfırla**. Parola sıfırlama dikey penceresi görüntülenir. 

![RDP yapılandırmasını Sıfırla](./media/reset-rdp/Portal-RM-RDP-Reset.png)

Seçin **yalnızca Yapılandırmayı Sıfırla** aşağı açılan menüden ardından **güncelleştirme**. Sanal makinenizde tekrar bağlanmayı deneyin.


## <a name="vmaccess-extension-and-powershell"></a>VMAccess uzantısı ile PowerShell
Sahip olduğunuzdan emin olun [en son PowerShell modülünün yüklü ve yapılandırılmış](/powershell/azure/overview) ve Azure aboneliğinizde oturum açtınız `Connect-AzureRmAccount` cmdlet'i.

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parola sıfırlama**
Yönetici parolası veya kullanıcı adı ile sıfırlama [kümesi AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet'i. Sürüm 1 kullanım dışı olduğundan typeHandlerVersion 2.0 veya daha büyük olmalıdır. 

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
> Sanal makinenizde geçerli yerel yönetici hesabı yerine farklı bir ad yazarsanız, VMAccess uzantısı bu ada sahip bir yerel yönetici hesabı ekleyin ve bu hesabı belirtilen parolanızı atayın. Sanal makinenizin üzerinde yerel yönetici hesabı varsa, parolayı sıfırlar ve hesap devre dışı bırakılırsa VMAccess uzantısı sağlar.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Uzak Masaüstü hizmet yapılandırmasını Sıfırla**
İle sanal makinenize uzaktan erişimi sıfırlama [kümesi AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet'i. Aşağıdaki örnekte adlı erişim uzantısı sıfırlar `myVMAccess` adlı VM'de `myVM` içinde `myResourceGroup` kaynak grubu:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> Herhangi bir noktada bir sanal makine yalnızca tek bir VM erişim aracı olabilir. VM erişimi başarıyla aracı özelliklerini ayarlamak için `-ForceRerun` seçeneği kullanılabilir. Kullanırken `-ForceRerun`, önceki tüm komutlarında kullanılan VM erişimi aracısı için aynı adı kullandığınızdan emin olun.

Yine de uzaktan sanal makinenize bağlanamıyorsanız, adresindeki denemek için daha fazla adımlara bakın [Uzak Masaüstü bağlantılarında sorun giderme Windows tabanlı bir Azure sanal makine](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Windows etki alanı denetleyicisine bağlantısı kesilirse, bir etki alanı denetleyicisi yedekten geri yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Azure VM erişimi uzantısı yanıt vermiyor ve parolanızı sıfırlayamıyoruz varsa [çevrimdışı yerel Windows parola sıfırlama](../windows/reset-local-password-without-agent.md). Bu yöntem, daha gelişmiş bir işlemdir ve sorunlu sanal makinenin sanal sabit diski başka bir sanal makineye bağlanmanızı gerektirir. Bu makalede ilk belgelenen adımları izleyin ve yalnızca son çare olarak çevrimdışı parola sıfırlama yöntemi çalışır.

[Azure VM uzantıları ve özellikleri](../extensions/features-windows.md)

[Bir Azure sanal makinesinde RDP veya SSH ile bağlanma](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Windows tabanlı bir Azure sanal makinesine Uzak Masaüstü bağlantılarında sorun giderme](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

