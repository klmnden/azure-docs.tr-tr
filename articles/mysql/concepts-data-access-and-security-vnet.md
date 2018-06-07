---
title: MySQL Server vnet için Azure veritabanı hizmetleri endpoint genel bakış | Microsoft Docs
description: MySQL sunucusu için Azure veritabanınızın vnet hizmet uç noktaları nasıl çalıştığını açıklar.
services: mysql
author: mbolz
ms.author: mbolz
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.topic: conceptual
ms.date: 5/29/2018
ms.openlocfilehash: 652657c8891f2320c026251ffa32e4787028ee18
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655792"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-database-for-mysql"></a>Sanal ağ hizmet uç noktaları ve kuralları Azure veritabanı için MySQL için kullanır.

*Sanal ağ kuralları* Azure veritabanınızı MySQL sunucusu için belirli alt ağları sanal ağlarda gönderildiği iletişim kabul edip etmeyeceğini denetleyen bir güvenlik duvarı güvenlik özelliğidir. Bu makalede neden sanal ağ kuralı özellik bazen güvenli bir şekilde Azure veritabanınıza MySQL sunucusu için iletişime izin vermek için en iyi seçenektir açıklanmaktadır.

Bir sanal ağ kuralı oluşturmak için öncelikle olmalıdır bir [sanal ağ] [ vm-virtual-network-overview] (VNet) ve bir [sanal ağ hizmeti uç noktası] [ vm-virtual-network-service-endpoints-overview-649d] için başvuru kural. Aşağıdaki resimde, bir sanal ağ hizmeti uç noktası Azure veritabanı için MySQL nasıl çalıştığı gösterilmektedir:

![VNet Hizmeti uç noktası nasıl çalıştığını örneği](media/concepts-data-access-and-security-vnet/vnet-concept.png)

> [!NOTE]
> MySQL için Azure veritabanı için bu özellik genel Önizleme Azure veritabanı için MySQL dağıtıldığı Azure genel bulut tüm bölgelerde kullanılabilir.

<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminoloji ve açıklaması

**Sanal ağ:** Azure aboneliğinizle ilişkili sanal ağına sahip olabilir.

**Alt ağ:** içeren bir sanal ağ **alt ağlar**. Tüm Azure sanal olan makineleri (VM'ler) alt ağlara atanır. Bir alt ağ, birden çok VM veya diğer işlem düğümleri içerebilir. Erişime izin vermek için güvenlik yapılandırmadığınız sürece sanal ağınızın dışında olan düğümleri sanal ağınıza erişemez işlem.

**Sanal Ağ Hizmeti uç noktası:** A [sanal ağ hizmeti uç noktası] [ vm-virtual-network-service-endpoints-overview-649d] özellik değerleri içeren bir veya daha fazla resmi Azure hizmeti tür adları bir alt ağ. Bu makalede biz tür adını ilgilendiğiniz **Microsoft.Sql**, adlandırılmış SQL Database, Azure hizmetine başvurduğu. Bu hizmet etiketi Azure veritabanı için MySQL ve PostgreSQL Hizmetleri için de geçerlidir. Uygularken dikkate almak önemlidir **Microsoft.Sql** hizmet etiketi VNet Hizmeti uç noktası için bu hizmeti uç noktası trafiğinin tüm Azure SQL veritabanı, Azure veritabanı için MySQL ve Azure veritabanı PostgreSQL sunucuları için yapılandırır alt ağda. 

**Sanal ağ kuralı:** bir sanal ağ Azure veritabanınızın MySQL sunucusu için erişim denetim listesi (ACL) listelenen bir alt ağ MySQL sunucusu için Azure veritabanınızın kuralıdır. MySQL sunucusu için Azure veritabanınızın ACL olması için alt ağı içermesi gerekir **Microsoft.Sql** türü adı.

Bir sanal ağ kuralı Azure veritabanınızı MySQL sunucusu için alt ağda yer her düğümden iletişimini kabul edecek şekilde söyler.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Bir sanal ağ kuralı yararları

Eylem kazanana kadar ağlarınızı sanal makinelerin MySQL sunucusu için Azure veritabanı ile iletişim kuramıyor. İletişim kuran bir sanal ağ kuralı oluşturulmasını eylemdir. Sanal ağ kuralı yaklaşım seçme stratejinin güvenlik duvarı tarafından sunulan rakip güvenlik seçenekleri içeren bir karşılaştırma karşıtlıklı tartışma gerektirir.

### <a name="a-allow-access-to-azure-services"></a>A. Azure hizmetlerine erişime izin ver

Bağlantı güvenliği bölmesi olan bir **açık/kapalı** etiketli düğmesi **Azure hizmetlerine erişime izin ver**. **ON** ayarı tüm Azure IP adresleri ve tüm Azure alt iletişimlerinden sağlar. Bu Azure IP veya alt size ait değil. Bu **ON** ayardır Azure veritabanınız için MySQL veritabanının olmasını istediğiniz daha büyük olasılıkla daha açık. Sanal ağ kuralı özelliği çok daha ayrıntılı bir denetim sunar.

### <a name="b-ip-rules"></a>B. IP kuralları

Azure veritabanı MySQL Güvenlik Duvarı için IP adres aralıklarını içinden iletişimleri MySQL veritabanı için Azure veritabanına kabul belirtmenize olanak tanır. Bu yaklaşım, Azure özel ağı dışında bulunan kararlı IP adresleri için uygundur. Ancak Azure özel ağ içindeki birçok düğümleri ile yapılandırılan *dinamik* IP adresleri. Dinamik IP adresleri, VM ne zaman yeniden gibi değiştirebilirsiniz. Bir üretim ortamında bir güvenlik duvarı kuralı dinamik bir IP adresi belirtmek üzere folly olacaktır.

Elde ederek IP seçeneği hurda bir *statik* , VM için IP adresi. Ayrıntılar için bkz [Azure portalını kullanarak bir sanal makine için özel IP adreslerini yapılandırın][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Ancak, statik IP yaklaşım yönetmek zor hale gelebilir ve ölçekte yapıldığında pahalıdır. Sanal ağ kuralları oluşturmak ve yönetmek için daha kolay.

### <a name="c-cannot-yet-have-azure-database-for-mysql-on-a-subnet-without-defining-a-service-endpoint"></a>C. Henüz Azure veritabanı için MySQL bir alt ağ üzerinde bir hizmet uç noktası tanımlamadan sahip olamaz

Varsa, **Microsoft.Sql** sunucusu, bir düğümde sanal ağınızdaki bir alt ağ, sanal ağ içindeki tüm düğümler MySQL sunucusu için Azure veritabanı ile iletişim kuramadı. Bu durumda, Vm'lerinizi Azure veritabanı için MySQL herhangi bir sanal ağ kuralları veya IP kuralları gerek kalmadan iletişim kuramadı.

Ancak May 2018 itibariyle, MySQL hizmeti için Azure veritabanı henüz doğrudan bir alt ağa atanabilir hizmetleri arasında değil.

<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Sanal ağ kurallarıyla ilgili ayrıntıları

Bu bölümde, sanal ağ kuralları hakkında birkaç Ayrıntılar açıklanmaktadır.

### <a name="only-one-geographic-region"></a>Yalnızca tek bir coğrafi bölge

Her sanal ağ hizmeti uç noktası, yalnızca bir Azure bölgesi için geçerlidir. Uç nokta, alt ağından gelen iletişimi kabul etmek üzere diğer bölgeler etkinleştirmez.

Tüm sanal ağ kuralı, temel alınan uç noktasında uygulandığı bölge sınırlıdır.

### <a name="server-level-not-database-level"></a>Sunucu düzeyinde, veritabanı düzeyi

Her sanal ağ kuralı tüm Azure veritabanınıza MySQL sunucusu için yalnızca sunucusundaki belirli bir veritabanı için geçerlidir. Diğer bir deyişle, sunucu düzeyinde-, veritabanı düzeyi değil, sanal ağ kuralı uygular.

### <a name="security-administration-roles"></a>Güvenlik Yönetim rolleri

Sanal ağ hizmet uç noktaları Yönetim güvenlik rollerini ayrılması yoktur. Eylem her aşağıdaki rolleri gereklidir:

- **Ağ Yöneticisi:** &nbsp; noktadaki açın.
- **Veritabanı Yöneticisi:** &nbsp; erişim denetim listesi (ACL) Azure veritabanı MySQL sunucusu için belirtilen alt ağ Ekle şekilde güncelleştirin.

*RBAC alternatif:*

Ağ Yöneticisi ve veritabanı yöneticisi rollerini sanal ağ kuralları yönetmek için gerekli olandan daha fazla özelliklere sahiptir. Yalnızca bir alt kümesini yeteneklerini gereklidir.

Kullanma seçeneğiniz [rol tabanlı erişim denetimi (RBAC)] [ rbac-what-is-813s] özellikleri yalnızca gerekli kısmı sahip tek bir özel rolü oluşturmak için Azure. Özel rol ağ yönetici veya veritabanı yöneticisine içeren yerine kullanılabilir. Güvenlik açıklarını'nın yüzey alanını, diğer iki ana yönetici rolü için kullanıcı ekleme ve özel bir rol için bir kullanıcı eklerseniz düşüktür.

> [!NOTE]
> Bazı durumlarda farklı Aboneliklerde Azure veritabanı için MySQL ve sanal alt şunlardır. Bu durumlarda, aşağıdaki yapılandırmaları emin olmalısınız:
> - Her iki aboneliğin aynı Azure Active Directory Kiracı içinde olmalıdır.
> - Kullanıcının hizmet uç noktaları etkinleştirme ve sanal alt verilen sunucuya ekleme gibi işlemleri başlatmak için gerekli izinlere sahip.

## <a name="limitations"></a>Sınırlamalar

MySQL için Azure veritabanı için sanal ağ kuralları özelliği aşağıdaki sınırlamalara sahiptir:

- MySQL için Azure veritabanınızın Güvenlik Duvarı'nda, her sanal ağ kuralı bir alt ağ başvurur. Bu başvurulan tüm alt ağlar için MySQL Azure veritabanını barındıran aynı coğrafi bölgede barındırılması gerekir.

- Her Azure veritabanı MySQL sunucusu için verilen herhangi bir sanal ağ için 128 ACL girişleri kadar sahip olabilir.

- Sanal ağ kuralları yalnızca Azure Resource Manager sanal ağlar için geçerlidir; ve değil [Klasik dağıtım modeli] [ arm-deployment-model-568f] ağlar.

- MySQL kullanmak için kapatma ON sanal ağ hizmet uç noktaları Azure veritabanı **Microsoft.Sql** hizmet etiketi, aynı zamanda tüm Azure veritabanı hizmetleri için uç nokta sağlar: MySQL, PostgreSQL, Azure veritabanı için Azure veritabanı Azure SQL Database ve Azure SQL veri ambarı.

- Genel Önizleme zaman VNet taşıma işlemleri için desteği yoktur. Bir sanal ağ kuralı taşımak için bırakın ve yeniden oluşturun.

- Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için VNet hizmet uç noktaları desteğidir.

- Güvenlik Duvarı'nda, IP adres aralıklarını aşağıdaki ağ öğelere uygulanır, ancak sanal ağ kuralları yapın:
    - [Siteden siteye (S2S) sanal özel ağ (VPN)][vpn-gateway-indexmd-608y]
    - Aracılığıyla şirket içi [ExpressRoute][expressroute-indexmd-744v]

## <a name="expressroute"></a>ExpressRoute

Ağ kullanımı aracılığıyla Azure ağına bağlı olup olmadığını [ExpressRoute][expressroute-indexmd-744v], her bağlantı hattı adresindeki Microsoft Edge iki ortak IP adresi ile yapılandırılır. İki IP adresi Microsoft Services gibi Azure depolama için Azure ortak eşleme kullanarak bağlanmak için kullanılır.

Arasında iletişimi bağlantı hattınız Azure veritabanı MySQL için izin vermek için ortak IP adresleri, bağlantı hatları için IP ağ kuralları oluşturmanız gerekir. Expressroute bağlantı hattı ortak IP adreslerini bulmak için Azure portalını kullanarak ExpressRoute ile bir destek bileti açın.

## <a name="related-articles"></a>İlgili makaleler
- [Azure sanal ağlar][vm-virtual-network-overview]
- [Azure sanal ağı hizmet uç noktaları][vm-virtual-network-service-endpoints-overview-649d]

## <a name="next-steps"></a>Sonraki adımlar
Sanal ağ kuralları oluşturma hakkında daha fazla makaleler için bkz:
- [Oluşturma ve Azure veritabanı için MySQL VNet kuralları Azure Portalı'nı kullanarak yönetme](howto-manage-vnet-using-portal.md)
- [Oluşturma ve Azure veritabanı için MySQL VNet kuralları Azure CLI kullanarak yönetme](howto-manage-vnet-using-cli.md)

<!-- Link references, to text, Within this same Github repo. -->
[arm-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md

[vm-virtual-network-overview]: ../virtual-network/virtual-networks-overview.md

[vm-virtual-network-service-endpoints-overview-649d]: ../virtual-network/virtual-network-service-endpoints-overview.md

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[rbac-what-is-813s]: ../active-directory/role-based-access-control-what-is.md

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.yml

[expressroute-indexmd-744v]: ../expressroute/index.yml