---
title: "Azure PowerShell Betiği örnek - farklı bir bölgeye depolama hesabında VHD olarak verme/kopyalama anlık görüntü | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - bir depolama hesabı aynı farklı bölgede VHD olarak verme/kopyalama anlık görüntü"
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
ms.openlocfilehash: be21a891121df1d645b430d87b572cde6c945d61
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-powershell"></a>Dışa aktarma/yönetilen anlık görüntüler VHD farklı bir bölgeye PowerShell ile depolama hesabında kopyalama

Bu komut dosyasını farklı bir bölgeye depolama hesabında yönetilen bir anlık görüntü verir. İlk anlık görüntü SAS URI'sini oluşturur ve farklı bölgede bulunan bir depolama hesabına kopyalamak için kullanır. Olağanüstü durum kurtarma için farklı bölgede yönetilen disklerinizi yedekleme korumak için bu komut dosyasını kullanın.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "Copy snapshot")]


## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası SAS URI'si için yönetilen bir anlık görüntü oluşturmak için komutları kullanır ve anlık görüntü SAS URI'sini kullanarak bir depolama hesabı kopyalar. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [GRANT-AzureRmSnapshotAccess](/powershell/module/azurerm.compute/New-AzureRmDisk) | Bir depolama hesabına kopyalamak için kullanılan bir anlık görüntü için SAS URI'sini oluşturur. |
| [AzureStorageContext yeni](/powershell/module/azure.storage/New-AzureStorageContext) | Hesap adı ve anahtarı kullanarak bir depolama hesabı bağlamını oluşturur. Bu bağlam, depolama hesabı üzerinde okuma/yazma işlemleri gerçekleştirmek için kullanılabilir. |
| [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | Bir depolama hesabı, bir anlık görüntüsü temel VHD kopyalar. |

## <a name="next-steps"></a>Sonraki adımlar

[Bir VHD'den yönetilen bir disk oluştur](virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[Yönetilen bir diskten bir sanal makine oluşturun](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek sanal makine PowerShell komut dosyası örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).