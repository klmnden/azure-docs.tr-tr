---
title: 'Şirket içi ağınızı bir Azure sanal ağına bağlama: Siteden siteye VPN: Portal | Microsoft Docs'
description: Şirket içi ağınız ile bir Azure sanal ağı arasında genel İnternet üzerinden bir IPSec bağlantısı oluşturma adımları. Bu adımlar portalı kullanarak Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı oluşturmanıza yardımcı olur.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: cherylmc
ms.openlocfilehash: 032b6a4f5147d06a4613a827a0372437dca47f47
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60407749"
---
# <a name="create-a-site-to-site-connection-in-the-azure-portal"></a>Azure portalında Siteden Siteye bağlantı oluşturma

Bu makalede, Azure portalını kullanarak şirket içi ağınızdan VNet’e Siteden Siteye VPN ağ geçidi bağlantısı oluşturma işlemi gösterilir. Bu makaledeki adımlar Resource Manager dağıtım modeli için geçerlidir. Ayrıca aşağıdaki listeden farklı bir seçenek belirtip farklı bir dağıtım aracı veya dağıtım modeli kullanarak da bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure portal (klasik)](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

Siteden Siteye VPN ağ geçidi bağlantısı, şirket içi ağınızı bir IPsec/IKE (IKEv1 veya IKEv2) tüneli üzerinden Azure sanal ağına bağlamak için kullanılır. Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir. VPN ağ geçitleri hakkında daha fazla bilgi için bkz. [VPN ağ geçidi hakkında](vpn-gateway-about-vpngateways.md).

![Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı diyagramı](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Uyumlu bir VPN cihazı ve bu cihazı yapılandırabilecek birinin bulunduğundan emin olun. Uyumlu VPN cihazları ve cihaz yapılandırması hakkında daha fazla bilgi için bkz.[VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md).
* VPN cihazınız için dışarıya dönük genel bir IPv4 adresi olduğunu doğrulayın.
* Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir. Bu yapılandırmayı oluşturduğunuzda, Azure’un şirket içi konumunuza yönlendireceği IP adres aralığı ön eklerini oluşturmanız gerekir. Şirket içi ağınızın alt ağlarından hiçbiri, bağlanmak istediğiniz sanal ağ alt ağlarıyla çakışamaz. 

### <a name="values"></a>Örnek değerler

Bu makaledeki örneklerde aşağıdaki değerler kullanılır. Bu değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bunlara bakabilirsiniz. Genel olarak VPN Gateway ayarları hakkında daha fazla bilgi için [VPN Gateway Ayarları Hakkında](vpn-gateway-about-vpn-gateway-settings.md) konusunu inceleyin.

* **VNet adı:** TestVNet1
* **Adres Alanı:** 10.1.0.0/16
* **Abonelik:** Kullanmak istediğiniz abonelik
* **Kaynak grubu:** TestRG1
* **Konum:** Doğu ABD
* **Alt ağı:** Ön uç: 10.1.0.0/24, BackEnd: 10.1.1.0/24 (Bu alıştırma için isteğe bağlı)
* **Ağ geçidi alt ağ adı:** GatewaySubnet (Bu işlem otomatik olarak Doldur Portalı'nda)
* **Ağ geçidi alt ağ adres aralığı:** 10.1.255.0/27
* **DNS sunucusu:** 8.8.8.8 - isteğe bağlı. DNS sunucunuzun IP adresidir.
* **Sanal ağ geçidi adı:** VNet1GW
* **Genel IP:** VNet1GWIP
* **VPN türü:** Rota tabanlı
* **Bağlantı türü:** Siteden siteye (IPSec)
* **Ağ geçidi türü:** VPN
* **Yerel ağ geçidi adı:** Site1
* **Bağlantı adı:** VNet1toSite1
* **Paylaşılan anahtar:** Bu örnekte abc123 kullanılır. Ancak, VPN donanımınızla uyumlu herhangi bir seçeneği kullanabilirsiniz. Önemli olan, değerin bağlantının her iki tarafında eşleşmesidir.

## <a name="CreatVNet"></a>1. Sanal ağ oluşturma

[!INCLUDE [Create a virtual network](../../includes/vpn-gateway-create-virtual-network-portal-include.md)]

## <a name="dns"></a>2. DNS sunucusu belirleme

Siteden Siteye bağlantı oluşturmak için DNS gerekli değildir. Ancak, sanal ağınıza dağıtılmış olan kaynaklarınız için ad çözümleme istiyorsanız bir DNS sunucusu belirtmeniz gerekir. Bu ayar, bu sanal ağ için ad çözümlemede kullanmak istediğiniz DNS sunucusunu belirtmenizi sağlar. Bir DNS sunucusu oluşturmaz. Ad çözümlemesi hakkında daha fazla bilgi için bkz. [VM'ler ve rol örnekleri için Ad Çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

[!INCLUDE [Specify a dns server - optional](../../includes/vpn-gateway-specify-dns-portal-include.md)]

## <a name="gatewaysubnet"></a>3. Ağ geçidi alt ağını oluşturma

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [Add a gateway subnet](../../includes/vpn-gateway-add-gateway-subnet-portal-include.md)]

[!INCLUDE [NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="VNetGateway"></a>4. VPN ağ geçidini oluşturma

[!INCLUDE [Create a vpn gateway](../../includes/vpn-gateway-add-gateway-portal-include.md)]

## <a name="LocalNetworkGateway"></a>5. Yerel ağ geçidini oluşturma

Yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Siteye Azure’un başvuruda bulunmak için kullanabileceği bir ad verir, ardından bağlantı oluşturacağınız şirket içi VPN cihazının IP adresini belirtirsiniz. Ayrıca, VPN ağ geçidi üzerinden VPN cihazına yönlendirilecek IP adresi ön eklerini de belirtirsiniz. Belirttiğiniz adres ön ekleri, şirket içi adresinizde yer alan ön eklerdir. Şirket içi ağınız değişirse veya VPN cihazının genel IP adresini değiştirmeniz gerekirse değerleri daha sonra kolayca güncelleştirebilirsiniz.

[!INCLUDE [Add a local network gateway](../../includes/vpn-gateway-add-local-network-gateway-portal-include.md)]

## <a name="VPNDevice"></a>6. VPN cihazınızı yapılandırma

Bir şirket içi ağı ile Siteden Siteye bağlantılar için VPN cihazı gerekir. Bu adımda VPN cihazınızı yapılandıracaksınız. VPN cihazınızı yapılandırırken şunlar gerekir:

- Paylaşılan bir anahtar. Siteden Siteye VPN bağlantınızı oluştururken belirttiğiniz paylaşılan anahtarın aynısıdır. Bu örneklerde temel bir paylaşılan anahtar kullanılır. Kullanmak için daha karmaşık bir anahtar oluşturmanız önerilir.
- Sanal ağ geçidinizin Genel IP adresi. Azure Portal, PowerShell veya CLI kullanarak genel IP adresini görüntüleyebilirsiniz. Azure portalını kullanarak VPN ağ geçidinizin Genel IP adresini bulmak için **Sanal ağ geçitleri**’ne gidin ve ağ geçidinizin adına tıklayın.

[!INCLUDE [Configure a VPN device](../../includes/vpn-gateway-configure-vpn-device-include.md)]

## <a name="CreateConnection"></a>7. VPN bağlantısını oluşturma

Sanal ağ geçidiniz ile şirket içi VPN cihazınız arasında Siteden Siteye VPN bağlantısı oluşturun.

[!INCLUDE [Add a site-to-site connection](../../includes/vpn-gateway-add-site-to-site-connection-portal-include.md)]

## <a name="VerifyConnection"></a>8. VPN bağlantısını doğrulama

[!INCLUDE [Verify the connection](../../includes/vpn-gateway-verify-connection-portal-include.md)]

## <a name="connectVM"></a>Sanal makineye bağlanma

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <a name="reset"></a>VPN ağ geçidini sıfırlama

Bir veya daha fazla Siteden Siteye VPN tünelinde şirketler arası VPN bağlantısını kaybederseniz bir Azure VPN ağ geçidinin sıfırlanması yararlıdır. Bu durumda şirket içi VPN cihazlarınızın tümü düzgün çalışır, ancak Azure VPN ağ geçitleriyle IPsec tünelleri kuramaz. Adımlar için bkz. [VPN ağ geçidini sıfırlama](vpn-gateway-resetgw-classic.md).

## <a name="resize"></a>Ağ geçidi SKU’sunu değiştirme (ağ geçidini yeniden boyutlandırma)

Bir ağ geçidi SKU'sunu değiştirme adımları için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

## <a name="addconnect"></a>Bir VPN ağ geçidine başka bağlantı ekleme

Hiçbir adres alanının bağlantılar arasında çakışmaması şartıyla başka bağlantılar ekleyebilirsiniz.

1. Başka bir bağlantı eklemek için VPN ağ geçidine gidin, ardından **Bağlantılar**’a tıklayarak Bağlantılar sayfasını açın.
2. Bağlantınızı eklemek için **+Ekle**’ye tıklayın. Bağlantı türünü VNet-VNet (başka bir VNet ağ geçidine bağlanılıyorsa) veya Site-site ayarını yansıtacak şekilde ayarlayın.
3. Site-site ile bağlantı kuruyorsanız ve bağlanmak istediğiniz site için henüz bir yerel ağ geçidi oluşturmadıysanız yeni bir tane oluşturabilirsiniz.
4. Kullanmak istediğiniz paylaşılan anahtarı belirtin, ardından **Tamam**’a tıklayarak bağlantıyı oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

* BGP hakkında bilgi edinmek için [BGP’ye Genel Bakış](vpn-gateway-bgp-overview.md) ve [BGP’yi yapılandırma](vpn-gateway-bgp-resource-manager-ps.md) makalelerine bakın.
* Zorlamalı tünel oluşturma hakkında bilgi için bkz. [Zorlamalı tünel oluşturma hakkında](vpn-gateway-forced-tunneling-rm.md).
* Yüksek Oranda Kullanılabilir Etkin-Etkin bağlantılar hakkında bilgi için bkz. [Yüksek Oranda Kullanılabilir Şirket İçi ve Dışı ile Sanal Ağdan Sanal Ağa Bağlantı](vpn-gateway-highlyavailable.md).
* Ağ trafiğini bir sanal ağ içindeki kaynaklarla sınırlama hakkında bilgi için bkz. [Ağ Güvenliği](../virtual-network/security-overview.md).
* Azure, şirket içi ve İnternet kaynakları arasındaki trafiğin Azure tarafından nasıl yönlendirdiği hakkında bilgi için bkz. [Sanal ağ trafiği yönlendirme](../virtual-network/virtual-networks-udr-overview.md).
* Azure Resource Manager şablonunu kullanarak siteden siteye VPN bağlantısı oluşturma hakkında bilgi için bkz. [Siteden Siteye VPN Bağlantısı Oluşturma](https://azure.microsoft.com/resources/templates/101-site-to-site-vpn-create/).
* Azure Resource Manager şablonunu kullanarak sanal ağlar arası VPN bağlantısı oluşturma hakkında bilgi için bkz. [HBase coğrafi çoğaltmayı dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-geo/).
