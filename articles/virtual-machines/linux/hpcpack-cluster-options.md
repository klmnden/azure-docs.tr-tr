---
title: "Azure Linux HPC paketi küme seçenekleri | Microsoft Docs"
description: "Oluşturup bir Linux yüksek performanslı bilgi işlem (HPC) küme Azure bulutta yönetmek için Microsoft HPC paketi ile seçenekleri hakkında bilgi edinin"
services: virtual-machines-linux,cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: ac60624e-aefa-40c3-8bc1-ef6d5c0ef1a2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/26/2017
ms.author: danlep
ms.openlocfilehash: ae36e64c0261b1fe8d02d6dcb80df7cdee333db9
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="options-with-hpc-pack-to-create-and-manage-a-cluster-for-linux-hpc-workloads-in-azure"></a>HPC Pack oluşturmak ve azure'da Linux HPC iş yükleri için bir kümeyi yönetmek için seçenekleri
[!INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Bu makalede, Linux iş yüklerini çalıştırmak için HPC Pack kullanma seçenekleri odaklanır. Ayrıca çalıştırmak için seçenek vardır [HPC paketi ile Windows HPC iş yüklerini](../windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="hpc-pack-cluster-in-azure-vms-and-vm-scale-sets"></a>HPC Pack kümede Azure Vm'leri ve VM ölçek kümesi
### <a name="azure-templates"></a>Azure şablonları
* (GitHub) [HPC Pack 2016 küme şablonları](https://github.com/MsHpcPack/HPCPack2016)
* (GitHub) [HPC Pack 2012 R2 küme şablonları](https://github.com/MsHpcPack/HPCPack2012R2)
* (Market) [Linux iş yükleri için HPC paketi küme](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)
* (Hızlı Başlangıç) [Linux işlem düğümleri ile bir HPC kümesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)

### <a name="powershell-deployment-script-for-hpc-pack-2012-r2"></a>HPC Pack 2012 R2 için PowerShell dağıtım komut dosyası
* [Linux HPC Kümesi ile HPC Pack Iaas dağıtım komut dosyası oluşturma](../windows/classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="tutorials"></a>Öğreticiler
* [Öğretici: Azure HPC Pack kümesinde Linux işlem düğümlerine kullanmaya başlama](classic/hpcpack-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Öğretici: Çalıştır NAMD Microsoft HPC paketi ile Linux işlem düğümlerini Azure](classic/hpcpack-cluster-namd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Öğretici: Azure Linux RDMA kümesinde Microsoft HPC paketi ile çalışma OpenFOAM](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Öğretici: Çalıştır yıldız-CCM + Linux RDMA üzerinde Microsoft HPC Pack ile Azure'da küme](classic/hpcpack-cluster-starccm.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="cluster-management"></a>Küme yönetimi
* [Azure'da bir HPC Pack kümeye iş göndermek](../windows/hpcpack-cluster-submit-jobs.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [HPC Pack iş yönetimi](https://technet.microsoft.com/library/jj899585.aspx)

## <a name="rdma-clusters-for-mpi-workloads"></a>RDMA kümeleri MPI iş yükleri için
* [Öğretici: Azure Linux RDMA kümesinde Microsoft HPC paketi ile çalışma OpenFOAM](classic/hpcpack-cluster-openfoam.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [MPI uygulamaları çalıştırmak için Linux RDMA küme ayarlama](classic/rdma-cluster.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

