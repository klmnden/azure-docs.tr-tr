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
ms.date: 10/12/2018
ms.author: jonbeck
ms.openlocfilehash: 44b965bd60d976d4d28dc5e31d78a1c838d4ee02
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60542357"
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>Yüksek performanslı bilgi işlem, sanal makine boyutları

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


### <a name="mpi"></a>MPI 

Yalnızca Intel MPI 5.x sürümler desteklenir.

> [!NOTE]
> Sonraki sürümlerinde (2017, 2018) Intel MPI çalışma zamanı kitaplığı olabilir veya Azure Linux RDMA sürücüleri ile uyumlu olmayabilir.

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
 
### <a name="cluster-configuration-options"></a>Küme yapılandırma seçenekleri

Azure, RDMA ağ aracılığıyla iletişim kuran Linux HPC VM kümeleri oluşturmak için çeşitli seçenekler sunar dahil olmak üzere: 

* **Sanal makineler** -RDMA özellikli HPC VM'lerin aynı kullanılabilirlik (Azure Resource Manager dağıtım modeli kullandığınız zaman) kümesinde dağıtın. Klasik dağıtım modelini kullanıyorsanız, aynı bulut hizmetindeki sanal makineleri dağıtın. 

* **Sanal makine ölçek kümeleri** - bir VM ölçek kümesi, tek bir yerleştirme grubu dağıtımı sınırladığınızdan emin olun. Örneğin, bir Resource Manager şablonunda ayarlamak `singlePlacementGroup` özelliğini `true`. 

* **Azure CycleCloud** -bir HPC kümesi oluşturma [Azure CycleCloud](/azure/cyclecloud/) Linux düğümlerinde MPI işlerini çalıştırma için.

* **Azure Batch** -oluşturma bir [Azure Batch](/azure/batch/) işlem düğümleri havuzu MPI iş yüklerini Linux üzerinde çalıştırılacak. Daha fazla bilgi için [kullanım RDMA özellikli veya GPU özellikli örnekler Batch havuzlarında](../../batch/batch-pool-compute-intensive-sizes.md). Ayrıca bkz: [Batch Shipyard](https://github.com/Azure/batch-shipyard) toplu olarak kapsayıcı tabanlı iş yüklerini çalıştırmaya yönelik proje.

* **Microsoft HPC Pack** - [HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) yönetilen bir Windows Server baş düğüm çalıştırmak için çeşitli Linux dağıtımları, RDMA özellikli Azure Vm'lerinde dağıtılan düğüm işlem destekler. Örnek bir dağıtım için bkz: [azure'da HPC Pack Linux RDMA kümesi oluşturma](https://docs.microsoft.com/powershell/high-performance-computing/hpcpack-linux-openfoam).

Tercih ettiğiniz küme yönetim aracını bağlı olarak, ek sistem yapılandırması MPI işlerini çalıştırma için gerekli olabilir. Örneğin, bir VM kümesinde SSH anahtarları oluşturma veya parolasız SSH güven oluşturma küme düğümleri arasında güven oluşturma gerekebilir.

### <a name="network-topology-considerations"></a>Ağ topolojisi hakkında önemli noktalar
* RDMA özellikli azure'da Linux VM'ler üzerinde Eth1 RDMA ağ trafiği için ayrılmış. Eth1 ayarları veya bu ağa başvuran yapılandırma dosyasındaki bilgileri değiştirmeyin. Eth0 normal Azure ağ trafiği için ayrılmış.

* Azure'da RDMA ağ adres alanı 172.16.0.0/16 ayırır. 




## <a name="other-sizes"></a>Diğer boyutları
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)
- [Önceki nesil](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları arasında işlem performansını karşılaştırmanıza yardımcı olabilir.




