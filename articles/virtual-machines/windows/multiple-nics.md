---
title: Oluşturma ve yönetme Windows Vm'lerini azure'da birden çok NIC kullanın | Microsoft Docs
description: Oluşturmak ve yönetmek için Azure PowerShell veya Resource Manager şablonları kullanarak bağlı birden çok NIC sahip bir Windows VM öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/26/2017
ms.author: iainfou
ms.openlocfilehash: 776ae83990a7799102c69347196a72a68561ee6b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34367224"
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a>Oluşturma ve birden çok NIC sahip olan bir Windows sanal makine yönetme
Azure sanal makineleri (VM'ler) kendisine bağlı birden çok sanal ağ arabirim kartı (NIC) olabilir. Ön uç ve arka uç bağlantısı ya da izleme veya yedekleme çözümü için ayrılmış bir ağ için farklı alt ağlara sahip ortak bir senaryodur. Bu makalede, birden çok NIC bağlı olan bir VM oluşturmak nasıl ayrıntıları verilmektedir. Aynı zamanda NIC var olan bir sanal makineden ekleyip öğrenin. Farklı [VM boyutları](sizes.md) NIC'ler değişen çok sayıda desteği, bu nedenle, VM buna göre boyutu.

## <a name="prerequisites"></a>Önkoşullar
Bilgisayarınızda yüklü olduğundan emin olun [yüklenmiş ve yapılandırılmış en son Azure PowerShell sürüm](/powershell/azure/overview).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında *myResourceGroup*, *myVnet*, ve *myVM*.


## <a name="create-a-vm-with-multiple-nics"></a>Birden çok NIC ile VM oluşturma
İlk olarak, bir kaynak grubu oluşturun. Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup* içinde *EastUs* konumu:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a>Sanal ağ ve alt ağlar oluşturun
İki veya daha fazla alt ağ bir sanal ağ için buna ortak bir senaryodur. Bir alt ağ ön uç trafiği için arka uç trafiği için diğer olabilir. Her iki alt ağlara bağlanmak için daha sonra VM üzerinde birden çok NIC kullanmanız gerekir.

1. İle iki sanal ağ alt ağları tanımlamak [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Aşağıdaki örnek, alt ağlar için tanımlar *mySubnetFrontEnd* ve *mySubnetBackEnd*:

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. Sanal ağ ve alt ağları ile oluşturmak [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVnet*:

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a>Birden çok NIC oluşturun
İki NIC ile oluşturma [yeni AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Ön uç alt ağa bir NIC ve bir NIC arka uç alt ağına bağlayın. Aşağıdaki örnek adlı NIC'ler oluşturur *myNic1* ve *myNic2*:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic1" `
    -Location "EastUs" `
    -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic2" `
    -Location "EastUs" `
    -SubnetId $backEnd.Id
```

Genellikle, ayrıca oluşturduğunuz bir [ağ güvenlik grubu](../../virtual-network/security-overview.md) VM ağ trafiğine filtre uygulamak için ve bir [yük dengeleyici](../../load-balancer/load-balancer-overview.md) trafiği arasında birden çok VM dağıtmak için.

### <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma
VM yapılandırması oluşturmak şimdi başlayın. Her VM boyutu, bir VM'ye ekleyebilirsiniz NIC toplam sayısı için bir sınır vardır. Daha fazla bilgi için bkz: [Windows VM boyutları](sizes.md).

1. VM kimlik bilgilerinizi ayarlamak `$cred` şekilde değişkeni:

    ```powershell
    $cred = Get-Credential
    ```

2. VM ile tanımlama [AzureRmVMConfig yeni](/powershell/module/azurerm.compute/new-azurermvmconfig). Aşağıdaki örnek, adlandırılmış bir VM'nin tanımlar *myVM* ve ikiden fazla NIC'ler destekleyen bir VM boyutu kullanır (*Standard_DS3_v2*):

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. VM yapılandırmanızı kalan oluşturma [kümesi AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) ve [kümesi AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage). Aşağıdaki örnek, bir Windows Server 2016 VM oluşturur:

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
        -Windows `
        -ComputerName "myVM" `
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
        -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" `
        -Skus "2016-Datacenter" `
        -Version "latest"
   ```

4. Daha önce ile oluşturulan iki NIC ekleme [Ekle AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. İle VM oluşturma [AzureRmVM yeni](/powershell/module/azurerm.compute/new-azurermvm):

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

6. İçin ikincil NIC'ler için işletim sistemi içindeki adımları tamamlayarak yollar [işletim sistemini yapılandırmak için birden çok NIC](#configure-guest-os-for-multiple-nics).

## <a name="add-a-nic-to-an-existing-vm"></a>Mevcut bir VM'yi bir NIC eklemeniz
Mevcut bir VM'yi sanal bir NIC'ye eklemek için VM serbest bırakma ekleyin sanal NIC sonra VM'yi başlatın. Farklı [VM boyutları](sizes.md) NIC'ler değişen çok sayıda desteği, bu nedenle, VM buna göre boyutu. Gerekirse, [bir VM'yi yeniden boyutlandırın](resize-vm.md).

1. VM ile serbest bırakma [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). Aşağıdaki örnek adlı VM kaldırır *myVM* içinde *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. VM ile varolan yapılandırmasını almak [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). Aşağıdaki örnek adlı VM için bilgilerini alır *myVM* içinde *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Aşağıdaki örnek, bir sanal NIC ile oluşturur [yeni AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) adlı *myNic3* için bağlı *mySubnetBackEnd*. Bir sanal NIC sonra adlı VM'ye bağlı *myVM* içinde *myResourceGroup* ile [Ekle AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

    ```powershell
    # Get info for the back end subnet
    $myVnet = Get-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get the ID of the new virtual NIC and add to VM
    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId | Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a>Birincil sanal NIC
    Multi-NIC VM Nıc'lerde birini birincil olması gerekir. VM varolan sanal Nıc'lerde birini zaten birincil olarak ayarlanmışsa, bu adımı atlayabilirsiniz. Aşağıdaki örnekte iki sanal NIC VM üzerinde mevcut şimdi ve ilk NIC eklemek istediğiniz varsayar (`[0]`) birincil olarak:
        
    ```powershell
    # List existing NICs on the VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 to be primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update the VM state in Azure
    Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. VM Başlat [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

5. İçin ikincil NIC'ler için işletim sistemi içindeki adımları tamamlayarak yollar [işletim sistemini yapılandırmak için birden çok NIC](#configure-guest-os-for-multiple-nics).

## <a name="remove-a-nic-from-an-existing-vm"></a>Bir NIC var olan bir sanal makineden kaldırın
Varolan bir sanal makineden sanal bir NIC'ye kaldırmak için VM serbest bırakma, sanal NIC kaldırın ve sonra VM'yi başlatın.

1. VM ile serbest bırakma [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). Aşağıdaki örnek adlı VM kaldırır *myVM* içinde *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. VM ile varolan yapılandırmasını almak [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). Aşağıdaki örnek adlı VM için bilgilerini alır *myVM* içinde *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. İle NIC kaldırma hakkında bilgi almak [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface). Aşağıdaki örnek bilgilerini alır *myNic3*:

    ```powershell
    # List existing NICs on the VM if you need to determine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. NIC ile Kaldır [Kaldır AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) ve VM ile güncelleştirme [güncelleştirme-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm). Aşağıdaki örnek kaldırır *myNic3* ile alınan `$nicId` önceki adımda:

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. VM Başlat [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a>Birden çok NIC ile şablonları oluşturma
Azure Resource Manager şablonları birden çok NIC oluşturma gibi dağıtımı sırasında bir kaynağın birden çok örneğini oluşturmak için bir yol sağlar. Resource Manager şablonları bildirim temelli JSON dosyaları ortamınızı tanımlamak için kullanın. Daha fazla bilgi için bkz: [genel bakış Azure Kaynak Yöneticisi'nin](../../azure-resource-manager/resource-group-overview.md). Kullanabileceğiniz *kopyalama* oluşturmak için örnek sayısını belirtmek için:

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

Daha fazla bilgi için bkz: [kullanarak birden çok örneği oluşturma *kopya*](../../resource-group-create-multiple.md). 

Aynı zamanda `copyIndex()` bir kaynak adı için bir sayı eklenecek. Daha sonra oluşturabilirsiniz *myNic1*, *MyNic2* ve benzeri. Aşağıdaki kod örneği dizin değeri ekleme gösterir:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Tam örnek okuyabilirsiniz [Resource Manager şablonları kullanarak birden çok NIC oluşturma](../../virtual-network/template-samples.md).

İçin ikincil NIC'ler için işletim sistemi içindeki adımları tamamlayarak yollar [işletim sistemini yapılandırmak için birden çok NIC](#configure-guest-os-for-multiple-nics).

## <a name="configure-guest-os-for-multiple-nics"></a>Konuk işletim sistemi için birden çok NIC yapılandırın

Azure sanal makineye bağlı ilk (birincil) ağ arabirimi için varsayılan ağ geçidi atar. Azure, bir sanal makineye bağlı ek (ikincil) ağ arabirimlerine varsayılan ağ geçidi atamaz. Bu nedenle varsayılan olarak, alt ağın dışında kalan ve ikincil bir ağ arabirimi içeren kaynaklarla iletişim kurulamaz. İletişimi etkinleştirmek için adımları farklı işletim sistemleri için farklı ancak ikincil ağ arabirimlerine ancak, kendi alt ağı dışından kaynaklarla iletişim kurabilirsiniz.

1. Bir Windows komut isteminden çalıştırın `route print` çıkış iki bağlı ağ arabirimine sahip bir sanal makine için çıktı aşağıdakine benzer döndürür komutu:

    ```
    ===========================================================================
    Interface List
    3...00 0d 3a 10 92 ce ......Microsoft Hyper-V Network Adapter #3
    7...00 0d 3a 10 9b 2a ......Microsoft Hyper-V Network Adapter #4
    ===========================================================================
    ```
 
    Bu örnekte, **Microsoft Hyper-V ağ bağdaştırıcısı #4** (7) arabirimidir kendisine atanmış bir varsayılan ağ geçidi yok ikincil ağ arabirimi.

2. Bir komut isteminden çalıştırın `ipconfig` ikincil ağ arabirimi için hangi IP adresi atanır görmek için komutu. Bu örnekte, 192.168.2.4 arabirimi 7 atanır. İkincil ağ arabirimi için bir varsayılan ağ geçidi adresi döndürülür.

3. Alt ağ için ağ geçidi için ikincil ağ arabirimi alt ağı dışında adresler için giden tüm trafiği yönlendirmek için aşağıdaki komutu çalıştırın:

    ```
    route add -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5015 IF 7
    ```

    Alt ağ için ağ geçidi adresi, alt ağ için tanımlanan adres aralığındaki (Bitiş içinde.1) ilk IP adresi değil. Alt ağ dışında tüm trafiği yönlendirmek istemiyorsanız, bireysel rotaların belirli hedeflere, bunun yerine ekleyebilirsiniz. Örneğin, yalnızca 192.168.3.0 ikincil ağ arabirimi trafiği yönlendirmek istiyorsanız ağ, aşağıdaki komutu girin:

      ```
      route add -p 192.168.3.0 MASK 255.255.255.0 192.168.2.1 METRIC 5015 IF 7
      ```
  
4. 192.168.3.0 kaynakta ile başarıyla iletişim onaylamak için ağ, örneğin, 7 (192.168.2.4) arabirimini kullanarak 192.168.3.4 ping işlemi yapmak için aşağıdaki komutu girin:

    ```
    ping 192.168.3.4 -S 192.168.2.4
    ```

    Aşağıdaki komutla ping işlemi sırasında aygıt Windows Güvenlik Duvarı üzerinden ICMP açmanız gerekebilir:
  
      ```
      netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
      ```
  
5. Eklenen yoldur rota tablosunda onaylamak için girin `route print` çıkışı aşağıdaki metni benzer döndürür komutu:

    ```
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4     15
              0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.4   5015
    ```

    Listelenen rota *192.168.1.1* altında **ağ geçidi**, birincil ağ arabirimi için varsayılan olarak olduğundan yoldur. Rota ile *192.168.2.1* altında **ağ geçidi**, eklediğiniz yoldur.

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [Windows VM boyutları](sizes.md) zaman çalıştığınız birden çok NIC sahip bir VM oluşturun. Her VM boyutu destekliyorsa NIC sayısı dikkat edin. 


