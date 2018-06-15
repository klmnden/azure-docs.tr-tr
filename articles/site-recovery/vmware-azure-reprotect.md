---
title: Azure VM'lerin bir şirket içi siteye koruyun | Microsoft Docs
description: Azure VM'ler, yük devretme sonrasında geri şirket içi sanal makineleri getirmek için yeniden çalışma başlatabilirsiniz. Yeniden çalışma önce koruyun öğrenin.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: rajanaki
ms.openlocfilehash: 4ee6eefa431b06e0cb694635e188c87a8a4175c9
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34737274"
---
# <a name="reprotect-machines-from-azure-to-an-on-premises-site"></a>Azure makinelerden bir şirket içi siteye koruyun

Sonra [yük devretme](site-recovery-failover.md) şirket içi VMware Vm'lerini veya fiziksel sunucuları azure'a, yük devretme sırasında oluşturulan Azure VM'ler yeniden korumak için başarısız olan ilk adımı, şirket içi siteye geri olmalıdır. Bu makalede bunun nasıl yapılacağı açıklanır. 

Hızlı bir genel bakış için Azure'dan şirket içi siteye yük devri konusunda aşağıdaki videoyu izleyin.<br /><br />
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="before-you-begin"></a>Başlamadan önce

Sanal makineler oluşturmak için bir şablon kullanıldığında, her bir sanal makine disklerin kendi UUID olduğundan emin olun. Her ikisi de aynı şablonu oluşturulmadığından şirket içi sanal makinenin UUID ana hedef UUID ile çakışıyor yükü başarısız olur. Aynı şablonu oluşturulmadıysa başka bir ana hedef dağıtın. Aşağıdaki bilgileri not edin:
- Alternatif bir vCenter başarısız çalışıyorsanız, yeni vCenter ve ana hedef sunucusunda bulunan emin olun. Tipik bir belirti datastores erişilemez veya görünür durumda olmayan olan **koruyun** iletişim kutusu.
- Geri şirket içi çoğaltmak için yeniden çalışma ilkesi gerekir. Bir iletme yönü ilkesi oluşturduğunuzda bu ilke otomatik olarak oluşturulur. Aşağıdaki bilgileri not edin:
    - Bu ilke otomatik oluşturma sırasında yapılandırma sunucusu ile ilişkili kullanılır.
    - Bu ilke düzenlenebilir değil.
    - İlke kümesi değerleri (RPO eşik 15 dakika, kurtarma noktası bekletme = 24 saat, uygulama tutarlılığı anlık görüntü sıklığı = 60 dakika =).
- Yükü ve yeniden çalışma sırasında şirket içi yapılandırma sunucusunun çalıştığından ve bağlı olması gerekir.
- Bir vCenter sunucusu için başarısız geri sanal makineleri yönetir, bilgisayarınızda yüklü olduğundan emin olun [gerekli izinleri](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) VM'ler bulma vCenter sunucuları üzerinde.
- Koruyun önce ana hedef sunucusunda anlık görüntüleri silin. Anlık görüntü mevcut şirket içi ana hedef ya da sanal makine yükü başarısız olur. Sanal makinenin anlık görüntüleri otomatik olarak yeniden koruma işi sırasında birleştirilir.
- Bir çoğaltma grubundaki tüm sanal makineleri aynı işletim sistemi türünü (tüm Windows veya tüm Linux) olması gerekir. Bir çoğaltma grubu şu anda karma işletim sistemleriyle yeniden koruma ve şirket içi yeniden çalışma için desteklenmiyor. Ana hedef sanal makine aynı işletim sisteminin olması gerektiğinden budur. Bir çoğaltma grubunun tüm sanal makineler, aynı ana hedefi olması gerekir. 
- Geri başarısız olduğunda bir yapılandırma gerekli şirket içi sunucusudur. Yeniden çalışma sırasında sanal makine yapılandırma sunucusu veritabanında mevcut olması gerekir. Aksi takdirde, yeniden çalışma başarısız olur. Yapılandırma sunucunuzun düzenli olarak zamanlanmış yedeklemeler yaptığınızdan emin olun. Bir olağanüstü durum varsa, böylece geri dönme çalışır sunucusuyla aynı IP adresini geri yükleyin.
- Yükü ve yeniden çalışma veri çoğaltmak siteden siteye (S2S) VPN gerektirir. (Ping) şirket içi yapılandırma sunucusu Azure başarısız üzerinden sanal makinelere erişebilmesi için ağ sağlar. Başarısız üzerinden sanal makinenin Azure ağında bir işlem sunucusu dağıtmak isteyebilirsiniz. Bu işlem sunucusu ayrıca şirket içi yapılandırma sunucusu ile iletişim kurabildiğinden olmalıdır.
- Yük devretme ve yeniden çalışma için aşağıdaki bağlantı noktalarını açın emin olun:

    ![Yük devretme ve yeniden çalışma için bağlantı noktaları](./media/vmware-azure-reprotect/failover-failback.png)

- Tüm okuma [bağlantı noktaları ve URL uygulamaları güvenilir listeye almayı önkoşulları](vmware-azure-deploy-configuration-server.md#prerequisites).

## <a name="deploy-a-process-server-in-azure"></a>Azure'da bir işlem sunucusu Dağıt

Şirket içi sitenize geri başarısız önce bir işlem sunucusu Azure gerekebilir:
- İşlem sunucusu azure'da korunan sanal makine verileri alan ve sonra verileri şirket içi siteye gönderir.
- İşlem sunucusu ve korumalı sanal makine arasında düşük gecikme süreli ağ gereklidir. Genel olarak, gecikme süresi bir işlem sunucusu Azure gerekmediğine karar verirken dikkate almanız gerekir:
    - Ayarlanmış bir Azure ExpressRoute bağlantınız varsa, sanal makine ve işlem sunucusu arasında gecikme düşük olduğundan veri göndermek için bir şirket içi işlem sunucusu kullanabilirsiniz.
    - Ancak, yalnızca bir S2S VPN varsa, Azure işlem sunucusu dağıtımı öneririz.
    - Bir Azure tabanlı işlem sunucusu yeniden çalışma sırasında kullanmanızı öneririz. Çoğaltma sanal makinesine (azure'da başarısız üzerinden makine) işlem sunucusu yakınsa çoğaltma performansı yüksektir. Kavram kanıtı için özel eşleme ile şirket içi işlem sunucusu ve ExpressRoute kullanabilirsiniz.

Bir işlem sunucusu azure'da dağıtmak için:

1. Azure'da bir işlem sunucusu dağıtımı yapmanız gerekirse bkz [yeniden çalışma için azure'da bir işlem sunucusu kurma](vmware-azure-set-up-process-server-azure.md).
2. Azure sanal makinelerini çoğaltma verilerini işlem sunucusuna gönderir. Azure sanal makinelerini işlem sunucusu erişebilmesi için ağlarını yapılandırın.
3. Unutmayın yalnızca S2S VPN veya özel eşleme, ExpressRoute ağ üzerinden azure'dan şirket içi çoğaltma oluşabilir. Yeterli bant genişliği ağ kanal üzerinden kullanılabilir olduğundan emin olun.

## <a name="deploy-a-separate-master-target-server"></a>Ayrı ana hedef sunucusu dağıtma

Ana hedef sunucu yeniden çalışma verilerini alır. Ana hedef sunucu varsayılan olarak şirket için yapılandırma sunucusunda çalışır. Ancak, başarısız geri trafik hacmi bağlı olarak, ayrı ana hedef sunucusu yeniden çalışma için oluşturmanız gerekebilir. Şöyle oluşturmak:

* [Bir Linux ana hedef sunucusu oluşturmak](vmware-azure-install-linux-master-target.md) Linux VM'ler geri dönmesi için. Bu gereklidir.
* İsteğe bağlı olarak, Windows VM yeniden çalışma için ayrı ana hedef sunucusu oluşturun. Bunu yapmak için birleşik Kurulum'u yeniden çalıştırın ve bir ana hedef sunucusu oluşturmak için seçin. [Daha fazla bilgi edinin](physical-azure-set-up-source.md#run-azure-site-recovery-unified-setup).

Bir ana hedef sunucusu oluşturduktan sonra aşağıdaki görevleri yapın:

- Sanal makine mevcut şirket içi vCenter sunucusunda ise, ana hedef sunucusu şirket içi sanal makinenin sanal makine Disk (VMDK) dosyaya erişimi gerekir. Erişim için sanal makinenin disklerinin çoğaltılan veri yazmak için gereklidir. Şirket içi sanal makinenin veri deposu okuma/yazma erişimine sahip ana hedefin ana bilgisayarına bağlandığından emin olun.
- Sanal makinenin mevcut şirket içi vCenter sunucusunda değilse, Site Recovery hizmetine yeni bir sanal makine yükü sırasında oluşturması gerekir. Bu sanal makine ana hedef oluşturma ESX ana bilgisayarda oluşturulur. Yeniden çalışma sanal makine istediğiniz konakta oluşturulan ESX konak dikkatlice seçin.
- Ana hedef sunucusu için depolama VMotion'ı kullanamazsınız. Ana hedef sunucusu için depolama VMotion'ı kullanarak yeniden çalışma başarısız olmasına neden olabilir. Diskleri için kullanılabilir olmadığından, sanal makine başlatılamıyor. Bunun gerçekleşmesini önlemek için ana hedef sunucusu VMotion'ı listenizden dışlayın.
- Bir ana hedef depolama VMotion'ı görev yükü sonra uğradığında, VMotion'ı görevin hedefi için ana hedefe bağlı korumalı sanal makine disklerini geçirin. Bundan sonra başarısız çalışırsanız, diskleri bulunamadı ayrılmayı diskin başarısız olur. Ardından, diskleri depolama hesaplarınızı bulmak zorlaşır. El ile bulmak ve bunları sanal makineye İliştir gerekir. Bundan sonra şirket içi sanal makine önyüklendiğinde.
- Saklama sürücüsünün var olan Windows ana hedef sunucunuzu ekleyin. Yeni bir disk ekleyin ve sürücü biçimlendirin. Saklama sürücüsünün sanal makine şirket içi siteye ne zaman çoğaltılacağını zaman içinde nokta durdurmak için kullanılır. Saklama sürücüsünün bazı ölçütlere aşağıda verilmiştir. Bu ölçütler karşılanmadığı takdirde sürücü ana hedef sunucu için listelenmeyen:
    - Birim, çoğaltma hedefi gibi başka bir amaç için kullanılmaz.
    - Birimin kilidi modunda değil.
    - Birimin bir önbellek birimi değil. Ana hedef yükleme bu birimde yer alamaz. İşlem sunucusu ve ana hedef için özel yükleme birim bekletme birimi için uygun değil. İşlem sunucusu ve ana hedef birimde yüklendiğinde, birim ana hedef önbellek birimdir.
    - FAT veya FAT32 birimin dosya sistemi türü değil.
    - Birim kapasitesi sıfırdan farklı.
    - Windows için varsayılan saklama biriminin R birimdir.
    - Linux için varsayılan saklama biriminin /mnt/retention ' dir.
- Var olan bir işlem sunucusu/configuration server makine veya ölçek veya işlem sunucusu/ana hedef sunucu makinesi kullanıyorsanız, yeni bir sürücü eklemeniz gerekir. Yeni sürücü önceki gereksinimlerini karşılaması gerekir. Saklama sürücüsünün mevcut değilse, portal seçimi aşağı açılan listede görünmüyor. Şirket içi ana hedef için bir sürücü ekledikten sonra portal seçimini görünmesi sürücü 15 dakika kadar alır. Sürücü 15 dakika sonra görünmüyorsa yapılandırma sunucusu de yenileyebilirsiniz.
- VMware araçları veya açık vm araçları ana hedef sunucuya yükleyin. Araçlar olmadan ana Hedef'in ESXi konağına üzerinde datastores algılanamıyor.
- Ayarlama `disk.EnableUUID=true` VMware ana hedef sanal makine yapılandırma parametrelerini ayarlama. Bu satır yoksa, ekleyin. Bu ayar, doğru bağlar VMDK'ye tutarlı UUID sağlamak için gereklidir.
- Ana hedef oluşturulduğu ESX konak en az bir sanal makine dosya sistemi (VMFS) datastore bağlı olması gerekir. Hiçbir VMFS datastores bağlıysa, **veri deposu** yeniden koruma sayfasında Giriş boşsa ve devam edemiyor.
- Ana hedef sunucusu anlık görüntüleri disklerde sahip olamaz. Anlık görüntüler varsa, yükü ve yeniden çalışma başarısız.
- Ana hedef Paravirtual SCSI denetleyicisine sahip olamaz. Denetleyici yalnızca bir LSI Logic denetleyicisi olabilir. LSI Logic denetleyicisi, yükü başarısız olur.
- Herhangi bir örnek için ana hedefe bağlı en fazla 60 disk olabilir. Şirket içi ana hedef korunmuş 60 disklerin toplam sayısından daha fazlasını sahip sanal makine sayısı yeniden korunmasını sağlayan ana hedefe başarısız olmaya başlar. Disk yuvaları hedef ya da ek ana hedef sunucusu dağıtın yeterli ana olduğundan emin olun.
    

## <a name="enable-reprotection"></a>Yükü Etkinleştir

Azure'da bir sanal makine önyükleme sonra yapılandırmayı sunucuya geri (en fazla 15 dakika) kaydetmek aracı için biraz zaman alabilir. Bu süre boyunca koruyun erişemezsiniz ve aracısı yüklü değil hata iletisi gösterir. Bu durumda, birkaç dakika bekleyin ve ardından yükü yeniden deneyin:


1. Seçin **kasa** > **öğeleri çoğaltılan**. Devredilen sanal makineye sağ tıklayın ve ardından **yeniden koruma**. Veya, komut düğmeleri makine seçin ve ardından **yeniden koruma**.
2. Doğrulayın **şirket içi Azure** koruma yönünü seçilidir.
3. İçinde **ana hedef sunucusu** ve **işlem sunucusu**, şirket içi ana hedef sunucusu ve işlem sunucusu seçin.  
4. İçin **veri deposu**, diskleri şirket içi kurtarmak istediğiniz veri deposu seçin. Bu seçenek şirket içi sanal makine silinir ve yeni disk oluşturmanız gerektiğinde kullanılır. Diskleri zaten varsa bu seçeneği göz ardı edilir. Yine bir değer belirtmeniz gerekir.
5. Saklama sürücüsünün seçin.
6. Yeniden çalışma ilkesi otomatik olarak seçilir.
7. Seçin **Tamam** yükü başlamak için. Sanal makineyi Azure’dan şirket içi siteye çoğaltan bir iş başlar. **İşler** sekmesinde ilerleme durumunu izleyebilirsiniz. Sanal makinenin yükü başarılı olduğunda korumalı bir duruma girer.

Aşağıdaki bilgileri not edin:
- (Şirket içi sanal makine silindiğinde) alternatif konuma kurtarma yapmak istiyorsanız, bekletme sürücüsü ve ana hedef sunucu için yapılandırılmış veri deposu seçin. Şirket içi sitede yeniden çalıştırmak, yeniden çalışma koruma planı VMware sanal makineleri aynı veri deposu ana hedef sunucusu olarak kullanın. Yeni bir sanal makine ardından Vcenter'da oluşturulur.
- Var olan bir şirket içi sanal makine için Azure üzerindeki sanal makineyi kurtarmak istiyorsanız, şirket içi sanal makinenin datastores okuma/yazma erişimi olan ana hedef sunucunun ESXi ana bilgisayarda bağlayın.

    ![İletişim kutusu koruyun](./media/vmware-azure-reprotect/reprotectinputs.png)

- Ayrıca bir kurtarma planı düzeyinde koruyun. Bir çoğaltma grubu yalnızca bir kurtarma planı reprotected. Bir kurtarma planı kullanarak koruyun, korunan her makine için değer sağlamalısınız.
- Çoğaltma grubunu yeniden korumak için aynı hedef sunucuyu kullanın. Bir çoğaltma grubunu yeniden korumak için farklı bir ana hedef sunucusu kullanıyorsanız, sunucunun ortak bir noktaya zamanında sağlayamaz.
- Şirket içi sanal makine yükü sırasında kapalıdır. Bu işlem, çoğaltma sırasında veri tutarlığını sağlar. Yükü tamamlandıktan sonra sanal makineyi açma.



## <a name="common-issues"></a>Genel sorunlar

- Şu anda yalnızca bir VMFS veya vSAN veri deposu geri başarısız olan Site Recovery destekler. NFS veri deposu desteklenmiyor. Bu sınırlama nedeniyle veri deposu seçimi giriş yeniden koruma ekran, NFS datastores için boş veya vSAN veri deposu gösterir ancak işi sırasında başarısız oluyor. Geri dönecek yapmak istiyorsanız, şirket içi bir VMFS veri deposu oluşturmak ve yeniden başarısız. Bu geri dönme VMDK tamamını indirmeyi neden olur.
- Salt okunur kullanıcı vCenter bulma işlemini ve sanal makineleri koruma, koruma başarılı ve yük devretme çalışır. Yükü sırasında datastores bulunan yük devretme başarısız olur. Bir belirti datastores yükü sırasında listelenmeyen olmasıdır. Bu sorunu gidermek için izinlere sahip uygun bir hesap ile vCenter kimlik bilgilerini güncelleştirin ve işi yeniden deneyin. 
- Linux sanal makine yeniden çalışmak ve şirket içi çalıştırın, ağ yöneticisi paket makineden kaldırıldı görebilirsiniz. Bu kaldırma işlemi, sanal makine Azure'da kurtarıldığında ağ yöneticisi paket kaldırılır oluşur.
- Linux sanal makine Azure'a yük devretti ve statik IP adresiyle yapılandırılmış olduğunda, IP adresi DHCP'den alınır. Şirket içi üzerinden başarısız olduğunda, sanal makinenin IP adresini almak için DHCP kullanmaya devam eder. Makineye el ile oturum açın ve ardından geri bir statik adrese gerekiyorsa IP adresi ayarlayın. Windows sanal makinesi statik IP adresini yeniden elde edebilirsiniz.
- ESXi 5.5 ücretsiz sürüm veya ücretsiz sürüm 6 vSphere hiper yönetici kullanıyorsanız, yük devretme başarılı olur, ancak yeniden çalışma başarısız değil. Yeniden çalışma etkinleştirmek için her iki programın değerlendirme lisansına yükseltme.
- Yapılandırma sunucusuna işlem sunucusundan bağlanamazsa, Telnet 443 numaralı bağlantı noktasını yapılandırma sunucusunda bağlantısını denetlemek için kullanın. Yapılandırma sunucusuna işlem sunucusundan ping deneyebilirsiniz. Yapılandırma sunucusuna bağlandığında bir işlem sunucusu bir sinyal de olmalıdır.
- Fiziksel şirket içi sunucu Azure'dan şirket içi siteye başarısız olamaz korunan bir Windows Server 2008 R2 SP1 sunucusu.
- Aşağıdaki durumlarda kapatamazsınız:
    - Makineler için Azure geçirildi. [Daha fazla bilgi edinin](migrate-overview.md#what-do-we-mean-by-migration).
    - Bir VM başka bir kaynak grubuna taşındı.
    - Azure VM silindi.
    - VM korumasını devre dışı.
    - VM el ile Azure'da oluşturduğunuz. Makine başlangıçta korunan şirket içi edilmiş ve Azure'a önce yükü devredilen gerekir.
    - Yalnızca bir ESXi ana başarısız olabilir. Yeniden çalışma VMware Vm'lerini veya fiziksel sunucuları Hyper-V konakları, fiziksel makineleri ya da VMware iş istasyonlarının yapamazsınız.


## <a name="next-steps"></a>Sonraki adımlar

Sanal makinenin korumalı bir duruma geçtiğini sonra [bir yeniden başlatma](vmware-azure-failback.md). Yeniden çalışma Azure sanal makineyi kapatır ve şirket içi sanal makine önyüklenir. Bazı uygulama kesintiler bekler. Uygulama kapalı kalma süresi dayanabileceği yeniden çalışma için bir saat seçin.


