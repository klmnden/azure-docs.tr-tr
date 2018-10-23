---
title: Azure sanal ağ hizmet uç noktaları | Microsoft Docs
description: Hizmet uç noktalarını kullanarak bir sanal ağdan Azure kaynaklarına doğrudan erişimi etkinleştirmeyi öğrenin.
services: virtual-network
documentationcenter: na
author: sumeetmittal
manager: narayan
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: sumeet.mittal
ms.custom: ''
ms.openlocfilehash: 77fad7b0035a9ba21d71e6c493a4f1a5bd9a2111
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49395216"
---
# <a name="virtual-network-service-endpoints"></a>Sanal Ağ Hizmeti Uç Noktaları

Sanal Ağ hizmet uç noktaları, sanal ağ özel adres alanınızı ve sanal ağınızın kimliğini doğrudan bağlantı üzerinden Azure hizmetlerine genişletir. Uç noktalar kritik Azure hizmeti kaynaklarınızı sanal ağlarınızla sınırlayarak güvenliğini sağlamanıza imkan verir. Sanal ağınızdan Azure hizmetine giden trafik her zaman Microsoft Azure omurga ağında kalır.

Bu özellik aşağıdaki Azure hizmetleri ve bölgeleri için sağlanır:

**Genel kullanıma sunuldu**

- **[Azure Depolama](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network)**: Tüm Azure bölgelerinde genel kullanıma sunuldu.
- **[Azure SQL Veritabanı](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Tüm Azure bölgelerinde genel kullanıma sunuldu.
- **[PostgreSQL için Azure Veritabanı sunucusu](../postgresql/howto-manage-vnet-using-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Veritabanı hizmetinin sağlandığı Azure bölgelerinde genel olarak kullanılabilir.
- **[MySQL için Azure Veritabanı sunucusu](../mysql/howto-manage-vnet-using-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Veritabanı hizmetinin sağlandığı Azure bölgelerinde genel olarak kullanılabilir.
- **[Azure Cosmos DB](../cosmos-db/vnet-service-endpoint.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Tüm Azure genel bulut bölgelerinde Genel Kullanıma sunuldu.
- **[Azure Key Vault](https://blogs.technet.microsoft.com/kv/2018/06/25/announcing-virtual-network-service-endpoints-for-key-vault-preview/)**: Tüm Azure genel bulut bölgelerinde genel kullanıma sunuldu.

**Önizleme**

- **[Azure SQL Veri Ambarı](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Tüm Azure genel bulut bölgelerini önizleme aşamasında kullanıma sunuldu.
- **[Azure Service Bus](../service-bus-messaging/service-bus-service-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Önizleme sürümünde kullanılabilir.
- **[Azure Event Hubs](../event-hubs/event-hubs-service-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Önizleme sürümünde kullanılabilir.
- **[Azure Data Lake Store 1. Nesil](../data-lake-store/data-lake-store-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Önizleme sürümünde kullanılabilir.

En güncel bildirimler için [Azure Sanal Ağ güncelleştirmeleri](https://azure.microsoft.com/updates/?product=virtual-network) sayfasını inceleyin.

## <a name="key-benefits"></a>Önemli avantajlar

Hizmet uç noktaları aşağıdaki avantajları sağlar:

- **Azure hizmet kaynaklarınız için geliştirilmiş güvenlik**: Sanal ağ özel adres alanı çakışabilir ve bu nedenle sanal ağınızdan gelen trafiğin benzersiz olarak tanımlanması için kullanılamaz. Hizmet uç noktaları, sanal ağ kimliğini hizmete taşıyarak Azure hizmeti kaynaklarının sanal ağınıza sunulmasını sağlar. Hizmet uç noktaları sanal ağınızda etkinleştirdikten sonra, kaynaklara bir sanal ağ kuralı ekleyerek sanal ağınıza bağlı olan Azure hizmet kaynaklarının güvenliğini sağlayabilirsiniz. Bu sayede kaynaklara genel İnternet erişimini tamamen kaldırıp yalnızca sanal ağınızdan gelen trafiğe izin vererek güvenliği artırabilirsiniz.
- **Sanal Ağınızdan gelen Azure trafiği için en uygun yönlendirme**: Günümüzde sanal ağınızda İnternet trafiğini şirket içi ve/veya sanal gereçlerden geçmeye zorlayan tüm rotalar (zorlamalı tünel oluşturma olarak bilinir) Azure hizmet trafiğini de İnternet trafiğiyle aynı rotadan geçmeye zorlar. Hizmet uç noktaları Azure trafiği için en uygun rotayı sunar. 

  Uç noktalar her zaman hizmet trafiğini sanal ağınızdan doğrudan Microsoft Azure omurga ağındaki hizmete yönlendirir. Trafiğin Azure omurga ağında tutulması, zorlamalı tünel aracılığıyla hizmet trafiğini etkilemeden, giden İnternet trafiğini sanal ağlarınızdan denetlemeye ve izlemeye devam etmenize olanak sağlar. [Kullanıcı tanımlı rotalar ve zorlamalı tünel oluşturma](virtual-networks-udr-overview.md) hakkında daha fazla bilgi edinin.
- **Kolay kurulum sayesinde daha az yönetim yükü**: Azure kaynaklarını IP güvenlik duvarı aracılığıyla güvenli hale getirmek için artık sanal ağınızda ayrılmış, ortak IP adresleri olması gerekmez. Hizmet uç noktalarını ayarlamak için herhangi bir NAT veya ağ geçidi cihazı gerekmez. Hizmet uç noktaları alt ağda tek tıklamayla yapılandırılabilir. Uç noktaların bakımını yapma yükü ortadan kalkar.

## <a name="limitations"></a>Sınırlamalar

- Bu özellik yalnızca Azure Resource Manager dağıtım modeli üzerinden dağıtılmış olan sanal ağlarda kullanılabilir.
- Uç noktalar Azure sanal ağlarında yapılandırılmış olan alt ağlarda etkindir. Uç noktalar şirket içi ortamdan Azure hizmetlerine giden trafik için kullanılamaz. Daha fazla bilgi için bkz. [Azure hizmetine erişimi şirket içi ortamla sınırlama](#securing-azure-services-to-virtual-networks).
- Azure SQL için, hizmet uç noktası yalnızca sanal ağ ile aynı bölgedeki Azure hizmeti trafiği için geçerlidir. Azure Depolama için, RA-GRS ve GRS trafiğinin desteklenmesi için özelliğin kapsamı sanal ağın dağıtılmış olduğu eşleştirilmiş bölgeye uzatılır. [Azure eşleştirilmiş bölgeleri](../best-practices-availability-paired-regions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-paired-regions) hakkında daha fazla bilgi edinin.

## <a name="securing-azure-services-to-virtual-networks"></a>Azure hizmetlerini sanal ağlar ile sınırlama

- Sanal ağ hizmet uç noktası, Azure hizmetine sanal ağınızın kimliğini sağlar. Hizmet uç noktaları sanal ağınızda etkinleştirdikten sonra, kaynaklara bir sanal ağ kuralı ekleyerek sanal ağınıza bağlı olan Azure hizmet kaynaklarının güvenliğini sağlayabilirsiniz.
- Günümüzde, bir sanal ağdan gelen Azure hizmet trafiği, kaynak IP adresleri olarak ortak IP adreslerini kullanır. Hizmet uç noktaları ile, hizmet trafiği sanal ağınızdan Azure hizmetinize erişim sırasında kaynak IP adresleri olarak sanal ağ özel adreslerini kullanır. Bu anahtar IP güvenlik duvarlarında kullanılan ayrılmış ve genel IP adreslerini kullanmadan hizmetlere erişmenizi sağlar.

>[!NOTE]
> Hizmet uç noktaları ile alt ağdaki sanal makinelerin kaynak IP adresleri hizmet trafiği için genel IPv4 adresleri yerine özel IPv4 adreslerini kullanmaya başlar. Bu değişikliğin ardından Azure genel IP adreslerini kullanan mevcut Azure hizmeti güvenlik duvarı kuralları çalışmamaya başlar. Hizmet uç noktalarını ayarlamadan önce Azure hizmeti güvenlik duvarı kurallarının bu değişikliğe uygun olduğundan emin olun. Hizmet uç noktalarını yapılandırırken bu alt ağdan gelen hizmet trafiğinde geçici kesintiler de yaşayabilirsiniz. 
 
- __Azure hizmetine erişimi şirket içi ortamla sınırlama__:

  Varsayılan olarak sanal ağlara ayrılmış olan Azure hizmeti kaynaklarına şirket içi ağlardan erişmek mümkün değildir. Şirket içinden gelen trafiğe izin vermek istiyorsanız şirket içi ortamdan veya ExpressRoute üzerinden genel (genelde NAT) IP adreslerine de izin vermeniz gerekir. Azure hizmet kaynakları için bu IP adresleri IP güvenlik duvarı yapılandırma adımından eklenebilir.

  ExpressRoute: Ortak eşleme veya Microsoft eşlemesi için şirket içinden [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) kullanıyorsanız, kullanılan NAT IP adreslerini tanımlamanız gerekir. Ortak eşleme için, her bir ExpressRoute varsayılan olarak bağlantı hattında trafik Microsoft Azure omurga ağına girdiğinde Azure hizmet trafiğine uygulanan iki NAT IP adresi kullanılır. Microsoft eşlemesi için, kullanılan NAT IP adresleri müşteri tarafından sağlanır veya hizmet sağlayıcısı tarafından sağlanır. Hizmet kaynaklarınıza erişime izin vermek için, bu genel IP adreslerine kaynak IP güvenlik duvarı ayarında izin vermeniz gerekir. Ortak eşleme ExpressRoute bağlantı hattı IP adreslerinizi bulmak için Azure portalında [ExpressRoute ile bir destek bileti açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). [ExpressRoute genel ve Microsoft eşlemesi için NAT](../expressroute/expressroute-nat.md?toc=%2fazure%2fvirtual-network%2ftoc.json#nat-requirements-for-azure-public-peering) hakkında daha fazla bilgi edinin.

![Azure hizmetlerini sanal ağlar ile sınırlama](./media/virtual-network-service-endpoints-overview/VNet_Service_Endpoints_Overview.png)

### <a name="configuration"></a>Yapılandırma

- Hizmet uç noktaları bir sanal ağ içindeki alt ağ üzerinde yapılandırılır. Uç noktalar ilgili alt ağ içinde çalışan tüm işlem örneği türleriyle birlikte çalışabilir.
- Bir alt ağ üzerindeki desteklenen tüm Azure hizmetleri (örneğin Azure Depolama veya Azure SQL Veritabanı) için birden fazla hizmet uç noktası yapılandırabilirsiniz.
- Azure SQL Veritabanı için, sanal ağların Azure hizmet kaynağıyla aynı bölgede bulunması gerekir. GRS ve RA-GRS Azure Depolama hesapları kullanılıyorsa, birincil hesap sanal ağ ile aynı bölgede olmalıdır. Diğer tüm hizmetler için, Azure hizmet kaynakları herhangi bir bölgedeki sanal ağlar ile güvenli hale getirilebilir. 
- Uç noktanın yapılandırıldığı sanal ağ, Azure hizmet kaynağıyla aynı veya ondan farklı abonelikte olabilir. Uç noktaları ayarlamak ve Azure hizmetlerinin güvenliğini sağlamak için gerekli olan izinler hakkında daha fazla bilgi için [Sağlama](#Provisioning) bölümüne bakın.
- Desteklenen hizmetler için yeni veya mevcut kaynaklar ile sanal ağlar arasındaki güvenliği hizmet uç noktaları kullanarak sağlayabilirsiniz.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

- Bir hizmet uç noktası etkinleştirildikten sonra alt ağdan hizmet ile iletişim kurarken, ilgili alt ağdaki sanal makinelerin kaynak IP adresleri genel IPv4 adreslerinden özel IPv4 adreslerine döner. Hizmete giden mevcut açık TCP bağlantıları bu geçiş sırasında kapatılır. Bir alt ağ için hizmete yönelik hizmet uç noktasını etkinleştirmeden veya devre dışı bırakmadan önce çalışan kritik görev olmadığından emin olun. Ayrıca uygulamalarınızın IP adresi değişikliğinin ardından Azure hizmetlerine otomatik olarak bağlanabildiğinden emin olun.

  IP adresi değişiklikleri yalnızca sanal ağınızdan giden hizmet trafiğini etkiler. Sanal makinelerinize atanmış genel IPv4 adreslerinin gelen/giden trafiği üzerinde herhangi bir etki görülmez. Azure hizmetleri açısından, Azure genel IP adreslerini kullanan mevcut güvenlik duvarı kurallarınız varsa bu kurallar sanal ağ özel adresine geçiş yapıldığında çalışmaz.
- Hizmet uç noktaları kullanıldığında, Azure hizmetlerine yönelik DNS girişleri güncel durumdaki halde kalır ve Azure hizmetine atanmış olan ortak IP adreslerini çözümlemeye devam eder.

- Hizmet uç noktasına sahip ağ güvenlik grupları (NSG):
  - Varsayılan olarak NSG'ler giden İnternet trafiğine ve dolayısıyla sanal ağınızdan Azure hizmetlerine giden trafiğe izin verir. Bu durum hizmet uç noktalarında da aynı şekilde çalışmaya devam eder. 
  - Giden İnternet trafiğinin tümünü reddetmek ve yalnızca belirli Azure hizmetlerine giden trafiğe izin vermek istiyorsanız NSG'lerinizde [hizmet etiketlerini](security-overview.md#service-tags) kullanabilirsiniz. Desteklenen Azure hizmetlerini NSG kurallarında hedef olarak belirtebilir ve her etikete ait IP adreslerinin bakımının Azure tarafından yapılmasını sağlayabilirsiniz. Daha fazla bilgi için bkz. [NSG'ler için Azure Hizmet etiketleri.](security-overview.md#service-tags) 

### <a name="scenarios"></a>Senaryolar

- **Eşlenmiş, bağlı veya birden çok sanal ağ**: Bir sanal ağ içindeki veya birden fazla sanal ağ üzerinde bulunan birden fazla alt ağdaki Azure hizmetlerinin güvenliğini sağlamak için, her bir alt ağdaki hizmet uç noktasını ayrı ayrı etkinleştirebilir ve bu alt ağlara giden Azure hizmet kaynaklarının güvenliğini sağlayabilirsiniz.
- **Sanal ağdan bir Azure hizmetine giden trafiği filtreleme**: Sanal ağdan bir Azure hizmetine giden trafiği incelemek veya filtrelemek isterseniz, ilgili sanal ağa bir ağ sanal gereci dağıtabilirsiniz. Ardından hizmet uç noktalarını ağ sanal gerecinin dağıtılmış olduğu alt ağa uygulayabilir ve Azure hizmet kaynağını yalnızca bu alt ağ ile sınırlayabilirsiniz. Bu senaryo, Azure hizmet erişimini ağ sanal gereci filtresi kullanarak sanal ağınızdan yalnızca belirli Azure kaynaklarına gidecek şekilde sınırlamak istediğinizde yararlı olabilir. Daha fazla bilgi için bkz. [Ağ sanal gereçleri ile çıkış](/azure/architecture/reference-architectures/dmz/nva-ha#egress-with-layer-7-nvas.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- **Azure kaynaklarını doğrudan sanal ağlara dağıtılan hizmetler ile sınırlandırma**: Çeşitli Azure hizmetleri doğrudan bir sanal ağdaki belirli alt ağlara dağıtılabilir. Yönetilen hizmet alt ağında bir hizmet uç noktası kurarak Azure hizmet kaynaklarını [yönetilen hizmet](virtual-network-for-azure-services.md) alt ağlarına ayırabilirsiniz.
- **Azure sanal makinesinden gelen disk trafiği**: Yönetilen veya yönetilmeyen disklere yönelik Sanal Makine Diski trafiği (takılan ve çıkarılan diskIO dahil), Azure Depolama için yapılan hizmet uç noktası yönlendirme değişikliklerinden etkilenmez. Ağ seçmek için hizmet uç noktaları ve [Azure Depolama ağ kuralları](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json) aracılığıyla REST erişimini sayfa bloblarına sınırlayabilirsiniz. 

### <a name="logging-and-troubleshooting"></a>Günlüğe kaydetme ve sorun giderme

Hizmet uç noktaları belirli bir hizmet için yapılandırıldıktan sonra geçerli olan hizmet uç noktasını aşağıdaki şekilde doğrulayabilirsiniz: 
 
- Hizmet tanılamada herhangi bir hizmet isteğinin kaynak IP adresini doğrulama. Hizmet uç noktalarına sahip tüm yeni isteklerin kaynak IP adresi değerinde isteği sanal ağınızdan gönderen istemciye atanmış olan sanal ağ özel IP adresi gösterilir. Uç noktası olmadığında bu adres Azure genel IP adreslerinden biri olur.
- Bir alt ağdaki herhangi bir ağ arabiriminin etkin yollarını görüntüleme. Hizmet yolu:
  - Her hizmetin adres ön eki aralıklarına daha belirli bir varsayılan yolu gösterir
  - nextHopType değeri *VirtualNetworkServiceEndpoint* olarak belirlenmiştir
  - Zorlamalı tünel rotalarıyla kıyaslandığında geçerli hizmete daha doğrudan bir bağlantı belirtir

>[!NOTE]
> Hizmet uç noktası rotaları, Azure hizmeti olarak adres ön eki eşleşmesi için BGP veya UDR rotasını geçersiz kılar. [Geçerli rotalarla sorun giderme](diagnose-network-routing-problem.md) hakkında daha fazla bilgi edinin

## <a name="provisioning"></a>Sağlama

Hizmet uç noktaları, bir sanal ağda yazma erişimine sahip bir kullanıcı tarafından sanal ağlarda birbirinden bağımsız olarak yapılandırılabilir. Azure hizmet kaynaklarını bir sanal ağ ile sınırlamak için kullanıcının eklenen alt ağlarda *Microsoft.Network/JoinServicetoaSubnet* iznine sahip olması gerekir. Bu izin varsayılan olarak yerleşik hizmet yöneticisi rollerinde mevcuttur ve özel roller oluşturularak değiştirilebilir.

[Yerleşik roller](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [özel rollere](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) belirli izinlerin atanması hakkında daha fazla bilgi edinin.

Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. Sanal ağ ve Azure hizmet kaynaklarının farklı aboneliklerde olması halinde kaynakların aynı Active Directory (AD) kiracısı altında bulunması gerekir. 

## <a name="pricing-and-limits"></a>Fiyatlandırma ve limitler

Hizmet uç noktalarının kullanımından ek ücret alınmaz. Azure hizmetleri (Azure Depolama, Azure SQL Veritabanı vb.) için güncel fiyatlandırma modeli uygulanır.

Bir sanal ağ içinde sınırsız sayıda hizmet uç noktası oluşturulabilir.

Bir Azure hizmet kaynağında (Azure Depolama hesabı gibi), hizmetler kaynağın güvenliğini sağlamak amacıyla kullanılan alt ağ sayısını sınırlandırabilir. Ayrıntılar için [Sonraki adımlar](#next-steps) bölümündeki hizmet belgelerini inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal ağ hizmet uç noktalarını nasıl yapılandıracağınızı](tutorial-restrict-network-access-to-resources.md) öğrenin
- [Bir Azure Depolama hesabını bir sanal ağ ile nasıl sınırlandıracağınızı](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json) öğrenin
- [Bir Azure SQL Veritabanını bir sanal ağ ile nasıl sınırlandıracağınızı](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) öğrenin
- [Sanal ağlar için Azure hizmet tümleştirmesi](virtual-network-for-azure-services.md) hakkında bilgi edinin
-  Hızlı başlangıç: Bir sanal ağın alt ağında hizmet uç noktası ve bu alt ağda güvenli Azure Depolama hesabı oluşturmak için [Azure resource manager şablonu](https://azure.microsoft.com/resources/templates/201-vnet-2subnets-service-endpoints-storage-integration).

