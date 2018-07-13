---
title: Bir Azure VM ve Azure SQL yönetilen örneğini geçirerek Contoso şirket içi uygulama barındırma | Microsoft Docs
description: Contoso şirket içi uygulama Azure Vm'leri ve Azure SQL yönetilen örnek nasıl rehosts öğrenin
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 49a5fb13a881b206c36dcd08a4c2945880e9a4bd
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39002305"
---
# <a name="contoso-migration-rehost-an-on-premises-app-on-azure-vms-and-azure-sql-managed-instance"></a>Contoso geçiş: şirket içi uygulama Azure Vm'leri ve Azure SQL yönetilen örneği yeniden barındırma

Bu makalede, Contoso kendi SmartHotel uygulama ön uç VM'nin Azure sanal makinelerine nasıl geçirdiğini Azure Site Recovery hizmeti ve Azure SQL yönetilen örneği için uygulama veritabanını kullanarak gösterilmektedir.

> [!NOTE]
> Azure SQL yönetilen örneği şu anda Önizleme aşamasındadır.

Bu belge Contoso adlı kurgusal şirketin şirket içi kaynaklarını Microsoft Azure bulutuna nasıl geçirdiğini belge makaleler serisinin biridir. Seri arka plan bilgileri ve bir dizi geçiş altyapısını kurma ve farklı türde geçiş çalıştırmak nasıl çalışılacağını senaryolar içerir. Senaryoları, karmaşık hale gelmesi ve diğer makaleler zamanla ekleyeceğiz.


**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale dizisini ve kullandığımız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[2. makale: bir Azure altyapısını dağıtma](contoso-migration-infrastructure.md) | Açıklayan nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar. Altyapıyı, tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](contoso-migration-assessment.md)  | Contoso değerlendirme Wmware'de çalışan bir şirket içi iki katmanlı SmartHotel uygulamanın nasıl çalıştığını gösterir. Contoso uygulaması Vm'lerle değerlendirir [Azure geçişi](migrate-overview.md) hizmet ve uygulama SQL Server veritabanıyla [veritabanı geçiş Yardımcısı'nı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
4. makale: Azure sanal makineler ve yönetilen bir SQL örneği üzerinde bir uygulamayı barındırma | Contoso lift-and-shift ile taşıma geçiş için Azure SmartHotel şirket içi uygulama için nasıl çalıştığını gösterir. Contoso VM ön uç uygulamasını kullanarak geçirir [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)ve SQL yönetilen örneği, uygulama veritabanını kullanarak [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Bu makalede.
[Makale 5: bir uygulamayı Azure vm'lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Nasıl Contoso geçirme SmartHotel uygulama sanal makinelerini Site Recovery hizmetini kullanarak Azure Vm'leri için gösterir. | Kullanılabilir
[Makale 6: Azure sanal makineleri ve Always On kullanılabilirlik grubu SQL Server üzerinde bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulamayı nasıl geçirdiğini gösterir. Contoso, uygulama sanal makinelerini ve veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için geçirmek için Site Recovery kullanır. | Kullanılabilir
[Makale 7: Azure sanal makineler'de Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Nasıl Contoso Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini Azure Vm'leri için Site RECOVERY'yi kullanarak yaptığını gösterir. | Kullanılabilir
[Makale 8: Azure sanal makineler ve Azure MySQL üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso Linux osTicket uygulaması için Azure Site RECOVERY'yi kullanarak VM'lerin nasıl geçirdiğini gösterir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
[Makale 9: bir uygulamayı Azure Web Apps ve Azure SQL veritabanında yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Nasıl Contoso SmartHotel uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanının Azure SQL Server örneğine geçirir gösterir | Kullanılabilir
[Makale 10: Azure Web Apps ve Azure MySQL üzerinde bir Linux uygulaması yeniden düzenleyin.](contoso-migration-refactor-linux-app-service-mysql.md) | Linux osTicket uygulaması Contoso birden çok sitede, GitHub ile sürekli teslim için tümleşik Azure Web Apps'e nasıl geçirdiğini gösterir. Bunlar, Azure MySQL örneğine uygulama veritabanına geçirin. | Kullanılabilir
[Makale 11: TFS VSTS üzerinde yeniden düzenleyin.](contoso-migration-tfs-vsts.md) | Geçiş yaparak Contoso şirket içi Team Foundation Server (TFS) dağıtımının nasıl geçirdiğini gösterir. Bunun için Visual Studio Team Services (VSTS) azure'da. | Kullanılabilir
[Makale 12: bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso geçirir ve Azure SmartHotel uygulamasının rearchitects nasıl gösterir. Bunlar, bir Windows kapsayıcısı ve bir Azure SQL veritabanı'nda uygulama veritabanı uygulama web katmanla yeniden oluşturma. | Kullanılabilir
[Makale 13: uygulamanızı Azure'a yeniden oluşturun.](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, uygulama hizmetleri, Azure Kubernetes, Azure işlevleri, Bilişsel hizmetler ve Cosmos DB dahil olmak üzere çeşitli kullanarak SmartHotel uygulamasının nasıl yeniden gösterir. | Kullanılabilir


Bu makalede kullanılan örnek SmartHotel uygulamasını kullanmak istiyorsanız, buradan indirebilirsiniz [github](https://github.com/Microsoft/SmartHotel360).

## <a name="on-premises-architecture"></a>Şirket içi mimarisi

İşte geçerli Contoso şirket içi altyapı gösteren diyagram.

![Contoso mimarisi](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)  

- Contoso, şehir, New York'ta olarak Doğu ABD'de bulunan bir ana veri merkezinde sahiptir.
- Amerika Birleşik Devletleri arasında üç ek yerel dalları sahiptirler.
- Ana veri merkezinin Fiber metro ethernet bağlantı (500 MB/sn) ile İnternet'e bağlı.
- Her dal, yerel olarak IPSec VPN tünelleri ana merkezine iş sınıfı bağlantıları kullanarak İnternet'e bağlı. Böylece, kalıcı olarak bağlanması, tüm ağ ve internet bağlantısı en iyi duruma getirir.
- Ana veri merkezinin VMware ile tam olarak sanallaştırılır. VCenter Server 6.5 tarafından yönetilen iki ESXi 6.5 sanallaştırma konaklarını sahiptirler.
- Contoso, kimlik yönetimi ve iç ağdaki DNS sunucuları için Active Directory kullanır.
- Etki alanı denetleyicileri veri merkezindeki VMware VM'ler üzerinde çalıştırın. Yerel dalları etki alanı denetleyicilerde fiziksel sunucularda çalıştırın.



## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, iş ile bu geçiş yapmanın istediği anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso giderek ve sonuç olarak, şirket içi sistemler ve altyapı Basıncı yoktur.
- **Verimliliği artırmak**: Contoso gereken gereksiz yordamları kaldırıp, geliştiriciler ve kullanıcılar için süreçlerini kolaylaştırabilirsiniz.  Hızlı olmasını iş ihtiyaçlarınıza BT ve değil atık zaman ve para, bu nedenle müşteri gereksinimlerine daha hızlı teslim.
- **Çevikliği artırın**: Contoso BT işletme ihtiyaçlarını daha hızlı olması gerekir. Market'te genel ekonomi de başarılı etkinleştirmek için değişiklikleri daha hızlı tepki vermek mümkün olması gerekir.  Ulaşın veya bir iş engelleyici hale gerekmez.
- **Ölçek**: başarıyla büyüdükçe, Contoso BT aynı yükselmeye mümkün sistemlerini sağlamanız gerekir.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini bu geçiş için aşağı sabitlenmiş. Bu hedefleri, en iyi geçiş yöntemini belirlemek için kullanılır:

- Bugün, şirket içi VMWare ortamlarında olduğu gibi geçişten sonra uygulamanızı Azure'a aynı performans özelliklerine sahip olmalıdır.  Buluta geçiş, uygulama performansını daha az kritik olduğundan gelmez.
- Contoso, bu uygulamada yatırım yapmaya istememektedir.  Kritik ve önemli iş, ancak mevcut haliyle yalnızca buluta olduğu gibi taşımak istedikleri.
- Uygulama geçirildikten sonra veritabanı yönetim görevleri en aza.
- Contoso, bu uygulama için bir Azure SQL veritabanı kullanmak istemiyorsa ve alternatifleri için arıyor.

## <a name="proposed-architecture"></a>Önerilen mimarisi

Bu senaryoda:

- Contoso, iki katmanlı şirket içi seyahat uygulamasını geçirme ister.
- Uygulama katmanlı iki VM arasında (WEBVM ve SQLVM) ve VMware ESXi ana bilgisayarda yer alan **contosohost1.contoso.com** (sürüm 6.5)
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Bunlar, Azure SQL yönetilen örneğini (SmartHotelDB) uygulama veritabanı geçirin.
- Bunlar, Azure VM'LERİNİ şirket içi VMware sanal makineleri geçirin.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).
- Geçiş tamamlandıktan sonra şirket içi Vm'leri Contoso veri merkezinde kullanımdan.

![Senaryo mimarisi](media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png) 

### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Veritabanı yönetim hizmeti](https://docs.microsoft.com/azure/dms/dms-overview) | DMS, birden çok veritabanı kaynağını sorunsuz geçiş için en düşük kapalı kalma süresi ile Azure veri platformu sağlar. | Hakkında bilgi edinin [desteklenen bölgeler](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) DMS ve get için [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/database-migration/).
[Azure SQL yönetilen örneği](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Yönetilen örnek Azure bulutunda tam olarak yönetilen SQL Server örneği temsil eden bir yönetilen veritabanı hizmetidir. SQL Server Veritabanı Altyapısı'nın en son sürümüyle aynı kodu paylaşır ve en son özellikleri, performans iyileştirmeleri ve güvenlik yamaları vardır. | Azure SQL veritabanı yönetilen örnekleri'ni kullanarak azure'da çalışan işlem ücreti kapasiteye bağlı. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Hizmet düzenler ve geçiş ve olağanüstü durum kurtarma için Azure Vm'leri yönetir ve şirket içi Vm'leri ve fiziksel sunucuları.  | Azure'a çoğaltma sırasında Azure depolama ücretleri uygulanır.  Azure Vm'leri oluşturulur ve yük devretme işlemi gerçekleştiğinde, ücret. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/) ücretleri ve fiyatlandırma hakkında.

 

## <a name="migration-process"></a>Geçiş işlemi

Contoso SmartHotel uygulama hem web hem de veri katmanlarını Azure'a geçirir.

1. Bunlar yalnızca belirli Azure bileşenlerini bu senaryo için birkaç eklemeniz gerekir Contoso, Azure altyapısının yerinde, zaten sahip.
2. Veri katmanı, veri geçiş hizmeti (DMS) kullanarak geçirilecektir.  DMS şirket içi SQL Server VM Contoso veri merkeziniz ile Azure arasında siteden siteye VPN bağlantısı üzerinden bağlanır ve ardından veritabanına geçirir.
3. Web katmanı, Azure Site Recovery ile lift-and-shift ile taşıma geçiş kullanarak geçirilecektir. Bu işlem şirket içi VMware ortamını hazırlama, ayarlama ve çoğaltmayı etkinleştirdikten ve bunları Azure'a devretmek tarafından Vm'leri geçirme dahildir.

     ![Geçiş mimarisi](media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png) 


## <a name="prerequisites"></a>Önkoşullar

Bu senaryoda Contoso (ve,) gerekenler aşağıda verilmiştir.

**Gereksinimleri** | **Ayrıntılar**
--- | ---
**Önizlemede kaydetme** | SQL yönetilen örneği sınırlı genel önizlemeye kaydedilmesi gerekir. Bir Azure aboneliği için gereksinim duyduğunuz [kaydolun](https://portal.azure.com#create/Microsoft.SQLManagedInstance). Kaydı tamamlamak için bu nedenle, bunu bu senaryoyu dağıtmaya başlamadan önce emin olun, birkaç gün sürebilir.
**Azure aboneliği** | Bu serideki ilk makaledeki değerlendirme gerçekleştirildiğinde aboneliği oluşturmuş olmanız. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.<br/><br/> Daha ayrıntılı izinler gerekirse gözden [bu makalede](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Site recovery (şirket içi)** | Şirket içi vCenter server 5.5, 6.0 veya 6.5 sürümünü çalıştırmalıdır<br/><br/> 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir ESXi ana bilgisayarı<br/><br/> Bir veya daha fazla VMware ESXi ana bilgisayarında çalışan VM'ler.<br/><br/> Vm'leri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Desteklenen [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) yapılandırma.
**DMS** | DMS için gereksinim duyduğunuz bir [uyumlu şirket içi VPN cihazı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Şirket içi VPN cihazının yapılandırmanız gerekir. Dışarıya yönelik genel bir IPv4 adresi olmalıdır ve bu adresi bir NAT cihazının arkasında yer almamalıdır.<br/><br/> Şirket içi SQL Server veritabanınıza erişimi olduğundan emin olun.<br/><br/> Windows Güvenlik Duvarı, kaynak veritabanı altyapısını erişebilir olmalıdır. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Veritabanını makinenize önünde bir güvenlik duvarı varsa, veritabanı ve dosyalarına SMB bağlantı noktası 445 üzerinden erişime izin vermek için kuralları ekleyin.<br/><br/> Yönetilen örnek hedef ve kaynak SQL Server'ı bağlanmak için kullanılan kimlik bilgileri sysadmin sunucu rolünün üyesi olması gerekir.<br/><br/> Bir ağ ihtiyacınız DMS kaynak veritabanını yedeklemek için kullanabileceğiniz şirket içi veritabanı paylaşın.<br/><br/> Kaynak SQL Server örneğini çalıştıran hizmet hesabının ağ paylaşımında yazma ayrıcalıkları olduğundan emin olun.<br/><br/> Ağ paylaşımı üzerinde tam denetim ayrıcalığı olan bir Windows kullanıcı (ve parola) not edin. Azure veritabanı geçiş hizmetinin yedekleme dosyalarını Azure depolama kapsayıcısına karşıya yüklemek için bu kullanıcı kimlik bilgilerini temsil eder.<br/><br/> TCP/IP Protokolü SQL Server Express yükleme işlemi ayarlar **devre dışı bırakılmış** varsayılan olarak. Etkin olduğundan emin olun.


## <a name="scenario-steps"></a>Senaryo adımları

Burada'nın dağıtımı ayarlamak üzere Contoso nasıl kolaylaştıracağını:

> [!div class="checklist"]
> * **1. adım: yönetilen bir SQL Azure örneği ayarlama**: gereksinim duydukları, şirket içi SQL Server veritabanını geçirme önceden oluşturulmuş bir yönetilen örnek.
> * **2. adım: DMS hazırlama**: yolculuğunuzu sağlayıcıyı kaydetmek için örneği oluşturup ardından DMS projesi oluşturmak için ihtiyaç duydukları. Bunlar ayrıca DMS SA URI'sini ayarlamanız gerekir. Depolama nesneleri için sınırlı izinleri size verebilir böylece bir paylaşılan erişim imzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI), depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. DMS hizmeti SQL Server Yedekleme dosyalarını karşıya yüklemeleri depolama hesabı kapsayıcısını erişebilmesi için bunlar bir SAS URI'sini ayarlayın.
> * **3. adım: Azure Site Recovery için hazırlama**: Site Recovery, için bunlar gerekir çoğaltılan verileri tutmak için bir Azure depolama hesabı oluşturun ve bir kurtarma Hizmetleri kasası oluşturun.
> * **4. adım: Site Recovery için şirket içi Vmware'leri hazırlama**: Contoso VM bulma ve aracı yükleme hesabı hazırlayın ve yük devretme sonrasında Azure Vm'lerine bağlanmak için hazırlık yapma.
> * **5. adım: Çoğaltma Vm'leri**: çoğaltmayı ayarlama, bunlar Site Recovery kaynak ve hedef ortamını yapılandırma, bir çoğaltma ilkesi ayarlayın ve Azure depolama alanına Vm'leri çoğaltmaya başlayın.
> * **6. adım: veritabanı DMS ile geçiş**: artık bu veritabanı geçiş yapabilirsiniz.
> * **7. adım: Site Recovery ile Vm'leri geçirme**: her şeyin çalıştığından emin olmak için yük devretme testi çalıştırma ve ardından sanal makineleri Azure'a geçirmek için bir tam yük devretme çalıştırın.


## <a name="step-1-prepare-an-azure-sql-managed-instance"></a>1. adım: Azure SQL yönetilen örneği hazırlama

Azure SQL yönetilen örneği için bir alt ağ Contoso gerekir:

- Alt ağ ayrılmış olmalıdır. Boş olmalıdır ve herhangi bir bulut hizmeti içermiyor ve bir ağ geçidi alt ağı olmamalıdır.
- Yönetilen örneği oluşturulduktan sonra, kaynakları eklemeniz gerekir.
- Alt ağ ile ilişkilendirilmiş bir NSG olması gerekmez.
- Alt ağı, kendisine atanmış yalnızca rota olarak 0.0.0.0/0 sonraki atlama Internet ile kullanıcı rota tablosu (UDR) olması gerekir. 
- İsteğe bağlı bir özel DNS: Azure'nın yinelemeli Çözümleyicileri IP adresi (168.63.129.16 gibi) VNet üzerinde özel DNS belirtilirse, listeye eklenmelidir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- Alt ağ ile ilişkili bir hizmet uç noktası (depolama veya SQL) olması gerekmez. Hizmet uç noktaları sanal ağda devre dışı bırakılması gerekir.
- Alt ağı, en az 16 IP adresleri olması gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances) yönetilen örnek alt boyutlandırma hakkında.
- Özel DNS ayarları, karma ortamda gereklidir. Contoso, bir veya daha fazla Azure DNS sunucularını kullanmak için DNS ayarlarını yapılandırır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns) DNS özelleştirme hakkında daha fazla.

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>Yönetilen örnek için sanal ağ ayarlama

Contoso VNet oluşturmak gibi ayarlar: 

1. Contoso ContosoNetworkingRG kaynak grubunda birincil Doğu ABD 2 bölgesinde yeni bir sanal ağ (VNET SQLMI EU2) oluşturur.
2. Contoso 10.235.0.0/24, bir adres alanı aralığı Contoso Kurumsal diğer ağlarla çakışmayacak sağlama atar.
2. Bunlar, iki alt ağa ekleyin:
    - SQLMI DS EUS2 (10.235.0.0.25)
    - SQLMI-SAW-EUS2 (10.235.0.128/29). Bu alt ağı, yönetilen örnek (SQLMI) dizin eklemek için kullanılır.

    ![Yönetilen örnek ağ](media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

6. VNet ve alt ağlar dağıtıldıktan sonra Contoso ağlar gibi eşleri:

    - Eş VNET-SQLMI-EUS2 ile VNET-HUB-EUS2 (merkez sanal ağa Doğu ABD 2 için).
    - Eş VNET-SQLMI-EUS2 VNET-PROD-EUS2 (üretim ağı) sahip.

    ![Ağ eşlemesi](media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

7. Contoso özel DNS ayarlarını belirler. DNS, Azure DC'lere ilk yönlendirir. Azure DNS, ikincil olacaktır. Contoso Azure DC'leri bulunduğu konum aşağıda verilmiştir:

    - Ürün DC EUS2 alt ağında yer alan, Doğu ABD 2 üretimde (VNET-PROD-EUS2) ağ.
    - CONTOSODC3 adresi: 10.245.42.4
    - CONTOSODC4 adresi: 10.245.42.5
    - Azure DNS Çözümleyicisi: 168.63.129.16

     ![Ağ DNS](media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

**Daha fazla yardıma mı ihtiyacınız var?**

- [Genel bakışın](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) Azure SQL yönetilen örnek.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#create-a-new-virtual-network-for-managed-instances) SQL yönetilen örneği için bir VNet oluşturma hakkında daha fazla.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering) eşleme ayarlama ayarlama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns) Azure AD DNS ayarları güncelleştiriliyor.



### <a name="set-up-routing"></a>Yönlendirme ayarlama

Contoso bir yol tablosu, Azure yönetim hizmeti ile iletişim kurmak için gereken şekilde yönetilen örneğe özel bir sanal ağ içinde yer alır. Bu, yöneten hizmet ile iletişim kuramıyorsa erişilemez duruma gelir.

- Rota tablosunu yönetilen örneği ' gönderilen paketlerin VNet içinde nasıl yönlendirileceğini belirtir (yol) kuralları kümesi içerir.
- Rota tablosunu yönetilen örnekler dağıtıldığı alt ağlar ile ilişkilidir. Bir alt ağdan çıkan her paket, ilişkili yol tablosuna dayalı olarak işlenir.
- Bir alt ağ yalnızca tek bir yol tablosu ile ilişkilendirilebilir.
- Microsoft Azure'da rota tabloları oluşturmak için herhangi bir ek ücret yoktur.

1. Contoso, bir kullanıcı tanımlı yol tablosu oluşturur. Rota tablosunu ContosoNetworkingRG kaynak grubunda oluşturulur.

    ![Rota tablosu](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

2. Contoso yol tablosu (MIRouteTable) dağıtıldıktan sonra yönetilen örneği gereksinimlerine uymak için bir adres ön eki 0.0.0.0/0 olan bir yol ekleyin ve **sonraki atlama türü** kümesine **Internet**.

    ![Rota tablosu öneki](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)
    
3. Contoso SQLMI DB EUS2 ağındaki alt ağ (VNET SQLMI EUS2) yönlendirme tablosunu ilişkilendirme. 

    ![Rota tablosunu alt ağ](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)
    
**Daha fazla yardıma mı ihtiyacınız var?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal#create-new-route-table-and-a-route) yönetilen örnek için rotalar ayarlama.

### <a name="create-a-managed-instance"></a>Yönetilen örnek oluşturma

Artık, Contoso bir SQL veritabanı yönetilen örneği sağlayabilirsiniz.

1. Yönetilen örnek bir iş uygulamasını gördüğünden Contoso dağıtmak, ContosoRG kaynak grubunda birincil kendi Doğu ABD 2 bölgesinde, 
2. Bunlar, bir fiyatlandırma katmanı ve boyutu işlem ve depolama için örneği seçin. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/sql-database/managed/) fiyatlandırma hakkında daha fazla.

    ![Yönetilen örnek](media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

3. Yönetilen örneği dağıtıldıktan sonra iki yeni kaynaklar ContosoRG kaynak grubunda görünür:

    - Bir sanal birden çok yönetilen örneği varsa küme
    - SQL Server yönetilen örneği. 

    ![Yönetilen örnek](media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

**Daha fazla yardıma mı ihtiyacınız var?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal) bir yönetilen örneğini sağlama.

## <a name="step-2-prepare-dms"></a>2. adım: DMS hazırlama

DMS hazırlamak için Contoso birkaç şey yapmanız gerekir:

- Azure'da DMS sağlayıcısını Kaydet
- DMS bir veritabanına geçiş yapmak için kullanılan yedekleme dosyalarını karşıya yükleme için Azure depolama erişimi sağlar. Bunlar, bir blob depolama kapsayıcısı oluşturarak bunu ve bir paylaşılan erişim imzası (SAS) URI oluşturmak. 
- DMS projesi oluşturun.


Adımları aşağıdaki gibi tamamlayın:

1. Contoso yolculuğunuzu sağlayıcıyı aboneliğini altında kaydeder.
    ![DMS kaydetme](media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

2. Bunlar, bir depolama blob kapsayıcı oluşturun ve DMS erişebilmesi bir SAS URI'si oluşturmak.

    ![SAS URI'Sİ](media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

3. Son olarak, DMS örneği oluşturun. 

    ![DMS örneği](media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

4. Contoso, VNET-PROD-DC-EUS2 VNet PROD DC EUS2 alt ağda DMS örneği yerleştirir.
    - Bir VPN ağ geçidi üzerinden şirket içi SQL Server VM erişimi olan bir VNet olmalıdır çünkü bunlar, oraya.
    - VNET-PROD-EUS2 VNET-HUB-EUS2 eşlenmişse ve uzak ağ geçitlerini kullanmasına izin verilir.  Bu, DMS gerektiği şekilde iletişim kurabildiğini sağlar.

        ![DMS ağ](media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

**Daha fazla yardıma mı ihtiyacınız var?**
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/dms/quickstart-create-data-migration-service-portal) DMS ayarı.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2) oluşturma ve SAS kullanma hakkında.


## <a name="step-3-prepare-azure-for-the-site-recovery-service"></a>3. adım: Azure Site Recovery hizmeti için hazırlama

Birçok Azure öğelerin Contoso için kendi web katmanı VM (WEBMV) geçişini Site kurtarma işlemini ayarlama olması gerekir

- Bir Vnet'te yük devretti kaynakları bulunur.
- Çoğaltılan verileri tutmak için bir Azure depolama hesabı. 
- Azure kurtarma Hizmetleri kasasında.

Bunlar Site kurtarma işlemini şu şekilde ayarlayın:

1. VM bir web ön ucu SmartHotel uygulama olduğundan, kendi mevcut üretim ağı (VNET-PROD-EUS2) ve alt ağ (PROD-FE-EUS2) birincil Doğu ABD 2 bölgesinde bir VM üzerinde başarısız olur. Bu ağ ne zaman ayarladıkları bunlar [dağıtılan Azure altyapısı](contoso-migration-infrastructure.md)
2. Bunlar, Azure depolama hesabınız (contosovmsacc20180528) oluşturur. Standart depolama ve LRS çoğaltma ile genel amaçlı bir hesabı kullanıyorsanız.

    ![Site Kurtarma Depolama](media/contoso-migration-rehost-vm-sql-managed-instance/asr-storage.png)

3. Ağ ve depolama hesabı ile yerinde, Contoso artık (ContosoMigrationVault) bir kasa oluşturun ve ContosoFailoverRG kaynak grubunda birincil Doğu ABD 2 bölgesinde yerleştirin.

    ![Kurtarma Hizmetleri kasası](media/contoso-migration-rehost-vm-sql-managed-instance/asr-vault.png)

**Daha fazla yardıma mı ihtiyacınız var?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Site Recovery için Azure'ı ayarlama.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>4. adım: Site Recovery için şirket içi Vmware'leri hazırlama

Site Recovery için VMware hazırlamak için işte Contoso yapması gerekir:

- VM bulmayı otomatikleştirmek için vCenter sunucusunda veya vSphere ESXi konağına, bir hesap hazırlayın.
- Çoğaltmak istediğiniz VMware Vm'lerinde Mobility hizmetini otomatik olarak yüklenmesini sağlayan bir hesap hazırlama.
- Yük devretme sonrasında oluşturulan Azure Vm'lerinin bağlanabilmesi için şirket içi Vm'leri hazırlayın.


### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- VM'leri otomatik olarak bulma. En az bir salt okunur hesap gereklidir.
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme. Oluşturma ve diskleri kaldırma ve Vm'leri kapatarak gibi işlemleri de çalıştırabilirsiniz bir hesabınız olmalıdır.

Contoso hesabı gibi ayarlar:

1. Bir rolü vCenter düzeyinde oluşturur.
2. Bu rol için gerekli izinleri atar.

**Daha fazla yardıma mı ihtiyacınız var?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) oluşturma ve Otomatik bulma için bir rol atama.

### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Çoğaltmak istediğiniz sanal makinede Mobility hizmeti yüklü olmalıdır.

- Sanal makine için çoğaltmayı etkinleştirdiğinizde site Recovery bu bileşeni otomatik gönderim yüklemesi yapabilirsiniz.
- Otomatik göndererek yükleme için Site Recovery'nin sanal Makineye erişmek için kullanacağı bir hesap hazırlamanız gerekir.
- Azure konsolunda çoğaltma ayarlarken bu hesabı belirtirsiniz.
- Bir etki alanı veya yerel hesap VM'de yüklemek için gerekli izinlere sahip gerekir.

**Daha fazla yardıma ihtiyacınız**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) Mobility hizmetinin göndererek yüklenmesine ilişkin için bir hesabı oluşturuluyor.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Azure'a yük devretme sonrasında Contoso çoğaltılmış Azure vm'lere bağlanmak atabilmek istiyor. Bunu yapmak için birkaç şey, geçiş işlemini çalıştırmadan önce şirket içi VM'de yapmak için ihtiyaçları vardır: 

1. İnternet üzerinden erişim, bunlar yük devretmeden önce şirket içi VM'de RDP'yi etkinleştirin ve için TCP ve UDP kurallarının eklendiğinden emin olun **genel** profili ve içinde RDP'ye izin verildiğinden **Windows Güvenlik Duvarı**  >  **Verilen uygulamaları** tüm profiller için.
2. Kendi siteden siteye VPN üzerinden erişim, bunlar şirket içi makinede RDP'yi etkinleştirin ve içinde RDP'ye izin ver **Windows Güvenlik Duvarı** -> **izin verilen uygulamalar ve Özellikler** için **etki alanı ve Özel** ağlar.
3. Bunlar şirket içi VM'deki işletim sisteminin SAN ilkesinin ayarlamak **OnlineAll**.

Ayrıca, bir yük devretme çalıştırdıklarında bunlar aşağıdakileri denetlemeniz gerekir:

- Bulunmamalıdır bekleyen herhangi bir Windows güncelleştirme VM üzerinde bir yük devretme tetiklendiğinde. Varsa, bunlar güncelleştirme tamamlanana kadar sanal makinede oturum açmak mümkün olmayacaktır.
- Yük devretme sonrasında, onlar iade **önyükleme tanılaması** VM'nin bir ekran görüntülemek için. Bu işe yaramazsa, bunlar VM'nin çalıştığından ve bu gözden denetlemelisiniz [sorun giderme ipuçları](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>5. adım: Azure Site Recovery ile şirket içi Vm'lerini çoğaltma

Azure'da bir geçiş çalıştırmadan önce çoğaltmayı ayarlama ve kendi şirket içi sanal Makineye yönelik çoğaltmayı etkinleştirmek Contoso gerekir.

### <a name="set-a-replication-goal"></a>Çoğaltma hedefi ayarlama

1. Kasada kasa adını (ContosoVMVault) altında çoğaltma hedefi ayarlayın (**Başlarken** > **Site Recovery** > **altyapıyıhazırlama**.
2. Bunlar, makinelerini şirket içi olduğunu, VMware Vm'leri ve Azure'a çoğaltmak istediğiniz olduğunuzu belirtin.

    ![Çoğaltma hedefi](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

Devam etmek için bunlar seçerek dağıtım planlamasını tamamladınız mı onaylamak için ihtiyaçları **Evet, yaptım**. Bu dağıtımda Contoso yalnızca tek bir VM geçirme olan ve dağıtım planlama gerekmez.

### <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Artık kendi kaynak ortamını yapılandırmak gerekir. Bunu yapmak için bir OVF şablonu indirin ve yapılandırma sunucusu ve yüksek oranda kullanılabilir olarak ilişkili bileşenlerini dağıtmak için kullanın, şirket içi VMware sanal makine. Sunucu üzerindeki bileşenler şunlardır:

- Şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir yapılandırma sunucusu.
- İşlem sunucusu çoğaltma ağ geçidi davranır. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir.
- İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.
- Sanal makine oluşturulup başlatıldıktan yapılandırma sunucusu sonra Contoso kasaya kaydedebilirsiniz.


Contoso adımları aşağıdaki gibi gerçekleştirin:

1. Bunlar Azure portalından OVF şablonu indirin (**altyapıyı hazırlama** > **kaynak** > **yapılandırma sunucusu**).
    
    ![OVF indirin](./media/contoso-migration-rehost-vm-sql-managed-instance/add-cs.png)

2. Bunlar, şablonu oluşturup VM dağıtmak için Vmware'e aktarın.

    ![OVF şablonu](./media/contoso-migration-rehost-vm-sql-managed-instance/vcenter-wizard.png)

3.  Sanal makinede ilk kez açtığınızda, oluşturan bir Windows Server 2016 yükleme deneyimi önyüklenir. Bunlar lisans sözleşmesini kabul edin ve bir yönetici parolasını girin.
4. Yükleme tamamlandıktan sonra VM için yönetici olarak oturum açın. İlk kez oturum açma işleminde, varsayılan olarak Azure Site kurtarma Yapılandırması aracını çalıştırır.
5. Aracı'nda, yapılandırma sunucusunu kasaya kaydetmeyi sırasında kullanmak için bir ad belirtin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra seçtikleri **oturum**, Azure aboneliği için oturum açın. Kimlik bilgileri, yapılandırma sunucusu kaydedilecek kasa erişiminiz olması gerekir. 

    [Yapılandırma sunucusunu kaydetmek](./media/contoso-migration-rehost-vm-sql-managed-instance/config-server-register2.png)

7. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır. Bunların makinede yeniden oturum açın ve yapılandırma sunucusu yönetim Sihirbazı otomatik olarak başlar.
8. Sihirbazda, çoğaltma trafiğini almak için NIC'yi seçin. Bu ayar yapılandırıldıktan sonra değiştirilemez.
9. Abonelik, kaynak grubu ve yapılandırma sunucusunu kaydetmek istediğiniz kasaya seçerler.
        ![Kasa](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz1.png) 

10. Bunlar sonra da indirin ve MySQL Server ve VMWare powerclı'yı yükleyin. Ardından, sunucu ayarlarını doğrulayın.
11. Doğrulama sonrasında, bunlar vCenter sunucusunda veya vSphere konağının FQDN'sini veya IP adresini belirtin. Bunlar, varsayılan bağlantı noktasını değiştirmeyin ve Azure'da vCenter sunucusu için bir kolay ad belirtin.
12. Site Recovery çoğaltma için kullanılabilen VMware Vm'lerini otomatik olarak keşfedebilmesi için daha önce oluşturduğunuz hesabı belirtmeniz gerekir. 
13. Bunlar, çoğaltma etkinleştirildiğinde Mobility hizmetini otomatik olarak yüklemek için kimlik bilgilerini belirtin. Windows makineleri için hesabın vm'lerinde yerel yönetici ayrıcalıkları gerekir. 

    ![vCenter](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz2.png)

7. Kayıt tamamlandığında Azure Portalı'nda yapılandırma sunucusunun ve VMware sunucusunun listelendiğini Contoso çift denetler **kaynak** kasadaki sayfası. Bulma, 15 dakika veya daha fazla sürebilir. 
8. Site Recovery belirtilen ayarları kullanarak VMware sunucularına bağlanır ve Vm'leri bulur.

### <a name="set-up-the-target"></a>Hedefi ayarlama

Hedef çoğaltma ortamı yapılandırmak, artık Contoso gerekir.

1. İçinde **altyapıyı hazırlama** > **hedef**, bunlar hedef ayarları seçin.
2. Site Recovery, bir Azure depolama hesabını ve belirtilen hedef ağda olup olmadığını denetler.

### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Kaynak ve hedef ayarlandıktan sonra Contoso bir çoğaltma ilkesi oluşturma ve yapılandırma sunucusu ile ilişkilendirmek hazırdır.

1. İçinde **altyapıyı hazırlama** > **çoğaltma ayarları** > **Çoğaltma İlkesi** >  **oluşturma ve İlişkilendirme**, bunlar bir ilke oluşturmak **ContosoMigrationPolicy**.
2. Bunlar, varsayılan ayarları kullanın:
    - **RPO eşiği**: varsayılan 60 dakika. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
    - **Kurtarma noktası bekletme**. Varsayılan 24 saat. Bu değer, ne kadar süreyle her kurtarma noktası için bekletme süresinin olacağını belirtir. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**. Varsayılan bir saat değeri. Bu değer, uygulamayla tutarlı anlık görüntülerin oluşturulma sıklığı belirtir.
 
        ![Çoğaltma ilkesi oluşturma](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy.png)

5. İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. 

    ![Çoğaltma İlkesi ilişkilendirme](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy2.png)


**Daha fazla yardıma mı ihtiyacınız var?**

- Bu adımlar tam bir kılavuza edinebilirsiniz [şirket içi VMware Vm'leri için olağanüstü durum kurtarmayı ayarlayın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Yardımcı olmak ayrıntılı yönergeler kullanılabilir [kaynak ortamını ayarlama](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [yapılandırma sunucusunu dağıtma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), ve [çoğaltma ayarlarını yapılandırma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).

### <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Artık Contoso WebVM çoğaltma başlayabilirsiniz.

1. İçinde **uygulama çoğaltma** > **kaynak** > **+ Çoğalt** bunlar kaynak ayarlarını seçin.
2. Bunlar, bunlar, sanal makineler etkinleştirmek istiyorsanız, vCenter sunucusu ile configuration server seçin gösterir.

 ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication1.png)
 
3. Şimdi, kaynak grubunu ve ağ, Azure VM yük devretme işleminden sonra yerleştirilir ve çoğaltılan verilerin depolanacağı depolama hesabı dahil olmak üzere hedef ayarlarını belirtin.

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication2.png)

4. Contoso WebVM çoğaltma için seçilir. Çoğaltmayı etkinleştirdiğinizde site Recovery, her sanal makinede Mobility hizmetini yükler. 

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication3.png)

5. Contoso doğru çoğaltma ilkesinin seçilir ve WEBVM için çoğaltmayı etkinleştirir denetler. Bunlar çoğaltma ilerlemeyi **işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.
6. İçinde **Essentials** Azure portalında, Azure'a çoğaltılan VM'ler için yapı Contoso görebilirsiniz.

    ![Altyapı görünümü](./media/contoso-migration-rehost-vm-sql-managed-instance/essentials.png)


**Daha fazla yardıma mı ihtiyacınız var?**

Bu adımlar tam bir kılavuza edinebilirsiniz [çoğaltmayı etkinleştir](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).

## <a name="step-6-migrate-the-database-with-dms"></a>6. adım: veritabanı DMS ile geçirme

DMS projesi oluşturun ve veritabanı geçirme contoso gerekir.

### <a name="create-a-dms-project"></a>DMS proje oluşturma
1. DMS proje oluştururlar. Bunlar, kaynak sunucu türü SQL Server ve hedef olarak Azure SQL veritabanı yönetilen örneği belirtin.

     ![DMS proje](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

2. Projeyi oluşturduktan sonra Geçiş Sihirbazı'nı açar.

### <a name="migrate-the-database"></a>Veritabanını geçirme 

1. Geçiş Sihirbazı'nda Contoso kaynak VM üzerinde şirket içi veritabanı bulunur ve ona erişmek için kimlik bilgilerini belirtir.

    ![DMS kaynak](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

2. Bunlar (SmartHotel.Registration) geçirmek için veritabanını seçin.

    ![DMS kaynak veritabanı](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-sourcedb.png)

3. Bir hedef olarak Azure ve erişim kimlik bilgileri, yönetilen örnek adını belirtin.

    ![DMS hedef](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

4. Ardından **+ yeni etkinlik** > **geçiş çalıştırmak**, bunlar geçişi çalıştırmak için ayarları belirtin:
    - Kaynak ve hedef kimlik bilgileri.
    - Geçiş için veritabanı.
    - Şirket içi VM'de oluşturduğu ağ paylaşımı. DMS, bu paylaşıma kaynak yedeklemeler alır.
        - Örneğinin olmalıdır kaynak SQL Server çalıştıran hizmet hesabının bu paylaşımda ayrıcalıkları yazın.
        - Paylaşım FQDN yolunu belirtin.
    - DMS hizmeti geçiş için yedekleme dosyalarının karşıya yükler, depolama hesabı kapsayıcı erişim sağlayan SAS URI'si.

        ![DMS ayarları](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

5. Bunlar geçiş kaydedin ve çalıştırın.
6. İçinde **genel bakış**, bunlar geçiş durumu izleyin.

    ![DMS İzleyicisi](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

7. Geçiş tamamlandıktan sonra bunlar hedef veritabanlarına yönetilen örneğinde var olduğunu doğrulayın.

    ![DMS İzleyicisi](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vm-with-site-recovery"></a>7. adım: Site Recovery ile VM'yi geçirme

Contoso bir hızlı yük devretme testi çalıştırır ve ardından VM'yi geçirme.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Geçirmeden önce WEBVM, yük devretme testi yardımcı olan her şeyin beklendiği gibi çalıştığından emin olun. 

1. Zaman içinde en son noktaya yük devretme testi, çalıştırıldıkları (**en son işlenen**).
2. Seçmeleri **yük devretmeye başlamadan önce makineyi Kapat**, böylece Site Recovery yük devretmeyi tetiklemeden önce kaynak sanal kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. 
3. Test yük devretme çalıştırır: 

    - Bir önkoşul denetimi geçiş için gerekli koşulları tümünün karşılandığından emin olmak için çalıştırılır.
    - Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
    - Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.
3. Yük devretme bittikten sonra çoğaltma Azure VM Azure Portalı'nda görünür. Bunlar, her şeyin düzgün çalıştığını denetleyin. VM'nin uygun boyutta olduğundan doğru ağa bağlandığından ve çalıştığından. 
4. Contoso, yük devretme testi doğruladıktan sonra Yük devretme ve kayıtları temizler ve gözlemlerinizi kaydeder. 

### <a name="migrate-the-vm"></a>VM'yi geçirme

1. Contoso, yük devretme testi beklendiği gibi çalıştığını doğruladıktan sonra geçiş için bir kurtarma planı oluşturur. Bunlar WEBVM planına ekleyin.

     ![Kurtarma planı](./media/contoso-migration-rehost-vm-sql-managed-instance/recovery-plan.png)

2. Ardından, bir yük devretme plan üzerinde çalıştırın. Bunlar en son kurtarma noktası seçin ve Site Recovery yük devretmeyi tetiklemeden önce şirket içi sanal makineyi denemesi gerektiğini belirtin.

    ![Yük devretme](./media/contoso-migration-rehost-vm-sql-managed-instance/failover1.png)

3. Yük devretme sonrasında, Contoso doğrulayın Azure VM'nin Azure portalında olması gerektiği gibi görünür.

   ![Kurtarma planı](./media/contoso-migration-rehost-vm-sql-managed-instance/failover2.png)

4. Azure'da VM doğruladıktan sonra geçiş işlemini tamamlayın, VM için çoğaltma durdurma ve sanal makine için Site Recovery Faturalaması durdurmak için geçişi tamamlayın.

    ![Yük devretme](./media/contoso-migration-rehost-vm-sql-managed-instance/failover3.png)

### <a name="update-the-connection-string"></a>Bağlantı dizesini güncelleştirme

Son adım olarak geçiş sürecinin Contoso bağlantı dizesi uygulamanın kendi SQL mı üzerinde çalışan geçirilen veritabanına işaret edecek şekilde güncelleştirir.

1. Azure portalında bağlantı dizesi tıklayarak buldukları **ayarları** > **bağlantı dizeleri**.

    ![Yük devretme](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)  

2. Bunlar, kullanıcı adını ve parolasını SQL yönetilen örneği ile dize güncelleştirin.
3. Dize yapılandırdıktan sonra bunlar, uygulamanın web.config dosyasında geçerli bağlantı dizesini değiştirin.
4. Dosyası güncelleştiriliyor ve kaydettikten sonra IIS WEBVM üzerinde yeniden başlatın. İle bunu **IISRESET /RESTART** bir komut isteminden.
5. IIS yeniden başlatıldıktan sonra uygulama SQL yönetilen örneği'nde çalışan veritabanı kullanacaklardır.
6. Bu noktada, Contoso SQLVM makine şirket içi kapatabilir ve geçiş tamamlandıktan.

**Daha fazla yardıma mı ihtiyacınız var?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) yük devretme testi çalıştırma. 
- [Bilgi](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) bir kurtarma planı oluşturma.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Azure'a devretmek.

## <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Geçiş tamamlandı, SmartHotel uygulamayı bir Azure sanal makinesinde çalışan ve SmartHotel veritabanı, Azure SQL yönetilen örneği'nde kullanılabilir.  

Şimdi, Contoso gibi biraz temizlik yapmanız gerekir:  

- VCenter stok WEBVM makine kaldırın.
- SQLVM makine vCenter stok kaldırın.
- WEBVM ve sqlvm ADLI yerel yedekleme işlerden kaldırın.
- Güncelleştirme iç belgeleri yeni konumu, IP adresi için WEBVM gösterir.
- SQLVM belgelerinden kaldırın. Alternatif olarak, bunlar silindi olarak göster ve artık sanal Makineye stok işaretleyebilir.
- Yetkisi alınan Vm'leri ile etkileşimde bulunan tüm kaynakları gözden geçirin ve herhangi bir ilgili ayarları veya belgeleri yeni yapılandırmayı yansıtacak şekilde güncelleştirin.

## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Azure'da geçirilen kaynakları ile tam olarak çalışır hale getirme ve yeni altyapılarını güvenli Contoso gerekir.

### <a name="security"></a>Güvenlik

Contoso güvenlik ekibi, Azure VM ve SQL yönetilen örneği, kendi uygulama güvenlik sorunları belirlemek için inceler.

- Bunlar, erişimi denetlemek için sanal makine için ağ güvenlik grupları (Nsg'ler) gözden geçirin. Nsg'ler, yalnızca uygulamaya izin trafik geçirebilirsiniz emin olmak için kullanılır.
- Bunlar ayrıca Azure Disk şifrelemesi ve Azure anahtar Kasası'nı kullanarak diskteki verilerin güvenliğini sağlama değerlendiriyorsanız.
- Bunlar, tehdit algılama üzerinde SQL yönetilen örneği (SQLMI) etkinleştirin. Bunlar, bir tehdit algılanırsa bilet için güvenlik takım/hizmet Masası sistemlerine uyarılar gönderir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-threat-detection).

     ![Yönetilen örnek güvenlik](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)  

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) VM'ler için önerilen güvenlik uygulamaları hakkında.

### <a name="backups"></a>Yedeklemeler
Contoso WEBVM Azure Backup hizmetini kullanarak verileri yedeklemek için geçiyor. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

1. Contoso için WEBVM var olan Lisansınızdan sahiptir ve Azure hibrit avantajı özelliğinden yararlanır.  Bunlar bu fiyatlandırmanın avantajlarından yararlanmak için var olan Azure VM dönüştürür.
2. Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet Yönetimi olanak sağlar. Bu, Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olan bir çoklu bulut maliyet yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında. 


## <a name="conclusion"></a>Sonuç

Bu makalede, Contoso uygulama ön uç VM'nin Site Recovery hizmetini kullanarak Azure'a geçiş yaparak Azure SmartHotel uygulamada rehosted. Bunlar, bir Azure yönetilen DMS kullanarak SQL örneği için şirket içi veritabanı geçirildi.

## <a name="next-steps"></a>Sonraki adımlar

İçinde [sonraki makalede](contoso-migration-rehost-vm.md) serisinde Contoso vm'lerinde Azure SmartHotel uygulama nasıl rehosts Azure Site Recovery hizmetini kullanarak göstereceğiz.

