---
title: Birden çok IP adresi için bir Azure sanal makineler - PowerShell | Microsoft Docs
description: PowerShell kullanarak bir sanal makineye birden çok IP adresi atama hakkında bilgi edinin. | Kaynak Yöneticisi
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: c44ea62f-7e54-4e3b-81ef-0b132111f1f8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/24/2017
ms.author: kumud;annahar
ms.openlocfilehash: ee6a2d36d88d9a80ba7e64819344f6cca56e47cd
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64730411"
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-powershell"></a>PowerShell kullanarak sanal makineler için birden çok IP adresi atama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Bu makalede, PowerShell kullanarak Azure Resource Manager dağıtım modeli sanal makine (VM) oluşturma açıklanmaktadır. Birden çok IP adresi Klasik dağıtım modeliyle oluşturulan kaynaklara atanamaz. Azure dağıtım modelleri hakkında daha fazla bilgi edinmek için [dağıtım modellerini anlama](../resource-manager-deployment-model.md) makalesi.

[!INCLUDE [virtual-network-multiple-ip-addresses-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Birden çok IP adresi ile VM oluşturma

Aşağıdaki adımları senaryoda açıklanan şekilde birden çok IP adresi ile VM örneği oluşturma açıklanmaktadır. Değişken değerleri, uygulamanız için gereken şekilde değiştirin.

1. Bir PowerShell komut istemi açın ve tek bir PowerShell oturumunda bu bölümün kalan adımları tamamlayın. PowerShell sürümünün yüklü ve yapılandırılmış yoksa bölümünde bulunan adımları tamamladığınızdan [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview) makalesi.
2. Oturum açma ile hesabınızı `Connect-AzAccount` komutu.
3. Değiştirin *myResourceGroup* ve *westus* adı ve seçtiğiniz konum. Bir kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

   ```powershell
   $RgName   = "MyResourceGroup"
   $Location = "westus"

   New-AzResourceGroup `
   -Name $RgName `
   -Location $Location
   ```

4. Kaynak grubu ile aynı konumda bir sanal ağ (VNet) ve alt ağ oluşturun:

   ```powershell

   # Create a subnet configuration
   $SubnetConfig = New-AzVirtualNetworkSubnetConfig `
   -Name MySubnet `
   -AddressPrefix 10.0.0.0/24

   # Create a virtual network
   $VNet = New-AzVirtualNetwork `
   -ResourceGroupName $RgName `
   -Location $Location `
   -Name MyVNet `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $subnetConfig

   # Get the subnet object
   $Subnet = Get-AzVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
   ```

5. Bir ağ güvenlik grubu (NSG) ve bir kural oluşturun. NSG, gelen ve giden kuralları kullanarak VM'yi korur. Bu durumda, bağlantı noktası 3389 için gelen masaüstü bağlantılarına izin veren bir gelen kuralı oluşturulur.

    ```powershell
    
    # Create an inbound network security group rule for port 3389

    $NSGRule = New-AzNetworkSecurityRuleConfig `
    -Name MyNsgRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow
    
    # Create a network security group
    $NSG = New-AzNetworkSecurityGroup `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyNetworkSecurityGroup `
    -SecurityRules $NSGRule
    ```

6. NIC için birincil IP yapılandırması tanımlayın Önceden tanımlanmış değer kullanmadıysanız, oluşturduğunuz alt ağ içinde geçerli bir adrese 10.0.0.4 değiştirin. Statik bir IP adresi atamadan önce zaten kullanımda olup olmadığını ilk doğrulayın önerilir. Komutu girdikten `Test-AzPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`. Çıkış adresi kullanılabilir değilse döndürür *True*. Bu uygun değilse, bir çıktı döndürür *False* ve kullanılabilir adresleri listesi. 

    Aşağıdaki komutlarda **değiştirin \<Değiştir-ile-your-benzersiz-adı > ile kullanmak için benzersiz bir DNS adı.** Bir Azure bölgesi içinde tüm genel IP adresleri arasında adı benzersiz olmalıdır. Bu isteğe bağlı bir parametredir. Yalnızca genel IP adresini kullanarak VM'ye bağlanmak isterseniz kaldırılabilir.

    ```powershell
    
    # Create a public IP address
    $PublicIP1 = New-AzPublicIpAddress `
    -Name "MyPublicIP1" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -DomainNameLabel <replace-with-your-unique-name> `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign the public IP address to it
    $IpConfigName1 = "IPConfig-1"
    $IpConfig1     = New-AzNetworkInterfaceIpConfig `
    -Name $IpConfigName1 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.4 `
    -PublicIpAddress $PublicIP1 `
    -Primary
    ```

    Bir NIC'ye birden fazla IP yapılandırması atadığınızda, bir yapılandırma olarak atanması gerekir *-birincil*.

    > [!NOTE]
    > Genel IP adreslerinin nominal bir ücreti vardır. IP adresi fiyatlandırması hakkında daha fazla bilgi edinmek için [IP adresi fiyatlandırması](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. Bir abonelikte kullanılan genel IP adresleri sayısına bir sınır yoktur. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesini okuyun.

7. İkincil NIC IP yapılandırmalarını tanımlayın Ekleyebilir veya yapılandırmaları gerekli olarak kaldırır. Her IP yapılandırması, özel IP adresi atanmış olmalıdır. Her yapılandırma, isteğe bağlı olarak atanmış bir genel IP adresine sahip olabilir.

    ```powershell
    
    # Create a public IP address
    $PublicIP2 = New-AzPublicIpAddress `
    -Name "MyPublicIP2" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign the public IP address to it
    $IpConfigName2 = "IPConfig-2"
    $IpConfig2     = New-AzNetworkInterfaceIpConfig `
    -Name $IpConfigName2 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.5 `
    -PublicIpAddress $PublicIP2
        
    $IpConfigName3 = "IpConfig-3"
    $IpConfig3 = New-AzNetworkInterfaceIpConfig `
    -Name $IPConfigName3 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.6
    ```

8. NIC oluşturun ve ona üç IP yapılandırmaları ilişkilendirin:

   ```powershell
   $NIC = New-AzNetworkInterface `
   -Name MyNIC `
   -ResourceGroupName $RgName `
   -Location $Location `
   -NetworkSecurityGroupId $NSG.Id `
   -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
   ```

   >[!NOTE]
   >Bu makalede bir NIC'e atanmış tüm yapılandırmalar da, VM'ye bağlı her NIC'ye birden fazla IP yapılandırması atayabilirsiniz. Birden çok NIC ile VM oluşturma konusunda bilgi edinmek için [birden çok NIC ile VM oluşturma](../virtual-machines/windows/multiple-nics.md) makalesi.

9. Aşağıdaki komutları girerek VM oluşturun:

    ```powershell
    
    # Define a credential object. When you run these commands, you're prompted to enter a username and password for the VM you're creating.
    $cred = Get-Credential
    
    # Create a virtual machine configuration
    $VmConfig = New-AzVMConfig `
    -VMName MyVM `
    -VMSize Standard_DS1_v2 | `
    Set-AzVMOperatingSystem -Windows `
    -ComputerName MyVM `
    -Credential $cred | `
    Set-AzVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
    Add-AzVMNetworkInterface `
    -Id $NIC.Id
    
    # Create the VM
    New-AzVM `
    -ResourceGroupName $RgName `
    -Location $Location `
    -VM $VmConfig
    ```

10. İşletim sisteminiz için adımları tamamlayarak VM işletim sistemine özel IP adresleri ekleme [ekleme IP adresleri için bir VM işletim sistemi](#os-config) bu makalenin. Genel IP adreslerine, işletim sistemine eklemeyin.

## <a name="add"></a>Bir VM'ye IP adresleri ekleme

Aşağıdaki adımları izleyerek Azure ağ arabirimi ile özel ve genel IP adresleri ekleyebilirsiniz. Aşağıdaki bölümlerde örneklerde, bir VM içinde açıklanan üç IP yapılandırmaları ile sahip olduğunuz varsayılmaktadır [senaryo](#scenario) bu makalede, ancak gerekli değildir, bunu.

1. Bir PowerShell komut istemi açın ve tek bir PowerShell oturumunda bu bölümün kalan adımları tamamlayın. PowerShell sürümünün yüklü ve yapılandırılmış yoksa bölümünde bulunan adımları tamamladığınızdan [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview) makalesi.
2. Aşağıdaki $Variables "values" NIC için IP adresi eklemek istediğiniz kaynak grubunu ve konumu NIC var. adı değiştirin:

   ```powershell
   $NicName  = "MyNIC"
   $RgName   = "MyResourceGroup"
   $Location = "westus"
   ```

   Sonra değiştirmek için aşağıdaki komutları girin, istediğiniz NIC adını bilmiyorsanız, önceki değişkenlerinin değerlerini değiştirin:

   ```powershell
   Get-AzNetworkInterface | Format-Table Name, ResourceGroupName, Location
   ```

3. Bir değişken oluşturun ve aşağıdaki komutu yazarak mevcut NIC'ye ayarlayın:

   ```powershell
   $MyNIC = Get-AzNetworkInterface -Name $NicName -ResourceGroupName $RgName
   ```

4. Aşağıdaki komutlar, değiştirme *MyVNet* ve *MySubnet* NIC'nin bağlı olduğu alt ağ ve VNet adları. NIC'nin bağlı olduğu sanal ağ ve alt nesneleri almak için komutları girin:

   ```powershell
   $MyVNet = Get-AzVirtualnetwork -Name MyVNet -ResourceGroupName $RgName
   $Subnet = $MyVnet.Subnets | Where-Object { $_.Name -eq "MySubnet" }
   ```

   NIC'nin bağlı olduğu sanal ağ veya alt ağ adı bilmiyorsanız, aşağıdaki komutu girin:

   ```powershell
   $MyNIC.IpConfigurations
   ```

   Aşağıdaki örnek çıktıya benzer bir metin çıktısında bulun:

   ```
   "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet/subnets/MySubnet"
   ```

    Bu çıkış, *MyVnet* vnet ve *MySubnet* NIC'nin bağlı olduğu alt ağ.

5. Gereksinimlerinize göre aşağıdaki bölümlerden birindeki adımları tamamlayın:

   **Özel bir IP adresi Ekle**

   Özel bir IP adresi bir NIC'ye eklemek için bir IP yapılandırması oluşturmanız gerekir. Aşağıdaki komut, bir yapılandırma 10.0.0.7 biçiminde bir statik IP adresi oluşturur. Statik bir IP adresi belirtirken, alt ağ için kullanılmayan bir adresi olması gerekir. İlk kullanılabilir girerek olduğundan emin olun adresine test önerilir `Test-AzPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` komutu. IP adresi kullanılabilir değilse, çıkış döndürür *True*. Bu uygun değilse, bir çıktı döndürür *False*ve kullanılabilir adresleri listesi.

   ```powershell
   Add-AzNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
   $MyNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
   ```

   Benzersiz yapılandırma adları ve özel IP adresleri (statik IP adresi ile yapılandırmaları) kullanarak gereksinim duyduğunuz kadar çok yapılandırmalarını oluşturun.

   İşletim sisteminiz için adımları tamamlayarak VM işletim sistemine özel IP adresini ekleyin [ekleme IP adresleri için bir VM işletim sistemi](#os-config) bu makalenin.

   **Genel IP adresi ekleme**

   Genel bir IP adresi, yeni bir IP yapılandırması veya mevcut bir IP yapılandırması için genel bir IP adresi kaynağı ilişkilendirerek eklenir. Gereksinim duyduğunuz kadar aşağıdaki bölümlerde birindeki adımları tamamlayın.

   > [!NOTE]
   > Genel IP adreslerinin nominal bir ücreti vardır. IP adresi fiyatlandırması hakkında daha fazla bilgi edinmek için [IP adresi fiyatlandırması](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. Bir abonelikte kullanılan genel IP adresleri sayısına bir sınır yoktur. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesini okuyun.
   >

   **Genel IP adresi kaynağı yeni bir IP yapılandırmasını ilişkilendirin**

   Yeni bir IP yapılandırmasında genel IP adresi eklediğinizde, tüm IP yapılandırmaları özel bir IP adresi olmalıdır çünkü özel bir IP adresi de eklemeniz gerekir. Var olan bir genel IP adresi kaynağı ekleyin veya yeni bir tane oluşturun. Yeni bir tane oluşturmak için aşağıdaki komutu girin:

   ```powershell
   $myPublicIp3 = New-AzPublicIpAddress `
   -Name "myPublicIp3" `
   -ResourceGroupName $RgName `
   -Location $Location `
   -AllocationMethod Static
   ```

   Statik özel IP adresi ve ilişkili ile yeni bir IP yapılandırması oluşturmak için *myPublicIp3* genel IP adresi kaynağı, aşağıdaki komutu girin:

   ```powershell
   Add-AzNetworkInterfaceIpConfig `
   -Name IPConfig-4 `
   -NetworkInterface $myNIC `
   -Subnet $Subnet `
   -PrivateIpAddress 10.0.0.7 `
   -PublicIpAddress $myPublicIp3
   ```

   **Genel IP adresi kaynağı var olan bir IP yapılandırmasını ilişkilendirin**

   Bir genel IP adresi kaynağı yalnızca zaten ilişkili olmayan bir IP yapılandırmasına ilişkilendirilebilir. Aşağıdaki komutu girerek bir IP yapılandırması ilişkili bir genel IP adresi olup olmadığını belirleyebilirsiniz:

   ```powershell
   $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
   ```

   Aşağıdakine benzer bir çıktı görürsünüz:

   ```
   Name       PrivateIpAddress PublicIpAddress                                           Primary

   IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
   IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
   IpConfig-3 10.0.0.6                                                                     False
   ```

   Bu yana **Publicıpaddress** sütunu için *IpConfig 3* olan boş, hiçbir genel IP adresi kaynağı şu anda ilişkili olduğundan. Var olan bir genel IP adresi kaynağı IpConfig-3'e ekleyebilir veya oluşturmak için aşağıdaki komutu girin:

   ```powershell
   $MyPublicIp3 = New-AzPublicIpAddress `
   -Name "MyPublicIp3" `
   -ResourceGroupName $RgName `
   -Location $Location -AllocationMethod Static
   ```

   Genel IP adresi kaynağı adı mevcut IP yapılandırmasına ilişkilendirmek için aşağıdaki komutu girin *IpConfig 3*:

   ```powershell
   Set-AzNetworkInterfaceIpConfig `
   -Name IpConfig-3 `
   -NetworkInterface $mynic `
   -Subnet $Subnet `
   -PublicIpAddress $myPublicIp3
   ```

6. Aşağıdaki komutu girerek NIC yeni IP yapılandırması ile ayarlayın:

   ```powershell
   Set-AzNetworkInterface -NetworkInterface $MyNIC
   ```

7. Özel IP adresleri ve aşağıdaki komutu girerek NIC'e atanmış genel IP adresi kaynağı görüntüleyin:

   ```powershell
   $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
   ```

8. İşletim sisteminiz için adımları tamamlayarak VM işletim sistemine özel IP adresini ekleyin [ekleme IP adresleri için bir VM işletim sistemi](#os-config) bu makalenin. İşletim sistemi için genel IP adresini eklemeyin.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]