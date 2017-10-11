---
title: "Azure Linux VM boyutları - HPC | Microsoft Docs"
description: "Linux yüksek performanslı bilgi işlem azure'da sanal makineler için kullanılabilir farklı boyutlarını listeler."
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
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: b7a3844ce86b4efac8a4fc3c2540e7b6460873a2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="high-performance-compute-linux-vm-sizes"></a>Yüksek performanslı işlem Linux VM boyutları

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="rdma-capable-instances"></a>RDMA özellikli örnekleri
Bir alt işlem yoğunluklu örnekler (H16r, H16mr, A8 ve A9), doğrudan uzak bellek erişimi (RDMA) bağlantısı için bir ağ arabirimi özelliği. Diğer VM boyutları için kullanılabilir standart Azure ağ arabirimi yanı sıra bu arabirimidir. 
  
Bu arabirim H16r ve H16mr sanal makineler için FDR hızlarını ve A8 ve A9 sanal makineler için QDR hızlarını işletim bir InfiniBand ağ üzerinden iletişim kurmak RDMA özellikli örneklerini verir. RDMA yeteneklere ölçeklenebilirlik ve ileti geçirme arabirimi (MPI) uygulamaların performansını artırabilir.

Azure RDMA ağ erişmek RDMA özellikli Linux VM'ler için gereksinimleri şunlardır:
 
* **Dağıtımları** -Azure Marketi RDMA özellikli SUSE Linux Enterprise Server (SLES) ya da yanlış Wave yazılım (önceki adıyla OpenLogic) CentOS tabanlı HPC görüntülerden VM'ler dağıtmanız gerekir. Aşağıdaki Market görüntülerini RDMA bağlantısı destekler:
  
    * SLES 12 SP1 HPC veya SLES 12 için SP1 için HPC (Premium)
    
    * CentOS tabanlı 7.3 HPC, CentOS tabanlı 7.1 HPC, CentOS tabanlı 6.8 HPC veya CentOS tabanlı 6.5 HPC  
 
        > [!NOTE]
        > Y-serisi VM'ler için HPC görüntüsü için SLES 12 SP1 veya CentOS tabanlı 7.1 veya sonraki HPC görüntü öneririz.
        >
        > CentOS tabanlı HPC görüntülerinde çekirdek güncelleştirmeleri de devre dışı **yum** yapılandırma dosyası. Linux RDMA sürücüleri RPM paket olarak dağıtılır ve çekirdek güncelleştirdiyseniz sürücü güncelleştirmelerini çalışmayabilir nedeni budur.
        > 
        > 
* **MPI** -Intel MPI kitaplığının 5.x
  
    Market görüntüsü bağlı olarak, ayrı lisans, yükleme, seçtiğiniz veya Intel MPI yapılandırmasını, aşağıdaki gibi gerekli olabilir: 
  
  * **SLES 12 SP1 HPC görüntü için** -Intel MPI paketleri VM dağıtılır. Aşağıdaki komutu çalıştırarak yükleyin:

      ```bash
      sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
      ```

  * **CentOS tabanlı HPC görüntüler** -Intel MPI 5.1 zaten yüklü.  
    
    Ek sistem yapılandırması, kümelenmiş sanal makinelerin MPI işlerini çalıştırmak için gereklidir. Örneğin, VM'lerin bir kümede işlem düğümleri arasında güven oluşturmanız gerekir. Tipik ayarları için bkz: [MPI uygulamaları çalıştırmak için Linux RDMA küme ayarlama](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="network-topology-considerations"></a>Ağ topolojisi hakkında önemli noktalar
* RDMA özellikli Linux VM'ler üzerinde Azure, Eth1 RDMA ağ trafiği için ayrılmış. Herhangi bir Eth1 ayarı veya bu ağa başvuran yapılandırma dosyasında herhangi bir bilgiyi değiştirmeyin. Eth0 normal Azure ağ trafiği için ayrılır.

* Azure'da, IP üzerinden InfiniBand (IB) desteklenmiyor. Yalnızca IB üzerinden RDMA desteklenir.

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




