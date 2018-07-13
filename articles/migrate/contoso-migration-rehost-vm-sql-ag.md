---
title: Geçiş yaparak, Contoso uygulama barındırma, Azure VM ve SQL Server AlwaysOn Kullanılabilirlik grubuna | Microsoft Docs
description: Nasıl Contoso yeniden barındırma şirket içi uygulama geçiş yaparak öğrenmek için Azure VM ve SQL Server AlwaysOn Kullanılabilirlik grubu
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: raynew
ms.openlocfilehash: 33ab56003f2d9428816ea0f32cfd6381ea857df3
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611297"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms-and-sql-server-alwayson-availability-group"></a>Contoso geçiş: şirket içi uygulama Azure VM ve SQL Server AlwaysOn Kullanılabilirlik grubuna yeniden barındırma

Bu makalede, Contoso Azure SmartHotel uygulamasının nasıl yeniden barındırma gösterilmektedir. Bunlar, uygulama ön uç sanal makine bir Azure VM ve bir Azure SQL Server SQL Server AlwaysOn Kullanılabilirlik grupları ile Windows Server Yük devretme kümesinde çalışan VM, uygulama veritabanına geçirin.

Bu belge, Contoso adlı kurgusal şirketin şirket içi kaynaklarından bazılarını Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapı kurulumu, geçiş için şirket içi kaynaklarınızı değerlendirerek ve geçişleri farklı türlerde çalıştıran gösteren senaryolar içerir. Senaryoları, karmaşık hale gelmesi ve zaman içinde ek makaleleri ekleyeceğiz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale dizisini ve kullandığımız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[2. makale: bir Azure altyapısını dağıtma](contoso-migration-infrastructure.md) | Açıklayan nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar. Altyapıyı, tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: şirket içi kaynaklara değerlendirin](contoso-migration-assessment.md)  | Contoso değerlendirme Wmware'de çalışan bir şirket içi iki katmanlı SmartHotel uygulamanın nasıl çalıştığını gösterir. Contoso uygulaması Vm'lerle değerlendirir [Azure geçişi](migrate-overview.md) hizmet ve uygulama SQL Server veritabanıyla [veritabanı geçiş Yardımcısı'nı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[4. makale: Azure sanal makineler ve yönetilen bir SQL örneği için bir uygulama barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso lift-and-shift ile taşıma geçiş için Azure SmartHotel uygulama için nasıl çalıştığını gösterir. Contoso VM ön uç uygulamasını kullanarak geçirir [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)ve SQL yönetilen örneği, uygulama veritabanını kullanarak [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir
[Makale 5: Azure sanal makinelerinde bir uygulamayı barındırma](contoso-migration-rehost-vm.md) | Contoso geçirme SmartHotel uygulama yalnızca Site RECOVERY'yi kullanarak VM'lerin nasıl gösterir.
Makale 6: Azure VM ve SQL Server Always On kullanılabilirlik grubu (Bu makale) bir uygulamaya yeniden barındırma | Contoso SmartHotel uygulamayı nasıl geçirdiğini gösterir. Contoso, uygulama sanal makinelerini ve veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için geçirmek için Site Recovery kullanır. | Kullanılabilir
[Makale 7: Azure sanal makinelerinde Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Nasıl Contoso Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini Azure Vm'leri için Site RECOVERY'yi kullanarak yaptığını gösterir. | Planlandı
[Makale 8: Azure sanal makineler ve Azure MySQL sunucusu için bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso Linux osTicket uygulaması için Azure Site RECOVERY'yi kullanarak VM'lerin nasıl geçirdiğini gösterir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir



Bu makalede, iki katmanlı Windows Contoso geçirin. Azure'a VMware Vm'lerinde çalışan NET SmartHotel uygulaması. Bu uygulamayı kullanmak istiyorsanız, açık kaynak sağlanır ve buradan indirebileceğiniz [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso giderek ve sonuç olarak, şirket içi sistemler ve altyapı Basıncı yoktur.
- **Verimliliği artırmak**: Contoso gereken gereksiz yordamları kaldırıp geliştiriciler ve kullanıcılar için süreçlerini kolaylaştırabilirsiniz.  Hızlı olmasını iş ihtiyaçlarınıza BT ve değil atık zaman ve para, bu nedenle müşteri gereksinimlerine daha hızlı teslim.
- **Çevikliği artırın**: Contoso BT işletme ihtiyaçlarını daha hızlı olması gerekir. Market'te genel ekonomi de başarılı etkinleştirmek için değişiklikleri daha hızlı tepki vermek mümkün olması gerekir.  Ulaşın veya bir iş engelleyici hale gerekmez.
- **Ölçek**: başarıyla büyüdükçe, Contoso BT aynı yükselmeye mümkün sistemlerini sağlamanız gerekir.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini bu geçiş için aşağı sabitlenmiş. Bu hedefleri, en iyi geçiş yöntemini belirlemek için kullanılıyordu:

- Bugün, VMWare içinde çalıştığı gibi geçişten sonra uygulamanızı Azure'a aynı performans özellikleri olmalıdır.  Uygulama, şirket içi olarak bulutta kritik olarak kalır.
- Contoso, bu uygulamada yatırım yapmaya istememektedir.  İş için önemlidir, ancak mevcut haliyle yalnızca buluta güvenli bir şekilde taşımak istedikleri.
- Uygulama için şirket içi veritabanı kullanılabilirlik sorunları oluşturdu. Azure'a yük devretme özellikleriyle, yüksek kullanılabilirlik kümesi olarak dağıtılmış görmek istiyorsunuz.
- Contoso, geçerli SQL Server 2008 R2 platformdan, SQL Server 2017'ye yükseltmek istemektedir.
- Contoso, bu uygulama için bir Azure SQL veritabanı kullanmak istemiyorsa ve alternatifleri için arıyor.

## <a name="proposed-architecture"></a>Önerilen mimarisi

Bu senaryoda:

- Uygulama, iki VM arasında (WEBVM ve SQLVM) katmanlı.
- Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com** (sürüm 6.5)
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).
- Geçiş tamamlandıktan sonra şirket içi Vm'leri Contoso veri merkezinde kullanımdan.

![Senaryo mimarisi](media/contoso-migration-rehost-vm-sql-ag/architecture.png) 

### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Veritabanı yönetim hizmeti](https://docs.microsoft.com/azure/dms/dms-overview) | Contoso DMS kullanarak uygulamayı veri katmanının geçirir. DMS bir siteden siteye VPN üzerinden şirket içi SQLVM makineye bağlanın ve DMS etkinleştirir sorunsuz geçişler en düşük kapalı kalma süresi ile Azure veri platformları için birden fazla veritabanı kaynaktan geçirme. | Hakkında bilgi edinin [desteklenen bölgeler](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) DMS ve get için [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/database-migration/).
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Contoso Site Recovery, uygulama ön uç sanal makine için lift-and-shift ile taşıma geçişini kullanır. Site kurtarma hizmetini düzenler ve geçiş ve olağanüstü durum kurtarma Azure Vm'leri için yönetir ve şirket içi Vm'leri ve fiziksel sunucuları.  | Azure'a çoğaltma sırasında Azure depolama ücretleri uygulanır.  Azure Vm'leri oluşturulur ve yük devretme işlemi gerçekleştiğinde, ücret. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/) ücretleri ve fiyatlandırma hakkında.

 

## <a name="migration-process"></a>Geçiş işlemi

- Contoso uygulama sanal makinelerini Azure'a geçirir.
- Bunlar, Azure Site Recovery kullanarak VM'ye ön uç sanal makine geçirme:
    - İlk adım, bunlar hazırlamak ve Azure bileşenlerini ayarlayın ve şirket içi VMware altyapısını hazırlama.
    - Her şey hazır, bunlar çoğaltılan sanal makine başlayabilirsiniz.
    - Çoğaltma etkinleştirildikten sonra çalışma, bunlar VM'yi Azure'a devrederek tarafından geçirme.
- Veri geçiş hizmeti (DMS) kullanarak azure'daki bir SQL Server kümesi, veritabanını geçirme.
    - İlk adım olarak bunlar SQL Server Vm'leri azure'da sağlamak için kümenin ve iç yük dengeleyici ayarlama ve AlwaysOn Kullanılabilirlik gruplarını yapılandırma.
    - Bu yerinde bunlar veritabanını geçirme
- Geçişten sonra veritabanı için AlwaysOn koruma tıklatmalarını sağlarsınız.

![Geçiş işlemi](media/contoso-migration-rehost-vm-sql-ag/migration-process.png) 
 
## <a name="prerequisites"></a>Önkoşullar

Siz (ve Contoso) Bu senaryo çalıştırmak gerekenler şu şekildedir:

**Gereksinimleri** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Bu serideki ilk makaledeki değerlendirme gerçekleştirildiğinde aboneliği oluşturmuş olmanız. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.<br/><br/> Daha ayrıntılı izinler gerekirse gözden [bu makalede](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure altyapı** | [Bilgi nasıl](contoso-migration-infrastructure.md) Contoso Azure altyapısının ayarlayın.<br/><br/> Özel hakkında daha fazla bilgi [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) Site Recovery için gereksinimleri.
**Site recovery (şirket içi)** | Şirket içi vCenter server 5.5, 6.0 veya 6.5 sürümünü çalıştırmalıdır<br/><br/> 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir ESXi ana bilgisayarı<br/><br/> Bir veya daha fazla VMware ESXi ana bilgisayarında çalışan VM'ler.<br/><br/> Vm'leri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Desteklenen [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) yapılandırma.<br/><br/> Çoğaltmak istediğiniz VM'lerin karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).
**DMS** | DMS için gereksinim duyduğunuz bir [uyumlu şirket içi VPN cihazı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Şirket içi VPN cihazının yapılandırmanız gerekir. Dışarıya yönelik genel bir IPv4 adresi olmalıdır ve bu adresi bir NAT cihazının arkasında yer almamalıdır.<br/><br/> Şirket içi SQL Server veritabanınıza erişimi olduğundan emin olun.<br/><br/> Windows Güvenlik Duvarı, kaynak veritabanı altyapısını erişebilir olmalıdır. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Veritabanını makinenize önünde bir güvenlik duvarı varsa, veritabanı ve dosyalarına SMB bağlantı noktası 445 üzerinden erişime izin vermek için kuralları ekleyin.<br/><br/> Yönetilen örnek hedef ve kaynak SQL Server'ı bağlanmak için kullanılan kimlik bilgileri sysadmin sunucu rolünün üyesi olması gerekir.<br/><br/> Bir ağ ihtiyacınız DMS kaynak veritabanını yedeklemek için kullanabileceğiniz şirket içi veritabanı paylaşın.<br/><br/> Kaynak SQL Server örneğini çalıştıran hizmet hesabının ağ paylaşımında yazma ayrıcalıkları olduğundan emin olun.<br/><br/> Ağ paylaşımı üzerinde tam denetim ayrıcalığı olan bir Windows kullanıcı (ve parola) not edin. Azure veritabanı geçiş hizmetinin yedekleme dosyalarını Azure depolama kapsayıcısına karşıya yüklemek için bu kullanıcı kimlik bilgilerini temsil eder.<br/><br/> TCP/IP Protokolü SQL Server Express yükleme işlemi ayarlar **devre dışı bırakılmış** varsayılan olarak. Etkin olduğundan emin olun.


## <a name="scenario-steps"></a>Senaryo adımları

Contoso geçişi nasıl çalışacağını aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure'da SQL Server Vm'leri oluşturma**: yüksek kullanılabilirlik için Contoso kümelenmiş bir veritabanı azure'da dağıtmak istediğiniz. Bunlar, iki SQL Server Vm'leri ve Azure iç yük dengeleyici dağıtın.
> * **2. adım: kümeyi dağıtmak**: SQL Server Vm'leri dağıttıktan sonra bir Azure SQL Server kümesi hazırlayın.  Bunlar, veritabanını önceden oluşturulmuş bu kümesine geçirme.
> * **3. adım: DMS hazırlama**: açtıklarında kaydolmayı DMS yolculuğunuzu sağlayıcıyı hazırlamak için DMS örneği ve bir proje oluşturun. Bunlar bir paylaşılan erişim imzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) ayarlayın. DMS hizmeti SQL Server Yedekleme dosyalarını karşıya yüklemeleri depolama hesabı kapsayıcısına erişmek için SA URI kullanır.
> * **4. adım: Azure Site Recovery için hazırlama**: Bunlar bir kurtarma Hizmetleri kasası ve çoğaltılan verileri tutmak için bir Azure depolama hesabı oluşturun.
> * **5. adım: Site Recovery için şirket içi Vmware'leri hazırlama**: VM bulma ve aracı yükleme hesabı hazırlayın ve yük devretme sonrasında Azure Vm'lerinin bağlanabilmesi için şirket içi Vm'leri hazırlama.
> * **6. adım: Çoğaltma Vm'leri**: çoğaltma ayarlarını yapılandırma ve VM çoğaltmayı etkinleştirin.
> * **7. adım: veritabanı DMS ile geçiş**: Bunlar veritabanını geçirme.
> * **8. adım: Site Recovery ile Vm'leri geçirme**: Bunlar önce her şeyin çalıştığından, sanal Makineyi geçirmek için tam bir yük devretme tarafından izlenen emin olmak için bir yük devretme testi çalıştırın.


## <a name="step-1-prepare-a-sql-server-alwayson-availability-group-cluster"></a>1. adım: bir SQL Server AlwaysOn Kullanılabilirlik grubu küme hazırlama

Contoso, azure'daki bir kümeye veritabanında dağıtarak İleriyi ister. Bunlar daha sonra bu kümedeki diğer veritabanları için kullanabilirsiniz. 

- SQL Server Vm'leri (üretim kaynaklar için kullanılan) ContosoRG kaynak grubundaki dağıtılır.
- Benzersiz adlar dışında her iki VM aynı ayarları kullanın.
- İç load balancer (ağ kaynakları için kullanılan) ContosoNetworkingRG dağıtılacağı.
- Sanal makineleri Windows Server 2016 SQL Server 2017 Enterprise Edition ile çalışır. Lisans ücreti için kendi Azure EA taahhüdü olarak sağlayan Azure Marketi'nde bir görüntü kullanacaksınız böylece Contoso için bu işletim sistemi lisans yok.

İşte nasıl Contoso kümesi ayarlayın:

1. Azure Marketi'nde SQL Server 2017 Enterprise Windows Server 2016 görüntüsü seçerek iki SQL Server Vm'leri oluştururlar. 

    ![SQL VM SKU](media/contoso-migration-rehost-vm-sql-ag/sql-vm-sku.png)

2. İçinde **sanal makine Sihirbazı oluşturma** > **Temelleri**, bunlar yapılandırın:

    - Sanal makinelerin adları: **SQLAOG1** ve **SQLAOG2**.
    - Makineleri iş açısından kritik olduğundan, sanal makine disk türünü SSD etkinleştirin.
    - Bunlar, makine kimlik bilgilerini belirtin.
    - Dağıttıkları VM'lerin birincil Doğu ABD 2 bölgesinde ContosoRG kaynak grubu.

3. İçinde **boyutu**, bunlar için her iki VM D2s_V3 SKU ile başlatın. Bunların ölçeğini daha sonra ihtiyaç duydukları gibi.
4. İçinde **ayarları**, bunlar aşağıdakileri yapın:

    - Bu VM'ler önemli veritabanları için uygulama olduğundan, bunlar yönetilen diskler kullanır.
    - Bunlar makineleri Doğu ABD 2 üretim ağında birincil yerleştirmek bölge (**VNET PROD EUS2**), veritabanı alt ağdaki (**PROD DB EUS2**).
    - Yeni bir kullanılabilirlik kümesi oluştururlar: **SQLAOGAVSET**iki hata etki alanı ve beş güncelleştirme etki alanları.

    ![SQL VM](media/contoso-migration-rehost-vm-sql-ag/sql-vm-settings.png)

4. İçinde **SQL Server ayarları**, bunlar varsayılan bağlantı noktası 1433'ten sanal ağa (özel), SQL Bağlantısı sınırlamak. Yerinde kullandıkları kimlik doğrulaması için kimlik bilgilerini kullandıkları (**contosoadmin**).

    ![SQL VM](media/contoso-migration-rehost-vm-sql-ag/sql-vm-db.png)

**Daha fazla yardıma mı ihtiyacınız var?**

- [Yardım](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision#1-configure-basic-settings) bir SQL Server VM'si sağlama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-prereq#create-sql-server-vms) farklı SQL Server SKU için Vm'leri yapılandırma.

## <a name="step-2-deploy-the-failover-cluster"></a>2. adım: yük devretme kümesi dağıtma

İşte nasıl Contoso kümesi ayarlayın:

1. Bunlar, bulut tanığı olarak görev yapacak bir Azure depolama hesabı ayarlayın.
2. Bunlar, SQL Server Vm'leri Contoso şirket içi veri merkezinde Active Directory etki alanına ekleyin.
3. Bunlar, Azure'da küme oluşturun.
4. Bunlar, bulut tanığı yapılandırın.
5. Son olarak, SQL Always On kullanılabilirlik grupları sağlar.

### <a name="set-up-a-storage-account-as-cloud-witness"></a>Bulut tanığı olarak bir depolama hesabı ayarlama

Bulut tanığı ayarlamak için Contoso için küme Tahkim kullanılan blob dosyası barındıracak bir Azure depolama hesabı gerekir. Aynı depolama hesabı, birden fazla küme için bulut tanığı ayarlamak için kullanılabilir. 

Contoso şu şekilde bir depolama hesabı oluşturur:

1. Bunlar hesap için tanınabilir bir ad belirtin (**contosocloudwitness**).
2. Bunlar genel çok amaçlı hesabı, LRS ile dağıtın.
3. Bunlar, üçüncü bir bölge içinde - Orta Güney ABD hesap yerleştirin. Bölgesel bir hata oluşması halinde kullanılabilir durumda kalır, böylece bunlar, birincil ve ikincil bölgesi dışında yerleştirin.
4. Bu altyapı kaynaklarını - tutan kendi kaynak grubuna yerleştirin **ContosoInfraRG**.

    ![Bulut tanığı](media/contoso-migration-rehost-vm-sql-ag/witness-storage.png)

5. Olduğunda, birincil depolama hesabı oluşturun ve ikincil erişim anahtarları için oluşturulur. Bunlar, bulut tanığı oluşturmak için birincil erişim anahtarı gerekir. Anahtar depolama hesabı adı altında görünür > **erişim anahtarlarını**.

    ![Erişim anahtarı](media/contoso-migration-rehost-vm-sql-ag/access-key.png)

### <a name="add-sql-server-vms-to-contoso-domain"></a>SQL Server Vm'leri için Contoso etki alanı Ekle

1. Contoso SQLAOG1 ve SQLAOG2 contoso.com etki alanına ekler.
2. Ardından, her sanal makinede, Windows Yük devretme kümesi özelliğini ve araçlarını yükleyin.

## <a name="set-up-the-cluster"></a>Küme oluşturma

Kümeyi ayarlamadan önce Contoso her makinede işletim sistemi diskinin anlık görüntüsünü alır.

![anlık görüntü](media/contoso-migration-rehost-vm-sql-ag/snapshot.png)

2. Ardından, bunlar koyarlar betik çalıştırma birlikte Windows Yük devretme kümesi oluşturun.

    ![Küme oluşturma](media/contoso-migration-rehost-vm-sql-ag/create-cluster1.png)

3. Bunlar küme oluşturduktan sonra Vm'leri küme düğümü olarak göründüğünü doğrulayın.

     ![Küme oluşturma](media/contoso-migration-rehost-vm-sql-ag/create-cluster2.png)

## <a name="configure-the-cloud-witness"></a>Bulut tanığı yapılandırın

1. Contoso kullanarak bulut tanığı yapılandırma **çekirdeği Yapılandırma Sihirbazı'nı** yük devretme kümesi Yöneticisi'nde.
2. Bulut tanığı ile depolama hesabı oluşturma Sihirbazı'nda bunlar seçin.
3. Bulut tanığı yapılandırdıktan sonra Yük Devretme Kümesi Yöneticisi ek bileşeninde görünür.

    ![Bulut tanığı](media/contoso-migration-rehost-vm-sql-ag/cloud-witness.png)
            

## <a name="enable-sql-server-always-on-availability-groups"></a>SQL Server Always On kullanılabilirlik gruplarını etkinleştir

Contoso, artık her zaman açık etkinleştirebilirsiniz:

1. SQL Server Yapılandırma Yöneticisi'nde tanırlar **AlwaysOn Kullanılabilirlik grupları** için **SQL Server (MSSQLSERVER)** hizmeti.

    ![AlwaysOn özelliğini etkinleştirin](media/contoso-migration-rehost-vm-sql-ag/enable-alwayson.png)

2. Bunlar, değişikliklerin etkili olabilmesi için hizmeti yeniden başlatın.

AlwaysOn etkin Contoso SmartHotel veritabanını koruyacak AlwaysOn Kullanılabilirlik grubu ayarlayabilirsiniz.

**Daha fazla yardıma mı ihtiyacınız var?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness) bulut tanığı ve bunun için bir depolama hesabı ayarlama.
- [Yönergeleri almak](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial) kümesi oluşturma ve bir kullanılabilirlik grubu oluşturma.

## <a name="step-3-deploy-the-azure-load-balancer"></a>3. adım: Azure yük dengeleyici dağıtma

Contoso artık istediğiniz küme düğümlerin önünde bulunur bir iç Yük Dengeleyiciyi dağıtmak. Yük dengeleyicinin trafiği dinlediği ve uygun düğümü için yönlendirir.

![Yük dengeleme](media/contoso-migration-rehost-vm-sql-ag/architecture-lb.png)

Bunlar gibi yük dengeleyici oluşturun:

1. Azure portalında > **ağ** > **Load Balancer**, bunlar yeni bir iç yük dengeleyicisi ayarlama: **ILB-PROD-DB-EUS2-SQLAOG**.
2. Bunlar bir üretim ağında yük dengeleyici yerleştirin **VNET PROD EUS2**, veritabanı alt **PROD DB EUS2**.
3. Bunlar statik bir IP adresi atayın: 10.245.40.100.
4. Yük Dengeleyici ağ kaynak grubunda bir ağ öğesi olarak dağıttıkları **ContosoNetworkingRG**.

    ![Yük dengeleme](media/contoso-migration-rehost-vm-sql-ag/lb-create.png)

İç load balancer dağıtıldıktan sonra Contoso kurabilecek. Bunlar bir arka uç adres havuzu oluşturun, bir durum araştırması ayarlayın ve Yük Dengeleme kuralı yapılandırın.

### <a name="add-a-backend-pool"></a>Arka uç havuzu Ekle

Trafiği kümedeki Vm'leri dağıtmak için yük dengeleyiciden ağ trafiği alacak sanal makineler için NIC IP adreslerini içeren bir arka uç adres havuzu Contoso ayarlayın.

1. Portalda yük dengeleyici ayarlarını Contoso bir arka uç havuzu Ekle: **ILB-PROD-DB-EUS-SQLAOG-BEPOOL**.
2. Bunlar havuzun SQLAOGAVSET kullanılabilirlik kümesiyle ilişkilendirin. Kümedeki sanal makineler (**SQLAOG1** ve **SQLAOG2**) havuzuna eklenir.

    ![Arka uç havuzu](media/contoso-migration-rehost-vm-sql-ag/backend-pool.png)

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Contoso bir durum araştırması oluşturur, böylece yük dengeleyici durumunu izleyebilirsiniz. Araştırma, dinamik olarak ekler veya Vm'leri üzerinde nasıl bunlar yanıt durum denetimleri için temel yük dengeleyici rotasyonuna kaldırır.

Bunlar gibi araştırması oluşturun: 

1. Yük Dengeleyici ayarlarında portalında Contoso bir durum araştırması oluşturur: **SQLAlwaysOnEndPointProbe**.
2. Bunlar, TCP bağlantı noktası 59999 Vm'leri izlemek için araştırma ayarlayın.
3. Bunlar arasındaki araştırmaları ve bir eşik değeri 2'in 5 saniye aralığını ayarlayın. İki araştırmaları başarısız olursa, sanal Makinenin sağlıksız kabul edilir.

    ![Araştırma](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

### <a name="configure-the-load-balancer-to-receive-traffic"></a>Trafiği almak için Yük Dengeleyiciyi yapılandırma


Yük Dengeleyici kuralı için trafiğin Vm'lere dağıtımını defins artık, Contoso gerekiyor.

- Ön uç IP adresi, gelen trafiği işler.
- Arka uç IP havuzu trafik alır.

Bunlar gibi kuralı oluşturun:

1. Portalda yük dengeleyici ayarlarını, yeni bir Yük Dengeleme Kuralı Ekle: **SQLAlwaysOnEndPointListener**.
2. Contoso TCP 1433'ten gelen SQL istemci trafiğini almak için ön uç bir dinleyici ayarlar.
3. Bunlar, arka uç havuzuna hangi trafik yönlendirilir ve trafiği için VM'ler üzerinde dinleme bağlantı noktasını belirtin.
4. Contoso kayan IP (doğrudan sunucu dönüşü) sağlar. Bu her zaman SQL AlwaysOn için gereklidir.

    ![Araştırma](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

**Daha fazla yardıma mı ihtiyacınız var?**

- [Genel bakışın](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) Azure Load Balancer'ın.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-basic-internal-portal) bir yük dengeleyici oluşturma.



## <a name="step-4-prepare-azure-for-the-site-recovery-service"></a>4. adım: Azure Site Recovery hizmeti için hazırlanma

Contoso Site Recovery dağıtmak için gereken Azure bileşenleri şunlardır:

- Yük devretme sırasında oluştururken, Vm'leri yer alacağı bir sanal ağ.
- Çoğaltılan verileri tutmak için bir Azure depolama hesabı. 
- Azure kurtarma Hizmetleri kasasında.

Bunlar bu aşağıdaki ayarlamaları yapın:

1.  Site Recovery için kullanabilecekleri bir ağ/alt oluşturmuş Contoso olduğunda bunlar [Azure altyapısı dağıtılan](contoso-migration-rehost-vm-sql-ag.md).

    - Bir üretim uygulaması SmartHotel uygulamasıdır ve WEBVM birincil Doğu ABD 2 bölgesinde Azure üretim ağına (VNET-PROD-EUS2) geçirilecektir.
    - WEBVM üretim kaynaklar için kullanılan ContosoRG kaynak grubundaki ve üretim alt ağdaki (ürün FE EUS2) yer alır.

2. Contoso birincil bölgede bir Azure depolama hesabı (contosovmsacc20180528) oluşturur.

    - Standart depolama ve LRS çoğaltma ile genel amaçlı bir hesabını kullanırlar.
    - Hesabı, kasa ile aynı bölgede olmalıdır.

    ![Site Kurtarma Depolama](media/contoso-migration-rehost-vm-sql-ag/asr-storage.png)

3. Ağ ve depolama hesabı ile yerinde, bunlar şimdi bir kurtarma Hizmetleri kasası oluşturun (**ContosoMigrationVault**) ve yerleştirebilir **ContosoFailoverRG** birincil Doğu ABD 2 bölgesinde bir kaynak grubu .

    ![Kurtarma Hizmetleri kasası](media/contoso-migration-rehost-vm-sql-ag/asr-vault.png)

**Daha fazla yardıma mı ihtiyacınız var?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Site Recovery için Azure'ı ayarlama.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>4. adım: Site Recovery için şirket içi Vmware'leri hazırlama

İşte şirket içinde Contoso hazırlar:

- VM bulmayı otomatikleştirmek için vCenter sunucusunda veya vSphere ESXi konağı üzerinde bir hesap.
- Çoğaltmak istediğiniz VMware Vm'lerinde Mobility hizmetini otomatik olarak yüklenmesini sağlayan bir hesap.
- VM ayarlarını Contoso çoğaltılan Azure VM'ye yük devretmeden sonra bağlanabilmesi için şirket içi.


### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- VM'leri otomatik olarak bulma. 
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme.
- En az bir salt okunur hesap gereklidir. Oluşturma ve diskleri kaldırma ve Vm'leri kapatarak gibi işlemleri de çalıştırabilirsiniz bir hesabınız olmalıdır.

Contoso hesabı gibi ayarlar:

1. Contoso bir rolü vCenter düzeyinde oluşturur.
2. Contoso, daha sonra bu rol gerekli izinleri atar.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Her sanal makinede Mobility hizmetinin yüklenmesi gerekir.

- VM için çoğaltma etkinleştirildiğinde site Recovery bu bileşeni otomatik gönderim yüklemesi yapabilirsiniz.
- Site Recovery, göndererek yükleme VM'ye erişmek için kullanabileceğiniz bir hesabınız olmalıdır. Azure konsolunda çoğaltma ayarlarken bu hesabı belirtirsiniz.
- Hesap etki alanı veya yerel, VM'ye yüklemek için gerekli izinlere sahip olabilir.




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


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>5. adım: Azure Site Recovery ile şirket içi Vm'lerini çoğaltma

Azure'da bir geçiş çalıştırmadan önce Contoso gerekir ayarlama ve çoğaltmayı etkinleştirin.

### <a name="set-a-replication-goal"></a>Çoğaltma hedefi ayarlama

1. Kasada kasa adını (ContosoVMVault) altında çoğaltma hedefi seçme (**Başlarken** > **Site Recovery** > **altyapıyı hazırlama** .
2. Bunlar, kendi makinelerine Vmware'de çalıştırılan ve Azure'a çoğaltılan şirket, olduğunu belirtin.

    ![Çoğaltma hedefi](./media/contoso-migration-rehost-vm-sql-ag/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

Devam etmek için bunlar seçerek dağıtım planlamasını tamamladınız mı onaylamak için ihtiyaçları **Evet, yaptım**. Bu senaryoda Contoso yalnızca bir sanal Makineyi geçirmek olan ve dağıtım planlama gerekmez.

### <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Contoso, kaynak ortamı yapılandırması gerektiğini belirtir. Bunu yapmak için bir OVF şablonu indirin ve Site Recovery yapılandırma sunucusunu yüksek oranda kullanılabilir olarak dağıtmak için kullanın, şirket içi VMware sanal makine. Yapılandırma sunucusunu çalışır duruma geldikten sonra bunlar bu kasaya kaydedin.

Yapılandırma sunucusu, çeşitli bileşenler çalıştırır:

- Şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir yapılandırma sunucusu bileşeni.
- İşlem sunucusu çoğaltma ağ geçidi davranır. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir.
- İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.

Contoso adımları aşağıdaki gibi gerçekleştirin:


1. Kasası'nda, OVF şablonu indirin **altyapıyı hazırlama** > **kaynak** > **yapılandırma sunucusu**.
    
    ![OVF indirin](./media/contoso-migration-rehost-vm-sql-ag/add-cs.png)

2. Bunlar, şablonu oluşturup VM dağıtmak için Vmware'e aktarın.

    ![OVF şablonu](./media/contoso-migration-rehost-vm-sql-ag/vcenter-wizard.png)

3. Sanal makinede ilk kez açtığınızda, oluşturan bir Windows Server 2016 yükleme deneyimi önyüklenir. Bunlar lisans sözleşmesini kabul edin ve bir yönetici parolasını girin.
4. Yükleme tamamlandıktan sonra VM için yönetici olarak oturum açın. İlk oturum açma işleminde, varsayılan olarak Azure Site kurtarma Yapılandırması aracını çalıştırır.
5. Aracı'nda, yapılandırma sunucusunu kasaya kaydetmek için kullanılacak bir ad belirtin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra bunlar Azure aboneliği için oturum açın. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.

    ![Yapılandırma sunucusunu kaydetmek](./media/contoso-migration-rehost-vm-sql-ag/config-server-register2.png)

7. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır.
8. Bunların makinede yeniden oturum açın ve yapılandırma sunucusu yönetim Sihirbazı otomatik olarak başlar.
9. Sihirbazda, çoğaltma trafiğini almak için NIC'yi seçin. Bu ayar yapılandırıldıktan sonra değiştirilemez.
10. Abonelik, kaynak grubu ve yapılandırma sunucusunu kaydetmek istediğiniz kasaya seçerler.
        ![Kasa](./media/contoso-migration-rehost-vm-sql-ag/cswiz1.png) 

10. Bunlar sonra da indirin ve MySQL Server ve VMWare powerclı'yı yükleyin. 
11. Doğrulama sonrasında, bunlar vCenter sunucusunda veya vSphere konağının FQDN'sini veya IP adresini belirtin. Bunlar, varsayılan bağlantı noktasını değiştirmeyin ve vCenter sunucusu için bir kolay ad belirtin.
12. Bunlar, bunlar otomatik bulma için oluşturduğunuz hesabı ve otomatik olarak Mobility hizmetini yükleme için kullanılan kimlik bilgilerini belirtin. Windows makineleri için hesabın vm'lerinde yerel yönetici ayrıcalıkları gerekir.

    ![vCenter](./media/contoso-migration-rehost-vm-sql-ag/cswiz2.png)

7. Kayıt tamamlandığında Azure Portalı'nda yapılandırma sunucusunun ve VMware sunucusunun listelendiğini Contoso çift denetler **kaynak** kasadaki sayfası. Bulma, 15 dakika veya daha fazla sürebilir. 
8. Site Recovery belirtilen ayarları kullanarak VMware sunucularına bağlanır ve Vm'leri bulur.

### <a name="set-up-the-target"></a>Hedefi ayarlama

Artık Contoso hedef çoğaltma ayarlarını belirtir.

1. İçinde **altyapıyı hazırlama** > **hedef**, bunlar hedef ayarları seçin.
2. Site Recovery, bir Azure depolama hesabını ve belirtilen hedef ağda olup olmadığını denetler.

### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Hayır, Contoso bir çoğaltma ilkesi oluşturabilirsiniz.

1. İçinde **altyapıyı hazırlama** > **çoğaltma ayarları** > **Çoğaltma İlkesi** >  **oluşturma ve İlişkilendirme**, bunlar bir ilke oluşturmak **ContosoMigrationPolicy**.
2. Bunlar, varsayılan ayarları kullanın:
    - **RPO eşiği**: varsayılan 60 dakika. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
    - **Kurtarma noktası bekletme**. Varsayılan 24 saat. Bu değer, ne kadar süreyle her kurtarma noktası için bekletme süresinin olacağını belirtir. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**. Varsayılan bir saat değeri. Bu değer, uygulamayla tutarlı anlık görüntülerin oluşturulma sıklığı belirtir.
 
        ![Çoğaltma ilkesi oluşturma](./media/contoso-migration-rehost-vm-sql-ag/replication-policy.png)

5. İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. 

    ![Çoğaltma İlkesi ilişkilendirme](./media/contoso-migration-rehost-vm-sql-ag/replication-policy2.png)



### <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Artık Contoso WebVM çoğaltma başlayabilirsiniz.

1. İçinde **uygulama çoğaltma** > **kaynak** > **+ Çoğalt** bunlar kaynak ayarlarını seçin.
2. Bunlar, bunlar, Vm'leri etkinleştirmek istiyorsanız, vCenter sunucusu ile configuration server seçin gösterir.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm-sql-ag/enable-replication1.png)

3. Şimdi, kaynak grubunu ve sanal ağ ve çoğaltılan verilerin depolanacağı depolama hesabı dahil olmak üzere hedef ayarlarını belirtin.

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm-sql-ag/enable-replication2.png)

3. Contoso, çoğaltma için WebVM seçer, çoğaltma ilkesini denetler ve çoğaltmayı etkinleştirir. Çoğaltma etkinleştirildiğinde site Recovery, sanal makinede Mobility hizmetini yükler.
 
    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm-sql-ag/enable-replication3.png)

4. Bunlar çoğaltma ilerlemeyi **işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.
5. İçinde **Essentials** Azure portalında, Azure'a çoğaltılan VM'ler için yapı Contoso görebilirsiniz.

    ![Altyapı görünümü](./media/contoso-migration-rehost-vm-sql-ag/essentials.png)


**Daha fazla yardıma mı ihtiyacınız var?**

- Bu adımlar tam bir kılavuza edinebilirsiniz [şirket içi VMware Vm'leri için olağanüstü durum kurtarmayı ayarlayın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Yardımcı olmak ayrıntılı yönergeler kullanılabilir [kaynak ortamını ayarlama](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [yapılandırma sunucusunu dağıtma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), ve [çoğaltma ayarlarını yapılandırma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- Daha fazla bilgi edinebilirsiniz [çoğaltma etkinleştirme](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-install-the-database-migration-assistant-dma"></a>5. adım: veritabanı geçiş Yardımcısı (DMA) yükleme

Contoso SmartHotel veritabanı için Azure VM geçirme **SQLAOG1** DMA'yı kullanarak. Bunlar DMA ' şu şekilde ayarlayın:

1. Bunlar aracı indirin [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) şirket içi SQL Server VM (**SQLVM**).
2. Bunlar Kurulum (DownloadMigrationAssistant.msi) sanal makinede çalıştırın.
3. Üzerinde **son** seçtikleri sayfa **başlatma Microsoft Data Migration Yardımcısı** Sihirbazı tamamlamadan önce.

## <a name="step-6-migrate-the-database-with-dma"></a>6. adım: DMA veritabanını geçirme

1. Yeni bir geçiş - çalıştırdığı DMA'da **SmartHotel**.
2. Seçmeleri **hedef sunucu türü** olarak **Azure Virtual Machines'de SQL Server**. 

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-1.png)

3. Geçiş ayrıntıları ekledikleri **SQLVM** kaynak sunucu ve **SQLAOG1** hedefi olarak. Bunlar, her makine için kimlik bilgilerini belirtin.

     ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-2.png)

4. Bunlar, veritabanı ve yapılandırma bilgileri için yerel bir paylaşım oluşturun. Yazma erişimiyle SQLVM ve SQLAOG1 SQL hizmet hesabı tarafından erişilebilir olması gerekir.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-3.png)

5. Contoso geçirilmelidir ve geçiş başlar oturumların seçer. DMA, bittikten sonra geçiş başarılı olarak gösterir.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-4.png)

6. Veritabanı çalışmakta olduğunu doğrulayın **SQLAOG1**.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-5.png)

DMS şirket içi SQL Server VM Contoso veri merkeziniz ile Azure arasında siteden siteye VPN bağlantısı üzerinden bağlanır ve ardından veritabanına geçirir.

## <a name="step-7-protect-the-database"></a>7. adım: veritabanını koruma

Üzerinde çalışan uygulama veritabanıyla **SQLAOG1**, Contoso artık koruyabilir AlwaysOn Kullanılabilirlik Grupları'nı kullanarak. Bunlar AlwaysOn, SQL Management Studio'yu kullanarak yapılandırın ve ardından Windows Kümelemesi kullanarak bir dinleyici atayın. 

### <a name="create-an-alwayson-availability-group"></a>AlwaysOn Kullanılabilirlik grubu oluşturma

1. SQL Management Studio'da, bunlar üzerinde sağ **her zaman yüksek kullanılabilirliğine** başlatmak için **yeni Kullanılabilirlik Grubu Sihirbazı'nı**.
2. İçinde **seçeneklerini belirtin**, bunlar kullanılabilirlik grubu adı **SHAOG**. İçinde **veritabanlarını seçin**, bunlar SmartHotel veritabanını seçin.

    ![AlwaysOn Kullanılabilirlik grubu](media/contoso-migration-rehost-vm-sql-ag/aog-1.png)

3. İçinde **çoğaltmaları belirle**, kullanılabilirlik çoğaltmaları olarak iki SQL düğüm ekleme ve bunları otomatik yük devretme ile zaman uyumlu işleme sağlamak üzere yapılandırın.

     ![AlwaysOn Kullanılabilirlik grubu](media/contoso-migration-rehost-vm-sql-ag/aog-2.png)

4. Yöneticiler grubu için bir dinleyici yapılandırma (**SHAOG**) ve bağlantı noktası. İç yük dengeleyici IP adresi, statik IP adresi (10.245.40.100) eklenir.

    ![AlwaysOn Kullanılabilirlik grubu](media/contoso-migration-rehost-vm-sql-ag/aog-3.png)

5. İçinde **veri eşitlemesini Seç**, otomatik dengeli dağıtımı etkinleştirin. Contoso yok el ile yedekleme ve bu geri yükleme Bu seçenek belirtilmişse, SQL Server otomatik olarak her veritabanı için ikincil çoğaltma grubunda oluşturur, bu nedenle. Doğrulama sonrasında, kullanılabilirlik grubu oluşturulur.

    ![AlwaysOn Kullanılabilirlik grubu](media/contoso-migration-rehost-vm-sql-ag/aog-4.png)

6. Contoso, grubu oluştururken bir sorunla karşılaşıldı. Bunlar Active Directory Windows tümleşik güvenliği kullanmıyorsanız ve bu nedenle Windows Yük devretme kümesi rollerini oluşturmak için SQL oturum açma için izinleri vermeniz gerekir.

    ![AlwaysOn Kullanılabilirlik grubu](media/contoso-migration-rehost-vm-sql-ag/aog-5.png)

6. Contoso grubu oluşturulduktan sonra SQL Management Studio'da görebilirsiniz.

### <a name="configure-a-listener-on-the-cluster"></a>Kümeye bir dinleyici yapılandırma

SQL dağıtımı ayarlama son adımda, Contoso küme üzerinde dinleyici olarak iç Yük Dengeleyiciyi yapılandırır ve dinleyici çevrimiçi duruma getirir. Bunu yapmak için bir betik kullanırlar.

![Küme dinleyicisi](media/contoso-migration-rehost-vm-sql-ag/cluster-listener.png)


### <a name="verify-the-configuration"></a>yapılandırıldığını doğrulayın

Her şeyi, Contoso artık sahip bir işlev kullanılabilirlik grubu geçirilen veritabanı kullanan Azure'da. Bunlar iç Yük Dengeleyiciyi SQL Management Studio'da bağlanarak doğrulayın.

![ILB bağlanma](media/contoso-migration-rehost-vm-sql-ag/ilb-connect.png)

**Daha fazla yardıma mı ihtiyacınız var?**
- Oluşturma hakkında bilgi edinin bir [kullanılabilirlik grubu](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#create-the-availability-group) ve [dinleyici](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#configure-listener).
- El ile [kümesi, yük dengeleyici IP adresi kullanacak şekilde ayarlama](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#configure-the-cluster-to-use-the-load-balancer-ip-address).
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2) oluşturma ve SAS kullanma hakkında.


## <a name="step-8-migrate-the-vm-with-site-recovery"></a>8. adım: Site Recovery ile VM'yi geçirme

Contoso hızlı çalıştırın, yük devretme testi ve sonra VM'yi geçirme.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Yardımcı olur, yük devretme testi çalıştıran her şeyin geçiş işleminden önce beklendiği gibi çalıştığından emin olun. 

1. Contoso zaman en son kullanılabilir noktaya yük devretme testi çalıştırır (**en son işlenen**).
2. Seçmeleri **yük devretmeye başlamadan önce makineyi Kapat**, böylece Site Recovery yük devretmeyi tetiklemeden önce kaynak sanal kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. 
3. Test yük devretme çalıştırır: 

    - Bir önkoşul denetimi geçiş için gerekli koşulları tümünün karşılandığından emin olmak için çalıştırılır.
    - Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
    - Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.

3. Yük devretme bittikten sonra çoğaltma Azure VM Azure Portalı'nda görünür. Contoso, VM doğru ağa bağlandığından uygun boyutta olduğundan ve çalıştığından emin denetler. 
4. Doğruladıktan sonra Contoso devretmeyi temizlemek ve gözlemlerinizi kaydetmek ve. 

### <a name="run-a-failover"></a>Yük devretme çalıştırma

1. Test yük devretmesi beklendiği gibi çalıştığını doğruladıktan sonra Contoso geçiş için bir kurtarma planı oluşturma ve WEBVM plana ekleyin.

     ![Kurtarma planı](./media/contoso-migration-rehost-vm-sql-ag/recovery-plan.png)

2. Bunlar, planı üzerinde bir yük devretme çalıştırın. Bunlar en son kurtarma noktası seçin ve Site Recovery yük devretmeyi tetiklemeden önce şirket içi sanal makineyi denemesi gerektiğini belirtin.

    ![Yük devretme](./media/contoso-migration-rehost-vm-sql-ag/failover1.png)

3. Yük devretmeden sonra bunlar Azure VM Azure Portalı'nda beklendiği gibi göründüğünü doğrulayın.

    ![Kurtarma planı](./media/contoso-migration-rehost-vm-sql-ag/failover2.png)

6. Azure'da VM doğruladıktan sonra geçiş işlemini tamamlayın, VM için çoğaltma durdurma ve sanal makine için Site Recovery Faturalaması durdurmak için geçişi tamamlayın.

    ![Yük devretme](./media/contoso-migration-rehost-vm-sql-ag/failover3.png)

### <a name="update-the-connection-string"></a>Bağlantı dizesini güncelleştirme

Contoso geçiş sürecinin son adım olarak, bağlantı dizesini uygulamanın SHAOG Dinleyicide çalıştıran geçirilen veritabanına işaret edecek şekilde güncelleştirin. Bu yapılandırma, artık Azure'da çalışan WEBVM üzerinde değiştirilecektir.  Bu yapılandırma, ASP uygulamasının web.config dosyasında bulunur. 

1. C:\inetpub\SmartHotelWeb\web.config dosyasını bulun.  AOG FQDN'sini yansıtacak şekilde sunucu adını değiştirme: shaog.contoso.com.

    ![Yük devretme](./media/contoso-migration-rehost-vm-sql-ag/failover4.png)  

2. Dosyası güncelleştiriliyor ve kaydettikten sonra IIS WEBVM üzerinde yeniden başlatın. Bunlar bir komut isteminden /RESTART IISRESET kullanarak yapın.
2. IIS yeniden başlatıldıktan sonra uygulama artık SQL mı üzerinde çalışan bir veritabanı kullanıyor.


**Daha fazla yardıma mı ihtiyacınız var?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) yük devretme testi çalıştırma. 
- [Bilgi](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) bir kurtarma planı oluşturma.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Azure'a devretmek.

## <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Geçişten sonra SmartHotel uygulamayı bir Azure sanal makinesinde çalışan ve SmartHotel veritabanı Azure SQL kümesi bulunur.

Şimdi, Contoso temizleme adımları tamamlaması gerekir:  

- Şirket içi sanal makineleri, vCenter stok kaldırın.
- Sanal makinelerin yerel yedekleme işlerden kaldırın.
- VM'ler için yeni konumlar ve IP adresleri göstermek için iç belgeleri güncelleştirin.
- Yetkisi alınan Vm'leri ile etkileşimde bulunan tüm kaynakları gözden geçirin ve herhangi bir ilgili ayarları veya belgeleri yeni yapılandırmayı yansıtacak şekilde güncelleştirin.
- (SQLAOG1 ve SQLAOG2) iki yeni Vm'lere izleme sistemlerinden üretime eklenmelidir ekleyin.

## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Azure'da geçirilen kaynakları ile tam olarak çalışır hale getirme ve yeni altyapılarını güvenli Contoso gerekir.

### <a name="security"></a>Güvenlik

Contoso güvenlik ekibi, Azure Vm'leri WEBVM, SQLAOG1 ve SQLAOG2 güvenlik sorunları belirlemek için inceler. 

- Bunlar, VM erişimi denetlemek ağ güvenlik grupları (Nsg'ler) gözden geçirin. Nsg'ler, yalnızca uygulamaya izin trafik geçirebilirsiniz emin olmak için kullanılır.
- Bunlar, Azure Disk şifreleme ve anahtar Kasası'nı kullanarak diskteki verilerin güvenliğini sağlama değerlendiriyorsanız.
- Bunlar saydam veri şifrelemesi (TDE) değerlendirmek ve yeni SQL AOG üzerinde çalıştırılan SmartHotel veritabanındaki etkinleştirin. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017).

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) VM'ler için önerilen güvenlik uygulamaları hakkında.

### <a name="backups"></a>Yedeklemeler

Contoso gittiğini WEBVM, SQLAOG1 ve SQLAOG2 verileri yedeklemek için Azure Yedekleme hizmetini kullanarak. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

1. Contoso için kendi WEBVM var olan Lisansınızdan sahiptir ve Azure hibrit avantajı özelliğinden yararlanır.  Bunlar bu fiyatlandırmanın avantajlarından yararlanmak için var olan Azure Vm'lerinin dönüştürmeniz.
2. Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet Yönetimi olanak sağlar. Bu Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olacak bir çoklu bulut maliyet yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında. 

## <a name="conclusion"></a>Sonuç

Bu makalede, Contoso uygulama ön uç VM'nin Site Recovery hizmetini kullanarak Azure'a geçiş yaparak Azure SmartHotel uygulamada rehosted. Azure'da sağlanan SQL Server kümesi için uygulama veritabanı geçişi ve bir SQL Server AlwaysOn Kullanılabilirlik grubunda korumalı.

## <a name="next-steps"></a>Sonraki adımlar

Serinin sonraki makalede, biz Contoso Linux'ta çalışan hizmet Masası osTicket uygulamasının nasıl yeniden barındırma göstereceğiz ve bir MySQL veritabanı ile dağıtılabilir.


