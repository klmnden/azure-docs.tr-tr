<properties
   pageTitle="Resource Manager Şablonları Kullanarak VNet Eşlemesi Oluşturma | Microsoft Azure"
   description="Resource Manager’da şablonları kullanarak bir sanal ağ eşlemesi oluşturmayı öğrenin."
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

# Resource Manager Şablonları Kullanarak VNet Eşlemesi Oluşturma

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Resource Manager Şablonlarını kullanarak VNet eşlemesi oluşturmak için lütfen aşağıdaki adımları uygulayın:

1. Daha önce Azure PowerShell kullanmadıysanız, [Azure PowerShell’i Yükleme ve Yapılandırma](../powershell-install-configure.md) sayfasına gidin ve Azure’da oturum açıp aboneliğinizi seçmek için talimatları sonuna kadar uygulayın.

Note: VNet eşlemesini yönetmeye yönelik PowerShell cmdlet’i [Azure PowerShell 1.6](http://www.powershellgallery.com/packages/Azure/1.6.0) ile birlikte gönderilir.

2. Aşağıdaki bölümde yukarıdaki senaryo temel alınarak VNet1’den VNet2’ye VNet eşleme bağlantısının tanımı gösterilmektedir. Buradaki içeriği bir dosyaya kopyalayın ve VNetPeeringVNet1.json dosyasına kaydedin

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }
    

3. Aşağıdaki bölümde yukarıdaki senaryo temel alınarak VNet2’den VNet1’e VNet eşleme bağlantısının tanımı gösterilmektedir.  Buradaki içeriği bir dosyaya kopyalayın ve VNetPeeringVNet2.json dosyasına kaydedin

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

Yukarıdaki şablonda görüldüğü gibi VNet eşlemesi için birkaç yapılandırılabilir özellik vardır:

|Seçenek|Açıklama|Varsayılan|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|Eş VNet’in adres alanının Virtual_network Etiketine eklenip eklenmeyeceği|Evet|
|AllowForwardedTraffic|Eşlenen VNet’ten kaynaklanmayan trafiğin kabul edilmesine veya bırakılmasına izin verir|Hayır|
|AllowGatewayTransit|Eş VNet’in VNet ağ geçidinizi kullanmasına izin verir|Hayır|
|UseRemoteGateways|Eşinizin VNet ağ geçidini kullanır. Eş VNet için yapılandırılmış bir ağ geçidi olmalı ve AllowGatewayTransit seçilmelidir. Yapılandırılmış bir ağ geçidiniz varsa bu seçeneği kullanamazsınız|Hayır|

VNet eşlemesindeki her bağlantı yukarıdaki özellikleri içerir. Örneğin, AllowVirtualNetworkAccess seçeneğini VNet1’den VNet2’ye VNet eşlemesi için True olarak ve diğer yöndeki VNet eşleme bağlantısı için False olarak ayarlayabilirsiniz. 


4. Şablon dosyasını dağıtmak için New-AzureRmResourceGroupDeployment cmdlet’ini çalıştırarak dağıtımı oluşturabilir veya güncelleştirebilirsiniz. Resource Manager şablonu kullanma hakkında daha fazla bilgi için lütfen bu [makaleye](../resource-group-template-deploy.md) bakın

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

> [AZURE.NOTE] lütfen kaynak grubu adını ve şablon dosyasını uygun şekilde değiştirin. 

Yukarıdaki senaryoyu temel alan bir örnek aşağıda verilmiştir:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

Çıktı şunu gösterir:

        DeploymentName      : VNetPeeringVNet1
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

Çıktı şunu gösterir:

        DeploymentName      : VNetPeeringVNet2
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. Dağıtım tamamlandıktan sonra eşleme durumunu görüntülemek için aşağıdaki cmdlet'i çalıştırabilirsiniz:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    Çıktı şunu gösterir:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : VNet101
        VirtualNetworkName  : VNet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Bu senaryoda eşleme oluşturulduktan sonra her iki VNet’in herhangi bir sanal makinesinden herhangi bir sanal makinesine bağlantılar başlatabilmeniz gerekir. Varsayılan olarak, AllowVirtualNetworkAccess değeri True şeklindedir ve VNet Eşlemesi VNet'ler arasındaki iletişime izin vermek için uygun ACL'leri sağlar. Ancak, örneğin belirli alt ağlar veya sanal makineler arasında iki sanal ağ arasındaki erişimi ayrıntılı olarak denetlemek amacıyla bağlantıyı engellemeye yönelik NSG kuralları uygulayabilirsiniz.  NSG kuralları oluşturma daha fazla bilgi için lütfen bu [makaleye](virtual-networks-create-nsg-arm-ps.md) bakın.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Abonelikler arasında VNet eşlemesi oluşturmak için lütfen aşağıdaki adımları izleyin:

1. Abonelik A için Kullanıcı A ayrıcalığıyla oturum açın ve şu cmdlet'i çalıştırın:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

Bu bir gereklilik değildir; istekler eşleştiği sürece kullanıcılar ilgili sanal ağları için eşleme isteklerini ayrı ayrı gönderse bile eşleme kurulabilir. Diğer VNet’in ayrıcalıklı kullanıcısının yerel VNet’e kullanıcı olarak eklenmesi kurulum yapmayı kolaylaştırır. 

2. Abonelik B için Kullanıcı B ayrıcalığıyla oturum açın ve şu cmdlet'i çalıştırın:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. Ardından Kullanıcı A’nın oturumunda aşağıdaki cmdlet’i çalıştırın

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    JSON dosyası bu şekilde tanımlanır.  
    
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }
   
4. Ardından Kullanıcı B’nin oturumunda aşağıdaki cmdlet’i çalıştırın

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all
   
   JSON dosyası bu şekilde tanımlanır:

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }
 
 Bu senaryoda eşleme oluşturulduktan sonra farklı aboneliklerde her iki VNet’in herhangi bir sanal makinesinden herhangi bir sanal makinesine bağlantılar başlatabilmeniz gerekir. 

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Bu senaryoda VNet eşlemesi oluşturmak için aşağıdaki örnek şablonu kullanabilirsiniz ve özellikle eşlenen VNet’teki Ağ Sanal gerecinin trafik gönderip almasına izin vermek üzere AllowForwardedTraffic özelliğini True olarak ayarlamanız gerekir. HubVNet’ten VNet1’e VNet eşlemesi oluşturmaya yönelik şablon aşağıda verilmiştir. AllowForwardedTraffic özelliği false olarak ayarlanır

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. VNet1’den HubVnet’e VNet eşlemesi oluşturmaya yönelik şablon aşağıda verilmiştir. AllowForwardedTraffic özelliği true olarak ayarlanır. 

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. Eşleme kurulduktan sonra bu [makaleye](virtual-network-create-udr-arm-ps.md) bakabilir ve Kullanıcı Tanımlı Yönlendirme’yi (UDR) tanımlayarak, özelliklerini kullanmak üzere bir sanal gereç aracılığıyla VNet1 trafiğini yeniden yönlendirebilirsiniz. Yolda Sonraki Atlama adresini belirttiğinizde eş VNet HubVNet içindeki sanal gerecin IP adresine ayarlayabilirsiniz


<!--HONumber=Aug16_HO1-->


