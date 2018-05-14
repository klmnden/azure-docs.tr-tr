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
ms.openlocfilehash: 0946d5234292cfb69a7e9b5bc7846e6acf94dff4
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="reprotect-machines-from-azure-to-an-on-premises-site"></a>Azure makinelerden bir şirket içi siteye koruyun

Sonra [yük devretme](site-recovery-failover.md) şirket içi VMware Vm'lerini veya fiziksel sunucuları azure'a, yük devretme sırasında oluşturulan Azure VM'ler yeniden korumak için başarısız olan ilk adımı, şirket içi siteye geri olmalıdır. Bu makalede bunun nasıl yapılacağı açıklanır. 

Hızlı bir genel bakış için Azure'dan şirket içi siteye yük devri konusunda aşağıdaki videoyu izleyin.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="before-you-start"></a>Başlamadan önce

Sanal makineler oluşturmak için bir şablon kullanıldığında, her bir sanal makine disklerin kendi UUID olduğundan emin olun. Her ikisi de aynı şablonu oluşturulmadığından şirket içi sanal makinenin UUID, ana hedef çakışıyor yükü başarısız olur. Aynı şablon kullanılarak oluşturulan olmayan başka bir ana hedef dağıtın.
- Alternatif bir vCenter başarısız çalışıyorsanız, yeni vCenter ve ana hedef sunucusunda bulunan emin olun. Tipik bir belirti datastores erişilebilir veya görünür durumda olmamasıdır **koruyun** iletişim kutusu.
- Geri şirket içi çoğaltmak için yeniden çalışma ilkesi gerekir. Bu ilke otomatik olarak oluşturulan bir iletme yönü ilkesi oluşturduğunuzda. Şunlara dikkat edin:
    - Bu ilke otomatik oluşturma sırasında yapılandırma sunucusu ile ilişkili alır.
    - Bu ilke düzenlenebilir değil.
    - İlke kümesi değerleri (RPO eşik 15 dakika, kurtarma noktası bekletme = 24 saat, uygulama tutarlılığı anlık görüntü sıklığı = 60 dakika =)
- Yükü ve yeniden çalışma sırasında şirket içi yapılandırma sunucusu çalışıyor olmalıdır ve bağlı.
- Bir vCenter sunucusu sanal makinelere yönetiyorsa geri başarısız, olduğundan emin olun [gerekli izinleri](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) VM'ler bulma vCenter sunucuları üzerinde.
- Koruyun önce ana hedef sunucusunda anlık görüntüleri silin. Şirket içi ana hedef veya sanal makine anlık görüntüler varsa yükü başarısız olur. Sanal makinenin anlık görüntüleri otomatik olarak yeniden koruma işi sırasında birleştirilir.
- Bir çoğaltma grubundaki tüm sanal makineleri aynı işletim sistemi türünü (tüm Windows veya tüm Linux) olmalıdır. Karma işletim sistemleri olan bir çoğaltma grubu, yeniden koruma ve şirket içi yeniden çalışma için şu anda desteklenmiyor. Bir çoğaltma grubunun tüm sanal makineleri aynı ana hedefi olmalıdır ve ana hedef sanal makine aynı işletim sisteminin olması gerekir çünkü budur. 
- Geri başarısız olduğunda bir yapılandırma gerekli şirket içi sunucusudur. Yeniden çalışma sırasında sanal makine yapılandırma sunucusu veritabanında mevcut olması gerekir. Aksi takdirde, yeniden çalışma başarısız olur. Yapılandırma sunucunuzun düzenli olarak zamanlanmış yedeklemeler ele emin olun. Bir olağanüstü durum varsa, böylece geri dönme çalışır sunucusuyla aynı IP adresini geri yükleyin.
- Yükü ve yeniden çalışma veri çoğaltmak siteden siteye VPN gerektirir. (Ping) şirket içi yapılandırma sunucusu Azure başarısız üzerinden sanal makinelere erişebilmesi için ağ sağlar. Ayrıca Azure ağında başarısız üzerinden sanal makinenin bir işlem sunucusu dağıtabilirsiniz. Bu işlem sunucusu ayrıca şirket içi yapılandırma sunucusu ile iletişim kurabildiğinden olmalıdır.
- Yük devretme ve yeniden çalışma için aşağıdaki bağlantı noktalarını açın emin olun.

    ![Yük devretme ve yeniden çalışma için bağlantı noktaları](./media/vmware-azure-reprotect/failover-failback.png)

- Bağlantı noktaları ve URL uygulamaları güvenilir listeye almayı tüm ön koşullar okuyabilirsiniz [burada](vmware-azure-deploy-configuration-server.md#prerequisites)

## <a name="deploy-a-process-server-in-azure"></a>Azure'da bir işlem sunucusu Dağıt

Şirket içi sitenize geri başarısız önce bir işlem sunucusu Azure gerekebilir:
- İşlem Sunucusu] azure'da korunan sanal makine verileri alan ve şirket içi siteye verileri gönderir.
- İşlem sunucusu ve korumalı sanal makine arasında düşük gecikme süreli ağ gereklidir. Genel olarak, gecikme süresi bir işlem sunucusu Azure gerekmediğine karar verirken dikkate almanız gerekir:
    - Ayarlanmış bir ExpressRoute bağlantınız varsa, sanal makine ve işlem sunucusu arasında gecikme düşük olduğundan veri göndermek için bir şirket içi işlem sunucusu kullanabilirsiniz.
    - Ancak, yalnızca bir S2S VPN varsa, Azure işlem sunucusu dağıtımı öneririz.
    - Bir Azure tabanlı işlem sunucusu yeniden çalışma sırasında kullanmanızı öneririz. Çoğaltma sanal makinesine (azure'da başarısız üzerinden makine) işlem sunucusu yakınsa çoğaltma performansı yüksektir. Bir--kavram kanıtı için özel eşleme ile şirket içi işlem sunucusu ExpressRoute ile birlikte kullanabilirsiniz.

Aşağıdaki gibi dağıtın:

1. Azure'da bir işlem sunucusu dağıtımı yapmanız gerekirse izleyin [bu yönergeleri](vmware-azure-set-up-process-server-azure.md)
2. Azure sanal makinelerini çoğaltma verilerini işlem sunucusuna gönderir. Azure sanal makinelerini işlem sunucusu erişebilmesi için ağlarını yapılandırın.
3. Unutmayın yalnızca S2S VPN üzerinden veya özel eşleme, ExpressRoute ağ üzerinden azure'dan şirket içi çoğaltma oluşabilir. Yeterli bant genişliği ağ kanal üzerinden kullanılabilir olduğundan emin olun.

## <a name="deploy-a-separate-master-target-server"></a>Ayrı ana hedef sunucusu dağıtma

Ana hedef sunucu yeniden çalışma verilerini alır. Ana hedef sunucu varsayılan olarak şirket için yapılandırma sunucusunda çalışır. Ancak, başarısız geri trafik hacmi bağlı olarak, ayrı ana hedef sunucusu yeniden çalışma için oluşturmanız gerekebilir. Şöyle oluşturmak:

    * [Bir Linux ana hedef sunucusu oluşturmak](vmware-azure-install-linux-master-target.md) Linux VM'ler geri dönmesi için. Bu gereklidir.
    * İsteğe bağlı olarak Windows VM yeniden çalışma için ayrı ana hedef sunucusu oluşturun. Bunu yapmak için birleşik Kurulum'u yeniden çalıştırın ve bir ana hedef sunucusu oluşturmak için seçin. [Daha fazla bilgi edinin](physical-azure-set-up-source.md#run-azure-site-recovery-unified-setup).

Bir ana hedef sunucusu oluşturduktan sonra aşağıdakileri yapın:

- Sanal makine mevcut şirket içi vCenter sunucusunda ise, ana hedef sunucusu şirket içi sanal makinenin VMDK erişimi olmalıdır. Erişim için sanal makinenin disklerinin çoğaltılan veri yazmak için gereklidir. Şirket içi sanal makinenin veri deposu okuma/yazma erişimine sahip ana hedefin ana bilgisayarına bağlandığından emin olun.
- Sanal makinenin mevcut şirket içi vCenter sunucusunda değilse, Site Recovery hizmetine yeni bir sanal makine yükü sırasında oluşturması gerekir. Bu sanal makine ana hedef oluşturma ESX ana bilgisayarda oluşturulur. Yeniden çalışma sanal makine istediğiniz konakta oluşturulan ESX konak dikkatlice seçin.
- Ana hedef sunucusu * için depolama VMotion'ı kullanamazsınız. Bu, yeniden çalışma başarısız olmasına neden olabilir. Diskleri için kullanılabilir olmadığından, sanal makine başlatılamıyor. Bu sorunu önlemek için ana hedef sunucusu VMotion'ı listenizden dışlayın.
- Bir ana hedef depolama VMotion'ı görev yükü sonra uğradığında, VMotion'ı görevin hedefi için ana hedefe bağlı korumalı sanal makineleri diskler geçirin. Bundan sonra başarısız çalışırsanız, diskleri bulunamadı ayrılmayı diskin başarısız olur. Ardından, diskleri depolama hesaplarınızı bulmak zorlaşır. El ile bulmak ve bunları sanal makineye İliştir gerekir. Bundan sonra şirket içi sanal makine önyüklendiğinde.
- Saklama sürücüsünün var olan Windows ana hedef sunucunuzu ekleyin. Yeni bir disk ekleyin ve sürücü biçimlendirin. Saklama sürücüsünün sanal makine şirket içi siteye ne zaman çoğaltılacağını zaman içinde nokta durdurmak için kullanılır. Saklama sürücüsünün bazı ölçütlere aşağıda verilmiştir. Bu ölçütler sürücü için ana hedef sunucusunda yer almaz.
    - Birim, çoğaltma hedefi gibi başka bir amaç için kullanılmaz.
    - Birimin kilidi modunda değil.
    - Birimi bir önbellek birimi değil. Ana hedef yükleme bu birimde mevcut döndürmemelidir. İşlem sunucusu ve ana hedef için özel yükleme birimi bekletme birimi için uygun değil. İşlem sunucusu ve ana hedef birimde yüklendiğinde, birim ana hedef önbellek birimdir.
    - Dosya sistemi birimi FAT veya FAT32 türünde değil.
    - Birim kapasitesi sıfırdan farklı.
    - Windows için varsayılan saklama biriminin R birimdir.
    - Linux için varsayılan saklama biriminin /mnt/retention ' dir.
- Var olan bir işlem sunucusu/configuration server makine veya bir ölçekte veya bir işlem sunucusu/ana hedef sunucu makinesi kullanıyorsanız, yeni bir sürücü eklemeniz gerekir. Yeni sürücü önceki gereksinimlerini karşılaması. Saklama sürücüsünün mevcut değilse, portal seçimi aşağı açılan listede görünmüyor. Şirket içi ana hedef için bir sürücü ekledikten sonra portal seçimini görünmesi sürücü 15 dakika kadar alır. Sürücü 15 dakika sonra görünmüyorsa, yapılandırma sunucusu de yenileyebilirsiniz.
- VMware araçları veya açık vm araçları ana hedef sunucuya yükleyin. Araçlar olmadan ana Hedef'in ESXi konağına üzerinde datastores algılanamıyor.
- Ayarlama `disk.EnableUUID=true` VMware ana hedef sanal makine yapılandırma parametrelerini ayarlama. Bu satır mevcut değilse, bunu ekleyin. Bu ayar, böylece doğru bağlar (VMDK) sanal makine diski için tutarlı bir UUID sağlamak için gereklidir.
- Ana hedef oluşturulduğu ESX konak en az bir VMFS veri deposu bağlı olması gerekir. Varsa none, **veri deposu** yeniden koruma sayfasında giriş boş olacaktır ve devam edemiyor.
- Ana hedef sunucusu anlık görüntüleri disklerde sahip olamaz. Anlık görüntüler varsa, yükü ve yeniden çalışma başarısız.
- Ana hedef Paravirtual SCSI denetleyicisine sahip olamaz. Denetleyici yalnızca bir LSI Logic denetleyicisi olabilir. LSI Logic denetleyicisi, yükü başarısız olur.
- Belirli bir örneğe bağlı atmst 60 disk ana hedef olabilir. Şirket içi ana hedef korunmuş sanal makine sayısı diskleri 60'den fazla Toplamı toplam sayısı sahip sonra ana hedefe yeniden korunmasını sağlayan, başarısız olan başlatılır. Disk yuvaları hedef veya ek ana hedef sunucusu dağıtın yeterli ana olduğundan emin olun.
    

## <a name="enable-reprotection"></a>Yükü Etkinleştir

Azure'da bir sanal makine önyükleme sonra yapılandırmayı sunucuya geri (en fazla 15 dakika) kaydetmek aracı için biraz zaman alabilir. Bu süre boyunca koruyun erişemezsiniz ve aracı yüklü olmadığı bir hata iletisi gösterir. Bu durumda, birkaç dakika bekleyin ve ardından yükü yeniden deneyin.


1. İçinde **kasa** > **öğeleri çoğaltılan**üzerinden başarısız sanal makineye sağ tıklayın ve ardından **yeniden koruma**. Ayrıca makine tıklatın ve seçin **yeniden koruma** komutu düğmelerle.
2. Koruma, yönünü doğrulayın **şirket içi Azure**, zaten seçilidir.
3. İçinde **ana hedef sunucusu** ve **işlem sunucusu**, şirket içi ana hedef sunucusu ve işlem sunucusu seçin.  
4. İçin **veri deposu**, diskleri şirket içi kurtarmak istediğiniz veri deposu seçin. Bu seçenek şirket içi sanal makine silinir ve yeni disk oluşturmanız gerektiğinde kullanılır. Bu seçenek diskleri zaten var, ancak yine de bir değer belirtmeniz gerekiyorsa, göz ardı edilir.
5. Saklama sürücüsünün seçin.
6. Yeniden çalışma ilkesi otomatik olarak seçilir.
7. Yeniden korumaya başlamak için **Tamam**’a tıklayın. Sanal makineyi Azure’dan şirket içi siteye çoğaltan bir iş başlar. **İşler** sekmesinde ilerleme durumunu izleyebilirsiniz. Sanal makinenin yükü başarılı olduktan sonra korumalı bir duruma girer.

Şunlara dikkat edin:
- (Şirket içi sanal makine silindiğinde) alternatif konuma kurtarma yapmak istiyorsanız, bekletme sürücüsü ve ana hedef sunucu için yapılandırılmış veri deposu seçin. Şirket içi sitede yeniden çalıştırmak, yeniden çalışma koruma planı VMware sanal makineleri aynı veri deposu ana hedef sunucusu olarak kullanın. Yeni bir sanal makine ardından Vcenter'da oluşturulur.
- Var olan bir şirket içi sanal makine için Azure üzerindeki sanal makineyi kurtarmak istiyorsanız, şirket içi sanal makinenin datastores okuma/yazma erişimi olan ana hedef sunucunun ESXi ana bilgisayarda bağlayın.
    ![İletişim kutusunu yeniden koruma](./media/vmware-azure-reprotect/reprotectinputs.png)

- Ayrıca bir kurtarma planı düzeyinde koruyun. Bir çoğaltma grubu yalnızca bir kurtarma planı reprotected. Bir kurtarma planı kullanarak koruyun, korunan her makine için değerler sağlamanız gerekir.
- Çoğaltma grubunu yeniden korumak için aynı hedef sunucuyu kullanın. Çoğaltma grubunu yeniden korumak için farklı bir ana hedef sunucu kullanırsanız, sunucu ortak bir zaman noktası sağlayamaz.
- Şirket içi sanal makine yükü sırasında kapalıdır. Bu işlem, çoğaltma sırasında veri tutarlığını sağlar. Yükü tamamlandıktan sonra sanal makinede etkinleştirmeyin.



## <a name="common-issues"></a>Genel sorunlar


- Şu anda yalnızca bir sanal makine dosya sistemi (VMFS) veya vSAN veri deposu için geri başarısız olan Site Recovery destekler. NFS veri deposu desteklenmiyor. Bu sınırlama nedeniyle, veri deposu seçimi ekran NFS datastores durumunda boş veya vSAN veri deposu gösterir ancak işi sırasında başarısız üzerinde yeniden koruma girin. Geri dönecek yapmak istiyorsanız, şirket içi bir VMFS veri deposu oluşturmak ve yeniden başarısız. Bu geri dönme VMDK tamamını indirmeyi neden olur.
- Salt okunur kullanıcı vCenter bulma işlemini ve sanal makineleri koruma, koruma başarılı ve yük devretme çalışır. Yükü sırasında datastores bulunan yük devretme başarısız olur. Bir belirti datastores yükü sırasında listelenmeyen olmasıdır. Bu sorunu gidermek için izinlere sahip uygun bir hesap ile vCenter kimlik bilgileri güncelleştirmek ve işi yeniden deneyin. 
- Linux sanal makine yeniden çalışmak ve şirket içi çalıştırın, ağ yöneticisi paket makineden kaldırıldı görebilirsiniz. Sanal makine Azure'da kurtarıldığında ağ yöneticisi paket kaldırıldığı bu kaldırma işlemi gerçekleşir.
- Linux sanal makine Azure'a yük devretti ve statik IP adresiyle yapılandırılmış olduğunda, IP adresi DHCP'den alınır. Şirket içi üzerinden başarısız olduğunda, sanal makinenin IP adresini almak için DHCP kullanmaya devam eder. El ile makinede oturum açmak ve IP adresini geri bir statik adrese gerekirse ayarlayın. Windows sanal makinesi statik IP yeniden elde edebilirsiniz.
- ESXi 5.5 ücretsiz sürüm veya 6 vSphere hiper yönetici ücretsiz sürümü kullanıyorsanız, yük devretme başarılı olur, ancak geri dönme başarılı olmaz. Yeniden çalışma etkinleştirmek için her iki programın değerlendirme lisansına yükseltme.
- Yapılandırma sunucusuna işlem sunucusundan bağlanamazsa, Telnet 443 numaralı bağlantı noktasını yapılandırma sunucusunda bağlantısını denetlemek için kullanın. Yapılandırma sunucusuna işlem sunucusundan ping deneyebilirsiniz. Yapılandırma sunucusuna bağlandığında bir işlem sunucusu bir sinyal de olmalıdır.
- Fiziksel şirket içi sunucu Azure'dan şirket içi siteye başarısız olamaz korunan bir Windows Server 2008 R2 SP1 sunucusu.
- Aşağıdaki durumlarda kapatamazsınız:
    - Makineler için Azure geçirildi. [Daha fazla bilgi edinin](migrate-overview.md#what-do-we-mean-by-migration).
    - Bir VM başka bir kaynak grubuna taşındı.
    - Azure VM silindi.
    - VM korumasını devre dışı.
    - VM el ile Azure'da oluşturduğunuz. Makine başlangıçta korunan şirket içi edilmiş ve Azure'a önce yeniden koruma devredilir gerekir.
    - Yalnızca bir ESXi konağına başarısız olabilir. Yeniden çalışma VMware Vm'lerini veya fiziksel sunucuları Hyper-V konakları, fiziksel makineleri ya da VMware iş istasyonlarının yapamazsınız.


## <a name="next-steps"></a>Sonraki adımlar

Sanal makinenin korumalı bir duruma geçtiğini sonra [bir yeniden başlatma](vmware-azure-failback.md). Yeniden çalışma azure'daki sanal makineyi kapatın ve şirket içi sanal makine önyükleme. Bazı uygulama kesintiler bekler. Uygulama kapalı kalma süresi dayanabileceği yeniden çalışma için bir saat seçin.

