---
title: "Azure Linux VM boyutları - HPC | Microsoft Docs"
description: "Linux yüksek performanslı bilgi işlem azure'da sanal makineler için kullanılabilir farklı boyutlarını listeler. Vcpu, veri diskleri ve NIC yanı sıra bu serideki boyutları için depolama üretilen iş ve ağ bant sayısı hakkında bilgi listeler."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: jonbeck
ms.openlocfilehash: 5f867140981649b73bf6d0bc13eca539c7dc2209
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>Yüksek performanslı işlem sanal makine boyutları

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


### <a name="mpi"></a>MPI 

Yalnızca Intel MPI 5.x sürümleri desteklenir. Sonraki sürümlerinde (2017, 2018) Intel MPI çalışma zamanı kitaplığı Azure Linux RDMA sürücüleri ile uyumlu değil.


### <a name="distributions"></a>Dağıtımlar
 
Bir işlem yoğunluklu VM RDMA bağlantısı destekleyen Azure Market görüntülerini birinden dağıtın:
  
* **Ubuntu** -Ubuntu Server 16.04 LTS. RDMA sürücüleri VM yapılandırın ve Intel MPI indirmek için Intel ile kaydedin:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **SUSE Linux Enterprise Server** -SLES 12 HPC, SLES 12 SP3 HPC (Premium), SLES 12 SP3 SP1 HPC, SLES 12 için SP1 için HPC (Premium). RDMA sürücülerinin yüklü olduğunu ve Intel MPI paketleri VM dağıtılmaz. Aşağıdaki komutu çalıştırarak MPI yükleyin:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
    
* **CentOS tabanlı HPC** -CentOS tabanlı 6.5 HPC veya sonraki bir sürümünü (H-seri için sürüm 7.1 veya üzeri önerilir). RDMA sürücüler ve Intel MPI 5.1 VM yüklenir.  
 
  > [!NOTE]
  > CentOS tabanlı HPC görüntülerinde çekirdek güncelleştirmeleri de devre dışı **yum** yapılandırma dosyası. Linux RDMA sürücüleri RPM paket olarak dağıtılır ve çekirdek güncelleştirdiyseniz sürücü güncelleştirmelerini çalışmayabilir nedeni budur.
  > 
 
### <a name="cluster-configuration"></a>Küme yapılandırması 
    
Ek sistem yapılandırması, kümelenmiş sanal makinelerin MPI işlerini çalıştırmak için gereklidir. Örneğin, VM'lerin bir kümede işlem düğümleri arasında güven oluşturmanız gerekir. Tipik ayarları için bkz: [MPI uygulamaları çalıştırmak için Linux RDMA küme ayarlama](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="network-topology-considerations"></a>Ağ topolojisi hakkında önemli noktalar
* RDMA özellikli Linux VM'ler üzerinde Azure, Eth1 RDMA ağ trafiği için ayrılmış. Herhangi bir Eth1 ayarı veya bu ağa başvuran yapılandırma dosyasında herhangi bir bilgiyi değiştirmeyin. Eth0 normal Azure ağ trafiği için ayrılır.

* Azure RDMA ağ adres alanı 172.16.0.0/16 ayırır. 


## <a name="using-hpc-pack"></a>HPC Pack kullanma
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft'un ücretsiz HPC küme ve iş yönetimi çözümü, işlem yoğunluklu örnekler Linux ile kullanmanız için bir seçenek değil. HPC Pack Destek'ın en son sürümlerine işlem bir Windows Server baş düğümü tarafından yönetilen Azure vm'lerinde dağıtılan düğümleri üzerinde çalışmak için birkaç Linux dağıtımları. Intel MPI çalıştıran RDMA özellikli Linux işlem düğümleri ile HPC Pack zamanlayabilir ve Linux MPI RDMA ağ erişim uygulamaları çalıştırın. Bkz: [Linux işlem düğümlerini Azure bir HPC Pack kümesindeki kullanmaya başlama](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Diğer boyutlara
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)


## <a name="next-steps"></a>Sonraki adımlar

- Dağıtma ve RDMA Linux üzerinde işlem yoğunluklu boyutları kullanarak başlamak için bkz: [MPI uygulamaları çalıştırmak için Linux RDMA küme ayarlama](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları üzerinde işlem performans karşılaştırmanıza yardımcı olur.




