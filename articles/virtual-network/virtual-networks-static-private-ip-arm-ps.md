---
title: Özel IP adresleri - Azure PowerShell VM'ler için yapılandırma | Microsoft Docs
description: PowerShell kullanarak sanal makineleri için özel IP adresleri yapılandırmayı öğrenin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: d5f18929-15e3-40a2-9ee3-8188bc248ed8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b0e8153f1d0cecd4efe66dc7cce64addd6ed62aa
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31424066"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-powershell"></a>PowerShell kullanarak bir sanal makine için özel IP adreslerini yapılandırın

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

Azure'un iki dağıtım modeli vardır: Azure Resource Manager ve klasik. Microsoft, kaynakların Resource Manager dağıtım modeliyle oluşturulmasını önerir. İki model arasındaki farkları öğrenmek için [Azure dağıtım modellerini anlama](../azure-resource-manager/resource-manager-deployment-model.md) makalesini okuyun. Bu makalede Resource Manager dağıtım modeli anlatılmaktadır. Ayrıca [Klasik dağıtım modelinde statik özel IP adresi yönetmek](virtual-networks-static-private-ip-classic-ps.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Aşağıdaki komutları önceden oluşturulmuş basit bir ortam beklediğiniz PowerShell örnek yukarıdaki senaryo tabanlı. Bu belgede gösterildiği komutları çalıştırmak istiyorsanız, önce açıklanan test ortamı oluşturmak [bir sanal ağ oluşturma](quick-create-powershell.md).

## <a name="create-a-vm-with-a-static-private-ip-address"></a>Statik özel IP adresiyle VM oluşturma
Adlı bir VM oluşturmak için *DNS01* içinde *ön uç* adlı bir sanal ağ alt ağı *TestVNet* özel bir statik IP *192.168.1.101*, aşağıdaki adımları izleyin:

1. Depolama hesabı, konumu, kaynak grubu ve kimlik bilgileri kullanılacak değişkenleri ayarlayın. VM için bir kullanıcı adı ve parola girmeniz gerekir. Depolama hesabı ve kaynak grubu zaten mevcut olmalıdır.

    ```powershell
    $stName  = "vnetstorage"
    $locName = "Central US"
    $rgName  = "TestRG"
    $cred    = Get-Credential -Message "Type the name and password of the local administrator account."
    ```

2. VM'yi oluşturmak istediğiniz alt ağ ve sanal ağ alın.

    ```powershell
    $vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    $subnet = $vnet.Subnets[0].Id
    ```

3. Gerekirse, VM Internet'ten erişmek için genel bir IP adresi oluşturun.

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name TestPIP -ResourceGroupName $rgName `
    -Location $locName -AllocationMethod Dynamic
    ```

4. VM atamak istediğiniz özel statik IP adresi kullanarak bir NIC oluşturun. IP alt ağı aralığından VM'ye ekleniyor olduğundan emin olun. Statik olarak özel IP ayarladığınız Bu makale için ana adım budur.

    ```powershell
    $nic = New-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName $rgName `
    -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id `
    -PrivateIpAddress 192.168.1.101
    ```

5. VM, NIC oluşturun:

    ```powershell
    $vm = New-AzureRmVMConfig -VMName DNS01 -VMSize "Standard_A1"
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName DNS01 `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/WindowsVMosDisk.vhd"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name "windowsvmosdisk" -VhdUri $osDiskUri `
    -CreateOption fromImage
    New-AzureRmVM -ResourceGroupName $rgName -Location $locName -VM $vm 
    ```

Statik olarak bir VM işletim sistemi içinde Azure sanal makineye atanan özel IP sürece atadığınız değil, önerilir gerekirse, ne zaman gibi [birden çok IP adresleri atama bir Windows VM](virtual-network-multiple-ip-addresses-powershell.md). İşletim sistemi içinde özel IP adresini el ile ayarlarsanız, Azure için atanan özel IP adresi aynı adresi olduğundan emin olun [ağ arabirimi](virtual-network-network-interface-addresses.md#change-ip-address-settings), ya da sanal makineye bağlantısını kaybedebilir. Daha fazla bilgi edinmek [özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarlar. Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir.

## <a name="retrieve-static-private-ip-address-information-for-a-network-interface"></a>Bir ağ arabirimi için özel statik IP adresi bilgilerini alma
Komut dosyası yukarıdaki VM oluşturulan için statik özel IP adresi bilgilerini görüntülemek için aşağıdaki PowerShell komutunu çalıştırın ve değerlerini uyun *PrivateIpAddress* ve *Privateıpallocationmethod*:

```powershell
Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
```

Beklenen çıktı:

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    Etag                 : W/"[Id]"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"[Id]\"",
                               "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Static",
                               "Subnet": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="remove-a-static-private-ip-address-from-a-network-interface"></a>Özel bir statik IP adresi bir ağ arabiriminden Kaldır
Özel statik IP adresi kaldırmak için yukarıdaki komut VM'ye aşağıdaki PowerShell komutlarını çalıştırın ekledi:

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Dynamic"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Beklenen çıktı:

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    Etag                 : W/"[Id]"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/WindowsVM"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"[Id]\"",
                               "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Dynamic",
                               "Subnet": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="add-a-static-private-ip-address-to-a-network-interface"></a>Bir ağ arabirimi için özel bir statik IP adresi Ekle
Yukarıdaki komut dosyası kullanılarak oluşturulan VM için özel bir statik IP adresi eklemek için aşağıdaki komutları çalıştırın:

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Static"
$nic.IpConfigurations[0].PrivateIpAddress = "192.168.1.101"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Statik olarak bir VM işletim sistemi içinde Azure sanal makineye atanan özel IP sürece atadığınız değil, önerilir gerekirse, ne zaman gibi [birden çok IP adresleri atama bir Windows VM](virtual-network-multiple-ip-addresses-powershell.md). İşletim sistemi içinde özel IP adresini el ile ayarlarsanız, Azure için atanan özel IP adresi aynı adresi olduğundan emin olun [ağ arabirimi](virtual-network-network-interface-addresses.md#change-ip-address-settings), ya da sanal makineye bağlantısını kaybedebilir. Daha fazla bilgi edinmek [özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarlar. Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir.

## <a name="change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface"></a>Bir ağ arabirimine atanmış özel bir IP adresi ayırma yöntemini değiştirme

Özel bir IP adresi statik veya dinamik ayırma yöntemiyle bir NIC'ye atanır. Dinamik IP adresleri, daha önce durduruldu (serbest bırakıldığında) durumdaydı bir VM başlattıktan sonra değiştirebilirsiniz. VM durduruldu (serbest bırakıldığında) durumundan yeniden başlatıldıktan sonra bile aynı IP adresi gerektiren bir hizmeti barındırma varsa bu olası sorunlara neden olabilir. Statik IP adresleri, VM silinene kadar bekletilir. Bir IP adresi ayırma yöntemini değiştirmek için ayırma yöntemi dinamik statik olarak değiştirir aşağıdaki betiği çalıştırın. Geçerli özel IP adresi için ayırma yöntemi statik ise, değiştirme *statik* için *dinamik* komut dosyasını çalıştırmadan önce.

```powershell
$RG = "TestRG"
$NIC_name = "testnic1"

$nic = Get-AzureRmNetworkInterface -ResourceGroupName $RG -Name $NIC_name
$nic.IpConfigurations[0].PrivateIpAllocationMethod = 'Static'
Set-AzureRmNetworkInterface -NetworkInterface $nic 
$IP = $nic.IpConfigurations[0].PrivateIpAddress

Write-Host "The allocation method is now set to"$nic.IpConfigurations[0].PrivateIpAllocationMethod"for the IP address" $IP"." -NoNewline
```

NIC adını bilmiyorsanız, aşağıdaki komutu girerek NIC'ler listesini bir kaynak grubu içinde görüntüleyebilirsiniz:

```powershell
Get-AzureRmNetworkInterface -ResourceGroupName $RG | Where-Object {$_.ProvisioningState -eq 'Succeeded'} 
```

## <a name="next-steps"></a>Sonraki adımlar

Yönetme hakkında bilgi edinin [IP adresi ayarlarını](virtual-network-network-interface-addresses.md).