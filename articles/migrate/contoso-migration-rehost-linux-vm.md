---
title: Rehost geçirmek ve Azure sanal makineleri şirket içi Linux uygulamayı rehost | Microsoft Docs
description: Nasıl Contoso yeniden barındırma bir şirket içi Linux uygulama için Azure sanal makineleri geçirerek öğrenin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 8f039884ca0ea46c128078984d6cab6fd29ac9af
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36220559"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms"></a>Contoso geçiş: şirket içi Linux uygulama Azure vm'lerine yeniden barındırma

Bu makalede Contoso şirket içi Linux tabanlı hizmet Masası uygulama nasıl yeniden barındırılmasını gösterir (**osTicket**), Azure Iaas vm'lerine.

Bu belge nasıl Contoso adlı kurgusal şirket için Microsoft Azure bulut şirket kaynaklarını geçirir belge makaleleri bir dizi yedinci değil. Seri arka plan bilgileri ve bir geçiş altyapısını ayarlamak ve farklı türde geçişler çalıştırmak verilmektedir senaryoları kümesini içerir. Karmaşık senaryolar büyümesine ve biz diğer makaleler zamanla eklenmesi.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale serisi ve kullanırız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[Makale 2: bir Azure altyapısı dağıtın](contoso-migration-infrastructure.md) | Nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar açıklar. Aynı alt tüm Contoso geçiş senaryoları için kullanılır. | Kullanılabilir
[Makale 3: şirket içi kaynakları değerlendirin](contoso-migration-assessment.md)  | Contoso VMware üzerinden çalışan kendi şirket içi iki katmanlı SmartHotel uygulamasının bir değerlendirme nasıl çalıştığı gösterilmektedir. Uygulama VM'ler ile değerlendirmek [Azure geçirmek](migrate-overview.md) hizmet ve uygulama SQL Server veritabanı ile [Azure veritabanı geçiş Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[Makale 4: Rehost Azure VM'ler ve SQL yönetilen örneği](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso SmartHotel uygulamayı Azure'a nasıl geçirir gösterir. VM ön uç uygulamasını kullanarak geçirmek [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), uygulamayı ve veritabanı kullanma [Azure veritabanı geçiş](https://docs.microsoft.com/azure/dms/dms-overview) yönetilen bir SQL örneğine geçirmek için hizmet. | Kullanılabilir
[Makale 5: Azure VM'ler için yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso SmartHotel uygulama yalnızca Site Recovery kullanarak sanal makineleri geçirmek nasıl gösterir.
[Makale 6: Azure VM'ler ve SQL Server kullanılabilirlik gruplarını yeniden barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulama nasıl geçirir gösterir. Sanal makineleri uygulama ve veritabanı geçiş hizmeti uygulama veritabanı için SQL Server kullanılabilirlik grubu geçirmek için geçirmek için Site Recovery kullanırlar. | Kullanılabilir
Makale 7: Azure vm'lerine (Bu makalede) Linux uygulama yeniden barındırma | Contoso Azure Site RECOVERY'yi kullanarak kendi osService Linux uygulama nasıl geçirir gösterir.
[Makale 8: Linux uygulama Azure VM'ler ve Azure MySQL sunucusu için yeniden barındırma](contoso-migration-rehost-linux-vm-mysql.md) | (Azure MySQL Server örneğine. geçirmek için Site Recovery VM geçiş için ve MySQL çalışma ekranı kullanarak Contoso osService Linux uygulama nasıl geçirir gösterir | Kullanılabilir

Bu makalede, iki katmanlı Contoso geçireceği **osTicket** için Azure Linux Apache MySQL PHP (AMPUL) üzerinde çalışan uygulama. Azure Site Recovery hizmetini kullanarak sanal makineleri uygulama geçirilecektir. Bu açık kaynaklı uygulamayı kullanmak istiyorsanız, buradan indirebilirsiniz [GitHub](https://github.com/osTicket/osTicket).

## <a name="business-drivers"></a>İş sürücüleri

BT liderlik ekibindeki yakından bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla çalışmıştır:

- **Adres iş büyümesi**: Contoso artmaktadır ve sonuç olarak şirket içi sistemlerini ve altyapı Basıncı yoktur.
- **Sınırlamak risk**: hizmet Masası uygulama Contoso iş için kritik öneme sahiptir. Azure ile sıfır risk geçmek istiyorsunuz.
- **Genişletme**: uygulama hemen şimdi değiştirmek istemiyorsanız. Yalnızca kararlı olduğundan emin olmak isterler.


## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım için en iyi geçiş yöntemini belirlemek için bu geçiş hedefleri aşağı sabitlenmiş:

- Geçişten sonra Azure uygulamasında, bugün şirket içi VMWare ortamlarında olduğu gibi aynı performans özellikleri olması gerekir.  Uygulama içi olarak bulutta kritik olarak kalır. 
- Bu uygulamada yatırım contoso istememektedir.  İş için önemlidir, ancak mevcut haliyle basitçe buluta güvenle taşımak istedikleri.
- Contoso ops modeli Bu uygulama için değiştirmek istediğiniz değil. Bulutta ile artık aynı şekilde etkileşim isterler.
- Contoso işlevini değiştirmek istediğiniz değil. Yalnızca uygulama konumunu değiştirir.
- Windows uygulama geçişler birkaç tamamlandığını Contoso Azure'da bir Linux tabanlı altyapı kullanmayı öğrenin istiyor.

## <a name="proposed-architecture"></a>Önerilen mimarisi

Bu senaryoda:

- Uygulama, iki VM arasında (OSTICKETWEB ve OSTICKETMYSQL) katmanlı.
- Sanal makineleri VMware ESXi ana bilgisayarda bulunan **contosohost1.contoso.com** (sürüm 6.5).
- VMware ortamı vCenter Server 6.5 tarafından yönetilen (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (**contoso datacenter**), bir şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).
- Uygulamayı bir üretim iş yükü olduğundan, azure'da Vm'leri üretim kaynak grubunda yer alacağı **ContosoRG**.
- Sanal makineleri birincil bölge (Doğu ABD 2) geçişi ve üretim ağı (VNET-üretim-EUS2) yerleştirilir:
    - VM web ön uç alt (üretim-FE-EUS2) bulunur.
    - VM veritabanı (üretim-DB-EUS2) veritabanı alt ağda yer alacaktır.
- Geçiş tamamlandıktan sonra şirket içi sanal makineleri Contoso veri merkezinde hizmetten.

![Senaryo mimarisi](./media/contoso-migration-rehost-linux-vm/architecture.png) 

## <a name="migration-process"></a>geçiş işlemi

Contoso gibi geçirir:

1. İlk adım olarak Contoso Azure ayarlar ve şirket içi Site Recovery dağıtmak için gerekli altyapıyı.
2. Azure ve şirket içi bileşenleri hazırlama sonra ayarlanır ve VM'ler için çoğaltma etkinleştirme.
3. Çoğaltma çalışmaya başladıktan sonra bunları Azure'a devretmek tarafından sanal makineleri geçirin.

![geçiş işlemi](./media/contoso-migration-rehost-linux-vm/migration-process.png)

### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Hizmet düzenler ve geçiş ve olağanüstü durum kurtarma Azure VM'ler için yönetir ve sanal makineleri ve fiziksel sunucuları şirket içi.  | Azure'a çoğaltma sırasında Azure depolama ücretleri ücrete.  Azure VM'ler oluşturulur ve yük devretme oluştuğunda ücretler, uygulanır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/) ücretleri ve fiyatlandırma hakkında.

 
## <a name="prerequisites"></a>Önkoşullar

Bu senaryo için (ve Contoso) gerekenler buradadır.

**Gereksinimleri** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Bir aboneliği erken makaleleri sırasında bu dizide oluşturmuş. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Var olan bir abonelik kullanın ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yönetici ile çalışmak için gerekir.<br/><br/> Daha ayrıntılı izinler gerekiyorsa, gözden [bu makalede](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure altyapı** | Contoso bölümünde açıklandığı gibi Azure kendi altyapısını ayarlamak [geçiş için Azure altyapı](contoso-migration-infrastructure.md).<br/><br/> Özel hakkında daha fazla bilgi [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) Site Recovery için gereksinimleri.
**Şirket içi sunucular** | Şirket içi vCenter server 5.5, 6.0 veya 6.5 sürümü çalıştırması gerekir<br/><br/> 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir ESXi ana bilgisayar<br/><br/> Bir veya daha fazla VMware ESXi ana bilgisayarında çalışan sanal makineleri.
**Şirket içi sanal makineleri** | [Linux makineler gözden](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) Site Recovery ile geçiş için desteklenir.<br/><br/> Doğrulama desteklenen [Linux dosya ve depolama sistemlerini](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> Sanal makineleri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Senaryo adımları

Azure geçiş nasıl tamamlayacak aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure Site Recovery için hazırlama**: Çoğaltılan verileri tutmak için bir Azure depolama hesabı oluşturmak ve bir kurtarma Hizmetleri kasası oluşturun.
> * **2. adım: şirket içi VMware Site Recovery için hazırlama**: VM Keşif ve aracı yükleme için kullanılacak hesabı hazırlayın ve yük devretme sonrasında Azure Vm'lerine bağlanmak hazırlayın.
> * **3. adım: Çoğaltma sanal makineleri**: Bunlar kaynak ve hedef geçiş ortamını ayarlarken, bir çoğaltma ilkesi oluşturun ve Azure depolama alanına sanal makineleri çoğaltma işlemi başlatma.
> * **4. adım: Site Recovery ile sanal makineleri geçirmek**: her şeyi çalıştığından emin olmak için yük devretme testi çalıştırın ve ardından sanal makineleri Azure'a geçirmek için tam bir yük devretme çalıştırın.


## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>1. adım: Azure Site Recovery hizmeti için hazırlama

Contoso Azure bileşenleri birkaç Site kurtarması gerekir:

- Bir VNet devredilir kaynakları konumlandırıldığını (VNet zaten dağıtılmış üretim Contoso kullanır)
- Çoğaltılan verileri tutmak için yeni bir Azure depolama hesabı. 
- Azure kurtarma Hizmetleri kasasına.

Zaten VNet sırasında oluşturulan Contoso [Azure altyapı dağıtımı](contoso-migration-infrastructure.md), bunlar yalnızca bir depolama hesabı ve kasası oluşturmanız gerekir.

1. Contoso Doğu ABD 2 bölgede bir Azure depolama hesabı (contosovmsacc20180528) oluşturur.

    - Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
    - Standart depolama ve LRS çoğaltma ile bunların bir genel amaçlı hesabı kullanıyorsanız.

    ![Site Kurtarma depolama alanı](./media/contoso-migration-rehost-linux-vm/asr-storage.png)

2. Ağ ve depolama hesabı yerinde Contoso kasası (ContosoMigrationVault) oluşturun ve yerleştirileceği **ContosoFailoverRG** kaynak grubunda birincil Doğu ABD 2 bölge.

    ![Kurtarma Hizmetleri kasası](./media/contoso-migration-rehost-linux-vm/asr-vault.png)


**Daha fazla yardım gerekiyor mu?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Azure Site Recovery için ayarlama.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>2. adım: şirket içi VMware Site Recovery için hazırlama

Contoso şirket içi VMware altyapı şu şekilde hazırlar:

- VM bulma otomatikleştirmek için vCenter sunucusu veya vSphere ESXi ana bilgisayarda, bir hesap oluşturun.
- Mobility hizmetinin otomatik olarak yüklenmesini çoğaltmak istediğiniz VMware Vm'lerinde sağlayan bir hesap oluşturun.
- Geçiş sonrasında oluşturulduğunda Azure Vm'lerinin bağlanabilmesi için şirket içi VM'ler hazırlayın.


### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- Sanal makineleri otomatik olarak bulur. En az bir salt okunur hesap gereklidir.
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme. Oluşturma ve diskleri kaldırma ve sanal makine kapatma gibi işlemleri çalıştırılan bir hesap gerekir.

Contoso hesabı gibi ayarlar:

1. Contoso vCenter düzeyinde bir rol oluşturur.
2. Contoso, daha sonra bu rol gerekli izinleri atar.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Mobility hizmetinin Contoso geçirme Linux VM'ler üzerinde yüklü olmalıdır:

- VM'ler için çoğaltma işlemini etkinleştirdiğinizde, site kurtarma Bu bileşen otomatik itme yüklemesi yapabilirsiniz.
- Otomatik gönderme yüklemesi için Site Recovery VM'ler erişmek için kullanacağı bir hesap hazırlamanız gerekir.
- Hesapları ayrıntıları çoğaltma Kurulum sırasında giriş. 
- Hesap etki alanı veya yerel hesap olarak Vm'lerinde yüklemek için gerekli izinlere sahip olabilir.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Azure yük devretme işleminden sonra Contoso çoğaltılan Azure vm'lerinin bağlanabilmesi istiyor. Bunu yapmak için şunları yapmak için gereksinim duydukları vardır:

- Internet üzerinden erişmek için geçiş işleminden önce şirket içi Linux VM üzerinde SSH bunlar etkinleştirin.  Ubuntu için bu aşağıdaki komutu kullanarak tamamlanabilir: **apt Sudo get ssh yükle -y**.
- (Yük devretme) geçiş çalıştırdıktan sonra denetlemesi gerektiğini **önyükleme tanılama** ekran VM görüntüsünü görüntülemek için.
- Bu işe yaramazsa, bunlar VM çalıştıran ve bunlar gözden olduğunu denetlemelisiniz [sorun giderme ipuçları](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Daha fazla yardım gerekiyor mu?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) oluşturma ve Otomatik bulma için bir rol atama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) mobilite hizmetinin göndermeli yüklemesi için bir hesap oluşturma.


## <a name="step-3-replicate-the-on-premises-vms"></a>3. adım: şirket içi Vm'lerini çoğaltma

Azure için web VM geçirebilmeniz için önce Contoso kurar ve çoğaltmayı etkinleştirir.

### <a name="set-a-protection-goal"></a>Koruma hedefi ayarlama

1. Kasasına kasa adı (ContosoVMVault) altında çoğaltma hedefi ayarlamak (**Başlarken** > **Site Recovery** > **altyapıyıhazırlama**.
2. Bunlar kendi makinelerine şirket içinde olduğundan, VMware Vm'lerini ve azure'a istediği olduğunuzu belirtin.
    ![Çoğaltma hedefi](./media/contoso-migration-rehost-linux-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Dağıtım Planlama onaylayın

Devam etmek için bunların bunlar seçerek dağıtım planlama tamamladınız onaylaması **Evet, yaptım**. Contoso yalnızca tek bir VM'ye Bu senaryoda geçiş olan ve dağıtım planlama olması gerekmez.

### <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Contoso kaynak ortamlarına yapılandırmak gerekir. Bunu yapmak için bunlar bir OVF şablonunu indirebilir ve Site Recovery yapılandırma sunucusu yüksek oranda kullanılabilir olarak dağıtmak için kullanın, şirket içi VMware VM. Yapılandırma sunucunun da çalışır durumda sonra bunlar bu kasaya kaydedin.

Yapılandırma sunucusu bir dizi bileşen çalıştırır:

- Şirket içi ve Azure arasındaki iletişimi düzenler ve veri çoğaltma yöneten yapılandırma sunucusu bileşeni.
- Bir çoğaltma ağ geçidi olarak davranır işlem sunucusu. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir.
- İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.

Contoso adımları aşağıdaki gibi gerçekleştirin:

1. OVF şablonu indirme **altyapıyı hazırlama** > **kaynak** > **yapılandırma sunucusu**.
    
    ![OVF indirin](./media/contoso-migration-rehost-linux-vm/add-cs.png)

2. Bunlar VMware VM oluşturmak için şablonu içeri aktarın ve VM dağıtabilirsiniz.

    ![OVF şablonu](./media/contoso-migration-rehost-linux-vm/vcenter-wizard.png)

3. VM üzerinde ilk kez açtığınızda, Windows Server 2016 yükleme deneyimi ön. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
4. Yükleme tamamlandıktan sonra yönetici olarak VM oturum. İlk oturum açma sırasında varsayılan olarak Azure Site kurtarma Yapılandırması aracını çalıştırır.
5. Aracı, bunlar yapılandırma sunucusunu kasaya kaydetmek için kullanılacak bir ad belirtin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra kullanıcılar Azure aboneliği ile oturum açın. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.

    ![Yapılandırma sunucusu kaydetme](./media/contoso-migration-rehost-linux-vm/config-server-register2.png)

7. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır.
8. Kullanıcılar için makine yeniden oturum açın ve yapılandırma sunucusu yönetim Sihirbazı'nı otomatik olarak başlatır.
9. Sihirbazda, çoğaltma trafiği almaya NIC seçerler. Bunu yapılandırıldıktan sonra bu ayar değiştirilemez.
10. Bunlar, abonelik, kaynak grubu ve yapılandırma sunucunun kaydedileceği kasayı seçin.

    ![Kasa](./media/contoso-migration-rehost-linux-vm/cswiz1.png) 

11. Bunlar daha sonra indirin ve MySQL Server ve VMWare Powerclı yükleyin. 
12. Doğrulama sonrasında, bunlar vCenter sunucusu veya vSphere ana bilgisayar FQDN veya IP adresini belirtin. Bunlar varsayılan bağlantı noktası bırakın ve vCenter sunucusu için bir kolay ad belirtin.
13. Bunlar, otomatik bulma için oluşturduğunuz hesabı ve Mobility hizmetinin otomatik olarak yüklemek için kullanılması gereken kimlik bilgilerini belirtin.

    ![vCenter](./media/contoso-migration-rehost-linux-vm/cswiz2.png)

14. Kayıt tamamlandığında Azure portalında Contoso VMware sunucusu ve yapılandırma sunucusu listelendiğini denetler **kaynak** kasası sayfasında. Bulma, 15 dakika veya daha fazla sürebilir. 
15. Site Recovery VMware sunucularına bağlanır ve sanal makineleri bulur.

### <a name="set-up-the-target"></a>Hedefi ayarlama

Artık Contoso hedef çoğaltma ayarları yapılandırır.

1. İçinde **altyapıyı hazırlama** > **hedef**, hedef ayarlarını seçin.
2. Site Recovery, bir Azure depolama hesabı ve belirtilen hedef ağında olup olmadığını denetler.

### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Kaynak ve hedef ayarlandıktan sonra Contoso bir çoğaltma ilkesi oluşturmak hazırdır.

1. İçinde **altyapıyı hazırlama** > **çoğaltma ayarları** > **Çoğaltma İlkesi** >  **oluşturma ve İlişkilendirme**, bir ilke oluşturmak **ContosoMigrationPolicy**.
2. Bunlar varsayılan ayarları kullanın:
    - **RPO eşiği**: varsayılan 60 dakika. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
    - **Kurtarma noktası bekletme**. Varsayılan 24 saatlik. Bu değer ne kadar bekletme penceresi için her kurtarma noktası belirtir. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**. Varsayılan bir saat. Bu değer oluşturulmasında ve uygulamayla tutarlı anlık görüntü sıklığı belirtir.
 
        ![Çoğaltma ilkesi oluşturma](./media/contoso-migration-rehost-linux-vm/replication-policy.png)

5. İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. 

    ![Çoğaltma İlkesi ilişkilendirme](./media/contoso-migration-rehost-linux-vm/replication-policy2.png)

**Daha fazla yardım gerekiyor mu?**

- Bu adımları tam bir kılavuz okuyabilirsiniz [şirket içi VMware Vm'leri için olağanüstü durum kurtarma kümesi](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Ayrıntılı yönergeler size yardımcı olmak için kullanılabilir [kaynak ortamını ayarlama](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [yapılandırma sunucusu dağıtmak](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), ve [çoğaltma ayarlarını yapılandırın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) Linux Azure Konuk Aracısı hakkında.

**Daha fazla yardım gerekiyor mu?**

- Bu adımları tam bir kılavuz okuyabilirsiniz [şirket içi VMware Vm'leri için olağanüstü durum kurtarma kümesi](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Ayrıntılı yönergeler size yardımcı olmak için kullanılabilir [kaynak ortamını ayarlama](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [yapılandırma sunucusu dağıtmak](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), ve [çoğaltma ayarlarını yapılandırın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) Linux Azure Konuk Aracısı hakkında.



### <a name="enable-replication-for-osticketweb"></a>OSTICKETWEB için çoğaltmayı etkinleştirme

Contoso çoğaltma başlatabilirsiniz artık **OSTICKETWEB** VM.

1. İçinde **uygulama çoğaltma** > **kaynak** > **+ Çoğalt** kaynak ayarlarını seçin.
2. Bunlar, sanal makineleri etkinleştirmek istiyorsanız, vCenter sunucusu ve yapılandırma sunucusu da dahil olmak üzere kaynak ayarları seçin seçerler.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm/enable-replication-source.png)

3. Bunlar hedef ayarları, kaynak grubu ve hangi Azure VM yük devretme sonrasında yer alacağı VNet ve çoğaltılan veriler depolanacağı depolama hesabı belirtin.

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm/enable-replication2.png)

3. Contoso seçer **OSTICKETWEB** çoğaltma için. 

    - Bu aşamada yalnızca Contoso seçer **OSTICKETWEB** çünkü VNet ve alt ağ seçilmelidir ve sanal makineleri aynı alt ağda değil.
    - Çoğaltma VM için etkinleştirildiğinde, site Recovery Mobility hizmeti otomatik olarak yükler.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm/enable-replication3.png)

4. VM Özellikleri'nde Contoso işlem sunucusu tarafından otomatik olarak makinede mobilite hizmetinin yüklenmesi için kullanılan hesabı seçer.

     ![Mobility hizmeti](./media/contoso-migration-rehost-linux-vm/linux-mobility.png)

5. içinde **çoğaltma ayarları** > **çoğaltma ayarlarını yapılandırın**, doğru çoğaltma ilkesi uygulanan ve seçim olup olmadığını denetleyin **çoğaltmayı etkinleştirme**.
6.  Çoğaltma sürüyor izlemek **işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.



### <a name="enable-replication-for-osticketmysql"></a>OSTICKETMYSQL için çoğaltmayı etkinleştirme

Contoso çoğaltma başlatabilirsiniz artık **OSTICKETMYSQL**.

1. İçinde **uygulama çoğaltma** > **kaynak** > **+ Çoğalt** kaynak ve hedef ayarları seçin.
2. Contoso seçer **OSTICKETMYSQL** çoğaltma için Mobility hizmeti yüklemesi için kullanılacak hesabı seçer.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm/mysql-enable.png)

3. Contoso OSTICKETWEB için kullanılan ve çoğaltma sağlayan aynı çoğaltma ilkesi uygulanır.  

**Daha fazla yardım gerekiyor mu?**

Bu adımları tam bir kılavuz okuyabilirsiniz [çoğaltmasını etkinleştir](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>4. adım: sanal makineleri geçirme 

Bir hızlı çalıştırmak Contoso yük devretme sınamasını ve ardından sanal makineleri geçirin.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Yardımcı olur, yük devretme testi çalıştıran her şeyi geçiş işleminden önce beklendiği gibi çalıştığından emin olun. 

1. Contoso çalıştıran yük devretme sınaması için son noktası zamanında (**son işlenen**).
2. Seçtikleri **yük devretme işlemine başlamadan önce makineyi kapatın**, böylece yük devretme tetiklemeden önce kaynak VM kapatmak Site Recovery çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. 
3. Sınama yük devretme çalıştırır: 
    - Tüm geçiş için gereken koşulların karşılandığından emin olmak için çalışır bir ön koşullarını denetleyin.
    - Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktası seçtiyseniz, kurtarma noktası verilerden oluşturulur.
    - Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.
3. Yük devretme işlemi tamamlandıktan sonra çoğaltma Azure portalında Azure VM görüntülenir. Bunlar, VM sağ ağa bağlı uygun boyutta olduğundan ve çalıştığından emin denetleyin. 
4. Doğruladıktan sonra bunlar yük devretmeyi, temizleme ve kayıt ve gözlemlerinizi kaydetmek.

### <a name="create-and-customize-a-recovery-plan"></a>Oluşturma ve bir kurtarma planı özelleştirme

 Yük devretme sınaması beklenen şekilde çalıştığını doğruladıktan sonra Contoso geçiş için bir kurtarma planı oluşturun. 

- Bir kurtarma planı sırayı belirtir hangi yük devretme, Azure sanal makinelerini Azure'da nasıl güncelleştirilecektir içinde.
- İki katmanlı uygulama geçirmek istedikleri olduğundan, böylece veri VM (SQLVM) (WEBVM) ön uç önce başlar, kurtarma planını özelleştirin.


1. İçinde **kurtarma planları (Site Recovery)** > **+ kurtarma planı**, bir plan oluşturur ve sanal makineleri ekleyin.

    ![Kurtarma planı](./media/contoso-migration-rehost-linux-vm/recovery-plan.png)

2. Planı oluşturduktan sonra bunlar için özelleştirme seçin (**kurtarma planları** > **OsTicketMigrationPlan** > **Özelleştir**.
3.  Bunlar kaldırmak **OSTICKETWEB** gelen **Grup 1: Başlangıç**.  Bu ilk başlatma eylemini etkilediği sağlar **OSTICKETMYSQL** yalnızca.

    ![Kurtarma grubu](./media/contoso-migration-rehost-linux-vm/recovery-group1.png)

4.  İçinde **+ grup** > **Ekle korunan öğeler**, ekledikleri **OSTICKETWEB** için **Grup 2: Başlangıç**.  Contoso bu iki farklı gruplardaki gerekir.

    ![Kurtarma grubu](./media/contoso-migration-rehost-linux-vm/recovery-group2.png)


### <a name="migrate-the-vms"></a>Vm'leri geçirme


Contoso sanal makineleri geçirmek için, kurtarma planı üzerinde bir yük devretme çalıştırılmaya hazır.

1. Planı seçin > **yük devretme**.
2.  En son kurtarma noktası yük devri ve Site Recovery yük devretme tetiklemeden önce şirket içi VM Kapatma denemesi gerektiğini belirtmek için seçin. Bunlar yük devretme işleminin ilerleyişini takip edebilirsiniz **işleri** sayfası.

    ![Yük devretme](./media/contoso-migration-rehost-vm/failover1.png)

3. Yük devretme sırasında vCenter Server ESXi ana bilgisayarında çalışan iki VM durdurmak için komutlar verir.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm/vcenter-failover.png)

3. Yük devretme sonrasında Contoso doğrulayın Azure VM Azure portalında beklendiği gibi görünür.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm/failover2.png)  

3. Azure VM'yi doğruladıktan sonra bunların her VM için geçiş işlemini tamamlamak için geçişi tamamlayın. Bu sanal makine için çoğaltmayı durdurur ve VM için Site Recovery Faturalaması durdurulur.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm/failover3.png)


### <a name="connect-the-vm-to-the-database"></a>VM veritabanına bağlan

Geçiş işleminin son adımı olarak Contoso güncelleştirme üzerinde çalışan uygulama veritabanına işaret edecek şekilde uygulama bağlantı dizesi **OSTICKETMYSQL** VM. 

1. Bunlar bir SSH bağlantısı **OSTICKETWEB** Putty kullanarak VM veya başka bir SSH istemcisi. Özel IP adresini kullanarak bağlanacak şekilde VM özeldir.

    ![veritabanına bağlan](./media/contoso-migration-rehost-linux-vm/db-connect.png)  

    ![veritabanına bağlan](./media/contoso-migration-rehost-linux-vm/db-connect2.png)  

2. Emin olmak gereksinim duydukları **OSTICKETWEB** VM ile iletişim kurabilir **OSTICKETMYSQL** VM. Şu anda şirket içi IP adresiyle 172.16.0.43 sabit kodlanmış bir yapılandırmadır.

    **Güncelleştirmeden önce**
    
    ![IP güncelleştir](./media/contoso-migration-rehost-linux-vm/update-ip1.png)  

    **Güncelleştirme sonrası**
    
    ![IP güncelleştir](./media/contoso-migration-rehost-linux-vm/update-ip2.png) 
    
3. Hizmeti ile yeniden **systemctl yeniden apache2**.

    ![Yeniden Başlatma](./media/contoso-migration-rehost-linux-vm/restart.png) 

4. Son olarak, bunlar için DNS kayıtlarını güncelleştirmek **OSTICKETWEB** ve **OSTICKETMYSQL**, Contoso etki alanı denetleyicilerinden biri üzerinde.

    ![DNS'yi güncelleştir](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

    ![DNS'yi güncelleştir](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

**Daha fazla yardım gerekiyor mu?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) yük devretme testi çalıştırma. 
- [Bilgi](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) bir kurtarma planı oluşturma.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Azure'a yük devrediliyor.

## <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Tam geçiş ile osTicket uygulama katmanları artık Azure Vm'lerinde çalışıyor.

Şimdi, Contoso bazı temizleme yapması gerekir:  

- Bunlar, vCenter stoktan şirket içi sanal makineleri kaldırın.
- Bunlar, yerel yedekleme işlerini şirket içi sanal makineleri kaldırın.
- Yeni konumu göstermek için kendi iç belgelerine güncelleştirin ve OSTICKETWEB ve OSTICKETMYSQL için IP adresleri.
- Bunlar, sanal makineleri ile etkileşim herhangi bir kaynağa gözden geçirin ve herhangi bir ilgili ayarları veya belge yeni yapılandırmayı yansıtacak şekilde güncelleştirin.
- Contoso Azure geçirmek bağımlılık eşlemesi ile sanal makineleri geçiş için değerlendirmek için kullanılan hizmet. Bunlar, Microsoft Monitoring Agent ve bu amaçla VM'den yükledikleri bağımlılık Aracısı kaldırmanız gerekir.

## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Uygulamayı şimdi çalıştıran Contoso tam olarak faaliyete ve yeni altyapılarını güvenli gerekiyor.

### <a name="security"></a>Güvenlik

Contoso güvenlik ekibine OSTICKETWEB ve OSTICKETMYSQLVMs güvenlik sorunları belirlemek için gözden geçirin.

- Bunlar, erişimi denetlemek VM'ler için ağ güvenlik grupları (Nsg'ler) gözden geçirin. Nsg'ler yalnızca uygulamaya izin verilen trafiğin geçebilmesi emin olmak için kullanılır.
- Bunlar ayrıca Disk şifrelemesi ve Azure KeyVault kullanarak VM disklerdeki verileri güvenli hale getirme değerlendiriyorsanız.

[Daha fazla bilgi](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) VM'ler için güvenlik uygulamaları hakkında.

### <a name="backups"></a>Yedeklemeler

Contoso Azure Yedekleme hizmetini kullanarak Vm'lerde verileri yedekleyin. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Lisans ve maliyet en iyi duruma getirme

- Kaynakları dağıttıktan sonra Contoso Azure etiketleri sırasında tanımlanan atar [Azure altyapı dağıtımı](contoso-migration-infrastructure.md#set-up-tagging).
- Contoso Ubuntu sunucularıyla lisans herhangi bir sorun var.
- Contoso Azure maliyeti Cloudyn, Microsoft ofisine lisanslı yönetim olanağı sağlar. Bunu kullanan ve Azure ve diğer bulut kaynakları yönetmenize yardımcı olacak bir çok bulut maliyeti yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyeti yönetimi hakkında. 


## <a name="next-steps"></a>Sonraki adımlar

Contoso nasıl geçirildiğine gösteriyordu bu makalede, Azure Site Recovery kullanarak Azure Iaas VM'ler için iki Linux sanal makineleri üzerinde bir şirket içi hizmet Masası uygulama katmanlı.

Serideki sonraki makalede nasıl Contoso geçirmek aynı hizmet Masası uygulamayı Azure'a göstereceğiz. Bu süre Contoso uygulaması için ön uç VM geçirmek için Site Recovery kullanan ve Yedekleme kullanarak uygulama veritabanını geçirme ve MySQL çalışma ekranı aracını kullanarak MySQL için Azure veritabanına geri yükleme. [Başlama](contoso-migration-rehost-linux-vm-mysql.md).
