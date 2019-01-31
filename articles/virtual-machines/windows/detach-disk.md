---
title: Windows VM'den - Azure veri diski çıkarma | Microsoft Docs
description: Azure Resource Manager dağıtım modelini kullanarak bir sanal makineden veri diski çıkarma.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: e27826629873566bf7b746649923c25e6ab70827
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55457165"
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Nasıl bir Windows sanal makinesinden veri diski çıkarma

Sanal makineye bağlı bir veri diskine ihtiyacınız olmadığında bunu kolayca ayırabilirsiniz. Bu diski sanal makineden kaldırır, ancak depolama alanından kaldırmaz.

> [!WARNING]
> Disk ayırma, otomatik olarak silinmez. Premium depolamaya abone olduğunuz, disk için depolama ücretleri uygulanmaya devam edecektir. Daha fazla bilgi için [fiyatlandırma ve Premium depolama kullanırken faturalama](premium-storage.md#pricing-and-billing).
>
>

Disk üzerinde var olan verileri yeniden kullanmak isterseniz bu verileri aynı sanal makineye veya başka birine yeniden ekleyebilirsiniz.


## <a name="detach-a-data-disk-using-powershell"></a>PowerShell kullanarak veri diski çıkarma

Yapabilecekleriniz *sık erişimli* PowerShell kullanarak veri diski kaldırma, ancak hiçbir şey etkin bir şekilde kullanarak diski sanal makineden kullanımdan çıkarmadan önce emin olun.

Bu örnekte biz adlı disk kaldırmak **myDisk** VM'den **myVM** içinde **myResourceGroup** kaynak grubu. İlk disk kullanarak kaldırmanız [Remove-AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk) cmdlet'i. Sanal makinenin durumunu güncelleştirmeden sonra kullanarak [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) veri diski kaldırma işlemini tamamlamak için cmdlet'i.

```azurepowershell-interactive
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "myDisk"
Update-AzureRmVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
```

Disk depolama alanında kalır, ancak artık bir sanal makineye bağlı değildir.


## <a name="detach-a-data-disk-using-the-portal"></a>Portalı kullanarak veri diski çıkarma

1. Sol menüde **sanal makineler**.
2. Önce ayırmak istediğiniz veri diskinin bulunduğu sanal makineyi seçin **Durdur** için VM'yi serbest bırakın.
3. Sanal makine bölmesinde seçin **diskleri**.
4. Üst kısmındaki **diskleri** bölmesinde **Düzenle**.
5. İçinde **diskleri** bölmesinde, en sağdaki ayırmak için istediğiniz veri diskinin ![Ayır düğmesi](./media/detach-disk/detach.png) düğmesi ayırma.
5. Disk kaldırıldıktan sonra tıklayın **Kaydet** Bölmenin üst kısmındaki.
6. Sanal makine bölmesinden **genel bakış** ve ardından **Başlat** VM'yi yeniden başlatmak için bölmenin üstünde düğme.

Disk depolama alanında kalır, ancak artık bir sanal makineye bağlı değildir.

## <a name="next-steps"></a>Sonraki adımlar
Veri diskini yeniden kullanmak istiyorsanız, eklediğiniz [başka bir VM ekleme](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

