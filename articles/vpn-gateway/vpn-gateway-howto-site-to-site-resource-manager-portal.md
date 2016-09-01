<properties
   pageTitle="Azure Resource Manager ve Azure Portal’ını kullanarak Siteden Siteye VPN bağlantısına sahip bir sanal ağ oluşturma | Microsoft Azure"
   description="Resource Manager modelini kullanarak bir sanal ağ oluşturma ve S2S VPN ağ geçidi bağlantısı kullanarak söz konusu ağı yerel şirket içi ağınıza bağlama."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="cherylmc"/>

# Azure Portal ve Azure Resource Manager kullanarak Siteden Siteye VPN bağlantısına sahip bir VNet oluşturma

> [AZURE.SELECTOR]
- [Azure Portalı](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Azure Klasik Portalı](vpn-gateway-site-to-site-create.md)
- [PowerShell - Resource Manager](vpn-gateway-create-site-to-site-rm-powershell.md)


Bu makalede, Azure Resource Manager dağıtım modelini ve Azure portalını kullanarak sanal bir ağ ve şirket içi ağınıza yönelik Siteden Siteye VPN bağlantısı oluşturma işlemi adım adım açıklanmaktadır.

![Diyagram](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)



### Siteden Siteye bağlantılar için dağıtım modelleri ve araçları

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)] 

Sanal ağları birbirine bağlamak istiyor ancak şirket içi bir konuma bağlantı oluşturmuyorsanız, bkz. [VNet’ten VNet’e bağlantıyı yapılandırma](vpn-gateway-vnet-vnet-rm-ps.md).

## Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın:

- Uyumlu bir VPN cihazı ve bu cihazı yapılandırabilecek biri. Bkz. [VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md). VPN cihazınızı yapılandırma konusuyla veya şirket içi ağ yapılandırmanızdaki IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir.

- VPN cihazınız için dışarıya yönelik genel bir IP adresi. Bu IP adresi bir NAT’nin arkasında olamaz.
    
- Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](http://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

### <a name="values"></a>Bu alıştırma için örnek yapılandırma değerleri


Bu adımları bir alıştırma olarak kullanırken, şu örnek yapılandırma değerlerini kullanabilirsiniz:

- VNet Name: TestVNet1
- Address Space: 10.11.0.0/16 ve 10.12.0.0/16
- Alt ağlar: 
    - FrontEnd: 10.11.0.0/24
    - BackEnd: 10.12.0.0/24
    - GatewaySubnet: 10.12.255.0/27
- Resource Group: TestRG1
- Location: East US
- DNS Server: 8.8.8.8
- Gateway Name: VNet1GW
- Public IP: VNet1GWIP
- VPN Type: Route-based
- Connection Type: Site-to-site (IPsec)
- Gateway Type: VPN
- Local Network Gateway Name: Site2
- Connection Name: VNet1toSite2



## 1. Sanal ağ oluşturma 

Zaten bir VNet'iniz varsa ayarların VPN ağ geçidi tasarımınızla uyumlu olduğunu doğrulayın. Diğer ağlarla çakışabilecek herhangi bir alt ağ olup olmadığına özellikle dikkat edin. Çakışan alt ağlarınız varsa bağlantınız düzgün şekilde gerçekleşmeyebilir. VNet'iniz doğru ayarlarla yapılandırıldıysa [DNS sunucusu belirtme](#dns) bölümündeki adımları uygulamaya başlayabilirsiniz.

### Sanal ağ oluşturmak için

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## 2. Ek adres alanı ve alt ağları ekleme

Ek adres alanı ve alt ağları VNet’iniz oluşturulduktan sonra ekleyebilirsiniz.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

## <a name="dns"></a>3. DNS sunucusu belirleme

Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız DNS sunucunuzu belirlerken bu [değerlere](#values) başvurun.

### DNS sunucusu belirlemek için

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## 4. Ağ geçidi alt ağı oluşturma

Sanal ağınızı bir ağ geçidine bağlamadan önce, bağlamak istediğiniz sanal ağ için ağ geçidi alt ağını oluşturmanız gerekir. Oluşturduğunuz ağ geçidi alt ağı *GatewaySubnet* olarak adlandırılmalıdır; aksi halde düzgün şekilde çalışmaz. 

Bazı yapılandırmalardaki ağ geçidi alt ağı ön adı, havuzda ihtiyaç duyulan IP adresleri sayısını sağlamak için /28’lik veya daha geniş bir alt ağı gerektirir.  Bunun anlamı ağ geçidi alt ağı ön adı /28, /27, /26 vb. şeklinde olması gerekir. Gelecekteki olası ek yapılandırmaları gerçekleştirebilmek için bu noktada daha geniş bir alt ağ oluşturmak isteyebilirsiniz.

Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız ağ geçidi alt ağınızı oluştururken bu [değerlere](#values) başvurun.

### Bir ağ geçidi alt ağı oluşturmak için

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## 5. Sanal ağ geçidi oluşturma

Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız ağ geçidinizi oluştururken bu [değerlere](#values) başvurun.

### Bir sanal ağ geçidi oluşturmak için

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## 6. Yerel ağ geçidi oluşturma

*Yerel ağ geçidi* şirket içi konumunuz anlamına gelir. Azure'ın başvurmak üzere kullanabilmesi için yerel ağ geçidinize bir ad verin. 

Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız yerel sitenizi eklerken bu [değerlere](#values) başvurun.

### Bir yerel ağ geçidi oluşturmak için

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## 7. VPN cihazınızı yapılandırma

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## 8. Siteden Siteye bağlantı kurma

Sanal ağ geçidiniz ile VPN cihazınız arasında Siteden Siteye VPN bağlantısı oluşturun. Değerlerin kendinizinkilerle değiştirildiğinden emin olun. Paylaşılan anahtar, VPN cihazınızın yapılandırması için kullandığınız değerin aynısı olmalıdır. 

Bu bölüme başlamadan önce sanal ağa ait ağ geçidi ve yerel ağa ait ağ geçidinizin oluşturulmasının tamamlandığından emin olun. Bu yapılandırmayı bir alıştırma olarak oluşturuyorsanız bağlantınızı oluştururken bu [değerlere](#values) başvurun.

### Bir VPN bağlantısını oluşturmak için


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## 9. VPN bağlantısını doğrulama

VPN bağlantınızı portaldan veya PowerShell kullanarak doğrulayabilirsiniz.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## Sonraki adımlar

- Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. sanal makineleri [öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/virtual-machines).

- BGP hakkında bilgi edinmek için [BGP’ye Genel Bakış](vpn-gateway-bgp-overview.md) ve [BGP’yi yapılandırma](vpn-gateway-bgp-resource-manager-ps.md) makalelerine bakın.



<!--HONumber=Aug16_HO4-->


