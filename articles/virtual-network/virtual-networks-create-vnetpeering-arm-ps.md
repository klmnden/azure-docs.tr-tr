<properties
   pageTitle="PowerShell cmdlet’lerini kullanarak VNet Eşlemesi oluşturma | Microsoft Azure"
   description="Resource Manager’da Azure portalını kullanarak bir sanal ağ oluşturmayı öğrenin."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="telmos"/>

# PowerShell cmdlet’lerini kullanarak VNet Eşlemesi oluşturma

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

PowerShell kullanarak VNet eşlemesi oluşturmak için lütfen aşağıdaki adımları uygulayın:

1. Daha önce Azure PowerShell kullanmadıysanız, [Azure PowerShell’i Yükleme ve Yapılandırma](../powershell-install-configure.md) sayfasına gidin ve Azure’da oturum açıp aboneliğinizi seçmek için talimatları sonuna kadar uygulayın.

Note: VNet eşlemesini yönetmeye yönelik PowerShell cmdlet’i [Azure PowerShell 1.6](http://www.powershellgallery.com/packages/Azure/1.6.0) ile birlikte gönderilir.

2. Sanal ağ nesnelerini okuyun:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

    
3. VNet eşlemesi oluşturmak için her biri bir yönde olan iki bağlantı oluşturmanız gerekir. Aşağıdaki adımda ilk olarak VNet1’den VNet2’ye VNet eşleme bağlantısı oluşturulmaktadır:

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.id 

Çıktı şunu gösterir:

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Bu adımda VNet2’den VNet1’ye VNet eşleme bağlantısı oluşturulmaktadır

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.id 

Çıktı şunu gösterir:

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Bu VNET eşleme bağlantısı oluşturulduktan sonra. Bağlantı durumunu aşağıdaki gibi görebilirsiniz:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

Çıktı şunu gösterir:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null


VNet eşlemesi için birkaç yapılandırılabilir özellik vardır:

|Seçenek|Açıklama|Varsayılan|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|Eş VNet’in adres alanının Virtual_network Etiketine eklenip eklenmeyeceği|Evet|
|AllowForwardedTraffic|Eşlenen VNet’ten kaynaklanmayan trafiğin kabul edilmesine veya bırakılmasına izin verir|Hayır|
|AllowGatewayTransit|Eş VNet’in VNet ağ geçidinizi kullanmasına izin verir|Hayır|
|UseRemoteGateways|Eşinizin VNet ağ geçidini kullanır. Eş VNet için yapılandırılmış bir ağ geçidi olmalı ve AllowGatewayTransit seçilmelidir. Yapılandırılmış bir ağ geçidiniz varsa bu seçeneği kullanamazsınız|Hayır|

VNet eşlemesindeki her bağlantı yukarıdaki özellikleri içerir. Örneğin, AllowVirtualNetworkAccess seçeneğini VNet1’den VNet2’ye VNet eşlemesi için True olarak ve diğer yöndeki VNet eşleme bağlantısı için False olarak ayarlayabilirsiniz.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

Ayrıca Get-AzureRmVirtualNetworkPeering seçeneğini çalıştırarak değişiklikten sonra özellik değerini iki kez denetleyebilirsiniz.  Çıktıda, yukarıdaki cmdlet’leri çalıştırdıktan sonra AllowForwardedTraffic seçeneğinin True olarak değiştiğini görebilirsiniz. 

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

Bu senaryoda eşleme oluşturulduktan sonra her iki VNet’in herhangi bir sanal makinesinden herhangi bir sanal makinesine bağlantılar başlatabilmeniz gerekir. Varsayılan olarak, AllowVirtualNetworkAccess değeri True şeklindedir ve VNet Eşlemesi VNet'ler arasındaki iletişime izin vermek için uygun ACL'leri sağlar. Ancak, örneğin belirli alt ağlar veya sanal makineler arasında iki sanal ağ arasındaki erişimi ayrıntılı olarak denetlemek amacıyla bağlantıyı engellemeye yönelik NSG kuralları uygulayabilirsiniz.  NSG kuralları oluşturma daha fazla bilgi için lütfen bu [makaleye](virtual-networks-create-nsg-arm-ps.md) bakın.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

PowerShell ile abonelikler arasında VNet eşlemesi oluşturmak için lütfen aşağıdaki adımları izleyin:

1. Abonelik A için Kullanıcı A ayrıcalığıyla oturum açın ve şu cmdlet'i çalıştırın:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

Bu bir gereklilik değildir; istekler eşleştiği sürece kullanıcılar ilgili sanal ağları için eşleme isteklerini ayrı ayrı gönderse bile eşleme kurulabilir. Diğer VNet’in ayrıcalıklı kullanıcısının yerel VNet’e kullanıcı olarak eklenmesi kurulum yapmayı kolaylaştırır. 

2. Abonelik B için Kullanıcı B ayrıcalığıyla oturum açın ve şu cmdlet'i çalıştırın:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. Ardından Kullanıcı A’nın oturumunda aşağıdaki cmdlet’i çalıştırın:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess


4. Ardından Kullanıcı B’nin oturumunda aşağıdaki cmdlet’i çalıştırın:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Eşleme oluşturulduktan sonra VNet3’teki tüm sanal makineler VNet5’teki tüm sanal makinelerle iletişim kurabilir

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Bu senaryoda VNet eşlemesi oluşturmak için aşağıdaki PowerShell cmdlet’lerini çalıştırabilirsiniz ve özellikle eşleşen VNet adres alanının dışından gelen trafiğe izin vermek üzere AllowForwardedTraffic özelliğini VNET1’den HubVnet’e True olarak ayarlamanız gerekir. 

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.id

2. Eşleme kurulduktan sonra bu [makaleye](virtual-network-create-udr-arm-ps.md) bakabilir ve Kullanıcı Tanımlı Yönlendirme’yi (UDR) tanımlayarak, özelliklerini kullanmak üzere bir sanal gereç aracılığıyla VNet1 trafiğini yeniden yönlendirebilirsiniz. Yolda Sonraki Atlama adresini belirttiğinizde eş VNet HubVNet içindeki sanal gerecin IP adresine ayarlayabilirsiniz. Bir örneği aşağıda verilmiştir:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1


## VNet Eşlemesini Kaldırma

1.  VNet eşlemesini kaldırmak için şu cmdlet'i çalıştırmanız gerekir 

        Remove-AzureRmVirtualNetworkPeering  
    
    aşağıda gösterildiği gibi her iki bağlantı kaldırmak için:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2 
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2 

3. VNET eşlemesindeki bir bağlantıyı kaldırdıktan sonra eşleme bağlantı durumu bağlantı kesildi olarak değişir.
  
4. Bu durumda eşleme bağlantı durumu Başlatıldı olana kadar bağlantıyı yeniden oluşturamazsınız. VNET eşlemesini yeniden oluşturmadan önce her iki bağlantıyı da kaldırmanız önerilir. 



<!--HONumber=Aug16_HO1-->


