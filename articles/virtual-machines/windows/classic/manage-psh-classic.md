---
title: Azure PowerShell kullanarak sanal makinelerinizi yönetme | Microsoft Docs
description: Sanal makinelerinizi yönetme görevleri otomatikleştirmek için kullanabileceğiniz komutlar öğrenin.
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
ms.openlocfilehash: b7fafa148417ba1667ec0277b414105f95e428ce
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53971794"
---
# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Azure PowerShell kullanarak sanal makinelerinizi yönetme
> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager modelini kullanarak ortak PowerShell komutları için bkz. [burada](../../virtual-machines-windows-ps-common-ref.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Azure PowerShell cmdlet'lerini kullanarak sanal makinelerinizi yönetmek için her gün bunu çok sayıda görev otomatikleştirilebilir. Bu makalede, örnek komutlar daha basit görevler ve bağlantılar daha karmaşık görevler için komutları gösterecek makaleler sağlar.

> [!NOTE]
> Azure PowerShell'i yükleyip henüz henüz makaledeki yönergeleri alabilirsiniz [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).
> 
> 

## <a name="how-to-use-the-example-commands"></a>Örnek komutlar kullanma
Bazı komutlar metinde ortamınız için uygun metin ile değiştirmeniz gerekir. < Ve > Semboller ihtiyacınız değiştirilecek metni belirtebilirsiniz. Metni değiştirdiğinizde simgeleri kaldırır ancak tırnak işaretleri yerinde bırakın.

## <a name="get-a-vm"></a>VM Al
Bu, genellikle kullanacağınız basit bir görevdir. VM hakkında bilgi almak, bir VM üzerinde görevleri veya bir değişkende depolamak için bir çıkış almanız için kullanın.

VM hakkında bilgi almak için tırnak işaretleri dahil olmak üzere, her şeyi değiştirerek bu komutu çalıştırın. < ve > karakterleri:

    Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Çıkış $vm değişkeninde depolar için şunu çalıştırın:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Windows tabanlı bir VM'de oturum
Şu komutları çalıştırın:

> [!NOTE]
> Sanal makine ve bulut hizmeti adı görünümünden alabilirsiniz **Get-AzureVM** komutu.
> 
> $svcName = `"<cloud service name>"` $vmName = `"<virtual machine name>"` $localPath = `"<drive and folder location to store the downloaded RDP file, example: c:\temp >"` $localFile $localPath = + "\" $vmname +".rdp"get-AzureRemoteDesktopFile - ServiceName $svcName-$vmName - LocalPath $localFile ad-Başlat
> 
> 

## <a name="stop-a-vm"></a>VM durdurma
Şu komutu çalıştırın:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

> [!IMPORTANT]
> Bu bulut hizmeti içindeki son VM olması durumunda sanal IP (VIP) bulut hizmetinin tutmak için bu parametreyi kullanın. <br><br> StayProvisioned parametresini kullanırsanız, yine de sanal makine için faturalandırılırsınız.
> 
> 

## <a name="start-a-vm"></a>VM başlatma
Şu komutu çalıştırın:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Veri diski ekleme
Bu görev, birkaç adım gerektirir. İlk olarak, kullandığınız Ekle-$vm nesnesine disk eklemek için ****AzureDataDisk**** cmdlet'i. Ardından, kullandığınız **güncelleştirme-AzureVM** sanal makinenin yapılandırmasını güncelleştirmek için cmdlet.

Ayrıca yeni bir disk veya veri içeren bir disk eklemeye karar vermeniz gerekir. Yeni bir disk için komut .vhd dosyasını oluşturur ve bunu ekler.

Yeni bir disk eklemek için şu komutu çalıştırın:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Var olan bir veri diski eklemek için şu komutu çalıştırın:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Blob depolama alanındaki bir .vhd dosyasından veri diskleri eklemek için şu komutu çalıştırın:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Windows tabanlı bir VM oluşturma
Azure'da yeni bir Windows tabanlı sanal makine oluşturmak için yönergeleri kullanın. [oluşturma ve önceden yapılandırma Windows tabanlı sanal makineler için Azure PowerShell](create-powershell.md). Bu konuda adımları Ayarla komutu, bir Azure PowerShell oluşturulmasını size önceden yapılandırılabilir Windows tabanlı bir VM oluşturur:

* Active Directory etki alanı üyeliği ile.
* Ek diskler ile.
* Mevcut bir yük dengeli bir üyesi olarak ayarlayın.
* Bir statik IP adresi ile.

