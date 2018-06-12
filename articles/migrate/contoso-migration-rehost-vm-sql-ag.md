---
title: Geçiş yaparak Contoso uygulamayı rehost Azure Vm'leri ve SQL Server AlwaysOn Kullanılabilirlik grubu için | Microsoft Docs
description: Nasıl Contoso yeniden barındırma bir şirket içi uygulama geçirerek öğrenmek için Azure Vm'leri ve SQL Server AlwaysOn Kullanılabilirlik grubu
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: raynew
ms.openlocfilehash: 03e3aaad810f6ccd5fb376765ddbada072dedb06
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301312"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms-and-sql-server-alwayson-availability-group"></a>Contoso geçiş: Azure Vm'leri ve SQL Server AlwaysOn Kullanılabilirlik grubu için bir şirket içi uygulama yeniden barındırma

Bu makalede, Contoso Azure kendi SmartHotel uygulamada nasıl yeniden barındırma gösterilmektedir. Bunlar, bir Azure VM ve bir Azure SQL Server SQL Server AlwaysOn Kullanılabilirlik grupları ile Windows Server Yük devretme kümesinde çalışan VM, uygulama veritabanına uygulama ön uç VM geçirin.

Bu belge nasıl Contoso adlı kurgusal şirket için Microsoft Azure bulut şirket kaynaklarını geçirir Göster makaleleri bir dizi biridir. Seri arka plan bilgileri ve geçiş altyapı ayarlamak, geçiş için şirket içi kaynakları değerlendirmek ve geçişler farklı türlerde çalıştıran gösteren senaryolar içerir. Karmaşık senaryolar büyür ve zaman içinde ek makaleleri ekleyeceğiz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale serisi ve kullanırız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[Makale 2: bir Azure altyapısı dağıtın](contoso-migration-infrastructure.md) | Nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar açıklar. Altyapıyı, tüm geçiş makaleler için kullanılır. | Kullanılabilir
[Makale 3: şirket içi kaynakları değerlendirin](contoso-migration-assessment.md)  | Contoso VMware üzerinden çalışan bir şirket içi iki katmanlı SmartHotel uygulamasının bir değerlendirme nasıl çalıştığı gösterilmektedir. Contoso değerlendirir app sanal makineleri ile [Azure geçirmek](migrate-overview.md) hizmet ve uygulama SQL Server veritabanı ile [veritabanı geçiş Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[Makale 4: Azure sanal makineleri ve SQL yönetilen örnek uygulamayı yeniden barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso bir yükseltme shift geçiş Azure'a SmartHotel uygulama için nasıl çalıştığı gösterilmektedir. Contoso geçirir VM ön uç uygulamasını kullanarak [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)ve yönetilen bir SQL örneğine uygulama veritabanı kullanarak [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir
[Makale 5: Azure VM'ler için uygulama yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso SmartHotel uygulama yalnızca Site Recovery kullanarak sanal makineleri geçirmek nasıl gösterir.
Makale 6: bir uygulamaya Azure Vm'leri ve SQL Server her zaman üzerinde kullanılabilirlik grubu (Bu makalede) yeniden barındırma | Contoso SmartHotel uygulama nasıl geçirir gösterir. Contoso Site Recovery VM'ler uygulama ve veritabanı geçiş hizmeti uygulama veritabanında bir AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesi geçirmek için geçirmek için kullanır. | Kullanılabilir
[Makale 7: Azure VM'ler için Linux uygulama yeniden barındırma](contoso-migration-rehost-linux-vm.md) | Nasıl Contoso Linux osTicket uygulama yükseltme shift geçişini Azure VM'ler için Site RECOVERY'yi kullanarak mu gösterir | Planlandı
[Makale 8: Linux uygulama Azure VM'ler ve Azure MySQL sunucusu için yeniden barındırma](contoso-migration-rehost-linux-vm-mysql.md) | MySQL çalışma ekranı kullanarak Azure MySQL Server örneğine uygulama veritabanı geçirir ve nasıl Contoso Linux osTicket uygulama Azure Site Recovery kullanarak sanal makineleri geçirir gösterilmiştir. | Kullanılabilir



Bu makalede, iki katmanlı Windows Contoso geçirin. Azure için VMware Vm'lerinde çalışan NET SmartHotel uygulama. Bu uygulamayı kullanmak istiyorsanız, açık kaynak olarak sağlanır ve buradan indirebilirsiniz [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>İş sürücüleri

BT liderlik ekibindeki yakından bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla çalışmıştır:

- **Adres iş büyümesi**: Contoso artmaktadır ve sonuç olarak şirket içi sistemlerini ve altyapı Basıncı yoktur.
- **Verimliliği artırma**: Contoso gerekiyor gereksiz yordamları kaldırın ve geliştiriciler ve kullanıcılar için işlemleri daha verimli hale getirin.  Hızlı olarak iş gereksinimlerini BT ve değil atık saat veya para, böylece daha hızlı müşteri gereksinimlerine gönderiliyor.
- **Çeviklik artırmak**: Contoso BT işletme ihtiyaçlarını daha iyi yanıt olması gerekir. Market'te başarılı şekilde bir genel ekonomi etkinleştirmek için değişiklikleri daha hızlı tepki mümkün olması gerekir.  Bu dpm'nin biçiminde alın veya bir iş engelleyici haline gelir.
- **Ölçek**: işi başarıyla büyüdükçe, Contoso BT aynı yükselmeye sorgulayabilmesi sistemleri sağlamanız gerekir.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut ekip hedeflerine bu geçiş için aşağı sabitlenmiş. Bu hedefleri en iyi geçiş yöntemini belirlemek için kullanılıyordu:

- Bugün VMWare yaptığı gibi geçişten sonra Azure uygulamasında aynı performans özellikleri olması gerekir.  Uygulama içi olarak bulutta kritik olarak kalır.
- Bu uygulamada yatırım contoso istememektedir.  İş için önemlidir, ancak mevcut haliyle basitçe buluta güvenle taşımak istedikleri.
- Uygulama için şirket içi veritabanı kullanılabilirlik sorunları yaşadı. Yük devretme yetenekleri ile yüksek kullanılabilirlik kümesi olarak Azure üzerinde dağıtılan görmek istiyorsunuz.
- Contoso, SQL Server 2017 geçerli kendi SQL Server 2008 R2 platformundan yükseltmek istiyor.
- Contoso bir Azure SQL veritabanı için bu uygulamayı kullanmak istememektedir ve alternatifleri için aramaktadır.

## <a name="proposed-architecture"></a>Önerilen mimarisi

Bu senaryoda:

- Uygulama, iki VM arasında (WEBVM ve SQLVM) katmanlı.
- Sanal makineleri VMware ESXi ana bilgisayarda bulunan **contosohost1.contoso.com** (sürüm 6.5)
- VMware ortamı vCenter Server 6.5 tarafından yönetilen (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'ile bir şirket içi etki alanı denetleyicisi (**contosodc1 adlı**).
- Geçiş tamamlandıktan sonra şirket içi sanal makineleri Contoso veri merkezinde hizmetten.

![Senaryo mimarisi](media/contoso-migration-rehost-vm-sql-ag/architecture.png) 

### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Veritabanı yönetim hizmeti](https://docs.microsoft.com/azure/dms/dms-overview) | Contoso DMS kullanarak uygulama veri katmanını geçirirsiniz. DMS bir siteden siteye VPN üzerinden şirket içi SQLVM makineye bağlanın ve en az kapalı kalma süresi ile Azure veri platformlar için birden fazla veritabanı kaynaktan DMS etkinleştirir sorunsuz geçiş geçirilir. | Hakkında bilgi edinin [bölgeler desteklenen](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) DMS ve get için [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/database-migration/).
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Contoso uygulaması ön uç VM yükseltme shift geçiş için Site Recovery kullanır. Site Recovery düzenler ve geçiş ve olağanüstü durum kurtarma Azure VM'ler için yönetir ve sanal makineleri ve fiziksel sunucuları şirket.  | Azure'a çoğaltma sırasında Azure depolama ücretleri ücrete.  Azure VM'ler oluşturulur ve yük devretme oluştuğunda ücretler, uygulanır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/) ücretleri ve fiyatlandırma hakkında.

 

## <a name="migration-process"></a>geçiş işlemi

- Contoso için Azure app sanal makineleri geçirir.
- Bunlar, Azure Site Recovery kullanarak VM için ön uç VM geçirme:
    - İlk adım olarak, bunlar hazırlamak ve Azure bileşenleri ayarlamak ve şirket içi VMware altyapı hazırlayın.
    - Olan her şeyi hazır, bunlar VM çoğaltma başlatabilirsiniz.
    - Çoğaltma etkin ve çalışıyor, bunlar VM'yi Azure'a devrederek tarafından geçirmek sonra.
- Veri Taşıma hizmeti (DMS) kullanarak azure'da bir SQL Server kümesi bunlar veritabanı geçiş.
    - İlk adım olarak ihtiyaç duydukları Azure, SQL Server Vm'lerinin sağlamak için kümenin ve bir iç yük dengeleyicisi ayarlayın ve AlwaysOn Kullanılabilirlik grupları yapılandırın.
    - Bu yerinde bunlar veritabanını geçirme
- Geçişten sonra bunlar veritabanı için AlwaysOn korumasını etkinleştirin.

![geçiş işlemi](media/contoso-migration-rehost-vm-sql-ag/migration-process.png) 
 
## <a name="prerequisites"></a>Önkoşullar

(Ve Contoso) Bu senaryo çalıştırmak gerekenleri aşağıda verilmiştir:

**Gereksinimleri** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Bu serideki ilk makaledeki değerlendirme gerçekleştirildiğinde, zaten bir abonelik oluşturmuş olmalıdır. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Var olan bir abonelik kullanın ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yönetici ile çalışmak için gerekir.<br/><br/> Daha ayrıntılı izinler gerekiyorsa, gözden [bu makalede](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure altyapı** | [Bilgi nasıl](contoso-migration-infrastructure.md) Contoso Azure altyapıyı ayarlayın.<br/><br/> Özel hakkında daha fazla bilgi [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) Site Recovery için gereksinimleri.
**Site Kurtarma (şirket içi)** | Şirket içi vCenter server 5.5, 6.0 veya 6.5 sürümü çalıştırması gerekir<br/><br/> 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir ESXi ana bilgisayar<br/><br/> Bir veya daha fazla VMware ESXi ana bilgisayarında çalışan sanal makineleri.<br/><br/> Sanal makineleri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Desteklenen [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) yapılandırma.<br/><br/> Çoğaltmak istediğiniz sanal makineleri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).
**DMS** | DMS için gereksinim duyduğunuz bir [içi uyumlu VPN cihazı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Şirket içi VPN cihazı yapılandırma olması gerekir. Dışarıya dönük bir genel IPv4 adresi olmalıdır ve bir NAT cihazının arkasında adresi bulunamıyor.<br/><br/> Şirket içi SQL Server veritabanına erişiminiz olduğundan emin olun.<br/><br/> Windows Güvenlik Duvarı'nı kaynak veritabanı altyapısı erişebilir olması gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Veritabanını makinenize önünde bir güvenlik duvarı varsa, veritabanını ve dosyalarına SMB bağlantı noktası 445 üzerinden erişime izin vermek için kurallar ekleyin.<br/><br/> Yönetilen örneğini hedeflemek ve SQL Server kaynağına bağlanmak için kullanılan kimlik bilgileri sysadmin sunucu rolünün üyeleri olmalıdır.<br/><br/> Bir ağ gerekir paylaşmak şirket içi veritabanınızda DMS kaynak veritabanını yedeklemek için kullanabilirsiniz.<br/><br/> Kaynak SQL Server örneğinin çalıştığı hizmet hesabının ağ paylaşımına yazma ayrıcalıklarına sahip olduğundan emin olun.<br/><br/> Ağ paylaşımında tam denetim ayrıcalığına sahip bir Windows kullanıcısı (ve parola) not edin. Azure veritabanı geçiş hizmeti Azure storage kapsayıcısına yedekleme dosyalarını karşıya yüklemek için bu kullanıcı kimlik bilgileri temsil eder.<br/><br/> TCP/IP Protokolü SQL Server Express yükleme işlemi ayarlar **devre dışı** varsayılan olarak. Etkin olduğundan emin olun.


## <a name="scenario-steps"></a>Senaryo adımları

Contoso geçiş nasıl çalışacağını aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure SQL Server Vm'lerinin oluşturma**: Contoso yüksek kullanılabilirlik için istediğiniz Azure kümelenmiş bir veritabanında dağıtılacak. Bunlar, iki SQL Server VM'ler ve Azure iç yük dengeleyiciye dağıtın.
> * **Adım 2: küme dağıtma**: SQL Server Vm'lerinin dağıttıktan sonra bunlar bir Azure SQL Server kümesi hazırlayın.  Bunlar kendi veritabanı önceden oluşturulmuş bu kümesine geçirmeniz.
> * **3. adım: DMS hazırlama**: Bunlar kaydetmek DMS veritabanı geçiş sağlayıcısı hazırlamak için DMS örneği ve bir proje oluşturun. Bunlar paylaşılan erişim imzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) ayarlayın. DMS SA URI hizmeti SQL Server Yedekleme dosyalarını karşıya yükleme depolama hesabı kapsayıcısının erişmek için kullanır.
> * **4. adım: Azure Site Recovery için hazırlama**: Çoğaltılan verileri ve bir kurtarma Hizmetleri kasası tutmak için bir Azure depolama hesabı oluşturun.
> * **5. adım: şirket içi VMware Site Recovery için hazırlama**: hesapları VM Keşif ve aracı yüklemesine hazırlamak ve yük devretme sonrasında Azure Vm'lerinin bağlanabilmesi için şirket içi sanal makineleri hazırlayın.
> * **6. adım: Çoğaltma sanal makineleri**: çoğaltma ayarlarını yapılandırın ve VM çoğaltmayı etkinleştirin.
> * **7. adım: DMS veritabanıyla geçirmek**: veritabanını geçirme.
> * **8. adım: Site Recovery ile sanal makineleri geçirmek**: Bunlar önce her şeyi çalıştığından, VM geçirmek için tam bir yük devretme tarafından izlenen emin olmak için yük devretme testi çalıştırın.


## <a name="step-1-prepare-a-sql-server-alwayson-availability-group-cluster"></a>1. adım: bir SQL Server AlwaysOn Kullanılabilirlik grubu küme hazırlama

Contoso Azure kümesinde veritabanında dağıtarak İleriyi istiyor. Bunlar daha sonra bu kümedeki diğer veritabanları için kullanabilirsiniz. 

- SQL Server Vm'lerinin ContosoRG kaynak grubunda (üretim kaynakları için kullanılır) dağıtılır.
- Benzersiz adlar dışında her iki VM aynı ayarları kullanır.
- İç yük dengeleyicisi ContosoNetworkingRG (ağ kaynakları için kullanılır) dağıtılır.
- Sanal makineleri Windows Server 2016 SQL Server 2017 Enterprise Edition ile çalışır. Kendi Azure EA taahhüt için bir ücret olarak lisans sağlar Azure markette görüntüyü kullanacağınız şekilde Contoso bu işletim sistemi lisansına sahip değil.

İşte nasıl Contoso kümesi:

1. Bunlar, iki SQL Server Vm'lerinin Azure Marketi'nde SQL Server 2017 kuruluş Windows Server 2016 görüntü seçerek oluşturur. 

    ![SQL VM SKU](media/contoso-migration-rehost-vm-sql-ag/sql-vm-sku.png)

2. İçinde **sanal makine Sihirbazı oluşturma** > **Temelleri**, bunlar yapılandırın:

    - Sanal makineleri için adları: **SQLAOG1** ve **SQLAOG2**.
    - Makineler iş açısından önemli olduğundan, VM disk türünü SSD etkinleştirin.
    - Bunlar, makine kimlik bilgileri belirtin.
    - Birincil Doğu ABD 2 sanal makineleri dağıtmak bölgede ContosoRG kaynak grubu.

3. İçinde **boyutu**, bunlar için her iki VM D2s_V3 SKU ile başlatın. Bunlar için gereksinim duydukları gibi daha sonra ölçeklendirme.
4. İçinde **ayarları**, aşağıdakileri yapın:

    - Bu sanal makineleri uygulama için kritik veritabanları olduğundan, bunlar yönetilen diskleri kullanın.
    - Bunlar makineleri Doğu ABD 2 üretim ağında birincil yerleştirmek bölge (**VNET üretim EUS2**), veritabanı alt (**üretim DB EUS2**).
    - Yeni bir kullanılabilirlik kümesi oluştur: **SQLAOGAVSET**iki hata etki alanları ve beş güncelleştirme etki alanları.

    ![SQL VM](media/contoso-migration-rehost-vm-sql-ag/sql-vm-settings.png)

4. İçinde **SQL Server ayarları**, bunlar SQL Bağlantısı (özel), sanal ağ için varsayılan bağlantı noktası 1433 sınırlayın. Yerinde kullandıkları kimlik doğrulaması için aynı kimlik bilgilerini kullanırlar (**contosoadmin**).

    ![SQL VM](media/contoso-migration-rehost-vm-sql-ag/sql-vm-db.png)

**Daha fazla yardım gerekiyor mu?**

- [Yardım](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision#1-configure-basic-settings) bir SQL Server VM sağlama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-prereq#create-sql-server-vms) VM'ler farklı SQL Server için SKU'ları yapılandırma.

## <a name="step-2-deploy-the-failover-cluster"></a>2. adım: yük devretme kümesi dağıtma

İşte nasıl Contoso kümesi:

1. Bunlar, bir Azure depolama hesabı bulut tanığı olarak davranacak şekilde ayarlayın.
2. Bunlar, SQL Server Vm'lerinin Contoso şirket içi veri merkezinde Active Directory etki alanına ekleyin.
3. Bunlar, Azure'da küme oluşturur.
4. Bunlar, bulut Tanık yapılandırın.
5. Son olarak, SQL Always On kullanılabilirlik gruplarını etkinleştirin.

### <a name="set-up-a-storage-account-as-cloud-witness"></a>Bir depolama hesabı bulut tanığı olarak ayarlama

Bulut tanığı ayarlamak için küme yönetimi için kullanılan blob dosya tutacak bir Azure Storage hesabı Contoso gerekir. Aynı depolama hesabı, birden fazla küme için bulut tanığı ayarlamak için kullanılabilir. 

Contoso aşağıdaki gibi bir depolama hesabı oluşturur:

1. Hesap için tanınabilir bir ad belirtin (**contosocloudwitness**).
2. Bunlar LRS ile genel çok amaçlı hesap dağıtın.
3. Bunlar, bir üçüncü bölgede - Orta Güney ABD hesap yerleştirin. Bölgesel başarısız olması durumunda kullanılabilir olmaya devam böylece bunlar, birincil ve ikincil bölgesinin dışındaki yerleştirin.
4. Bunlar altyapı kaynaklar - tutan kendi kaynak grubuna yerleştirin **ContosoInfraRG**.

    ![Bulut tanığı](media/contoso-migration-rehost-vm-sql-ag/witness-storage.png)

5. Ne zaman birincil depolama hesabı oluşturun ve ikincil erişim anahtarları için oluşturulur. Bulut Tanık oluşturmak için birincil erişim anahtarını ihtiyaç duyar. Anahtar depolama hesap adı altında görünür > **erişim tuşları**.

    ![Erişim anahtarı](media/contoso-migration-rehost-vm-sql-ag/access-key.png)

### <a name="add-sql-server-vms-to-contoso-domain"></a>SQL Server Vm'lerinin Contoso etki alanına ekleme

1. Contoso SQLAOG1 ve SQLAOG2 contoso.com etki alanına ekler.
2. Ardından, her VM bunlar Windows Yük devretme kümesi özelliğini ve araçlarını yükler.

## <a name="set-up-the-cluster"></a>Küme ayarlama

Kümeyi ayarlamadan önce Contoso her makinede işletim sistemi diski, bir anlık görüntüsünü alır.

![Anlık görüntü](media/contoso-migration-rehost-vm-sql-ag/snapshot.png)

2. Daha sonra bunlar koyarlar bir betik çalıştırın birlikte Windows Yük devretme kümesi oluşturun.

    ![Küme oluşturma](media/contoso-migration-rehost-vm-sql-ag/create-cluster1.png)

3. Küme oluşturduktan sonra sanal makineleri küme düğümleri olarak göründüğünü doğrulayın.

     ![Küme oluşturma](media/contoso-migration-rehost-vm-sql-ag/create-cluster2.png)

## <a name="configure-the-cloud-witness"></a>Bulut tanığı yapılandırma

1. Contoso kullanarak bulut tanığı yapılandırma **çekirdek Yapılandırma Sihirbazı'nı** yük devretme kümesi Yöneticisi'nde.
2. Sihirbazda bunlar bir bulut tanığı ile depolama hesabı oluşturmak için seçin.
3. Bulut Tanık yapılandırıldıktan sonra Yük Devretme Kümesi Yöneticisi ek bileşeninde görüntülenir.

    ![Bulut tanığı](media/contoso-migration-rehost-vm-sql-ag/cloud-witness.png)
            

## <a name="enable-sql-server-always-on-availability-groups"></a>SQL Server Always On kullanılabilirlik gruplarını etkinleştir

Contoso, şimdi her zaman açık etkinleştirebilirsiniz:

1. SQL Server Yapılandırma Yöneticisi'nde bunlar etkinleştirmek **AlwaysOn Kullanılabilirlik grupları** için **SQL Server (MSSQLSERVER)** hizmet.

    ![AlwaysOn etkinleştirin](media/contoso-migration-rehost-vm-sql-ag/enable-alwayson.png)

2. Bunlar, değişikliklerin etkili olması hizmeti yeniden başlatın.

AlwaysOn etkinleştirme ile Contoso SmartHotel veritabanını koruyacak AlwaysOn Kullanılabilirlik grubu ayarlayabilirsiniz.

**Daha fazla yardım gerekiyor mu?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness) bulut Tanık ve bir depolama hesabı için ayarlama.
- [Yönergeleri almak](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial) bir kümeyi ayarlamadan ve bir kullanılabilirlik grubu oluşturma.

## <a name="step-3-deploy-the-azure-load-balancer"></a>3. adım: Azure yük dengeleyici dağıtma

Contoso şimdi istediğiniz küme düğümleri önünde bulunur bir iç yük dengeleyici dağıtın. Yük Dengeleyici trafiğini dinlemediğinden ve uygun düğüme yeniden yönlendirir.

![Yük dengeleme](media/contoso-migration-rehost-vm-sql-ag/architecture-lb.png)

Bunlar yük dengeleyici gibi oluşturun:

1. Azure portalında > **ağ** > **yük dengeleyici**, yeni bir iç yük dengeleyicisi oluşturun ayarlayın: **ILB-üretim-DB-EUS2-SQLAOG**.
2. Bunlar üretim ağında yük dengeleyici yerleştirin **VNET üretim EUS2**, veritabanı alt **üretim DB EUS2**.
3. Bunlar bir statik IP adresi atayın: 10.245.40.100.
4. Bir ağ öğesi olarak, yük dengeleyici ağ kaynak grubunda dağıtmak **ContosoNetworkingRG**.

    ![Yük dengeleme](media/contoso-migration-rehost-vm-sql-ag/lb-create.png)

İç yük dengeleyicisi dağıtıldıktan sonra Contoso, yapılandırmanız. Bunlar bir arka uç adres havuzu oluşturma, bir sistem durumu araştırması ayarlayabilir ve Yük Dengeleme kuralını yapılandırın.

### <a name="add-a-backend-pool"></a>Arka uç havuzu ekleme

Kümedeki sanal makineleri için trafiği dağıtmak için ağ trafiğini yük dengeleyiciden alacak olan VM'ler için NIC IP adreslerini içeren bir arka uç adres havuzu Contoso ayarlayın.

1. Portalda yük dengeleyici ayarları Contoso bir arka uç havuzu ekleyin: **ILB-PROD-DB-EUS-SQLAOG-BEPOOL**.
2. Bunlar havuzu SQLAOGAVSET kullanılabilirlik kümesi ile ilişkilendirin. Kümedeki sanal makineleri (**SQLAOG1** ve **SQLAOG2**) havuzuna eklenir.

    ![Arka uç havuzu](media/contoso-migration-rehost-vm-sql-ag/backend-pool.png)

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Contoso bir sistem durumu araştırması oluşturur, böylece yük dengeleyici uygulama sistem durumu izleyebilirsiniz. Araştırma dinamik olarak ekler veya sistem durumu denetimlerinin vereceği olmadığını üzerinde temel bir yük dengeleyici döndürme VM'ler kaldırır.

Bunlar araştırma gibi oluşturun: 

1. Portalda yük dengeleyici ayarları Contoso bir sistem durumu araştırması oluşturur: **SQLAlwaysOnEndPointProbe**.
2. Bunlar, TCP bağlantı noktası 59999 Vm'lerinde izlemek için araştırma ayarlayın.
3. Bunlar araştırmalar ve 2 eşiğinin arasındaki 5 saniye aralığını ayarlayın. İki araştırmalar başarısız olursa VM sağlıksız kabul edilir.

    ![Araştırma](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

### <a name="configure-the-load-balancer-to-receive-traffic"></a>Trafiği almak için yük dengeleyici yapılandırma


Yük Dengeleyici kuralı için trafiği Vm'lere nasıl dağıtıldığını defins şimdi, Contoso gerekiyor.

- Ön uç IP adresi gelen trafiği işler.
- Arka uç IP havuzu trafik alır.

Bunlar aşağıdaki gibi bir kural oluşturun:

1. Yeni bir Yük Dengeleme kuralı ekledikleri portalında yük dengeleyici ayarları: **SQLAlwaysOnEndPointListener**.
2. Contoso TCP 1433 gelen SQL istemci trafiğini almak için bir ön uç dinleyici ayarlar.
3. Bunlar arka uç havuzuna hangi trafik yönlendirilir ve trafiği için sanal makineleri üzerinde dinleme bağlantı noktası belirtin.
4. Contoso kayan IP (doğrudan sunucu dönüşü) sağlar. Bu her zaman SQL AlwaysOn için gereklidir.

    ![Araştırma](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

**Daha fazla yardım gerekiyor mu?**

- [Genel bir bakış elde](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) Azure yük dengeleyici.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-basic-internal-portal) bir yük dengeleyici oluşturma.



## <a name="step-4-prepare-azure-for-the-site-recovery-service"></a>4. adım: Azure Site Recovery hizmeti için hazırlama

Contoso Site Recovery dağıtmak için gereken Azure bileşenleri şunlardır:

- Bir sanal ağ yük devretme sırasında oluştururken, VM'ler bulunur.
- Çoğaltılan verileri tutmak için bir Azure depolama hesabı. 
- Azure kurtarma Hizmetleri kasasına.

Bunlar bu aşağıdaki gibi ayarlayın:

1.  Site Recovery için kullanabilecekleri bir ağ/alt önceden oluşturulmuş Contoso olduğunda bunlar [Azure altyapı dağıtılan](contoso-migration-rehost-vm-sql-ag.md).

    - Bir üretim uygulaması SmartHotel uygulamasıdır ve WEBVM birincil Doğu US2 bölgede Azure üretim ağa (VNET-üretim-EUS2) geçirilecektir.
    - WEBVM üretim kaynakları için kullanılır, ContosoRG kaynak grubunda ve üretim alt ağ (üretim-FE-EUS2) yerleştirilir.

2. Contoso birincil bölgede bir Azure depolama hesabı (contosovmsacc20180528) oluşturur.

    - Standart depolama ve LRS çoğaltma genel amaçlı bir hesap ile kullanırlar.
    - Hesabın, kasa ile aynı bölgede olması gerekir.

    ![Site Kurtarma depolama alanı](media/contoso-migration-rehost-vm-sql-ag/asr-storage.png)

3. Yerinde ağ ve depolama hesabıyla bunlar şimdi bir kurtarma Hizmetleri kasası oluşturma (**ContosoMigrationVault**) ve yerleştirileceği **ContosoFailoverRG** kaynak grubunda birincil Doğu ABD 2 bölge .

    ![Kurtarma Hizmetleri kasası](media/contoso-migration-rehost-vm-sql-ag/asr-vault.png)

**Daha fazla yardım gerekiyor mu?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Azure Site Recovery için ayarlama.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>4. adım: şirket içi VMware Site Recovery için hazırlama

İşte Contoso şirket içi hazırlar:

- VM bulma otomatikleştirmek için vCenter sunucusu veya vSphere ESXi ana bilgisayarında, bir hesap.
- Çoğaltmak istediğiniz VMware Vm'lerinde Mobility hizmeti otomatik olarak yüklenmesini sağlayan bir hesap.
- VM ayarları, Contoso yük devretme sonrasında çoğaltılmış Azure VM'ye bağlanabilmesi için şirket içi.


### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- Sanal makineleri otomatik olarak bulur. 
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme.
- En az bir salt okunur hesap gereklidir. Oluşturma ve diskleri kaldırma ve sanal makine kapatma gibi işlemleri çalıştırılan bir hesap gerekir.

Contoso hesabı gibi ayarlar:

1. Contoso vCenter düzeyinde bir rol oluşturur.
2. Contoso, daha sonra bu rol gerekli izinleri atar.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Her VM mobilite hizmetinin yüklenmesi gerekir.

- Çoğaltma VM için etkinleştirildiğinde, site kurtarma Bu bileşen otomatik itme yüklemesi yapabilirsiniz.
- Site Recovery göndermeli yüklemesi için VM erişmek için kullanabileceğiniz bir hesap gerekir. Azure konsolunda çoğaltma ayarladığınızda bu hesabı belirtin.
- Hesap etki alanı veya yerel VM yüklemek için gerekli izinlere sahip olabilir.




### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Yük devretme sonrasında, Contoso Azure Vm'lerine bağlanmak istiyor. Bunu yapmak için geçiş işleminden önce aşağıdakileri yapın:

1. Internet üzerinden erişim için bunlar:

 - Yük devretmeden önce şirket içi VM üzerinde RDP etkinleştir
 - İçin TCP ve UDP kurallarının eklendiğinden emin olun **ortak** profili.
 - RDP izin onay **Windows Güvenlik Duvarı** > **izin verilen uygulamaları** tüm profiller için.
 
2. Siteden siteye VPN üzerinden erişim için bunlar:

 - RDP şirket içi makinede etkinleştirin.
 - RDP izin **Windows Güvenlik Duvarı** -> **verilen uygulamalar ve Özellikler**, için **etki alanı ve özel** ağlar.
 - Şirket içi VM için işletim sisteminin SAN ilkesinin Ayarla **OnlineAll**.

Ayrıca, bir yük devretme çalıştırdığınızda, bunlar aşağıdakileri denetleyin gerekir:

- Olmamalıdır bekleyen hiçbir Windows güncelleştirmeleri VM üzerinde bir yük devretme tetiklendiğinde. Varsa, güncelleştirme tamamlanana kadar VM'de oturum oturum değiştiremezler.
- Yük devretme sonrasında, bunlar denetleyebilirsiniz **önyükleme tanılama** ekran VM görüntüsünü görüntülemek için. Bu işe yaramazsa, bunlar VM çalıştıran ve bunlar gözden olduğunu doğrulamalısınız [sorun giderme ipuçları](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Daha fazla yardım gerekiyor mu?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) oluşturma ve Otomatik bulma için bir rol atama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) mobilite hizmetinin göndermeli yüklemesi için bir hesap oluşturma.


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>5. adım: Azure Site Recovery ile şirket içi Vm'lerini çoğaltma

Bir geçiş için Azure çalıştırmadan önce iş Contoso ayarlamak ve çoğaltmayı etkinleştirmek gerekir.

### <a name="set-a-replication-goal"></a>Bir çoğaltma hedefi ayarlama

1. Kasada kasa adı (ContosoVMVault) altında çoğaltma hedefi seçin (**Başlarken** > **Site Recovery** > **altyapıyı hazırlama** .
2. Bunlar, kendi makinelerine bulunduğu VMware üzerinden çalışan ve Azure'a çoğaltma şirket olduğunu belirtin.

    ![Çoğaltma hedefi](./media/contoso-migration-rehost-vm-sql-ag/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Dağıtım Planlama onaylayın

Devam etmek için bunlar seçerek dağıtım planlamasını tamamladınız onaylamak ihtiyaç duydukları **Evet, yaptım**. Bu senaryoda Contoso yalnızca bir VM geçirme olur ve dağıtım planlama olması gerekmez.

### <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Contoso kaynak ortamlarına yapılandırmak gerekir. Bunu yapmak için bunlar bir OVF şablonunu indirebilir ve Site Recovery yapılandırma sunucusu yüksek oranda kullanılabilir olarak dağıtmak için kullanın, şirket içi VMware VM. Yapılandırma sunucunun da çalışır durumda sonra bunlar bu kasaya kaydedin.

Yapılandırma sunucusu bir dizi bileşen çalıştırır:

- Şirket içi ve Azure arasındaki iletişimi düzenler ve veri çoğaltma yöneten yapılandırma sunucusu bileşeni.
- Bir çoğaltma ağ geçidi olarak davranır işlem sunucusu. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir.
- İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.

Contoso adımları aşağıdaki gibi gerçekleştirin:


1. OVF şablondan indirdikleri kasasına **altyapıyı hazırlama** > **kaynak** > **yapılandırma sunucusu**.
    
    ![OVF indirin](./media/contoso-migration-rehost-vm-sql-ag/add-cs.png)

2. Bunlar, oluşturma ve VM dağıtmak için VMware şablonu içeri aktarın.

    ![OVF şablonu](./media/contoso-migration-rehost-vm-sql-ag/vcenter-wizard.png)

3. VM üzerinde ilk kez açtığınızda, Windows Server 2016 yükleme deneyimi ön. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
4. Yükleme tamamlandıktan sonra bunların VM'ye yönetici olarak oturum açın. İlk oturum açma sırasında varsayılan olarak Azure Site kurtarma Yapılandırması aracını çalıştırır.
5. Aracı, bunlar yapılandırma sunucusunu kasaya kaydetmek için kullanılacak bir ad belirtin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra kullanıcılar Azure aboneliği ile oturum açın. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.

    ![Yapılandırma sunucusu kaydetme](./media/contoso-migration-rehost-vm-sql-ag/config-server-register2.png)

7. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır.
8. Kullanıcılar için makine yeniden oturum açın ve yapılandırma sunucusu yönetim Sihirbazı'nı otomatik olarak başlatır.
9. Sihirbazda, çoğaltma trafiği almaya NIC seçerler. Bunu yapılandırıldıktan sonra bu ayar değiştirilemez.
10. Bunlar, abonelik, kaynak grubu ve yapılandırma sunucunun kaydedileceği kasayı seçin.
        ![Kasa](./media/contoso-migration-rehost-vm-sql-ag/cswiz1.png) 

10. Bunlar daha sonra indirin ve MySQL Server ve VMWare Powerclı yükleyin. 
11. Doğrulama sonrasında, bunlar vCenter sunucusu veya vSphere ana bilgisayar FQDN veya IP adresini belirtin. Bunlar varsayılan bağlantı noktası bırakın ve vCenter sunucusu için bir kolay ad belirtin.
12. Bunlar, otomatik bulma için oluşturduğunuz hesabı ve Mobility hizmetinin otomatik olarak yüklemek için kullanılan kimlik bilgilerini belirtin. Windows makineler için hesap VM'ler üzerinde yerel yönetici ayrıcalıkları gerekiyor.

    ![vCenter](./media/contoso-migration-rehost-vm-sql-ag/cswiz2.png)

7. Kayıt tamamlandığında Azure portalında Contoso çift VMware sunucusu ve yapılandırma sunucusu listelendiğini denetler **kaynak** kasası sayfasında. Bulma, 15 dakika veya daha fazla sürebilir. 
8. Site Recovery belirtilen ayarları kullanarak VMware sunucularına bağlanır ve sanal makineleri bulur.

### <a name="set-up-the-target"></a>Hedefi ayarlama

Şimdi Contoso hedef çoğaltma ayarlarını belirtir.

1. İçinde **altyapıyı hazırlama** > **hedef**, hedef ayarlarını seçin.
2. Site Recovery, bir Azure depolama hesabı ve belirtilen hedef ağında olup olmadığını denetler.

### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Hayır, Contoso bir çoğaltma ilkesi oluşturabilirsiniz.

1. İçinde **altyapıyı hazırlama** > **çoğaltma ayarları** > **Çoğaltma İlkesi** >  **oluşturma ve İlişkilendirme**, bir ilke oluşturmak **ContosoMigrationPolicy**.
2. Bunlar varsayılan ayarları kullanın:
    - **RPO eşiği**: varsayılan 60 dakika. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
    - **Kurtarma noktası bekletme**. Varsayılan 24 saatlik. Bu değer ne kadar bekletme penceresi için her kurtarma noktası belirtir. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**. Varsayılan bir saat. Bu değer oluşturulmasında ve uygulamayla tutarlı anlık görüntü sıklığı belirtir.
 
        ![Çoğaltma ilkesi oluşturma](./media/contoso-migration-rehost-vm-sql-ag/replication-policy.png)

5. İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. 

    ![Çoğaltma İlkesi ilişkilendirme](./media/contoso-migration-rehost-vm-sql-ag/replication-policy2.png)



### <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Şimdi Contoso WebVM çoğaltma başlatabilirsiniz.

1. İçinde **uygulama çoğaltma** > **kaynak** > **+ Çoğalt** kaynak ayarlarını seçin.
2. Bunlar, bunlar, sanal makineleri etkinleştirmek istiyorsanız, vCenter sunucusu ve yapılandırma sunucusu seçin gösterir.

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm-sql-ag/enable-replication1.png)

3. Şimdi, kaynak grubu ve VNet ve çoğaltılan veriler depolanacağı depolama hesabı da dahil olmak üzere hedef ayarları belirtin.

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm-sql-ag/enable-replication2.png)

3. Contoso çoğaltma WebVM seçer, çoğaltma ilkesi denetler ve çoğaltmayı etkinleştirir. Çoğaltma etkinleştirilmişse, site kurtarma VM Mobility hizmetini yükler.
 
    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm-sql-ag/enable-replication3.png)

4. Çoğaltma sürüyor izlemek **işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.
5. İçinde **Essentials** Azure portalında, Azure'a çoğaltma VM'ler için yapısı Contoso görebilirsiniz.

    ![Altyapı görünümü](./media/contoso-migration-rehost-vm-sql-ag/essentials.png)


**Daha fazla yardım gerekiyor mu?**

- Bu adımları tam bir kılavuz okuyabilirsiniz [şirket içi VMware Vm'leri için olağanüstü durum kurtarma kümesi](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Ayrıntılı yönergeler size yardımcı olmak için kullanılabilir [kaynak ortamını ayarlama](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [yapılandırma sunucusu dağıtmak](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), ve [çoğaltma ayarlarını yapılandırın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- Hakkında daha fazla bilgiyi [çoğaltma etkinleştirme](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-install-the-database-migration-assistant-dma"></a>5. adım: veritabanı geçiş Yardımcısı (DMA) yükleme

Contoso Azure VM'ye SmartHotel veritabanı geçirme **SQLAOG1** DMA kullanma. Bunlar DMA aşağıdaki gibi ayarlayın:

1. Aracı indirmek [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) şirket içi SQL Server VM (**SQLVM**).
2. Bunlar, VM kurulumu (DownloadMigrationAssistant.msi) çalıştırın.
3. Üzerinde **son** sayfasında, seçtikleri **başlatma Microsoft veri geçiş Yardımcısı** Sihirbazı tamamlamadan önce.

## <a name="step-6-migrate-the-database-with-dma"></a>6. adım: DMA veritabanıyla geçirme

1. Yeni bir geçiş - çalıştırdıkları DMA **SmartHotel**.
2. Seçtikleri **hedef sunucu türü** olarak **Azure Virtual Machines'de SQL Server**. 

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-1.png)

3. Geçiş ayrıntılarında ekledikleri **SQLVM** kaynak sunucu ve **SQLAOG1** hedefi olarak. Bunlar, her makine için kimlik bilgilerini belirtin.

     ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-2.png)

4. Bunlar, veritabanı ve yapılandırma bilgileri için yerel bir paylaşım oluşturun. Yazma erişimi SQLVM ve SQLAOG1 SQL Hizmeti hesabı tarafından erişilebilir olması gerekir.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-3.png)

5. Contoso geçirilmelidir ve geçişi başlatan oturumların seçer. Sona erdikten sonra DMA geçiş başarılı olarak gösterilir.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-4.png)

6. Veritabanını çalışır durumda olduğunu doğrulama **SQLAOG1**.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-5.png)

DMS şirket içi SQL Server VM Contoso veri merkezi ve Azure arasında bir siteden siteye VPN bağlantısı üzerinden bağlanır ve veritabanı geçirir.

## <a name="step-7-protect-the-database"></a>7. adım: veritabanını koruma

Üzerinde çalışan uygulama veritabanıyla **SQLAOG1**, Contoso şimdi Koruyabileceğiniz AlwaysOn Kullanılabilirlik grupları kullanarak. Bunlar AlwaysOn SQL Management Studio'yu kullanarak yapılandırın ve ardından Windows Kümeleme kullanarak bir dinleyici atayın. 

### <a name="create-an-alwayson-availability-group"></a>AlwaysOn Kullanılabilirlik grubu oluşturma

1. SQL Management Studio'da bunlar üzerinde sağ **yüksek kullanılabilirlik her zaman** başlatmak için **yeni Kullanılabilirlik Grubu Sihirbazı'nı**.
2. İçinde **seçeneklerini belirtin**, kullanılabilirlik grubu adı **SHAOG**. İçinde **seçin veritabanları**, SmartHotel veritabanını seçin.

    ![AlwaysOn Kullanılabilirlik grubu](media/contoso-migration-rehost-vm-sql-ag/aog-1.png)

3. İçinde **çoğaltmaları belirle**, kullanılabilirlik çoğaltmaları olarak iki SQL düğümleri eklemek ve bunları zaman uyumlu işleme ile otomatik yük devretme sağlamak üzere yapılandırın.

     ![AlwaysOn Kullanılabilirlik grubu](media/contoso-migration-rehost-vm-sql-ag/aog-2.png)

4. Grup için bir dinleyici yapılandırın (**SHAOG**) ve bağlantı noktası. İç yük dengeleyicisi IP adresini statik IP adresi (10.245.40.100) eklenir.

    ![AlwaysOn Kullanılabilirlik grubu](media/contoso-migration-rehost-vm-sql-ag/aog-3.png)

5. İçinde **veri eşitleme Seç**, bunlar otomatik dengeli dağıtımı sağlar. Contoso değilsiniz şekilde el ile yedekleme ve bu geri yükleme bu seçenekle, SQL Server otomatik olarak her veritabanı için ikincil çoğaltmaları grubunda oluşturur. Doğrulama sonrasında, kullanılabilirlik grubu oluşturulur.

    ![AlwaysOn Kullanılabilirlik grubu](media/contoso-migration-rehost-vm-sql-ag/aog-4.png)

6. Contoso grubunu oluştururken bir sorun verdi. Bunlar Active Directory Windows tümleşik güvenliği kullanmadığınız ve bu nedenle Windows Yük devretme kümesi rolleri oluşturmak için SQL oturum açma izinleri gerekir.

    ![AlwaysOn Kullanılabilirlik grubu](media/contoso-migration-rehost-vm-sql-ag/aog-5.png)

6. Contoso grubu oluşturulduktan sonra SQL Management Studio'da görebilirsiniz.

### <a name="configure-a-listener-on-the-cluster"></a>Kümede bir dinleyici yapılandırın

SQL dağıtımı ayarlama son adımda olarak Contoso iç yük dengeleyicisi küme üzerinde dinleyici olarak yapılandırır ve dinleyici çevrimiçi duruma getirir. Bunu yapmak için bir komut dosyası kullanırlar.

![Küme dinleyicisi](media/contoso-migration-rehost-vm-sql-ag/cluster-listener.png)


### <a name="verify-the-configuration"></a>yapılandırıldığını doğrulayın

Olan her şeyi ayarlamak, Contoso artık var. işlev kullanılabilirlik grubu geçirilen veritabanı kullanan Azure Bunlar SQL Management Studio'da iç yük dengeleyicisi bağlanarak doğrulayın.

![ILB Bağlan](media/contoso-migration-rehost-vm-sql-ag/ilb-connect.png)

**Daha fazla yardım gerekiyor mu?**
- Oluşturma hakkında bilgi edinin bir [kullanılabilirlik grubu](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#create-the-availability-group) ve [dinleyici](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#configure-listener).
- El ile [yük dengeleyici IP adresi kullanmak için kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#configure-the-cluster-to-use-the-load-balancer-ip-address).
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-2) oluşturma ve SAS kullanma hakkında.


## <a name="step-8-migrate-the-vm-with-site-recovery"></a>8. adım: Site Recovery ile VM geçirme

Bir hızlı çalıştırmak Contoso yük devretme sınamasını ve ardından VM geçirin.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Yardımcı olur, yük devretme testi çalıştıran her şeyi geçiş işleminden önce beklendiği gibi çalıştığından emin olun. 

1. Contoso çalıştıran yük devretme sınaması için son noktası zamanında (**son işlenen**).
2. Seçtikleri **yük devretme işlemine başlamadan önce makineyi kapatın**, böylece yük devretme tetiklemeden önce kaynak VM kapatmak Site Recovery çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. 
3. Sınama yük devretme çalıştırır: 

    - Tüm geçiş için gereken koşulların karşılandığından emin olmak için çalışır bir ön koşullarını denetleyin.
    - Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
    - Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.

3. Yük devretme işlemi tamamlandıktan sonra çoğaltma Azure portalında Azure VM görüntülenir. Contoso VM sağ ağa bağlı uygun boyutta olduğundan ve çalıştığından emin denetler. 
4. Doğruladıktan sonra Contoso yük devretmeyi, temizleme ve kayıt ve gözlemlerinizi kaydetmek. 

### <a name="run-a-failover"></a>Yük devretme çalıştırma

1. Contoso, yük devretme sınaması beklenen şekilde çalıştığını doğruladıktan sonra geçiş için bir kurtarma planı oluşturmak ve plana WEBVM ekleyin.

     ![Kurtarma planı](./media/contoso-migration-rehost-vm-sql-ag/recovery-plan.png)

2. Bunlar plan üzerinde bir yük devretme çalıştırın. Bunların en son kurtarma noktası seçin ve Site Recovery yük devretme tetiklemeden önce şirket içi VM Kapatma denemesi gerektiğini belirtin.

    ![Yük devretme](./media/contoso-migration-rehost-vm-sql-ag/failover1.png)

3. Yük devretme sonrasında, bunlar Azure VM Azure portalında beklendiği gibi göründüğünü doğrulayın.

    ![Kurtarma planı](./media/contoso-migration-rehost-vm-sql-ag/failover2.png)

6. Azure'da VM doğruladıktan sonra geçiş işlemini tamamlayın, sanal makine için çoğaltmayı durdurma ve Site Recovery Faturalaması VM durdurmak için geçişi tamamlayın.

    ![Yük devretme](./media/contoso-migration-rehost-vm-sql-ag/failover3.png)

### <a name="update-the-connection-string"></a>Bağlantı dizesi güncelleştir

Geçiş işleminin son adımı olarak uygulama SHAOG Dinleyicide çalışan geçirilmiş veritabanına işaret etmek için bağlantı dizesi Contoso güncelleştirin. Bu yapılandırma, artık Azure'da çalışan WEBVM üzerinde değiştirilir.  Bu yapılandırma, ASP uygulaması web.config dosyasında bulunur. 

1. C:\inetpub\SmartHotelWeb\web.config dosyasını bulun.  AOG FQDN'sini yansıtacak şekilde sunucunun adını değiştirin: shaog.contoso.com.

    ![Yük devretme](./media/contoso-migration-rehost-vm-sql-ag/failover4.png)  

2. Dosyayı güncelleştirme ve kaydettikten sonra IIS WEBVM üzerinde yeniden başlatın. Bunlar bir komut isteminden /RESTART IISRESET kullanarak yapın.
2. IIS yeniden başlatıldıktan sonra uygulama artık SQL mı üzerinde çalışan veritabanını kullanıyor.


**Daha fazla yardım gerekiyor mu?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) yük devretme testi çalıştırma. 
- [Bilgi](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) bir kurtarma planı oluşturma.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Azure'a yük devrediliyor.

## <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Geçişten sonra SmartHotel uygulamayı bir Azure VM çalıştıran ve SmartHotel veritabanını Azure SQL kümede bulunur.

Şimdi, Contoso temizleme adımları tamamlaması gerekir:  

- Şirket içi sanal makineleri vCenter stoktan kaldırın.
- Sanal makineleri yerel yedekleme işlerden kaldırın.
- VM'ler için yeni konumlar ve IP adreslerini göstermek için iç belgeleri güncelleştirin.
- Kullanımdan Kaldırılan sanal makineleri ile etkileşim herhangi bir kaynağa gözden geçirin ve herhangi bir ilgili ayarları veya belge yeni yapılandırmayı yansıtacak şekilde güncelleştirin.
- İki yeni VM'ler (SQLAOG1 ve SQLAOG2) sistemleri izleme üretim eklenmelidir ekleyin.

## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Contoso Azure geçirilen kaynakları ile tam olarak faaliyete ve yeni altyapılarını güvenli gerekir.

### <a name="security"></a>Güvenlik

Contoso güvenlik ekibine Azure VM'ler WEBVM, SQLAOG1 ve SQLAOG2 güvenlik sorunları belirlemek için inceler. 

- Bunlar, VM erişimi denetlemek için ağ güvenlik grupları (Nsg'ler) gözden geçirin. Nsg'ler yalnızca uygulamaya izin verilen trafiğin geçebilmesi emin olmak için kullanılır.
- Bunlar Azure Disk şifrelemesi ve KeyVault kullanarak disk üzerindeki verilerin güvenliğini sağlama değerlendiriyorsanız.
- Bunlar saydam veri şifreleme (TDE) değerlendirmek ve yeni SQL AOG üzerinde çalıştırılan SmartHotel veritabanındaki etkinleştirin. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017).

[Daha fazla bilgi](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) VM'ler için güvenlik uygulamaları hakkında.

### <a name="backups"></a>Yedeklemeler

Contoso giderek WEBVM, SQLAOG1 ve SQLAOG2 verileri yedeklemek için Azure Yedekleme hizmetini kullanarak. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Lisans ve maliyet en iyi duruma getirme

1. Contoso kendi WEBVM için var olan lisans var ve Azure karma avantajı özelliğinden yararlanır.  Bunlar, bu fiyatlandırma avantajlarından yararlanmak için var olan Azure VM'ler dönüştürmeniz.
2. Contoso Azure maliyeti Cloudyn, Microsoft ofisine lisanslı yönetim olanağı sağlar. Bunu kullanan ve Azure ve diğer bulut kaynakları yönetmenize yardımcı olacak bir çok bulut maliyeti yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyeti yönetimi hakkında. 

## <a name="conclusion"></a>Sonuç

Bu makalede, Azure Site Recovery hizmetini kullanarak uygulama ön uç VM geçirerek Contoso Azure SmartHotel uygulamada rehosted. Bunlar, Azure'da sağlanan SQL Server kümesi için uygulama veritabanı geçiş ve bir SQL Server AlwaysOn Kullanılabilirlik grubunda korumalı.

## <a name="next-steps"></a>Sonraki adımlar

Serideki sonraki makalede, biz Contoso Linux üzerinde çalışan, hizmet Masası osTicket uygulama nasıl yeniden barındırma göstereceğiz ve MySQL veritabanı ile dağıtılır.


