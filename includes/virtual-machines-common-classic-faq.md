---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: bc3590e90cacfa4966f0d1f64aa1c8d49483cb1b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61485499"
---
Bu makale, kullanıcıların klasik dağıtım modeliyle oluşturulmuş Azure sanal makineleri hakkında sorduğu bazı yaygın sorular ele alınmıştır.

## <a name="can-i-migrate-my-vm-created-in-the-classic-deployment-model-to-the-new-resource-manager-model"></a>Klasik dağıtım modelinde oluşturulmuş sanal makinemi Resource Manager modeline geçirebilir miyim?
Evet. Nasıl geçiş yapıldığı ile ilgili yönergeler için bkz.

* [Azure PowerShell kullanarak klasikten Azure Resource Manager’a geçiş](../articles/virtual-machines/windows/migration-classic-resource-manager-ps.md).
* [Azure CLI kullanarak klasikten Azure Resource Manager’a geçiş](../articles/virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md).

## <a name="what-can-i-run-on-an-azure-vm"></a>Azure sanal makinesinde ne çalıştırabilirim?
Tüm aboneler bir Azure sanal makinesinde sunucu yazılımı çalıştırabilir. Windows Server’ın yeni sürümlerinin yanı sıra çeşitli Linux dağıtımlarını çalıştırabilirsiniz. Destek ayrıntıları için bkz.

• Windows VM’leri için -- [Azure Sanal Makineleri için Microsoft sunucu yazılımı desteği](https://go.microsoft.com/fwlink/p/?LinkId=393550)

• Linux VM’leri için -- [Azure Destekli Dağıtımlarda Linux](https://go.microsoft.com/fwlink/p/?LinkId=393551)

Windows istemci görüntüleri için MSDN Azure avantajı aboneleri ve MSDN Geliştirme ve Test Kullandıkça Öde aboneleri geliştirme ve test görevlerinde Windows 7 ve Windows 8.1’in belirli sürümlerini kullanabilir. Yönerge ve kısıtlamalar dahil olmak üzere ayrıntılı bilgi edinmek için bkz. [MSDN aboneleri için Windows İstemci görüntüleri](https://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/).

## <a name="why-are-affinity-groups-being-deprecated"></a>Benzeşim grupları neden kullanımdan kaldırılıyor?
Benzeşim grupları, bir müşterinin Azure içindeki bulut hizmeti dağıtımlarının ve depolama hesaplarının coğrafi olarak gruplandırılmasıyla ilgili eski bir kavramdır. Başlangıçta, ilk Azure ağ tasarımlarında VM’ler arası ağ performansını geliştirmek için sağlanıyordu. Bunlar, sanal ağların (VNet) bir bölgedeki az sayıda donanımla sınırlı olacak şekilde ilk kez yayınlanmasını da destekliyordu.

Şu anki bölge içi Azure ağı, benzeşim gruplarını gereksinimini ortadan kaldıracak şekilde tasarlanmıştır. Sanal ağlar da bölgesel bir kapsamda olduğundan, artık sanal ağ kullanılırken benzeşim grubu gerekmez. Bu geliştirmeler nedeniyle, müşterilerin bazı senaryolarda kısıtlayıcı olabilecek benzeşim gruplarını kullanması artık önerilmez. Benzeşim gruplarının kullanılması VM’lerinizi gereksiz yere belirli donanımlarla ilişkilendirir ve bu da VM boyutu seçeneklerinizi azaltır. Ayrıca, benzeşim grubuyla ilişkili donanımın kapasitesi dolmak üzere olduğunda yeni VM ekleme girişimleriniz kapasiteyle ilgili hatalara yol açabilir.

Benzeşim grubu özellikleri Azure Resource Manager dağıtım modelinde ve Azure portalında zaten kullanımdan kaldırılmıştır. Klasik Azure portalı için benzeşim grupları oluşturma ve bir benzeşim grubuna sabitlenen depolama kaynakları oluşturma için sunduğumuz desteği sonlandırıyoruz. Benzeşim grubu kullanmakta olan mevcut bulut hizmetlerini değiştirmenize gerek yoktur. Bununla birlikte, bir Azure destek uzmanı tarafından önerilmediği sürece yeni bulut hizmetleri için benzeşim gruplarını kullanmamalısınız.

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Bir sanal makineyle birlikte ne kadar depolama alanı kullanabilirim?
Her veri diskinin kapasitesi 1 TB'a kadar olabilir. Kullanabileceğiniz veri diski sayısı, sanal makinenin boyutuna bağlıdır. Ayrıntılar için bkz. [Virtual Machines boyutları](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

İşletim sistemi diski ve varsa veri diskleri için bir Azure depolama hesabı tarafından depolama alanı sağlanır. Her disk bir sayfa blobu olarak depolanan bir .vhd dosyasıdır. Fiyatlandırma ayrıntıları için bkz. [Depolama Fiyatlandırma Ayrıntıları](https://go.microsoft.com/fwlink/p/?LinkId=396819).

## <a name="which-virtual-hard-disk-types-can-i-use"></a>Hangi sanal sabit disk türlerini kullanabilirim?
Azure yalnızca değişmeyen, VHD biçimli sanal sabit diskleri destekler. Azure’da kullanmak istediğiniz bir VHDX varsa, önce Hyper-V Manager’ı ya da [convert-VHD](https://go.microsoft.com/fwlink/p/?LinkId=393656) cmdlet’ini kullanarak bunu dönüştürmeniz gerekir. Bu işlemi yaptıktan sonra, VHD’yi sanal makinelerle kullanabilmek için [Add-AzureVHD](https://msdn.microsoft.com/library/azure/dn495173.aspx) cmdlet’ini (Hizmet Yönetimi modunda) kullanarak Azure’daki bir depolama hesabına yükleyin.

* Linux yönergeleri için bkz. [Linux İşletim Sistemi İçeren Bir Sanal Sabit Disk Oluşturma ve Karşıya Yükleme](../articles/virtual-machines/linux/classic/create-upload-vhd-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="are-these-virtual-machines-the-same-as-hyper-v-virtual-machines"></a>Bu sanal makineler Hyper-V sanal makineleri ile aynı mıdır?
Birçok yönden “1. Nesil” Hyper-V VM’lerine benzer, ancak tam olarak aynı değildir. Her iki tür de sanallaştırılmış donanım sağlar ve VHD biçimli sanal sabit diskler uyumludur. Bu, Hyper-V ile Azure arasında geçiş yapabileceğiniz anlamına gelir. Bazen Hyper-V kullanıcılarını şaşırtan üç temel fark şunlardır:

* Azure bir sanal makineye konsol erişimi sağlamaz. Önyüklemesi tamamlanana kadar bir VM’ye erişmenin hiçbir yolu yoktur.
* Çoğu [boyuttaki](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Azure VM’ler yalnızca 1 sanal ağ bağdaştırıcısına sahiptir ve bu, yalnızca 1 dış IP adresine sahip olabilecekleri anlamına gelir. (A8 ve A9 boyutları, sınırlı sayıda senaryoda örnekler arası iletişim için ikinci bir ağ bağdaştırıcısı kullanır.)
* Azure VM’leri 2. Nesil Hyper-V VM özelliklerini desteklemez. Bu özellikler hakkında ayrıntılı bilgi edinmek için bkz. [Hyper-V için Sanal Makine Belirtimleri](https://technet.microsoft.com/library/dn592184.aspx) ve [2. Nesil Sanal Makineye Genel Bakış](https://technet.microsoft.com/library/dn282285.aspx).

## <a name="can-these-virtual-machines-use-my-existing-on-premises-networking-infrastructure"></a>Bu sanal makineler mevcut, şirket içi ağ altyapımı kullanabilir mi?
Klasik dağıtım modelinde oluşturulan sanal makineler için Azure Sanal Ağ’ı kullanarak mevcut altyapınızı genişletebilirsiniz. Bu yaklaşım, şube ayarlamak gibidir. Azure’da Sanal özel ağlar (VPN) sağlayıp bunları yönetebilir ve şirket içi BT altyapısına güvenli bir biçimde bağlayabilirsiniz. Ayrıntılar için bkz. [Sanal Ağa Genel Bakış](../articles/virtual-network/virtual-networks-overview.md).

Sanal makineyi oluştururken sanal makinenin ait olmasını istediğiniz ağı belirtmeniz gerekir. Mevcut bir sanal makineyi sanal ağa bağlayamazsınız. Bununla birlikte, önce mevcut sanal makineden sanal sabit diski (VHD) ayırıp sonra bu diskle istediğiniz ağ yapılandırmasına sahip yeni bir sanal makine oluşturarak bu soruna geçici bir çözüm bulabilirsiniz.

## <a name="how-can-i-access--my-virtual-machine"></a>Sanal makinelerime nasıl erişebilirim?
Bir Windows VM veya bir güvenli Kabuk (SSH) için bir Linux VM için Uzak Masaüstü bağlantısı kullanarak sanal makineye oturum açmak için Uzak bağlantı kurmak gerekir. Yönergeler için bkz.

* [Windows Server çalıştıran bir sanal makinede oturum açma](../articles/virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Sunucu bir Uzak Masaüstü Hizmetleri oturum konağı olarak yapılandırılmadığı sürece en fazla 2 eş zamanlı bağlantı desteklenir.  
* [Linux Çalıştıran Bir Sanal Makinede Oturum Açma](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Varsayılan olarak, SSH en fazla 10 eş zamanlı bağlantıya izin verir. Yapılandırma dosyasını düzenleyerek bu sayıyı artırabilirsiniz.

Uzak Masaüstü veya SSH ile ilgili sorun yaşıyorsanız, sorunun giderilmesine yardımcı olacak [VMAccess](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) uzantısını yükleyip kullanın.

Windows VM’ler için ek seçenekler şunlardır:

* Azure portalında VM'yi bulun ve tıklayın **uzaktan erişimi Sıfırla** komut çubuğundan.
* [Windows tabanlı Azure Sanal Makinesine Uzak Masaüstü bağlantıları ile ilgili sorunları giderme](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) konusunu gözden geçirin.
* Windows PowerShell Uzaktan İletişimini kullanarak VM’ye bağlanın veya diğer kaynakların VM’ye bağlanması için ek uç noktalar oluşturun. Ayrıntılar için bkz. [Sanal Makineye Uç Noktaları Ayarlama](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Hyper-V deneyiminiz varsa VMConnect’e benzer bir araç arıyor olabilirsiniz. Sanal makineye konsol erişimi desteklenmediğinden, Azure benzer bir araç sunmaz.

## <a name="can-i-use-the-temporary-disk-the-d-drive-for-windows-or-devsdb1-for-linux-to-store-data"></a>Verileri depolamak için geçici diski (Windows için D: sürücüsü veya Linux için /dev/sdb1) kullanabilir miyim?
Verileri depolamak için geçici diski (Windows için varsayılan olarak D: sürücüsü veya Linux için /dev/sdb1) kullanmamalısınız. Bunlar yalnızca geçici depolama alanı olduğundan, verileri kurtarılamaz biçimde kaybetme riskiyle karşılaşırsınız. Sanal makine başka bir konağa taşındığında bu durum gerçekleşebilir. Sanal makinenin yeniden boyutlandırılması, konağın güncelleştirilmesi veya konaktaki bir donanım hatası, sanal makinenin taşınmasını gerektirecek olası nedenler arasındadır.

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Geçici diskin sürücü harfini nasıl değiştirebilirim?
Windows sanal makinesinde disk belleği dosyasını taşıyıp sürücü harflerini yeniden atayarak sürücü harfini değiştirebilirsiniz, ancak adımları belirli bir sırada gerçekleştirdiğinizden emin olmanız gerekir. Yönergeler için bkz. [Windows geçici diskinin sürücü harfini değiştirme](../articles/virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="how-can-i-upgrade-the-guest-operating-system"></a>Konuk işletim sistemini nasıl yükseltebilirim?
Yükseltme terimi genellikle donanımı değiştirmeksizin işletim sisteminin daha yeni bir sürümüne geçmeyi ifade eder. Azure VM’leri için daha yeni bir sürüme geçme süreci Linux ve Windows için farklıdır:

* Linux VM'leri için dağıtıma yönelik paket yönetimi araçlarını ve yordamlarını kullanın.
* Windows sanal makinesi için Windows Server Geçiş Araçları gibi bir araçla sunucuyu geçirmeniz gerekir. Konuk işletim sistemi Azure’dayken işletim sistemini yükseltmeye çalışmayın. Bu işlem, sanal makine erişimini kaybetme riski nedeniyle desteklenmez. Yükseltme sırasında sorun oluşursa, Uzak Masaüstü oturumu başlatma özelliğini kullanamaz hale gelir ve sorunları gideremezsiniz.

Windows Server geçişine yönelik araçlar ve işlemler hakkındaki genel ayrıntılar için bkz. [Rolleri ve Özellikleri Windows Server’a Geçirme](https://go.microsoft.com/fwlink/p/?LinkId=396940).

## <a name="whats-the-default-user-name-and-password-on-the-virtual-machine"></a>Sanal makinede varsayılan kullanıcı adı ve parola nedir?
Azure tarafından sağlanan görüntülerin önceden yapılandırılmış bir kullanıcı adı ve parolası yoktur. Bu görüntülerden birini kullanarak sanal makine oluşturduğunuzda, bir kullanıcı adı ve sanal makineye oturum açmak için kullanacağınız bir parola sağlamanız gerekir.

Kullanıcı adını veya parolayı unuttuysanız ve VM Aracısını yüklediyseniz sorunu çözmek için [VMAccess](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) uzantısını yükleyip kullanabilirsiniz.

Ek ayrıntılar:

* Linux görüntüleri için Azure portalını kullanıyorsanız, varsayılan kullanıcı adı olarak 'azureuser' verilir, ancak bu şekilde sanal makine oluşturmak için 'Galeri'den' 'Hızlı Oluştur' yerine'i kullanarak değiştirebilirsiniz. ‘Galeri’den’ seçeneğini kullanmanız, oturumunuzu açmak için bir parola mı, SSH anahtarı mı yoksa ikisini birden mi kullanmak istediğinize karar vermenize de imkan tanır. Kullanıcı hesabı, ayrıcalıklı komutları çalıştırmak için ‘sudo’ erişimi olan ayrıcalıksız bir kullanıcıdır. ‘Root’ hesabı devre dışıdır.
* Windows görüntüleri için VM’yi oluştururken bir kullanıcı adı ve parola sağlamanız gerekir. Hesap Administrators grubuna eklenir.

## <a name="can-azure-run-anti-virus-on-my-virtual-machines"></a>Azure sanal makinelerimde virüsten koruma yazılımı çalıştırabilir mi?
Azure, virüsten koruma çözümleri için çeşitli seçenek sunar ancak bunların yönetimi size bırakılır. Örneğin, kötü amaçlı yazılımdan koruma yazılımı için ayrı bir aboneliğe ihtiyacınız olabilir; ne zaman tarama çalıştırılacağına ve güncelleştirmelerin yükleneceğine karar vermeniz gerekir. Windows sanal makinesi oluştururken veya daha sonra Microsoft Kötü Amaçlı Yazılımdan Koruma, Symantec Endpoint Protection veya TrendMicro Deep Security Agent için bir VM uzantısıyla virüsten koruma desteği ekleyebilirsiniz. Symantec ve TrendMicro uzantıları, ücretsiz sınırlı süreli deneme aboneliği veya mevcut bir kurumsal abonelik kullanmanıza olanak tanır. Microsoft Kötü Amaçlı Yazılımdan Koruma ücretsizdir. Ayrıntılar için bkz.

* [Azure Sanal Makinesinde Symantec Uç Nokta Koruması yükleme ve yapılandırma](https://go.microsoft.com/fwlink/p/?LinkId=404207)
* [Azure Sanal Makinesinde Hizmet Olarak Trend Micro Deep Security yükleme ve yapılandırma](https://go.microsoft.com/fwlink/p/?LinkId=404206)
* [Azure Sanal Makinelerinde Kötü Amaçlı Yazılıma Karşı Koruma Çözümleri Dağıtma](https://azure.microsoft.com/blog/2014/05/13/deploying-antimalware-solutions-on-azure-virtual-machines/)

## <a name="what-are-my-options-for-backup-and-recovery"></a>Yedekleme ve kurtarma seçeneklerim nelerdir?
Azure Backup belirli bölgelerde önizleme olarak sunulmaktadır. Ayrıntılar için bkz. [Azure sanal makinelerini yedekleme](../articles/backup/backup-azure-arm-vms.md). Sertifikalı iş ortaklarının sunduğu başka çözümler vardır. Şu anda hangi çözümlerin kullanılabilir olduğunu öğrenmek için Azure Market’te arama yapın.

Bir başka seçenek de blob depolamanın anlık görüntü özelliklerini kullanmaktır. Bunu yapmak için, blob anlık görüntüsüne bağlı herhangi bir işlem gerçekleştirmeden önce VM’yi kapatmanız gerekir. Bunu yaptığınızda bekleyen veri yazma işlemleri kaydedilir ve dosya sistemi kararlı bir hale getirilir.

## <a name="how-does-azure-charge-for-my-vm"></a>Azure, sanal makinem için nasıl bir ücretlendirme uygular?
Azure, sanal makinenin boyutuna ve işletim sistemine bağlı olarak saatlik fiyat uygular. Kısmi saatler için, Azure yalnızca kullanılan dakika sayısını ücretlendirir. Sanal makineyi önceden yüklenmiş belirli yazılımlar içeren bir VM görüntüsüyle oluşturursanız ek saatlik yazılım ücretleri uygulanabilir. Azure, sanal makinenin işletim sistemi ve veri diskleri için ayrı ayrı depolama ücretleri uygular. Geçici disk depolama ücretsizdir.

VM durumu Çalışıyor veya Durduruldu olduğunda ücretlendirilirsiniz, ancak VM durumu Durduruldu (Serbest bırakıldı) olduğunda ücret ödemezsiniz. Bir sanal makinenin durumunu Durduruldu (Serbest bırakıldı) yapmak için aşağıdakilerden birini yapın:

* Kapat veya VM Azure portalından silin.
* Azure PowerShell modülünden erişebileceğiniz Stop-AzureVM cmdlet’ini kullanın.
* Hizmet Yönetimi REST API’sindeki Kapatma Rolü işlemini kullanarak PostShutdownAction öğesi için StoppedDeallocated değerini belirtin.

Daha ayrıntılı bilgi edinmek için bkz. [Sanal Makine Fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="will-azure-reboot-my-vm-for-maintenance"></a>Azure, bakım için sanal makinemi yeniden başlatır mı?
Azure bazen Azure veri merkezlerindeki düzenli, planlanmış bakım güncelleştirmeleri için sanal makinenizi yeniden başlatır.

Azure tarafından sanal makinenizi etkileyen ciddi bir donanım sorunu algılandığında planlanmamış bakım olayları gerçekleşebilir. Planlanmamış olaylar için Azure otomatik olarak VM’yi sağlıklı bir konağa geçirir ve yeniden başlatır.

Herhangi bir tek başına VM (yani bir kullanılabilirlik kümesine ait olmayan bir VM) için Azure, planlı bakımdan en az bir hafta önce sanal makinelerin güncelleştirme sırasında yeniden başlatılabileceği konusunda aboneliğin Hizmet Yöneticisi’ne e-posta ile bildirim gönderir. VM’lerde çalışan uygulamalar kapalı kalma süresiyle karşılaşabilir.

Planlı bakım nedeniyle yeniden başlatma işlemi meydana geldiğinde, yeniden başlatma günlüklerini görüntülemek için Azure portal veya Azure PowerShell kullanabilirsiniz. Ayrıntılar için bkz. [VM Yeniden Başlatma Günlüklerini Görüntüleme](https://azure.microsoft.com/blog/2015/04/01/viewing-vm-reboot-logs/).

Yedeklilik sağlamak için benzer şekilde yapılandırılmış iki veya daha fazla sanal makineyi aynı kullanılabilirlik kümesine koyun. Bu, planlı veya planlanmamış bakım sırasında en az bir sanal makinenin kullanılabilir kalmasına yardımcı olur. Azure, bu yapılandırma için belirli düzeylerde VM kullanılabilirliği garantisi verir. Ayrıntılar için bkz. [Sanal makinelerin kullanılabilirliğini yönetme](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="additional-resources"></a>Ek kaynaklar
[Azure Sanal Makineleri hakkında](../articles/virtual-machines/virtual-machines-linux-about.md)

[Azure CLI ile Linux VM’leri Oluşturma ve Yönetme](../articles/virtual-machines/linux/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Azure PowerShell ile Windows Vm'leri oluşturma ve yönetme](../articles/virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

