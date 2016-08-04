<properties
   pageTitle="Resource Manager dağıtım modelinde bir arada var olabilen Expressroute ve Siteden Siteye VPN bağlantılarını yapılandırma | Microsoft Azure"
   description="Bu makalede Resource Manager modeli için bir arada var olabilen ExpressRoute ve bir Siteden Siteye VPN bağlantısını nasıl yapılandıracağınız anlatılmaktadır."
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/04/2016"
   ms.author="charleywen"/>

# Resource Manager dağıtım modeli için aynı anda var olabilen ExpressRoute ve Siteden Siteye bağlantılarını yapılandırma

> [AZURE.SELECTOR]
- [PowerShell - Resource Manager](expressroute-howto-coexist-resource-manager.md)
- [PowerShell - Klasik](expressroute-howto-coexist-classic.md)

Siteden Siteye VPN ve ExpressRoute yapılandırma yeteneğine sahip olmanın çeşitli avantajları vardır. ExressRoute için güvenli bir yük devretme yolu olarak Siteden Siteye VPN yapılandırabilir veya ExpressRoute aracılığıyla bağlanmayan sitelere bağlanmak için Siteden Siteye VPN’ler kullanabilirsiniz. Bu makalede iki senaryo için de yapılandırma adımları verilmektedir. Bu tablo Resource Manager dağıtım modelleri için geçerlidir. Bu yapılandırma Azure portalında kullanılamaz.


**Azure dağıtım modelleri hakkında**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

>[AZURE.IMPORTANT] Aşağıdaki yönergeleri izlemeden önce ExpressRoute bağlantı hatlarının önceden yapılandırılmış olması gerekir. Aşağıdaki adımları izlemeden önce [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md) ve [yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md) kılavuzlarını izlediğinizden emin olun.

## Sınırlar ve sınırlamalar

- **Geçiş yönlendirme desteklenmiyor:** Siteden Siteye VPN aracılığıyla bağlanan yerel ağınız ve ExpressRoute aracılığıyla bağlanan yerel ağınız arasında (Azure üzerinden) yönlendirme yapamazsınız.
- **Zorlanan tünel Siteden Siteye VPN ağ geçidinde etkinleştirilemez:** İnternet’i hedefleyen tüm trafiği yalnızca ExpressRoute aracılığıyla şirket içi ağınıza geri dönmeye zorlayabilirsiniz. 
- **Yalnızca standart veya yüksek performanslı ağ geçitleri:** ExpressRoute ağ geçidi ve Siteden Siteye VPN ağ geçidi için standart veya yüksek performanslı bir ağ geçidi kullanmanız gerekir. Ağ geçidi SKU’ları hakkında bilgi için bkz. [Ağ geçidi SKU’ları](../vpn-gateway/vpn-gateway-about-vpngateways.md).
- **Yalnızca yol tabanlı VPN ağ geçidi:** Yol tabanlı bir VPN ağ geçidi kullanmanız gerekir. Yol tabanlı VPN ağ geçidi hakkında bilgi için bkz. [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).
- **Statik yol gereksinimi:** Yerel ağınız ExpressRoute ve bir Siteden Siteye VPN’e bağlıysa, Siteden Siteye VPN bağlantısını genel İnternet’e yönlendirmek için yerel ağınızda bir statik yol yapılandırılmış olmalıdır.
- **ExpressRoute ağ geçidi önce yapılandırılmalıdır:** ExpressRoute ağ geçidini ilk olarak, Siteden Siteye VPN ağ geçidini eklemeden önce oluşturmanız gerekir.


## Yapılandırma tasarımları

### Siteden siteye VPN’i ExpressRoute için bir yük devretme yolu olarak yapılandırma

Siteden siteye bir VPN bağlantısını ExpressRoute için yedek olarak yapılandırabilirsiniz. Bu yalnızca Azure özel eşleme yoluna bağlı sanal ağlar için geçerlidir. Azure ortak ve Microsoft eşlemeleri aracılığıyla erişilebilen hizmetler için VPN tabanlı yük devretme çözümü yoktur. ExpressRoute bağlantı hattı her zaman birincil bağlantıdır. Veriler yalnızca ExpressRoute bağlantı hattı başarısız olursa, Siteden Siteye VPN üzerinden akar. 

![Bir arada var olma](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### ExpressRoute aracılığıyla bağlanılmayan sitelere bağlanmak için Siteden Siteye VPN yapılandırma

Ağınızı bazı sitelerin Azure’a Siteden Siteye VPN üzerinden doğrudan ve bazı sitelerin ExpressRoute üzerinden bağlanması için yapılandırabilirsiniz. 

![Bir arada var olma](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

>[AZURE.NOTE] Bir sanal ağı geçiş yönlendiricisi olarak yapılandıramazsınız.

## Kullanılacak adımları seçme

Bir arada var olabilen bağlantılar yapılandırmak için seçebileceğiniz iki farklı yordam kümesi vardır. Seçtiğiniz yapılandırma yordamı, bağlanmak istediğiniz mevcut bir sanal ağ olup olmadığına veya yeni bir sanal ağ oluşturmak isteyip istememenize bağlıdır.


- Bir VNet’im yok ve bir tane oluşturmam gerekiyor.
    
    Zaten bir sanal ağınız yoksa, bu yordam Resource Manager dağıtım modelini kullanarak yeni bir sanal ağ oluşturmak ve yeni ExpressRoute ve Siteden Siteye VPN bağlantıları oluşturmak için size yol gösterir. Yapılandırmak için, makalenin [Yeni bir sanal ağ ve bir arada var olabilen bağlantılar oluşturmak için](#new) bölümündeki adımları izleyin.

- Zaten bir Resource Manager dağıtım modeli VNet’im var.

    Mevcut bir Siteden Siteye VPN bağlantısı veya ExpressRoute bağlantısına sahip bir sanal ağınız zaten olabilir. [Zaten olan bir VNet için aynı anda mevcut bağlantılar yapılandırma](#add) bölümü, ağ geçidini silme ve ardından yeni ExpressRoute ve Siteden Siteye VPN bağlantıları oluşturma işlemi boyunca size yol gösterir. Yeni bir bağlantı oluşturulurken adımların belirli bir sırayla tamamlanması gerektiğine dikkat edin. Ağ geçitleriniz ve bağlantılarınızı oluşturmak için diğer makalelerdeki yönergeleri kullanmayın.

    Bu yordamda, bir arada var olabilen bağlantılar oluşturmak, ağ geçidinizi silmenizi ve ardından yeni ağ geçitlerini yapılandırmanızı gerektirir. Bu, ağ geçidiniz ve bağlantıları silip yeniden oluştururken şirket içi ve dışı bağlantılarınız için kapalı kalma süresi yaşayacağınız ancak VM’leriniz veya hizmetlerinizi yeni bir sanal ağa geçirmeniz gerekmeyeceği anlamına gelir. VM'leriniz ve hizmetleriniz bunu yapmak için yapılandırılmışsa, ağ geçidi yapılandırması sırasında yük dengeleyici üzerinden iletişim kurmaya devam eder.


## <a name="new"></a>Yeni bir sanal ağ ve bir arada var olabilen bağlantılar oluşturmak için

Bu yordamda, bir VNet oluşturma ve bir arada var olabilen Siteden Siteye ve ExpressRoute bağlantıları oluşturma adım adım açıklanmıştır.
    
1. Azure PowerShell cmdlet’lerinin en yeni sürümünü yüklemeniz gerekir. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](../powershell-install-configure.md). Bu yapılandırma için kullanacağınız cmdlet'lerin tanıdıklarınızdan biraz farklı olabileceğini unutmayın. Bu yönergelerde belirtilen cmdlet'leri kullandığınızdan emin olun.

2. Hesabınızda oturum açın ve ortamı ayarlayın.
    
        login-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
        $location = "Central US"
        $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location

3. Ağ Geçidi Alt Ağı içeren bir sanal ağ oluşturun. Sanal ağ yapılandırması hakkında daha fazla bilgi için bkz. [Azure Virtual Network yapılandırma](../virtual-network/virtual-networks-create-vnet-arm-ps.md).

    >[AZURE.IMPORTANT] Ağ Geçidi Alt Ağı /27 veya daha kısa bir ön ek (örneğin /26 veya /25) olmalıdır.
    
    Yeni bir VNet oluşturun.

        $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16" 

    Alt ağlar ekleyin.

        Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
        Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"

    VNet yapılandırmasını kaydedin.

        $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

4. <a name="gw"></a>Bir ExpressRoute ağ geçidi oluşturun. ExpressRoute ağ geçidi yapılandırması hakkında daha fazla bilgi için bkz. [ExpressRoute ağ geçidi yapılandırması](expressroute-howto-add-gateway-resource-manager.md). The GatewaySKU değeri *Standard* veya *HighPerformance* olmalıdır.

        $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
        $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
        $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard 

5. ExpressRoute ağ geçidini ExpressRoute bağlantı hattına bağlayın. Bu adım tamamlandıktan sonra, ExpressRoute aracılığıyla şirket içi ağınız ve Azure arasında bağlantı kurulur. Bağlantı işlemi hakkında daha fazla bilgi için bkz.[VNets’i ExpressRoute’a bağlama](expressroute-howto-linkvnet-arm.md).

        $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
        New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute

6. <a name="vpngw"></a>Ardından, Siteden Siteye VPN ağ geçidinizi oluşturun. VPN ağ geçidi yapılandırması hakkında daha fazla bilgi için bkz. [VNet’ten VNet’e bağlantı yapılandırma](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md). The GatewaySKU değeri *Standard* veya *HighPerformance* olmalıdır. VpnType değeri *RouteBased* olmalıdır.

        $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
        $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
        New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"

7. Bir yerel site VPN ağ geçidi varlığı oluşturun. Bu komut, şirket içi VPN ağ geçidinizi yapılandırmaz. Azure VPN ağ geçidinin bağlanabilmesi için ortak IP ve şirket içi adres alanı gibi yerel ağ geçidi ayarlarını paylaşmanıza olanak tanır. 
    >[AZURE.NOTE] Yerel ağınızda birden çok yol varsa, hepsini bir dizi olarak geçirebilirsiniz.  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")  

        $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix '10.100.0.0/16'

8. Yerel VPN cihazınızı yeni Azure VPN ağ geçidine bağlanmak için yapılandırın. VPN cihazını yapılandırma hakkında daha fazla bilgi için bkz. [VPN Cihazı Yapılandırma](../vpn-gateway/vpn-gateway-about-vpn-devices.md).

9. Azure’da Siteden Siteye ağ geçidini yerel ağ geçidine bağlayın.

        $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
        New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>


## <a name="add"></a>Zaten olan bir VNet için aynı anda mevcut bağlantıları yapılandırmak için

Zaten bir sanal ağınız varsa, ağ geçidi alt ağ boyutunu kontrol edin. Ağ geçidi alt ağı /28 veya /29 ise, önce sanal ağ geçidi silmeniz ve ağ geçidi alt ağı boyutunu artırmanız gerekir. Bu bölümdeki adımlar bunu nasıl yapacağınızı gösterir.

Ağ geçidi alt ağı /27 veya daha büyükse ve sanal ağ ExpressRoute üzerinden bağlanıyorsa, aşağıdaki adımları atlayarak önceki bölümdeki ["6. Adım - Siteden Siteye VPN ağ geçidi oluşturma"](#vpngw) bölümüne geçebilirsiniz. 

>[AZURE.NOTE] Mevcut ağ geçidini sildiğinizde, bu yapılandırma üzerinde çalışırken, yerel şirket sanal ağ bağlantısını kaybeder. 

1. Azure PowerShell cmdlet’lerinin en yeni sürümünü yüklemeniz gerekir. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](../powershell-install-configure.md). Bu yapılandırma için kullanacağınız cmdlet'lerin tanıdıklarınızdan biraz farklı olabileceğini unutmayın. Bu yönergelerde belirtilen cmdlet'leri kullandığınızdan emin olun. 

2. Mevcut ExpressRoute veya Siteden Siteye VPN ağ geçidini silin. 

        Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>

3. Ağ Geçidi Alt Ağını silin.
        
        $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> 
        Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet

4. /27 veya daha büyük bir Ağ Geçidi Alt Ağı ekleyin.
    >[AZURE.NOTE] Sanal ağınızda ağ geçidi alt ağı boyutunu artırmak için yeterli IP adresi kalmadıysa, daha fazla IP adresi alanı eklemeniz gerekir.

        $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
        Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"

    VNet yapılandırmasını kaydedin.

        $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

5. Bu noktada, hiçbir ağ geçidi olmayan bir VNet’e sahip olursunuz. Yeni ağ geçitleri oluşturmak ve bağlantılarınızı tamamlamak için, önceki adım kümesinde bulabileceğiniz [4. Adım - Bir ExpressRoute ağ geçidi oluşturma](#gw) bölümüyle devam edebilirsiniz.

## VPN ağ geçidine noktadan siteye yapılandırması eklemek için
Bir arada var olan kurulumda VPN ağ geçidinize Noktadan Siteye yapılandırması eklemek için aşağıdaki adımları izleyebilirsiniz.

1. VPN İstemcisi adres havuzunu ekleyin. 

        $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
        Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"

2. VPN ağ geçidiniz için VPN kök sertifikasını Azure’a yükleyin. Bu örnekte, kök sertifikasının aşağıdaki PowerShell cmdlet’lerinin çalıştığı yerel makinede saklandığı varsayılır. 

        $p2sCertFullName = "RootErVpnCoexP2S.cer"
        $p2sCertMatchName = "RootErVpnCoexP2S"
        $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName}
        if ($p2sCertToUpload.count -eq 1){
            write-host "cert found"
        } else {
            write-host "cert not found"
            exit
        } 
        $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData)
        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData

Noktadan Siteye VPN hakkında daha fazla bilgi içini bkz. [Noktadan Siteye bağlantı yapılandırma](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).

## Sonraki adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).



<!----HONumber=Jun16_HO2-->


