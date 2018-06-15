---
title: Bir Linux VM Azure CLI 1.0 ile yeniden boyutlandırmak nasıl | Microsoft Docs
description: Yukarı veya Linux sanal makine, VM boyutunu değiştirerek ölçeklendirmek nasıl.
services: virtual-machines-linux
documentationcenter: na
author: mikewasson
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2016
ms.author: mwasson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4407a3662e6a50d650ab4ecc6dd1c2dad0c53bdb
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32186122"
---
# <a name="resize-a-linux-vm-with-azure-cli-10"></a>Bir Linux VM Azure CLI 1.0 ile yeniden boyutlandırın

## <a name="overview"></a>Genel Bakış

Bir sanal makine (VM) sağlama sonra VM yukarı veya aşağı değiştirerek ölçeklendirmek [VM boyutu][vm-sizes]. Bazı durumlarda, ilk VM ayırması gerekir. Yeni boyutu VM'i barındırma donanım kümede kullanılabilir değilse, bu durum oluşabilir.

Bu makalede kullanarak bir Linux VM yeniden boyutlandırmak gösterilmiştir [Azure CLI][azure-cli].

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#resize-a-linux-vm) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](change-vm-size.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız


## <a name="resize-a-linux-vm"></a>Bir Linux VM yeniden boyutlandırma
Bir VM yeniden boyutlandırmak için aşağıdaki adımları gerçekleştirin.

1. Aşağıdaki CLI komutunu çalıştırın. Bu komut, VM'nin barındırıldığı donanım kümede kullanılabilir VM boyutları listeler.
   
    ```azurecli
    azure vm sizes -g myResourceGroup --vm-name myVM
    ```
2. İstenen boyut listeleniyorsa, VM yeniden boyutlandırmak için aşağıdaki komutu çalıştırın.
   
    ```azurecli
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM  \
        --enable-boot-diagnostics
        --boot-diagnostics-storage-uri https://mystorageaccount.blob.core.windows.net/ 
    ```
   
    VM, bu işlem sırasında yeniden başlatılır. Yeniden başladıktan sonra var olan işletim sistemi ve veri diskleri yeniden eşlenirken. Geçici diskteki herhangi bir şey kaybolur.
   
    Kullanım `--enable-boot-diagnostics` seçeneği belirlendiğinde [önyükleme tanılama][boot-diagnostics], başlatma için ilgili hataları günlüğe kaydetmek için.
3. Aksi takdirde, istenen boyut, VM serbest bırakma için aşağıdaki komutları çalıştırın listelenmemişse yeniden boyutlandırabilir ve VM yeniden başlatın.
   
    ```azurecli
    azure vm deallocate -g myResourceGroup myVM
    azure vm set -g myResourceGroup --vm-size <new-vm-size> -n myVM \
        --enable-boot-diagnostics --boot-diagnostics-storage-uri \
        https://mystorageaccount.blob.core.windows.net/ 
    azure vm start -g myResourceGroup myVM
    ```
   
   > [!WARNING]
   > VM serbest bırakma ayrıca VM'ye atanan dinamik IP adreslerini serbest bırakır. İşletim sistemi ve veri diskleri etkilenmez.
   > 
   > 

## <a name="next-steps"></a>Sonraki adımlar
Ek ölçeklenebilirlik için birden çok VM örnekleri çalıştırın ve ölçeğini. Daha fazla bilgi için bkz: [otomatik olarak bir sanal makine ölçek kümesindeki Linux makineler ölçekleme][scale-set]. 

<!-- links -->

[azure-cli]:../../cli-install-nodejs.md
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
