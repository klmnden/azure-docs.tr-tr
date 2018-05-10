---
title: Azure ağ güvenliği | Microsoft Docs
description: Geniş işlem örnekleri dahil bulut tabanlı bilgi işlem Hizmetleri & Yukarı ve aşağı otomatik olarak uygulamanızı veya Kurumsal ihtiyaçlarını karşılamak üzere ölçeği hizmetleri hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: 774d678c00b830f3932455c5b79fb44bde284d91
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="azure-network-security"></a>Azure ağ güvenliği

Güvenlik bulutta iş ve nasıl önemli Azure güvenliği hakkında doğru ve güncel bilgi bulma olduğunu olduğunu biliyoruz. Azure'nın çeşitli güvenlik araçları ve yetenekleri yararlanmak için uygulamalar ve hizmetler için Azure kullanmak için en iyi nedenlerinden biridir. Bu araçları ve yetenekleri Azure platformunda güvenli çözümler oluşturmak mümkün kılar yardımcı olur.

Microsoft Azure gizlilik, bütünlük ve müşteri verilerini, kullanılabilirliğini de saydam sorumluluk etkinleştirirken sağlar. Müşteri'nin açısından Microsoft Azure içinde uygulanan ağ güvenlik denetimleri koleksiyonunu daha iyi anlamanıza yardımcı olması için bu makalede, "Azure ağ güvenliği" güvenlik denetimleri ağ kapsamlı bir bakış sağlar yazılır Microsoft Azure ile kullanılabilir.

Bu yazı, çeşitli Azure'da dağıtmak çözümleri güvenliğini yapılandırdığınız ağ denetimleri hakkında bilgilendirmek için tasarlanmıştır. Microsoft Azure platformu ağ dokusu güvenli hale getirmek için yaptığı ilgileniyorsanız, Azure güvenlik bölümüne bakın [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/azure-security).

## <a name="azure-platform"></a>Azure platformu

Azure işletim sistemleri, programlama dilleri, çerçeveleri, Araçlar, veritabanları ve aygıtları geniş çapta destekleyen bir genel bulut hizmeti platformudur.  Docker Tümleştirmesi ile Linux kapsayıcıları çalıştırabilirsiniz; JavaScript, Python, .NET, PHP, Java ve Node.js ile uygulamalar oluşturma; Yapı geri-iOS, Android ve Windows cihazları sona erer. Azure bulut hizmetlerine aynı teknolojileri geliştiriciler milyonlarca destekler ve BT uzmanları zaten kullanır ve güven.

Oluşturmanıza veya BT varlıklarına geçirmek, genel bulut hizmeti sağlayıcı, uygulamalar ve hizmetler ve bulut tabanlı varlıklarınızı güvenliği yönetmek için sağladıkları denetimleri ile verileri korumak için bir kuruluşun yeteneklerini öğesine bağlı.

Azure'nın altyapı tesis aynı anda milyonlarca müşteri barındırmak için uygulamalar için tasarlanmıştır ve güvenlik gereksinimlerine bağlı işletmeler karşılayabilecek güvenilir bir temel sağlar. Ayrıca, Azure ile yapılandırılabilir güvenlik seçenekleri ve kuruluşunuzun dağıtımları benzersiz gereksinimlerini karşılamak için güvenlik özelleştirebilirsiniz böylece bunları denetleme olanağı kapsamlı bir koleksiyon sağlar.

## <a name="abstract"></a>Soyut

Microsoft Genel bulut Hizmetleri hiper ölçekli hizmetler ve altyapı, Kurumsal düzeydeki özellikleri ve karma bağlantı için birçok seçenek sunar. Internet üzerinden veya özel ağ bağlantısı sağlayan Azure ExpressRoute ile bu hizmetlere erişmek seçebilirsiniz. Microsoft Azure platformu, çok katmanlı mimarileri oluşturmak ve sorunsuz bir şekilde altyapınızı buluta genişletmek olanak sağlar. Ayrıca, üçüncü tarafların güvenlik hizmetleri ve sanal gereçler sunarak Gelişmiş özellikleri etkinleştirebilirsiniz.

Azure Ağ Hizmetleri tasarım gereği esneklik, kullanılabilirlik, dayanıklılık, güvenlik ve bütünlüğü ekranı kaplamasını sağlayın. Bu teknik incelemede ağ işlevleri Azure müşterilerin kendi bilgi varlıklarını korumak için Azure'nın yerel güvenlik özellikleri nasıl kullanabileceğiniz bilgileri ve ayrıntılar sağlar.

Bu teknik hedeflenen kitleleri şunları içerir:

- Teknik yöneticileri, ağ yöneticileri ve güvenlik çözümleri kullanılabilir ve desteklenen Azure arıyorsunuz geliştiriciler.

-   SME veya üst düzey bir genel bakış Azure ağ oluşturma teknolojilerini ve ağ güvenliği Azure genel bulutunda tartışmalarını ilgili hizmetleri almak istediğiniz iş işlemi Yöneticiler.

## <a name="azure-networking-big-picture"></a>Büyük resim ağ azure
Microsoft Azure uygulama ve hizmet bağlantı gereksinimlerini desteklemek için sağlam bir ağ altyapısı içerir. Azure'da, şirket içi arasında bulunan kaynaklar arasındaki ağ bağlantısı mümkündür ve Azure barındırılan kaynakları ve kitaplığa ve Internet ve Azure.

![Büyük resim ağ azure](media/azure-network-security/azure-network-security-fig-1.png)

[Azure ağ altyapısı](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) güvenli bir şekilde Azure kaynaklarını birbirlerine sanal ağlar (Vnet'ler) bağlanmanıza olanak sağlar. Bir VNet kendi ağ bulutta gösterimidir. Bir VNet Azure bulut ağ aboneliğinize adanmış mantıksal bir yalıtım ' dir. Şirket içi ağlarınız sanal ağlara bağlanabilir.

Azure destekler, şirket içi ağınız ve Azure sanal ağı ile WAN bağlantısının bağlantı ayrılmış [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). Azure ve sitenizi arasındaki bağlantıyı genel Internet üzerinden geçmez adanmış bir bağlantı kullanır. Azure uygulamanız birden çok veri merkezlerinde çalıştığından, kullanabileceğiniz [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) akıllıca uygulama örnekleri arasında kullanıcılardan rota isteklerine. Ayrıca, Internet'ten erişilebilir olması durumunda Azure üzerinde çalışmayan hizmetlerine trafiğini yönlendirebilir.

## <a name="enterprise-view-of-azure-networking-components"></a>Azure ağ bileşenlerinin Kurumsal görünümü
Azure için ağ güvenlik tartışmalara ilgili birçok ağ bileşenleri içerir. Biz bu ağ bileşenlerini açıklar ve bunlarla ilgili güvenlik sorunları odaklanır.

> [!Note]
> Azure ağı tüm yönlerini açıklanan – yalnızca planlama ve Azure'da dağıtımı uygulamaları ve Hizmetleri geçici güvenli ağ altyapısını tasarlama bileşendirler olduğu kabul aşağıdakiler ele.

Bu yazıda, Kurumsal özellikleri ağ aşağıdaki Azure kapak olacaktır:

-   Temel ağ bağlantısı

-   Karma bağlantı

-   Güvenlik denetimleri

-   Ağ doğrulama

### <a name="basic-network-connectivity"></a>Temel ağ bağlantısı

[Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) hizmeti, güvenli bir şekilde Azure kaynaklarını diğer için sanal ağ (VNet) bağlanmanıza olanak sağlar. Bir VNet kendi ağ bulutta gösterimidir. Bir VNet Azure ağ altyapısının aboneliğinize adanmış mantıksal bir yalıtım ' dir. Sanal ağlar birbirlerine ve siteden siteye VPN kullanarak şirket içi ağlarınız da bağlanabilirsiniz ve ayrılmış [WAN bağlantıları](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![Temel ağ bağlantısı](media/azure-network-security/azure-network-security-fig-2.png)

Ana bilgisayar sunucularına Azure VM kullanmak anlama ile bu VM'lerin bir ağa nasıl bağlanacağını soru olur. Sanal makineleri bağlanma cevaptır bir [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

Azure sanal ağları olan sanal ağlar gibi şirket içi Microsoft Hyper-V veya VMware gibi kendi sanallaştırma platformunu çözümleriyle kullanın.

#### <a name="intra-vnet-connectivity"></a>İçi-VNet bağlantısı

Sanal ağlar birbirlerine Vnet'lerde birbirleri ile iletişim kurmak için herhangi bir Vnet'e bağlı kaynakları etkinleştirme bağlayabilirsiniz. Sanal ağları birbirine bağlama veya her ikisini aşağıdaki seçenekleri kullanabilirsiniz:

- **Eşliği:** farklı Azure birbirleri ile iletişim kurmak için sanal ağlar aynı Azure konumunda içinde bağlı kaynaklar sağlar. Sanal ağ arasında gecikme süresi ve bant genişliği aynıdır kaynaklar aynı Vnet'e bağlıymış gibi. Eşleme hakkında daha fazla bilgi için okuma [sanal ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

 ![Eşleme](media/azure-network-security/azure-network-security-fig-3.png)

- **VNet-VNet bağlantısı:** farklı Azure sanal ağ içinde aynı veya farklı Azure konumu için bağlı kaynaklar sağlar. Bir Azure VPN ağ geçidi üzerinden trafik akışı gerekir çünkü eşliği farklı olarak, bant genişliği sanal ağlar arasında sınırlıdır.

![VNet-VNet bağlantısı](media/azure-network-security/azure-network-security-fig-4.png)


VNet-VNet bağlantısı ile sanal ağlara bağlanma hakkında daha fazla bilgi için okuma [VNet-VNet bağlantı makale yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json).

#### <a name="azure-virtual-network-capabilities"></a>Azure sanal ağ özellikleri:

Gördüğünüz gibi diğer ağ kaynaklarına güvenli bir şekilde bağlanabilmesi ağa bağlanmak için sanal makineleri bir Azure sanal ağı sağlar. Ancak, temel bağlantıyı sadece başlangıçtır. Azure sanal ağ hizmeti aşağıdaki özelliklerini Azure sanal ağ güvenlik özelliklerini kullanır:

-   Yalıtım

-   İnternet bağlantısı

-   Azure kaynak bağlantısı

-   VNet bağlantısı

-   Şirket içi bağlantı

-   Trafik filtreleme

-   Yönlendirme

**Yalıtım**

Sanal ağlar olan [yalıtılmış](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) birbirinden. Aynı ayrı sanal ağlar geliştirme, test ve üretim için oluşturabileceğiniz [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) adres blokları. Buna karşılık, ağları birbirine bağlamak ve farklı CIDR adres bloklarını kullanan birden çok sanal ağlar oluşturabilirsiniz. Bir sanal ağ birden çok alt ağa bölebilirsiniz.

Azure VM'ler için dahili ad çözümlemesi sağlar ve [bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services/) rol örneklerinin bir sanal ağa bağlı. İsteğe bağlı olarak bir VNet Azure dahili ad çözümlemesi kullanmak yerine kendi DNS sunucularını kullanmak için yapılandırabilirsiniz.

Her Azure içinde birden çok sanal ağlar uygulayabilirsiniz [abonelik](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json) ve Azure [bölge](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json). Her sanal ağ, diğer sanal ağlardan yalıtılır. Her sanal ağ için şunları yapabilirsiniz:

-   Genel ve özel (RFC 1918) adresleri kullanarak özel bir gizli IP adresi alanı belirtin. Azure atar kaynakları Vnet'e özel bir IP adresi adres alanından bağlı, atadığınız.

-   Sanal ağ bir veya daha fazla alt ağlara ayırabilir ve her alt ağ için sanal ağ adres alanının bir bölümü ayırın.

-   Azure tarafından sağlanan ad çözümlemesi kullanın veya bir sanal ağa bağlı kaynaklar tarafından kullanmak için kendi DNS sunucusu belirtin. Sanal ağlar ad çözümleme hakkında daha fazla bilgi için okuma [VM'ler ve bulut Hizmetleri için ad çözümlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances).

**İnternet bağlantısı**

Tüm [Azure sanal makineler (VM)](https://docs.microsoft.com/azure/virtual-machines/windows/) ve bir sanal ağa bağlı bulut Hizmetleri rol örnekleri varsayılan olarak Internet erişimi vardır. Belirli kaynaklara gelen erişim gerektiğinde de etkinleştirebilirsiniz. (VM) ve bir sanal ağa bağlı bulut Hizmetleri rol örnekleri varsayılan olarak Internet erişimi vardır. Belirli kaynaklara gelen erişim gerektiğinde de etkinleştirebilirsiniz.

Bir sanal ağa bağlı tüm kaynakları, varsayılan olarak giden Internet bağlantısı vardır. Özel IP adresi kaynağının çevrilmiş kaynak ağ adresine (SNAT) genel bir IP adresi Azure altyapısı tarafından ' dir. Varsayılan bağlantı özel Yönlendirme ve trafik filtreleme uygulayarak değiştirebilirsiniz. Giden Internet bağlantısı hakkında daha fazla bilgi için okuma [azure'da giden bağlantılar anlama](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections?toc=%2fazure%2fvirtual-network%2ftoc.json).

Azure kaynaklarına Internet'ten gelen bağlantı kurmak veya SNAT olmadan Internet'e giden iletişim kurmak için bir kaynak genel bir IP adresi atanması gerekir. Genel IP adresleri hakkında daha fazla bilgi için okuma [ortak IP adresleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address).

**Azure kaynak bağlantısı**

[Azure kaynaklarını](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) gibi bulut Hizmetleri ve sanal makineleri aynı Vnet'e bağlanabilir. Farklı alt ağlarda olsalar bile, kaynakları birbirlerine özel IP adresleri kullanarak bağlanabilir. Azure, yapılandırmak ve yollar yönetmek zorunda kalmamak için varsayılan alt ağlar, sanal ağlar ve şirket içi ağlar arasında yönlendirme sağlar.

Sanal makineler (VM), bulut Hizmetleri, uygulama hizmeti ortamları ve sanal makine ölçek kümeleri gibi bir VNet birkaç Azure kaynaklarına bağlanabilir. Bir alt ağ bir ağ arabirimi (NIC) aracılığıyla bir sanal ağ içindeki VM'ler bağlayın. NIC hakkında daha fazla bilgi için okuma [ağ arabirimleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface).

**VNet bağlantısı**

[Sanal ağlar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) birbirine herhangi bir kaynak başka bir VNet ile iletişim kurmak için hiçbir sanal ağa bağlı kaynaklar etkinleştirme bağlanabilir.

Sanal ağlar birbirlerine Vnet'lerde birbirleri ile iletişim kurmak için herhangi bir Vnet'e bağlı kaynakları etkinleştirme bağlayabilirsiniz. Sanal ağları birbirine bağlama veya her ikisini aşağıdaki seçenekleri kullanabilirsiniz:

- **Eşliği:** farklı Azure birbirleri ile iletişim kurmak için sanal ağlar aynı Azure konumunda içinde bağlı kaynaklar sağlar. Sanal ağlar arasında gecikme süresi ve bant genişliği aynı kaynakları aynı VNet.To bağlıymış gibi hakkında daha fazla bilgi eşliği, okuma [sanal ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).

- **VNet-VNet bağlantısı:** farklı Azure sanal ağ içinde aynı veya farklı Azure konumu için bağlı kaynaklar sağlar. Bir Azure VPN ağ geçidi üzerinden trafik akışı gerekir çünkü eşliği farklı olarak, bant genişliği sanal ağlar arasında sınırlıdır. VNet-VNet bağlantısı ile sanal ağlara bağlanma hakkında daha fazla bilgi edinmek için. Daha fazla bilgi edinmek için okuma [VNet-VNet bağlantı yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json) .

**Şirket içi bağlantı**

Sanal ağlara bağlanabilir [şirket içi](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) ağınız ve Azure arasında özel ağ bağlantıları üzerinden veya Internet üzerinden bir siteden siteye VPN bağlantısı üzerinden ağlar.

Şirket içi ağınıza aşağıdaki seçeneklerden herhangi bir bileşimini kullanarak bir sanal ağa bağlanabilir:

- **Noktadan siteye sanal özel ağ (VPN):** , ağ ve sanal ağ için bağlı tek bir bilgisayar arasında kurulan. Azure’ı kullanmaya yeni başladıysanız bu bağlantı türü mükemmeldir. Mevcut ağınız üzerinde çok az bir değişiklik gerektirdiğinden veya hiç değişiklik gerektirmediğinden geliştiriciler için de mükemmeldir. Bağlantı, PC ve sanal ağ arasında Internet üzerinden şifreli iletişim sağlamak için SSTP protokolünü kullanır. Bir noktadan siteye VPN için gecikme süresini tahmin edilemez olduğu Internet trafiği erişir.

- **Siteden siteye VPN:** VPN cihazınız arasındaki bir Azure VPN ağ geçidi kuruldu. Bu bağlantı türü bir VNet erişmek üzere yetkilendirmek herhangi bir şirket içi kaynak sağlar. Şirket içi Cihazınızı ve Azure VPN ağ geçidi arasında Internet üzerinden şifrelenmiş iletişimi sağlayan bir IPSec/IKE VPN bağlantısıdır. Siteden siteye bağlantı için gecikme süresini tahmin edilemez olduğu Internet trafiği erişir.

- **Azure ExpressRoute:** Bir ExpressRoute iş ortağı aracılığıyla ağınız ile Azure arasında oluşur. Bu bağlantı özeldir. Trafik Internet'e erişmez. ExpressRoute bağlantısı için gecikme süresini tahmin edilebilir olduğu trafik Internet'e çapraz geçiş değil. Önceki tüm bağlantı seçenekleri hakkında daha fazla bilgi için okuma [bağlantı topoloji diyagramları](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

**Trafik filtreleme**

VM ve bulut Hizmetleri rol örnekleri [ağ trafiğini](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) gelen ve giden kaynak IP adresi ve bağlantı noktası, hedef IP adresi ve bağlantı noktası ve protokol göre filtrelenebilir.

Aşağıdaki seçeneklerden birini veya her ikisini de kullanarak alt ağlar arasındaki ağ trafiğini filtreleyebilirsiniz:

- **Ağ güvenlik grubu (NSG):** her NSG trafiğine kaynak ve hedef IP adresi, bağlantı noktası ve protokol göre filtre uygulamak için etkinleştirmeniz birden fazla gelen ve giden güvenlik kuralları içerebilir. Her NIC'nin bir VM için bir NSG uygulayabilirsiniz. Bir NSG'yi bir NIC alt ağına uygulayabilirsiniz veya diğer Azure kaynak bağlı. Nsg'ler hakkında daha fazla bilgi için okuma [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).

- **Sanal ağ uygulamaları:** bir güvenlik duvarı gibi bir ağ işlevi gerçekleştiren yazılımı çalıştıran bir VM sanal ağ Gereci değil. Azure Marketi'nde kullanılabilir NVAs listesini görüntüleyin. NVAs WAN iyileştirmesi ve diğer ağ trafiği işlevleri sağlayan de kullanılabilir durumdadır. NVAs, kullanıcı tanımlı ile genellikle kullanılır ya da BGP yolları. Bir NVA, sanal ağlar arasında trafiği filtrelemek için de kullanabilirsiniz.

**Yönlendirme**

İsteğe bağlı olarak, kendi yolları yapılandırmak veya bir ağ geçidi üzerinden BGP yollarını kullanarak yönlendirme Azure'un varsayılan ayarlarını geçersiz kılabilir.

Azure varsayılan olarak birbirleri ile iletişim kurmak için herhangi bir VNet içindeki herhangi bir alt ağa bağlı kaynaklar etkinleştirmek yönlendirme tabloları oluşturur. Azure’ın oluşturduğu varsayılan rotaları geçersiz kılmak için aşağıdaki seçeneklerden birini veya her ikisini uygulayabilirsiniz:

- **Kullanıcı tanımlı yollar:** özel yol tablolarını burada trafik yönlendirilir için her alt ağ için bu denetim yollar oluşturabilirsiniz. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için okuma [kullanıcı tanımlı yollar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

- **BGP yolları:** bir Azure VPN ağ geçidi veya ExpressRoute bağlantısı kullanarak şirket içi ağınıza ağınızı bağlanırsanız, sanal ağlar için BGP yollarını yayabilir.

### <a name="hybrid-internet-connectivity-connect-to-an-on-premises-network"></a>Karma Internet bağlantısı: bir şirket ağına bağlanma
Şirket içi ağınıza aşağıdaki seçeneklerden herhangi bir bileşimini kullanarak bir sanal ağa bağlanabilir:

-   İnternet bağlantısı

-   Noktadan siteye VPN (P2S VPN)

-   Siteden siteye VPN (S2S VPN)

-   ExpressRoute

#### <a name="internet-connectivity"></a>Internet bağlantısı

Adından da anlaşılacağı gibi Internet bağlantısı iş yüklerinizi Internet'ten erişilebilir, sanal ağ içinde Canlı iş yükleri için farklı ortak uç noktalar kullanıma sağlayarak yapar. Bu iş yükleri kullanarak açığa çıkabileceği [Internet'e yönelik Yük Dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview) veya yalnızca VM için bir ortak IP adresi atama. Bu şekilde, bir ana bilgisayar güvenlik duvarı sağlanan bu sanal makine ulaşabilmesi için Internet üzerindeki herhangi bir şey olanaklı hale [ağ güvenlik grubu (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg), ve [kullanıcı tanımlı rotaları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) için izin ver gerçekleşir.

Bu senaryoda, Internet'e genel ve içinden herhangi bir yere veya iş yüklerinizi yapılandırmasına bağlı olarak belirli konumlardan bağlanabilmek için gereken bir uygulama geçmesine neden olabilir.

#### <a name="point-to-site-vpn-or-site-to-site-vpn"></a>Noktadan siteye VPN veya siteden siteye VPN
Bu iki aynı kategoride yer alır. Her ikisi de bir VPN ağ geçidi için sanal ağınızı gerekir ve ya da kullanarak VPN istemcisi için iş istasyonunuzu parçası olarak bağlayabilirsiniz [noktadan siteye Yapılandırması](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) veya şirket içi yapılandırabilirsiniz [VPN cihazı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices)bir siteden siteye VPN sonlandırma yapabilmek için. Bu şekilde, şirket içi cihazları VNet içindeki kaynaklara bağlanabilir.

Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan bir sanal ağa yönelik güvenli bağlantı oluşturmanıza olanak sağlar. P2S, SSTP (Güvenli Yuva Tünel Protokolü) aracılığıyla gerçekleşen bir VPN bağlantısıdır.

![Noktadan siteye VPN](media/azure-network-security/azure-network-security-fig-5.png)

Uzak bir konumdan gibi giriş veya bir Konferanstan Merkezi'nden bağlanmak istediğinizde ya da yalnızca bir sanal ağa bağlanmak için gereken birkaç istemciniz bulunduğunda noktadan siteye bağlantıları faydalıdır.

P2S bağlantılarının bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. VPN bağlantısını istemci bilgisayardan kurarsınız. Bu nedenle, P2S birçok şirket içi cihazlar ve Bilgisayarları'ndan kalıcı bir bağlantı Azure ağınıza gerektiğinde Azure'a bağlanmak için yol önerilmez.

![Konumdan Konuma VPN](media/azure-network-security/azure-network-security-fig-6.png)

> [!Note]
> Noktadan siteye bağlantılar hakkında daha fazla bilgi için bkz: [noktası siteye FA v Q](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal).

Siteden Siteye VPN ağ geçidi bağlantısı, şirket içi ağınızı bir IPsec/IKE (IKEv1 veya IKEv2) tüneli üzerinden Azure sanal ağına bağlamak için kullanılır.

Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir. Bu bağlantının Internet üzerinden gerçekleşir ve ağınız ve Azure arasında şifrelenmiş bir bağlantısı içinde "tünel" bilgileri sağlar. Siteden siteye VPN ölçekteki kurumların on yılları için dağıtılan güvenli, olgun bir teknolojidir. Tünel şifreleme kullanarak gerçekleştirilir [IPSec tünel modu](https://technet.microsoft.com/library/cc786385.aspx).

Siteden siteye VPN güvenilir, güvenilir ve yerleşik bir teknoloji olsa da, tünel içindeki trafiği Internet çapraz geçiş. Ayrıca, bant genişliği en fazla 200 MB/sn için görece sınırlı değildir.

Şirket içi bağlantılarınız için güvenlik ve performans olağanüstü bir düzeyi gerekiyorsa, şirket içi bağlantılar için Azure ExpressRoute kullanmanızı öneririz. Expressroute'dur ayrılmış WAN şirket içi konumunuz veya bir Exchange barındırma sağlayıcısı arasındaki bağlantı. Bu telco bağlantı olduğundan, verilerinizi Internet üzerinden yolculuk değil ve bu nedenle Internet iletişimlerde devralınmış olası riskleri maruz kalmaz.

> [!Note]
> VPN ağ geçitleri hakkında daha fazla bilgi için bkz: [VPN ağ geçidi](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

#### <a name="dedicated-wan-link"></a>Ayrılmış bir WAN bağlantısı
Microsoft Azure ExpressRoute bağlantı sağlayıcı tarafından kolaylaştırılan adanmış özel bağlantı üzerinden Azure'da şirket içi ağlarınız genişletmenizi sağlar.

ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Bu, ExpressRoute bağlantılarına İnternet üzerindeki sıradan bağlantılara göre daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantılardan daha yüksek güvenlik sağlar.

![ Ayrılmış bir WAN bağlantısı](media/azure-network-security/azure-network-security-fig-7.png)

> [!Note]
> ExpressRoute kullanarak Microsoft ağınıza bağlanma hakkında daha fazla bilgi için bkz: [ExpressRoute bağlantı modelleri](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) ve [ExpressRoute teknik genel bakış](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

Olarak siteden siteye VPN seçeneklerle ExpressRoute mutlaka yalnızca bir VNet içinde olmayan kaynaklara bağlanmak sağlar. Aslında, SKU bağlı olarak 10 sanal ağlara bağlanabilir. Varsa [premium eklentisi](https://docs.microsoft.com/azure/expressroute/expressroute-faqs), en fazla 100 sanal ağlara bağlantılar olası bağlı olarak bant genişliği. Ne gibi bu bağlantıları görünüm türleri hakkında daha fazla bilgi için okuma [bağlantı topoloji diyagramları](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="security-controls"></a>Güvenlik denetimleri
Bir Azure sanal ağı diğer sanal ağlardan yalıtılmış ve şirket içi ağlarınız kullanan çok sayıda güvenlik denetimleri destekleyen güvenli, mantıksal bir ağ sağlar. Müşteriler kullanarak kendi yapısı oluşturun: alt ağlar — bunlar, kendi özel IP adresi aralığı kullanın, yol tablolarını yapılandırmak, ağ güvenlik grubu, erişim denetim listeleri (ACL'ler), ağ geçitleri ve sanal gereçler, iş yüklerini bulutta çalıştırmak için.

Azure sanal ağlarda kullanabilirsiniz güvenlik denetimleri şunlardır:

-   Ağ erişim denetimleri

-   Kullanıcı tanımlı yollar

-   Ağ güvenlik uygulaması

-   Application Gateway

-   Azure Web uygulaması güvenlik duvarı

-   Ağ kullanılabilirlik denetimi

#### <a name="network-access-controls"></a>Ağ erişim denetimleri
Azure sanal ağ (VNet) Azure ağ modeli dönüm ve yalıtım ve koruma sağlar ancak [ağ güvenlik grupları (NSG)](https://blogs.msdn.microsoft.com/igorpag/2016/05/14/azure-network-security-groups-nsg-best-practices-and-lessons-learned/) zorlamak ve ağ trafiği kuralları adresindeki denetlemek için kullandığı ana aracı Ağ düzeyi.

![ Ağ erişim denetimleri](media/azure-network-security/azure-network-security-fig-8.png)


Erişimine izin verme veya reddetme iş yükleri bir sanal ağdan şirket içi bağlantılar aracılığıyla müşterinin ağlarındaki sistemleri arasındaki iletişim tarafından erişimi denetlemek veya Internet iletişimi doğrudan.

Diyagramda, yığındaki Nsg'ler, UDR ve ağ sanal Gereçleri korumalı ağ uygulama dağıtımları korumak için güvenlik sınırları oluşturmak için kullanıldığı Azure genel güvenlik, belirli bir katmandaki sanal ağlar ve Nsg'ler bulunur.

Nsg'ler trafiği değerlendirmek için 5-tanımlama grubu kullanın (ve NSG için yapılandırma kuralları'nda kullanılır):

-   [Kaynak ve hedef IP adresi](https://support.microsoft.com/help/969029/the-functionality-for-source-ip-address-selection-in-windows-server-2008-and-in-windows-vista-differs-from-the-corresponding-functionality-in-earlier-versions-of-windows)

-   [Kaynak ve hedef bağlantı noktası](https://technet.microsoft.com/library/dd197515)

-   Protokol: [İletim Denetimi Protokolü (TCP)](https://technet.microsoft.com/library/cc940037.aspx) veya [kullanıcı veri birimi Protokolü (UDP)](https://technet.microsoft.com/library/cc940034.aspx)

Başka bir deyişle, tüm alt ağlar arasında veya tek bir VM'ye ve bir grup VM'ler veya başka bir tek VM için tek bir VM'ye arasında erişimi denetleyebilirsiniz. Yeniden, bu filtreleme, basit durum bilgisi olan paket olan değil tam paket incelemesi unutmayın. Protokol doğrulama veya ağ düzeyi kimlik veya bir ağ güvenlik grubundaki IP'leri yeteneği yoktur.

Bir NSG'yi farkında olmanız gereken bazı yerleşik kurallar ile birlikte gelir. Bunlar:

-   **Belirli bir sanal ağ içindeki tüm trafiğe izin:** tüm sanal makineleri aynı Azure sanal ağ iletişim kurabilir birbirleri ile.

-   **Azure yük dengeleyici gelen izin ver:** bu kural tüm hedef adresleri için herhangi bir kaynak adresine gelen trafiği için Azure yük dengeleyici sağlar.

-   **Gelenlerin tümünü Reddet:** bu kural açıkça izin verdiğiniz Internet'ten kaynak Hizmeti'nden tüm trafiği engeller.

-   **Tüm trafik Internet'e Gidene izin ver:** bu kural VM'ler Internet bağlantıları başlatmasını sağlar. Bu bağlantılar başlatılan istemiyorsanız, bu bağlantıları engelle veya zorlamalı tünel zorlamak için bir kural oluşturmanız gerekir.

#### <a name="system-routes-and-user-defined-routes"></a>Sistem yollarıyla ve kullanıcı tanımlı yollar

Azure'da sanal bir ağa (VNet) sanal makineleri (VM'ler) eklediğinizde, VM'lerin birbirleri ile ağ üzerinden otomatik olarak iletişim kurabildiklerini olduğuna dikkat edin. VM'ler farklı alt ağlarda bulunsa bile bir ağ geçidini belirtmenize gerek yoktur.

Aynı şey VM'lerden genel İnternet'e giden iletişimlerde ve hatta Azure'dan kendi veri merkezinize karma bir bağlantı bulunduğunda şirket içi ağınıza giden iletişimlerde de geçerlidir.

![Sistem Yolları](media/azure-network-security/azure-network-security-fig-9.png)

Bu iletişim akışının mümkün olmasını sağlayan şey, Azure'ın IP trafiğinin nasıl akacağını belirlemek için kullandığı bir dizi sistem yoludur. Sistem yolları aşağıdaki senaryolarda iletişim akışını denetler:

-   Aynı alt ağ içinden.

-   Bir sanal ağ içinde bir alt ağdan başka bir alt ağa.

-   VM'lerden İnternet'e.

-   Bir VPN ağ geçidi yoluyla bir sanal ağdan başka bir sanal ağa.

-   VNet-VNet eşlemesi aracılığıyla başka bir VNet gelen ([hizmet zincirleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)).

-   Bir VPN ağ geçidi yoluyla bir sanal ağdan şirket içi ağınıza.

Çoğu işletmenin sıkı güvenlik varsa ve şirket içi denetleme belirli zorlamak için tüm ağ paketlerinin gerektiren uyumluluk gereksinimlerine ilkeleri. Azure adlı bir mekanizma sağlar [zorlanan tünel](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-forced-tunneling) özel bir yol oluşturarak veya şirket içi sanal makineleri giden trafiği yönlendirmeleri [sınır ağ geçidi Protokolü (BGP)](https://docs.microsoft.com/windows-server/remote/remote-access/bgp/border-gateway-protocol-bgp) ExpressRoute aracılığıyla reklamları Arama veya VPN.

Zorlamalı tünel Azure'da sanal ağ kullanıcı tanımlı yolları (UDR) yapılandırılır. Bir şirket içi siteye trafiği yönlendirerek Azure VPN ağ geçidi için varsayılan bir yol olarak ifade edilir.

Aşağıdaki bölümde, bir Azure sanal ağı için yönlendirme tablosunu ve yollar geçerli sınırlaması listelenmektedir:

-   Her sanal ağ alt yerleşik sistem yönlendirme tablosu vardır. Sistem yönlendirme tablosu yolların aşağıdaki üç grup vardır:

 -  **Yerel VNet yollar:** doğrudan hedefe Vm'leri aynı sanal ağda

 - **Şirket içi rota:** için Azure VPN ağ geçidi

 -  **Varsayılan yol:** doğrudan Internet'e. Önceki iki yoldan tarafından kapsanmayan özel IP adreslerine hedeflenen paketlerin bırakılır.

-   Kullanıcı tanımlı yollar sürümle birlikte, varsayılan yol eklemek üzere bir yönlendirme tablosu oluşturun ve ardından bu alt ağlardaki zorlamalı tüneli etkinleştirmek için sanal ağ alt ağına yönlendirme tablosu ilişkilendirin.

-   "Sanal ağa bağlı bir varsayılan site" şirket içi yerel siteleri arasında ayarlamanız gerekir.

-   Zorlamalı tünel dinamik yönlendirme VPN ağ geçidi (olmayan bir statik ağ geçidi) sahip bir sanal ağ ile ilişkilendirilmiş olması gerekir.

- Zorlanan tünel ExpressRoute bu düzenek yapılandırılmamış, ancak bunun yerine, varsayılan rota ExpressRoute BGP eşliği oturumlarını üzerinden reklam tarafından etkinleştirilir.

> [!Note]
> Daha fazla bilgi için bkz: [ExpressRoute belgeleri](https://azure.microsoft.com/documentation/services/expressroute/) daha fazla bilgi için.

#### <a name="network-security-appliances"></a>Ağ güvenlik uygulamaları
Ağ güvenlik grupları ve kullanıcı tanımlı rotaları belirli bir ölçü ağ güvenlik ağ ve Aktarım katmanı en sağlayabilir sırada [OSI modeli](https://en.wikipedia.org/wiki/OSI_model), var olan geçmeye istediğiniz veya güvenlik sağlamak için ihtiyacınız olduğu durumlar olabilir ağ yığınını daha yüksek düzeyde. Böyle durumlarda, Azure iş ortakları tarafından sağlanan sanal ağ güvenlik Gereçleri dağıtmanızı öneririz.

![Ağ güvenlik uygulamaları](./media/azure-network-security/azure-network-security-fig-10.png)

VNet güvenlik ve ağ işlevlerini Azure ağ güvenliği uygulamaları geliştirmek ve çok sayıda satıcılardan kullanılabilir [Azure Marketi](https://azuremarketplace.microsoft.com). Bu sanal güvenlik Gereçleri sağlamak için dağıtılabilir:

-   Yüksek oranda kullanılabilir güvenlik duvarları

-   Yetkisiz erişim önleme

-   Yetkisiz erişim algılama

-   Web uygulaması Güvenlik Duvarı (WAFs)

-   WAN iyileştirmesi

-   Yönlendirme

-   Yük dengeleme

-   VPN

-   Sertifika yönetimi

-   Active Directory

-   Çok faktörlü kimlik doğrulaması

#### <a name="application-gateway"></a>Uygulama ağ geçidi

[Microsoft Azure uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) uygulama teslim denetleyicisi (ADC) bir hizmet olarak sunar ayrılmış bir sanal gereç olduğu.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-11.png)

Uygulama ağ geçidi, web grubu performans ve kullanılabilirlik CPU yoğunluklu SSL sonlandırma uygulama ağ geçidi (SSL boşaltma) boşaltarak en iyi duruma getirme sağlar. Ayrıca, dahil olmak üzere diğer katman 7 Yönlendirme yetenekleri sağlar:

-   Gelen trafiğin hepsini dağıtım

-   Tanımlama bilgisi tabanlı oturum benzeşimi

-   URL yolu tabanlı yönlendirme

-   Tek bir uygulama ağ geçidi arkasında birden çok Web sitesini barındırmak için özelliği


A [web uygulaması Güvenlik Duvarı (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) de uygulama ağ geçidi bir parçası olarak sağlanır. Bu koruma web uygulamaları için ortak web Güvenlik Açıkları ve açıkları sağlar. Uygulama ağ geçidi, ağ geçidi, iç yalnızca ağ geçidi veya her ikisinin birleşimini Internet'e yönelik yapılandırılabilir.

Uygulama ağ geçidi WAF algılama veya önleme modda çalıştırabilirsiniz. Kötü amaçlı düzenleri trafiğini izlemek için algılama modunda çalıştırmak Yöneticiler için ortak kullanım durumu gösterir. Olası açıkları algılanan sonra önleme moduna kapatma şüpheli gelen trafiği engeller.

 ![Application Gateway](./media/azure-network-security/azure-network-security-fig-12.png)

Ayrıca, uygulama ağ geçidi WAF web uygulamaları ile tümleşik gerçek zamanlı WAF günlüğünü kullanarak saldırılarına karşı izlemenize yardımcı [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) WAF uyarıları izlemek için ve kolayca eğilimlerini izleyin.

JSON biçimli günlük doğrudan Müşteri'nin depolama hesabına gider. Bu günlükler üzerinde tam denetime sahiptir ve kendi bekletme ilkeleri uygulayabilirsiniz.

Kullanarak kendi analytics sistem Bu günlükler işleyebilen [Azure günlük tümleştirme](https://aka.ms/AzLog). WAF günlükleri ile tümleşik de [günlük analizi](../log-analytics/log-analytics-overview.md) Gelişmiş hassas sorgularını yürütmek için günlük analizi kullanabilirsiniz.

#### <a name="azure-web-application-firewall-waf"></a>Azure web uygulaması Güvenlik Duvarı (WAF)

Web uygulamaları olan giderek SQL ekleme, siteler arası komut dosyası saldırıları ve görünen diğer saldırıları gibi ortak bilinen güvenlik açıklarından faydalanır kötü amaçlı saldırıları hedefleri [OWASP ilk 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project). Uygulamada bu tür açıkları önleme düzeltme eki uygulama ve uygulama topolojisinin birden çok katmanına izleme sıkı bakım gerektirir.

 ![Azure Web uygulaması Güvenlik Duvarı (WAF)](./media/azure-network-security/azure-network-security-fig-13.png)

Bir merkezi [web uygulaması Güvenlik Duvarı (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) web saldırılarına karşı korumak ve uygulama değişiklikleri gerek kalmadan güvenlik yönetimini basitleştirir.

Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.

#### <a name="network-availability-controls"></a>Ağ kullanılabilirliğini denetimleri

Microsoft Azure’u kullanarak ağ trafiğini dağıtmak için farklı seçenekler bulunur. Bu seçenekler birbirlerinden farklı şekilde çalışır, farklı özelliklere sahiptir ve farklı senaryoları destekler. Birbirlerinden ayrı olarak veya birleştirilerek kullanılabilirler.

Ağ kullanılabilirliğini denetimleri şunlardır:

-   Azure Load Balancer

-   Application Gateway

-   Traffic Manager

**Azure yük dengeleyici**

Yüksek kullanılabilirlik ve ağ performansı uygulamalarınıza sunar. Gelen trafiği yükü dengelenmiş bir kümede tanımlanmış Hizmetleri sağlıklı örnekleri arasında dağıtır bir katman 4 (TCP, UDP) yük dengeleyici olur.

 ![Azure Load Balancer](media/azure-network-security/azure-network-security-fig-14.png)


Azure yük dengeleyici için yapılandırılabilir:

-   Gelen Internet trafiği dengelemek için sanal makineler yükleyin. Bu yapılandırma olarak bilinir [Internet'e yönelik Yük Dengeleme](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Yük Dengeleme trafiği sanal ağ içindeki sanal makineler arasında bulut Hizmetleri içindeki sanal makineler arasında veya şirket içi bilgisayarlar ve bir şirket içi sanal ağdaki sanal makineler arasında. Bu yapılandırma olarak bilinir [iç Yük Dengeleme](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview).

-   Belirli bir sanal makine için dış trafiğin iletin.

Buluttaki tüm kaynaklara Internet'ten erişilebilir olması için bir ortak IP adresi gerekir. Azure bulut altyapısında kaynaklarını için yönlendirilebilir olmayan IP adreslerini kullanır. Azure Internet ile iletişim kurmak için ortak IP adresi ile ağ adresi çevirisi (NAT) kullanır.

 **Uygulama ağ geçidi**

 Uygulama ağ geçidi (OSI Ağ başvurusu yığınında katman 7) uygulama katmanında çalışır. İstemci bağlantısını sonlandıran ve istekleri arka uç noktalarına ileten ters proxy hizmeti olarak çalışır.

 **Trafik Yöneticisi**

Microsoft Azure trafik Yöneticisi, farklı veri merkezlerinde bulunan hizmet uç noktaları için kullanıcı trafiğinin dağıtımını denetlemenize olanak sağlar. Trafik Yöneticisi tarafından desteklenen hizmet uç noktaları ve bulut hizmetlerini Azure VM'ler, Web uygulamaları içerir. Traffic Manager’ı harici, Azure dışı uç noktalar için de kullanabilirsiniz.

Trafik yöneticisi etki alanı adı sistemi (DNS) istemci isteklerini göre en uygun uç noktasına yönlendirmek için kullandığı bir [trafik yönlendirme yöntemini](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) ve bitiş noktalarının sistem durumu. Trafik Yöneticisi uç noktası sistem durumu, farklı uygulama gereksinimlerinde uyacak şekilde trafik yönlendirme yöntemleri bir dizi sağlar [izleme](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring)ve otomatik yük devretme. Trafik Yöneticisi dahil tüm bir Azure bölgesi başarısızlığını hatalarına karşı esnektir.

Azure trafik Yöneticisi, uygulama uç noktalar arasında trafiğinin dağıtımını denetlemenize olanak sağlar. Bir uç nokta içinde veya Azure dışında barındırılan tüm Internet'e hizmetidir.

Trafik Yöneticisi iki temel fayda sağlar:

-   Bazı göre trafiğinin dağıtımını [trafik yönlendirme yöntemleri](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods).

-   [Sürekli uç noktası durumunu izleme](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring) ve uç noktaları başarısız olduğunda otomatik yük devretme.

Bir istemci bir hizmete bağlanma girişiminde bulunduğunda, hizmetin DNS adını bir IP adresi ilk çözmeniz gerekir. İstemci ardından hizmete erişmek için bu IP adresine bağlanır. Trafik Yöneticisi DNS istemcileri belirli hizmet uç noktalarına trafik yönlendirme yöntemini kurallara göre yönlendirmek için kullanır. İstemcileri seçili uç noktasına doğrudan bağlanın. Trafik Yöneticisi, bir proxy veya ağ geçidi değil. Trafik Yöneticisi, istemci ile hizmet arasında geçen trafiğine görmez.

### <a name="azure-network-validation"></a>Azure ağı doğrulama

Azure ağı doğrulama olan Azure ağı olarak yapılandırıldığından ve doğrulama yapılabilir işlediğinden emin olmak için hizmetleri ve Özellikler Ağ İzleyicisi'ni kullanarak. Azure Ağ İzleyicisi ile günlük sayısız erişebilir ve ağ performansı ve sistem durumu anlamak için ınsights ile güç kazandırma tanılama yetenekleri. Bu özellikler, Portal, Power Shell, CLI, Rest API ve SDK erişilebilir.

Azure işletimsel güvenlik hizmetleri, denetimleri ve kullanıcılar için kullanılabilir özellikler verilerini, uygulamaları ve diğer Microsoft Azure varlıkları korumak için ifade eder. Azure işlem güvenliği Microsoft Security Development Lifecycle (SDL), Microsoft Güvenlik Yanıt Merkezi programı da dahil olmak üzere Microsoft'a özgü çeşitli özellikleri aracılığıyla elde edilen bilgilerden içerir çerçevesi üzerine inşa edilmiştir ve siber güvenlik tehdit derin farkındalığınızı.

-   [Azure Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)

-   [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)

-   [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

-   [Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

-   [Azure depolama çözümlemeleri](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

-   Azure Resource Manager

#### <a name="azure-resource-manager"></a>Azure Kaynak Yöneticisi

Microsoft Azure çalışan işlemleri ve insanlara belki de en önemli güvenlik platformun özelliğidir. Bu bölümde geliştirmek ve güvenlik, sürekliliği ve gizlilik korumanıza yardımcı Microsoft'un küresel veri merkezi altyapı özelliklerini açıklar.

Uygulamanızın altyapısı genellikle birçok bileşenlerinin – belki de bir sanal makine, depolama hesabı ve sanal ağ veya bir web uygulaması, veritabanı, veritabanı sunucusu ve üçüncü taraf hizmetleri oluşur. Bu bileşenleri ayrı varlıklar olarak değerlendirmez, bunun yerine bunları tek bir varlığın ilgili ve birbirine bağımlı parçaları olarak kabul edersiniz. Bunları gruplar halinde dağıtmak, yönetmek ve izlemek isteyebilirsiniz. Azure Resource Manager, çözümünüzdeki kaynaklar ile gruplar halinde çalışmanıza olanak sağlar.

Çözümünüzdeki tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilirsiniz. Dağıtım için bir şablon kullanabilirsiniz. Üstelik bu şablon test, hazırlık ve üretim gibi farklı ortamlarda da çalışabilir. Resource Manager kaynaklarınızı dağıttıktan sonra yönetmenize yardımcı olmak için güvenlik, denetleme ve etiketleme özellikleri sunar.

**Resource Manager kullanmanın avantajları**

Resource Manager çeşitli avantajlar sunar:

-   Çözümünüzdeki tüm kaynakları ayrı ayrı ele almak yerine bunları grup halinde dağıtabilir, yönetebilir ve izleyebilirsiniz.

-   Çözümünüzü geliştirme yaşam döngüsü boyunca defalarca dağıtabilirsiniz. Kaynaklarınızın tutarlı bir durumda dağıtılması da size güven verir.

-   Altyapınızı betikler yerine bildirim temelli şablonları kullanarak yönetebilirsiniz.

-   Doğru sırayla dağıtılmalarını sağlamak kaynaklarınız arasındaki bağımlılıkları tanımlayabilirsiniz.

-   Rol Tabanlı Erişim Denetimi (RBAC) yönetim platformuyla doğrudan tümleşik olduğu için kaynak grubunuzdaki tüm hizmetlere erişim denetimi uygulayabilirsiniz.

-   Aboneliğinizdeki tüm kaynakları mantıksal olarak düzenlemek için kaynaklarınıza etiketler ekleyebilirsiniz.

-   Etiketi paylaşan kaynak grubu için maliyetleri görüntüleyerek kuruluşunuzun fatura açıklık getirebilir.

> [!Note]
> Resource Manager çözümlerinizi dağıtmanın ve yönetmenin yeni bir yolunu sunar. Önceki dağıtım modelini kullandıysanız ve değişiklikler hakkında bilgi edinmek isterseniz, bkz. [Resource Manager dağıtımını ve klasik dağıtımı anlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model)

## <a name="azure-network-logging-and-monitoring"></a>Azure ağı günlüğe kaydetme ve izleme

Azure izlemek, engellemenize, algılamanıza ve ağ güvenlik olaylarına yanıt için birçok araçlar sunar. Bu alanda kullanılabilir en güçlü araçlardan bazıları şunlardır:

-   Ağ İzleyicisi

-   Ağ kaynak düzeyi izleme

-   Log Analytics

### <a name="network-watcher"></a>Ağ izleyicisi

[Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) -senaryo tabanlı izleme Ağ İzleyicisi'deki özelliklerle sağlanır. Bu hizmet içeren paket yakalama, sonraki atlama IP akış, güvenlik grubu görünümü, NSG akış günlükleri doğrulayın. Senaryo düzeyi izleme kaynak tek tek ağ izleme aksine ağ kaynaklarına bir uçtan uca görünümünü sağlar.

 ![Ağ İzleyicisi](./media/azure-network-security/azure-network-security-fig-15.png)

Ağ İzleyicisi İzleme ve koşullar bir ağ düzeyinde senaryo içinde gelen ve giden Azure tanılama sağlayan bölgesel bir hizmettir. Ağ Tanılama ve görselleştirme Ağ İzleyicisi ile kullanılabilen araçlar anlamak, tanılama ve Azure ağınızdaki serisidir yardımcı olur.

Ağ İzleyicisi'ni şu anda aşağıdaki özellikleri içerir:

#### <a name="topology"></a>Topoloji

[Topoloji](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) bir sanal ağda ağ kaynaklarının bir grafik döndürür. Grafikte uçtan uca ağ bağlantısını temsil etmek için kaynakları arasındaki bağlantısı gösterilmektedir. Portalda, topoloji kaynak nesneleri sanal ağ temeli başına döndürür. Kaynak grubu görüntülenmeyecek olsa bile Ağ İzleyicisi bölgesi dışında kaynaklar arasındaki çizgilerle ilişkileri gösterilen. Portal görünümünde döndürülen kaynakları grafiği çizilecek ağ bileşenleri bir alt kümesidir. Ağ kaynakları tam listesini görmek için kullanabileceğiniz [PowerShell](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-powershell) veya [REST](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-rest).

Kaynakları döndürülen bunlar arasındaki bağlantıyı Modellenen altında iki ilişki.

- **Kapsama** -sanal ağ, bir NIC içeren bir alt ağ içerir

- **İlişkili** -A NIC, bir VM ile ilişkilendirilmiş.

#### <a name="variable-packet-capture"></a>Değişken paket yakalama

Ağ İzleyicisi [değişken paket yakalama](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) bir sanal makine gelen ve giden trafiği izlemek için paket yakalama oturumları oluşturmanıza olanak sağlar. Her iki ağ anormallikleri Tepkisel tanılamak için paket Yakalama yardımcı olur ve proactivity. Diğer kullanımlar ağ yetkisiz erişim, istemci-sunucu iletişimleri ve çok daha fazlasını hata ayıklamak için bilgi sağlamasını ağ istatistikleri toplama içerir.

Paket yakalama Ağ İzleyicisi uzaktan başlatılan bir sanal makine uzantısıdır. Bu özellik, bir paket yakalama değerli zaman kazandırır istenen sanal makinesinin üzerinde el ile çalıştırmayı yükünü kolaylaştırır. Paket yakalama portal, PowerShell'i, CLI veya REST API tetiklenebilir. Paket yakalama nasıl tetiklenebilir bir sanal makine uyarılara örnektir.

#### <a name="ip-flow-verify"></a>IP akış doğrulayın

[IP akışları doğrulayın](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) bir paket izin verilen veya için veya 5-tanımlama bilgilerine dayalı bir sanal makineden reddedildi olup olmadığını denetler. Bu bilgiler yönü, protokol, yerel IP, uzak IP, yerel bağlantı noktası ve uzak bağlantı noktası oluşur. Paket bir güvenlik grubu tarafından engellenirse Paket reddedildi kuralının adı döndürülür. Kaynak veya hedef IP seçilebilir olsa da, bu özellik hızla gelen veya Internet ve gelen veya şirket içi ortamına bağlantı sorunları tanılamak yöneticilerin yardımcı olur.

IP akışları doğrulayın hedefleyen bir sanal makinenin ağ arabirimi. Trafik akışı için ya da bu ağ arabirimini yapılandırılan ayarlara göre doğrulanır. Bu özellik, bir ağ güvenlik grubu kural giriş ve çıkış trafiği için veya bir sanal makineden durumunda engelliyor onaylayan yararlıdır.

#### <a name="next-hop"></a>Sonraki atlama

Belirler [sonraki atlama](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) Azure ağ yapıda yönlendirilen paketler için herhangi bir tanılamak etkinleştirme kullanıcı tanımlı yollar yanlış yapılandırılmış. Bir VM'ye gelen trafiği bir NIC ile ilişkili etkili rotalarını dayalı bir hedefe gönderilir Sonraki atlama IP adresini bir paket ve sonraki atlama türü bir belirli bir sanal makine ve NIC alır Bu paketin hedef yönlendirilmiş veya siyah olan trafik holed belirlemeye yardımcı olur.

Sonraki atlama ayrıca sonraki atlama ile ilişkili yol tablosu döndürür. Varsa rota kullanıcı tanımlı bir yol olarak tanımlanan bir sonraki atlama sorgularken bu rota döndürülür. Aksi takdirde sonraki atlama "Sistem yolu" döndürür.

#### <a name="security-group-view"></a>Güvenlik grubu görünümü

Bir VM üzerinde uygulanan etkili ve uygulanan güvenlik kuralları alır. Ağ güvenlik grupları, bir alt ağ düzeyinde veya bir NIC düzeyi ilişkilendirilir. Bir alt düzeyde ilişkili olduğunda, alt ağdaki tüm VM örnekleri için geçerlidir. Ağ [güvenlik grubu görünümü](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) yapılandırılan Nsg'ler ve yapılandırma hakkında bilgi sağlayan bir sanal makine için bir NIC ve alt ağ düzeyinde ilişkili kurallarını döndürür. Ayrıca, her bir VM NIC'ler için etkili güvenlik kuralları döndürülür. Ağ güvenlik grubu kullanarak görünüm açık bağlantı noktaları gibi ağ güvenlik açıkları için bir VM değerlendirebilirsiniz. De doğrulamak, ağ güvenlik grubu temel alarak beklendiği gibi çalışıp çalışmadığını bir [yapılandırılmış ve etkin güvenlik kuralları arasında karşılaştırma](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-auditing-powershell).

#### <a name="nsg-flow-logging"></a>NSG akış günlüğe kaydetme

 Akış günlükleri ağ güvenlik grupları için izin verilen ya da grubu güvenlik kuralları tarafından reddedilen trafiği ilgili günlükleri yakalamanıza olanak sağlar. Akış bir 5-tanımlama grubu bilgileriyle – kaynak IP, hedef IP, kaynak bağlantı noktası, hedef bağlantı noktası ve protokol tanımlanır.

[Ağ güvenlik grubu akış günlükleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) giriş ve çıkış IP trafiği bir ağ güvenlik grubu ile ilgili bilgileri görüntülemek izin veren bir Ağ İzleyicisi bir özelliğidir.

#### <a name="virtual-network-gateway-and-connection-troubleshooting"></a>Sanal ağ geçidi ve bağlantı sorunlarını giderme

Ağ kaynaklarınızı Azure anlamak için ilgili olarak Ağ İzleyicisi çok sayıda özellik sağlar. Bu özelliklerin biri kaynak sorun giderme. [Kaynak sorun giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) PowerShell'i, CLI veya REST API tarafından çağrılabilir. Çağrıldığında, Ağ İzleyicisi bir sanal ağ geçidi veya bağlantı durumunu inceler ve bulduklarını döndürür.

Bu bölüm kaynak sorun giderme için şu anda kullanılabilir farklı yönetim görevleri yoluyla alır.

-   [Bir sanal ağ geçidi sorun giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

-   [Bir bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

#### <a name="network-subscription-limits"></a>Ağ abonelik sınırları

[Ağ abonelik sınırları](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) her bir abonelikte kullanılabilir kaynakları sayısı karşı bir bölgede ağ kaynağı kullanımını ayrıntılarını sağlar.

#### <a name="configuring-diagnostics-log"></a>Tanılama günlük yapılandırma

Ağ İzleyicisi sağlayan bir [tanılama günlüklerini](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) görünümü. Bu görünüm, tanılama günlüğünün destekleyen tüm ağ kaynaklarını içerir. Bu görünümden etkinleştirin ve ağ kaynaklarını kolayca ve hızlı bir şekilde devre dışı bırakın.

### <a name="network-resource-level-monitoring"></a>Ağ kaynak düzeyi izleme

Aşağıdaki özellikler, kaynak düzeyi izleme için kullanılabilir:

#### <a name="audit-log"></a>Denetleme günlüğü

Ağ yapılandırmasının bir parçası gerçekleştirilen işlemleri günlüğe kaydedilir. Bu denetim günlükleri çeşitli compliances kurmak için gereklidir. Bu günlükler Azure Portalı'nda görüntülenebilir veya Power BI gibi Microsoft araçları veya üçüncü taraf araçlarını kullanarak alınamıyor. Denetim günlükleri, portal, PowerShell'i, CLI ve Rest API kullanılabilir.

> [!Note]
> Denetim günlükleri hakkında daha fazla bilgi için bkz: [denetim işlemleri Resource Manager ile](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
Denetim günlükleri, tüm ağ kaynaklarına yapılan işlemleri için kullanılabilir.


#### <a name="metrics"></a>Ölçümler

Performans ölçümleri ve belirli bir süre boyunca toplanan sayaçları ölçümleridir. Ölçümleri uygulama ağ geçidi için şu anda kullanılabilir. Ölçümleri eşiğine dayalı uyarılar tetiklemek için kullanılabilir. Varsayılan olarak Azure uygulama ağ geçidi arka uç havuzundaki tüm kaynakların durumunu izler ve herhangi bir kaynak havuzundan sağlıksız kabul otomatik olarak kaldırır. Uygulama ağ geçidi düzgün çalışmayan örnekleri izlemek devam eder ve kullanılabilir hale gelir ve sistem durumu araştırmalarının yanıt sonra bunları sağlıklı arka uç havuzuna ekler. Uygulama ağ geçidi arka uç HTTP Ayarları'nda tanımlanan aynı bağlantı noktası ile sistem durumu araştırmalarının gönderir. Bu yapılandırma, araştırma müşteriler arka ucuna bağlanmak için kullanılmasını aynı bağlantı noktasını test ediyor sağlar.

> [!Note]
> Bkz: [uygulama ağ geçidi tanılama](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview) ölçümleri uyarıları oluşturmak için nasıl kullanılabileceğini görüntülemek için.

#### <a name="diagnostic-logs"></a>Tanılama günlükleri

Dönemsel ve spontaneous olayları ağ kaynakları tarafından oluşturulan ve bir olay hub'ı veya günlük analizi için gönderilen depolama hesaplarındaki günlüğe. Bu günlükleri bir kaynak sistem durumu fikir sağlar. Bu günlükler Power BI ve günlük analizi gibi araçları görüntülenebilir. Tanılama günlükleri görüntüleme konusunda bilgi için [günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

Tanılama günlükleri için kullanılabilir [yük dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), yollar ve [uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

Ağ İzleyicisi görünümü bir tanılama günlükleri sağlar. Bu görünüm, tanılama günlüğünün destekleyen tüm ağ kaynaklarını içerir. Bu görünümden etkinleştirin ve ağ kaynaklarını kolayca ve hızlı bir şekilde devre dışı bırakın.

### <a name="log-analytics"></a>Log Analytics

[Günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) bir bulut izler ve şirket içi ortamları kendi kullanılabilirliğini ve performansını korumak için Azure hizmetidir. Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar.

Günlük analizi, ağları izleme için aşağıdaki çözümleri sunar:

-   Ağ Performans İzleyicisi'ni (NPM)

-   Azure uygulama ağ geçidi analizi

-   Azure ağ güvenlik grubu analizi

#### <a name="network-performance-monitor-npm"></a>Ağ Performans İzleyicisi'ni (NPM)
[Ağ Performansı İzleyicisi](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor) yönetimi çözümüdür ağ durumu, kullanılabilirliği ve ulaşılabilirliği ağların izler çözüm izleme.

Arasında bağlantı izlemek için kullanılır:

-   Genel Bulut ve şirket içi

-   veri merkezleri ve kullanıcı konumları (şubelere)

-   çok katmanlı bir uygulama çeşitli katmanlarını barındırma alt ağlar.


#### <a name="azure-application-gateway-analytics-in-log-analytics"></a>Azure uygulama ağ geçidi analizleri günlük analizi

Günlükleri, uygulama ağ geçitleri için desteklenir:

-   ApplicationGatewayAccessLog

-   ApplicationGatewayPerformanceLog

-   ApplicationGatewayFirewallLog

Aşağıdaki ölçümleri uygulama ağ geçitleri için desteklenir:

-   5 dakikalık işleme

#### <a name="azure-network-security-group-analytics-in-log-analytics"></a>Azure ağ güvenlik grubu analizleri günlük analizi

Günlükleri için desteklenen [ağ güvenlik grubu](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log):

- **NetworkSecurityGroupEvent:** hangi NSG kuralları Vm'lere uygulanan ve örnek MAC adresine dayalı rolleri girişleri içerir. Bu kurallar durumunun her 60 saniyede toplanır.

- **NetworkSecurityGroupRuleCounter:** kaç kez her NSG için içerir girişleri kural reddetmek veya trafiğine izin vermek üzere uygulanır.

## <a name="next-steps"></a>Sonraki adımlar
Güvenlik hakkında daha fazla bizim kapsamlı güvenlik konuların bazıları okuyarak bulun:

-   [Ağ güvenlik grupları (Nsg'ler) için günlük analizi](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)

-   [Bulut kesintisi sürücü ağ yenilikleri](https://azure.microsoft.com/blog/networking-innovations-that-drive-the-cloud-disruption/)

-   [SONiC: Ağ geçiş yazılım bu powers Microsoft Genel bulut](https://azure.microsoft.com/blog/sonic-the-networking-switch-software-that-powers-the-microsoft-global-cloud/)

-   [Microsoft, hızlı ve güvenilir bir genel ağ nasıl oluşturulur](https://azure.microsoft.com/blog/how-microsoft-builds-its-fast-and-reliable-global-network/)

-   [Aydınlatma ağ yenilik ayarlama](https://azure.microsoft.com/blog/lighting-up-network-innovation/)
