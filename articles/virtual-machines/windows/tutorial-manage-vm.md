---
title: Öğretici - Azure PowerShell ile Windows VM’leri oluşturma ve yönetme | Microsoft Docs
description: Bu öğreticide, Azure PowerShell kullanarak Azure’da Windows VM’leri oluşturup yönetmeyi öğrenirsiniz
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/02/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: b725713777eb6ca25c829d327f91921b28cd4203
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51035977"
---
# <a name="tutorial-create-and-manage-windows-vms-with-azure-powershell"></a>Öğretici: Azure PowerShell ile Windows VM’leri Oluşturma ve Yönetme

Azure sanal makineleri tam olarak yapılandırılabilir ve esnek bir bilgi işlem ortamı sağlar. Bu öğretici sanal makine (VM) boyutu seçme, VM görüntüsü seçme ve VM dağıtma gibi temel Azure sanal makine dağıtımı görevlerini kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * VM oluşturma ve bir VM’ye bağlanma
> * VM görüntülerini seçme ve kullanma
> * Belirli VM boyutlarını görüntüleme ve kullanma
> * VM’yi yeniden boyutlandırma
> * VM durumunu görüntüleme ve anlama

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutu ile yeni bir kaynak grubu oluşturun.

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir sanal makineden önce bir kaynak grubu oluşturulmalıdır. Bu örnekte, *EastUS* bölgesinde *myResourceGroupVM* adlı bir kaynak grubu oluşturulur:

```azurepowershell-interactive
New-AzureRmResourceGroup `
   -ResourceGroupName "myResourceGroupVM" `
   -Location "EastUS"
```

Kaynak grubu, bu öğretici boyunca görülebileceği gibi bir VM oluşturulurken veya değiştirilirken belirtilir.

## <a name="create-a-vm"></a>VM oluşturma

VM oluştururken, işletim sistemi görüntüsü, ağ yapılandırması ve yönetici kimlik bilgileri gibi çeşitli seçenekler bulunur. Bu örnekte Windows Server 2016 Datacenter'ın varsayılan sürümünü çalıştıran *myVM* adlı bir VM oluşturulur.

[Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-6) komutuyla VM’de yönetici hesabı için gereken kullanıcı adı ve parolasını ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile sanal makineyi oluşturun.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM" `
    -Location "EastUS" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```

## <a name="connect-to-vm"></a>VM’ye bağlanma

Dağıtım tamamlandıktan sonra VM ile bir uzak masaüstü bağlantısı oluşturun.

VM'nin genel IP adresini döndürmek için aşağıdaki komutları çalıştırın. Sonraki bir adımda web bağlantısını test etmek üzere tarayıcınızla bağlanabilmek için bu IP Adresini not alın.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
   -ResourceGroupName "myResourceGroupVM"  | Select IpAddress
```

VM ile bir uzak masaüstü oturumu oluşturmak için yerel makinenizde aşağıdaki komutu kullanın. IP adresini, sanal makinenizin *publicIPAddress* değeriyle değiştirin. İstendiğinde, VM oluşturulurken kullanılan kimlik bilgilerini girin.

```powershell
mstsc /v:<publicIpAddress>
```

**Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. VM için oluşturduğunuz kullanıcı adı ve parolayı yazıp **Tamam**’a tıklayın.

## <a name="understand-marketplace-images"></a>Market resimlerini anlama

Azure Market, yeni VM oluşturmak için kullanılabilecek çok sayıda görüntü içerir. Önceki adımlarda, Windows Server 2016 Datacenter görüntüsü kullanılarak bir VM oluşturuldu. Bu adımda, PowerShell modülü markette diğer Windows görüntülerini aramak için kullanılır, bu da yeni sanal makinelerin toplandığı yer olarak ayrıca kullanılabilir. Bu işlem, görüntüyü [tanımlamak](cli-ps-findimage.md#terminology) için yayımcının, teklifin, SKU’nun ve isteğe bağlı olarak bir sürüm numarasının bulunmasını kapsar.

Görüntü yayımcılarının bir listesini döndürmek için [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) komutunu kullanın:

```azurepowershell-interactive
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Görüntü tekliflerinin bir listesini döndürmek için [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) komutunu kullanın. Bu komutla, döndürülen liste belirtilen `MicrosoftWindowsServer` adlı yayımcı üzerinde filtrelenir:

```azurepowershell-interactive
Get-AzureRmVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"
```

Sonuçlar şu örneğe benzer olacaktır: 

```powershell
Offer             PublisherName          Location
-----             -------------          --------
Windows-HUB       MicrosoftWindowsServer EastUS
WindowsServer     MicrosoftWindowsServer EastUS
WindowsServer-HUB MicrosoftWindowsServer EastUS
```

[Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) komutu daha sonra görüntü adlarının bir listesini döndürmek için yayımcı ve teklif adını filtreler.

```azurepowershell-interactive
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

Sonuçlar şu örneğe benzer olacaktır: 

```powershell
Skus                                      Offer         PublisherName          Location
----                                      -----         -------------          --------
2008-R2-SP1                               WindowsServer MicrosoftWindowsServer EastUS  
2008-R2-SP1-smalldisk                     WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter                        WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter-smalldisk              WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core               WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core-smalldisk     WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers           WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers-smalldisk WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-RDSH                 WindowsServer MicrosoftWindowsServer EastUS
2016-Nano-Server                          WindowsServer MicrosoftWindowsServer EastUS
```

Bu bilgiler, belirli bir görüntüye sahip bir VM’yi dağıtmak için kullanılabilir. Bu örnek, Kapsayıcılar’ı içeren Windows Server 2016 görüntüsünün en son sürümünü kullanarak VM'yi dağıtır.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM2" `
    -Location "EastUS" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress2" `
    -ImageName "MicrosoftWindowsServer:WindowsServer:2016-Datacenter-with-Containers:latest" `
    -Credential $cred `
    -AsJob
```

PowerShell komut istemlerinin size döndürülmesi için `-AsJob` parametresi VM’yi arka plan görevi olarak oluşturur. Arka plan işlerinin ayrıntılarını `Get-Job` cmdlet'i ile görüntüleyebilirsiniz.

## <a name="understand-vm-sizes"></a>VM boyutlarını anlama

VM'nin boyutu VM tarafından kullanılabilen CPU, GPU ve bellek gibi kaynakların miktarını belirler. Sanal makineler, beklenen iş yüküne uygun bir VM boyutu kullanılarak oluşturulmalıdır. İş yükü artarsa, mevcut sanal makine de yeniden boyutlandırılabilir.

### <a name="vm-sizes"></a>VM Boyutları

Aşağıdaki tabloda boyutlar kullanım durumlarına göre kategorilere ayrılmaktadır.  
| Tür                     | Ortak boyutlar           |    Açıklama       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Genel amaçlı](sizes-general.md)         |B, Dsv3, Dv3, DSv2, Dv2, Av2, DC| Dengeli CPU/bellek. Küçük ve orta ölçekli uygulama ve veri çözümlerini geliştirmek/test etmek için idealdir.  |
| [İşlem için iyileştirilmiş](sizes-compute.md)   | Fsv2, Fs, F             | Yüksek CPU/bellek. Orta düzey trafiğe sahip uygulamalar, ağ gereçleri ve toplu işlemler için idealdir.        |
| [Bellek için iyileştirilmiş](sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, Dv2  | Yüksek bellek/çekirdek. İlişkisel veritabanı, orta veya büyük boyutlu önbellekler ve bellek içi analiz için idealdir.                 |
| [Depolama için iyileştirilmiş](sizes-storage.md)      | Ls                | Yüksek disk aktarım hızı ve GÇ. Büyük Veri, SQL ve NoSQL veritabanları için ideal.                                                         |
| [GPU](sizes-gpu.md)          | NV, NVv2, NC, NCv2, NCv3, ND            | Ağır grafik işlemleri ile video düzenleme işlemleri için özel olarak hedeflenen VM’ler.       |
| [Yüksek performans](sizes-hpc.md) | H        | İşleme düzeyi yüksek olan isteğe bağlı ağ arabirimleri (RDMA) içeren VM’lerimiz, şimdiye kadarki en güçlü CPU ile sunuluyor. |

### <a name="find-available-vm-sizes"></a>Kullanılabilir VM boyutlarını bulma

Belirli bir bölgede kullanılabilen VM boyutlarının listesini görmek için, [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) komutunu kullanın.

```azurepowershell-interactive
Get-AzureRmVMSize -Location "EastUS"
```

## <a name="resize-a-vm"></a>VM’yi yeniden boyutlandırma

VM dağıtıldıktan sonra, kaynak ayırmayı artırmak veya azaltmak için yeniden boyutlandırılabilir.

Bir VM’yi yeniden boyutlandırmadan önce, istenen boyutun VM kümesinde kullanılabilir olup olmadığını denetleyin. [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) komutu, boyutlarının listesini döndürür.

```azurepowershell-interactive
Get-AzureRmVMSize -ResourceGroupName "myResourceGroupVM" -VMName "myVM"
```

İstenen boyut kullanılabilirse, VM açık durumdayken yeniden boyutlandırılabilir ancak işlem sırasında yeniden başlatılır.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroupVM"  -VMName "myVM"
$vm.HardwareProfile.VmSize = "Standard_DS3_v2"
Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroupVM"
```

İstediğiniz boyut geçerli kümede değilse, yeniden boyutlandırma işlemi gerçekleştirilmeden önce VM’nin serbest bırakılması gerekir. VM'nin serbest bırakılmasıyla geçici diskteki tüm veriler kaldırılır ve statik IP adresi kullanılmadığı sürece genel IP adresi değişir.

```azurepowershell-interactive
Stop-AzureRmVM `
   -ResourceGroupName "myResourceGroupVM" `
   -Name "myVM" -Force
$vm = Get-AzureRmVM `
   -ResourceGroupName "myResourceGroupVM"  `
   -VMName "myVM"
$vm.HardwareProfile.VmSize = "Standard_E2s_v3"
Update-AzureRmVM -VM $vm `
   -ResourceGroupName "myResourceGroupVM"
Start-AzureRmVM `
   -ResourceGroupName "myResourceGroupVM"  `
   -Name $vm.name
```

## <a name="vm-power-states"></a>VM güç durumları

Bir Azure VM’si birçok güç durumuna sahip olabilir. Bu durum VM’nin hiper yönetici açısından bulunduğu geçerli durumu temsil eder.

### <a name="power-states"></a>Güç durumları

| Güç Durumu | Açıklama
|----|----|
| Başlatılıyor | Sanal makinenin başlatıldığını gösterir. |
| Çalışıyor | Sanal makinenin çalıştığını gösterir. |
| Durduruluyor | Sanal makinenin durdurulmakta olduğunu gösterir. |
| Durduruldu | VM'nin durdurulduğunu gösterir. Durduruldu durumundaki sanal makinelere bilgi işlem ücretleri uygulanmaya devam eder.  |
| Serbest bırakılıyor | VM'nin serbest bırakılmakta olduğunu gösterir. |
| Serbest bırakıldı | VM'nin hiper yöneticiden kaldırıldığını ancak denetim masasında hala kullanılabilir olduğunu gösterir. `Deallocated` durumundaki sanal makinelere bilgi işlem ücretleri uygulanmaz. |
| - | VM'nin güç durumunun bilinmediğini gösterir. |

### <a name="find-power-state"></a>Güç durumunu bulma

Belirli bir VM’nin durumunu almak için, [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) komutunu kullanın. VM ve kaynak grubu için geçerli bir ad belirttiğinizden emin olun.

```azurepowershell-interactive
Get-AzureRmVM `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM" `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

Çıkış şu örneğe benzer olacaktır:

```powershell
Status
------
PowerState/running
```

## <a name="management-tasks"></a>Yönetim görevleri

VM'nin yaşam döngüsü boyunca, VM'yi başlatmak, durdurmak veya silmek gibi yönetim görevleri gerçekleştirmek isteyebilirsiniz. Ayrıca, yinelenen veya karmaşık görevleri otomatikleştirmek için betikler oluşturmak isteyebilirsiniz. Azure PowerShell kullanarak, birçok ortak yönetim görevi komut satırından veya betikler içinde çalıştırılabilir.

### <a name="stop-a-vm"></a>VM durdurma

[Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) ile VM'yi durdurun ve serbest bırakın:

```azurepowershell-interactive
Stop-AzureRmVM `
   -ResourceGroupName "myResourceGroupVM" `
   -Name "myVM" -Force
```

VM'yi sağlanan bir durumda tutmak istiyorsanız, - StayProvisioned parametresini kullanın.

### <a name="start-a-vm"></a>VM başlatma

```azurepowershell-interactive
Start-AzureRmVM `
   -ResourceGroupName "myResourceGroupVM" `
   -Name "myVM"
```

### <a name="delete-resource-group"></a>Kaynak grubunu silme

Kaynak grubunu silmek, ayrıca grubun içerdiği tüm kaynakları da siler.

```azurepowershell-interactive
Remove-AzureRmResourceGroup `
   -Name "myResourceGroupVM" `
   -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakiler gibi temel VM oluşturma ve yönetim görevlerini öğrendiniz:

> [!div class="checklist"]
> * VM oluşturma ve bir VM’ye bağlanma
> * VM görüntülerini seçme ve kullanma
> * Belirli VM boyutlarını görüntüleme ve kullanma
> * VM’yi yeniden boyutlandırma
> * VM durumunu görüntüleme ve anlama

VM diskleri hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.  

> [!div class="nextstepaction"]
> [VM diskleri oluşturma ve yönetme](./tutorial-manage-data-disk.md)
