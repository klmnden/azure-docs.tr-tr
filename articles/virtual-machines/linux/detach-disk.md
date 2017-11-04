---
title: "Bir Linux VM - Azure bir veri diski kullanımdan çıkarın | Microsoft Docs"
description: "Bir veri diskini CLI 2.0 ya da Azure portal kullanarak azure'da bir sanal makineden öğrenin."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: de0222d897ed2cf94be98501c39385ac88f866fc
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="how-to-detach-a-data-disk-from-a-linux-virtual-machine"></a>Nasıl bir Linux sanal makine veri diski kullanımdan çıkarın

Sanal makineye bağlı bir veri diskine ihtiyacınız olmadığında bunu kolayca ayırabilirsiniz. Bu diski sanal makineden kaldırır, ancak depolama biriminden kaldırmaz. 

> [!WARNING]
> Bir disk ayırırsanız otomatik olarak silinmez. Premium depolama alanına aboneliğiniz varsa, disk depolama ücretleri uygulanmaya devam eder. Daha fazla bilgi için bkz [fiyatlandırma ve faturalama Premium depolama kullanırken](../windows/premium-storage.md#pricing-and-billing). 
> 
> 

Disk üzerinde var olan verileri yeniden kullanmak isterseniz bu verileri aynı sanal makineye veya başka birine yeniden ekleyebilirsiniz.  

## <a name="detach-a-data-disk-using-cli-20"></a>CLI 2.0 kullanan bir veri diskini

```azurecli
az vm disk detach -g myResourceGroup --vm-name myVm -n myDataDisk
```

Disk depolama alanında kalır, ancak artık bir sanal makineye bağlı değildir.


## <a name="detach-a-data-disk-using-the-portal"></a>Portalı kullanarak veri diski çıkarma
1. Portal hub seçin **sanal makineleri**.
2. Önce ayırmak istediğiniz veri diskine sahip bir sanal makineyi seçin **durdurmak** VM ayırması kaldırılacak.
3. Sanal makine dikey penceresinde, seçin **diskleri**.
4. Üstündeki **diskleri** dikey penceresinde, select **Düzenle**.
5. İçinde **diskleri** ayırmak için istediğiniz veri diski en sağdaki dikey ![ayırma düğme görüntüsü](./media/detach-disk/detach.png) düğmesi ayırın.
5. Disk kaldırıldıktan sonra dikey pencerenin üst kısmında Kaydet'i tıklatın.
6. Sanal makine dikey penceresinde tıklayın **genel bakış** ve ardından **Başlat** VM'yi yeniden başlatmak için dikey pencerenin üstündeki düğmesi.

Disk depolama alanında kalır, ancak artık bir sanal makineye bağlı değildir.








## <a name="next-steps"></a>Sonraki adımlar
Veri diski yeniden kullanmak istiyorsanız, yeni [başka bir VM'e ekleyin](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

