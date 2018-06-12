---
title: Bir Azure VM ve Azure SQL yönetilen örneğini geçirerek Azure Contoso uygulamada yeniden barındırma | Microsoft Docs
description: Contoso Azure Vm'leri ve Azure SQL yönetilen örneği üzerinde bir şirket içi uygulama nasıl rehosts öğrenin
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: raynew
ms.openlocfilehash: c7dc9e8406494739aa5d8f21397a606e0b74a617
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301261"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms-and-azure-sql-managed-instance"></a>Contoso geçiş: Azure Vm'leri ve yönetilen Azure SQL örneği için bir şirket içi uygulama yeniden barındırma

Bu makale size nasıl Contoso Azure VM'ler için kendi SmartHotel uygulama ön uç VM taşır. Azure Site Recovery hizmeti ve yönetilen bir Azure SQL örneği uygulama veritabanına kullanılarak gösterir.

> [!NOTE]
> Yönetilen Azure SQL örneği şu anda önizlemede değil.

Bu belge nasıl Contoso adlı kurgusal şirket için Microsoft Azure bulut şirket kaynaklarını geçirir belge makaleleri bir dizi dördüncü değil. Seri arka plan bilgileri ve bir geçiş altyapısını ayarlamak ve farklı türde geçişler çalıştırmak nasıl çalışılacağını senaryoları bir dizi içerir. Karmaşık senaryolar büyümesine ve biz diğer makaleler zamanla eklenmesi.


**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale serisi ve kullanırız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[Makale 2: bir Azure altyapısı dağıtın](contoso-migration-infrastructure.md) | Nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar açıklar. Aynı alt tüm Contoso geçiş senaryoları için kullanılır. | Kullanılabilir
[Makale 3: şirket içi kaynakları değerlendirin](contoso-migration-assessment.md)  | Contoso VMware üzerinden çalışan kendi şirket içi iki katmanlı SmartHotel uygulamasının bir değerlendirme nasıl çalıştığı gösterilmektedir. Uygulama VM'ler ile değerlendirmek [Azure geçirmek](migrate-overview.md) hizmet ve uygulama SQL Server veritabanı ile [Azure veritabanı geçiş Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
Makale 4: Rehost Azure Vm'leri ve bir SQL yönetilen örneği (Bu makalede) | Contoso SmartHotel uygulamayı Azure'a nasıl geçirir gösterir. VM ön uç uygulamasını kullanarak geçirmek [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), uygulamayı ve veritabanı kullanma [Azure veritabanı geçiş](https://docs.microsoft.com/azure/dms/dms-overview) yönetilen bir SQL örneğine geçirmek için hizmet. | Kullanılabilir
[Makale 5: Azure VM'ler için yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso SmartHotel uygulama yalnızca Site Recovery kullanarak sanal makineleri nasıl geçirir gösterir.
[Makale 6: Azure VM'ler ve SQL Server kullanılabilirlik gruplarını yeniden barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulama nasıl geçirir gösterir. Sanal makineleri uygulama ve veritabanı geçiş hizmeti uygulama veritabanı için SQL Server kullanılabilirlik grubu geçirmek için geçirmek için Site Recovery kullanırlar. | Kullanılabilir
[Makale 7: Azure VM'ler için Linux uygulama yeniden barındırma](contoso-migration-rehost-linux-vm.md) | Contoso Linux osTicket uygulama Azure Site Recovery kullanarak sanal makineleri nasıl geçirir gösterir. | Kullanılabilir
[Makale 8: Linux uygulama Azure VM'ler ve Azure MySQL sunucusu için yeniden barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Azure Site Recovery kullanarak sanal makineleri ve MySQL çalışma ekranı kullanarak bir Azure MySQL Server örneğini Contoso Linux osTicket uygulama nasıl geçirir gösterir. | Kullanılabilir

Bu makalede kullanılan örnek SmartHotel uygulaması kullanmak istiyorsanız, buradan indirebilirsiniz [github](https://github.com/Microsoft/SmartHotel360).

## <a name="on-premises-architecture"></a>Şirket içi mimarisi

Geçerli Contoso şirket içi altyapı gösteren bir diyagram aşağıdadır.

![Contoso mimarisi](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)  

- Contoso içinde Şehir, New York, Doğu Amerika Birleşik Devletleri bulunan bir ana veri merkezi vardır.
- Amerika Birleşik Devletleri arasında üç ek yerel dalları sahiptirler.
- Ana veri merkezi bir Fiber metro ethernet bağlantısı (500 MB/sn) ile Internet'e bağlı.
- Her dal Internet'e IPSec VPN tünelleri dön ana veri merkezi ile iş sınıfı bağlantıları kullanarak yerel olarak bağlı. Böylece, kalıcı olarak bağlı olması, tüm ağ ve internet bağlantısı en iyi duruma getirir.
- Ana veri merkezi, VMware ile tam olarak sanallaştırılmış. VCenter Server 6.5 tarafından yönetilen iki ESXi 6.5 sanallaştırma ana bilgisayarı sahiptirler.
- Contoso Active Directory kimlik yönetimi ve iç ağdaki DNS sunucuları için kullanır.
- Etki alanı denetleyicileri veri merkezindeki VMware VM'ler üzerinde çalıştırın. Yerel dalları etki alanı denetleyicilerde fiziksel sunucularda çalıştırın.



## <a name="business-drivers"></a>İş sürücüleri

BT liderlik ekibindeki yakından iş ile bu geçiş elde etmek istediği anlamak için iş ortaklarıyla çalışmıştır:

- **Adres iş büyümesi**: Contoso artmaktadır ve sonuç olarak şirket içi sistemlerini ve altyapı Basıncı yoktur.
- **Verimliliği artırma**: Contoso gereken gereksiz yordamları kaldırmak ve işlemler, geliştiriciler ve kullanıcılar için verimli hale getirmek.  Hızlı olarak iş gereksinimlerini BT ve değil atık saat veya para, böylece daha hızlı müşteri gereksinimlerine gönderiliyor.
- **Çeviklik artırmak**: Contoso BT işletme ihtiyaçlarını daha iyi yanıt olması gerekir. Market'te başarılı şekilde bir genel ekonomi etkinleştirmek için değişiklikleri daha hızlı tepki mümkün olması gerekir.  Bu dpm'nin biçiminde alın veya bir iş engelleyici haline gelir.
- **Ölçek**: işi başarıyla büyüdükçe, Contoso BT aynı yükselmeye sorgulayabilmesi sistemleri sağlamanız gerekir.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut ekip hedeflerine bu geçiş için aşağı sabitlenmiş. Bu hedefleri en iyi geçiş yöntemini belirlemek için kullanılır:

- Geçişten sonra Azure uygulamasında, bugün şirket içi VMWare ortamlarında olduğu gibi aynı performans özellikleri olması gerekir.  Buluta geçiş uygulama performansı daha az önemli olduğu anlamına gelmez.
- Bu uygulamada yatırım contoso istememektedir.  Kritik ve iş önemlidir, ancak mevcut haliyle yalnızca taşıma buluta olduğu gibi istedikleri.
- Uygulama geçirildikten sonra veritabanı yönetim görevlerini en aza.
- Contoso bir Azure SQL veritabanı için bu uygulamayı kullanmak istememektedir ve alternatifleri için aramaktadır.

## <a name="proposed-architecture"></a>Önerilen mimarisi

Bu senaryoda:

- Contoso, iki katmanlı şirket içi seyahat uygulama geçirmek istiyor.
- Uygulama iki VM'ler arasında (WEBVM ve SQLVM) katmanlı ve VMware ESXi konağında **contosohost1.contoso.com** (sürüm 6.5)
- VMware ortamı vCenter Server 6.5 tarafından yönetilen (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Bunlar, Azure SQL yönetilen örneğine uygulama veritabanı (SmartHotelDB) geçirin.
- Bunlar, bir Azure VM için şirket içi VMware sanal makineleri geçirin.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'ile bir şirket içi etki alanı denetleyicisi (**contosodc1 adlı**).
- Geçiş tamamlandıktan sonra şirket içi sanal makineleri Contoso veri merkezinde hizmetten.

![Senaryo mimarisi](media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png) 

### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Veritabanı yönetim hizmeti](https://docs.microsoft.com/azure/dms/dms-overview) | DMS en az kapalı kalma süresi ile Azure veri platformlar için birden fazla veritabanı kaynaktan sorunsuz geçiş sağlar. | Hakkında bilgi edinin [bölgeler desteklenen](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) DMS ve get için [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/database-migration/).
[Azure SQL yönetilen örneği](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Bir tam olarak yönetilen SQL Server örneği Azure bulutta temsil eden bir yönetilen veritabanı hizmeti yönetilen örneğidir. SQL Server Veritabanı Altyapısı'nın en son sürümüyle aynı kodu paylaşır ve en son özellikleri, performans iyileştirmeleri ve güvenlik yamaları vardır. | Azure SQL veritabanı yönetilen örnekleri kullanılarak azure'da çalışan kapasitesini temel ücret doğurur. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Hizmet düzenler ve geçiş ve olağanüstü durum kurtarma Azure VM'ler için yönetir ve sanal makineleri ve fiziksel sunucuları şirket içi.  | Azure'a çoğaltma sırasında Azure depolama ücretleri ücrete.  Azure VM'ler oluşturulur ve yük devretme oluştuğunda ücretler, uygulanır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/) ücretleri ve fiyatlandırma hakkında.

 

## <a name="migration-process"></a>geçiş işlemi

Contoso SmartHotel uygulama web ve veri katmanlarını Azure'a geçirir.

1. Yalnızca birkaç bu senaryo için belirli Azure bileşenleri eklemek ihtiyaç duydukları şekilde Contoso Azure altyapı yerinde zaten içeriyor.
2. Veri katmanı, veri taşıma hizmeti (DMS) kullanarak geçirilecektir.  DMS şirket içi SQL Server VM Contoso veri merkezi ve Azure arasında bir siteden siteye VPN bağlantısı üzerinden bağlanır ve veritabanı geçirir.
3. Web katmanı, Azure Site Recovery ile yükseltme shift geçişi kullanarak geçirilecektir. Bu şirket içi VMware ortamını hazırlama, ayarlama ve çoğaltma etkinleştirme ve bunları Azure'a devretmek tarafından sanal makineleri geçirme oluşturulmasını gerektirir.

     ![Geçiş mimarisi](media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png) 


## <a name="prerequisites"></a>Önkoşullar

Contoso (ve) Bu senaryo için ihtiyaç duyduğu aşağıda verilmiştir.

**Gereksinimleri** | **Ayrıntılar**
--- | ---
**Önizlemede kaydetme** | SQL yönetilen örneği sınırlı genel önizlemede kayıtlı olması gerekir. Aşağıdakileri yapmak için bir Azure aboneliğine ihtiyacınız [kaydolun](https://portal.azure.com#create/Microsoft.SQLManagedInstance). Kaydolma tamamlayın, bu nedenle, bunu, bu senaryoyu dağıtmaya başlamadan önce emin olmak için birkaç gün sürebilir.
**Azure aboneliği** | Bu serideki ilk makaledeki değerlendirme gerçekleştirildiğinde, zaten bir abonelik oluşturmuş olmalıdır. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Var olan bir abonelik kullanın ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yönetici ile çalışmak için gerekir.<br/><br/> Daha ayrıntılı izinler gerekiyorsa, gözden [bu makalede](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Site Kurtarma (şirket içi)** | Şirket içi vCenter server 5.5, 6.0 veya 6.5 sürümü çalıştırması gerekir<br/><br/> 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir ESXi ana bilgisayar<br/><br/> Bir veya daha fazla VMware ESXi ana bilgisayarında çalışan sanal makineleri.<br/><br/> Sanal makineleri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Desteklenen [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) yapılandırma.
**DMS** | DMS için gereksinim duyduğunuz bir [içi uyumlu VPN cihazı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Şirket içi VPN cihazı yapılandırma olması gerekir. Dışarıya dönük bir genel IPv4 adresi olmalıdır ve bir NAT cihazının arkasında adresi bulunamıyor.<br/><br/> Şirket içi SQL Server veritabanına erişiminiz olduğundan emin olun.<br/><br/> Windows Güvenlik Duvarı'nı kaynak veritabanı altyapısı erişebilir olması gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Veritabanını makinenize önünde bir güvenlik duvarı varsa, veritabanını ve dosyalarına SMB bağlantı noktası 445 üzerinden erişime izin vermek için kurallar ekleyin.<br/><br/> Yönetilen örneğini hedeflemek ve SQL Server kaynağına bağlanmak için kullanılan kimlik bilgileri sysadmin sunucu rolünün üyeleri olmalıdır.<br/><br/> Bir ağ gerekir paylaşmak şirket içi veritabanınızda DMS kaynak veritabanını yedeklemek için kullanabilirsiniz.<br/><br/> Kaynak SQL Server örneğinin çalıştığı hizmet hesabının ağ paylaşımına yazma ayrıcalıklarına sahip olduğundan emin olun.<br/><br/> Ağ paylaşımında tam denetim ayrıcalığına sahip bir Windows kullanıcısı (ve parola) not edin. Azure veritabanı geçiş hizmeti Azure storage kapsayıcısına yedekleme dosyalarını karşıya yüklemek için bu kullanıcı kimlik bilgileri temsil eder.<br/><br/> TCP/IP Protokolü SQL Server Express yükleme işlemi ayarlar **devre dışı** varsayılan olarak. Etkin olduğundan emin olun.


## <a name="scenario-steps"></a>Senaryo adımları

Burada'nın nasıl Contoso dağıtımı ayarlamak üzere geçiyor:

> [!div class="checklist"]
> * **1. adım: yönetilen bir SQL Azure örneği ayarlama**:, şirket içi SQL Server veritabanına geçirme önceden oluşturulmuş bir yönetilen örneği gerekir.
> * **2. adım: DMS hazırlama**: veritabanı geçiş sağlayıcısını kaydetmek, örnek oluşturmak ve bir DMS projesi oluşturmak için gereksinim duydukları. Bunlar ayrıca DMS SA URI'sini ayarlamanız gerekir. Depolama nesneleri sınırlı izinleri verebilirsiniz paylaşılan erişim imzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) depolama hesabınızdaki kaynaklara yetkilendirilmiş erişim sağlar. DMS hizmetini SQL Server Yedekleme dosyalarını karşıya yükleme depolama hesabı kapsayıcısının erişebilmesi için bunlar bir SAS URI'sini ayarlayın.
> * **3. adım: Azure Site Recovery için hazırlama**: için Site Recovery, bunlar çoğaltılan verileri tutmak için bir Azure depolama hesabı oluşturma ve bir kurtarma Hizmetleri kasası oluşturmanız gerekir.
> * **4. adım: şirket içi VMware Site Recovery için hazırlama**: Contoso hesapları VM Keşif ve aracı yüklemesine hazırlamak ve yük devretme sonrasında Azure Vm'lerine bağlanmak hazırlayın.
> * **5. adım: Çoğaltma sanal makineleri**: çoğaltmayı ayarlama, bunlar Site Recovery kaynak ve hedef ortamını yapılandırmak, bir çoğaltma ilkesini ayarlayın ve Azure depolama alanına sanal makineleri çoğaltma işlemi başlatma.
> * **6. adım: DMS veritabanıyla geçirmek**: artık veritabanı geçirebilirsiniz.
> * **7. adım: Site Recovery ile sanal makineleri geçirmek**: her şeyi çalıştığından emin olmak için yük devretme testi çalıştırın ve ardından sanal makineleri Azure'a geçirmek için tam bir yük devretme çalıştırın.


## <a name="step-1-prepare-an-azure-sql-managed-instance"></a>1. adım: Azure SQL yönetilen örneği hazırlama

Yönetilen bir Azure SQL örneği ayarlamak için bir alt ağ Contoso gerekir:

- Alt ağ ayrılmış olmalıdır. Boş olması gerekir ve herhangi bir bulut hizmeti içermiyor ve bir ağ geçidi alt ağı olamaz.
- Yönetilen örneği oluşturulduktan sonra, kaynakları eklemeniz gerekir.
- Alt ağ ile ilişkilendirilmiş bir NSG olması gerekir.
- Alt ağ, kendisine atanmış yalnızca rota olarak bir kullanıcı rota tablosu (UDR) 0.0.0.0/0 sonraki atlama Internet ile olması gerekir. 
- İsteğe bağlı özel DNS: özel DNS VNet üzerinde belirtilirse, Azure'nın özyinelemeli çözümleyiciler IP adresi (örneğin, 168.63.129.16) listesine eklenmesi gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- Alt ağ ilişkili hizmet uç noktası (depolama veya SQL) olması gerekir. Hizmet uç noktaları sanal ağda devre dışı bırakılması gerekir.
- Alt ağ 16 IP adresini en az olmalıdır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances) yönetilen örneği alt boyutlandırma hakkında.
- Karma ortamlarında özel DNS ayarlarını gereklidir. Contoso bir veya daha fazla Azure DNS sunucularını kullanmak için DNS ayarlarını yapılandırır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns) DNS özelleştirme hakkında.

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>Yönetilen örneği için bir sanal ağ ayarlama

Contoso VNet gibi ayarlar: 

1. Contoso ContosoNetworkingRG kaynak grubunda birincil Doğu ABD 2 bölge içinde yeni bir sanal ağ (VNET-SQLMI-EU2) oluşturur.
2. Contoso 10.235.0.0/24, bir adres alanı aralığı Contoso Kurumsal diğer ağlarla örtüşmeyecek sağlama atar.
2. Bunlar iki alt ağa ekleyin:
    - SQLMI DS EUS2 (10.235.0.0.25)
    - SQLMI-TESTERE-EUS2 (10.235.0.128/29). Bu alt dizin yönetilen örneğine (SQLMI) eklemek için kullanılır.

    ![Yönetilen örnek ağ](media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

6. VNet ve alt dağıtıldıktan sonra Contoso ağlar gibi eşlere:

    - Eş VNET-SQLMI-EUS2 ile VNET HUB EUS2 (hub VNet Doğu ABD 2 için).
    - Eş VNET-SQLMI-EUS2 VNET üretim EUS2 (üretim ağı) sahip.

    ![Ağ eşlemesi](media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

7. Contoso özel DNS ayarlarını belirler. DNS, önce kendi Azure DC'lere işaret edecek. Azure DNS ikincil olacaktır. Contoso Azure DC'leri bulunan aşağıdaki gibidir:

    - Üretim DC EUS2 alt ağında yer alan, Doğu ABD 2 üretimde (VNET-üretim-EUS2) ağ.
    - CONTOSODC3 adresi: 10.245.42.4
    - CONTOSODC4 adresi: 10.245.42.5
    - Azure DNS Çözümleyicisi: 168.63.129.16

     ![Ağ DNS](media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

**Daha fazla yardım gerekiyor mu?**

- [Genel bir bakış elde](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) Azure SQL yönetilen örnekleri.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#create-a-new-virtual-network-for-managed-instances) yönetilen SQL örneği için bir VNet oluşturma hakkında daha fazla.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering) eşliği yukarı ayarlama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns) Azure AD DNS ayarları güncelleştiriliyor.



### <a name="set-up-routing"></a>Yönlendirmeyi kurmak

Contoso Azure yönetim hizmetiyle iletişim kurmasına izin için bir yol tablosu gereken şekilde yönetilen örnek özel bir sanal ağ içinde yer alır. Yönettiği hizmetiyle iletişim kuramıyor, erişilemez duruma gelir.

- Yol tablosu yönetilen örneğinden gönderilen paketleri VNet içinde nasıl yönlendirileceğini belirten bir dizi kuralla (rotaları) içerir.
- Yol tablosu yönetilen örnekleri dağıtılmış olduğundan alt ağlar ile ilişkilidir. Bir alt ağdan çıkan her paket ilişkili rota tablosuna dayalı ele alınır.
- Bir alt ağ yalnızca tek bir yol tablosu ile ilişkilendirilebilir.
- Microsoft Azure'da yönlendirme tabloları oluşturmak için hiçbir ek ücret vardır.

1. Contoso bir kullanıcı tanımlı yönlendirme tablosu oluşturur. Yol tablosu ContosoNetworkingRG kaynak grubunda oluşturulur.

    ![Rota tablosu](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

2. Contoso (MIRouteTable) yönlendirme tablosunu dağıtıldıktan sonra yönetilen örneği gereksinimlerine uymak için 0.0.0.0/0, bir adres öneki sahip bir yol ekleyin ve **sonraki atlama türü** kümesine **Internet**.

    ![Rota tablosu öneki](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)
    
3. Contoso yol tablosu SQLMI DB EUS2 ağındaki alt ağ (VNET SQLMI EUS2) ile ilişkilendirin. 

    ![Rota tablosu alt ağ](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)
    
**Daha fazla yardım gerekiyor mu?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal#create-new-route-table-and-a-route) yollar yönetilen bir örneği için ayarlama.

### <a name="create-a-managed-instance"></a>Yönetilen bir örneği oluşturma

Şimdi, Contoso yönetilen bir SQL veritabanı örneği sağlayabilirsiniz.

1. Yönetilen örnek bir iş uygulamasını gördüğünden Contoso Dağıt, ContosoRG kaynak grubunda birincil kendi Doğu ABD 2 bölgesinde, 
2. Bunlar, bir fiyatlandırma katmanı ve boyutu işlem ve depolama örneği için seçin. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/sql-database/managed/) fiyatlandırma hakkında.

    ![Yönetilen örnek](media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

3. Yönetilen örneği dağıtıldıktan sonra iki yeni kaynaklar ContosoRG kaynak grubunda görünür:

    - Bir sanal birden çok yönetilen örnekleri olması durumunda küme
    - SQL Server örneği yönetilen. 

    ![Yönetilen örnek](media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

**Daha fazla yardım gerekiyor mu?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal) yönetilen örneği sağlama.

## <a name="step-2-prepare-dms"></a>2. adım: DMS hazırlama

DMS Contoso hazırlamak için şunları yapması gerekir:

- DMS sağlayıcısı Azure'a kaydetme
- DMS bir veritabanı geçirmek için kullanılan yedekleme dosyaları karşıya yükleme için Azure depolamaya erişim sağlar. Bunlar, bir blob depolama kapsayıcısını oluşturarak bunu ve bir paylaşılan erişim imzası (SAS) URI oluşturmak. 
- DMS projesi oluşturun.


Adımları aşağıdaki gibi tamamlayın:

1. Contoso veritabanı geçiş sağlayıcı aboneliğini altında kaydedin.
    ![DMS kaydetme](media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

2. Bunlar bir depolama blob kapsayıcısını oluşturun ve DMS erişebilmesi bir SAS URI'sini oluşturur.

    ![SAS URI'Sİ](media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

3. Son olarak, bir DMS örneği oluşturun. 

    ![DMS örneği](media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

4. Contoso DMS örneği üretim DC EUS2 VNET VNet üretim DC EUS2 alt ağ yerleştirir.
    - Şirket içi SQL Server VM bir VPN ağ geçidi üzerinden erişebilen bir sanal ağda olması gerekir çünkü bunlar onu var. yerleştirin.
    - VNET üretim EUS2 için VNET-HUB-EUS2 eşlenirse ve uzak ağ geçitleri kullanmasına izin verilir.  Bu, DMS gerektiği şekilde iletişim kurabilir sağlar.

        ![DMS ağ](media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

**Daha fazla yardım gerekiyor mu?**
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/dms/quickstart-create-data-migration-service-portal) DMS ayarlama.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-2) oluşturma ve SAS kullanma hakkında.


## <a name="step-3-prepare-azure-for-the-site-recovery-service"></a>3. adım: Azure Site Recovery hizmeti için hazırlama

Azure öğeleri Contoso kendi web katmanı VM (WEBMV) geçiş için Site Recovery ayarlamak için gereken bir dizi vardır

- Sanal ağ içinde hangi devredilir kaynakları konumlandırıldığını.
- Çoğaltılan verileri tutmak için bir Azure depolama hesabı. 
- Azure kurtarma Hizmetleri kasasına.

Bunlar Site Recovery aşağıdaki gibi ayarlayın:

1. VM bir web ön uç SmartHotel uygulama olduğundan, varolan bir üretim ağı (VNET-üretim-EUS2) ve alt ağ (üretim-FE-EUS2) birincil Doğu ABD 2 bölgesindeki VM üzerinden başarısız olur. Bu ağ ne zaman kurma ayarlamak bunlar [Azure altyapı dağıtılan](contoso-migration-infrastructure.md)
2. Bunlar, bir Azure depolama hesabı (contosovmsacc20180528) oluşturur. Standart depolama ve LRS çoğaltma ile bunların bir genel amaçlı hesabı kullanıyorsanız.

    ![Site Kurtarma depolama alanı](media/contoso-migration-rehost-vm-sql-managed-instance/asr-storage.png)

3. Yerinde ağ ve depolama hesabıyla Contoso artık bir kasa (ContosoMigrationVault) oluşturabilir ve ContosoFailoverRG kaynak grubunda birincil Doğu ABD 2 bölge yerleştirin.

    ![Kurtarma Hizmetleri kasası](media/contoso-migration-rehost-vm-sql-managed-instance/asr-vault.png)

**Daha fazla yardım gerekiyor mu?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) Azure Site Recovery için ayarlama.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>4. adım: şirket içi VMware Site Recovery için hazırlama

VMware Site kurtarma için hazırlamak üzere İşte Contoso yapmanız gerekir:

- VM bulma otomatikleştirmek için vCenter sunucusu veya vSphere ESXi ana bilgisayarında, bir hesap hazırlayın.
- Mobility hizmetinin otomatik olarak yüklenmesini çoğaltmak istediğiniz VMware Vm'lerinde sağlayan bir hesap hazırlayın.
- Yük devretme sonrasında oluşturulduğunda Azure Vm'lerinin bağlanabilmesi için şirket içi VM'ler hazırlayın.


### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- Sanal makineleri otomatik olarak bulur. En az bir salt okunur hesap gereklidir.
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme. Oluşturma ve diskleri kaldırma ve sanal makine kapatma gibi işlemleri çalıştırılan bir hesap gerekir.

Contoso hesabı gibi ayarlar:

1. Bir rol vCenter düzeyinde oluşturur.
2. Bu rol için gerekli izinleri atar.

**Daha fazla yardım gerekiyor mu?**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) oluşturma ve Otomatik bulma için bir rol atama.

### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Çoğaltmak istediğiniz sanal makinede Mobility hizmeti yüklü olmalıdır.

- Sanal makine için çoğaltmayı etkinleştirdiğinizde, site kurtarma Bu bileşen otomatik itme yüklemesi yapabilirsiniz.
- Otomatik gönderme yüklemesi için Site Recovery VM erişmek için kullanacağı bir hesap hazırlamanız gerekir.
- Azure konsolunda çoğaltma ayarladığınızda bu hesabı belirtin.
- Bir etki alanı veya yerel hesap VM'de yüklemek için gerekli izinlere sahip gerekir.

**Daha fazla yardıma gereksinim**

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) mobilite hizmetinin göndermeli yüklemesi için bir hesap oluşturma.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Azure yük devretme işleminden sonra Contoso çoğaltılan Azure vm'lerinin bağlanabilmesi istiyor. Bunu yapmak için geçiş çalıştırmadan önce şirket içi VM yapmak için gereksinim duydukları şunları vardır: 

1. İnternet üzerinden erişim, bunlar yük devretmeden önce şirket içi VM üzerinde RDP etkinleştirmek ve için TCP ve UDP kurallarının eklendiğinden emin olun **ortak** profili ve RDP izin verildiğinden emin **Windows Güvenlik Duvarı**  >  **İzin verilen uygulamaları** tüm profiller için.
2. Kendi siteden siteye VPN üzerinden erişim, bunlar şirket içi makinede RDP etkinleştirmek ve RDP izin **Windows Güvenlik Duvarı** -> **verilen uygulamalar ve Özellikler** için **etki alanı ve Özel** ağlar.
3. Bunlar işletim sisteminin SAN ilkesinin şirket içi VM Ayarla **OnlineAll**.

Ayrıca, bir yük devretme çalıştırdığınızda, bunlar aşağıdakileri denetleyin gerekir:

- Olmamalıdır bekleyen hiçbir Windows güncelleştirmeleri VM üzerinde bir yük devretme tetiklendiğinde. Varsa, güncelleştirme tamamlanana kadar sanal makineye oturum açmak değiştiremezler.
- Yük devretme sonrasında, bunlar denetlemelisiniz **önyükleme tanılama** ekran VM görüntüsünü görüntülemek için. Bu işe yaramazsa, bunlar VM çalıştıran ve bunlar gözden denetlemelisiniz [sorun giderme ipuçları](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>5. adım: Azure Site Recovery ile şirket içi Vm'lerini çoğaltma

Bir geçiş için Azure çalıştırmadan önce Contoso çoğaltmayı ayarlama ve kendi şirket içi sanal makine için çoğaltmayı etkinleştirmek gerekir.

### <a name="set-a-replication-goal"></a>Bir çoğaltma hedefi ayarlama

1. Kasasına kasa adı (ContosoVMVault) altında çoğaltma hedefi ayarlamak (**Başlarken** > **Site Recovery** > **altyapıyıhazırlama**.
2. Bunlar kendi makinelerine şirket içinde olduğundan, VMware Vm'lerini ve azure'a istediği olduğunuzu belirtin.

    ![Çoğaltma hedefi](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Dağıtım Planlama onaylayın

Devam etmek için bunlar seçerek dağıtım planlamasını tamamladınız onaylamak ihtiyaç duydukları **Evet, yaptım**. Bu dağıtımda Contoso yalnızca tek bir VM geçirme olan ve dağıtım planlama olması gerekmez.

### <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Şimdi kendi kaynak ortamını yapılandırmak gerekir. Bunu yapmak için bunlar bir OVF şablonu yükleme ve yapılandırma sunucusunun ve yüksek oranda kullanılabilir olarak ilişkili bileşenlerini dağıtmak için kullanın, şirket içi VMware VM. Sunucu üzerindeki bileşenler şunlardır:

- Şirket içi ve Azure arasındaki iletişimi düzenler ve veri çoğaltma yöneten yapılandırma sunucusu.
- Bir çoğaltma ağ geçidi olarak davranır işlem sunucusu. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir.
- İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.
- VM oluşturulan başlatıldığında ve yapılandırma sunucusu sonra Contoso kasaya bunu kaydedebilirsiniz.


Contoso adımları aşağıdaki gibi gerçekleştirin:

1. Azure portalından OVF şablonu indirdikleri (**altyapıyı hazırlama** > **kaynak** > **yapılandırma sunucusu**).
    
    ![OVF indirin](./media/contoso-migration-rehost-vm-sql-managed-instance/add-cs.png)

2. Bunlar, oluşturma ve VM dağıtmak için VMware şablonu içeri aktarın.

    ![OVF şablonu](./media/contoso-migration-rehost-vm-sql-managed-instance/vcenter-wizard.png)

3.  VM üzerinde ilk kez açtığınızda, Windows Server 2016 yükleme deneyimi ön. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
4. Yükleme tamamlandıktan sonra bunların VM'ye yönetici olarak oturum açın. İlk kez oturum açma sırasında varsayılan olarak Azure Site kurtarma Yapılandırması aracını çalıştırır.
5. Aracı, bunlar yapılandırma sunucusu kasasına kaydederken kullanmak için bir ad belirtin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra seçtikleri **oturum**, Azure aboneliğine oturum açmak için. Kimlik bilgilerini yapılandırma sunucusu kaydedilecek kasa erişimi olmalıdır. 

    [Yapılandırma sunucusu kaydetme](./media/contoso-migration-rehost-vm-sql-managed-instance/config-server-register2.png)

7. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır. Kullanıcılar için makine yeniden oturum açın ve yapılandırma sunucusu yönetim Sihirbazı'nı otomatik olarak başlatır.
8. Sihirbazda, çoğaltma trafiği almaya NIC seçerler. Bunu yapılandırıldıktan sonra bu ayar değiştirilemez.
9. Bunlar, abonelik, kaynak grubu ve yapılandırma sunucunun kaydedileceği kasayı seçin.
        ![Kasa](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz1.png) 

10. Bunlar daha sonra indirin ve MySQL Server ve VMWare Powerclı yükleyin. Ardından, sunucu ayarlarını doğrulayın.
11. Doğrulama sonrasında, bunlar vCenter sunucusu veya vSphere ana bilgisayar FQDN veya IP adresini belirtin. Bunlar varsayılan bağlantı noktası bırakın ve Azure'da vCenter sunucusu için bir kolay ad belirtin.
12. Bunlar, böylece Site Recovery çoğaltma için kullanılabilir olan VMware VM'ler otomatik olarak bulabilir daha önce oluşturduğunuz hesabı belirtmeniz gerekir. 
13. Bunlar, çoğaltma etkin olduğunda otomatik olarak mobilite hizmetini yüklemek için kimlik bilgilerini belirtin. Windows makineler için hesap VM'ler üzerinde yerel yönetici ayrıcalıkları gerekiyor. 

    ![vCenter](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz2.png)

7. Kayıt tamamlandığında Azure portalında Contoso çift VMware sunucusu ve yapılandırma sunucusu listelendiğini denetler **kaynak** kasası sayfasında. Bulma, 15 dakika veya daha fazla sürebilir. 
8. Site Recovery belirtilen ayarları kullanarak VMware sunucularına bağlanır ve sanal makineleri bulur.

### <a name="set-up-the-target"></a>Hedefi ayarlama

Hedef çoğaltma ortamı yapılandırmak, artık Contoso gerekir.

1. İçinde **altyapıyı hazırlama** > **hedef**, hedef ayarlarını seçin.
2. Site Recovery, bir Azure depolama hesabı ve belirtilen hedef ağında olup olmadığını denetler.

### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Kaynak ve hedef ayarlandıktan sonra Contoso bir çoğaltma ilkesi oluşturmak ve yapılandırma sunucusuyla ilişkilendirmek hazırdır.

1. İçinde **altyapıyı hazırlama** > **çoğaltma ayarları** > **Çoğaltma İlkesi** >  **oluşturma ve İlişkilendirme**, bir ilke oluşturmak **ContosoMigrationPolicy**.
2. Bunlar varsayılan ayarları kullanın:
    - **RPO eşiği**: varsayılan 60 dakika. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
    - **Kurtarma noktası bekletme**. Varsayılan 24 saatlik. Bu değer ne kadar bekletme penceresi için her kurtarma noktası belirtir. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**. Varsayılan bir saat. Bu değer oluşturulmasında ve uygulamayla tutarlı anlık görüntü sıklığı belirtir.
 
        ![Çoğaltma ilkesi oluşturma](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy.png)

5. İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. 

    ![Çoğaltma İlkesi ilişkilendirme](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy2.png)


**Daha fazla yardım gerekiyor mu?**

- Bu adımları tam bir kılavuz okuyabilirsiniz [şirket içi VMware Vm'leri için olağanüstü durum kurtarma kümesi](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Ayrıntılı yönergeler size yardımcı olmak için kullanılabilir [kaynak ortamını ayarlama](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [yapılandırma sunucusu dağıtmak](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), ve [çoğaltma ayarlarını yapılandırın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).

### <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Şimdi Contoso WebVM çoğaltma başlatabilirsiniz.

1. İçinde **uygulama çoğaltma** > **kaynak** > **+ Çoğalt** kaynak ayarlarını seçin.
2. Bunlar, bunlar, sanal makineleri etkinleştirmek istiyorsanız, vCenter sunucusu ve yapılandırma sunucusu seçin gösterir.

 ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication1.png)
 
3. Şimdi, kaynak grubu ve ağ içinde Azure VM yük devretme sonrasında bulunur ve çoğaltılan veriler depolanacağı depolama hesabı da dahil olmak üzere hedef ayarları belirtin.

     ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication2.png)

4. Contoso çoğaltma WebVM seçer. Site Recovery Mobility hizmeti her VM yükler çoğaltmayı etkinleştirebilirsiniz. 

    ![Çoğaltmayı etkinleştirme](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication3.png)

5. Contoso doğru çoğaltma ilkesi seçilir ve WEBVM için çoğaltmayı etkinleştirir denetler. Çoğaltma sürüyor izlemek **işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.
6. İçinde **Essentials** Azure portalında, Azure'a çoğaltma VM'ler için yapısı Contoso görebilirsiniz.

    ![Altyapı görünümü](./media/contoso-migration-rehost-vm-sql-managed-instance/essentials.png)


**Daha fazla yardım gerekiyor mu?**

Bu adımları tam bir kılavuz okuyabilirsiniz [çoğaltmasını etkinleştir](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).

## <a name="step-6-migrate-the-database-with-dms"></a>6. adım: DMS veritabanıyla geçirme

Contoso DMS proje oluşturma ve veritabanını geçirme gerekiyor.

### <a name="create-a-dms-project"></a>DMS projesi oluşturma
1. Bunlar DMS projesi oluşturun. Bunlar, SQL Server ve Azure SQL veritabanı yönetilen örneği olarak hedef olarak kaynak sunucu türünü belirtin.

     ![DMS proje](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

2. Projesi oluşturduktan sonra Geçiş Sihirbazı'nı açar.

### <a name="migrate-the-database"></a>Veritabanını geçirme 

1. Geçiş Sihirbazı'nda Contoso kaynak VM üzerinde şirket içi veritabanına bulunur ve erişmek için kimlik bilgilerini belirtir.

    ![DMS kaynak](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

2. Bunlar (SmartHotel.Registration) geçirmek için veritabanını seçin.

    ![DMS kaynak veritabanı](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-sourcedb.png)

3. Hedef olarak, bunlar Azure ve erişim kimlik bilgileri yönetilen örneğinin adını belirtin.

    ![DMS hedef](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

4. Ardından **+ yeni etkinlik** > **çalıştırmak geçiş**, geçiş çalıştırmak için ayarları belirtin:
    - Kaynak ve hedef kimlik bilgileri.
    - Geçirmek için veritabanı.
    - Ağ paylaşımı üzerinde şirket içi VM oluşturma. DMS bu paylaşıma kaynak yedeklemeler alır.
        - Örneğinin olmalıdır kaynağı SQL Server çalıştıran hizmet hesabını, bu paylaşımda ayrıcalıkları yazma.
        - Paylaşım FQDN yolunu belirtin.
    - DMS hizmet geçiş için yedekleme dosyalarının karşıya yükleme depolama hesabının kapsayıcıya erişimi sağlayan SAS URI'si.

        ![DMS ayarları](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

5. Bunlar geçiş kaydedin ve çalıştırın.
6. İçinde **genel bakış**, geçiş durumunu izleyin.

    ![DMS İzleyicisi](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

7. Geçiş tamamlandıktan sonra bunların hedef veritabanlarına yönetilen örneğinde var olduğunu doğrulayın.

    ![DMS İzleyicisi](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vm-with-site-recovery"></a>7. adım: Site Recovery ile VM geçirme

Contoso hızlı sınama yük devretme çalıştırır ve ardından VM geçirin.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Geçirmeden önce WEBVM, yük devretme testi yardımcı olan her şeyin beklendiği gibi çalıştığından emin olun. 

1. Yük devretme testi, zaman içinde son noktasına çalıştırdıkları (**son işlenen**).
2. Seçtikleri **yük devretme işlemine başlamadan önce makineyi kapatın**, böylece yük devretme tetiklemeden önce kaynak VM kapatmak Site Recovery çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. 
3. Sınama yük devretme çalıştırır: 

    - Tüm geçiş için gereken koşulların karşılandığından emin olmak için çalışır bir ön koşullarını denetleyin.
    - Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
    - Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.
3. Yük devretme işlemi tamamlandıktan sonra çoğaltma Azure portalında Azure VM görüntülenir. Bunlar, her şeyi düzgün çalıştığından emin olun. VM'nin uygun boyutta olduğundan, doğru ağa bağlı ve çalışır. 
4. Yük devretme sınaması doğruladıktan sonra Contoso yük devretme ve kayıtları temizler ve gözlemlerinizi kaydeder. 

### <a name="migrate-the-vm"></a>VM geçirme

1. Yük devretme sınaması beklenen şekilde çalıştığını doğruladıktan sonra Contoso geçiş için bir kurtarma planı oluşturur. Bunlar WEBVM plana ekleyin.

     ![Kurtarma planı](./media/contoso-migration-rehost-vm-sql-managed-instance/recovery-plan.png)

2. Ardından, bunlar bir yük devretme plan üzerinde çalıştırın. Bunların en son kurtarma noktası seçin ve Site Recovery yük devretme tetiklemeden önce şirket içi VM Kapatma denemesi gerektiğini belirtin.

    ![Yük devretme](./media/contoso-migration-rehost-vm-sql-managed-instance/failover1.png)

3. Yük devretme sonrasında Contoso doğrulayın Azure VM Azure portalında beklendiği gibi görünür.

   ![Kurtarma planı](./media/contoso-migration-rehost-vm-sql-managed-instance/failover2.png)

4. Azure'da VM doğruladıktan sonra geçiş işlemini tamamlayın, sanal makine için çoğaltmayı durdurma ve Site Recovery Faturalaması VM durdurmak için geçişi tamamlayın.

    ![Yük devretme](./media/contoso-migration-rehost-vm-sql-managed-instance/failover3.png)

### <a name="update-the-connection-string"></a>Bağlantı dizesi güncelleştir

Geçiş işleminin son adımı olarak Contoso kendi SQL mı üzerinde çalışan geçirilmiş veritabanına işaret etmek için uygulama bağlantı dizesini güncelleştirir.

1. Azure portalında bağlantı dizesi tıklayarak buldukları **ayarları** > **bağlantı dizeleri**.

    ![Yük devretme](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)  

2. Bunlar kullanıcı adı ve parola yönetilen SQL örneğinin dize güncelleştirin.
3. Dize yapılandırıldıktan sonra bunlar kendi uygulamanın web.config dosyasında geçerli bağlantı dizesini değiştirin.
4. Dosyayı güncelleştirme ve kaydettikten sonra IIS WEBVM üzerinde yeniden başlatın. Bu ile **IISRESET /RESTART** bir komut isteminden.
5. IIS yeniden başlatıldıktan sonra uygulamayı yönetilen SQL örneğinde çalışan veritabanını kullanacak.
6. Bu noktada Contoso SQLVM makine şirket içi kapatabilir ve geçiş tamamlanır.

**Daha fazla yardım gerekiyor mu?**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) yük devretme testi çalıştırma. 
- [Bilgi](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) bir kurtarma planı oluşturma.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) Azure'a yük devrediliyor.

## <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Tam geçiş, bir Azure VM SmartHotel uygulama çalışırken ve yönetilen Azure SQL örneğinde SmartHotel veritabanı kullanılabilir.  

Şimdi, Contoso bazı temizleme gibi yapmalıdır:  

- WEBVM makine vCenter stoktan kaldırın.
- SQLVM makine vCenter stoktan kaldırın.
- WEBVM ve SQLVM yerel yedekleme işlerden kaldırın.
- Güncelleştirme iç belgelerine WEBVM için IP adresi yeni konumu gösterir.
- SQLVM belgelerinden kaldırın. Alternatif olarak, bunlar silinmiş olarak göstermek ve artık VM'yi stok için işaretleyebilir.
- Kullanımdan Kaldırılan sanal makineleri ile etkileşim herhangi bir kaynağa gözden geçirin ve herhangi bir ilgili ayarları veya belge yeni yapılandırmayı yansıtacak şekilde güncelleştirin.

## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Contoso Azure geçirilen kaynakları ile tam olarak faaliyete ve yeni altyapılarını güvenli gerekir.

### <a name="security"></a>Güvenlik

Contoso güvenlik ekibi, Azure Vm'leri ve SQL örneği, yönetilen uygulaması ile ilgili tüm güvenlik sorunları belirlemek için inceler.

- Bunlar, erişimi denetlemek üzere bağlı olarak, VM için ağ güvenlik grupları (Nsg'ler) gözden geçirin. Nsg'ler yalnızca uygulama için izin verilen trafiğin geçebilmesi emin olmak için kullanılır.
- Bunlar ayrıca Azure Disk şifrelemesi ve Azure KeyVault kullanarak disk üzerindeki verilerin güvenliğini sağlama değerlendiriyorsanız.
- Bunlar tehdit algılama üzerinde SQL yönetilen örneği (SQLMI) etkinleştirin. Bunlar, Uyarılar'ı bir tehdit algılanırsa bilet için kendi güvenlik takım/hizmet Masası sistemine gönderir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-threat-detection).

     ![Yönetilen örnek güvenlik](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)  

[Daha fazla bilgi](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) VM'ler için güvenlik uygulamaları hakkında.

### <a name="backups"></a>Yedeklemeler
Contoso Azure Yedekleme hizmetini kullanarak WEBVM üzerinde verileri yedeklemek için geçiyor. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Lisans ve maliyet en iyi duruma getirme

1. Contoso WEBVM için var olan lisans var ve Azure karma avantajı özelliğinden yararlanır.  Bunlar, bu fiyatlandırma avantajlarından yararlanmak için var olan Azure VM dönüştürür.
2. Contoso Azure maliyeti Cloudyn, Microsoft ofisine lisanslı yönetim olanağı sağlar. Bu, kullanmasına ve Azure ve diğer bulut kaynakları yönetmenize yardımcı olan bir çok bulut maliyeti yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyeti yönetimi hakkında. 


## <a name="conclusion"></a>Sonuç

Bu makalede, Azure Site Recovery hizmetini kullanarak uygulama ön uç VM geçirerek Contoso Azure SmartHotel uygulamada rehosted. Bir Azure SQL yönetilen DMS kullanma örneği için şirket içi veritabanına geçirildiklerinden.

## <a name="next-steps"></a>Sonraki adımlar

Serideki sonraki makalede Contoso Azure VM'ler SmartHotel uygulamaya nasıl yeniden barındırma yalnızca Azure Site Recovery hizmetini kullanarak göstereceğiz.

