---
title: Azure ağ güvenliği | Microsoft Docs
description: Çeşitli bilgi işlem örnekleri dahil bulut tabanlı bilgi işlem Hizmetleri ve yukarı ve aşağı otomatik olarak uygulama veya Kurumsal ihtiyaçlarını karşılamak üzere ölçeklenebilir hizmetler hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: f684a9d7bca77a8aa3aa60f5079dda0ce3b58a1c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60587486"
---
# <a name="azure-network-security"></a>Azure ağ güvenliği

Güvenlik bulutta iş ve nasıl önemli Azure güvenliği hakkında doğru ve güncel bilgi bulmak olduğunu olduğunu biliyoruz. Azure uygulamaları ve Hizmetleri için kullanılacak en iyi nedeniyle Azure'nın çeşit güvenlik araçları ve özelliklerinden yararlanmak için biridir. Azure platformunda güvenli çözümleri kolayca oluşturmasına olanak sağlar, bu araçları ve özellikleri yardımcı olur.

Microsoft Azure, saydam sorumluluk etkinleştirirken, gizlilik, bütünlük ve kullanılabilirlik Müşteri verilerinin sağlar. Microsoft Azure'da Müşteri'nin açısından uygulanan ağ güvenlik denetimleri koleksiyonunu daha iyi anlamanıza yardımcı olmak için bu makalede, "Azure ağ güvenliği" güvenlik denetimleri Network kapsamlı bir bakış sağlar yazılır Microsoft Azure ile kullanılabilir.

Bu belgede, çeşitli ağ denetimleri, Azure'da dağıttığınız çözümlerinin güvenliğini geliştirmek için yapılandırabileceğiniz hakkında bilgilendirmek için tasarlanmıştır. Microsoft Azure platformu ağ dokusu güvenliğini sağlamak için yaptığı ilgileniyorsanız, Azure güvenlik bölümüne bakın. [Microsoft Trust Center](https://microsoft.com/en-us/trustcenter/cloudservices/azure).

## <a name="azure-platform"></a>Azure platformu

Azure çok sayıda işletim sistemi, dilleri, çerçeveler, Araçlar, veritabanları ve cihazlar programlama destekleyen bir genel bulut hizmeti platformudur.  Linux kapsayıcılarını Docker tümleştirmesiyle çalıştırabilirsiniz; JavaScript, Python, .NET, PHP, Java ve Node.js kullanarak uygulamalar oluşturun; arka iOS, Android ve Windows cihazları uçlar oluşturun. Azure bulut Hizmetleri geliştiricilerin milyonlarca teknolojileri destekler ve BT profesyonellerinin zaten kullandığı ve güvendiği.

Oluşturmak ya da BT varlıklarınızı geçirme bir genel bulut hizmet sağlayıcısı, uygulamalar ve hizmetler ve bulut tabanlı varlıklarınızın güvenliğinin yönetmenizi sağlarlar denetimleri ile verileri korumak için bir kuruluşun yeteneklerine bağlı.

Azure altyapısı tesisten uygulamalara milyonlarca müşteriye aynı anda barındırmak için tasarlanmıştır ve işletmelerin güvenlik gereksinimlerine bağlı karşılayabilecek güvenilir bir temel sunar. Ayrıca, Azure kuruluşunuzun dağıtımlarının benzersiz gereksinimleri karşılamak için güvenlik özelleştirebilmeniz için bunları denetleme olanağı ve yapılandırılabilir güvenlik seçenekleri ile kapsamlı bir koleksiyonu sağlar.

## <a name="abstract"></a>Özet

Microsoft Genel bulut Hizmetleri, Hiper ölçekli hizmetler ve altyapı, Kurumsal düzeydeki özellikleri ve karma bağlantı için birçok seçenek sunar. Bu hizmetler Internet üzerinden veya özel ağ bağlantısı sağlayan Azure ExpressRoute ile erişmek seçebilirsiniz. Microsoft Azure platformu, sorunsuz bir şekilde altyapınızı buluta genişletin ve çok katmanlı mimariler oluşturun olanak tanır. Ayrıca, üçüncü taraf güvenlik hizmetleri ve sanal gereçler sunarak Gelişmiş özellikleri etkinleştirebilirsiniz.

Azure'nın ağ hizmetleri esneklik, kullanılabilirlik, dayanıklılık, güvenlik ve bütünlüğünü tasarım gereği en üst düzeye çıkarın. Bu teknik incelemede, Azure müşterileri, bilgi varlıklarının korunmasına yardımcı olmak için Azure'nın yerel güvenlik özellikleri nasıl kullanabileceğiniz bilgileri ve ağ işlevleri hakkında ayrıntılı bilgi sağlar.

Hedeflenen izleyiciler için bu Teknik İnceleme şunları içerir:

- Teknik yöneticileri, ağ yöneticileri ve güvenlik çözümleri kullanılabilir ve azure'da desteklenen arayan geliştiriciler.

-   SMEs veya Azure ağ teknolojileri ve ağ güvenliği Azure genel bulutunda etrafında tartışmalarında ilgili hizmetlerini üst düzey bir genel bakış elde etmek isteyen iş işlem Yöneticiler.

## <a name="azure-networking-big-picture"></a>Azure ağ büyük resmi
Microsoft Azure, uygulama ve hizmet bağlantı gereksinimlerini desteklemek için sağlam bir ağ altyapısı içerir. Azure'da, şirket içi arasında bulunan kaynaklar arasında ağ bağlantısı mümkündür ve Azure kaynakları, barındırılan ve gelen ve giden İnternet'e ve Azure.

![Azure ağ büyük resmi](media/azure-network-security/azure-network-security-fig-1.png)

[Azure ağ altyapısını](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) güvenli bir şekilde Azure kaynaklarını birbirine sanal ağlar (Vnet'ler) bağlamanıza olanak sağlar. Bir sanal ağ, buluttaki kendi ağınızın bir gösterimidir. Bir sanal ağ, Azure bulut ağı aboneliğinize adanmış mantıksal bir yalıtımının olur. Şirket içi ağlarınızı, sanal ağlara bağlanabilirsiniz.

Azure'un destekledikleri WAN bağlantısının bağlantı, şirket içi ağınız ve Azure sanal ağ ile ayrılmış [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). Azure ile sitenizi arasındaki bağlantı, genel Internet üzerinden geçmez adanmış bir bağlantı kullanır. Birden çok veri merkezlerinde Azure uygulamanız çalışıyorsa, kullanabileceğiniz [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) akıllı bir şekilde uygulama örnekleri arasında kullanıcılardan gelen istekleri. Trafik, Internet'ten erişilebilen olmaları durumunda Azure'da çalışan olmayan hizmetlere de yönlendirebilirsiniz.

## <a name="enterprise-view-of-azure-networking-components"></a>Azure ağ iletişimi bileşenlerinin Kurumsal görünümü
Azure ağ güvenlik tartışmalar için uygun olan çok sayıda ağ bileşenleri içerir. Biz bu ağ bileşenlerini açıklar ve bunlarla ilgili güvenlik sorunlarına odaklanır.

> [!Note]
> Azure ağı tüm yönlerini açıklanmıştır – yalnızca hizmet ve uygulamalarınızı Azure'da dağıtın etrafında bir güvenli ağ altyapısını tasarlama ve planlama pivotal olarak kabul ele alır.

Bu yazıda, kurumsal özelliklere ağ aşağıdaki Azure kapak olacaktır:

-   Temel ağ bağlantısı

-   Karma bağlantı

-   Güvenlik denetimleri

-   Ağ doğrulaması

### <a name="basic-network-connectivity"></a>Temel ağ bağlantısı

[Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) hizmet güvenli bir şekilde Azure kaynaklarını birbirine sanal ağları (VNet) bağlamanıza olanak sağlar. Bir sanal ağ, buluttaki kendi ağınızın bir gösterimidir. Bir sanal ağ, Azure ağ altyapısının aboneliğinize adanmış mantıksal bir yalıtım olur. Sanal ağlar birbirlerine ve siteden siteye VPN kullanarak şirket içi ağlarınız da bağlanabilirsiniz ve ayrılmış [WAN bağlantıları](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![Temel ağ bağlantısı](media/azure-network-security/azure-network-security-fig-2.png)

Ana bilgisayar sunucularına Azure Vm'leri kullanın ve anlama ile bu sanal ağa nasıl bağlanacağını soru şudur. VM'ler için bağlama yanıttır bir [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

Azure sanal ağlar, sanal ağlar gibi şirket içi Microsoft Hyper-V veya VMware gibi kendi sanallaştırma platformunu çözümleri kullanın.

#### <a name="intra-vnet-connectivity"></a>İçi-VNet bağlantısı

Sanal ağlar birbiriyle sanal ağlarda birbirleri ile iletişim kurmak için herhangi bir sanal ağa bağlı kaynaklara etkinleştirme bağlanabilirsiniz. Vnet'leri birbirine bağlamak için veya her ikisini aşağıdaki seçeneklerden birini kullanabilirsiniz:

- **Eşleme:** Farklı Azure birbirleri ile iletişim kurmak için sanal ağlar aynı Azure konumunda içinde bağlı kaynakları sağlar. Sanal ağ arasında gecikme süresi ve bant genişliği var. aynı kaynakları aynı sanal ağa bağlı olmasıyla Eşlemesi hakkında daha fazla bilgi edinmek için [sanal ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

  ![Eşleme](media/azure-network-security/azure-network-security-fig-3.png)

- **VNet-VNet bağlantısı:** Aynı veya farklı Azure konumları içinde farklı Azure sanal ağa bağlı kaynaklar sağlar. Bir Azure VPN ağ geçidi üzerinden trafik akışı gerekir çünkü eşleme aksine, bant genişliği sanal ağlar arasında sınırlıdır.

![VNet-VNet bağlantısı](media/azure-network-security/azure-network-security-fig-4.png)


Bir VNet-VNet bağlantısı ile sanal ağları bağlama hakkında daha fazla bilgi edinmek için [yapılandırma bir VNet-VNet bağlantı makalesine](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json).

#### <a name="azure-virtual-network-capabilities"></a>Azure sanal ağ özellikleri:

Gördüğünüz gibi bir Azure sanal ağı diğer ağ kaynaklarına güvenli bir şekilde bağlanabilmesi için ağa bağlamak için sanal makineler sağlar. Ancak, temel bağlantı yalnızca bir başlangıçtır. Azure sanal ağ hizmeti'nin aşağıdaki özellikleri, Azure sanal ağ güvenlik özelliklerini kullanır:

-   Yalıtım

-   İnternet bağlantısı

-   Azure kaynak bağlantısı

-   Sanal ağa bağlantı

-   Şirket içi bağlantı

-   Trafik filtreleme

-   Yönlendirme

**Yalıtım**

Sanal ağ [yalıtılmış](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) diğerinden. Aynı geliştirme, test ve üretim için ayrı ağlar oluşturabilirsiniz [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) adres bloğu. Buna karşılık, ağları birbirine bağlamak ve farklı CIDR adres bloklarını kullanan birden fazla sanal ağ oluşturabilirsiniz. Bir sanal ağa birden fazla alt ağa bölebilirsiniz.

Azure sanal makineler için iç ad çözümlemesi sağlar ve [Cloud Services](https://azure.microsoft.com/services/cloud-services/) rol örnekleri, bir sanal ağa bağlı. İsteğe bağlı olarak, kendi DNS sunucularınızı Azure dahili ad çözümlemesi yerine kullanılacak bir sanal ağ da yapılandırabilirsiniz.

Her Azure içinde birden fazla sanal ağ uygulayabilirsiniz [abonelik](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json) ve Azure [bölge](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json). Her VNet diğer Vnet'lere yalıtılır. Her sanal ağ için şunları yapabilirsiniz:

-   Genel ve özel (RFC 1918) adresleri kullanarak özel bir gizli IP adresi alanı belirtin. Azure atar kaynakları Vnet'e adres alanından özel bir IP adresi bağlı, atadığınız.

-   Sanal ağ bir veya daha fazla alt ağa bölün ve her alt ağa VNet adres alanının bir bölümü ayırın.

-   Azure tarafından sağlanan ad çözümlemesini kullanabilir veya bir sanal ağa bağlı kaynaklar tarafından kullanılmak üzere kendi DNS sunucunuzu belirtin. Ad çözümleme sanal ağları hakkında daha fazla bilgi edinmek için [VM'ler ve bulut Hizmetleri için ad çözümlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances).

**İnternet bağlantısı**

Tüm [Azure sanal makineler (VM)](https://docs.microsoft.com/azure/virtual-machines/windows/) ve bir sanal ağa bağlı Cloud Services rol örneklerini varsayılan olarak İnternet'e erişimi vardır. Gelen erişimi belirli kaynaklara, gerektiğinde de etkinleştirebilirsiniz. (VM) ve bir sanal ağa bağlı Cloud Services rol örneklerini varsayılan olarak İnternet'e erişimi vardır. Gelen erişimi belirli kaynaklara, gerektiğinde de etkinleştirebilirsiniz.

Bir sanal ağa bağlı tüm kaynaklar varsayılan olarak İnternet'e giden bağlantı vardır. Özel IP adresini kaynağın çevrilmiş kaynak ağ (SNAT) bir genel IP adresini Azure altyapısı tarafından adresidir. Varsayılan bağlantı, özel Yönlendirme ve trafik filtreleme uygulayarak değiştirebilirsiniz. Giden Internet bağlantısı hakkında daha fazla bilgi edinmek için [azure'da giden bağlantıları anlama](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections?toc=%2fazure%2fvirtual-network%2ftoc.json).

Azure kaynaklarına Internet'ten gelen iletişim kurmak için ya da SNAT olmadan İnternet'e giden iletişim kurmak için bir kaynak bir genel IP adresi atanmış olmalıdır. Genel IP adresleri hakkında daha fazla bilgi edinmek için [genel IP adresleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).

**Azure kaynak bağlantısı**

[Azure kaynaklarını](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) gibi bulut Hizmetleri ve sanal makineler aynı sanal ağa bağlanabilir. Farklı alt ağlarda olsalar bile, kaynaklar özel IP adresleri kullanarak bağlantı kurabilir. Azure, varsayılan yapılandırma ve rotaları yönetme zorunluluğunu alt ağlar, sanal ağlar ve şirket içi ağlar arasında yönlendirme sağlar.

Bir sanal ağa sanal makineler (VM), bulut Hizmetleri, App Service ortamları ve sanal makine ölçek kümeleri gibi çeşitli Azure kaynaklarına bağlanabilirsiniz. Vm'leri bir ağ arabirimi (NIC) bir sanal ağ içindeki alt ağa bağlayın. NIC'ler hakkında daha fazla bilgi edinmek için [ağ arabirimleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface).

**Sanal ağa bağlantı**

[Sanal ağlar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) birbiriyle etkinleştirme başka bir VNet üzerinde herhangi bir kaynağa ile iletişim kurmak için herhangi bir sanal ağa bağlı kaynaklara bağlanabilir.

Sanal ağlar birbiriyle sanal ağlarda birbirleri ile iletişim kurmak için herhangi bir sanal ağa bağlı kaynaklara etkinleştirme bağlanabilirsiniz. Vnet'leri birbirine bağlamak için veya her ikisini aşağıdaki seçeneklerden birini kullanabilirsiniz:

- **Eşleme:** Farklı Azure birbirleri ile iletişim kurmak için sanal ağlar aynı Azure konumunda içinde bağlı kaynakları sağlar. Sanal ağlar arasında gecikme süresi ve bant genişliği olan aynı kaynakları için aynı VNet.To bağlıymış gibi hakkında daha fazla bilgi eşlemesi, okuma [sanal ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

- **VNet-VNet bağlantısı:** Aynı veya farklı Azure konumları içinde farklı Azure sanal ağa bağlı kaynaklar sağlar. Bir Azure VPN ağ geçidi üzerinden trafik akışı gerekir çünkü eşleme aksine, bant genişliği sanal ağlar arasında sınırlıdır. Bir VNet-VNet bağlantısı ile sanal ağları bağlama hakkında daha fazla bilgi edinmek için. Daha fazla bilgi edinmek için [bir VNet-VNet bağlantısını yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json) .

**Şirket içi bağlantı**

Sanal ağlara bağlanabilir [şirket içi](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) ağları, ağınız ve Azure arasında özel ağ bağlantıları üzerinden veya Internet üzerinden bir siteden siteye VPN bağlantısı aracılığıyla.

Şirket içi ağınıza aşağıdaki seçeneklerden herhangi bir birleşimini kullanarak bir sanal ağa bağlanabilir:

- **Noktadan siteye sanal özel ağ (VPN):** Tek bir PC bağlı, ağ ve VNet arasında kurdu. Azure’ı kullanmaya yeni başladıysanız bu bağlantı türü mükemmeldir. Mevcut ağınız üzerinde çok az bir değişiklik gerektirdiğinden veya hiç değişiklik gerektirmediğinden geliştiriciler için de mükemmeldir. Bağlantı, PC ve sanal ağ arasında Internet üzerinden şifrelenmiş iletişim sağlamak üzere SSTP protokolünü kullanır. Trafik Internet'ten gönderilir. bu yana bir noktadan siteye VPN için gecikme süresini tahmin edilemez.

- **Siteden siteye VPN:** Bir Azure VPN ağ geçidi ile VPN cihazınız arasında kurdu. Bu bağlantı türü, bir sanal ağa erişebilmesi için yetkilendirme herhangi bir şirket içi kaynak sağlar. Cihazınız şirket içi ve Azure VPN ağ geçidi arasında Internet üzerinden şifrelenmiş iletişimi sağlayan bir IPSec/IKE VPN bağlantısıdır. Trafik Internet'ten gönderilir. bu yana bir siteden siteye bağlantı için gecikme süresini tahmin edilemez.

- **Azure ExpressRoute:** Bir ExpressRoute iş ortağı aracılığıyla ağınız ve Azure arasında kurdu. Bu bağlantı özeldir. Trafik Internet'i dolaşmaz. ExpressRoute bağlantısı için gecikme süresini tahmin edilebilir olduğu trafiği İnternet'e geçiş değil. Önceki bağlantı seçenekleri hakkında daha fazla bilgi edinmek için [bağlantı topolojisi diyagramları](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Trafik filtreleme**

VM ve bulut Hizmetleri rol örnekleri [ağ trafiği](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) gelen ve giden kaynak IP adresi ve bağlantı noktası, hedef IP adresi ve bağlantı noktası ve protokol göre filtrelenebilir.

Aşağıdaki seçeneklerden birini veya her ikisini de kullanarak alt ağlar arasındaki ağ trafiğini filtreleyebilirsiniz:

- **Ağ güvenlik grupları (NSG):** Her NSG'de kaynak ve hedef IP adresi, bağlantı noktası ve protokol olarak giden trafiği filtrelemek için olanak tanıyan birden fazla gelen ve giden güvenlik kuralları içerir. Bir VM'deki her NIC, bir NSG uygulayabilirsiniz. NSG'yi alt ağa bir NIC uygulayabilirsiniz veya diğer Azure kaynaklarında bağlı olduğu. Nsg'ler hakkında daha fazla bilgi edinmek için [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).

- **Sanal ağ Gereçleri:** Sanal ağ Gereci, güvenlik duvarı gibi bir ağ işlevi gerçekleştiren yazılımı çalıştıran bir vm'dir. Azure Marketi'nde kullanılabilir nva'ların listesini görüntüleyin. Nva'ları WAN iyileştirme ve diğer ağ trafiği işlevleri sağlar. Ayrıca kullanılabilir durumdadır. Nva genellikle kullanılan kullanıcı tanımlı olan veya BGP yolları. Bir NVA, sanal ağlar arasındaki trafiği filtrelemek için de kullanabilirsiniz.

**Yönlendirme**

İsteğe bağlı olarak, Azure'nın varsayılan kendi yönlendirmelerinizi yapılandırma ya da bir ağ geçidi arasında BGP yolları kullanarak yönlendirme geçersiz kılabilirsiniz.

Azure, varsayılan olarak birbirleri ile iletişim kurmak için herhangi bir sanal ağ içindeki herhangi bir alt ağa bağlı kaynaklara sağlayan rota tabloları oluşturur. Azure’ın oluşturduğu varsayılan rotaları geçersiz kılmak için aşağıdaki seçeneklerden birini veya her ikisini uygulayabilirsiniz:

- **Kullanıcı tanımlı yollar:** Trafiği için her alt ağ için yönlendirildiği denetleyen rotalarla özel rota tabloları oluşturabilirsiniz. Kullanıcı tanımlı yollar hakkında daha fazla bilgi edinmek için [kullanıcı tanımlı yollar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

- **BGP yolları:** Sanal ağınızı bir Azure VPN Gateway veya ExpressRoute bağlantısı kullanarak şirket içi ağınıza bağlanırsa BGP yolları sanal ağlarınıza yayabilirsiniz.

### <a name="hybrid-internet-connectivity-connect-to-an-on-premises-network"></a>Karma internet bağlantısı: Bir şirket içi ağa bağlanma
Şirket içi ağınıza aşağıdaki seçeneklerden herhangi bir birleşimini kullanarak bir sanal ağa bağlanabilir:

-   İnternet bağlantısı

-   Noktadan siteye VPN (P2S VPN)

-   VPN siteden siteye (S2S VPN)

-   ExpressRoute

#### <a name="internet-connectivity"></a>Internet bağlantısı

Adından da anlaşılacağı gibi Internet bağlantısı iş yüklerinizi Internet'ten erişilebilir, sanal ağ içinde Canlı iş yüklerini farklı genel uç noktalarını kullanıma sağlayarak yapar. Bu iş yükleri kullanarak sunulabilir [Internet'e yönelik Yük Dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview) veya yalnızca VM'ye bir genel IP adresi atama. Bu şekilde, bir ana bilgisayar güvenlik duvarı, sağlanan sanal makine, ulaşmak için Internet üzerindeki herhangi bir şey mümkün hale [ağ güvenlik grupları (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg), ve [kullanıcı tanımlı rotaları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) değere izin ver gerçekleşir.

Bu senaryoda, genel Internet'e ve içinden herhangi bir yere veya iş yüklerinizin yapılandırmasına bağlı olarak belirli konumlar üzerinden bağlanmak için gereken bir uygulama geçmesine neden olabilir.

#### <a name="point-to-site-vpn-or-site-to-site-vpn"></a>Noktadan siteye VPN veya siteden siteye VPN
Bu iki aynı kategoriye döner. Her ikisi de ağınıza bir VPN ağ geçidi olması gerekir ve ya da kullanarak bir VPN istemcisi için iş istasyonunuzdan parçası olarak bağlanabildiğinizden [noktadan siteye yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) veya şirket içi yapılandırabilirsiniz [VPN cihazı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices)bir siteden siteye VPN'yi sonlandırmak için. Bu şekilde şirket içi cihazlar sanal ağ içindeki kaynaklara bağlanabilir.

Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan bir sanal ağa yönelik güvenli bağlantı oluşturmanıza olanak sağlar. P2S, SSTP (Güvenli Yuva Tünel Protokolü) aracılığıyla gerçekleşen bir VPN bağlantısıdır.

![Noktadan siteye VPN](media/azure-network-security/azure-network-security-fig-5.png)

Sanal ağınıza uzak bir konumdan örneğin ev veya bir konferans Merkezi'nden bağlanmak istediğinizde ya da yalnızca bir sanal ağa bağlanması gereken birkaç istemciniz bulunduğunda noktadan siteye bağlantıları kullanışlıdır.

P2S bağlantılarının bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. VPN bağlantısını istemci bilgisayardan kurarsınız. Bu nedenle, P2S yolu Azure ağınız birçok şirket içi cihazlar ve bilgisayarlar kalıcı bir bağlantı gerektiği durumlarda Azure'a bağlanmak için önerilmez.

![Siteden siteye VPN](media/azure-network-security/azure-network-security-fig-6.png)

> [!Note]
> Noktadan siteye bağlantılar hakkında daha fazla bilgi için bkz. [noktadan siteye FA v Q](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal).

Siteden Siteye VPN ağ geçidi bağlantısı, şirket içi ağınızı bir IPsec/IKE (IKEv1 veya IKEv2) tüneli üzerinden Azure sanal ağına bağlamak için kullanılır.

Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir. Bu bağlantı Internet üzerinden gerçekleşir ve ağ ve Azure arasında şifrelenmiş bir bağlantı içinde "tüneli" bilgi sağlar. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. Tünel şifreleme kullanarak gerçekleştirilir [IPSec tünel modu](https://technet.microsoft.com/library/cc786385.aspx).

Siteden siteye VPN güvenli, güvenilir ve yerleşik bir teknoloji olsa da, tünel içindeki trafik İnternet'e geçiş. Ayrıca, en fazla 200 MB/sn bant genişliği görece sınırlıdır.

İçi ve dışı bağlantılarınız için bir olağanüstü güvenlik veya performans düzeyine ihtiyacınız varsa, Azure ExpressRoute içi ve dışı karışık bağlantı için kullanmanızı öneririz. ExpressRoute adanmış WAN olan şirket içi konumunuz ve Exchange barındırma sağlayıcısı arasındaki bağlantı. Bu telco bağlantı olduğundan, verilerinizi Internet üzerinden yolculuk değil ve bu nedenle Internet iletişim devralınan olası riskler gösterilmez.

> [!Note]
> VPN ağ geçitleri hakkında daha fazla bilgi için bkz: [VPN ağ geçidi](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

#### <a name="dedicated-wan-link"></a>Adanmış bir WAN bağlantısı
Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan adanmış özel bağlantı üzerinden Azure'a şirket içi ağlarınızı genişletmenizi sağlar.

ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Bu, ExpressRoute bağlantılarına İnternet üzerindeki sıradan bağlantılara göre daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantılardan daha yüksek güvenlik sağlar.

![ Adanmış bir WAN bağlantısı](media/azure-network-security/azure-network-security-fig-7.png)

> [!Note]
> ExpressRoute kullanarak Microsoft'a ağınıza bağlanma hakkında daha fazla bilgi için bkz: [ExpressRoute bağlantı modelleri](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) ve [Expressroute'a teknik genel bakış](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

Olarak siteden siteye VPN seçeneklerle ExpressRoute mutlaka yalnızca bir sanal ağda olmayan kaynaklara bağlanmasını sağlar. Aslında, SKU'ya bağlı olarak, 10 sanal ağlara bağlanabilirsiniz. Varsa [premium eklenti](https://docs.microsoft.com/azure/expressroute/expressroute-faqs), en fazla 100 sanal ağlar için bağlantıları olası bağlı olarak bant genişliği. Bu tür bağlantıları görünüm beğendiğiniz özellikler hakkında daha fazla bilgi edinmek için [bağlantı topolojisi diyagramları](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="security-controls"></a>Güvenlik denetimleri
Bir Azure sanal ağı diğer sanal ağlardan yalıtılır ve şirket içi ağlarınızı kullanan çok sayıda güvenlik denetimleri destekleyen güvenli, mantıksal bir ağ sağlar. Müşteriler, kendi yapı kullanarak oluşturun: alt ağlar — bunlar, kendi özel IP adresi aralığı kullanın, rota tablolarını yapılandırmak, ağ güvenlik grupları, erişim denetimi listeleri (ACL'ler) ağ geçitleri ve sanal gereçler, iş yüklerini bulutta çalıştırmak için.

Azure sanal ağlarda kullanabilirsiniz güvenlik denetimleri şunlardır:

-   Ağ erişim denetimleri

-   Kullanıcı tanımlı yollar

-   Ağ güvenlik Gereci

-   Application Gateway

-   Azure Web uygulaması güvenlik duvarı

-   Ağ kullanılabilirlik denetimi

#### <a name="network-access-controls"></a>Ağ erişim denetimleri
Azure sanal ağı (VNet) Azure ağ modelinin temel taşıdır ve yalıtım ve koruma sağlayan [ağ güvenlik grupları (NSG)](https://blogs.msdn.microsoft.com/igorpag/2016/05/14/azure-network-security-groups-nsg-best-practices-and-lessons-learned/) zorlamak ve ağ trafiği kuralları, kontrol etmek için kullandığınız ana aracı Ağ düzeyi.

![ Ağ erişim denetimleri](media/azure-network-security/azure-network-security-fig-8.png)


Erişimine izin verme veya reddetme iş yükleri bir sanal ağdan şirket içi bağlantılar aracılığıyla müşterinin ağlarındaki sistemleri arasındaki iletişim tarafından erişimi denetlemek veya Internet iletişimi doğrudan.

Diyagramda, hem sanal ağlar ve Nsg'ler yığındaki burada Nsg'ler, UDR ve ağ sanal Gereçleri korumalı ağ uygulama dağıtımları korumak için güvenlik sınırları oluşturmak için kullanılan Azure genel güvenlik, belirli bir katman içinde yer alır.

Nsg'ler trafik değerlendirmek için 5-tuple kullanın (ve NSG için yapılandırma kuralları kullanılır):

-   [Kaynak ve hedef IP adresi](https://support.microsoft.com/help/969029/the-functionality-for-source-ip-address-selection-in-windows-server-2008-and-in-windows-vista-differs-from-the-corresponding-functionality-in-earlier-versions-of-windows)

-   [Kaynak ve hedef bağlantı noktası](https://technet.microsoft.com/library/dd197515)

-   Protokol: [İletim Denetimi Protokolü (TCP)](https://technet.microsoft.com/library/cc940037.aspx) veya [kullanıcı veri birimi Protokolü (UDP)](https://technet.microsoft.com/library/cc940034.aspx)

Başka bir deyişle, tüm alt ağlar arasında veya tek bir VM ve VM veya başka bir tek VM için tek bir VM grubu arasında erişimi denetleyebilirsiniz. Yeniden basit durum bilgisi olan paket filtreleme, bu, tam paket incelemesi unutmayın. Protokol doğrulama veya ağ düzeyi kimlik veya bir ağ güvenlik grubu IP'ler yeteneği yoktur.

Bir NSG farkında olmanız gereken bazı yerleşik kurallar ile birlikte gelir. Bunlar:

-   **Belirli bir sanal ağ içindeki tüm trafiğe izin ver:** Tüm VM'lerin aynı Azure sanal ağı üzerinde birbirleriyle iletişim kurabilir.

-   **Azure Yük Dengeleme için gelen izin ver:**  bu kural Azure yük dengeleyici için herhangi bir hedef adresi herhangi bir kaynak adresinden gelen trafiği sağlar.

-   **Gelenlerin tümünü Reddet:**  bu kural açıkça izin Internet'ten kaynağını tüm trafiği engeller.

-   **İnternet'e giden tüm trafiğe izin:** Bu kural, VM'lerin İnternet'e yönelik bağlantıları başlatmasını sağlar. Başlatılan bu bağlantıları istemiyorsanız, bu bağlantıları engelle veya zorlamalı tünel zorlamak için bir kural oluşturmanız gerekir.

#### <a name="system-routes-and-user-defined-routes"></a>Sistem yolları ve kullanıcı tanımlı yollar

Azure'da bir sanal ağ (VNet) sanal makineleri (VM'ler) eklediğiniz zaman, VM'lerin birbirleri ile ağ üzerinden otomatik olarak iletişim kurabilir olduğuna dikkat edin. VM'ler farklı alt ağlarda bulunsa bile bir ağ geçidini belirtmenize gerek yoktur.

Aynı şey VM'lerden genel İnternet'e giden iletişimlerde ve hatta Azure'dan kendi veri merkezinize karma bir bağlantı bulunduğunda şirket içi ağınıza giden iletişimlerde de geçerlidir.

![Sistem Yolları](media/azure-network-security/azure-network-security-fig-9.png)

Bu iletişim akışının mümkün olmasını sağlayan şey, Azure'ın IP trafiğinin nasıl akacağını belirlemek için kullandığı bir dizi sistem yoludur. Sistem yolları aşağıdaki senaryolarda iletişim akışını denetler:

-   Aynı alt ağ içinden.

-   Bir sanal ağ içinde bir alt ağdan başka bir alt ağa.

-   VM'lerden İnternet'e.

-   Bir VPN ağ geçidi yoluyla bir sanal ağdan başka bir sanal ağa.

-   VNet eşlemesi aracılığıyla başka bir sanal ağa bir vnet'ten ([hizmet zinciri](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)).

-   Bir VPN ağ geçidi yoluyla bir sanal ağdan şirket içi ağınıza.

Çoğu kurum katı güvenlik varsa ve şirket içi İnceleme belirli zorlamak için tüm ağ paketlerinin gerektiren uyumluluk gereksinimlerini ilkeleri. Azure adlı bir mekanizma sağlar [zorlamalı tünel](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-forced-tunneling) özel bir yol oluşturarak veya şirket içi Vm'lerden gelen trafiği yönlendirmeleri [Border Gateway Protocol (BGP)](https://docs.microsoft.com/windows-server/remote/remote-access/bgp/border-gateway-protocol-bgp) ExpressRoute aracılığıyla reklamları veya VPN.

Zorlamalı tünel Azure'da sanal ağ kullanıcı tanımlı yollar (UDR) yapılandırılır. Bir şirket içi siteye trafik yönlendirme Azure VPN ağ geçidi için varsayılan bir yol olarak ifade edilir.

Aşağıdaki bölümde, Azure sanal ağı için yönlendirme tablosu ve yol geçerli sınırlama listelenmektedir:

- Her sanal ağ alt ağı, yerleşik bir sistem yönlendirme tablosu vardır. Sistem yönlendirme tablosu yolların aşağıdaki üç grup vardır:

  -  **Yerel sanal ağ yolları:** Doğrudan hedefe Vm'leri aynı sanal ağda

  - **Şirket içi yollara:** Azure VPN ağ geçidi

  -  **Varsayılan yol:** Doğrudan Internet'e. Önceki iki yol tarafından kapsandığından değil özel IP adreslerini hedefleyen paketler bırakılır.

- Kullanıcı tanımlı yollar'ın yayınlanmasıyla birlikte, varsayılan bir yol eklemek için bir yönlendirme tablosu oluşturun ve ardından bu alt ağlarda zorlamalı tüneli etkinleştirmek için sanal ağ alt ağına yönlendirme tablosunu ilişkilendirme.

- "Sanal ağa bağlı bir varsayılan site" şirket içi yerel siteleri arasında ayarlamanız gerekir.

- Zorlamalı tünel dinamik yönlendirme VPN ağ geçidi (bir statik ağ geçidi sorunsuz değil) sahip bir sanal ağ ile ilişkilendirilmiş olması gerekir.

- Zorlamalı tünel ExpressRoute Bu mekanizma yapılandırılmamış, ancak bunun yerine, bir varsayılan rota üzerinden ExpressRoute BGP eşliği oturumlarını reklam tarafından etkinleştirilir.

> [!Note]
> Daha fazla bilgi için [ExpressRoute belgeleri](https://azure.microsoft.com/documentation/services/expressroute/) daha fazla bilgi için.

#### <a name="network-security-appliances"></a>Ağ güvenlik Gereçleri
Ağ güvenlik gruplarını ve kullanıcı tanımlı rotaları belirli bir ağ ve Aktarım katmanı güvenlik ağ ölçü sağlarken [OSI modeli](https://en.wikipedia.org/wiki/OSI_model), var olan geçmeye burada istediğiniz veya güvenlik etkinleştirmek gereken durumlar olabilir. Ağ yığınının çoğunun yüksek düzey. Bu gibi durumlarda, Azure iş ortakları tarafından sağlanan sanal ağ güvenlik Gereçleri dağıtmanızı öneririz.

![Ağ güvenlik Gereçleri](./media/azure-network-security/azure-network-security-fig-10.png)

Azure ağ güvenlik Gereçleri, sanal ağ güvenlik ve ağ işlevlerini geliştirmek ve bunlara çok sayıda satıcılardan erişilebilir [Azure Marketi](https://azuremarketplace.microsoft.com). Bu sanal güvenlik Gereçleri sağlamak için dağıtılabilir:

-   Yüksek oranda kullanılabilir güvenlik duvarları

-   Yetkisiz erişim önleme

-   Yetkisiz giriş algılama

-   Web uygulaması güvenlik duvarları (Waf)

-   WAN iyileştirmesi

-   Yönlendirme

-   Yük dengeleme

-   VPN

-   Sertifika yönetimi

-   Active Directory

-   Çok faktörlü kimlik doğrulaması

#### <a name="application-gateway"></a>Uygulama ağ geçidi

[Microsoft Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) bir uygulama teslim denetleyicisi (ADC) hizmet olarak sağlar özel bir sanal gereçtir.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-11.png)

Uygulama ağ geçidi, yoğun CPU SSL sonlandırması yükünü uygulama ağ geçidi (SSL boşaltma) boşaltarak web grubu performansı ve kullanılabilirliği iyileştirmek sağlar. Ayrıca, dahil olmak üzere diğer 7. Katman yönlendirme özelliklerini sağlar:

-   Gelen trafiğin dönüşümlü dağıtımı

-   Tanımlama bilgilerine dayalı oturum benzeşimi

-   URL yolu tabanlı yönlendirme

-   Tek bir Application Gateway arkasında birden çok Web sitesini barındırma olanağı


A [web uygulaması Güvenlik Duvarı (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) de uygulama ağ geçidi bir parçası olarak sağlanır. Bu koruma web uygulamalarını yaygın web güvenlik açıklarına ve açıklardan yararlanmaya karşı sağlar. Uygulama ağ geçidini ağ geçidi, yalnızca dahili ağ geçidi veya her ikisinin bir birleşiminde Internet'e yönelik yapılandırılabilir.

Application Gateway WAF algılama veya önleme modunda çalıştırabilirsiniz. Yöneticiler trafik için kötü amaçlı düzenleri gözlemlemek için algılama modunda çalışacak şekilde yaygın bir kullanım örneği içindir. Olası açıklara algılandığında önleme moduna kapatma şüpheli gelen trafiği engeller.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-12.png)

Ayrıca, Application Gateway WAF ile tümleştirilen ve gerçek zamanlı bir WAF günlüğü kullanarak saldırılarına karşı web uygulamalarını izlemenize yardımcı olur [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) WAF uyarıları izlemek için ve eğilimleri kolayca izleyin.

JSON biçimli günlük müşterinin depolama hesabına doğrudan gider. Bu günlükler üzerinde tam denetime sahiptir ve kendi bekletme ilkeleri uygulayabilirsiniz.

Kendi Analytics'i kullanarak sistem bu günlükleri alabilen [Azure günlük tümleştirmesi](https://aka.ms/AzLog). WAF günlükleri ile tümleştirilerek ayrıca [Azure İzleyici günlükleri](../log-analytics/log-analytics-overview.md) Gelişmiş ayrıntılı sorguları yürütmek için Azure İzleyici günlüklerine kullanabilirsiniz.

#### <a name="azure-web-application-firewall-waf"></a>Azure web uygulaması Güvenlik Duvarı (WAF)

Web uygulamaları olan giderek SQL ekleme, siteler arası komut dosyası saldırıları ve görünen diğer saldırıları gibi bilinen yaygın güvenlik açıklarından kötü amaçlı saldırıları hedefleri [OWASP ilk 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project). Uygulama bu tür saldırılara önleme ayrıntılı bakım, düzeltme eki uygulama ve uygulama topolojisinin birden çok katmanına izleme gerektirir.

 ![Azure Web uygulaması Güvenlik Duvarı (WAF)](./media/azure-network-security/azure-network-security-fig-13.png)

Bir merkezi [web uygulaması Güvenlik Duvarı (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) web saldırılarına karşı koruyabilir ve herhangi bir uygulama değişikliğe gerek kalmadan güvenlik yönetimini basitleştirir.

Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.

#### <a name="network-availability-controls"></a>Ağ kullanılabilirlik denetimleri

Microsoft Azure’u kullanarak ağ trafiğini dağıtmak için farklı seçenekler bulunur. Bu seçenekler birbirlerinden farklı şekilde çalışır, farklı özelliklere sahiptir ve farklı senaryoları destekler. Birbirlerinden ayrı olarak veya birleştirilerek kullanılabilirler.

Ağ kullanılabilirlik denetimleri aşağıda verilmiştir:

-   Azure Load Balancer

-   Application Gateway

-   Traffic Manager

**Azure yük dengeleyici**

Uygulamalarınıza yüksek düzeyde kullanılabilirlik ve ağ performansı sağlar. Bu gelen trafiği sağlıklı yük dengeli bir küme içinde tanımlı Hizmetleri örnekleri arasında dağıtan bir katman 4 (TCP, UDP) yük dengeleyicidir.

 ![Azure Load Balancer](media/azure-network-security/azure-network-security-fig-14.png)


Azure Load Balancer için yapılandırılabilir:

-   Sanal makinelere gelen Internet trafiğini Dengeleme yükleyin. Bu yapılandırma olarak bilinir [Internet'e yönelik Yük Dengeleme](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Bir sanal ağdaki sanal makineler arasında bulut Hizmetleri sanal makineler arasında veya şirket içi bilgisayarlar ile şirketler arası sanal ağdaki sanal makineler arasında Yük Dengeleme trafiği. Bu yapılandırma olarak bilinir [iç Yük Dengeleme](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview).

-   Dış trafiği belirli bir sanal makineye iletir.

Buluttaki tüm kaynaklara Internet'ten erişilebilir olması için genel bir IP adresi gerekir. Azure'da bulut altyapısı için kaynaklarını yönlendirilemeyen IP adreslerini kullanır. Azure, Internet ile iletişim kurmak için genel IP adresi ile ağ adresi çevirisi (NAT) kullanır.

 **Uygulama ağ geçidi**

 Uygulama ağ geçidini uygulama katmanında (OSI Ağ başvurusu yığınında katman 7) çalışır. İstemci bağlantısını sonlandıran ve istekleri arka uç noktalarına ileten ters proxy hizmeti olarak çalışır.

 **Traffic manager**

Microsoft Azure Traffic Manager, farklı veri merkezlerindeki hizmet uç noktaları için kullanıcı trafiğinin dağıtımını denetlemenizi sağlar. Hizmet uç noktaları traffic Manager tarafından desteklenen Azure Vm'lerinde, Web uygulamaları ve bulut Hizmetleri. Traffic Manager’ı harici, Azure dışı uç noktalar için de kullanabilirsiniz.

Traffic Manager, istemci isteklerine göre en uygun uç noktaya doğrudan etki alanı adı sistemi (DNS) kullanan bir [trafik yönlendirme yöntemine](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) ve sistem durumu uç nokta. Traffic Manager trafik yönlendirme yöntemlerini uç nokta sistem durumu olan farklı uygulama gereksinimlerinize göre bir dizi sunar [izleme](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring)ve otomatik yük devretme. Traffic Manager, bir Azure bölgesinin tamamının devre dışı kalması dahil olmak üzere hatalara dayanıklıdır.

Azure Traffic Manager, uygulama uç noktalar genelinde trafiğinin dağıtımını denetlemenizi sağlar. Uç nokta, Azure içinde veya dışında barındırılan İnternet'e yönelik bir hizmettir.

Traffic Manager, iki temel fayda sağlar:

-   Dağıtım birkaç birine göre trafik [trafik yönlendirme yöntemlerini](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods).

-   [Uç nokta durumunu sürekli izleme](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring) ve uç noktaları başarısız olduğunda otomatik yük devretme.

Bir istemci bir hizmete bağlanma girişiminde bulunduğunda, bu hizmetin DNS adını bir IP adresi için ilk çözmeniz gerekir. Ardından, istemci hizmete erişmek için bu IP adresine bağlanır. Traffic Manager trafik yönlendirme yönteminin kurallara göre belirli hizmet uç noktaları istemcilere yönlendirmek için DNS kullanır. İstemciler, seçili uç noktaya doğrudan bağlanır. Traffic Manager, bir proxy ya da bir ağ geçidi değil. Traffic Manager, istemci ile hizmet arasında geçen trafiği görmez.

### <a name="azure-network-validation"></a>Azure ağ doğrulaması

Azure ağ doğrulaması olan Azure ağı yapılandırıldığı ve doğrulama yapılabilir gibi çalıştığından emin olmak için ağ izleme kullanılabilen özellikleri ve Hizmetleri kullanarak. Azure Ağ İzleyicisi ile günlüğe kaydetme deseninizi oluşturmayı erişebilir ve öngörüleri ağ performansını ve durumunu anlamak için ihtiyaç duyduğunuz güce sahip tanılama özellikleri. Bu özellikler, Portal, PowerShell, CLI, Rest API ve SDK erişilebilir.

Azure operasyonel güvenlik hizmetleri, denetimleri ve kullanıcılara sunulan özellikleri verilerini, uygulamalarını ve diğer varlıklardan Microsoft azure'da korumak için ifade eder. Azure çalışma Güvenliği aracılığıyla edinilen Microsoft Security Development Lifecycle (SDL), programın Microsoft Güvenlik Yanıt Merkezi dahil Microsoft'a özgü bir çeşitli özellikleri bilgi içeren bir framework üzerine inşa edilmiştir ve siber güvenlik tehditleri hakkındaki ayrıntılı tanıma.

-   [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)

-   [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

-   [Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

-   [Azure depolama analizi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

-   Azure Resource Manager

#### <a name="azure-resource-manager"></a>Azure resource manager

Microsoft Azure çalışan işlemleri ve insanlara belki de en önemli güvenlik platform özelliğidir. Bu bölümde, geliştirmek, güvenlik, sürekliliğini ve gizlilik yardımcı Microsoft'un küresel veri merkezi altyapı özelliklerini açıklar.

Uygulamanızın altyapısı genellikle bir sanal makine, depolama hesabı, sanal ağ veya web uygulaması, veritabanı, veritabanı sunucusu ya da üçüncü taraf hizmetler gibi birçok bileşenden meydana gelir. Bu bileşenleri ayrı varlıklar olarak değerlendirmez, bunun yerine bunları tek bir varlığın ilgili ve birbirine bağımlı parçaları olarak kabul edersiniz. Bunları gruplar halinde dağıtmak, yönetmek ve izlemek isteyebilirsiniz. Azure Resource Manager, çözümünüzdeki kaynaklar ile gruplar halinde çalışmanıza olanak sağlar.

Çözümünüzdeki tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilirsiniz. Dağıtım için bir şablon kullanabilirsiniz. Üstelik bu şablon test, hazırlık ve üretim gibi farklı ortamlarda da çalışabilir. Resource Manager kaynaklarınızı dağıttıktan sonra yönetmenize yardımcı olmak için güvenlik, denetleme ve etiketleme özellikleri sunar.

**Resource Manager'ı kullanmanın avantajları**

Resource Manager çeşitli avantajlar sunar:

-   Çözümünüzdeki tüm kaynakları ayrı ayrı ele almak yerine bunları grup halinde dağıtabilir, yönetebilir ve izleyebilirsiniz.

-   Çözümünüzü geliştirme yaşam döngüsü boyunca defalarca dağıtabilirsiniz. Kaynaklarınızın tutarlı bir durumda dağıtılması da size güven verir.

-   Altyapınızı betikler yerine bildirim temelli şablonları kullanarak yönetebilirsiniz.

-   Doğru sırayla dağıtılmalarını sağlamak kaynaklarınız arasındaki bağımlılıkları tanımlayabilirsiniz.

-   Rol Tabanlı Erişim Denetimi (RBAC) yönetim platformuyla doğrudan tümleşik olduğu için kaynak grubunuzdaki tüm hizmetlere erişim denetimi uygulayabilirsiniz.

-   Aboneliğinizdeki tüm kaynakları mantıksal olarak düzenlemek için kaynaklarınıza etiketler ekleyebilirsiniz.

-   Etiketi paylaşan bir kaynak grubunun maliyetlerini görüntüleyerek kuruluşunuzun faturalarına açıklık getirebilirsiniz.

> [!Note]
> Resource Manager çözümlerinizi dağıtmanın ve yönetmenin yeni bir yolunu sunar. Önceki dağıtım modelini kullandıysanız ve değişiklikler hakkında bilgi edinmek isterseniz, bkz. [Resource Manager dağıtımını ve klasik dağıtımı anlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model)

## <a name="azure-network-logging-and-monitoring"></a>Azure ağ günlüğe kaydetme ve izleme

Azure izleme, önleyin, algılayın ve ağ güvenlik olaylarına yanıt vermek için birçok araç sunar. Bu alanda kullanılabilir en güçlü araçlardan bazıları şunlardır:

-   Ağ İzleyicisi

-   Ağ kaynak düzeyi izleme

-   Azure İzleyici günlükleri

### <a name="network-watcher"></a>Ağ İzleyicisi

[Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) -senaryo tabanlı izleme Ağ İzleyicisi özellikleri ile birlikte sağlanır. Bu hizmet içeren paket yakalama, sonraki atlama IP akışı doğrulama, güvenlik grubu görünümü, NSG akış günlükleri. Senaryo düzeyi izleme ağ kaynaklarını tek tek ağ kaynak izleme aksine bir uçtan uca görünümünü sağlar.

 ![Ağ İzleyicisi](./media/azure-network-security/azure-network-security-fig-15.png)

Ağ İzleyicisi, koşulları ağ senaryosu düzeyinde, azure'a veya azure'dan izlemenizi ve tanılamanızı sağlayan bölgesel bir hizmettir. Ağ Tanılama ve görselleştirme araçları Ağ İzleyicisi ile kullanılabilen anlamanıza, tanılamanıza ve ağınıza azure'da Öngörüler elde etmeye yardımcı olur.

Ağ İzleyicisi şu anda aşağıdaki özellikleri içerir:

#### <a name="topology"></a>Topoloji

[Topoloji](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) bir sanal ağda ağ kaynaklarının bir grafik döndürür. Grafiğin bağlantısı uçtan uca ağ bağlantısını temsil etmek için kaynakları arasındaki gösterilmektedir. Portalda, topoloji kaynak nesneleri üzerinde göre sanal ağ başına döndürür. Kaynak grubu görüntülenmeyecek olsa bile Ağ İzleyicisi bölgesinin dışındaki kaynaklar arasındaki çizgilerle ilişkileri açıklanmamıştır. Portal Görünümü'nde döndürülen kaynaklar grafiği çizilecek ağ bileşenleri bir alt kümesidir. Ağ kaynaklarının tam listesini görmek için kullanabileceğiniz [PowerShell](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-powershell) veya [REST](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-rest).

Kaynakları döndürülen bunlar arasındaki bağlantıyı modellenmiş altında iki ilişkisi.

- **Kapsama** -sanal ağ, bir NIC içeren bir alt ağ içerir

- **İlişkili** -bir NIC, bir VM ile ilişkilendirilmiş.

#### <a name="variable-packet-capture"></a>Değişken paket yakalama

Ağ İzleyicisi [değişken paket yakalama](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) bir sanal makineye gelen ve giden trafiği izlemek için paket yakalama oturumu oluşturmanıza olanak sağlar. Ve proactivity her iki ağ anomalileri öngörülebiliyorsa tanılamak için paket Yakalama yardımcı olur. Diğer kullanımlar ağ izinsiz girişi, istemci-sunucu iletişimleri ve daha fazlasını hata ayıklamak için bilgi elde etme, ağ istatistikleri toplama içerir.

Paket yakalaması Uzaktan Ağ İzleyicisi başlatılan bir sanal makine uzantısıdır. Bu özellik, bir paket yakalama değerli zaman kazandırır istenen sanal makineye üzerinde el ile çalıştırmayı yükünü kolaylaştırır. Paket yakalama, portal, PowerShell, CLI veya REST API tetiklenebilir. Paket yakalaması nasıl tetiklenebilir bir sanal makine uyarılara örnektir.

#### <a name="ip-flow-verify"></a>IP akışı doğrulama

[IP akışlarını doğrulama](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) paket izin veya için veya 5-tuple bilgilere dayanarak bir sanal makineden reddedildi olup olmadığını denetler. Bu bilgiler yönü, protokol, yerel IP, uzak IP, yerel bağlantı noktası ve uzak bağlantı noktası oluşur. Paket bir güvenlik grubu tarafından reddedilirse, paketi reddeden kuralın adı döndürülür. Herhangi bir kaynak veya hedef IP seçilebilir olsa da bu özelliği, gelen veya internet ve gelen veya şirket içi ortamına bağlantı sorunları çabucak tanılamanızı Yöneticiler yardımcı olur.

IP akışlarını doğrulama hedefleyen bir sanal makinenin ağ arabirimi. Trafik akışı veya ağ arabirimi için yapılandırılan ayarlara dayanan doğrulanır. Bu özellik, bir ağ güvenlik grubu kuralı veya bir sanal makineden girişi veya çıkışı trafiği engelleyip engellemediğini onaylayan olarak yararlıdır.

#### <a name="next-hop"></a>Sonraki atlama

Belirler [sonraki atlama](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) herhangi tanılamanıza olanak sağlayan kullanıcı tanımlı yollar Azure ağ Dokusunda yönlendirilen paketleri için yanlış. Trafiği bir VM'den, bir NIC ile ilişkili geçerli rotalar dayalı bir hedefe gönderilir Sonraki atlama IP adresi için bir paket ve sonraki atlama türü belirli bir sanal makine ve NIC alır Bu paketin hedefine yönelik veya siyah trafiğinin holed belirlemenize yardımcı olur.

Sonraki atlama, ayrıca sonraki atlama ile ilişkili yol tablosuna döndürür. Yol, kullanıcı tanımlı bir yol tanımlanırsa, bir sonraki atlama sorgulanırken yönlendiren döndürülür. Aksi takdirde sonraki atlama "Sistem yolu" döndürür.

#### <a name="security-group-view"></a>güvenlik grubu görünümü

Bir VM'de uygulanan etkili ve uygulanan güvenlik kuralları alır. Ağ güvenlik grupları, bir alt ağ düzeyinde veya bir NIC düzeyinde ilişkilendirilir. Bir alt ağ düzeyinde ilişkilendirilmiş, alt ağdaki tüm VM örnekleri için geçerlidir. Ağ [güvenlik grubu görünümü](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) yapılandırılan Nsg'ler ve yapılandırma hakkında Öngörüler sağlayan bir sanal makine için NIC'te veya alt ağ düzeyinde ilişkilendirilmiş kuralları döndürür. Ayrıca, geçerli güvenlik kuralları, her bir sanal makinede NIC döndürülür. Kullanarak ağ güvenlik grubu görünümü, ağ güvenlik açıklarını açık bağlantı noktaları gibi sanal Makineyi değerlendirebilirsiniz. Siz de doğrulayabilir, ağ güvenlik grubu temel alarak beklendiği gibi çalışıp çalışmadığını bir [yapılandırılmış ve geçerli güvenlik kuralları arasında karşılaştırma](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-auditing-powershell).

#### <a name="nsg-flow-logging"></a>NSG akış günlüğü

 Akış günlüklerini ağ güvenlik grupları için izin verilen veya grubu güvenlik kuralları tarafından engellenen trafik ilgili günlükleri tutmak etkinleştirin. Akış, bir 5 demet bilgi tarafından – kaynak IP, hedef IP, kaynak bağlantı noktası, hedef bağlantı noktası ve protokol tanımlanır.

[Ağ güvenlik grubu akış günlüklerini](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) Ağ İzleyicisi, bir ağ güvenlik grubu üzerinden giriş ve çıkış IP trafiğini hakkındaki bilgileri görüntülemek izin veren bir özelliğidir.

#### <a name="virtual-network-gateway-and-connection-troubleshooting"></a>Sanal ağ geçidi ve bağlantı sorunlarını giderme

Ağ İzleyicisi, ağ kaynaklarınıza azure'da anlamak için bağlantılı olarak çok sayıda özellik sağlar. Bu özelliklerin biri kaynak sorunlarını giderme. [Kaynak sorun giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) PowerShell, CLI veya REST API'si tarafından çağrılabilir. Çağrıldığında, Ağ İzleyicisi, bir sanal ağ geçidi veya bağlantı durumunu inceler ve bulguları döndürür.

Bu bölümde kaynak sorun giderme için şu anda kullanılabilir olan farklı yönetim görevleri alır.

-   [Bir sanal ağ geçidi sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

-   [Bir bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

#### <a name="network-subscription-limits"></a>Ağ aboneliği sınırı

[Ağ aboneliği sınırı](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) her bir Abonelikteki kaynaklar sayısı karşı bir bölgede ağ kaynağı kullanımını ayrıntılarıyla sağlayın.

#### <a name="configuring-diagnostics-log"></a>Tanılama günlüğünü yapılandırma

Ağ İzleyicisi sağlar bir [tanılama günlükleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) görünümü. Bu görünüm, tanılama günlük kaydını destekleyen tüm ağ kaynakları içerir. Bu görünümde, etkinleştirin ve ağ kaynakları kolayca ve hızlı bir şekilde devre dışı bırakın.

### <a name="network-resource-level-monitoring"></a>Ağ kaynak düzeyi izleme

Aşağıdaki özellikler, kaynak düzeyi izleme için kullanılabilir:

#### <a name="audit-log"></a>Denetim günlüğü

Ağ yapılandırmasının bir parçası gerçekleştirilen işlemleri günlüğe kaydedilir. Bu denetim günlükleri çeşitli özellikleri oluşturmak için gereklidir. Bu günlükler, Azure portalında görüntülenebilir veya veya üçüncü taraf araçları gibi Power BI Microsoft araçlarını kullanarak alınır. Denetim günlükleri, portal, PowerShell, CLI ve Rest API kullanılabilir.

> [!Note]
> Denetim günlükleri hakkında daha fazla bilgi için bkz. [Resource Manager denetim işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
Denetim günlükleri, tüm ağ kaynaklarında yapılan işlemler için kullanılabilir.


#### <a name="metrics"></a>Ölçümler

Performans ölçümleri ve bir süre içinde toplanan sayaçları ölçümleridir. Ölçümler için Application Gateway şu anda kullanılabilir. Ölçüm eşiği temel alarak uyarılar tetiklemek için kullanılabilir. Varsayılan olarak Azure Application Gateway, arka uç havuzunda tüm kaynakların izler ve herhangi bir kaynak havuzundan iyi durumda olmadığı kabul otomatik olarak kaldırır. Application Gateway, iyi durumda olmayan örnekler izlemeye devam eder ve kullanılabilir hale gelir ve sistem durumu araştırmaları yanıt sonra bunları sağlıklı arka uç havuzuna ekler. Uygulama ağ geçidi arka uç HTTP Ayarları'nda tanımlanan aynı bağlantı noktası ile sistem durumu araştırmalarının gönderir. Bu yapılandırma, araştırma müşteriler arka ucuna bağlanmak için kullanılmasını aynı bağlantı noktasını sınıyor sağlar.

> [!Note]
> Bkz: [uygulama ağ geçidi tanılama](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview) ölçümleri uyarı oluşturmak için nasıl kullanılabileceğini görmek için.

#### <a name="diagnostic-logs"></a>Tanılama günlükleri

Düzenli ve spontaneous olayları ağ kaynaklar tarafından oluşturulan ve depolama hesaplarında, bir olay Hub'ına gönderilen oturum veya Azure İzleyici günlüğe kaydeder. Bu günlükler bir kaynak durumu hakkında ayrıntılı bilgiler sağlar. Bu günlükler, Power BI ve Azure İzleyici günlükleri gibi Araçları'nda görüntülenebilir. Tanılama günlükleri görüntüleme hakkında bilgi edinmek için [Azure İzleyicisi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

Tanılama günlükleri için kullanılabilir [yük dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), yollar ve [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

Ağ İzleyicisi, bir tanılama günlüklerine yönelik görünüm sağlar. Bu görünüm, tanılama günlük kaydını destekleyen tüm ağ kaynakları içerir. Bu görünümde, etkinleştirin ve ağ kaynakları kolayca ve hızlı bir şekilde devre dışı bırakın.

### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

[Azure İzleyici günlüklerine](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) bir bulut izler ve şirket içi Ortamlarınızdaki kullanılabilirliği ve performansı korumak için bir Azure hizmetidir. Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar.

Azure İzleyici günlüklerine ağlarınızı izlemek için aşağıdaki çözümleri sunar:

-   Ağ Performansı İzleyicisi'ni (NPM)

-   Azure Application Gateway analytics

-   Azure ağ güvenlik grubu analizi

#### <a name="network-performance-monitor-npm"></a>Ağ Performansı İzleyicisi'ni (NPM)
[Ağ Performansı İzleyicisi](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor) yönetim çözümü olan bir ağ izleme sistem durumu, kullanılabilirliği ve ulaşılabilirliği ağların izleyen çözümü.

Arasındaki bağlantıyı izlemek için kullanılır:

-   Genel Bulut ve şirket içi

-   Veri merkezleri ve kullanıcı konumlarında (şubelere)

-   Çok katmanlı bir uygulamanın çeşitli katmanları barındıran alt ağlar.


#### <a name="azure-application-gateway-analytics-in-azure-monitor-logs"></a>Azure İzleyici günlüklerine Azure uygulama ağ geçidi analizi

Uygulama ağ geçitleri için aşağıdaki günlüklere desteklenir:

-   ApplicationGatewayAccessLog

-   ApplicationGatewayPerformanceLog

-   ApplicationGatewayFirewallLog

Aşağıdaki ölçümler Application Gateway'ler için desteklenir:

-   5 dakikalık aktarım hızı

#### <a name="azure-network-security-group-analytics-in-azure-monitor-logs"></a>Azure İzleyici günlüklerine Azure ağ güvenlik grubu analizi

İçin aşağıdaki günlüklere desteklenen [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log):

- **NetworkSecurityGroupEvent:** Vm'lere ve örnek rollerinizin MAC adresini temel alarak için hangi NSG kuralları uygulanır girişler içeriyor. Bu kurallar durumu, 60 saniyede toplanır.

- **NetworkSecurityGroupRuleCounter:** Girişleri için kaç kez trafiğine izin vermek veya reddetmek için uygulanan her bir NSG kuralı içerir.

## <a name="next-steps"></a>Sonraki adımlar
Güvenlik hakkında daha fazla müşterilerimize kapsamlı güvenlik konuları okuyarak öğrenin:

-   [Ağ güvenlik grupları (Nsg'ler) için Azure izleme günlükleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)

-   [Bulut kesintisi sürücü ağ yenilikleri](https://azure.microsoft.com/blog/networking-innovations-that-drive-the-cloud-disruption/)

-   [SONiC: Ağ anahtarı yazılım destek veren Microsoft Genel bulut](https://azure.microsoft.com/blog/sonic-the-networking-switch-software-that-powers-the-microsoft-global-cloud/)

-   [Microsoft, hızlı ve güvenilir bir global ağda nasıl derler](https://azure.microsoft.com/blog/how-microsoft-builds-its-fast-and-reliable-global-network/)

-   [Ağ yeniliğin aydınlatma](https://azure.microsoft.com/blog/lighting-up-network-innovation/)
