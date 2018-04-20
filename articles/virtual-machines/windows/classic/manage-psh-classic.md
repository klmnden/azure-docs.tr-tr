---
title: Azure PowerShell kullanarak sanal makinelerinizi yönetme | Microsoft Docs
description: Sanal makinelerinizi yönetme görevlerini otomatikleştirmek için kullanabileceğiniz komutlar hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: windows
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 7cdf9bd3-6578-4069-8a95-e8585f04a394
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2016
ms.author: kasing
ms.openlocfilehash: 942141fad09e6233efc7f850212a73f8a39c163c
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Azure PowerShell kullanarak sanal makinelerinizi yönetme
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager modelini kullanarak ortak PowerShell komutları için bkz: [burada](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Birçok görevi her gün, sanal makineleri yönetmek için bunu Azure PowerShell cmdlet'lerini kullanarak otomatik olarak. Bu makalede, örnek komutları daha basit görevleri ve bağlantıları için daha karmaşık görevleri için komutları göster makalelere sağlar.

> [!NOTE]
> Azure PowerShell'i yükleyip henüz henüz makaledeki yönergeleri alabilir, [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).
> 
> 

## <a name="how-to-use-the-example-commands"></a>Örnek komutları kullanma
Bazı komutlar metinde ortamınız için uygun olan metin ile değiştirmeniz gerekir. < Ve > simgeler gereksinim değiştirmek için metin gösterir. Metin değiştirdiğinizde simgeleri Kaldır ancak tırnak işaretleri bırakın.

## <a name="get-a-vm"></a>VM Al
Bu, genellikle kullanacağınız temel bir görevdir. VM hakkında bilgi almak, bir VM üzerinde görevleri veya bir değişkeni depolamak için bir çıkış almanız için kullanın.

VM hakkında bilgi almak için de dahil olmak üzere tırnak, her şeyi değiştirerek bu komutu çalıştırın < ve > karakterleri:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Çıktı $vm değişkeninde depolamak için çalıştırın:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Oturum Windows tabanlı bir VM açın
Şu komutları çalıştırın:

> [!NOTE]
> Sanal makine ve bulut hizmeti adı görünümünden alabilirsiniz **Get-AzureVM** komutu.
> 
> $svcName = "<cloud service name>" $vmName = "<virtual machine name>" $localPath = "< sürücü ve klasör konumu indirilen RDP dosyasını depolamak için örnek: c:\temp >" $localFile $localPath = + "\" $vmname +".rdp"Get-AzureRemoteDesktopFile - ServiceName $svcName-$vmName - LocalPath $localFile ad-Başlat
> 
> 

## <a name="stop-a-vm"></a>VM durdurma
Şu komutu çalıştırın:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> Bu bulut Hizmeti'nde son VM olması durumunda sanal IP (VIP) bulut hizmeti tutmak için bu parametreyi kullanın. <br><br> StayProvisioned parametreyi kullanırsanız, VM için fatura.
> 
> 

## <a name="start-a-vm"></a>VM başlatma
Şu komutu çalıştırın:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Veri diski ekleme
Bu görev birkaç adımı gerektirir. İlk olarak, kullandığınız *** Ekle-$vm nesnesine disk eklemek için AzureDataDisk *** cmdlet'i. Ardından, kullandığınız **güncelleştirme-AzureVM** cmdlet'i VM'ye yapılandırmasını güncelleştirmek için.

Ayrıca yeni bir disk veya veri içeren bir disk eklemeye karar vermeniz gerekir. Yeni bir disk için komut .vhd dosyası oluşturur ve bunları ekler.

Yeni bir disk eklemek için şu komutu çalıştırın:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Var olan bir veri diski eklemek için şu komutu çalıştırın:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Blob depolama alanındaki bir .vhd dosyasından veri diski eklemek için şu komutu çalıştırın:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Windows tabanlı bir VM oluşturma
Azure'da yeni bir Windows tabanlı sanal makine oluşturmak için konusundaki yönergeleri kullanın [oluşturmak ve Windows tabanlı sanal makineleri önceden için Azure PowerShell](create-powershell.md). Bu konuda adım adım komut kümesi bir Azure PowerShell oluşturulmasını size önceden yapılandırılabilir Windows tabanlı bir VM oluşturur:

* Active Directory etki alanı üyeliği ile.
* Ek disklerle.
* Varolan yük dengeli bir üye olarak ayarlayın.
* Statik bir IP adresine sahip.

