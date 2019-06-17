---
title: Azure portalını kullanarak bir VNet-VNet VPN ağ geçidi bağlantısı yapılandırma | Microsoft Docs
description: Resource Manager ve Azure portalı kullanarak sanal ağlar arasında VPN ağ geçidi bağlantısı oluşturun.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.assetid: a7015cfc-764b-46a1-bfac-043d30a275df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/03/2018
ms.author: cherylmc
ms.openlocfilehash: 94b32595cf2c884ccfd1362f6c8d03f542aabfc5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62128390"
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-by-using-the-azure-portal"></a>Azure portalını kullanarak bir VNet-VNet VPN ağ geçidi bağlantısı yapılandırma

Bu makalede, sanal ağlar (Vnet'ler) bağlanma VNet-VNet bağlantı türünü kullanarak yardımcı olur. Sanal ağlar farklı bölgelerde ve farklı aboneliklere ait olabilir. Farklı aboneliklerden sanal ağları birbirine bağlama, aboneliklerin aynı Active Directory kiracısıyla ilişkilendirilmiş olması gerekmez. 

![v2v diyagramı](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

Bu makaledeki adımlarda, Azure Resource Manager dağıtım modeli için geçerlidir ve Azure portalını kullanın. Aşağıdaki makalelerde açıklanan seçeneklerini kullanarak, farklı bir dağıtım aracı veya modeli ile bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure portal (klasik)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Farklı dağıtım modellerini bağlama - Azure portalı](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Farklı dağıtım modellerini bağlama - PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>


## <a name="about-connecting-vnets"></a>Sanal ağları bağlama hakkında

Aşağıdaki bölümlerde, sanal ağları bağlamak için farklı yollar açıklanmaktadır.

### <a name="vnet-to-vnet"></a>Sanal Ağdan Sanal Ağa

Bir VNet-VNet bağlantısını yapılandırma Vnet'leri bağlamak için basit bir yoludur. VNet-VNet bağlantı türünü (VNet2VNet) bir sanal ağı başka bir sanal ağa bağlama, bir şirket içi konumuna siteden siteye IPSec bağlantısı oluşturma işlemiyle benzerdir. Her iki bağlantı türü de IPSec/IKE ile güvenli bir tünel sunmak ve iletişim kurarken aynı şekilde işlev için bir VPN ağ geçidi kullanın. Ancak, yerel ağ geçidi yapılandırılmış biçimde farklıdır. 

Bir VNet-VNet bağlantı oluşturduğunuzda yerel ağ geçidi adres alanını otomatik olarak oluşturulan doldurulur ve. Bir sanal ağın adres alanını güncelleştirirseniz diğer sanal ağ güncelleştirilmiş adres alanına otomatik olarak yönlendirir. Genellikle, daha hızlı ve kolay bir siteden siteye bağlantı'dan VNet-VNet bağlantısı oluşturmak.

### <a name="site-to-site-ipsec"></a>Siteden Siteye (IPsec)

Karmaşık ağ yapılandırmasıyla çalışıyorsanız, sanal ağlarınız kullanarak bağlamayı tercih edebilirsiniz bir [siteden siteye bağlantı](vpn-gateway-howto-site-to-site-resource-manager-portal.md) yerine. Siteden siteye IPSec adımlarını takip, yerel ağ geçitlerini kendiniz oluşturup yapılandırırsınız. Her sanal ağa ait yerel ağ geçidi, diğer sanal ağa yerel bir site gibi davranır. Bu adımlar yerel ağ geçidi trafiği yönlendirmek için ek adres alanları belirtmenizi sağlar. Bir sanal ağın adres alanı değiştiğinde, ona karşılık gelen yerel ağ geçidini el ile güncelleştirmeniz gerekir.

### <a name="vnet-peering"></a>VNet eşlemesi

Sanal ağlarınız VNet eşlemesi kullanarak da bağlanabilirsiniz. VNet eşlemesi bir VPN ağ geçidi kullanmaz ve farklı kısıtlamaları vardır. Ayrıca, [sanal ağ eşleme fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-network), [Sanal Ağlar Arası VPN Gateway fiyatlandırmasından](https://azure.microsoft.com/pricing/details/vpn-gateway) farklı olarak hesaplanır. Daha fazla bilgi için bkz. [VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md).

## <a name="why-create-a-vnet-to-vnet-connection"></a>Neden bir VNet-VNet bağlantı oluştururum

Aşağıdaki nedenlerle bir VNet-VNet bağlantısı kullanarak sanal ağları bağlama isteyebilirsiniz:

### <a name="cross-region-geo-redundancy-and-geo-presence"></a>Çapraz bölgede coğrafi yedeklilik ve iletişim durumu

  * Kendi coğrafi çoğaltma veya eşitlemenizi güvenli bağlantıyla internet'e yönelik Uç noktalara gitmeden ayarlayabilirsiniz.
  * Azure Traffic Manager ve Azure Load Balancer ile yüksek oranda kullanılabilir iş yükü coğrafi yedeklilik ile birçok Azure bölgesinde ayarlayabilirsiniz. Örneğin, SQL Server Always On kullanılabilirlik gruplarını Azure bölgelerinde ayarlayabilirsiniz.

### <a name="regional-multi-tier-applications-with-isolation-or-administrative-boundaries"></a>Yalıtım veya yönetim sınır bölgesel çok katmanlı uygulamalar

  * Aynı bölge içinde yalıtım ve yönetim gereksinimlerinden dolayı birbirlerine bağlı birden fazla sanal ağ ile çok katmanlı uygulamalar ayarlayabilirsiniz.

Hatta Sanal Ağdan Sanal Ağa iletişim çok siteli yapılandırmalarla bile birleştirilebilir. Bu yapılandırmalar birleştiren ağ topolojileri kurabilmenize olanak sağlar içi ve dışı karışık bağlantı ile sanal ağlar arası bağlantı, aşağıdaki diyagramda gösterildiği gibi:

![Bağlantılar hakkında](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "About connections")

Bu makale, VNet-VNet bağlantı türünü kullanarak sanal ağları bağlama işlemini göstermektedir. Bir alıştırma olarak adımları izlediğinizde, aşağıdaki örnek ayar değerlerini kullanabilirsiniz. Örnekte, sanal ağlar aynı abonelikte ancak farklı kaynak gruplarındadır. Sanal ağlarınız farklı aboneliklerdeyse, portalda bağlantı oluşturamazsınız. Kullanım [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) veya [CLI](vpn-gateway-howto-vnet-vnet-cli.md) yerine. VNet-VNet bağlantıları hakkında daha fazla bilgi için bkz. [VNet-VNet SSS](#vnet-to-vnet-faq).

### <a name="example-settings"></a>Örnek ayarlar

**Değerler TestVNet1 için:**

- **Sanal ağ ayarları**
    - **Ad**: Girin *TestVNet1*.
    - **Adres alanı**: Girin *10.11.0.0/16*.
    - **Abonelik**: Kullanmak istediğiniz aboneliği seçin.
    - **Kaynak grubu**: Girin *TestRG1*.
    - **Konum**: **Doğu ABD**’yi seçin.
    - **Alt ağ**
        - **Ad**: Girin *ön uç*.
        - **Adres aralığı**: Girin *10.11.0.0/24*.
    - **Ağ geçidi alt ağı**:
        - **Ad**: *GatewaySubnet* autofilled olduğu.
        - **Adres aralığı**: Girin *10.11.255.0/27*.
    - **DNS sunucusu**: Seçin **özel** ve DNS sunucunuzun IP adresini girin.

- **Sanal ağ geçidi ayarları** 
    - **Ad**: Girin *TestVNet1GW*.
    - **Ağ geçidi türü**: Seçin **VPN**.
    - **VPN türü**: Seçin **rota tabanlı**.
    - **SKU**: SKU kullanmak istediğiniz ağ geçidi seçin.
    - **Genel IP adresi adı**: Girin *Testvnet1gwıp*
    - **bağlantı** 
       - **Ad**: Girin *TestVNet1toTestVNet4*.
       - **Paylaşılan anahtar**: Girin *abc123*. Paylaşılan anahtarı kendiniz oluşturabilirsiniz. Sanal ağlar arası bağlantı oluşturduğunuzda, değerlerin eşleşmesi gerekir.

**Değerler TestVNet4 için:**

- **Sanal ağ ayarları**
   - **Ad**: Girin *testvnet4'ü*.
   - **Adres alanı**: Girin *10.41.0.0/16*.
   - **Abonelik**: Kullanmak istediğiniz aboneliği seçin.
   - **Kaynak grubu**: Girin *TestRG4*.
   - **Konum**: Seçin **Batı ABD**.
   - **Alt ağ** 
      - **Ad**: Girin *ön uç*.
      - **Adres aralığı**: Girin *10.41.0.0/24*.
   - **GatewaySubnet** 
      - **Ad**: *GatewaySubnet* autofilled olduğu.
      - **Adres aralığı**: Girin *10.41.255.0/27*.
   - **DNS sunucusu**: Seçin **özel** ve DNS sunucunuzun IP adresini girin.

- **Sanal ağ geçidi ayarları** 
    - **Ad**: Girin *TestVNet4GW*.
    - **Ağ geçidi türü**: Seçin **VPN**.
    - **VPN türü**: Seçin **rota tabanlı**.
    - **SKU**: SKU kullanmak istediğiniz ağ geçidi seçin.
    - **Genel IP adresi adı**: Girin *Testvnet4gwıp*.
    - **bağlantı** 
       - **Ad**: Girin *TestVNet4toTestVNet1*.
       - **Paylaşılan anahtar**: Girin *abc123*. Paylaşılan anahtarı kendiniz oluşturabilirsiniz. Sanal ağlar arası bağlantı oluşturduğunuzda, değerlerin eşleşmesi gerekir.

## <a name="create-and-configure-testvnet1"></a>TestVNet1’i oluşturma ve yapılandırma
Zaten bir sanal ağınız varsa, ayarların VPN ağ geçidi tasarımınızla uyumlu olduğunu doğrulayın. Diğer ağlarla çakışabilecek herhangi bir alt ağ olup olmadığına özellikle dikkat edin. Çakışan alt ağlarınız varsa bağlantınız düzgün çalışmaz. Vnet'inizi doğru ayarlarla yapılandırıldıktan sonra bir DNS sunucusu bölümü belirtin adımları uygulamaya başlayabilirsiniz.

### <a name="to-create-a-virtual-network"></a>Sanal ağ oluşturmak için
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="add-additional-address-space-and-create-subnets"></a>Ek adres alanı ekleme ve alt ağ oluşturma
Sanal ağınız oluşturulduktan sonra ek adres alanı ekleyebilir ve alt ağ oluşturabilirsiniz.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="create-a-gateway-subnet"></a>Ağ geçidi alt ağı oluşturma
Sanal ağınız için bir sanal ağ geçidi oluşturmadan önce ilk olarak ağ geçidi alt ağını oluşturmanız gerekir. Ağ geçidi alt ağı, sanal ağ geçidinin kullandığı IP adreslerini içerir. Mümkünse, gelecekteki ek yapılandırma gereksinimlerini karşılamaya yetecek sayıda IP adresine sağlamak için/28'i veya/27 CIDR bloğu kullanarak bir ağ geçidi alt ağı oluşturulması idealdir.

Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız başvurun [örnek ayarları](#example-settings) ağ geçidi alt ağınızı oluştururken.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>Bir ağ geçidi alt ağı oluşturmak için
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="specify-a-dns-server-optional"></a>DNS sunucusu belirtme (isteğe bağlı)
DNS, VNet-VNet bağlantıları için gerekli değildir. Ancak, sanal ağınıza dağıtılmış kaynaklar için ad çözümleme istiyorsanız bir DNS sunucusu belirtin. Bu ayar, bu sanal ağ için ad çözümlemede kullanmak istediğiniz DNS sunucusunu belirtmenizi sağlar. Bunu yapmak için bir DNS sunucusu oluşturmaz.

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="create-a-virtual-network-gateway"></a>Sanal ağ geçidi oluşturma
Bu adımda sanal ağınız için sanal ağ geçidi oluşturacaksınız. Bir ağ geçidinin oluşturulması, seçili ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir. Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız bkz [örnek ayarları](#example-settings).

### <a name="to-create-a-virtual-network-gateway"></a>Bir sanal ağ geçidi oluşturmak için
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="create-and-configure-testvnet4"></a>TestVNet4’ü oluşturma ve yapılandırma
Testvnet1'i yapılandırdıktan sonra testvnet4'ü önceki adımları yineleyerek ve değerleri testvnet4'ü değerlerle değiştirerek oluşturun. TestVNet1 için sanal ağ geçidi testvnet4'ü yapılandırmadan önce oluşturma işlemlerinin tamamlanmasını beklemenize gerek yoktur. Kendi değerlerinizi kullanıyorsanız, tüm bağlanmak istediğiniz sanal ağ adres alanlarının çakışmadığından emin olun.

## <a name="configure-the-testvnet1-gateway-connection"></a>TestVNet1 ağ geçidi bağlantısını yapılandırma
TestVNet1 ve TestVNet4 için sanal ağ geçidi oluşturma işlemleri tamamlandıktan sonra sanal ağ geçidi bağlantılarınızı oluşturabilirsiniz. Bu bölümde VNet1 ile VNet4 arasında bir bağlantı oluşturursunuz. Bu adımlar yalnızca aynı abonelikteki sanal ağlar için geçerlidir. Sanal ağlarınız farklı aboneliklerdeyse kullanmalısınız [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) bağlantı kurmak için. Sanal ağlarınız aynı Abonelikteki farklı kaynak gruplarında yoksa, ancak, bunları portalını kullanarak bağlanabilirsiniz.

1. Azure portalında **tüm kaynakları**, girin *sanal ağ geçidi* arama kutusuna ve ardından sanal ağınıza ait sanal ağ geçidine gidin. Örnek: **TestVNet1GW**. Açmak için seçin **sanal ağ geçidi** sayfası.

   ![Bağlantılar sayfası](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/1to4connect2.png "Connections page")
2. Altında **ayarları**seçin **bağlantıları**ve ardından **Ekle** açmak için **Bağlantı Ekle** sayfası.

   ![Bağlantı ekleme](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add.png "Add a connection")
3. Üzerinde **Bağlantı Ekle** sayfasında, bağlantınız için değerleri girin:

   - **Ad**: Bağlantınız için bir ad girin. Örnek: *TestVNet1toTestVNet4*.

   - **Bağlantı türü**: Seçin **VNet-VNet** açılır listeden.

   - **İlk sanal ağ geçidi**: Bu bağlantıyı belirtilen sanal ağ geçidinden oluşturduğunuz için bu alanın değeri otomatik olarak doldurulur.

   - **İkinci sanal ağ geçidi**: Bu alan için bir bağlantı oluşturmak istediğiniz sanal ağın sanal ağ geçidi olur. Seçin **başka bir sanal ağ geçidi seçin** açmak için **sanal ağ geçidi Seç** sayfası.

     - Bu sayfada listelenen sanal ağ geçitlerini görüntüleyin. Yalnızca aboneliğinizdeki sanal ağ geçitleri listelenir. Aboneliğinizde kullanmak olmayan bir sanal ağ geçidine bağlanmak istiyorsanız [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md).

     - Bağlanmak istediğiniz sanal ağ geçidi seçin.

     - **Paylaşılan anahtar (PSK)** : Bu alanda bağlantınız için bir paylaşılan anahtar girin. Bu anahtarı kendiniz üretebilir veya oluşturabilirsiniz. Siteden siteye bağlantı, kullanacağınız anahtarın şirket içi Cihazınızı ve sanal ağ geçidi bağlantınızı aynıdır. Başka bir sanal ağ geçidi için bağlantı kurduğunuz, kavram, yerine bir VPN cihazına bağlanmak dışında burada benzer.
    
4. Seçin **Tamam** yaptığınız değişiklikleri kaydedin.

## <a name="configure-the-testvnet4-gateway-connection"></a>TestVNet4 ağ geçidi bağlantısını yapılandırma
Bu adımda TestVNet4 ile TestVNet1 arasında bir bağlantı oluşturun. Portalda TestVNet4 ile ilişkili sanal ağ geçidini bulun. TestVNet4’ten TestVNet1’e bağlantı oluşturmak için değerleri değiştirerek önceki bölümde verilen adımları izleyin. Aynı paylaşılan anahtarı kullandığınızdan emin olun.

## <a name="verify-your-connections"></a>Bağlantılarınızı doğrulayın

Azure portalında sanal ağ geçidini bulun. Üzerinde **sanal ağ geçidi** sayfasında **bağlantıları** görüntülemek için **bağlantıları** sayfası için sanal ağ geçidi. Bağlantı kurulduktan sonra göreceğiniz **durumu** değerlerini değiştirmek için **başarılı** ve **bağlı**. Bir bağlantıyı açmak için seçin **Essentials** sayfasında ve daha fazla bilgi görüntüleyin.

![Başarılı](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Succeeded")

Veri akışı başladığında değerlerini görürsünüz **verilerinde** ve **verileri**.

![Temel Parçalar](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Essentials")

## <a name="add-additional-connections"></a>Başka bağlantılar eklemek

Başka bağlantılar eklemek istiyorsanız, bağlantı oluşturmak istediğiniz sanal ağ geçidine gidin ve ardından **bağlantıları**. Başka bir VNet-VNet bağlantısı oluşturabilir veya bir şirket içi konum ile IPSec Siteden Siteye bağlantısı oluşturabilirsiniz. **Bağlantı türünü**, oluşturmak istediğiniz bağlantı türüyle eşleşecek şekilde ayarladığınızdan emin olun. Başka bağlantılar oluşturmadan önce sanal ağınız için adres alanlarının bağlanmak istediğiniz adres alanlarından herhangi biriyle ile çakışmayacak doğrulayın. Siteden Siteye bağlantı oluşturma adımları için bkz. [Siteden Siteye bağlantı oluşturma](vpn-gateway-howto-site-to-site-resource-manager-portal.md).

## <a name="vnet-to-vnet-faq"></a>Sanal Ağdan Sanal Ağa - SSS
Sanal ağlar arası bağlantılar hakkında ek bilgi için SSS sayfasını görüntüleyin.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Nasıl, ağ trafiğini bir sanal ağ içindeki kaynaklarla sınırlayabilirsiniz hakkında daha fazla bilgi için bkz: [ağ güvenliği](../virtual-network/security-overview.md).

Azure, şirket içi ve İnternet kaynakları arasındaki trafiğin Azure tarafından nasıl yönlendirdiği hakkında bilgi için bkz. [Sanal ağ trafiği yönlendirme](../virtual-network/virtual-networks-udr-overview.md).
