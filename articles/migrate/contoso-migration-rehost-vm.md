---
title: Azure Site Recovery ile Azure vm'lerine geçiş ile bir Contoso uygulaması yeniden barındırma | Microsoft Docs
description: Şirket içi lift-and-shift ile taşıma geçişini ile şirket içi barındırma uygulaması Azure Site Recovery hizmetini kullanarak Azure'a nasıl makineleri öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: raynew
ms.openlocfilehash: 11859beb3d7bf0d0b0b801328c6570d274f1ea68
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "42061496"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms"></a>Contoso taşıma: Azure sanal makinelerini şirket içi uygulamaya yeniden barındırma


Bu makalede, Contoso uygulama sanal makinelerini Azure Vm'lerine geçiş yaparak, azure'da şirket içi SmartHotel uygulamayı nasıl rehosts gösterilmektedir.


Bu belge, Contoso adlı kurgusal şirketin n içi kaynaklar Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapı kurulumu, geçiş için şirket içi kaynaklarınızı değerlendirerek ve geçişleri farklı türlerde çalıştıran gösteren senaryolar içerir. Senaryoları, karmaşık hale gelmesi ve zaman içinde ek makaleleri ekleyeceğiz.


**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale dizisini ve kullandığımız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[2. makale: bir Azure altyapısını dağıtma](contoso-migration-infrastructure.md) | Açıklayan nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar. Altyapıyı, tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](contoso-migration-assessment.md)  | Contoso değerlendirme Wmware'de çalışan bir şirket içi iki katmanlı SmartHotel uygulamanın nasıl çalıştığını gösterir. Contoso uygulaması Vm'lerle değerlendirir [Azure geçişi](migrate-overview.md) hizmet ve uygulama SQL Server veritabanıyla [veritabanı geçiş Yardımcısı'nı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[4. makale: Azure sanal makineler ve yönetilen bir SQL örneği üzerinde bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso lift-and-shift ile taşıma geçiş için Azure SmartHotel şirket içi uygulama için nasıl çalıştığını gösterir. Contoso VM ön uç uygulamasını kullanarak geçirir [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)ve SQL yönetilen örneği, uygulama veritabanını kullanarak [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir
Makale 5: bir uygulamayı Azure vm'lerinde yeniden barındırma | Nasıl Contoso geçirme SmartHotel uygulama sanal makinelerini Site Recovery hizmetini kullanarak Azure Vm'leri için gösterir. | Bu makalede.
[Makale 6: Azure sanal makineleri ve Always On kullanılabilirlik grubu SQL Server üzerinde bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulamayı nasıl geçirdiğini gösterir. Contoso, uygulama sanal makinelerini ve veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için geçirmek için Site Recovery kullanır. | Kullanılabilir
[Makale 7: Azure sanal makineler'de Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Nasıl Contoso Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini Azure Vm'leri için Site RECOVERY'yi kullanarak yaptığını gösterir. | Kullanılabilir
[Makale 8: Azure sanal makineler ve Azure MySQL üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso Linux osTicket uygulaması için Azure Site RECOVERY'yi kullanarak VM'lerin nasıl geçirdiğini gösterir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
[Makale 9: bir uygulamayı Azure Web Apps ve Azure SQL veritabanında yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Nasıl Contoso SmartHotel uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanının Azure SQL Server örneğine geçirir gösterir | Kullanılabilir
[Makale 10: Azure Web Apps ve Azure MySQL üzerinde bir Linux uygulaması yeniden düzenleyin.](contoso-migration-refactor-linux-app-service-mysql.md) | Linux osTicket uygulaması Contoso birden çok sitede, GitHub ile sürekli teslim için tümleşik Azure Web Apps'e nasıl geçirdiğini gösterir. Bunlar, Azure MySQL örneğine uygulama veritabanına geçirin. | Kullanılabilir
[Makale 11: TFS VSTS üzerinde yeniden düzenleyin.](contoso-migration-tfs-vsts.md) | Geçiş yaparak Contoso şirket içi Team Foundation Server (TFS) dağıtımının nasıl geçirdiğini gösterir. Bunun için Visual Studio Team Services (VSTS) azure'da. | Kullanılabilir
[Makale 12: bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso geçirir ve Azure SmartHotel uygulamasının rearchitects nasıl gösterir. Bunlar, bir Windows kapsayıcısı ve bir Azure SQL veritabanı'nda uygulama veritabanı uygulama web katmanla yeniden oluşturma. | Kullanılabilir
[Makale 13: uygulamanızı Azure'a yeniden oluşturun.](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, uygulama hizmetleri, Azure Kubernetes, Azure işlevleri, Bilişsel hizmetler ve Cosmos DB dahil olmak üzere çeşitli kullanarak SmartHotel uygulamasının nasıl yeniden gösterir. | Kullanılabilir



Bu makalede, iki katmanlı Windows Contoso geçirir. Azure'a VMware Vm'lerinde çalışan NET SmartHotel uygulama. Bu uygulamayı kullanmak istiyorsanız, açık kaynaklı sağlanır ve buradan indirebileceğiniz [github](https://github.com/Microsoft/SmartHotel360).



## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso giderek ve sonuç olarak, şirket içi sistemler ve altyapı Basıncı yoktur.
- **Risk sınırlamak**: SmartHotel uygulamadır Contoso iş açısından kritik. Azure'a sıfır riskle taşımak istiyorum.
- **Genişletme**: Contoso uygulaması değiştirmek istediğiniz değil. Yalnızca kararlı olduğundan emin olmak isterler.


## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini bu geçiş için aşağı sabitlenmiş. Bu hedefleri, en iyi geçiş yöntemini belirlemek için kullanılır:

- Bugün, VMware içinde çalıştığı gibi geçişten sonra uygulamanızı Azure'a aynı performans özellikleri olmalıdır.  Uygulama, şirket içi olarak bulutta kritik olarak kalır. 
- Contoso, bu uygulamada yatırım yapmaya istememektedir.  İş için önemlidir, ancak mevcut haliyle yalnızca buluta güvenli bir şekilde taşımak istedikleri.
- Contoso, bu uygulama için ops modeli değiştirmek istememektedir. Bulutta, şimdi aynı şekilde etkileşim isterler.
- Contoso, herhangi bir uygulama işlevsellik değiştirmek istememektedir. Yalnızca uygulama konumu değişir.

## <a name="proposed-architecture"></a>Önerilen mimarisi

Geçerli ortamı aşağıdadır.

- Uygulama, iki VM arasında katmanlı (**WEBVM** ve **SQLVM**).
- Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com** (sürüm 6.5).
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).
- Geçiş tamamlandıktan sonra şirket içi Vm'leri Contoso veri merkezinde kullanımdan.

![Senaryo mimarisi](./media/contoso-migration-rehost-vm/architecture.png) 

## <a name="migration-process"></a>Geçiş işlemi

Contoso uygulama ön uç ve veritabanını VM'ler için Azure Site RECOVERY'yi kullanarak VM'lerin Geçir:

- İlk adım, bunlar hazırlamak ve Site Recovery için Azure bileşenlerini ayarlayın ve şirket içi VMware altyapısını hazırlama.
- Zaten sahip oldukları kendi [Azure altyapı](contoso-migration-infrastructure.md) yerine, bu nedenle bunlar yalnızca birkaç Azure bileşenlerini özellikle Site Recovery için eklemeniz gerekir.
- Her şey hazır, bunlar Vm'lerini çoğaltma başlatabilirsiniz.
-Çoğaltma etkinleştirildikten ve çalışma, bunlar VM'yi Azure'a devrederek tarafından geçirme sonra.

![Geçiş işlemi](./media/contoso-migration-rehost-vm/migraton-process.png) 



### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Hizmet düzenler ve geçiş ve olağanüstü durum kurtarma için Azure Vm'leri yönetir ve şirket içi Vm'leri ve fiziksel sunucuları.  | Azure'a çoğaltma sırasında Azure depolama ücretleri uygulanır.  Azure Vm'leri oluşturulur ve yük devretme işlemi gerçekleştiğinde, ücret. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/) ücretleri ve fiyatlandırma hakkında.


## <a name="prerequisites"></a>Önkoşullar

Siz (ve Contoso) Bu senaryo çalıştırmak gerekenleri aşağıda verilmiştir.

**Gereksinimleri** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Erken makaleleri aboneliğinde bu dizide oluşturmuş olmanız. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.<br/><br/> Daha ayrıntılı izinler gerekirse gözden [bu makalede](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure altyapı** | [Bilgi nasıl](contoso-migration-infrastructure.md) Contoso Azure altyapısının ayarlayın.<br/><br/> Özel hakkında daha fazla bilgi [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) Site Recovery için gereksinimleri.
**Şirket içi sunucular** | Şirket içinde vCenter sunucuları 5.5, 6.0 veya 6.5 sürümünü çalıştırmalıdır<br/><br/> ESXi ana sürüm 5.5, 6.0 veya 6.5 çalıştırmanız gerekir<br/><br/> Bir veya daha fazla VMware Vm'lerini ESXi konağı üzerinde çalıştırıyor olmalıdır.
**Şirket içi Vm'leri** | Vm'leri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Senaryo adımları

Contoso geçişi nasıl çalışacağını aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure Site Recovery için hazırlama**: Bunlar bir kurtarma Hizmetleri kasası ve çoğaltılan verileri tutmak için bir Azure depolama hesabı oluşturun.
> * **2. adım: Site Recovery için şirket içi Vmware'leri hazırlama**: VM bulma ve aracı yükleme hesabı hazırlayın ve yük devretme sonrasında Azure Vm'lerine bağlanmak için hazırlık yapma.
> * **3. adım: Çoğaltma Vm'leri**: çoğaltmayı ayarlama ve Azure depolama alanına Vm'leri çoğaltmaya başlayın.
> * **4. adım: Site Recovery ile Vm'leri geçirme**: her şeyin çalıştığından emin olmak için yük devretme testi çalıştırma ve ardından sanal makineleri Azure'a geçirmek için bir tam yük devretme çalıştırın.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>1. adım: Azure Site Recovery hizmeti için hazırlama

Contoso sanal makineleri Azure'a geçirmek için gereken Azure bileşenleri şunlardır:

- Yük devretme sırasında oluşturulduğunda, Azure Vm'leri yer alacağı bir sanal ağ.
- Çoğaltılan verileri tutmak için bir Azure depolama hesabı. 
- Azure kurtarma Hizmetleri kasasında.

Bunlar bu aşağıdaki ayarlamaları yapın:

1. Site Recovery için kullanabilecekleri bir ağ zaten kurma Contoso olduğunda bunlar [dağıtılan Azure altyapısı](contoso-migration-infrastructure.md)

    - Bir üretim uygulaması SmartHotel uygulamasıdır ve Vm'leri birincil Doğu ABD 2 bölgesinde Azure üretim ağına (VNET-PROD-EUS2) geçirilecektir.
    - Her iki VM üretim kaynaklar için kullanılan ContosoRG kaynak grubunda yer alır.
    - Uygulama ön uç VM (WEBVM), üretim ağındaki ön uç alt ağına (PROD-FE-EUS2) geçirir.
    - Uygulama veritabanı VM (SQLVM) bir üretim ağında alt ağ (PROD-DB-EUS2) veritabanı geçirir.

2. Contoso birincil bölgede bir Azure depolama hesabı (contosovmsacc20180528) oluşturur.
    - Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
    - Standart depolama ve LRS çoğaltma ile genel amaçlı bir hesabını kullanırlar. 

    ![Site Kurtarma Depolama](./media/contoso-migration-rehost-vm/asr-storage.png)

3. Ağ ve depolama hesabı ile yerinde, Contoso şimdi bir kurtarma Hizmetleri kasası (ContosoMigrationVault) oluşturur ve birincil Doğu ABD 2 bölgesinde ContosoFailoverRG kaynak grubunda yerleştirir.

    ![Kurtarma Hizmetleri kasası](./media/contoso-migration-rehost-vm/asr-vault.png)

**Daha fazla yardıma mı ihtiyacınız var?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Site Recovery için Azure'ı ayarlama.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>2. adım: Site Recovery için şirket içi Vmware'leri hazırlama

İşte şirket içinde Contoso hazırlar:

- VM bulmayı otomatikleştirmek için vCenter sunucusunda veya vSphere ESXi konağı üzerinde bir hesap.
- VMware Vm'lerinde Mobility hizmetini otomatik olarak yüklenmesini sağlayan bir hesap. 
- VM ayarlarını Contoso çoğaltılmış Azure Vm'lere yük devretme sonrasında bağlanabilmesi için şirket içi.


### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- VM'leri otomatik olarak bulma. 
- VM'ler için çoğaltma, yük devretme ve yeniden düzenleyin.
- En az bir salt okunur hesap gereklidir. Hesap oluşturma ve diskleri kaldırma ve Vm'leri kapatarak gibi işlemleri çalıştırmak mümkün olması gerekir.

Contoso hesabı gibi ayarlar:

1. Bunlar, bir rolü vCenter düzeyinde oluşturun.
2. Bunlar, bu rol gerekli izinleri atayın.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Her sanal makinede Mobility hizmetinin yüklenmesi gerekir.

- VM çoğaltma etkinleştirildiğinde site Recovery Mobility hizmeti yüklemesi otomatik gönderim yapabilirsiniz.
- Site Recovery Vm'leri göndererek yükleme erişebilmesi için bir hesap gereklidir. Çoğaltma ayarlamadan olduğunda bu hesabı belirtirsiniz.
- Hesap etki alanı veya yerel, Vm'lerde yüklemek için gerekli izinlere sahip olabilir.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Yük devretmeden sonra Azure Vm'lerine bağlanmak Contoso istiyor. Bunu yapmak için geçiş işleminden önce aşağıdakileri yapın:

1. İnternet üzerinden erişim için bunlar:

 - Yük devretmeden önce şirket içi VM'de RDP'yi etkinleştirin
 - İçin TCP ve UDP kurallarının eklendiğinden emin olun **genel** profili.
 - RDP'ye izin verildiğinden onay **Windows Güvenlik Duvarı** > **verilen uygulamaları** tüm profiller için.
 
2. Siteden siteye VPN üzerinden erişim için bunlar:

 - Şirket içi makinede RDP'yi etkinleştirin.
 - İçinde RDP'ye izin ver **Windows Güvenlik Duvarı** -> **izin verilen uygulamalar ve Özellikler**, için **etki alanı ve özel** ağlar.
 - Şirket içi VM'deki işletim sisteminin SAN ilkesinin ayarlamak **OnlineAll**.

Ayrıca, bir yük devretme çalıştırdıklarında bunlar aşağıdakileri denetlemeniz gerekir:

- Bulunmamalıdır bekleyen herhangi bir Windows güncelleştirme VM üzerinde bir yük devretme tetiklendiğinde. Varsa, güncelleştirme tamamlanana kadar sanal Makinede oturum açın mümkün olmayacaktır.
- Yük devretmeden sonra bunlar denetleyebilirsiniz **önyükleme tanılaması** VM'nin bir ekran görüntülemek için. Bu işe yaramazsa, bunlar VM çalıştıran ve bunlar gözden olduğunu doğrulamalısınız [sorun giderme ipuçları](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Daha fazla yardıma mı ihtiyacınız var?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) oluşturma ve Otomatik bulma için bir rol atama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) Mobility hizmetinin göndererek yüklenmesine ilişkin için bir hesabı oluşturuluyor.


## <a name="step-3-replicate-the-on-premises-vms"></a>3. adım: şirket içi sanal makineleri çoğaltma

Azure'da bir geçiş çalıştırmadan önce Contoso ayarlama ve çoğaltmayı etkinleştirme gerekiyor.

### <a name="set-a-replication-goal"></a>Çoğaltma hedefi ayarlama

1. Kasada kasa adını (ContosoVMVault) altında çoğaltma hedefi seçme (**Başlarken** > **Site Recovery** > **altyapıyı hazırlama** .
2. Bunlar, kendi makinelerine Vmware'de çalıştırılan ve Azure'a çoğaltılan şirket, olduğunu belirtin.

    ![Çoğaltma hedefi](./media/contoso-migration-rehost-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

Devam etmek için bunlar seçerek dağıtım planlamasını tamamladınız mı onaylayın **Evet, yaptım**. Bu senaryoda Contoso yalnızca iki sanal makine geçişi ve dağıtım planlama gerekmez.


### <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Contoso, kaynak ortamı yapılandırması gerektiğini belirtir. Bunu yapmak için bir OVF şablonu indirin ve Site Recovery yapılandırma sunucusunu yüksek oranda kullanılabilir olarak dağıtmak için kullanın, şirket içi VMware sanal makine. Yapılandırma sunucusunu çalışır duruma geldikten sonra bunlar bu kasaya kaydedin.

Yapılandırma sunucusu, çeşitli bileşenler çalıştırır:

- Şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir yapılandırma sunucusu bileşeni.
- İşlem sunucusu çoğaltma ağ geçidi davranır. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir.
- İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.



Contoso gibi aşağıdaki adımları gerçekleştirir:

1. Kasası'nda, OVF şablonu indirin **altyapıyı hazırlama** > **kaynak** > **yapılandırma sunucusu**.
    
    ![OVF indirin](./media/contoso-migration-rehost-vm/add-cs.png)

2. Bunlar, şablonu oluşturup VM dağıtmak için Vmware'e aktarın.

    ![OVF şablonu](./media/contoso-migration-rehost-vm/vcenter-wizard.png)

3.  Sanal makinede ilk kez açtığınızda, oluşturan bir Windows Server 2016 yükleme deneyimi önyüklenir. Bunlar lisans sözleşmesini kabul edin ve bir yönetici parolasını girin.
4. Yükleme tamamlandıktan sonra VM için yönetici olarak oturum açın. İlk oturum açma işleminde, varsayılan olarak Azure Site kurtarma Yapılandırması aracını çalıştırır.
5. Aracı'nda, yapılandırma sunucusunu kasaya kaydetmek için bir ad belirtin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra bunlar Azure aboneliği için oturum açın. Kimlik bilgileri, yapılandırma sunucusu kaydedeceksiniz kasa erişiminiz olması gerekir.

    ![Yapılandırma sunucusunu kaydetmek](./media/contoso-migration-rehost-vm/config-server-register2.png)

7. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır.
8. Bunların makinede yeniden oturum açın ve yapılandırma sunucusu yönetim Sihirbazı otomatik olarak başlar.
9. Sihirbazda, çoğaltma trafiğini almak için NIC'yi seçin. Bu ayar yapılandırıldıktan sonra değiştirilemez.
10. Abonelik, kaynak grubu ve yapılandırma sunucusunu kaydetmek istediğiniz kasaya seçerler.
        ![Kasa](./media/contoso-migration-rehost-vm/cswiz1.png) 

10. Bunlar, indirin ve MySQL Server ve VMWare powerclı'yı yükleyin. 
11. Doğrulama sonrasında, bunlar vCenter sunucusunda veya vSphere konağının FQDN'sini veya IP adresini belirtin. Bunlar, varsayılan bağlantı noktasını değiştirmeyin ve Azure'da sunucu için bir kolay ad belirtin.
12. Bunlar, bunlar otomatik bulma için oluşturduğunuz hesabı ve otomatik olarak Mobility hizmetini yükleme için kullanılan kimlik bilgilerini belirtin. Windows makineleri için hesabın vm'lerinde yerel yönetici ayrıcalıkları gerekir.

    ![vCenter](./media/contoso-migration-rehost-vm/cswiz2.png)

7. Kayıt tamamlandığında Azure Portalı'nda yapılandırma sunucusunun ve VMware sunucusunun listelendiğini Contoso çift denetler **kaynak** kasadaki sayfası. Bulma, 15 dakika veya daha fazla sürebilir. 
8. Site Recovery belirtilen ayarları kullanarak VMware sunucularına bağlanır ve Vm'leri bulur.

### <a name="set-up-the-target"></a>Hedefi ayarlama

Artık Contoso hedef çoğaltma ayarlarını belirtir.

1. İçinde **altyapıyı hazırlama** > **hedef**, bunlar hedef ayarları seçin.
2. Site Recovery, bir Azure depolama hesabını ve belirtilen hedef konum ağında olup olmadığını denetler.

### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Artık Contoso bir çoğaltma ilkesi oluşturabilirsiniz.

1. İçinde **altyapıyı hazırlama** > **çoğaltma ayarları** > **Çoğaltma İlkesi** >  **oluşturma ve İlişkilendirme**, bunlar bir ilke oluşturmak **ContosoMigrationPolicy**.
2. Bunlar, varsayılan ayarları kullanın:
    - **RPO eşiği**: varsayılan 60 dakika. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
    - **Kurtarma noktası bekletme**. Varsayılan 24 saat. Bu değer, ne kadar süreyle her kurtarma noktası için bekletme süresinin olacağını belirtir. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**. Varsayılan bir saat değeri. Bu değer, uygulamayla tutarlı anlık görüntülerin oluşturulma sıklığı belirtir.

        ![Çoğaltma ilkesi oluşturma](./media/contoso-migration-rehost-vm/replication-policy.png)

5. İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. 

    ![Çoğaltma İlkesi ilişkilendirme](./media/contoso-migration-rehost-vm/replication-policy2.png)

### <a name="enable-replication-for-webvm"></a>WEBVM için çoğaltmayı etkinleştirme

Olan her şeyi yerinde, Contoso VM'ler için çoğaltmayı etkinleştirebilirsiniz. Bunlar WebVM ile başlayın.

1. İçinde **uygulama çoğaltma** > **kaynak** > **+ Çoğalt** bunlar kaynak ayarlarını seçin.
2. Bunlar, bunlar, Vm'leri etkinleştirmek istiyorsanız, vCenter sunucusu ile configuration server seçin gösterir.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication1.png)

3. Bunlar, kaynak grubu ve Azure ağ ve depolama hesabı dahil olmak üzere hedef ayarlarını seçin.

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication2.png)

4. Contoso seçer **WebVM** çoğaltma için çoğaltma ilkesini denetler ve çoğaltmayı etkinleştirir.

    - Bu aşamada Contoso yalnızca VNet ve alt ağ seçilmelidir ve Contoso uygulama sanal makinelerini farklı alt ağlarda yerleştiriyor WEBVM seçer.
    - Çoğaltma etkinleştirildiğinde site Recovery Mobility hizmeti VM üzerinde otomatik olarak yükler.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication3.png)

5. Bunlar çoğaltma ilerlemeyi **işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.
6. İçinde **Essentials** Azure portalında, Azure'a çoğaltılan VM'ler için yapı Contoso görebilirsiniz.


### <a name="enable-replication-for-sqlvm"></a>SQLVM için çoğaltmayı etkinleştirme

Artık Contoso SQLVM makineyi çoğaltmak, aynı işlemi olarak yukarıdaki kullanarak başlayabilirsiniz.

1. Bunlar, kaynak ayarlarını seçin.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication1.png)

2. Bunlar daha sonra hedef ayarlarını belirtin.

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication2-sqlvm.png)

3. Çoğaltma için SQLVM seçerler. 

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication3-sqlvm.png)

4. Bunlar WEBVM için kullanılan aynı çoğaltma ilkesine uygulamak ve çoğaltmayı etkinleştirin.

    ![Altyapı görünümü](./media/contoso-migration-rehost-vm/essentials.png)

**Daha fazla yardıma mı ihtiyacınız var?**

- Bu adımlar tam bir kılavuza edinebilirsiniz [şirket içi VMware Vm'leri için olağanüstü durum kurtarmayı ayarlayın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Yardımcı olmak ayrıntılı yönergeler kullanılabilir [kaynak ortamını ayarlama](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [yapılandırma sunucusunu dağıtma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), ve [çoğaltma ayarlarını yapılandırma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- Daha fazla bilgi edinebilirsiniz [çoğaltma etkinleştirme](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>4. adım: sanal makineleri geçirme 

Contoso hızlı test yük devretme ve Vm'leri geçirme için tam bir yük devretme çalıştırır.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Her şeyin beklendiği gibi çalıştığından emin olmak için yük devretme testi yardımcı olur. 

1. Contoso zaman en son kullanılabilir noktaya yük devretme testi çalıştırır (**en son işlenen**).
2. Seçmeleri **yük devretmeye başlamadan önce makineyi Kapat**, böylece Site Recovery yük devretmeyi tetiklemeden önce kaynak sanal kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. 
3. Test yük devretme çalıştırır: 

    - Bir önkoşul denetimi geçiş için gerekli koşulları tümünün karşılandığından emin olmak için çalıştırılır.
    - Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
    - Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.
    
3. Yük devretme bittikten sonra çoğaltma Azure VM Azure Portalı'nda görünür. Contoso, VM doğru ağa bağlı uygun boyutta olduğundan ve çalışır durumda olduğunu denetler. 
4. Test yük devretmesi doğruladıktan sonra bunlar devretmeyi temizlemek ve gözlemlerinizi kaydetmek ve. 

### <a name="create-and-customize-a-recovery-plan"></a>Oluşturma ve bir kurtarma planı özelleştirme

 Contoso, yük devretme testi beklendiği gibi çalıştığını doğruladıktan sonra geçiş için bir kurtarma planı oluşturur. 

- Bir kurtarma planı sırasını belirtir. yük devretme gerçekleşir ve nasıl Azure Vm'leri Azure'da çevrimiçi kapsama dahil edilecektir gösterir.
- İki katmanlı bir uygulama olduğundan, verileri ön uç (WEBVM) önce VM (SQLVM) başlatır. böylece, kurtarma planını özelleştirin.

1. İçinde **kurtarma planları (Site Recovery)** > **+ kurtarma planı**, bir plan oluşturur ve sanal makineleri ekleyin.

    ![Kurtarma planı](./media/contoso-migration-rehost-vm/recovery-plan.png)

2. Planı oluşturduktan sonra bunlar özelleştirin (**kurtarma planları** > **SmartHotelMigrationPlan** > **Özelleştir**).
2.  Bunlar WEBVM öğesinden kaldırmak **Grup 1: Başlangıç**.  Bu, ilk başlatma eylemini SQLVM yalnızca etkiler sağlar.
3.  İçinde **+ grup** > **Ekle korumalı öğeler**, Grup 2 WEBVM ekledikleri: başlatın.  İki farklı gruplardaki VM'lerin gerekir.


### <a name="migrate-the-vms"></a>Vm'leri geçirme


Artık Contoso Geçişi tamamlamak için tam bir yük devretme çalıştırabilirsiniz.

1. Kurtarma planı seçmeleri > **yük devretme**.
2. Bunlar en son kurtarma noktasına yük devretmek için seçin ve o Site Recovery yük devretmeyi tetiklemeden önce şirket içi sanal makineyi çalışmanız gerekir. Yöneticiler yük devretme işleminin ilerleyişini izleyebilirsiniz **işleri** sayfası.

    ![Yük devretme](./media/contoso-migration-rehost-vm/failover1.png)


3. Yük devretmeden sonra bunlar Azure VM Azure Portalı'nda beklendiği gibi göründüğünü doğrulayın.

    ![Yük devretme](./media/contoso-migration-rehost-vm/failover2.png)  

3. Doğrulamadan sonra bunlar her VM için geçişi tamamlayın. Bu VM için çoğaltma durdurulur ve onun için Site Recovery Faturalaması durdurulur.

    ![Yük devretme](./media/contoso-migration-rehost-vm/failover3.png)

**Daha fazla yardıma mı ihtiyacınız var?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) yük devretme testi çalıştırma. 
- [Bilgi](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) bir kurtarma planı oluşturma.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Azure'a devretmek.

## <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Tam geçişi ile SmartHotel uygulama katmanlarında artık Azure Vm'leri üzerinde çalışıyor.

Şimdi, Contoso temizleme adımları tamamlaması gerekir:  

- VCenter stok WEBVM makine kaldırın.
- SQLVM makine vCenter stok kaldırın.
- WEBVM ve sqlvm ADLI yerel yedekleme işlerden kaldırın.
- VM'ler için yeni konum ve IP adresleri göstermek için iç belgeleri güncelleştirin.
- Sanal makineleri ile etkileşim kuran tüm kaynakları gözden geçirin ve herhangi bir ilgili ayarları veya belgeleri yeni yapılandırmayı yansıtacak şekilde güncelleştirin.

## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Artık çalışan bir uygulamayla Contoso artık tam olarak çalışır hale getirme ve Azure'da güvenli gerekir.

### <a name="security"></a>Güvenlik

Contoso güvenlik ekibi, güvenlik sorunları belirlemek için Azure Vm'leri inceler.

- Erişimi denetlemek için ağ güvenlik grupları (Nsg'ler) VM'ler için gözden geçirin. Nsg'ler, yalnızca uygulamaya izin trafik, ulaşabileceği emin olmak için kullanılır.
- Ayrıca Azure Disk şifreleme ve anahtar Kasası'nı kullanarak diskteki verilerin güvenliğini sağlama düşünün.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) VM'ler için önerilen güvenlik uygulamaları hakkında.

### <a name="backups"></a>Yedeklemeler

Azure Backup hizmetini kullanarak sanal makinelerin verilerini yedeklemek için contoso geçiyor. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

1. Contoso mevcut Vm'leri için lisans sahiptir ve Azure hibrit avantajı özelliğinden yararlanır.  Bunlar mevcut Azure bu fiyatlandırmanın avantajlarından yararlanmak için sanal makinelerin dönüştürür.
2. Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet Yönetimi olanak sağlar. Bu Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olan bir çoklu bulut maliyet yönetimi çözümüdür. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında. 

## <a name="conclusion"></a>Sonuç

Bu makalede, Contoso uygulama sanal makinelerini Site Recovery hizmetini kullanarak Azure Vm'lerine geçiş yaparak Azure SmartHotel uygulamada rehosted. 


## <a name="next-steps"></a>Sonraki adımlar

İçinde [sonraki makalede](contoso-migration-rehost-vm-sql-ag.md) dizide nasıl Contoso SmartHotel uygulama ön uç bir Azure sanal makinesinde VM rehosts ve azure'da bir SQL Server AlwaysOn Kullanılabilirlik grubu veritabanı geçirir göstereceğiz.

