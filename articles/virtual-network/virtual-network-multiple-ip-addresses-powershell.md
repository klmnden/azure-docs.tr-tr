---
title: "Azure sanal makineleri - PowerShell için birden çok IP adresi | Microsoft Docs"
description: "PowerShell kullanarak bir sanal makine için birden çok IP adresi atama hakkında bilgi edinin | Resource Manager."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c44ea62f-7e54-4e3b-81ef-0b132111f1f8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/24/2017
ms.author: jdial;annahar
ms.openlocfilehash: b3690ec991add437afdaba3ef22022d49c962b34
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-powershell"></a>PowerShell kullanarak sanal makineleri için birden çok IP adresi atayın

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Bu makalede PowerShell kullanarak Azure Resource Manager dağıtım modeli sanal makine (VM) oluşturma açıklanmaktadır. Birden çok IP adresi Klasik dağıtım modeli aracılığıyla oluşturulan kaynakları atanamaz. Azure dağıtım modelleri hakkında daha fazla bilgi için okuma [dağıtım modellerini anlama](../resource-manager-deployment-model.md) makalesi.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Birden çok IP adresiyle bir VM oluşturma

Adımları birden çok IP adresleriyle VM örneği oluşturmak senaryosunda açıklandığı şekilde açıklanmaktadır. Değişken değerleri, uygulamanız için gereken şekilde değiştirin.

1. Bir PowerShell komut istemi açın ve tek bir PowerShell oturumunda bu bölümdeki kalan adımları tamamlayın. Zaten yüklü ve yapılandırılmış PowerShell sahip değilseniz, bölümündeki adımları tamamlamanız [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) makale.
2. Oturum açma ile hesabınızı `login-azurermaccount` komutu.
3. Değiştir *myResourceGroup* ve *westus* bir adı ve seçtiğiniz konum. Bir kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

    ```powershell
    $RgName   = "MyResourceGroup"
    $Location = "westus"

    New-AzureRmResourceGroup `
    -Name $RgName `
    -Location $Location
    ```

4. Kaynak grubu ile aynı konumda bir sanal ağ (VNet) ve alt ağ oluşturun:

    ```powershell
    
    # Create a subnet configuration
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name MySubnet `
    -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $VNet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyVNet `
    -AddressPrefix 10.0.0.0/16 `
    -Subnet $subnetConfig

    # Get the subnet object
    $Subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
    ```

5. Bir ağ güvenlik grubu (NSG) ve bir kural oluşturun. NSG gelen ve giden kuralları kullanarak VM güvenliğini sağlar. Bu durumda, bağlantı noktası 3389 için gelen masaüstü bağlantılarına izin veren bir gelen kuralı oluşturulur.

    ```powershell
    
    # Create an inbound network security group rule for port 3389

    $NSGRule = New-AzureRmNetworkSecurityRuleConfig `
    -Name MyNsgRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow
    
    # Create a network security group
    $NSG = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyNetworkSecurityGroup `
    -SecurityRules $NSGRule
    ```

6. Birincil NIC IP yapılandırmasını tanımlayın Önceden tanımlanmış değer kullanmadıysanız, oluşturduğunuz alt ağdaki geçerli bir adrese 10.0.0.4 değiştirin. Statik bir IP adresi atamadan önce zaten kullanımda olmadığı ilk onaylamanız önerilir. Aşağıdaki komutu girin `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`. Adres bulunup bulunmadığını çıktıyı döndürür *doğru*. Çıktıyı döndürür, kullanılabilir değilse, *False* ve kullanılabilir adresleri listesi. 

    Aşağıdaki komutlarda **< Değiştir-ile-bilgisayarınızı-benzersiz-adı > kullanmak için benzersiz DNS adı ile değiştirin.** Bir Azure bölgesi içindeki tüm ortak IP adresleri arasında adının benzersiz olması gerekir. Bu isteğe bağlı bir parametredir. Yalnızca genel IP adresini kullanarak VM bağlanmak isterseniz kaldırılabilir.

    ```powershell
    
    # Create a public IP address
    $PublicIP1 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP1" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -DomainNameLabel <replace-with-your-unique-name> `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign the public IP ddress to it
    $IpConfigName1 = "IPConfig-1"
    $IpConfig1     = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IpConfigName1 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.4 `
    -PublicIpAddress $PublicIP1 `
    -Primary
    ```

    Birden fazla IP yapılandırması için bir NIC atadığınızda, bir yapılandırma olarak atanması gerekir *-birincil*.

    > [!NOTE]
    > Nominal bir ücret ortak IP adresine sahip. IP adresi fiyatlandırma hakkında daha fazla bilgi için okuma [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. Bir abonelikte kullanılabilir genel IP adresleri sayısına bir sınır yoktur. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesini okuyun.

7. NIC ikincil IP yapılandırmalarını tanımlayın Ekleyebilir veya gerektiği gibi yapılandırmaları kaldırabilirsiniz. Her IP yapılandırması atanan özel bir IP adresi olmalıdır. Her yapılandırma isteğe bağlı olarak atanmış bir genel IP adresi olabilir.

    ```powershell
    
    # Create a public IP address
    $PublicIP2 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP2" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign the public IP ddress to it
    $IpConfigName2 = "IPConfig-2"
    $IpConfig2     = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IpConfigName2 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.5 `
    -PublicIpAddress $PublicIP2
        
    $IpConfigName3 = "IpConfig-3"
    $IpConfig3 = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IPConfigName3 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.6
    ```

8. NIC oluşturun ve ona üç IP yapılandırmaları ilişkilendirin:

    ```powershell
    
    $NIC = New-AzureRmNetworkInterface `
    -Name MyNIC `
    -ResourceGroupName $RgName `
    -Location $Location `
    -NetworkSecurityGroupId $NSG.Id `
    -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
    ```

    >[!NOTE]
    >Bu makalede bir NIC'e atanmış tüm yapılandırmalar olsa, VM'ye bağlı her NIC birden çok IP yapılandırmaları atayabilirsiniz. Bir VM ile birden çok NIC oluşturmayı öğrenmek için okuma [bir VM ile birden çok NIC oluşturma](../virtual-machines/windows/multiple-nics.md) makalesi.

9. VM, aşağıdaki komutları girerek oluşturabilirsiniz:

    ```powershell
    
    # Define a credential object. When you run these commands, you're prompted to enter a sername and password for the VM you're reating.
    $cred = Get-Credential
    
    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
    -VMName MyVM `
    -VMSize Standard_DS1_v2 | `
    Set-AzureRmVMOperatingSystem -Windows `
    -ComputerName MyVM `
    -Credential $cred | `
    Set-AzureRmVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
    Add-AzureRmVMNetworkInterface `
    -Id $NIC.Id
    
    # Create the VM
    New-AzureRmVM `
    -ResourceGroupName $RgName `
    -Location $Location `
    -VM $VmConfig
    ```

10. İşletim sisteminiz için adımları tamamlayarak VM işletim sistemine özel IP adresleri ekleme [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin. Genel IP adreslerine işletim sistemine eklemeyin.

## <a name="add"></a>Bir VM için IP adreslerini ekleyin

Adımları tamamlayarak bir NIC'ye özel ve genel IP adresleri ekleyebilirsiniz. Aşağıdaki bölümlerde yer alan örnekler, zaten bir VM açıklanan üç IP yapılandırmaya sahip olduğunu varsayın [senaryo](#Scenario) Bu makale, ancak gerekli değildir, yapın.

1. Bir PowerShell komut istemi açın ve tek bir PowerShell oturumunda bu bölümdeki kalan adımları tamamlayın. Zaten yüklü ve yapılandırılmış PowerShell sahip değilseniz, bölümündeki adımları tamamlamanız [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) makale.
2. Aşağıdaki $Variables "değerler" adı IP adresine eklemek istediğiniz NIC ve kaynak grubu ve NIC bulunmaktadır konumu ile değiştirin:

    ```powershell
    $NicName  = "MyNIC"
    $RgName   = "MyResourceGroup"
    $Location = "westus"
    ```

    İstediğiniz değiştirmek için aşağıdaki komutları girin NIC adını bilmiyorsanız, önceki değişkenlerin değerleri değiştirin:

    ```powershell
    Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location
    ```
3. Bir değişken oluşturun ve aşağıdaki komutu yazarak mevcut NIC'in ayarlayın:

    ```powershell
    $MyNIC = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName
    ```
4. Aşağıdaki komutlarda değiştirme *MyVNet* ve *MySubnet* VNet ve NIC bağlı alt ağ adları için. NIC bağlı VNet ve alt ağ nesneleri almak için komutları girin:

    ```powershell
    $MyVNet = Get-AzureRMVirtualnetwork -Name MyVNet -ResourceGroupName $RgName
    $Subnet = $MyVnet.Subnets | Where-Object { $_.Name -eq "MySubnet" }
    ```
    NIC bağlı sanal ağ veya alt ağ adını bilmiyorsanız, aşağıdaki komutu girin:
    ```powershell
    $MyNIC.IpConfigurations
    ```
    Aşağıdaki örnek çıkış benzer bir metin çıktıda arayın:
    
    ```
    "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet/subnets/MySubnet"
    ```
    Bu çıkışı *MyVnet* VNet olduğu ve *MySubnet* NIC bağlı alt ağ.

5. Gereksinimlerinize göre aşağıdaki bölümlerden birindeki adımları tamamlayın:

    **Özel bir IP adresi Ekle**

    Özel bir IP adresi için bir NIC eklemeniz için bir IP yapılandırması oluşturmanız gerekir. Aşağıdaki komut bir yapılandırma 10.0.0.7 ile bir statik IP adresi oluşturur. Statik bir IP adresi belirtirken, alt ağ için kullanılmayan bir adres olmalıdır. Kullanılabilir girerek olduğundan emin olmak için adresi önce test önerilir `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` komutu. IP adresi bulunup bulunmadığını çıktıyı döndürür *doğru*. Çıktıyı döndürür, kullanılabilir değilse, *yanlış*ve kullanılabilir adresleri listesi.

    ```powershell
    Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
    $MyNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
    ```
    Benzersiz yapılandırma adları ve özel IP adresleri (yapılandırmaları statik IP adresleriyle) kullanarak gereksinim duyduğunuz kadar çok yapılandırmaları oluşturun.

    İşletim sisteminiz için adımları tamamlayarak VM işletim sistemine özel IP adresi Ekle [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin.

    **Bir ortak IP adresi Ekle**

    Bir ortak IP adresi, yeni bir IP yapılandırması ya da var olan bir IP yapılandırması için genel bir IP adresi kaynağı ilişkilendirerek eklenir. Gereksinim duyduğunuz kadar aşağıdaki bölümlerde birindeki adımları tamamlayın.

    > [!NOTE]
    > Nominal bir ücret ortak IP adresine sahip. IP adresi fiyatlandırma hakkında daha fazla bilgi için okuma [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfası. Bir abonelikte kullanılabilir genel IP adresleri sayısına bir sınır yoktur. Sınırlar hakkında daha fazla bilgi için [Azure limitleri](../azure-subscription-service-limits.md#networking-limits) makalesini okuyun.
    >

    - **Yeni bir IP yapılandırması için genel IP adresi kaynağı ilişkilendirme**
    
        Yeni bir IP yapılandırmasında bir ortak IP adresi eklediğinizde, tüm IP yapılandırmalarının özel bir IP adresi olması gerektiği için özel bir IP adresi eklemelisiniz. Varolan bir ortak IP adresi kaynağı ekleyin veya yeni bir tane oluşturun. Yeni bir tane oluşturmak için aşağıdaki komutu girin:
    
        ```powershell
        $myPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "myPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location `
        -AllocationMethod Static
        ```

        Özel bir statik IP adresi ile ilişkili yeni bir IP yapılandırması oluşturmak için *myPublicIp3* genel IP adresi kaynak, aşağıdaki komutu girin:

        ```powershell
        Add-AzureRmNetworkInterfaceIpConfig `
        -Name IPConfig-4 `
        -NetworkInterface $myNIC `
        -Subnet $Subnet `
        -PrivateIpAddress 10.0.0.7 `
        -PublicIpAddress $myPublicIp3
        ```

    - **Var olan bir IP yapılandırması için genel IP adresi kaynağı ilişkilendirme**

        Yalnızca genel IP adresi kaynağı zaten ilişkili olmayan bir IP yapılandırmasıyla ilişkilendirilmiş olabilir. Aşağıdaki komutu girerek bir IP yapılandırması ilişkili bir ortak IP adresi olup olmadığını belirleyebilirsiniz:

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

        Bu yana **Publicıpaddress** sütunu için *IpConfig 3* olduğundan, hiçbir ortak IP adresi kaynağı ilişkili şu anda boştur. Mevcut bir ortak IP adresi kaynağı IpConfig-3'e ekleyin veya oluşturmak için aşağıdaki komutu girin:

        ```powershell
        $MyPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "MyPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location -AllocationMethod Static
        ```

        Adlı varolan IP yapılandırması için genel IP adresi kaynağı ilişkilendirmek için aşağıdaki komutu girin *IpConfig 3*:
    
        ```powershell
        Set-AzureRmNetworkInterfaceIpConfig `
        -Name IpConfig-3 `
        -NetworkInterface $mynic `
        -Subnet $Subnet `
        -PublicIpAddress $myPublicIp3
        ```

6. NIC yeni IP yapılandırması ile aşağıdaki komutu girerek ayarlayın:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $MyNIC
    ```

7. Özel IP adresleri ve aşağıdaki komutu girerek NIC'ye atanan ortak IP adresi kaynaklara bakın:

    ```powershell   
    $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```
8. İşletim sisteminiz için adımları tamamlayarak VM işletim sistemine özel IP adresi Ekle [eklemek IP adresleri bir VM işletim sistemine](#os-config) bu makalenin. Genel IP adresi işletim sistemine eklemeyin.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
