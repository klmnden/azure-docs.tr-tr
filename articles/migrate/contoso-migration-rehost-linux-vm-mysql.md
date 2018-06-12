---
title: Azure ve Azure MySQL Contoso Linux hizmet Masası uygulamayı rehost | Microsoft Docs
description: Contoso geçirerek bir şirket içi Linux uygulama nasıl rehosts öğrenin Azure Vm'leri ve Azure MySQL için.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/10/2018
ms.author: raynew
ms.openlocfilehash: 4367bf7cb02bb6a1e343dc3fb171be731e15c32b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300720"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms-and-azure-mysql"></a>Contoso geçiş: Azure Vm'leri ve Azure MySQL için şirket içi Linux uygulama yeniden barındırma

Bu makalede nasıl Contoso yeniden barındırılmasını kendi şirket içi iki katmanlı Linux hizmet Masası uygulama (osTicket) geçirerek gösterilmektedir Azure ve Azure MySQL.

Bu elge nasıl Contoso adlı kurgusal şirket için Microsoft Azure bulut şirket kaynaklarını geçirir Göster makaleleri bir dizi sekizinci ' dir. Seri arka plan bilgileri ve bir geçiş altyapısını ayarlamak ve farklı türde geçişler çalıştırmak nasıl çalışılacağını senaryolar içerir. Karmaşık senaryolar büyümesine ve biz diğer makaleler zamanla eklenmesi.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale serisi ve kullanırız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[Makale 2: bir Azure altyapısı dağıtın](contoso-migration-infrastructure.md) | Nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar açıklar. Aynı alt tüm Contoso geçiş senaryoları için kullanılır. | Kullanılabilir
[Makale 3: şirket içi kaynakları değerlendirin](contoso-migration-assessment.md)  | Contoso VMware üzerinden çalışan kendi şirket içi iki katmanlı SmartHotel uygulamasının bir değerlendirme nasıl çalıştığı gösterilmektedir. Uygulama VM'ler ile değerlendirmek [Azure geçirmek](migrate-overview.md) hizmet ve uygulama SQL Server veritabanı ile [Azure veritabanı geçiş Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[Makale 4: Rehost Azure VM'ler ve SQL yönetilen örneği](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso SmartHotel uygulamayı Azure'a nasıl geçirir gösterir. VM web uygulamasını kullanarak geçirmek [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), uygulamayı ve veritabanı kullanma [Azure veritabanı geçiş](https://docs.microsoft.com/azure/dms/dms-overview) yönetilen bir SQL örneğine geçirmek için hizmet. | Kullanılabilir
[Makale 5: Azure VM'ler için yeniden barındırma](contoso-migration-rehost-vm.md) | Nasıl Contoso geçirmek kendi SmartHotel Site Recovery hizmetini kullanarak Azure Iaas VM'ler için gösterir.
[Makale 6: Azure VM'ler ve SQL Server kullanılabilirlik gruplarını yeniden barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulama nasıl geçirir gösterir. Sanal makineleri uygulama ve veritabanı geçiş hizmeti uygulama veritabanı için SQL Server kullanılabilirlik grubu geçirmek için geçirmek için Site Recovery kullanırlar. | Kullanılabilir
[Makale 7: Azure VM'ler için Linux uygulama yeniden barındırma](contoso-migration-rehost-linux-vm.md) | Contoso osTicket Linux uygulama Azure Site Kurtarma'yı kullanarak Azure Iaas Vm'leri için nasıl geçirir gösterir.
Makale 8: Azure Vm'leri ve Azure MySQL Server (Bu makalede) için bir Linux uygulama yeniden barındırma | Contoso osTicket Linux uygulama nasıl geçirir gösterir. Bunlar VM geçiş için Site Recovery ve MySQL çalışma ekranı Azure MySQL Server örneğine geçirmek için kullanın. | Kullanılabilir

Bu makalede, Contoso için Azure bir iki katmanlı Linux Apache MySQL PHP (AMPUL) servis Masası Uygulaması'nı (osTicket) geçirir. Bu açık kaynaklı uygulamayı kullanmak istiyorsanız, buradan indirebilirsiniz [GitHub](https://github.com/osTicket/osTicket).



## <a name="business-drivers"></a>İş sürücüleri

BT liderlik ekibindeki yakından elde etmek istedikleri anlamak için iş ortaklarıyla çalışmıştır:

- **Adres iş büyümesi**: Contoso artmaktadır ve sonuç olarak şirket içi sistemlerini ve altyapı Basıncı yoktur.
- **Sınırlamak risk**: hizmet Masası uygulama Contoso iş için kritik öneme sahiptir. Azure ile sıfır risk geçmek istiyorsunuz.
- **Genişletme**: Contoso uygulamayı şimdi değiştirmek istediğiniz değil. Yalnızca kararlı tutmak isterler.


## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut ekip hedeflerine bu geçiş için aşağı en iyi geçiş yöntemini belirlemek için sabitlenmiş:

- Geçişten sonra Azure uygulamasında, bugün şirket içi VMWare ortamlarında olduğu gibi aynı performans özellikleri olması gerekir.  Uygulama içi olarak bulutta kritik olarak kalır. 
- Bu uygulamada yatırım contoso istememektedir.  İş için önemlidir, ancak mevcut haliyle basitçe buluta güvenle taşımak istedikleri.
- Windows uygulama geçişler birkaç tamamlandığını Contoso Azure'da bir Linux tabanlı altyapı kullanmayı öğrenin istiyor.
- Uygulama buluta taşındıktan sonra veritabanı yönetici görevleri en aza indirmek contoso istemektedir.

## <a name="proposed-architecture"></a>Önerilen mimarisi

Bu senaryoda:

- Uygulama, iki VM arasında (OSTICKETWEB ve OSTICKETMYSQL) katmanlı.
- Sanal makineleri VMware ESXi ana bilgisayarda bulunan **contosohost1.contoso.com** (sürüm 6.5).
- VMware ortamı vCenter Server 6.5 tarafından yönetilen (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'ile bir şirket içi etki alanı denetleyicisi (**contosodc1 adlı**).
- Bir Azure Iaas sanal OSTICKETWEB web katmanı uygulamasında geçirilecektir.
- Uygulama veritabanı için MySQL PaaS hizmeti Azure veritabanına geçirilecektir.
- Bir üretim iş yükü geçişi bu yana kaynakları üretim kaynak grubunda yer alacağı **ContosoRG**.
- Kaynakları birincil bölge (Doğu ABD 2) kopyalandığı ve üretim ağı (VNET-üretim-EUS2) yerleştirilir:
    - VM web ön uç alt (üretim-FE-EUS2) bulunur.
    - Veritabanı örneği (üretim-DB-EUS2) veritabanı alt ağda yer alacaktır.
- Uygulama veritabanı için Azure MySQL araçlarını kullanarak MySQL geçirilecektir.
- Geçiş tamamlandıktan sonra şirket içi sanal makineleri Contoso veri merkezinde hizmetten.


![Senaryo mimarisi](./media/contoso-migration-rehost-linux-vm-mysql/architecture.png) 


## <a name="migration-process"></a>geçiş işlemi

Contoso geçiş işlemi aşağıdaki gibi tamamlayın:

VM web geçirmek için:

1. İlk adım olarak Contoso Azure ayarlar ve şirket içi Site Recovery dağıtmak için gerekli altyapıyı.
2. Azure ve şirket içi bileşenleri hazırlama sonra ayarlanır ve VM web çoğaltmayı etkinleştirin.
3. Yukarı ve çalışan çoğaltma olduktan sonra Azure'a devrederek tarafından VM geçirin.

Veritabanı geçirmek için:

1. Contoso Azure MySQL örneğinde sağlar.
2. Bunlar MySQL çalışma ekranı ayarlamak ve yerel olarak veritabanını yedekleyin.
3. Bunlar daha sonra veritabanını yerel yedekten Azure'a geri yükleyin.

![geçiş işlemi](./media/contoso-migration-rehost-linux-vm-mysql/migration-process.png) 


### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Hizmet düzenler ve geçiş ve olağanüstü durum kurtarma Azure VM'ler için yönetir ve sanal makineleri ve fiziksel sunucuları şirket içi.  | Azure'a çoğaltma sırasında Azure depolama ücretleri ücrete.  Azure VM'ler oluşturulur ve yük devretme oluştuğunda ücretler, uygulanır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/) ücretleri ve fiyatlandırma hakkında.
[MySQL için Azure Veritabanı](https://docs.microsoft.com/azure/mysql/) | Veritabanı açık kaynak MySQL Server altyapısını temel alır. Tam olarak yönetilen, Kurumsal kullanıma hazır bir topluluk, uygulama geliştirme ve dağıtım için bir hizmet olarak MySQL veritabanı sağlar. 

 
## <a name="prerequisites"></a>Önkoşullar

(Ve Contoso) Bu senaryo çalıştırmak istiyorsanız İşte olması gerekir.

**Gereksinimleri** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Bir aboneliği erken makaleleri sırasında bu dizide oluşturmuş. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Var olan bir abonelik kullanın ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yönetici ile çalışmak için gerekir.<br/><br/> Daha ayrıntılı izinler gerekiyorsa, gözden [bu makalede](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure altyapı** | Contoso bölümünde açıklandığı gibi Azure kendi altyapısını ayarlamak [geçiş için Azure altyapı](contoso-migration-infrastructure.md).<br/><br/> Özel hakkında daha fazla bilgi [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) Site Recovery için gereksinimleri.
**Şirket içi sunucular** | Şirket içi vCenter server 5.5, 6.0 veya 6.5 sürümü çalıştırması gerekir<br/><br/> 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir ESXi ana bilgisayar<br/><br/> Bir veya daha fazla VMware ESXi ana bilgisayarında çalışan sanal makineleri.
**Şirket içi sanal makineleri** | [Linux VM gereksinimlerini gözden geçirme](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) Site Recovery ile geçiş için desteklenir.<br/><br/> Doğrulama desteklenen [Linux dosya ve depolama sistemlerini](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> Sanal makineleri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Senaryo adımları

Azure geçiş nasıl tamamlayacak aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure Site Recovery için hazırlama**: Çoğaltılan verileri tutmak için bir Azure depolama hesabı oluşturun ve bir kurtarma Hizmetleri kasası oluşturun.
> * **2. adım: şirket içi VMware Site Recovery için hazırlama**: hesapları VM Keşif ve aracı yüklemesine hazırlamak ve yük devretme sonrasında Azure Vm'lerine bağlanmak hazırlayın.
 * **3. adım: veritabanını sağlamak]**: Azure MySQL veritabanı örneği ile Azure, sağlama.
> * **4. adım: Çoğaltma sanal makineleri**: Site Recovery kaynak ve hedef ortamını yapılandırmak, bir çoğaltma ilkesini ayarlayın ve Azure depolama alanına sanal makineleri çoğaltma işlemi başlatma.
> * **5. adım: veritabanını geçirme**: MySQL araçları ile geçiş ayarlayın.
> * **6. adım: Site Recovery ile sanal makineleri geçirmek**: her şeyi çalıştığından emin olmak için yük devretme testi çalıştırmak ve sanal makineleri Azure'a geçirmek için tam bir yük devretme çalıştırın.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>1. adım: Azure Site Recovery hizmeti için hazırlama

Contoso Azure bileşenleri birkaç Site kurtarması gerekir:

- Bir VNet devredilir kaynakları konumlandırıldığını (VNet zaten dağıtılmış üretim Contoso kullanır).
- Çoğaltılan verileri tutmak için yeni bir Azure depolama hesabı. 
- Azure kurtarma Hizmetleri kasasına.

Zaten VNet sırasında oluşturulan Contoso [Azure altyapı dağıtımı](contoso-migration-infrastructure.md), bunlar yalnızca bir depolama hesabı ve kasası oluşturmanız gerekir.


1. Contoso Azure storage hesabı oluşturur (**contosovmsacc20180528**) Doğu ABD 2 bölgesindeki.

    - Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
    - Contoso standart depolama ve LRS çoğaltma ile bir genel amaçlı hesabını kullanır.

    ![Site Kurtarma depolama alanı](./media/contoso-migration-rehost-linux-vm-mysql/asr-storage.png)

3. Ağ ve depolama hesabıyla yerinde Contoso kasası (ContosoMigrationVault) oluşturur ve yerleştirir **ContosoFailoverRG** kaynak grubunda birincil Doğu ABD 2 bölge.

    ![Kurtarma Hizmetleri kasası](./media/contoso-migration-rehost-linux-vm-mysql/asr-vault.png)

**Daha fazla yardım gerekiyor mu?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Azure Site Recovery için ayarlama.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>2. adım: şirket içi VMware Site Recovery için hazırlama

Contoso şirket içi VMware altyapı şu şekilde hazırlar:

- Bir hesap VM bulma otomatikleştirmek için vCenter sunucusunda oluşturur.
- Mobility hizmetinin otomatik olarak yüklenmesini çoğaltmak istediğiniz VMware Vm'lerinde sağlayan bir hesabı oluşturur.
- Geçiş sonrasında oluşturulduğunda Azure Vm'lerinin bağlanabilmesi için şirket içi VM'ler hazırlar.


### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- Sanal makineleri otomatik olarak bulur. En az bir salt okunur hesap gereklidir.
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme. Oluşturma ve diskleri kaldırma ve sanal makine kapatma gibi işlemleri çalıştırılan bir hesap gerekir.

Contoso hesabı gibi ayarlar:

1. Contoso vCenter düzeyinde bir rol oluşturur.
2. Contoso, daha sonra bu rol gerekli izinleri atar.


### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Mobility hizmetinin Contoso geçirmek istediği her bir VM üzerinde yüklü olmalıdır.

- VM'ler için çoğaltma işlemini etkinleştirdiğinizde, site kurtarma Bu bileşen otomatik itme yüklemesi yapabilirsiniz.
- Otomatik yükleme. Site kurtarma VM erişmek için gerekli izinlere sahip bir hesap gerekir. 
- Hesap ayrıntıları çoğaltma Kurulum sırasında giriş. 
- Yükleme izinleri olduğu sürece hesap etki alanı veya yerel hesap olabilir.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Azure'a yük devretme sonrasında Azure Vm'lerinin bağlanabilmesi Contoso istemektedir. Bunu yapmak için bunlar şunları yapmanız gerekir: 

- Internet üzerinden erişmek için geçiş işleminden önce şirket içi Linux VM üzerinde SSH bunlar etkinleştirin.  Ubuntu için bu aşağıdaki komutu kullanarak tamamlanabilir: **apt Sudo get ssh yükle -y**.
- Yük devretme sonrasında, bunlar denetlemelisiniz **önyükleme tanılama** ekran VM görüntüsünü görüntülemek için.
- Bu işe yaramazsa, VM çalıştıran ve bunlar gözden olduğunu doğrulamak ihtiyaç duydukları [sorun giderme ipuçları](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

**Daha fazla yardım gerekiyor mu?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) oluşturma ve Otomatik bulma için bir rol atama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) mobilite hizmetinin göndermeli yüklemesi için bir hesap oluşturma.


## <a name="step-3-provision-azure-database-for-mysql"></a>3. adım: Azure veritabanı için MySQL sağlama

Birincil Doğu ABD 2 bölge örneğinde contoso hükümleri bir MySQL veritabanı.

1. Azure portalında bunlar MySQL kaynak için bir Azure veritabanı oluşturun. 

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-1.png)

2. Ad ekleyin **contosoosticket** Azure veritabanı için. Bunlar üretim kaynak grubuna veritabanı Ekle **ContosoRG**ve kimlik bilgilerini belirtin.
3. Bu sürüm uyumluluğu için seçebilmek şirket içi MySQL veritabanına 5.7, sürümüdür. Kendi veritabanı gereksinimlerine göre varsayılan boyutları kullanırlar.

     ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-2.png)

4. İçin **yedekleme artıklık seçenekleri**, Contoso seçer kullanmak için **coğrafi olarak yedekli**. Bu seçenek bir kesinti oluşursa kendi ikincil Orta ABD bölgesinde veritabanını geri yüklemek sağlar. Veritabanı sağladığınızda yalnızca bu seçenek yapılandırabilirsiniz.

     ![Yedeklilik](./media/contoso-migration-rehost-linux-vm-mysql/db-redundancy.png)

4. İçinde **VNET üretim EUS2** Ağ > **hizmet uç noktaları**, SQL Service hizmet uç noktası (veritabanı alt ağ) ekleyin.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-3.png)

5. Alt ağ eklendikten sonra bunlar üretim ağında veritabanı alt ağdan erişimine izin veren bir sanal ağ kuralı oluşturur.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-4.png)


## <a name="step-4-replicate-the-on-premises-vms"></a>4. adım: şirket içi Vm'lerini çoğaltma

Azure için web VM geçirebilmeniz için önce Contoso kurar ve çoğaltmayı etkinleştirir.

### <a name="set-a-protection-goal"></a>Koruma hedefi ayarlama

1. Kasasına kasa adı (ContosoVMVault) altında çoğaltma hedefi ayarlamak (**Başlarken** > **Site Recovery** > **altyapıyıhazırlama**.
2. Bunlar kendi makinelerine şirket içinde olduğundan, VMware Vm'lerini ve azure'a istediği olduğunuzu belirtin.

    ![Çoğaltma hedefi](./media/contoso-migration-rehost-linux-vm-mysql/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Dağıtım Planlama onaylayın

Devam etmek için bunların bunlar seçerek dağıtım planlama tamamladınız onaylaması **Evet, yaptım**. Contoso yalnızca tek bir VM'ye Bu senaryoda geçiş olan ve dağıtım planlama olması gerekmez.

### <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Contoso kaynak ortamlarına yapılandırmak gerekir. Bunu yapmak için bir yüksek oranda kullanılabilir bir Site Recovery yapılandırma sunucusuyla dağıtmak bir OVF şablonu kullanarak şirket içi VMware VM. Yapılandırma sunucunun da çalışır durumda sonra bunlar bu kasaya kaydedin.

Yapılandırma sunucusu bir dizi bileşen çalıştırır:

- Şirket içi ve Azure arasındaki iletişimi düzenler ve veri çoğaltma yöneten yapılandırma sunucusu bileşeni.
- Bir çoğaltma ağ geçidi olarak davranır işlem sunucusu. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir.
- İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.

Contoso adımları aşağıdaki gibi gerçekleştirin:


1. OVF şablonu indirme **altyapıyı hazırlama** > **kaynak** > **yapılandırma sunucusu**.
    
    ![OVF indirin](./media/contoso-migration-rehost-linux-vm-mysql/add-cs.png)

2. Bunlar VMware VM oluşturmak için şablonu içeri aktarın ve VM dağıtabilirsiniz.

    ![OVF şablonu](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-wizard.png)

3. VM üzerinde ilk kez açtığınızda, Windows Server 2016 yükleme deneyimi ön. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
4. Yükleme tamamlandıktan sonra bunların VM'ye yönetici olarak oturum açın. İlk oturum açma sırasında varsayılan olarak Azure Site kurtarma Yapılandırması aracını çalıştırır.
5. Aracı, bunlar yapılandırma sunucusunu kasaya kaydetmek için kullanılacak bir ad belirtin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler.
7. Bağlantı kurulduktan sonra kullanıcılar Azure aboneliği ile oturum açın. Kimlik bilgilerini yapılandırma sunucusu kaydedeceksiniz kasa erişimi olmalıdır.

    ![Yapılandırma sunucusu kaydetme](./media/contoso-migration-rehost-linux-vm-mysql/config-server-register2.png)

8. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır.
9. Kullanıcılar için makine yeniden oturum açın ve yapılandırma sunucusu yönetim Sihirbazı'nı otomatik olarak başlatır.
10. Sihirbazda, çoğaltma trafiği almaya NIC seçerler. Bunu yapılandırıldıktan sonra bu ayar değiştirilemez.
11. Bunlar, abonelik, kaynak grubu ve yapılandırma sunucunun kaydedileceği kasayı seçin.

    ![Kasa](./media/contoso-migration-rehost-linux-vm-mysql/cswiz1.png) 

12. Şimdi, indirin ve MySQL Server ve VMWare Powerclı yükleyin. 
13. Doğrulama sonrasında, bunlar vCenter sunucusu veya vSphere ana bilgisayar FQDN veya IP adresini belirtin. Bunlar varsayılan bağlantı noktası bırakın ve vCenter sunucusu için bir kolay ad belirtin.
14. Bunlar, otomatik bulma için oluşturduğunuz hesabı ve Site Recovery Mobility hizmeti otomatik olarak yüklemek için kullanacağı kimlik bilgilerini girin. 

    ![vCenter](./media/contoso-migration-rehost-linux-vm-mysql/cswiz2.png)

14. Kayıt tamamlandığında Azure portalında Contoso VMware sunucusu ve yapılandırma sunucusu listelendiğini denetler **kaynak** kasası sayfasında. Bulma, 15 dakika veya daha fazla sürebilir. 
15. Her yerde Site Recovery VMware sunucularına bağlanır ve sanal makineleri bulur.

### <a name="set-up-the-target"></a>Hedefi ayarlama

Şimdi Contoso girişleri çoğaltma ayarları hedefleyin.

1. İçinde **altyapıyı hazırlama** > **hedef**, hedef ayarlarını seçin.
2. Site Recovery, bir Azure depolama hesabı ve belirtilen hedef ağında olup olmadığını denetler.


### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Kaynak ve hedef ayarlanan Contoso bir çoğaltma ilkesi oluşturmak hazırdır.

1. İçinde **altyapıyı hazırlama** > **çoğaltma ayarları** > **Çoğaltma İlkesi** >  **oluşturma ve İlişkilendirme**, bir ilke oluşturmak **ContosoMigrationPolicy**.
2. Bunlar varsayılan ayarları kullanın:
    - **RPO eşiği**: varsayılan 60 dakika. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
    - **Kurtarma noktası bekletme**. Varsayılan 24 saatlik. Bu değer ne kadar bekletme penceresi için her kurtarma noktası belirtir. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**. Varsayılan bir saat. Bu değer oluşturulmasında ve uygulamayla tutarlı anlık görüntü sıklığı belirtir.
 
        ![Çoğaltma ilkesi oluşturma](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy.png)

5. İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. 

    ![Çoğaltma İlkesi ilişkilendirme](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy2.png)


**Daha fazla yardım gerekiyor mu?**

- Bu adımları tam bir kılavuz okuyabilirsiniz [şirket içi VMware Vm'leri için olağanüstü durum kurtarma kümesi](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Ayrıntılı yönergeler size yardımcı olmak için kullanılabilir [kaynak ortamını ayarlama](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [yapılandırma sunucusu dağıtmak](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), ve [çoğaltma ayarlarını yapılandırın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) Linux Azure Konuk Aracısı hakkında.

### <a name="enable-replication-for-the-web-vm"></a>Web sanal makine için çoğaltmayı etkinleştirme

Contoso çoğaltma başlatabilirsiniz artık **OSTICKETWEB** VM.

1. İçinde **uygulama çoğaltma** > **kaynak** > **+ Çoğalt** kaynak ayarlarını seçin.
2. Bunlar, sanal makineleri etkinleştirmek ve vCenter sunucusu ve yapılandırma sunucusu da dahil olmak üzere kaynak ayarları seçmek istedikleri gösterir.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication-source.png)

3. Artık hedef ayarlarını belirtin. Bunlar, kaynak grubu ve hangi Azure VM yük devretme sonrasında yer alacağı ağ ve çoğaltılan veriler depolanacağı depolama hesabını içerir. 

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication2.png)

3. Contoso seçer **OSTICKETWEB** çoğaltma için. 

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication3.png)

4. VM Özellikleri'nde bunlar otomatik olarak VM mobilite hizmetini yüklemek için kullanılacak hesabı seçin.

     ![Mobility hizmeti](./media/contoso-migration-rehost-linux-vm-mysql/linux-mobility.png)

5. içinde **çoğaltma ayarları** > **çoğaltma ayarlarını yapılandırın**, doğru çoğaltma ilkesi uygulanan ve seçim olup olmadığını denetleyin **çoğaltmayı etkinleştirme**. Mobility hizmetinin otomatik olarak yüklenir.
6.  Çoğaltma sürüyor izlemek **işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.


**Daha fazla yardım gerekiyor mu?**

Bu adımları tam bir kılavuz okuyabilirsiniz [çoğaltmasını etkinleştir](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-migrate-the-database"></a>5. adım: veritabanını geçirme

Contoso veritabanını yedekleme ve geri yükleme, MySQL araçlarıyla kullanarak geçirirsiniz. Bunlar MySQL çalışma ekranı yüklemek, OSTICKETMYSQL veritabanını yedekleyin ve ardından MySQL sunucusu için Azure veritabanına geri.

### <a name="install-mysql-workbench"></a>MySQL Workbench’i yükleme

1. Contoso denetimleri [önkoşulları ve yüklemeler MySQL çalışma ekranı](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool).
2. Windows ile uyumlu olarak için MySQL çalışma ekranı yükledikleri [yükleme yönergeleri](https://dev.mysql.com/doc/workbench/en/wb-installing.html).
3. MySQL çalışma ekranı bunlar OSTICKETMYSQL MySQL bağlantı oluşturun. 

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench1.png)

4. Veritabanı olarak verme **osticket**, yerel bir müstakil dosya.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench2.png)

5. Veritabanı yerel olarak yedeklendiğinden sonra bunlar MySQL örneği için Azure veritabanına bağlantı oluşturun.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench3.png)

6. Şimdi, Contoso (geri yükleme) Azure MySQL örneğinden, kendi içinde bulunan veritabanında dosyasını içeri aktarabilirsiniz. Yeni bir şema (osticket) için örneği oluşturulur.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench4.png)

## <a name="step-6-migrate-the-vms-with-site-recovery"></a>6. adım: Site Recovery ile Vm'leri geçirme

Bir hızlı çalıştırmak Contoso yük devretme sınamasını ve ardından VM geçirin.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Yük devretme testi çalıştırma yardımcı her şeyin, geçişten önce beklendiği gibi çalıştığını doğrulayın. 

1. Contoso çalıştıran yük devretme sınaması için son noktası zamanında (**son işlenen**).
2. Seçtikleri **yük devretme işlemine başlamadan önce makineyi kapatın**, böylece yük devretme tetiklemeden önce kaynak VM kapatmak Site Recovery çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. 
3. Sınama yük devretme çalıştırır: 

    - Tüm geçiş için gereken koşulların karşılandığından emin olmak için çalışır bir ön koşullarını denetleyin.
    - Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
    - Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.

3. Yük devretme işlemi tamamlandıktan sonra çoğaltma Azure portalında Azure VM görüntülenir. Contoso VM sağ ağa bağlı uygun boyutta olduğundan ve çalıştığından emin denetler. 
4. Doğruladıktan sonra bunlar yük devretmeyi, temizleme ve kayıt ve gözlemlerinizi kaydetmek.

### <a name="migrate-the-vm"></a>VM geçirme

VM geçirmek için Contoso VM içeren bir kurtarma planı oluşturur ve Azure üzerinden plana başarısız.

1. Contoso bir plan oluşturur ve ekler **OSTICKETWEB** ona.

    ![Kurtarma planı](./media/contoso-migration-rehost-linux-vm-mysql/recovery-plan.png)

2. Bunlar plan üzerinde bir yük devretme çalıştırın. Bunların en son kurtarma noktası seçin ve Site Recovery yük devretme tetiklemeden önce şirket içi VM Kapatma denemesi gerektiğini belirtin. Bunlar yük devretme işleminin ilerleyişini takip edebilirsiniz **işleri** sayfası.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm-mysql/failover1.png)

3. Yük devretme sırasında vCenter Server ESXi ana bilgisayarında çalışan iki VM durdurmak için komutlar verir.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-failover.png)

4. Yük devretme sonrasında Azure VM'de Azure portalında beklendiği gibi göründüğünü Contoso doğrular.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm-mysql/failover2.png)  

5. VM denetledikten sonra bunlar geçişi tamamlayın. Bu sanal makine için çoğaltmayı durdurur ve VM için Site Recovery Faturalaması durdurulur.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm-mysql/failover3.png)

**Daha fazla yardım gerekiyor mu?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) yük devretme testi çalıştırma. 
- [Bilgi](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) bir kurtarma planı oluşturma.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Azure'a yük devrediliyor.


### <a name="connect-the-vm-to-the-database"></a>VM veritabanına bağlan

Geçiş işleminin son adımı olarak Contoso MySQL için Azure veritabanı'nın üzerine yazmasına bağlantı dizesi güncelleştirin. 

1. Putty kullanarak OSTICKETWEB VM'ye yaptıkları bir SSH bağlantısı veya başka bir SSH istemcisi. Özel IP adresini kullanarak bağlanacak şekilde VM özeldir.

    ![veritabanına bağlan](./media/contoso-migration-rehost-linux-vm-mysql/db-connect.png)  

    ![veritabanına bağlan](./media/contoso-migration-rehost-linux-vm-mysql/db-connect2.png)  

2. Ayarlarını güncelleştirmek için **OSTICKETWEB** VM ile iletişim kurabilir **OSTICKETMYSQL** veritabanı. Şu anda şirket içi IP adresiyle 172.16.0.43 sabit kodlanmış bir yapılandırmadır.

    **Güncelleştirmeden önce**
    
    ![IP güncelleştir](./media/contoso-migration-rehost-linux-vm-mysql/update-ip1.png)  

    **Güncelleştirme sonrası**
    
    ![IP güncelleştir](./media/contoso-migration-rehost-linux-vm-mysql/update-ip2.png) 
    
    ![IP güncelleştir](./media/contoso-migration-rehost-linux-vm-mysql/update-ip3.png) 

3. Hizmeti ile yeniden **systemctl yeniden apache2**.

    ![Yeniden Başlatma](./media/contoso-migration-rehost-linux-vm-mysql/restart.png) 

4. Son olarak, bunlar için DNS kayıtlarını güncelleştirmek **OSTICKETWEB**, Contoso etki alanı denetleyicilerinden biri üzerinde.

    ![DNS'yi güncelleştir](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 


##  <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Tam geçiş ile osTicket uygulama katmanları Azure Vm'lerinde çalışıyor.

Şimdi, Contoso aşağıdakileri yapmalıdır: 
- VCenter stoktan VMware sanal makinelerini Kaldır
- Şirket içi sanal makineleri yerel yedekleme işlerden kaldırın.
- Güncelleştirme iç belgelerine yeni konumlar ve IP adreslerini gösterir. 
- Şirket içi sanal makineleri ile etkileşim herhangi bir kaynağa gözden geçirin ve herhangi bir ilgili ayarları veya belge yeni yapılandırmayı yansıtacak şekilde güncelleştirin.
- Contoso değerlendirmek için bağımlılık eşlemesi ile Azure geçirmek hizmet kullanılan **OSTICKETWEB** VM geçişi için. Şimdi bu amaçla VM'den yüklü aracıları (Microsoft İzleme Aracısı/bağımlılık Aracısı) kaldırmanız gerekir.

## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Uygulamayı şimdi çalıştıran Contoso tam olarak faaliyete ve yeni altyapılarını güvenli gerekir.

### <a name="security"></a>Güvenlik

Contoso güvenlik ekibi, VM ve veritabanı güvenlik sorunları belirlemek için gözden geçirin.

- Bunlar, erişimi denetlemek üzere bağlı olarak, VM için ağ güvenlik grupları (Nsg'ler) gözden geçirin. Nsg'ler yalnızca uygulamaya izin verilen trafiğin geçebilmesi emin olmak için kullanılır.
- Bunlar, Disk şifrelemesi ve Azure KeyVault kullanarak VM disklerde veri güvenliğini sağlamak için göz önünde bulundurun.
- VM ve veritabanı örneği arasındaki iletişim için SSL yapılandırılmamış. Veritabanı trafiğinin ele sağlamak için bunu yapmanız gerekecektir.

[Daha fazla bilgi](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) VM'ler için güvenlik uygulamaları hakkında.

### <a name="backups"></a>Yedeklemeler

- Contoso Azure Yedekleme hizmetini kullanarak VM üzerinde verileri yedekler. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- Veritabanı için yedekleme yapılandırması gerekmez. Azure veritabanı için MySQL server yedekleri otomatik olarak oluşturur ve depolar. Bunlar, esnek ve üretime hazır olması için veritabanı için coğrafi yedeklilik kullanmayı seçtiniz.

### <a name="licensing-and-cost-optimization"></a>Lisans ve maliyet en iyi duruma getirme

- Kaynakları dağıttıktan sonra Contoso verilen sırasında kararlarına göre Azure etiketleri atar [Azure altyapı](contoso-migration-infrastructure.md#set-up-tagging) dağıtım.
- Contoso Ubuntu sunucuları için hiçbir lisans sorunları vardır.
- Contoso Azure maliyeti Cloudyn, Microsoft ofisine lisanslı yönetim olanağı sağlar. Bunu kullanan ve Azure ve diğer bulut kaynakları yönetmenize yardımcı olacak bir çok bulut maliyeti yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyeti yönetimi hakkında.


## <a name="next-steps"></a>Sonraki adımlar

Bu senaryoda, Contoso çalıştığını biz son rehost senaryosu gösterilmiştir. Bunlar bir Azure VM için ön uç şirket içi Linux osTicket uygulamasının VM geçişi ve uygulama veritabanı Azure MySQL örneğine geçişi.

Basit yükseltme shift geçişler yerine uygulama, yeniden düzenleme içeren Contoso geçişler, daha karmaşık bir dizi nasıl gerçekleştirileceğini göstermek için öğreticileri geçiş serideki sonraki kümesinde yapacağız.
