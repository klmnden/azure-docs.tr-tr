---
title: "Azure Resource Manager ve Azure Portal’ı kullanarak Konumdan Konuma VPN bağlantısına sahip bir sanal ağ oluşturma | Microsoft Belgeleri"
description: "Resource Manager dağıtım modelini kullanarak bir sanal ağ oluşturma ve S2S VPN ağ geçidi bağlantısı kullanarak söz konusu ağı yerel şirket içi ağınıza bağlama."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 827a4db7-7fa5-4eaf-b7e1-e1518c51c815
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/14/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: d269d9a76ff4ccd973eee70d2d5b54a7262383ef
ms.openlocfilehash: f0491df77418c4d7c79beff87302b64ddc3fa9be


---
# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>Azure portalını kullanarak Siteden Siteye bağlantı ile VNet oluşturma
> [!div class="op_single_selector"]
> * [Resource Manager - Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Klasik - Klasik Portal](vpn-gateway-site-to-site-create.md)
> 
> 

Bu makale, Azure Resource Manager dağıtım modelini ve Azure portal’ı kullanarak sanal ağ ve şirket içi ağınıza yönelik Konumdan Konuma VPN ağ geçidi bağlantısı oluşturma işleminde size yol gösterir. Siteden Siteye bağlantılar, şirket içi ve dışı karışık yapılandırmalar ve karma yapılandırmalar için kullanılabilir.

![Diyagram](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)

### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Konumdan Konuma bağlantılar için dağıtım modelleri ve yöntemleri
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Aşağıdaki tabloda, şu anda Siteden Siteye yapılandırmalar için kullanılabilen dağıtım modelleri ve yöntemleri gösterilmektedir. Yapılandırma adımlarını içeren bir makale olduğunda, bu tablodan makaleye yönelik doğrudan bağlantı oluştururuz.

[!INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Ek yapılandırmalar
Sanal ağları birbirine bağlamak istiyor ancak şirket içi bir konuma bağlantı oluşturmuyorsanız, bkz. [VNet’ten VNet’e bağlantıyı yapılandırma](vpn-gateway-vnet-vnet-rm-ps.md). Zaten bağlantısı bulunan bir sanal ağa Siteden Siteye bağlantı eklemek istiyorsanız bkz. [VPN ağ geçidi bağlantısı bulunan bir sanal ağa S2S bağlantısı ekleme](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Başlamadan önce
Yapılandırmanıza başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın:

* Uyumlu bir VPN cihazı ve bu cihazı yapılandırabilecek biri. Bkz. [VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md). VPN cihazınızı yapılandırma konusuyla veya şirket içi ağ yapılandırmanızdaki IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir.
* VPN cihazınız için dışarıya yönelik genel bir IP adresi. Bu IP adresi bir NAT’nin arkasında olamaz.
* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) etkinleştirebilir veya [ücretsiz bir hesap](http://azure.microsoft.com/pricing/free-trial) için kaydolabilirsiniz.

### <a name="a-namevaluesasample-configuration-values-for-this-exercise"></a><a name="values"></a>Bu alıştırma için örnek yapılandırma değerleri
Bu adımları bir alıştırma olarak kullanırken, şu örnek yapılandırma değerlerini kullanabilirsiniz:

* **VNet Name:** TestVNet1
* **Adres Alanı:** 10.11.0.0/16 ve 10.12.0.0/16
* **Alt ağlar:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24
  * GatewaySubnet: 10.12.255.0/27
* **Kaynak Grubu:** TestRG1
* **Konum:** Doğu ABD
* **DNS Sunucusu:** 8.8.8.8
* **Ağ Geçidi Adı:** VNet1GW
* **Genel IP:** VNet1GWIP
* **VPN Türü:** Yol tabanlı
* **Bağlantı Türü:** Siteden siteye (IPsec)
* **Ağ Geçidi Türü:** VPN
* **Yerel Ağ Geçidi Adı:** Site2
* **Bağlantı Adı:** VNet1toSite2

## <a name="a-namecreatvneta1-create-a-virtual-network"></a><a name="CreatVNet"></a>1. Sanal ağ oluşturma
Zaten bir VNet'iniz varsa ayarların VPN ağ geçidi tasarımınızla uyumlu olduğunu doğrulayın. Diğer ağlarla çakışabilecek herhangi bir alt ağ olup olmadığına özellikle dikkat edin. Çakışan alt ağlarınız varsa bağlantınız düzgün şekilde gerçekleşmeyebilir. VNet'iniz doğru ayarlarla yapılandırıldıysa [DNS sunucusu belirtme](#dns) bölümündeki adımları uygulamaya başlayabilirsiniz.

### <a name="to-create-a-virtual-network"></a>Sanal ağ oluşturmak için
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="a-namesubnetsa2-add-additional-address-space-and-subnets"></a><a name="subnets"></a>2. Ek adres alanı ve alt ağları ekleme
Ek adres alanı ve alt ağları VNet’iniz oluşturulduktan sonra ekleyebilirsiniz.

[!INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="a-namednsa3-specify-a-dns-server"></a><a name="dns"></a>3. DNS sunucusu belirleme
### <a name="to-specify-a-dns-server"></a>DNS sunucusu belirlemek için
[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="a-namegatewaysubneta4-create-a-gateway-subnet"></a><a name="gatewaysubnet"></a>4. Ağ geçidi alt ağı oluşturma
Sanal ağınızı bir ağ geçidine bağlamadan önce, bağlamak istediğiniz sanal ağ için ağ geçidi alt ağını oluşturmanız gerekir. Mümkünse, gelecekteki ek yapılandırma gereksinimlerini karşılamaya yetecek sayıda IP adresi sağlamak için /28 veya /27 CIDR bloğu kullanılarak ağ geçidi alt ağı oluşturulması idealdir.

Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız ağ geçidi alt ağınızı oluştururken bu [değerlere](#values) başvurun.

### <a name="to-create-a-gateway-subnet"></a>Bir ağ geçidi alt ağı oluşturmak için
[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="a-namevnetgatewaya5-create-a-virtual-network-gateway"></a><a name="VNetGateway"></a>5. Sanal ağ geçidi oluşturma
Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız [örnek yapılandırma değerlerine](#values) başvurabilirsiniz.

### <a name="to-create-a-virtual-network-gateway"></a>Bir sanal ağ geçidi oluşturmak için
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="a-namelocalnetworkgatewaya6-create-a-local-network-gateway"></a><a name="LocalNetworkGateway"></a>6. Yerel ağ geçidi oluşturma
‘Yerel ağ geçidi’ şirket içi konumunuz anlamına gelir. Azure'ın başvurmak üzere kullanabilmesi için yerel ağ geçidinize bir ad verin. 

Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız [örnek yapılandırma değerlerine](#values) başvurabilirsiniz.

### <a name="to-create-a-local-network-gateway"></a>Bir yerel ağ geçidi oluşturmak için
[!INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="a-namevpndevicea7-configure-your-vpn-device"></a><a name="VPNDevice"></a>7. VPN cihazınızı yapılandırma
[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="a-namecreateconnectiona8-create-a-site-to-site-vpn-connection"></a><a name="CreateConnection"></a>8. Siteden Siteye bağlantı kurma
Sanal ağ geçidiniz ile VPN cihazınız arasında Siteden Siteye VPN bağlantısı oluşturun. Değerlerin kendinizinkilerle değiştirildiğinden emin olun. Paylaşılan anahtar, VPN cihazınızın yapılandırması için kullandığınız değerin aynısı olmalıdır. 

Bu bölüme başlamadan önce sanal ağa ait ağ geçidi ve yerel ağa ait ağ geçidinizin oluşturulmasının tamamlandığından emin olun. Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız bağlantınızı oluştururken bu [değerlere](#values) başvurun.

### <a name="to-create-the-vpn-connection"></a>Bir VPN bağlantısını oluşturmak için
[!INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="a-nameverifyconnectiona9-verify-the-vpn-connection"></a><a name="VerifyConnection"></a>9. VPN bağlantısını doğrulama
VPN bağlantınızı portaldan veya PowerShell kullanarak doğrulayabilirsiniz.

[!INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
*  Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
*  BGP hakkında bilgi edinmek için [BGP’ye Genel Bakış](vpn-gateway-bgp-overview.md) ve [BGP’yi yapılandırma](vpn-gateway-bgp-resource-manager-ps.md) makalelerine bakın.




<!--HONumber=Nov16_HO4-->


