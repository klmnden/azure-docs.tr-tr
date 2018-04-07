---
title: Bir Windows VM - Azure veri diski kullanımdan çıkarın | Microsoft Docs
description: Resource Manager dağıtım modelini kullanarak azure'da bir sanal makineden bir veri diskini öğrenin.
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
ms.date: 11/17/2017
ms.author: cynthn
ms.openlocfilehash: e56e9ce22cc9e2bad75c944c20bff812d8720d18
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Bir Windows sanal makineden bir veri diskini nasıl
Sanal makineye bağlı bir veri diskine ihtiyacınız olmadığında bunu kolayca ayırabilirsiniz. Bu diski sanal makineden kaldırır, ancak depolama biriminden kaldırmaz.

> [!WARNING]
> Bir disk ayırırsanız otomatik olarak silinmez. Premium depolama alanına aboneliğiniz varsa, disk depolama ücretleri uygulanmaya devam eder. Daha fazla bilgi için bkz [fiyatlandırma ve faturalama Premium depolama kullanırken](premium-storage.md#pricing-and-billing).
>
>

Disk üzerinde var olan verileri yeniden kullanmak isterseniz bu verileri aynı sanal makineye veya başka birine yeniden ekleyebilirsiniz.

## <a name="detach-a-data-disk-using-the-portal"></a>Portalı kullanarak veri diski çıkarma

1. Soldaki menüde seçin **sanal makineleri**.
2. Önce ayırmak istediğiniz veri diskine sahip bir sanal makineyi seçin **durdurmak** VM ayırması kaldırılacak.
3. Sanal makine bölmesinde seçin **diskleri**.
4. Üstündeki **diskleri** bölmesinde, **Düzenle**.
5. İçinde **diskleri** ayırmak için istediğiniz veri diski sağ bölmesinde ![ayırma düğme görüntüsü](./media/detach-disk/detach.png) düğmesi ayırın.
5. Disk kaldırıldıktan sonra tıklatın **kaydetmek** bölmesinin üst kısmında.
6. Sanal makine bölmesinde **genel bakış** ve ardından **Başlat** VM'yi yeniden başlatmak için bölmenin üstündeki düğmesi.



Disk depolama alanında kalır, ancak artık bir sanal makineye bağlı değildir.

## <a name="detach-a-data-disk-using-powershell"></a>PowerShell kullanarak bir veri diskini
Bu örnekte, ilk komut adlı sanal makineye alır **MyVM07** içinde **RG11** kaynak grubunu kullanarak [Get-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet'i ve depolar**$VirtualMachine** değişkeni.

İkinci satır kullanarak sanal makineyi DataDisk3 adlı veri diski kaldırır [Kaldır AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk) cmdlet'i.

Üçüncü satır sanal makinenin durumunu güncelleştirir kullanarak [güncelleştirme-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) veri diski kaldırma işlemini tamamlamak için cmdlet'i.

```azurepowershell-interactive
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
Update-AzureRmVM -ResourceGroupName "RG11" -VM $VirtualMachine
```

Daha fazla bilgi için bkz: [Kaldır AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).

## <a name="next-steps"></a>Sonraki adımlar
Veri diski yeniden kullanmak istiyorsanız, yeni [başka bir VM'e ekleyin](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

