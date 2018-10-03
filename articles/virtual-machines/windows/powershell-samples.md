---
title: Azure sanal makine PowerShell örnekleri | Microsoft Docs
description: Azure sanal makine PowerShell örnekleri
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/30/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 564e8e2968f31b9e2aef8f7eadbf848885037ccb
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48040771"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Azure sanal makine PowerShell örnekleri

Aşağıdaki tabloda, Windows sanal makineler oluşturabilir ve yönetebilirsiniz, PowerShell Betiği örneklerinin bağlantılarını içerir.

| | |
|---|---|
|**Sanal makineler oluşturma**||
| [Hızla bir sanal makine oluşturun](./../scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Komut istemlerini en düşük ile bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturur.|
| [Tam olarak yapılandırılmış bir sanal makine oluşturun](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturur.|
| [Yüksek oranda kullanılabilir sanal makineler oluşturun](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Birden çok yüksek oranda kullanılabilir sanal makineler ve yük dengeli küme yapılandırması oluşturur.|
| [VM oluşturma ve yapılandırma betiğini çalıştırın](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure özel betik uzantısı'nı kullanır. |
| [Bir VM oluşturun ve çalışma DSC yapılandırması](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure Desired State Configuration (DSC) uzantısı kullanır. |
| [Bir VHD'yi karşıya yükleme ve VM oluşturma](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | Yerel bir VHD dosyasını Azure'a yükler, oluşturur ve bir VHD'den görüntü ve daha sonra bu görüntüden bir VM oluşturur. |
| [Bir yönetilen işletim sistemi diskinden VM oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Mevcut bir yönetilen diski işletim sistemi diski olarak ekleyerek sanal makine oluşturur. |
| [Anlık görüntüden VM oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | İlk anlık görüntüden yönetilen disk oluşturma ve sonra yeni yönetilen diski işletim sistemi diski olarak ekleyerek bir anlık görüntüden sanal makine oluşturur. |
|**Depolamayı yönetme**||
| [Aynı veya farklı Abonelikteki bir VHD'den yönetilen disk oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir veri VHD'SİNDEN veri diski aynı veya farklı Abonelikteki olarak ya da bir işletim sistemi diski olarak özelleştirilmiş bir VHD'den yönetilen disk oluşturur.  |
| [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Anlık görüntüden yönetilen disk oluşturur. |
| [Yönetilen disk aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopya, yönetilen disk aynı veya farklı aboneliğe ancak üst ile aynı bölgede yönetilen disk. 
| [Anlık görüntü, bir depolama hesabına VHD dışarı aktarma](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Yönetilen bir anlık görüntü, farklı bölgedeki bir depolama hesabına VHD dışarı aktarır. |
| [Yönetilen disk VHD'si bir depolama hesabına aktarın](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Yönetilen disk, temel alınan VHD farklı bölgedeki bir depolama hesabına dışarı aktarır. |
| [Bir VHD'den anlık görüntü oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Birden fazla aynı yönetilen disk anlık görüntüden kısa bir sürede oluşturmak için VHD'den anlık görüntü oluşturur.  |
| [Anlık görüntüsünü aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Kopya anlık görüntüsünü aynı veya farklı aboneliğe üst anlık görüntü ile aynı bölgede ancak. |
|**Sanal makineler güvenli**||
| [VM ve veri diskleri şifreleme](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | Bir Azure Key Vault, şifreleme anahtarı ve hizmet sorumlusu oluşturur ve ardından VM şifreler. |
|**Sanal makineleri izleme**||
| [Operations Management Suite ile bir VM izleme](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir sanal makine oluşturur Operations Management Suite aracısını yükler ve VM'yi bir OMS çalışma alanına kaydeder.  |
| | |

