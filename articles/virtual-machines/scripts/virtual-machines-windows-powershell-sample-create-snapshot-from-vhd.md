---
title: "Azure PowerShell Betiği örnek - kısa sürede birden çok özdeş yönetilen diskler oluşturmak için bir VHD'den bir anlık görüntü oluşturma | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - kısa sürede birden çok özdeş yönetilen diskler oluşturmak için bir VHD'den bir anlık görüntü oluşturma"
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
ms.openlocfilehash: 4cd6d9cc4f2b1fa41530349c957e180e2513586e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>Kısa sürede PowerShell ile birden çok özdeş yönetilen diskler oluşturmak için bir VHD'den bir anlık görüntü oluşturma

Bu komut dosyası, aynı veya farklı Abonelikteki depolama hesabındaki bir VHD dosyasından bir anlık görüntüsü oluşturur. Özelleştirilmiş içeri aktarmak için bu betiği kullanın (değil genelleştirilmiş/Sysprep kullanılarak hazırlanmış) birden çok özdeş oluşturmak için bir anlık görüntü ve ardından anlık görüntü için VHD diskleri kısa sürede yönetilen. Ayrıca, verileri VHD bir anlık görüntü alın ve ardından anlık görüntü kısa sürede birden çok yönetilen diskler oluşturmak için kullanın. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Yüklemek ve PowerShell yerel olarak kullanmak seçerseniz, bu öğreticide Azure PowerShell modülü sürüm 4.0 veya üzeri olmasını gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Gerekirse yükleyin veya yükseltin, bakın [Azure PowerShell yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]


## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası, farklı Abonelikteki bir VHD'den yönetilen bir disk oluşturmak için komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [AzureRmDiskConfig yeni](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Disk oluşturmak için kullanılan disk yapılandırması oluşturur. Depolama türü, konum, kaynak kimliği olan üst VHD depolandığı depolama hesabını ve VHD üst VHD URİ'si içerir. |
| [AzureRmDisk yeni](/powershell/module/azurerm.compute/New-AzureRmDisk) | Disk yapılandırması, disk adı ve parametre olarak geçirilen kaynak grubu adı kullanarak bir disk oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

[Yönetilen bir disk anlık görüntüden oluşturma](virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Yönetilen bir disk işletim sistemi diski olarak ekleyerek bir sanal makine oluşturun](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek sanal makine PowerShell komut dosyası örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).