---
title: "Azure sanal makinesi PowerShell örnekleri | Microsoft Docs"
description: "Azure sanal makinesi PowerShell örnekleri"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/20/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 042b2b4a5c4ccb573b0d1d13abe7855aea779348
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="azure-virtual-machine-powershell-samples"></a>Azure sanal makine PowerShell örnekleri

Aşağıdaki tabloda, Windows sanal makineler oluşturun ve yönetin PowerShell komut dosyaları örnekleri bağlantılarını içerir.

| | |
|---|---|
|**Sanal makineler oluşturma**||
| [Hızla bir sanal makine oluşturulur](./../scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir kaynak grubu, sanal makineyi ve ilgili tüm kaynakları istemleri az ile oluşturur.|
| [Tam olarak yapılandırılmış bir sanal makine oluşturun](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir kaynak grubu, sanal makine ve tüm ilgili kaynaklar oluşturur.|
| [Yüksek oranda kullanılabilir sanal makineler oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Birkaç yüksek oranda kullanılabilir sanal makineleri ve yükü dengelenmiş yapılandırma oluşturur.|
| [Bir VM oluşturma ve yapılandırma komut dosyası çalıştırma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure özel betik uzantısı kullanır. |
| [Bir VM oluşturun ve DSC yapılandırması çalıştırın](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure istenen durum yapılandırması (DSC) uzantısını kullanır. |
| [Bir VHD yüklemek ve VM'ler oluşturun](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | Yerel bir VHD dosyasını Azure'a yükler, oluşturur ve görüntü VHD'den ve daha sonra bu görüntüden VM oluşturur. |
| [Yönetilen bir işletim sistemi disk, bir VM oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Varolan bir yönetilen Disk işletim sistemi diski olarak ekleyerek bir sanal makine oluşturur. |
| [Bir anlık görüntüden bir VM oluşturma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir sanal makine bir anlık görüntüden ilk yönetilen disk anlık görüntüden oluşturarak ve ardından yeni yönetilen işletim sistemi diski olarak diskindeki oluşturur. |
|**Yeni AzVM kullanarak sanal makine oluşturma**||
| [Tam olarak yapılandırılmış bir sanal makine oluşturun](./../scripts/virtual-machines-windows-powershell-sample-create-vm-auto.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir kaynak grubu, sanal makine ve tüm ilgili kaynaklar oluşturur.|
| [Bir VM oluşturma ve yapılandırma komut dosyası çalıştırma](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis-auto.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure özel betik uzantısı kullanır. |
| [Bir VM oluşturun ve DSC yapılandırması çalıştırın](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc-auto.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir sanal makine oluşturur ve IIS yüklemek için Azure istenen durum yapılandırması (DSC) uzantısını kullanır. |
|**Depolama yönetin**||
| [Aynı veya farklı Abonelikteki bir VHD'den yönetilen disketi oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Yönetilen bir disk, işletim sistemi diski olarak özel bir VHD veya VHD aynı veya farklı Abonelikteki veri diski olarak veri oluşturur.  |
| [Bir anlık görüntüden yönetilen bir disk oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Yönetilen bir disk anlık görüntüden oluşturur. |
| [Aynı veya farklı bir abonelik yönetilen diske kopyalama](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopya disk aynı veya farklı bir abonelik için yönetilen ancak üst ile aynı bölgede disk yönetilen. 
| [Bir anlık görüntü depolama hesabına VHD dışarı aktarma](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Yönetilen bir anlık görüntü VHD farklı bir bölgeye depolama hesabında dışa aktarır. |
| [Bir VHD'den bir anlık görüntü oluştur](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Birden çok özdeş yönetilen diskler anlık görüntüden kısa sürede oluşturmak için bir VHD'den anlık görüntü oluşturur.  |
| [Aynı veya farklı abonelik için anlık görüntü kopyalama](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Aynı veya farklı bir abonelik ancak üst anlık görüntü ile aynı bölgede anlık kopyalar. |
|**Sanal makineler güvenli**||
| [VM ve veri diskleri şifrele](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | Azure anahtar kasası, şifreleme anahtarı ve hizmet sorumlusu oluşturur ve ardından VM şifreler. |
|**Sanal makineleri izleme**||
| [Bir VM Operations Management Suite ile izleme](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Bir sanal makine oluşturur, Operations Management Suite aracısını yükler ve bir OMS çalışma alanında VM kaydeder.  |
| | |

