---
title: Windows Azure CLI örnekleri | Microsoft Docs
description: Windows Azure CLI örnekleri
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
ms.date: 03/01/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 8bc151089f76e3f27ababb479f5e893ca9a99365
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60844066"
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a>Azure CLI örnekleri Windows'ı için sanal makineleri

Aşağıdaki tabloda, Windows sanal makineleri dağıtmak için Azure CLI kullanılarak oluşturulan bash betiklerine yönelik bağlantılar içerir.

| | |
|---|---|
|**Sanal makineler oluşturma**||
| [Sanal makine oluşturun](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | En düşük yapılandırmayla bir Windows sanal makine oluşturur. |
| [Tam olarak yapılandırılmış bir sanal makine oluşturun](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturur.|
| [Yüksek oranda kullanılabilir sanal makineler oluşturun](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Birden çok yüksek oranda kullanılabilir sanal makineler ve yük dengeli küme yapılandırması oluşturur. |
| [VM oluşturma ve yapılandırma betiğini çalıştırın](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure özel betik uzantısı'nı kullanır. |
| [Bir VM oluşturun ve çalışma DSC yapılandırması](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure Desired State Configuration (DSC) uzantısı kullanır. |
|**Depolamayı yönetme**||
| [Bir VHD'den yönetilen disk oluşturma](../scripts/virtual-machines-windows-cli-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir veri VHD'SİNDEN veri diski olarak ya da bir işletim sistemi diski olarak özelleştirilmiş bir VHD'den yönetilen disk oluşturur.  |
| [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-windows-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Anlık görüntüden yönetilen disk oluşturur. |
| [Yönetilen disk aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-windows-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Kopya, yönetilen disk aynı veya farklı aboneliğe ancak üst ile aynı bölgede yönetilen disk. 
| [Anlık görüntü, bir depolama hesabına VHD dışarı aktarma](../scripts/virtual-machines-windows-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yönetilen bir anlık görüntü, farklı bölgedeki bir depolama hesabına VHD dışarı aktarır. |
| [Yönetilen disk VHD'si bir depolama hesabına aktarın](../scripts/virtual-machines-windows-cli-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yönetilen disk, temel alınan VHD farklı bölgedeki bir depolama hesabına dışarı aktarır. |
| [Anlık görüntüsünü aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-windows-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Kopya anlık görüntüsünü aynı veya farklı aboneliğe üst anlık görüntü ile aynı bölgede ancak. |
|**Ağ sanal makineler**||
| [Sanal makineler arasındaki ağ trafiğinin güvenliğini sağlama](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | İki sanal makine ve tüm ilgili kaynakları bir iç ve dış ağ güvenlik grupları (NSG) oluşturur. |
|**Sanal makineler güvenli**||
| [VM ve veri diskleri şifreleme](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir Azure Key Vault, şifreleme anahtarı ve hizmet sorumlusu oluşturur ve ardından VM şifreler. |
|**Sanal makineleri izleme**||
| [Azure İzleyici ile bir VM izleme](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir sanal makine oluşturur, Log Analytics aracısını yükler ve Log Analytics çalışma alanındaki VM kaydeder.  |
| | |
