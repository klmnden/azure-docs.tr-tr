---
title: Azure Windows VM boyutları - HPC | Microsoft Docs
description: Farklı Windows yüksek performanslı Azure sanal makinelere bilgi işlem için kullanılabilir boyutları listeler. Vcpu, veri diskleri ve NIC yanı sıra bu serideki boyutları için depolama aktarım hızı ve ağ bant sayısı hakkında bilgiler listelenir.
services: virtual-machines-windows
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/12/2018
ms.author: jonbeck
ms.openlocfilehash: 58d4ced041b6f5cf767b45191e28a4b395f584b6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60540521"
---
# <a name="high-performance-compute-vm-sizes"></a>Yüksek performanslı bilgi işlem VM boyutları

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


* **İşletim sistemi** -Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

* **MPI** -Microsoft MPI (MS-MPI) 2012 R2 veya sonraki sürümlerde, Intel MPI kitaplığının 5.x

  Desteklenen MPI uygulamaları Microsoft Network Direct arabirimi örnekleri arasında iletişim kurmak için kullanır. 

* **RDMA ağ adres alanı** -azure'da RDMA ağ adres alanı 172.16.0.0/16 ayırır. Bir Azure sanal ağında dağıtılan örneklerinde MPI uygulamalarını çalıştırmak için sanal ağ adres alanı RDMA ağ çakışmadığından emin olun.

* **HpcVmDrivers VM uzantısı** - RDMA özellikli Vm'lerde RDMA bağlantısı için Windows ağ aygıt sürücülerini yüklemek için HpcVmDrivers uzantısını ekleyin. (A8 ve A9 örnekleri bazı dağıtımlarda HpcVmDrivers uzantı otomatik olarak eklenir.) Bir VM için VM uzantısı eklemek için kullanabileceğiniz [Azure PowerShell](/powershell/azure/overview) cmdlet'leri. 

  
  Aşağıdaki komutu adlı bir mevcut RDMA özellikli sanal makinesinde en son sürüm 1.1 HpcVMDrivers uzantıyı yükler *myVM* adlı kaynak grubunda dağıtılan *myResourceGroup* içinde  *Batı ABD* bölgesi:

  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "HpcVmDrivers" -Publisher "Microsoft.HpcCompute" -Type "HpcVmDrivers" -TypeHandlerVersion "1.1"
  ```
  
  Daha fazla bilgi için [sanal makine uzantıları ve özellikleri](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Uzantıları ile dağıtılmış VM'ler için çalışabilir [Klasik dağıtım modeli](classic/manage-extensions.md).

### <a name="cluster-configuration-options"></a>Küme yapılandırma seçenekleri

Azure, RDMA ağ aracılığıyla iletişim kuran bir Windows HPC VM kümeleri oluşturmak için çeşitli seçenekler sunar dahil olmak üzere: 

* **Sanal makineler** -RDMA özellikli HPC VM'lerin aynı kullanılabilirlik (Azure Resource Manager dağıtım modeli kullandığınız zaman) kümesinde dağıtın. Klasik dağıtım modelini kullanıyorsanız, aynı bulut hizmetindeki sanal makineleri dağıtın. 

* **Sanal makine ölçek kümeleri** - bir VM ölçek kümesi, tek bir yerleştirme grubu dağıtımı sınırladığınızdan emin olun. Örneğin, bir Resource Manager şablonunda ayarlamak `singlePlacementGroup` özelliğini `true`. 

* **Azure CycleCloud** -bir HPC kümesi oluşturma [Azure CycleCloud](/azure/cyclecloud/) Windows düğümlerinde MPI işlerini çalıştırma için.

* **Azure Batch** -oluşturma bir [Azure Batch](/azure/batch/) Windows Server'da MPI iş yüklerini çalıştırmak için havuz işlem düğümlerini. Daha fazla bilgi için [kullanım RDMA özellikli veya GPU özellikli örnekler Batch havuzlarında](../../batch/batch-pool-compute-intensive-sizes.md). Ayrıca bkz: [Batch Shipyard](https://github.com/Azure/batch-shipyard) toplu olarak kapsayıcı tabanlı iş yüklerini çalıştırmaya yönelik proje.

* **Microsoft HPC Pack** - [HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) RDMA Özellikli Windows Vm'lerinde dağıtılan Azure RDMA ağ kullanan MS MPI çalışma zamanı ortamı içerir. Örneğin dağıtımlar için bkz. [MPI uygulamalarını çalıştırmak için HPC Pack ile Windows RDMA kümesi ayarlama](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Diğer boyutları
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](../virtual-machines-windows-sizes-memory.md)
- [Depolama için iyileştirilmiş](../virtual-machines-windows-sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Önceki nesil](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

- Yoğun işlem gücü kullanımlı örnekler Windows Server'da HPC Pack ile kullanmak için bkz [MPI uygulamalarını çalıştırmak için HPC Pack ile Windows RDMA kümesi ayarlama](classic/hpcpack-rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

- Yoğun işlem gücü kullanımlı örnekler MPI uygulamalarını Azure Batch ile çalışırken kullanmak için bkz: [Azure Batch'de ileti geçirme arabirimi (MPI) uygulamalarını çalıştırmak için çok örnekli görevleri kullanma](../../batch/batch-mpi.md).

- Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları arasında işlem performansını karşılaştırmanıza yardımcı olabilir.




