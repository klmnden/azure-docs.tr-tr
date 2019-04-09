---
title: Yeniden barındırma geçişi ve Azure Vm'leri için şirket içi Linux uygulama barındırma | Microsoft Docs
description: Nasıl Contoso yeniden barındırma bir şirket içi Linux uygulaması Azure Vm'lerine geçiş yaparak öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: raynew
ms.openlocfilehash: 200f9c5df0d4165341e38ca9d4dd85ad75c8403c
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59010371"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms"></a>Contoso geçişi: Azure sanal makineler için bir şirket içi Linux uygulaması barındırma

Contoso şirket içi Linux tabanlı bir hizmet Masası uygulama nasıl yeniden barındırma Bu makale (**osTicket**), Azure Iaas vm'lerine.

Bu belge Contoso adlı kurgusal şirketin şirket içi kaynaklarını Microsoft Azure bulutuna nasıl geçirdiğini belge makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapısını kurma ve farklı türde geçiş çalıştırmak nasıl çalışılacağını senaryoları kümesi içerir. Senaryoları, karmaşık hale gelmesi. Zaman içinde ek makaleleri ekleyeceğiz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[1. makale: Genel Bakış](contoso-migration-overview.md) | Makale serisi, Contoso'nun geçiş stratejisi ve dizisinde kullanılan örnek uygulamalar genel bakış. | Kullanılabilir
[2. makale: Azure altyapısı dağıtma](contoso-migration-infrastructure.md) | Contoso şirket içi altyapısını ve Azure altyapısını geçiş için hazırlar. Altyapıyı, serideki tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: Şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](contoso-migration-assessment.md)  | Contoso, Vmware'de çalıştırılan şirket içi SmartHotel360 uygulamasının bir değerlendirme çalışır. Contoso Azure geçişi hizmeti ve veri geçiş Yardımcısı'nı kullanarak uygulama SQL Server veritabanı kullanarak uygulama Vm'leri değerlendirir. | Kullanılabilir
[4. makale: Bir Azure VM ve SQL veritabanı yönetilen örneği bir uygulamada barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso, Azure'a lift-and-shift ile taşıma geçiş için kendi şirket içi SmartHotel360 uygulaması çalışır. Contoso geçirir uygulama ön uç VM kullanarak [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso geçirir uygulama veritabanını kullanarak bir Azure SQL veritabanı yönetilen örneği [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir   
[Makale 5: Bir uygulamayı Azure VM’lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso, SmartHotel360 uygulama sanal makinelerini Azure Site Recovery hizmetini kullanarak sanal makineleri geçirir. | Kullanılabilir
[Makale 6: Azure sanal makinelerinde ve SQL Server AlwaysOn Kullanılabilirlik grubuna bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel360 uygulamaya geçirir. Contoso, uygulama sanal makinelerini geçirmek için Site Recovery kullanır. Veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için kullanır. | Kullanılabilir 
Makale 7: Azure vm'lerinde Linux uygulaması barındırma | Azure Site Recovery kullanarak Azure vm'lerine Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini contoso tamamlar | Bu makalede
[Makale 8: Azure sanal makineler ve Azure MySQL üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso, Azure Site Recovery kullanarak Azure Vm'leri için Linux osTicket uygulaması geçirir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
[Makale 9: Azure Web Apps ve Azure SQL veritabanında bir uygulamayı yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Contoso SmartHotel360 uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanı için veritabanı geçiş Yardımcısı'nı kullanarak bir Azure SQL Server örneği geçirir | Kullanılabilir
[Makale 10: Azure Web Apps ve Azure MySQL üzerinde bir Linux uygulamayı yeniden düzenleme](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso, bir Azure web uygulamasına GitHub ile sürekli teslim için tümleşik Azure Traffic Manager'ı kullanarak birden fazla Azure bölgesini üzerinde kendi Linux osTicket uygulaması geçirir. Contoso uygulaması veritabanı örneği MySQL için Azure veritabanı geçirir. | Kullanılabilir 
[11. makale: Azure DevOps hizmetlerinde TFS yeniden düzenleyin](contoso-migration-tfs-vsts.md) | Contoso, Azure DevOps Hizmetleri azure'da, şirket içi Team Foundation Server dağıtımı geçirir. | Kullanılabilir
[Makale 12: Bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso, SmartHotel uygulamayı Azure'a geçirir. Ardından, Azure Service Fabric ve Azure SQL veritabanı ile veritabanı çalıştıran bir Windows kapsayıcısı olarak app web katmanından rearchitects. | Kullanılabilir
[Makale 13: Bir uygulamayı Azure’da yeniden oluşturma](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, Azure App Service, Azure Kubernetes Service (AKS), Azure işlevleri, Azure Bilişsel hizmetler ve Azure Cosmos DB dahil olmak üzere çeşitli kullanarak kendi SmartHotel uygulaması oluşturur. | Kullanılabilir
[Makale 14: Azure'a geçiş ölçeklendirin](contoso-migration-scale.md) | Geçiş birleşimleri denedikten sonra Contoso Azure tam geçişi ölçeklendirilebilecek şekilde hazırlar. | Kullanılabilir



Bu makalede, iki katmanlı Contoso geçiş yapacağınız **osTicket** Azure'da Linux Apache MySQL PHP (LAMP) üzerinde çalışan uygulama. Uygulama sanal makinelerini, Azure Site Recovery hizmetini kullanarak geçirilecektir. Bu açık kaynaklı uygulamayı kullanmak istiyorsanız, buradan indirebilirsiniz [GitHub](https://github.com/osTicket/osTicket).

## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso büyüyor ve sonuç olarak şirket içi sistemler ve altyapı Basıncı yoktur.
- **Risk sınırlamak**: Hizmet Masası uygulaması Contoso işletmeler için önemlidir. Contoso Azure'a sıfır riskle taşımak istiyor.
- **Genişletme**:  Contoso uygulaması hemen şimdi değiştirmek istemiyorsanız. Yalnızca uygulama tutarlı olduğundan emin olmak istiyor.


## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini en iyi geçiş yöntemini belirlemek için bu geçiş için aşağı sabitlenmiş:

- Bugün, şirket içi VMWare ortamlarında olduğu gibi geçişten sonra uygulamanızı Azure'a aynı performans özelliklerine sahip olmalıdır.  Uygulama, şirket içi olarak bulutta kritik olarak kalır. 
- Contoso, bu uygulamada yatırım yapmaya istememektedir.  İş için önemlidir, ancak mevcut haliyle Contoso yalnızca buluta güvenli bir şekilde taşımak ister.
- Contoso, bu uygulama için ops modeli değiştirmek istememektedir. Bulutta uygulama, artık aynı şekilde etkileşime geçmek istiyor.
- Contoso işlevini değiştirmek istememektedir. Yalnızca uygulama konumu değişir.
- Windows uygulama geçişleri birkaç tamamlandıktan sonra Azure'da bir Linux tabanlı altyapı kullanmayı öğrenmek Contoso istiyor.


## <a name="solution-design"></a>Çözüm tasarımı

Hedefleri ve gereksinimleri sabitleme sonra Contoso tasarlar ve bir dağıtım çözümü gözden geçirin ve Contoso geçiş için kullanacağınız Azure hizmetlerini de dahil olmak üzere geçiş işlemi tanımlar.

### <a name="current-app"></a>Geçerli uygulama

- OSTicket uygulama, iki VM arasında katmanlı (**OSTICKETWEB** ve **OSTICKETMYSQL**).
- Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com** (sürüm 6.5).
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (**contoso-datacenter**), bir şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**)

### <a name="proposed-architecture"></a>Önerilen mimarisi

- Uygulamayı bir üretim iş yükü olduğundan, Azure sanal makineleri üretim kaynak grubunda yer alacağı **ContosoRG**.
- VM'ler birincil bölge (Doğu ABD 2) geçişi ve üretim ağı (VNET-PROD-EUS2) yerleştirilir:
    - VM web ön uç (FE-PROD-EUS2) alt ağda yer alacaktır.
    - ' % S'veritabanı VM (PROD-DB-EUS2) veritabanı alt ağda yer alacaktır.
- Geçiş tamamlandıktan sonra şirket içi Vm'leri Contoso veri merkezinde kullanımdan.

![Senaryo mimarisi](./media/contoso-migration-rehost-linux-vm/architecture.png) 

### <a name="solution-review"></a>Çözümü gözden geçirme

Contoso, Artıları ve eksileri listesini birbirine koyarak önerilen tasarım değerlendirir.

**Önemli noktalar** | **Ayrıntılar**
--- | ---
**Uzmanları** | Uygulama Vm'leri Azure'a geçiş basit hale değişikliğe gerek kalmadan taşınır.<br/><br/> Her iki uygulama Vm'leri için contoso lift-and-shift ile taşıma kullandığından, özel bir yapılandırma veya geçiş araçları uygulama veritabanı için gereklidir.<br/><br/> Contoso, uygulamayı azure'da sanal makineler üzerinde tam denetim korur. </br>/br > Uygulama sanal makinelerini, Ubuntu 16.04-desteklenen bir Linux dağıtımı olan TLS çalışıyor. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).
**Simgeler** | Web ve veri katmanı uygulaması, tek bir yük devretme noktası kalır. <br/><br/> Contoso, MySQL için Azure App Service ve Azure veritabanı gibi yönetilen bir hizmet taşımak yerine Azure Vm'leri olarak uygulama destekleyen devam etmek gerekir.<br/><br/> Contoso şeyler lift-and-shift ile taşıma VM geçişi ile basit tutarak, bunlar tarafından sağlanan özelliklerden tam anlamıyla yönlendiriyoruz değil, farkında [MySQL için Azure veritabanı](https://docs.microsoft.com/azure/mysql/overview) (yerleşik yüksek kullanılabilirlik, öngörülebilir performans Basit ölçeklendirme, otomatik yedeklemeler ve yerleşik güvenlik).

### <a name="migration-process"></a>Geçiş işlemi

Contoso gibi geçirir:

1. İlk adım, Contoso Azure'ı ayarlar ve şirket içi Site Recovery dağıtmak için gerekli altyapı.
2. Azure ve şirket içi bileşenleri hazırlanıyor sonra Contoso kurar ve VM'ler için çoğaltmayı etkinleştirir.
3. Çoğaltma çalışmaya başladıktan sonra bunları Azure'a devretmek tarafından Contoso sanal makineleri geçirir.

![Geçiş işlemi](./media/contoso-migration-rehost-linux-vm/migration-process.png)

### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Hizmet düzenler ve geçiş ve olağanüstü durum kurtarma için Azure Vm'leri yönetir ve şirket içi Vm'leri ve fiziksel sunucuları.  | Azure'a çoğaltma sırasında Azure depolama ücretleri uygulanır.  Azure Vm'leri oluşturulur ve yük devretme işlemi gerçekleştiğinde, ücret. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/) ücretleri ve fiyatlandırma hakkında.

 
## <a name="prerequisites"></a>Önkoşullar

Bu senaryoda Contoso gerekenler aşağıda verilmiştir.

**Gereksinimler** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Contoso abonelikleri Bu seriyi erken bir makaledeki oluşturdunuz. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.<br/><br/> Daha ayrıntılı izinler gerekirse gözden [bu makalede](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure altyapı** | Contoso bölümünde anlatıldığı gibi kendi Azure altyapısını ayarlamak [geçiş için Azure altyapı](contoso-migration-infrastructure.md).<br/><br/> Özel hakkında daha fazla bilgi [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) Site Recovery için gereksinimleri.
**Şirket içi sunucular** | Şirket içi vCenter server 5.5, 6.0 veya 6.5 sürümünü çalıştırmalıdır<br/><br/> 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir ESXi ana bilgisayarı<br/><br/> Bir veya daha fazla VMware ESXi ana bilgisayarında çalışan VM'ler.
**Şirket içi Vm'leri** | [Linux makineleri gözden](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) Site Recovery ile geçiş için desteklenir.<br/><br/> Doğrulama desteklenen [Linux dosya ve depolama sistemlerini](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> Vm'leri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Senaryo adımları

Contoso geçişi nasıl tamamlanacağını garanti aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure Site Recovery için hazırlama**: Contoso çoğaltılan verileri tutmak için bir Azure depolama hesabı oluşturur ve bir kurtarma Hizmetleri kasası oluşturur.
> * **2. adım: Site Recovery için şirket içi Vmware'leri hazırlama**: Contoso, VM bulma ve aracı yükleme için kullanılacak hesapları hazırlar ve yük devretme sonrasında Azure Vm'lerine bağlanmak hazırlar.
> * **3. adım: Vm'lerini çoğaltma**: Contoso kaynak ve hedef geçiş ortamı ayarlar, bir çoğaltma ilkesi oluşturur ve VM'ler için Azure storage çoğaltmaya başlar.
> * **4. adım: Site Recovery ile Vm'leri geçirme**: Contoso, her şeyin çalıştığından ve ardından sanal makineleri Azure'a geçirmek için bir tam yük devretme çalıştırır emin olmak için bir yük devretme testi çalıştırır.


## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>1. Adım: Azure Site Recovery hizmeti için hazırlama

Contoso, Site Recovery için birkaç Azure bileşenlerini gerekir:

- Çoğaltılan verileri tutmak için yeni bir Azure depolama hesabı. 
- Azure kurtarma Hizmetleri kasasında.
- Bir Vnet'te yük devretti kaynakları bulunur. Contoso sırasında sanal ağ oluşturmuş [Azure altyapı dağıtımı](contoso-migration-infrastructure.md), bunlar yalnızca bir depolama hesabı ve kasası oluşturmanız gerekir.

1. Contoso yöneticileri, Doğu ABD 2 bölgesinde (contosovmsacc20180528) Azure depolama hesabı oluşturun.

    - Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
    - Bunlar, standart depolama ve LRS çoğaltma ile genel amaçlı bir hesabı kullanın.

      ![Site Kurtarma Depolama](./media/contoso-migration-rehost-linux-vm/asr-storage.png)

2. Ağ ve depolama hesabı ile yerinde, bunlar (ContosoMigrationVault) bir kasa oluşturun ve içine yerleştirin **ContosoFailoverRG** birincil Doğu ABD 2 bölgesinde bir kaynak grubu.

    ![Kurtarma Hizmetleri kasası](./media/contoso-migration-rehost-linux-vm/asr-vault.png)


**Daha fazla yardıma mı ihtiyacınız var?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Site Recovery için Azure'ı ayarlama.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>2. Adım: Site Recovery için şirket içi Vmware'leri hazırlama

Contoso admins gibi şirket içi VMware altyapınızı hazırlayın:

- Bunlar, VM bulmayı otomatikleştirmek için vCenter sunucusunda veya vSphere ESXi ana bilgisayarındaki, bir hesap oluşturun.
- Bunlar, çoğaltmak istediğiniz VMware Vm'lerinde Mobility hizmetini otomatik olarak yüklenmesini sağlayan bir hesap oluşturun.
- Bunlar, geçiş sonrasında oluşturulduğunda Azure Vm'lerinin bağlanabilmesi için şirket içi Vm'leri hazırlayın.


### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- VM'leri otomatik olarak bulma. En az bir salt okunur hesap gereklidir.
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme. Oluşturma ve diskleri kaldırma ve Vm'leri kapatarak gibi işlemleri de çalıştırabilirsiniz bir hesabınız olmalıdır.

Contoso admins hesabı aşağıdaki gibi ayarlayın:

1. Bunlar, bir rolü vCenter düzeyinde oluşturun.
2. Bunlar, bu rol gerekli izinleri atayın.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Geçirilecek Linux Vm'lerinde Mobility hizmetinin yüklenmesi gerekir.

- VM'ler için çoğaltma etkinleştirildiğinde site Recovery bu bileşeni otomatik gönderim yüklemesi yapabilirsiniz.
- Otomatik gönderim yüklemesi için Site Recovery'nin sanal makinelere erişmek için kullanacağı bir hesap hazırlamanız gerekir.
- Hesapları ayrıntıları çoğaltma kurulumu sırasında giriş. 
- Hesap etki alanı veya yerel hesap olarak Vm'lerde yüklemek için gerekli izinlere sahip olabilir.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Azure'a yük devretme sonrasında Contoso çoğaltılmış Azure vm'lere bağlanmak atabilmek istiyor. Bunu yapmak için birkaç Contoso yöneticileri gerçekleştirmeniz gereken şey vardır:

- Azure sanal makineleri internet üzerinden erişmek için geçiş işleminden önce şirket içi Linux VM üzerinde SSH bunlar etkinleştirin.  Ubuntu için bu aşağıdaki komutu kullanarak tamamlayabilirsiniz: **Sudo yüklemeyi apt-get ssh -y**.
- Bunlar geçiş (yük devretme) çalıştırdıktan sonra kontrol edebilirsiniz **önyükleme tanılaması** VM'nin bir ekran görüntülemek için.
- Bu işe yaramazsa, bunlar VM çalıştıran ve bunları gözden geçirin, denetlenecek gerekir [sorun giderme ipuçları](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Daha fazla yardıma mı ihtiyacınız var?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) oluşturma ve Otomatik bulma için bir rol atama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) Mobility hizmetinin göndererek yüklenmesine ilişkin için bir hesabı oluşturuluyor.


## <a name="step-3-replicate-the-on-premises-vms"></a>3. Adım: Şirket içi sanal makinelerini çoğaltma

Azure'a web VM geçişi yapmadan önce Contoso yöneticileri ayarlama ve çoğaltmayı etkinleştirin.

### <a name="set-a-protection-goal"></a>Koruma hedefi ayarlama

1. Kasada kasa adını (ContosoVMVault) altında çoğaltma hedefi ayarlayın (**Başlarken** > **Site Recovery** > **altyapıyıhazırlama**.
2. Bunlar, makinelerini şirket içi olduğunu, VMware Vm'leri ve Azure'a çoğaltmak istediğiniz olduğunuzu belirtin.
    ![Çoğaltma hedefi](./media/contoso-migration-rehost-linux-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

Devam etmek için bunlar seçerek dağıtım planlamasını tamamladınız onaylayın **Evet, yaptım**. Contoso tek bir VM Bu senaryoda yalnızca geçiş olan ve dağıtım planlama gerekmez.

### <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Contoso yöneticileri artık kaynak ortamı yapılandırmanız gerekir. Bunu yapmak için bir OVF şablonu indirin ve Site Recovery yapılandırma sunucusunu yüksek oranda kullanılabilir olarak dağıtmak için kullanın, şirket içi VMware sanal makine. Yapılandırma sunucusunu çalışır duruma geldikten sonra bunlar bu kasaya kaydedin.

Yapılandırma sunucusu, çeşitli bileşenler çalıştırır:

- Şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir yapılandırma sunucusu bileşeni.
- İşlem sunucusu çoğaltma ağ geçidi davranır. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir.
- İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.

Contoso yöneticileri bu adımları aşağıdaki gibi gerçekleştirin:

1. Bunlar OVF şablonu indirin **altyapıyı hazırlama** > **kaynak** > **yapılandırma sunucusu**.
    
    ![OVF indirin](./media/contoso-migration-rehost-linux-vm/add-cs.png)

2. Bunlar, şablonu VM oluşturmak için Vmware'e aktarın ve VM'yi dağıtın.

    ![OVF şablonu](./media/contoso-migration-rehost-linux-vm/vcenter-wizard.png)

3. Sanal makinede ilk kez açtığınızda, oluşturan bir Windows Server 2016 yükleme deneyimi önyüklenir. Bunlar lisans sözleşmesini kabul edin ve bir yönetici parolasını girin.
4. Yükleme tamamlandıktan sonra yönetici olarak VM'de oturum açmak. İlk oturum açma işleminde, varsayılan olarak Azure Site kurtarma Yapılandırması aracını çalıştırır.
5. Aracı'nda, yapılandırma sunucusunu kasaya kaydetmek için kullanılacak bir ad belirtin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra bunlar Azure aboneliği için oturum açın. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.

    ![Yapılandırma sunucusunu kaydetmek](./media/contoso-migration-rehost-linux-vm/config-server-register2.png)

7. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır.
8. Bunların makinede yeniden oturum açın ve yapılandırma sunucusu yönetim Sihirbazı otomatik olarak başlar.
9. Sihirbazda, çoğaltma trafiğini almak için NIC'yi seçin. Bu ayar yapılandırıldıktan sonra değiştirilemez.
10. Abonelik, kaynak grubu ve yapılandırma sunucusunu kaydetmek istediğiniz kasaya seçerler.

    ![Kasa](./media/contoso-migration-rehost-linux-vm/cswiz1.png) 

11. Bunlar sonra da indirin ve MySQL Server ve VMWare powerclı'yı yükleyin. 
12. Doğrulama sonrasında, bunlar vCenter sunucusunda veya vSphere konağının FQDN'sini veya IP adresini belirtin. Bunlar, varsayılan bağlantı noktasını değiştirmeyin ve vCenter sunucusu için bir kolay ad belirtin.
13. Bunlar, bunlar otomatik bulma için oluşturduğunuz hesabı ve otomatik olarak Mobility hizmetini yükleme için kullanılması gereken kimlik bilgilerini belirtin.

    ![vCenter](./media/contoso-migration-rehost-linux-vm/cswiz2.png)

14. Azure portalında kayıt tamamlandıktan sonra bunlar üzerinde yapılandırma sunucusunun ve VMware sunucusunun listelenip listelenmediğini denetleyin **kaynak** kasadaki sayfası. Bulma, 15 dakika veya daha fazla sürebilir. 
15. Site Recovery için VMware sunucularına bağlanır ve Vm'leri bulur.

### <a name="set-up-the-target"></a>Hedefi ayarlama

Artık Contoso yöneticileri hedef çoğaltma ayarlarını yapılandırın.

1. İçinde **altyapıyı hazırlama** > **hedef**, bunlar hedef ayarları seçin.
2. Site Recovery, bir Azure depolama hesabını ve belirtilen hedef ağda olup olmadığını denetler.

### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Kaynak ve hedef ayarlandıktan sonra bir çoğaltma ilkesi oluşturmak hazır.

1. İçinde **altyapıyı hazırlama** > **çoğaltma ayarları** > **Çoğaltma İlkesi** >  **oluşturma ve İlişkilendirme**, bunlar bir ilke oluşturmak **ContosoMigrationPolicy**.
2. Bunlar, varsayılan ayarları kullanın:
    - **RPO eşiği**: Varsayılan 60 dakika. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
    - **Kurtarma noktası bekletme**. Varsayılan 24 saat. Bu değer, ne kadar süreyle her kurtarma noktası için bekletme süresinin olacağını belirtir. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**. Varsayılan bir saat değeri. Bu değer, uygulamayla tutarlı anlık görüntülerin oluşturulma sıklığı belirtir.
 
        ![Çoğaltma ilkesi oluşturma](./media/contoso-migration-rehost-linux-vm/replication-policy.png)

5. İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. 

    ![Çoğaltma İlkesi ilişkilendirme](./media/contoso-migration-rehost-linux-vm/replication-policy2.png)

**Daha fazla yardıma mı ihtiyacınız var?**

- Bu adımlar tam bir kılavuza edinebilirsiniz [şirket içi VMware Vm'leri için olağanüstü durum kurtarmayı ayarlayın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Yardımcı olmak ayrıntılı yönergeler kullanılabilir [kaynak ortamını ayarlama](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [yapılandırma sunucusunu dağıtma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), ve [çoğaltma ayarlarını yapılandırma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) Linux Azure Konuk Aracısı hakkında.

**Daha fazla yardıma mı ihtiyacınız var?**

- Bu adımlar tam bir kılavuza edinebilirsiniz [şirket içi VMware Vm'leri için olağanüstü durum kurtarmayı ayarlayın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Yardımcı olmak ayrıntılı yönergeler kullanılabilir [kaynak ortamını ayarlama](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [yapılandırma sunucusunu dağıtma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), ve [çoğaltma ayarlarını yapılandırma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) Linux Azure Konuk Aracısı hakkında.



### <a name="enable-replication-for-osticketweb"></a>OSTICKETWEB için çoğaltmayı etkinleştirme

Contoso yöneticileri çoğaltmaya başlamak şimdi **OSTICKETWEB** VM.

1. İçinde **uygulama çoğaltma** > **kaynak** > **+ Çoğalt** bunlar kaynak ayarlarını seçin.
2. Bunlar, sanal makineler etkinleştirmek istiyorsanız, vCenter sunucusu ve yapılandırma sunucusu da dahil olmak üzere kaynak ayarlarını seçin seçerler.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm/enable-replication-source.png)

3. Bunlar, kaynak grubunu ve sanal ağ, Azure VM yük devretme işleminden sonra yerleştirilir ve çoğaltılan verilerin depolanacağı depolama hesabı dahil olmak üzere hedef ayarlarını belirtin.

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm/enable-replication2.png)

3. Seçmeleri **OSTICKETWEB** VM çoğaltma için. 

   - Bu aşamada seçmeleri **OSTICKETWEB** yalnızca VNet ve alt ağ hem de seçilmelidir ve Vm'leri aynı alt ağda değil.
   - VM için çoğaltma etkinleştirildiğinde site Recovery Mobility hizmetinin otomatik olarak yükler.

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm/enable-replication3.png)

4. VM Özellikleri'nde, işlem sunucusu tarafından otomatik olarak Mobility hizmetini makineye yüklemek için kullandığınız hesabı seçin.

     ![Mobility hizmeti](./media/contoso-migration-rehost-linux-vm/linux-mobility.png)

5. içinde **çoğaltma ayarları** > **çoğaltma ayarlarını yapılandırma**, bunların doğru çoğaltma ilkesinin uygulanan ve seçim olup olmadığını denetleyin **çoğaltmayı etkinleştirme**.
6. Bunlar çoğaltma ilerlemeyi **işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.



### <a name="enable-replication-for-osticketmysql"></a>OSTICKETMYSQL için çoğaltmayı etkinleştirme

Contoso yöneticileri çoğaltmaya başlamak şimdi **OSTICKETMYSQL**.

1. İçinde **uygulama çoğaltma** > **kaynak** > **+ Çoğalt** , kaynak ve hedef ayarları seçin.
2. Seçmeleri **OSTICKETMYSQL** Mobility hizmeti yüklemesi için kullanılacak sanal makine için çoğaltma ve hesabı seçin.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-linux-vm/mysql-enable.png)

3. Bunlar OSTICKETWEB için kullanılan aynı çoğaltma ilkesine uygulamak ve çoğaltmayı etkinleştirin.  

**Daha fazla yardıma mı ihtiyacınız var?**

Bu adımlar tam bir kılavuza edinebilirsiniz [çoğaltmayı etkinleştir](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>4. Adım: Vm'leri geçirme 

Contoso admins hızlı çalıştırın, yük devretme testi ve sonra Vm'leri geçirme.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Yardımcı olur, yük devretme testi çalıştıran her şeyin geçiş işleminden önce beklendiği gibi çalıştığından emin olun. 

1. Yük devretme testi için son noktası sürede çalıştırdığı (**en son işlenen**).
2. Seçmeleri **yük devretmeye başlamadan önce makineyi Kapat**, böylece Site Recovery yük devretmeyi tetiklemeden önce kaynak sanal kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. 
3. Test yük devretme çalıştırır: 
    - Bir önkoşul denetimi geçiş için gerekli koşulları tümünün karşılandığından emin olmak için çalıştırılır.
    - Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktası seçtiyseniz, verilerden bir kurtarma noktası oluşturulur.
    - Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.
3. Yük devretme bittikten sonra çoğaltma Azure VM Azure Portalı'nda görünür. Bunlar, VM doğru ağa bağlandığından uygun boyutta olduğundan ve çalıştığından emin denetleyin. 
4. Doğruladıktan sonra bunlar devretmeyi temizlemek ve gözlemlerinizi kaydetmek ve.

### <a name="create-and-customize-a-recovery-plan"></a>Oluşturma ve bir kurtarma planı özelleştirme

 Contoso yöneticileri, yük devretme testi beklendiği gibi çalıştığını doğruladıktan sonra geçiş için bir kurtarma planı oluşturun. 

- Bir kurtarma planı sırasını belirtir de hangi yük devretme gerçekleştikten, nasıl Azure Vm'leri Azure'da kapsama dahil edilecektir.
- VM (SQLVM) verileri ön uç (WEBVM) önce başlar. böylece, iki katmanlı bir uygulama geçirmek istediğiniz olduğundan, bunlar kurtarma planı özelleştireceksiniz.


1. İçinde **kurtarma planları (Site Recovery)** > **+ kurtarma planı**, bir plan oluşturur ve sanal makineleri ekleyin.

    ![Kurtarma planı](./media/contoso-migration-rehost-linux-vm/recovery-plan.png)

2. Planı oluşturduktan sonra bunlar için özelleştirmeyi seçin (**kurtarma planları** > **OsTicketMigrationPlan** > **Özelleştir**.
3.  Bunlar kaldırmak **OSTICKETWEB** gelen **Grup 1: Başlangıç**.  Bu ilk başlatma eylemini etkilediği sağlar **OSTICKETMYSQL** yalnızca.

    ![Kurtarma grubu](./media/contoso-migration-rehost-linux-vm/recovery-group1.png)

4.  İçinde **+ grup** > **Ekle korumalı öğeler**, ekledikleri **OSTICKETWEB** için **2. Grup: Başlangıç**.  Bu iki farklı gruplardaki gerekir.

    ![Kurtarma grubu](./media/contoso-migration-rehost-linux-vm/recovery-group2.png)


### <a name="migrate-the-vms"></a>Vm'leri geçirme


Contoso yöneticileri artık kurtarma planında sanal makineleri geçirmek için bir yük devretme çalıştırmak hazır olursunuz.

1. Bunlar planı seçin > **yük devretme**.
2.  En son kurtarma noktasına yük devretmek ve Site Recovery yük devretmeyi tetiklemeden önce şirket içi sanal makineyi denemesi gerektiğini belirtmek için seçin. Yöneticiler yük devretme işleminin ilerleyişini izleyebilirsiniz **işleri** sayfası.

    ![Yük devretme](./media/contoso-migration-rehost-vm/failover1.png)

3. Yük devretme sırasında vCenter Server ESXi ana bilgisayarında çalışan iki VM durdurmak için komutlar verir.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm/vcenter-failover.png)

3. Yük devretmeden sonra bunlar Azure VM Azure Portalı'nda beklendiği gibi göründüğünü doğrulayın.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm/failover2.png)  

3. Azure'da VM doğruladıktan sonra bunlar her VM için geçiş işlemini tamamlamak için geçişi tamamlayın. Bu VM için çoğaltma durdurulur ve sanal makine için Site Recovery Faturalaması durdurulur.

    ![Yük devretme](./media/contoso-migration-rehost-linux-vm/failover3.png)


### <a name="connect-the-vm-to-the-database"></a>VM veritabanına bağlanma

Geçiş işleminin son adımı, Contoso yöneticileri üzerinde çalışan uygulama veritabanına işaret edecek şekilde uygulamanın bağlantı dizesini güncelleştirme **OSTICKETMYSQL** VM. 

1. Bunlar bir SSH bağlantısı **OSTICKETWEB** Putty kullanarak sanal makine veya başka bir SSH istemcisi. Özel IP adresini kullanarak bağlanmak için özel bir vm'dir.

    ![Veritabanı'na bağlanma](./media/contoso-migration-rehost-linux-vm/db-connect.png)  

    ![Veritabanı'na bağlanma](./media/contoso-migration-rehost-linux-vm/db-connect2.png)  

2. Emin olmak ihtiyaç duydukları **OSTICKETWEB** VM ile iletişim kurabilir **OSTICKETMYSQL** VM. Şu anda şirket içi IP adresiyle 172.16.0.43 sabit kodlanmış bir yapılandırmadır.

    **Güncelleştirmeden önce**
    
    ![IP güncelleştir](./media/contoso-migration-rehost-linux-vm/update-ip1.png)  

    **Güncelleştirme sonrasında**
    
    ![IP güncelleştir](./media/contoso-migration-rehost-linux-vm/update-ip2.png) 
    
3. Hizmetle yeniden başlatıldığında **systemctl yeniden apache2**.

    ![Yeniden Başlatma](./media/contoso-migration-rehost-linux-vm/restart.png) 

4. Son olarak, bunlar için DNS kayıtlarını güncelleştirmek **OSTICKETWEB** ve **OSTICKETMYSQL**, Contoso etki alanı denetleyicilerinin birindeki.

    ![DNS'yi güncelleştir](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

    ![DNS'yi güncelleştir](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

**Daha fazla yardıma mı ihtiyacınız var?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) yük devretme testi çalıştırma. 
- [Bilgi](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) bir kurtarma planı oluşturma.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Azure'a devretmek.

## <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Tam geçişi ile osTicket uygulama katmanlarında artık Azure Vm'leri üzerinde çalışıyor.

Şimdi, Contoso gibi temizlemek gerekir: 

- Şirket içi sanal makineleri, vCenter stok kaldırın.
- Şirket içi Vm'leri yerel yedekleme işlerden kaldırın.
- Yeni bir konum ve IP adresleri OSTICKETWEB ve OSTICKETMYSQL göstermek için kendi iç belgeleri güncelleştirin.
- Sanal makineleri ile etkileşim kuran tüm kaynakları gözden geçirin ve herhangi bir ilgili ayarları veya belgeleri yeni yapılandırmayı yansıtacak şekilde güncelleştirin.
- Azure geçişi hizmeti, contoso sanal makinelerin geçişi için değerlendirmek için bağımlılık eşlemesi ile kullanılır. Yöneticiler, Microsoft Monitoring Agent ve bu amaçla VM'den yükledikleri bağımlılık aracısını kaldırmanız gerekir.

## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Contoso şu anda çalışıyor ve uygulama ile tam olarak çalışır hale getirme ve yeni altyapılarını güvenli gerekir.

### <a name="security"></a>Güvenlik

Contoso güvenlik ekibi OSTICKETWEB ve OSTICKETMYSQLVMs güvenlik sorunları belirlemek için gözden geçirin.

- Takım erişimi denetlemek Vm'leri için ağ güvenlik grupları (Nsg'ler) inceler. Nsg'ler, yalnızca uygulamaya izin trafik geçirebilirsiniz emin olmak için kullanılır.
- Takım, Disk şifreleme ve Azure anahtar Kasası'nı kullanarak VM disk üzerindeki verilerin güvenliğini sağlama de dikkate alır.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms) VM'ler için önerilen güvenlik uygulamaları hakkında.

### <a name="bcdr"></a>BCDR

Contoso, iş sürekliliği ve olağanüstü durum kurtarma için aşağıdaki işlemleri yapar:

- **Verileri güvende tutun**: Contoso Vm'leri Azure Backup hizmetini kullanarak verileri yedekler. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Uygulamalarınızı çalışır halde tutun**: Contoso uygulama Azure sanal makinelerini Site Recovery kullanarak ikincil bir bölgeye çoğaltır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart).

### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

- Kaynakları dağıttıktan sonra Contoso Azure etiketleri sırasında tanımlanan atar [Azure altyapı dağıtımı](contoso-migration-infrastructure.md#set-up-tagging).
- Contoso, Ubuntu sunucuları ile herhangi bir lisans sorun yok.
- Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet Yönetimi olanak sağlar. Bu Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olacak bir çoklu bulut maliyet yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında. 


## <a name="next-steps"></a>Sonraki adımlar

Contoso nasıl geçişi gösterdi bu makalede Azure Site Recovery kullanarak Azure Iaas vm'lerine iki Linux vm'lerinde şirket içi hizmet Masası uygulama katmanlı.

Serinin sonraki makalede Contoso aynı hizmet Masası uygulamasını Azure'a nasıl geçirdiğini göstereceğiz. Şu contoso uygulaması için ön uç VM geçirmek için Site Recovery kullanır ve geçirir yedekleme uygulamasını kullanarak veritabanı ve MySQL workbench aracını kullanarak MySQL için Azure veritabanı'na geri. [Başlama](contoso-migration-rehost-linux-vm-mysql.md).
