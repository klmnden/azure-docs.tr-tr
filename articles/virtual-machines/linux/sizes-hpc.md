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
ms.openlocfilehash: 003a14174ff65bab253f27a458d4f3e2c0a1a6db
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069991"
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>Yüksek performanslı bilgi işlem, sanal makine boyutları

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


### <a name="mpi"></a>MPI 

SR-IOV etkin VM boyutları, Azure üzerinde neredeyse izin herhangi kullanılacak MPI flavor.
SR-IOV olmayan etkin Vm'lerde yalnızca Intel MPI 5.x sürümler desteklenir. Sonraki sürümlerinde (2017, 2018) Intel MPI çalışma zamanı kitaplığı olabilir veya Azure Linux RDMA sürücüleri ile uyumlu olmayabilir.


### <a name="supported-os-images"></a>Desteklenen işletim sistemi görüntüleri
 
Azure marketi, RDMA bağlantısı destekleyen çok sayıda Linux dağıtımları sahiptir:
  
* **CentOS tabanlı HPC** - olmayan-SR-IOV özellikli VM'ler, CentOS tabanlı sürüm 6.5 için HPC veya 7.5 kadar sonraki bir sürümünü uygun. Sürüm 7.1 için 7.5 H serisi VM'ler için önerilir. RDMA sürücüleri ve Intel MPI 5.1 sanal makinede yüklü.
  SR-IOV sanal makineler için CentOS HPC 7.6 en iyi duruma getirilmiş ve önceden yüklenen RDMA sürücüleri ve çeşitli MPI paketlerin yüklü ile birlikte gelir.
  Diğer RHEL/CentOS VM görüntüleri için InfiniBand etkinleştirmek için InfiniBandLinux uzantısı ekleyin. Bu bir Linux VM uzantısı RDMA bağlantısı için Mellanox OFED sürücüleri (vm'lerde SR-IOV) yükler. Aşağıdaki PowerShell cmdlet'ini, var olan bir RDMA özellikli sanal makinesinde InfiniBandDriverLinux uzantının en son sürümü (sürüm 1.0) yükler. RDMA özellikli VM adlı *myVM* ve adlı kaynak grubunda dağıtılan *myResourceGroup* içinde *Batı ABD* bölge aşağıdaki gibi:

  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "InfiniBandDriverLinux" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverLinux" -TypeHandlerVersion "1.0"
  ```
  Alternatif olarak, VM uzantılarını aşağıdaki JSON öğesi ile kolay dağıtım için Azure Resource Manager şablonları eklenebilir:
  ```json
  "properties":{
  "publisher": "Microsoft.HpcCompute",
  "type": "InfiniBandDriverLinux",
  "typeHandlerVersion": "1.0",
  } 
  ```
 
  > [!NOTE]
  > CentOS tabanlı HPC görüntülerinde de çekirdek güncelleştirmeler devre dışı bırakıldı **yum** yapılandırma dosyası. Linux RDMA sürücüleri bir RPM paket olarak dağıtılır ve çekirdek güncelleştirildiyse, sürücü güncelleştirmelerini çalışmayabilir nedeni budur.
  >
  

* **SUSE Linux Enterprise Server** -SLES 12 SP3 için HPC, SLES 12 SP3 HPC (Premium), SLES 12 için HPC, SLES 12 SP1 for HPC (Premium), SLES 12 SP4 ve SLES 15 SP1. RDMA sürücüleri yüklenir ve Intel MPI paketler VM'de dağıtılır. MPI, aşağıdaki komutu çalıştırarak yükleyin:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
  
* **Ubuntu** -Ubuntu Server 16.04 LTS, 18.04 LTS. VM'de RDMA sürücüleri yapılandırın ve Intel MPI indirmek için Intel ile kaydedin:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]  

  MPI ayarı Infiniband, etkinleştirme hakkında daha fazla ayrıntı için bkz. [etkinleştirme InfiniBand](../workloads/hpc/enable-infiniband.md).


### <a name="cluster-configuration-options"></a>Küme yapılandırma seçenekleri

Azure, RDMA ağ aracılığıyla iletişim kuran Linux HPC VM kümeleri oluşturmak için çeşitli seçenekler sunar dahil olmak üzere: 

* **Sanal makineler** -RDMA özellikli HPC VM'lerin aynı kullanılabilirlik (Azure Resource Manager dağıtım modeli kullandığınız zaman) kümesinde dağıtın. Klasik dağıtım modelini kullanıyorsanız, aynı bulut hizmetindeki sanal makineleri dağıtın. 

* **Sanal makine ölçek kümeleri** - bir sanal makine ölçek kümesi, tek bir yerleştirme grubu dağıtımı sınırladığınızdan emin olun. Örneğin, bir Resource Manager şablonunda ayarlamak `singlePlacementGroup` özelliğini `true`. 

* **Azure CycleCloud** -bir HPC kümesi oluşturma [Azure CycleCloud](/azure/cyclecloud/) Linux düğümlerinde MPI işlerini çalıştırma için.

* **Azure Batch** -oluşturma bir [Azure Batch](/azure/batch/) işlem düğümleri havuzu MPI iş yüklerini Linux üzerinde çalıştırılacak. Daha fazla bilgi için [kullanım RDMA özellikli veya GPU özellikli örnekler Batch havuzlarında](../../batch/batch-pool-compute-intensive-sizes.md). Ayrıca bkz: [Batch Shipyard](https://github.com/Azure/batch-shipyard) toplu olarak kapsayıcı tabanlı iş yüklerini çalıştırmaya yönelik proje.

* **Microsoft HPC Pack** - [HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) yönetilen bir Windows Server baş düğüm çalıştırmak için çeşitli Linux dağıtımları, RDMA özellikli Azure Vm'lerinde dağıtılan düğüm işlem destekler. Örnek bir dağıtım için bkz: [azure'da HPC Pack Linux RDMA kümesi oluşturma](https://docs.microsoft.com/powershell/high-performance-computing/hpcpack-linux-openfoam).


### <a name="network-considerations"></a>Ağ konuları
* Olmayan-SR-IOV hakkında RDMA özellikli Linux Vm'leri azure'da eth1 RDMA ağ trafiği için ayrılmış. Eth1 ayarları veya bu ağa başvuran yapılandırma dosyasındaki bilgileri değiştirmeyin.
* Üzerinde SR-IOV etkin Vm'leri (HB ve HC serisi), ib0 RDMA ağ trafiği için ayrılır.
* Azure'da RDMA ağ adres alanı 172.16.0.0/16 ayırır. Bir Azure sanal ağında dağıtılan örneklerinde MPI uygulamalarını çalıştırmak için sanal ağ adres alanı RDMA ağ çakışmadığından emin olun.
* Tercih ettiğiniz küme yönetim aracını bağlı olarak, ek sistem yapılandırması MPI işlerini çalıştırma için gerekli olabilir. Örneğin, bir VM kümesinde SSH anahtarları oluşturma veya parolasız SSH oturumu açma oluşturarak küme düğümleri arasında güven oluşturma gerekebilir.


## <a name="other-sizes"></a>Diğer boyutları
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)
- [Önceki nesil](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

- Kurulum, en iyi duruma getirmek ve ölçeklendirme hakkında daha fazla bilgi [HPC iş yüklerini](../workloads/hpc/configure.md) azure'da.
- Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları arasında işlem performansını karşılaştırmanıza yardımcı olabilir.
