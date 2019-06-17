---
title: Azure için güvenilir Internet bağlantıları Kılavuzu
description: Azure ve SaaS Hizmetleri için güvenilir Internet bağlantıları Kılavuzu
services: security
author: dlapiduz
ms.assetid: 09511e03-a862-4443-81ac-ede815bdaf25
ms.service: security
ms.topic: article
ms.date: 06/20/2018
ms.author: dlap
ms.openlocfilehash: bb186ab2700b147bee3a7dd81474409ccafb76fc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60608092"
---
# <a name="trusted-internet-connections-guidance"></a>Güvenilir bir Internet bağlantıları Kılavuzu

Bu makalede, devlet kurumları ile güvenilir Internet bağlantılarını (TIC) girişim uyumluluğu ulaşmaya yardımcı olmak için Microsoft Azure kamu nasıl kullanabileceğiniz anlatılmaktadır. Makale Azure kamu Azure altyapıda bir hizmet (Iaas) ve Azure platform olarak hizmet (PaaS) teklifi olarak nasıl kullanılacağını açıklar.

## <a name="trusted-internet-connections-overview"></a>Güvenilir bir Internet bağlantıları genel bakış

TIC girişim amacı, en iyi duruma getirmek ve güvenlik, federal Aracılar tarafından kullanılan tek tek dış ağ bağlantılarının standartlaştırmak sağlamaktır. İlke açıklandığı [Office Yönetim ve bütçe (OMB) Memorandum M-08-05](https://georgewbush-whitehouse.archives.gov/omb/memoranda/fy2008/m08-05.pdf).

Kasım 2007'de, federal ağ çevre güvenliği ve olay yanıtlama işlevlerini geliştirmek için TIC program OMB kurdu. TIC belirli imzalar ve desen tabanlı bir veri tanımlamak için tüm gelen ve giden .gov trafiği ağ analiz gerçekleştirmek üzere tasarlanmıştır. TIC botnet etkinliği gibi davranış anormallikleri açıklığa kavuşturur. Kurumlar, dış ağ bağlantılarını birleştirmek ve izinsiz giriş algılama ve önleme cihazları EINSTEIN bilinen aracılığıyla tüm trafiği yönlendirmek için uygulanan. Cihazları olarak adlandırılır ağ uç noktaları sınırlı sayıda konumunda barındırılan _internet bağlantıları güvenilen_.

TIC amacı bilmek, kuruluşları için verilmiştir:
- (Yetkili veya yetkisiz) Ağımdaki kimdir?
- Ne zaman ağım erişilen ve neden?
- Hangi kaynaklara erişildiğini?

Tüm Ajans dış bağlantıları şu anda bir OMB onaylı TIC yönlendirmek gerekir. Federal kurumlar bir TIC erişim sağlayıcısı (TICAP) olarak veya ana Katman 1 internet hizmet sağlayıcıları birini hizmetleriyle ihtiyaçlarımıza göre TIC programa katılmak için gerekli değildir. Bu sağlayıcılar yönetilen güvenilir Internet Protokolü hizmet (MTIPS) sağlayıcıları olarak adlandırılır. TIC kurumu ve MTIPS sağlayıcısı tarafından gerçekleştirilen zorunlu kritik özellikleri içerir. Hızlandırılmış geçerli sürümünde TIC EINSTEIN sürüm 2 izinsiz giriş algılama ve EINSTEIN sürüm 3 (3A), her TICAP ve MTIPS yetkisiz erişim önleme cihazları dağıtılır. Kurum bir "Memorandum'ın anlayış" ile departmanı, anlarında güvenlik (EINSTEIN özellikleri federal sistemlerini dağıtmak için DHS) oluşturur.

Sorumluluğunu .gov ağı korumak için bir parçası olarak, ham veri akışları federal kuruluş genelinde olayların ilişkilendirin ve özel araçlar kullanarak çözümlemeler gerçekleştirmek için net akışı veri Ajans DHS gerektirir. DHS yönlendiriciler girer veya bir arabirim çıkar IP ağ trafiğini toplama olanağı sağlar. Ağ yöneticileri, kaynak ve hedef trafik hizmeti sınıfının belirlemek ve benzeri için net akış verileri analiz edebilirsiniz. NET akış veri "içerik veri" olarak kabul edilir üst bilgisi, kaynak IP, hedef IP ve benzeri benzer. İçeriği olmayan veri sağlayan DHS içeriği hakkında bilgi edinmek: kimin yaptığını nasıl ve ne kadar süre.

Girişim, güvenlik ilkeleri, Kılavuzlar ve şirket içi altyapı varsayar çerçeveleri de içerir. Devlet kurumları, maliyet tasarrufu, işlem verimliliğini ve yenilik yapmanın buluta taşımak gibi uygulama gereksinimlerini TIC ağ trafiği yavaşlatabilir. Hız ve çeviklikle hangi devlet kurumları ile kullanıcıların bulut tabanlı verilerine erişebilir sınırlı sonucunda.

## <a name="azure-networking-options"></a>Azure ağ seçenekleri

Azure hizmetlerine bağlanmak için üç ana seçeneğiniz vardır:

- Doğrudan internet bağlantısı: Azure hizmetlerini açık internet bağlantısı aracılığıyla doğrudan bağlanın. Orta ve bağlantı büyük/küçük harf ortaktır. Uygulama ve aktarım düzeyinde şifreleme sonrasında gizlilik emin olmak için yararlandı. Bant genişliği, bir sitenin internet bağlantısı ile sınırlıdır. Dayanıklılık sağlamak için birden fazla etkin sağlayıcısını kullanın.
- Sanal özel ağ (VPN): Azure sanal ağınız için özel olarak bir VPN ağ geçidi kullanarak bağlanın.
Bir sitenin standart internet bağlantısı geçirir, ancak gizlilik emin olmak için bir tünel bağlantısı şifreli olduğundan ortak ortamdır. VPN cihazları ve seçilen yapılandırmasına bağlı olarak, bant genişliği sınırlıdır. Azure noktadan siteye bağlantılar için 100 MB/sn genellikle sınırlıdır ve siteden siteye bağlantılar için 1,25 Gbps sınırlıdır.
- Azure ExpressRoute: ExpressRoute, Microsoft hizmetlerine doğrudan bir bağlantıdır. Bağlantı bir yalıtılmış fiber kanal aracılığıyla olduğundan, genel veya özel kullanılan yapılandırmasına bağlı olarak bağlantı olabilir. Bant, genellikle en fazla 10 GB/sn sınırlıdır.

TIC ek H (bulut konuları), departman, anlarında Security'nin belirtildiği gibi gereksinimlerinize birkaç şekilde "Güvenilen Internet bağlantılarını (TIC) başvuru mimarisi belgesini, sürüm 2.0." Bu makalede, DHS TIC yönergeler verilir **TIC 2.0**.

Bağlantıyı etkinleştirmek için **departman veya Ajansın (D/A)** Azure veya Office 365, D/A TIC aracılığıyla trafiği yönlendirme olmadan D/A şifrelenmiş bir tünel veya Bulut hizmeti sağlayıcısı (CSP) için ayrılmış bir bağlantı kullanmanız gerekir. CSP Hizmetleri D/bulut varlıklarını bağlantısını genel İnternet'e doğrudan Ajans personelin erişimi için sunulan değil emin olabilirsiniz.

Office 365 TIC 2.0 ek H ile uyumlu ya da ExpressRoute ile kullanarak [Microsoft Peering](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings) etkin veya TLS 1.2 kullanarak tüm trafiği şifreleyen internet bağlantısı. D/ağ D/A son kullanıcılara kendi Ajans ağ ve Internet üzerinden ONA altyapısı aracılığıyla bağlanabilirsiniz. Office 365 tüm uzak İnternet'e erişimi engellenir ve yolları Aracısı üzerinden. D/A etkin bir ExpressRoute bağlantısı üzerinden Microsoft Peering (genel eşdüzey hizmet sağlama türü) ile Office 365'e bağlantı kurabilir.  

Yalnızca Azure için internet erişimi sınırlayan hizmetleriyle birlikte kullanıldıktan olduğunda ikinci seçeneği (VPN) ve üçüncü bir seçenek (ExpressRoute) bu gereksinimleri karşılayabilirsiniz.

![Microsoft güvenilir Internet bağlantısı (TIC)](media/tic-diagram-a.png)

## <a name="azure-infrastructure-as-a-service-offerings"></a>Azure hizmet sunumlarına olarak altyapı

Azure müşterilerin kendi sanal ağ yönlendirme yönetmek için Azure Iaas kullanarak TIC ilke ile uyumluluğunu oldukça basittir.

Ana gereksinimini ONA başvuru mimarisi ile uyumluluk sağlamak amacıyla sanal ağınızda özel bir D/ağ uzantısıdır sağlamaktır. Olacak şekilde bir _özel_ uzantısı, ilke gerektiren hiçbir trafik TIC ağ bağlantısı şirket içi ağınız dışında bırakın. Bu işlem olarak bilinir _zorlamalı tünel_. TIC uyumluluk için işlem bir şirket içi ağ geçidi üzerinden bir kuruluşun ağındaki TIC üzerinden İnternet'e CSP ortamında herhangi bir sistemden tüm trafiği yönlendirir.

Azure Iaas TIC uyumluluk iki büyük adım ayrılmıştır:

- 1\. adım: Yapılandırma.
- 2\. adım: Denetim.

### <a name="azure-iaas-tic-compliance-configuration"></a>Azure Iaas TIC uyumluluk: Yapılandırma

Azure ile uyumlu TIC mimarisi yapılandırmak için ilk uygulamanızı sanal ağınıza doğrudan internet erişimini engellemek ve ardından internet trafiğini şirket içi ağ üzerinden zorlayan gerekir.

#### <a name="prevent-direct-internet-access"></a>Doğrudan internet erişimini engelle

Azure Iaas ağ, sanal makinelerin ağ arabirim denetleyicilerini (NIC'ler) ilişkili alt ağdan oluşan sanal ağlar yönetilir.

Bir sanal makine veya sanal makine koleksiyonunu herhangi bir dış kaynağa bağlanamıyor güvence altına almak için ONA uyumluluğunu desteklemek için basit bir senaryodur. Bağlantı kesilmesi dış ağlardaki ağ güvenlik grupları (Nsg'ler) kullanarak güvence altına alır. Nsg'ler, NIC'ler veya alt ağları sanal ağınızda bir veya daha fazla denetim trafiği için kullanın. NSG’de trafik yönüne, protokole, kaynak adresle bağlantı noktasına ve hedef adresle bağlantı noktasına göre trafiğe izin veren ya da reddeden erişim denetim kuralları yer alır. Bir NSG kurallarını dilediğiniz zaman değiştirebilirsiniz ve değişiklikleri ilişkili tüm örneklerine uygulanır. Bir NSG oluşturma hakkında daha fazla bilgi için bkz. [bir ağ güvenlik grubu ile ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

#### <a name="force-internet-traffic-through-an-on-premises-network"></a>İnternet trafiğini şirket içi ağ üzerinden zorla

Azure, sistem yollarını otomatik olarak oluşturur ve bir sanal ağ içindeki her alt ağa bu yolları atar. Oluşturamaz veya sistem yollarını kaldırma, ancak bazı sistem yollarını özel yollar ile geçersiz kılabilirsiniz. Azure, varsayılan sistem yollarını her alt ağ oluşturur. Belirli Azure özelliklerini kullandığınızda belirli alt ağlara veya her alt ağ, azure isteğe bağlı varsayılan yollar ekler. Bu tür bir yönlendirme sağlar:
- Sanal ağ içinde hedefleyen trafik, sanal ağ içinde kalır.
- Sanal ağ adres alanında dahil sürece, IANA tarafından atanan özel adres alanlarından 10.0.0.0/8 gibi bırakılır.
- "Son çare" 0.0.0.0/0 sanal ağ internet uç noktasına yönlendirme.

![TIC zorla tünel](media/tic-diagram-c.png)

Sanal ağının dışına çıktıklarında tüm trafiği, tüm trafiği D/A TIC erişir emin olmak için şirket içi bağlantısı yönlendirmek gerekir. Kullanıcı tanımlı yollar oluşturarak veya şirket içi ağ geçidiniz ile bir Azure VPN gateway arasında Border Gateway Protocol (BGP) rotaları değişimi özel rotalar oluşturun. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz. [sanal ağ trafiği yönlendirme: Kullanıcı tanımlı yollar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined). BGP hakkında daha fazla bilgi için bkz: [sanal ağ trafiği yönlendirme: Sınır Ağ Geçidi Protokolü](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol).

#### <a name="add-user-defined-routes"></a>Kullanıcı tanımlı yollar ekleme

Rota tabanlı sanal ağ geçidi kullanıyorsanız, Azure'da tünel zorlayabilirsiniz. 0\.0.0.0/0 trafiği yönlendirmek için ayarlayan bir kullanıcı tanımlı yol (UDR) ekleyin bir **sonraki atlama** sanal ağ geçidinizin. Azure kullanıcı tanımlı yollar üzerindeki sistem tarafından tanımlanan yollar önceliklendirir. Sanal olmayan tüm ağ trafiğini, trafiği şirket içi ardından yönlendirebilir, sanal ağ geçidine gönderilir. UDR tanımladıktan sonra yol mevcut alt ağlar veya içindeki tüm sanal ağları Azure ortamınızda yeni alt ağlar ile ilişkilendirin.

![Kullanıcı tanımlı yollar ve ONA](media/tic-diagram-d.png)

#### <a name="use-the-border-gateway-protocol"></a>Sınır Ağ Geçidi Protokolü kullanın

ExpressRoute veya BGP özellikli bir sanal ağ geçidi kullanıyorsanız, BGP yolların tanıtılması için tercih edilen mekanizmadır. Bir BGP tanıtılan yolu 0.0.0.0/0 için ExpressRoute ve BGP kullanan sanal ağ geçitleri, sanal ağlarınızı içindeki tüm alt ağlar varsayılan yolun uygulandığı emin olun.

### <a name="azure-iaas-tic-compliance-auditing"></a>Azure Iaas TIC uyumluluk: Denetim

Azure TIC uyumluluğunu denetlemek için çeşitli yollar sunar.

#### <a name="view-effective-routes"></a>Geçerli yollar bölümünü inceleyin

Varsayılan rotanız gözlemleyerek yayılır onaylayın _geçerli rotalar_ belirli bir sanal makine, belirli bir NIC veya bir kullanıcı tarafından tanımlanan rota tablosunda [Azure portalında](https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal#diagnose-using-azure-portal) veya [ Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-powershell#diagnose-using-powershell). **Geçerli rotalar** ilgili kullanıcı tanımlı yollar, BGP, yollar ve ilgili varlık üzerinde uygulanabilmesi sistem yolları aşağıdaki şekilde gösterildiği gibi bildirilen göster:

![Etkili yollar](media/tic-screen-1.png)

> [!NOTE]
> NIC çalışan bir sanal makineyle ilişkili olmadığı sürece, bir NIC için geçerli rotalar görüntüleyemezsiniz.

#### <a name="use-azure-network-watcher"></a>Azure Ağ İzleyicisi

Azure Ağ İzleyicisi TIC uyumluluk denetim çeşitli araçlar sunar. Daha fazla bilgi için [Ağ İzleyicisi hakkında bu genel bakışta](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).

##### <a name="capture-network-security-group-flow-logs"></a>Ağ güvenlik grubu akış günlüklerini yakalama 

IP trafiğinin çevreleyen meta verileri gösteren ağ akışı günlükleri tutmak için Ağ İzleyicisi'ni kullanın. Ağ akışı günlükleri, kaynak ve hedef adresleri hedefleri ve diğer verileri içerir. Sanal ağ geçidinden günlükleri ile bu verileri kullanabilirsiniz, şirket içi cihazlar veya tüm trafiği şirket içi yolları izlemek için TIC kenar. 

## <a name="azure-platform-as-a-service-offerings"></a>Hizmet sunumları olarak Azure platformu

Azure depolama gibi Azure PaaS hizmetlerine, İnternet'ten erişilebilen bir URL aracılığıyla erişilebilir. Bir depolama hesabı gibi kaynak onaylı kimlik bilgileri kimseyle bir TIC geçiş olmadan herhangi bir konumdan erişebilirsiniz. Bu nedenle, birçok kamu müşterileri, Azure PaaS hizmetlerine TIC ilkeleri ile uyumlu olmayan yanlış sonlandırma. Birçok Azure PaaS hizmetlerine TIC ilkesiyle uyumlu olabilir. Bir hizmet mimarisi TIC uyumlu Iaas ortamı olarak aynı olmadığında uyumludur ([daha önce açıklandığı gibi](https://docs.microsoft.com/azure/security/compliance/compliance-tic#azure-infrastructure-as-a-service-offerings)) ve hizmet bir Azure sanal ağına eklenir.

Azure PaaS hizmetlerinin sanal ağ ile tümleştirildiğinde, bu sanal ağdan erişilebilen özel bir hizmettir. Özel 0.0.0.0/0 kullanıcı tanımlı yollar veya BGP üzerinden yönlendirmeyi uygulayabilirsiniz. Tüm İnternet'e bağlı trafiği TIC geçiş için şirket yönlendiren özel yönlendirme sağlar. Azure Hizmetleri, aşağıdaki desenleri kullanarak sanal ağlara tümleştirme:

- **Adanmış bir hizmet örneği dağıtmak**: Sayısı giderek artan PaaS Hizmetleri, sanal ağa bağlı uç noktaları ile ayrılmış örnek olarak dağıtılabilir. PowerApps için App Service ortamı, bir sanal ağa kısıtlı ağ uç noktası izin vermek için "Yalıtılmış" modunda dağıtabilirsiniz. App Service ortamı, ardından Azure Web Apps, Azure API Management ve Azure işlevleri gibi birçok Azure PaaS hizmetlerine barındırabilirsiniz.
- **Sanal ağ hizmet uç noktalarını kullanacak**: Sayısı giderek artan PaaS Hizmetleri, bir sanal ağ özel IP genel adresi yerine kendi uç nokta byok'ye geçme seçeneğiniz izin verin.

Ayrılmış örneğe ayrılmış bir sanal ağa dağıtımını ya da hizmet uç noktaları, Mayıs 2018'den itibaren kullanımını destekleyen hizmetleri aşağıdaki tablolarda listelenmiştir.

> [!Note]
> Kullanılabilirlik durumu ticari olarak kullanılabilen Azure hizmetlerine karşılık gelir. Azure kamu'da Azure Hizmetleri için kullanılabilirlik durumu beklemede'dir.

### <a name="support-for-service-endpoints"></a>Hizmet uç noktaları için destek

|Hizmet                        |Kullanılabilirlik      |
|-------------------------------|------------------|
|Azure Key Vault                | Özel önizleme  |
|Azure Cosmos DB                | Özel önizleme  |
|Kimlik Hizmetleri              | Özel önizleme  |
|Azure Data Lake                | Özel önizleme  |
|PostgreSQL için Azure Veritabanı  | Özel önizleme  |
|MySQL için Azure Veritabanı       | Özel önizleme  |
|Azure SQL Veri Ambarı       | Genel önizleme   |
|Azure SQL Veritabanı             | Genel kullanılabilirlik (GA) |
|Azure Storage                  | GA               |

### <a name="support-for-virtual-network-injection"></a>Destek için sanal ağ ekleme

|Hizmet                               |Kullanılabilirlik      |
|--------------------------------------|------------------|
|Azure SQL Veritabanı Yönetilen Örneği   | Genel önizleme   |
|Azure Kubernetes Hizmeti (AKS)        | Genel önizleme   |
|Azure Service Fabric                  | GA               |
|Azure API Management                  | GA               |
|Azure Active Directory                | GA               |
|Azure Batch                           | GA               |
|App Service Ortamı               | GA               |
|Redis için Azure Önbelleği                     | GA               |
|Azure HDInsight                       | GA               |
|Sanal makine ölçek kümesi             | GA               |
|Azure bulut Hizmetleri                  | GA               |


### <a name="virtual-network-integration-details"></a>Sanal ağ tümleştirmesi ayrıntıları

Aşağıdaki diyagramda PaaS hizmetlerine erişim için genel ağ akışı gösterilmektedir. Erişim, sanal ağ ekleme hem sanal ağ hizmet tüneli gösterilir. Ağ hizmeti ağ geçitleri, sanal ağlar ve hizmet etiketleri hakkında daha fazla bilgi için bkz: [ağ ve uygulama güvenlik grupları: Hizmet etiketleri](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

![TIC PaaS bağlantı seçenekleri](media/tic-diagram-e.png)

1. Özel bir bağlantı, ExpressRoute kullanarak Azure'a yapılır. Zorlamalı tünel ile ExpressRoute özel eşlemesi, tüm müşteri sanal ağ trafiği ExpressRoute üzerinden zorlamak ve şirket içi yedekleme için kullanılır. Microsoft Peering gerekli değildir.
2. Azure VPN ExpressRoute ve Microsoft Peering ile birlikte kullanıldığında ağ geçidi, müşteri sanal ağ ile şirket içi kenarı arasındaki uçtan uca IPSec şifrelemesi bindirebilirsiniz. 
3. İzin verme/reddetme müşterilere IP, bağlantı noktası ve protokole göre izin veren bir Nsg kullanarak müşteri sanal ağ için ağ bağlantısını kontrol edilir.
4. Müşteri sanal ağ, müşterinin hizmeti için hizmet uç noktası oluşturarak PaaS hizmetine genişletir.
5. PaaS Hizmeti uç noktası güvenli **varsayılan reddetme tüm** ve yalnızca müşteri sanal ağdaki belirli alt ağlar erişime izin verecek şekilde. Varsayılan da engeller Internet'ten bağlantıları içerir.
6. Müşteri sanal ağ içindeki kaynaklara erişmesi gereken diğer Azure Hizmetleri ya da olmalıdır:  
   - Doğrudan sanal ağa dağıtılır.
   - Seçmeli olarak izin verilen, ilgili Azure hizmeti kılavuzdan temel.

#### <a name="option-a-deploy-a-dedicated-instance-of-a-service-virtual-network-injection"></a>Seçenek A: Bir hizmeti (sanal ağ ekleme) ayrılmış bir örneğini dağıtma

Sanal ağ ekleme müşterilerin belirli bir Azure hizmetinin, HDInsight gibi kendi sanal ağına ayrılmış örnekleri seçmeli olarak dağıtmasına olanak sağlar. Hizmet örnekleri, bir müşterinin sanal ağ içinde ayrılmış bir alt ağa dağıtılır. Sanal ağ ekleme dışında Internet yönlendirilebilir adresleri üzerinden hizmeti kaynaklarına erişimi sağlar. Şirket içi örneklerini, ExpressRoute veya siteden siteye VPN aracılığıyla ortak Internet adres alanı için bir güvenlik duvarını açmak yerine, sanal ağ adres alanı, hizmet örneklerinin doğrudan erişmek için kullanın. Bir uç nokta için ayrılmış bir örnek eklendiğinde, aynı stratejiler Iaas TIC uyumluluk olduğu gibi kullanabilirsiniz. Varsayılan yönlendirme Internet bağlantılı trafiğin şirket içi için bağlı bir sanal ağ geçidine yönlendirilir sağlar. Daha fazla belirli alt ağ için Nsg aracılığıyla gelen ve giden erişimi denetleyebilirsiniz.

![Sanal ağ ekleme genel bakış](media/tic-diagram-f.png)

#### <a name="option-b-use-virtual-network-service-endpoints-service-tunnel"></a>Seçenek B: Sanal ağ hizmet uç noktaları (hizmet tüneli) kullanın

Azure çok kiracılı hizmetler artan sayıda sunan "hizmet uç noktaları." Hizmet uç noktaları, Azure sanal ağlarına tümleştirmek için alternatif bir yöntemdir. Sanal ağ hizmet uç noktaları, sanal ağ IP adres alanınızı ve hizmetine sanal ağınızın kimliğini doğrudan bağlantı üzerinden genişletin. Bir Azure hizmetine sanal ağ arasında trafik her zaman Azure omurga ağında kalır. 

Bir hizmet için hizmet uç noktası etkinleştirildikten sonra ilkeleri hizmet tarafından sunulan bu sanal ağa bağlantıları için hizmet kısıtlamak için kullanın. Erişim denetimleri platform Azure hizmeti tarafından uygulanır. Kilitli bir kaynağa erişimi yalnızca istek izin verilen sanal ağ veya alt ağ veya ExpressRoute kullanıyorsanız, şirket içi trafiği tanımlamak için kullanılan iki IP'ler kaynaklanan verilir. Etkili bir şekilde doğrudan bir PaaS hizmeti bırakmasını gelen/giden trafiği engellemek için bu yöntemi kullanın.

![Hizmet uç noktalarına genel bakış](media/tic-diagram-g.png)

## <a name="cloud-native-tools-for-network-situational-awareness"></a>Ağ bitenden için buluta özgü araçları

Azure, ağınızın trafik akışları anlamak için gerekli olan bitenden olmasını sağlamaya yardımcı olmak amacıyla bulutta yerel araçlar sağlar. Araçlar TIC İlkesi ile uyumluluk için gerekli değildir. Araçları, güvenlik özelliklerinizin büyük ölçüde artırabilir.

### <a name="azure-policy"></a>Azure İlkesi

[Azure İlkesi](../../governance/policy/overview.md) denetim ve uyumluluk girişimlerini zorlamak için daha iyi özelliği sayesinde kuruluşunuz tarafından sağlanan bir Azure hizmetidir. Müşteriler, planlama ve gelecekteki TIC uyumluluğu güvence altına almak için artık Azure İlkesi kuralları test.

Azure İlkesi, abonelik düzeyinde yöneliktir. Hizmeti gibi uyumluluk görevleri yapabileceğiniz merkezi bir arabirim sağlar:
- Girişimler yönetme
- İlke tanımları yapılandırın
- Uyumluluğu denetle
- Uyumluluğu zorlama
- Özel durumları yönetme

Birçok yerleşik tanımları yanı sıra yöneticileri, kendi özel tanımları basit JSON şablonları kullanarak tanımlayabilirsiniz. Microsoft, mümkün olduğunda zorlama üzerinde denetim önceliği önerir.

Aşağıdaki örnek ilkeleri TIC uyumluluk senaryolar için kullanılabilir:

|İlke  |Örnek senaryo  |Şablon  |
|---------|---------|---------|
|Kullanıcı tanımlı yol tablosu uygular. | Tüm sanal Ağları üzerindeki varsayılan yolu bir onaylı sanal ağ geçidi, şirket içi yönlendirme işaret ettiğinden emin olun.    | Bu başlama [şablon](../../governance/policy/samples/no-user-defined-route-table.md). |
|Bir bölgede Ağ İzleyicisi etkin değil olup olmadığını denetle.  | Tüm bölgeler kullanılan için Ağ İzleyicisi etkin olduğundan emin olun.  | Bu başlama [şablon](../../governance/policy/samples/network-watcher-not-enabled.md). |
|NSG her alt ağda x.  | Internet trafiğini bloke ile bir NSG (veya bir dizi onaylanan Nsg'ler) her sanal ağ içindeki tüm alt ağlara uygulandığından emin olun. | Bu başlama [şablon](../../governance/policy/samples/nsg-on-subnet.md). |
|NSG her NIC üzerinde x | Internet trafiğini bloke sahip bir NSG tüm sanal makinelerinde tüm ağ arabirimlerine uygulandığından emin olun. | Bu başlama [şablon](../../governance/policy/samples/nsg-on-nic.md). |
|Onaylanan bir sanal ağ, sanal makine ağ arabirimleri için kullanın.  | Tüm NIC'ler onaylı bir sanal ağda olduğundan emin olun. | Bu başlama [şablon](../../governance/policy/samples/use-approved-vnet-vm-nics.md). |
|İzin verilen konumlar. | Tüm kaynaklar bölgelere uyumlu sanal ağlar ve Ağ İzleyicisi yapılandırmayla dağıtıldığından emin olun.  | Bu başlama [şablon](../../governance/policy/samples/allowed-locations.md). |
|Aşağıdaki gibi izin verilmeyen kaynak türleri **Publicıp'lerinde**. | Uyumluluk planınız olmadan kaynak türleri dağıtımını engelliyor. Bu ilke, genel IP adresi kaynakları dağıtımını engellemek için kullanın. NSG kuralları etkili bir şekilde gelen internet trafiği engellemek için kullanılabilse de, daha fazla genel IP'ler kullanımını engelleyen saldırı yüzeyi küçültülür.   | Bu başlama [şablon](../../governance/policy/samples/not-allowed-resource-types.md).  |

### <a name="network-watcher-traffic-analytics"></a>Ağ İzleyicisi trafik analizi

Ağ İzleyicisi [trafik analizi](https://azure.microsoft.com/blog/traffic-analytics-in-preview/) akış günlüğü verileri ve diğer günlükler ağ trafiğini üst düzey bir genel bakış sağlar. Veri TIC uyumluluk denetimi ve sorunlu noktaları tanımlamak için yararlıdır. TIC yönlendirme için odaklanmış bir listesini almak ve internet ile iletişim kuran sanal makinelerin hızlı bir şekilde ekran için üst düzey panoyu kullanabilirsiniz.

![Trafik analizi](media/tic-traffic-analytics-1.png)

Kullanım **coğrafi harita** internet trafiğini sanal makineleriniz için olası fiziksel hedefleridir hızlı bir şekilde tanımlamak için. Tanımlamak ve şüpheli konumlardan veya desen değişiklikleri değerlendirin:

![Coğrafi harita](media/tic-traffic-analytics-2.png)

Kullanım **sanal ağlar topoloji** hızlı bir şekilde var olan sanal ağları anket:

![Ağ topolojisi haritası](media/tic-traffic-analytics-3.png)

### <a name="network-watcher-next-hop-tests"></a>Ağ İzleyicisi sonraki atlama testleri

Ağ İzleyicisi tarafından izlenen bölgelerdeki ağlar, sonraki atlama testleri oluşturabilecekler. Azure portalında bir kaynak ve hedef Ağ İzleyicisi'nin sonraki atlama hedefi çözümlemek için örnek ağ akış için girebilirsiniz. Bu test sanal makineleri ve sonraki atlama hedefi beklenen ağ sanal ağ geçidi olduğundan emin olmak için örnek Internet adreslerine göre çalıştırın.

![Sonraki atlama testleri](media/tic-network-watcher.png)

## <a name="conclusions"></a>Sonuçları

Microsoft Azure, Office 365 ve Dynamics 365 olarak yazılmış ve tanımlanmış Mayıs 2018 TIC 2.0 ek H rehberlik ile uyum sağlanmasına yardımcı erişimi kolayca yapılandırabilirsiniz. Microsoft, TIC Kılavuzu değişebilir olduğunu algılar. Yeni bir kılavuz yayınlandığında Kılavuzu zamanında karşılamak müşterilere yardımcı olmak Microsoft çalışmaları.

## <a name="appendix-trusted-internet-connections-patterns-for-common-workloads"></a>Ek: Ortak iş yükleri için güvenilir Internet bağlantıları desenleri

| Kategori | İş yükü | IaaS | PaaS ayrılmış / sanal ağ ekleme  | Hizmet uç noktaları  |
|---------|---------|---------|---------|--------|
| İşlem | Azure Linux sanal makineleri | Evet | | |
| İşlem | Azure Windows sanal makineleri | Evet | | |
| İşlem | Sanal makine ölçek kümeleri | Evet | | |
| İşlem | Azure İşlevleri | | App Service Ortamı | |
| Web ve mobil | Şirket içi web uygulaması | | App Service Ortamı| |
| Web ve mobil | İç Mobil uygulama | | App Service Ortamı | |
| Web ve mobil | API uygulamaları | | App Service Ortamı | |
| Kapsayıcılar | Azure Container Service | | | Evet |
| Kapsayıcılar | Azure Kubernetes Service'i (AKS) \* | | | Evet |
| Database | Azure SQL Veritabanı | | Azure SQL veritabanı yönetilen örneği \* | Azure SQL |
| Database | MySQL için Azure Veritabanı | | | Evet |
| Database | PostgreSQL için Azure Veritabanı | | | Evet |
| Database | Azure SQL Veri Ambarı | | | Evet |
| Database | Azure Cosmos DB | | | Evet |
| Database | Redis için Azure Önbelleği | | Evet | |
| Depolama | Azure Blob depolama | Evet | | |
| Depolama | Azure Dosyaları | Evet | | |
| Depolama | Azure kuyruk depolama | Evet | | |
| Depolama | Azure Tablo depolama | Evet | | |
| Depolama | Azure Disk Depolama | Evet | | |

\* Azure devlet kurumları, Mayıs 2018 genel önizlemede.
