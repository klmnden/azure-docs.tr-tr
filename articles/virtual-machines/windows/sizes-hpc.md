---
title: "Azure Windows VM boyutları - HPC | Microsoft Docs"
description: "Windows yüksek performanslı bilgi işlem azure'da sanal makineler için kullanılabilir farklı boyutlarını listeler. Vcpu, veri diskleri ve NIC yanı sıra bu serideki boyutları için depolama üretilen iş ve ağ bant sayısı hakkında bilgi listeler."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: jonbeck
ms.openlocfilehash: 6f2c72689811d26f95a64fdf5f473606f3ea3f7d
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="high-performance-compute-vm-sizes"></a>Yüksek performanslı bilgi işlem VM boyutları

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


* **İşletim sistemi** -Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

* **MPI** -Microsoft MPI (MS-MPI) 2012 R2 veya sonraki sürümlerde, Intel MPI kitaplığının 5.x

  Desteklenen MPI uygulamaları örnekleri arasında iletişim kurmak için Microsoft Network Direct arabirimini kullanın. 

* **RDMA ağ adresi alanını** -Azure RDMA ağ adres alanı 172.16.0.0/16 ayırır. Bir Azure sanal ağında dağıtılan örneklerinde MPI uygulamaları çalıştırmak için sanal ağ adres alanı RDMA ağ çakışmadığından emin olun.

* **HpcVmDrivers VM uzantısı** - RDMA özellikli Vm'lerinde RDMA bağlantısı için Windows ağ aygıt sürücülerini yüklemek için HpcVmDrivers uzantısını ekleyin. (A8 ve A9 örneklerinin bazı dağıtımlarda HpcVmDrivers uzantı otomatik olarak eklenir.) Bir VM için VM uzantısı eklemek için kullanabileceğiniz [Azure PowerShell](/powershell/azure/overview) cmdlet'leri. 

  
  Aşağıdaki komutu en son sürüm 1.1 HpcVMDrivers uzantısı adlı bir varolan RDMA özelliğine sahip VM yükler *myVM* adlı kaynak grubunda dağıtılan *myResourceGroup* içinde  *Batı ABD* bölge:

  ```PowerShell
  Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  Daha fazla bilgi için bkz: [sanal makine uzantıları ve özellikleri](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). İçinde dağıtılan VM'ler uzantıları ile çalışabilirsiniz [Klasik dağıtım modeli](classic/manage-extensions.md).


## <a name="using-hpc-pack"></a>HPC Pack kullanma

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft'un ücretsiz HPC küme ve iş yönetimi çözümü olan Windows tabanlı MPI uygulamaları ve diğer HPC iş yüklerini çalıştırmak için bir seçenek, bir işlem kümesi oluşturmak. HPC Pack 2012 R2 ve sonraki sürümler için RDMA özellikli sanal makineler dağıtıldığında Azure RDMA ağ kullanan MS MPI çalışma zamanı ortamı içerir.



## <a name="other-sizes"></a>Diğer boyutlara
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](../virtual-machines-windows-sizes-memory.md)
- [Depolama için iyileştirilmiş](../virtual-machines-windows-sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)

## <a name="next-steps"></a>Sonraki adımlar

- Windows Server'da HPC paketi ile işlem yoğunluklu örnekler kullanılacak denetim listeleri için bkz: [MPI uygulamaları çalıştırmak için Windows RDMA küme HPC paketi ile ayarlama](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- İşlem yoğunluklu örnekler MPI uygulamaları ile Azure Batch çalıştırırken kullanmak için bkz: [Azure Batch'de ileti geçirme arabirimi (MPI) uygulamalarını çalıştırmak için çok örnekli görevleri kullanma](../../batch/batch-mpi.md).

- Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları üzerinde işlem performans karşılaştırmanıza yardımcı olur.




