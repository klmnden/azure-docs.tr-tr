---
title: "Kopya etki alanı denetleyicilerini şirket içi Active Directory etki alanı için Azure sanal makinelere yüklemeniz | Microsoft Docs"
description: "Çoğaltma DC'leri Azure sanal makinelerde (VM'ler) şirket içi Active Directory etki alanı için bir Azure sanal ağında yükleme."
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 8c9ebf1b-289a-4dd6-9567-a946450005c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: f4e64fbc6c2fda026297b69bd54471d49b6785a1
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Bir Azure sanal ağındaki bir çoğaltma Active Directory etki alanı denetleyicisi yükleme
Bu makalede, bir Azure sanal ağında nasıl ek etki alanı denetleyicileri (DC'ler) Azure sanal makinelerde (VM'ler) bir şirket içi Active Directory etki alanı DC'leri çoğaltma olarak kullanılacak yükleneceği anlatılmaktadır. Ayrıca [bir Azure sanal ağ üzerinde bir Windows Server Active Directory ormanı yüklemek](active-directory-new-forest-virtual-machine.md). Bir Azure sanal ağında Active Directory etki alanı Hizmetleri (AD DS) yüklemek için bkz: nasıl [yönergeleri dağıtma Windows Server Active Directory için Azure sanal makineler üzerinde](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Senaryo diyagramı
Bu senaryoda, dış kullanıcılar etki alanına katılmış sunucularda çalışan uygulamalara erişmek için gereklidir. Sanal makineleri, uygulama sunucusu çalıştırabilir ve çoğaltma DC'leri bir Azure sanal ağında yüklenir. Sanal ağ tarafından şirket ağına bağlı [ExpressRoute](../expressroute/expressroute-locations-providers.md), veya kullanabileceğiniz bir [siteden siteye VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) gösterildiği gibi bağlantı: 

![Bir Azure sanal PF çoğaltma Active Directory etki alanı denetleyicisi diyagram][1]

Uygulama sunucuları ve DC'leri işlem işleme dağıtmak için ayrı cloud services dahilindeki ve geliştirilmiş hataya dayanıklılık için kullanılabilirlik kümesi içinde dağıtılır.
DC'ler birbirleriyle ve şirket içi DC'leri ile Active Directory çoğaltmasını kullanarak çoğaltabilir. Hiçbir eşitleme araçları gereklidir.

## <a name="create-an-on-premises-active-directory-site-for-the-azure-virtual-network"></a>Azure sanal ağı için bir şirket içi Active Directory sitesi oluşturun
Active Directory'de sanal ağa karşılık gelen ağ bölgesini temsil eden bir site oluşturun. Bu site, kimlik doğrulama, çoğaltma ve diğer DC Konum işlemleri en iyi duruma getirme yardımcı olabilir. Aşağıdaki adımlar bir site oluşturun ve daha fazla arka plan için bkz: açıklamaktadır [yeni bir Site Ekleme](https://technet.microsoft.com/library/cc781496.aspx).

1. Active Directory Siteleri ve Hizmetleri'ni açmak: **Sunucu Yöneticisi'ni** > **Araçları** > **Active Directory Siteleri ve Hizmetleri**.
2. Bir Azure sanal ağı oluşturulduğu bölgesini temsil etmek için bir site oluşturun: tıklatın **siteleri** > **eylem** > **yeni site** > türü Azure ABD Batı gibi yeni sitenin adı > bir site bağlantısı seçin > **Tamam**.
3. Bir alt ağ oluşturun ve yeni site ile ilişkilendirin: çift **siteleri** > sağ tıklayın **alt ağlar** > **yeni bir alt ağ** > IP adres aralığı yazın sanal ağa (örneğin, 10.1.0.0/16 senaryo diyagramdaki) > Yeni Azure siteyi seçin > **Tamam**.

## <a name="create-an-azure-virtual-network"></a>Bir Azure sanal ağ oluşturma
Bir Azure sanal ağı oluşturmak ve siteden siteye VPN ayarlamak için bu makalede bulunan adımları [bir siteden siteye bağlantı oluşturmak](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md). 

Güvenli bir siteden siteye VPN bağlantısı oluşturmak için sanal ağ geçidi de yapılandırabilirsiniz. Yeni bir sanal ağ ve bir şirket içi VPN cihazı arasında siteden siteye VPN bağlantısı oluşturun. Yönergeler için bkz: [bir sanal ağ geçidi yapılandırma](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="create-azure-vms-for-the-dc-roles"></a>Azure VM'ler için DC rolleri oluşturun
DC rolü barındırmak için sanal makineleri oluşturmak için adımları yineleyin [Azure portalı ile Windows sanal makine oluşturma](../virtual-machines/windows/quick-create-portal.md) gerektiğinde. Hataya dayanıklılık ve artıklık sağlamak için en az iki sanal DC'leri dağıtın. Azure sanal ağı benzer şekilde yapılandırılmış en az iki DC'leri varsa, bu DC'leri kullanılabilirlik kümesi geliştirilmiş hataya dayanıklılık için çalışan sanal makineleri yerleştirebilirsiniz.

Azure portal yerine Windows PowerShell kullanarak sanal makineleri oluşturmak için bkz: [oluşturmak ve Windows tabanlı sanal makineleri önceden için kullanım Azure PowerShell](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Statik bir IP adresi DC rolü çalıştıracak VM'ler için ayırın. Bir statik IP adresini ayırmak için Microsoft Web Platformu yükleyicisi indirin ve [Azure PowerShell'i yükleme](/powershell/azure/overview) ve Set-AzureStaticVNetIP cmdlet'ini çalıştırın. Örneğin:

````
Get-AzureVM -ServiceName AzureDC1 -Name AzureDC1 | Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureVM
````
Statik bir IP adresi ayarlama hakkında daha fazla bilgi için bkz: [statik iç IP adresi için bir VM yapılandırma](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Azure VM'ler üzerinde AD DS'yi yüklemek
Bir VM için oturum açın ve, kaynak siteden siteye VPN veya ExpressRoute bağlantısı üzerinden şirket içi ağınızda bağlantınız olduğunu doğrulayın. Daha sonra AD DS Azure Vm'lerinde yükleyin. Ek DC, şirket içi ağınızda (kullanıcı Arabirimi, Windows PowerShell veya bir yanıt dosyası) yüklemek için kullandığınız işlemin aynısını kullanabilirsiniz. AD DS'yi yüklemek gibi yeni birim AD veritabanı, günlükler ve SYSVOL konumu için belirttiğinizden emin olun. AD DS yüklemesi üzerinde Yenileyici gerekirse bkz [Active Directory etki alanı hizmetlerini yükleme (düzey 100)](https://technet.microsoft.com/library/hh472162.aspx) veya [varolan bir etki alanında (Düzey 200) kopya Windows Server 2012 etki alanı denetleyicisi yükleme](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>Sanal ağ için DNS sunucusunu yeniden yapılandırın
1. Sanal ağ adlarının bir listesini buna almak için [Azure portal](https://portal.azure.com), arama *sanal ağlar*seçeneğini belirleyip **sanal ağlar** listesini görmek için. 
2. Yönetmek istediğiniz sanal ağ açın ve ardından [sanal ağınızın DNS sunucusu IP adreslerini yeniden](../virtual-network/manage-virtual-network.md#change-dns-servers) şirket içi DNS sunucuları için IP adresi yerine DC'leri çoğaltma atanan statik IP adreslerini kullanmak için.
3. Üzerindeki tüm çoğaltma DC VM'ler emin olmak için sanal ağ ile sanal ağdaki DNS sunucularını kullanacak şekilde yapılandırılmış:
  1. Seçin **sanal makineleri**.
  2. Sanal makineleri seçin ve ardından **yeniden**. 
  3. VM olana kadar bekleyin **çalıştıran** yeniden ve ardından içine oturum açın.

## <a name="create-vms-for-application-servers"></a>Uygulama sunucuları için sanal makineleri oluşturma

Uygulama sunucusu rolünü barındırmak için sanal makineleri oluşturmak için adımları yineleyin [Azure portalı ile Windows sanal makine oluşturma](../virtual-machines/windows/quick-create-portal.md) gerektiğinde. Azure portal yerine Microsoft PowerShell kullanarak sanal makineleri oluşturmak için bkz: [oluşturmak ve Windows tabanlı sanal makineleri yapılandırmak için kullanım Azure PowerShell](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Aşağıdaki tabloda, önerilen ayarları içerir.

| Ayar | Değerler |
| --- | --- |
|  **Bir görüntü seçin** | Windows Server 2012 R2 Datacenter |
|  **Sanal Makine Yapılandırması** |<p>Sanal makine adı: (örneğin, AppServer1) tek etiketli bir ad yazın.</p><p>Yeni bir kullanıcı adı: bir kullanıcının adını yazın. Bu kullanıcı VM yerel Yöneticiler grubunun bir üyesi olacaktır. Bu ad VM ilk kez oturum açmak için gerekir. Yönetici adlı yerleşik hesap çalışmaz.</p><p>Yeni Parola/Onayla: bir parola yazın</p> |
|  **Sanal Makine Yapılandırması** |<p>Bulut hizmeti: Seçin **yeni bir bulut hizmeti oluşturma** seçin ve ilk VM için daha fazla sanal makineleri oluşturduğunuzda, bulut hizmet adı aynı uygulamayı barındıracak.</p><p>Bulut hizmeti DNS adı: genel olarak benzersiz bir ad belirtin</p><p>Bölge/benzeşim grubu/sanal ağ: sanal ağ adı (örneğin, WestUSVNet) belirtin.</p><p>Depolama hesabı: Seçin **otomatik olarak oluşturulan depolama hesabı kullan** seçin ve ilk VM için daha fazla sanal makineleri oluştururken aynı depolama hesabı adı uygulamayı barındıracak.</p><p>Kullanılabilirlik kümesi: Seçin **bir kullanılabilirlik kümesi oluştur**.</p><p>Kullanılabilirlik kümesi adı: ilk VM oluşturup ardından daha fazla sanal makineleri oluşturduğunuzda aynı ad kullanılabilirlik kümesi için bir ad yazın.</p> |
|  **Sanal Makine Yapılandırması** |<p>Seçin <b>VM Aracısı yükleme</b> ve gereksinim duyduğunuz diğer uzantılar.</p> |
  
Her VM sağlandıktan sonra oturum açın ve etki alanına katılın. 
1. İçinde **Sunucu Yöneticisi'ni** &gt; **yerel sunucu** &gt; **çalışma grubu** &gt; **Değiştir...** seçin **etki alanı**.
2. Şirket içi etki alanınızın adını girin. 
3. Bir etki alanı kullanıcı kimlik bilgilerini sağlayın.
4. VM’yi yeniden başlatın.

## <a name="additional-resources"></a>Ek kaynaklar

* Windows PowerShell'i kullanma hakkında daha fazla bilgi için bkz: [Azure cmdlet'leri ile çalışmaya başlama](/powershell/azure/overview) ve [Azure Cmdlet başvurusu](/powershell/azure/get-started-azureps).
* [Windows Server Active Directory, Azure sanal makinelerde dağıtmak için yönergeler](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Mevcut şirket içi Hyper-V etki alanı denetleyicileri için Azure Azure PowerShell kullanarak nasıl yükleneceğini](http://support.microsoft.com/kb/2904015)
* [Bir Azure sanal ağ üzerinde yeni bir Active Directory ormanı yüklemek](active-directory-new-forest-virtual-machine.md)
* [Azure Sanal Ağ](../virtual-network/virtual-networks-overview.md)
* [Microsoft Azure BT Uzmanı Iaas: (01) sanal makine temelleri](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure BT Uzmanı Iaas: oluşturma (05) sanal ağlar ve şirket içi bağlantılar](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure yönetim cmdlet'leri](/powershell/module/azurerm.compute/#virtual_machines)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
