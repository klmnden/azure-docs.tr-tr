---
title: "Azure Windows VM için RDP ile bağlantı kurulamıyor | Microsoft Docs"
description: "Windows sanal makinenizde Uzak Masaüstü kullanarak Azure bağlanamadığınızda sorunlarını giderme"
keywords: "Uzak Masaüstü hata, Uzak Masaüstü Bağlantısı hatası, VM için bağlantı kuramıyor Uzak Masaüstü sorunlarını giderme"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 0d740f8e-98b8-4e55-bb02-520f604f5b18
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 4731a34d143d402372aaff7c03f95dbf0bb508a4
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Bir Azure sanal makinesi için Uzak Masaüstü bağlantı sorunlarını giderme
Uzak Masaüstü Protokolü (RDP) bağlantısı, Windows tabanlı Azure sanal makine (VM), VM erişilemiyor bırakarak çeşitli nedenlerle başarısız olabilir. VM, ağ bağlantısı veya ana bilgisayarınızda Uzak Masaüstü İstemcisi Uzak Masaüstü hizmetiyle sorunu olabilir. Bu makalede, bazı RDP bağlantı sorunlarını gidermek için en yaygın yöntemleri size yol gösterir. 

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Hızlı sorun giderme adımları
Sorun giderme her adımdan sonra VM yeniden bağlanmayı deneyin:

1. Uzak Masaüstü yapılandırmasının sıfırlayın.
2. Ağ güvenlik grubu denetleme kuralları / bulut Hizmetleri uç noktaları.
3. VM Konsolu günlüklerini gözden geçirin.
4. NIC VM için sıfırlayın.
5. VM kaynak durumu denetleyin.
6. VM parolanızı sıfırlayın.
7. VM'yi yeniden başlatın.
8. VM'yi yeniden dağıtın.

Ayrıntılı adımları ve açıklamalar gerekiyorsa okuma devam edin. Bu yerel ağ ekipmanları yönlendiriciler gibi doğrulayın ve güvenlik duvarları engellemediğini giden TCP bağlantı noktası 3389, içinde belirtildiği gibi [sorun giderme senaryoları RDP ayrıntılı](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!TIP]
> Varsa **Bağlan** VM'yi kapatma portalda gri ve Azure bağlanmamış düğmesini bir [hızlı rota](../../expressroute/expressroute-introduction.md) veya [siteden siteye VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) bağlantısına ihtiyacı oluşturmak ve RDP kullanmadan önce VM genel bir IP adresi atamak. [Azure’da genel IP adresleri](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) hakkında daha fazlasını okuyabilirsiniz.


## <a name="ways-to-troubleshoot-rdp-issues"></a>RDP sorunlarını gidermenin yolları
Aşağıdaki yöntemlerden birini kullanarak Resource Manager dağıtım modeli kullanılarak oluşturulan sanal makineleri giderebilirsiniz:

* [Azure portal](#using-the-azure-portal) - RDP yapılandırması veya kullanıcı kimlik bilgileri hızlı bir şekilde sıfırlamanız gerekir ve Azure araçlarının yüklü olduğu yoksa harika.
* [Azure PowerShell](#using-azure-powershell) - PowerShell istemiyle kullanabiliyorsanız hızlı bir şekilde Azure PowerShell cmdlet'lerini kullanarak RDP yapılandırması veya kullanıcı kimlik bilgilerini sıfırlayın.

Ayrıca kullanılarak oluşturulan sanal makineleri sorun giderme adımları bulabilirsiniz [Klasik dağıtım modeli](#troubleshoot-vms-created-using-the-classic-deployment-model).

<a id="fix-common-remote-desktop-errors"></a>

## <a name="troubleshoot-using-the-azure-portal"></a>Azure Portalı'nı kullanarak sorun giderme
Sorun giderme her adımdan sonra VM yeniden bağlanmayı deneyin. Hala bağlanamıyorsanız, sonraki adıma deneyin.

1. **RDP bağlantınızı sıfırlama**. Sorun giderme Bu adım, uzak bağlantıları devre dışı bırakıldığında veya Windows Güvenlik duvarı kurallarını RDP, örneğin engelliyor RDP yapılandırması sıfırlar.
   
    Azure portalında, VM'yi seçin. Ayarları bölmesine aşağı **destek + sorun giderme** listenin alt kısmına. Tıklatın **parola sıfırlama** düğmesi. Ayarlama **modu** için **yalnızca sıfırlama yapılandırma** ve ardından **güncelleştirme** düğmesi:
   
    ![Azure portalında RDP yapılandırması sıfırlandı](./media/troubleshoot-rdp-connection/reset-rdp.png)
2. **Doğrulama ağ güvenlik grubu kuralları**. [IP akışı doğrulamayı](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) kullanarak Ağ Güvenlik Grubu’ndaki bir kuralın bir sanal makineye giden veya gelen trafiği engelleyip engellemediğini doğrulayın. Gelen "izin ver" NSG emin olmak için etkili güvenlik grubu kuralları gözden geçirebilirsiniz kuralı var ve RDP bağlantı noktası (varsayılan 3389) öncelik. Daha fazla bilgi için bkz: [kullanarak etkili güvenlik VM gidermek için kuralları trafiğinin akmasını](../../virtual-network/virtual-network-nsg-troubleshoot-portal.md#using-effective-security-rules-to-troubleshoot-vm-traffic-flow).

3. **VM önyükleme tanılaması gözden**. Bu sorun giderme adımı VM bir sorun raporlama yapıp yapmadığını belirlemek için VM konsol günlükleri gözden geçirir. Tüm sanal makineleri önyükleme tanılaması etkin, sahip, bu nedenle sorun giderme Bu adım isteğe bağlı olabilir.
   
    Özel sorun giderme adımları bu makalenin kapsamı dışındadır, ancak RDP bağlantı etkileyen daha geniş bir sorunu gösterebilir. Konsol günlükleri ve VM ekran görüntüsünü gözden geçirme hakkında daha fazla bilgi için bkz: [VM'ler için önyükleme tanılaması](boot-diagnostics.md).

4. **VM için NIC sıfırlama**. Daha fazla bilgi için bkz: [Azure Windows VM için NIC sıfırlamaya](reset-network-interface.md).
5. **VM kaynak sistem durumu denetimi**. Bu sorun giderme adımı VM bağlantı etkileyebilir Azure platformu ile bilinen bir sorun doğrular.
   
    Azure portalında, VM'yi seçin. Ayarları bölmesine aşağı **destek + sorun giderme** listenin alt kısmına. Tıklatın **kaynak durumu** düğmesi. Sağlıklı bir VM raporları olarak **kullanılabilir**:
   
    ![Azure portalında VM'nin kaynak sistem durumu denetimi](./media/troubleshoot-rdp-connection/check-resource-health.png)
6. **Kullanıcı kimlik bilgilerini sıfırlama**. Bu sorun giderme adımı emin değilseniz veya kimlik bilgilerini unutmuş bir yerel yönetici hesabının parolasını sıfırlar.
   
    Azure portalında, VM'yi seçin. Ayarları bölmesine aşağı **destek + sorun giderme** listenin alt kısmına. Tıklatın **parola sıfırlama** düğmesi. Emin olun **modu** ayarlanır **parola sıfırlama** ve kullanıcı adınızı ve yeni bir parola girin. Son olarak, tıklatın **güncelleştirme** düğmesi:
   
    ![Azure portalında kullanıcı kimlik bilgilerini sıfırlama](./media/troubleshoot-rdp-connection/reset-password.png)
7. **VM'yi yeniden**. Bu sorun giderme adımı VM sahip temel sorunları düzeltebilir.
   
    Azure portalında, VM seçin ve tıklatın **genel bakış** sekmesi. Tıklatın **yeniden** düğmesi:
   
    ![Azure portalında VM yeniden başlatma](./media/troubleshoot-rdp-connection/restart-vm.png)
8. **VM'yi yeniden**. Bu sorun giderme adımı herhangi bir temel platform ya da ağ sorunları düzeltmek için Azure içindeki başka bir ana bilgisayara VM'yi yeniden dağıtır.
   
    Azure portalında, VM'yi seçin. Ayarları bölmesine aşağı **destek + sorun giderme** listenin alt kısmına. Tıklatın **dağıtmanız** düğmesine tıklayın ve ardından **dağıtmanız**:
   
    ![Azure portalında VM yeniden dağıtın](./media/troubleshoot-rdp-connection/redeploy-vm.png)
   
    Bu işlem tamamlandıktan sonra kısa ömürlü disk veriler kaybolur ve VM ile ilişkili olan dinamik IP adreslerini güncelleştirilir.

Hala RDP sorunlarla karşılaşıyorsanız, şunları yapabilirsiniz [bir destek isteği açın](https://azure.microsoft.com/support/options/) veya okuma [kavramlar ve adımlar sorun giderme RDP ayrıntılı](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-using-azure-powershell"></a>Azure PowerShell kullanarak sorun giderme
Henüz yapmadıysanız [en son Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

Aşağıdaki örnekler değişkenler gibi kullandığınız `myResourceGroup`, `myVM`, ve `myVMAccessExtension`. Bu değişken adları ve konumları kendi değerlerinizle değiştirin.

> [!NOTE]
> Kullanarak kullanıcı kimlik bilgilerini ve RDP yapılandırması sıfırlama [kümesi AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) PowerShell cmdlet'i. Aşağıdaki örneklerde, `myVMAccessExtension` işleminin bir parçası belirttiğiniz bir addır. VMAccessAgent ile önceden çalıştıysa, varolan uzantısının adını kullanarak alabileceğiniz `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` VM özelliklerini denetlemek için. Adını görüntülemek için Çıkış 'Uzantılar' bölümüne bakın.

Sorun giderme her adımdan sonra VM yeniden bağlanmayı deneyin. Hala bağlanamıyorsanız, sonraki adıma deneyin.

1. **RDP bağlantınızı sıfırlama**. Sorun giderme Bu adım, uzak bağlantıları devre dışı bırakıldığında veya Windows Güvenlik duvarı kurallarını RDP, örneğin engelliyor RDP yapılandırması sıfırlar.
   
    Adlı bir VM üzerinde RDP bağlantısı izleyin örnek sıfırlar `myVM` içinde `WestUS` konumu ve adlı kaynak grubunda `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```
2. **Doğrulama ağ güvenlik grubu kuralları**. Bu sorun giderme adımı ağ güvenlik RDP trafiğine izin vermek için grubunuzdaki bir kural sahip olduğunu doğrular. RDP için varsayılan bağlantı noktası 3389 numaralı TCP bağlantı noktası var. VM'nizi oluşturduğunuzda RDP trafiğine izin verme kuralı otomatik olarak oluşturulabilir değil.
   
    İlk olarak, ağ güvenlik grubu için tüm yapılandırma verilerini atayın `$rules` değişkeni. Aşağıdaki örnek adlı ağ güvenlik grubu hakkında bilgi edinir `myNetworkSecurityGroup` kaynak grubunda adlı `myResourceGroup`:
   
    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```
   
    Şimdi, bu ağ güvenlik grubu için yapılandırılmış olan kuralları görüntüleyin. Gelen bağlantılar için TCP bağlantı noktası 3389 şu şekilde izin vermek için bir kuralı var olduğundan emin olun:
   
    ```powershell
    $rules.SecurityRules
    ```
   
    Aşağıdaki örnek RDP trafiğine izin verir. geçerli güvenlik kuralı gösterir. Gördüğünüz `Protocol`, `DestinationPortRange`, `Access`, ve `Direction` doğru şekilde yapılandırılır:
   
    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```
   
    RDP trafiğine izin veren bir kural yoksa [ağ güvenlik grubu kural oluşturma](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). TCP bağlantı noktası 3389 izin verin.
3. **Kullanıcı kimlik bilgilerini sıfırlama**. Bu sorun giderme adımı emin değilseniz, ya da unutulursa, kimlik bilgileri, belirttiğiniz yerel yönetici hesabının parolasını sıfırlar.
   
    İlk olarak, kimlik bilgilerini atayarak kullanıcı adı ve yeni bir parola belirtin `$cred` şekilde değişkeni:
   
    ```powershell
    $cred=Get-Credential
    ```
   
    Şimdi, VM'yi kimlik bilgilerini güncelleştirin. Aşağıdaki örnek adlı VM üzerinde kimlik bilgilerini güncelleştirir `myVM` içinde `WestUS` konumu ve adlı kaynak grubunda `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```
4. **VM'yi yeniden**. Bu sorun giderme adımı VM sahip temel sorunları düzeltebilir.
   
    Aşağıdaki örnek adlı VM yeniden `myVM` kaynak grubunda adlı `myResourceGroup`:
   
    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```
5. **VM'yi yeniden**. Bu sorun giderme adımı herhangi bir temel platform ya da ağ sorunları düzeltmek için Azure içindeki başka bir ana bilgisayara VM'yi yeniden dağıtır.
   
    Aşağıdaki örnek adlı VM yeniden dağıtır `myVM` içinde `WestUS` konumu ve adlı kaynak grubunda `myResourceGroup`:
   
    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Hala RDP sorunlarla karşılaşıyorsanız, şunları yapabilirsiniz [bir destek isteği açın](https://azure.microsoft.com/support/options/) veya okuma [kavramlar ve adımlar sorun giderme RDP ayrıntılı](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Klasik dağıtım modeli kullanılarak oluşturulan sanal makineleri sorun giderme
Sorun giderme her adımdan sonra VM yeniden bağlanmayı deneyin.

1. **RDP bağlantınızı sıfırlama**. Sorun giderme Bu adım, uzak bağlantıları devre dışı bırakıldığında veya Windows Güvenlik duvarı kurallarını RDP, örneğin engelliyor RDP yapılandırması sıfırlar.
   
    Azure portalında, VM'yi seçin. Tıklatın **... Daha fazla** düğmesine ve ardından **sıfırlama uzaktan erişim**:
   
    ![Azure portalında RDP yapılandırması sıfırlandı](./media/troubleshoot-rdp-connection/classic-reset-rdp.png)
2. **Bulut Hizmetleri bitiş noktalarını doğrulama**. Bu sorun giderme adımı, bulut hizmetlerinizde RDP trafiğine izin vermek için uç noktalar sahip olduğunu doğrular. RDP için varsayılan bağlantı noktası 3389 numaralı TCP bağlantı noktası var. VM'nizi oluşturduğunuzda RDP trafiğine izin verme kuralı otomatik olarak oluşturulabilir değil.
   
   Azure portalında, VM'yi seçin. Tıklatın **uç noktaları** VM için şu anda yapılandırılmış uç noktaları görüntülemek için düğmesi. Uç noktaları TCP bağlantı noktası 3389 üzerinde RDP trafiğine izin veren var olduğunu doğrulayın.
   
   Aşağıdaki örnek, RDP trafiğine izin vermek geçerli uç nokta gösterir:
   
   ![Azure portalında bulut Hizmetleri bitiş noktalarını doğrulama](./media/troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)
   
   RDP trafiğine izin veren bir uç nokta yoksa [bulut Hizmetleri uç noktası oluşturma](classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). TCP özel 3389 numaralı bağlantı noktasına izin verin.
3. **VM önyükleme tanılaması gözden**. Bu sorun giderme adımı VM bir sorun raporlama yapıp yapmadığını belirlemek için VM konsol günlükleri gözden geçirir. Tüm sanal makineleri önyükleme tanılaması etkin, sahip, bu nedenle sorun giderme Bu adım isteğe bağlı olabilir.
   
    Özel sorun giderme adımları bu makalenin kapsamı dışındadır, ancak RDP bağlantı etkileyen daha geniş bir sorunu gösterebilir. Konsol günlükleri ve VM ekran görüntüsünü gözden geçirme hakkında daha fazla bilgi için bkz: [VM'ler için önyükleme tanılaması](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).
4. **VM kaynak sistem durumu denetimi**. Bu sorun giderme adımı VM bağlantı etkileyebilir Azure platformu ile bilinen bir sorun doğrular.
   
    Azure portalında, VM'yi seçin. Ayarları bölmesine aşağı **destek + sorun giderme** listenin alt kısmına. Tıklatın **kaynak durumu** düğmesi. Sağlıklı bir VM raporları olarak **kullanılabilir**:
   
    ![Azure portalında VM'nin kaynak sistem durumu denetimi](./media/troubleshoot-rdp-connection/classic-check-resource-health.png)
5. **Kullanıcı kimlik bilgilerini sıfırlama**. Bu sorun giderme adımı emin değilseniz veya kimlik bilgilerini unuttuysanız, belirttiğiniz yerel yönetici hesabının parolasını sıfırlar.
   
    Azure portalında, VM'yi seçin. Ayarları bölmesine aşağı **destek + sorun giderme** listenin alt kısmına. Tıklatın **parola sıfırlama** düğmesi. Kullanıcı adınızı ve yeni bir parola girin. Son olarak, tıklatın **kaydetmek** düğmesi:
   
    ![Azure portalında kullanıcı kimlik bilgilerini sıfırlama](./media/troubleshoot-rdp-connection/classic-reset-password.png)
6. **VM'yi yeniden**. Bu sorun giderme adımı VM sahip temel sorunları düzeltebilir.
   
    Azure portalında, VM seçin ve tıklatın **genel bakış** sekmesi. Tıklatın **yeniden** düğmesi:
   
    ![Azure portalında VM yeniden başlatma](./media/troubleshoot-rdp-connection/classic-restart-vm.png)

Hala RDP sorunlarla karşılaşıyorsanız, şunları yapabilirsiniz [bir destek isteği açın](https://azure.microsoft.com/support/options/) veya okuma [kavramlar ve adımlar sorun giderme RDP ayrıntılı](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-specific-rdp-errors"></a>Özel RDP hatalarında sorun giderme
RDP aracılığıyla VM bağlanmaya çalışırken, belirli bir hata iletisi karşılaşabilirsiniz. En sık karşılaşılan hata iletileri şunlardır:

* [Uzak Masaüstü lisans sunucusu lisans sağlamak kullanılabilir olmadığından uzak oturum kesildi](troubleshoot-specific-rdp-errors.md#rdplicense).
* [Uzak Masaüstü bilgisayar "name" bulamıyor](troubleshoot-specific-rdp-errors.md#rdpname).
* [Bir kimlik doğrulama hatası oluştu. Yerel Güvenlik Yetkilisi kurulamıyor](troubleshoot-specific-rdp-errors.md#rdpauth).
* [Windows güvenlik hatası: kimlik bilgilerinizi çalışmama](troubleshoot-specific-rdp-errors.md#wincred).
* [Bu bilgisayar uzak bilgisayara bağlanamıyor](troubleshoot-specific-rdp-errors.md#rdpconnect).

## <a name="additional-resources"></a>Ek kaynaklar
Hiçbiri bu hatalar oluştu ve hala VM'ye Uzak Masaüstü bağlanamıyor, ayrıntılı okuma [Uzak Masaüstü için sorun giderme kılavuzu](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Bir VM üzerinde çalışan uygulamalar erişimde adımları sorun giderme için bkz: [bir Azure VM üzerinde çalışan bir uygulama için erişim sorunlarını giderme](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Güvenli Kabuk (SSH) kullanarak azure'da bir Linux VM bağlanmak için bkz: sorunları yaşıyorsanız [sorun giderme SSH bağlantıları azure'da bir Linux VM](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

