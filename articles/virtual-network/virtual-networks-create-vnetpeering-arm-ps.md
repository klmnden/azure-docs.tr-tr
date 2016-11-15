---
title: "PowerShell cmdlet’lerini kullanarak VNet Eşlemesi oluşturma | Microsoft Belgeleri"
description: "Resource Manager’da Azure portalını kullanarak bir sanal ağ oluşturmayı öğrenin."
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
tags: azure-resource-manager
ms.assetid: dac579bd-7545-461a-bdac-301c87434c84
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: narayanannamalai; annahar
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 348b23b277c80867f600a408736e13b8ceb665f4


---
# <a name="create-vnet-peering-using-powershell-cmdlets"></a>PowerShell cmdlet’lerini kullanarak VNet Eşlemesi oluşturma
[!INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[!INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

PowerShell kullanarak VNet eşlemesi oluşturmak için lütfen aşağıdaki adımları uygulayın:

1. Daha önce Azure PowerShell kullanmadıysanız, [Azure PowerShell’i Yükleme ve Yapılandırma](../powershell-install-configure.md) sayfasına gidin ve Azure’da oturum açıp aboneliğinizi seçmek için talimatları sonuna kadar uygulayın.

> [!NOTE]
> VNet eşlemesini yönetmeye yönelik PowerShell cmdlet’i [Azure PowerShell 1.6](http://www.powershellgallery.com/packages/Azure/1.6.0) ile birlikte gönderilir.
> 
> 

1. Sanal ağ nesnelerini okuyun:
   
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2
2. VNet eşlemesi oluşturmak için iki (her yön için bir tane) bağlantı oluşturmanız gerekir. Aşağıdaki adımda ilk olarak VNet1'den VNet2'ye yönelik bir VNet eşleme bağlantısı oluşturulmaktadır:
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id
   
    Çıktı şunu gösterir:
   
        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : vnet101
        VirtualNetworkName    : vnet1
        PeeringState        : Initiated
        ProvisioningState    : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic    : False
        AllowGatewayTransit    : False
        UseRemoteGateways    : False
        RemoteGateways        : null
        RemoteVirtualNetworkAddressSpace : null
3. Bu adımda VNet2'den VNet1'e yönelik bir VNet eşleme bağlantısı oluşturulmaktadır:
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id
   
    Çıktı şunu gösterir:
   
        Name            : LinkToVNet1
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : vnet101
        VirtualNetworkName    : vnet2
        PeeringState        : Connected
        ProvisioningState    : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic    : False
        AllowGatewayTransit    : False
        UseRemoteGateways    : False
        RemoteGateways        : null
        RemoteVirtualNetworkAddressSpace : null
4. VNet eşleme bağlantısı oluşturulduktan sonra bağlantı durumunu aşağıda görebilirsiniz:
   
        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2
   
    Çıktı şunu gösterir:
   
        Name            : LinkToVNet2
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : vnet101
        VirtualNetworkName    : vnet1
        PeeringState        : Connected
        ProvisioningState    : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null
   
    VNet eşlemesi için birkaç yapılandırılabilir özellik vardır:
   
   | Seçenek | Açıklama | Varsayılan |
   |:--- |:--- |:--- |
   | AllowVirtualNetworkAccess |Eş VNet’in adres alanının Virtual_network Etiketine eklenip eklenmeyeceği |Evet |
   | AllowForwardedTraffic |Eşlenen sanal ağdan gelmeyen trafiğin kabul edilip edilmeyeceği |Hayır |
   | AllowGatewayTransit |Eş VNet’in VNet ağ geçidinizi kullanmasına izin verir |Hayır |
   | UseRemoteGateways |Eşinizin VNet ağ geçidini kullanır. Eş VNet'in yapılandırılmış bir ağ geçidi olmalı ve AllowGatewayTransit seçili olmalıdır. Yapılandırılmış bir ağ geçidiniz varsa bu seçeneği kullanamazsınız |Hayır |
   
    VNet eşlemesindeki her bağlantı yukarıdaki özellikleri içerir. Örneğin, AllowVirtualNetworkAccess seçeneğini VNet1'den VNet2'ye yönelik bir VNet eşlemesi için True olarak ve diğer yöndeki VNet eşleme bağlantısı için False olarak ayarlayabilirsiniz.
   
        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2
   
    Get-AzureRmVirtualNetworkPeering seçeneğini çalıştırarak değişiklikten sonra özellik değerini iki kez denetleyebilirsiniz. Çıktıda, yukarıdaki cmdlet’leri çalıştırdıktan sonra AllowForwardedTraffic seçeneğinin True olarak ayarlandığını görebilirsiniz.
   
        Name            : LinkToVNet2
        Id            : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : vnet101
        VirtualNetworkName    : vnet1
        PeeringState        : Connected
        ProvisioningState    : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic        : True
        AllowGatewayTransit        : False
        UseRemoteGateways        : False
        RemoteGateways        : null
        RemoteVirtualNetworkAddressSpace : null
   
    Bu senaryoda eşleme oluşturulduktan sonra her iki VNet’in herhangi bir sanal makinesinden herhangi bir sanal makinesine bağlantılar başlatabilmeniz gerekir. Varsayılan olarak, AllowVirtualNetworkAccess değeri True'dur ve VNet eşlemesi, VNet'ler arasındaki iletişime izin vermek için uygun ACL'leri sağlar. Ancak yine de iki sanal ağ arasındaki erişimi ayrıntılı olarak denetlemek amacıyla belirli alt ağlar ve sanal makineler arasındaki bağlantıyı engellemek için ağ güvenlik grubu (NSG) kuralları uygulayabilirsiniz.  NSG kuralları oluşturma hakkında daha fazla bilgi için lütfen bu [makaleye](virtual-networks-create-nsg-arm-ps.md) bakın.

[!INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

PowerShell kullanarak abonelikler arasında VNet eşlemesi oluşturmak için lütfen aşağıdaki adımları uygulayın:

1. A Aboneliği için ayrıcalıklı A kullanıcısının hesabıyla Azure'da oturum açın ve şu cmdlet'i çalıştırın:
   
        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5
   
    Bu zorunlu değildir; istekler eşleştiği sürece kullanıcılar ilgili sanal ağları için ayrı ayrı eşleme isteğinde bulunsa da eşleme oluşturulabilir. Diğer VNet'in ayrıcalıklı bir kullanıcısının yerel VNet'e kullanıcı olarak eklenmesi kurulumu kolaylaştırır.
2. B Aboneliği için ayrıcalıklı B kullanıcısının hesabıyla Azure'da oturum açın ve şu cmdlet'i çalıştırın:
   
        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3
3. A Kullanıcısının oturumunda aşağıdaki cmdlet'i çalıştırın:
   
        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess
4. B Kullanıcısının oturumunda aşağıdaki cmdlet'i çalıştırın:
   
        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess
5. Eşleme oluşturulduktan sonra VNet3'teki tüm sanal makineler, VNet5'teki tüm sanal makinelerle iletişim kurabilir.

[!INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Bu senaryoda VNet eşlemesini oluşturmak için aşağıdaki PowerShell cmdlet'lerini çalıştırabilirsiniz.  AllowForwardedTraffic özelliğini True olarak ayarlamanız ve VNET1'i HubVNet'e bağlamanız gerekir. Bu, eşleme yapılan sanal ağ adres alanının dışından gelen trafiğe izin verir.
   
        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id
2. Eşleme gerçekleştikten sonra bu [makaleye](virtual-network-create-udr-arm-ps.md) bakabilir ve kullanıcı tanımlı yol (UDR) tanımlayarak, özelliklerini kullanmak üzere bir sanal gereç aracılığıyla VNet1 trafiğini yeniden yönlendirebilirsiniz. Yolda bir sonraki atlama adresini belirttiğinizde bu adresi VNet - HubVNet eşlemesindeki sanal gerecin IP adresi olarak ayarlayabilirsiniz. Bir örneği aşağıda verilmiştir:
   
        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5
   
        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route
   
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1
   
        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable
   
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[!INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

PowerShell'de klasik bir sanal ağ ile Azure Resource Manager sanal ağı arasında bir sanal ağ eşlemesi oluşturmak için şu adımları uygulayın:

1. **VNET1** (Azure Resource Manager sanal ağı) için sanal ağı nesnesini şu şekilde okuyun:
   
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
2. Bu senaryoda VNet eşlemesi oluşturmak için başta **VNET1**’den **VNET2**’ye bağlantı olmak üzere yalnızca bir bağlantı gereklidir. Bu adım klasik sanal ağın kaynak kimliğini bilmeyi gerektirir. Kaynak grubu kimlik biçimi şu şekilde görünür:
   
        /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}
   
    SubscriptionID, ResourceGroupName ve VirtualNetworkName değerlerini uygun adlarla değiştirdiğinizden emin olun.
   
    Bunu yapmak için aşağıdaki adımları izleyin:
   
        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2
3. VNet eşleme bağlantısı oluşturulduktan sonra bağlantı durumunu aşağıdaki çıktıda gösterildiği gibi görebilirsiniz:
   
        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-vnet-peering"></a>VNet Eşlemesini Kaldırma
1. Sanal ağ eşlemesini kaldırmak için şu cmdlet'i çalıştırmanız gerekir:
   
     Remove-AzureRmVirtualNetworkPeering  
   
     Her iki bağlantıyı da kaldırmak için aşağıdaki komutları kullanın:
   
     Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2   Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
2. VNET eşlemesindeki bir bağlantıyı kaldırdıktan sonra, eşleme bağlantısının durumu "bağlantı kesildi" olarak değişir. Bu durumda eşleme bağlantı durumu Başlatıldı olana kadar bağlantıyı yeniden oluşturamazsınız. VNet eşlemesini yeniden oluşturmadan önce her iki bağlantıyı da kaldırmanız önerilir.




<!--HONumber=Nov16_HO2-->


