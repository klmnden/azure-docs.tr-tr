---
title: "Bir Azure sanal ağ üzerinde bir Active Directory ormanı yükleme | Microsoft Docs"
description: "Bir sanal makine (VM) yeni bir Active Directory ormanı oluşturmak bir Azure sanal ağında açıklanmaktadır Öğreticisi."
services: active-directory, virtual-network
keywords: "Active directory sanal makine, yükleme active directory ormanı, azure active directory videolarını "
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
tags: 
ms.assetid: eb7170d0-266a-4caa-adce-1855589d65d1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2017
ms.author: joflore
<<<<<<< HEAD
ms.openlocfilehash: 18151f647b857dec78e659a3394359ff21a818c7
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
=======
ms.openlocfilehash: 23bea4b6e3351bdce77e6d265ba258ce60a22a36
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Bir Azure sanal ağ üzerinde yeni bir Active Directory ormanı yüklemek
Bu makalede, bir sanal makine (VM) üzerinde yeni bir Windows Server Active Directory ortamı oluşturmak gösterilmiştir bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md). Bu durumda, Azure sanal ağı bir şirket ağına bağlı değil.

Bu ilgili makalelerde ilginizi çekebilir:

* Bu adımları gösteren bir video için bkz [bir Azure sanal ağ üzerinde yeni bir Active Directory ormanı yükleme](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
* İsteğe bağlı olarak yapabileceğiniz [siteden siteye VPN bağlantısını yapılandırma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) ve ardından yeni bir orman yüklemek veya bir Azure sanal ağı şirket içi ormana genişletir. Bu adımlar için bkz: [bir Azure sanal ağında bir çoğaltma Active Directory etki alanı denetleyicisi yükleme](active-directory-install-replica-active-directory-domain-controller.md).
* Active Directory etki alanı Hizmetleri (AD DS) bir Azure sanal ağ üzerinde yükleme ilgili kavramsal kılavuzlar için bkz: [yönergeleri dağıtma Windows Server Active Directory için Azure sanal makineler üzerinde](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Senaryo diyagramı
Bu senaryoda, dış kullanıcılar etki alanına katılmış sunucularda çalışan uygulamalara erişmek için gereklidir. Uygulama sunucusu çalıştırabilir VM'ler ve etki alanı denetleyicileri çalışan sanal makineleri, kendi Azure sanal ağı bulut hizmetinde yüklü yüklenir. Ayrıca kullanılabilirlik geliştirilmiş hataya dayanıklılık için kümesi içinde dahil edilir.

![Azure sanal ağındaki sanal makinelerde Active Directory ormanı ][1] 7

## <a name="how-does-this-differ-from-on-premises"></a>Bu şirket içi nasıl farkı nedir?
Şirket içi Azure üzerinde bir etki alanı denetleyicisi yükleme arasındaki kadar fark değil. Aşağıdaki tabloda farklar listelenmektedir.

| Yapılandırmak için... | Şirket içi | Azure sanal ağı |
| --- | --- | --- |
| **Etki alanı denetleyicisi için IP adresi** |Ağ bağdaştırıcısı özelliklerinin üzerinde statik IP adresi atayın |Statik IP adresi atamak için Set-AzureStaticVNetIP cmdlet'ini çalıştırın |
| **DNS istemci çözümleyicisi** |Tercih edilen ve alternatif DNS sunucusu adresi ağ üzerinde etki alanı üyeleri bağdaştırıcı özelliklerini ayarlayın. |DNS sunucusu adresi kümesi sanal ağ özellikleri |
| **Active Directory veritabanı depolama** |İsteğe bağlı olarak C:\ varsayılan depolama konumunu değiştirme |C:\ varsayılan depolama konumunu değiştirmeniz gerekir |

## <a name="create-an-azure-virtual-network"></a>Bir Azure sanal ağ oluşturma
1. Azure Portal’da oturum açın.
2. Sanal ağ oluşturun. Tıklatın **ağlar** > **bir sanal ağ oluşturma**. Sihirbazı tamamlamak için aşağıdaki tabloda değerleri kullanın.

   | Bu sihirbaz sayfasında... | Bu değerleri belirtin |
   | --- | --- |
   |  **Sanal ağ ayrıntıları** |<p>Ad: sanal ağınız için bir ad girin</p><p>Bölge: en yakın bölgeyi seçin</p> |
   |  **DNS ve VPN** |<p>DNS sunucusu boş bırakın</p><p>Ya da VPN seçeneği kullanmayın</p> |
   |  **Sanal ağ adres alanları** |<p>Alt ağ adı: alt ağınız için bir ad girin</p><p>Başlangıç IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p> |

## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>Etki alanı denetleyicisi ve DNS sunucu rollerini çalıştıran VM'ler oluşturma
Gerektiğinde DC rolü barındırmak için sanal makineleri oluşturmak için aşağıdaki adımları yineleyin. Hataya dayanıklılık ve artıklık sağlamak için en az iki sanal DC'leri dağıtmanız gerekir. Azure sanal ağı benzer şekilde yapılandırılmış en az iki DC'leri içeriyorsa (diğer bir deyişle, çalışma DNS sunucusu, her iki GC'ler oldukları ve ikisi herhangi bir FSMO rolüne vb. tutan) olanlar DC'leri kullanılabilirlik kümesi geliştirilmiş hataya dayanıklılık için çalışan sanal makineleri yerleştirin.

Kullanıcı Arabirimi yerine Windows PowerShell kullanarak sanal makineleri oluşturmak için bkz: [oluşturmak ve Windows tabanlı sanal makineleri önceden için kullanım Azure PowerShell](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

1. Azure portalında seçin **yeni** > **işlem**ve ardından sanal makineyi seçin. Sihirbazı tamamlamak için aşağıdaki değerleri kullanın. Başka bir değer önerilen ya da gerekli olmadıkça bir ayar için varsayılan değeri kabul edin.

   | Bu sihirbaz sayfasında... | Bu değerleri belirtin |
   | --- | --- |
   |  **Bir görüntü seçin** |Windows Server 2012 R2 Datacenter |
   |  **Sanal Makine Yapılandırması** |<p>Sanal makine adı: (örneğin, AzureDC1) tek etiketli bir ad yazın.</p><p>Yeni bir kullanıcı adı: bir kullanıcının adını yazın. Bu kullanıcı VM yerel Yöneticiler grubunun bir üyesi olacaktır. Bu ad VM ilk kez oturum açmak için gerekir. Yönetici adlı yerleşik hesap çalışmaz.</p><p>Yeni Parola/Onayla: bir parola yazın</p> |
   |  **Sanal Makine Yapılandırması** |<p>Bulut hizmeti: Seçin <b>yeni bir bulut hizmeti oluşturma</b> seçin ve ilk VM için daha fazla sanal makineleri oluşturduğunuzda, bulut hizmet adı aynı DC rolünü barındıracak.</p><p>Bulut hizmeti DNS adı: genel olarak benzersiz bir ad belirtin</p><p>Bölge/benzeşim grubu/sanal ağ: sanal ağ adı (örneğin, WestUSVNet) belirtin.</p><p>Depolama hesabı: Seçin <b>otomatik olarak oluşturulan depolama hesabı kullan</b> seçin ve ilk VM için daha fazla sanal makineleri oluştururken aynı depolama hesabı adı DC rolünü barındıracak.</p><p>Kullanılabilirlik kümesi: Seçin <b>bir kullanılabilirlik kümesi oluştur</b>.</p><p>Kullanılabilirlik kümesi adı: ilk VM oluşturup ardından daha fazla sanal makineleri oluşturduğunuzda aynı ad kullanılabilirlik kümesi için bir ad yazın.</p> |
   |  **Sanal Makine Yapılandırması** |<p>Seçin <b>VM Aracısı yükleme</b> ve gereksinim duyduğunuz diğer uzantılar.</p> |
2. DC sunucusu rolünü çalıştıracak her VM'ye bir disk ekleyin. Ek disk AD veritabanı, günlükler ve SYSVOL depolamak için gereklidir. (Örneğin, 10 GB) disk için bir boyut belirtin ve bırakın **konak önbelleği tercihi** kümesine **hiçbiri**. Adımları için bkz: [bir Windows sanal makineye bir veri diski Ekle nasıl](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. VM ilk kez oturum açtığınızda sonra açmak **Sunucu Yöneticisi'ni** > **dosya ve depolama hizmetleri** birim NTFS kullanılarak bu diskte oluşturmak için.
4. Statik bir IP adresi DC rolü çalıştıracak VM'ler için ayırın. Bir statik IP adresini ayırmak için Microsoft Web Platformu yükleyicisi indirin ve [Azure PowerShell'i yükleme](/powershell/azure/overview) ve Set-AzureStaticVNetIP cmdlet'ini çalıştırın. Örneğin:

    `Get-AzureVM -ServiceName AzureDC1 -Name AzureDC1 | Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureVM`

Statik bir IP adresi ayarlama hakkında daha fazla bilgi için bkz: [statik iç IP adresi için bir VM yapılandırma](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-windows-server-active-directory"></a>Windows Server Active Directory yükleyin
Aynı yordamını kullanın [AD DS'yi yüklemek](https://technet.microsoft.com/library/jj574166.aspx) şirket içi kullanma (diğer bir deyişle, kullanıcı Arabirimi, bir yanıt dosyası ya da Windows PowerShell kullanabilirsiniz). Yeni bir orman yüklemek için yönetici kimlik bilgilerini sağlamanız gerekir. Active Directory veritabanı, günlükler ve SYSVOL için konumu belirtmek için VM'ye bağlı ek veri diski için işletim sistemi sürücüsünden varsayılan depolama konumunu değiştirebilirsiniz.

DC yükleme tamamlandıktan sonra VM yeniden bağlanın ve DC'ye oturum açın. Etki alanı kimlik bilgilerini belirtmek unutmayın.

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>Azure sanal ağı için DNS sunucusu Sıfırla
1. Yeni DC/DNS sunucusu DNS ileticisi ayarını sıfırlayın.
   1. Sunucu Yöneticisi'nde **Araçları** > **DNS**.
   2. İçinde **DNS Yöneticisi'ni**, DNS sunucusunun adını sağ tıklatın ve **özellikleri**.
   3. Üzerinde **İleticiler** sekmesinde iletici IP adresini ve öğesini tıklatın **Düzenle**.  IP adresini seçin ve'ı tıklatın **silmek**.
   4. Tıklatın **Tamam** Düzenleyicisi'ni kapatmak için ve **Tamam** DNS sunucusu özellikleri kapatın.
2. Sanal ağın DNS sunucusu ayarlarını güncelleştirin.
   1. Tıklatın **sanal ağlar** >, oluşturduğunuz sanal ağ çift tıklayın > **yapılandırma** > **DNS sunucuları**, adını ve IP VM'ler birinin yazın ' ı tıklatın ve DC/DNS sunucusu rolünü çalıştıran **kaydetmek**.
   2. VM seçin ve tıklatın **yeniden** yeni DNS sunucusu IP adresi ile DNS Çözümleyicisi ayarları yapılandırmak için VM tetiklemek için.

## <a name="create-vms-for-domain-members"></a>Etki alanı üyeleri için sanal makineleri oluşturma
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

## <a name="see-also"></a>Ayrıca Bkz.
* [Bir Azure sanal ağ üzerinde yeni bir Active Directory ormanı yükleme](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
* [Windows Server Active Directory, Azure sanal makinelerde dağıtmak için yönergeler](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Siteden siteye VPN bağlantısını yapılandırma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Bir Azure sanal ağındaki bir çoğaltma Active Directory etki alanı denetleyicisi yükleme](active-directory-install-replica-active-directory-domain-controller.md)
* [Microsoft Azure BT Uzmanı Iaas: (01) sanal makine temelleri](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure BT Uzmanı Iaas: oluşturma (05) sanal ağlar ve şirket içi bağlantılar](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Sanal ağ genel bakış](../virtual-network/virtual-networks-overview.md)
* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure Cmdlet Başvurusu](/powershell/azure/get-started-azureps)
* [Azure VM statik IP adresi kümesi](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
* [Azure VM için statik IP atama](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
* [Yeni bir Active Directory ormanı yüklemek](https://technet.microsoft.com/library/jj574166.aspx)
* [Active Directory etki alanı Hizmetleri (AD DS) sanallaştırma (düzey 100) giriş](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
