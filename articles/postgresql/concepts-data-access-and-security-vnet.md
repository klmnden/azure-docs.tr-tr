---
title: Uç nokta genel bakış PostgreSQL sunucusu sanal ağ için Azure veritabanı hizmetleri
description: PostgreSQL için Azure veritabanı sunucunuza için sanal ağ hizmet uç noktaları'nasıl çalıştığını öğrenin.
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 08/20/2018
ms.date: 04/22/2019
ms.openlocfilehash: c6549ad170a0fc3b4387d5bc5163ca0548b92119
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60560130"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-database-for-postgresql"></a>Sanal ağ hizmet uç noktaları ve kuralları için PostgreSQL için Azure veritabanı kullanın.

*Sanal ağ kuralları* PostgreSQL için Azure veritabanı sunucunuza belirli alt ağları sanal ağlardaki gönderildiği iletişimleri kabul edip etmeyeceğini denetleyen bir güvenlik duvarı güvenliği özelliğidir. Bu makalede, sanal ağ kuralı özellik bazen PostgreSQL için Azure veritabanı sunucunuza iletişimi güvenli bir şekilde izin vermek için en iyi seçenek olup neden açıklar.

Bir sanal ağ kuralı oluşturmak için öncelikle olmalıdır bir [sanal ağ] [ vm-virtual-network-overview] (VNet) ve bir [sanal ağ hizmet uç noktası] [ vm-virtual-network-service-endpoints-overview-649d] için başvuru kural. Aşağıdaki resimde, bir sanal ağ hizmet uç noktası PostgreSQL için Azure veritabanı ile işleyişi gösterilmektedir:

![Örneği, bir sanal ağ hizmet uç noktasının nasıl çalışır?](media/concepts-data-access-and-security-vnet/vnet-concept.png)

> [!NOTE]
> Bu özellik, PostgreSQL için Azure veritabanı genel amaçlı ve bellek için iyileştirilmiş sunucuları için dağıtıldığı Azure genel bulut, tüm bölgelerde kullanılabilir.
> Trafiği ortak bir VNet ağ geçidi hizmet uç noktaları ile üzerinden akan ve eşler arası akışı beklenir, VNet eşlemesi olması durumunda, Azure sanal makineler PostgreSQL sunucusu için Azure veritabanına erişmek için ağ geçidi sanal ağda izin vermek için bir ACL/sanal ağ kuralı Lütfen oluşturun.

<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminoloji ve açıklaması

**Sanal ağ:** Azure aboneliğinizle ilişkili sanal ağları olabilir.

**Alt ağı:** Bir sanal ağ içeren **alt ağlar**. Tüm Azure sahip olduğunuz sanal makinelerin (VM'ler), alt ağa atanır. Bir alt ağ, birden çok VM veya başka bir işlem düğümünde içerebilir. Sanal ağınızın dışında düğümleri erişime izin vermek için güvenlik yapılandırmadığınız sürece, sanal ağınızın erişemiyor işlem.

**Sanal ağ hizmet uç noktası:** A [sanal ağ hizmet uç noktası] [ vm-virtual-network-service-endpoints-overview-649d] özellik değerleri içeren bir veya daha fazla biçimsel Azure hizmet türü adları bir alt ağ. Bu makalede şu tür adı ile ilgilenen **Microsoft.Sql**, adlandırılmış SQL veritabanı, Azure hizmetini ifade eder. Bu hizmet etiketi, hizmetleri, PostgreSQL ve MySQL için Azure veritabanı için de geçerlidir. Uygularken dikkate almak önemlidir **Microsoft.Sql** hizmet etiketi bir sanal ağ hizmet uç noktası için tüm Azure SQL veritabanı, PostgreSQL için Azure veritabanı ve MySQL Server için Azure veritabanı için hizmet uç noktası trafiğini yapılandırın alt ağda. 

**Sanal ağ kuralı:** PostgreSQL için Azure veritabanı sunucunuza erişim denetimi listesi (ACL) listelenen bir alt ağ için Azure veritabanınızı PostgreSQL sunucusu için bir sanal ağ kuralı önemlidir. PostgreSQL için Azure veritabanı sunucunuza ACL'si olması için alt ağ içermelidir **Microsoft.Sql** tür adı.

Azure veritabanınızı PostgreSQL sunucusu için bir alt ağda bulunan her düğüme gelen iletişimleri kabul etmek için bir sanal ağ kuralı söyler.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Bir sanal ağ kuralı avantajları

Önlem kadar alt ağlardaki Vm'leri PostgreSQL sunucusu için Azure veritabanı ile iletişim kuramıyor. İletişim kuran bir sanal ağ kuralı oluşturulmasını eylemdir. Sanal ağ kuralı yaklaşım seçme stratejinin güvenlik duvarı tarafından sunulan rakip güvenlik seçenekleri içeren bir karşılaştırma ve karşıtlık tartışma gerektirir.

### <a name="a-allow-access-to-azure-services"></a>A. Azure hizmetlerine erişime izin ver

Bağlantı güvenliği bölmesi olan bir **açık/kapalı** etiketli bir düğme **Azure hizmetlerine erişime izin ver**. **ON** ayarı, tüm Azure IP adresleri ve tüm Azure alt ağlar arasındaki iletişimler sağlar. Bu Azure IP'ler veya alt ağlara sahip değil. Bu **ON** ayardır olarak PostgreSQL veritabanı için Azure veritabanınızı istediğinizden daha büyük olasılıkla daha açık. Sanal ağ kuralı özelliği çok daha ayrıntılı denetim olanağı sunar.

### <a name="b-ip-rules"></a>B. IP kuralları

Güvenlik Duvarı'nı PostgreSQL için Azure veritabanı, IP adresi aralıklarını içinden iletişimleri PostgreSQL veritabanı için Azure veritabanı'na kabul belirtmenizi sağlar. Bu yaklaşım, Azure özel ağ dışından kararlı IP adresleri için uygundur. Ancak Azure özel ağ içindeki birçok düğümleri ile yapılandırılan *dinamik* IP adresleri. Sanal makinenizin ne zaman yeniden gibi dinamik IP adresleri değişebilir. Bu bir güvenlik duvarı kuralı, bir üretim ortamında dinamik bir IP adresi belirtmek için folly olacaktır.

IP seçeneği elde ederek hurda bir *statik* , VM için IP adresi. Ayrıntılar için bkz [Azure portalını kullanarak bir sanal makine için özel IP adreslerini yapılandırın][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

Ancak, statik IP yaklaşım yönetmek zor olabilir ve uygun ölçekte kullanıldıklarında maliyeti yüksek. Sanal ağ kuralları oluşturmak ve yönetmek için daha kolay okunuyor.

### <a name="c-cannot-yet-have-azure-database-for-postgresql-on-a-subnet-without-defining-a-service-endpoint"></a>C. Henüz Azure veritabanı PostgreSQL için bir alt ağdaki hizmet uç noktası tanımlamadan sahip olamaz

Varsa, **Microsoft.Sql** sunucusuydu bir düğümde sanal ağınızdaki bir alt ağ, sanal ağ içindeki tüm düğümleri PostgreSQL sunucusu için Azure veritabanı ile iletişim kurulamadı. Bu durumda, sanal makinelerinizi Azure veritabanı ile PostgreSQL için herhangi bir sanal ağ kuralları veya IP kuralları gerek kalmadan iletişim kurulamadı.

Ancak Ağustos 2018'den itibaren hizmet PostgreSQL için Azure veritabanı henüz bir alt ağa doğrudan atanabilir hizmetleri arasında değil.

<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Sanal ağ kuralları hakkında ayrıntılar

Bu bölümde, sanal ağ kuralları hakkında bazı ayrıntılar açıklanmaktadır.

### <a name="only-one-geographic-region"></a>Yalnızca tek bir coğrafi bölge

Her sanal ağ hizmet uç noktası yalnızca bir Azure bölgesine geçerlidir. Uç nokta, alt ağından gelen iletişimi kabul etmek üzere diğer bölgeler etkinleştirmez.

Herhangi bir sanal ağ kuralı, temel alınan bitim uygulandığı bölgeye sınırlıdır.

### <a name="server-level-not-database-level"></a>Sunucu düzeyinde, veritabanı düzeyinde

Her sanal ağ kuralı, tüm Azure veritabanınızı PostgreSQL sunucusuna, yalnızca sunucu üzerindeki belirli bir veritabanı için geçerlidir. Diğer bir deyişle, sunucu düzeyinde-, veritabanı düzeyinde değil, sanal ağ kuralı uygular.

#### <a name="security-administration-roles"></a>Güvenlik Yönetim rolleri

Sanal ağ hizmet uç noktaları Yönetim güvenlik rollerini ayrımı yoktur. Eylem her aşağıdaki roller gereklidir:

- **Ağ Yöneticisi:** &nbsp; Uç noktada bırakın.
- **Veritabanı Yöneticisi:** &nbsp; Erişim denetimi listesi (ACL) belirli alt PostgreSQL sunucusu için Azure veritabanı eklemek için güncelleştirin.

*RBAC alternatif:*

Ağ Yöneticisi ve veritabanı yöneticisi rollerini sanal ağ kuralları yönetmek için gerekli olandan daha fazla özelliğe sahip. Yalnızca bir alt kümesini yeteneklerini gereklidir.

Kullanma seçeneğiniz [rol tabanlı erişim denetimi (RBAC)] [ rbac-what-is-813s] özellikleri yalnızca gerekli kısmı olan tek bir özel rol oluşturmak için azure'da. Özel rol ağ yöneticisi ya da veritabanı yöneticisi içeren yerine kullanılabilir. Diğer iki ana Yöneticisi rollere kullanıcı ekleyerek yerine özel bir rol için bir kullanıcı eklerseniz, güvenlik açıklarını'nın yüzey alanını düşüktür.

> [!NOTE]
> Bazı durumlarda, PostgreSQL ve VNet-alt ağ için Azure veritabanı farklı Aboneliklerde olduğundan. Bu durumlarda aşağıdaki yapılandırmaları emin olmanız gerekir:
> - Her iki aboneliğin aynı Azure Active Directory kiracısı olmalıdır.
> - Kullanıcı, hizmet uç noktaları etkinleştiriliyor ve verilen bir sunucu için bir sanal ağ alt ağı ekleme gibi işlemleri başlatmak için gerekli izinlere sahip.

## <a name="limitations"></a>Sınırlamalar

PostgreSQL için Azure veritabanı için sanal ağ kuralları özelliği aşağıdaki sınırlamalara sahiptir:

- Bir Web uygulaması, bir VNet/alt ağ içinde bir özel IP eşlenebilir. Hizmet uç noktaları belirtilen VNet/alt ağ üzerinde etkin olsa bile, bir Azure genel IP kaynağı, bir VNet/alt ağ kaynak sunucuya Web uygulamasından bağlantıları gerekir. Sanal ağ güvenlik duvarı kuralları olan bir sunucuyu bir Web uygulamasından bağlantıyı etkinleştirmek için sunucu sunucusuna erişmek için izin Azure Hizmetleri gerekir.

- PostgreSQL için Azure veritabanı için Güvenlik Duvarı'nda, her sanal ağ kuralı bir alt ağ başvuruyor. Bu başvurulan tüm alt ağlar, PostgreSQL için Azure veritabanını barındıran aynı coğrafi bölgede barındırılması gerekir.

- Her bir PostgreSQL sunucusu için Azure veritabanı, belirli herhangi bir sanal ağ için 128 ACL girişleri kadar olabilir.

- Sanal ağ kuralları yalnızca Azure Resource Manager sanal ağlara uygulanır. ve değil [Klasik dağıtım modeli] [ arm-deployment-model-568f] ağlar.

- Kullanılarak PostgreSQL için kapatma şirket sanal ağ hizmet uç noktaları için Azure veritabanı **Microsoft.Sql** hizmet etiketi, aynı zamanda uç noktaları tüm Azure veritabanı hizmetleri sağlar: MySQL için Azure veritabanı, PostgreSQL için Azure veritabanı, Azure SQL veritabanı ve Azure SQL veri ambarı.

- Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için sanal ağ hizmet uç noktaları desteğidir.

- Güvenlik Duvarı, IP adresi aralıklarını aşağıdaki ağ öğeleri için geçerlidir, ancak bu sanal ağ kuralları yapın:
    - [Siteden siteye (S2S) sanal özel ağ (VPN)][vpn-gateway-indexmd-608y]
    - Aracılığıyla şirket [ExpressRoute][expressroute-indexmd-744v]

## <a name="expressroute"></a>ExpressRoute

Ağınız aracılığıyla Azure ağına bağlı olup olmadığını [ExpressRoute][expressroute-indexmd-744v], her bağlantı hattı, Microsoft Edge, iki genel IP adresi ile yapılandırılır. İki IP adresi, Microsoft Services gibi Azure depolama için Azure ortak eşleme kullanarak bağlanmak için kullanılır.

İletişiminden bağlantı hattınız için Azure veritabanı PostgreSQL için izin vermek için genel IP adresleri, bağlantı hatları için IP ağ kuralları oluşturmanız gerekir. ExpressRoute devreniz genel IP adreslerini bulmak için Azure portalını kullanarak ExpressRoute ile bir destek bileti açın.

## <a name="adding-a-vnet-firewall-rule-to-your-server-without-turning-on-vnet-service-endpoints"></a>Üzerinde sanal ağ hizmet uç noktaları açmadan sunucunuza bir VNET güvenlik duvarı kuralı ekleme

Yalnızca ayar bir güvenlik duvarı kuralı sunucunun sanal ağa güvenli yardımcı olmaz. Sanal ağ hizmet uç noktalarını da açmanız gerekir **üzerinde** etkili olması güvenlik. Hizmet uç noktaları kapatma zaman **üzerinde**, Geçiş tamamlanana kadar sanal ağ alt ağınızın kapalı kalma süresi deneyimleri **kapalı** için **üzerinde**. Bu özellikle büyük sanal ağlar bağlamında geçerlidir. Kullanabileceğiniz **IgnoreMissingServiceEndpoint** azaltmak veya geçiş sırasında kapalı kalma süresini ortadan kaldırmak için bayrak.

Ayarlayabileceğiniz **IgnoreMissingServiceEndpoint** Azure CLI veya portalı kullanarak bayrağı.

## <a name="related-articles"></a>İlgili makaleler
- [Azure sanal ağları][vm-virtual-network-overview]
- [Azure sanal ağ hizmet uç noktaları][vm-virtual-network-service-endpoints-overview-649d]

## <a name="next-steps"></a>Sonraki adımlar
Sanal ağ kuralları oluşturma hakkında makaleler için bkz:
- [Oluşturma ve Azure portalını kullanarak PostgreSQL VNet kuralları için Azure veritabanı'nı yönetme](howto-manage-vnet-using-portal.md)
- [Oluşturma ve Azure CLI kullanarak PostgreSQL VNet kuralları için Azure veritabanı'nı yönetme](howto-manage-vnet-using-cli.md)


<!-- Link references, to text, Within this same GitHub repo. -->
[arm-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md

[vm-virtual-network-overview]: ../virtual-network/virtual-networks-overview.md

[vm-virtual-network-service-endpoints-overview-649d]: ../virtual-network/virtual-network-service-endpoints-overview.md

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[rbac-what-is-813s]: ../active-directory/role-based-access-control-what-is.md

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.yml

[expressroute-indexmd-744v]: ../expressroute/index.yml