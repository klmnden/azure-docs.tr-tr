---
title: Azure'da Windows sanal makinesi üzerinde Uzak Masaüstü yapılandırması ve parola sıfırlama | Microsoft Docs
description: Uzak Masaüstü Hizmetleri'yle azure'da bir Windows VM Azure portal veya Azure PowerShell kullanarak Klasik dağıtım modeli kullanılarak oluşturulmuş veya bir hesap parolası sıfırlama öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cynthn
ms.openlocfilehash: bbe8059b3a239570c2c9b25586dae9adbe25312d
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37931387"
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-the-classic-deployment-model"></a>Uzak Masaüstü hizmetini veya Klasik dağıtım modeli kullanılarak oluşturulan bir Windows VM'de oturum açma parolasını sıfırlama
> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Ayrıca [Resource Manager dağıtım modeliyle oluşturulan sanal makineler için bu adımları uygulamadan](../reset-rdp.md).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]


Bir Windows sanal makinesi (VM) bağlanamıyorsanız, yerel yönetici parolasını sıfırlayabilir ya da Uzak Masaüstü hizmet yapılandırmasını sıfırlayın. Parola sıfırlama için Azure PowerShell'de Azure portal'ı veya VM erişimi uzantısı kullanabilirsiniz.

## <a name="ways-to-reset-configuration-or-credentials"></a>Yapılandırma veya kimlik bilgilerini sıfırlama yolları
Uzak Masaüstü Hizmetleri ve kimlik bilgilerini, gereksinimlerinize bağlı olarak birkaç farklı yolla sıfırlayabilir:

- [Azure portalını kullanarak Sıfırla](#azure-portal)
- [Azure PowerShell kullanarak Sıfırla](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure portalına
Kullanabileceğiniz [Azure portalında](https://portal.azure.com) Uzak Masaüstü hizmetini sıfırlama. Portal menü genişletmek için sol üst köşedeki üç çubuğa tıklayın ve ardından **sanal makineler (Klasik)**:

![Azure sanal Makineniz için Gözat](./media/reset-rdp/Portal-Select-Classic-VM.png)

Windows sanal makinenizi seçin ve ardından **uzaktan Sıfırla...** . Uzak Masaüstü yapılandırmasını sıfırlamak için aşağıdaki iletişim kutusu görüntülenir:

![Sıfırlama RDP yapılandırma sayfası](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

Ayrıca, kullanıcı adı ve yerel yönetici hesabının parolasını sıfırlayabilirsiniz. Sanal makineden tıklayın **destek + sorun giderme** > **parolayı Sıfırla**. Parola sıfırlama dikey penceresi görüntülenir:

![Parola sıfırlama sayfası](./media/reset-rdp/Portal-PW-Reset-Windows.png)

Yeni kullanıcı adı ve parolanızı girdikten sonra tıklayın **Kaydet**.

## <a name="vmaccess-extension-and-powershell"></a>VMAccess uzantısı ile PowerShell
VM Aracısı sanal makinede yüklü olduğundan emin olun. VMAccess uzantısını VM Aracısı kullanılabilir olduğu sürece, kullanabilmeniz için önce yüklü olması gerekmez. VM aracısı aşağıdaki komutu kullanarak zaten yüklendiğini doğrulayın. ("MyCloudService" ve "myVM" bulut hizmetinizin ve sanal makinenize adlarıyla sırasıyla değiştirin. Bu adlar çalıştırarak öğrenebilirsiniz `Get-AzureVM` hiçbir parametre olmadan.)

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

Varsa **write-host** komutunu görüntüler **True**, VM Aracısı yüklenir. Görüntüler, **False**, yönergeleri ve indirme bağlantısı [VM aracısı ve uzantıları - 2. bölüm](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blog gönderisinden.

Portalı kullanarak sanal makine oluşturduğunuzda, denetleyin olmadığını `$vm.GetInstance().ProvisionGuestAgent` döndürür **True**. Aksi durumda, bu komutu kullanarak ayarlayabilirsiniz:

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

Çalıştırdığınız zaman, bu komut şu hata engeller **kümesi AzureVMExtension** sonraki adımlarda komutu: "Sağlama Konuk Aracısı etkin olmalıdır VM nesnede Iaas VM erişimi uzantısı ayarlamadan önce."

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parola sıfırlama**
Geçerli yerel yönetici hesabı adı ve yeni bir parola ile oturum açma kimlik bilgileri oluşturun ve çalıştırın `Set-AzureVMAccessExtension` gibi.

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

Geçerli hesaptan farklı bir ad yazın, VMAccess uzantısı yerel yönetici hesabı yeniden adlandırır, o hesabı için parola atar ve Uzak Masaüstü oturumu kapatma sorunlarını. Yerel yönetici hesabı devre dışı bırakılırsa, bu VMAccess uzantısı sağlar.

Bu komutlar da Uzak Masaüstü hizmet yapılandırmasını sıfırlayın.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Uzak Masaüstü hizmet yapılandırmasını Sıfırla**
Uzak Masaüstü hizmet yapılandırmasını sıfırlamak için aşağıdaki komutu çalıştırın:

```powershell
Set-AzureVMAccessExtension –vm $vm | Update-AzureVM
```

VMAccess uzantısı, sanal makinede iki komutu çalıştırır:

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

Bu komut, kullandığı TCP bağlantı noktası 3389 gelen Uzak Masaüstü trafiğine izin veren yerleşik Windows Güvenlik Duvarı grubu sağlar.

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

Bu komut, kayıt defteri değerini 0 olarak, Uzak Masaüstü bağlantıları etkinleştirme fDenyTSConnections ayarlar.

## <a name="next-steps"></a>Sonraki adımlar
Azure VM erişimi uzantısı yanıt vermiyor ve parolanızı sıfırlayamıyoruz varsa [çevrimdışı yerel Windows parola sıfırlama](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bu yöntem, daha gelişmiş bir işlemdir ve sorunlu sanal makinenin sanal sabit diski başka bir sanal makineye bağlanmanızı gerektirir. Bu makalede ilk belgelenen adımları izleyin ve yalnızca son çare olarak çevrimdışı parola sıfırlama yöntemi çalışır.

[Azure VM uzantıları ve özellikleri](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Bir Azure sanal makinesinde RDP veya SSH ile bağlanma](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Windows tabanlı bir Azure sanal makinesine Uzak Masaüstü bağlantılarında sorun giderme](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

