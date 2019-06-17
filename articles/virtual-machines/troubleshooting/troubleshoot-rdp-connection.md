---
title: Azure'da bir Windows sanal makinesi için RDP ile bağlantı kurulamıyor | Microsoft Docs
description: Windows sanal makinenize Uzak Masaüstü'nü kullanarak azure'daki bağlanamadığında sorunlarını giderme
keywords: Uzak Masaüstü hatası, Uzak Masaüstü bağlantısı hata, VM'ye bağlanamıyor Uzak Masaüstü sorunlarını giderme
services: virtual-machines-windows
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 0d740f8e-98b8-4e55-bb02-520f604f5b18
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 03/23/2018
ms.author: roiyz
ms.openlocfilehash: 50adab1eaa199473a8da857d38c3a08c424c677a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64728943"
---
# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Bir Azure sanal makinesine Uzak Masaüstü bağlantılarında sorun giderme
Windows tabanlı Azure sanal makinenize (VM) olan Uzak Masaüstü Protokolü (RDP) bağlantısı çeşitli sebeplerle başarısız olabilir ve VM'nize erişememenize yol açabilir. Bu sorun VM'deki Uzak Masaüstü hizmetinden, ağ bağlantısından veya ana bilgisayarınızdaki Uzak Masaüstü istemcisinden kaynaklanabilir. Bu makalede, RDP bağlantı sorunlarını gidermek için en sık kullanılan yöntemlerden bazıları size yol gösterir. 

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Hızlı sorun giderme adımları
Sorun giderme her adımdan sonra sanal Makineye yeniden bağlanmayı deneyin:

1. Uzak Masaüstü yapılandırmasını sıfırlayın.
2. Ağ güvenlik grubu denetleme kuralları / bulut Hizmetleri uç noktaları.
3. VM konsol günlüklerini gözden geçirin.
4. Sanal makine için NIC sıfırlayın.
5. VM kaynak durumunu denetleyin.
6. VM parolanızı sıfırlayın.
7. VM'nizi yeniden başlatın.
8. Sanal makinenizi yeniden dağıtın.

Daha ayrıntılı adımlar ve açıklamaları gerekiyorsa okumaya devam edin. Bu yerel ağ donanımı yönlendiriciler gibi doğrulayın ve güvenlik duvarları engellenmemesi giden TCP bağlantı noktası 3389, belirtilen [ayrıntılı sorun giderme senaryoları RDP](detailed-troubleshoot-rdp.md).

> [!TIP]
> Varsa **Connect** VM'nizi out portalda renkte görüntülenir ve aracılığıyla azure'a bağlanmayan düğmesine bir [Express Route](../../expressroute/expressroute-introduction.md) veya [siteden siteye VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) bağlantısı oluşturmanız gerekir ve RDP kullanmadan önce sanal Makinenizin genel IP adresi atayın. [Azure’da genel IP adresleri](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) hakkında daha fazlasını okuyabilirsiniz.


## <a name="ways-to-troubleshoot-rdp-issues"></a>RDP sorunlarını gidermenin yolları
Aşağıdaki yöntemlerden birini kullanarak Resource Manager dağıtım modeli kullanılarak oluşturulan VM'ler giderebilirsiniz:

* Azure portalı - yüklü Azure Araçları, RDP yapılandırması veya kullanıcı kimlik bilgilerini ve hızlı bir şekilde sıfırlamanız gerekirse harika yok.
* Bir PowerShell İstemi ile kullanabiliyorsanız, azure PowerShell - hızlı bir şekilde Azure PowerShell cmdlet'lerini kullanarak RDP yapılandırması veya kullanıcı kimlik bilgilerini sıfırlayın.

Kullanılarak oluşturulan VM'ler sorun giderme adımları da bulabilirsiniz [Klasik dağıtım modelini](#troubleshoot-vms-created-using-the-classic-deployment-model).

<a id="fix-common-remote-desktop-errors"></a>

## <a name="troubleshoot-using-the-azure-portal"></a>Azure portalını kullanarak sorun giderme
Sorun giderme her adımdan sonra sanal makinenizde yeniden bağlanmayı deneyin. Yine de bağlanamıyorsanız, sonraki adıma deneyin.

1. **RDP bağlantısını sıfırlama**. Bu sorun giderme adımı RDP yapılandırması uzaktan bağlantıların devre dışı bırakıldığı veya Windows Güvenlik duvarı kurallarının RDP'yi, örneğin engellediği sıfırlar.
   
    Azure portalında VM'nizi seçin. Ayarlar bölmesini aşağı **destek + sorun giderme** listenin kısmına. Tıklayın **parolayı Sıfırla** düğmesi. Ayarlama **modu** için **yalnızca Yapılandırmayı Sıfırla** ve ardından **güncelleştirme** düğmesi:
   
    ![Azure portalında RDP yapılandırmasını Sıfırla](./media/troubleshoot-rdp-connection/reset-rdp.png)
2. **Doğrulama ağ güvenlik grubu kurallarını**. [IP akışı doğrulamayı](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) kullanarak Ağ Güvenlik Grubu’ndaki bir kuralın bir sanal makineye giden veya gelen trafiği engelleyip engellemediğini doğrulayın. Ayrıca, gelen "izin ver" NSG emin olmak için etkin güvenlik grubu kurallarını gözden geçirebilirsiniz kuralının mevcut olduğundan ve RDP bağlantı noktası (varsayılan olarak 3389) için önceliklendirildiğinden. Daha fazla bilgi için [kullanılarak geçerli güvenlik VM sorunlarını gidermek için kuralları trafik akışı](../../virtual-network/diagnose-network-traffic-filter-problem.md).

3. **VM önyükleme tanılaması gözden**. Bu sorun giderme adımı, VM bir sorunu bildirip bildirmediğini belirlemek için VM konsol günlüklerini inceler. Bu sorun giderme adımı isteğe bağlı olacak şekilde tüm VM'ler önyükleme tanılaması etkin, sahiptir.
   
    Özel sorun giderme adımları bu makalenin kapsamı dışındadır, ancak RDP bağlantısını etkileyen daha geniş bir sorunu gösterebilir. Konsol günlükleri ve VM ekran gözden geçirme hakkında daha fazla bilgi için bkz: [VM'ler için önyükleme tanılaması](boot-diagnostics.md).

4. **VM için NIC'yi sıfırlama**. Daha fazla bilgi için [nasıl Azure Windows VM için NIC'yi sıfırlama](../windows/reset-network-interface.md).
5. **VM kaynak durumunu denetleme**. Bu sorun giderme adımı sanal makine bağlantısını etkileyebilir Azure platformu ile birlikte bilinen bir sorun olmadığından doğrular.
   
    Azure portalında VM'nizi seçin. Ayarlar bölmesini aşağı **destek + sorun giderme** listenin kısmına. Tıklayın **kaynak durumu** düğmesi. Sağlıklı bir VM raporlar olarak **kullanılabilir**:
   
    ![Azure portalında sanal makine kaynak durumunu denetleme](./media/troubleshoot-rdp-connection/check-resource-health.png)
6. **Kullanıcı kimlik bilgilerini sıfırlama**. Emin değilseniz ya da kimlik bilgilerini unuttuysanız, bu sorun giderme adımı bir yerel yönetici hesabının parolasını sıfırlar.  VM'de oturum açtıktan sonra bu kullanıcının parolasını sıfırlamalısınız.
   
    Azure portalında VM'nizi seçin. Ayarlar bölmesini aşağı **destek + sorun giderme** listenin kısmına. Tıklayın **parolayı Sıfırla** düğmesi. Emin **modu** ayarlanır **parolayı Sıfırla** ve kullanıcı adınızı ve yeni bir parola girin. Son olarak, tıklayın **güncelleştirme** düğmesi:
   
    ![Azure Portalı'nda kullanıcı kimlik bilgilerini Sıfırla](./media/troubleshoot-rdp-connection/reset-password.png)
7. **Sanal makinenizin yeniden**. Bu sorun giderme adımı, temel alınan VM sahip herhangi bir sorunu düzeltebilirsiniz.
   
    Azure portalında VM'nizi seçin ve tıklayın **genel bakış** sekmesi. Tıklayın **yeniden** düğmesi:
   
    ![Azure portalında VM'yi yeniden başlatın](./media/troubleshoot-rdp-connection/restart-vm.png)
8. **Sanal makinenizi yeniden dağıtın**. Bu sorun giderme adımı, temel alınan herhangi bir platform veya ağ sorunları düzeltmek için Azure içinde başka bir konağa sanal makinenizin yeniden dağıtır.
   
    Azure portalında VM'nizi seçin. Ayarlar bölmesini aşağı **destek + sorun giderme** listenin kısmına. Tıklayın **yeniden** düğmesine ve ardından **yeniden**:
   
    ![Azure portalında bir VM'yi yeniden dağıtma](./media/troubleshoot-rdp-connection/redeploy-vm.png)
   
    Bu işlem tamamlandıktan sonra kısa ömürlü disk verileri kaybedilirse ve VM ile ilişkili olan dinamik IP adresleri güncelleştirilir.

9. **Yönlendirmeyi doğrulayın**. Ağ İzleyicisi'nin [sonraki atlama](../../network-watcher/network-watcher-check-next-hop-portal.md) yönlendiriliyor veya bir sanal makineden bir yol trafiği engelleyen değil olduğunu onaylamak için yeteneği. Ayrıca, bir ağ arabirimi için tüm geçerli rotalar görmek için geçerli rotalar gözden geçirebilirsiniz. Daha fazla bilgi için [VM sorunlarını gidermek için geçerli rotaları kullanma trafik akışı](../../virtual-network/diagnose-network-routing-problem.md).

10. Tüm şirket içi güvenlik duvarı veya bilgisayarınızda, güvenlik duvarı azure'a giden TCP 3389 trafiğe izin verdiğinden emin olun.

Hala RDP sorunlarla karşılaşıyorsanız, yapabilecekleriniz [bir destek isteği açın](https://azure.microsoft.com/support/options/) veya okuma [kavramlar ve adımlar sorun giderme RDP ayrıntılı](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-using-azure-powershell"></a>Azure PowerShell kullanarak sorun giderme
Henüz kaydolmadıysanız [en son Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

Aşağıdaki değişkenleri gibi örneklerde `myResourceGroup`, `myVM`, ve `myVMAccessExtension`. Bu değişken adları ve konumları, kendi değerlerinizle değiştirin.

> [!NOTE]
> Kullanarak kullanıcı kimlik bilgilerini ve RDP yapılandırmasını sıfırlama [kümesi AzVMAccessExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmaccessextension) PowerShell cmdlet'i. Aşağıdaki örneklerde, `myVMAccessExtension` işleminin bir parçası belirttiğiniz bir addır. Daha önce VMAccessAgent ile çalıştıysanız, mevcut uzantı adı kullanarak alabilirsiniz `Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"` VM özelliklerini denetlemek için. Adını görüntülemek için çıkış altında 'Uzantılar' bölümüne bakın.

Sorun giderme her adımdan sonra sanal makinenizde yeniden bağlanmayı deneyin. Yine de bağlanamıyorsanız, sonraki adıma deneyin.

1. **RDP bağlantısını sıfırlama**. Bu sorun giderme adımı RDP yapılandırması uzaktan bağlantıların devre dışı bırakıldığı veya Windows Güvenlik duvarı kurallarının RDP'yi, örneğin engellediği sıfırlar.
   
    Aşağıdaki örnekte adlı bir VM'de RDP bağlantısı sıfırlar `myVM` içinde `WestUS` konumu ve adlı kaynak grubunda `myResourceGroup`:
   
    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```
2. **Doğrulama ağ güvenlik grubu kurallarını**. Bu sorun giderme adımı, ağ güvenlik RDP trafiğine izin vermek için grubunuza bir kural sahip olduğunu doğrular. RDP için varsayılan bağlantı noktası TCP bağlantı noktası 3389 ' dir. RDP trafiğine izin veren bir kural, VM oluşturduğunuzda otomatik olarak oluşturulmayabilir.
   
    İlk olarak, tüm yapılandırma verilerini, ağ güvenlik grubuna atayın `$rules` değişkeni. Aşağıdaki örnekte adlı ağ güvenlik grubu hakkında bilgi edinir `myNetworkSecurityGroup` adlı kaynak grubunda `myResourceGroup`:
   
    ```powershell
    $rules = Get-AzNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```
   
    Şimdi, bu ağ güvenlik grubu için yapılandırılan kuralları görüntüleyin. Bir kural gibi TCP bağlantı noktası 3389 için gelen bağlantılara izin verecek şekilde var olduğundan emin olun:
   
    ```powershell
    $rules.SecurityRules
    ```
   
    Aşağıdaki örnek, RDP trafiğine izin veren bir geçerli güvenlik kuralı gösterir. Gördüğünüz `Protocol`, `DestinationPortRange`, `Access`, ve `Direction` doğru şekilde yapılandırılır:
   
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
   
    RDP trafiğine izin verecek bir kural yoksa [bir ağ güvenlik grubu kuralı oluşturma](../windows/nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). TCP bağlantı noktası 3389 izin verin.
3. **Kullanıcı kimlik bilgilerini sıfırlama**. Bu sorun giderme adımı, değilseniz ya da unutulursa, kimlik bilgileri, belirttiğiniz yerel yönetici hesabının parolasını sıfırlar.
   
    İlk olarak, kimlik bilgilerini atayarak kullanıcı adı ve yeni bir parola belirtin `$cred` değişkeni aşağıdaki gibi:
   
    ```powershell
    $cred=Get-Credential
    ```
   
    Şimdi vm'nizde kimlik bilgilerini güncelleştirin. Aşağıdaki örnekte adlı bir VM üzerinde kimlik bilgilerini güncelleştirir `myVM` içinde `WestUS` konumu ve adlı kaynak grubunda `myResourceGroup`:
   
    ```powershell
    Set-AzVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```
4. **Sanal makinenizin yeniden**. Bu sorun giderme adımı, temel alınan VM sahip herhangi bir sorunu düzeltebilirsiniz.
   
    Aşağıdaki örnekte adlı VM yeniden `myVM` adlı kaynak grubunda `myResourceGroup`:
   
    ```powershell
    Restart-AzVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```
5. **Sanal makinenizi yeniden dağıtın**. Bu sorun giderme adımı, temel alınan herhangi bir platform veya ağ sorunları düzeltmek için Azure içinde başka bir konağa sanal makinenizin yeniden dağıtır.
   
    Aşağıdaki örnekte adlı VM yeniden dağıtır `myVM` içinde `WestUS` konumu ve adlı kaynak grubunda `myResourceGroup`:
   
    ```powershell
    Set-AzVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

6. **Yönlendirmeyi doğrulayın**. Ağ İzleyicisi'nin [sonraki atlama](../../network-watcher/network-watcher-check-next-hop-portal.md) yönlendiriliyor veya bir sanal makineden bir yol trafiği engelleyen değil olduğunu onaylamak için yeteneği. Ayrıca, bir ağ arabirimi için tüm geçerli rotalar görmek için geçerli rotalar gözden geçirebilirsiniz. Daha fazla bilgi için [VM sorunlarını gidermek için geçerli rotaları kullanma trafik akışı](../../virtual-network/diagnose-network-routing-problem.md).

7. Tüm şirket içi güvenlik duvarı veya bilgisayarınızda, güvenlik duvarı azure'a giden TCP 3389 trafiğe izin verdiğinden emin olun.

Hala RDP sorunlarla karşılaşıyorsanız, yapabilecekleriniz [bir destek isteği açın](https://azure.microsoft.com/support/options/) veya okuma [kavramlar ve adımlar sorun giderme RDP ayrıntılı](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Klasik dağıtım modeli kullanılarak oluşturulan VM'ler sorunlarını giderme
Sorun giderme her adımdan sonra sanal Makineye yeniden bağlanmayı deneyin.

1. **RDP bağlantısını sıfırlama**. Bu sorun giderme adımı RDP yapılandırması uzaktan bağlantıların devre dışı bırakıldığı veya Windows Güvenlik duvarı kurallarının RDP'yi, örneğin engellediği sıfırlar.
   
    Azure portalında VM'nizi seçin. Tıklayın **... Daha fazla** düğmesine ve ardından tıklayın **uzaktan erişimi Sıfırla**:
   
    ![Azure portalında RDP yapılandırmasını Sıfırla](./media/troubleshoot-rdp-connection/classic-reset-rdp.png)
2. **Bulut Hizmetleri bitiş noktalarını doğrulama**. Bu sorun giderme adımı, bulut hizmetlerinizi RDP trafiğine izin vermek için uç noktalarına sahip olduğunu doğrular. RDP için varsayılan bağlantı noktası TCP bağlantı noktası 3389 ' dir. RDP trafiğine izin veren bir kural, VM oluşturduğunuzda otomatik olarak oluşturulmayabilir.
   
   Azure portalında VM'nizi seçin. Tıklayın **uç noktaları** sanal makinenizin şu anda yapılandırılmış uç noktaları görüntülemek için düğme. Uç noktaları 3389 numaralı TCP bağlantı noktasında RDP trafiğine izin veren mevcut olduğunu doğrulayın.
   
   Aşağıdaki örnek, RDP trafiğine izin veren geçerli uç noktaları gösterir:
   
   ![Azure portalında cloud Services bitiş noktalarını doğrulama](./media/troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)
   
   RDP trafiğine izin veren bir uç nokta yoksa [Cloud Services uç noktası oluşturma](../windows/classic/setup-endpoints.md). TCP özel 3389 numaralı bağlantı noktasına izin verin.
3. **VM önyükleme tanılaması gözden**. Bu sorun giderme adımı, VM bir sorunu bildirip bildirmediğini belirlemek için VM konsol günlüklerini inceler. Bu sorun giderme adımı isteğe bağlı olacak şekilde tüm VM'ler önyükleme tanılaması etkin, sahiptir.
   
    Özel sorun giderme adımları bu makalenin kapsamı dışındadır, ancak RDP bağlantısını etkileyen daha geniş bir sorunu gösterebilir. Konsol günlükleri ve VM ekran gözden geçirme hakkında daha fazla bilgi için bkz: [VM'ler için önyükleme tanılaması](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).
4. **VM kaynak durumunu denetleme**. Bu sorun giderme adımı sanal makine bağlantısını etkileyebilir Azure platformu ile birlikte bilinen bir sorun olmadığından doğrular.
   
    Azure portalında VM'nizi seçin. Ayarlar bölmesini aşağı **destek + sorun giderme** listenin kısmına. Tıklayın **kaynak durumu** düğmesi. Sağlıklı bir VM raporlar olarak **kullanılabilir**:
   
    ![Azure portalında sanal makine kaynak durumunu denetleme](./media/troubleshoot-rdp-connection/classic-check-resource-health.png)
5. **Kullanıcı kimlik bilgilerini sıfırlama**. Bu sorun giderme adımı emin değilseniz ya da kimlik bilgilerini unuttuysanız, belirttiğiniz yerel yönetici hesabının parolasını sıfırlar.  VM'de oturum açtıktan sonra bu kullanıcının parolasını sıfırlamalısınız.
   
    Azure portalında VM'nizi seçin. Ayarlar bölmesini aşağı **destek + sorun giderme** listenin kısmına. Tıklayın **parolayı Sıfırla** düğmesi. Kullanıcı adınızı ve yeni bir parola girin. Son olarak, tıklayın **Kaydet** düğmesi:
   
    ![Azure Portalı'nda kullanıcı kimlik bilgilerini Sıfırla](./media/troubleshoot-rdp-connection/classic-reset-password.png)
6. **Sanal makinenizin yeniden**. Bu sorun giderme adımı, temel alınan VM sahip herhangi bir sorunu düzeltebilirsiniz.
   
    Azure portalında VM'nizi seçin ve tıklayın **genel bakış** sekmesi. Tıklayın **yeniden** düğmesi:
   
    ![Azure portalında VM'yi yeniden başlatın](./media/troubleshoot-rdp-connection/classic-restart-vm.png)

7. Tüm şirket içi güvenlik duvarı veya bilgisayarınızda, güvenlik duvarı azure'a giden TCP 3389 trafiğe izin verdiğinden emin olun.

Hala RDP sorunlarla karşılaşıyorsanız, yapabilecekleriniz [bir destek isteği açın](https://azure.microsoft.com/support/options/) veya okuma [kavramlar ve adımlar sorun giderme RDP ayrıntılı](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="troubleshoot-specific-rdp-errors"></a>Belirli RDP sorunlarını giderme
Sanal makinenize RDP aracılığıyla bağlanmaya çalışırken bir özel hata iletisiyle karşılaşabilirsiniz. En sık hata iletileri şunlardır:

* [Lisans sağlanabilecek Uzak Masaüstü lisans sunucusu olmadığından uzak oturumun bağlantısı kesildi](troubleshoot-specific-rdp-errors.md#rdplicense).
* [Uzak Masaüstü bilgisayar "name" bulamıyor](troubleshoot-specific-rdp-errors.md#rdpname).
* [Bir kimlik doğrulama hatası oluştu. Yerel Güvenlik Yetkilisi temas kurulamıyor](troubleshoot-specific-rdp-errors.md#rdpauth).
* [Windows güvenlik hatası: Kimlik bilgilerinizi çalışmama](troubleshoot-specific-rdp-errors.md#wincred).
* [Bu bilgisayar, uzak bilgisayara bağlanamıyor](troubleshoot-specific-rdp-errors.md#rdpconnect).

## <a name="additional-resources"></a>Ek kaynaklar
Hiçbiri şu hata oluştu ve yine Uzak Masaüstü aracılığıyla sanal makineye bağlanamıyorsanız, ayrıntılı okuma [Uzak Masaüstü için sorun giderme kılavuzu](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Bir VM'de çalışan uygulamalara erişme adımlar sorun giderme için bkz: [bir Azure sanal makinesinde çalışan bir uygulamaya erişim sorunlarını giderme](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Güvenli Kabuk (SSH) kullanarak azure'da bir Linux VM bağlanmak için bkz sorunları yaşıyorsanız [sorun giderme SSH bağlantıları için azure'da bir Linux sanal makinesi](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


