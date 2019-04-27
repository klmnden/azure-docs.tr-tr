---
title: Azure Vm'leri ve Azure SQL veritabanı yönetilen örneği için geçiş yaparak Contoso şirket içi uygulama barındırma | Microsoft Docs
description: Contoso şirket içi uygulama Azure sanal makinelerinde ve Azure SQL veritabanı yönetilen örneği'ni kullanarak nasıl rehosts öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: 832614c46f0269460245d081f20897b591e31fce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60671308"
---
# <a name="contoso-migration-rehost-an-on-premises-app-on-an-azure-vm-and-sql-database-managed-instance"></a>Contoso geçişi: Bir Azure VM ve SQL veritabanı yönetilen örneği üzerinde bir şirket içi uygulama yeniden barındırma

Bu makalede, Contoso, SmartHotel360 uygulama geçirir. Azure Site Recovery hizmetini kullanarak bir Azure VM için ön uç VM'si. Contoso, uygulama veritabanını Azure SQL veritabanı yönetilen örneği için ayrıca geçirir.

> [!NOTE]
> Azure SQL veritabanı yönetilen örneği şu anda Önizleme aşamasındadır.

Bu makalede, şirket içi kaynaklara Contoso adlı kurgusal şirketin Microsoft Azure bulutuna nasıl geçirdiğini belgeleri makaleler serisinin biridir. Seri arka plan bilgileri ve bir dizi geçiş altyapısını kurma ve farklı türde geçiş çalıştırmak nasıl çalışılacağını senaryolar içerir. Senaryoları, karmaşık hale gelmesi. Makaleler, zaman içinde serinin eklenir.


**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[1. makale: Genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale dizisini ve dizisinde kullanılan örnek uygulamalar genel bakış. | Kullanılabilir
[2. makale: Bir Azure altyapısı dağıtma](contoso-migration-infrastructure.md) | Contoso şirket içi altyapısını ve Azure altyapısını geçiş için hazırlar. Altyapıyı, serideki tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: Şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](contoso-migration-assessment.md) | Contoso, Wmware'de çalışan kendi şirket içi iki katmanlı SmartHotel uygulamasının bir değerlendirme çalışır. Contoso uygulaması Vm'leri kullanarak değerlendirir [Azure geçişi](migrate-overview.md) hizmeti. Contoso kullanarak uygulama SQL Server veritabanı değerlendirir [Data Migration Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
4. makale: Bir Azure VM ve SQL veritabanı yönetilen örneği bir uygulamada barındırma | Contoso, Azure'a lift-and-shift ile taşıma geçiş için kendi şirket içi SmartHotel uygulaması çalışır. Contoso uygulaması geçirir kullanarak ön uç VM'si [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso geçirir uygulama veritabanını Azure SQL veritabanı yönetilen örneği için kullanarak [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Bu makalede
[Makale 5: Bir uygulamayı Azure vm'lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso, kendi SmartHotel uygulama sanal makinelerini Site Recovery hizmetini kullanarak Azure Vm'lerine geçirir. | Kullanılabilir
[Makale 6: Azure sanal makinelerinde ve SQL Server AlwaysOn Kullanılabilirlik grubuna bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulamaya geçirir. Contoso, uygulama sanal makinelerini geçirmek için Site Recovery kullanır. Veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için kullanır. | Kullanılabilir
[Makale 7: Azure vm'lerinde Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Contoso, Site Recovery kullanarak Azure vm'lerine kendi Linux osTicket uygulamasının lift-and-shift ile taşıma geçiş tamamlanır. | Kullanılabilir
[Makale 8: MySQL için Azure sanal makineler ve Azure veritabanı üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso, Linux osTicket uygulaması, Site Recovery kullanarak Azure Vm'lerine geçirir. Bu uygulama veritabanı için Azure veritabanı için MySQL MySQL Workbench kullanarak geçirir. | Kullanılabilir
[Makale 9: Bir Azure web uygulaması ve Azure SQL veritabanı içinde bir uygulamayı yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Contoso, bir Azure web uygulaması için kendi SmartHotel uygulama geçirir ve uygulama veritabanının Azure SQL Server örneğine geçirir. | Kullanılabilir
[Makale 10: MySQL için bir Azure web uygulaması ve Azure veritabanı'nda bir Linux uygulama yeniden düzenleyin](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso, Linux osTicket uygulaması birden çok siteye bir Azure web uygulamasında geçirir. Web uygulaması için sürekli teslimi GitHub ile tümleşiktir. Contoso uygulaması veritabanı örneği MySQL için Azure veritabanı geçirir. | Kullanılabilir
[11. makale: Azure DevOps Hizmetleri Team Foundation Server'da yeniden düzenleyin](contoso-migration-tfs-vsts.md) | Contoso geçiş yaparak kendi şirket içi Team Foundation Server dağıtımı geçirir, azure'da Azure DevOps hizmetlerine. | Kullanılabilir
[Makale 12: Bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso Azure SmartHotel, uygulama geçirir ve sonra uygulamayı rearchitects. Contoso rearchitects olarak bir Windows kapsayıcı uygulaması web katmanı ve Azure SQL veritabanı kullanarak uygulama veritabanı rearchitects. | Kullanılabilir
[Makale 13: Uygulamanızı Azure'a yeniden oluşturun](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, Azure App Service, Azure Kubernetes hizmeti, Azure işlevleri, Azure Bilişsel hizmetler ve Azure Cosmos DB dahil olmak üzere çeşitli kullanarak kendi SmartHotel uygulaması oluşturur. | Kullanılabilir
[Makale 14: Azure'a geçiş ölçeklendirin](contoso-migration-scale.md) | Geçiş birleşimleri denedikten sonra Contoso Azure tam geçişi ölçeklendirilebilecek şekilde hazırlar. | Kullanılabilir




Bu makalede kullanılan örnek SmartHotel360 uygulamasını indirebileceğiniz [GitHub](https://github.com/Microsoft/SmartHotel360).



## <a name="business-drivers"></a>İş sürücüleri

Contoso'nun BT liderlik ekibindeki çalıştığınız yakından iş bu geçişle elde etmek istediği anlamak için şirketin iş ortakları:

- **Adres büyütmeye**: Contoso artmaktadır. Sonuç olarak, şirketin şirket içi sistemler ve altyapı baskısı arttı.
- **Verimliliği artırmak**: Contoso, gereksiz yordamları kaldırın ve işlemleri, geliştiriciler ve kullanıcılar için kolaylaştırmak için gerekir. Hızlı ve değil atık zaman veya para şirket kadar olacak şekilde iş gereksinimlerini BT müşteri gereksinimlerine daha hızlı sunabilirsiniz.
- **Çevikliği artırın**:  Contoso BT işletme ihtiyaçlarını daha duyarlı olmamız gerekir. Market'te genel ekonomisine başarılı olması şirket için gerçekleşen değişiklikleri, daha hızlı tepki vermek mümkün olması gerekir. Contoso BT gereken şekilde alınamadı veya bir iş engelleyici haline gelir.
- **Ölçek**: Şirketin başarıyla büyüdükçe, Contoso BT sistemlerinin aynı hızda büyüyebilir sağlamanız gerekir.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut ekibi, bu geçiş için hedef belirlemiştir. Şirket, en iyi geçiş yöntemini belirlemek için geçiş hedefleri kullanır.

- Geçişten sonra uygulamanızı Azure'a uygulamasının Contoso'nun şirket içi VMWare ortamınızda bugün olduğu aynı performans özelliklerine sahip olmalıdır. Buluta geçiş, uygulama performansını daha az kritik olduğundan gelmez.
- Contoso, uygulamada yatırım yapmaya istememektedir. Kritik ve önemli iş uygulamadır, ancak Contoso yalnızca uygulama mevcut haliyle buluta taşımak istiyor.
- Uygulama geçirildikten sonra veritabanı yönetim görevlerinizi en aza.
- Contoso, bu uygulama için bir Azure SQL veritabanı kullanmak istememektedir. Alternatifleri için bakar.


## <a name="solution-design"></a>Çözüm tasarımı

Sonra bunların hedefleri ve gereksinimleri sabitleme, Contoso tasarlar ve bir dağıtım çözümü inceler ve geçiş için kullanacağınız Azure hizmetlerini de dahil olmak üzere geçiş işlemi tanımlar.

### <a name="current-architecture"></a>Geçerli mimari 

- Contoso olan bir ana veri merkezi (**contoso-datacenter**). Veri Merkezi içinde Şehir, İstanbul, Doğu ABD bulunur.
- Contoso, Amerika Birleşik Devletleri arasında üç ek yerel dalları sahiptir.
- Ana veri merkezinde bir fiber Metro Ethernet bağlantısı (500 MB/sn) ile İnternet'e bağlı.
- Her dal, ana merkezine IPSec VPN tüneli ile işletme sınıfı bağlantıları kullanarak internet'e yerel olarak bağlıdır. Kurulum, Contoso'nun kalıcı olarak bağlı tüm ağ sağlar ve internet bağlantısı en iyi duruma getirir.
- Ana veri merkezinin VMware ile tam olarak sanallaştırılır. Contoso, vCenter Server 6.5 tarafından yönetilen iki ESXi 6.5 sanallaştırma konaklarını sahiptir.
- Contoso, kimlik yönetimi için Active Directory kullanır. Contoso DNS sunucuları iç ağda kullanır.
- Contoso olan bir şirket içi etki alanı denetleyicisi (**contosodc1 adlı**).
- Etki alanı denetleyicileri, VMware Vm'lerinde çalıştırın. Yerel dalları etki alanı denetleyicilerde fiziksel sunucularda çalıştırın.
- SmartHotel360 uygulama, iki VM arasında katmanlı (**WEBVM** ve **SQLVM**) bir VMware ESXi sürüm 6.5 konakta bulunan (**contosohost1.contoso.com**). 
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**) bir VM'de çalışan.

![Geçerli Contoso mimarisi](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)  

### <a name="proposed-architecture"></a>Önerilen mimarisi

Bu senaryoda Contoso, iki katmanlı şirket içi seyahat uygulamasını şu şekilde geçirme ister:

- Uygulama veritabanını geçirme (**SmartHotelDB**) bir Azure SQL veritabanı yönetilen örneği.
- Ön uç geçirme **WebVM** Azure VM'ye.
- Geçiş tamamlandığında, şirket içi Vm'leri Contoso veri merkezinde kullanımdan.

![Senaryo mimarisi](media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png) 

### <a name="database-considerations"></a>Veritabanı konuları

Çözüm tasarımı işleminin bir parçası olarak, bir Azure SQL veritabanı ve SQL Server yönetilen örneği arasında özellik karşılaştırması Contoso vermedi. Aşağıdaki konular bunları yönetilen örneği ile Git karar vermek için yardımcı olmuştur.

- En son şirket içi SQL Server sürümü ile neredeyse % 100 uyumluluk sağlamak için örnek aıms yönetilen. Microsoft, en az bir tasarım değişiklikleri ile tam olarak yönetilen bir hizmet uygulamalarını geçirmek isteyen bir Iaas VM üzerindeki veya şirket içi SQL Server çalıştıran müşteriler için yönetilen örnek önerir.
- Contoso, çok sayıda uygulamaları şirket içinden için Iaas geçirmek planlama. Bunların çoğu, sağlanan ISV sertifikalıdır. Contoso yönetilen örneği'ni kullanarak SQL veritabanı, desteklenmeyen kullanmak yerine bu uygulamalar için veritabanı uyumluluk olmanıza yardımcı olur, fark etti.
- Contoso yalnızca yönetilen örneğe lift-and-shift ile taşıma geçiş tam otomatik veri geçiş hizmeti (DMS) kullanarak bunu yapabilirsiniz. Yerinde bu hizmet ile Contoso gelecekteki veritabanı geçişlerini için bunu kullanabilirsiniz.
- SQL yönetilen örneği SQL Server Aracısı SmartHotel360 uygulamanın önemli bir sorundur destekler. Contoso uygulaması için gereken bakım planları yeniden tasarlamanız gerekir bu uyumluluk, aksi takdirde gerekir.
- Yazılım Güvencesi içeren mevcut lisanslarını indirimli fiyatlar üzerinden bir SQL veritabanı yönetilen SQL Server için Azure hibrit avantajı kullanma örneği için Contoso değiştirebilir. Bu, Contoso yönetilen örneği'nde % 30 kaydetmeye izin verebilirsiniz.
- Contoso'nun veriler için yüksek düzeyde güvenlik ve yalıtım sağlar, böylece yönetilen örnek tam olarak sanal ağ içinde yer alır. Contoso genel Internet'ten yalıtılmış ortam tutarken genel bulut avantajlarını elde edebilirsiniz.
- Always encrypted, dinamik veri maskeleme, satır düzeyi güvenlik ve tehdit algılama gibi birçok güvenlik özelliği örnek destekleyen yönetilen.


### <a name="solution-review"></a>Çözümü gözden geçirme

Contoso, Artıları ve eksileri listesini birbirine koyarak önerilen tasarım değerlendirir.

**Önemli noktalar** | **Ayrıntılar**
--- | ---
**Uzmanları** |  WEBVM Azure'a geçiş basit hale değişikliğe gerek kalmadan taşınır.<br/><br/> SQL yönetilen örneği, Contoso'nun teknik gereksinimlerini ve hedeflerini destekler.<br/><br/> Yönetilen örnek SQL Server 2008 R2 uzağa taşırken, geçerli dağıtımı ile % 100 uyumluluk sağlar.<br/><br/>  Bunlar, Yazılım Güvencesi ve Azure hibrit avantajı forSQL sunucu ve Windows Server'ı kullanarak bir yatırım getirisi yararlanabilirsiniz.<br/><br/> Bunlar, gelecekteki ek geçişlerin için veritabanı geçiş hizmeti yeniden kullanabilirsiniz.<br/><br/> SQL yönetilen örneği, Contoso yapılandırır gerekmez hata toleransı sunmuştur. Bu veri katmanı artık tek bir yük devretme noktası olmasını sağlar.
**Simgeler** | Windows Server 2008 R2 WEBVM çalışıyor.  Bu işletim sistemi Azure tarafından desteklenir, ama artık desteklenen bir platform değil. [Daha fazla bilgi edinin](https://support.microsoft.com/en-us/help/956893).<br/><br/> Web katmanı yük devretme yalnızca WEBVM sağlama hizmetleri ile tek bir noktadan kalır.<br/><br/> Contoso yönetilen bir hizmet olan Azure App Service gibi taşımak yerine destekleyen bir VM olarak app web katmanından devam etmek gerekir.<br/><br/> Contoso işletim sistemini veya veritabanı sunucusu özelleştirmek istiyorsanız ya da SQL Server yanı sıra üçüncü taraf uygulamaları çalıştırmak istiyorsanız veri katmanı için en iyi çözüm yönetilen örneği olmayabilir. Bir Iaas sanal makinesinde SQL Server çalıştıran bu esneklik sağlayabilir. 

### <a name="migration-process"></a>Geçiş işlemi

Contoso web ve veri katmanları, SmartHotel360 uygulamasının, Azure için şu adımları tamamlayarak geçişi:

1. Yalnızca birkaç bu senaryo için belirli Azure bileşenleri eklemek gereken şekilde Contoso Azure altyapısını yerinde, zaten sahip.
2. Veri katmanı, veri geçiş hizmeti kullanarak geçirilecektir. Veri geçiş hizmeti şirket içi SQL Server VM Contoso veri merkeziniz ile Azure arasında siteden siteye VPN bağlantısı üzerinden bağlanır. Ardından, veri geçiş hizmeti, veritabanı geçirir.
3. Web katmanı, Site Recovery kullanılarak lift-and-shift ile taşıma geçiş geçirilecektir. Şirket içi VMware ortamını hazırlama, ayarlama ve çoğaltmayı etkinleştirdikten ve bunları Azure'a devretmek tarafından Vm'leri geçirme işlemi gerektirir.

     ![Geçiş mimarisi](media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png) 

### <a name="azure-services"></a>Azure hizmetleri

Hizmet | Açıklama | Maliyet
--- | --- | ---
[Veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview) | Veritabanı geçiş hizmeti, birden çok veritabanı kaynağını sorunsuz geçiş için en düşük kapalı kalma süresi ile Azure veri platformu sağlar. | Hakkında bilgi edinin [desteklenen bölgeler](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) ve [veritabanı geçiş hizmeti fiyatlandırma](https://azure.microsoft.com/pricing/details/database-migration/).
[Azure SQL veritabanı yönetilen örneği](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Yönetilen örnek Azure bulutunda tam olarak yönetilen bir SQL Server örneğini temsil eden bir yönetilen veritabanı hizmetidir. SQL Server Veritabanı Altyapısı'nın en son sürümü olarak aynı kodu kullanır ve en son özellikleri, performans iyileştirmeleri ve güvenlik yamaları vardır. | SQL veritabanı yönetilen Azure'da çalışan örneği kullanarak kapasiteye bağlı ücreti alınmaz. Daha fazla bilgi edinin [yönetilen örnek fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Site kurtarma hizmetini düzenler ve geçiş ve olağanüstü durum kurtarma Azure Vm'leri için yönetir ve şirket içi Vm'leri ve fiziksel sunucuları.  | Azure'a çoğaltma sırasında Azure depolama ücretleri uygulanır.  Azure Vm'leri oluşturulur ve yük devretme gerçekleştiğinde ücreti. Daha fazla bilgi edinin [Site Recovery ücreti ve fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/).

 

## <a name="prerequisites"></a>Önkoşullar

Contoso ve diğer kullanıcılar bu senaryo için aşağıdaki önkoşulları karşılaması gerekir:

Gereksinimler | Ayrıntılar
--- | ---
**Yönetilen örnek önizleme kaydetme** | SQL veritabanı yönetilen örneği sınırlı genel Önizleme sürümünde kaydedilmesi gerekir. Bir Azure aboneliğine ihtiyacınız [kaydolun](https://portal.azure.com#create/Microsoft.SQLManagedInstance). Kaydolmayı tamamlamak için bu nedenle, bu senaryoyu dağıtmaya başlamadan önce kaydolmanız emin olun, birkaç gün sürebilir.
**Azure aboneliği** | Bu serideki ilk makaledeki değerlendirme gerçekleştirdiğinizde aboneliği oluşturmuş olmanız. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve abonelik yöneticisi değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.<br/><br/> Daha ayrıntılı izinlerine ihtiyaç duyarsanız bkz [Site Recovery erişimi yönetmek için rol tabanlı erişim denetimi kullanın](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Site Recovery (şirket içi)** | Şirket içi vCenter Server örneğinizi 5.5, 6.0 veya 6.5 sürümünü çalıştırmalıdır<br/><br/> 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir ESXi ana bilgisayarı<br/><br/> Bir veya daha fazla VMware ESXi ana bilgisayarında çalışan VM'ler.<br/><br/> Vm'leri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Desteklenen [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) yapılandırma.
**Veritabanı geçiş hizmeti** | Veritabanı geçiş hizmeti için gereksinim duyduğunuz bir [uyumlu şirket içi VPN cihazı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Şirket içi VPN cihazının yapılandırmanız gerekir. Bunu dönük genel IPv4 adresi olmalıdır. Adresi bir NAT cihazının arkasında yer almamalıdır.<br/><br/> Şirket içi SQL Server veritabanınıza erişimi olduğundan emin olun.<br/><br/> Windows Güvenlik Duvarı, kaynak veritabanı altyapısını erişebilir olmalıdır. Bilgi edinmek için nasıl [veritabanı altyapısı erişimi için Windows Güvenlik Duvarı Yapılandırma](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Veritabanını makinenize önünde bir güvenlik duvarı varsa, veritabanını ve dosyaları SMB 445 bağlantı noktası aracılığıyla erişmesine izin vermek için kuralları ekleyin.<br/><br/> Kaynak SQL Server örneğine bağlanmak için kullanılan ve yönetilen örneği hangi hedefin kimlik bilgileri sysadmin sunucu rolünün üyesi olması gerekir.<br/><br/> Bir ağ ihtiyacınız paylaşmak şirket içi veritabanınızda kaynak veritabanını yedeklemek için veritabanı geçiş hizmetini kullanabilirsiniz.<br/><br/> Kaynak SQL Server örneğini çalıştıran hizmet hesabının ağ paylaşımında yazma izinlerine sahip olduğundan emin olun.<br/><br/> Bir Windows kullanıcı ve ağ paylaşımı üzerinde Tam Denetim izinlerine sahip parolayı not edin. Veritabanı geçiş hizmetinin yedekleme dosyalarını Azure depolama kapsayıcısına karşıya yüklemek için bu kullanıcı kimlik bilgilerini temsil eder.<br/><br/> TCP/IP Protokolü SQL Server Express yükleme işlemi ayarlar **devre dışı bırakılmış** varsayılan olarak. Etkin olduğundan emin olun.

## <a name="scenario-steps"></a>Senaryo adımları

Nasıl dağıtımı ayarlamak üzere Contoso planlar aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Bir SQL veritabanı yönetilen örneği oluşturan ayarlamak**: Contoso bir önceden oluşturulmuş yönetilen şirket içi SQL Server veritabanı geçiş yapacağınız örneği gerekir.
> * **2. adım: Veritabanı geçiş hizmeti hazırlama**: Contoso gerekir, veritabanı geçiş sağlayıcısını kaydedin, örneği oluşturun ve sonra bir veritabanı geçişi hizmeti projesi oluşturun. Contoso de bir paylaşılan erişim imzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) için veritabanı geçiş hizmeti ayarlamanız gerekir. Bir SAS URI Contoso'nun depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. böylece contoso depolama nesnelere sınırlı izinler verebilirsiniz. Veritabanı geçiş hizmeti, depolama hesabı kapsayıcı hizmeti SQL Server Yedekleme dosyalarını karşıya yüklemeleri erişebilmesi için bir SAS URI Contoso ayarlar.
> * **3. adım: Azure Site Recovery için hazırlama**: Contoso için Site Recovery çoğaltılan verileri tutmak için bir depolama hesabı oluşturmanız gerekir. Ayrıca Azure kurtarma Hizmetleri kasası oluşturmanız gerekir.
> * **4. adım: Site Recovery için şirket içi Vmware'leri hazırlama**: Contoso hesapları yük devretmeden sonra Azure Vm'lerine bağlanmak VM bulma ve aracı yükleme için hazırlık yapacaksınız.
> * **5. adım: Vm'lerini çoğaltma**: Contoso çoğaltmayı ayarlama için Site Recovery kaynak ve hedef ortamları yapılandırmak, bir çoğaltma ilkesi ayarlar ve VM'ler için Azure Storage çoğaltmaya başlar.
> * **6. adım: Veritabanı geçiş hizmetini kullanarak veritabanını geçirme**: Contoso veritabanı geçirir.
> * **7. adım: Site Recovery kullanarak sanal makineleri geçirme**: Contoso çalıştıran bir yük devretme testi, her şeyi emin olmak için çalışmaktadır. Ardından, Contoso sanal makineleri Azure'a geçirmek için bir tam yük devretme çalıştırır.

## <a name="step-1-prepare-a-sql-database-managed-instance"></a>1. Adım: SQL veritabanı yönetilen örneği'ni Hazırlama

Azure SQL veritabanı yönetilen örneği ayarlamak için aşağıdaki gereksinimleri karşılayan bir alt ağ Contoso gerekir:

- Alt ağ ayrılmış olmalıdır. Boş olmalıdır ve herhangi bir bulut hizmeti içeremez. Alt ağ bir ağ geçidi alt ağı olamaz.
- Contoso yönetilen örneği oluşturulduktan sonra kaynaklar alt ağa eklemeyin.
- Alt ağ ile ilişkilendirilmiş ağ güvenlik grubu olamaz.
- Alt ağı, bir kullanıcı tanımlı yönlendirme (UDR) yol tablosu olması gerekir. Yalnızca rota atanmış olmalıdır 0.0.0.0/0 sonraki atlama internet. 
- Özel DNS isteğe bağlı: Azure sanal ağda özel DNS belirtilirse, Azure'nın yinelemeli Çözümleyicileri IP adresi (örneğin, 168.63.129.16) listeye eklenmelidir. Bilgi edinmek için nasıl [bir yönetilen örnek için özel DNS yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- Alt ağ ile ilişkili bir hizmet uç noktası (depolama veya SQL) olması gerekmez. Hizmet uç noktaları sanal ağda devre dışı bırakılması gerekir.
- Alt ağı, en az 16 IP adresleri olması gerekir. Bilgi edinmek için nasıl [yönetilen örnek alt boyut](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration).
- Contoso'nun karma ortamda özel DNS ayarlarını gereklidir. Contoso birini veya daha fazlasını şirketin Azure DNS sunucuları için DNS ayarlarını yapılandırır. Daha fazla bilgi edinin [DNS özelleştirme](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>Yönetilen örnek için bir sanal ağ ayarlama

Contoso yöneticileri sanal ağı aşağıdaki gibi ayarlayın: 

1. Bunlar yeni bir sanal ağ oluştur (**VNET SQLMI EU2**) birincil Doğu ABD 2 bölgesinde. Sanal ağa ekler **ContosoNetworkingRG** kaynak grubu.
2. Bunlar, bir adres alanı 10.235.0.0/24 atayın. Bunlar, Kurumsal diğer ağlarla aralığı çakışmayacak emin olun.
3. Bunlar, iki alt ağa ekleyin:
    - **SQLMI-DS-EUS2** (10.235.0.0.25)
    - **SQLMI SAW EUS2** (10.235.0.128/29). Bu alt ağı, yönetilen örneği'ne bir dizin eklemek için kullanılır.

      ![Yönetilen örnek - sanal ağ oluşturma](media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

4. Sanal ağ ve alt ağları dağıtıldıktan sonra ağları aşağıdaki gibi eş:

    - Eş **VNET SQLMI EUS2** ile **VNET HUB EUS2** (hub sanal ağ için Doğu ABD 2).
    - Eş **VNET SQLMI EUS2** ile **VNET PROD EUS2** (üretim ağı).

      ![Ağ eşlemesi](media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

5. Bunlar, özel DNS ayarlarını yapın. DNS, Contoso'nun Azure etki alanı denetleyicilerine ilk işaret eder. Azure DNS, ikincil. Contoso Azure etki alanı denetleyicileri bulunduğu konum aşağıda verilmiştir:

    - Bulunan **PROD DC EUS2** Doğu ABD 2 üretim ağdaki bir alt ağ (**VNET PROD EUS2**)
    - **CONTOSODC3** adresi: 10.245.42.4
    - **CONTOSODC4** adresi: 10.245.42.5
    - Azure DNS Çözümleyicisi: 168.63.129.16

      ![Ağ DNS sunucularına](media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

*Daha fazla yardıma mı ihtiyacınız var?*

- Genel Bakış [SQL veritabanı yönetilen örneği](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance).
- Bilgi edinmek için nasıl [SQL veritabanı yönetilen örneği için bir sanal ağ oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration).
- Bilgi edinmek için nasıl [eşleme ayarlama](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering).
- Bilgi edinmek için nasıl [Azure Active Directory DNS ayarlarını güncelleştirme](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns).

### <a name="set-up-routing"></a>Yönlendirme ayarlama

Yönetilen örneğe özel bir sanal ağa yerleştirilir. Contoso, Azure yönetim hizmetiyle iletişim kurmak sanal ağ için bir yol tablosu gerekir. Sanal ağın sanal ağ, yöneten hizmet ile iletişim kuramıyorsa erişilemez duruma gelir.

Contoso aşağıdaki faktörleri göz önünde bulundurur:

- Rota tablosunu yönetilen örnek gönderilen paketleri sanal ağ içinde nasıl yönlendirileceğini belirtin (yol) kuralları kümesi içerir.
- Rota tablosunu yönetilen örnekler dağıtıldığı alt ağlar ile ilişkilidir. Bir alt ağ ayrıldığında her paket, ilişkili yol tablosuna dayalı olarak işlenir.
- Bir alt ağ yalnızca bir yol tablosu ile ilişkilendirilebilir.
- Microsoft Azure'da rota tabloları oluşturmak için herhangi bir ek ücret yoktur.

  Yöneticileri yönlendirme Contoso ayarlamak için aşağıdakileri yapın:

1. UDR (yol) tablosunda oluşturdukları **ContosoNetworkingRG** kaynak grubu.

    ![Rota tablosu](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

2. Sonra yol tablosunu yönetilen örnek gereksinimlerine uymak için (**MIRouteTable**) olan dağıtılmış, bunlar bir adres ön eki 0.0.0.0/0 olan bir yol ekleyin. **Sonraki atlama türü** seçeneği **Internet**.

    ![Rota tablosu öneki](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)
    
3. Bunlar ile yönlendirme tablosunu ilişkilendirme **SQLMI DB EUS2** alt ağ (içinde **VNET SQLMI EUS2** ağ). 

    ![Rota tablosunu alt ağ](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)
    
*Daha fazla yardıma mı ihtiyacınız var?*

Bilgi edinmek için nasıl [bir yönetilen örnek için rotalar ayarlayabilir](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal).

### <a name="create-a-managed-instance"></a>Yönetilen Örnek oluşturma

Artık, Contoso yöneticilerinin bir SQL veritabanı yönetilen örneği sağlayabilirsiniz:

1. Bir iş uygulaması yönetilen örneğe proxy'sisi işlevi görmesi nedeniyle, bunlar yönetilen örneği'nde şirketin birincil Doğu ABD 2 bölgesinde dağıtın. Bunlar yönetilen örneği'ne Ekle **ContosoRG** kaynak grubu.
2. Bunlar, bir fiyatlandırma katmanı, boyutu işlem ve depolama için örneği seçin. Daha fazla bilgi edinin [yönetilen örnek fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/managed/).

    ![Yönetilen Örnek](media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

3. Yönetilen örneği dağıtıldıktan sonra iki yeni kaynaklar görünür **ContosoRG** kaynak grubu:

    - Büyük/küçük harf Contoso sanal bir kümede birden çok yönetilen örneğe sahip.
    - SQL Server veritabanı yönetilen örneği. 

      ![Yönetilen Örnek](media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

*Daha fazla yardıma mı ihtiyacınız var?*

Bilgi edinmek için nasıl [bir yönetilen örneğini sağlama](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal).

## <a name="step-2-prepare-the-database-migration-service"></a>2. Adım: Veritabanı geçiş hizmeti hazırlama

Veritabanı geçiş hizmeti hazırlamak için Contoso yöneticilerinin birkaç şey yapmanız gerekir:

- Veritabanı geçiş hizmeti sağlayıcı Azure'da kaydedin.
- Veritabanı geçiş hizmeti, veritabanını geçirmek için kullanılan yedekleme dosyalarını yüklemek için Azure depolama ile erişim sağlar. Azure depolama alanına erişim sağlamak için Azure Blob Depolama kapsayıcısına oluştururlar. Bunlar, bir Blob Depolama kapsayıcısının SAS URI'sini oluşturur. 
- Veritabanı geçiş hizmeti projesi oluşturun.

Ardından, aşağıdaki adımları tamamlayın:

1. Bunlar, veritabanı geçiş sağlayıcısı, abonelik altında kaydedin.
    ![Veritabanı geçiş hizmeti - kayıt](media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

2. Bunlar, bir Blob Depolama kapsayıcısı oluşturun. Veritabanı geçiş hizmeti erişebilmesi Contoso SAS URI'sini oluşturur.

    ![Veritabanı geçiş hizmeti - bir SAS URI'si oluşturma](media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

3. Bunlar, bir veritabanı geçiş hizmeti örneği oluşturur. 

    ![Veritabanı geçiş hizmeti - örneği oluşturma](media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

4. Bunlar veritabanı geçiş hizmeti örneğinde yerleştirin **PROD DC EUS2** alt **VNET-PROD-DC-EUS2** sanal ağ.
    - Veritabanı geçiş hizmeti, hizmet bir VPN ağ geçidi üzerinden şirket içi SQL Server VM erişimi olan bir sanal ağda olması gerektiğinden burada yerleştirilir.
    - **VNET PROD EUS2** için eşlenmiş **VNET HUB EUS2** ve uzak ağ geçitlerini kullanmasına izin verilir. **Uzak ağ geçitlerini kullan** seçeneği, veritabanı geçiş hizmeti gereken şekilde iletişim kurabildiğini sağlar.

        ![Veritabanı geçiş hizmeti - ağ yapılandırma](media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

*Daha fazla yardıma mı ihtiyacınız var?*

- Bilgi edinmek için nasıl [veritabanı geçiş hizmeti'kurmak](https://docs.microsoft.com/azure/dms/quickstart-create-data-migration-service-portal).
- Bilgi edinmek için nasıl [SAS oluşturup](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2).


## <a name="step-3-prepare-azure-for-the-site-recovery-service"></a>3. Adım: Azure Site Recovery hizmeti için hazırlama

Kendi web katman sanal makine taşıma için Site RECOVERY'yi ayarlamak, Contoso için çeşitli Azure öğeleri gereklidir (**WEBMV**):

- Sanal ağ devredilen kaynakları bulunur.
- Çoğaltılan verileri tutmak için bir depolama hesabı. 
- Azure kurtarma Hizmetleri kasasında.

Contoso yöneticileri Site RECOVERY'yi aşağıdaki gibi ayarlayın:

1. VM bir web ön ucu SmartHotel360 uygulama olduğundan Contoso, var olan bir üretim ağı VM yük devreder (**VNET PROD EUS2**) ve alt ağ (**PROD FE EUS2**). Ağ ve alt ağı birincil Doğu ABD 2 bölgesinde yer alır. Ağ kurmak Contoso, bunu [Azure altyapısı dağıtılan](contoso-migration-infrastructure.md).
2. Bunlar depolama hesabı oluşturma (**contosovmsacc20180528**). Contoso genel amaçlı bir hesabı kullanır. Contoso, standart depolama ve yerel olarak yedekli depolama çoğaltma seçer.

    ![Site Recovery - depolama hesabı oluşturma](media/contoso-migration-rehost-vm-sql-managed-instance/asr-storage.png)

3. Ağ ve depolama hesabı ile yerinde, bunlar bir kasası oluşturun (**ContosoMigrationVault**). Contoso yerleştirir kasaya **ContosoFailoverRG** birincil Doğu ABD 2 bölgesinde bir kaynak grubu.

    ![Oluştur - kurtarma Hizmetleri kasası](media/contoso-migration-rehost-vm-sql-managed-instance/asr-vault.png)

*Daha fazla yardıma mı ihtiyacınız var?*

Bilgi edinmek için nasıl [Site Recovery için Azure'ı ayarlama](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure).


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>4. Adım: Site Recovery için şirket içi Vmware'leri hazırlama

Site Recovery için VMware hazırlamak için Contoso yöneticileri bu görevleri tamamlamanız gerekir:

- Bir hesapta vCenter Server örneği veya vSphere ESXi konağı hazırlayın. VM bulma hesabı otomatikleştirir.
- Contoso çoğaltmak isteyen VMware Vm'lerinde Mobility hizmetini otomatik olarak yüklenmesini sağlayan bir hesap hazırlama.
- Şirket içi Vm'leri yük devretme sonrasında oluşturulduğunda Azure Vm'lerine bağlanmak için hazırlayın.


### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- VM'leri otomatik olarak bulma. En az bir salt okunur hesap gereklidir.
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme. Contoso, oluşturma ve diskleri kaldırma ve Vm'leri kapatarak gibi işlemleri de çalıştırabilirsiniz bir hesaba ihtiyacı vardır.

Bu görevleri tamamladığınızda, contoso admins hesabı ayarlayın:

1. Bir rolü vCenter düzeyinde oluşturur.
2. Bu rol için gerekli izinleri atar.

*Daha fazla yardıma mı ihtiyacınız var?*

Bilgi edinmek için nasıl [oluşturun ve Otomatik bulma için bir rol atayın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery).

### <a name="prepare-an-account-for-mobility-service-installation"></a>Mobility hizmeti yüklemesi için bir hesap hazırlama

Contoso çoğaltmak isteyen sanal makinede Mobility hizmetinin yüklenmesi gerekir. Contoso faktörlerin Mobility hizmeti hakkında göz önünde bulundurur:

- Site kurtarma, contoso sanal makine için çoğaltmayı etkinleştirdiğinde bu bileşeni otomatik gönderim yüklemesi yapabilirsiniz.
- Otomatik göndererek yükleme için Contoso sanal Makineye erişmek için Site Recovery kullanan bir hesap hazırlamanız gerekir.
- Azure konsolunda çoğaltma yapılandırıldığında, bu hesabı belirtilir.
- Contoso etki alanı veya yerel hesap VM'de yüklemek için gerekli izinlere sahip olması gerekir.

*Daha fazla yardıma ihtiyacınız*

Bilgi edinmek için nasıl [Mobility hizmetinin göndererek yüklenmesine ilişkin için bir hesap](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation).

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Azure'a yük devretme sonrasında Contoso çoğaltılmış Azure vm'lere bağlanmak atabilmek istiyor. Çoğaltılmış Azure Vm'lere bağlanmak için Contoso yöneticileri şirket içi sanal makine geçiş işleminden önce birkaç görevi tamamlamanız gerekir: 

1. İnternet üzerinden erişim için bunlar yük devretmeden önce şirket içi VM'de RDP'yi etkinleştirin. İçin TCP ve UDP kurallarının eklendiğinden emin olun **genel** profili ve içinde RDP'ye izin verildiğinden **Windows Güvenlik Duvarı** > **verilen uygulamaları** tüm profiller için.
2. Contoso'nun siteden siteye VPN üzerinden erişim için bunlar şirket içi makinede RDP'yi etkinleştirin. Bunlar içinde RDP'ye izin ver **Windows Güvenlik Duvarı** > **izin verilen uygulamalar ve Özellikler** için **etki alanı ve özel** ağlar.
3. Bunlar şirket içi VM'deki işletim sisteminin SAN ilkesinin ayarlamak **OnlineAll**.

Contoso yöneticilerinin Ayrıca, bir yük devretme çalıştırdığınızda şu öğeleri kontrol gerekir:

- Bulunmamalıdır bekleyen herhangi bir Windows güncelleştirme VM üzerinde bir yük devretme tetiklendiğinde. Windows güncellemesi askıda değilse, güncelleştirme tamamlanana kadar kullanıcılar Contoso sanal makineye oturum açamazsınız.
- Yük devretme sonrasında Yöneticiler denetlemelisiniz **önyükleme tanılaması** VM'nin bir ekran görüntülemek için. Önyükleme tanılaması göremiyorsanız, VM çalıştıran ve daha sonra gözden olduğunu denetlemelisiniz [sorun giderme ipuçları](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

## <a name="step-5-replicate-the-on-premises-vms-to-azure"></a>5. Adım: Şirket içi Vm'leri Azure'a çoğaltma

Azure'da bir geçiş çalıştırmadan önce ayarlamak ve şirket içi sanal Makineye yönelik çoğaltmayı etkinleştirmek Contoso yöneticilerinin gerekir.

### <a name="set-a-replication-goal"></a>Çoğaltma hedefi ayarlama

1. Kasa adı altında Kasası'nda (**ContosoVMVault**), çoğaltma hedefi ayarladıkları (**Başlarken** > **Site Recovery**  >   **Altyapıyı hazırlama**).
2. Bunlar makineleri şirket içinde bulunan, Vmware'den Azure'a çoğaltılan VM'ler oldukları belirtin.

    ![Koruma hedefi altyapıyı - hazırlama](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

Devam etmek için bunlar dağıtım planlamasını tamamladınız Contoso yöneticileri onaylayın. Seçmeleri **Evet, yaptım**. Bu dağıtımda Contoso, yalnızca tek bir VM'yi geçirme, dağıtım planlama gerekmez.

### <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Şimdi, Contoso yöneticilerin kaynak ortamı yapılandırın. Kendi kaynak ortamı ayarlamak için bir OVF şablonu indirin ve yapılandırma sunucusu ve yüksek oranda kullanılabilir olarak ilişkili bileşenlerini dağıtmak için kullanın, şirket içi VMware sanal makine. Sunucu üzerindeki bileşenler şunlardır:

- Şirket içi altyapı ile Azure arasındaki iletişimi koordine eden yapılandırma sunucusu. Yapılandırma sunucusu, veri çoğaltma işlemlerini yönetir.
- İşlem sunucusu çoğaltma ağ geçidi davranır. İşlem sunucusu:
    - Çoğaltma verilerini alır.
    - Önbelleğe alma, sıkıştırma ve şifreleme kullanarak çoğaltma tarihi en iyi duruma getirir.
    - Çoğaltma tarih Azure depolamaya gönderir.
- İşlem sunucusu, ayrıca çoğaltılacak VM'ler üzerinde Mobility hizmetini yükler. İşlem sunucusu, şirket içi VMware Vm'lerini otomatik olarak bulunmasını gerçekleştirir.
- Sanal makine oluşturulup başlatıldıktan yapılandırma sunucusu sonra sunucu kasaya Contoso kaydeder.

Kaynağı'kurmak için ortam Contoso yöneticileri aşağıdakileri yapın:

1. Bunlar Azure portalından OVF şablonu indirin (**altyapıyı hazırlama** > **kaynak** > **yapılandırma sunucusu**).
    
    ![Yapılandırma sunucusunu Ekle](./media/contoso-migration-rehost-vm-sql-managed-instance/add-cs.png)

2. Bunlar, şablonu oluşturup VM dağıtmak için Vmware'e aktarın.

    ![OVF şablonu dağıtma](./media/contoso-migration-rehost-vm-sql-managed-instance/vcenter-wizard.png)

3.  Sanal makinede ilk kez açtığınızda bir Windows Server 2016 yükleme deneyimi başlatır. Bunlar, lisans sözleşmesini kabul edin ve bir yönetici parolasını girer.
4. Yükleme tamamlandığında, VM için yönetici olarak oturum açın. İlk zaman oturum açma işleminde, Azure Site Recovery Configuration Tool otomatik olarak çalıştırılır.
5. Site kurtarma yapılandırması Aracı'nda, yapılandırma sunucusunu kasaya kaydetmek için kullanılacak bir ad girin.
6. Azure VM bağlantısı Aracı'nı denetler. Bağlantı kurulduktan sonra seçtikleri **oturum** Azure aboneliğinize oturum açmak için. Kimlik bilgileri, hangi yapılandırma sunucusunun kayıtlı olduğu kasanın erişiminiz olması gerekir. 

    ![Yapılandırma sunucusunu kaydetme](./media/contoso-migration-rehost-vm-sql-managed-instance/config-server-register2.png)

7. Araç birkaç yapılandırma görevi gerçekleştirir ve yeniden başlatır. Bunların makinede yeniden oturum açın. Yapılandırma sunucusu yönetim Sihirbazı otomatik olarak başlar.
8. Sihirbazda, çoğaltma trafiğini almak için NIC'yi seçin. Bu ayar yapılandırıldıktan sonra değiştirilemez.
9. Abonelik, kaynak grubu ve yapılandırma sunucusunu kaydetmek için kurtarma Hizmetleri kasasında seçerler.

    ![Recovery Services kasasını seçin](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz1.png)

10. Bunlar, indirin ve MySQL Server ve VMWare powerclı'yı yükler. Ardından, bunlar doğrular sunucusu ayarları.
11. Doğrulama sonrasında, bunlar vCenter sunucu örneğini veya vSphere konağının FQDN'sini veya IP adresini girin. Bunlar, varsayılan bağlantı noktasını değiştirmeyin ve Azure'da vCenter Server örneği için bir görünen ad girin.
12. Bunlar, Site Recovery çoğaltma için kullanılabilen VMware Vm'lerini otomatik olarak keşfedebilmesi için daha önce oluşturduğunuz hesabı belirtin. 
13. Çoğaltma etkinleştirildiğinde Mobility hizmetini otomatik olarak yüklenir, böylece bunlar kimlik bilgilerini girin. Windows makineleri için hesabın vm'lerinde yerel yönetici izinleri gerekir. 

    ![VCenter Server'ı yapılandırma](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz2.png)

7. Kayıt tamamlandığında, Azure portalında bunlar yeniden yapılandırma sunucusunun ve VMware sunucusunun üzerinde listelendiğini doğrulayın **kaynak** kasadaki sayfası. Bulma, 15 dakika veya daha fazla sürebilir. 
8. Site Recovery, belirtilen ayarları kullanarak VMware sunucularına bağlanır ve Vm'leri bulur.

### <a name="set-up-the-target"></a>Hedefi ayarlama

Şimdi, Contoso yöneticileri hedef çoğaltma ortamı yapılandırın:

1. İçinde **altyapıyı hazırlama** > **hedef**, bunlar hedef ayarları seçin.
2. Site Recovery, bir depolama hesabı ve belirtilen hedef ağda olup olmadığını denetler.

### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Kaynak ve hedef ayarlandığında Contoso yöneticileri bir çoğaltma ilkesi oluşturun ve ilkeyi yapılandırma sunucusu ile ilişkilendirir:

1. İçinde **altyapıyı hazırlama** > **çoğaltma ayarları** > **Çoğaltma İlkesi** >  **oluşturma ve İlişkilendirme**, oluşturdukları **ContosoMigrationPolicy** ilkesi.
2. Bunlar, varsayılan ayarları kullanın:
   - **RPO eşiği**: Varsayılan 60 dakika. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
   - **Kurtarma noktası bekletme**: Varsayılan 24 saat. Bu değer, ne kadar süreyle her kurtarma noktası için bekletme süresinin olacağını belirtir. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
   - **Uygulamayla tutarlı anlık görüntü sıklığı**: 1 saatlik varsayılan. Bu değer, uygulamayla tutarlı anlık görüntülerin oluşturulma sıklığı belirtir.
 
     ![-Çoğaltma İlkesi oluşturma](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy.png)

3. İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. 

    ![Çoğaltma İlkesi - ilişkilendirme](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy2.png)

*Daha fazla yardıma mı ihtiyacınız var?*

- Bu adımlarda eksiksiz izlenecek edinebilirsiniz [şirket içi VMware Vm'leri için olağanüstü durum kurtarmayı ayarlayın](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Yardımcı olmak ayrıntılı yönergeler kullanılabilir [kaynak ortamını ayarlama](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [yapılandırma sunucusunu dağıtma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), ve [çoğaltma ayarlarını yapılandırma](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).

### <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Artık, Contoso yöneticileri WebVM çoğaltma başlayabilirsiniz.

1. İçinde **uygulama çoğaltma** > **kaynak** > **çoğaltmak**, bunların kaynak ayarlarını seçin.
2. Bunlar, sanal makineleri sağlar, vCenter sunucu örneğini seçin ve yapılandırma sunucusu ayarlamak istediğiniz gösterir.

    ![Çoğaltma - kaynak etkinleştir](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication1.png)
 
3. Bunlar, kaynak grubu ve ağ, Azure VM yük devretme sonrasında bulunacağı dahil olmak üzere hedef ayarlarını belirtin. Bunlar, çoğaltılan verilerin depolanacağı depolama hesabı belirtin.

    ![Çoğaltma - hedef etkinleştir](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication2.png)

4. Seçmeleri **WebVM** çoğaltma. Çoğaltma etkinleştirildiğinde site Recovery, her sanal makinede Mobility hizmetini yükler. 

    ![Çoğaltmayı etkinleştirme - sanal Makineyi seçin](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication3.png)

5. Doğru çoğaltma ilkesinin seçilir ve için çoğaltmayı etkinleştirme denetleyin **WEBVM**. Bunlar çoğaltma ilerlemeyi **işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.
6. İçinde **Essentials** Azure portalında bunlar durumunu Azure'a çoğaltılan sanal makineler için görebilirsiniz:

    ![Altyapı görünümü](./media/contoso-migration-rehost-vm-sql-managed-instance/essentials.png)

*Daha fazla yardıma mı ihtiyacınız var?*

Bu adımlarda eksiksiz izlenecek edinebilirsiniz [çoğaltmayı etkinleştir](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).

## <a name="step-6-migrate-the-database"></a>6. Adım: Veritabanını geçirme 

Veritabanı geçiş hizmeti projesi oluşturun ve ardından veritabanını geçirme contoso yöneticilerinin gerekir.

### <a name="create-a-database-migration-service-project"></a>Veritabanı geçiş hizmeti projesi oluşturma

1. Bunlar, bir veritabanı geçişi hizmeti projesi oluşturun. Seçmeleri **SQL Server** kaynak sunucu türü ve **Azure SQL veritabanı yönetilen örneği** hedefi olarak.

     ![Veritabanı geçiş hizmeti - yeni geçiş projesi](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

2. Geçiş Sihirbazı'nı açar.

### <a name="migrate-the-database"></a>Veritabanını geçirme 

1. Geçiş Sihirbazı'nda, şirket içi veritabanının bulunduğu kaynak VM belirtin. Bunlar, veritabanına erişmek için kimlik bilgilerini girin.

    ![Veritabanı geçiş hizmeti - kaynak ayrıntıları](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

2. Bunlar geçirmek için veritabanını seçin (**SmartHotel.Registration**):

    ![Veritabanı geçiş hizmeti - veritabanlarını kaynak seçin](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-sourcedb.png)

3. Hedef için bunlar yönetilen örneği'nde Azure ve erişim kimlik bilgileri adını girin.

    ![Veritabanı geçiş hizmeti - hedef ayrıntıları](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

4. İçinde **yeni etkinlik** > **geçiş çalıştırmak**, bunlar geçişi çalıştırmak için ayarları belirtin:
    - Kaynak ve hedef kimlik bilgileri.
    - Geçiş için veritabanı.
    - Şirket içi VM üzerinde oluşturulan ağ paylaşımı. Veritabanı geçiş hizmeti, bu paylaşıma kaynak yedeklemeler alır. 
        - Kaynak SQL Server örneğini çalıştıran hizmet hesabının bu paylaşımında yazma izinlerine sahip olmalıdır.
        - Paylaşım FQDN yolu kullanılmalıdır.
    - Veritabanı geçiş hizmeti hizmet geçiş için yedekleme dosyalarının karşıya yükler depolama hesabının kapsayıcıya erişimi sağlayan SAS URI'si.

        ![Veritabanı geçiş hizmeti - geçiş ayarlarını yapılandırma](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

5. Bunlar geçiş ayarları kaydedin ve ardından bir geçiş çalıştırın.
6. İçinde **genel bakış**, bunlar geçiş durumu izleyin.

    ![Veritabanı geçiş hizmeti - izleme](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

7. Geçiş tamamlandığında, bunlar hedef veritabanlarına yönetilen örneği'nde mevcut olduğunu doğrulayın.

    ![Veritabanı geçiş hizmeti - veritabanı geçişi doğrulama](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vm"></a>7. Adım: VM'yi geçirme

Contoso yöneticileri hızlı çalıştırın, yük devretme testi ve sonra VM'yi geçirme.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Geçirmeden önce WEBVM, yük devretme testi yardımcı olan her şeyin beklendiği gibi çalıştığından emin olun. Yöneticiler aşağıdaki adımları tamamlayın:

1. Yük devretme testi için son noktası sürede çalıştırdığı (**en son işlenen**).
2. Seçmeleri **yük devretmeye başlamadan önce makineyi Kapat**. Bu seçenek ile Site Recovery yük devretmeyi tetiklemeden önce kaynak sanal makineyi kapatmaya çalışır. Kapatma başarısız olsa bile yük devretme devam eder. 
3. Test yük devretme çalıştırır: 
    1. Bir önkoşul denetimi geçiş için gerekli tüm koşulların karşılandığından emin olmak için çalıştırılır.
    2. Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktası seçtiyseniz, verilerden bir kurtarma noktası oluşturulur.
    3.  Azure VM, önceki adımda işlenen veriler kullanılarak oluşturulur.
3. Çoğaltma, yük devretme tamamlandığında, Azure portalında Azure VM görünür. her şeyin düzgün çalıştığını doğrulayın: VM'nin uygun boyutta olduğundan, doğru ağa bağlandığından ve çalıştığından. 
4. Test yük devretmesi doğruladıktan sonra Yük devretme ' temizleyin ve gözlemlerinizi kaydetmek. 

### <a name="migrate-the-vm"></a>VM'yi geçirme

1. Test yük devretmesi beklendiği gibi çalıştığını doğruladıktan sonra Contoso yöneticileri geçiş için bir kurtarma planı oluşturma ve WEBVM plana ekleyin:

     ![Kurtarma planı - öğeleri oluşturma](./media/contoso-migration-rehost-vm-sql-managed-instance/recovery-plan.png)

2. Bunlar, en son kurtarma noktası seçerek plan üzerinde bir yük devretme çalıştırın. Bunlar, Site Recovery yük devretmeyi tetiklemeden önce şirket içi sanal makineyi denemesi gerektiğini belirtin.

    ![Yük devretme](./media/contoso-migration-rehost-vm-sql-managed-instance/failover1.png)

3. Yük devretmeden sonra bunlar Azure VM Azure Portalı'nda beklendiği gibi göründüğünü doğrulayın.

   ![Kurtarma planı ayrıntıları](./media/contoso-migration-rehost-vm-sql-managed-instance/failover2.png)

4. Doğrulama, geçiş işlemini tamamlamak için geçiş işlemi tamamlandıktan sonra VM için çoğaltma ve Site Recovery VM için fatura durdurun.

    ![Yük devretme - geçişi Tamamla](./media/contoso-migration-rehost-vm-sql-managed-instance/failover3.png)

### <a name="update-the-connection-string"></a>Bağlantı dizesini güncelleştirme

Son adım olarak geçiş sürecinin Contoso yöneticileri Contoso'nun yönetilen örneği'nde çalışan geçirilen veritabanına işaret edecek şekilde uygulamanın bağlantı dizesini güncelleştirin.

1. Azure portalında bağlantı dizesi seçerek buldukları **ayarları** > **bağlantı dizeleri**.

    ![Bağlantı dizeleri](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)  

2. Bunlar, kullanıcı adını ve parolasını SQL veritabanı yönetilen örneği ile dize güncelleştirin.
3. Dize yapılandırdıktan sonra bunlar, uygulamanın web.config dosyasında geçerli bağlantı dizesini değiştirin.
4. Dosyası güncelleştiriliyor ve kaydettikten sonra IIS WEBVM üzerinde çalıştırarak yeniden başlatıldığında `IISRESET /RESTART` bir komut istemi penceresinde.
5. IIS yeniden başlatıldıktan sonra uygulama SQL veritabanı yönetilen örneği'nde çalışan veritabanı kullanır.
6. Bu noktada, bunlar şirket içi SQLVM makineyi kapatmanız yeterlidir. Geçiş tamamlandı.

*Daha fazla yardıma mı ihtiyacınız var?*

- Bilgi edinmek için nasıl [yük devretme testi çalıştırma](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure). 
- Bilgi edinmek için nasıl [bir kurtarma planı oluşturma](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans).
- Bilgi edinmek için nasıl [Azure'a yük devretme](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover).

## <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Geçiş tamamlandı, SmartHotel360 uygulamayı bir Azure sanal makinesinde çalışan ve SmartHotel360 veritabanı, Azure SQL veritabanı yönetilen örneği'nde kullanılabilir.  

Şimdi, Contoso aşağıdaki temizleme görevlerini yapmanız gerekir:  

- WEBVM makine vCenter sunucu envanterinden kaldırma.
- SQLVM makine vCenter sunucu envanterinden kaldırma.
- WEBVM ve sqlvm ADLI yerel yedekleme işlerden kaldırın.
- İç belgeleri için WEBVM IP adresini ve yeni konumunu gösterecek şekilde güncelleştirin.
- SQLVM iç belgelerinden kaldırın. Alternatif olarak, Contoso SQLVM silindi olarak göster ve artık sanal Makineye stok için belgeleri gözden geçirebilirsiniz.
- Yetkisi alınan Vm'leri ile etkileşimde bulunan tüm kaynakları gözden geçirin. Herhangi bir ilgili ayarları veya belgeleri yeni yapılandırmayı yansıtacak şekilde güncelleştirin.

## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Azure'da geçirilen kaynakları ile tam olarak çalışır hale getirme ve yeni temel altyapıyla güvenli Contoso gerekir.

### <a name="security"></a>Güvenlik

Contoso güvenlik ekibi, Azure VM ve SQL veritabanı yönetilen örneği, bir uygulama güvenlik sorunları olup olmadığını denetlemek için gözden geçirmeleri:

- Takım, VM için erişimi denetlemek için kullanılan ağ güvenlik grupları inceler. Ağ güvenlik grupları uygulamaya izin yalnızca bu trafiği sağlamak geçirebilirsiniz.
- Contoso güvenlik ekibi, ayrıca Azure Disk şifrelemesi ve Azure anahtar Kasası'nı kullanarak diskteki verilerin güvenliğini sağlama dikkate almaktır.
- Takım, yönetilen örnek tehdit algılamayı etkinleştirir. Tehdit algılama, tehdit algılanırsa bilet için Contoso'nun güvenlik takım/hizmet Masası sistem için bir uyarı gönderir. Daha fazla bilgi edinin [tehdit algılama yönetilen örneği için](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-threat-detection).

     ![Yönetilen örnek güvenlik - tehdit algılama](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)  

VM'ler için önerilen güvenlik uygulamaları hakkında daha fazla bilgi edinmek için [Azure Iaas iş yükleri için en iyi güvenlik uygulamaları](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms).

### <a name="bcdr"></a>BCDR

Contoso, iş sürekliliği ve olağanüstü durum kurtarma (BCDR) için aşağıdaki işlemleri yapar:

- Verileri güvende tutun: Contoso Vm'leri Azure Backup hizmetini kullanarak verileri yedekler. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- Uygulamalarınızı çalışır halde tutun: Contoso uygulama Azure sanal makinelerini Site Recovery kullanarak ikincil bir bölgeye çoğaltır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart).
- Contoso yönetilen SQL örneği yönetme hakkında daha fazla öğrenir dahil olmak üzere [veritabanı yedeklemeleri](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).


### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

- Contoso, varolan bir lisans için WEBVM sahiptir. Azure karma avantajı ile fiyatlandırmanın avantajlarından yararlanmak için mevcut bir Azure VM'yi Contoso dönüştürür.
- Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet yönetimi sağlar. Maliyet yönetimi, Contoso kullanın ve Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olan çoklu bulut maliyet yönetimi çözümüdür. Daha fazla bilgi edinin [Azure maliyet Yönetimi](https://docs.microsoft.com/azure/cost-management/overview). 


## <a name="conclusion"></a>Sonuç

Bu makalede, Contoso uygulama geçiş yaparak Azure SmartHotel360 uygulamada rehosts Site Recovery hizmetini kullanarak azure'a ön uç VM'si. Contoso, şirket içi veritabanının Azure SQL veritabanı yönetilen örneği için Azure veritabanı geçiş hizmeti kullanarak geçirir.

## <a name="next-steps"></a>Sonraki adımlar

Contoso serideki sonraki makalede [SmartHotel360 uygulama Azure sanal makinelerinde rehosts](contoso-migration-rehost-vm.md) Azure Site Recovery hizmetini kullanarak.

