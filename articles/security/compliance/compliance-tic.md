---
title: Azure için güvenilir bir Internet bağlantısı Kılavuzu
description: Azure ve SaaS Hizmetleri için güvenilir Internet bağlantısı Kılavuzu
services: security
author: dlapiduz
ms.assetid: 09511e03-a862-4443-81ac-ede815bdaf25
ms.service: security
ms.topic: article
ms.date: 06/20/2018
ms.author: dlap
ms.openlocfilehash: 7b813500eecba3aa1902c28b9b7c56da6c4516b7
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37967554"
---
# <a name="trusted-internet-connection-guidance"></a>Güvenilir bir Internet bağlantısı Kılavuzu

## <a name="background"></a>Arka plan

Güvenilir Internet bağlantılarını (TIC) girişim amacı en iyi duruma getirmek ve federal kurumlar tarafından şu anda dış ağ bağlantılarının güvenlik standartlaştırın. İlke OMB (Office Yönetim ve bütçe) özetlenen [memorandum M-08-05](https://georgewbush-whitehouse.archives.gov/omb/memoranda/fy2008/m08-05.pdf).

Kasım 2007'de, federal ağ çevre güvenliği ve olay yanıtlama işlevlerini geliştirmek için TIC program OMB kurdu. TIC belirli imzalar ve desen tabanlı bir veri belirleyip botnet etkinliği gibi davranış anormallikleri açığa çıkarmak üzere tüm gelen ve giden .gov trafiği ağ analiz gerçekleştirmek üzere tasarlanmıştır. Kurumlar kendi dış ağ bağlantılarını birleştirmek ve tüm trafiğin sınırlı sayıda ağ uç noktaları (güvenilen Internet adlandırılır, barındırılan (EINSTEIN bilinir) izinsiz giriş algılama ve önleme cihazlar üzerinden yönlendirilmesini için zorunlu Bağlantılar için).

Kısacası, bilmek, kuruluşları için TIC amacı şudur:
- (Yetkili veya yetkisiz) Ağımdaki kimdir?
- Ne zaman ağım erişilen ve neden?
- Hangi kaynaklara eriştiğini?

Bugün, tüm Ajans dış bağlantıları bir OMB onaylı TIC yönlendirilmesi gerekir. Federal kurumlar, bir TIC erişim sağlayıcısı (TICAP) olarak veya temel katmanı 1 Internet hizmet sağlayıcıları birini hizmetleriyle ihtiyaçlarımıza göre yönetilen güvenilir Internet Protokolü hizmet (MTIPS) sağlayıcıları olarak başvurulan TIC programı'na katılmak için gereklidir.  TIC bugün kurumu ve MTIPS sağlayıcısı tarafından gerçekleştirilen zorunlu kritik özellikleri içerir. Hızlandırılmış geçerli sürümünde TIC EINSTEIN sürüm 2 izinsiz giriş algılama ve EINSTEIN sürüm 3 (3A) yetkisiz erişim önleme cihazlar her TICAP ve MTIPS dağıtılan ve kurum, Memorandum anlama bölümünü ile oluşturur Anlarında güvenlik (EINSTEIN özellikleri federal sistemlerini dağıtmak için DHS).

Sorumluluğunu .gov ağı korumak için bir parçası olarak, ham veri akışı Aracısı Netflow veri federal kuruluş genelinde olayları ilişkilendirmenize ve özel araçlar kullanarak analizler DHS gerektirir. DHS yönlendiriciler girer veya bir arabirim çıkar IP ağ trafiğini toplama olanağı sağlar. Net akış verileri analiz ederek, bir ağ yöneticisi kaynak ve hedef trafiği, hizmet vs. sınıfı gibi şeyler belirleyebilirsiniz. NET akış veri "içerik data" olarak kabul edilir (örneğin, üst bilgisi, kaynak IP, hedef IP, vb.) ve; içeriği çevresinde yukarıdakileri bilmesine DHS sağlar diğer bir deyişle, kimin ne ve ne kadar süreyle yapmakta olduğu.

Girişim, güvenlik ilkeleri, Kılavuzlar ve şirket içi altyapı varsayar çerçeveleri de içerir. Devlet kurumları, maliyet tasarrufu, işlem verimliliğini ve yenilik yapmanın buluta taşıdığınızda, ağ trafiği yavaşlatmasını ve hız ve çeviklikle hangi devlet kurumları ile kullanıcılar şunları yapabilir sınırlama bazı durumlarda TIC uygulama gereksinimleri olan bulut tabanlı verilerine erişebilirler.

Bu makalede, devlet kurumlarının TIC ilkesiyle hem Iaas ve PaaS Hizmetleri genelinde ulaşmaya yardımcı olmak için Microsoft Azure kamu nasıl kullanabileceğiniz anlatılmaktadır.

## <a name="summary-of-azure-networking-options"></a>Azure ağ seçeneklerin özeti

Azure hizmetlerine bağlanırken üç ana seçeneğiniz vardır:

1. Doğrudan Internet bağlantısı: doğrudan Açık Internet bağlantısı üzerinden Azure hizmetlerine bağlanın. Bağlantının yanı sıra genel ortamdır. Uygulama ve aktarım düzeyinde şifreleme sonrasında gizlilik emin olmak için yararlandı. Bir sitenin Internet bağlantısı ile sınırlı bant genişliğine ve dayanıklılık sağlamak için birden çok etkin sağlayıcısı kullanılabilir
1. Sanal özel ağ: Azure sanal özel olarak bir VPN ağ geçidi kullanarak ağınıza bağlayın.
Bir sitenin standart Internet bağlantısı erişir ancak gizlilik emin olmak için bir tünel bağlantısı şifreli Orta geneldir. VPN cihazları ve seçilen yapılandırmasına bağlı olarak, bant genişliği sınırlıdır. Siteden siteye bağlantılar için 1,25 Gbps sınırlı durumdayken azure noktadan siteye bağlantılar için 100 MB/sn genellikle sınırlıdır.
1. Microsoft ExpressRoute: ExpressRoute Microsoft hizmetlerine doğrudan bir bağlantıdır. Bağlantı bir yalıtılmış fiber kanal olduğundan, genel veya özel kullanılan yapılandırmasına bağlı olarak bağlantı olabilir. Bant, genellikle en fazla 10 GB/sn sınırlıdır.

Güvenilir Internet bağlantısı ek H (bulut konuları) gereksinimlerini karşılamak için "departmanı, anlarında Security'nin içinde güvenilen Internet bağlantıları (TIC) başvuru mimarisi belgesi, sürüm 2. 0" bulundu birkaç yolu vardır. Bu belge boyunca DHS TIC kılavuzu için TIC 2.0 adlandırılır.

Departman veya Ajansın (D/A) ile bağlantı Azure veya Office 365 D/A TIC aracılığıyla trafiği yönlendirme olmadan etkinleştirmek için D/A şifrelenmiş bir tünel ve/veya Bulut hizmeti sağlayıcısı (CSP) için ayrılmış bir bağlantı kullanmanız gerekir. CSP Hizmetleri D/bulut varlıklarını bağlantısı sunulmaz, genel İnternet'e doğrudan Ajans Personel erişim sağlayabilirsiniz.

O365 uyumlu TIC 2.0 ek ya da Express Route ile kullanarak H ile [Microsoft Peering](https://docs.microsoft.com/azure/expressroute/expressroute-circuit-peerings#expressroute-routing-domains) etkin veya TLS 1.2 kullanarak tüm trafiği şifreleyen internet bağlantısı.  D/ağ D/A son kullanıcılara kendi Ajans ağ ve Internet üzerinden ONA altyapısı aracılığıyla bağlanabilirsiniz. O365 tüm uzak internet erişimi engellenir ve Aracısı yönlendirilir. D/A da genel eşdüzey hizmet sağlama etkinleştirilmiş, bir tür olan O365 için Microsoft eşlemesi ile bir Expressroute bağlantısı üzerinden bağlanabilirsiniz.  

Yalnızca Azure için Internet erişimi sınırlayan hizmetleriyle birlikte kullanıldığında seçenekleri (VPN) 2 ve 3 (ExpressRoute) bu gereksinimleri karşılayabilirsiniz.

![Microsoft güvenilir Internet bağlantısı (TIC) diyagramı](media/tic-diagram-a.png)

## <a name="how-azure-infrastructure-as-a-service-offerings-can-help-with-tic-compliance"></a>Hizmet sunumları olarak Azure altyapısı ile uyumluluk TIC nasıl yardımcı olabileceğini

Azure müşterilerin kendi sanal ağ yönlendirme yönetme olduğundan, Iaas hizmetini kullanarak TIC ilkesiyle uymak oldukça basittir.

Uyumluluk ONA başvuru mimarisine sahip olmalarını sağlamak için ana gereksinim, sanal ağınız (VNet) özel bir departman veya Ajansın'ın ağ uzantısı olur sağlamaktır. Olacak bir _özel_ uzantısı, ilke gerektiren hiçbir trafik TIC şirket içi ağ bağlantısıyla ağınızın dışında bırakın. Bu işlem bilinir "Zorlamalı tünel" olarak, hangi zaman TIC uyumluluk için kullanılan, bir şirket içi ağ geçidi Internet üzerinden ONA bir kuruluşun ağındaki gitmek için CSP ortamında herhangi bir sistemden tüm trafik yönlendirme, işlemidir.

Azure Iaas TIC uyumluluk iki büyük adım bölünebilir:

1. Yapılandırma
1. Denetim

### <a name="azure-iaas-tic-compliance-configuration"></a>Azure Iaas TIC uyumluluk yapılandırma

Azure ile uyumlu TIC mimarisi yapılandırmak için ilk uygulamanızı sanal ağınıza doğrudan internet erişimini engellemek ve ardından internet trafiğini şirket içi ağ üzerinden zorlayan gerekecektir.

#### <a name="prevent-direct-internet-access"></a>Doğrudan Internet erişimi engelle

Azure Iaas ağ, sanal ağlar alt ağlar, sanal makinelerin ağ arabirim denetleyicilerini (NIC'ler) ilişkilendirilmiş oluşan yönetilir.

Sanal makine ya da bir koleksiyon üzerindeki, herhangi bir dış kaynağa bağlanamıyor güvence altına almak için ONA uyumluluğunu desteklemek için basit bir senaryodur. Dış ağlar kesilmesinin NIC'ler veya alt ağları sanal ağınızda bir veya daha fazla trafiği denetlemek için kullanılabilen ağ güvenlik grupları (Nsg'ler) kullanarak olabilirsiniz. NSG’de trafik yönüne, protokole, kaynak adresle bağlantı noktasına ve hedef adresle bağlantı noktasına göre trafiğe izin veren ya da reddeden erişim denetim kuralları yer alır. Bir NSG kurallarını herhangi bir zamanda değiştirilebilir ve değişiklik ilişkili tüm örneklerine uygulanır.  Bir NSG oluşturma hakkında daha fazla bilgi için bu makaleye bakın [bir NSG oluşturmayı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

#### <a name="force-internet-traffic-through-on-premises-network"></a>Internet trafiğini şirket içi ağ üzerinden zorla

Azure, sistem yollarını otomatik olarak oluşturur ve bir sanal ağ içindeki her alt ağa bu yolları atar. Sistem yolları oluşturulamıyor ya da sistem yollarını kaldırma seçeneğiniz, ancak bazı sistem yollarını özel yollar ile geçersiz kılabilirsiniz. Azure, varsayılan sistem yollarını her alt ağ oluşturur ve belirli Azure özelliklerini kullandığınızda belirli alt ağlara ya da her alt ağ için ek isteğe bağlı varsayılan yollar ekler. Bu yönlendirme sağlar sanal ağ içinde gönderilen trafik, sanal ağ içinde kalır, IANA atanan özel adres alanlarından 10.0.0.0/8 atlanıyor gibi (sanal ağın adres alanında yer sürece) ve "son çare" yönlendirme Sanal ağın Internet uç noktasına 0.0.0.0/0.

![TIC zorla tünel](media/tic-diagram-c.png)

Tüm trafiği D/A TIC erişir emin olmak için tüm trafiğe sanal ağ şirket içi bağlantı üzerinden yönlendirilmesi gerekir.  Özel rotalar oluşturma ya da kullanıcı tanımlı yollar veya şirket içi ağ geçidiniz ile bir Azure sanal ağ geçidi arasında sınır ağ geçidi Protokolü (BGP) rotaları değişimi oluşturduğunuz. Kullanıcı tanımlı yollar hakkında daha fazla bilgi şu adreste bulunabilir: https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined. Sınır ağ geçidi protokolü hakkında daha fazla bilgi, ayrıca bulunabilir https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#border-gateway-protocol.

#### <a name="user-defined-routes"></a>Kullanıcı tanımlı yollar

Rota tabanlı bir sanal ağ geçidi kullanıyorsanız, zorlamalı tünel Azure içinde bir kullanıcı tanımlı yol (UDR) ayarı 0.0.0.0/0 trafiği bir "sonraki atlama değeri için" sanal ağ geçidinizin yönlendirilmesini ekleyerek gerçekleştirilir. Bu, sanal ağ ardından şirket içine yönlendirmek ağ geçidi için gönderilen tüm ağlar arası trafik sonuçlanır bu nedenle azure kullanıcı tanımlı yollar sistem tanımlı yolları üzerinde önceliklendirir. Tanımlandıktan sonra bu kullanıcı tanımlı yol var olan tüm alt ağlar ile ilişkili veya yeni Azure ortamınızdaki tüm sanal ağları içinde oluşturulmuş.

![Kullanıcı tanımlı yollar ve ONA](media/tic-diagram-d.png)

#### <a name="border-gateway-protocol"></a>Sınır Ağ Geçidi Protokolü

Border Gateway Protocol (BGP) etkin sanal ağ geçidi veya ExpressRoute kullanıyorsanız, BGP yolların tanıtılması için tercih edilen mekanizmadır. BGP ile tanıtılan yolu 0.0.0.0/0, ExpressRoute ve BGP kullanan sanal ağ geçitleri, bu varsayılan rotada sanal ağlarınızdaki tüm alt ağlara uygulanır sağlayacaktır.

### <a name="azure-iaas-tic-compliance-auditing"></a>Azure Iaas bilgisi uyumluluk denetimi

Azure, TIC uyumluluğunu denetlemek için birden çok yol sunar.

#### <a name="effective-routes"></a>Geçerli Yollar

Varsayılan rotanız yayılan onaylamak için belirli bir VM'nin, belirli bir NIC veya bir kullanıcı tanımlı yol tablosu "Geçerli rotalar" gözlemleyebilirsiniz. Bu Azure portalı üzerinden anlatıldığı gibi yapılabilir https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal, ya da anlatıldığı şekilde PowerShell aracılığıyla https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-powershell. Geçerli rotalar dikey penceresinde, ilgili kullanıcı tanımlı yollar BGP yolları ve ilgili varlık üzerinde uygulanabilmesi sistem yolları aşağıda görüldüğü gibi tanıtılan görmenize olanak sağlar.

![yollar ekran görüntüsü](media/tic-screen-1.png)

**Not**: çalışan bir VM ile ilişkili değilse, geçerli rotalar için bir NIC görüntüleyemezsiniz.

#### <a name="network-watcher"></a>Ağ İzleyicisi

Azure Ağ İzleyicisi TIC uyumluluğunu denetlemek için kullanılabilecek birden fazla araçları sunar.  Ağ İzleyicisi hakkında daha fazla bilgi https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview.

##### <a name="network-security-groups-flow-logs"></a>Ağ güvenlik grupları akış günlükleri 

Azure Ağ İzleyicisi IP trafiğinin çevreleyen meta verileri gösteren ağ akışı günlükleri yakalama olanağı sağlar. Diğer verilerine ek olarak, ağ akışı günlükleri hedefleri kaynak ve hedefleri adreslerini içerir. Bu, sanal ağ geçidi, şirket içi uç cihazlarına ve/veya TIC günlükleri ile bir araya gelen tüm trafiği şirket yönlendirilmiş gerçekten ettiğinden izleme için izin verir. 

## <a name="how-azure-platform-as-a-service-offerings-can-help-with-tic-compliance"></a>Hizmet sunumları olarak Azure platformu ile uyumluluk TIC nasıl yardımcı olabileceğini

Azure depolama gibi Azure PaaS Hizmetleri, İnternet'ten erişilebilen bir URL aracılığıyla erişilebilir. Bir depolama hesabı gibi kaynak onaylı kimlik bilgileri kimseyle bir TIC geçiş olmadan herhangi bir konumdan erişebilirsiniz. Bu nedenle, birçok kamu müşterileri, Azure PaaS hizmetlerine TIC ilkeleri ile uyumlu olmayan yanlış sonlandırma. Aslında, birçok Azure PaaS hizmetlerine bir sanal ağa (VNet) bağlı değilse, yukarıda açıklanan bir TIC uyumlu Iaas ortamı olarak aynı mimari kullanarak TIC İlkesi ile uyumsuz olabilir. Bir Azure sanal ağı ile Azure PaaS Hizmetleri Tümleştirme hizmeti özel olarak bu sanal ağdan erişilebilir ve kullanıcı tanımlı rotaları veya tüm İnternet'e bağlı trafiği yönlendirilmiş şirket içine olduğundan emin olmanın BGP aracılığıyla uygulanacak 0.0.0.0/0 yönlendirmeyi özel olanak sağlar TIC çapraz geçiş yapma.  Bazı Azure Hizmetleri aşağıdaki desenleri kullanarak sanal ağlara tümleştirilebilir:

- **Service'nın ayrılmış örneğini dağıtma**: PaaS Hizmetleri, sanal ağ ile ayrılmış örnek olarak dağıtılabilir artan sayıda uç noktaları bağlı. Örneğin, bir App Service ortamı (ASE) bir sanal ağa kısıtlı, ağ uç noktası izin verme "Ayrılmış" modunda dağıtılabilir. Bu ASE, ardından Web uygulamaları, API'leri ve işlevleri gibi birçok Azure PaaS hizmetlerine barındırabilirsiniz.
- **Sanal ağ hizmet uç noktaları**: PaaS hizmetlerinin giderek artan sayıda genel adresi yerine bir sanal ağ özel IP kullanıcılar uç byok'ye geçme seçeneğiniz izin.

Aşağıda listelenen ayrılmış örneğe ayrılmış bir sanal ağ veya hizmet uç noktaları Mayıs 2018'den itibaren dağıtımını destekleyen hizmetler: * (kullanılabilirlik temsil ticari Azure, Azure kamu kullanılabilirlik bekleyen).

### <a name="service-endpoints"></a>Hizmet Uç Noktaları

|Hizmet                   |Durum            |
|--------------------------|------------------|
|Azure anahtar kasası            | Özel önizleme  |
|Cosmos DB                 | Özel önizleme  |
|Kimlik                  | Özel önizleme  |
|Azure Data Lake           | Özel önizleme  |
|SQL Postgress/Mysql       | Özel önizleme  |
|Azure SQL Veri Ambarı  | Genel önizlemeye sunuldu   |
|Azure SQL                 | GA               |
|Depolama                   | GA               |

### <a name="vnet-injection"></a>Sanal ağ ekleme

|Hizmet                            |Durum            |
|-----------------------------------|------------------|
|SQL yönetilen örnek               | Genel önizlemeye sunuldu   |
|Azure Container Service(AKS)       | Genel önizlemeye sunuldu   |
|Service Fabric                     | GA               |
|API Management                     | GA               |
|Azure Active Directory             | GA               |
|Azure Batch                        | GA               |
|ASE                                | GA               |
|Redis                              | GA               |
|HDI                                | GA               |
|Sanal makine ölçek kümesi işlem  | GA               |
|Bulut hizmeti işlem              | GA               |

### <a name="vnet-integration-details"></a>VNet tümleştirmesi ayrıntıları

Aşağıdaki diyagramda genel VNet ekleme ve sanal ağ hizmet tüneli kullanarak PaaS hizmetlerine erişim için ağ akışı gösterilmektedir.  Ağ hizmeti ağ geçitleri, sanal ağın ve hizmet etiketleri hakkında daha fazla bilgi şurada bulunabilir https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags.

![TIC PaaS bağlantı seçenekleri](media/tic-diagram-e.png)

1. Azure ExpressRoute kullanarak özel bağlantı. Zorlamalı tünel ile ExpressRoute özel eşlemesi, tüm müşteri VNet trafiği ExpressRoute üzerinden geri şirket içine zorlamak için kullanılır. Microsoft Peering gerekli değildir.
2. ExpressRoute Microsoft eşlemesi ile birlikte kullanılan Azure VPN Gateway, müşteri sanal ağ ile şirket içi uç arasında uçtan uca IPSec şifrelemesi kaplamak için kullanılabilir. 
3. Müşteri VNet ağ bağlantısını kullanarak ağ güvenlik grupları (NSG) izin verme/reddetme müşterilerin IP, bağlantı noktası ve protokole göre denetlenir.
4. Müşteri sanal ağ, müşterinin hizmeti için hizmet uç noktası oluşturarak PaaS hizmetine genişletilir.
5. PaaS Hizmeti uç noktası güvenli varsayılan tümünü Reddet ve yalnızca belirli alt ağlar müşteri sanal ağ içinden gelen erişime izin.  Varsayılan reddetme de Internet'ten gelen bağlantıları içerir.
6. Müşteri sanal ağ içindeki kaynaklara erişmesi gereken herhangi bir Azure Hizmetleri ya da olmalıdır:  
  a. Doğrudan Vnet'e dağıtılabilir  
  b. İlgili Azure hizmetinden Kılavuzu seçili olarak izin verilir.

#### <a name="option-1-dedicated-instance-vnet-injection"></a>1. seçenek: Ayrılmış örnek "VNet ekleme"

VNet ekleme ile müşteriler, seçmeli olarak HDInsight gibi belirli bir Azure hizmetinin ayrılmış örnekleri kendi sanal ağ içinde dağıtabilirsiniz. Hizmet örnekleri, bir müşterinin sanal ağ içinde ayrılmış bir alt ağa dağıtılır. VNet ekleme hizmet kaynakları dışındaki Internet yönlendirilebilir adresleri erişilebilir olmasını sağlar.  Şirket içi örneklerini, bu hizmet örneklerine ExpressRoute veya siteden siteye VPN, güvenlik duvarları genel Internet adres alanına açmak yerine aracılığıyla doğrudan VNet adres alanı aracılığıyla erişebilir. Bir uç noktasına bağlı özel bir örneği ile Iaas TIC uyumluluk için kullanılan aynı stratejiler yararlanılabilir, varsayılan sağlayarak, Internet'e bağlı trafik yönlendirme için şirket içi bağlı bir sanal ağ geçidine yönlendirilir. Gelen ve giden erişimi daha fazla ağ güvenlik grupları (Nsg'ler) belirli bir alt ağ için denetlenebilir.

![VNet ekleme genel bakış diyagramı](media/tic-diagram-f.png)

#### <a name="option-2-vnet-service-endpoints"></a>2. seçenek: Sanal ağ hizmet uç noktaları 

Azure'nın çok kiracılı hizmetler artan sayıda "Hizmet uç noktası" özelliği, Azure sanal ağlarına tümleştirmek için alternatif bir yöntem sunar. Sanal ağ hizmet uç noktaları, sanal ağ IP adres alanınızı ve hizmet, sanal ağınızın kimliğini doğrudan bağlantı üzerinden genişletin.  VNet trafiği Azure hizmeti için her zaman Azure omurga ağında kalır. Bir hizmet için hizmet uç noktası etkinleştirildikten sonra bu sanal ağa hizmet tarafından sunulan ilkeleri aracılığıyla hizmete kısıtlanabilir. Erişim denetimleri platform Azure hizmeti tarafından zorunlu tutulmaz ve kilitli bir kaynağa erişimi yalnızca istek izin verilen VNet/alt ağından çıkıp internet'i ve/veya iki IP'ler ExpressRoute kullanıyorsanız, şirket içi trafiği tanımlamak için kullanılan, verilir. Bu, etkili bir şekilde doğrudan bir PaaS hizmeti bırakmasını gelen/giden trafiği engellemek için kullanılabilir.

![Hizmet uç noktalarına genel bakış diyagramı](media/tic-diagram-g.png)

## <a name="using-cloud-native-tools-for-network-situational-awareness"></a>Yerel bulut araçları için olan bitenden ağ kullanma

Azure bulut ağınızın trafik akışları anlamak için gerekli olan bitenden olmasını sağlamaya yardımcı olmak için yerel araçlar sağlar. TIC ilkeye uymak için gerekli değildir, ancak güvenlik özelliklerinizin büyük ölçüde artırabilir.

### <a name="azure-policy"></a>Azure İlkesi

Azure İlkesi (https://azure.microsoft.com/services/azure-policy/) denetim ve uyumluluk girişimlerini zorlamak için daha iyi özelliği sayesinde kuruluşunuz tarafından sağlanan bir Azure hizmetidir.  Şu anda genel önizlemede Azure ticari bulutlarında ancak henüz Microsoft Azure kamu için kullanılabilir, TIC açısından temkinli davranan müşteriler planlama ve gelecekte uyumluluk güvencesi kendi ilke kuralları sınama başlayabilirsiniz. Azure İlkesi, abonelik düzeyinde yöneliktir ve girişimler, ilke tanımları, Denetim ve zorlama sonuçlar ve özel durum yönetimi yönetmek için merkezi bir arabirim sağlar. Birçok yerleşik Azure ilke tanımları ek olarak, Yöneticiler, kendi özel tanımları basit json şablon aracılığıyla tanımlayabilirsiniz. Birçok müşteri için Microsoft, mümkün olduğunda zorlama üzerinde denetim önceliği önerir.

Aşağıdaki örnek ilkeleri TIC uyumluluk senaryoları için yararlı olabilir:

|İlke  |Örnek senaryo  |Başlangıç şablonu  |
|---------|---------|---------|
|Kullanıcı tanımlı yol tablosu zorla |     Şirket içi yönlendirme için onaylanmış bir sanal ağ geçidine yönelik tüm sanal ağları noktalarında varsayılan yol olduğundan emin olun | https://docs.microsoft.com/azure/azure-policy/scripts/no-user-def-route-table |
|Ağ İzleyicisi bölge için etkin olup olmadığını denetle  | Tüm bölgeler kullanılan için Ağ İzleyicisi etkin olduğundan emin olun  | https://docs.microsoft.com/azure/azure-policy/scripts/net-watch-not-enabled |
|Her alt ağ üzerinde NSG x  | NSG (veya onaylanan Nsg'ler kümesi), Internet trafiğini bloke ile her sanal ağ içindeki tüm alt ağlara uygulandığından emin olun | https://docs.microsoft.com/azure/azure-policy/scripts/nsg-on-subnet |
|Her NIC üzerinde NSG x | Internet trafiğini bloke sahip bir NSG tüm sanal makineler, tüm ağ arabirimlerine uygulandığından emin olun. | https://docs.microsoft.com/azure/azure-policy/scripts/nsg-on-nic |
|Kullanım VNet için sanal makine ağ arabirimleri Onaylandı  | Tüm NIC'ler onaylı bir VNet üzerinde olduğundan emin olun | https://docs.microsoft.com/azure/azure-policy/scripts/use-approved-vnet-vm-nics |
|İzin verilen konumlar | Tüm kaynaklar bölgelere uyumlu sanal ağlar ve Ağ İzleyicisi yapılandırmayla dağıtıldığından emin olun  | https://docs.microsoft.com/azure/azure-policy/scripts/allowed-locs |
|İzin verilmeyen kaynak türleri gibi genel IP'ler  | Kaynak türleri, uyumluluk planınız yok dağıtımını engelliyor. Örnek olarak, bu ilke, genel IP adresi kaynakları dağıtımını engellemek üzere kullanılabilir. NSG kuralları etkili bir şekilde gelen Internet trafiği engellemek için kullanılabilse de, ek kullanımını genel IP'lerin engelleme saldırı yüzeyi küçültülür.    | https://docs.microsoft.com/azure/azure-policy/scripts/not-allowed-res-type  |

### <a name="azure-traffic-analytics"></a>Azure trafik analizi

Azure Ağ İzleyicisi'nin trafik analizi, akış günlüğü verileri ve diğer günlükler, ağ trafiğini üst düzey genel bakış sağlamak için kullanır. Bu veriler TIC uyumluluk denetim ve sorunlu noktaları tanımlamak için yararlı olabilir. Üst düzey bir pano, hızlı bir şekilde Vm'leri odaklanmış listesini TIC yönlendirme için sağlandığından internet ile iletişim kuran ekran için kullanılabilir.

![Trafik analizi ekran görüntüsü](media/tic-traffic-analytics-1.png)

"Coğrafi harita" Internet trafiğini olası fiziksel hedefleri tanımlamanıza ve şüpheli konumlardan veya desen değişiklikleri önceliklendirme izin vererek Vm'leriniz için hızlı bir şekilde tanımlamak için kullanılabilir.

![Trafik analizi ekran görüntüsü](media/tic-traffic-analytics-2.png)

Bir ağ topolojisi haritası, var olan sanal ağlar hızlı bir şekilde anket kullanılabilir:

![Trafik analizi ekran görüntüsü](media/tic-traffic-analytics-3.png)

### <a name="azure-network-watcher-next-hop"></a>Azure Ağ İzleyicisi sonraki atlama

Ağ İzleyicisi tarafından izlenen bölgelerdeki ağlar kaynak ve hedef Ağ İzleyicisi "Sonraki atlama" hedef çözülecektir bir örnek ağ akışı için yazacağınız kolay Portal tabanlı erişime izin verme "Sonraki atlama" testleri oluşturabilecekler. Bu VM'ler ve örnek Internet adresleri karşı "sonraki atlama", ağ sanal ağ geçidi gerçekten de olduğundan emin olmak için kullanılabilir.

![Ağ İzleyicisi sonraki atlama](media/tic-network-watcher.png)

## <a name="conclusions"></a>Sonuçları

Microsoft Azure, Office 365 ve Dynamics 365 erişim TIC 2.0 ek H kılavuzu olarak yazılmış ve Mayıs, 2018'de tanımlanan ile uyum sağlanmasına yardımcı için kolayca yapılandırılabilir.  Microsoft, bu kılavuzun değiştirilebilir ve yeni bir kılavuz yayınlandığında Kılavuzu zamanında karşılamak müşterilere yardımcı olmak endeavor farkındadır.

## <a name="appendix-tic-patterns-for-common-workloads"></a>Ek: Ortak iş yükleri TIC desenleri

| Kategori | İş yükü | IaaS | PaaS ayrılmış / sanal ağ ekleme  | Hizmet Uç Noktaları  |
|---------|---------|---------|---------|--------|
| İşlem | Linux Sanal Makineleri | Evet | | |
| İşlem | Windows Sanal Makineleri | Evet | | |
| İşlem | Sanal Makine Ölçek Kümeleri | Evet | | |
| İşlem | Azure İşlevleri | | App Service ortamı (ASE) | |
| Web ve Mobil | Şirket içi Web uygulaması | | App Service ortamı (ASE) | |
| Web ve Mobil | İç Mobil uygulama | | App Service ortamı (ASE) | |
| Web ve Mobil | API Apps | | App Service ortamı (ASE) | |
| Kapsayıcılar | Azure Container Service (ACS) | | | Evet |
| Kapsayıcılar | Azure Container Service'i (AKS) * | | | Evet |
| Database | SQL Veritabanı | | Azure SQL veritabanı yönetilen örneği * | Azure SQL |
| Database | MySQL için Azure Veritabanı | | | Evet |
| Database | PostgreSQL için Azure Veritabanı | | | Evet |
| Database | SQL Veri Ambarı | | | Evet |
| Database | Azure Cosmos DB | | | Evet |
| Database | Redis Cache | | Evet | |
| Depolama | Bloblar | Evet | | |
| Depolama | Dosyalar | Evet | | |
| Depolama | Kuyruklar | Evet | | |
| Depolama | Tablolar | Evet | | |
| Depolama | Diskler | Evet | | |

*: Mayıs 2018'den itibaren Azure kamu genel önizlemede  
**: Mayıs 2018'den itibaren Azure kamu'da özel Önizleme

