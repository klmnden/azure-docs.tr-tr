---
title: Windows VM'den - Azure veri diski çıkarma | Microsoft Docs
description: Azure Resource Manager dağıtım modelini kullanarak bir sanal makineden veri diski çıkarma.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
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
ms.openlocfilehash: ca9a4478249e935afb6a52520c77d9df159fe9e7
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718734"
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Nasıl bir Windows sanal makinesinden veri diski çıkarma

Sanal makineye bağlı bir veri diskine ihtiyacınız olmadığında bunu kolayca ayırabilirsiniz. Bu diski sanal makineden kaldırır, ancak depolama alanından kaldırmaz.

> [!WARNING]
> Disk ayırma, otomatik olarak silinmez. Premium depolamaya abone olduğunuz, disk için depolama ücretleri uygulanmaya devam edecektir. Daha fazla bilgi için [fiyatlandırma ve Premium depolama kullanırken faturalama](disks-types.md#billing).

Disk üzerinde var olan verileri yeniden kullanmak isterseniz bu verileri aynı sanal makineye veya başka birine yeniden ekleyebilirsiniz.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="detach-a-data-disk-using-powershell"></a>PowerShell kullanarak veri diski çıkarma

Yapabilecekleriniz *sık erişimli* PowerShell kullanarak veri diski kaldırma, ancak hiçbir şey etkin bir şekilde kullanarak diski sanal makineden kullanımdan çıkarmadan önce emin olun.

Bu örnekte biz adlı disk kaldırmak **myDisk** VM'den **myVM** içinde **myResourceGroup** kaynak grubu. İlk disk kullanarak kaldırmanız [Remove-AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/remove-azvmdatadisk) cmdlet'i. Sanal makinenin durumunu güncelleştirmeden sonra kullanarak [güncelleştirme-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) veri diski kaldırma işlemini tamamlamak için cmdlet'i.

```azurepowershell-interactive
$VirtualMachine = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
Remove-AzVMDataDisk -VM $VirtualMachine -Name "myDisk"
Update-AzVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
```

Disk depolama alanında kalır, ancak artık bir sanal makineye bağlı değildir.

## <a name="detach-a-data-disk-using-the-portal"></a>Portalı kullanarak veri diski çıkarma

1. Sol menüde **sanal makineler**.
2. Önce ayırmak istediğiniz veri diskinin bulunduğu sanal makineyi seçin **Durdur** VM ayırmayı iptal etmek için.
3. Sanal makine bölmesinde seçin **diskleri**.
4. Üst kısmındaki **diskleri** bölmesinde **Düzenle**.
5. İçinde **diskleri** bölmesinde, en sağdaki ayırmak için istediğiniz veri diskinin ![Ayır düğmesi](./media/detach-disk/detach.png) düğmesi ayırma.
5. Disk kaldırıldıktan sonra tıklayın **Kaydet** Bölmenin üst kısmındaki.
6. Sanal makine bölmesinden **genel bakış** ve ardından **Başlat** VM'yi yeniden başlatmak için bölmenin üstünde düğme.

Disk depolama alanında kalır, ancak artık bir sanal makineye bağlı değildir.

## <a name="next-steps"></a>Sonraki adımlar

Veri diskini yeniden kullanmak istiyorsanız, eklediğiniz [başka bir VM ekleme](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)