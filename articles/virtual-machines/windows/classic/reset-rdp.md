---
title: Parola veya uzak masaüstü yapılandırmasının Azure Windows VM üzerinde sıfırlama | Microsoft Docs
description: Bir hesap parolası sıfırlama öğrenin veya bir Windows VM üzerinde Uzak Masaüstü Hizmetleri Azure portalında veya Azure PowerShell kullanarak Klasik dağıtım modeli kullanılarak oluşturulmuş.
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
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
ms.author: iainfou
ms.openlocfilehash: 516cb740f9acad19dac77db0239341b42a2b27fe
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30917658"
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-the-classic-deployment-model"></a>Uzak Masaüstü hizmetini veya Windows Klasik dağıtım modeli kullanılarak oluşturulan bir VM'de oturum açma parolasını sıfırlama
> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Ayrıca [Resource Manager dağıtım modeliyle oluşturulan VM'ler için bu adımları uygulamadan](../reset-rdp.md).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]


Bir Windows sanal makine (VM) bağlanamıyorsanız, yerel yönetici parolasını sıfırlama veya Uzak Masaüstü hizmet yapılandırmasını sıfırlayın. Parola sıfırlama için Azure PowerShell'de Azure portalından veya VM erişim uzantısı kullanabilirsiniz.

## <a name="ways-to-reset-configuration-or-credentials"></a>Yapılandırma veya kimlik bilgilerini sıfırlama yolları
Gereksinimlerinize bağlı olarak birkaç farklı şekilde, Uzak Masaüstü Hizmetleri ve kimlik bilgilerini sıfırlayabilirsiniz:

- [Azure portalını kullanarak Sıfırla](#azure-portal)
- [Azure PowerShell kullanarak Sıfırla](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Azure portalına
Kullanabileceğiniz [Azure portal](https://portal.azure.com) Uzak Masaüstü hizmetini sıfırlanır. Portal menü genişletmek için sol üst köşedeki üç çubuklarında'ı tıklatın ve ardından **sanal makineleri (Klasik)**:

![Azure VM için Gözat](./media/reset-rdp/Portal-Select-Classic-VM.png)

Windows sanal makineyi seçin ve ardından **uzaktan Sıfırla...** . Uzak Masaüstü yapılandırmasının sıfırlamak için aşağıdaki iletişim kutusu görüntülenir:

![Sıfırlama RDP yapılandırma sayfası](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

Kullanıcı adı ve parola yerel yönetici hesabının da sıfırlayabilirsiniz. Sanal makineden tıklatın **destek + sorun giderme** > **parola sıfırlama**. Parola sıfırlama dikey penceresinde görüntülenir:

![Parola sıfırlama sayfası](./media/reset-rdp/Portal-PW-Reset-Windows.png)

Yeni bir kullanıcı adı ve parolanızı girdikten sonra tıklatın **kaydetmek**.

## <a name="vmaccess-extension-and-powershell"></a>VMAccess uzantısını ve PowerShell
VM aracısının sanal makinede yüklü olduğundan emin olun. VMAccess uzantısını VM Aracısı kullanılabilir olduğu sürece, kullanmadan önce yüklü olması gerekmez. VM Aracısı zaten aşağıdaki komutu kullanarak yüklü olduğunu doğrulayın. ("MyCloudService" ve "myVM" bulut hizmetiniz ve, VM adlarıyla sırasıyla değiştirin. Çalıştırarak bu adları öğrenebilirsiniz `Get-AzureVM` hiçbir parametre olmadan.)

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

Varsa **write-host** komutu görüntüler **doğru**, VM aracısının yüklü olduğundan. Görüntüler, **False**, yönergeler ve karşıdan yükleme bağlantısı bkz [VM aracısı ve uzantılar - 2. parça](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blog gönderisi.

Portalı kullanarak sanal makine oluşturduysanız denetleyin olup olmadığını `$vm.GetInstance().ProvisionGuestAgent` döndürür **doğru**. Aksi halde, bu komutu kullanarak ayarlayabilirsiniz:

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

Çalıştırdığınız zaman bu komutu aşağıdaki hata engeller **kümesi AzureVMExtension** sonraki adımlarda komutu: "Sağlama Konuk Aracısı etkinleştirilmelidir VM nesnesinde Iaas VM erişim uzantısı ayarlamadan önce."

### <a name="reset-the-local-administrator-account-password"></a>**Yerel yönetici hesabı parolasını sıfırlama**
Geçerli yerel yönetici hesabı adı ve yeni bir parola ile oturum açma kimlik bilgileri oluşturun ve çalıştırın `Set-AzureVMAccessExtension` gibi.

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

Geçerli hesabından farklı bir ad yazın, VMAccess uzantısını yerel yönetici hesabı yeniden adlandırır, o hesabı için parola atar ve Uzak Masaüstü oturumu kapatma sorunlarını. Yerel yönetici hesabı devre dışı bırakılırsa, VMAccess uzantısını etkinleştirir.

Bu komutlar da Uzak Masaüstü hizmet yapılandırmasını sıfırlayın.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Uzak Masaüstü hizmet yapılandırmasını sıfırlama**
Uzak Masaüstü hizmet yapılandırmasını sıfırlamak için aşağıdaki komutu çalıştırın:

```powershell
Set-AzureVMAccessExtension –vm $vm | Update-AzureVM
```

VMAccess uzantısını sanal makinede iki komutu çalıştırır:

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

Bu komut 3389 numaralı TCP bağlantı noktasını kullanır gelen Uzak Masaüstü trafiğe izin veren yerleşik Windows Güvenlik Duvarı Grup etkinleştirir.

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

Bu komut fDenyTSConnections Uzak Masaüstü bağlantıları etkinleştirme 0 için kayıt defteri değerini ayarlar.

## <a name="next-steps"></a>Sonraki adımlar
Azure VM erişim uzantısı yanıt vermiyor ve parola sıfırlamaya yoksa, şunları yapabilirsiniz [çevrimdışı yerel Windows parola sıfırlama](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bu yöntem daha gelişmiş bir işlemdir ve başka bir VM için sanal sabit disk sorunlu VM bağlanmanızı gerektirir. Bu makalede ilk belgelenen adımları izleyin ve yalnızca son çare olarak çevrimdışı parola sıfırlama yöntemi deneyin.

[Azure VM uzantıları ve özellikleri](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[RDP veya SSH ile bir Azure sanal makineye bağlanmak](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Windows tabanlı bir Azure sanal makine için Uzak Masaüstü bağlantı sorunlarını giderme](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

