---
title: Azure sanal ağ hizmet uç noktası'nı kullanarak bir Azure Cosmos DB hesabı güvenli erişim
description: Bu belgede, sanal ağ ve alt ağ erişim denetimi için bir Azure Cosmos hesap hakkında açıklanmaktadır.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 672c62c440708f8e949d67d545bee2179c6066b2
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894944"
---
# <a name="access-azure-cosmos-db-from-virtual-networks-vnet"></a>Azure Cosmos DB'den sanal ağları (VNet) erişim

Yalnızca sanal ağın (VNet) belirli bir alt ağından erişime izin vermek için Azure Cosmos hesabı yapılandırabilirsiniz. Etkinleştirerek [hizmet uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) bir sanal ağ içindeki alt ağdaki Azure Cosmos DB'ye erişmek için bu alt ağ trafiği için Azure Cosmos DB ile sanal ağ ve alt ağ kimliğini gönderilir. Azure Cosmos DB hizmet uç noktası etkinleştirildikten sonra Azure Cosmos hesabınıza ekleyerek alt ağa erişimi sınırlayabilirsiniz.

Varsayılan olarak, isteğin bir geçerli bir yetkilendirme belirteciyle birlikte sunulduğu, bir Azure Cosmos hesabı herhangi bir kaynaktan erişilebilir. Sanal ağ içindeki bir veya daha fazla alt ağlar eklediğinizde, yalnızca bu alt ağlardan kaynaklanan isteklerin geçerli bir yanıt alırsınız. Başka bir kaynaktan gelen istekler, 403 (Yasak) yanıt alırsınız. 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Sanal ağlardan erişim yapılandırma hakkında sık sorulan bazı sorular şunlardır:

### <a name="can-i-specify-both-virtual-network-service-endpoint-and-ip-access-control-policy-on-an-azure-cosmos-account"></a>Bir Azure Cosmos hesapta hem sanal ağ hizmet uç noktası hem de IP erişim denetimi ilkesini belirtebilirsiniz? 

Sanal ağ hizmet uç noktası hem de bir IP erişim denetimi İlkesi'ni (güvenlik duvarı olarak da bilinir), Azure Cosmos hesabınızda etkinleştirebilirsiniz. Bu iki özellik tamamlayıcıdır ve topluca yalıtım ve Azure Cosmos hesabınızın güvenliğini sağlamak. IP kullanarak, güvenlik duvarı statik IP hesabınızı erişmesini sağlar. 

### <a name="how-do-i-limit-access-to-subnet-within-a-virtual-network"></a>Bir sanal ağ içindeki alt ağ erişimini sınırlamak nasıl? 

Bir alt ağdan Azure Cosmos hesabına erişimi sınırlamak için gereken iki adımı vardır. İlk olarak, kendi alt ağı ve sanal ağ kimliğini Azure Cosmos DB'ye taşımak için alt ağından gelen trafiği sağlar. Bu, Azure Cosmos DB için alt ağda hizmet uç noktası sağlayarak gerçekleştirilir. Sonraki hesabı erişilebilir bir kaynak olarak bu alt ağ belirtme Azure Cosmos hesabındaki bir kuralı ekleniyor.

### <a name="will-virtual-network-acls-and-ip-firewall-reject-requests-or-connections"></a>Sanal ağ ACL'leri ve IP Güvenlik Duvarı, isteklerin veya bağlantıları reddeder? 

IP Güvenlik Duvarı'nı veya sanal ağ erişim kuralları eklenir, yalnızca izin verilen kaynakları get Geçerli yanıtlar gelen istekleri. Bir 403 (Yasak) ile diğer istekler reddedilir. Azure Cosmos hesabın güvenlik duvarı bir bağlantı düzeyi güvenlik duvarı ayırt etmek önemlidir. Kaynak hizmete bağlanmaya devam edebilirler ve bağlantıları reddedilen değildir.

### <a name="my-requests-started-getting-blocked-when-i-enabled-service-endpoint-to-azure-cosmos-db-on-the-subnet-what-happened"></a>İsteklerim miyim alt ağdaki hizmet uç noktası için Azure Cosmos DB etkinleştirildiğinde engellenen başlatıldı. Ne oldu?

Bir alt ağ üzerindeki Azure Cosmos DB için hizmet uç noktası etkinleştirildikten sonra hesap ulaşmasını trafik kaynağını genel IP ile sanal ağ ve alt ağ için geçer. Azure Cosmos hesabınızda IP tabanlı güvenlik duvarı varsa yalnızca, etkin hizmet alt ağından gelen trafiği artık IP güvenlik duvarı kurallarını eşleşir ve bu nedenle reddedilir. IP tabanlı güvenlik duvarı sanal ağ tabanlı erişim denetimi için sorunsuz bir biçimde geçiş adımlarını inceleyeceğiz.

### <a name="do-the-peered-virtual-networks-also-have-access-to-azure-cosmos-account"></a>Eşlenen sanal ağlarda, Azure Cosmos hesabına erişimi de gerekiyor? 
Yalnızca sanal ağ ve Azure Cosmos hesaba eklenen ağlarından erişime sahiptir. Eşlenen sanal ağ içindeki alt ağların hesabınıza eklenene kadar eşlenmiş Vnet'ler hesabına erişemiyor.

### <a name="what-is-the-maximum-number-of-subnets-allowed-to-access-a-single-cosmos-account"></a>Hangi alt ağların sayısı tek bir Cosmos hesap erişmesine izin verilir? 
Şu anda, Azure Cosmos hesabınız için izin verilen en fazla 64 alt ağlara sahip olabilir.

### <a name="can-i-enable-access-from-vpn-and-express-route"></a>VPN ve Expressroute üzerinden erişimi etkinleştirebilirim? 
Azure Cosmos hesabı şirket içi Express route'tan üzerinden erişmek için Microsoft eşdüzey hizmet sağlama etkinleştirmek gerekir. IP Güvenlik Duvarı'nı veya sanal ağ erişim kuralları koymak sonra Azure Cosmos hesabı şirket içi Hizmetleri erişimine izin vermek için Azure Cosmos hesabı IP Güvenlik Duvarı'nı Microsoft eşlemesi için kullanılan genel IP adreslerini ekleyebilirsiniz. 

### <a name="do-i-need-to-update-the-network-security-groups-nsg-rules"></a>Ağ güvenlik grupları (NSG) kurallarını güncelleştirme gerekiyor mu? 
NSG kuralları bağlantısı için ve bir alt ağdaki sanal ağ ile sınırlamak için kullanılır. Alt ağ ile Azure Cosmos DB için hizmet uç noktası eklediğinizde, Azure Cosmos hesabınız için NSG'de giden bağlantıyı açmaya gerek yoktur. 

### <a name="are-service-endpoints-available-for-all-vnets"></a>Hizmet uç noktaları tüm sanal ağları için kullanılabilir mi?
Hayır, yalnızca Azure Resource Manager sanal ağları etkin hizmet bitiş noktası olabilir. Klasik sanal ağ hizmet uç noktaları desteklemez.

### <a name="can-i-accept-connections-from-within-public-azure-datacenters-when-service-endpoint-access-is-enabled-for-azure-cosmos-db"></a>Olabilir miyim "Kabul et bağlantılarından genel Azure veri merkezlerinin içinde" Azure Cosmos DB için hizmet uç noktası erişimi etkinleştirildiğinde?  
Azure Data factory, Azure Search veya tüm hizmet gibi diğer Azure birinci taraf tarafından erişilecek, Azure Cosmos DB hesabı istediğinizde Hizmetleri yalnızca bu gereklidir belirli bir Azure bölgesine dağıtılır.


## <a name="next-steps"></a>Sonraki adımlar

* [Sanal ağ içindeki alt ağı, başarılı Azure Cosmos hesabı erişimini sınırlama](how-to-configure-vnet-service-endpoint.md)
* [Azure Cosmos hesabınız için IP Güvenlik Duvarı yapılandırma](how-to-configure-firewall.md)

