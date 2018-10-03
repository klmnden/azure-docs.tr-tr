---
title: Azure CLI örnekleri | Microsoft Docs
description: Azure CLI Örnekleri
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: ca9823a76064e504ee04bf5896f1362b5187bc34
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48042045"
---
# <a name="azure-cli-samples-for-linux-virtual-machines"></a>Linux sanal makineleri için Azure CLI örnekleri

Aşağıdaki tablo, Azure CLI’si kullanılarak oluşturulan bash komut dosyalarına yönelik bağlantılar içerir.

| | |
|---|---|
|**Sanal makineler oluşturma**||
| [Sanal makine oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | En düşük yapılandırmayla bir Linux sanal makinesi oluşturur. |
| [Tam olarak yapılandırılmış bir sanal makine oluşturun](./../scripts/virtual-machines-linux-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturur.|
| [Yüksek oranda kullanılabilir sanal makineler oluşturun](./../scripts/virtual-machines-linux-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Birden çok yüksek oranda kullanılabilir sanal makineler ve yük dengeli küme yapılandırması oluşturur. |
| [VM oluşturma ve yapılandırma betiğini çalıştırın](./../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json) | Bir sanal makine oluşturur ve NGINX yüklemek için Azure özel betik uzantısı'nı kullanır. |
| [Yüklü WordPress ile VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-wordpress.md?toc=%2fcli%2fazure%2ftoc.json) | Bir sanal makine oluşturur ve Azure özel betik uzantısı WordPress yükler. |
| [Bir yönetilen işletim sistemi diskinden VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json) | Mevcut bir yönetilen diski işletim sistemi diski olarak ekleyerek sanal makine oluşturur. |
| [Anlık görüntüden VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | İlk anlık görüntüden yönetilen disk oluşturma ve sonra yeni yönetilen diski işletim sistemi diski olarak ekleyerek bir anlık görüntüden sanal makine oluşturur. |
|**Depolamayı yönetme**||
| [Bir VHD'den yönetilen disk oluşturma](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json) | Özel bir VHD’den işletim sistemi diski olarak veya bir veri VHD’sinden veri diski olarak yönetilen bir disk oluşturur.  |
| [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Anlık görüntüden yönetilen disk oluşturur. |
| [Yönetilen disk aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopya, yönetilen disk aynı veya farklı aboneliğe ancak üst ile aynı bölgede yönetilen disk. 
| [Anlık görüntü, bir depolama hesabına VHD dışarı aktarma](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fcli%2fmodule%2ftoc.json) | Yönetilen bir anlık görüntü, farklı bölgedeki bir depolama hesabına VHD dışarı aktarır. |
| [Yönetilen disk VHD'si bir depolama hesabına aktarın](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-vhd.md?toc=%2fcli%2fmodule%2ftoc.json) | Yönetilen disk, temel alınan VHD farklı bölgedeki bir depolama hesabına dışarı aktarır. |
| [Anlık görüntüsünü aynı veya farklı aboneliğe kopyalama](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopya anlık görüntüsünü aynı veya farklı aboneliğe üst anlık görüntü ile aynı bölgede ancak. |
|**Ağ sanal makineler**||
| [Sanal makineler arasındaki ağ trafiğinin güvenliğini sağlama](./../scripts/virtual-machines-linux-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | İki sanal makine ve tüm ilgili kaynakları bir iç ve dış ağ güvenlik grupları (NSG) oluşturur. |
|**Sanal makineler güvenli**||
| [VM ve veri diskleri şifreleme](./../scripts/virtual-machines-linux-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Bir Azure Key Vault, şifreleme anahtarı ve hizmet sorumlusu oluşturur ve ardından VM şifreler. |
|**Sanal makineleri izleme**||
| [Operations Management Suite ile bir VM izleme](./../scripts/virtual-machines-linux-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Bir sanal makine oluşturur Operations Management Suite aracısını yükler ve VM'yi bir OMS çalışma alanına kaydeder.  |
|**Sanal makine sorunlarını giderme**||
| [VM işletim sistemi diski sorun giderme](./../scripts/virtual-machines-linux-cli-sample-mount-os-disk.md?toc=%2fcli%2fazure%2ftoc.json) | İşletim sistemi diski, ikinci bir sanal makine üzerinde bir VM'den veri diski olarak bağlar. |
| | |
