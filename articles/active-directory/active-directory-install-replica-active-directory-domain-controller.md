---
title: "Azure'da bir çoğaltma Active Directory etki alanı denetleyicisi yükleme | Microsoft Docs"
description: "Bir Azure sanal makinesinde bir şirket içi Active Directory ormanındaki bir etki alanı denetleyicisi yüklemek açıklanmaktadır Öğreticisi."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8c9ebf1b-289a-4dd6-9567-a946450005c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: fb0bacac346445e6bde9df22f3355419e3162a3c
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Bir Azure sanal ağındaki bir çoğaltma Active Directory etki alanı denetleyicisi yükleme
Bu konu, bir Azure sanal ağı Azure sanal makinelerle (VM'ler) şirket içi Active Directory etki alanı için ek etki alanı denetleyicileri (olarak da bilinen çoğaltma DC'ler) yüklemek nasıl gösterir.

> [!IMPORTANT]
> Microsoft, Azure AD’yi bu makalede bahsedilen Klasik Azure Portalı yerine Azure portalındaki [Azure AD yönetim merkezini](https://aad.portal.azure.com) kullanarak yönetmenizi öneriyor.

Bu ilgili konularda ilginizi çekebilir:

* Bu gibi durumlarda, yeni bir Active Directory ormanı isteğe bağlı olarak bir Azure sanal ağ üzerinde yükleyebilirsiniz. Bu adımlar için bkz: [bir Azure sanal ağ üzerinde yeni bir Active Directory ormanı yüklemek](active-directory-new-forest-virtual-machine.md).
* Active Directory etki alanı Hizmetleri (AD DS) bir Azure sanal ağ üzerinde yükleme ilgili kavramsal kılavuzlar için bkz: [yönergeleri dağıtma Windows Server Active Directory için Azure sanal makineler üzerinde](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Senaryo diyagramı
Bu senaryoda, dış kullanıcılar etki alanına katılmış sunucularda çalışan uygulamalara erişmek için gereklidir. Sanal makineleri, uygulama sunucusu çalıştırabilir ve çoğaltma DC'leri bir Azure sanal ağında yüklenir. Sanal ağ tarafından şirket ağına bağlı bir [siteden siteye VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) Aşağıdaki diyagramda veya, gösterildiği gibi bağlantı kullanabileceğiniz [ExpressRoute](../expressroute/expressroute-locations-providers.md) daha hızlı bağlantı.

Uygulama sunucuları ve DC'leri işlem işleme dağıtmak için ayrı cloud services dahilindeki ve içinde dağıtılan [kullanılabilirlik kümeleri](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) geliştirilmiş hataya dayanıklılık için.
DC'ler birbirleriyle ve şirket içi DC'leri ile Active Directory çoğaltmasını kullanarak çoğaltabilir. Hiçbir eşitleme araçları gereklidir.

![Bir Azure sanal PF çoğaltma Active Directory etki alanı denetleyicisi diyagram][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Azure sanal ağı için bir Active Directory sitesi oluşturun
Active Directory'de sanal ağa karşılık gelen ağ bölgesini temsil eden bir site oluşturmak için iyi bir fikirdir. Kimlik doğrulama, çoğaltma ve diğer DC Konum işlemleri en iyi duruma getirme yardımcı olur. Aşağıdaki adımlar bir site oluşturun ve daha fazla arka plan için bkz: açıklamaktadır [yeni bir Site Ekleme](https://technet.microsoft.com/library/cc781496.aspx).

1. Active Directory Siteleri ve Hizmetleri'ni açmak: **Sunucu Yöneticisi'ni** > **Araçları** > **Active Directory Siteleri ve Hizmetleri**.
2. Bir Azure sanal ağı oluşturulduğu bölgesini temsil etmek için bir site oluşturun: tıklatın **siteleri** > **eylem** > **yeni site** > türü Azure ABD Batı gibi yeni sitenin adı > bir site bağlantısı seçin > **Tamam**.
3. Bir alt ağ oluşturun ve yeni site ile ilişkilendirin: çift **siteleri** > sağ tıklayın **alt ağlar** > **yeni bir alt ağ** > IP adres aralığı yazın sanal ağa (örneğin, 10.1.0.0/16 senaryo diyagramdaki) > Yeni Azure siteyi seçin > **Tamam**.

## <a name="create-an-azure-virtual-network"></a>Bir Azure sanal ağ oluşturma
1. İçinde [Klasik Azure portalı](https://manage.windowsazure.com), tıklatın **yeni** > **Ağ Hizmetleri** > **sanal ağ**  >  **Özel Oluştur** ve Sihirbazı tamamlamak için aşağıdaki değerleri kullanın.

   | Bu sihirbaz sayfasında... | Bu değerleri belirtin |
   | --- | --- |
   |  **Sanal ağ ayrıntıları** |<p>Ad: WestUSVNet gibi sanal ağ için bir ad yazın.</p><p>Bölge: en yakın bölgeyi seçin.</p> |
   |  **DNS ve VPN bağlantısı** |<p>DNS sunucuları: ad ve bir veya daha fazla şirket içi DNS sunucusunun IP adresini belirtin.</p><p>Bağlantı: Seçin **siteden siteye VPN bağlantısını yapılandırma**.</p><p>Yerel ağ: yeni bir yerel ağ belirtin.</p><p>ExpressRoute yerine bir VPN kullanıyorsanız bkz [bir ExpressRoute bağlantı Exchange sağlayıcısı aracılığıyla yapılandırmayı](../expressroute/expressroute-locations-providers.md).</p> |
   |  **Siteden siteye bağlantı** |<p>Ad: şirket içi ağ için bir ad yazın.</p><p>VPN cihazı IP adresi: sanal ağa bağlanacak aygıt ortak IP adresini belirtin. VPN cihazı bir NAT’nin arkasına yerleştirilemez.</p><p>Adres: şirket içi ağınız (örneğin, senaryo diyagramı 192.168.0.0/16) adres aralıklarını belirtin.</p> |
   |  **Sanal ağ adres alanları** |<p>Adres alanı: Azure sanal ağı (örneğin, 10.1.0.0/16 senaryo diyagramdaki) çalıştırmak istediğiniz VM'ler için IP adresi aralığı belirtin. Bu adres aralığı, şirket içi ağ adresi aralıklarıyla çakışamaz.</p><p>Alt ağları: bir ad ve uygulama sunucuları için bir alt ağ adresi belirtin (gibi ön uç, 10.1.1.0/24) ve DC'ler için (arka uç gibi 10.1.2.0/24).</p><p>Tıklatın **ağ geçidi alt ağı eklemek**.</p> |
2. Ardından, güvenli bir siteden siteye VPN bağlantısı oluşturmak için sanal ağ geçidini yapılandıracaksınız. Bkz: [bir sanal ağ geçidi yapılandırma](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) yönergeleri için.
3. Yeni bir sanal ağ ve bir şirket içi VPN cihazı arasında siteden siteye VPN bağlantısı oluşturun. Bkz: [bir sanal ağ geçidi yapılandırma](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) yönergeleri için.

## <a name="create-azure-vms-for-the-dc-roles"></a>Azure VM'ler için DC rolleri oluşturun
Gerektiğinde DC rolü barındırmak için sanal makineleri oluşturmak için aşağıdaki adımları yineleyin. Hataya dayanıklılık ve artıklık sağlamak için en az iki sanal DC'leri dağıtmanız gerekir. Azure sanal ağı benzer şekilde yapılandırılmış en az iki DC'leri içeriyorsa (diğer bir deyişle, çalışma DNS sunucusu, her iki GC'ler oldukları ve ikisi herhangi bir FSMO rolüne vb. tutan) olanlar DC'leri kullanılabilirlik kümesi geliştirilmiş hataya dayanıklılık için çalışan sanal makineleri yerleştirin.
Kullanıcı Arabirimi yerine Windows PowerShell kullanarak sanal makineleri oluşturmak için bkz: [oluşturmak ve Windows tabanlı sanal makineleri önceden için kullanım Azure PowerShell](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

1. İçinde [Klasik Azure portalı](https://manage.windowsazure.com), tıklatın **yeni** > **işlem** > **sanal makine**  >  **Galerisinden**. Sihirbazı tamamlamak için aşağıdaki değerleri kullanın. Başka bir değer önerilen ya da gerekli olmadıkça bir ayar için varsayılan değeri kabul edin.

   | Bu sihirbaz sayfasında... | Bu değerleri belirtin |
   | --- | --- |
   |  **Bir görüntü seçin** |Windows Server 2012 R2 Datacenter |
   |  **Sanal Makine Yapılandırması** |<p>Sanal makine adı: (örneğin, AzureDC1) tek etiketli bir ad yazın.</p><p>Yeni bir kullanıcı adı: bir kullanıcının adını yazın. Bu kullanıcı VM yerel Yöneticiler grubunun bir üyesi olacaktır. Bu ad VM ilk kez oturum açmak için gerekir. Yönetici adlı yerleşik hesap çalışmaz.</p><p>Yeni Parola/Onayla: bir parola yazın</p> |
   |  **Sanal Makine Yapılandırması** |<p>Bulut hizmeti: Seçin <b>yeni bir bulut hizmeti oluşturma</b> seçin ve ilk VM için daha fazla sanal makineleri oluşturduğunuzda, bulut hizmet adı aynı DC rolünü barındıracak.</p><p>Bulut hizmeti DNS adı: genel olarak benzersiz bir ad belirtin</p><p>Bölge/benzeşim grubu/sanal ağ: sanal ağ adı (örneğin, WestUSVNet) belirtin.</p><p>Depolama hesabı: Seçin <b>otomatik olarak oluşturulan depolama hesabı kullan</b> seçin ve ilk VM için daha fazla sanal makineleri oluştururken aynı depolama hesabı adı DC rolünü barındıracak.</p><p>Kullanılabilirlik kümesi: Seçin <b>bir kullanılabilirlik kümesi oluştur</b>.</p><p>Kullanılabilirlik kümesi adı: ilk VM oluşturup ardından daha fazla sanal makineleri oluşturduğunuzda aynı ad kullanılabilirlik kümesi için bir ad yazın.</p> |
   |  **Sanal Makine Yapılandırması** |<p>Seçin <b>VM Aracısı yükleme</b> ve gereksinim duyduğunuz diğer uzantılar.</p> |
2. DC sunucusu rolünü çalıştıracak her VM'ye bir disk ekleyin. Ek disk AD veritabanı, günlükler ve SYSVOL depolamak için gereklidir. (Örneğin, 10 GB) disk için bir boyut belirtin ve bırakın **konak önbelleği tercihi** kümesine **hiçbiri**. Adımları için bkz: [bir Windows sanal makineye bir veri diski Ekle nasıl](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. VM ilk kez oturum açtığınızda sonra açmak **Sunucu Yöneticisi'ni** > **dosya ve depolama hizmetleri** birim NTFS kullanılarak bu diskte oluşturmak için.
4. Statik bir IP adresi DC rolü çalıştıracak VM'ler için ayırın. Bir statik IP adresini ayırmak için Microsoft Web Platformu yükleyicisi indirin ve [Azure PowerShell'i yükleme](/powershell/azure/overview) ve Set-AzureStaticVNetIP cmdlet'ini çalıştırın. Örneğin:

    ' Get-AzureVM - ServiceName AzureDC1-ad AzureDC1 | Set-AzureStaticVNetIP - IPADDRESS 10.0.0.4 | Update-AzureVM

Statik bir IP adresi ayarlama hakkında daha fazla bilgi için bkz: [statik iç IP adresi için bir VM yapılandırma](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Azure VM'ler üzerinde AD DS'yi yüklemek
Bir VM için oturum açın ve, kaynak siteden siteye VPN veya ExpressRoute bağlantısı üzerinden şirket içi ağınızda bağlantınız olduğunu doğrulayın. Daha sonra AD DS Azure Vm'lerinde yükleyin. Ek DC, şirket içi ağınızda (kullanıcı Arabirimi, Windows PowerShell veya bir yanıt dosyası) yüklemek için kullandığınız işlemin aynısını kullanabilirsiniz. AD DS'yi yüklemek gibi yeni birim AD veritabanı, günlükler ve SYSVOL konumu için belirttiğinizden emin olun. AD DS yüklemesi üzerinde Yenileyici gerekirse bkz [Active Directory etki alanı hizmetlerini yükleme (düzey 100)](https://technet.microsoft.com/library/hh472162.aspx) veya [varolan bir etki alanında (Düzey 200) kopya Windows Server 2012 etki alanı denetleyicisi yükleme](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>Sanal ağ için DNS sunucusunu yeniden yapılandırın
1. İçinde [Azure portal](https://portal.azure.com), **arama kaynakları** kutusuna *sanal ağlar*, ardından **sanal ağları (Klasik)** içinde Arama sonuçları. Sanal ağ adına tıklayın ve ardından [sanal ağınızın DNS sunucusu IP adreslerini yeniden](../virtual-network/virtual-network-manage-network.md#dns-servers) bir şirket içi DNS sunucularının IP adreslerini yerine DC'leri çoğaltma atanan statik IP adreslerini kullanmak için.
2. Sanal ağ üzerinde çoğaltma DC VM'ler ile sanal ağ DNS sunucularını kullanmak için yapılandırıldığından emin olmak için tıklatın **sanal makineleri**, her VM için Durum sütununda'ı tıklatın ve ardından **yeniden**. VM görüntüleyene kadar bekleyin **çalıştıran** içine oturum denemeden önce belirtin.

## <a name="create-vms-for-application-servers"></a>Uygulama sunucuları için sanal makineleri oluşturma
1. Uygulama sunucuları olarak çalıştırmak için VM'ler oluşturmak için aşağıdaki adımları yineleyin. Başka bir değer önerilen ya da gerekli olmadıkça bir ayar için varsayılan değeri kabul edin.

   | Bu sihirbaz sayfasında... | Bu değerleri belirtin |
   | --- | --- |
   |  **Bir görüntü seçin** |Windows Server 2012 R2 Datacenter |
   |  **Sanal Makine Yapılandırması** |<p>Sanal makine adı: (örneğin, AppServer1) tek etiketli bir ad yazın.</p><p>Yeni bir kullanıcı adı: bir kullanıcının adını yazın. Bu kullanıcı VM yerel Yöneticiler grubunun bir üyesi olacaktır. Bu ad VM ilk kez oturum açmak için gerekir. Yönetici adlı yerleşik hesap çalışmaz.</p><p>Yeni Parola/Onayla: bir parola yazın</p> |
   |  **Sanal Makine Yapılandırması** |<p>Bulut hizmeti: Seçin **yeni bir bulut hizmeti oluşturma** seçin ve ilk VM için daha fazla sanal makineleri oluşturduğunuzda, bulut hizmet adı aynı uygulamayı barındıracak.</p><p>Bulut hizmeti DNS adı: genel olarak benzersiz bir ad belirtin</p><p>Bölge/benzeşim grubu/sanal ağ: sanal ağ adı (örneğin, WestUSVNet) belirtin.</p><p>Depolama hesabı: Seçin **otomatik olarak oluşturulan depolama hesabı kullan** seçin ve ilk VM için daha fazla sanal makineleri oluştururken aynı depolama hesabı adı uygulamayı barındıracak.</p><p>Kullanılabilirlik kümesi: Seçin **bir kullanılabilirlik kümesi oluştur**.</p><p>Kullanılabilirlik kümesi adı: ilk VM oluşturup ardından daha fazla sanal makineleri oluşturduğunuzda aynı ad kullanılabilirlik kümesi için bir ad yazın.</p> |
   |  **Sanal Makine Yapılandırması** |<p>Seçin <b>VM Aracısı yükleme</b> ve gereksinim duyduğunuz diğer uzantılar.</p> |
2. Her VM sağlandıktan sonra oturum açın ve etki alanına katılın. İçinde **Sunucu Yöneticisi'ni**, tıklatın **yerel sunucu** > **çalışma grubu** > **Değiştir...** ve ardından **etki alanı** ve şirket içi etki alanınızın adını yazın. Bir etki alanı kullanıcı kimlik bilgilerini sağlayın ve etki alanına katılmayı tamamlamak için VM'yi yeniden başlatın.

Kullanıcı Arabirimi yerine Windows PowerShell kullanarak sanal makineleri oluşturmak için bkz: [oluşturmak ve Windows tabanlı sanal makineleri önceden için kullanım Azure PowerShell](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Windows PowerShell'i kullanma hakkında daha fazla bilgi için bkz: [Azure cmdlet'leri ile çalışmaya başlama](/powershell/azure/overview) ve [Azure Cmdlet başvurusu](/powershell/azure/get-started-azureps).

## <a name="additional-resources"></a>Ek kaynaklar
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
