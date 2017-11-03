---
title: "Azure CLI komut dosyası örneği - bağlama işletim sistemi diski | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - bağlama işletim sistemi diski"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c32ea5e6cade34a9c8dac0eab523ebcaa10ef039
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>Sanal makineleri işletim sistemi disk sorunlarını gider

Bu komut dosyası başarısız veya sorunlu bir sanal makinenin işletim sistemi diski ikinci bir sanal makine veri diski olarak bağlar. Sorun giderme disk sorunları ya da veri kurtarma bu kullanışlı olabilir. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, sanal makine ve tüm ilgili kaynaklar oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az vm Göster](https://docs.microsoft.com/cli/azure/vm#az_vm_show) | Sanal makinelerin listesini döndürür. Bu durumda, sorgu seçeneği, sanal makine işletim sistemi diski döndürmek için kullanılır. Bu değer daha sonra bir değişken adı 'uri' eklenir. |
| [az vm silme](https://docs.microsoft.com/cli/azure/vm#az_vm_delete) | Bir sanal makineyi siler. |
| [az vm oluşturma](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Bir sanal makine oluşturur.  |
| [az vm diski kullanıma açın](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) | Bir disk bir sanal makineye iliştirir. |
| [az vm-IP-adresleri listesi](https://docs.microsoft.com/cli/azure/vm#az_vm_list_ip_addresses) | Bir sanal makine IP adreslerini döndürür. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine CLI kod örnekleri bulunabilir [Azure Linux VM'de belgelerine](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
