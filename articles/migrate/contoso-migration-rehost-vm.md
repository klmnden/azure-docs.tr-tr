---
title: Azure Site Recovery ile Azure vm'lerine geçiş ile bir Contoso uygulaması yeniden barındırma | Microsoft Docs
description: Şirket içi lift-and-shift ile taşıma geçişini ile şirket içi barındırma uygulaması Azure Site Recovery hizmetini kullanarak Azure'a nasıl makineleri öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: 4a6ed900753747c1d5bf394aced54da11177320f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60668426"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms"></a>Contoso geçişi: Şirket içi bir uygulamayı Azure VM’lerde yeniden barındırma


Bu makalede, Contoso uygulama sanal makinelerini Azure Vm'lerine geçiş yaparak, azure'da şirket içi SmartHotel360 uygulamayı nasıl rehosts gösterilmektedir.


Bu belge, Contoso adlı kurgusal şirketin şirket içi kaynaklara Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapı kurulumu, geçiş için şirket içi kaynaklarınızı değerlendirerek ve geçişleri farklı türlerde çalıştıran gösteren senaryolar içerir. Senaryoları, karmaşık hale gelmesi. Zaman içinde ek makaleleri ekleyeceğiz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[1. makale: Genel bakış](contoso-migration-overview.md) | Makale serisi, Contoso'nun geçiş stratejisi ve dizisinde kullanılan örnek uygulamalar genel bakış. | Kullanılabilir
[2. makale: Azure altyapısı dağıtma](contoso-migration-infrastructure.md) | Contoso şirket içi altyapısını ve Azure altyapısını geçiş için hazırlar. Altyapıyı, serideki tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: Şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](contoso-migration-assessment.md)  | Contoso, Vmware'de çalıştırılan şirket içi SmartHotel360 uygulamasının bir değerlendirme çalışır. Contoso Azure geçişi hizmeti ve veri geçiş Yardımcısı'nı kullanarak uygulama SQL Server veritabanı kullanarak uygulama Vm'leri değerlendirir. | Kullanılabilir
[4. makale: Bir Azure VM ve SQL veritabanı yönetilen örneği bir uygulamada barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso, Azure'a lift-and-shift ile taşıma geçiş için kendi şirket içi SmartHotel360 uygulaması çalışır. Contoso geçirir uygulama ön uç VM kullanarak [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso geçirir uygulama veritabanını kullanarak bir Azure SQL veritabanı yönetilen örneği [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir
Makale 5: Bir uygulamayı Azure VM’lerinde yeniden barındırma | Contoso, SmartHotel360 uygulama sanal makinelerini Azure Site Recovery hizmetini kullanarak sanal makineleri geçirir. | Bu makalede
[Makale 6: Azure sanal makinelerinde ve SQL Server AlwaysOn Kullanılabilirlik grubuna bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel360 uygulamaya geçirir. Contoso, uygulama sanal makinelerini geçirmek için Site Recovery kullanır. Veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için kullanır. | Kullanılabilir
[Makale 7: Azure vm'lerinde Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Azure Site Recovery kullanarak Azure vm'lerine Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini contoso tamamlar | Kullanılabilir
[Makale 8: Azure sanal makineler ve Azure MySQL üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso, Azure Site Recovery kullanarak Azure Vm'leri için Linux osTicket uygulaması geçirir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
[Makale 9: Azure Web Apps ve Azure SQL veritabanında bir uygulamayı yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Contoso SmartHotel360 uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanı için veritabanı geçiş Yardımcısı'nı kullanarak bir Azure SQL Server örneği geçirir | Kullanılabilir
[Makale 10: Azure Web Apps ve Azure MySQL üzerinde bir Linux uygulamayı yeniden düzenleme](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso, bir Azure web uygulamasına GitHub ile sürekli teslim için tümleşik Azure Traffic Manager'ı kullanarak birden fazla Azure bölgesini üzerinde kendi Linux osTicket uygulaması geçirir. Contoso uygulaması veritabanı örneği MySQL için Azure veritabanı geçirir. | Kullanılabilir
[11. makale: Azure DevOps hizmetlerinde TFS yeniden düzenleyin](contoso-migration-tfs-vsts.md) | Contoso, Azure DevOps Hizmetleri azure'da, şirket içi Team Foundation Server dağıtımı geçirir. | Kullanılabilir
[Makale 12: Bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso, SmartHotel uygulamayı Azure'a geçirir. Ardından, Azure Service Fabric ve Azure SQL veritabanı ile veritabanı çalıştıran bir Windows kapsayıcısı olarak app web katmanından rearchitects. | Kullanılabilir
[Makale 13: Uygulamanızı Azure'a yeniden oluşturun](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, Azure App Service, Azure Kubernetes Service (AKS), Azure işlevleri, Azure Bilişsel hizmetler ve Azure Cosmos DB dahil olmak üzere çeşitli kullanarak kendi SmartHotel uygulaması oluşturur. | Kullanılabilir
[Makale 14: Azure'a geçiş ölçeklendirin](contoso-migration-scale.md) | Geçiş birleşimleri denedikten sonra Contoso Azure tam geçişi ölçeklendirilebilecek şekilde hazırlar. | Kullanılabilir


Bu makalede, iki katmanlı Windows Contoso geçirir. Azure'a VMware Vm'lerinde çalışan NET SmartHotel360 uygulama. Bu uygulamayı kullanmak istiyorsanız, açık kaynaklı sağlanır ve buradan indirebileceğiniz [GitHub](https://github.com/Microsoft/SmartHotel360).



## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso büyüyor ve sonuç olarak, şirket içi sistemler ve altyapı Basıncı yoktur.
- **Risk sınırlamak**: SmartHotel360 uygulama Contoso işletmeler için önemlidir. Sıfır riskle uygulamayı Azure'a taşımak istiyor.
- **Genişletme**: Contoso uygulaması değiştirmek istediğiniz değil ancak kararlı olduğundan emin olun ister.


## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini bu geçiş için aşağı sabitlenmiş. Bu hedefleri, en iyi geçiş yöntemini belirlemek için kullanılır:

- Bugün, VMware içinde çalıştığı gibi geçişten sonra uygulamanızı Azure'a aynı performans özellikleri olmalıdır. Uygulama, şirket içi olarak bulutta kritik olarak kalır.
- Contoso, bu uygulamada yatırım yapmaya istememektedir. İş için önemlidir, ancak mevcut haliyle Contoso yalnızca buluta güvenli bir şekilde taşımak ister.
- Contoso, bu uygulama için ops modeli değiştirmek istememektedir. Contoso istediğiniz bulutta, şimdi aynı şekilde etkileşim.
- Contoso, herhangi bir uygulama işlevsellik değiştirmek istememektedir. Yalnızca uygulama konumu değişir.


## <a name="solution-design"></a>Çözüm tasarımı

Hedefleri ve gereksinimleri sabitleme sonra Contoso tasarlar ve bir dağıtım çözümü gözden geçirin ve Contoso geçiş için kullanacağınız Azure hizmetlerini de dahil olmak üzere geçiş işlemi tanımlar.

### <a name="current-app"></a>Geçerli uygulama

- Uygulama, iki VM arasında katmanlı (**WEBVM** ve **SQLVM**).
- Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com** (sürüm 6.5).
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).

### <a name="proposed-architecture"></a>Önerilen mimarisi

- Uygulamayı bir üretim iş yükü olduğundan, uygulama sanal makinelerini azure'da ContosoRG üretim kaynak grubunda yer alacaktır.
- Uygulama sanal makinelerini Birincil Azure bölge (Doğu ABD 2) için geçirilen ve üretim ağı (VNET-PROD-EUS2) yerleştirilir.
- VM web ön uç (FE-PROD-EUS2) üretim ağındaki frontend alt yer alacaktır.
- ' % S'veritabanı VM üretim ağındaki veritabanı alt (PROD-DB-EUS2) yer alacaktır.
- Geçiş tamamlandıktan sonra şirket içi Vm'leri Contoso veri merkezinde kullanımdan.

![Senaryo mimarisi](./media/contoso-migration-rehost-vm/architecture.png)

### <a name="database-considerations"></a>Veritabanı konuları

Çözüm tasarımı işleminin bir parçası olarak, bir Azure SQL veritabanı ve SQL Server arasındaki özellik karşılaştırması Contoso vermedi. Aşağıdaki konuları ile bir Azure Iaas sanal makinesinde çalışan SQL Server gitmek karar vermek için yaşadıkları:

- SQL Server çalıştıran bir Azure VM kullanarak bir en iyi çözüm Contoso işletim sistemini veya veritabanı sunucusu özelleştirmek gerekiyorsa ya da birlikte bulundurma ve üçüncü taraf uygulamalar aynı VM'de çalıştırmak isteyebilirsiniz gibi görünüyor.
- Yazılım Güvencesi içeren SQL veritabanı yönetilen SQL Server için Azure hibrit teklifi kullanarak örneği indirimli Fiyatlardan için var olan lisans Contoso gelecekte gönderip alabilir. Bu, yönetilen örneği'nde % 30 tasarruf edebilirsiniz.



### <a name="solution-review"></a>Çözümü gözden geçirme

Contoso, Artıları ve eksileri listesini birbirine koyarak önerilen tasarım değerlendirir.

**Önemli noktalar** | **Ayrıntılar**
--- | ---
**Uzmanları** | Uygulama Vm'leri Azure'a geçiş basit hale değişikliğe gerek kalmadan taşınır.<br/><br/> Her iki uygulama Vm'leri için contoso lift-and-shift ile taşıma kullandığından, özel bir yapılandırma veya geçiş araçları uygulama veritabanı için gereklidir.<br/><br/> Contoso, Azure hibrit Avantajı'nı kullanarak, Yazılım Güvencesi yatırımları yararlanabilirsiniz.<br/><br/> Contoso, uygulamayı azure'da sanal makineler üzerinde tam denetim korur.
**Simgeler** | WEBVM ve sqlvm ADLI Windows Server 2008 R2 çalıştırıyorsanız. İşletim sistemi için belirli rolleri (Temmuz 2018) Azure tarafından desteklenir. [Daha fazla bilgi edinin](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).<br/><br/> Uygulama web ve veri katmanları yük devretme tek bir noktadan devam edecektir.</br><br/> SQLVM, ana akım desteği olmayan SQL Server 2008 R2 üzerinde çalışıyor. Ancak (Temmuz 2018) Azure Vm'leri için desteklenir. [Daha fazla bilgi edinin](https://support.microsoft.com/en-us/help/956893).<br/><br/> Contoso uygulaması Azure Vm'leri olarak destekleyen yerine Azure App Service ve Azure SQL veritabanı gibi yönetilen bir hizmet taşıma devam etmesi gerekir.



### <a name="migration-process"></a>Geçiş işlemi

Contoso, uygulama ön ucu ve veritabanı Vm'leri, Site Recovery ile Azure Vm'leri için geçirir:

- İlk adım, Contoso hazırlar ve Azure bileşenlerini Site Recovery için ayarlar ve şirket içi VMware altyapısı hazırlar.
- Zaten sahip oldukları [Azure altyapı](contoso-migration-infrastructure.md) yerinde, böylece Contoso yalnızca gerekiyor özellikle Site Recovery için birkaç Azure bileşenlerini ekleyin.
- Her şey hazır, Contoso Vm'lerini çoğaltma başlatabilirsiniz.
- Çoğaltma etkinleştirildikten sonra çalışma, Contoso sanal Makineyi Azure'a devrederek tarafından geçirir.

![Geçiş işlemi](./media/contoso-migration-rehost-vm/migraton-process.png)



### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Hizmet düzenler ve geçiş ve olağanüstü durum kurtarma için Azure Vm'leri yönetir ve şirket içi Vm'leri ve fiziksel sunucuları.  | Azure'a çoğaltma sırasında Azure depolama ücretleri uygulanır. Azure Vm'leri oluşturulur ve yük devretme işlemi gerçekleştiğinde, ücret. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/) ücretleri ve fiyatlandırma hakkında.


## <a name="prerequisites"></a>Önkoşullar

Bu senaryo çalıştırmak için Contoso gerekenler aşağıda verilmiştir.

**Gereksinimler** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Contoso abonelikleri daha önceki bir makalede bu serideki oluşturdunuz. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.<br/><br/> Daha ayrıntılı izinler gerekirse gözden [bu makalede](../site-recovery/site-recovery-role-based-linked-access-control.md).
**Azure altyapı** | [Bilgi nasıl](contoso-migration-infrastructure.md) Contoso Azure altyapısının ayarlayın.<br/><br/> Özel hakkında daha fazla bilgi [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) Site Recovery için gereksinimleri.
**Şirket içi sunucular** | Şirket içinde vCenter sunucuları 5.5, 6.0 veya 6.5 sürümünü çalıştırmalıdır<br/><br/> ESXi ana sürüm 5.5, 6.0 veya 6.5 çalıştırmanız gerekir<br/><br/> Bir veya daha fazla VMware Vm'lerini ESXi konağı üzerinde çalıştırıyor olmalıdır.
**Şirket içi Vm'leri** | Vm'leri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Senaryo adımları

Contoso yöneticileri geçişi nasıl çalışacağını aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure Site Recovery için hazırlama**: Bunlar, bir kurtarma Hizmetleri kasası ve çoğaltılan verileri tutmak için bir Azure depolama hesabı oluşturun.
> * **2. adım: Site Recovery için şirket içi Vmware'leri hazırlama**: Bunlar için VM bulma ve aracı yükleme hesabı hazırlayın ve yük devretme sonrasında Azure Vm'lerine bağlanmak için hazırlık yapma.
> * **3. adım: Vm'lerini çoğaltma**: Bunlar, çoğaltmayı ayarlama ve Azure depolama alanına Vm'leri çoğaltmaya başlayın.
> * **4. adım: Site Recovery ile Vm'leri geçirme**: Bunlar her şeyin çalıştığından emin olmak için bir yük devretme testi çalıştırın ve ardından sanal makineleri Azure'a geçirmek için bir tam yük devretme çalıştırın.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>1. Adım: Azure Site Recovery hizmeti için hazırlama

Contoso sanal makineleri Azure'a geçirmek için gereken Azure bileşenleri şunlardır:

- Yük devretme sırasında oluşturulduğunda, Azure Vm'leri yer alacağı bir sanal ağ.
- Çoğaltılan verileri tutmak için bir Azure depolama hesabı.
- Azure kurtarma Hizmetleri kasasında.

Bunlar bu aşağıdaki ayarlamaları yapın:

1. Site Recovery için ağ kurma zaten bir ağ Contoso ayarlayın, bunlar [dağıtılan Azure altyapısı](contoso-migration-infrastructure.md)

    - Bir üretim uygulaması SmartHotel360 uygulamasıdır ve Vm'leri birincil Doğu ABD 2 bölgesinde Azure üretim ağına (VNET-PROD-EUS2) geçirilecektir.
    - Her iki VM üretim kaynaklar için kullanılan ContosoRG kaynak grubunda yer alır.
    - Uygulama ön uç VM (WEBVM), üretim ağındaki ön uç alt ağına (PROD-FE-EUS2) geçirir.
    - Uygulama veritabanı VM (SQLVM) bir üretim ağında alt ağ (PROD-DB-EUS2) veritabanı geçirir.

2. Bir depolama hesabı-Contoso ayarlama birincil bölgede bir Azure depolama hesabı (contosovmsacc20180528) oluşturur.
   - Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
   - Standart depolama ve LRS çoğaltma ile genel amaçlı bir hesabını kullanırlar.

     ![Site Kurtarma Depolama](./media/contoso-migration-rehost-vm/asr-storage.png)

3. Bir kasa ile yerinde ağ ve depolama hesabı oluşturma, Contoso şimdi bir kurtarma Hizmetleri kasası (ContosoMigrationVault) oluşturulur ve birincil Doğu ABD 2 bölgelerindeki ContosoFailoverRG kaynak grubundaki yerleştirir.

    ![Kurtarma Hizmetleri kasası](./media/contoso-migration-rehost-vm/asr-vault.png)

**Daha fazla yardıma mı ihtiyacınız var?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Site Recovery için Azure'ı ayarlama.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>2. Adım: Site Recovery için şirket içi Vmware'leri hazırlama

İşte şirket içinde Contoso hazırlar:

- VM bulmayı otomatikleştirmek için vCenter sunucusunda veya vSphere ESXi konağı üzerinde bir hesap.
- VMware Vm'lerinde Mobility hizmetini otomatik olarak yüklenmesini sağlayan bir hesap.
- VM ayarlarını Contoso çoğaltılmış Azure Vm'lere yük devretme sonrasında bağlanabilmesi için şirket içi.


### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- VM'leri otomatik olarak bulma.
- VM'ler için çoğaltma, yük devretme ve yeniden düzenleyin.
- En az bir salt okunur hesap gereklidir. Hesap oluşturma ve diskleri kaldırma ve Vm'leri kapatarak gibi işlemleri çalıştırmak mümkün olması gerekir.

Contoso admins hesabı aşağıdaki gibi ayarlayın:

1. Bunlar, bir rolü vCenter düzeyinde oluşturun.
2. Bunlar, bu rol gerekli izinleri atayın.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Her sanal makinede Mobility hizmetinin yüklenmesi gerekir.

- VM çoğaltma etkinleştirildiğinde site Recovery Mobility hizmeti yüklemesi otomatik gönderim yapabilirsiniz.
- Site Recovery Vm'leri göndererek yükleme erişebilmesi için bir hesap gereklidir. Çoğaltma ayarlamadan olduğunda bu hesabı belirtirsiniz.
- Hesap etki alanı veya yerel, Vm'lerde yüklemek için gerekli izinlere sahip olabilir.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Yük devretmeden sonra Azure Vm'lerine bağlanmak Contoso istiyor. Bunu yapmak için Contoso yöneticileri geçişten önce aşağıdakileri yapın:

1. İnternet üzerinden erişim için bunlar:

   - Yük devretmeden önce şirket içi VM'de RDP'yi etkinleştirin.
   - İçin TCP ve UDP kurallarının eklendiğinden emin olun **genel** profili.
   - RDP'ye izin verildiğinden onay **Windows Güvenlik Duvarı** > **verilen uygulamaları** tüm profiller için.

2. Siteden siteye VPN üzerinden erişim için bunlar:

   - Şirket içi makinede RDP'yi etkinleştirin.
   - İçinde RDP'ye izin ver **Windows Güvenlik Duvarı** -> **izin verilen uygulamalar ve Özellikler**, için **etki alanı ve özel** ağlar.
   - Şirket içi VM'deki işletim sisteminin SAN ilkesinin ayarlamak **OnlineAll**.

Ayrıca, bir yük devretme çalıştırdıklarında bunlar aşağıdakileri denetlemeniz gerekir:

- Bulunmamalıdır bekleyen herhangi bir Windows güncelleştirme VM üzerinde bir yük devretme tetiklendiğinde. Varsa, güncelleştirme tamamlanana kadar sanal Makinede oturum açın mümkün olmayacaktır.
- Yük devretmeden sonra bunlar denetleyebilirsiniz **önyükleme tanılaması** VM'nin bir ekran görüntülemek için. Bu işe yaramazsa, bunlar VM çalıştıran ve bunlar gözden olduğunu doğrulamalısınız [sorun giderme ipuçları](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Daha fazla yardıma mı ihtiyacınız var?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) oluşturma ve Otomatik bulma için bir rol atama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) Mobility hizmetinin göndererek yüklenmesine ilişkin için bir hesabı oluşturuluyor.


## <a name="step-3-replicate-the-on-premises-vms"></a>3. Adım: Şirket içi sanal makinelerini çoğaltma

Contoso yöneticileri, Azure'da bir geçiş çalıştırabilmeniz için önce ayarlama ve çoğaltmayı etkinleştirme gerekir.

### <a name="set-a-replication-goal"></a>Çoğaltma hedefi ayarlama

1. Kasada kasa adını (ContosoVMVault) altında çoğaltma hedefi seçme (**Başlarken** > **Site Recovery** > **altyapıyı hazırlama** .
2. Bunlar, kendi makinelerine Vmware'de çalıştırılan ve Azure'a çoğaltılan şirket, olduğunu belirtin.

    ![Çoğaltma hedefi](./media/contoso-migration-rehost-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

Devam etmek için bunlar seçerek dağıtım planlamasını tamamladınız mı onaylayın **Evet, yaptım**. Bu senaryoda Contoso yalnızca iki sanal makine geçişi ve dağıtım planlama gerekmez.


### <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamı yapılandırmak contoso yöneticilerinin gerekir. Bunu yapmak için bir OVF şablonu indirin ve Site Recovery yapılandırma sunucusunu yüksek oranda kullanılabilir olarak dağıtmak için kullanın, şirket içi VMware sanal makine. Yapılandırma sunucusunu çalışır duruma geldikten sonra bunlar bu kasaya kaydedin.

Yapılandırma sunucusu, çeşitli bileşenler çalıştırır:

- Şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir yapılandırma sunucusu bileşeni.
- İşlem sunucusu çoğaltma ağ geçidi davranır. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir.
- İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.



Contoso yöneticileri bu adımları aşağıdaki gibi gerçekleştirin:

1. Kasası'nda, OVF şablonu indirin **altyapıyı hazırlama** > **kaynak** > **yapılandırma sunucusu**.
    
    ![OVF indirin](./media/contoso-migration-rehost-vm/add-cs.png)

2. Bunlar, şablonu oluşturup VM dağıtmak için Vmware'e aktarın.

    ![OVF şablonu](./media/contoso-migration-rehost-vm/vcenter-wizard.png)

3. Sanal makinede ilk kez açtığınızda, oluşturan bir Windows Server 2016 yükleme deneyimi önyüklenir. Bunlar lisans sözleşmesini kabul edin ve bir yönetici parolasını girin.
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

7. Azure portalında kayıt tamamlandıktan sonra çift bunlar üzerinde yapılandırma sunucusunun ve VMware sunucusunun listelenip listelenmediğini denetleyin **kaynak** kasadaki sayfası. Bulma, 15 dakika veya daha fazla sürebilir.
8. Site Recovery belirtilen ayarları kullanarak VMware sunucularına bağlanır ve Vm'leri bulur.

### <a name="set-up-the-target"></a>Hedefi ayarlama

Artık Contoso yöneticileri, hedef çoğaltma ayarlarını belirtirsiniz.

1. İçinde **altyapıyı hazırlama** > **hedef**, bunlar hedef ayarları seçin.
2. Site Recovery, bir Azure depolama hesabını ve belirtilen hedef konum ağında olup olmadığını denetler.

### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Artık Contoso yöneticileri bir çoğaltma ilkesi oluşturabilirsiniz.

1. İçinde **altyapıyı hazırlama** > **çoğaltma ayarları** > **Çoğaltma İlkesi** >  **oluşturma ve İlişkilendirme**, bunlar bir ilke oluşturmak **ContosoMigrationPolicy**.
2. Bunlar, varsayılan ayarları kullanın:
    - **RPO eşiği**: Varsayılan 60 dakika. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
    - **Kurtarma noktası bekletme**. Varsayılan 24 saat. Bu değer, ne kadar süreyle her kurtarma noktası için bekletme süresinin olacağını belirtir. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**. Varsayılan bir saat değeri. Bu değer, uygulamayla tutarlı anlık görüntülerin oluşturulma sıklığı belirtir.

        ![Çoğaltma ilkesi oluşturma](./media/contoso-migration-rehost-vm/replication-policy.png)

5. İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir.

    ![Çoğaltma İlkesi ilişkilendirme](./media/contoso-migration-rehost-vm/replication-policy2.png)

### <a name="enable-replication-for-webvm"></a>WEBVM için çoğaltmayı etkinleştirme

Olan her şeyi yerinde, Contoso yöneticileri artık VM'ler için çoğaltmayı etkinleştirebilirsiniz. Bunlar WebVM ile başlayın.

1. İçinde **uygulama çoğaltma** > **kaynak** > **+ Çoğalt** bunlar kaynak ayarlarını seçin.
2. Bunlar, bunlar, Vm'leri etkinleştirmek istiyorsanız, vCenter sunucusu ile configuration server seçin gösterir.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication1.png)

3. Bunlar, kaynak grubu ve Azure ağ ve depolama hesabı dahil olmak üzere hedef ayarlarını seçin.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication2.png)

4. Seçmeleri **WebVM** çoğaltma için çoğaltma ilkesi denetleyin ve çoğaltmayı etkinleştirin.

   - Bu aşamada, yalnızca seçer WEBVM VNet ve alt ağ seçilmelidir ve uygulama sanal makinelerini farklı alt ağlarda yer alır.
   - Çoğaltma etkinleştirildiğinde site Recovery Mobility hizmeti VM üzerinde otomatik olarak yükler.

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm/enable-replication3.png)

5. Bunlar çoğaltma ilerlemeyi **işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.
6. İçinde **Essentials** Azure portalında yapısı Azure'a çoğaltılan VM'ler için görebilir.


### <a name="enable-replication-for-sqlvm"></a>SQLVM için çoğaltmayı etkinleştirme

Artık Contoso yöneticileri SQLVM makineyi çoğaltmak, aynı işlemi olarak yukarıdaki kullanarak başlayabilirsiniz.

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


## <a name="step-4-migrate-the-vms"></a>4. Adım: Vm'leri geçirme

Contoso yöneticileri bir hızlı yük devretme ve Vm'leri geçirme için tam bir yük devretme çalıştırın.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Her şeyin beklendiği gibi çalıştığından emin olmak için yük devretme testi yardımcı olur.

1. Yük devretme testi için son noktası sürede çalıştırdığı (**en son işlenen**).
2. Seçmeleri **yük devretmeye başlamadan önce makineyi Kapat**, böylece Site Recovery yük devretmeyi tetiklemeden önce kaynak sanal kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder.
3. Test yük devretme çalıştırır:

    - Bir önkoşul denetimi geçiş için gerekli koşulları tümünün karşılandığından emin olmak için çalıştırılır.
    - Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
    - Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.

3. Yük devretme bittikten sonra çoğaltma Azure VM Azure Portalı'nda görünür. Bunlar, VM doğru ağa uygun boyutta olduğundan ve çalışır durumda olduğunu denetleyin.
4. Test yük devretmesi doğruladıktan sonra bunlar devretmeyi temizlemek ve gözlemlerinizi kaydetmek ve.

### <a name="create-and-customize-a-recovery-plan"></a>Oluşturma ve bir kurtarma planı özelleştirme

 Contoso yöneticileri, yük devretme testi beklendiği gibi çalıştığını doğruladıktan sonra geçiş için bir kurtarma planı oluşturun.

- Bir kurtarma planı sırasını belirtir. yük devretme gerçekleşir ve nasıl Azure Vm'leri Azure'da çevrimiçi kapsama dahil edilecektir gösterir.
- İki katmanlı bir uygulama olduğundan, verileri ön uç (WEBVM) önce VM (SQLVM) başlatır. böylece, kurtarma planını özelleştirin.

1. İçinde **kurtarma planları (Site Recovery)** > **+ kurtarma planı**, bir plan oluşturur ve sanal makineleri ekleyin.

    ![Kurtarma planı](./media/contoso-migration-rehost-vm/recovery-plan.png)

2. Planı oluşturduktan sonra bunlar özelleştirin (**kurtarma planları** > **SmartHotelMigrationPlan** > **Özelleştir**).
2.  Bunlar WEBVM öğesinden kaldırmak **Grup 1: Başlangıç**. Bu, ilk başlatma eylemini SQLVM yalnızca etkiler sağlar.
3.  İçinde **+ grup** > **Ekle korumalı öğeler**, bunlar WEBVM grubu 2'ye ekleyin: Başlatın. İki farklı gruplardaki VM'lerin gerekir.


### <a name="migrate-the-vms"></a>Vm'leri geçirme


Şimdi Contoso yöneticileri, geçişi tamamlamak için tam bir yük devretme çalıştırın.

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

Tam geçişi ile SmartHotel360'ın uygulama katmanlarında artık Azure Vm'leri üzerinde çalışıyor.

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

- Erişimi denetlemek için takım VM'ler için ağ güvenlik grupları (Nsg'ler) gözden geçirir. Nsg'ler, yalnızca uygulamaya izin trafik, ulaşabileceği emin olmak için kullanılır.
- Takım, ayrıca Azure Disk şifreleme ve anahtar Kasası'nı kullanarak diskteki verilerin güvenliğini sağlama göz önünde bulundurun.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms) VM'ler için önerilen güvenlik uygulamaları hakkında.

## <a name="bcdr"></a>BCDR

Contoso, iş sürekliliği ve olağanüstü durum kurtarma (BCDR) için aşağıdaki işlemleri yapar:

- Verileri güvende tutun: Contoso Vm'leri Azure Backup hizmetini kullanarak verileri yedekler. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- Uygulamalarınızı çalışır halde tutun: Contoso uygulama Azure sanal makinelerini Site Recovery kullanarak ikincil bir bölgeye çoğaltır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart).



### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

1. Contoso mevcut Vm'leri için lisans sahiptir ve Azure hibrit avantajı özelliğinden yararlanır. Contoso mevcut Azure bu fiyatlandırmanın avantajlarından yararlanmak için sanal makinelerin dönüştürür.
2. Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet Yönetimi olanak sağlar. Bu Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olan bir çoklu bulut maliyet yönetimi çözümüdür. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında.

## <a name="conclusion"></a>Sonuç

Bu makalede, Contoso uygulama sanal makinelerini Site Recovery hizmetini kullanarak Azure Vm'lerine geçiş yaparak Azure SmartHotel360 uygulamada rehosted.


## <a name="next-steps"></a>Sonraki adımlar

İçinde [sonraki makalede](contoso-migration-rehost-vm-sql-ag.md) dizide nasıl Contoso SmartHotel360 uygulama ön uç bir Azure sanal makinesinde VM rehosts ve azure'da bir SQL Server AlwaysOn Kullanılabilirlik grubu veritabanı geçirir göstereceğiz.

