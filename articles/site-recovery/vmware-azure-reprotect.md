---
title: VMware Vm'lerini ve fiziksel sunucuları olağanüstü durum kurtarma sırasında Azure Vm'lerinden şirket içi siteye yeniden koruma | Microsoft Docs
description: Yük devretmeden sonra azure'da VMware Vm'lerini ve fiziksel sunucuları olağanüstü durum kurtarma sırasında Azure'dan şirket içi siteye yeniden çalışma hakkında bilgi edinin.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 3/12/2019
ms.author: mayg
ms.openlocfilehash: 4202d95b540efb98b526f8a8abd17da22a908ebe
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60482940"
---
# <a name="reprotect-and-fail-back-machines-to-an-on-premises-site-after-failover-to-azure"></a>Yeniden koruma ve bir şirket içi siteye geri makineleri, azure'a yük devredildikten sonra başarısız

Sonra [yük devretme](site-recovery-failover.md) şirket içi VMware Vm'lerini veya fiziksel sunucuları azure'a, ilk başarısız olan şirket içi sitenize geri yük devretme sırasında oluşturulan Azure Vm'lerini yeniden koruma için adımdır. Bu makalede bunun nasıl yapılacağı açıklanır. 

Hızlı bir genel bakış için Azure'dan şirket içi bir siteye yük devretme konusunda aşağıdaki videoyu izleyin.<br /><br />
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="before-you-begin"></a>Başlamadan önce

Sanal makinelerinizi oluşturmak için bir şablon kullandıysanız, her bir sanal makine diskleri için kendi UUID sahip olduğundan emin olun. Her ikisi de aynı şablonu oluşturulmadığından şirket içi sanal makinenin UUID ana hedef UUID'si ile çakışıyor. yeniden koruma başarısız olur. Aynı şablondan oluşturulmadıysa başka bir ana hedef dağıtın. Aşağıdaki bilgileri not edin:
- Alternatif bir vCenter başarısız çalışıyorsanız, yeni vCenter'ınıza ve ana hedef sunucusunda bulunduğundan emin emin olun. Bir normal veri depoları erişilemez veya görünür olmayan belirtisidir **yeniden koruma** iletişim kutusu.
- Çoğaltma yeniden şirket içine yeniden çalışma ilkesi gerekir. İleri yönde bir ilke oluşturduğunuzda bu ilke otomatik olarak oluşturulur. Aşağıdaki bilgileri not edin:
    - Bu ilke otomatik oluşturma sırasında yapılandırma sunucusu ile ilişkili kullanılır.
    - Bu ilke düzenlenemez.
    - İlke kümesi değerleri (RPO eşiği = 15 dakika, kurtarma noktası bekletme = 24 saat, uygulama tutarlılığı anlık görüntü sıklığı = 60 dakika).
- Yeniden koruma ve yeniden çalışma sırasında şirket içi yapılandırma sunucusu çalışır durumda ve bağlı olması gerekir.
- Bir vCenter sunucusu için başarısız geri sanal makineleri yönetir, bilgisayarınızda yüklü olduğundan emin olun [gerekli izinler](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) vCenter sunucuları üzerindeki sanal makinelerin bulma.
- Yeniden koruma önce ana hedef sunucusu üzerindeki anlık görüntüleri silin. Anlık görüntüleri şirket içi ana hedef veya sanal makinede mevcut olması durumunda, yeniden koruma başarısız olur. Sanal makine anlık görüntüleri otomatik olarak yeniden koruma işi sırasında birleştirilir.
- Bir çoğaltma grubundaki tüm sanal makineleri aynı işletim sistemi türünü (tüm Windows veya Linux tüm) olmalıdır. Yeniden koruma ve şirket içine yeniden çalışma için bir çoğaltma grubu şu anda karma işletim sistemlerinde desteklenmez. Ana hedef sanal makineyle aynı işletim sisteminin olması gerektiğinden budur. Bir çoğaltma grubundaki tüm sanal makineler, aynı ana hedefi olması gerekir. 
- Yapılandırma sunucusunu yeniden çalışma özelliğini gerekli şirket içi andır. Yeniden çalışma sırasında sanal makinenin yapılandırma sunucusuna veritabanında mevcut olmalıdır. Aksi takdirde, yeniden çalışma başarısız olur. Düzenli olarak zamanlanmış yedeklemeleri yapılandırma sunucunuzun yaptığınızdan emin olun. Böylece gerçekleştiğini gösteren bir olağanüstü durum varsa, sunucu ile aynı IP adresini geri yükleyin.
- Yeniden koruma ve yeniden çalışma verilerini çoğaltmak bir siteden siteye (S2S) VPN gerektirir. Azure'da devredilen sanal makineler (ping) şirket içi yapılandırma sunucusuna erişebilmesi için ağ sağlar. Bir Azure ağı devredilen sanal makinenin işlem sunucusunu dağıtmak isteyebilirsiniz. Bu işlem sunucusu da şirket içi yapılandırma sunucusu ile iletişim kurabildiğini olması gerekir.
- Yük devretme ve yeniden çalışma için aşağıdaki bağlantı noktalarının açık olduğundan emin olun:

    ![Yük devretme ve yeniden çalışma için bağlantı noktaları](./media/vmware-azure-reprotect/failover-failback.png)

- Tüm okuma [bağlantı noktaları ve URL beyaz listeye ekleme önkoşulları](vmware-azure-deploy-configuration-server.md#prerequisites).

## <a name="deploy-a-process-server-in-azure"></a>Azure'da bir işlem sunucusu dağıtma

Azure'da bir işlem sunucusu, önce şirket içi sitede yeniden çalıştırmak gerekebilir:
- İşlem sunucusu, korumalı sanal makine azure'da veri alır ve sonra verileri şirket içi siteye gönderir.
- İşlem sunucusu ve korumalı sanal makine arasında bir gecikme süresi düşük ağ gereklidir. Genel olarak, gecikme süresi, azure'da bir işlem sunucusu gerekmediğine karar verirken dikkate almanız gerekir:
    - Ayarlanmış bir Azure ExpressRoute bağlantınız varsa, sanal makine ve işlem sunucusu arasındaki gecikme süresi düşük olduğundan veri göndermek için bir şirket içi işlem sunucusunu kullanabilirsiniz.
    - Ancak, yalnızca S2S VPN varsa, azure'daki işlem sunucusu dağıtılıyor öneririz.
    - Bir Azure tabanlı işlem sunucusu yeniden çalışma sırasında kullanmanızı öneririz. Çoğaltma sanal makinesine (devredilen makine azure'da) işlem sunucusu yakınsa çoğaltma performansı daha yüksektir. Bir kavram kanıtı için şirket içi işlem sunucusu ve ExpressRoute özel eşlemesi ile kullanabilirsiniz.

Azure'da bir işlem sunucusu dağıtmak için:

1. Azure'da bir işlem sunucusu dağıtmak için ihtiyacınız varsa bkz [azure'da yeniden çalışma için bir işlem sunucusu ayarlamak](vmware-azure-set-up-process-server-azure.md).
2. Azure Vm'lerini çoğaltma verilerini işlem sunucusuna gönderir. Azure Vm'lerini işlem sunucusuna erişebilmesi için ağları yapılandırın.
3. Unutmayın azure'dan şirket içine çoğaltma, yalnızca S2S VPN veya özel eşleme, ExpressRoute ağı üzerinden oluşabilir. Yeterli bant genişliği ağ kanal üzerinden kullanılabilir olduğundan emin olun.

## <a name="deploy-a-separate-master-target-server"></a>Ayrı bir ana hedef sunucusu dağıtma

Ana hedef sunucu yeniden çalışma verilerini alır. Ana hedef sunucu varsayılan olarak şirket için yapılandırma sunucusunda çalışır. Ancak, başarısız oldu-yeniden trafik hacmine bağlı olarak, ayrı bir ana hedef sunucu yeniden çalışma için oluşturmanız gerekebilir. Nasıl oluşturacağınızı şöyledir:

* [Bir Linux ana hedef sunucu oluşturma](vmware-azure-install-linux-master-target.md) Linux Vm'leri yeniden çalışma için. Bu gereklidir. Not, ana hedef sunucuda LVM desteklenmiyor.
* İsteğe bağlı olarak, bir Windows VM yeniden çalışma için ayrı bir ana hedef sunucusu oluşturun. Bunu yapmak için birleşik Kurulum'u yeniden çalıştırın ve bir ana hedef sunucusu oluşturmak için seçin. [Daha fazla bilgi edinin](site-recovery-plan-capacity-vmware.md#deploy-additional-master-target-servers). 

Bir ana hedef sunucu oluşturduktan sonra aşağıdaki görevleri yapın:

- Mevcut şirket içi vCenter sunucusundaki sanal makine ise ana hedef sunucusu şirket içi sanal makinenin sanal makine diski (VMDK) dosya erişimi olmalıdır. Çoğaltılan veriler sanal makinenin disklerine yazmak için erişim gereklidir. Şirket içi sanal makinenin veri deposunu okuma/yazma erişimiyle ana hedef konaktaki bağlı olduğundan emin olun.
- Mevcut şirket içi vCenter sunucusundaki sanal makine değil, Site Recovery hizmeti yeniden koruma sırasında yeni bir sanal makine oluşturması gerekir. Bu sanal makine ana hedefi oluşturduğunuz ESX konağında oluşturulur. ESX konağı dikkatli bir şekilde, böylece istediğiniz konakta yeniden çalışma sanal makine oluşturulduğunda seçin.
- Ana hedef sunucusu için depolama vMotion kullanamazsınız. Ana hedef sunucusu için depolama VMotion'ı kullanarak, yeniden çalışma başarısız olmasına neden olabilir. Diskleri kullanılabilir olmadığından, sanal makine başlatılamıyor. Bunun gerçekleşmesini önlemek için ana hedef sunucuları vMotion listenizin dışında hariç tutun.
- Ana hedef depolama vMotion görev sonra yeniden koruma uğradığında, hedef vMotion görevin ana hedefe bağlı korunan sanal makine disklerini geçirin. Bundan sonra başarısız denerseniz, diskleri bulunamadı çünkü ayrılmayı diskin başarısız olur. Daha sonra diskleri depolama hesaplarınızda zor olur. El ile bulmak ve onları sanal makineye eklemek gerekir. Bundan sonra şirket içi sanal makine önyüklenebilecek.
- Bekletme sürücüsü mevcut Windows ana hedef sunucusuna ekleyin. Yeni bir disk ekleyin ve diski biçimlendirin. Bekletme sürücüsü noktaları sanal makine şirket içi sitede yeniden ne zaman çoğaltılacağını zaman durdurmak için kullanılır. Bazı bekletme sürücüsü ölçütleri aşağıda verilmiştir. Bu ölçütler sağlanmadıysa, sürücü ana hedef sunucu için listede:
    - Birim, çoğaltma hedefi gibi başka amaçla kullanılmaz.
    - Birim Kilit modunda değil.
    - Birim önbellek birimi değil. Ana hedef yüklemesi bu birimde var olamaz. İşlem sunucusu ve ana hedef için özel yükleme birimi, bekletme birimi için uygun değildir. İşlem sunucusu ve ana hedef birimde yüklendiğinde, birim ana hedefinin önbellek birimdir.
    - Birim dosya sistemi türü FAT veya FAT32 değildir.
    - Birim kapasitesi sıfır.
    - Windows için varsayılan elde tutma birimi R birimdir.
    - Linux için varsayılan elde tutma birimi/mnt/Retention ' dir.
- Varolan bir işlem sunucusu/yapılandırma sunucusu makinesini veya ölçeklendirme veya işlem sunucusu/ana hedef sunucu makinesi kullanıyorsanız, yeni bir sürücüye eklemeniz gerekir. Yeni sürücü önceki gereksinimleri karşılaması gerekir. Bekletme sürücüsü yoksa, portal seçimi aşağı açılan listede görünmez. Şirket içi ana hedef için bir sürücüsü ekledikten sonra sürücü seçimi portalında görünmesi 15 dakika kadar sürer. Sürücü 15 dakika sonra görünmüyorsa yapılandırma sunucusunu aynı zamanda yenileyebilirsiniz.
- VMware araçları veya açık vm araçları ana hedef sunucusuna yükleyin. Araçlar olmasaydı, veri depoları ana hedefin ESXi ana bilgisayarındaki algılanamıyor.
- Ayarlama `disk.EnableUUID=true` VMware ana hedef sanal makinenin yapılandırma parametrelerini ayarlama. Bu satır yoksa, bunu ekleyin. Bu ayarı, doğru bir şekilde bağlar, böylece tutarlı UUID VMDK'dan sağlamak için gereklidir.
- Ana hedef oluşturulduğu ESX konağına bağlı en az bir sanal makine dosya sistemi (VMFS) veri deposu olması gerekir. Hiçbir VMFS veri depoları bağlıysa, **veri deposu** yeniden koruma sayfasında giriş boş olabilir ve devam edemiyor.
- Ana hedef sunucusunda anlık görüntüleri disklerine sahip olamaz. Anlık görüntüler varsa, yeniden koruma ve yeniden çalışma başarısız.
- Ana hedef Paravirtual SCSI denetleyicisine sahip olamaz. Denetleyici yalnızca LSI Logic denetleyicisi olabilir. LSI Logic denetleyicisi, yeniden koruma başarısız olur.
- Herhangi bir örneğine bağlı en fazla 60 disk ana hedef olabilir. Şirket içi ana hedef yeniden korumaya alınmış en fazla 60 disk, toplam sayısı olan sanal makine sayısı yeniden korunmasını sağlayan, ana hedef için başarısız olmaya başlar. Hedef disk yuvaları veya ek ana hedef sunucuları dağıtma yeterli ana olduğundan emin olun.
    

## <a name="enable-reprotection"></a>Yeniden koruma etkinleştirme

Azure'da bir sanal makine başlatıldıktan sonra aracının yapılandırma sunucusunu yeniden (en fazla 15 dakika) kaydetmek biraz zaman alabilir. Bu süre boyunca yeniden korumak mümkün olmayacaktır ve aracı yüklü olmayan bir hata iletisi gösterir. Bu durumda, birkaç dakika bekleyin ve sonra yeniden koruma yeniden deneyin:


1. Seçin **kasası** > **çoğaltılan öğeler**. Yük devretme sanal makinesine sağ tıklayın ve ardından **yeniden koruma**. Ya da komut düğmeleri makineyi seçin ve ardından **yeniden koruma**.
2. Doğrulayın **şirket içi Azure** gelen koruma yönünün seçilidir.
3. İçinde **ana hedef sunucu** ve **işlem sunucusu**, şirket içi ana hedef sunucusu ve işlem sunucusunu seçin.  
4. İçin **veri deposu**, şirket içi diskleri kurtarmak istediğiniz veri deposu seçin. Bu seçenek, şirket içi sanal makine silinir ve yeni diskler oluşturmanız gerektiğinde kullanılır. Diskler zaten varsa, bu seçenek göz ardı edilir. Yine de bir değer belirtmeniz gerekir.
5. Bekletme sürücüsünü seçin.
6. Yeniden çalışma ilkesi otomatik olarak seçilir.
7. Seçin **Tamam** yeniden korumaya başlamak için. Sanal makineyi Azure’dan şirket içi siteye çoğaltan bir iş başlar. **İşler** sekmesinde ilerleme durumunu izleyebilirsiniz. Yeniden koruma başarılı olduğunda, sanal makine, korumalı bir duruma girer.

Aşağıdaki bilgileri not edin:
- (Şirket içi sanal makine silindiğinde) farklı bir konuma kurtarmak istiyorsanız, ana hedef sunucu için yapılandırılan veri deposu ve bekletme sürücüsünü seçin. Şirket içi sitede yeniden çalıştırmak, VMware sanal makineleri yeniden koruma planı ana hedef sunucusu olarak aynı veri deposunu kullanın. Yeni bir sanal makine vCenter sonra oluşturulur.
- Mevcut bir şirket içi sanal makine için azure'da sanal makineyi kurtarmak istiyorsanız, şirket içi sanal makinenin veri depoları okuma/yazma erişimiyle ana hedef sunucunun ESXi ana bilgisayarındaki bağlayın.

    ![İletişim kutusunu yeniden koruma](./media/vmware-azure-reprotect/reprotectinputs.png)

- Ayrıca bir kurtarma planı düzeyinde yeniden koruyabilir. Bir çoğaltma grubu yalnızca bir kurtarma planı korunabilir. Bir kurtarma planı kullanarak yeniden koruma, korunan her makine için değerler sağlamanız gerekir.
- Çoğaltma grubunu yeniden korumak için aynı hedef sunucuyu kullanın. Bir çoğaltma grubunu yeniden korumak için farklı bir ana hedef sunucusu kullanıyorsanız, sunucu ortak bir noktaya zaman sağlayamaz.
- Şirket içi sanal makine yeniden koruma sırasında kapatılır. Bu işlem, çoğaltma sırasında veri tutarlığını sağlar. Yeniden koruma bittikten sonra sanal makineyi açma.



## <a name="common-issues"></a>Genel sorunlar

- Salt okunur kullanıcı vCenter bulma işlemini gerçekleştirmek ve sanal makinelerini koruma, koruma başarılı olur ve yük devretme çalışır. Veri depoları bulunamıyor çünkü yeniden koruma sırasında yük devretme başarısız olur. Veri depoları yeniden koruma sırasında listelenmeyen bir belirtisidir. Bu sorunu çözmek için izinleri olan uygun bir hesap ile vCenter kimlik bilgilerini güncelleştirin ve sonra işi yeniden deneyin. 
- Linux sanal makinesi yeniden çalışma ve şirket içinde çalıştırın, ağ yöneticisi paketi makineden kaldırıldı görebilirsiniz. Azure'da sanal makine kurtarıldığında ağ yöneticisi paketi kaldırıldığından bu kaldırma gerçekleşir.
- Bir Linux sanal makine Azure'a yük devretme ve statik IP adresiyle yapılandırılmış olduğunda, IP adresini DHCP alınır. Şirket içine yük devretme, sanal makinenin IP adresini almak için DHCP kullanmaya devam eder. Makinede el ile oturum açın ve ardından geri bir statik adrese gerekiyorsa IP adresini ayarlayın. Bir Windows sanal makinesi statik IP adresini yeniden elde edebilirsiniz.
- ESXi 5.5 ücretsiz sürümü veya ücretsiz sürüm 6 vSphere hiper yönetici kullanıyorsanız, yük devretme başarılı, ancak yeniden çalışma işlemi başarılı değil. Yeniden çalışma etkinleştirmek için her iki programın değerlendirme lisansına yükseltme.
- İşlem sunucusundan yapılandırma sunucusuna ulaşamazsa, yapılandırma sunucusu bağlantı noktası 443 üzerinden bağlantıyı denetlemek için Telnet kullanın. İşlem sunucusundan yapılandırma sunucusuna ping işlemi yapmayı deneyebilirsiniz. Yapılandırma sunucusuna bağlı olduğunda bir işlem sunucusu bir sinyal de sahip olmalıdır.
- Fiziksel şirket içi sunucusu olarak korumalı bir Windows Server 2008 R2 SP1 sunucu, Azure'dan bir şirket içi siteye devredilemez.
- Aşağıdaki durumlarda çalışma özelliğini kullanamazsınız:
    - Makinelerin Azure için geçişiniz. [Daha fazla bilgi edinin](migrate-overview.md#what-do-we-mean-by-migration).
    - Bir VM için başka bir kaynak grubuna taşındı.
    - Azure sanal makine silindi.
    - VM'nin korumasını devre dışı.
    - Sanal Makineyi el ile Azure'da oluşturduğunuz. Makine başlangıçta korumalı şirket içinde olan ve Azure'a yeniden koruma önce yük devretti gerekir.
    - Yalnızca bir ESXi konağı için başarısız olabilir. Yeniden çalışma VMware Vm'lerini veya fiziksel sunucuları Hyper-V konakları, fiziksel makineler veya VMware iş istasyonlarını yapamazsınız.


## <a name="next-steps"></a>Sonraki adımlar

Sanal makine, korumalı bir duruma geçtiğini sonra [bir yeniden başlatma](vmware-azure-failback.md). Yeniden çalışma, Azure sanal makineyi kapatır ve şirket içi sanal makine önyüklenir. Uygulama için bazı kapalı kalma süresi bekler. Uygulama kapalı kalma süresi tolere edebilen yeniden çalışma için bir saat seçin.


