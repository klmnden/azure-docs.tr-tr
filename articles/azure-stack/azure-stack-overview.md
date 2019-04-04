---
title: Azure Stack nedir? | Microsoft Docs
description: Azure Stack, veri merkezinizde Azure hizmetlerini çalıştırmak için nasıl sağladığını öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/29/2019
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: ''
ms.lastreviewed: 03/29/2019
ms.openlocfilehash: e5e05ad4f2ae7e718e160916144f03bfb74a2a52
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631314"
---
# <a name="azure-stack-overview"></a>Azure Stack’e genel bakış

Azure Stack, bir şirket içi ortamda uygulamalarını çalıştırmak ve veri merkezinizde Azure Hizmetleri sunmak için bir yol sağlayan Azure'nın bir uzantısıdır. Tutarlı bir bulut platformuyla, kuruluşların güvenle teknoloji sınırlamalar dayalı iş kararları yerine iş gereksinimlerine göre teknoloji kararları yapabilirsiniz.

## <a name="why-use-azure-stack"></a>Azure Stack neden kullanmalısınız?

Azure, geliştiricilerin modern uygulamalar oluşturmak için zengin bir platform sağlar. Ancak, bazı bulut tabanlı uygulamalar gibi gecikme süresi, aralıklı bağlantı ve düzenlemeleri engellerini karşı karşıyadır. Azure ve Azure Stack, hem müşterilere yönelik hem de iç iş kolu uygulamaları için yeni hibrit bulut kullanım örnekleri kilidini açma:

- **Uç çözümlerinde ve bağlantısız çözümlerde**. Azure Stack'te yerel olarak veri işleme ve ardından her ikisi arasında ortak bir uygulama mantığı ile daha fazla analiz için Azure'da yeniden tarafından gecikme süresi ve bağlantı gereksinimleri adresi. Azure Stack internet bağlantısı olmadan Azure'a bağlantısı kesildi bile dağıtabilirsiniz. Fabrika katlarında düşünün, verilir ve örnekler benim Miller İyi Yolculuklar partimize.

- **Bulut çeşitli düzenlemelerini karşılamak uygulamalarına**. Geliştirin ve şirket içi Azure Stack, Mevzuat karşılamak için veya ilke gereksinimleri ile hiçbir kod değişikliğine ihtiyaç dağıtmak için tam esneklik, azure'daki uygulamalar dağıtın. Uygulama örnekleri, genel denetim, finansal raporlama, döviz alım-satım, çevrimiçi oyun ve harcama raporlama içerir.

- **Bulut uygulaması modeli şirket içi**. Azure Hizmetleri, kapsayıcılar, sunucusuz ve mikro hizmet mimarileri güncelleştirmek ve mevcut uygulamalarınızı genişletin veya yenilerini oluşturmak için kullanın. Azure bulut arasında tutarlı kullanımı DevOps işler ve Azure Stack çekirdek görev açısından kritik uygulamalar için uygulama modernizasyonu hızlandırmak için şirket.

Azure Stack sağlayarak bu kullanım senaryolarına olanak sağlar:

- Bir duruma getirmek için tümleşik deneyiminin ve amaca yönelik olarak tasarlanan Azure Stack ile tümleştirilmiş sistemlerle güvenilen donanım iş ortaklarından. Teslimden sonra Azure Stack veri merkezinde System Center Operations Manager Yönetim Paketi veya Nagios uzantısı izleme ile kolayca tümleştirir.

- Azure'da ve Azure Stack'te karma ortamlar için Azure Active Directory (Azure AD) kullanarak ve Active Directory Federasyon Hizmetleri (AD FS) için yararlanarak esnek Kimlik Yönetimi, dağıtımları bağlantısı kesildi. 

- Geliştirici üretkenliğini en üst düzeye çıkarmak ve ortak DevOps etkinleştirmek için Azure ile tutarlı uygulama geliştirme ortamı, karma ortamlarda yaklaşıyor.

- Azure hizmetlerinin tesliminden hibrit bulut bilgi işlem kullanarak şirket içi. Azure ve Azure Stack'te Azure aynı yönetim deneyimleri ve araçları kullanarak Azure Iaas ve PaaS Hizmetleri dağıtmanıza ve ortak işlemsel uygulamalar benimseyin. Microsoft Azure Stack, yeni Azure Hizmetleri, mevcut hizmet ve ek Azure Market uygulamalar ve görüntüleri güncelleştirmeleri dahil olmak üzere Azure sürekli yenilik sunar.

## <a name="azure-stack-architecture"></a>Azure Stack mimarisi
Azure Stack tümleşik sistemleri tarafından oluşturulan 4-16 sunucu rafları içinde oluşur, güvenilen donanım iş ortakları ve düz veri Merkezinize teslim edilir. Teslimden sonra çözümü sağlayıcısı tümleşik sistem dağıtmak ve Azure Stack çözüm iş gereksinimlerinizi karşıladığından emin olmak için sizinle birlikte çalışır. Veri merkezinizi sağlama tüm gerekli güç ve soğutma hazırlamak gerekir, kenarlık bağlantısı ve diğer gerekli veri merkezi tümleştirmesi gereksinim yerinde olduğundan. 

> Azure Stack veri merkezi tümleştirme deneyimi hakkında daha fazla bilgi için bkz. [Azure Stack'i veri merkezi tümleştirmesi](azure-stack-customer-journey.md).

Azure Stack sektörde standart donanım üzerinde oluşturulmuştur ve zaten Azure Aboneliklerini yönetmek için kullandığınız aynı araçları kullanılarak yönetilir. Sonuç olarak, veya Azure'a bağlı olup olmadığını tutarlı bir DevOps süreçlerini uygulayabilir. 

Azure yığını mimarisinin, Azure hizmetlerini uçta uzak konumlardan veya internet'ten bağlantı kesildi, aralıklı bağlantı sağlamak sağlar. Azure Stack'te yerel olarak veri işleme ve sonra toplama, Azure'da ek işleme ve analiz için hibrit çözümler oluşturabilirsiniz. Son olarak, Azure Stack şirket yüklü olduğundan, herhangi bir kod değişikliği yapmadan bulut uygulaması şirket içi dağıtma esnekliğiyle özgü Mevzuat veya ilke gereksinimleri karşılayabilirsiniz. 

## <a name="deployment-options"></a>Dağıtım seçenekleri

### <a name="production-or-evaluation-environments"></a>Üretim ya da değerlendirme ortamı
Azure Stack, Azure Stack tümleşik sistemleri üretim kullanımı için ve Azure Stack değerlendirme için Azure Stack geliştirme Seti'ni'nı (ASDK) sizin ihtiyaçlarınızı karşılamak üzere iki dağıtım seçeneği, sunulur:

- **Azure Stack tümleşik sistemleri**. Azure Stack tümleşik sistemleri bir çözüm oluşturma Microsoft ve donanım iş ortakları, iş ortaklığı aracılığıyla sunulan bulut hızını sizin belirlediğiniz yenilik ve bilgi işlem yönetimi kolaylık sunar. Azure Stack bir tümleşik donanım ve yazılım sistemi sunulan olduğundan, esneklik ve buluttan yenilik olanağı yanı sıra, gereken denetim sahip. Azure Stack tümleşik sistemleri boyutu 4-16 düğümlerden aralığı ve tüm dünyada donanım iş ortağı ve Microsoft tarafından desteklenir. Yeni senaryoları oluşturabilir ve üretim iş yükleri için yeni çözümlerini dağıtmak için Azure Stack tümleşik sistemleri kullanın.

- **Azure Stack geliştirme Seti'ni**. [Azure Stack geliştirme Seti'ni (ASDK)](./asdk/asdk-what-is.md) değerlendirmek ve Azure Stack hakkında bilgi edinmek için kullanabileceğiniz bir Azure Stack ücretsiz, tek düğümlü dağıtımıdır. Azure ile tutarlı olan araç ve API'leri kullanarak uygulamalar oluşturmak için ASDK bir geliştirme ortamı olarak da kullanabilirsiniz. Ancak, ASDK üretim ortamı olarak kullanılmak üzere tasarlanmamıştır ve tam tümleşik sistemleri Üretim dağıtımı karşılaştırıldığında aşağıdaki sınırlamalara sahiptir:

    - ASDK yalnızca bir tek Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kimlik sağlayıcısı ile ilişkilendirilebilir.
    - Azure Stack bileşenleri tek bir ana bilgisayarda dağıtıldığından, Kiracı kaynaklarına için sınırlı fiziksel kaynakları vardır. Bu yapılandırma, Ölçek veya performans değerlendirmesi için tasarlanmamıştır.
    - Ağ senaryoları NIC dağıtım gereksinimleri ve tek bir konak nedeniyle sınırlıdır.

### <a name="connection-models"></a>Bağlantı modelleri
Ya da Azure Stack dağıtmayı seçtiğiniz **bağlı** İnternet'e (ve Azure) veya **bağlantısı kesildi** almaktır. Bu seçenek (Azure AD veya AD FS) kimlik deposu ve faturalandırma modeli için hangi seçenekler kullanılabilir tanımlar (siz kullanım tabanlı ödemesi fatura veya kapasite tabanlı faturalandırma).

> Daha fazla bilgi için dikkat edilecek noktalara bakın [bağlı](azure-stack-connected-deployment.md) ve [bağlantısı kesildi](azure-stack-disconnected-deployment.md) dağıtım modelleri. 

### <a name="identity-provider"></a>Kimlik sağlayıcısı 
Azure Stack Azure Stack kimlikler için kimlik sağlayıcısı olarak Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kullanır. 

> [!IMPORTANT]
> Bir anahtar karar noktası budur! Seçme Azure AD veya AD FS, kimlik sağlayıcısı olarak, dağıtım sırasında yapmanız gereken tek seferlik bir karardır. Bu daha sonra tüm sistemin yeniden dağıtmadan değiştiremezsiniz.

Azure AD, Microsoft'un bulut tabanlı, çok kiracılı kimlik sağlayıcıdır. Karma senaryo dağıtımları İnternet'e bağlı olan Azure AD kimlik deposu olarak kullanırsınız. Ancak, Azure Stack'i bağlantısız dağıtımları için Active Directory Federasyon Hizmetleri (AD FS) kullanmayı tercih edebilirsiniz. Azure AD ile yaptıkları gibi azure Stack kaynak sağlayıcıları ve diğer uygulamalar çok AD FS ile aynı şekilde çalışır. Azure Stack, kendi Active Directory örneğine ve bir Active Directory Graph API'sini içerir. 

> Azure Stack kimlik konuları hakkında daha fazla bilgi [kimlik Azure Stack için genel bakış](azure-stack-identity-overview.md).

## <a name="how-is-azure-stack-managed"></a>Azure Stack nasıl yönetilir?
Azure Stack tümleşik sistemleri dağıtım ya ASDK yüklemesinin dağıtıldıktan sonra Azure Stack ile etkileşim kurmanın birincil Yönetim Portalı, kullanıcı portalı ve PowerShell yöntemlerdir. Azure Stack portalı her ayrı örnekleri, Azure Resource Manager tarafından desteklenir. Bir **Azure Stack operatörü** Azure Stack, yönetmenizi ve Kiracı sunumları oluşturma gibi işlemler yapar ve tümleşik sistem durumu ve İzleyici durumunu korumak için Yönetim Portalı'nı kullanır. (Kiracı portalı olarak da bilinir) kullanıcı portalı için sanal makineler, depolama hesapları ve web apps gibi bulut kaynaklarını kullanım bir Self Servis deneyimi sağlar. 

> Azure Stack yönetim portalını kullanarak yönetme hakkında daha fazla bilgi için bkz [Azure Stack Yönetim Portalı hızlı başlangıcı](azure-stack-manage-portals.md).

Azure Stack operatör çok çeşitli hizmetler ve uygulamalar gibi gerçekleştirebiliyor [sanal makineler](azure-stack-tutorial-tenant-vm.md), [web uygulamaları](azure-stack-app-service-overview.md), yüksek oranda kullanılabilir [SQL Server](azure-stack-tutorial-sql.md), ve [MySQL Server](azure-stack-tutorial-mysql.md) veritabanları. Ayrıca [Azure Stack hızlı başlangıç Azure Resource Manager şablonları](https://github.com/Azure/AzureStack-QuickStart-Templates) SharePoint, Exchange ve daha fazlası dağıtılacak. 

Yönetim Portalı'nı kullanarak şunları yapabilirsiniz [Hizmetleri sunmak için Azure Stack yapılandırma](azure-stack-plan-offer-quota-overview.md) planlarını, kotaları, teklifler ve abonelikler kullanarak kiracılara. Kiracı kullanıcılar için birden çok teklife abone olabilirsiniz. Bir veya daha fazla plan Teklife sahip olabilirsiniz ve bir veya daha fazla hizmet planları olabilir. İşleçler kapasitesini yönetmek ve uyarılarını yanıtlama. 

Azure Stack yapılandırıldığında, bir **Azure Stack kullanıcısı** (Kiracı olarak da bilinir) işleci sağlayan hizmetlerini kullanır. Kullanıcılar sağlama, izleme ve bunlar, web uygulamaları, depolama ve sanal makineler gibi abone olduğunuz Hizmetleri yönetin.

> Ne nereye, tipik işletmen sorumlulukları kullanmak için hesapları dahil olmak üzere, Azure Stack yönetme hakkında daha fazla bilgi edinmek için kullanıcılarınıza söylemeniz gerekenler ve Yardım almak nasıl gözden [Azure Stack yönetim temel bilgileri](azure-stack-manage-basics.md).

## <a name="resource-providers"></a>Kaynak sağlayıcıları 
Kaynak sağlayıcıları için tüm Azure Stack Iaas temelini web hizmetleri ve PaaS Hizmetleri altındadır. Azure Resource Manager hizmetine erişim sağlamak için farklı kaynak sağlayıcıları kullanır. Her kaynak sağlayıcısı yapılandırmanız ve ilgili kaynaklarını denetlemenize yardımcı olur. Hizmet yöneticileri, yeni özel kaynak sağlayıcılarını da ekleyebilirsiniz. 

### <a name="foundational-resource-providers"></a>Temel kaynak sağlayıcıları 
Üç temel Iaas kaynak sağlayıcıları vardır: İşlem, ağ ve Depolama:

- **İşlem**. İşlem kaynak sağlayıcısı, Azure Stack kiracıların kendi sanal makinelerini oluşturma sağlar. İşlem kaynak sağlayıcısı sanal makineleri, hem de sanal makine uzantıları oluşturmak olanağı içerir. Sanal makine uzantısı hizmeti, Iaas özelliklerini sağlamak için Windows ve Linux sanal makineleri yardımcı olur.  Örneğin, bir Linux sanal makinesi sağlama ve VM'yi yapılandırmak için dağıtım sırasında Bash betikleri çalıştırmak için işlem kaynak Sağlayıcısı'nı kullanabilirsiniz.
- **Ağ kaynak sağlayıcısı**. Ağ kaynak sağlayıcısı, yazılım tanımlı ağ (SDN) ve ağ işlevi sanallaştırma (NFV) özelliklerini özel bulut için bir dizi sunar. Ağ kaynak sağlayıcısı, yazılım yük dengeleyici, genel IP'ler, ağ güvenlik grupları ve sanal ağlar gibi kaynakları oluşturmak için kullanabilirsiniz.
- **Depolama kaynak sağlayıcısı**. Depolama kaynak sağlayıcısı dört tutarlı Azure depolama hizmetleri sunar: [blob](https://docs.microsoft.com/azure/storage/common/storage-introduction#blob-storage), [kuyruk](https://docs.microsoft.com/azure/storage/common/storage-introduction#queue-storage), [tablo](https://docs.microsoft.com/azure/storage/common/storage-introduction#table-storage), yönetim sağlayan KeyVault hesap yönetimi ve parolalar ve sertifikalar gibi gizli denetimi. Depolama kaynak sağlayıcısı, bulut depolama yönetim hizmeti tutarlı Azure depolama hizmetleri hizmet sağlayıcısı yönetimini kolaylaştırmak için de sunar. Azure depolama, depolama ve büyük miktarlardaki yapılandırılmamış veriler, belgeler ve medya dosyalarını Azure BLOB'ları ile gibi alma esnekliği sağlar ve Azure tabloları verilerle yapılandırılmış NoSQL tabanlı. 

### <a name="optional-resource-providers"></a>İsteğe bağlı kaynak sağlayıcıları
Dağıtma ve Azure Stack ile kullanmak üç isteğe bağlı PaaS kaynak sağlayıcıları vardır: App Service, SQL Server ve MySQL Server kaynak sağlayıcıları:

- **App Service**. [Azure Stack'te Azure App Service](azure-stack-app-service-overview.md) bir platform-bir hizmet olarak (PaaS) teklifi, Microsoft Azure, Azure Stack için kullanılabilir. Web API ve Azure işlevleri'ni oluşturmak iç veya dış müşterilere hizmet sağlar herhangi bir platform veya cihaz için uygulamalar. 
- **SQL Server**. Kullanım [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider.md) SQL veritabanları Azure Stack, hizmet olarak sunmak için. Kaynak sağlayıcısını yüklemek ve bir veya daha fazla SQL Server örneklerine bağlanabilirsiniz sonra siz ve kullanıcılarınız bulutta çalışan uygulamalar SQL kullanan Web siteleri ve SQL kullanan diğer iş yükleri için veritabanları oluşturabilirsiniz.
- **MySQL sunucusu**. Kullanım [MySQL Server Kaynak sağlayıcısı](azure-stack-mysql-resource-provider-deploy.md) MySQL veritabanları Azure Stack hizmet olarak kullanıma sunmak için. MySQL kaynak sağlayıcısı, bir hizmet olarak Windows Server 2016 Server Core sanal makinede (VM) çalışır.

## <a name="providing-high-availability"></a>Yüksek kullanılabilirlik sağlama
Bir çoklu VM üretim sisteminin azure'da yüksek kullanılabilirlik elde etmek için Vm'leri yerleştirilir bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) , bunları birden çok hata etki alanları ve güncelleme etki alanları arasında yayılır. Azure Stack daha küçük ölçek hata etki alanı bir kullanılabilirlik kümesinde tek bir düğüm ölçek birimi olarak tanımlanır.  

Azure Stack altyapısını zaten hatalara karşı dayanıklı olsa da, bir donanım hatası olursa (Yük Devretme Kümelemesi) temel alınan teknoloji hala bazı kapalı kalma süresi VM'ler için etkilenen bir fiziksel sunucuda artmasına neden olur. Azure Stack, Azure ile tutarlı olacak şekilde en fazla üç hata etki alanı ile bir kullanılabilirlik sahip destekler.

- **Hata etki alanları**. Vm'leri bir kullanılabilirlik kümesine yerleştirilir bunları mümkün olduğunca eşit olarak birden çok hata etki alanları üzerinde (Azure Stack düğüm) yayarak birbirinden fiziksel olarak izole edilmiş olur. Bir donanım hatası varsa, başarısız hata etki alanı Vm'lerden diğer hata etki alanları yeniden, ancak, mümkün olduğunda, aynı kullanılabilirlik kümesindeki diğer vm'lerden ayrı hata etki alanlarında tutulur. Donanım tekrar çevrimiçi olduğunda, yüksek kullanılabilirliği sürdürmek için Vm'leri yeniden Dengelenecek. 
 
- **Güncelleme etki alanları**. Güncelleştirme etki alanlarını kullanılabilirlik kümelerinde yüksek kullanılabilirlik sağlayan başka bir Azure kavramdır. Bir güncelleme etki alanı, aynı anda bakımdan geçirilebilen temel alınan donanım mantıksal grubudur. Aynı güncelleştirme etki alanında bulunan VM'ler, planlanan bakım sırasında birlikte yeniden başlatılır. Kiracılar, bir kullanılabilirlik kümesinde VM'ler oluşturduğunuzda, Azure platformu otomatik olarak Vm'leri bunlar arasında dağıtır güncelleştirme etki alanı. Azure Stack'te kendi temel konak güncelleştirilmeden önce kümedeki çevrimiçi diğer konaklar arasında geçişi, Vm'leri Canlı. Bir konak güncelleştirme sırasında kapalı kalma süresi olmadan Kiracı olduğundan, Azure Stack'te güncelleştirme etki alanı özelliği yalnızca şablon Azure ile uyumluluk için bulunmaktadır. 

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi
Abonelik, kaynak grubu veya tek başına bir kaynak düzeyinde rolleri atayarak yetkili kullanıcılar, gruplar ve Hizmetleri için sistem erişimi vermek için rol tabanlı erişim denetimi (RBAC) kullanabilirsiniz. Her bir rol, bir kullanıcı, Grup veya hizmet Microsoft Azure Stack kaynaklara sahip erişim düzeyini tanımlar.

Azure Stack RBAC, tüm kaynak türleri için geçerli olan üç temel rolüne sahiptir: Sahip, katkıda bulunan ve okuyucu. Sahip tüm kaynaklara temsilci erişimi başkalarına hakkı dahil olmak üzere tam erişimi vardır. Katkıda bulunan oluşturabilir ve tüm Azure kaynakları türlerini yönetmek, ancak diğerleri için erişim izni veremiyor. Okuyucu, mevcut kaynaklar yalnızca görüntüleyebilir. RBAC rolleri kalan belirli bir Azure kaynak yönetimi sağlar. Örneğin, sanal makine Katılımcısı rolü oluşturma ve sanal makinelerin yönetimini sağlar ancak Yönetim sanal ağ veya sanal makine bağlanan bir alt ağ izin vermiyor.

> Bkz: [Manage Role-Based erişim denetimi](azure-stack-manage-permissions.md) daha fazla bilgi için. 

## <a name="reporting-usage-data"></a>Kullanım verilerini raporlama
Microsoft Azure Stack toplar ve tüm kaynak sağlayıcılarını kullanım verilerini toplayan ve bunu Azure'a işleme için Azure ticaret tarafından iletir. Azure Stack'te toplanan kullanım verileri, bir REST API aracılığıyla görüntülenebilir. Bir Azure ile tutarlı Kiracı API'si yanı sıra sağlayıcısı ve sağlayıcı API'leri temsilci tüm Kiracı aboneliklerindeki kullanım verilerini almak için yoktur. Bu veriler, bir dış aracı ya da hizmet için fatura veya geri ödeme ile tümleştirmek için kullanılabilir. Azure ticaret tarafından kullanım işlendikten sonra Azure fatura Portalı'nda görüntülenebilir.

> Daha fazla bilgi edinin [Azure'da Azure Stack kullanım verilerini raporlama](azure-stack-usage-reporting.md).

## <a name="next-steps"></a>Sonraki adımlar

[Azure yığını ve genel Azure karşılaştırın](compare-azure-azure-stack.md)

[Yönetim temel bilgileri](azure-stack-manage-basics.md)

[Hızlı Başlangıç: Azure Stack yönetim portalını kullanın](azure-stack-manage-portals.md)