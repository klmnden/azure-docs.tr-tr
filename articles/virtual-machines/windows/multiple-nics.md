---
title: Windows Vm'leri azure'daki birden çok NIC kullanma oluşturma ve yönetme | Microsoft Docs
description: Oluşturma ve Azure PowerShell veya Resource Manager şablonlarını kullanarak bağlı birden çok NIC içeren bir Windows VM yönetme hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/26/2017
ms.author: cynthn
ms.openlocfilehash: d6387fc0f2f7342eb7a1fda25fcf1272889d13f7
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "64698155"
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a>Oluşturma ve birden çok NIC içeren bir Windows sanal makine yönetme
Azure'da sanal makineler (VM) bağlı birden çok sanal ağ arabirim kartları (NIC) sahip olabilir. Ön uç ve arka uç bağlantısı için farklı alt ağlara sahip ortak bir senaryodur. Birden fazla alt ağdaki bir VM'de birden çok NIC ilişkilendirebilirsiniz, ancak aynı sanal ağ (vNet) içinde tüm bu alt ağlardan bulunmalıdır. Bu makalede, bağlı birden çok NIC içeren bir VM oluşturma işlemi açıklanmaktadır. Ayrıca eklemek veya mevcut bir VM'den NIC kaldırmak nasıl öğrenin. Farklı [VM boyutları](sizes.md) değişen sayıda NIC desteği, bu nedenle, sanal Makinenizin uygun şekilde boyutu.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup*, *myVnet*, ve *myVM*.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="create-a-vm-with-multiple-nics"></a>Birden çok NIC ile VM oluşturma
İlk olarak bir kaynak grubu oluşturun. Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myResourceGroup* içinde *EastUs* konumu:

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a>Sanal ağ oluşturup alt ağları
Bir sanal ağ için iki veya daha fazla alt ağa sahip bir yaygın senaryodur. Bir alt ağ, arka uca trafik için ön uç trafiği için olabilir. Her iki alt ağa bağlanmak için ardından sanal makinenizde birden çok NIC kullanmanız gerekir.

1. İki sanal ağ alt ağları ile tanımlama [yeni AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig). Aşağıdaki örnek, alt ağlar için tanımlar *mySubnetFrontEnd* ve *mySubnetBackEnd*:

    ```powershell
    $mySubnetFrontEnd = New-AzVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. Sanal ağ ve alt ağa sahip oluşturma [yeni AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork). Aşağıdaki örnekte adlı bir sanal ağ oluşturur *myVnet*:

    ```powershell
    $myVnet = New-AzVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a>Birden çok NIC oluşturma
İki NIC ile oluşturma [yeni AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface). Arka uç alt ağına ön uç alt ağına bir NIC ve bir ağ Arabirimi ekleyin. Aşağıdaki örnekte adlı NIC oluşturur *myNic1* ve *myNic2*:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic1" `
    -Location "EastUs" `
    -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic2" `
    -Location "EastUs" `
    -SubnetId $backEnd.Id
```

Ayrıca oluşturmak genellikle bir [ağ güvenlik grubu](../../virtual-network/security-overview.md) VM ağ giden trafiği filtrelemek ve [yük dengeleyici](../../load-balancer/load-balancer-overview.md) birden çok VM arasında trafiği dağıtmak için.

### <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma
Şimdi, VM yapılandırması oluşturmaya başlayın. Her VM boyutu, bir VM'ye ekleyebilirsiniz NIC toplam sayısı sınırı vardır. Daha fazla bilgi için [Windows VM boyutları](sizes.md).

1. VM kimlik bilgilerinizi ayarlayın `$cred` değişkeni aşağıdaki gibi:

    ```powershell
    $cred = Get-Credential
    ```

2. VM'nizi tanımlamak [AzVMConfig yeni](https://docs.microsoft.com/powershell/module/az.compute/new-azvmconfig). Aşağıdaki örnek adlı bir VM tanımlar *myVM* ve bir VM boyutu ikiden fazla NIC'yi desteklediği kullanır (*Standard_DS3_v2*):

    ```powershell
    $vmConfig = New-AzVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. VM yapılandırmanızı geri kalanını oluşturmak [kümesi AzVMOperatingSystem](https://docs.microsoft.com/powershell/module/az.compute/set-azvmoperatingsystem) ve [kümesi AzVMSourceImage](https://docs.microsoft.com/powershell/module/az.compute/set-azvmsourceimage). Aşağıdaki örnek, bir Windows Server 2016 VM oluşturur:

    ```powershell
    $vmConfig = Set-AzVMOperatingSystem -VM $vmConfig `
        -Windows `
        -ComputerName "myVM" `
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
    $vmConfig = Set-AzVMSourceImage -VM $vmConfig `
        -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" `
        -Skus "2016-Datacenter" `
        -Version "latest"
   ```

4. Daha önce oluşturduğunuz iki NIC eklemek [Ekle AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface):

    ```powershell
    $vmConfig = Add-AzVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. İle VM oluşturma [yeni-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm):

    ```powershell
    New-AzVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

6. İçindeki adımları tamamlayarak yollar için ikincil ağ arabirimlerine işletim sistemine ekleme [işletim sistemini yapılandırmak için birden çok NIC](#configure-guest-os-for-multiple-nics).

## <a name="add-a-nic-to-an-existing-vm"></a>Mevcut bir VM'ye bir NIC'yi ekleyebilir
Mevcut bir VM'ye sanal NIC eklemek için VM'yi serbest bırakın. sanal NIC'yi ekleyin, sonra VM'yi başlatın. Farklı [VM boyutları](sizes.md) değişen sayıda NIC desteği, bu nedenle, sanal Makinenizin uygun şekilde boyutu. Gerekirse, [VM'yi yeniden boyutlandırma](resize-vm.md).

1. VM serbest [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm). Aşağıdaki örnekte adlı VM serbest bırakılır *myVM* içinde *myResourceGroup*:

    ```powershell
    Stop-AzVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Var olan VM yapılandırmasını almak [Get-AzVm](https://docs.microsoft.com/powershell/module/az.compute/get-azvm). Aşağıdaki örnekte adlı VM'nin bilgileri alır *myVM* içinde *myResourceGroup*:

    ```powershell
    $vm = Get-AzVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Aşağıdaki örnek, bir sanal NIC ile oluşturur [yeni AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) adlı *myNic3* için bağlı *mySubnetBackEnd*. Sanal NIC'nin ardından adlı VM'ye ekli *myVM* içinde *myResourceGroup* ile [Ekle AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/add-azvmnetworkinterface):

    ```powershell
    # Get info for the back end subnet
    $myVnet = Get-AzVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get the ID of the new virtual NIC and add to VM
    $nicId = (Get-AzNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzVMNetworkInterface -VM $vm -Id $nicId | Update-AzVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a>Birincil sanal NIC
    Multi-NIC VM üzerindeki NIC biri birincil olması gerekir. Bir VM'nin mevcut sanal Nıc'lerde zaten birincil olarak ayarlanırsa, bu adımı atlayabilirsiniz. Aşağıdaki örnek iki sanal NIC, bir sanal makinede mevcut olduğuna ve ilk NIC eklemek istediğiniz varsayılır (`[0]`) birincil olarak:
        
    ```powershell
    # List existing NICs on the VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 to be primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update the VM state in Azure
    Update-AzVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. VM Başlat [Başlat-AzVm](https://docs.microsoft.com/powershell/module/az.compute/start-azvm):

    ```powershell
    Start-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

5. İçindeki adımları tamamlayarak yollar için ikincil ağ arabirimlerine işletim sistemine ekleme [işletim sistemini yapılandırmak için birden çok NIC](#configure-guest-os-for-multiple-nics).

## <a name="remove-a-nic-from-an-existing-vm"></a>Mevcut VM'den bir NIC Kaldır
Mevcut bir VM'den sanal NIC'yi kaldırmak için VM'yi serbest bırakın, sanal NIC'yi kaldırın sonra VM'yi başlatın.

1. VM serbest [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm). Aşağıdaki örnekte adlı VM serbest bırakılır *myVM* içinde *myResourceGroup*:

    ```powershell
    Stop-AzVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Var olan VM yapılandırmasını almak [Get-AzVm](https://docs.microsoft.com/powershell/module/az.compute/get-azvm). Aşağıdaki örnekte adlı VM'nin bilgileri alır *myVM* içinde *myResourceGroup*:

    ```powershell
    $vm = Get-AzVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. İle NIC kaldırma hakkında bilgi almak [Get-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkinterface). Aşağıdaki örnek bilgileri alır *myNic3*:

    ```powershell
    # List existing NICs on the VM if you need to determine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. NIC ile kaldırma [Remove-AzVMNetworkInterface](https://docs.microsoft.com/powershell/module/az.compute/remove-azvmnetworkinterface) ve sonra VM ile güncelleştirme [güncelleştirme-AzVm](https://docs.microsoft.com/powershell/module/az.compute/update-azvm). Aşağıdaki örnek kaldırır *myNic3* ile alınan `$nicId` önceki adımda:

    ```powershell
    Remove-AzVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzVm -ResourceGroupName "myResourceGroup"
    ```   

5. VM Başlat [Başlat-AzVm](https://docs.microsoft.com/powershell/module/az.compute/start-azvm):

    ```powershell
    Start-AzVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a>Birden çok NIC ile şablonları oluşturma
Azure Resource Manager şablonları, birden çok NIC oluşturma gibi dağıtım sırasında bir kaynağın birden çok örneğini oluşturmak için bir yol sağlar. Resource Manager şablonları, ortamınızı tanımlamak için bildirim temelli JSON dosyalarını kullanın. Daha fazla bilgi için [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md). Kullanabileceğiniz *kopyalama* oluşturmak için örnek sayısını belirtmek için:

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

Daha fazla bilgi için [kullanarak birden çok örnek oluşturma *kopyalama*](../../resource-group-create-multiple.md). 

Ayrıca `copyIndex()` bir kaynak adının sonuna bir sayı eklenecek. Daha sonra oluşturabilirsiniz *myNic1*, *MyNic2* ve benzeri. Aşağıdaki kod, dizin değeri ekleyerek bir örnek gösterilmektedir:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Tam örnek edinebilirsiniz [Resource Manager şablonları kullanarak birden çok NIC oluşturma](../../virtual-network/template-samples.md).

İçindeki adımları tamamlayarak yollar için ikincil ağ arabirimlerine işletim sistemine ekleme [işletim sistemini yapılandırmak için birden çok NIC](#configure-guest-os-for-multiple-nics).

## <a name="configure-guest-os-for-multiple-nics"></a>Konuk işletim sistemi için birden çok NIC yapılandırın

Azure, varsayılan bir ağ geçidi sanal makinesine bağlı ilk (birincil) ağ arabirimine atar. Azure, bir sanal makineye bağlı ek (ikincil) ağ arabirimlerine varsayılan ağ geçidi atamaz. Bu nedenle varsayılan olarak, alt ağın dışında kalan ve ikincil bir ağ arabirimi içeren kaynaklarla iletişim kurulamaz. İletişimi etkinleştirmek için adımları farklı işletim sistemleri için farklı olsa ikincil ağ arabirimlerine ancak, kendi alt dışındaki kaynaklarla iletişim kurabilirsiniz.

1. Bir Windows Komut İstemi'nden çalıştırmak `route print` komutu, iki bağlı ağ arabirimleri ile bir sanal makine için çıktı aşağıdakine benzer bir çıktı döndürür:

    ```
    ===========================================================================
    Interface List
    3...00 0d 3a 10 92 ce ......Microsoft Hyper-V Network Adapter #3
    7...00 0d 3a 10 9b 2a ......Microsoft Hyper-V Network Adapter #4
    ===========================================================================
    ```
 
    Bu örnekte, **Microsoft Hyper-V ağ bağdaştırıcısı #4** (7) arabirimidir kendisine atanmış bir varsayılan ağ geçidi yok ikincil ağ arabirimi.

2. Bir komut isteminden çalıştırın `ipconfig` ikincil ağ arabirimi ile hangi IP adresi atanır görmek için komutu. Bu örnekte, 192.168.2.4 arabirimine 7 atanır. İkincil ağ arabirimi için hiçbir varsayılan ağ geçidi adresi döndürülür.

3. Alt ağ için ağ geçidi alt ağı için ikincil ağ arabiriminin dışındaki adresleri hedefleyen tüm trafiği yönlendirmek için aşağıdaki komutu çalıştırın:

    ```
    route add -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5015 IF 7
    ```

    Ağ geçidi alt ağının (.1 içinde biten) ilk IP adresi alt ağ için tanımlanan adres aralığında adresidir. Alt ağın dışında tüm trafiği yönlendirmek istemiyorsanız, tekil yollar yerine belirli bir hedefe ekleyebilirsiniz. Örneğin, yalnızca 192.168.3.0 için ikincil ağ arabirimi trafiği yönlendirmek istiyorsanız ağ komutu girin:

      ```
      route add -p 192.168.3.0 MASK 255.255.255.0 192.168.2.1 METRIC 5015 IF 7
      ```
  
4. 192.168.3.0 kaynak ile başarıyla iletişim onaylamak için ağ, örneğin, 7 (192.168.2.4) arabirimini kullanarak 192.168.3.4 ping işlemi yapmak için aşağıdaki komutu girin:

    ```
    ping 192.168.3.4 -S 192.168.2.4
    ```

    Aşağıdaki komutla ping işlemi cihazın Windows Güvenlik Duvarı üzerinden ICMP'ye açmanız gerekebilir:
  
      ```
      netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
      ```
  
5. Eklenen yoldur rota tablosunda doğrulamak için şunu girin `route print` komutu aşağıdaki metne benzer bir çıktı döndürür:

    ```
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4     15
              0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.4   5015
    ```

    Listelenen rota *192.168.1.1* altında **ağ geçidi**, birincil ağ arabirimi için varsayılan olarak yoktur yoldur. Yolun *192.168.2.1* altında **ağ geçidi**, eklediğiniz yoldur.

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [Windows VM boyutları](sizes.md) zaman çalıştığınız birden çok NIC içeren bir VM oluşturun. Her VM boyutu destekleyen NIC sayısı üst sınırı dikkat edin. 


