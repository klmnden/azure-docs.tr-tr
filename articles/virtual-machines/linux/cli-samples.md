---
title: "Azure CLI örnekleri | Microsoft Docs"
description: "Azure CLI örnekleri"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/08/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: ec76e211062890b4325b5c2cb1cf186ac7b89c55
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="azure-cli-samples-for-linux-virtual-machines"></a>Linux sanal makineleri için Azure CLI örnekleri

Aşağıdaki tablo, Azure CLI’si kullanılarak oluşturulan bash komut dosyalarına yönelik bağlantılar içerir.

| | |
|---|---|
|**Sanal makineler oluşturma**||
| [Bir sanal makine oluşturun](./../scripts/virtual-machines-linux-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | Linux sanal makinesi en az yapılandırma ile oluşturur. |
| [Tam olarak yapılandırılmış bir sanal makine oluşturun](./../scripts/virtual-machines-linux-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Bir kaynak grubu, sanal makine ve tüm ilgili kaynaklar oluşturur.|
| [Yüksek oranda kullanılabilir sanal makineler oluşturma](./../scripts/virtual-machines-linux-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Birkaç yüksek oranda kullanılabilir sanal makineleri ve yükü dengelenmiş yapılandırma oluşturur. |
| [Docker etkin bir VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-docker-host.md?toc=%2fcli%2fazure%2ftoc.json) | Bir sanal makine oluşturur, bu VM Docker ana bilgisayar olarak yapılandırır ve bir NGINX kapsayıcısı çalıştırır. |
| [Bir VM oluşturma ve yapılandırma komut dosyası çalıştırma](./../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json) | Bir sanal makine oluşturur ve NGINX yüklemek için Azure özel betik uzantısı kullanır. |
| [WordPress yüklü bir VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-wordpress.md?toc=%2fcli%2fazure%2ftoc.json) | Bir sanal makine oluşturur ve WordPress yüklemek için Azure özel betik uzantısı kullanır. |
| [Yönetilen bir işletim sistemi disk, bir VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json) | Varolan bir yönetilen Disk işletim sistemi diski olarak ekleyerek bir sanal makine oluşturur. |
| [Bir anlık görüntüden bir VM oluşturma](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Bir sanal makine bir anlık görüntüden ilk yönetilen disk anlık görüntüden oluşturarak ve ardından yeni yönetilen işletim sistemi diski olarak diskindeki oluşturur. |
|**Depolama yönetin**||
| [Bir VHD'den yönetilen disk oluştur](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json) | Yönetilen bir disk, işletim sistemi diski olarak özel bir VHD veya VHD veri diski olarak veri oluşturur.  |
| [Bir anlık görüntüden yönetilen bir disk oluşturma](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Yönetilen bir disk anlık görüntüden oluşturur. |
| [Aynı veya farklı bir abonelik yönetilen diske kopyalama](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopya disk aynı veya farklı bir abonelik için yönetilen ancak üst ile aynı bölgede disk yönetilen. 
| [Bir anlık görüntü depolama hesabına VHD dışarı aktarma](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fcli%2fmodule%2ftoc.json) | Yönetilen bir anlık görüntü VHD farklı bir bölgeye depolama hesabında dışa aktarır. |
| [Aynı veya farklı abonelik için anlık görüntü kopyalama](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Aynı veya farklı bir abonelik ancak üst anlık görüntü ile aynı bölgede anlık kopyalar. |
|**Ağ sanal makineler**||
| [Sanal makineler arasındaki ağ trafiğini güvenli](./../scripts/virtual-machines-linux-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | İki sanal makine, tüm ilgili kaynaklar ve bir iç ve dış ağ güvenlik grupları (NSG) oluşturur. |
|**Sanal makineler güvenli**||
| [VM ve veri diskleri şifrele](./../scripts/virtual-machines-linux-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Azure anahtar kasası, şifreleme anahtarı ve hizmet sorumlusu oluşturur ve ardından VM şifreler. |
|**Sanal makineleri izleme**||
| [Bir VM Operations Management Suite ile izleme](./../scripts/virtual-machines-linux-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Bir sanal makine oluşturur, Operations Management Suite aracısını yükler ve bir OMS çalışma alanında VM kaydeder.  |
|**Sanal makineler sorun giderme**||
| [Sanal makineleri işletim sistemi disk sorunlarını gider](./../scripts/virtual-machines-linux-cli-sample-mount-os-disk.md?toc=%2fcli%2fazure%2ftoc.json) | İşletim sistemi diski bir sanal makineden bir veri diski ikinci bir VM bağlar. |
| | |
