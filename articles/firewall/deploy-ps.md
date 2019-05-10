---
title: Dağıtma ve Azure Azure PowerShell kullanarak güvenlik duvarı yapılandırma
description: Bu makalede, dağıtma ve Azure için Azure PowerShell kullanarak güvenlik duvarı yapılandırma hakkında bilgi edinin.
services: firewall
author: vhorne
ms.service: firewall
ms.date: 4/10/2019
ms.author: victorh
ms.openlocfilehash: 7c30e0aa0ae9735f5d08e1a2c4d6e6d36d778e27
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410237"
---
# <a name="deploy-and-configure-azure-firewall-using-azure-powershell"></a>Dağıtma ve Azure Azure PowerShell kullanarak güvenlik duvarı yapılandırma

Giden ağ erişimini denetleme, genel ağ güvenlik planının önemli bir parçasıdır. Örneğin, web sitelerine erişimi sınırlamak isteyebilirsiniz. Veya giden IP adresleri ve erişilebilen bağlantı noktalarını sınırlamak isteyebilirsiniz.

Azure Güvenlik Duvarı, Azure alt ağından giden ağ erişimini denetlemenin bir yoludur. Azure Güvenlik Duvarı ile şunları yapılandırabilirsiniz:

* Bir alt ağdan erişilebilen tam etki alanı adlarını (FQDN) tanımlayan uygulama kuralları.
* Kaynak adres, protokol, hedef bağlantı noktası ve hedef adresini tanımlayan ağ kuralları.

Ağ trafiğinizi güvenlik duvarından alt ağın varsayılan ağ geçidi olarak yönlendirdiğinizde ağ trafiği yapılandırılan güvenlik duvarı kurallarına tabi tutulur.

Bu makale için kolay dağıtım için üç alt ağ ile Basitleştirilmiş tek bir VNet oluşturun. Üretim dağıtımları için bir [merkez ve ışınsal modelini](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) , Güvenlik Duvarı'nı kendi Vnet'ine olduğu önerilir. Bir veya daha fazla alt ağ ile aynı bölgedeki eşlenmiş Vnet'ler içindeki iş yükü sunucularıdır.

* **AzureFirewallSubnet** - güvenlik duvarı bu alt ağdadır.
* **Workload-SN**: İş yükü sunucusu bu alt ağda yer alır. Bu alt ağın ağ trafiği güvenlik duvarından geçer.
* **Jump-SN**: "Atlama" sunucusu bu alt ağda yer alır. Atlama sunucusu, Uzak Masaüstü ile bağlanabileceğiniz genel IP adresine sahiptir. Buradan iş yükü sunucusuna bağlanabilirsiniz (başka bir Uzak Masaüstü oturumuyla).

![Öğretici ağı altyapısı](media/tutorial-firewall-rules-portal/Tutorial_network.png)

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Test amaçlı ağ ortamı oluşturma
> * Güvenlik duvarı dağıtma
> * Varsayılan rota oluşturma
> * Www.google.com erişmesine izin vermek için bir uygulama kuralı yapılandırma
> * Dış DNS sunucularına erişime izin vermek için ağ kuralı yapılandırma
> * Güvenlik duvarını test etme

Tercih ederseniz, bu yordamı kullanarak tamamlayabilirsiniz [Azure portalında](tutorial-firewall-deploy-portal.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu yordam, PowerShell'i yerel olarak çalıştırmanızı gerektirir. Azure PowerShell Modülü yüklü olmalıdır. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-Az-ps). PowerShell sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `Connect-AzAccount` komutunu çalıştırın.

## <a name="set-up-the-network"></a>Ağı ayarlama

İlk olarak güvenlik duvarını dağıtmak için gerekli olan kaynakları içerecek bir kaynak grubu oluşturun. Ardından sanal ağı, alt ağları ve test sunucularını oluşturun.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Kaynak grubu dağıtımı için tüm kaynakları içerir.

```azurepowershell
New-AzResourceGroup -Name Test-FW-RG -Location "East US"
```

### <a name="create-a-vnet"></a>Sanal ağ oluşturma

Bu sanal ağ, üç alt sahiptir:

```azurepowershell
$FWsub = New-AzVirtualNetworkSubnetConfig -Name AzureFirewallSubnet -AddressPrefix 10.0.1.0/24
$Worksub = New-AzVirtualNetworkSubnetConfig -Name Workload-SN -AddressPrefix 10.0.2.0/24
$Jumpsub = New-AzVirtualNetworkSubnetConfig -Name Jump-SN -AddressPrefix 10.0.3.0/24
```

> [!NOTE]
> En düşük AzureFirewallSubnet alt /26 boyutudur.

Şimdi sanal ağı oluşturun:

```azurepowershell
$testVnet = New-AzVirtualNetwork -Name Test-FW-VN -ResourceGroupName Test-FW-RG `
-Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $FWsub, $Worksub, $Jumpsub
```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Şimdi atlama ve iş yükü sanal makinelerini oluşturup uygun alt ağlara yerleştirin.
İstendiğinde, sanal makine için bir kullanıcı adı ve parola yazın.

Srv hızlı sanal makine oluşturun.

```azurepowershell
New-AzVm `
    -ResourceGroupName Test-FW-RG `
    -Name "Srv-Jump" `
    -Location "East US" `
    -VirtualNetworkName Test-FW-VN `
    -SubnetName Jump-SN `
    -OpenPorts 3389 `
    -Size "Standard_DS2"
```

Bir iş yükü sanal makine ile genel bir IP adresi oluşturun.
İstendiğinde, sanal makine için bir kullanıcı adı ve parola yazın.

```azurepowershell
#Create the NIC
$NIC = New-AzNetworkInterface -Name Srv-work -ResourceGroupName Test-FW-RG `
 -Location "East US" -Subnetid $testVnet.Subnets[1].Id 

#Define the virtual machine
$VirtualMachine = New-AzVMConfig -VMName Srv-Work -VMSize "Standard_DS2"
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName Srv-Work -ProvisionVMAgent -EnableAutoUpdate
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2016-Datacenter' -Version latest

#Create the virtual machine
New-AzVM -ResourceGroupName Test-FW-RG -Location "East US" -VM $VirtualMachine -Verbose
```

## <a name="deploy-the-firewall"></a>Güvenlik duvarını dağıtma

Şimdi sanal ağa Güvenlik Duvarı'nı dağıtın.

```azurepowershell
# Get a Public IP for the firewall
$FWpip = New-AzPublicIpAddress -Name "fw-pip" -ResourceGroupName Test-FW-RG `
  -Location "East US" -AllocationMethod Static -Sku Standard
# Create the firewall
$Azfw = New-AzFirewall -Name Test-FW01 -ResourceGroupName Test-FW-RG -Location "East US" -VirtualNetworkName Test-FW-VN -PublicIpName fw-pip

#Save the firewall private IP address for future use

$AzfwPrivateIP = $Azfw.IpConfigurations.privateipaddress
$AzfwPrivateIP
```

Özel IP adresini not edin. Varsayılan rotayı oluştururken bu adresi kullanacaksınız.

## <a name="create-a-default-route"></a>Varsayılan rota oluşturma

BGP yol yaymayı devre dışı bir tablo oluşturma

```azurepowershell
$routeTableDG = New-AzRouteTable `
  -Name Firewall-rt-table `
  -ResourceGroupName Test-FW-RG `
  -location "East US" `
  -DisableBgpRoutePropagation

#Create a route
 Add-AzRouteConfig `
  -Name "DG-Route" `
  -RouteTable $routeTableDG `
  -AddressPrefix 0.0.0.0/0 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress $AzfwPrivateIP `
 | Set-AzRouteTable

#Associate the route table to the subnet

Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $testVnet `
  -Name Workload-SN `
  -AddressPrefix 10.0.2.0/24 `
  -RouteTable $routeTableDG | Set-AzVirtualNetwork
```

## <a name="configure-an-application-rule"></a>Uygulama kuralı yapılandırma

Uygulama kuralı www.google.com giden erişim sağlar.

```azurepowershell
$AppRule1 = New-AzFirewallApplicationRule -Name Allow-Google -SourceAddress 10.0.2.0/24 `
  -Protocol http, https -TargetFqdn www.google.com

$AppRuleCollection = New-AzFirewallApplicationRuleCollection -Name App-Coll01 `
  -Priority 200 -ActionType Allow -Rule $AppRule1

$Azfw.ApplicationRuleCollections = $AppRuleCollection

Set-AzFirewall -AzureFirewall $Azfw
```

Azure Güvenlik Duvarı'nda varsayılan olarak izin verilen altyapı FQDN'leri için yerleşik bir kural koleksiyonu bulunur. Bu FQDN'ler platforma özgüdür ve başka amaçlarla kullanılamaz. Daha fazla bilgi için bkz. [Altyapı FQDN'leri](infrastructure-fqdns.md).

## <a name="configure-a-network-rule"></a>Ağ kuralını yapılandırma

Ağ kuralı bağlantı noktası 53'ün (DNS) iki IP adreslerine giden erişime izin verir.

```azurepowershell
$NetRule1 = New-AzFirewallNetworkRule -Name "Allow-DNS" -Protocol UDP -SourceAddress 10.0.2.0/24 `
   -DestinationAddress 209.244.0.3,209.244.0.4 -DestinationPort 53

$NetRuleCollection = New-AzFirewallNetworkRuleCollection -Name RCNet01 -Priority 200 `
   -Rule $NetRule1 -ActionType "Allow"

$Azfw.NetworkRuleCollections = $NetRuleCollection

Set-AzFirewall -AzureFirewall $Azfw
```

### <a name="change-the-primary-and-secondary-dns-address-for-the-srv-work-network-interface"></a>**Srv-Work** ağ arabiriminin birincil ve ikincil DNS adresini değiştirme

Bu yordamda test amacıyla, sunucunun birincil ve ikincil DNS adreslerini yapılandırın. Bu genel bir Azure güvenlik duvarı gereksinim değildir.

```azurepowershell
$NIC.DnsSettings.DnsServers.Add("209.244.0.3")
$NIC.DnsSettings.DnsServers.Add("209.244.0.4")
$NIC | Set-AzNetworkInterface
```

## <a name="test-the-firewall"></a>Güvenlik duvarını test etme

Şimdi beklendiği gibi çalıştığını doğrulamak için Güvenlik Duvarı'nı test edin.

1. Özel IP adresini Not **Srv iş** sanal makine:

   ```
   $NIC.IpConfigurations.PrivateIpAddress
   ```

1. Bağlanmak için Uzak Masaüstü **Srv atlama** sanal makine ve oturum açın. Burada, bir Uzak Masaüstü Bağlantısı açın **Srv iş** özel IP adresi ve oturum açın.

3. Üzerinde **SRV iş**, bir PowerShell penceresi açın ve aşağıdaki komutları çalıştırın:

   ```
   nslookup www.google.com
   nslookup www.microsoft.com
   ```

   Her iki komut yanıtları, DNS sorguları güvenlik duvarı üzerinden alıyorsanız gösteren, döndürmelidir.

1. Aşağıdaki komutları çalıştırın:

   ```
   Invoke-WebRequest -Uri https://www.google.com
   Invoke-WebRequest -Uri https://www.google.com

   Invoke-WebRequest -Uri https://www.microsoft.com
   Invoke-WebRequest -Uri https://www.microsoft.com
   ```

   Www.google.com isteklerinin başarılı olması gerekir ve www.microsoft.com istekleri başarısız olması. Bu, güvenlik duvarı kurallarınız beklendiği gibi çalışıp çalışmadığını gösterir.

Şimdi güvenlik duvarı kuralları çalıştığını doğruladığınıza göre:

* Yapılandırılmış dış DNS sunucusunu kullanarak DNS adlarını çözümleyebilirsiniz.
* İzin verilen bir FQDN'ye göz atabilir ancak diğerlerine göz atamazsınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik Duvarı kaynaklarınızı korumak için sonraki öğreticiye veya artık gerekli değilse silin **Test FW RG** güvenlik duvarı ile ilgili tüm kaynakları silmek için kaynak grubu:

```azurepowershell
Remove-AzResourceGroup -Name Test-FW-RG
```

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure güvenlik duvarı günlüklerini izleyin](./tutorial-diagnostics.md)
