---
title: Azure'da sorun giderme ayrıntılı Uzak Masaüstü | Microsoft Docs
description: Azure'da Windows sanal makineleri için ayrıntılı sorun giderme adımları için erişemezsiniz iletisinin nereden Uzak Masaüstü hataları gözden geçirin
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: bağlantı kurulamıyor Uzak Masaüstü, Uzak Masaüstü sorun giderin, Uzak Masaüstü bağlanamıyor, Uzak Masaüstü hataları, Uzak Masaüstü sorun giderme, Uzak Masaüstü sorunları
ms.assetid: 9da36f3d-30dd-44af-824b-8ce5ef07e5e0
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 4b4d2e2099f0d49c7dd9a150ac659ffde62eaa21
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60506424"
---
# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Azure'da Windows VM'ler Uzak Masaüstü Bağlantısı sorunlarında ayrıntılı sorun giderme adımları
Bu makalede, Windows tabanlı Azure sanal makineleri için karmaşık Uzak Masaüstü hataları tanılayıp ayrıntılı sorun giderme adımları sağlar.

> [!IMPORTANT]
> Uzak Masaüstü daha yaygın hataları ortadan kaldırmak için okuduğunuzdan emin olun [Uzak Masaüstü için temel sorun giderme makalesi](troubleshoot-rdp-connection.md) devam etmeden önce.

Karşılaşabileceğiniz belirli hata iletilerinin benzemez hata iletisi ele alınmıştır Uzak Masaüstü [temel Uzak Masaüstü sorun giderme kılavuzu](troubleshoot-rdp-connection.md). Uzak Masaüstü (RDP) istemci Azure VM'de RDP hizmetine bağlanamıyor neden olduğunu belirlemek için aşağıdaki adımları izleyin.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) tıklatıp **destek al**. Azure desteği'ni kullanma hakkında daha fazla bilgi için okuma [Microsoft Azure desteği SSS](https://azure.microsoft.com/support/faq/).

## <a name="components-of-a-remote-desktop-connection"></a>Uzak Masaüstü bağlantısının bileşenleri
Aşağıdaki bileşenler bir RDP bağlantısında ilgilidir:

![](./media/detailed-troubleshoot-rdp/tshootrdp_0.png)

Devam etmeden önce VM için son başarılı Uzak Masaüstü Bağlantısı beri değiştirilen akıl incelemek için yararlı olabilir. Örneğin:

* Sanal makine veya sanal Makineyi içeren bir bulut hizmeti genel IP adresini (sanal IP adresi olarak da bilinir [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) değiştirildi. DNS istemci önbelleğinizi hala olduğundan RDP hata olabilir *eski IP adresini* kayıtlı DNS adı. DNS istemci önbelleğinizi temizlemek ve VM yeniden bağlanmayı deneyin. Veya doğrudan yeni VIP ile bağlanmayı deneyin.
* Azure portal tarafından oluşturulan bağlantı kullanmak yerine, Uzak Masaüstü bağlantıları yönetmek için üçüncü taraf bir uygulama kullanıyorsunuz. Uygulama yapılandırması, Uzak Masaüstü trafiğine doğru TCP bağlantı noktasını içerdiğini doğrulayın. Klasik bir sanal makine için bu bağlantı noktası denetleyebilirsiniz [Azure portalında](https://portal.azure.com), sanal makinenin ayarlarını tıklayarak > uç noktalar.

## <a name="preliminary-steps"></a>Başlangıç adımları
Ayrıntılı sorun giderme için devam etmeden önce

* Tüm açık sorunları için Azure portalında sanal makine durumunu denetleyin.
* İzleyin [hızlı düzeltme adımları temel sorun giderme Kılavuzu'nda sık karşılaşılan RDP hataları](troubleshoot-rdp-connection.md#quick-troubleshooting-steps).
* Özel görüntüler için VHD'nizi karşıya yüklemek için önceki düzgün hazırlandığından emin olun. Daha fazla bilgi için [Windows VHD veya VHDX yüklemek için hazırlama](../windows/prepare-for-upload-vhd-image.md).


VM'ye Uzak Masaüstü aracılığıyla Bu adımlardan sonra yeniden bağlanmayı deneyin.

## <a name="detailed-troubleshooting-steps"></a>Ayrıntılı sorun giderme adımları
Uzak masaüstü istemcisini Uzak Masaüstü hizmetini sorunları aşağıdaki kaynaklardan en nedeniyle Azure VM'de ulaşabildiğinden olmayabilir:

* [Uzak Masaüstü istemcisi bilgisayar](#source-1-remote-desktop-client-computer)
* [Kuruluş intranet edge cihazı](#source-2-organization-intranet-edge-device)
* [Bulut Hizmeti uç noktası ve erişim denetimi listesi (ACL)](#source-3-cloud-service-endpoint-and-acl)
* [Ağ güvenlik grupları](#source-4-network-security-groups)
* [Windows tabanlı Azure VM](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>1. kaynak: Uzak Masaüstü istemcisi bilgisayar
Bilgisayarınızda başka bir şirket içi, Windows tabanlı bir bilgisayarda Uzak Masaüstü bağlantılarında yapabilirsiniz doğrulayın.

![](./media/detailed-troubleshoot-rdp/tshootrdp_1.png)

Çözemezseniz, bilgisayarınızın aşağıdaki ayarlarını kontrol edin:

* Uzak Masaüstü trafiği engelleyen bir yerel güvenlik duvarı ayarı.
* Yerel istemci proxy Uzak Masaüstü bağlantıları engelleyen bir yazılım yüklenir.
* Ağ izleme Uzak Masaüstü bağlantıları engelliyor yazılımını yerel olarak yüklenir.
* Trafiği izlemek ya da Uzak Masaüstü bağlantıları engelliyor trafiği belirli türlerini izin vermek/engellemek diğer güvenlik yazılım türleri.

Tüm bu gibi durumlarda, geçici olarak devre dışı yazılım ve bir şirket içi bilgisayara Uzak Masaüstü aracılığıyla bağlanmak deneyin. Bu şekilde gerçek nedeninin bulabilirsiniz Uzak Masaüstü bağlantılarına izin verecek şekilde yazılım ayarları düzeltmek için ağ yöneticinizle birlikte çalışın.

## <a name="source-2-organization-intranet-edge-device"></a>2. kaynak: Kuruluş intranet edge cihazı
Doğrudan Internet'e bağlı bir bilgisayarı Azure sanal makinenize Uzak Masaüstü bağlantılarında yapabilirsiniz doğrulayın.

![](./media/detailed-troubleshoot-rdp/tshootrdp_2.png)

Doğrudan Internet'e bağlı bir bilgisayarda yoksa, oluşturma ve bir kaynak grubu veya Bulut hizmetinde yeni bir Azure sanal makine ile test edin. Daha fazla bilgi için [Windows Azure'da çalışan bir sanal makine oluşturma](../virtual-machines-windows-hero-tutorial.md). Sanal makine ve kaynak grubu veya Bulut hizmeti, test sonra silebilirsiniz.

Doğrudan Internet'e bağlı bir bilgisayar ile bir Uzak Masaüstü bağlantısı oluşturmak için kuruluş intranet edge Cihazınızı kontrol edin:

* İnternet'e HTTPS bağlantıları engelleyen bir iç güvenlik duvarı.
* Uzak Masaüstü bağlantıları engelleyen bir proxy sunucu.
* Yetkisiz giriş algılama veya ağ edge ağınızdaki Uzak Masaüstü bağlantıları engelliyor cihazlarda çalışan yazılım izleme.

İnternet HTTPS tabanlı Uzak Masaüstü bağlantılarına izin verecek şekilde kuruluş intranet edge cihazınızın ayarları düzeltmek için ağ yöneticinizle birlikte çalışın.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>3. kaynak: Bulut Hizmeti uç noktası ve ACL
Klasik dağıtım modeli kullanılarak oluşturulan VM'ler için başka bir Azure VM, aynı bulut hizmeti veya sanal ağ ile Uzak Masaüstü bağlantılarında Azure VM için yapabileceğiniz doğrulayın.

![](./media/detailed-troubleshoot-rdp/tshootrdp_3.png)

> [!NOTE]
> Resource Manager'da oluşturulan sanal makineler için atlamak [kaynak 4: Ağ güvenlik grupları](#source-4-network-security-groups).

Aynı bulut hizmetinde veya sanal ağ başka bir sanal makine yoksa bir tane oluşturun. Bağlantısındaki [Windows Azure'da çalışan bir sanal makine oluşturma](../virtual-machines-windows-hero-tutorial.md). Test tamamlandıktan sonra test sanal makinesini silin.

Aynı bulut hizmetinde veya sanal ağ içindeki bir sanal makineye Uzak Masaüstü aracılığıyla bağlantı, bu ayarlar için denetleyin:

* Uzak Masaüstü trafik hedef VM için uç nokta yapılandırması: Uç noktasının özel TCP bağlantı noktası sanal makinenin Uzak Masaüstü hizmetini dinlediği TCP bağlantı noktası eşleşmesi gerekir (varsayılan olarak 3389).
* Hedef VM'de Uzak Masaüstü trafiği uç noktası için ACL: ACL, kaynak IP adresine göre Internet'ten gelen trafiğe izin verileceğini veya belirtmenize olanak sağlar. Yanlış ACL'ler uç noktaya gelen Uzak Masaüstü trafiğine engel olabilir. Bu genel IP adreslerinizi Ara sunucunuzun'ten gelen trafiği emin olmak için ACL'ler denetleyin veya başka bir uç sunucusu izin verilir. Daha fazla bilgi için [bir ağ erişim denetimi listesi (ACL) nedir?](../../virtual-network/virtual-networks-acl.md)

Uç nokta sorunun kaynağı olup olmadığını denetlemek için geçerli uç noktasını kaldırın ve dış bağlantı noktası numarası 49152-65535 aralığında rastgele bir bağlantı noktası seçerek yeni bir tane oluşturun. Daha fazla bilgi için [bir sanal makineye uç noktaları ayarlama işlemini](../windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="source-4-network-security-groups"></a>4. kaynak: Ağ Güvenlik Grupları
Ağ güvenlik grupları, izin verilen gelen ve giden trafik daha ayrıntılı denetim sağlar. Alt ağlar kapsayan kurallar oluşturabilir ve bulut Hizmetleri, bir Azure sanal ağında.

[IP akışı doğrulamayı](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) kullanarak Ağ Güvenlik Grubu’ndaki bir kuralın bir sanal makineye giden veya gelen trafiği engelleyip engellemediğini doğrulayın. Ayrıca, gelen "izin ver" NSG emin olmak için etkin güvenlik grubu kurallarını gözden geçirebilirsiniz kuralının mevcut olduğundan ve RDP bağlantı noktası (varsayılan olarak 3389) için önceliklendirildiğinden. Daha fazla bilgi için [kullanılarak geçerli güvenlik VM sorunlarını gidermek için kuralları trafik akışı](../../virtual-network/diagnose-network-traffic-filter-problem.md).

## <a name="source-5-windows-based-azure-vm"></a>5. kaynak: Windows tabanlı Azure VM
![](./media/detailed-troubleshoot-rdp/tshootrdp_5.png)

Bölümündeki yönergeleri [bu makalede](../windows/reset-rdp.md). Bu makalede, sanal makinede Uzak Masaüstü hizmetini sıfırlar:

* "Uzak Masaüstü" Windows Güvenlik Duvarı varsayılan kural (TCP bağlantı noktası 3389) etkinleştirin.
* HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections kayıt defteri değerini 0 olarak ayarlayarak, Uzak Masaüstü bağlantılarını etkinleştirin.

Bilgisayarınızdan bağlantıyı yeniden deneyin. Uzak Masaüstü aracılığıyla bağlanmak hala kaldıramıyorsanız, aşağıdaki olası sorunlar için denetleyin:

* Hedef sanal Makineye Uzak Masaüstü hizmeti çalışmıyor.
* Uzak Masaüstü hizmetini 3389 numaralı TCP bağlantı noktasında dinleme yapmıyor.
* Windows Güvenlik Duvarı veya başka bir yerel güvenlik duvarı Uzak Masaüstü trafiği engelleyen bir giden kuralı vardır.
* Yetkisiz giriş algılama veya ağ Azure sanal makine üzerinde çalışan yazılım izleme Uzak Masaüstü bağlantıları engelliyor.

Klasik dağıtım modeli kullanılarak oluşturulan VM'ler için Azure sanal makinesine yapılan uzak bir Azure PowerShell oturumu kullanabilirsiniz. İlk olarak, sanal makinenin barındırma bulut hizmeti için bir sertifika yüklemeniz gerekir. Git [yapılandırma güvenli uzaktan PowerShell erişim için Azure sanal makineler](https://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) ve indirme **InstallWinRMCertAzureVM.ps1** komut dosyasını yerel bilgisayarınıza.

Ardından, henüz yapmadıysanız Azure PowerShell'i yükleyin. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

Ardından, bir Azure PowerShell komut istemi açın ve geçerli klasörde konumuyla değiştirin **InstallWinRMCertAzureVM.ps1** betik dosyası. Azure PowerShell Betiği çalıştırmak için doğru yürütme ilkesini ayarlamanız gerekir. Çalıştırma **Get-ExecutionPolicy** komutu, geçerli bir ilke düzeyini belirleyin. Uygun düzeyde ayarlama hakkında daha fazla bilgi için bkz: [Set-ExecutionPolicy](https://technet.microsoft.com/library/hh849812.aspx).

Ardından, Azure abonelik adınız, bulut hizmeti adı ve sanal makinenizin adını doldurun (kaldırma < ve > karakterleri), ve ardından şu komutları çalıştırın.

```powershell
$subscr="<Name of your Azure subscription>"
$serviceName="<Name of the cloud service that contains the target virtual machine>"
$vmName="<Name of the target virtual machine>"
.\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName
```

Doğru abonelik adını alabilirsiniz *SubscriptionName* görüntülenmesini özelliği **Get-AzureSubscription** komutu. Bulut hizmeti adı sanal makine için alabilirsiniz *ServiceName* görüntülenmesini sütununda **Get-AzureVM** komutu.

Yeni sertifika olup olmadığını denetleyin. Geçerli kullanıcı ile bir sertifika ek bileşenini açın ve konum **güvenilen kök sertifika Yetkilileri\Sertifikalar** klasör. Bulut hizmetinizin çıkarılan sütunda DNS adına sahip bir sertifika görmeniz gerekir (örnek: cloudservice4testing.cloudapp.net).

Ardından, şu komutları kullanarak uzak bir Azure PowerShell oturumu başlatın.

```powershell
$uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
$creds = Get-Credential
Enter-PSSession -ConnectionUri $uri -Credential $creds
```

Geçerli yönetici kimlik bilgilerini girdikten sonra aşağıdaki Azure PowerShell istemi için benzer bir şey görmeniz gerekir:

```powershell
[cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>
```

Bu istemi ilk kısmı, "cloudservice4testing.cloudapp.net" farklı olabilir, hedef sanal makine, içeren, bulut hizmet adıdır. Azure PowerShell komutları sorunları araştırmak bu bulut hizmeti için belirtilen ve yapılandırmayı düzeltmek artık uygulayabilirsiniz.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>TCP bağlantı noktasını dinlemeyi Uzak Masaüstü Hizmetleri el ile düzeltmek için
Uzak Azure PowerShell oturumu komut isteminde şu komutu çalıştırın.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

PortNumber özelliği geçerli bir bağlantı noktası numarasını gösterir. Gerekli olursa, Uzak Masaüstü değiştirmek şu komutu kullanarak bağlantı noktası numarası arka varsayılan değerine (3389).

```powershell
Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389
```

Bu komutu kullanarak bağlantı noktası 3389 değiştirildiğini doğrulayın.

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

Bu komutu kullanarak uzak Azure PowerShell oturumu kapatın.

```powershell
Exit-PSSession
```

Azure sanal makine için Uzak Masaüstü uç noktası de TCP bağlantı noktası 3398 iç bağlantı noktasını kullandığından emin olun. Azure VM'yi yeniden başlatın ve Uzak Masaüstü bağlantısı yeniden deneyin.

## <a name="additional-resources"></a>Ek kaynaklar
[Bir parola veya Windows sanal makineler için Uzak Masaüstü hizmetini sıfırlama](../windows/reset-rdp.md)

[Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)

[Linux tabanlı bir Azure sanal makine için güvenli Kabuk (SSH) bağlantılarında sorun giderme](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Azure sanal makinesinde çalışan bir uygulamaya erişim sorunlarını giderme](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

