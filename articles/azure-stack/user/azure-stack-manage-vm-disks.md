---
title: Azure yığınında VM diskleri yönetme | Microsoft Docs
description: Azure yığınında sanal makineler için diskleri sağlayın.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 4e5833cf-4790-4146-82d6-737975fb06ba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/11/2018
ms.author: brenduns
ms.reviewer: jiahan
ms.openlocfilehash: 8e91b4d83aa90a7e744fb8e73cda788dbf8c58ec
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="provision-virtual-machine-disk-storage-in-azure-stack"></a>Sanal makine disk depolama alanı hazırla Azure yığınında

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bu makalede, Azure yığın Portalı'nı kullanarak veya PowerShell kullanarak sanal makine disk depolama sağlamak açıklar.

## <a name="overview"></a>Genel Bakış

Azure yığın kullanımını destekler [yönetilmeyen diskleri](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds#unmanaged-disks) bir işletim sistemi (OS) ve bir veri diski olarak sanal makinelerde.

Yönetilmeyen diskleri kullanmak için oluşturduğunuz bir [depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) disklerini depolamak için. Oluşturduğunuz diskleri VM diskleri olarak adlandırılır ve kapsayıcılarında depolama hesabında depolanır.

### <a name="best-practice-guidelines"></a>En iyi yöntem kılavuzları

Performansı iyileştirmek ve genel maliyetleri azaltmak için ayrı bir kapsayıcıda her VM disk yerleştirin öneririz. Bir kapsayıcı, bir işletim sistemi diski veya veri diski ancak ikisini aynı anda tutun. (Ancak, şey yoktur aynı kapsayıcıda disk her iki tür koyma önlemek için.)

Ek kapsayıcıları konumu olarak bir VM için bir veya daha fazla veri diski eklerseniz, bu diskleri depolamak için kullanın. Ek VM'ler için işletim sistemi diski de kendi kapsayıcılarında olmalıdır.

Birden çok VM oluşturduğunuzda, aynı depolama hesabı her yeni bir sanal makine için yeniden kullanabilirsiniz. Oluşturduğunuz kapsayıcıları benzersiz olmalıdır.

### <a name="adding-new-disks"></a>Yeni disk ekleme

Aşağıdaki tabloda diskleri portalını kullanarak ve PowerShell kullanarak nasıl ekleneceği özetler.

| Yöntem | Seçenekler
|-|-|
|[Kullanıcı Portalı](#use-the-portal-to-add-additional-disks-to-a-vm)|-Mevcut bir VM'yi yeni veri diski ekleyin. Yeni diskler Azure yığını tarafından oluşturulur. </br> </br>-Daha önce sağlanan bir VM için var olan bir disk (.vhd) dosyasına ekleyin. Bunu yapmak için .vhd hazırlamak ve Azure yığınına dosyasını karşıya yükleyin. |
|[PowerShell](#use-powershell-to-add-multiple-unmanaged-disks-to-a-vm) | -Bir işletim sistemi diski ile yeni bir VM oluşturun ve aynı anda o VM için bir veya daha fazla disk ekleyin. |

## <a name="use-the-portal-to-add-disks-to-a-vm"></a>Bir VM diskleri eklemek üzere portalı kullanın

Çoğu Market öğesi için bir VM oluşturmak için portalı kullandığınızda, varsayılan olarak, yalnızca işletim sistemi diski oluşturulur.

Bir VM oluşturduktan sonra portala kullanabilirsiniz:
* Yeni bir veri diski oluşturun ve VM'e ekleyin.
* Varolan bir veri diski karşıya yükleme ve VM'e ekleyin.

Eklediğiniz her yönetilmeyen disk ayrı bir kapsayıcıda moduna geçirmelisiniz.

>[!NOTE]
>Oluşturulan ve Azure tarafından yönetilen diskleri çağrılır [yönetilen diskleri](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/managed-disks-overview).

### <a name="use-the-portal-to-create-and-attach-a-new-data-disk"></a>Oluşturun ve yeni bir veri diskini üzere portalı kullanın

1.  Portalı'nda seçin **sanal makineleri**.    
    ![Örnek: VM Panosu](media/azure-stack-manage-vm-disks/vm-dashboard.png)

2.  Daha önce sağlanan bir sanal makineyi seçin.   
    ![Örnek: VM panosunda seçin.](media/azure-stack-manage-vm-disks/select-a-vm.png)

3.  Sanal makine için seçin **diskleri** > **Attach yeni**.       
    ![Örnek: yeni bir disk VM'e ekleyin.](media/azure-stack-manage-vm-disks/Attach-disks.png)    

4.  İçinde **Attach yeni disk** bölmesinde, **konumu**. Varsayılan olarak, işletim sistemi diski tutan kapsayıcıya konum ayarlanır.      
    ![Örnek: disk konumu ayarlayın](media/azure-stack-manage-vm-disks/disk-location.png)

5.  Seçin **depolama hesabı** kullanmak için. Ardından, **kapsayıcı** veri diski yerleştirmek istediğiniz. Gelen **kapsayıcıları** sayfasında, oluşturabileceğiniz yeni bir kapsayıcı istiyorsanız. Daha sonra kendi kapsayıcıya yeni disk konumunu değiştirebilirsiniz. Her disk için ayrı bir kapsayıcı kullandığınızda, performansı geliştirebilir veri diski yerleşimini dağıtın. Seçin **seçin** seçimi kaydetmek için.     
    ![Örnek: bir kapsayıcı seçin](media/azure-stack-manage-vm-disks/select-container.png)

6.  İçinde **Attach yeni disk** sayfasında, güncelleştirme **adı**, **türü**, **boyutu**, ve **ana bilgisayar önbelleğe alma** ayarları disk. Ardından **Tamam** VM için yeni disk yapılandırmasını kaydetmek için.  
    ![Örnek: Tam disk eki](media/azure-stack-manage-vm-disks/complete-disk-attach.png)  

7.  Azure yığın disk oluşturur ve sanal makineye iliştirir sonra yeni disk sanal makinenin disk ayarları altında listelenen **veri DİSKLERİ**.   
    ![Örnek: Diski görüntüleme](media/azure-stack-manage-vm-disks/view-data-disk.png)


### <a name="attach-an-existing-data-disk-to-a-vm"></a>Varolan bir veri diski bir VM'e ekleyin

1.  [.Vhd dosyasını hazırlama](https://docs.microsoft.com/azure/virtual-machines/windows/classic/createupload-vhd) bir VM için veri diski olarak kullanmak için. .Vhd dosyasına eklemek istediğiniz VM kullanan bir depolama hesabı, .vhd dosyası yükleyin.

  İşletim sistemi diski tutan kapsayıcı daha .vhd dosyası tutmak için farklı bir kapsayıcı kullanmayı planlayın.   
  ![Örnek: bir VHD dosyasını karşıya yükleyin](media/azure-stack-manage-vm-disks/upload-vhd.png)

2.  .Vhd dosyasını karşıya yüklendikten sonra VHD'yi VM'e hazırsınız. Soldaki menüde seçin **sanal makineleri**.  
 ![Örnek: VM panosunda seçin.](media/azure-stack-manage-vm-disks/vm-dashboard.png)

3.  Sanal makine listeden seçin.    
  ![Örnek: VM panosunda seçin.](media/azure-stack-manage-vm-disks/select-a-vm.png)

4.  Sanal makine için sayfasında seçin **diskleri** > **Attach varolan**.   
  ![Örnek: var olan bir diski kullanıma açın](media/azure-stack-manage-vm-disks/attach-disks2.png)

5.  İçinde **varolan bir diski İlişti** sayfasında, **VHD dosyasını**. **Depolama hesapları** sayfası açılır.    
  ![Örnek: bir VHD dosyasını seçin](media/azure-stack-manage-vm-disks/select-vhd.png)

6.  Altında **depolama hesapları**kullanılacak hesabı seçin ve ardından daha önce karşıya .vhd dosyası tutan bir kapsayıcı seçin. .Vhd dosyasını seçin ve ardından **seçin** seçimi kaydetmek için.    
  ![Örnek: bir kapsayıcı seçin](media/azure-stack-manage-vm-disks/select-container2.png)

7.  Altında **varolan bir diski İlişti**, seçtiğiniz dosya altında listelenen **VHD dosyasını**. Güncelleştirme **ana bilgisayar önbelleğe alma** disk ayarlama ve ardından **Tamam** VM için yeni disk yapılandırmasını kaydetmek için.    
  ![Örnek: VHD dosyası ekleme](media/azure-stack-manage-vm-disks/attach-vhd.png)

8.  Azure yığın disk oluşturur ve sanal makineye iliştirir sonra yeni disk sanal makinenin disk ayarları altında listelenen **veri diskleri**.   
  ![Örnek: tam disk ekleme](media/azure-stack-manage-vm-disks/complete-disk-attach.png)


## <a name="use-powershell-to-add-multiple-unmanaged-disks-to-a-vm"></a>Bir VM için birden çok yönetilmeyen disk eklemek için PowerShell kullanın
PowerShell VM sağlamak ve yeni bir veri diski ekleyin veya önceden varolan eklemek için kullanabileceğiniz **.vhd** dosyası bir veri diski olarak.

**Ekle AzureRmVMDataDisk** cmdlet, bir sanal makineye bir veri diski ekler. Bir sanal makine oluşturun veya varolan bir sanal makineye bir veri diski ekleyin, bir veri diski ekleyebilirsiniz. Belirtin **VhdUri** farklı kapsayıcılar disklere dağıtma parametresi.

### <a name="add-data-disks-to-a-new-virtual-machine"></a>Yeni bir sanal makine veri diski Ekle
Aşağıdaki örnekler, üç veri diskleri bir VM oluşturmak için PowerShell komutlarını kullanın, her farklı bir kapsayıcıda yerleştirilir.

İlk komut bir sanal makine oluşturur ve ardından depolar *$VirtualMachine* değişkeni. Komut bir ad ve boyutunu sanal makineyi atar.
  ```
  $VirtualMachine = New-AzureRmVMConfig -VMName "VirtualMachine" `
                                      -VMSize "Standard_A2"
  ```

Sonraki üç komutları üç veri disklerinin yolları atayın *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, ve *$DataDiskVhdUri03* değişkenleri. Farklı kapsayıcılar disklere dağıtmak için URL'de farklı bir yol adı tanımlayın.     
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```

  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```

  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```

Son üç komut depolanan sanal makine veri diski ekleme *$VirtualMachine*. Her komut adını, konumunu ve diskin ek özelliklerini belirtir. Her disk URI'sini depolanan *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, ve *$DataDiskVhdUri03*.
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

VM için işletim sistemi disk ve ağ yapılandırması eklemek için aşağıdaki PowerShell komutlarını kullanın ve yeni VM başlatın.
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



### <a name="add-data-disks-to-an-existing-virtual-machine"></a>Varolan bir sanal makineye veri diski Ekle
Aşağıdaki örneklerde, üç veri diskleri için mevcut bir VM'yi eklemek için PowerShell komutlarını kullanın.
İlk komut kullanarak VirtualMachine adlı sanal makineye alır **Get-AzureRmVM** cmdlet'i. Sanal makinede komut depolar *$VirtualMachine* değişkeni.
  ```
  $VirtualMachine = Get-AzureRmVM -ResourceGroupName "myResourceGroup" `
                                  -Name "VirtualMachine"
  ```
Sonraki üç komutları üç veri disklerinin yolları $DataDiskVhdUri01, $DataDiskVhdUri02 ve $DataDiskVhdUri03 değişkenlere atayın.  Vhduri farklı yol adlarında disk yerleştirme için farklı kapsayıcılar gösterir.
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```
  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```
  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```


  Sonraki üç komut depolanan sanal makine veri diski ekleme *$VirtualMachine* değişkeni. Her komut adını, konumunu ve diskin ek özelliklerini belirtir. Her disk URI'sini depolanan *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, ve *$DataDiskVhdUri03*.
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


  Son komut depolanan sanal makinenin durumunu güncelleştirir *$VirtualMachine* içinde -*ResourceGroupName*.
  ```
  Update-AzureRmVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
  ```
<!-- Pending scripts  

## Distribute the data disks of an existing VM
If you have a VM with more than one disk in the same container, the service operator of the Azure Stack deployment might ask you to redistribute the disks into individual containers.

To do so, use the scripts from the following location in GitHub. These scripts can be used to move the data disks to different containers.
-->

## <a name="next-steps"></a>Sonraki Adımlar
Azure yığın sanal makineler hakkında daha fazla bilgi için bkz: [Azure yığınında sanal makineler için ilgili önemli noktalar](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-considerations).
