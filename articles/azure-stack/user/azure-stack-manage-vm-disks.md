---
title: Azure Stack'te bir VM disklerini yönetme | Microsoft Docs
description: Azure Stack'te sanal makineler için diskler oluşturma.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/18/2019
ms.author: sethm
ms.reviewer: jiahan
ms.lastreviewed: 01/18/2019
ms.openlocfilehash: e38612e0d4e0707525b313c79143018c74c4c77b
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56326519"
---
# <a name="create-virtual-machine-disk-storage-in-azure-stack"></a>Azure Stack'te sanal makine disk depolama alanı oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu makalede Azure Stack portalını kullanarak veya PowerShell kullanarak sanal makine disk depolama alanı oluşturmayı açıklar.

## <a name="overview"></a>Genel Bakış

Azure Stack 1808 sürümünden başlayarak, sanal makinelerde bir işletim sistemi (OS) hem de veri diski olarak yönetilen ve yönetilmeyen disklerle kullanımını destekler. 1808 sürümünden önce yalnızca yönetilmeyen diskleri desteklenir. 

**[Yönetilen diskler](../../virtual-machines/windows/managed-disks-overview.md)**  VM diskleriyle ilişkili depolama hesaplarını yöneterek Azure Iaas Vm'leri için disk yönetimini basitleştirin. Yalnızca boyutu belirtmek zorunda duyduğunuz disk ve Azure Stack oluşturur ve diski oluşturup yönetebilmesi.

Yönetilmeyen diskler gerektiren disklerini depolamak için bir depolama hesabı oluşturun. Oluşturduğunuz diskler, VM diskleri adlandırılır ve depolama hesabındaki kapsayıcıları depolanır.

### <a name="best-practice-guidelines"></a>En iyi uygulama kılavuzları

Performansı artırmak ve maliyetleri azaltmak için her sanal makine disk içinde ayrı bir kapsayıcıya Yerleştir öneririz. Bir kapsayıcı, bir işletim sistemi diski veya veri diski ancak aynı anda hem tutun. (Ancak, hiçbir şey yoktur aynı kapsayıcıda her iki tür disk yerleştirmekten önlemek için.)

Bir sanal Makineye bir veya daha fazla veri diski ekleyin, ek kapsayıcıları bir konumu olarak bu disklerini depolamak için kullanın. Ek sanal makineler için işletim sistemi diski, ayrıca kendi kapsayıcılarda olmalıdır.

VM'ler oluşturduğunuzda, aynı depolama hesabını her yeni sanal makine için yeniden kullanabilirsiniz. Yalnızca oluşturduğunuz kapsayıcı benzersiz olmalıdır.

### <a name="adding-new-disks"></a>Yeni disk ekleme

Portalı kullanarak ve PowerShell kullanarak diskleri ekleme aşağıdaki tabloda özetlenmiştir.

| Yöntem | Seçenekler
|-|-|
|Kullanıcı portalı|-Mevcut bir VM'ye yeni veri diskleri ekleme. Yeni diskler, Azure Stack tarafından oluşturulur. </br> </br>-Daha önce oluşturulmuş bir VM için mevcut bir disk (.vhd) dosyasına ekleyin. Bunu yapmak için .vhd hazırlamak ve Azure Stack'e dosyayı karşıya yükleyin. |
|[PowerShell](#use-powershell-to-add-multiple-unmanaged-disks-to-a-vm) | -Bir işletim sistemi diski ile yeni bir VM oluşturun ve aynı zamanda, o sanal makineye bir veya daha fazla veri diski ekleyin. |

## <a name="use-the-portal-to-add-disks-to-a-vm"></a>Bir VM'ye disk ekleme için portalı kullanma

Çoğu Market öğesi için bir VM oluşturmak için portalı kullandığınızda varsayılan olarak, yalnızca işletim sistemi diski oluşturulur.

Bir VM oluşturduktan sonra portalda kullanabilirsiniz:
* Yeni bir veri diski oluşturun ve VM'e ekleyin.
* Varolan bir veri diski yükleyin ve VM'e ekleyin.

Eklediğiniz her bir yönetilmeyen disk içinde ayrı bir kapsayıcıya koymanız gerekir.

>[!NOTE]  
>Oluşturulan ve Azure tarafından yönetilen diskleri çağrılır [yönetilen diskler](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview).

### <a name="use-the-portal-to-create-and-attach-a-new-data-disk"></a>Oluşturun ve yeni bir veri diski için portalı kullanma

1.  Portalında, **tüm hizmetleri** > **sanal makineler**.    
    ![Örnek: Sanal makine Panosu'ndan](media/azure-stack-manage-vm-disks/vm-dashboard.png)

2.  Daha önce oluşturduğunuz bir sanal makineyi seçin.   
    ![Örnek: Panoda bir VM seçin](media/azure-stack-manage-vm-disks/select-a-vm.png)

3.  Sanal makine için seçin **diskleri** > **Ekle veri diski**.       
    ![Örnek: VM'ye yeni bir disk ekleme](media/azure-stack-manage-vm-disks/Attach-disks.png)    

4.  Veri diski için:
    -  Girin **LUN**. LUN, geçerli bir sayı olmalıdır.
    -  Seçin **Oluştur disk**.
    ![Örnek: VM'ye yeni bir disk ekleme](media/azure-stack-manage-vm-disks/add-a-data-disk-create-disk.png)

5.  Oluştur, disk dikey yönetilen:
    -  Girin **adı** diskin.
    -  Mevcut bir seçin veya yeni bir **kaynak grubu**.
    -  Seçin **konumu**. Varsayılan olarak, konum, işletim sistemi diskini barındıran aynı kapsayıcıya ayarlanır.
    -  Seçin **hesap türü**. 
        ![Örnek: VM'ye yeni bir disk ekleme](media/azure-stack-manage-vm-disks/create-manage-disk.png)

        **Premium SSD**  
        Premium diskler (SSD) katı hal sürücüleriyle desteklenir ve tutarlı, düşük gecikme süreli performans sunar. Bunlar, fiyat ve performans arasında en iyi dengeyi sağlar ve g/Ç açısından yoğun uygulamalar ve üretim iş yükleri için idealdir.
       
        **Standart HDD**  
        Standart diskler (HDD) manyetik sürücüler tarafından desteklenir ve verilere sık erişilmeyen uygulamalar için tercih edilebilir. Bölge - yedekli diskler, verilerinizi birden çok alanda çoğaltan bölgesel olarak yedekli depolama (ZRS) tarafından desteklenir ve tek bir bölge çalışmıyor olsa bile kullanılabilir. 

    -  Seçin **kaynak türünü**.

       Farklı bir diskin anlık görüntüsünden ya da depolama hesabındaki bir blobdan bir disk oluşturun veya boş bir disk oluşturun.

        **Anlık görüntü**  
        Varsa, bir anlık görüntü seçin. Anlık görüntü olmalıdır sanal makinenin abonelik ve konumdaki kullanılabilir.

        **Depolama blobu**  
        - Disk görüntüsünü içeren bir depolama blobu URI'si ekleyin.  
        - Seçin **Gözat** depolama hesapları dikey penceresini açın. Yönergeler için bkz [bir depolama hesabından bir veri diski ekleme](#add-a-data-disk-from-a-storage-account).
        - Görüntü işletim sistemi türünü seçin **Windows**, **Linux**, veya **yok (veri diski)**.

        **Yok (boş disk)**

    -  Seçin **boyutu (GiB)**.

       Standart disk disk boyutuna göre artış maliyeti. Diskin boyutuna göre premium diskin maliyetleri ve performansı artar. Daha fazla bilgi için [yönetilen diskler fiyatlandırması](https://go.microsoft.com/fwlink/?linkid=843142).

    -  **Oluştur**’u seçin. Azure Stack oluşturur ve yönetilen disk doğrular.

5.  Azure Stack disk oluşturur ve sanal makineye iliştirir sonra yeni disk sanal makinenin disk ayarları altında listelenir **veri DİSKLERİ**.   

    ![Örnek: Diski görüntüleme](media/azure-stack-manage-vm-disks/view-data-disk.png)

### <a name="add-a-data-disk-from-a-storage-account"></a>Bir depolama hesabından bir veri diski ekleme

Azure stack'teki depolama hesapları ile çalışma hakkında daha fazla bilgi için bkz. [Azure Stack depolamaya giriş](azure-stack-storage-overview.md).

1. Seçin **depolama hesabı** kullanılacak.
2. Seçin **kapsayıcı** veri diski yerleştirmek istediğiniz. Gelen **kapsayıcıları** dikey penceresinde oluşturmak için kullanabileceğiniz yeni bir kapsayıcı istiyorsanız. Yeni disk konumu için kendi kapsayıcı daha sonra değiştirebilirsiniz. Her disk için ayrı bir kapsayıcı kullandığınızda, performansı geliştirebilir veri diski yerleşimini dağıtın.
3. Seçin **seçin** seçimini kaydetmek için.

    ![Örnek: Kapsayıcı seçin](media/azure-stack-manage-vm-disks/select-container.png)

## <a name="attach-an-existing-data-disk-to-a-vm"></a>Bir VM'ye olan bir veri diski ekleme

1.  [Bir .vhd dosyasını hazırlama](https://docs.microsoft.com/azure/virtual-machines/windows/classic/createupload-vhd) VM veri diski olarak kullanılmak için. Bu .vhd dosyası .vhd dosyasına eklemek istediğiniz VM ile kullandığınız bir depolama hesabına yükleyin.

    İşletim sistemi diskini barındıran kapsayıcı .vhd dosyasından tutmak için farklı bir kapsayıcıya kullanmayı planlayın.   
    ![Örnek: Karşıya VHD dosyası yükleme](media/azure-stack-manage-vm-disks/upload-vhd.png)

2.  .Vhd dosyası karşıya yüklendikten sonra bir VM için VHD'yi hazırsınız. Soldaki menüde **sanal makineler**.  
 ![Örnek: Panoda bir VM seçin](media/azure-stack-manage-vm-disks/vm-dashboard.png)

3.  Sanal makine listeden seçin.

    ![Örnek: Panoda bir VM seçin](media/azure-stack-manage-vm-disks/select-a-vm.png)

4.  Sanal makine için sayfasında seçin **diskleri** > **iliştirme varolan**.   

    ![Örnek: Var olan bir diski ekleme](media/azure-stack-manage-vm-disks/attach-disks2.png)

5.  İçinde **mevcut diski** sayfasında **VHD dosyasını**. **Depolama hesapları** sayfası açılır.    

    ![Örnek: Bir VHD dosyasını seçin](media/azure-stack-manage-vm-disks/select-vhd.png)

6.  Altında **depolama hesapları**, kullanılacak hesabı seçin ve ardından daha önce yüklenmiş .vhd dosyasını barındıran bir kapsayıcı seçin. .Vhd dosyasını seçin ve ardından **seçin** seçimini kaydetmek için.    

    ![Örnek: Kapsayıcı seçin](media/azure-stack-manage-vm-disks/select-container2.png)

7.  Altında **mevcut diski**, seçtiğiniz dosya altında listelenen **VHD dosyasını**. Güncelleştirme **ana bilgisayar önbelleğe alma** disk ayarlama ve ardından **Tamam** VM için yeni disk yapılandırmasını kaydetmek için.    

    ![Örnek: VHD dosyasını ekleme](media/azure-stack-manage-vm-disks/attach-vhd.png)

8.  Azure Stack disk oluşturur ve sanal makineye iliştirir sonra yeni disk sanal makinenin disk ayarları altında listelenir **veri diskleri**.   

    ![Örnek: Tam disk ekleme](media/azure-stack-manage-vm-disks/complete-disk-attach.png)

## <a name="use-powershell-to-add-multiple-unmanaged-disks-to-a-vm"></a>Bir VM'ye birden fazla yönetilmeyen disk eklemek için PowerShell'i kullanma

Bir VM sağlama ve yeni bir veri diski ekleyin veya önceden var olan eklemek için PowerShell kullanabilirsiniz **.vhd** veri diski olarak dosya.

**Add-AzureRmVMDataDisk** cmdlet'i, bir sanal makineye veri diski ekler. Bir sanal makine oluşturduğunuzda veya mevcut bir sanal makineye veri diski ekleyebilirsiniz, bir veri diski ekleyebilirsiniz. Belirtin **VhdUri** farklı kapsayıcılar diskleri dağıtmak için parametre.

### <a name="add-data-disks-to-a-new-virtual-machine"></a>Yeni bir sanal makineye veri diski ekleme
Aşağıdaki örnekler, üç veri diskine bir VM oluşturmak için PowerShell komutlarını kullanın, her farklı bir kapsayıcıda yer.

İlk komut, bir sanal makine oluşturur ve ardından depolar *$VirtualMachine* değişkeni. Komut, sanal makine için bir ad ve boyut atar.

```powershell
$VirtualMachine = New-AzureRmVMConfig -VMName "VirtualMachine" `
                                    -VMSize "Standard_A2"
```

Sonraki üç komutları üç veri diskleri yollarını atayın *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, ve *$DataDiskVhdUri03* değişkenleri. Farklı kapsayıcılar diskleri dağıtmak için URL farklı bir yol adı tanımlayın.

```powershell
$DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
```

```powershell
$DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
```

```powershell
$DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
```

Son üç komutlar, depolanan sanal makineye veri diski ekleme *$VirtualMachine*. Her komut, adı, konumu ve disk ek özelliklerini belirtir. Her disk URI'si depolanan *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, ve *$DataDiskVhdUri03*.

```powershell
$VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk1' `
                -Caching 'ReadOnly' -DiskSizeInGB 10 -Lun 0 `
                -VhdUri $DataDiskVhdUri01 -CreateOption Empty
```

```powershell
$VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk2' `
                -Caching 'ReadOnly' -DiskSizeInGB 11 -Lun 1 `
                -VhdUri $DataDiskVhdUri02 -CreateOption Empty
```

```powershell
$VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk3' `
                -Caching 'ReadOnly' -DiskSizeInGB 12 -Lun 2 `
                -VhdUri $DataDiskVhdUri03 -CreateOption Empty
```

VM için işletim sistemi disk ve ağ yapılandırması eklemek için aşağıdaki PowerShell komutlarını kullanın ve yeni VM'yi başlatın.

```powershell
#set variables
$rgName = "myResourceGroup"
$location = "local"
#Set OS Disk
$osDiskUri = "https://contoso.blob.local.azurestack.external/vhds/osDisk.vhd"
$osDiskName = "osDisk"

$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $osDiskName -VhdUri $osDiskUri `
    -CreateOption FromImage -Windows

# Create a subnet configuration
$subnetName = "mySubNet"
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24

# Create a vnet configuration
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet

# Create a public IP
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
    -AllocationMethod Dynamic

# Create a network security group configuration
$nsgName = "myNsg"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule

# Create a NIC configuration
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
-Location $location -SubnetId $vnet.Subnets[0].Id -NetworkSecurityGroupId $nsg.Id -PublicIpAddressId $pip.Id

#Create the new VM
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName VirtualMachine | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $VirtualMachine
```

### <a name="add-data-disks-to-an-existing-virtual-machine"></a>Varolan bir sanal makineye veri diski ekleme

Aşağıdaki örnekler, mevcut bir VM'ye üç veri diskleri eklemek için PowerShell komutlarını kullanın.
İlk komut kullanarak VirtualMachine adlı sanal makine alır **Get-AzureRmVM** cmdlet'i. Komut sanal makinesinde depolayan *$VirtualMachine* değişkeni.

```powershell
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "myResourceGroup" `
                                -Name "VirtualMachine"
```
Sonraki üç komutları DataDiskVhdUri01 $ $DataDiskVhdUri02 ve $DataDiskVhdUri03 değişkenlere üç veri disklerinin yolları atayın.  Farklı kapsayıcılar için disk yerleştirme vhduri farklı yol adları gösterir.
```powershell
$DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
```
```powershell
$DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
```
```powershell
$DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
```


Sonraki üç komut depolanan sanal makine veri diskleri ekleme *$VirtualMachine* değişkeni. Her komut, adı, konumu ve disk ek özelliklerini belirtir. Her disk URI'si depolanan *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, ve *$DataDiskVhdUri03*.

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk1" `
                      -VhdUri $DataDiskVhdUri01 -LUN 0 `
                      -Caching ReadOnly -DiskSizeinGB 10 -CreateOption Empty
```

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk2" `
                      -VhdUri $DataDiskVhdUri02 -LUN 1 `
                      -Caching ReadOnly -DiskSizeinGB 11 -CreateOption Empty
```

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk3" `
                      -VhdUri $DataDiskVhdUri03 -LUN 2 `
                      -Caching ReadOnly -DiskSizeinGB 12 -CreateOption Empty
```

Son komut, depolanan sanal makinenin durumunu güncelleştirir *$VirtualMachine* -*ResourceGroupName*.

```powershell
Update-AzureRmVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Stack sanal makineleri hakkında daha fazla bilgi için bkz: [sanal Makineler'de Azure Stack için Değerlendirmeler](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-considerations).
