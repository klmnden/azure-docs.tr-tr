---
title: "Bir Windows VM - Azure veri diski kullanımdan çıkarın | Microsoft Docs"
description: "Resource Manager dağıtım modelini kullanarak azure'da bir sanal makineden bir veri diskini öğrenin."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: bbbd31313db44d32a829e9e4c6c9b5fd9c0e533e
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Bir Windows sanal makineden bir veri diskini nasıl
Sanal makineye bağlı bir veri diskine ihtiyacınız olmadığında bunu kolayca ayırabilirsiniz. Bu diski sanal makineden kaldırır, ancak depolama biriminden kaldırmaz.

> [!WARNING]
> Bir disk ayırırsanız otomatik olarak silinmez. Premium depolama alanına aboneliğiniz varsa, disk depolama ücretleri uygulanmaya devam eder. Daha fazla bilgi için bkz [fiyatlandırma ve faturalama Premium depolama kullanırken](premium-storage.md#pricing-and-billing).
>
>

Disk üzerinde var olan verileri yeniden kullanmak isterseniz bu verileri aynı sanal makineye veya başka birine yeniden ekleyebilirsiniz.

## <a name="detach-a-data-disk-using-the-portal"></a>Portalı kullanarak veri diski çıkarma
1. Portal hub seçin **sanal makineleri**.
2. Önce ayırmak istediğiniz veri diskine sahip bir sanal makineyi seçin **durdurmak** VM ayırması kaldırılacak.
3. Sanal makine dikey penceresinde, seçin **diskleri**.
4. Üstündeki **diskleri** dikey penceresinde, select **Düzenle**.
5. İçinde **diskleri** ayırmak için istediğiniz veri diski en sağdaki dikey ![ayırma düğme görüntüsü](./media/detach-disk/detach.png) düğmesi ayırın.
5. Disk kaldırıldıktan sonra dikey pencerenin üst kısmında Kaydet'i tıklatın.
6. Sanal makine dikey penceresinde tıklayın **genel bakış** ve ardından **Başlat** VM'yi yeniden başlatmak için dikey pencerenin üstündeki düğmesi.



Disk depolama alanında kalır, ancak artık bir sanal makineye bağlı değildir.

## <a name="detach-a-data-disk-using-powershell"></a>PowerShell kullanarak bir veri diskini
Bu örnekte, ilk komut adlı sanal makineye alır **MyVM07** içinde **RG11** Get-AzureRmVM cmdlet'ini kullanarak kaynak grubu. Sanal makinede komut depolar **$VirtualMachine** değişkeni.

İkinci komut sanal makineden DataDisk3 adlı veri diski kaldırır.

Son komut, veri diski kaldırma işlemini tamamlamak için sanal makinenin durumunu güncelleştirir.

```azurepowershell-interactive
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine
```

Daha fazla bilgi için bkz: [Kaldır AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).

## <a name="next-steps"></a>Sonraki adımlar
Veri diski yeniden kullanmak istiyorsanız, yeni [başka bir VM'e ekleyin](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

