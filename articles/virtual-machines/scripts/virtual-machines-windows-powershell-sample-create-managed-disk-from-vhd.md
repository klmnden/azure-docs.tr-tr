---
title: "Azure PowerShell Betiği örnek - yönetilen bir disk bir depolama hesabı aynı veya farklı Abonelikteki bir VHD dosyasında oluşturun | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - aynı veya farklı Abonelikteki depolama hesabındaki bir VHD dosyasından yönetilen bir disk oluşturma"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 6d37fb1308ce0b866b42f961ada84d0869f25615
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a>PowerShell ile aynı veya farklı Abonelikteki depolama hesabındaki bir VHD dosyasından yönetilen bir disk oluşturma

Bu komut dosyası, aynı veya farklı Abonelikteki depolama hesabındaki bir VHD dosyasından yönetilen bir disk oluşturur. Özelleştirilmiş içeri aktarmak için bu betiği kullanın (değil genelleştirilmiş/Sysprep kullanılarak hazırlanmış) bir sanal makine oluşturmak için yönetilen işletim sistemi diski VHD'ye. Ayrıca, yönetilen veri diski VHD veri almak için kullanın. 

Birden çok özdeş yönetilen diskler bir VHD dosyasından kısa sürede oluşturmayın. Bir vhd dosyasından yönetilen diskler oluşturmak için vhd dosyasının blob anlık görüntü oluşturulur ve ardından yönetilen diskler oluşturmak için kullanılır. Yalnızca bir blob anlık görüntü kısıtlama nedeniyle disk oluşturma hataları neden olan bir dakika içinde oluşturulabilir. Bu azaltma önlemek için oluşturun bir [yönetilen anlık görüntüden vhd dosyasını](virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) ve birden çok oluşturmak için yönetilen anlık görüntü yönetilen kullanım kısa süre miktarı diskler. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Yüklemek ve PowerShell yerel olarak kullanmak seçerseniz, bu öğreticide Azure PowerShell modülü sürüm 4.0 veya üzeri olmasını gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Gerekirse yükleyin veya yükseltin, bakın [Azure PowerShell yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]


## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası, farklı Abonelikteki bir VHD'den yönetilen bir disk oluşturmak için komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [AzureRmDiskConfig yeni](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Disk oluşturmak için kullanılan disk yapılandırması oluşturur. Depolama türü, konum, kaynak üst VHD depolandığı depolama hesabını VHD üst VHD URİ'si kimliğini içerir. |
| [AzureRmDisk yeni](/powershell/module/azurerm.compute/New-AzureRmDisk) | Disk yapılandırması, disk adı ve parametre olarak geçirilen kaynak grubu adı kullanarak bir disk oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

[Yönetilen bir disk işletim sistemi diski olarak ekleyerek bir sanal makine oluşturun](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek sanal makine PowerShell komut dosyası örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).