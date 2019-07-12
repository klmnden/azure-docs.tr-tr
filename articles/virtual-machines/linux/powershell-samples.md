---
title: Azure sanal makine PowerShell örnekleri | Microsoft Docs
description: Azure sanal makine PowerShell örnekleri
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2019
ms.author: cynthn
ms.openlocfilehash: 2d954bc068693a34ef1d69e4296e972979d4f61b
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671024"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Azure sanal makine PowerShell örnekleri

Aşağıdaki tabloda, Linux sanal makineler oluşturabilir ve yönetebilirsiniz, PowerShell Betiği örneklerinin bağlantılarını içerir.

| | |
|---|---|
|**Sanal makineler oluşturma**||
| [Tam olarak yapılandırılmış bir sanal makine oluşturun](./../scripts/virtual-machines-linux-powershell-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturur.|
| [Etkin Docker ile VM oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-docker-host.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir sanal makine oluşturur, bu VM'yi bir Docker konağı olarak yapılandırır ve NGINX kapsayıcısı çalıştırır. |
| [VM oluşturma ve yapılandırma betiğini çalıştırın](./../scripts/virtual-machines-linux-powershell-sample-create-vm-nginx.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir sanal makine oluşturur ve NGINX yüklemek için Azure özel betik uzantısı'nı kullanır. |
| [Yüklü WordPress ile VM oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-vm-wordpress.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir sanal makine oluşturur ve Azure özel betik uzantısı WordPress yükler. |
| [Bir yönetilen işletim sistemi diskinden VM oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Mevcut bir yönetilen diski işletim sistemi diski olarak ekleyerek sanal makine oluşturur. |
| [Anlık görüntüden VM oluşturma](./../scripts/virtual-machines-linux-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | İlk anlık görüntüden yönetilen disk oluşturma ve sonra yeni yönetilen diski işletim sistemi diski olarak ekleyerek bir anlık görüntüden sanal makine oluşturur. |
|**Depolamayı yönetme**||
| [Aynı veya farklı bir abonelikte bir VHD'den yönetilen disk oluşturma](../scripts/virtual-machines-linux-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir veri VHD'SİNDEN veri diski, aynı olarak veya bir işletim sistemi diski olarak özelleştirilmiş bir VHD'den yönetilen disk veya farklı bir abonelik oluşturur.  |
| [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-linux-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Anlık görüntüden yönetilen disk oluşturur. |
| [Bir depolama hesabına VHD olarak anlık görüntü dışarı aktarma](../scripts/virtual-machines-linux-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Farklı bir bölgede bir depolama hesabına VHD olarak yönetilen bir anlık görüntüsü aktarır. |
| [Yönetilen disk VHD'si bir depolama hesabına aktarın](../scripts/virtual-machines-linux-powershell-sample-copy-managed-disks-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Yönetilen disk, temel alınan VHD farklı bir bölgede bir depolama hesabına dışarı aktarır. |
| [Bir VHD'den anlık görüntü oluşturma](../scripts/virtual-machines-linux-powershell-sample-create-snapshot-from-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir VHD'den anlık görüntüsünü oluşturur ve ardından hızlı bir şekilde birden fazla aynı yönetilen disk oluşturmak için bu anlık görüntüyü kullanır.  |
| [Bir anlık görüntüsünü aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-linux-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Aynı veya farklı bir aboneliğe üst anlık görüntü ile aynı bölgede olan anlık görüntü kopyalar. |
|**Sanal makineleri izleme**||
| [Azure İzleyici günlükleri ile bir VM izleme](./../scripts/virtual-machines-linux-powershell-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bir sanal makine oluşturur, Log Analytics aracısını yükler ve Log Analytics çalışma alanındaki VM kaydeder.  |
| [Yönetilen disk aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-linux-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Aynı veya farklı bir aboneliğe üst yönetilen diskin aynı bölgede olan bir yönetilen diski kopyalar.
| [PowerShell ile bir Abonelikteki tüm VM'ler hakkında bilgi toplayın](../scripts/virtual-machines-powershell-sample-collect-vm-details.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | VM adı, kaynak grubu adı, bölge, sanal ağ, alt ağ, özel IP adresi, işletim sistemi türü ve belirtilen Abonelikteki sanal makinelerin genel IP adresi içeren bir csv oluşturur.
| | |
