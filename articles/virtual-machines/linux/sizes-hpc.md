---
title: Azure Linux VM boyutları - HPC | Microsoft Docs
description: Farklı Linux yüksek performanslı Azure sanal makinelere bilgi işlem için kullanılabilir boyutları listeler. Vcpu, veri diskleri ve NIC yanı sıra bu serideki boyutları için depolama aktarım hızı ve ağ bant sayısı hakkında bilgiler listelenir.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: jonbeck
ms.openlocfilehash: 748cb4612b2b5aed26ba8197cfad0782f2645e1e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37902138"
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>Yüksek performanslı bilgi işlem, sanal makine boyutları

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


### <a name="mpi"></a>MPI 

Yalnızca Intel MPI 5.x sürümler desteklenir. Sonraki sürümlerinde (2017, 2018) Intel MPI çalışma zamanı kitaplığı, Azure Linux RDMA sürücüleri ile uyumlu değildir.


### <a name="distributions"></a>Dağıtımlar
 
Yoğun işlem gücü kullanımlı VM görüntüleri Azure Market'te RDMA bağlantısı destekler birinden dağıtın:
  
* **Ubuntu** -Ubuntu Server 16.04 LTS. VM'de RDMA sürücüleri yapılandırın ve Intel MPI indirmek için Intel ile kaydedin:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **SUSE Linux Enterprise Server** -SLES 12 SP3 HPC, SLES 12 için HPC (Premium), SLES 12 için SP3 SP1 için HPC, SLES 12 SP1 için HPC (Premium). RDMA sürücüleri yüklenir ve Intel MPI paketler VM'de dağıtılır. MPI, aşağıdaki komutu çalıştırarak yükleyin:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
    
* **CentOS tabanlı HPC** -6.5 HPC CentOS tabanlı veya sonraki bir sürümü (sürüm 7.1 veya sonraki sürümleri için H-serisi, önerilir). RDMA sürücüleri ve Intel MPI 5.1 sanal makinede yüklü.  
 
  > [!NOTE]
  > CentOS tabanlı HPC görüntülerinde de çekirdek güncelleştirmeler devre dışı bırakıldı **yum** yapılandırma dosyası. Linux RDMA sürücüleri bir RPM paket olarak dağıtılır ve çekirdek güncelleştirildiyse, sürücü güncelleştirmelerini çalışmayabilir nedeni budur.
  > 
 
### <a name="cluster-configuration"></a>Küme yapılandırması 
    
Ek sistem yapılandırması, kümelenmiş VM'ler üzerinde MPI işlerini çalıştırma için gereklidir. Örneğin, üzerinde bir VM kümesi, işlem düğümleri arasında bir güven gerekir. Normal ayarları için bkz: [MPI uygulamalarını çalıştırmak için bir Linux RDMA kümesi ayarlama](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="network-topology-considerations"></a>Ağ topolojisi hakkında önemli noktalar
* RDMA özellikli azure'da Linux VM'ler üzerinde Eth1 RDMA ağ trafiği için ayrılmış. Eth1 ayarları veya bu ağa başvuran yapılandırma dosyasındaki bilgileri değiştirmeyin. Eth0 normal Azure ağ trafiği için ayrılmış.

* Azure'da RDMA ağ adres alanı 172.16.0.0/16 ayırır. 


## <a name="using-hpc-pack"></a>HPC Pack kullanma
[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft'un ücretsiz HPC küme ve iş yönetim çözümü, yoğun işlem gücü kullanımlı örnekler Linux ile kullanabilmeniz için bir seçenektir. HPC Pack desteği en son sürümleri üzerinde çalışmak üzere çeşitli Linux dağıtımları Windows Server baş düğümü tarafından yönetilen Azure vm'lerinde dağıtılan bilgi işlem düğümü. Intel MPI çalıştıran RDMA özellikli Linux işlem düğümleriyle HPC Pack zamanlayabilir ve Linux MPI RDMA ağ erişen uygulamalar çalıştırın. Bkz: [azure'da HPC Pack kümesinde Linux işlem düğümleri kullanmaya başlama](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="other-sizes"></a>Diğer boyutları
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)
- [Önceki nesil](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

- Dağıtma ve Linux'ta RDMA ile yoğun işlem gücü kullanımlı boyutlar başlamak için bkz: [MPI uygulamalarını çalıştırmak için bir Linux RDMA kümesi ayarlama](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

- Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları arasında işlem performansını karşılaştırmanıza yardımcı olabilir.




