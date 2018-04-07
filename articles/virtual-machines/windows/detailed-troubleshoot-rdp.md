---
title: Azure'da sorun giderme ayrıntılı Uzak Masaüstü | Microsoft Docs
description: Azure'da Windows sanal makineler için ayrıntılı sorun giderme adımları için burada yapamazsınız Uzak Masaüstü hataları gözden geçirin
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: bağlanamıyor için Uzak Masaüstü, Uzak Masaüstü sorun giderme, Uzak Masaüstü bağlantı kuramıyor, Uzak Masaüstü hataları, Uzak Masaüstü sorunlarını giderme, Uzak Masaüstü sorunları
ms.assetid: 9da36f3d-30dd-44af-824b-8ce5ef07e5e0
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/06/2017
ms.author: genli
ms.openlocfilehash: 1485bc5ac7ae47df9a1a36c8b88d6515b5624360
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Windows Azure vm'lerinin ayrıntılı sorun giderme adımları için Uzak Masaüstü bağlantısı sorunları
Bu makalede tanılamak ve Windows tabanlı Azure sanal makineleri için karmaşık Uzak Masaüstü hataları düzeltmek için ayrıntılı sorun giderme adımları sağlar.

> [!IMPORTANT]
> Daha fazla ortak Uzak Masaüstü hataları ortadan kaldırmak için okuduğunuzdan emin olun [Uzak Masaüstü için temel sorun giderme makalesi](troubleshoot-rdp-connection.md) devam etmeden önce.

Karşılaşabileceğiniz belirli hata iletilerinin benzemez hata iletisi ele Uzak Masaüstü [temel Uzak Masaüstü sorun giderme kılavuzu](troubleshoot-rdp-connection.md). Uzak Masaüstü (RDP) istemci neden Azure VM'de RDP hizmete bağlanmada sorun olduğunu belirlemek için aşağıdaki adımları izleyin.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumlar](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) tıklatıp **destek alın**. Azure desteği hakkında daha fazla bilgi için okuma [Microsoft Azure desteği ile ilgili SSS](https://azure.microsoft.com/support/faq/).

## <a name="components-of-a-remote-desktop-connection"></a>Uzak Masaüstü bağlantısının bileşenleri
Aşağıdaki bileşenleri bir RDP bağlantısının oynayan:

![](./media/detailed-troubleshoot-rdp/tshootrdp_0.png)

Devam etmeden önce VM için son başarılı Uzak Masaüstü Bağlantısı beri değiştirilen şirketine yönelik gözden yardımcı olabilir. Örneğin:

* VM veya VM içeren bulut hizmeti genel IP adresi (sanal IP adresi olarak da bilinir [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) değişti. DNS istemci önbelleği hala RDP hatası olabilir *eski IP adresi* için DNS adını kayıtlı. DNS istemci önbelleğini temizlemek ve VM yeniden bağlanmayı deneyin. Veya doğrudan yeni VIP ile bağlanmayı deneyin.
* Azure portal tarafından oluşturulan bağlantı kullanmak yerine, Uzak Masaüstü bağlantıları yönetmek için bir üçüncü taraf uygulama kullanıyor. Uygulama yapılandırması doğru TCP bağlantı noktası Uzak Masaüstü trafiği içerdiğini doğrulayın. Bu bağlantı noktası bir Klasik sanal makine için kontrol edebilirsiniz [Azure portal](https://portal.azure.com), sanal makinenin ayarlarını tıklayarak > uç noktaları.

## <a name="preliminary-steps"></a>Başlangıç adımları
Ayrıntılı sorun giderme devam etmeden önce

* Azure portalında belirgin sorunlar için sanal makine durumunu denetleyin.
* İzleyin [hızlı düzeltme adımları temel sorun giderme Kılavuzu'nda sık karşılaşılan RDP hataları](troubleshoot-rdp-connection.md#quick-troubleshooting-steps).
* Özel resimler için VHD karşıya yüklemek için önceki doğru şekilde hazırlandığından emin olun. Daha fazla bilgi için bkz: [Windows VHD veya VHDX Azure'a yüklemeniz hazırlama](prepare-for-upload-vhd-image.md).


Uzak Masaüstü VM adımları sonra yeniden bağlanmayı deneyin.

## <a name="detailed-troubleshooting-steps"></a>Ayrıntılı sorun giderme adımları
Uzak masaüstü istemcisini Azure VM'de aşağıdaki kaynaklardan sorunu nedeniyle Uzak Masaüstü hizmete erişmek mümkün olmayabilir:

* [Uzak Masaüstü istemcisi bilgisayar](#source-1-remote-desktop-client-computer)
* [Kuruluş intranet sınır cihazı](#source-2-organization-intranet-edge-device)
* [Bulut Hizmeti uç noktası ve erişim denetimi listesi (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Ağ güvenlik grupları](#source-4-network-security-groups)
* [Windows tabanlı Azure VM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>Kaynak 1: Uzak Masaüstü istemcisi bilgisayar
Bilgisayarınızı başka bir şirket içi, Windows tabanlı bir bilgisayarda Uzak Masaüstü bağlantıları yapabilir doğrulayın.

![](./media/detailed-troubleshoot-rdp/tshootrdp_1.png)

Şunları yapamazsınız bilgisayarınızda aşağıdaki ayarları için kontrol edin:

* Uzak Masaüstü trafiği engelleyen bir yerel güvenlik duvarı ayarı.
* Yerel olarak Uzak Masaüstü bağlantıları engelliyor istemci ara yazılımı yüklü.
* Yerel ağ Uzak Masaüstü bağlantıları engelliyor yazılım izleme yüklü.
* Trafiği izlemek ya da belirli Uzak Masaüstü bağlantıları engelliyor trafik türlerine izin ver/engellemek diğer güvenlik yazılım türleri.

Tüm bu durumlarda, geçici olarak yazılımını devre dışı bırakma ve Uzak Masaüstü üzerinden bir şirket içi bilgisayara bağlanmayı deneyin. Bu şekilde gerçek nedenini bulabilirsiniz, Uzak Masaüstü bağlantılarına izin verecek şekilde yazılım ayarları düzeltmek için ağ yöneticinizle birlikte çalışır.

## <a name="source-2-organization-intranet-edge-device"></a>2. kaynak: Kuruluş intranet sınır cihazı
Doğrudan Internet'e bağlı bir bilgisayar, Azure sanal makinesi için Uzak Masaüstü bağlantıları yapabilir doğrulayın.

![](./media/detailed-troubleshoot-rdp/tshootrdp_2.png)

Doğrudan Internet'e bağlı bir bilgisayar yoksa oluşturun ve yeni bir Azure sanal makine bir kaynak grubu veya Bulut hizmetinde sınayın. Daha fazla bilgi için bkz: [Windows Azure üzerinde çalışan bir sanal makine oluşturma](../virtual-machines-windows-hero-tutorial.md). Sanal makine ve kaynak grubu veya Bulut hizmeti test sonra silebilirsiniz.

Doğrudan Internet'e bağlı olan bir bilgisayarla bir Uzak Masaüstü bağlantısı oluşturup oluşturamayacağını kuruluş intranet kenar cihazınız için denetleyin:

* HTTPS bağlantılarını engelleyen bir iç güvenlik duvarı.
* Uzak Masaüstü bağlantıları engelleyen bir proxy sunucu.
* Yetkisiz erişim algılama veya ağ kenar ağınızdaki Uzak Masaüstü bağlantıları engelliyor cihazlarda çalışan yazılım izleme.

HTTPS tabanlı Uzak Masaüstü bağlantılarını izin vermek için kuruluş intranet kenar Cihazınızı ayarlarını düzeltmek için ağ yöneticinizle birlikte çalışın.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>3. kaynak: Bulut Hizmeti uç noktası ve ACL
Klasik dağıtım modeli kullanılarak oluşturulan VM'ler için başka aynı bulut hizmeti ya da sanal ağ Azure VM Uzak Masaüstü bağlantıları için Azure VM'yi yapabileceğiniz doğrulayın.

![](./media/detailed-troubleshoot-rdp/tshootrdp_3.png)

> [!NOTE]
> Kaynak Yöneticisi'nde oluşturulan sanal makineler için geçin [kaynak 4: ağ güvenlik grupları](#source-4-network-security-groups).

Başka bir sanal makineyi aynı bulut hizmetinde veya sanal ağ yoksa bir tane oluşturun. Adımları [Windows Azure üzerinde çalışan bir sanal makine oluşturma](../virtual-machines-windows-hero-tutorial.md). Test tamamlandıktan sonra test sanal makinesi silin.

Aynı bulut hizmeti ya da sanal ağ içindeki bir sanal makineye Uzak Masaüstü aracılığıyla bağlanmak için bu ayarları kontrol edin:

* Uzak Masaüstü trafiği VM hedefte için uç nokta yapılandırması: özel TCP bağlantı noktası bitiş noktasının VM'ın Uzak Masaüstü hizmetini dinlerken TCP bağlantı noktası eşleşmesi gerekir (varsayılan olarak 3389).
* VM hedefte Uzak Masaüstü trafiği uç noktası için ACL: ACL belirtmenize olanak tanır izin verilen veya kaynak IP adresine göre Internet'ten gelen trafiği reddedildi. Yanlış yapılandırılmış ACL'ler uç noktasına gelen Uzak Masaüstü trafiğini engelleyebilirsiniz. Bu gelen trafiğin proxy, ortak IP adreslerinden emin olmak için ACL denetleyin veya başka bir uç sunucusu izin verilir. Daha fazla bilgi için bkz: [bir ağ erişim denetimi listesi (ACL) nedir?](../../virtual-network/virtual-networks-acl.md)

Uç nokta sorunun kaynağı olup olmadığını denetlemek için geçerli uç nokta kaldırın ve dış bağlantı noktası numarası için 49152-65535 aralığında bir rastgele bağlantı noktası seçerek yeni bir tane oluşturun. Daha fazla bilgi için bkz: [uç noktaları bir sanal makine için ayarlamak üzere nasıl](classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="source-4-network-security-groups"></a>4. kaynak: Ağ güvenlik grupları
Ağ güvenlik grupları, izin verilen gelen ve giden trafik daha ayrıntılı denetim sağlar. Alt ağlar kapsayıcı kurallar oluşturabilir ve bir Azure sanal ağı Hizmetleri'nde bulut.

[IP akışı doğrulamayı](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) kullanarak Ağ Güvenlik Grubu’ndaki bir kuralın bir sanal makineye giden veya gelen trafiği engelleyip engellemediğini doğrulayın. Gelen "izin ver" NSG emin olmak için etkili güvenlik grubu kuralları gözden geçirebilirsiniz kuralı var ve RDP bağlantı noktası (varsayılan 3389) öncelik. Daha fazla bilgi için bkz: [kullanarak etkili güvenlik VM gidermek için kuralları trafiğinin akmasını](../../virtual-network/virtual-network-nsg-troubleshoot-portal.md#using-effective-security-rules-to-troubleshoot-vm-traffic-flow).

## <a name="source-5-windows-based-azure-vm"></a>Kaynak 5: Windows tabanlı Azure VM
![](./media/detailed-troubleshoot-rdp/tshootrdp_5.png)

' Ndaki yönergeleri izleyin [bu makalede](reset-rdp.md). Bu makalede, sanal makinede Uzak Masaüstü hizmetini sıfırlar:

* "Uzak Masaüstü" Windows Güvenlik Duvarı varsayılan kuralı (TCP bağlantı noktası 3389) etkinleştirin.
* Uzak Masaüstü bağlantıları HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections kayıt defteri değerini 0 olarak ayarlayarak etkinleştirin.

Bilgisayarınızdan bağlantıyı yeniden deneyin. Uzak Masaüstü aracılığıyla bağlanma hala erişilemiyor ilgili aşağıdaki olası sorunları kontrol edin:

* Uzak Masaüstü hizmetini hedef VM çalışmıyor.
* Uzak Masaüstü hizmetini 3389 numaralı TCP bağlantı noktasında dinleme yapmıyor.
* Windows Güvenlik Duvarı veya başka bir yerel güvenlik duvarı Uzak Masaüstü trafiği engelleyen bir giden kuralı vardır.
* Yetkisiz erişim algılama veya ağ Azure sanal makinede çalışan yazılımı izleme Uzak Masaüstü bağlantıları engelliyor.

Klasik dağıtım modeli kullanılarak oluşturulan VM'ler için Azure sanal makine uzak Azure PowerShell oturumu kullanabilirsiniz. İlk olarak, sanal makinenin barındırma bulut hizmeti için bir sertifika yüklemeniz gerekir. Git [yapılandırma güvenli uzaktan PowerShell erişim için Azure sanal makineleri](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) ve indirme **InstallWinRMCertAzureVM.ps1** komut dosyası yerel bilgisayarınızda.

Ardından, henüz yapmadıysanız Azure PowerShell'i yükleyin. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

Ardından, bir Azure PowerShell komut istemi açın ve geçerli klasör konumuna değiştirme **InstallWinRMCertAzureVM.ps1** komut dosyası. Bir Azure PowerShell betiğini çalıştırmak için doğru yürütme ilkesini ayarlamanız gerekir. Çalıştırma **Get-ExecutionPolicy** komutu, geçerli ilke düzeyini belirleyin. Uygun düzeyini ayarlama hakkında daha fazla bilgi için bkz: [Set-ExecutionPolicy](https://technet.microsoft.com/library/hh849812.aspx).

Ardından, Azure aboneliği adınızı, bulut hizmet adı ve sanal makine adı doldurun (kaldırma < ve > karakterleri), ve ardından aşağıdaki komutları çalıştırın.

```powershell
$subscr="<Name of your Azure subscription>"
$serviceName="<Name of the cloud service that contains the target virtual machine>"
$vmName="<Name of the target virtual machine>"
.\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName
```

Doğru Abonelik adından alabilirsiniz *varlığıyla SubscriptionName* görüntüsünü özelliğinin **Get-AzureSubscription** komutu. Sanal makineden bulut hizmeti adını alabilir *ServiceName* görüntüsünü sütununda **Get-AzureVM** komutu.

Yeni sertifika olup olmadığını denetleyin. Geçerli kullanıcı ile bir sertifika ek bileşenini açın ve konum **güvenilen kök sertifika Authorities\Certificates** klasör. Bulut hizmetinizin çıkarılan sütununda DNS adına sahip bir sertifika görmeniz gerekir (örnek: cloudservice4testing.cloudapp.net).

Ardından, bu komutları kullanarak uzak bir Azure PowerShell oturumu başlatın.

```powershell
$uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
$creds = Get-Credential
Enter-PSSession -ConnectionUri $uri -Credential $creds
```

Geçerli yönetici kimlik bilgileri girdikten sonra aşağıdaki Azure PowerShell istemi benzer bir şey görmeniz gerekir:

```powershell
[cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>
```

Bu istem ilk bölümü hedef VM, hangi "cloudservice4testing.cloudapp.net" farklı olabilir içeren bulut hizmeti adınızdır. Artık Azure komutlar sorunları araştırmak bu bulut hizmeti için belirtilen ve yapılandırmasını düzeltmek PowerShell uygulayabilirsiniz.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>TCP bağlantı noktasını dinlemeyi Uzak Masaüstü Hizmetleri el ile düzeltmek için
Uzaktan Azure PowerShell oturumu isteminde bu komutu çalıştırın.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

PortNumber özelliği geçerli bağlantı noktası numarasını gösterir. Gerekli olursa, Uzak Masaüstü'nü değiştirmek bu komutu kullanarak bağlantı noktası numarası geri varsayılan değerine (3389).

```powershell
Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389
```

Bu komutu kullanarak bağlantı noktası 3389 için değiştirilmiş doğrulayın.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

Bu komutu kullanarak uzaktan Azure PowerShell oturumu çıkın.

```powershell
Exit-PSSession
```

Uzak Masaüstü uç nokta Azure VM için de TCP bağlantı noktası 3398 kendi iç bağlantı noktası olarak kullandığından emin olun. Azure VM yeniden başlatın ve Uzak Masaüstü bağlantısı yeniden deneyin.

## <a name="additional-resources"></a>Ek kaynaklar
[Bir parola veya Windows sanal makineler için Uzak Masaüstü hizmetini sıfırlama](reset-rdp.md)

[Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)

[Linux tabanlı bir Azure sanal makine için güvenli Kabuk (SSH) bağlantı sorunlarını giderme](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Azure sanal makinesinde çalışan bir uygulamaya erişim sorunlarını giderme](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

