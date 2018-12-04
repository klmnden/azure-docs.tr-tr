---
title: Azure Stack'te bir VM disklerini yönetme | Microsoft Docs
description: Azure stack'teki sanal makineler için diskler sağlayın.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/03/2018
ms.author: mabrigg
ms.reviewer: jiahan
ms.openlocfilehash: 2e3cec4564c509cd225a9bcd43185f6f5b344e8c
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52833472"
---
# <a name="provision-virtual-machine-disk-storage-in-azure-stack"></a>Azure stack'teki sanal makine disk depolama sağlama

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu makalede, Azure Stack portalını kullanarak veya PowerShell kullanarak sanal makine disk depolama alanı sağlama işlemi açıklanır.

## <a name="overview"></a>Genel Bakış

Azure Stack 1808 sürümünden başlayarak, sanal makinelerde bir işletim sistemi (OS) hem de veri diski olarak yönetilen ve yönetilmeyen disklerle kullanımını destekler. Sürüm 1808 önce yalnızca yönetilmeyen diskleri desteklenir. 

**[Yönetilen diskler](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds#managed-disks)**  VM diskleriyle ilişkili depolama hesaplarını yöneterek Azure Iaas Vm'leri için disk yönetimini basitleştirin. Yalnızca boyutu belirtmek zorunda duyduğunuz disk ve Azure Stack oluşturur ve diski oluşturup yönetebilmesi.

**[Yönetilmeyen diskler](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds#unmanaged-disks)**, oluşturmanızı gerektiren bir [depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) disklerini depolamak için. Oluşturduğunuz diskler, VM diskleri adlandırılır ve depolama hesabındaki kapsayıcıları depolanır.

 

### <a name="best-practice-guidelines"></a>En iyi uygulama kılavuzları

Performansı artırmak ve maliyetleri azaltmak için her sanal makine disk içinde ayrı bir kapsayıcıya Yerleştir öneririz. Bir kapsayıcı, bir işletim sistemi diski veya veri diski ancak aynı anda hem tutun. (Ancak, hiçbir şey yoktur aynı kapsayıcıda her iki tür disk yerleştirmekten önlemek için.)

Bir sanal Makineye bir veya daha fazla veri diski ekleyin, ek kapsayıcıları bir konumu olarak bu disklerini depolamak için kullanın. Ek sanal makineler için işletim sistemi diski, ayrıca kendi kapsayıcılarda olmalıdır.

Birden çok VM oluşturduğunuzda, aynı depolama hesabını her yeni sanal makine için yeniden kullanabilirsiniz. Yalnızca oluşturduğunuz kapsayıcı benzersiz olmalıdır.

### <a name="adding-new-disks"></a>Yeni disk ekleme

Portalı kullanarak ve PowerShell kullanarak diskleri ekleme aşağıdaki tabloda özetlenmiştir.

| Yöntem | Seçenekler
|-|-|
|[Kullanıcı Portalı](#use-the-portal-to-add-additional-disks-to-a-vm)|-Mevcut bir VM'ye yeni veri diskleri ekleme. Yeni diskler, Azure Stack tarafından oluşturulur. </br> </br>-Daha önce sağlanan bir VM için mevcut bir disk (.vhd) dosyasına ekleyin. Bunu yapmak için .vhd hazırlamak ve Azure Stack'e dosyayı karşıya yükleyin. |
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

1.  Portalında, **sanal makineler**.    
    ![Örnek: Sanal makine Panosu'ndan](media/azure-stack-manage-vm-disks/vm-dashboard.png)

2.  Daha önce sağlanmış bir sanal makineyi seçin.   
    ![Örnek: bir VM Pano seçin.](media/azure-stack-manage-vm-disks/select-a-vm.png)

3.  Sanal makine için seçin **diskleri** > **iliştirme yeni**.       
    ![Örnek: yeni bir disk VM'e ekleyin.](media/azure-stack-manage-vm-disks/Attach-disks.png)    

4.  İçinde **yeni disk Attach** bölmesinde **konumu**. Varsayılan olarak, konum, işletim sistemi diskini barındıran aynı kapsayıcıya ayarlanır.      
    ![Örnek: disk konumu ayarlayın](media/azure-stack-manage-vm-disks/disk-location.png)

5.  Seçin **depolama hesabı** kullanılacak. Ardından, **kapsayıcı** veri diski yerleştirmek istediğiniz. Gelen **kapsayıcıları** sayfası oluşturmak için kullanabileceğiniz yeni bir kapsayıcı istiyorsanız. Yeni disk konumu için kendi kapsayıcı daha sonra değiştirebilirsiniz. Her disk için ayrı bir kapsayıcı kullandığınızda, performansı geliştirebilir veri diski yerleşimini dağıtın. Seçin **seçin** seçimini kaydetmek için.     
    ![Örnek: bir kapsayıcı seçin](media/azure-stack-manage-vm-disks/select-container.png)

6.  İçinde **yeni disk Attach** sayfasında, güncelleştirme **adı**, **türü**, **boyutu**, ve **ana bilgisayar önbelleğe alma** ayarları diski. Ardından **Tamam** VM için yeni disk yapılandırmasını kaydetmek için.  
    ![Örnek: Tam disk eki](media/azure-stack-manage-vm-disks/complete-disk-attach.png)  

7.  Azure Stack disk oluşturur ve sanal makineye iliştirir sonra yeni disk sanal makinenin disk ayarları altında listelenir **veri DİSKLERİ**.   
    ![Örnek: Görünüm disk](media/azure-stack-manage-vm-disks/view-data-disk.png)


### <a name="attach-an-existing-data-disk-to-a-vm"></a>Bir VM'ye olan bir veri diski ekleme

1.  [Bir .vhd dosyasını hazırlama](https://docs.microsoft.com/azure/virtual-machines/windows/classic/createupload-vhd) VM veri diski olarak kullanılmak için. Bu .vhd dosyası .vhd dosyasına eklemek istediğiniz VM ile kullandığınız bir depolama hesabına yükleyin.

  İşletim sistemi diskini barındıran kapsayıcı .vhd dosyasından tutmak için farklı bir kapsayıcıya kullanmayı planlayın.   
  ![Örnek: bir VHD dosyasını karşıya yükleyin](media/azure-stack-manage-vm-disks/upload-vhd.png)

2.  .Vhd dosyası karşıya yüklendikten sonra bir VM için VHD'yi hazırsınız. Soldaki menüde **sanal makineler**.  
 ![Örnek: bir VM Pano seçin.](media/azure-stack-manage-vm-disks/vm-dashboard.png)

3.  Sanal makine listeden seçin.    
  ![Örnek: bir VM Pano seçin.](media/azure-stack-manage-vm-disks/select-a-vm.png)

4.  Sanal makine için sayfasında seçin **diskleri** > **iliştirme varolan**.   
  ![Örnek: var olan bir diski](media/azure-stack-manage-vm-disks/attach-disks2.png)

5.  İçinde **mevcut diski** sayfasında **VHD dosyasını**. **Depolama hesapları** sayfası açılır.    
  ![Örnek: bir VHD dosyasını seçin](media/azure-stack-manage-vm-disks/select-vhd.png)

6.  Altında **depolama hesapları**, kullanılacak hesabı seçin ve ardından daha önce yüklenmiş .vhd dosyasını barındıran bir kapsayıcı seçin. .Vhd dosyasını seçin ve ardından **seçin** seçimini kaydetmek için.    
  ![Örnek: bir kapsayıcı seçin](media/azure-stack-manage-vm-disks/select-container2.png)

7.  Altında **mevcut diski**, seçtiğiniz dosya altında listelenen **VHD dosyasını**. Güncelleştirme **ana bilgisayar önbelleğe alma** disk ayarlama ve ardından **Tamam** VM için yeni disk yapılandırmasını kaydetmek için.    
  ![Örnek: VHD dosyasını ekleme](media/azure-stack-manage-vm-disks/attach-vhd.png)

8.  Azure Stack disk oluşturur ve sanal makineye iliştirir sonra yeni disk sanal makinenin disk ayarları altında listelenir **veri diskleri**.   
  ![Örnek: tam disk ekleme](media/azure-stack-manage-vm-disks/complete-disk-attach.png)


## <a name="use-powershell-to-add-multiple-unmanaged-disks-to-a-vm"></a>Bir VM'ye birden fazla yönetilmeyen disk eklemek için PowerShell'i kullanma
Bir VM sağlama ve yeni bir veri diski ekleyin veya önceden var olan eklemek için PowerShell kullanabilirsiniz **.vhd** veri diski olarak dosya.

**Add-AzureRmVMDataDisk** cmdlet'i, bir sanal makineye veri diski ekler. Bir sanal makine oluşturduğunuzda veya mevcut bir sanal makineye veri diski ekleyebilirsiniz, bir veri diski ekleyebilirsiniz. Belirtin **VhdUri** farklı kapsayıcılar diskleri dağıtmak için parametre.

### <a name="add-data-disks-to-a-new-virtual-machine"></a>Yeni bir sanal makineye veri diski ekleme
Aşağıdaki örnekler, üç veri diskine bir VM oluşturmak için PowerShell komutlarını kullanın, her farklı bir kapsayıcıda yer.

İlk komut, bir sanal makine oluşturur ve ardından depolar *$VirtualMachine* değişkeni. Komut, sanal makine için bir ad ve boyut atar.
  ```
  $VirtualMachine = New-AzureRmVMConfig -VMName "VirtualMachine" `
                                      -VMSize "Standard_A2"
  ```

Sonraki üç komutları üç veri diskleri yollarını atayın *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, ve *$DataDiskVhdUri03* değişkenleri. Farklı kapsayıcılar diskleri dağıtmak için URL farklı bir yol adı tanımlayın.     
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```

  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```

  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```

Son üç komutlar, depolanan sanal makineye veri diski ekleme *$VirtualMachine*. Her komut, adı, konumu ve disk ek özelliklerini belirtir. Her disk URI'si depolanan *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, ve *$DataDiskVhdUri03*.
  ```
  $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk1' `
                  -Caching 'ReadOnly' -DiskSizeInGB 10 -Lun 0 `
                  -VhdUri $DataDiskVhdUri01 -CreateOption Empty
  ```

  ```
  $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk2' `
                 -Caching 'ReadOnly' -DiskSizeInGB 11 -Lun 1 `
                 -VhdUri $DataDiskVhdUri02 -CreateOption Empty
  ```

  ```
  $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk3' `
                  -Caching 'ReadOnly' -DiskSizeInGB 12 -Lun 2 `
                  -VhdUri $DataDiskVhdUri03 -CreateOption Empty
  ```

VM için işletim sistemi disk ve ağ yapılandırması eklemek için aşağıdaki PowerShell komutlarını kullanın ve yeni VM'yi başlatın.
  ```
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

  # Create a network security group cnfiguration
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
  ```
  $VirtualMachine = Get-AzureRmVM -ResourceGroupName "myResourceGroup" `
                                  -Name "VirtualMachine"
  ```
Sonraki üç komutları DataDiskVhdUri01 $ $DataDiskVhdUri02 ve $DataDiskVhdUri03 değişkenlere üç veri disklerinin yolları atayın.  Farklı kapsayıcılar için disk yerleştirme vhduri farklı yol adları gösterir.
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```
  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```
  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```


  Sonraki üç komut depolanan sanal makine veri diskleri ekleme *$VirtualMachine* değişkeni. Her komut, adı, konumu ve disk ek özelliklerini belirtir. Her disk URI'si depolanan *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, ve *$DataDiskVhdUri03*.
  ```
  Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk1" `
                        -VhdUri $DataDiskVhdUri01 -LUN 0 `
                        -Caching ReadOnly -DiskSizeinGB 10 -CreateOption Empty
  ```
  ```
  Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk2" `
                        -VhdUri $DataDiskVhdUri02 -LUN 1 `
                        -Caching ReadOnly -DiskSizeinGB 11 -CreateOption Empty
  ```
  ```
  Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk3" `
                        -VhdUri $DataDiskVhdUri03 -LUN 2 `
                        -Caching ReadOnly -DiskSizeinGB 12 -CreateOption Empty
  ```


  Son komut, depolanan sanal makinenin durumunu güncelleştirir *$VirtualMachine* -*ResourceGroupName*.
  ```
  Update-AzureRmVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
  ```
<!-- Pending scripts  

## Distribute the data disks of an existing VM
If you have a VM with more than one disk in the same container, the service operator of the Azure Stack deployment might ask you to redistribute the disks into individual containers.

To do so, use the scripts from the following location in GitHub. These scripts can be used to move the data disks to different containers.
-->

## <a name="next-steps"></a>Sonraki Adımlar
Azure Stack sanal makineleri hakkında daha fazla bilgi için bkz: [sanal Makineler'de Azure Stack için Değerlendirmeler](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-considerations).
