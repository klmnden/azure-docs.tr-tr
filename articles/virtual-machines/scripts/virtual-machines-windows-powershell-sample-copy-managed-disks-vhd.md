---
title: Azure PowerShell betik örneği - Yönetilen diskin VHD dosyasını farklı bölgedeki bir depolama hesabına aktarma/kopyalama | Microsoft Docs
description: Azure PowerShell betik örneği - Yönetilen diskin VHD dosyasını aynı veya farklı bölgedeki bir depolama hesabına aktarma/kopyalama
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/17/2018
ms.author: ramankum
ms.openlocfilehash: 1bb116f2a2153515f3b61c050f0c952523c13528
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55976637"
---
# <a name="exportcopy-the-vhd-of-a-managed-disk-to-a-storage-account-in-different-region-with-powershell"></a>PowerShell ile yönetilen diskin VHD dosyasını farklı bölgedeki bir depolama hesabına aktarma/kopyalama

Bu betik yönetilen diskin VHD dosyasını farklı bölgede bulunan bir depolama hesabına aktarır. İlk olarak yönetilen diskin SAS URI'sini oluşturur ve sonra onu VHD dosyasını farklı bölgedeki bir depolama hesabına kopyalamak için kullanır. Bu betiği bölgesel genişleme için yönetilen diskleri farklı bölgelere kopyalama amacıyla kullanabilirsiniz.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-managed-disks-vhd-to-storage-account/copy-managed-disks-vhd-to-storage-account.ps1 "Copy the VHD of a managed disk")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir yönetilen diskin SAS URI'sini oluşturmak için aşağıdaki komutları kullanır ve SAS URI'sini kullanarak VHD dosyasını bir depolama hesabına kopyalar. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Grant-AzDiskAccess](https://docs.microsoft.com/powershell/module/az.compute/grant-azdiskaccess) | VHD dosyasını bir depolama hesabına kopyalamak için kullanılan yönetilen disk SAS URI değerini oluşturur. |
| [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/New-AzureStorageContext) | Hesap adını ve anahtarını kullanarak depolama hesabı bağlamı oluşturur. Bu bağlam depolama hesabında okuma/yazma işlemi gerçekleştirmek için kullanılabilir. |
| [Start-AzureStorageBlobCopy](https://docs.microsoft.com/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | Bir anlık görüntüde kullanılan VHD dosyasını bir depolama hesabına kopyalar |

## <a name="next-steps"></a>Sonraki adımlar

[VHD'den yönetilen disk oluşturma](virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[Yönetilen diskten sanal makine oluşturma](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.