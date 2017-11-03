---
title: "Azure'dan şirket içi siteye yeniden koruyun. | Microsoft Docs"
description: "Azure VM'ler, yük devretme sonrasında geri şirket içi sanal makineleri getirmek için yeniden çalışma başlatabilirsiniz. Yeniden çalışma önce koruyun öğrenin."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: 3644b41c3e3293a263bd9ff996d4e3d26417aeed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="reprotect-from-azure-to-an-on-premises-site"></a>Azure'dan şirket içi siteye yeniden koruyun.



## <a name="overview"></a>Genel Bakış
Bu makalede, Azure sanal makinelerden Azure bir şirket içi siteye koruyun açıklar. Başarısız olmaya hazır olduğunuzda, bu makaledeki yönergeleri için Azure site bunlar şirket içi devredilir sonra fiziksel sunucuları, VMware sanal makineleri veya Windows/Linux geri izleyin (açıklandığı gibi [çoğaltmak VMware sanal makineler ve fiziksel sunucuları Azure Site Recovery ile azure'a](site-recovery-failover.md)).

> [!WARNING]
> Ya da sonra geri dönme olamaz [tamamlandı geçiş](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), bir sanal makine başka bir kaynak grubuna taşınmış veya Azure sanal makine silindi. Sanal makinenin korumasını devre dışı bırakırsanız, yeniden çalışma yapamazsınız.


Yükü tamamlandıktan sonra korumalı sanal makineleri çoğaltma, şirket içi siteye getirmek için sanal makinelerde geri dönme başlatabilirsiniz.

> [!NOTE]
> Yalnızca yeniden koruma ve ESXi konağa yeniden kullanabilirsiniz. Yeniden çalışma Hyper-v konakları veya VMware iş istasyonlarının veya başka bir sanallaştırma platformu sanal makineye uygulanamaz.

POST yorumlarınızı ve sorularınızı bu makalenin veya sonunda [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Hızlı bir genel bakış için Azure'dan şirket içi siteye yük devri konusunda aşağıdaki videoyu izleyin.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="prerequisites"></a>Ön koşullar

> [!IMPORTANT]
> Azure'a yük devretme sırasında şirket içi site erişilebilir olmayabilir ve yapılandırma sunucusu ya da bu nedenle olabilir beklemediğiniz kullanılabilir veya kapatın. Yeniden koruma ve yeniden çalışma sırasında şirket içi yapılandırma sunucusu çalışır ve bağlı OK durumda olması gerekir.


Sanal makineler yeniden korumak hazırlarken alın veya aşağıdaki önkoşul eylemleri göz önünde bulundurun:

* Bir vCenter sunucusu geri devretmek istediğiniz sanal makineleri yönetir, bilgisayarınızda yüklü olduğundan emin olun [gerekli izinleri](site-recovery-vmware-to-azure-classic.md) vCenter sunucuları üzerindeki sanal makinelerin bulma.

  > [!WARNING]
  > Şirket içi ana hedef veya sanal makine anlık görüntüler varsa yükü başarısız olur. Yeniden korumak için devam etmeden önce anlık görüntüleri ana hedefte silebilirsiniz. Sanal makinenin anlık görüntüleri otomatik olarak yeniden koruma işi sırasında birleştirilir.

* Geri dönecek önce iki ek bileşeni oluşturun:

  * **İşlem sunucusu**: [işlem sunucusu](site-recovery-vmware-setup-azure-ps-resource-manager.md) azure'da korunan sanal makine verileri alan ve şirket içi siteye verileri gönderir. İşlem sunucusu ve korumalı sanal makine arasında düşük gecikme süreli ağ gereklidir. Bu nedenle, bir Azure ExpressRoute bağlantısı veya bir Azure tabanlı işlem sunucusu ve VPN kullanıyorsanız, bir şirket içi işlem sunucusu olabilir.
  
  * **Ana hedef sunucusu**: ana hedef sunucusu yeniden çalışma verileri alır. Oluşturduğunuz şirket içi yönetim sunucusu varsayılan olarak yüklenen bir ana hedef sunucusu vardır. Ancak, başarısız geri trafik hacmi bağlı olarak, ayrı ana hedef sunucusu yeniden çalışma için oluşturmanız gerekebilir.
    * [Linux sanal makine bir Linux ana hedef sunucusunun gerekiyor](site-recovery-how-to-install-linux-master-target.md).
    * Windows sanal makine bir Windows ana hedef sunucusu gerekir. Şirket içi işlem sunucusu ve ana hedef makinelere yeniden kullanabilirsiniz.

    Ana hedef listelenen diğer Önkoşullar sahip [bir ana hedef yeniden koruma önce denetlemek için ortak interneti](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server).

* Geri başarısız olduğunda bir yapılandırma gerekli şirket içi sunucusudur. Yeniden çalışma sırasında sanal makine yapılandırma sunucusu veritabanında mevcut olması gerekir. Aksi takdirde, yeniden çalışma başarısız olur. 

> [!IMPORTANT]
> Yapılandırma sunucunuzun düzenli olarak zamanlanmış yedeklemeler ele emin olun. Bir olağanüstü durum varsa, böylece geri dönme çalışır sunucusuyla aynı IP adresini geri yükleyin.

* Ayarlama `disk.EnableUUID=true` VMware ana hedef sanal makine yapılandırma parametrelerini ayarlama. Bu satır mevcut değilse, bunu ekleyin. Bu ayar, böylece doğru bağlar (VMDK) sanal makine diski için tutarlı bir UUID sağlamak için gereklidir.

* *Ana hedef sunucusunda depolama VMotion'ı kullanamazsınız*. Bu, yeniden çalışma başarısız olmasına neden olabilir. Diskleri için kullanılabilir olmadığından, sanal makine başlatılamıyor. Bu sorunu önlemek için ana hedef sunucusu VMotion'ı listenizden dışlayın.

* Ana hedef sunucuya yeni bir sürücü ekleyin: bekletme sürücüsü. Yeni bir disk ekleyin ve sürücü biçimlendirin.


### <a name="frequently-asked-questions"></a>Sık sorulan sorular

#### <a name="why-do-i-need-a-s2s-vpn-or-an-expressroute-connection-to-replicate-data-back-to-the-on-premises-site"></a>S2S VPN veya ExpressRoute bağlantısı geri şirket içi siteye veri çoğaltmak için neden ihtiyacım var mı?
Şirket içi çoğaltma Azure için Internet veya ortak eşleme, yükü ve veri çoğaltmak siteden siteye (S2S) VPN gerektiren geri dönme sahip bir ExpressRoute bağlantı üzerinden oluşabilir ise. (Ping) şirket içi yapılandırma sunucusu Azure başarısız üzerinden sanal makinelere erişebilmesi için ağ sağlar. Ayrıca Azure ağında başarısız üzerinden sanal makinenin bir işlem sunucusu dağıtabilirsiniz. Bu işlem sunucusu ayrıca şirket içi yapılandırma sunucusu ile iletişim kurabildiğinden olmalıdır.

#### <a name="when-should-i-install-a-process-server-in-azure"></a>Bir işlem sunucusu Azure'da yüklediğimde?


Koruyun istediğiniz Azure sanal makinelerde çoğaltma verilerini işlem sunucusuna gönderir. Azure sanal makinelerde işlem sunucusu erişebilmesi için ağınızı ayarlayın.

Bir işlem sunucusu azure'da dağıtmak veya yük devretme sırasında kullanılan mevcut işlem sunucusu kullanın. Gecikme ve işlem sunucusuna Azure ile ilgili sanal makinelerden veri göndermek için dikkate alınması gereken önemli noktadır.

Ayarlanmış bir ExpressRoute bağlantınız varsa, sanal makine ve işlem sunucusu arasında gecikme düşük olduğundan veri göndermek için bir şirket içi işlem sunucusu kullanabilirsiniz.

 ![ExpressRoute için mimarisi diyagramı](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)



Ancak, yalnızca bir S2S VPN varsa, Azure işlem sunucusu dağıtımı öneririz.

 ![VPN mimarisi diyagramı](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)


Unutmayın yalnızca S2S VPN veya özel eşleme, ExpressRoute ağ azure'dan şirket içi çoğaltma gerçekleşebilir. Yeterli bant genişliği ağ kanal üzerinden kullanılabilir olduğundan emin olun.

Bir Azure tabanlı işlem sunucusu yükleme hakkında daha fazla bilgi için bkz: [Azure'da çalışan bir işlem sunucusu yönetmek](site-recovery-vmware-setup-azure-ps-resource-manager.md).

> [!TIP]
> Bir Azure tabanlı işlem sunucusu yeniden çalışma sırasında kullanmanızı öneririz. Çoğaltma sanal makinesine (azure'da başarısız üzerinden makine) işlem sunucusu yakınsa çoğaltma performansı yüksektir. Ancak, kanıtınızı prototip (PT) ya da gösterim sırasında ExpressRoute yanı sıra şirket içi işlem sunucusu özel eşleme ile POC daha hızlı tamamlamak için kullanabilirsiniz.

> [!NOTE]
> Şirket içi çoğaltma Azure için yalnızca Internet veya ExpressRoute genel eşliği ile gerçekleşebilir. Şirket içi Azure çoğaltmayı yalnızca S2S VPN ya da ExpressRoute özel eşleme ile oluşabilir


#### <a name="what-ports-should-i-open-on-different-components-so-that-reprotection-can-work"></a>Yükü çalışabilmeniz için hangi bağlantı noktalarının t farklı bileşenleri açmalı mısınız?

![Yük devretme ve yeniden çalışma için bağlantı noktaları](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

#### <a name="which-master-target-server-should-i-use-for-reprotection"></a>Yükü için hangi ana hedef sunucusu kullanmalıyım?
Bir şirket içi ana hedef sunucusuna işlem sunucusundan verileri almak ve şirket içi sanal makinenin VMDK'ye yazmak için gereklidir. Windows sanal makineleri koruyorsanız, Windows ana hedef sunucusu gerekir. Şirket içi işlem sunucusu ve ana hedef yeniden<!-- !todo component -->. Linux sanal makineleri için şirket içinde ek bir Linux ana hedefinin ayarlamanız gerekir.


Bir ana hedef sunucusu yükleme hakkında daha fazla bilgi için bkz:

* [Windows ana hedef sunucusu nasıl yüklenir](site-recovery-vmware-to-azure.md)
* [Linux ana hedef sunucusu nasıl yüklenir](site-recovery-how-to-install-linux-master-target.md)


#### <a name="what-datastore-types-are-supported-on-the-on-premises-esxi-host-during-failback"></a>Hangi veri deposu tür şirket içi ESXi ana bilgisayarda yeniden çalışma sırasında destekleniyor mu?

Şu anda yalnızca bir sanal makine dosya sistemi (VMFS) veya vSAN veri deposu için geri başarısız olan Azure Site Recovery destekler. NFS veri deposu desteklenmiyor. Bu sınırlama nedeniyle, veri deposu seçimi ekran NFS datastores durumunda boş veya vSAN veri deposu gösterir ancak işi sırasında başarısız üzerinde yeniden koruma girin. Geri dönecek yapmak istiyorsanız, şirket içi bir VMFS veri deposu oluşturmak ve yeniden başarısız. Bu geri dönme VMDK tamamını indirmeyi neden olur.

### <a name="common-things-to-check-after-completing-installation-of-the-master-target-server"></a>Ana hedef sunucusu yüklemesini tamamladıktan sonra denetlemek için ortak interneti

* Sanal makine mevcut şirket içi vCenter sunucusunda ise, ana hedef sunucusu şirket içi sanal makinenin VMDK erişimi olmalıdır. Erişim için sanal makinenin disklerinin çoğaltılan veri yazmak için gereklidir. Şirket içi sanal makinenin veri deposu okuma/yazma erişimine sahip ana hedefin ana bilgisayarına bağlandığından emin olun.

* Sanal makinenin mevcut şirket içi vCenter sunucusunda değilse, Site Recovery hizmetine yeni bir sanal makine yükü sırasında oluşturması gerekir. Bu sanal makine ana hedef oluşturma ESX ana bilgisayarda oluşturulur. Yeniden çalışma sanal makine istediğiniz konakta oluşturulan ESX konak dikkatlice seçin.

* *Ana hedef sunucusu için depolama VMotion'ı kullanamazsınız*. Bu, yeniden çalışma başarısız olmasına neden olabilir. Diskleri için kullanılabilir olmadığından, sanal makine başlatılamıyor.

  > [!WARNING]
  > Bir ana hedef depolama VMotion'ı görev yükü sonra uğradığında, VMotion'ı görevin hedefi için ana hedefe bağlı korumalı sanal makineleri diskler geçirin. Bundan sonra başarısız çalışırsanız, diskleri bulunamadı ayrılmayı diskin başarısız olur. Ardından, diskleri depolama hesaplarınızı bulmak zorlaşır. El ile bulmak ve bunları sanal makineye İliştir gerekir. Bundan sonra şirket içi sanal makine önyüklendiğinde.

* Saklama sürücüsünün var olan Windows ana hedef sunucunuzu ekleyin. Yeni bir disk ekleyin ve sürücü biçimlendirin. Saklama sürücüsünün sanal makine şirket içi siteye ne zaman çoğaltılacağını zaman içinde nokta durdurmak için kullanılır. Saklama sürücüsünün bazı ölçütlere aşağıda verilmiştir. Bu ölçütler sürücü için ana hedef sunucusunda yer almaz.

   * Birim, çoğaltma hedefi gibi başka bir amaç için kullanılmaz.

   * Birimin kilidi modunda değil.

   * Birimi bir önbellek birimi değil. Ana hedef yükleme bu birimde mevcut döndürmemelidir. İşlem sunucusu ve ana hedef için özel yükleme birimi bekletme birimi için uygun değil. İşlem sunucusu ve ana hedef birimde yüklendiğinde, birim ana hedef önbellek birimdir.

   * Dosya sistemi birimi FAT veya FAT32 türünde değil.

   * Birim kapasitesi sıfırdan farklı.

   * Windows için varsayılan saklama biriminin R birimdir.

   * Linux için varsayılan saklama biriminin /mnt/retention ' dir.

  > [!IMPORTANT]
  > Var olan bir işlem sunucusu/configuration server makine veya bir ölçekte veya bir işlem sunucusu/ana hedef sunucu makinesi kullanıyorsanız, yeni bir sürücü eklemeniz gerekir. Yeni sürücü önceki gereksinimlerini karşılaması. Saklama sürücüsünün mevcut değilse, portal seçimi aşağı açılan listede görünmüyor. Şirket içi ana hedef için bir sürücü ekledikten sonra portal seçimini görünmesi sürücü 15 dakika kadar alır. Sürücü 15 dakika sonra görünmüyorsa, yapılandırma sunucusu de yenileyebilirsiniz.

* Linux başarısız üzerinden sanal makine bir Linux ana hedef sunucusu gerektirir. Windows başarısız üzerinden sanal makine bir Windows ana hedef sunucusu gerektirir.

* VMware araçları ana hedef sunucuya yükleyin. VMware araçları ana Hedef'in ESXi konağına üzerinde datastores algılanamıyor.

* Etkinleştirme `disk.EnableUUID=true` vCenter özelliklerini kullanarak ana hedef sanal makinedeki parametresi. <!-- !todo Needs link. -->

* Ana hedef en az bir VMFS veri deposu bağlı olması gerekir. Varsa none, **veri deposu** yeniden koruma sayfasında giriş boş olacaktır ve devam edemiyor.

* Ana hedef sunucusu anlık görüntüleri disklerde sahip olamaz. Anlık görüntüler varsa, yükü ve yeniden çalışma başarısız.

* Ana hedef Paravirtual SCSI denetleyicisine sahip olamaz. Denetleyici yalnızca bir LSI Logic denetleyicisi olabilir. LSI Logic denetleyicisi, yükü başarısız olur.

<!--
### Failback policy
To replicate back to on-premises, you will need a failback policy. This policy get automatically created when you create a forward direction policy. Note that

1. This policy gets auto associated with the configuration server during creation.
2. This policy is not editable.
3. The set values of the policy are (RPO Threshold = 15 Mins, Recovery Point Retention = 24 Hours, App Consistency Snapshot Frequency = 60 Mins)
   ![](./media/site-recovery-failback-azure-to-vmware-new/failback-policy.png)

-->

## <a name="steps-to-reprotect"></a>Yeniden korumak için adımlar

> [!NOTE]
> Azure'da bir sanal makine önyükleme sonra yapılandırmayı sunucuya geri (en fazla 15 dakika) kaydetmek aracı için biraz zaman alabilir. Bu süre boyunca yükü başarısız olur ve aracı yüklü olmadığı bir hata iletisi gösterir. Birkaç dakika bekleyin ve ardından yükü yeniden deneyin.



1. İçinde **kasa** > **öğeleri çoğaltılan**üzerinden başarısız sanal makineye sağ tıklayın ve ardından **yeniden koruma**. Ayrıca makine tıklatın ve seçin **yeniden koruma** komutu düğmelerle.
2. Dikey penceresinde dikkat koruma, yönünü **şirket içi Azure**, zaten seçilidir.
3. İçinde **ana hedef sunucusu** ve **işlem sunucusu**, şirket içi ana hedef sunucusu ve işlem sunucusu seçin.
4. İçin **veri deposu**, diskleri şirket içi kurtarmak istediğiniz veri deposu seçin. Bu seçenek şirket içi sanal makine silinir ve yeni disk oluşturmanız gerektiğinde kullanılır. Bu seçenek diskleri zaten var, ancak yine de bir değer belirtmeniz gerekiyorsa, göz ardı edilir.
5. Saklama sürücüsünün seçin.
6. Yeniden çalışma ilkesi otomatik olarak seçilir.
7. Tıklatın **Tamam** yükü başlamak için. Sanal makine Azure'dan şirket içi siteye çoğaltmak bir iş başlatır. Üzerinde ilerleme durumunu izleyebilirsiniz **işleri** sekmesi.

(Şirket içi sanal makine silindiğinde) alternatif konuma kurtarma yapmak istiyorsanız, bekletme sürücüsü ve ana hedef sunucu için yapılandırılmış veri deposu seçin. Şirket içi sitede yeniden çalıştırmak, yeniden çalışma koruma planı VMware sanal makineleri aynı veri deposu ana hedef sunucusu olarak kullanın. Yeni bir sanal makine ardından Vcenter'da oluşturulur.

Var olan bir şirket içi sanal makine için Azure üzerindeki sanal makineyi kurtarmak istiyorsanız, şirket içi sanal makinenin datastores okuma/yazma erişimi olan ana hedef sunucunun ESXi ana bilgisayarda bağlayın.
    ![İletişim kutusunu yeniden koruma](./media/site-recovery-failback-azure-to-vmware-new/reprotectinputs.png)

Ayrıca bir kurtarma planı düzeyinde koruyun. Bir çoğaltma grubu yalnızca bir kurtarma planı reprotected. Bir kurtarma planı kullanarak koruyun, korunan her makine için değerler sağlamanız gerekir.

> [!NOTE]
> Aynı ana hedef sunucusunda bir çoğaltma grubunu yeniden korumak için kullanın. Bir çoğaltma grubunu yeniden korumak için farklı bir ana hedef sunucusu kullanıyorsanız, sunucunun ortak bir noktaya zamanında sağlayamaz.

> [!NOTE]
> Şirket içi sanal makine yükü sırasında kapalıdır. Bu, çoğaltma sırasında veri tutarlılığını sağlamaya yardımcı olur. Yükü tamamlandıktan sonra sanal makinede etkinleştirmeyin.

Sanal makinenin yükü başarılı olduktan sonra korumalı bir duruma girer.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinenin korumalı bir duruma geçtiğini sonra [bir yeniden başlatma](site-recovery-how-to-failback-azure-to-vmware.md#steps-to-fail-back). 

Yeniden çalışma azure'daki sanal makineyi kapatın ve şirket içi sanal makine önyükleme. Bazı uygulama kesintiler bekler. Uygulama kapalı kalma süresi dayanabileceği yeniden çalışma için bir saat seçin.

## <a name="common-problems"></a>Sık karşılaşılan sorunları

* Sanal makineler oluşturmak için bir şablon kullanıldığında, her bir sanal makine disklerin kendi UUID olduğundan emin olun. Her ikisi de aynı şablonu oluşturulmadığından şirket içi sanal makinenin UUID, ana hedef çakışıyor yükü başarısız olur. Aynı şablon kullanılarak oluşturulan olmayan başka bir ana hedef dağıtın.

* Salt okunur kullanıcı vCenter bulma işlemini ve sanal makineleri koruma, koruma başarılı ve yük devretme çalışır. Yükü sırasında datastores bulunan yük devretme başarısız olur. Bir belirti datastores yükü sırasında listelenmeyen olmasıdır. Bu sorunu gidermek için izinlere sahip uygun bir hesap ile vCenter kimlik bilgileri güncelleştirmek ve işi yeniden deneyin. Daha fazla bilgi için bkz: [çoğaltmak VMware sanal makineleri ve fiziksel sunucuları Azure Site Recovery ile azure'a](site-recovery-vmware-to-azure-classic.md).

* Linux sanal makine yeniden çalışmak ve şirket içi çalıştırın, ağ yöneticisi paket makineden kaldırıldı görebilirsiniz. Sanal makine Azure'da kurtarıldığında ağ yöneticisi paket kaldırıldığı bu kaldırma işlemi gerçekleşir.

* Linux sanal makine Azure'a yük devretti ve statik IP adresiyle yapılandırılmış olduğunda, IP adresi DHCP'den alınır. Şirket içi üzerinden başarısız olduğunda, sanal makinenin IP adresini almak için DHCP kullanmaya devam eder. El ile makinede oturum açmak ve IP adresini geri bir statik adrese gerekirse ayarlayın. Windows sanal makinesi statik IP yeniden elde edebilirsiniz.

* ESXi 5.5 ücretsiz sürüm veya 6 vSphere hiper yönetici ücretsiz sürümü kullanıyorsanız, yük devretme başarılı olur, ancak geri dönme başarılı olmaz. Yeniden çalışma etkinleştirmek için her iki programın değerlendirme lisansına yükseltme.

* Yapılandırma sunucusuna işlem sunucusundan bağlanamazsa, Telnet 443 numaralı bağlantı noktasını yapılandırma sunucusunda bağlantısını denetlemek için kullanın. Yapılandırma sunucusuna işlem sunucusundan ping deneyebilirsiniz. Yapılandırma sunucusuna bağlandığında bir işlem sunucusu bir sinyal de olmalıdır.

* Alternatif bir vCenter başarısız çalışıyorsanız, yeni vCenter ve ana hedef sunucusunda bulunan emin olun. Tipik bir belirti datastores erişilebilir veya görünür durumda olmamasıdır **koruyun** iletişim kutusu.

* Fiziksel şirket içi sunucu Azure'dan şirket içi siteye başarısız olamaz korunan bir Windows Server 2008 R2 SP1 sunucusu.

### <a name="common-error-codes"></a>Genel hata kodları

#### <a name="error-code-95226"></a>Hata kodu 95226

*Azure sanal makinesi şirket içi yapılandırma sunucusuna erişemediği için olmadığından yeniden koruma başarısız oldu.*

Böyle olduğunda 
1. Azure sanal makinesi şirket içi yapılandırma sunucusuna erişemediği ve bu nedenle değil bulunmalı ve yapılandırma sunucusuna kayıtlı. 
2. Post yük devretme iletişim kurmak için şirket içi yapılandırma sunucusuna çalıştırılması gerekiyor Azure sanal makinede Inmage Scout uygulama hizmeti çalışmıyor.

Bu sorunu gidermek için
1. Sanal makine şirket içi yapılandırma sunucusu ile iletişim kurabilmesi gibi Azure sanal makinesinin ağ yapılandırıldığından emin olmanız gerekir. Bunu yapmak için şirket içi veri merkeziniz dön siteden siteye VPN ayarlamak veya özel Azure sanal makinenin sanal ağ eşleme ile bir ExpressRoute bağlantı yapılandırabilirsiniz. 
2. Azure sanal makinesi şirket içi yapılandırma sunucusu ile iletişim kurabilen sağlayacak şekilde yapılandırılmış bir ağ zaten varsa, sanal makinede oturum açın ve 'Inmage Scout Application Service' denetleyin. Inmage Scout Application Service çalışmadığını görüyorsanız hizmeti elle başlatın ve hizmet başlangıç türünü otomatik olarak ayarlandığından emin olun.

### <a name="error-code-78052"></a>Hata kodu 78052
Yeniden koruma hata iletisiyle başarısız olur: *sanal makine için koruma tamamlanamadı.*

Bu iki nedenden kaynaklanabilir
1. Sanal makineyi yeniden korumayı bir Windows Server 2016 ' dir. Curently bu işletim sistemi için yeniden çalışma desteklenmez, ancak çok yakında desteklenecek.
2. Zaten var. ana aynı ada sahip bir sanal makine hedef sunucunun geri başarısız oluyor.

Bu sorunu gidermek için yeniden koruma makine nerede adları değil birbiriyle çakışır farklı bir konak üzerinde oluşturacak böylece başka bir ana bilgisayardaki farklı bir ana hedef sunucusu seçebilirsiniz. Ayrıca, VMotion'ı farklı bir konak adı çakışma değil nerede olacağını ana hedefe erişebilirsiniz. Var olan sanal makine parazit makine ise, böylece yeni bir sanal makine aynı ESXi ana bilgisayarda oluşturulan yalnızca onu yeniden adlandırabilirsiniz.

### <a name="error-code-78093"></a>Hata kodu 78093

*VM, askıdaki bir durumda veya erişilebilir çalışmıyor.*

Sanal makineye geri şirket içi üzerinden başarısız koruyun çalıştıran Azure sanal makine gerekir. Mobility hizmetinin yapılandırma sunucusuyla kaydeder böylece budur şirket içi ve işlem sunucusu ile iletişim kurarak çoğaltma başlatabilirsiniz. Makine yanlış bir ağ veya (askıda durumu veya kapatma) çalışmıyor ise, yapılandırma sunucusunu yeniden koruma başlamak için sanal makinede mobility hizmeti ulaşamıyor. Geri şirket içi iletişim başlatılabilmesi sanal makineyi yeniden başlatabilirsiniz. Azure sanal makinesini başlattıktan sonra yeniden koruma işi yeniden başlatın

### <a name="error-code-8061"></a>Hata kodu 8061

*Veri deposu ESXi ana bilgisayardan erişilebilir değil.*

Başvurmak [ana hedef ön koşullar](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server) ve [destek datastores](site-recovery-how-to-reprotect.md#what-datastore-types-are-supported-on-the-on-premises-esxi-host-during-failback) yeniden çalışma için

