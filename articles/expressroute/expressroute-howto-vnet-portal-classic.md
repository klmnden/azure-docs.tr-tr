---
title: "Bir sanal ağ ve ağ geçidi Klasik portalda ExpressRoute için yapılandırma | Microsoft Docs"
description: "Bu makalede Klasik dağıtım modeli ve klasik Portalı'nı kullanarak ExpressRoute için bir sanal ağ ayarı aracılığıyla anlatılmaktadır."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 688817d9-51c8-4372-9af8-33fa098c7c5a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2016
ms.author: cherylmc
ms.openlocfilehash: f62254b2a7df50aa55a2a49009702848a9aecebd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Klasik portalda ExpressRoute için bir sanal ağ oluşturma
Bu makaledeki adımları sanal ağ ve sanal ağ geçidi kullanmak için Klasik dağıtım modeli ve klasik Portalı'nı kullanarak ExpressRoute ile nasıl yapılandıracağınız anlatılmaktadır.

Resource Manager dağıtım modeli için yönergeler arıyorsanız, aşağıdaki makaleler kullanabilirsiniz: [PowerShell kullanarak bir sanal ağ oluşturma](../virtual-network/virtual-networks-create-vnet-arm-ps.md) ve [Resource Manager Vnet'i ExpressRoute için bir VPN ağ geçidi eklemek](expressroute-howto-add-gateway-resource-manager.md).

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Azure dağıtım modelleri hakkında**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="create-a-classic-vnet-and-gateway"></a>Bir Klasik VNet ve ağ geçidi oluşturun
Aşağıdaki adımlar, Klasik VNet ve bir sanal ağ geçidi oluşturun. Klasik bir VNet zaten varsa, bkz: [olan bir Klasik VNet yapılandırma](#config) bu makalenin bölümünde.

1. [Klasik Azure portalında](http://manage.windowsazure.com) oturum açın.
2. Ekranın sol alt köşesinde, **yeni**. Gezinme bölmesinde **Ağ Hizmetleri**’ne, sonra da **Virtual Network**’a tıklayın. Yapılandırma sihirbazını başlatmak için **Özel Oluştur**’a tıklayın.
3. Üzerinde **sanal ağ ayrıntıları** sayfasında, aşağıdakileri girin:
   
   * **Ad** – sanal ağınızı adlandırın. Çok karışık bir isim vermekten istemeyebilirsiniz şekilde Vm'leri ve PaaS örnekleri dağıtırken bu sanal ağ adı kullanacaksınız.
   * **Konum** – konum, kaynaklarınızın (VM'ler) bulunmasını istediğiniz fiziksel konum (bölge) doğrudan ilişkilidir. Örneğin, bu sanal ağa dağıttığınız VM’lerin fiziksel olarak Doğu ABD’de bulunmasını istiyorsanız, o konumu seçin. Oluşturduktan sonra sanal ağınızla ilişkili bölgeyi değiştiremezsiniz.
4. **DNS Sunucuları ve VPN Bağlantısı** sayfasında aşağıdaki bilgileri girin ve sağ alt köşedeki ileri okuna tıklayın. 
   
   * **DNS sunucuları** - IP adresi ve DNS sunucusu adı girin veya kısayol menüsünden daha önce kaydedilmiş bir DNS sunucusu seçin. Bu ayarla bir DNS sunucusu oluşturulmaz. Söz konusu ayar, bu sanal ağa ilişkin ad çözümlemesi için kullanmak istediğiniz DNS sunucularını belirtmenize olanak sağlar.
   * **Siteden siteye bağlantı** -onay kutusunu seçip **siteden siteye VPN bağlantısını yapılandırma**.
   * **ExpressRoute** – onay kutusunu işaretleyin **kullanım ExpressRoute**. Seçtiyseniz bu seçeneği yalnızca görünür **siteden siteye VPN bağlantısını yapılandırma**.
   * **Yerel ağ** -ExpressRoute için bir yerel ağ sitesi için gereklidir. Ancak, bir ExpressRoute bağlantı söz konusu olduğunda yerel ağ site için belirtilen adres öneklerini yoksayılacak. Bunun yerine, expressroute bağlantı hattı Microsoft'a tanıtılan adres öneklerini yönlendirme amaçlar için kullanılır.<BR>ExpressRoute bağlantınızı için oluşturulan bir yerel ağ zaten varsa, açılan listeden seçebilirsiniz. Aksi takdirde, seçin **yeni bir yerel ağ belirtmek**.
5. **Siteden siteye bağlantı** önceki adımda yeni bir yerel ağ belirtmek için seçtiyseniz sayfası görüntülenir. Yerel ağınızın yapılandırmak için aşağıdaki bilgileri girin ve ardından sonraki oka tıklayın. 
   
   * **Ad** -ağ sitesini (şirket içi) yerel çağırmak istediğiniz adı.
   * **Adres alanı** - dahil olmak üzere başlangıç IP'si ve CIDR'si (adres sayısı). Sanal ağınız için adres aralığıyla örtüşmeyecek sürece herhangi bir adres aralığı belirtebilirsiniz. Genellikle, bunun şirket içi ağlarınız adres aralıklarını belirtebilirsiniz, ancak ExpressRoute söz konusu olduğunda, bu ayarlar kullanılmaz. Ancak, bu ayar, Klasik Portalı'nı kullanırken, yerel ağ oluşturmak için gereklidir.
   * **Adres Alanı Ekle** -Bu ayar için ExpressRoute ilgili değildir.
6. Üzerinde **sanal ağ adres alanları** sayfasında, aşağıdaki bilgileri girin ve ağınızı yapılandırmak için alt köşedeki onay işaretine tıklayın. 
   
   * **Adres alanı** - başlangıç IP dahil olmak üzere ve adres sayısı. Belirlediğiniz adres alanlarından herhangi biri yerel ağınızda sahip adres alanları çakışmadığını doğrulayın.
   * **Alt ağ Ekle** - dahil olmak üzere başlangıç IP'si ve adres sayısı. Ek alt ağlar gerekli değildir.
   * **Ağ geçidi alt ağı eklemek** -ağ geçidi alt ağı eklemek için tıklatın. Ağ geçidi alt ağı, yalnızca sanal ağ geçidi için kullanılır ve bu yapılandırma için gereklidir.<BR>CIDR (adres sayısı) için ExpressRoute ağ geçidi alt ağı /28 olmalıdır veya daha büyük (/ 27, / 26 vb..). Bu alt yeterli IP adresi çalışmak yapılandırma olanak sağlar. ExpressRoute, kullanmak için onay kutusunu seçtiyseniz, Klasik portalda /28 ile bir ağ geçidi alt ağı portal belirtir.  Klasik portalda CIDR adres sayısı ayarlayamazsınız. Ağ geçidi alt ağı olarak görünür **ağ geçidi** Klasik Portalı'nda oluşturulan ağ geçidi alt ağı gerçek adı gerçekte olmasına rağmen **GatewaySubnet**. Bu ad, Azure portalında veya PowerShell kullanarak görüntüleyebilirsiniz.
7. Sayfanın altındaki onay işaretine tıkladığınızda sanal ağınız oluşturulmaya başlar. Tamamlandığında, görürsünüz **oluşturulan** altında listelenen **durum** üzerinde **ağlar** Klasik portalında sayfası.

## <a name="gw"></a>Ağ geçidi oluşturma
1. Üzerinde **ağlar** sayfasında, yeni sanal ağ oluşturulan tıklayın ve ardından **Pano** sayfanın üst kısmındaki.
2. Pencerenin alt kısmındaki **Pano** sayfasında, **ağ geçidi Oluştur** seçip **dinamik yönlendirme**. Tıklatın **Evet** bir ağ geçidi oluşturmak istediğinizi onaylamak için.
3. Ağ geçidi oluşturma başlatıldığında, ağ geçidi başlatılmış olduğunu bildiğiniz bir ileti veren görürsünüz. Ağ geçidinin oluşturulması 45 dakika kadar sürebilir.
4. Ağınızdaki bir hattına bağlayın. Makalesindeki yönergeleri izleyin [ExpressRoute bağlantı hatları için sanal ağlara bağlanma](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Varolan Klasik VNet ExpressRoute için yapılandırma
Klasik bir VNet zaten varsa, ExpressRoute için Klasik portalda bağlanmak için yapılandırabilirsiniz. Yukarıdaki bölümlerde aynı şekilde gerekli ayarlarla tanımak için bu bölümler üzerinden okuma ayarlardır. Bir ExpressRoute /-siteye eşzamanlı bağlantı oluşturmak istiyorsanız, bkz: [bu makalede](expressroute-howto-coexist-classic.md) adımlar için. Bunlar, bu makaledeki adımları farklı değildir.

1. Yerel ağ VNet ayarlarınızı kalan güncelleştirmeden önce oluşturmanız gerekir. ExpressRoute Klasik Portalı'nı yapılandırırken gerekli olan yeni bir yerel ağ oluşturmak için tıklatın **yeni**  **>**  **Ağ Hizmetleri**  **>**  **sanal ağ**  **>**  **Ekle yerel ağ**. Yerel ağ oluşturmak için sihirbazın adımlarını izleyin.
2. Kullanım **yapılandırma** sayfa ayarlarını geri kalanı güncelleştirmek ve yerel ağ Vnet'e ilişkilendirmek için.
3. Ayarlarını yapılandırdıktan sonra Git [ağ geçidi oluşturmak](#gw) ağ geçidi oluşturmak için bu makalenin.

## <a name="next-steps"></a>Sonraki adımlar
* Sanal makineler, sanal ağınıza eklemek istiyorsanız, bkz: [sanal makine öğrenme yolları](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
* ExpressRoute hakkında daha fazla bilgi edinmek istiyorsanız, bkz: [ExpressRoute genel bakış](expressroute-introduction.md).

