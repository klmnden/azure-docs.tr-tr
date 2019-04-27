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
ms.date: 03/01/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 576fe268bec12c16c7c2e2076dfa066c908693d5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60583698"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Azure sanal makine PowerShell örnekleri

Aşağıdaki tabloda, PowerShell Betiği örnekleri, oluşturmak ve Windows sanal makineleri (VM'ler) yönetmek için bağlantılar sağlar.

| | |
|---|---|
|**Sanal makineler oluşturma**||
| [Hızla bir sanal makine oluşturun](./../scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Komut istemlerini ile en az bir kaynak grubu, bir sanal makine ve tüm ilgili kaynakları oluşturur.|
| [Tam olarak yapılandırılmış bir sanal makine oluşturun](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir kaynak grubu, bir sanal makine ve tüm ilgili kaynakları oluşturur.|
| [Yüksek oranda kullanılabilir sanal makineler oluşturun](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yüksek oranda kullanılabilir ve yük dengeli bir yapılandırmada birden fazla sanal makine oluşturur.|
| [VM oluşturma ve yapılandırma betiğini çalıştırın](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure özel betik uzantısı'nı kullanır. |
| [VM oluşturma ve bir DSC Yapılandırması'nı çalıştırın](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure Desired State Configuration (DSC) uzantısı kullanır. |
| [Bir VHD'yi karşıya yükleme ve VM oluşturma](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | Yerel bir VHD dosyasını Azure'a yükler, bir VHD'den görüntü oluşturur ve ardından bu görüntüden bir VM oluşturur. |
| [Bir yönetilen işletim sistemi diskinden VM oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Mevcut bir yönetilen diski işletim sistemi diski olarak ekleyerek sanal makine oluşturur. |
| [Anlık görüntüden VM oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | İlk anlık görüntüden yönetilen disk oluşturma ve sonra yeni yönetilen diski işletim sistemi diski olarak ekleyerek bir anlık görüntüden sanal makine oluşturur. |
|**Depolamayı yönetme**||
| [Aynı veya farklı bir abonelikte bir VHD'den yönetilen disk oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir veri VHD'SİNDEN veri diski, aynı olarak veya bir işletim sistemi diski olarak özelleştirilmiş bir VHD'den yönetilen disk veya farklı bir abonelik oluşturur.  |
| [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Anlık görüntüden yönetilen disk oluşturur. |
| [Yönetilen disk aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Aynı veya farklı bir aboneliğe üst yönetilen diskin aynı bölgede olan bir yönetilen diski kopyalar.
| [Bir depolama hesabına VHD olarak anlık görüntü dışarı aktarma](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Farklı bir bölgede bir depolama hesabına VHD olarak yönetilen bir anlık görüntüsü aktarır. |
| [Yönetilen disk VHD'si bir depolama hesabına aktarın](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Yönetilen disk, temel alınan VHD farklı bir bölgede bir depolama hesabına dışarı aktarır. |
| [Bir VHD'den anlık görüntü oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir VHD'den anlık görüntüsünü oluşturur ve ardından hızlı bir şekilde birden fazla aynı yönetilen disk oluşturmak için bu anlık görüntüyü kullanır.  |
| [Bir anlık görüntüsünü aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Aynı veya farklı bir aboneliğe üst anlık görüntü ile aynı bölgede olan anlık görüntü kopyalar. |
|**Sanal makineler güvenli**||
| [Bir VM ile veri disklerini şifreler](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | Bir Azure key vault, şifreleme anahtarı ve bir hizmet sorumlusu oluşturur ve ardından VM şifreler. |
|**Sanal makineleri izleme**||
| [Azure İzleyici ile bir VM izleme](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Bir sanal makine oluşturur, Azure Log Analytics aracısını yükler ve Log Analytics çalışma alanındaki VM kaydeder.  |
| | |
